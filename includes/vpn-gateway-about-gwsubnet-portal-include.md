---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/24/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 92a6fef3305891b47c4dc2e0f0432e1079df5a69
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "71266510"
---
De virtuele netwerkgateway maakt gebruik van een specifiek subnet: het gatewaysubnet. Het gatewaysubnet maakt deel uit van het IP-adresbereik van het virtuele netwerk dat u opgeeft bij het configureren ervan. Het bevat de IP-adressen waarvan de resources en services van de virtuele netwerkgateway gebruikmaken. 


Wanneer u het gatewaysubnet maakt, geeft u op hoeveel IP-adressen het subnet bevat. Hoeveel IP-adressen er nodig zijn, is afhankelijk van de configuratie van de VPN-gateway die u wilt maken. Sommige configuraties vereisen meer IP-adressen dan andere. We raden u aan om een gatewaysubnet te maken die gebruikmaakt van een /27 of /28.


Als er een fout wordt weergegeven waarin staat dat de adresruimte met een subnet overlapt of dat het subnet niet is opgenomen in de adresruimte voor het virtuele netwerk, controleert u uw VNet-adresbereik. U hebt mogelijk onvoldoende beschikbare IP-adressen in het adresbereik dat u voor het virtuele netwerk hebt gemaakt. Als uw standaardsubnet bijvoorbeeld het gehele adresbereik omvat, zijn er geen IP-adressen over om extra subnetten te maken. U kunt uw subnetten in de bestaande adresruimte aanpassen om IP-adressen vrij te maken of een aanvullend adresbereik opgeven en daar het gatewaysubnet maken.