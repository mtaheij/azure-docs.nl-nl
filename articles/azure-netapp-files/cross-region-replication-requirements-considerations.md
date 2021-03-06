---
title: Vereisten en overwegingen voor het gebruik van Azure NetApp Files volume replicatie tussen regio's | Microsoft Docs
description: Hierin worden de vereisten en overwegingen beschreven voor het gebruik van de replicatie functionaliteit voor meerdere regio's van Azure NetApp Files.
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
ms.topic: conceptual
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: f2a50872fdb71419a0c3f068712ec67523a098e0
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708646"
---
# <a name="requirements-and-considerations-for-using-cross-region-replication"></a>Vereisten en overwegingen voor het gebruik van replicatie tussen regio's 

Houd rekening met de volgende vereisten en overwegingen voor [het gebruik van de functionaliteit voor replicatie van meerdere regio's](cross-region-replication-create-peering.md) van Azure NetApp files:  

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen 

* De functie voor replicatie van meerdere regio's is momenteel beschikbaar als open bare preview. U moet een Waitlist-aanvraag indienen om toegang te krijgen tot de functie via de [Waitlist-verzend pagina van Azure NetApp files cross-Region replicatie](https://aka.ms/anfcrrpreviewsignup). Wacht op een officiële bevestigings-e-mail van het Azure NetApp Files team voordat u de functie voor replicatie tussen regio's gebruikt.
* Azure NetApp Files replicatie is alleen beschikbaar in bepaalde paren met vaste regio's. Zie [ondersteunde regio paren](cross-region-replication-introduction.md#supported-region-pairs). 
* SMB-volumes worden samen met NFS-volumes ondersteund. Voor de replicatie van SMB-volumes is een Active Directory verbinding vereist in de bron-en doel-NetApp-accounts. De doel-AD-verbinding moet toegang hebben tot de DNS-servers of domein controllers toevoegen die bereikbaar zijn vanuit het overgedragen subnet in de doel regio. Zie [vereisten voor Active Directory verbindingen](azure-netapp-files-create-volumes-smb.md#requirements-for-active-directory-connections)voor meer informatie. 
* Het doel account moet zich in een andere regio bevinden dan de regio van het bron volume. U kunt ook een bestaand NetApp-account in een andere regio selecteren.  
* Azure NetApp Files replicatie biedt momenteel geen ondersteuning voor meerdere abonnementen. alle replicaties moeten worden uitgevoerd onder één abonnement.
* U kunt Maxi maal vijf volumes voor replicatie instellen binnen één abonnement per regio. U kunt een ondersteunings ticket openen om een verhoging van het standaard quotum van vijf replicatie doel volumes (per abonnement in een regio) aan te vragen. 
* Er kan een vertraging van Maxi maal vijf minuten zijn voor de interface om een nieuwe toegevoegde moment opname op het bron volume weer te geven.  
* Trapsgewijze topologieën en ventilatoren worden niet ondersteund.
* Het configureren van de volume replicatie voor bron volumes die zijn gemaakt op basis van een moment opname, wordt op dit moment niet ondersteund.
* Nadat u de replicatie van meerdere regio's hebt ingesteld, maakt het replicatie proces *snapmirror-moment opnamen* om verwijzingen tussen het bron volume en het doel volume op te geven. Snapmirror-moment opnamen worden automatisch gerecycled wanneer er een nieuwe wordt gemaakt voor elke incrementele overdracht. U kunt snapmirror-moment opnamen pas verwijderen als de replicatie relatie en het volume zijn verwijderd. 
* U kunt hand matige moment opnamen verwijderen van het bron volume van een replicatie relatie wanneer de replicatie relatie actief of verbroken is, en ook nadat de replicatie relatie is verwijderd. U kunt hand matige moment opnamen voor het doel volume pas verwijderen als de replicatie relatie is verbroken.

## <a name="next-steps"></a>Volgende stappen
* [Replicatie-peering maken](cross-region-replication-create-peering.md)
* [Status van replicatie relatie weer geven](cross-region-replication-display-health-status.md)
* [Herstel na noodgevallen beheren](cross-region-replication-manage-disaster-recovery.md)
* [Metrische gegevens van de volume replicatie](azure-netapp-files-metrics.md#replication)
* [Problemen met replicatie tussen regio's oplossen](troubleshoot-cross-region-replication.md)


