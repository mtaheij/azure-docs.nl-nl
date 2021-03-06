---
title: Status van Azure NetApp Files replicatie relatie weer geven | Microsoft Docs
description: Hierin wordt beschreven hoe u de replicatie status op het bron volume of het doel volume van Azure NetApp Files weergeeft.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: 95c1202fb56e882554d40926e9d5ecec7be49086
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708657"
---
# <a name="display-health-status-of-replication-relationship"></a>Status van replicatie relatie weer geven 

U kunt de replicatie status weer geven op het bron volume of het doel volume.  

## <a name="display-replication-status"></a>Replicatie status weer geven

1. Klik vanuit het bron volume of het doel volume op **replicatie** onder Storage-service voor elk volume.

    De volgende status-en status gegevens voor replicatie worden weer gegeven:  
    * **Type eind punt** : geeft aan of het volume de bron of het doel van de replicatie is.
    * **Status** : hier wordt de status van de replicatie relatie weer gegeven.
    * **Spiegel status** : toont een van de volgende waarden:
        * Niet *geïnitialiseerd*:  
            Dit is de initiële en standaard status wanneer een peering-relatie wordt gemaakt. De status blijft niet-geïnitialiseerd totdat de initialisatie is voltooid.
        * *Gespiegeld*:   
            Het doel volume is geïnitialiseerd en is klaar voor het ontvangen van mirroring-updates.
        * *Verbroken*:   
            Dit is de status nadat u de peering-relatie hebt verbroken. Het doel volume is `‘RW’` en moment opnamen zijn aanwezig.
    * **Relatie status** : toont een van de volgende waarden: 
        * *Niet-actief*:  
            Er wordt geen overdrachts bewerking uitgevoerd en toekomstige overdrachten zijn niet uitgeschakeld.
        * *Overdragen*:  
            Er wordt een overdrachts bewerking uitgevoerd en toekomstige overdrachten worden niet uitgeschakeld.
    * **Replicatie schema** : geeft weer hoe vaak incrementele mirroring updates worden uitgevoerd wanneer de initialisatie (basislijn kopie) is voltooid.

    * **Totale voortgang** : toont de totale hoeveelheid gegevens in bytes die zijn overgedragen voor de huidige overdrachts bewerking. Dit is de werkelijke bits die worden overgedragen, en kan verschillen van de logische ruimte die het rapport van de bron-en doel volumes is.  

    ![Replicatie status](../media/azure-netapp-files/cross-region-replication-health-status.png)

> [!NOTE] 
> De replicatie relatie toont *de status beschadigd als eerdere* replicatie taken niet zijn voltooid. Deze status is het resultaat van grote volumes die worden overgebracht met een lager overdrachts venster (bijvoorbeeld een overdrachts tijd van tien minuten voor een groot volume). In dit *geval wordt de* status van de relatie weer gegeven en wordt de *status beschadigd weer gegeven.*

## <a name="next-steps"></a>Volgende stappen  

* [Replicatie tussen regio's](cross-region-replication-introduction.md)
* [Herstel na noodgevallen beheren](cross-region-replication-manage-disaster-recovery.md)
* [Metrische gegevens van de volume replicatie](azure-netapp-files-metrics.md#replication)
* [Problemen met replicatie tussen regio's oplossen](troubleshoot-cross-region-replication.md)

