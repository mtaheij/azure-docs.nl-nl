---
title: CLI-voorbeeld van actieve geo-replicatie voor één Azure SQL Database
description: Azure CLI-voorbeeldscript voor het instellen van actieve geo-replicatie voor een individuele database in Azure SQL Database en het uitvoeren van een failover.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 48ebf87a732380188b1c5f03954385b61bc765f9
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528822"
---
# <a name="use-cli-to-configure-active-geo-replication-for-a-single-database-in-azure-sql-database"></a>CLI gebruiken voor het configureren van actieve geo-replicatie voor een individuele database in Azure SQL Database

In dit voorbeeld van een Azure CLI-script configureert u actieve geo-replicatie voor één database en voert u de failover uit naar een secundaire replica van de database.

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Voorbeeldscript

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Het script uitvoeren

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-single-database.sh "Set up active geo-replication for single database")]

### <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Voorbeeldverwijzing

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Beschrijving |
|---|---|
| [az sql db replica](/cli/azure/sql/db/replica) | Opdrachten voor databasereplicatie. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende voorbeelden van SQL Database CLI-scripts vindt u in de [documentatie van Azure SQL Database](../../azure-sql/database/az-cli-script-samples-content-guide.md).