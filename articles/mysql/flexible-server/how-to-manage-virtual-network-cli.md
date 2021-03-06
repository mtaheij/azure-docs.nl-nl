---
title: Virtuele netwerken beheren-Azure CLI-Azure Database for MySQL-flexibele server
description: Virtuele netwerken maken en beheren voor Azure Database for MySQL-flexibele server met behulp van de Azure CLI
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 5cd35b896419dd30a8a4a18056ac1ccd48d7df6c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91331706"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-mysql---flexible-server-using-the-azure-cli"></a>Virtuele netwerken maken en beheren voor Azure Database for MySQL-flexibele server met behulp van de Azure CLI

> [!IMPORTANT]
> Azure Database for MySQL Flexible Server is momenteel beschikbaar als openbare preview

Azure Database for MySQL Flexibele server ondersteunt twee typen elkaar wederzijds uitsluitende netwerkverbindingsmethoden om verbinding te maken met uw flexibele server. De twee opties zijn:

- Openbare toegang (toegestane IP-adressen)
- Privétoegang (VNet-integratie)

In dit artikel wordt de nadruk gelegd op het maken van MySQL-server met **persoonlijke toegang (VNet-integratie)** met behulp van Azure cli. Met *persoonlijke toegang (VNet-integratie)* kunt u uw flexibele server implementeren in uw eigen [Azure-Virtual Network](../../virtual-network/virtual-networks-overview.md). Virtuele Azure-netwerken bieden particuliere en beveiligde netwerk communicatie. In persoonlijke toegang worden de verbindingen met de MySQL-server beperkt tot alleen binnen het virtuele netwerk. Raadpleeg voor meer informatie over [persoonlijke toegang (VNet-integratie)](./concepts-networking.md#private-access-vnet-integration).

In Azure Database for MySQL flexibele server kunt u de server alleen implementeren in een virtueel netwerk en een subnet tijdens het maken van de-server. Nadat de flexibele server op een virtueel netwerk en subnet is geïmplementeerd, kunt u deze niet naar een ander virtueel netwerk, subnet of *open bare toegang (toegestane IP-adressen)* verplaatsen.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

[Azure Cloud Shell](../../cloud-shell/overview.md) is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. Als u naar [https://shell.azure.com/bash](https://shell.azure.com/bash) gaat, kunt u Cloud Shell ook openen in een afzonderlijk browsertabblad. Selecteer **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en selecteer vervolgens **Enter** om de code uit te voeren.

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u voor deze snelstart versie 2.0 of hoger van Azure CLI nodig. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="prerequisites"></a>Vereisten

U moet zich aanmelden bij uw account met behulp van de opdracht [az login](https://docs.microsoft.com/cli/azure/reference-index#az-login). Let op de eigenschap **id** , die verwijst naar de **abonnements-id** voor uw Azure-account.

```azurecli-interactive
az login
```

Selecteer het specifieke abonnement in uw account met de opdracht [az account set](https://docs.microsoft.com/cli/azure/account#az-account-set). Noteer de **id-** waarde van de AZ- **aanmeld** uitvoer die moet worden gebruikt als de waarde voor het argument **abonnement** in de opdracht. Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. U kunt al uw abonnementen ophalen met de opdracht [az account list](https://docs.microsoft.com/cli/azure/account#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-azure-database-for-mysql-flexible-server-using-cli"></a>Azure Database for MySQL flexibele server maken met behulp van CLI
U kunt de `az mysql flexible-server` opdracht gebruiken om de flexibele server met *persoonlijke toegang (VNet-integratie)* te maken. Deze opdracht maakt gebruik van particuliere toegang (VNet-integratie) als de standaard verbindings methode. Er wordt voor u een virtueel netwerk en subnet gemaakt als er geen wordt weer gegeven. U kunt ook het bestaande virtuele netwerk en subnet met subnet-ID opgeven. <!-- You can provide the **vnet**,**subnet**,**vnet-address-prefix** or**subnet-address-prefix** to customize the virtual network and subnet.--> Er zijn verschillende opties voor het maken van een flexibele server met behulp van CLI, zoals wordt weer gegeven in de onderstaande voor beelden.

>[!Important]
> Met deze opdracht wordt het subnet gedelegeerd naar **micro soft. DBforMySQL/flexibleServers**. Deze delegatie houdt in dat alleen Azure Database for MySQL Flexibele servers dat subnet kunnen gebruiken. Er kunnen zich geen andere Azure-resourcetypen in het gedelegeerde subnet bevinden.
>

Raadpleeg de [naslag documentatie](/cli/azure/mysql/flexible-server) van Azure CLI voor de volledige lijst met CONFIGUREER bare cli-para meters. In de onderstaande opdrachten kunt u optioneel de resource groep opgeven.

- Een flexibele server maken met behulp van het standaard virtuele netwerk, het subnet met het standaard adres voorvoegsel
    ```azurecli-interactive
    az mysql flexible-server create
    ```
<!--- Create a flexible server using already existing virtual network and subnet
    ```azurecli-interactive
    az mysql flexible-server create --vnet myVnet --subnet mySubnet
    ```-->
- Maak een flexibele server met behulp van al het bestaande virtuele netwerk, subnet en de subnet-ID. Er mag geen andere bron in het subnet worden geïmplementeerd. dit subnet wordt overgedragen aan **micro soft. DBforMySQL/flexibleServers**, als dit nog niet is gedelegeerd.
    ```azurecli-interactive
    az mysql flexible-server create --subnet /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNetName}/subnets/{SubnetName}
    ```
    > [!Note]
    > Het virtuele netwerk en het subnet moeten zich in dezelfde regio en hetzelfde abonnement bevinden als uw flexibele server.
<!--
- Create a flexible server using new virtual network, subnet with non-default address prefix
    ```azurecli-interactive
    az mysql flexible-server create --vnet myVnet --vnet-address-prefix 10.0.0.0/24 --subnet mySubnet --subnet-address-prefix 10.0.0.0/24
    ```-->
Raadpleeg de [naslag documentatie](/cli/azure/mysql/flexible-server) van Azure CLI voor de volledige lijst met CONFIGUREER bare cli-para meters.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [netwerken in azure database for MySQL flexibele server](./concepts-networking.md).
- [Virtueel netwerk met Azure Database for MySQL Flexible Server maken met behulp van de Azure-portal](./how-to-manage-virtual-network-portal.md).
- Meer informatie over [Azure database for MySQL flexibel server virtueel netwerk](./concepts-networking.md#private-access-vnet-integration).
