---
title: Hybride verbinding door geven als gebeurtenis-handler voor Azure Event Grid gebeurtenissen
description: Hierin wordt beschreven hoe u Azure Relay hybride verbindingen als gebeurtenis-handlers kunt gebruiken voor Azure Event Grid-gebeurtenissen.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 3807e2d125d652b8f5ed7c9dec1b972d69f699f6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91270199"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Hybride verbinding door geven als gebeurtenis-handler voor Azure Event Grid gebeurtenissen
Een gebeurtenis-handler is de plaats waar de gebeurtenis wordt verzonden. De handler heeft een aantal verdere acties nodig om de gebeurtenis te verwerken. Verschillende Azure-Services worden automatisch geconfigureerd voor het afhandelen van gebeurtenissen en **Azure relay** is een hiervan. 

Gebruik Azure **Relay hybride verbindingen** om gebeurtenissen te verzenden naar toepassingen die zich binnen een bedrijfs netwerk bevinden en geen openbaar toegankelijk eind punt hebben.

## <a name="tutorials"></a>Zelfstudies
Raadpleeg de volgende zelf studie voor een voor beeld van het gebruik van een Azure Relay hybride verbinding als een gebeurtenis-handler. 

|Titel  |Beschrijving  |
|---------|---------|
| [Zelf studie: gebeurtenissen verzenden naar hybride verbinding](custom-event-to-hybrid-connection.md) | Hiermee wordt een aangepaste gebeurtenis verzonden naar een bestaande hybride verbinding voor verwerking door een listener-toepassing. |

## <a name="rest-example-for-put"></a>REST-voor beeld (voor PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "HybridConnection",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Relay/namespaces/<RELAY NAMESPACE NAME>/hybridconnections/<HYBRID CONNECTION NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

> [!NOTE]
> Het leveren van gebeurtenissen aan een Azure Relay hybride verbinding in **een andere Tenant** wordt niet ondersteund. 

## <a name="next-steps"></a>Volgende stappen
Zie het artikel over [gebeurtenis-handlers](event-handlers.md) voor een lijst met ondersteunde gebeurtenis-handlers. 