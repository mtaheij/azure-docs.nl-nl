---
title: Av2-serie
description: Specificaties voor de virtuele machines uit de Av2-serie.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 33d481d546d94bb1279cafbd85ae23c8ac4d00b8
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/03/2020
ms.locfileid: "89437264"
---
# <a name="av2-series"></a>Av2-serie

De virtuele machines uit de Av2-serie kunnen worden geïmplementeerd op diverse typen hardware en processors. Vm's uit de Av2-serie hebben CPU-prestaties en geheugen configuraties die het meest geschikt zijn voor workloads op instap niveau, zoals ontwikkelen en testen. De grootte wordt beperkt om consistente processor prestaties te bieden voor het actieve exemplaar, ongeacht de hardware waarop het wordt geïmplementeerd. Om de fysieke hardware te bepalen waarop deze grootte is geïmplementeerd, vraagt u vanuit de virtuele machine gegevens over de virtuele hardware op. Enkele voor beelden van use-cases zijn: ontwikkelings-en test servers, webservers met weinig verkeer, kleine tot middel grote data bases, controle van concepten en code opslagplaatsen.

ACU: 100

Premium Storage: niet ondersteund

Premium Storage caching: niet ondersteund

Livemigratie: ondersteund

Updates voor geheugen behoud: ondersteund


| Grootte | vCore | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslag doorvoer: IOPS/MBps lezen/MBps schrijven | Maximum aantal gegevens schijven/door Voer: IOPS | Max. aantal NIC's | Verwachte netwerk bandbreedte (Mbps)
|---|---|---|---|---|---|---|---|
| Standard_A1_v2  | 1 | 2  | 10 | 1000/20/10  | 2/2x500   | 2 | 250  |
| Standard_A2_v2  | 2 | 4  | 20 | 2000/40/20  | 4-4x500   | 2 | 500  |
| Standard_A4_v2  | 4 | 8  | 40 | 4000/80/40  | 8-8x500   | 4 | 1000 |
| Standard_A8_v2  | 8 | 16 | 80 | 8000/160/80 | 16-16x500 | 8 | 2000 |
| Standard_A2m_v2 | 2 | 16 | 20 | 2000/40/20  | 4-4x500   | 2 | 500  |
| Standard_A4m_v2 | 4 | 32 | 40 | 4000/80/40  | 8-8x500   | 4 | 1000 |
| Standard_A8m_v2 | 8 | 64 | 80 | 8000/160/80 | 16-16x500 | 8 | 2000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Andere grootten en informatie

- [Algemeen gebruik](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

Prijs calculator: [prijs calculator](https://azure.microsoft.com/pricing/calculator/)

Meer informatie over schijven typen: [schijf typen](./disks-types.md#ultra-disk)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
