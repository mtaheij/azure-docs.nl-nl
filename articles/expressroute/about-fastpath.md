---
title: Over Azure ExpressRoute FastPath
description: Meer informatie over Azure ExpressRoute FastPath voor het verzenden van netwerk verkeer door de gateway over te slaan
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: duau
ms.openlocfilehash: bf600d835e177ce51870ed2a8894adcd667e3901
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89398049"
---
# <a name="about-expressroute-fastpath"></a>Over ExpressRoute FastPath

ExpressRoute virtuele netwerk gateway is ontworpen voor het uitwisselen van netwerk routes en het routeren van netwerk verkeer. FastPath is ontworpen om de prestaties van het gegevenspad tussen uw on-premises netwerk en het virtuele netwerk te verbeteren. Als deze functie is ingeschakeld, verzendt FastPath netwerk verkeer rechtstreeks naar virtuele machines in het virtuele netwerk, waarbij de gateway wordt omzeild.

## <a name="requirements"></a>Vereisten

### <a name="circuits"></a>Circuits

FastPath is beschikbaar op alle ExpressRoute-circuits.

### <a name="gateways"></a>Gateways

FastPath moet nog een virtuele netwerk gateway maken voor het uitwisselen van routes tussen het virtuele netwerk en het on-premises netwerk. Zie [virtuele netwerk gateways van ExpressRoute](expressroute-about-virtual-network-gateways.md)voor meer informatie over virtuele netwerk gateways en ExpressRoute, met inbegrip van prestatie gegevens en gateway-sku's.

Voor het configureren van FastPath moet de gateway van het virtuele netwerk een van de volgende zijn:

* Ultra prestaties
* ErGw3AZ

## <a name="supported-features"></a>Ondersteunde functies

Hoewel FastPath de meeste configuraties ondersteunt, biedt het geen ondersteuning voor de volgende functies:

* UDR op het gateway-subnet: als u een UDR toepast op het gateway-subnet van het virtuele netwerk, wordt het netwerk verkeer van uw on-premises netwerk naar de gateway van het virtuele netwerk verzonden.

* VNet-peering: als u andere virtuele netwerken hebt die zijn gekoppeld aan ExpressRoute, wordt het netwerk verkeer van uw on-premises netwerk naar de andere virtuele netwerken (d.w.z. de zogenaamde "spoke" VNets) naar de virtuele netwerk gateway verzonden. De tijdelijke oplossing is om alle virtuele netwerken rechtstreeks aan het ExpressRoute-circuit te koppelen.

* Basis Load Balancer: als u een intern interne load balancer in uw virtuele netwerk implementeert of als de Azure PaaS-service die u in uw virtuele netwerk implementeert, gebruikmaakt van een Basic interne load balancer, wordt het netwerk verkeer van uw on-premises netwerk naar de virtuele IP-adressen die worden gehost op de basis load balancer, verzonden naar de gateway van het virtuele netwerk. De oplossing is het bijwerken van de basis load balancer naar een [standaard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview).

* Privé-koppeling: als u vanuit uw on-premises netwerk verbinding maakt met een [persoonlijk eind punt](../private-link/private-link-overview.md) in uw virtuele netwerk, wordt de verbinding via de gateway van het virtuele netwerk door lopen.
 
## <a name="next-steps"></a>Volgende stappen

Zie [een virtueel netwerk koppelen aan ExpressRoute](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath)om FastPath in te scha kelen.
