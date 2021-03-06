---
title: Uw Classifier-Custom Vision Service verbeteren
titleSuffix: Azure Cognitive Services
description: In dit artikel leert u hoe het aantal, de kwaliteit en de verscheidenheid aan gegevens de kwaliteit van uw classificatie in de Custom Vision-service kunnen verbeteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: a77d3d5c1225fdd85e27db20cdae23e0c77a5e28
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91271355"
---
# <a name="how-to-improve-your-classifier"></a>De classificatie verbeteren

In deze hand leiding wordt uitgelegd hoe u de kwaliteit van uw Custom Vision Service classificatie kunt verbeteren. De kwaliteit van uw classificatie is afhankelijk van het aantal, de kwaliteit en de verscheidenheid van de gelabelde gegevens die u opgeeft en de verdeling van de algemene gegevensset. Een goede classificatie heeft een gebalanceerde trainings gegevensset die representatief is voor wat wordt verzonden naar de classificatie. Het proces van het bouwen van een dergelijke classificatie is een herhaling; het is gebruikelijk om enkele ronden te volgen om de verwachte resultaten te bereiken.

Hier volgt een algemeen patroon waarmee u een nauw keurigere classificatie kunt bouwen:

1. First-Round training
1. Meer installatie kopieën en balans gegevens toevoegen; opnieuw trainen
1. Voeg afbeeldingen met verschillende achtergrond, belichting, object grootte, camera hoek en stijl toe. opnieuw trainen
1. Nieuwe afbeelding (en) gebruiken om voor spelling te testen
1. Bestaande trainings gegevens aanpassen op basis van voorspellings resultaten

## <a name="prevent-overfitting"></a>Overmontage voor komen

Soms leert een classificatie om voor spellingen te maken op basis van wille keurige kenmerken die uw afbeeldingen gemeen hebben. Als u bijvoorbeeld een classificatie maakt voor Apples vs. citrus en u afbeeldingen van appels in handen hebt en van citrus op witte platen hebt gebruikt, kan de classificatie onterecht belang rijk zijn voor handen van de hand, in plaats van met Apples vs. Citrus.

![Afbeelding van onverwachte classificatie](./media/getting-started-improving-your-classifier/unexpected.png)

U kunt dit probleem verhelpen met behulp van de volgende richt lijnen voor training met meer gevarieerde afbeeldingen: bieden afbeeldingen met verschillende hoeken, achtergronden, object grootte, groepen en andere variaties.

## <a name="data-quantity"></a>Hoeveelheid gegevens

Het aantal trainings installatie kopieën is de belangrijkste factor. U kunt het beste ten minste 50 installatie kopieën per label gebruiken als uitgangs punt. Als er minder afbeeldingen zijn, is er een hoger risico op de overmontage en terwijl uw prestatie cijfers een goede kwaliteit kunnen Voorst Ellen, kan uw model moeilijk met echte gegevens worden gerealiseerd. 

## <a name="data-balance"></a>Gegevens saldo

Het is ook belang rijk dat u rekening houdt met de relatieve hoeveel heden van uw trainings gegevens. Bijvoorbeeld: met 500 installatie kopieën voor één label en 50 installatie kopieën voor een ander label wordt een niet-sluitende trainings gegevensset gemaakt. Dit zorgt ervoor dat het model nauw keuriger is bij het voors pellen van één label dan het andere. U ziet waarschijnlijk betere resultaten als u ten minste een 1:2-verhouding tussen het label en de minste afbeeldingen en het label met de meeste afbeeldingen wilt behouden. Als het label met de meeste installatie kopieën 500 installatie kopieën heeft, moet het label met de minste installatie kopieën ten minste 250 installatie kopieën voor de training hebben.

## <a name="data-variety"></a>Gegevens soort

Zorg ervoor dat u installatie kopieën gebruikt die representatief zijn voor wat wordt verzonden naar de classificatie tijdens normaal gebruik. Als u dit niet doet, kan de classificatie meer informatie geven over de voor spellingen op basis van wille keurige kenmerken die uw afbeeldingen gemeen hebben. Als u bijvoorbeeld een classificatie maakt voor Apples vs. citrus en u afbeeldingen van appels in handen hebt en van citrus op witte platen hebt gebruikt, kan de classificatie onterecht belang rijk zijn voor handen van de hand, in plaats van met Apples vs. Citrus.

![Afbeelding van onverwachte classificatie](./media/getting-started-improving-your-classifier/unexpected.png)

U kunt dit probleem oplossen door diverse installatie kopieën te gebruiken om ervoor te zorgen dat uw classificatie goed kan worden gegeneraliseerd. Hieronder vindt u enkele manieren waarop u uw training kunt instellen:

* __Achtergrond:__ Geef afbeeldingen van uw object voor de verschillende achtergronden op. Foto's in natuurlijke contexten zijn beter dan Foto's voor neutrale achtergronden, aangezien ze meer informatie over de classificatie bieden.

    ![Afbeelding van achtergrond voorbeelden](./media/getting-started-improving-your-classifier/background.png)

* __Verlichting:__ Bied installatie kopieën met een variabele verlichting (die wordt gemaakt met Flash, hoge bloot stelling enzovoort), met name als de installatie kopieën die worden gebruikt voor de voor spelling, een andere belichting hebben. Het is ook handig om installatie kopieën te gebruiken met een variërende intensiteit, kleur Toon en helderheid.

    ![Afbeelding van belichtings voorbeelden](./media/getting-started-improving-your-classifier/lighting.png)

* __Object grootte:__ Geef installatie kopieën op waarin de objecten qua grootte en cijfer variëren (bijvoorbeeld een foto van een aantal bananen en een close van één bananen). Met een andere grootte kunt u de classificatie generalize verbeteren.

    ![Afbeelding van grootte voorbeelden](./media/getting-started-improving-your-classifier/size.png)

* __Camera hoek:__ Geef installatie kopieën op die zijn gemaakt met verschillende camera hoeken. Als al uw Foto's moeten worden uitgevoerd met vaste camera's (zoals surveillance camera's), moet u er ook voor zorgen dat u een ander label toewijst aan elk regel matig object, om te voor komen dat niet- &mdash; gerelateerde objecten (zoals lampposts) worden geïnterpreteerd als de belangrijkste functie.

    ![Afbeelding van hoek voorbeelden](./media/getting-started-improving-your-classifier/angle.png)

* __Stijl:__ Geef afbeeldingen van verschillende stijlen van dezelfde klasse op (bijvoorbeeld verschillende variëteiten van hetzelfde fruit). Als u echter objecten van zeer verschillende stijlen hebt (zoals Mickey Mouse versus een Real-Life muis), raden we u aan ze als afzonderlijke klassen aan te duiden, zodat ze beter hun afzonderlijke functies vertegenwoordigen.

    ![Afbeelding van stijl voorbeelden](./media/getting-started-improving-your-classifier/style.png)

## <a name="negative-images"></a>Negatieve afbeeldingen

Op een bepaald moment in uw project moet u mogelijk negatieve voor _beelden_ toevoegen om uw classificatie nauw keuriger te maken. Negatieve steek proeven zijn de voor beelden die niet overeenkomen met een van de andere tags. Wanneer u deze installatie kopieën uploadt, moet u de speciale **negatieve** label hierop Toep assen.

> [!NOTE]
> Het Custom Vision Service ondersteunt een automatische negatieve verwerking van de afbeelding. Als u bijvoorbeeld een wijn van druiven versus bananen bouwt en een afbeelding van een schoenen voor voor spelling verzendt, moet de classificatie die afbeelding even nauw keurig afzetten tot 0% voor zowel druiven als bananen.
> 
> Aan de andere kant, in gevallen waarin de negatieve installatie kopieën slechts een variant zijn van de afbeeldingen die in de training worden gebruikt, is het waarschijnlijk dat het model de negatieve afbeeldingen als een klasse met labels geclassificeerd als gevolg van de fantastische overeenkomsten. Als u bijvoorbeeld een oranje-en grapefruit-classificatie hebt en u een afbeelding van een Clementine invoert, kan de Clementine als oranje worden gezien, omdat veel kenmerken van de Clementine lijken op die van sinaasappels. Als uw negatieve installatie kopieën van deze aard zijn, raden we u aan om een of meer extra tags (zoals **andere**) te maken en de negatieve afbeeldingen met deze tag te labelen tijdens de training, zodat het model beter kan worden gedifferentieerd tussen deze klassen.

## <a name="use-prediction-images-for-further-training"></a>Voorspellings afbeeldingen gebruiken voor verdere training

Wanneer u de classificatie van de afbeelding gebruikt of test door installatie kopieën naar het Voorspellings eindpunt te verzenden, slaat de Custom Vision-service deze installatie kopieën op. U kunt ze vervolgens gebruiken om het model te verbeteren.

1. Als u afbeeldingen wilt weer geven die zijn verzonden naar de classificatie, opent u de [webpagina Custom Vision](https://customvision.ai), gaat u naar uw project en selecteert u het tabblad voor __spellingen__ . In de standaard weergave worden afbeeldingen van de huidige herhaling weer gegeven. U kunt de vervolg keuzelijst __iteratie__ gebruiken om afbeeldingen weer te geven die zijn verzonden tijdens vorige iteraties.

    ![scherm afbeelding van het tabblad voor spellingen met afbeeldingen in de weer gave](./media/getting-started-improving-your-classifier/predictions.png)

2. Beweeg de muis aanwijzer over een afbeelding om de labels te zien die zijn voor speld door de classificatie. Afbeeldingen worden gesorteerd, zodat de meeste verbeteringen van de classificatie worden weer gegeven. Als u een andere sorteer methode wilt gebruiken, maakt u een selectie in het gedeelte __sorteren__ . 

    Als u een afbeelding wilt toevoegen aan uw bestaande trainings gegevens, selecteert u de installatie kopie, stelt u de juiste code (s) in en klikt u op __opslaan en sluiten__. De afbeelding wordt verwijderd uit de voor __spellingen__ en toegevoegd aan de set trainings afbeeldingen. U kunt deze weer geven door het tabblad __trainings afbeeldingen__ te selecteren.

    ![Afbeelding van de pagina labelen](./media/getting-started-improving-your-classifier/tag.png)

3. Gebruik vervolgens de __trein__ knop om de classificatie opnieuw te trainen.

## <a name="visually-inspect-predictions"></a>Voor spellingen visueel controleren

Als u afbeeldings voorspellingen wilt controleren, gaat u naar het tabblad __trainings afbeeldingen__ , selecteert u uw vorige trainingens iteratie in de vervolg keuzelijst **iteratie** en controleert u een of meer tags onder de sectie **Tags** . In de weer gave wordt nu een rood vak weer gegeven rond elk van de afbeeldingen waarvoor het model de opgegeven tag niet correct kan voors pellen.

![Afbeelding van de iteratie geschiedenis](./media/getting-started-improving-your-classifier/iteration.png)

Soms kan een visuele inspectie patronen identificeren die u vervolgens kunt corrigeren door meer trainings gegevens toe te voegen of bestaande trainings gegevens te wijzigen. Bijvoorbeeld: een classificatie voor Apples vergeleken met kalkten kan alle groene appels onjuist labelen als kalkten. U kunt dit probleem vervolgens verhelpen door trainings gegevens toe te voegen en te voorzien die gelabelde afbeeldingen van groene Apple bevatten.

## <a name="next-steps"></a>Volgende stappen

In deze hand leiding hebt u verschillende technieken geleerd om uw aangepaste installatie kopie-classificatie model nauw keuriger te maken. Vervolgens leert u hoe u afbeeldingen programmatisch kunt testen door ze naar de Voorspellings-API te verzenden.

> [!div class="nextstepaction"]
> [De voorspellings-API gebruiken](use-prediction-api.md)
