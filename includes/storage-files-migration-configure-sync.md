---
title: Azure File Sync configureren
description: Azure File Sync configureren. Een gemeen schappelijk tekst blok, gedeeld via migratie documenten.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 562099c65273cad50596268b89055ec78d92d18a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82143553"
---
Deze stap is gekoppeld aan alle resources en mappen die u tijdens de vorige stappen hebt ingesteld voor uw Windows Server-exemplaar.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoek de resource van de opslag synchronisatie service.
1. Maak een nieuwe *synchronisatie groep* in de resource voor de opslag synchronisatie service voor elke Azure-bestands share. In Azure File Sync terminologie wordt de Azure-bestands share een Cloud- *eind punt* in de synchronisatie topologie die u beschrijft bij het maken van een synchronisatie groep. Wanneer u de synchronisatie groep maakt, geeft u deze een vertrouwde naam zodat u kunt herkennen welke set bestanden hier wordt gesynchroniseerd. Zorg ervoor dat u verwijst naar de Azure-bestands share met een overeenkomende naam.
1. Nadat de synchronisatie groep is gemaakt, wordt er een rij voor weer gegeven in de lijst met synchronisatie groepen. Selecteer de naam (een koppeling) om de inhoud van de synchronisatie groep weer te geven. U ziet uw Azure-bestands share onder **Cloud eindpunten**.
1. Zoek de knop **+ Server-eind punt toevoegen** . De map op de lokale server die u hebt ingericht, wordt het pad voor dit *Server eindpunt*.
