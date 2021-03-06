---
title: Azure-abonnementsstatussen
description: Dit artikel beschrijft de verschillende statussen van een Azure-abonnement.
keywords: azure-abonnementsstatus
author: anuragdalmia
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: andalmia
ms.openlocfilehash: 70d03b4aeeaecee522552b381ce1309485719dc6
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686680"
---
# <a name="azure-subscription-states"></a>Azure-abonnementsstatussen

In dit artikel worden de verschillende statussen beschreven die een Azure-abonnement kan hebben. U vindt deze statussen als **Status** in de abonnementsblades in de Azure-portal.

| Abonnementsstatus | Beschrijving |
|-------------| ----------------|
| **Actief**/**Ingeschakeld** | Uw Azure-abonnement is actief. U kunt het abonnement gebruiken om nieuwe resources te implementeren en bestaande te beheren.<br><br>Alle bewerkingen (PUT, PATCH, DELETE, POST en GET) zijn beschikbaar voor de resourceproviders die zijn [geregistreerd voor uw abonnement](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). |
| **Verwijderd** | Uw Azure-abonnement is samen met alle onderliggende resources/gegevens verwijderd.<br><br>Er zijn geen bewerkingen beschikbaar. |
| **Uitgeschakeld** | Uw Azure-abonnement is uitgeschakeld en kan niet langer worden gebruikt om Azure-resources te maken of te beheren. In deze status wordt de toewijzing van uw virtuele machines ongedaan gemaakt, worden tijdelijke IP-adressen vrijgemaakt, is opslag alleen-lezen en worden andere services uitgeschakeld. Een abonnement kan om de volgende redenen worden uitgeschakeld: Uw tegoed is mogelijk verlopen. De bestedingslimiet is mogelijk bereikt. U hebt een achterstallige factuur. De limiet voor uw creditcard is overschreden. Of het abonnement is expliciet uitgeschakeld of geannuleerd. Afhankelijk van het abonnementstype kan een abonnement tussen de 1 en 90 dagen uitgeschakeld blijven. Daarna wordt het permanent verwijderd. Raadpleeg [Een uitgeschakeld Azure-abonnement opnieuw activeren](subscription-disabled.md) voor meer informatie.<br><br>Bewerkingen voor het aanmaken of bijwerken van resources (PUT, PATCH) zijn uitgeschakeld. Bewerkingen die een actie uitvoeren (POST), zijn ook uitgeschakeld. U kunt resources ophalen of verwijderen (GET, DELETE). Uw resources zijn nog steeds beschikbaar. |
| **Verlopen** | Uw Azure-abonnement is verlopen omdat het is geannuleerd. U kunt een verlopen abonnement opnieuw activeren. Raadpleeg [Een uitgeschakeld Azure-abonnement opnieuw activeren](subscription-disabled.md) voor meer informatie.<br><br>Bewerkingen voor het aanmaken of bijwerken van resources (PUT, PATCH) zijn uitgeschakeld. Bewerkingen die een actie uitvoeren (POST), zijn ook uitgeschakeld. U kunt resources ophalen of verwijderen (GET, DELETE).|
| **Einddatum verstreken** | Uw Azure-abonnement heeft een openstaande factuur. Uw abonnement is nog steeds actief, maar als u de factuur niet voldoet, leidt dit er mogelijk toe dat uw abonnement wordt uitgeschakeld. Raadpleeg voor meer informatie [Verschuldigd bedrag betalen voor uw Azure-abonnement](resolve-past-due-balance.md).<br><br>Alle bewerkingen zijn beschikbaar. |
| **Gewaarschuwd** | Uw Azure-abonnement heeft de status Gewaarschuwd, en hoewel u het gewoon kunt gebruiken, wordt het binnenkort uitgeschakeld als de waarschuwingsreden niet wordt geadresseerd. Een abonnement kan de status Gewaarschuwd hebben als het te laat is betaald, door de gebruiker is geannuleerd, is verlopen, enz.<br><br>Alle bewerkingen zijn beschikbaar. |
