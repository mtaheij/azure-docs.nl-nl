---
title: Een SQL-pool maken met een Azure Resource Manager-sjabloon
description: Meer informatie over het maken van een Azure Synapse Analytics SQL-pool met behulp van een Azure Resource Manager-sjabloon.
services: azure-resource-manager
author: julieMSFT
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: jrasnick
ms.date: 06/09/2020
ms.openlocfilehash: 29d4e4d696b34aa493714c870ebb466f491c47fe
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2020
ms.locfileid: "88641871"
---
# <a name="quickstart-create-an-azure-synapse-analytics-sql-pool-by-using-an-arm-template"></a>Quickstart: Een Azure Synapse Analytics SQL-pool maken met een ARM-sjabloon

Met deze Azure Resource Manager-sjabloon (ARM-sjabloon) wordt een Azure Synapse Analytics SQL-pool gemaakt waarvoor Transparent Data Encryption is ingeschakeld. Synapse SQL-pool verwijst naar de functies voor datawarehousing voor ondernemingen, die algemeen beschikbaar zijn in Azure Synapse.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Als uw omgeving voldoet aan de vereisten en u benkend bent met het gebruik van ARM-sjablonen, selecteert u de knop **Implementeren naar Azure**. De sjabloon wordt in Azure Portal geopend.

[![Implementeren in Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="review-the-template"></a>De sjabloon controleren

De sjabloon die in deze quickstart wordt gebruikt, komt uit [Azure-quickstartsjablonen](https://azure.microsoft.com/resources/templates/201-sql-data-warehouse-transparent-encryption-create/).

:::code language="json" source="~/quickstart-templates/201-sql-data-warehouse-transparent-encryption-create/azuredeploy.json":::

In de sjabloon is een resource gedefinieerd:

- [Microsoft.Sql/servers](/azure/templates/microsoft.sql/servers)

## <a name="deploy-the-template"></a>De sjabloon implementeren

1. Selecteer de volgende afbeelding om u aan te melden bij Azure en de sjabloon te openen. Met deze sjabloon maakt u een Synapse SQL-pool.
   
   [![Implementeren in Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

1. Voer de volgende waarden in of werk deze bij:

   * **Abonnement**: Selecteer een Azure-abonnement.
   * **Resourcegroep**: Selecteer **Nieuwe maken**, geef een unieke naam op voor de resourcegroep en selecteer **OK**. Met een nieuwe resourcegroep kunt u makkelijker resources opschonen.
   * **Regio**: Selecteer een regio.  Bijvoorbeeld **VS - centraal**.
   * **SQL-servernaam**: Accepteer de standaardnaam of voer een naam in voor de SQL-servernaam.
   * **Aanmeldgegevens van SQL-beheerder**: Geef de gebruikersnaam van de beheerder voor de SQL-server op.
   * **Wachtwoord van SQL-beheerder**: Geef het beheerderswachtwoord voor de SQL-server op.
   * **Naam van datawarehouse**: Voer de naam van de SQL-pool in.
   * **Transparent Data Encryption**: Accepteer de standaardwaarde Ingeschakeld. 
   * **Service Level Objective**: Accepteer de standaardwaarde DW400c.
   * **Locatie**: Accepteer de standaardlocatie van de resourcegroep.
   * **Controleren en maken**: Selecteren.
   * **Maken**: Selecteren.

## <a name="review-deployed-resources"></a>Geïmplementeerde resources bekijken

U kunt de Azure-portal gebruiken om de geïmplementeerde resources te controleren of Azure CLI of een Azure PowerShell-script gebruiken om de geïmplementeerde resources weer te geven.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the resource group where your Synapse SQL pool exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name where your SQL pool account exists"
(Get-AzResource -ResourceType "Microsoft.Sql/servers/databases" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig hebt, verwijdert u de resourcegroep met Azure CLI of Azure PowerShell:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een Azure Synapse Analytics SQL-pool gemaakt met behulp van een ARM-sjabloon en de implementatie gevalideerd. Als u meer wilt weten over Azure Synapse Analytics en Azure Resource Manager, vindt u meer informatie in de onderstaande artikelen.

- Lees een [Overzicht van Azure Synapse Analytics](sql-data-warehouse-overview-what-is.md)
- Meer informatie over [Azure Resource Manager](../../azure-resource-manager/management/overview.md)
- [Uw eerste ARM-sjabloon maken en implementeren](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)
