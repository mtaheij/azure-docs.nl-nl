---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 091e4dc0759ce1b227349d5d1b6b08b9eda66ee0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84237711"
---
* Voor de conversie moet de VM opnieuw worden opgestart. Plan daarom de migratie van uw VM's tijdens een reeds bestaand onderhoudsvenster. 

* De conversie kan niet ongedaan worden gemaakt. 

* Houd er rekening mee dat gebruikers met de rol [Inzender voor virtuele machines](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) de VM-grootte niet kunnen wijzigen (zoals vóór de conversie). De reden hiervoor is dat de gebruiker van de virtuele machines van het besturings systeem de machtiging micro soft. Compute/disks/write moet hebben voor Vm's met Managed disks.

* Test de conversie. Migreer de test-VM voordat u de migratie in de productieomgeving uitvoert.

* Tijdens de conversie moet u de toewijzing van de VM ongedaan maken. De VM ontvangt een nieuw IP-adres wanneer deze na de conversie wordt opgestart. Indien vereist kunt u [een statisch IP-adres toewijzen](../articles/virtual-network/public-ip-addresses.md) aan de VM.

* Controleer de minimale versie van de Azure VM-agent die is vereist voor de ondersteuning van het conversie proces. Zie [minimale versie ondersteuning voor VM-agents in azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voor informatie over het controleren en bijwerken van de versie van de agent.