---
title: MFA inschakelen voor VPN-gebruikers met behulp van Azure AD-verificatie
description: Meer informatie over het inschakelen van Azure Multi-Factor Authentication (MFA) voor VPN-gebruikers met behulp van Azure AD-verificatie.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: alzam
ms.openlocfilehash: efe01c9e0907fef4d33d2a70b3e479b30c471a7c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91267887"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users-by-using-azure-ad-authentication"></a>Azure Multi-Factor Authentication (MFA) inschakelen voor VPN-gebruikers met behulp van Azure AD-verificatie

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

## <a name="enable-authentication"></a><a name="enableauth"></a>Verificatie inschakelen

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

## <a name="configure-sign-in-settings"></a><a name="enablesign"></a>Aanmeldings instellingen configureren

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

## <a name="option-1---per-user-access"></a><a name="peruser"></a>Optie 1: toegang per gebruiker

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

## <a name="option-2---conditional-access"></a><a name="conditional"></a>Optie 2: voorwaardelijke toegang

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>Volgende stappen

Als u verbinding wilt maken met uw virtuele netwerk, moet u een VPN-client profiel maken en configureren. Zie [Azure AD-verificatie configureren voor punt-naar-site-verbinding met Azure](virtual-wan-point-to-site-azure-ad.md).
