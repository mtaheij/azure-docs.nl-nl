---
title: 'Aangepaste IPsec-beleids regels maken en instellen voor punt-naar-site: Power shell'
titleSuffix: Azure VPN Gateway
description: Dit artikel helpt u bij het maken en instellen van aangepaste IPSec-beleids regels voor VPN Gateway P2S-configuraties
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/09/2020
ms.author: cherylmc
ms.openlocfilehash: 852ff5e6ad847ff33f8e32b4c3d9f8b325b80716
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2020
ms.locfileid: "89664518"
---
# <a name="create-and-set-custom-ipsec-policies-for-point-to-site"></a>Aangepaste IPsec-beleids regels voor punt-naar-site maken en instellen

Als voor uw omgeving een aangepast IPsec-beleid voor versleuteling is vereist, kunt u eenvoudig een beleids object configureren met de vereiste instellingen. Dit artikel helpt u bij het maken van een aangepast beleids object en het vervolgens in te stellen met behulp van Power shell.

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten

Controleer of uw omgeving voldoet aan de volgende vereisten:

* U hebt al een functionerend punt-naar-site-VPN geconfigureerd. Als dat niet het geval is, configureert u een met behulp van de stappen het artikel **een punt-naar-site-VPN maken**  met behulp van [Power shell](vpn-gateway-howto-point-to-site-rm-ps.md)of de [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

### <a name="working-with-azure-powershell"></a>Werken met Azure PowerShell

[!INCLUDE [PowerShell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="1-set-variables"></a><a name="signin"></a>1. variabelen instellen

Declareer de waarden die u wilt gebruiken. Gebruik het volgende voor beeld om de waarden te vervangen wanneer dat nodig is. Als u uw Power shell/Cloud Shell-sessie op een wille keurig moment tijdens de oefening sluit, kopieert en plakt u de waarden opnieuw om de variabelen opnieuw te declareren.

  ```azurepowershell-interactive
  $RG = "TestRG"
  $GWName = "VNet1GW"
  ```

## <a name="2-create-policy-object"></a><a name="create"></a>2. Maak een beleids object

Maak een aangepast IPsec-beleids object. U kunt de waarden aanpassen om te voldoen aan de criteria die u nodig hebt.

```azurepowershell-interactive
$vpnclientipsecpolicy = New-AzVpnClientIpsecPolicy -IpsecEncryption AES256 -IpsecIntegrity SHA256 -SALifeTime 86471 -SADataSize 429496 -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup2 -PfsGroup PFS2
```

## <a name="3-update-gateway-and-set-policy"></a><a name="update"></a>3. gateway bijwerken en beleid instellen

In deze stap werkt u uw bestaande P2S VPN-gateway bij en stelt u het IPsec-beleid in.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -name $GWName
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway -VpnClientIpsecPolicy $vpnclientipsecpolicy
```

## <a name="next-steps"></a>Volgende stappen

Zie [about Point-to-site VPN](point-to-site-about.md)(Engelstalig) voor meer informatie over P2S-configuraties.