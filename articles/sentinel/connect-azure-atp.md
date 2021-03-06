---
title: Verbinding maken tussen micro soft Defender voor identiteits gegevens (voorheen Azure ATP) aan Azure Sentinel | Microsoft Docs
description: Meer informatie over het streamen van logboeken van micro soft Defender voor identiteit (voorheen Azure Advanced Threat Protection) (ATP) in azure Sentinel met één klik.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 64c1a7155d0cc4e80f97db138a0626d6e9fdc9e9
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657806"
---
# <a name="connect-data-from-microsoft-defender-for-identity-formerly-azure-advanced-threat-protection"></a>Gegevens van micro soft Defender verbinden voor identiteit (voorheen Azure Advanced Threat Protection)

> [!IMPORTANT]
> De micro soft Defender for Identity Data Connector in azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt Logboeken van [micro soft Defender streamen voor identiteiten](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) in azure Sentinel met één klik.

## <a name="prerequisites"></a>Vereisten

- Gebruiker met de machtigingen globale beheerder of beveiligings beheerder
- U moet een preview-klant zijn van micro soft Defender voor identiteit en de integratie tussen micro soft Defender voor identiteit en Microsoft Cloud App Security inschakelen. Zie voor meer informatie [micro soft Defender voor identiteits integratie](https://docs.microsoft.com/cloud-app-security/aatp-integration).

## <a name="connect-to-microsoft-defender-for-identity"></a>Verbinding maken met micro soft Defender voor identiteit

Zorg ervoor dat de preview-versie van micro soft Defender voor identiteit is [ingeschakeld in uw netwerk](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Als micro soft Defender voor identiteit wordt geïmplementeerd en uw gegevens worden opgenomen, kunnen de verdachte waarschuwingen eenvoudig worden gestreamd naar Azure Sentinel. Het kan tot 24 uur duren voordat de waarschuwingen streamen naar Azure Sentinel starten.


1. Als u micro soft Defender voor identiteit wilt verbinden met Azure Sentinel, moet u eerst integratie tussen micro soft Defender inschakelen voor identiteit en Microsoft Cloud App Security. Zie voor meer informatie over hoe u dit doet, [micro soft Defender voor identiteits integratie](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. In azure Sentinel selecteert u **Data connectors** en klikt u vervolgens op de tegel **micro soft Defender voor identiteit (preview)** .

1. U kunt selecteren of u wilt dat de waarschuwingen van micro soft Defender voor identiteit automatisch incidenten in azure Sentinel automatisch genereren. Selecteer **inschakelen** onder **incidenten maken** om de standaard analyse regel in te scha kelen waarmee incidenten automatisch worden gemaakt op basis van waarschuwingen die zijn gegenereerd in de verbonden beveiligings service. U kunt deze regel vervolgens bewerken onder **analyse** en vervolgens op **actieve regels**.

1. Klik op **Verbinding maken**.

1. Zoek naar **SecurityAlert**om het relevante schema in log Analytics voor micro soft Defender voor identiteits waarschuwingen te gebruiken.

> [!NOTE]
> Als de waarschuwingen groter zijn dan 30 KB, wordt in azure Sentinel stopt het veld entities in de waarschuwingen weer gegeven.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u micro soft Defender kunt verbinden voor identiteiten voor Azure Sentinel. Zie de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over het [verkrijgen van inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag met [het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).

