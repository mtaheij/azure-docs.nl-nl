---
title: Overzicht van tekst naar spraak-spraak service
titleSuffix: Azure Cognitive Services
description: Met de functie voor tekst naar spraak in de speech-service kunt u uw toepassingen, hulpprogram ma's of apparaten gebruiken om tekst te converteren naar natuurlijke menselijke-achtige gesynthesizerde spraak. Dit artikel bevat een overzicht van de voor delen en mogelijkheden van de tekst naar spraak-service.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: trbye
ms.custom: cog-serv-seo-aug-2020
keywords: tekst naar spraak
ms.openlocfilehash: 5d60279a2e3cb6aa7226f518783d53a1a38ddaa8
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91357451"
---
# <a name="what-is-text-to-speech"></a>Wat is tekst-naar-spraak?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

In dit overzicht vindt u meer informatie over de voor delen en mogelijkheden van de tekst naar spraak-service, waarmee u uw toepassingen, hulpprogram ma's of apparaten tekst kunt converteren naar mensen met een menselijke ervaring. Kies uit standaard-en Neural stemmen of maak een aangepaste spraak die uniek is voor uw product of merk. 75 + standaard stemmen zijn verkrijgbaar in meer dan 45 talen en land instellingen en vijf Neural stemmen zijn beschikbaar in een geselecteerd aantal talen en land instellingen. Zie [ondersteunde talen](language-support.md#text-to-speech)voor een volledige lijst met ondersteunde stemmen, talen en land instellingen.

> [!NOTE]
> Bing Speech is uit bedrijf genomen op 15 oktober 2019. Als uw toepassingen, hulpprogram ma's of producten gebruikmaken van de Bing Speech Api's of Custom Speech, hebben we gidsen gemaakt om u te helpen migreren naar de speech-service.
> - [Migreren van Bing Speech naar de speech-service](how-to-migrate-from-bing-speech.md)

## <a name="core-features"></a>Kern functies

* Spraak-synthese: gebruik de [Speech SDK](quickstarts/text-to-speech-audio-file.md) of [rest API](rest-text-to-speech.md) om tekst naar spraak te converteren met behulp van standaard, Neural of aangepaste stemmen.

* Asynchrone synthese van lange audio: gebruik de [lange audio-API](long-audio-api.md) om tekst-naar-spraak-bestanden asynchroon te synthesizeren die langer zijn dan 10 minuten (bijvoorbeeld audio boeken of colleges). In tegens telling tot synthese die wordt uitgevoerd met behulp van de spraak-SDK of spraak-naar-tekst REST API, worden antwoorden niet in realtime geretourneerd. De verwachting is dat aanvragen asynchroon worden verzonden, reacties worden gepeild en dat de gesynthesizerde audio wordt gedownload wanneer deze beschikbaar wordt gesteld vanuit de service. Alleen aangepaste Neural stemmen worden ondersteund.

* Standaard stemmen-gemaakt met behulp van statistische parametrische synthese en/of samenvoeg synthese technieken. Deze stemmen zijn zeer begrijpelijk en klinkt natuurlijk. U kunt uw toepassingen eenvoudig laten spreken in meer dan 45 talen, met een breed scala aan spraak opties. Deze stemmen bieden een hoge nauw keurigheid van de uitspraak, inclusief ondersteuning voor afkortingen, acroniem uitbrei dingen, datum-en tijd interpretaties, telefoons en meer. Zie [ondersteunde talen](language-support.md#text-to-speech)voor een volledige lijst met standaard stemmen.

* Neural stemmen-diepe Neural-netwerken worden gebruikt voor het oplossen van de limieten van traditionele spraak synthese met betrekking tot stress en intonation in gesp roken taal. Prosody-voor spelling en spraak synthese worden gelijktijdig uitgevoerd, wat leidt tot meer vloei bare en natuurlijke geluids uitvoer. Neural stemmen kunnen worden gebruikt om interacties te maken met chat bots uitbreiden en spraak assistenten die natuurlijk en aantrekkelijker zijn, en om digitale teksten, zoals e-books, te converteren naar Audiobooks en de navigatie systemen in de auto te verbeteren. Met het menselijke net zoals natuurlijke prosody en heldere afbakening van woorden, verlaagt Neural stemmen veel luister intensief wanneer u met AI-systemen communiceert. Zie [ondersteunde talen](language-support.md#text-to-speech)voor een volledige lijst met Neural stemmen.

* SSML (Speech synthese Markup Language): een XML-opmaak taal die wordt gebruikt voor het aanpassen van de uitvoer van spraak naar tekst. Met SSML kunt u de Toon hoogte aanpassen, onderbrekingen toevoegen, de uitspraak verbeteren, de spraak snelheid verlagen of vertragen, het volume verg Roten of verkleinen, en het kenmerk meerdere stemmen op één document. Zie [SSML](speech-synthesis-markup.md).

## <a name="get-started"></a>Aan de slag

Bekijk de [Snelstartgids](get-started-text-to-speech.md) om aan de slag te gaan met tekst-naar-spraak. De tekst-naar-spraak-service is beschikbaar via de [Speech SDK](speech-sdk.md), het [rest API](rest-text-to-speech.md)en de [Speech cli](spx-overview.md)

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeld code voor tekst-naar-spraak is beschikbaar op GitHub. Deze voor beelden hebben betrekking op conversie van tekst naar spraak in de populairste programmeer talen.

- [Voor beelden van tekst naar spraak (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Voorbeelden van tekst-naar-spraak (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customization"></a>Aanpassing

Naast de standaard-en Neural stemmen, kunt u aangepaste stemmen maken en verfijnen die uniek zijn voor uw product of merk. Alles wat u nodig hebt om aan de slag te gaan zijn een aantal audio bestanden en de bijbehorende transcripties. Zie [aan de slag met aangepaste spraak](how-to-custom-voice.md) voor meer informatie.

## <a name="pricing-note"></a>Prijs notitie

Wanneer u de service tekst naar spraak gebruikt, wordt u gefactureerd voor elk teken dat naar spraak wordt geconverteerd, inclusief Lees tekens. Hoewel het SSML-document zelf niet factureerbaar is, worden optionele elementen die worden gebruikt voor het aanpassen van de manier waarop de tekst naar spraak wordt geconverteerd, zoals fonemen en pitch, geteld als factureer bare tekens. Hier volgt een lijst met wat factureerbaar is:

- Tekst die wordt door gegeven aan de service tekst naar spraak in de SSML-hoofd tekst van de aanvraag
- Alle opmaak in het tekst veld van de aanvraag tekst in de SSML-indeling, met uitzonde ring van de `<speak>` `<voice>` Tags en
- Letters, lees tekens, spaties, tabs, opmaak en alle spatie tekens
- Elk code punt dat in Unicode is gedefinieerd

Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)voor gedetailleerde informatie.

> [!IMPORTANT]
> Elk Chinees, Japans en Koreaans teken worden als twee tekens beschouwd voor facturering.

## <a name="reference-docs"></a>Naslagdocumentatie

- [Speech-SDK](speech-sdk.md)
- [REST API: Tekst-naar-spraak](rest-text-to-speech.md)

## <a name="next-steps"></a>Volgende stappen

- [Een gratis spraak service-abonnement ontvangen](overview.md#try-the-speech-service-for-free)
- [De Speech SDK ophalen](speech-sdk.md)
