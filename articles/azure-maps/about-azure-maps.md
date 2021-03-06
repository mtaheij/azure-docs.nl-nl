---
title: Overzicht voor Microsoft Azure Maps
description: Meer informatie over services en mogelijkheden in Microsoft Azure Maps en hoe u deze kunt gebruiken in uw toepassingen.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/31/2020
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc, references_regions
ms.openlocfilehash: 11a0ddc5c7b297d0700e6fd07d60f8efe0e55a8f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91285329"
---
# <a name="what-is-azure-maps"></a>Wat is Azure Maps?

Azure Maps is een verzameling georuimtelijke services en SDK’s die nieuwe kaartgegevens gebruiken om webtoepassingen en mobiele toepassingen te voorzien van een nauwkeurige geografische context. Azure Maps biedt:

* REST-API's voor het weergeven van vector- en rasterkaarten in meerdere stijlen en satellietbeelden.
* Creator-services om kaarten te maken en weer te geven op basis van gegevens van een privé-kaart.
* Search-services om adressen, plaatsen en interessante zaken over de hele wereld te vinden.
* Verschillende routeringsopties; zoals point-to-point, multipoint, multipoint-optimalisatie, isochroon, elektrische voertuigen, bedrijfswagens, verkeer dat wordt beïnvloed en matrixroutering.
* Weergave van verkeersstroom en incidenten, voor toepassingen waarvoor realtime verkeersgegevens nodig zijn.
* Mobility-service om informatie over het openbaar vervoer aan te vragen, routes te plannen door verschillende reismodi en realtime aankomsttijden te combineren.
* Services voor tijdzones en geolocaties.
* Geofencing-service en toewijzing van gegevensopslag, met locatiegegevens die worden gehost in Azure.
* Locatie-informatie via georuimtelijke analyse.

Daarnaast zijn Azure Maps-services beschikbaar via de Web-SDK en de Android-SDK. Met deze hulpprogramma's kunnen ontwikkelaars snel oplossingen ontwikkelen en schalen waarmee locatiegegevens worden geïntegreerd in Azure-oplossingen.

U kunt zich aanmelden voor een gratis [Azure Maps-account](https://azure.microsoft.com/services/azure-maps/) en begin met ontwikkelen.

In de volgende video wordt Azure Maps in detail uitgelegd:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kaartbesturingselementen

### <a name="web-sdk"></a>Web SDK

Met de Azure Maps Web-SDK kunt u interactieve kaarten aanpassen met uw eigen inhoud en beelden. U kunt deze interactieve kaart gebruiken voor uw web- of mobiele toepassingen. Het besturingselement maakt gebruik van WebGL, zodat u grote gegevenssets kunt weergeven met hoge prestaties. U kunt deze ontwikkelen met de SDK met behulp van JavaScript of TypeScript.

:::image type="content" source="./media/about-azure-maps/intro_web_map_control.png" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

### <a name="android-sdk"></a>Android SDK

Gebruik de Azure Maps Android-SDK om toepassingen voor mobiele toewijzing te maken.

:::image type="content" source="./media/about-azure-maps/android_sdk.png" border="false" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

## <a name="services-in-azure-maps"></a>Services in Azure Maps

Azure Maps bestaat uit de volgende services die uw Azure-toepassingen van geografische context kunnen voorzien.

### <a name="data-service"></a>Gegevensservice

Gegevens zijn van cruciaal belang voor kaarten. Gebruik de gegevensservice om georuimtelijke gegevens te uploaden en op te slaan voor gebruik met ruimtelijke bewerkingen of om installatiekopieën samen te stellen.  Door klantgegevens dichter bij de Azure Maps-service te brengen, wordt de latentie verminderd, de productiviteit verhoogd en nieuwe scenario's gemaakt in uw toepassingen. Zie de [documentatie van de gegevensservice](https://docs.microsoft.com/rest/api/maps/data) voor meer informatie over deze service.

### <a name="geolocation-service"></a>Geolocatie-service

Gebruik de IP van de Geolocatie-service om een voorbeeld te zien van het opgehaalde tweeletterige land-/regionummer voor een IP-adres. Deze service kan u helpen om de gebruikerservaring te verbeteren door een aangepaste toepassingsinhoud op basis van geografische locatie te bieden.

Lees voor meer informatie de [documentatie over de geolocatie-service](https://docs.microsoft.com/rest/api/maps/geolocation).

### <a name="mobility-service"></a>Mobility-service

De Azure Maps Mobility-service verbetert de ontwikkelingstijd voor toepassingen voor het openbaar vervoer, zoals transitroutering en zoeken naar OV-stops in de buurt. Gebruikers kunnen gedetailleerde informatie ophalen over OV-haltes, -lijnen en -schema's. De Mobility-service biedt gebruikers ook de mogelijkheid om halte- en lijngeometrieën, waarschuwingen voor haltes, lijnen en servicegebieden, en realtime aankomsttijden van openbaar vervoer en servicewaarschuwingen. Daarnaast biedt de Mobility-service mogelijkheden voor routering met multimodale opties voor reisplanning. Met multimodale reisplanning worden de opties voor lopen, fietsen en openbaar vervoer in één reis opgenomen. Gebruikers hebben ook toegang tot gedetailleerde multimodale stapsgewijze routes.

Zie de [Mobility-documentatie](https://docs.microsoft.com/rest/api/maps/mobility) voor meer informatie over de service.

### <a name="render-service"></a>Service voor rendering

De [Render-Service v2](https://docs.microsoft.com/rest/api/maps/renderv2) waarvan nu een preview-versie beschikbaar is, introduceert een nieuwe versie van de [Get Map Tile V2-API](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview). Met de Get Map Tile V2-API kunnen klanten nu Azure Maps wegtegels, weertegels of kaarttegels aanvragen die zijn gemaakt met behulp van Azure Maps Maker. Het is raadzaam om de nieuwe Get Map Tile V2-API te gebruiken.  

:::image type="content" source="./media/about-azure-maps/intro_map.png" border="false" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

Lees de pagina [Render service V2-documentatie](https://docs.microsoft.com/rest/api/maps/renderv2) voor meer informatie.

Voor meer informatie over de Render service v1 die algemeen beschikbaar is, raadpleegt u de [Render service v1-documentatie](https://docs.microsoft.com/rest/api/maps/render).  

### <a name="route-service"></a>Routeservice

De routeservices kunnen worden gebruikt om de geschatte aankomsttijden (ETAs) voor elke aangevraagde route te berekenen. Route-API's houden rekening met factoren, zoals gegevens over realtime verkeer en historische verkeersgegevens, zoals de typische snelheden op de gewenste dag van de week en het tijdstip van de dag. De API's retourneren de kortste of snelste routes die beschikbaar zijn voor meerdere bestemmingen tegelijk in een reeks of in een geoptimaliseerde volgorde, op basis van tijd of afstand. Met de service kunnen ontwikkelaars richtingen berekenen voor verschillende manieren van reizen, zoals met de auto, vrachtwagen, fiets of wandelen. De service houdt ook rekening met invoerwaarden voor verkeersomstandigheden, zoals vertrektijden, gewichtsbeperkingen of transport van gevaarlijke stoffen.

:::image type="content" source="./media/about-azure-maps/intro_route.png" border="false" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

De routeservice biedt geavanceerde instelfuncties, zoals:

* Batchverwerking van meerdere route-aanvragen.
* De matrices van reistijd en afstand tussen een reeks aanvangspunten en bestemmingen.
* Routes of afstanden zoeken die gebruikers kunnen reizen op basis van tijd- of brandstofvereisten.

Lees de pagina [Routeservice-documentatie](https://docs.microsoft.com/rest/api/maps/route) voor meer informatie over de routeringsmogelijkheden.

### <a name="search-service"></a>Zoekservice

De Search-service helpt ontwikkelaars bij het zoeken naar adressen, plaatsen, bedrijfsvermeldingen op naam of categorie, en andere geografische informatie. Met de Search-service kan ook [de geocode worden omgekeerd](https://en.wikipedia.org/wiki/Reverse_geocoding) voor adressen en dwarsstraten op basis van breedte- en lengtegraden.

:::image type="content" source="./media/about-azure-maps/intro_search.png" border="false" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

De Search-service biedt ook geavanceerde functies zoals:

* Zoeken langs een route.
* Zoeken binnen een breder gebied.
* Batch een groep met zoekopdrachten.
* Zoek oplaadstations en POI-gegevens op merknaam.

Lees de pagina [Zoekservice-documentatie](https://docs.microsoft.com/rest/api/maps/search) voor meer informatie over zoekmogelijkheden.

### <a name="spatial-service"></a>Ruimtelijke service

De locatiegegevens worden snel geanalyseerd door de ruimtelijke service om klanten te informeren over lopende gebeurtenissen die in de tijd en ruimte plaatsvinden. De service maakt bijna realtime analyse en voorspellende modellering van gebeurtenissen mogelijk.

Met de service kunnen klanten hun locatiegegevens verbeteren met een bibliotheek met algemene georuimtelijke wiskundige berekeningen. Veelvoorkomende berekeningen zijn onder andere het dichtstbijzijnde punt, orthodromische afstand en buffers. Lees voor meer informatie over de service en de verschillende functies de [Ruimtelijke service-documentatie](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="timezone-service"></a>Tijdzone-service

Met de Time zone-service kunt u de huidige, historische en toekomstige tijdzone-informatie opvragen. U kunt voor de invoer gebruikmaken van de breedte- en lengtegraad-paren of een [IANA-id](https://www.iana.org/). De Time zone-service biedt ook het volgende:

* De tijdzone-id's van Microsoft Windows converteren naar IANA-tijdzones.
* Een tijdzone-offset wordt opgehaald naar UTC.
* De huidige tijd in een gekozen tijdzone ophalen.

Een typisch JSON-antwoord voor een query in de Time zone-service ziet er als volgt uit:

```JSON
{
  "Version": "2020a",
  "ReferenceUtcTimestamp": "2020-07-31T19:15:14.4570053Z",
  "TimeZones": [
    {
      "Id": "America/Los_Angeles",
      "Names": {
        "ISO6391LanguageCode": "en",
        "Generic": "Pacific Time",
        "Standard": "Pacific Standard Time",
        "Daylight": "Pacific Daylight Time"
      },
      "ReferenceTime": {
        "Tag": "PDT",
        "StandardOffset": "-08:00:00",
        "DaylightSavings": "01:00:00",
        "WallTime": "2020-07-31T12:15:14.4570053-07:00",
        "PosixTzValidYear": 2020,
        "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
      }
    }
  ]
}
```

Lees de [Tijdzone-documentatie](https://docs.microsoft.com/rest/api/maps/timezone) voor meer informatie over deze service.

### <a name="traffic-service"></a>Verkeersservice

De Traffic-service is een suite van webservices die ontwikkelaars kunnen gebruiken voor web- of mobiele toepassingen waarvoor verkeersgegevens nodig zijn. De service omvat twee gegevenstypen:

* Verkeersstroom: Waargenomen snelheden en reistijden in realtime voor alle belangrijke wegen in het netwerk.
* Verkeersincidenten: Een up-to-date weergave van files en incidenten rond het wegnetwerk.

![Voorbeeld van een kaart met verkeersgegevens](media/about-azure-maps/intro_traffic.png)

Zie de [documentatie van de verkeersservice](https://docs.microsoft.com/rest/api/maps/traffic) voor meer informatie.

### <a name="weather-service"></a>Weerservices

De weerservice biedt API’s die ontwikkel aars kunnen gebruiken om weergegevens voor een bepaalde locatie op te halen. De informatie bevat details, zoals de datum en tijd van observatie, een korte beschrijving van de weersomstandigheden, het weerpictogram, de vlagmarkeringen, de temperatuur en de windsnelheid. Er worden ook extra gegevens weergegeven, zoals de RealFeel™-temperatuur en UV-index.

Ontwikkelaars kunnen de [Get Weather along route-API](https://docs.microsoft.com/rest/api/maps/weather/getweatheralongroutepreview) gebruiken om weerinformatie te verkrijgen voor een bepaalde route. De service biedt ook ondersteuning voor het genereren van weermeldingen voor waypoints die worden beïnvloed door weerrisico's, zoals overstromingen of zware regenbuien.

Met de [Get Map Tile V2-API](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview) kunt u de tegels voor vroegere, huidige en toekomstige radar- en satellietgegevens opvragen.

![Voorbeeld van een kaart met realtime weergave van radargegeven](media/about-azure-maps/intro_weather.png)

### <a name="maps-creator-service"></a>Kaartmaakservice

Maps Creator-service is een suite van webservices die ontwikkelaars kunnen gebruiken voor het maken van toepassingen met kaartfuncties op basis van indoor kaartgegevens.

Maps Creator biedt drie basisservices:

* [Gegevenssetservice](https://docs.microsoft.com/rest/api/maps/dataset). De DataSet-service gebruiken om een gegevensset te maken op basis van geconverteerde gegevens voor tekenpakketen. Zie Vereisten tekenpakket voor meer informatie over vereisten voor tekenpakketten.

* [Conversieservice](https://docs.microsoft.com/rest/api/maps/dataset). Gebruik de conversieservice om een DWG-ontwerp bestand te converteren naar gegevens voor tekenpakketten voor indoor kaarten.

* [Tegelsetservice](https://docs.microsoft.com/rest/api/maps/tileset). Gebruik de Tegelset-service om een op vector gebaseerde weergave van een gegevensset te maken. Toepassingen kunnen een tegelset gebruiken om een weergave op basis van een visuele tegel van de gegevensset te presenteren.

* [Functiestatusservice](https://docs.microsoft.com/rest/api/maps/featurestate). Gebruik de functie Statusservice om dynamische kaartstijlen te ondersteunen. Met dynamische kaartstijlen kunnen toepassingen realtime gebeurtenissen weergeven in ruimten van IoT-systemen.

* [WFS-service](https://docs.microsoft.com/rest/api/maps/featurestate). Gebruik de WFS-service om een query uit te voeren op uw indoor kaartgegevens. De WFS-service volgt de [Open Geospatial Consortium-API](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) standaarden voor het uitvoeren van query's op één gegevensset.

## <a name="programming-model"></a>Programmeermodel

Azure Maps is gebouwd voor mobiliteit en kan u helpen platformoverschrijdende toepassingen te ontwikkelen. Het maakt gebruik van een programmeermodel dat taalonafhankelijk is en dat JSON-uitvoer ondersteunt door middel van [REST API's](https://docs.microsoft.com/rest/api/maps/).

Azure Maps biedt ook een handig [kaartbesturingselement van JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) met een eenvoudig programmeermodel. De ontwikkeling is snel en eenvoudig voor zowel web- als mobiele toepassingen.

## <a name="power-bi-visual"></a>Power BI-visual

De Azure Maps-visual voor Power BI biedt een uitgebreide set gegevensvisualisaties voor ruimtelijke gegevens over een kaart. Naar schatting heeft 80% van de bedrijfsgegevens een geografische context. De Azure Maps-visual biedt een oplossing zonder code om inzicht te krijgen in de manier waarop deze geografische context is gekoppeld aan en invloed heeft op uw bedrijfsgegevens.

:::image type="content" source="./media/about-azure-maps/intro-power-bi.png" border="false" alt-text="Voorbeeld van een toewijzing van een gewijzigde populatie die is gemaakt met Azure Maps Web SDK":::

Zie de 'Aan de slag gaan‘-documentatie voor de [Azure Maps Power BI-visual](power-bi-visual-getting-started.md) voor meer informatie.

## <a name="usage"></a>Gebruik

Als u toegang wilt krijgen tot Azure Maps Services, gaat u naar [Azure Portal](https://portal.azure.com) en maakt u een Azure Maps-account.

Azure Maps maakt gebruik van een verificatieschema op basis van sleutels. Wanneer u uw account maakt, worden er twee sleutels gegenereerd. Als u wilt verifiëren voor Azure Maps Services, kunt u een van beide sleutels gebruiken.

Opmerking - Azure Maps deelt door klanten verstrekte adressen/locatie-query's ("Query's") met derde partij TomTom voor het toewijzen van functionaliteit. Query's worden niet aan een klant of eindgebruiker gekoppeld wanneer ze worden gedeeld met TomTom en kunnen niet worden gebruikt om individuen te identificeren. De Mobility- en Weather-services die integratie met Moovit en AccuWeather bevatten, bevinden zich momenteel in [PREVIEW](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)-modus.

Microsoft is momenteel bezig met het toevoegen van TomTom, Moovit en AccuWeather aan de lijst met toeleveranciers van online services.

## <a name="supported-regions"></a>Ondersteunde regio’s

Azure Maps Services zijn momenteel beschikbaar, behalve in de volgende landen/regio's:

* China
* Zuid-Korea

Verifieer of de locatie van uw IP-adres in een ondersteund land/regio is.

## <a name="next-steps"></a>Volgende stappen

Probeer een sample app uit die Azure Maps weergeeft:

[Snelstart: Een webtoepassing maken](quick-demo-map-app.md)

Houd Azure Maps up-to-date:

[Azure Maps-blog](https://azure.microsoft.com/blog/topics/azure-maps/)
