---
title: Eind punten voor de Bing Afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: De Afbeeldingen zoeken-API bevat drie eind punten. Eind punt 1 retourneert installatie kopieën van het web. Eind punt 2 retourneert ImageInsights. Eind punt 3 retourneert trend afbeeldingen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 38416f6a580d270aefc287de0c198bd418a44db9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "74072632"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Eind punten voor de Bing Afbeeldingen zoeken-API

De **afbeeldingen zoeken-API** bevat drie eind punten.  Eind punt 1 retourneert installatie kopieën van het web op basis van een query. Eind punt 2 retourneert [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse).  Eind punt 3 retourneert trend afbeeldingen.

## <a name="endpoints"></a>Eindpunten

Als u afbeeldings resultaten wilt ophalen met behulp van de Bing API, verzendt u een aanvraag naar een van de volgende eind punten. Gebruik de para meters headers en URL om verdere specificaties te definiëren.

**Eind punt 1:** Hiermee worden installatie kopieën geretourneerd die relevant zijn voor de zoek query van `?q=""`de gebruiker die is gedefinieerd door.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Eind punt 2:** Retourneert inzichten over een installatie kopie met `GET` behulp `POST`van of.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Een GET-aanvraag retourneert inzichten over een afbeelding, zoals webpagina's die de afbeelding bevatten. Neem de para meter [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) op `GET` met een aanvraag.

U kunt ook een binaire installatie kopie in de hoofd tekst van een `POST` aanvraag toevoegen en de para meter [modules](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) instellen op `RecognizedEntities`. Hiermee wordt een [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken) geretourneerd dat als para meter wordt gebruikt in een `GET` volgende aanvraag, die informatie over personen in de installatie kopie retourneert.  Stel `modules` deze `All` waarde in op om alle inzichten `RecognizedEntities` op te halen, behalve `POST` in de resultaten van de zonder `insightsToken`een andere aanroep te gebruiken.


**Eind punt 3:** Hiermee worden afbeeldingen geretourneerd die worden getrendd op basis van zoek opdrachten die door anderen zijn gemaakt. De afbeeldingen worden onderverdeeld in verschillende categorieën, bijvoorbeeld op basis van de vervelende personen of gebeurtenissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Zie [trending afbeeldingen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)voor een lijst met markten die trending afbeeldingen ondersteunen.

Zie de naslag informatie over [Bing afbeeldingen zoeken-API V7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) voor meer informatie over kopteksten, para meters, markt codes, reactie objecten, fouten, enzovoort.
## <a name="response-json"></a>JSON van antwoord
Het antwoord op een aanvraag voor het zoeken naar een afbeelding bevat resultaten als JSON-objecten. Raadpleeg de [zelf studie](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) en de [bron code](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source)voor voor beelden van het parseren van de resultaten.

## <a name="next-steps"></a>Volgende stappen
De **Bing** api's ondersteunen zoek acties die resultaten retour neren op basis van hun type.Alle zoek eindpunten retour neren resultaten als JSON-antwoord objecten. Alle eind punten ondersteunen query's die een specifieke taal en/of locatie retour neren met de lengte graad, breedte graad en de zoek RADIUS.

Zie de naslag pagina's voor elk type voor volledige informatie over de para meters die door elk eind punt worden ondersteund.
Zie [afbeeldingen zoeken Quick Start (](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web)Engelstalig) voor voor beelden van basis aanvragen met behulp van de afbeelding zoeken-API.
