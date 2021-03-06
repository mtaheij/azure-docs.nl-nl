---
title: Voorbeeldsjablonen van Azure Resource Manager - Event Grid | Microsoft Docs
description: In dit artikel vindt u een lijst met Azure Resource Manager-sjabloonvoorbeelden voor Azure Event Grid op GitHub.
ms.topic: sample
ms.date: 07/07/2020
ms.openlocfilehash: 910012adf2dc930e6f1a26f1a7fc41f5ed0580c9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119052"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-sjablonen voor Event Grid

Zie [Microsoft.EventGrid resource types](/azure/templates/microsoft.eventgrid/allversions) (Microsoft.EventGrid-resourcetypen) voor de JSON-syntaxis en eigenschappen die u kunt gebruiken in een sjabloon. De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor Event Grid.

## <a name="event-grid-subscriptions"></a>Event Grid-abonnementen
- [Aangepast onderwerp en abonnement met webhook-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) - hiermee implementeert u een aangepast Event Grid-onderwerp. Hiermee maakt u een abonnement op dat aangepaste onderwerp dat gebruikmaakt van een WebHook-eindpunt. 
- [Aangepast onderwerp met EventHub-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler) - hiermee maakt u een Event Grid-abonnement op een aangepast onderwerp. Het abonnement maakt gebruik van een Event Hub voor het eindpunt. 
- [Abonnement voor Azure-abonnement of resourcegroep](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) - hiermee kunt u zich abonneren op gebeurtenissen voor een resourcegroep of een Azure-abonnement. De resourcegroep die u als het doel opgeeft tijdens de implementatie, is de bron van gebeurtenissen. Het abonnement maakt gebruik van een webhook voor het eindpunt. 
- [Blob-opslagaccount en abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) - hiermee implementeert u een Azure Blob-opslagaccount en abonneert u zich op gebeurtenissen van dat opslagaccount. 

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende voorbeelden:

- [PowerShell-voorbeelden](powershell-samples.md)
- [CLI-voorbeelden](cli-samples.md)
