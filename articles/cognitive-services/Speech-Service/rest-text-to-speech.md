---
title: Naslag informatie voor de tekst-naar-spraak-API (REST)-Speech Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van de tekst-naar-spraak-REST API. In dit artikel vindt u meer informatie over autorisatie opties, query opties, het structureren van een aanvraag en het ontvangen van een reactie.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: c7c43e02e6bdf75c9551ccdbb9dd8f75bf37a806
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/29/2020
ms.locfileid: "91534978"
---
# <a name="text-to-speech-rest-api"></a>REST API voor tekst-naar-spraak

Met de spraak service kunt u [tekst omzetten in gesynthesizerde spraak](#convert-text-to-speech) en [een lijst met ondersteunde stemmen](#get-a-list-of-voices) voor een regio ophalen met behulp van een set rest-api's. Elk beschikbaar eind punt is gekoppeld aan een regio. Er is een abonnements sleutel vereist voor het eind punt/de regio die u wilt gebruiken.

De tekst-naar-spraak-REST API ondersteunt Neural-en standaard tekst-naar-spraak-stemmen, die elk een specifieke taal en dialect ondersteunen, geïdentificeerd door land instellingen.

* Zie [taal ondersteuning](language-support.md#text-to-speech)voor een volledige lijst met stemmen.
* Zie [regio's](regions.md#text-to-speech)voor meer informatie over regionale Beschik baarheid.

> [!IMPORTANT]
> De kosten variëren per standaard, aangepast en Neural stemmen. Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)voor meer informatie.

Voordat u deze API gebruikt, moet u het volgende weten:

* De tekst-naar-spraak-REST API vereist een autorisatie-header. Dit betekent dat u een token uitwisseling moet volt ooien om toegang te krijgen tot de service. Zie [Verificatie](#authentication) voor meer informatie.

> [!TIP]
> Zie de [documentatie](https://docs.microsoft.com/azure/azure-government/compare-azure-government-global-azure) van Azure Government voor Government Cloud (FairFax)-eind punten.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Een lijst met stemmen ophalen

`voices/list`Met het eind punt kunt u een volledige lijst met stemmen voor een specifieke regio/eind punt ophalen.

### <a name="regions-and-endpoints"></a>Regio's en eind punten

| Region | Eindpunt |
|--------|----------|
| Australië - oost | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Brazilië - zuid | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Canada - midden | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - centraal | `https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Azië - oost | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - oost | `https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - oost 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Frankrijk - centraal | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| India - centraal | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Japan East | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Korea - centraal | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - noord-centraal | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Europa - noord | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| South Central US | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Azië - zuidoost | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Verenigd Koninkrijk Zuid | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Europa -west | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - west | `https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| VS - west 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |

### <a name="request-headers"></a>Aanvraagheaders

In deze tabel vindt u de vereiste en optionele kopteksten voor tekst-naar-spraak-aanvragen.

| Koptekst | Beschrijving | Vereist/optioneel |
|--------|-------------|---------------------|
| `Authorization` | Een autorisatie token dat wordt voorafgegaan door het woord `Bearer` . Zie [Verificatie](#authentication) voor meer informatie. | Vereist |

### <a name="request-body"></a>Aanvraagbody

Een hoofd tekst is niet vereist voor `GET` aanvragen voor dit eind punt.

### <a name="sample-request"></a>Voorbeeld aanvraag

Voor deze aanvraag is alleen een autorisatie-header vereist.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Voorbeeldantwoord

Dit antwoord is afgekapt om de structuur van een antwoord te illustreren.

> [!NOTE]
> De beschik baarheid van de Voice varieert per regio/eind punt.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)",
        "ShortName": "zh-CN-XiaoxiaoNeural",
        "Gender": "Female",
        "Locale": "zh-CN",
        "SampleRateHertz": "24000",
        "VoiceType": "Neural"
    },

    ...
]
```

### <a name="http-status-codes"></a>HTTP-statuscode

De HTTP-status code voor elke reactie wijst op geslaagde of veelvoorkomende fouten.

| HTTP-statuscode | Beschrijving | Mogelijke reden |
|------------------|-------------|-----------------|
| 200 | OK | De aanvraag is voltooid. |
| 400 | Onjuiste aanvraag | Een vereiste para meter ontbreekt, is leeg of null. Of de waarde die is door gegeven aan een vereiste of optionele para meter is ongeldig. Een veelvoorkomend probleem is een header die te lang is. |
| 401 | Niet geautoriseerd | De aanvraag is niet geautoriseerd. Controleer of de abonnements sleutel of het token geldig is en in de juiste regio is. |
| 429 | Te veel aanvragen | U hebt het quotum of de frequentie overschreden van aanvragen die zijn toegestaan voor uw abonnement. |
| 502 | Ongeldige gateway    | Probleem met het netwerk of de server. Kan ook duiden op ongeldige headers. |


## <a name="convert-text-to-speech"></a>Tekst naar spraak converteren

`v1`Met het eind punt kunt u tekst naar spraak converteren met behulp van [SSML (Speech synthese Markup Language)](speech-synthesis-markup.md).

### <a name="regions-and-endpoints"></a>Regio's en eind punten

Deze regio's worden ondersteund voor tekst naar spraak met behulp van de REST API. Zorg ervoor dat u het eind punt selecteert dat overeenkomt met de regio van uw abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>Aanvraagheaders

In deze tabel vindt u de vereiste en optionele kopteksten voor tekst-naar-spraak-aanvragen.

| Koptekst | Beschrijving | Vereist/optioneel |
|--------|-------------|---------------------|
| `Authorization` | Een autorisatie token dat wordt voorafgegaan door het woord `Bearer` . Zie [Verificatie](#authentication) voor meer informatie. | Vereist |
| `Content-Type` | Hiermee geeft u het type inhoud op voor de opgegeven tekst. Geaccepteerde waarde: `application/ssml+xml` . | Vereist |
| `X-Microsoft-OutputFormat` | Hiermee geeft u de indeling van de audio-uitvoer. Zie [audio-uitvoer](#audio-outputs)voor een volledige lijst met geaccepteerde waarden. | Vereist |
| `User-Agent` | De toepassingsnaam. De gegeven waarde moet kleiner zijn dan 255 tekens. | Vereist |

### <a name="audio-outputs"></a>Audio-uitvoer

Dit is een lijst met ondersteunde audio-indelingen die in elke aanvraag als de header worden verzonden `X-Microsoft-OutputFormat` . Elk bevat een bitsnelheid en coderings type. De spraak service ondersteunt 24 kHz, 16 kHz en 8 kHz audio-uitvoer.

```output
raw-16khz-16bit-mono-pcm            raw-8khz-8bit-mono-mulaw
riff-8khz-8bit-mono-alaw            riff-8khz-8bit-mono-mulaw
riff-16khz-16bit-mono-pcm           audio-16khz-128kbitrate-mono-mp3
audio-16khz-64kbitrate-mono-mp3     audio-16khz-32kbitrate-mono-mp3
raw-24khz-16bit-mono-pcm            riff-24khz-16bit-mono-pcm
audio-24khz-160kbitrate-mono-mp3    audio-24khz-96kbitrate-mono-mp3
audio-24khz-48kbitrate-mono-mp3     ogg-24khz-16bit-mono-opus
```

> [!NOTE]
> Als uw geselecteerde spraak-en uitvoer indeling verschillende bitsnelheden heeft, wordt de steek proef van de audio indien nodig opnieuw uitgevoerd. Ogg-24khz-16-mono-Opus kan worden gedecodeerd met [Opus-codec](https://opus-codec.org/downloads/)

### <a name="request-body"></a>Aanvraagbody

De hoofd tekst van elke `POST` aanvraag wordt verzonden als [SSML (Speech synthese Markup Language)](speech-synthesis-markup.md). Met SSML kunt u de stem en taal kiezen van de gesynthesizerde spraak die wordt geretourneerd door de service tekst naar spraak. Zie [taal ondersteuning](language-support.md#text-to-speech)voor een volledige lijst met ondersteunde stemmen.

> [!NOTE]
> Als u een aangepaste stem gebruikt, kan de hoofd tekst van een aanvraag worden verzonden als tekst zonder opmaak (ASCII of UTF-8).

### <a name="sample-request"></a>Voorbeeld aanvraag

Deze HTTP-aanvraag gebruikt SSML om de stem en taal op te geven. Als de lengte van de hoofd tekst lang is en de resulterende Audio langer is dan 10 minuten, wordt deze afgebroken tot 10 minuten. Met andere woorden, de audio lengte mag niet langer zijn dan 10 minuten.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='en-US-AriaRUS'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Bekijk onze Quick starts voor taalspecifieke voor beelden:

* [.NET core, C #](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=dotnetcore)
* [Python](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-python)
* [Node.js](quickstart-nodejs-text-to-speech.md)

### <a name="http-status-codes"></a>HTTP-statuscode

De HTTP-status code voor elke reactie wijst op geslaagde of veelvoorkomende fouten.

| HTTP-statuscode | Beschrijving | Mogelijke reden |
|------------------|-------------|-----------------|
| 200 | OK | De aanvraag is voltooid. de antwoord tekst is een audio bestand. |
| 400 | Onjuiste aanvraag | Een vereiste para meter ontbreekt, is leeg of null. Of de waarde die is door gegeven aan een vereiste of optionele para meter is ongeldig. Een veelvoorkomend probleem is een header die te lang is. |
| 401 | Niet geautoriseerd | De aanvraag is niet geautoriseerd. Controleer of de abonnements sleutel of het token geldig is en in de juiste regio is. |
| 413 | De aanvraag entiteit is te groot | De SSML-invoer is langer dan 1024 tekens. |
| 415 | Niet-ondersteund media type | Het is mogelijk dat de verkeerde naam `Content-Type` is gegeven. `Content-Type` moet worden ingesteld op `application/ssml+xml` . |
| 429 | Te veel aanvragen | U hebt het quotum of de frequentie overschreden van aanvragen die zijn toegestaan voor uw abonnement. |
| 502 | Ongeldige gateway    | Probleem met het netwerk of de server. Kan ook duiden op ongeldige headers. |

Als de HTTP-status is `200 OK` , bevat de hoofd tekst van het antwoord een audio bestand met de aangevraagde indeling. Dit bestand kan worden afgespeeld wanneer het wordt overgedragen, wordt opgeslagen in een buffer of wordt opgeslagen in een bestand.

## <a name="next-steps"></a>Volgende stappen

- [Een gratis Azure-account maken](https://azure.microsoft.com/free/cognitive-services/)
- [Asynchrone synthese voor lange-vorm audio](quickstarts/text-to-speech/async-synthesis-long-form-audio.md)
- [Aan de slag met Custom Voice](how-to-custom-voice.md)
