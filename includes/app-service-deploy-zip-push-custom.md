---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 0019e50615f3e66778709ad8cb28f92967c66e2e
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050428"
---
## <a name="deployment-customization"></a>Implementatie aanpassing

Bij het implementatie proces wordt ervan uitgegaan dat het zip-bestand dat u pusht, een kant-en-klare app bevat. Standaard worden er geen aanpassingen uitgevoerd. Voeg het volgende toe aan de toepassings instellingen om dezelfde bouw processen in te scha kelen die u met continue integratie krijgt:

`SCM_DO_BUILD_DURING_DEPLOYMENT=true`

Wanneer u. zip push-implementatie gebruikt, is deze instelling standaard **Onwaar** . De standaard waarde is **True** voor continue integratie-implementaties. Als deze waarde is ingesteld op **True**, worden de implementatie-instellingen gebruikt tijdens de implementatie. U kunt deze instellingen configureren als app-instellingen of in een. implementatie configuratie bestand dat zich in de hoofdmap van uw zip-bestand bevindt. Zie instellingen voor opslag plaatsen [en implementaties](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) in de naslag informatie over de implementatie.