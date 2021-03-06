---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8087025810214f3edbb74e628698eb69558f3500
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "74085232"
---
Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU die aan uw vereisten voldoet op basis van de typen werkbelasting, doorvoer, functies en SLA's. Zie [Azure-beschikbaarheidszones gateway-sku's](../articles/vpn-gateway/about-zone-redundant-vnet-gateways.md)voor virtuele netwerk gateway-sku's in azure-beschikbaarheidszones.

###  <a name="gateway-skus-by-tunnel-connection-and-throughput"></a><a name="benchmark"></a>Gateway-SKU's per tunnel, verbinding en doorvoer

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

> [!NOTE]
> VpnGw Sku's (VpnGw1, VpnGw1AZ, VpnGw2, VpnGw2AZ, VpnGw3, VpnGw3AZ, VpnGw4, VpnGw4AZ, VpnGw5 en VpnGw5AZ) worden alleen ondersteund voor het Resource Manager-implementatie model. Klassieke virtuele netwerken moeten de oude (verouderde) Sku's blijven gebruiken.
>  * Zie [werken met VPN gateway-sku's (verouderde sku's)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md)voor meer informatie over het werken met de verouderde gateway-Sku's (Basic, Standard en high performance).
>  * Zie [Virtual Network gateways voor ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md)voor ExpressRoute gateway-sku's.
>

###  <a name="gateway-skus-by-feature-set"></a><a name="feature"></a>Gateway-Sku's per functieset

De nieuwe VPN-gateway-Sku's stroom lijnen de functie sets die worden aangeboden op de gateways:

| **SKU**| **Functies**|
| ---    | ---         |
|**Basic** (* *)   | **Op route gebaseerd VPN**: 10 tunnels voor S2S/Connection; geen RADIUS-authenticatie voor P2S; geen IKEv2 voor P2S<br>**Op beleid gebaseerde VPN**: (IKEv1): 1 S2S/Connection tunnel; geen P2S|
| **Alle Generation1-en Generation2-Sku's behalve basis** | **Op route gebaseerd VPN**: Maxi maal 30 tunnels (*), P2S, BGP, actief-actief, aangepast IPSec/IKE-beleid, ExpressRoute/VPN-samen werking |
|        |             |

(*) U kunt "PolicyBasedTrafficSelectors" configureren om een op route gebaseerde VPN-gateway te verbinden met meerdere on-premises op beleid gebaseerde firewall apparaten. Raadpleeg [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (VPN-gateways verbinden met meerdere on-premises,op beleid gebaseerde VPN-apparaten met behulp van PowerShell) voor meer informatie.

( \* \* ) De basis-SKU wordt beschouwd als een verouderde SKU. Deze SKU kent bepaalde functiebeperkingen. Zo kunt u een gateway die gebruikmaakt van een Basic-SKU, niet overzetten naar een van de nieuwe gateway-SKU's. In plaats daarvan moet u een nieuwe SKU gebruiken, wat betekent dat u uw VPN-gateway eerst moet verwijderen en vervolgens opnieuw moet maken.

###  <a name="gateway-skus---production-vs-dev-test-workloads"></a><a name="workloads"></a>Gateway-Sku's-productie versus dev-test-workloads

Vanwege de verschillen in SLA's en functiesets, raden we de volgende SKU's aan voor productie vs. ontwikkelen en testen:

| **Workload**                       | **SKU's**               |
| ---                                | ---                    |
| **Productie, kritieke werkbelastingen** | Alle Generation1-en Generation2-Sku's behalve basis |
| **Ontwikkelen en testen of conceptontwerpen**   | Basic (* *)                 |
|                                    |                        |

( \* \* ) De basis-SKU wordt beschouwd als een verouderde SKU en heeft functie beperkingen. Controleer of de functie die u nodig hebt, wordt ondersteund voordat u kiest voor de Basic-SKU.

Als u de oude Sku's (verouderd) gebruikt, zijn de aanbevelingen voor productie-SKU'S standaard en high performance. Zie [Gateway-sku's (verouderd)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md)voor informatie over en instructies voor oude sku's.
