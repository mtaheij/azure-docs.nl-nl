---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7b41105bafedb8eeaffe5f266f5dd824957c57e0
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/29/2020
ms.locfileid: "91441184"
---
Voor nu hebben Ultra disks de volgende beperkingen:

De enige opties voor de infra structuur-redundantie die momenteel beschikbaar zijn voor Ultra disks zijn beschikbaarheids zones. Vm's met andere redundantie opties kunnen geen Ultra schijf koppelen.

De volgende tabel geeft een overzicht van de regio's Ultra disks zijn beschikbaar in, evenals de bijbehorende beschikbaarheids opties:

> [!NOTE]
> Als een regio in de volgende lijst geen beschik bare beschikbaarheids zones met hoge schijven heeft, moeten de Vm's in die regio worden geïmplementeerd zonder enige infrastructuur redundantie opties om een ultra schijf te kunnen koppelen.

|Regio's  |Redundantie opties  |
|---------|---------|
|Brazil South     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|Central India     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|Azië - oost     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|Duitsland - west-centraal     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|Korea - centraal     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|South Central US    |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|VS (overheid) - Arizona     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|VS (overheid) - Virginia     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|VS - west     |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)        |
|Australië - centraal    |Alleen enkele Vm's (beschikbaarheids sets en virtuele-machine schaal sets worden niet ondersteund)|
|Australië - oost     |Drie beschikbaarheids zones         |
|Azië - zuidoost    |Drie beschikbaarheids zones        |
|Canada-centraal *     |Drie beschikbaarheids zones          |
|Central US     |Drie beschikbaarheids zones          |
|VS - oost     |Drie beschikbaarheids zones          |
|VS - oost 2     |Drie beschikbaarheids zones         |
|Frankrijk - centraal    |Twee beschikbaarheids zones        |
|Japan East    |Drie beschikbaarheids zones        |
|Europa - noord    |Drie beschikbaarheids zones        |
|Verenigd Koninkrijk Zuid    |Drie beschikbaarheids zones        |
|Europa -west    | Drie beschikbaarheids zones|
|West US 2    |Drie beschikbaarheids zones|

\* Neem contact op met de ondersteuning van Azure om toegang te krijgen tot Beschikbaarheidszones voor deze regio.

- Worden alleen ondersteund in de volgende VM-reeks:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
    - [Easv4](../articles/virtual-machines/eav4-easv4-series.md#easv4-series)
    - [Edsv4](../articles/virtual-machines/edv4-edsv4-series.md#edsv4-series)
    - [Esv4](../articles/virtual-machines/ev4-esv4-series.md#esv4-series)
    - [DSv3](../articles/virtual-machines/dv3-dsv3-series.md#dsv3-series)
    - [Dasv4](../articles/virtual-machines/dav4-dasv4-series.md#dasv4-series)
    - [Ddsv4](../articles/virtual-machines/ddv4-ddsv4-series.md#ddsv4-series)
    - [Dsv4](../articles/virtual-machines/dv4-dsv4-series.md#dsv4-series)
    - [FSv2](../articles/virtual-machines/fsv2-series.md)
    - [LSv2](../articles/virtual-machines/lsv2-series.md)
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Niet elke VM-grootte is beschikbaar in elke ondersteunde regio met ultra disks.
- Zijn alleen beschikbaar als gegevens schijven. 
- Standaard de fysieke sector grootte van 4.000 ondersteunen. de 512E-sector grootte is beschikbaar als een algemeen beschik bare aanbieding, maar u moet [zich wel registreren](https://aka.ms/ultradisk512e). De meeste toepassingen zijn compatibel met formaten van 4.000 sectoren, maar sommige sectoren vereisen een grootte van 512 bytes. Een voor beeld hiervan is Oracle Database, waarvoor release 12,2 of hoger is vereist om de onbestelbare schijven van 4.000 te ondersteunen. Voor oudere versies van Oracle DB is de sector grootte van 512 bytes vereist.
- Kan alleen worden gemaakt als lege schijven.
- Biedt momenteel geen ondersteuning voor schijf momentopnamen, VM-installatie kopieën, beschikbaarheids sets, voor Azure toegewezen hosts of Azure Disk Encryption.
- Biedt momenteel geen ondersteuning voor integratie met Azure Backup of Azure Site Recovery.
- Ondersteunt alleen niet-opgeslagen Lees bewerkingen en schrijf bewerkingen in de cache.
- De huidige maximum limiet voor IOPS op GA Vm's is 80.000.

Azure Ultra disks biedt standaard Maxi maal 16 TiB per regio per abonnement, maar Ultra schijven ondersteunen hogere capaciteit op aanvraag. Neem contact op met de ondersteuning van Azure om een verhoging van de capaciteit aan te vragen.
