---
title: Voorbeelden van Azure CLI - Apps installeren
description: Met dit script maakt u een virtuele-machineschaalset die in Ubuntu wordt uitgevoerd en die de Custom Script-extensie gebruikt voor het installeren van een eenvoudige webtoepassing.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: cli
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 78569459de9d6a632f256d25ffb0870e36ce4a79
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499684"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-the-azure-cli"></a>Apps installeren in een virtuele-machineschaalset met de Azure CLI
Met dit script maakt u een virtuele-machineschaalset die in Ubuntu wordt uitgevoerd en die de Custom Script-extensie gebruikt voor het installeren van een eenvoudige webtoepassing. Nadat het script is uitgevoerd, kunt u de web-app via een webbrowser openen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/install-apps/install-apps.sh "Install apps into a scale set")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep, de schaalset en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script
In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtuele-machineschaalset en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/ad/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az vmss create](/cli/azure/vmss) | Hiermee maakt u de virtuele machine en verbindt u deze met het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. Met deze opdracht geeft u ook de VM-installatiekopie op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [az vmss extension set](/cli/azure/vmss/extension) | Hiermee installeert u de aangepaste scriptextensie van Azure om een script uit te voeren dat de gegevensschijven op elk VM-exemplaar voorbereidt. |
| [az network lb rule create](/cli/azure/network/lb/rule) | Hiermee maakt u een load balancer-regel voor het distribueren van verkeer op TCP-poort 80 voor VM-exemplaren in de schaalset. |
| [az network public-ip show](/cli/azure/network/public-ip) | Hiermee haalt u informatie op over het toegewezen openbare IP-adres dat door de load balancer wordt gebruikt. |
| [az group delete](/cli/azure/ad/group) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de [documentatie van Azure CLI](/cli/azure/overview) voor meer informatie over de Azure CLI.
