---
title: Upstream-instellingen in de Azure signalerings service
description: Down load een inleiding in de instellingen van de upstream en protocollen van upstream-berichten.
author: chenyl
ms.service: signalr
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: chenyl
ms.openlocfilehash: c3e317a87ba888fac3c069cc5327bd89c859e9de
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514234"
---
# <a name="upstream-settings"></a>Upstream-instellingen

Upstream is een preview-functie waarmee de Azure signalerings service berichten en verbindings gebeurtenissen kan verzenden naar een set eind punten in de serverloze modus. U kunt upstream gebruiken om een hub-methode aan te roepen van clients in de serverloze modus, en laat eind punten een melding ontvangen wanneer client verbindingen zijn verbonden of verbroken.

> [!NOTE]
> Alleen serverloze modus kan upstream-instellingen configureren.

## <a name="details-of-upstream-settings"></a>Details van de upstream-instellingen

De upstream-instellingen bestaan uit een lijst met volg orde gevoelige items. Elk item bestaat uit:

* Een URL-sjabloon, waarmee wordt aangegeven waar berichten naar worden verzonden.
* Een set regels.
* Verificatie configuraties. 

Wanneer de opgegeven gebeurtenis plaatsvindt, worden de regels van een item één voor één in de aangegeven volg orde gecontroleerd. Berichten worden verzonden naar de URL van de upstream van het eerste overeenkomende item.

### <a name="url-template-settings"></a>URL-sjabloon instellingen

U kunt de URL voor het ondersteunen van verschillende patronen para meters. Er zijn drie vooraf gedefinieerde para meters:

|Vooraf gedefinieerde para meter|Beschrijving|
|---------|---------|
|hub| Een hub is een concept van de Azure signalerings service. Een hub is een isolatie-eenheid. Het bereik van gebruikers en bericht levering is beperkt tot een hub.|
|rubriek| Een categorie kan een van de volgende waarden hebben: <ul><li>**verbindingen**: levens duur van de verbinding. Het wordt geactiveerd wanneer een client verbinding is verbonden of verbroken. Het bevat verbonden en niet-verbonden gebeurtenissen.</li><li>**berichten**: wordt geactiveerd wanneer clients een hub-methode aanroepen. Het bevat alle andere gebeurtenissen, behalve die in de categorie **verbindingen** .</li></ul>|
|gebeurtenislog| Voor de categorie **berichten** is een gebeurtenis het doel in een [aanroep bericht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) dat door clients wordt verzonden. Voor de categorie **verbindingen** worden alleen *verbinding* gemaakt met en de *verbinding is verbroken* .|

Deze vooraf gedefinieerde para meters kunnen worden gebruikt in het URL-patroon. Para meters worden vervangen door een opgegeven waarde wanneer u de URL van de upstream wilt evalueren. Bijvoorbeeld: 
```
http://host.com/{hub}/api/{category}/{event}
```
Wanneer een client verbinding in de hub ' chat ' is verbonden, wordt een bericht verzonden naar deze URL:
```
http://host.com/chat/api/connections/connected
```
Wanneer een client in de ' chat '-hub de hub-methode aanroept `broadcast` , wordt een bericht verzonden naar deze URL:
```
http://host.com/chat/api/messages/broadcast
```

### <a name="rule-settings"></a>Regel instellingen

U kunt *regels voor kanbanregels*, *categorie regels*en *gebeurtenis regels* afzonderlijk instellen. De overeenkomende regel ondersteunt drie indelingen. Neem gebeurtenis regels als voor beeld:
- Gebruik een asterisk (*) om een wille keurige gebeurtenis te zoeken.
- Gebruik een komma (,) om lid te worden van meerdere gebeurtenissen. Bijvoorbeeld, `connected, disconnected` komt overeen met de gebeurtenissen Connected en disconnected.
- De volledige gebeurtenis naam moet overeenkomen met de gebeurtenis. Bijvoorbeeld, `connected` komt overeen met de gebeurtenis Connected.

> [!NOTE]
> Als u gebruikmaakt van Azure Functions en [signaal schakelaar](../azure-functions/functions-bindings-signalr-service-trigger.md), wordt door de signaal trigger een enkel eind punt beschikbaar gesteld in de volgende indeling: `https://<APP_NAME>.azurewebsites.net/runtime/webhooks/signalr?code=<API_KEY>` .
> U kunt de URL-sjabloon alleen configureren voor deze URL.

### <a name="authentication-settings"></a>Verificatie-instellingen

U kunt verificatie voor elk item van een upstream-instelling afzonderlijk configureren. Wanneer u authenticatie configureert, wordt een token ingesteld in de `Authentication` koptekst van het upstream-bericht. De Azure signalerings service ondersteunt momenteel de volgende verificatie typen:
- `None`
- `ManagedIdentity`

Wanneer u selecteert `ManagedIdentity` , moet u een beheerde identiteit in de Azure signalerings service vooraf inschakelen en optioneel een resource opgeven. Zie [beheerde identiteiten voor de Azure signalerings service](howto-use-managed-identity.md) voor meer informatie.

## <a name="create-upstream-settings-via-the-azure-portal"></a>Upstream-instellingen maken via de Azure Portal

1. Ga naar de Azure signalerings service.
2. Selecteer **instellingen** en Schakel **Service modus** in op **serverloos**. De upstream-instellingen worden weer gegeven:

    :::image type="content" source="media/concept-upstream/upstream-portal.png" alt-text="Upstream-instellingen":::

3. Voeg Url's toe onder het URL-patroon van de **upstream**. Vervolgens wordt de standaard waarde weer gegeven in de instellingen van de **hub** .
4. Als u instellingen wilt instellen voor **regels**voor de hub, **gebeurtenis regels**, **categorie regels**en **upstream-verificatie**, selecteert u de waarde hub- **regels**. Er wordt een pagina weer gegeven waarin u de instellingen kunt bewerken:

    :::image type="content" source="media/concept-upstream/upstream-detail-portal.png" alt-text="Details van de upstream-instelling":::

5. Als u een **upstream-verificatie**wilt instellen, moet u ervoor zorgen dat u eerst een beheerde identiteit hebt ingeschakeld. Selecteer vervolgens **beheerde identiteit gebruiken**. Afhankelijk van uw behoeften kunt u opties selecteren onder **verificatie resource-id**. Zie [beheerde identiteiten voor de Azure signalerings service](howto-use-managed-identity.md) voor meer informatie.

## <a name="create-upstream-settings-via-resource-manager-template"></a>Upstream-instellingen maken via Resource Manager-sjabloon

Als u de instellingen voor de upstream wilt maken met behulp van een [Azure Resource Manager sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview), stelt u de `upstream` eigenschap in de `properties` eigenschap in. Het volgende code fragment laat zien hoe u de eigenschap kunt instellen `upstream` voor het maken en bijwerken van de instellingen voor de upstream.

```JSON
{
  "properties": {
    "upstream": {
      "templates": [
        {
          "UrlTemplate": "http://host.com/{hub}/api/{category}/{event}",
          "EventPattern": "*",
          "HubPattern": "*",
          "CategoryPattern": "*",
          "Auth": {
            "Type": "ManagedIdentity",
            "ManagedIdentity": {
              "Resource": "<resource>"
            }
          }
        }
      ]
    }
  }
}
```

## <a name="serverless-protocols"></a>Serverloze protocollen

De Azure signalerings service stuurt berichten naar eind punten die de volgende protocollen volgen.

### <a name="method"></a>Methode

POST

### <a name="request-header"></a>Aanvraagheader

|Naam |Beschrijving|
|---------|---------|
|X-ASRS-Connection-ID |De verbindings-ID voor de client verbinding.|
|X-ASRS-hub |De hub waartoe de client verbinding behoort.|
|X-ASRS-categorie |De categorie waarvan het bericht deel uitmaakt.|
|X-ASRS-Event |De gebeurtenis waarvan het bericht deel uitmaakt.|
|X-ASRS-hand tekening |Een HMAC (Hash-based Message Authentication Code) die wordt gebruikt voor validatie. Zie [hand tekening](#signature) voor meer informatie.|
|X-ASRS-User-claims |Een groep claims van de client verbinding.|
|X-ASRS-User-ID |De gebruikers-id van de client die het bericht verzendt.|
|X-ASRS-client-query |De query van de aanvraag wanneer clients verbinding maken met de service.|
|Verificatie |Een optioneel token wanneer u gebruikt `ManagedIdentity` . |

### <a name="request-body"></a>Aanvraagbody

#### <a name="connected"></a>Verbonden

Content-type: Application/JSON

#### <a name="disconnected"></a>Ontkoppeld

Inhouds type: `application/json`

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|Fout |tekenreeks |Het fout bericht van een gesloten verbinding. Leeg wanneer de verbindingen worden gesloten zonder dat er een fout optreedt.|

#### <a name="invocation-message"></a>Aanroep bericht

Content-type: `application/json` of `application/x-msgpack`

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|InvocationId |tekenreeks | Een optionele teken reeks die een aanroep bericht vertegenwoordigt. Details zoeken in [aanroepen](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocations).|
|Doel |tekenreeks | Hetzelfde als de gebeurtenis en dezelfde als het doel in een [aanroep bericht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding). |
|Argumenten |Matrix van object |Een matrix die argumenten bevat die moeten worden toegepast op de methode waarnaar wordt verwezen in `Target` . |

### <a name="signature"></a>Handtekening

De service berekent de SHA256-code voor de `X-ASRS-Connection-Id` waarde door zowel de primaire toegangs sleutel als de secundaire toegangs sleutel als de `HMAC` sleutel te gebruiken. De service stelt deze in de `X-ASRS-Signature` koptekst in bij het maken van HTTP-aanvragen voor de upstream:
```
Hex_encoded(HMAC_SHA256(accessKey, connection-id))
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor de Azure signalerings service](howto-use-managed-identity.md)
- [Ontwikkeling en configuratie van Azure Functions met Azure SignalR Service](signalr-concept-serverless-development-config.md)
