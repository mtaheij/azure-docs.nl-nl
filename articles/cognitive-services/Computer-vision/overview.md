---
title: Wat is Computer Vision?
titleSuffix: Azure Cognitive Services
description: De Computer Vision-service geeft u toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 09/11/2020
ms.author: pafarley
ms.custom:
- seodec18
- cog-serv-seo-aug-2020
keywords: computer vision, computer vision-toepassingen, computer vision-service
ms.openlocfilehash: fa00265f3c591578f9970b77770fbf7290d87510
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90907075"
---
# <a name="what-is-computer-vision"></a>Wat is Computer Vision?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

De Computer Vision-service van Azure geeft u toegang tot geavanceerde algoritmen waarmee afbeeldingen worden verwerkt en informatie wordt geretourneerd op basis van de visuele kenmerken waarin u geïnteresseerd bent. Met Computer Vision kunt u bijvoorbeeld bepalen of een afbeelding inhoud voor volwassenen bevat, specifieke merken of objecten zoeken, of gezichten van mensen zoeken.

U kunt Computer Vision-toepassingen maken met behulp van een clientbibliotheek-SDK of door de REST API rechtstreeks aan te roepen. Deze pagina laat grosso modo zien wat u kunt doen met Computer Vision.

## <a name="computer-vision-for-digital-asset-management"></a>Computer Vision voor beheer van digitale assets

Met Computer Vision kunt u tal van DAM-scenario's (Digitaal Asset Management) ondersteunen. DAM is het bedrijfsproces voor het organiseren, opslaan en ophalen van geavanceerde media-assets en het beheren van digitale rechten en machtigingen. Het is bijvoorbeeld mogelijk dat een bedrijf afbeeldingen wil groeperen en identificeren op basis van zichtbare logo's, gezichten, objecten, kleuren, enzovoort. U kunt ook automatisch [bijschriften voor afbeeldingen genereren](./Tutorials/storage-lab-tutorial.md) en trefwoorden toevoegen zodat ze zoekbaar zijn. Zie de handleiding [Knowledge Mining Solution Accelerator](https://github.com/Azure-Samples/azure-search-knowledge-mining) (Oplossingsverbetering voor kennisanalyse) op GitHub voor een alles-in-eenoplossing met Cognitive Services, Azure Cognitive Search en intelligente rapportage. Zie de opslagplaats voor [Computer Vision Solution Templates](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates) (Oplossingssjablonen van Computer Vision) voor andere DAM-voorbeelden.

## <a name="optical-character-recognition-ocr"></a>Optische tekenherkenning (OCR)

Computer Vision bevat mogelijkheden voor [Optical Character Recognition (OCR)](concept-recognizing-text.md). U kunt de nieuwe Read-API gebruiken om gedrukte en handgeschreven tekst uit afbeeldingen en documenten te extraheren. De API maakt gebruik van de nieuwste modellen en werkt met tekst op verschillende oppervlakken en achtergronden. Waaronder recepten, posters, visitekaartjes, brieven en whiteboards. De twee OCR-API's bieden ondersteuning voor het extraheren van gedrukte tekst in [diverse talen](./language-support.md). Volg een [snelstart](#next-steps) om aan de slag te gaan.

## <a name="analyze-images-for-insight"></a>Analyseren van afbeeldingen voor inzicht

U kunt afbeeldingen analyseren om inzicht te krijgen in de visuele kenmerken en eigenschappen van die afbeeldingen. Alle functies in de onderstaande tabel worden geleverd door de [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. Volg een [snelstart](#next-steps) om aan de slag te gaan.


### <a name="tag-visual-features"></a>Visuele kenmerken taggen

Identificeer en tag visuele kenmerken in een afbeelding op basis van een set met duizenden herkenbare objecten, levende wezens, landschappen en acties. Als de tags dubbelzinnig of niet algemeen bekend zijn, worden via de API-reactie tips gegeven om de context van de tag te verduidelijken. U kunt tagging niet alleen gebruiken voor het hoofdonderwerp, zoals een persoon op de voorgrond, maar ook voor de omgeving (binnen of buiten), meubels, gereedschap, planten, dieren, accessoires, gadgets en enzovoort. [Visuele kenmerken taggen](concept-tagging-images.md)

### <a name="detect-objects"></a>Objecten detecteren

Objectdetectie is vergelijkbaar met het gebruik van tags, maar de API retourneert de coördinaten van de omsluitende box voor elke tag die wordt toegepast. Als een afbeelding bijvoorbeeld een hond, kat en persoon bevat, zal de detectiebewerking deze objecten vermelden, samen met hun coördinaten in de afbeelding. U kunt deze functie gebruiken om verdere relaties tussen de objecten in een afbeelding te verwerken. Ook weet u daardoor wanneer er meerdere exemplaren van dezelfde tag in een afbeelding voorkomen. [Objecten detecteren](concept-object-detection.md)

### <a name="detect-brands"></a>Merken detecteren

Identificeer commerciële merken in afbeeldingen of video's met behulp van een database met duizenden logo's. U kunt deze functie bijvoorbeeld gebruiken om te ontdekken welke merken het populairst zijn op sociale media of het meest voorkomen in productplaatsing in de media. [Merken detecteren](concept-brand-detection.md)

### <a name="categorize-an-image"></a>Een afbeelding categoriseren

Identificeer en categoriseer een volledige afbeelding met behulp van een [categorietaxonomie](Category-Taxonomy.md) met bovenliggende/onderliggende erfelijke hiërarchieën. Categorieën kunnen zelfstandig worden gebruikt of met onze nieuwe tagmodellen.<br/>Engels is momenteel de enige ondersteunde taal voor het taggen en categoriseren van afbeeldingen. [Een afbeelding categoriseren](concept-categorizing-images.md)

### <a name="describe-an-image"></a>Een afbeelding beschrijven

Genereer een beschrijving van een volledige afbeelding in leesbare taal met behulp van volledige zinnen. Met de algoritmen van Computer Vision worden verschillende beschrijvingen gegenereerd op basis van de objecten die zijn geïdentificeerd in de afbeelding. De beschrijvingen worden geëvalueerd en hiervoor wordt een betrouwbaarheidsscore gegenereerd. Vervolgens wordt er een lijst geretourneerd die is geordend van de hoogste naar de laagste betrouwbaarheidsscore. [Een afbeelding beschrijven](concept-describing-images.md)

### <a name="detect-faces"></a>Gezichten detecteren

Detecteer gezichten in een afbeelding en geef informatie op over elk gedetecteerd gezicht. Met Computer Vision worden de coördinaten, de rechthoek, het geslacht en de leeftijd van elk gedetecteerd gezicht geretourneerd.<br/>Computer Vision biedt een subset van de servicefunctionaliteit [Face](/azure/cognitive-services/face/). U kunt de Face-service gebruiken voor een gedetailleerdere analyse, zoals gezichtsidentificatie en posedetectie. [Gezichten detecteren](concept-detecting-faces.md)

### <a name="detect-image-types"></a>Afbeeldingstypen detecteren

Detecteer kenmerken van een afbeelding, bijvoorbeeld of een afbeelding een lijntekening of een illustratie is. [Afbeeldingstypen detecteren](concept-detecting-image-types.md)

### <a name="detect-domain-specific-content"></a>Domeinspecifieke inhoud detecteren

Gebruik domeinmodellen om domeinspecifieke inhoud in een afbeelding te detecteren en te identificeren, zoals beroemdheden en oriëntatiepunten. Als een afbeelding bijvoorbeeld mensen bevat, kan Computer Vision een domeinmodel gebruiken voor beroemdheden om te bepalen of de personen die in de afbeelding worden gedetecteerd, overeenkomen met bekende beroemdheden. [Domeinspecifieke inhoud detecteren](concept-detecting-domain-content.md)

### <a name="detect-the-color-scheme"></a>Het kleurenschema detecteren

Analyseer het kleurgebruik in een afbeelding. Met Computer Vision kan worden bepaald of een afbeelding zwart-wit of kleur is en voor kleurenafbeeldingen kunnen de dominante kleuren en accentkleuren worden geïdentificeerd. [Het kleurenschema detecteren](concept-detecting-color-schemes.md)

### <a name="generate-a-thumbnail"></a>Een miniatuur genereren

Analyseer de inhoud van een afbeelding om een geschikte miniatuur voor deze afbeelding te genereren. Computer Vision genereert eerst een miniatuur van hoge kwaliteit, waarna de objecten in de afbeelding worden geanalyseerd om het *interessegebied* te bepalen. Computer Vision past de afbeelding vervolgens aan de vereisten van het interessegebied aan. De gegenereerde miniatuur kan worden weergegeven met een andere hoogte-breedteverhouding dan wordt gebruikt in de oorspronkelijke afbeelding, afhankelijk van uw behoeften. [Een miniatuur genereren](concept-generating-thumbnails.md)

### <a name="get-the-area-of-interest"></a>Interessegebied ophalen

Analyseer de inhoud van een afbeelding om de coördinaten van het *interessegebied* te retourneren. In plaats van de miniatuur bij te snijden en te genereren, retourneert Computer Vision de coördinaten van het begrenzingsvak van het gebied, zodat de aanroepende toepassing de oorspronkelijke afbeelding naar wens kan wijzigen. [Interessegebied ophalen](concept-generating-thumbnails.md#area-of-interest)

## <a name="moderate-content-in-images"></a>Beheren van inhoud in afbeeldingen

U kunt Computer Vision gebruiken om [erotische inhoud te detecteren](concept-detecting-adult-content.md) in een afbeelding en vertrouwensscores voor verschillende classificaties te retourneren. De drempel voor het markeren van inhoud kan worden ingesteld met een glijdende schaal om uw voorkeuren aan te geven.

## <a name="use-containers"></a>Containers gebruiken

Gebruik Docker-containers van Computer Vision om de service on-premises te gebruiken. Er zijn momenteel twee containers beschikbaar:

* Met de [Computer Vision-leescontainer](computer-vision-how-to-install-containers.md) kunt u gedrukte en handgeschreven tekst in afbeeldingen herkennen.

* Met de [container voor ruimtelijke analyse van Computer Vision](spatial-analysis-container.md) kunt u realtime streaming-video analyseren om inzicht te krijgen in de ruimtelijke relaties tussen mensen en hun verplaatsing door fysieke omgevingen.

## <a name="image-requirements"></a>Vereisten voor installatiekopieën

Met Computer Vision kunt u afbeeldingen analyseren die voldoen aan de volgende vereisten:

- De afbeelding moet worden weergegeven in de JPEG-, PNG-, GIF- of BMP-indeling
- De afbeelding moet kleiner zijn dan 4 megabyte (MB)
- De afmetingen van de afbeelding moet groter zijn dan 50 x 50 pixels
  - Voor de Read-API moet de afbeelding tussen de 50 x 50 en 10000 x 10000 pixels groot zijn.

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle Cognitive Services, dienen ontwikkelaars die de Computer Vision-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Computer Vision door een quickstart te volgen:

- [Snelstart: Computer Vision .NET-clientbibliotheek](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Snelstart: Computer Vision-clientbibliotheek voor Python](./quickstarts-sdk/client-library.md?pivots=programming-language-python)
- [Snelstart: Computer Vision-clientbibliotheek voor Java](./quickstarts-sdk/client-library.md?pivots=programming-language-java)
