---
title: Red Hat Enterprise Linux installatie kopieën die beschikbaar zijn in azure
description: Meer informatie over Red Hat Enterprise Linux installatie kopieën in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/16/2020
ms.author: alsin
ms.openlocfilehash: f4ebf433237193c0db7859b029bb224c72c92eae
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87291017"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Red Hat Enterprise Linux-installatie kopieën (RHEL) die beschikbaar zijn in azure
Azure biedt een aantal RHEL-installatie kopieën voor verschillende use cases.

> [!NOTE]
> Alle RHEL-installatie kopieën zijn beschikbaar in open bare en Azure Government Clouds van Azure. Ze zijn niet beschikbaar in Clouds van Azure China.

## <a name="list-of-rhel-images"></a>Lijst met RHEL-installatie kopieën
Dit is een lijst met RHEL-installatie kopieën die beschikbaar zijn in Azure. Tenzij anders vermeld, worden alle installatie kopieën gepartitioneerd en gekoppeld aan gewone RHEL-opslag plaatsen (niet EUS, niet E4S). De volgende installatie kopieën zijn momenteel beschikbaar voor algemeen gebruik:

> [!NOTE]
> Onbewerkte afbeeldingen worden niet meer geproduceerd in het voor deel van LVM-gepartitioneerde installatie kopieën. LVM biedt verschillende voor delen ten opzichte van het oudere schema voor onbewerkte (niet-LVM), waaronder aanzienlijk meer flexibele opties voor het wijzigen van de partitie.

Aanbieding| SKU | Partitionering | Inrichten | Opmerkingen
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | UITGANG    | Linux-agent |
|             | 6.8      | UITGANG    | Linux-agent |
|             | 6,9      | UITGANG    | Linux-agent |
|             | 6,10     | UITGANG    | Linux-agent |
|             | 7-RAW    | UITGANG    | Linux-agent | RHEL 7. x-familie van installatie kopieën. <br> Standaard gekoppeld aan reguliere opslag plaatsen (niet EUS).
|             | 7-LVM    | LVM    | Linux-agent | RHEL 7. x-familie van installatie kopieën. <br> Standaard gekoppeld aan reguliere opslag plaatsen (niet EUS). Als u op zoek bent naar een standaard RHEL-installatie kopie die u wilt implementeren, gebruikt u deze set installatie kopieën en/of de generatie 2.
|             | 7lvm-Gen2| LVM    | Linux-agent | Generatie 2, RHEL 7. x-familie van installatie kopieën. <br> Standaard gekoppeld aan reguliere opslag plaatsen (niet EUS). Als u op zoek bent naar een standaard RHEL-installatie kopie die u wilt implementeren, gebruikt u deze set installatie kopieën en/of het equivalent van de eerste generatie.
|             | 7-RAW-CI | RAW-CI | cloud-init  | RHEL 7. x-familie van installatie kopieën. <br> Standaard gekoppeld aan reguliere opslag plaatsen (niet EUS).
|             | 7.2      | UITGANG    | Linux-agent |
|             | 7.3      | UITGANG    | Linux-agent |
|             | 7.4      | UITGANG    | Linux-agent | Is vanaf april 2019 standaard gekoppeld aan EUS-opslag plaatsen.
|             | 74-Gen2  | UITGANG    | Linux-agent | Standaard gekoppeld aan EUS-opslag plaatsen.
|             | 7,5      | UITGANG    | Linux-agent | Is vanaf juni 2019 standaard gekoppeld aan EUS-opslag plaatsen.
|             | 75-Gen2  | UITGANG    | Linux-agent | Standaard gekoppeld aan EUS-opslag plaatsen.
|             | 7,6      | UITGANG    | Linux-agent | Gekoppeld aan EUS-opslag plaatsen standaard vanaf mei 2019.
|             | 76-Gen2  | UITGANG    | Linux-agent | Standaard gekoppeld aan EUS-opslag plaatsen.
|             | 7,7      | LVM    | Linux-agent | Standaard gekoppeld aan EUS-opslag plaatsen.
|             | 77-Gen2  | LVM    | Linux-agent | Standaard gekoppeld aan EUS-opslag plaatsen.
|             | 7,8      | LVM    | Linux-agent | Gekoppeld aan reguliere opslag plaatsen (EUS niet beschikbaar voor RHEL 7,8)
|             | 78-Gen2  | LVM    | Linux-agent | Gekoppeld aan reguliere opslag plaatsen (EUS niet beschikbaar voor RHEL 7,8)
|             | 8-LVM    | LVM    | Linux-agent | RHEL 8. x-familie van installatie kopieën. Gekoppeld aan reguliere opslag plaatsen.
|             | 8-LVM-Gen2| LVM    | Linux-agent | Hyper-V-generatie 2-RHEL 8. x-familie van installatie kopieën. Gekoppeld aan reguliere opslag plaatsen.
|             | 8        | LVM    | Linux-agent | RHEL 8,0-installatie kopieën.
|             | 8-Gen2   | LVM    | Linux-agent | Hyper-V-generatie 2-RHEL 8,0 installatie kopieën.
|             | 8.1      | LVM    | Linux-agent | RHEL 8,2-installatie kopieën. Momenteel gekoppeld aan reguliere opslag plaatsen.
|             | 81gen2   | LVM    | Linux-agent | Hyper-V-generatie 2-RHEL 8,1 installatie kopieën. Momenteel gekoppeld aan reguliere opslag plaatsen.
|             | 8,1-CI   | LVM    | Linux-agent | RHEL 8,1-installatie kopieën die gebruikmaken van Cloud-init als inrichtings agent. Momenteel gekoppeld aan reguliere opslag plaatsen.
|             | 81-CI-Gen2| LVM    | Linux-agent | Hyper-V-generatie 2-RHEL 8,1-installatie kopieën met Cloud-init als inrichtings agent. Momenteel gekoppeld aan reguliere opslag plaatsen.
|             | 8,2      | LVM    | Linux-agent | RHEL 8,2-installatie kopieën. Momenteel gekoppeld aan reguliere opslag plaatsen.
|             | 82gen2   | LVM    | Linux-agent | Hyper-V-generatie 2-RHEL 8,1 installatie kopieën. Momenteel gekoppeld aan reguliere opslag plaatsen.
RHEL-SAP      | 7.4      | LVM    | Linux-agent | RHEL 7,4 voor SAP HANA en zakelijke apps. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 74sap-Gen2| LVM    | Linux-agent | RHEL 7,4 voor SAP HANA en zakelijke apps. Installatie kopie van de 2e generatie. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 7,5       | LVM    | Linux-agent | RHEL 7,5 voor SAP HANA en zakelijke apps. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 75sap-Gen2| LVM    | Linux-agent | RHEL 7,5 voor SAP HANA en zakelijke apps. Installatie kopie van de 2e generatie. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 7,6       | LVM    | Linux-agent | RHEL 7,6 voor SAP HANA en zakelijke apps. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 76sap-Gen2| LVM    | Linux-agent | RHEL 7,6 voor SAP HANA en zakelijke apps. Installatie kopie van de 2e generatie. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
|             | 7,7       | LVM    | Linux-agent | RHEL 7,7 voor SAP HANA en zakelijke apps. Aan de E4S-opslag plaatsen wordt een Premium voor SAP en RHEL, en de basis reken kosten in rekening gebracht.
RHEL-SAP-HANA | 6.7       | UITGANG    | Linux-agent | RHEL 6,7 voor SAP HANA. Verouderd in het voor deel van de RHEL-SAP-installatie kopieën.
|             | 7.2       | LVM    | Linux-agent | RHEL 7,2 voor SAP HANA. Verouderd in het voor deel van de RHEL-SAP-installatie kopieën.
|             | 7.3       | LVM    | Linux-agent | RHEL 7,3 voor SAP HANA. Verouderd in het voor deel van de RHEL-SAP-installatie kopieën.
RHEL-SAP-APPS | 6.8       | UITGANG    | Linux-agent | RHEL 6,8 voor SAP Business Applications. Verouderd in het voor deel van de RHEL-SAP-installatie kopieën.
|             | 7.3       | LVM    | Linux-agent | RHEL 7,3 voor SAP Business Applications. Verouderd in het voor deel van de RHEL-SAP-installatie kopieën.
RHEL-HA       | 7.4       | LVM    | Linux-agent | RHEL 7,4 met HA-invoeg toepassing. In wordt een Premium voor HA en RHEL boven op de basis reken kosten in rekening gebracht.
|             | 7,5       | LVM    | Linux-agent | RHEL 7,5 met HA-invoeg toepassing. In wordt een Premium voor HA en RHEL boven op de basis reken kosten in rekening gebracht.
|             | 7,6       | LVM    | Linux-agent | RHEL 7,6 met HA-invoeg toepassing. In wordt een Premium voor HA en RHEL boven op de basis reken kosten in rekening gebracht.
RHEL-SAP-HA   | 7.4          | LVM    | Linux-agent | RHEL 7,4 voor SAP met HA en Update Services. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 74sapha-Gen2 | LVM    | Linux-agent | RHEL 7,4 voor SAP met HA en Update Services. Installatie kopie van de 2e generatie. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 7,5          | LVM    | Linux-agent | RHEL 7,5 voor SAP met HA en Update Services. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 7,6          | LVM    | Linux-agent | RHEL 7,6 voor SAP met HA en Update Services. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 76sapha-Gen2 | LVM    | Linux-agent | RHEL 7,6 voor SAP met HA en Update Services. Installatie kopie van de 2e generatie. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 7,7          | LVM    | Linux-agent | RHEL 7,7 voor SAP met HA en Update Services. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
|             | 77sapha-Gen2 | LVM    | Linux-agent | RHEL 7,7 voor SAP met HA en Update Services. Installatie kopie van de 2e generatie. Gekoppeld aan E4S-opslag plaatsen. Berekent een Premium voor SAP-en HA-opslag plaatsen, evenals RHEL, bovenop de kosten voor basis berekeningen.
RHEL-BYOS     |RHEL-lvm74| LVM    | Linux-agent | RHEL 7,4 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm75| LVM    | Linux-agent | RHEL 7,5 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm76| LVM    | Linux-agent | RHEL 7,6 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm76-Gen2| LVM    | Linux-agent | RHEL 7,6 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm77| LVM    | Linux-agent | RHEL 7,7 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm77-Gen2| LVM    | Linux-agent | RHEL 7,7 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm78| LVM    | Linux-agent | RHEL 7,8 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm78-Gen2| LVM    | Linux-agent | RHEL 7,8 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm8 | LVM    | Linux-agent | RHEL 8,0 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm8-Gen2 | LVM    | Linux-agent | RHEL 8,0 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm81 | LVM    | Linux-agent | RHEL 8,1 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm81-Gen2 | LVM    | Linux-agent | RHEL 8,1 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm82 | LVM    | Linux-agent | RHEL 8,2 BYOS-installatie kopieën die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.
|             |RHEL-lvm82-Gen2 | LVM    | Linux-agent | RHEL 8,2 Generation 2 BYOS installatie kopieën, die niet zijn gekoppeld aan een bron van updates, brengen geen kosten in rekening voor een RHEL Premium.

> [!NOTE]
> De product aanbieding van RHEL-SAP-HANA wordt als einde van de levens duur beschouwd door Red Hat. Bestaande implementaties blijven normaal werken, maar Red Hat raadt aan dat klanten migreren van de RHEL-SAP-HANA-afbeeldingen naar de RHEL-SAP-HA-installatie kopieën die de SAP HANA-opslag plaatsen en de HA-invoeg toepassing bevatten. Meer informatie over de SAP-Cloud aanbiedingen van Red Hat vindt u [hier](https://access.redhat.com/articles/3751271).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [Red Hat-afbeeldingen in azure](./redhat-images.md).
* Meer informatie over de [infra structuur voor Red Hat-updates](./redhat-rhui.md).
* Meer informatie over de [RHEL BYOS-aanbieding](./byos.md).
* Informatie over Red Hat-ondersteunings beleid voor alle versies van RHEL vindt u op de pagina [levens cyclus van Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata) .
