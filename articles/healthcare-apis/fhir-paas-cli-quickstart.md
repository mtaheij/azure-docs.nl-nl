---
title: 'Quickstart: Azure API for FHIR implementeren met Azure CLI'
description: In deze quickstart wordt uitgelegd hoe u Azure API for FHIR implementeert in Azure met behulp van Azure CLI.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: matjazl
ms.custom: devx-track-azurecli
ms.openlocfilehash: e1aa3362217a4edcfcf547c69b9effde00459f7e
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2020
ms.locfileid: "87850542"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>Quickstart: Azure API for FHIR implementeren met Azure CLI

In deze quickstart wordt uitgelegd hoe u Azure API for FHIR implementeert in Azure met behulp van Azure CLI.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-healthcareapis-extension"></a>HealthcareAPIs-extensie toevoegen

```azurecli-interactive
az extension add --name healthcareapis
```

Een lijst met opdrachten voor HealthcareAPIs ophalen:

```azurecli-interactive
az healthcareapis --help
```

## <a name="create-azure-resource-group"></a>Azure-resourcegroep maken

Kies een naam voor de resourcegroep die de Azure API for FHIR bevat en maak deze:

```azurecli-interactive
az group create --name "myResourceGroup" --location westus2
```

## <a name="deploy-the-azure-api-for-fhir"></a>De Azure API for FHIR implementeren

```azurecli-interactive
az healthcareapis create --resource-group myResourceGroup --name nameoffhiraccount --kind fhir-r4 --location westus2 
```

## <a name="fetch-fhir-api-capability-statement"></a>FHIR API-mogelijheidsinstructie ophalen

Krijg een mogelijkheidsinstructie van de FHIR API met:

```azurecli-interactive
curl --url "https://nameoffhiraccount.azurehealthcareapis.com/metadata"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze toepassing verder niet gaat gebruiken, verwijder dan de resourcegroep met de volgende stappen:

```azurecli-interactive
az group delete --name "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u de Azure API for FHIR geïmplementeerd in uw abonnement. Als u aanvullende instellingen wilt instellen in uw Azure API for FHIR, gaat u verder met de handleiding bij aanvullende instellingen.

>[!div class="nextstepaction"]
>[Aanvullende instellingen in Azure API for FHIR](azure-api-for-fhir-additional-settings.md)
