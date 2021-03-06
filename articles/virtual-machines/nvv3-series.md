---
title: NVv3-serie-Azure Virtual Machines
description: Specificaties voor de virtuele machines uit de NVv3-serie.
services: virtual-machines
ms.subservice: sizes
author: vikancha-MSFT
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 7fcc8d89df6ac346a44e0356a4416e4168933fd8
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543908"
---
# <a name="nvv3-series"></a>NVv3-serie

De virtuele machines uit de NVv3-serie worden aangedreven door [Nvidia Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) gpu's en de NVIDIA-raster technologie met Intel E5-2690 v4 (Broadwell) Cpu's en Intel Hyper-Threading-technologie. Deze virtuele machines zijn gericht op GPU-versnelde grafische toepassingen en virtuele Bureau bladen waar klanten hun gegevens willen visualiseren, de resultaten kunnen simuleren om te bekijken, te werken met CAD of inhoud te genereren en te streamen. Daarnaast kunnen deze virtuele machines single Precision-werk belastingen uitvoeren, zoals code ring en rendering. Virtuele machines van NVv3 ondersteunen Premium Storage en komen twee maal zoveel systeem geheugen (RAM) te staan in vergelijking met de bijbehorende voorafgaande NV-serie.  

Elke GPU in NVv3-exemplaren wordt geleverd met een GRID-licentie. Deze licentie geeft u de flexibiliteit om een NV-exemplaar te gebruiken als een virtueel werk station voor één gebruiker, of 25 gelijktijdige gebruikers kunnen verbinding maken met de virtuele machine voor een virtuele-toepassings scenario.

Premium Storage caching: ondersteund

Livemigratie: niet ondersteund

Updates voor het behouden van geheugen: niet ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | GPU | GPU-geheugen: GiB | Max. aantal gegevensschijven | Maxi maal aantal niet-opgeslagen schijf doorvoer: IOPS/MBps | Maximum aantal Nic's/verwachte netwerk bandbreedte (Mbps) | Virtuele werk stations | Virtuele toepassingen |
|---|---|---|---|---|---|---|---|---|---|---|
| Standard_NV12s_v3 |12 | 112 | 320  | 1 | 8  | 12 | 20000/200 | 4 / 6000 | 1 | 25  |
| Standard_NV24s_v3 |24 | 224 | 640  | 2 | 16 | 24 | 40000/400 | 8 / 12000 | 2 | 50  |
| Standard_NV48s_v3 |48 | 448 | 1280 | 4 | 32 | 32 | 80000/800 | 8 / 24000 | 4 | 100 |

<sup>1</sup> 1 GPU = One-halve M60-kaart.

<sup>2</sup> op dit moment wordt versneld netwerken niet ondersteund in deze VM-familie, waardoor de netwerk prestaties van de werkelijke wereld mogelijk lager zijn dan de maximale theoretische getallen die hier worden vastgelegd.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

Om te profiteren van de GPU-mogelijkheden van Vm's uit de Azure N-serie, moeten de NVIDIA GPU-Stuur Programma's zijn geïnstalleerd.

Met de [uitbrei ding NVIDIA GPU-stuur programma](./extensions/hpccompute-gpu-windows.md) worden de juiste NVIDIA-CUDA of raster Stuur Programma's geïnstalleerd op een virtuele machine uit de N-serie. De uitbrei ding installeren of beheren met de Azure Portal of hulpprogram ma's, zoals Azure PowerShell of Azure Resource Manager sjablonen. Zie de [documentatie over NVIDIA GPU-Stuur Programma's](./extensions/hpccompute-gpu-windows.md) voor ondersteunde besturings systemen en implementaties tappen. Zie [extensies en functies van virtuele Azure-machines](./extensions/overview.md)voor algemene informatie over VM-extensies.

Als u ervoor kiest om de NVIDIA GPU-Stuur Programma's hand matig te installeren, raadpleegt u [het stuur programma voor](./windows/n-series-driver-setup.md) de installatie van de Windows-of [n-Series GPU-](./linux/n-series-driver-setup.md) stuur programma voor Linux voor ondersteunde besturings systemen, stuur Programma's, installatie en verificaties tappen.

## <a name="other-sizes"></a>Andere grootten

- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.
