---
title: HTTP-extensieprotocol - Azure
description: In dit artikel leert u het HTTP-extensieprotocol gebruiken om berichten te verzenden tussen de Live Video Analytics-module en uw aangepaste AI- of CV-module.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: bfc8672185d284abc8f2985b44cea92000bc73db
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/03/2020
ms.locfileid: "89448046"
---
# <a name="http-extension-protocol"></a>HTTP-extensieprotocol

In dit artikel leert u het HTTP-extensieprotocol gebruiken om berichten te verzenden tussen de Live Video Analytics-module en uw aangepaste AI- of CV-module.

Het HTTP-contract wordt op basis van de volgende twee onderdelen gedefinieerd:

* HTTP-server
* Live Video Analytics op de IoT Edge-module fungeert als de HTTP-client

## <a name="request"></a>Aanvraag

Aanvragen van de Live Video Analytics-module naar uw HTTP-server zien er als volgt uit:

|Sleutel|Waarde|
|---|---|
|POST|`https://hostname/optional-path?optional-query`|
|Accepteren|application/json,  */*|
|Autorisatie|Basic, Digest, Bearer (door ondersteuning voor aangepaste headers)|
|Content-Type|image/jpeg<br/>image/png<br/>image/bmp<br/>image/x-raw|
|Lengte Content-Length: lengte van de hoofdtekst, in bytes|
|User-Agent|Azure Media Services|
|Hoofdtekst|Bytes per afbeelding, binair versleuteld in een van de ondersteunde inhoudstypen.|

### <a name="example"></a>Voorbeeld

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

## <a name="response"></a>Antwoord

Antwoorden van uw module aan de Live Video Analytics-module moeten er als volgt uitzien:

|Sleutel|Waarde|
|---|---|
|Statuscodes|200 OK - deductieresultaten gevonden<br/>204 Geen inhoud - de kunstmatige intelligentie heeft geen inhoud gevonden<br/>400 Ongeldige aanvraag - niet verwacht<br/>500 Interne serverfout - niet verwacht<br/>503 Server bezet - AMS trekt zich terug op basis van de header 'Proberen-Na' of op basis van een standaardhoeveelheid tijd in het geval de header niet vooraf is ingesteld.|
|Content-Type|application/json|
|Content-Length|Lengte van de hoofdtekst, in bytes|
|Hoofdtekst|JSON-object met één inferences-eigenschap.|

### <a name="example"></a>Voorbeeld

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Het wordt aanbevolen dat antwoorden worden geretourneerd met behulp van geldige JSON-documenten volgens het hieronder gedefinieerde schema, dat vooraf is ingesteld. Dit zorgt voor betere interoperabiliteit met andere onderdelen en eventueel toekomstige mogelijkheden die aan de Live Video Analytics-module worden toegevoegd.

Als een antwoord wordt geretourneerd waarbij het inhoudstype niet 'application/json' is, wordt het bericht door Live Video Analytics als basis-64-inhoud versleuteld en geserialiseerd als een ondoorzichtige JSON-nettolading.

Als er wel een antwoord met inhoudstype 'application/json' wordt geretourneerd maar het JSON-schema niet het hieronder beschreven metagegevensschema voor deductie volgt, wordt de nettolading van het bericht doorgestuurd via de pijplijn, maar dan is er wel sprake van beperkte interoperabiliteit.

> [!NOTE]
> Als er geen enkel antwoord wordt geproduceerd, moet de HTTP 204-statuscode (geen inhoud) worden geretourneerd, met een lege hoofdtekst. Live Video Analytics interpreteert dit als een leeg resultaat. De gebeurtenis wordt niet via de pijplijn doorgestuurd.

## <a name="data-contracts---class-hierarchy"></a>Gegevenscontracten: klassehiërarchie

![klassehiërarchie](./media/http-extension-protocol/class-hierarchy.png)

## <a name="next-steps"></a>Volgende stappen

[gRPC-gegevenscontract](grpc-data-contract.md)
