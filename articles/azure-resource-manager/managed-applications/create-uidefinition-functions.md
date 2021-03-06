---
title: UI-definitie functies maken
description: Hierin worden de functies beschreven die moeten worden gebruikt bij het maken van UI-definities voor Azure Managed Applications
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: e4255f0d42e28a72ad55d9b7f81d0dc49b2950cb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87040992"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition-functies

Dit artikel bevat een overzicht van de ondersteunde functies voor CreateUiDefinition.

## <a name="function-syntax"></a>Syntaxis van functie

Als u een functie wilt gebruiken, plaatst u de aanroep tussen vier Kante haken. Bijvoorbeeld:

```json
"[function()]"
```

Naar teken reeksen en andere functies kan worden verwezen als para meters voor een functie, maar teken reeksen moeten tussen enkele aanhalings tekens worden geplaatst. Bijvoorbeeld:

```json
"[fn1(fn2(), 'demo text')]"
```

Waar van toepassing kunt u verwijzen naar eigenschappen van de uitvoer van een functie met behulp van de punt operator. Bijvoorbeeld:

```json
"[func().prop1]"
```

## <a name="collection-functions"></a>Verzamelings functies

Deze functies kunnen worden gebruikt in combi natie met verzamelingen, zoals JSON-teken reeksen, matrices en objecten.

* [daarin](create-ui-definition-collection-functions.md#contains)
* [leeg](create-ui-definition-collection-functions.md#empty)
* [Filterwebonderdelen](create-ui-definition-collection-functions.md#filter)
* [instantie](create-ui-definition-collection-functions.md#first)
* [duren](create-ui-definition-collection-functions.md#last)
* [lange](create-ui-definition-collection-functions.md#length)
* [diagram](create-ui-definition-collection-functions.md#map)
* [verdergaan](create-ui-definition-collection-functions.md#skip)
* [delen](create-ui-definition-collection-functions.md#split)
* [take](create-ui-definition-collection-functions.md#take)

## <a name="conversion-functions"></a>Conversie functies

Deze functies kunnen worden gebruikt om waarden te converteren tussen JSON-gegevens typen en-code ringen.

* [booleaans](create-ui-definition-conversion-functions.md#bool)
* [decodeBase64](create-ui-definition-conversion-functions.md#decodebase64)
* [decodeUriComponent](create-ui-definition-conversion-functions.md#decodeuricomponent)
* [encodeBase64](create-ui-definition-conversion-functions.md#encodebase64)
* [encodeUriComponent](create-ui-definition-conversion-functions.md#encodeuricomponent)
* [float](create-ui-definition-conversion-functions.md#float)
* [int](create-ui-definition-conversion-functions.md#int)
* [parseren](create-ui-definition-conversion-functions.md#parse)
* [tekenreeksexpressie](create-ui-definition-conversion-functions.md#string)

## <a name="date-functions"></a>Datumfuncties

* [addHours](create-ui-definition-date-functions.md#addhours)
* [addMinutes](create-ui-definition-date-functions.md#addminutes)
* [addSeconds](create-ui-definition-date-functions.md#addseconds)
* [utcNow](create-ui-definition-date-functions.md#utcnow)

## <a name="logical-functions"></a>Logische functies

Deze functies kunnen worden gebruikt in voor waarden. Sommige functies bieden mogelijk geen ondersteuning voor alle JSON-gegevens typen.

* [en](create-ui-definition-logical-functions.md#and)
* [Voeg](create-ui-definition-logical-functions.md#coalesce)
* [gelijk is aan](create-ui-definition-logical-functions.md#equals)
* [groter](create-ui-definition-logical-functions.md#greater)
* [greaterOrEquals](create-ui-definition-logical-functions.md#greaterorequals)
* [If](create-ui-definition-logical-functions.md#if)
* [less](create-ui-definition-logical-functions.md#less)
* [lessOrEquals](create-ui-definition-logical-functions.md#lessorequals)
* [ten](create-ui-definition-logical-functions.md#not)
* [or](create-ui-definition-logical-functions.md#or)

## <a name="math-functions"></a>Wiskundige functies

* [toe](create-ui-definition-math-functions.md#add)
* [ceil](create-ui-definition-math-functions.md#ceil)
* [div](create-ui-definition-math-functions.md#div)
* [Floor](create-ui-definition-math-functions.md#floor)
* [aantal](create-ui-definition-math-functions.md#max)
* [min.](create-ui-definition-math-functions.md#min)
* [mod](create-ui-definition-math-functions.md#mod)
* [mul](create-ui-definition-math-functions.md#mul)
* [ASELECT](create-ui-definition-math-functions.md#rand)
* [bereik](create-ui-definition-math-functions.md#range)
* [sub](create-ui-definition-math-functions.md#sub)

## <a name="referencing-functions"></a>Verwijzende functies

* [bewerkingen](create-ui-definition-referencing-functions.md#basics)
* [locatie](create-ui-definition-referencing-functions.md#location)
* [resourceGroup](create-ui-definition-referencing-functions.md#resourcegroup)
* [stappen](create-ui-definition-referencing-functions.md#steps)
* [abonnees](create-ui-definition-referencing-functions.md#subscription)

## <a name="string-functions"></a>Tekenreeksfuncties

* [concat](create-ui-definition-string-functions.md#concat)
* [endsWith](create-ui-definition-string-functions.md#endswith)
* [guid](create-ui-definition-string-functions.md#guid)
* [indexOf](create-ui-definition-string-functions.md#indexof)
* [lastIndexOf](create-ui-definition-string-functions.md#lastindexof)
* [vervangen](create-ui-definition-string-functions.md#replace)
* [startsWith](create-ui-definition-string-functions.md#startswith)
* [subtekenreeks](create-ui-definition-string-functions.md#substring)
* [toLower](create-ui-definition-string-functions.md#tolower)
* [toUpper](create-ui-definition-string-functions.md#toupper)

## <a name="next-steps"></a>Volgende stappen

* Zie [overzicht van Azure Resource Manager](../management/overview.md)voor een inleiding tot Azure Resource Manager.
