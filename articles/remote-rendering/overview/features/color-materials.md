---
title: Kleurmaterialen
description: Hiermee wordt het type kleur materiaal beschreven.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: cda4aa9a811bac0ccf20caec32ee38da9b46b6c7
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613775"
---
# <a name="color-materials"></a>Kleurmaterialen

*Kleur materiaal* is een van de ondersteunde [materiaal typen](../../concepts/materials.md) in azure rendering op afstand. Ze worden gebruikt voor [netten](../../concepts/meshes.md) die geen soort verlichting mogen ontvangen, maar moeten te allen tijde volledige helderheid zijn. Dit kan het geval zijn bij het ' oplopen ' van materialen, zoals auto's, lampen, gloei lampen of gegevens die al een statische verlichting in beslag neemt, zoals modellen die zijn verkregen via [Photogrammetry](https://en.wikipedia.org/wiki/Photogrammetry).

Kleur materiaal is efficiënter om weer te geven dan [PBR-materialen](pbr-materials.md) vanwege het gecompliceerde arcerings model. Ze ondersteunen ook verschillende transparantie modi.

## <a name="common-material-properties"></a>Algemene materiaal eigenschappen

Deze eigenschappen zijn gebruikelijk voor alle materialen:

* **albedoColor:** Deze kleur wordt vermenigvuldigd met andere kleuren, zoals de *albedoMap* of * :::no-loc text="vertex"::: kleuren*. Als *transparantie* is ingeschakeld voor een materiaal, wordt het Alfa kanaal gebruikt voor het aanpassen van de dekking, met een `1` volledig ondoorzichtigheid en duidelijk `0` doorzichtig. De standaard waarde is wit.

  > [!NOTE]
  > Omdat kleur materialen niet overeenkomen met de omgeving, wordt een volledig transparant kleuren materiaal onzichtbaar. Dit wijkt af van de [PBR-materialen](pbr-materials.md).

* **albedoMap:** Een [2D-patroon](../../concepts/textures.md) voor albedo waarden per pixel.

* **alphaClipEnabled** en **AlphaClipThreshold:** als *alphaClipEnabled* is ingesteld op True, worden alle pixels waarvan de waarde van albedo alpha lager is dan *alphaClipThreshold* niet getekend. Alfa knipsel kan zelfs worden gebruikt zonder transparantie in te scha kelen en is veel sneller te renderen. Afgekapte Alfa materialen zijn nog steeds langzamer dan volledig dekkende materialen, maar. Alpha-knipsels zijn standaard uitgeschakeld.

* **textureCoordinateScale** en **textureCoordinateOffset:** de schaal wordt vermenigvuldigd met de UV-textuur coördinaten, de offset wordt hieraan toegevoegd. Kan worden gebruikt om de bitmappatronen te spreiden en te verschuiven. De standaard schaal is (1, 1) en de offset is (0, 0).

* **useVertexColor:** Als de mesh :::no-loc text="vertex"::: kleuren bevat en deze optie is ingeschakeld, wordt de kleur van de netten :::no-loc text="vertex"::: vermenigvuldigd met de *albedoColor* en *albedoMap*. Standaard is *useVertexColor* uitgeschakeld.

* **isDoubleSided:** Als Double-sidedness is ingesteld op True, worden drie hoeken met dit materiaal weer gegeven, zelfs als de camera op de achtergrond is. Deze optie is standaard uitgeschakeld. Zie ook [ :::no-loc text="Single-sided"::: rendering](single-sided-rendering.md).

## <a name="color-material-properties"></a>Eigenschappen van kleur materiaal

De volgende eigenschappen zijn specifiek voor kleuren materialen:

* **vertexMix:** Deze waarde tussen `0` en `1` geeft aan hoe sterk de :::no-loc text="vertex"::: kleur in een [net](../../concepts/meshes.md) bijdraagt aan de uiteindelijke kleur. Bij de standaard waarde van 1 wordt de :::no-loc text="vertex"::: kleur in de albedo-kleuren volledig vermenigvuldigd. Met de waarde 0 :::no-loc text="vertex"::: worden de kleuren volledig genegeerd.

* **transparencyMode:** In tegens telling tot [PBR-materialen](pbr-materials.md), onderscheidt kleur materiaal van verschillende transparantie modi:

  1. **Dekkend:** In de standaard modus wordt transparantie uitgeschakeld. Alpha-knipsels zijn echter nog steeds mogelijk, en moeten daarom als voldoende worden aangeraden.
  
  1. **AlphaBlended:** Deze modus is vergelijkbaar met de transparantie modus voor PBR-materialen. Dit moet worden gebruikt voor lees bare materialen zoals glas.

  1. **Additief:** Deze modus is de eenvoudigste en meest efficiënte transparantie modus. De bijdrage van het materiaal wordt toegevoegd aan de gerenderde afbeelding. Deze modus kan worden gebruikt voor het simuleren van glanzende objecten (maar wel transparant), zoals markeringen die worden gebruikt voor het markeren van belang rijke objecten.

## <a name="api-documentation"></a>API-documentatie

* [C# ColorMaterial-klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.colormaterial)
* [C# RemoteManager. CreateMaterial ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.creatematerial)
* [C++ ColorMaterial-klasse](https://docs.microsoft.com/cpp/api/remote-rendering/colormaterial)
* [C++ RemoteManager:: CreateMaterial ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#creatematerial)

## <a name="next-steps"></a>Volgende stappen

* [PBR-materialen](pbr-materials.md)
* [Patronen](../../concepts/textures.md)
* [Meshes](../../concepts/meshes.md)
