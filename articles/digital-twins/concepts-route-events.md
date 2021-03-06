---
title: Gebeurtenisroutes
titleSuffix: Azure Digital Twins
description: Meer informatie over het routeren van gebeurtenissen in azure Digital Apparaatdubbels en naar andere Azure-Services.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: d41518b1fc0d8cdda3ded1e8036bd29e24e2b34a
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541353"
---
# <a name="route-events-within-and-outside-of-azure-digital-twins"></a>Gebeurtenissen binnen en buiten Azure Digital Apparaatdubbels routeren

Azure Digital apparaatdubbels gebruikt **gebeurtenis routes** voor het verzenden van gegevens naar gebruikers buiten de service. 

Tijdens de preview zijn er twee belang rijke situaties voor het verzenden van Azure Digital Apparaatdubbels-gegevens:
* Het verzenden van gegevens van een dubbele in de Azure Digital Apparaatdubbels-grafiek naar een andere. Als u bijvoorbeeld een eigenschap op één digitaal nadien wijzigt, wilt u wellicht een melding ontvangen en een andere digitale navolgende digitaal bijwerken.
* Verzenden van gegevens naar downstream gegevens Services voor extra opslag of verwerking (ook wel bekend als *gegevens*uitvoer). Bijvoorbeeld
  - Een zieken huis wil mogelijk Azure Digital Apparaatdubbels-gebeurtenis gegevens naar [Time Series Insights (TSI)](../time-series-insights/time-series-insights-update-overview.md)verzenden om de tijdreeks gegevens van handwashing gebeurtenissen voor bulk analyse vast te leggen.
  - Een bedrijf dat al gebruikmaakt van [Azure Maps](../azure-maps/about-azure-maps.md) kan gebruikmaken van Azure Digital apparaatdubbels om de oplossing te verbeteren. Ze kunnen snel een Azure-kaart inschakelen na het instellen van Azure Digital Apparaatdubbels, Azure-toewijzings entiteiten in azure Digital Apparaatdubbels als [digitale apparaatdubbels](concepts-twins-graph.md) in het dubbele diagram, of krachtige query's uitvoeren met behulp van hun Azure Maps en Azure Digital apparaatdubbels-gegevens samen.

Gebeurtenis routes worden gebruikt voor beide scenario's.

## <a name="about-event-routes"></a>Over gebeurtenis routes

Met een gebeurtenis route kunt u gebeurtenis gegevens van digitale apparaatdubbels in azure Digital Apparaatdubbels verzenden naar aangepaste eind punten in uw abonnementen. Drie Azure-Services worden momenteel ondersteund voor eind punten: [Event hub](../event-hubs/event-hubs-about.md), [Event grid](../event-grid/overview.md)en [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md). Elk van deze Azure-Services kan worden verbonden met andere services en fungeert als middelsteman, waarbij gegevens worden verzonden naar eind bestemmingen, zoals een TSI of Azure Maps voor de gewenste verwerking.

In het volgende diagram ziet u de stroom van gebeurtenis gegevens via een grotere IoT-oplossing met een Azure Digital Apparaatdubbels-aspect:

:::image type="content" source="media/concepts-route-events/routing-workflow.png" alt-text="Azure Digital Apparaatdubbels routerings gegevens via eind punten naar verschillende downstream-Services" border="false":::

Typische downstream doelen voor gebeurtenis routes zijn bronnen als TSI-, Azure Maps-, opslag-en analyse oplossingen.

### <a name="event-routes-for-internal-digital-twin-events"></a>Gebeurtenis routes voor interne digitale dubbele gebeurtenissen

Tijdens de huidige preview-versie worden gebeurtenis routes ook gebruikt voor het afhandelen van gebeurtenissen in de dubbele grafiek en het verzenden van gegevens van digitale twee naar digitale twee. Dit wordt gedaan door gebeurtenis routes te verbinden via Event Grid naar reken resources, zoals [Azure functions](../azure-functions/functions-overview.md). Deze functies bepalen vervolgens hoe apparaatdubbels moeten worden ontvangen en reageren op gebeurtenissen. 

Wanneer een compute-resource de dubbele grafiek wil wijzigen op basis van een gebeurtenis die hij via de gebeurtenis route heeft ontvangen, is het handig om te weten welke dubbele it in de loop van de tijd wil wijzigen. 

Het gebeurtenis bericht bevat ook de ID van de bron die het bericht heeft verzonden, zodat de compute-resource query's kan gebruiken of relaties kunt door lopen om een doel te vinden dat ligt voor de gewenste bewerking. 

De compute-resource moet ook afzonderlijke beveiligings-en toegangs machtigingen tot stand brengen.

Zie [*How-to: een Azure-functie instellen voor het verwerken van gegevens*](how-to-create-azure-function.md)om het proces van het instellen van een Azure-functie voor het verwerken van digitale dubbele gebeurtenissen door te lopen.

## <a name="create-an-endpoint"></a>Een eindpunt maken

Ontwikkel aars moeten eerst eind punten definiëren om een gebeurtenis route te definiëren. Een **eind punt** is een bestemming buiten Azure Digital apparaatdubbels die een route verbinding ondersteunt. Ondersteunde bestemmingen in de huidige preview-versie zijn:
* Aangepaste onderwerpen Event Grid
* Event Hub
* Service Bus

Als u een eind punt wilt maken, kunt u de Azure Digital Apparaatdubbels [**Control-api's**](how-to-manage-routes-apis-cli.md#create-an-endpoint-for-azure-digital-twins), [**cli-opdrachten**](how-to-manage-routes-apis-cli.md#manage-endpoints-and-routes-with-cli)of de [**Azure Portal**](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins)gebruiken. 

Wanneer u een eind punt definieert, moet u het volgende opgeven:
* De naam van het eind punt
* Het type eind punt (Event Grid, Event hub of Service Bus)
* De primaire connection string en secundaire connection string voor verificatie 
* Het pad naar het onderwerp van het eind punt, bijvoorbeeld *Your-topic.westus2.eventgrid.Azure.net*

De eindpunt-Api's die beschikbaar zijn in het besturings vlak zijn:
* Eind punt maken
* Lijst met eind punten ophalen
* Eind punt ophalen op naam
* Eind punt verwijderen op naam

## <a name="create-an-event-route"></a>Een gebeurtenis route maken
 
Als u een gebeurtenis route wilt maken, kunt u de Azure Digital Apparaatdubbels [**Data-vlak-api's**](how-to-manage-routes-apis-cli.md#create-an-event-route), CLI- [**opdrachten**](how-to-manage-routes-apis-cli.md#manage-endpoints-and-routes-with-cli)of de [**Azure Portal**](how-to-manage-routes-portal.md#create-an-event-route)gebruiken. 

Hier volgt een voor beeld van het maken van een gebeurtenis route binnen een client toepassing, met behulp van de `CreateEventRoute` [.net (C#) SDK-](how-to-use-apis-sdks.md) aanroep: 

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

1. Eerst wordt een- `EventRoute` object gemaakt en de constructor neemt de naam van een eind punt. Dit `endpointName` veld identificeert een eind punt, zoals een event hub, Event grid of service bus. Deze eind punten moeten in uw abonnement worden gemaakt en aan Azure Digital Apparaatdubbels gekoppeld met behulp van Control-Api's voordat u deze registratie oproep doet.

2. Het gebeurtenis route object heeft ook een [**filter**](./how-to-manage-routes-apis-cli.md#filter-events) veld dat kan worden gebruikt om de typen gebeurtenissen te beperken die volgen op deze route. Met een filter van `true` kunt u de route inschakelen zonder extra filters (een filter van `false` de route wordt uitgeschakeld). 

3. Dit gebeurtenis route object wordt vervolgens door gegeven aan `CreateEventRoute` , samen met een naam voor de route.

> [!TIP]
> Alle SDK-functies zijn beschikbaar in synchrone en asynchrone versies.

Routes kunnen ook worden gemaakt met behulp van de [Azure Digital APPARAATDUBBELS cli](how-to-use-cli.md).

### <a name="types-of-event-messages"></a>Typen gebeurtenis berichten

Verschillende typen gebeurtenissen in IoT Hub en Azure Digital Apparaatdubbels genereren verschillende typen meldings berichten, zoals hieronder wordt beschreven.

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

## <a name="next-steps"></a>Volgende stappen

Zie een gebeurtenis route instellen en beheren:
* [*Instructies: eind punten en routes beheren*](how-to-manage-routes-apis-cli.md)

U kunt ook zien hoe u Azure Functions gebruikt voor het routeren van gebeurtenissen in azure Digital Apparaatdubbels:
* [*Instructies: een Azure-functie voor het verwerken van gegevens instellen*](how-to-create-azure-function.md)