---
title: 'Quickstart: Een server maken - Azure CLI - Azure Database for MySQL - Flexible Server'
description: In deze snelstartgids wordt beschreven hoe u Azure CLI gebruikt om een Azure Database for MySQL Flexible Server in een Azure-resourcegroep te maken.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 9/21/2020
ms.custom: mvc
ms.openlocfilehash: bae6e9f04eced02130ae628d5308a87a1baaa8fa
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90945038"
---
# <a name="quickstart-create-an-azure-database-for-mysql-flexible-server-using-azure-cli"></a>Quickstart: Een Azure Database for MySQL Flexible Server maken met behulp van Azure CLI

In deze snelstartgids wordt beschreven hoe u de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)-opdrachten in [Azure Cloud Shell](https://shell.azure.com) gebruikt om binnen ongeveer vijf minuten een Azure Database for MySQL Flexible Server te maken. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!IMPORTANT] 
> Azure Database for MySQL Flexible Server is momenteel beschikbaar als openbare preview

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

[Azure Cloud Shell](../../cloud-shell/overview.md) is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. Als u naar [https://shell.azure.com/bash](https://shell.azure.com/bash) gaat, kunt u Cloud Shell ook openen in een afzonderlijk browsertabblad. Selecteer **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en selecteer vervolgens **Enter** om de code uit te voeren.

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u voor deze snelstart versie 2.0 of hoger van Azure CLI nodig. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="prerequisites"></a>Vereisten

U moet zich aanmelden bij uw account met behulp van de opdracht [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login). Let op de eigenschap **id**, die verwijst naar **abonnements-id** voor uw Azure-account.

```azurecli-interactive
az login
```

Selecteer het specifieke abonnement in uw account met de opdracht [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Noteer de **id**-waarde uit de uitvoer van **az login** en gebruik deze als de waarde voor het argument **abonnement** in de opdracht. Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. U kunt al uw abonnementen ophalen met de opdracht [az account list](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-a-flexible-server"></a>Een flexibele server maken

Maak een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) met behulp van de opdracht `az group create`, en maak vervolgens de flexibele MySQL-server in deze resourcegroep. U moet een unieke naam opgeven. In het volgende voorbeeld wordt een resourcegroep met de naam `myresourcegroup` gemaakt op de locatie `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

Maak een flexibele server met de opdracht `az mysql flexible-server create`. Een server kan meerdere databases bevatten. Met de volgende opdracht maakt u een server met de standaardinstellingen van de service en waarden van de [lokale context](https://docs.microsoft.com/cli/azure/local-context?view=azure-cli-latest) van Azure CLI: 

```azurecli
az mysql flexible-server create
```

De gemaakte server heeft de volgende kenmerken: 
- Automatisch gegenereerde servernaam, beheerdersnaam, beheerderswachtwoord, resourcegroepnaam (indien nog niet opgegeven in de lokale context) en op dezelfde locatie als de resourcegroep 
- Servicestandaardwaarden voor resterende serverconfiguraties: rekenlaag (Burstable), rekenomvang/SKU (B1MS), bewaartermijn voor back-ups (7 dagen) en MySQL-versie (5.7)
- De standaard verbindingsmethode is privétoegang (VNet-integratie) met een automatisch gegenereerd virtueel netwerk en subnet

> [!NOTE] 
> De verbindingsmethode kan niet worden gewijzigd na het maken van de server. Als u bijvoorbeeld *Privétoegang (VNet-integratie)* hebt geselecteerd tijdens het maken, kunt u na het maken niet wijzigen naar *Openbare toegang (toegestane IP-adressen)* . U kunt het beste een server met privétoegang maken om veilig toegang te krijgen tot uw server met behulp van VNet-integratie. Meer informatie over privétoegang vindt u in het [artikel over concepten](./concepts-networking.md).

Als u de standaardinstellingen wilt wijzigen, raadpleegt u de Azure CLI-referentiedocumentatie <!--FIXME --> voor de volledige lijst met configureerbare CLI-parameters. 

> [!NOTE]
> Verbindingen met Azure Database voor MySQL communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Als u verbinding met uw server wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

```azurecli-interactive
az mysql flexible-server show --resource-group myresourcegroup --name mydemoserver
```

Het resultaat wordt in JSON-indeling weergegeven. Noteer de **fullyQualifiedDomainName** en de **administratorLogin**.

<!--FIXME-->
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/flexibleServers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 1,
    "name": "Standard_B1ms",
    "size": null,
    "tier": "Burstable"
  },
  "publicAccess": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/flexibleServers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-using-mysql-command-line-client"></a>Verbinding maken met de MySQL-opdrachtregel-client

Aangezien de flexibele server is gemaakt met *Privétoegang (VNet-integratie)* , moet u verbinding maken met uw server vanuit een resource binnen hetzelfde VNet als uw server. U kunt een virtuele machine maken en toevoegen aan het gemaakte virtuele netwerk. 

Nadat de VM is gemaakt, kunt u zich met SSH aanmelden bij de machine en het opdrachtregelprogramma **[mysql.exe](https://dev.mysql.com/downloads/)** installeren.

Met mysql.exe kunt u verbinding maken met behulp van de onderstaande opdracht. Vervang de waarden door uw werkelijke servernaam en wachtwoord. 

```bash
 mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere Quickstart of zelfstudie nodig hebt, kunt u ze verwijderen met de volgende opdracht:

```azurecli-interactive
az group delete --name myresourcegroup
```

Als u alleen de ene zojuist gemaakte server wilt verwijderen, kunt u de opdracht `az mysql server delete` uitvoeren.

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Een PHP-web-app (Laravel) bouwen met MySQL](tutorial-php-database-app.md)