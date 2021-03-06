---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: f8c7b9ee90e947534d6f938b1eb22f58b57270e6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376566"
---
:::row:::
    :::column span="3":::
        De Speech SDK voor Java script is beschikbaar als een NPM-pakket, Zie <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">micro soft-cognitiveservices- <span class="docon docon-navigate-external x-hidden-focus"></span> Speech-SDK</a> en de bijbehorende Companion github repository <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitieve-Services-speech <span class="docon docon-navigate-external x-hidden-focus"></span> -SDK-js </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Hoewel de Speech SDK voor Java script beschikbaar is als een NPM-pakket, kunnen webbrowsers en Node.js van de client dit gebruiken. Houd rekening met de verschillende architectuur implicaties van elke omgeving. Zo is het <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Document object model (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> niet beschikbaar voor server toepassingen, net zoals het <a href="https://nodejs.org/api/fs.html" target="_blank">Bestands systeem <span class="docon docon-navigate-external x-hidden-focus"></span> </a> niet beschikbaar is voor toepassingen op de client.

### <a name="nodejs-package-manager-npm"></a>Pakket beheer Node.js (NPM)

Voer de volgende opdracht uit om de Speech SDK voor Java script te installeren `npm install` .

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>HTML-script code

U kunt ook rechtstreeks een `<script>` tag in het HTML- `<head>` element toevoegen, afhankelijk van de <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank"> **JSDelivr** NPM Syndicate <span class="docon docon-navigate-external x-hidden-focus"></span> </a>.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@latest/distrib/browser/microsoft.cognitiveservices.speech.sdk.bundle-min.js">
</script>
```

Zie de <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">Quick <span class="docon docon-navigate-external x-hidden-focus"></span> Start voor de WEBBROWSER Speech SDK </a>voor meer informatie.
