---
title: 'Quickstart: een Azure-sleutelkluis maken met behulp van Azure PowerShell'
description: Quickstart voor het maken van een Azure-sleutelkluis met behulp van Azure PowerShell
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/2019
ms.author: mbaldwin
ms.openlocfilehash: e24f1e546de1ce01896e271dbc9155c47f6c2bb6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101045"
---
# <a name="quickstart-create-a-key-vault-using-powershell"></a>Quickstart: een sleutelkluis maken met behulp van PowerShell

Azure Key Vault is een cloudservice die een beveiligd archief biedt voor [sleutels](../keys/index.yml), [geheimen](../secrets/index.yml) en [certificaten](../certificates/index.yml). Zie [Over Azure Key Vault](overview.md) voor meer informatie over Key Vault. Zie [Sleutels, geheimen en certificaten](about-keys-secrets-certificates.md) voor meer informatie over wat er kan worden opgeslagen in een sleutelkluis.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

In deze quickstart maakt u een sleutelkluis met behulp van [Azure PowerShell](/powershell/azure/). Als u ervoor kiest om PowerShell lokaal te installeren en gebruiken, is versie 1.0.0 of hoger van de Azure PowerShell-module vereist voor deze zelfstudie. Typ `$PSVersionTable.PSVersion` om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met behulp van de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```azurepowershell-interactive
New-AzResourceGroup -Name 'myResourceGroup" -Location "EastUS"
```

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Maak een sleutelkluis in de resourcegroep uit de vorige stap. U moet enkele gegevens verstrekken:

- Naam van de sleutelkluis: Een tekenreeks van 3 tot en met 24 tekens, die alleen cijfers (0-9), letters (a-z, A-Z) en afbreekstreepjes (-) mag bevatten.

  > [!Important]
  > Elke sleutelkluis moet een unieke naam hebben. Vervang <your-unique-keyvault-name> door de naam van uw sleutelkluis in de volgende voorbeelden.

- Naam van resourcegroep: **myResourceGroup**.
- De locatie: **EastUS**.

```azurepowershell-interactive
New-AzKeyVault -Name "&lt;your-unique-key-vault-name&gt; -ResourceGroupName "myResourceGroup" -Location "East US"
```

De uitvoer van deze cmdlet toont eigenschappen van de nieuw gemaakte sleutelkluis. Let op de onderstaande twee eigenschappen:

- **Kluisnaam**: de naam die u hierboven hebt opgegeven voor de parameter --name.
- **Kluis-URI**: In het voorbeeld is dit https://&lt;unieke-naam-van-uw-sleutelkluis&gt;.vault.azure.net/. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="clean-up-resources"></a>Resources opschonen

Andere snelstartgidsen en zelfstudies in deze verzameling zijn gebaseerd op deze snelstartgids. Als u van plan bent om verder te gaan met andere snelstarts en zelfstudies, kunt u deze resources intact laten.

U kunt de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) van Azure PowerShell gebruiken om de resourcegroep en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een sleutelkluis gemaakt en daar een geheim in opgeslagen. Voor meer informatie over Key Vault en hoe u Key Vault integreert met uw toepassingen gaat u verder naar de artikelen hieronder.

- Lees een [Overzicht van Azure Key Vault](overview.md)
- Zie de referentie voor de [Azure PowerShell Key Vault-cmdlets](/powershell/module/az.keyvault/?view=azps-2.6.0#key_vault)
- Bekijk de [best practices voor Azure Key Vault](best-practices.md)
