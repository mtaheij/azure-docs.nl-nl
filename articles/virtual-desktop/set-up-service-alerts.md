---
title: Servicewaarschuwingen instellen voor Windows Virtual Desktop - Azure
description: Hoe stel ik Azure Service Health in om servicemeldingen voor Windows Virtual Desktop te ontvangen?
author: Heidilohr
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0391102683ebafba63c429072c8afa9f24223955
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009423"
---
# <a name="tutorial-set-up-service-alerts"></a>Zelfstudie: Servicewaarschuwingen instellen

>[!IMPORTANT]
>Deze inhoud is van toepassing op Windows Virtual Desktop met Azure Resource Manager Windows Virtual Desktop-objecten. Zie [dit artikel](./virtual-desktop-fall-2019/set-up-service-alerts-2019.md) als u Windows Virtual Desktop (klassiek) zonder Azure Resource Manager-objecten gebruikt.

U kunt Azure Service Health gebruiken om serviceproblemen en statusadviezen voor Windows Virtual Desktop te monitoren. Via Azure Service Health kunt u verschillende soorten meldingen ontvangen (bijvoorbeeld e-mail of sms), inzicht krijgen in de impact van een probleem en op de hoogte worden gehouden tijdens het oplossen van een probleem. Ook kan Azure Service Health u helpen bij het verminderen van downtime en bij de voorbereiding op gepland onderhoud en wijzigingen die de beschikbaarheid van uw bronnen kunnen beïnvloeden.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * servicewaarschuwingen maken en configureren.

Bekijk de [documentatie over Azure Health](https://docs.microsoft.com/azure/service-health/) voor meer informatie over Azure Service Health.

## <a name="create-service-alerts"></a>Servicewaarschuwingen maken

In deze sectie wordt uitgelegd hoe u Azure Service Health configureert en hoe u meldingen instelt die u kunt openen op de Microsoft Azure-portal. U kunt verschillende soorten waarschuwingen instellen en deze zo plannen dat ze u tijdig op de hoogte brengen.

### <a name="recommended-service-alerts"></a>Aanbevolen servicewaarschuwingen

We raden u aan servicewaarschuwingen te maken voor de volgende statusgebeurtenistypes:

- **Serviceprobleem:** ontvang meldingen over belangrijke problemen die van invloed zijn op de connectiviteit van uw gebruikers met de service of met de mogelijkheid om uw Windows Virtual Desktop-tenant te beheren.
- **Statusadvies:** ontvang meldingen die uw aandacht vereisen. Hier volgen enkele voorbeelden van dit type melding:
    - Virtual Machines (Vm's) niet veilig geconfigureerd als open poort 3389
    - Afkeuring van functionaliteit

### <a name="configure-service-alerts"></a>Servicewaarschuwingen configureren

Om servicewaarschuwingen te configureren:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Service Health.**
3. Volg de instructies in [Waarschuwingen voor activiteitenlogboeken maken met servicemeldingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) om uw waarschuwingen en meldingen in te stellen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u Azure Service Health instelt en gebruikt om serviceproblemen en statusadviezen voor Windows Virtual Desktop te monitoren. Voor meer informatie over hoe u zich aanmeldt bij Windows Virtual Desktop, gaat u verder met de instructies voor verbinding maken met Windows Virtual Desktop.

> [!div class="nextstepaction"]
> [Verbinding maken met de Remote Desktop-client op Windows 7 en Windows 10](./connect-windows-7-10.md)
