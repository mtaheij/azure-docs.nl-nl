---
title: Toegang tot Kubernetes-resources via de Azure Portal (preview-versie)
description: Meer informatie over hoe u met Kubernetes-resources een Azure Kubernetes service-cluster (AKS) kunt beheren vanuit de Azure Portal.
services: container-service
author: laurenhughes
ms.topic: article
ms.date: 09/21/2020
ms.author: lahugh
ms.openlocfilehash: 6a9567669445cb5aa94c1108051c961a216fabad
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91335599"
---
# <a name="access-kubernetes-resources-from-the-azure-portal-preview"></a>Toegang tot Kubernetes-resources via de Azure Portal (preview-versie)

De Azure Portal bevat een Kubernetes resource Viewer (preview) voor eenvoudige toegang tot de Kubernetes-resources in uw Azure Kubernetes service (AKS)-cluster. Het weer geven van Kubernetes-resources in de Azure Portal reduceert de context wisseling tussen de Azure Portal en het `kubectl` opdracht regel programma, waarmee u de ervaring voor het weer geven en bewerken van uw Kubernetes-resources kunt stroom lijnen. De resource Viewer bevat momenteel meerdere resource typen, zoals implementaties, peulen en replica sets.

De resource weergave Kubernetes van de Azure Portal vervangt de [AKS dashboard-invoeg toepassing][kubernetes-dashboard], die is ingesteld voor afschaffing.

>[!NOTE]
>De capabilty wordt momenteel niet ondersteund in [particuliere Azure Kubernetes-Service clusters](https://docs.microsoft.com/azure/aks/private-clusters).

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="prerequisites"></a>Vereisten

Als u Kubernetes-resources wilt weer geven in de Azure Portal, hebt u een AKS-cluster nodig. Elk cluster wordt ondersteund, maar als de integratie van Azure Active Directory (Azure AD) wordt gebruikt, moet uw cluster gebruikmaken van [Azure AD-integratie met AKS-beheer][aks-managed-aad]. Als uw cluster verouderde Azure AD gebruikt, kunt u uw cluster bijwerken in de portal of met de [Azure cli][cli-aad-upgrade].

## <a name="view-kubernetes-resources"></a>Kubernetes-resources weer geven

Als u de Kubernetes-resources wilt bekijken, gaat u naar uw AKS-cluster in de Azure Portal. Het navigatie deel venster aan de linkerkant wordt gebruikt om toegang te krijgen tot uw resources. De resources zijn onder andere:

- In **naam ruimten** worden de naam ruimten van het cluster weer gegeven. Het filter aan de bovenkant van de lijst met naam ruimten biedt een snelle manier om uw naam ruimte bronnen te filteren en weer te geven.
- **Werk belastingen** toont informatie over implementaties, peulen, replica sets en daemon-sets die zijn geïmplementeerd in uw cluster. In de onderstaande scherm afbeelding ziet u het standaard systeem-peul in een voor beeld van een AKS-cluster.
- **Services en ingresses** toont alle service-en ingangs bronnen van uw cluster.

:::image type="content" source="media/kubernetes-portal/workloads.png" alt-text="Kubernetes pod-informatie die wordt weer gegeven in de Azure Portal." lightbox="media/kubernetes-portal/workloads.png":::

### <a name="deploy-an-application"></a>Een app implementeren

In dit voor beeld gebruiken we onze voor beeld-AKS-cluster om de Azure stem-toepassing te implementeren vanuit de [AKS Quick][portal-quickstart]start.

1. Selecteer **toevoegen** uit de resource weergaven (naam ruimte, werk belasting of services en ingresses).
1. Plak het YAML voor de Azure stem-toepassing vanuit de [AKS Quick][portal-quickstart]start.
1. Selecteer **toevoegen** aan de onderkant van de yaml-editor om de toepassing te implementeren. 

Zodra het YAML-bestand is toegevoegd, worden in de resource Viewer zowel Kubernetes-services weer gegeven die zijn gemaakt: de interne service (Azure-stem) als de externe service (Azure-stemmen) om toegang te krijgen tot de Azure stem-toepassing. De externe service bevat een gekoppeld extern IP-adres, zodat u de toepassing eenvoudig kunt bekijken in uw browser.

:::image type="content" source="media/kubernetes-portal/portal-services.png" alt-text="Informatie over Azure stem toepassingen die worden weer gegeven in de Azure Portal." lightbox="media/kubernetes-portal/portal-services.png":::

### <a name="monitor-deployment-insights"></a>Implementatie inzichten bewaken

AKS-clusters met [Azure monitor voor containers][enable-monitor] ingeschakeld kunnen snel implementatie inzichten weer geven. In de weer gave Kubernetes bronnen kunnen gebruikers de live status van afzonderlijke implementaties zien, met inbegrip van CPU-en geheugen gebruik, evenals overgang naar Azure monitor voor meer gedetailleerde informatie. Hier volgt een voor beeld van implementatie Insights van een voor beeld van een AKS-cluster:

:::image type="content" source="media/kubernetes-portal/deployment-insights.png" alt-text="Implementatie Insights wordt weer gegeven in de Azure Portal." lightbox="media/kubernetes-portal/deployment-insights.png":::

## <a name="edit-yaml"></a>YAML bewerken

De resource weergave Kubernetes bevat ook een YAML-editor. Met een ingebouwde YAML-editor kunt u services en implementaties bijwerken of maken vanuit de portal en wijzigingen direct Toep assen.

:::image type="content" source="media/kubernetes-portal/service-editor.png" alt-text="YAML-editor voor een Kubernetes-service die wordt weer gegeven in de Azure Portal.":::

Nadat u de YAML hebt bewerkt, worden de wijzigingen toegepast door **revisie + opslaan**te selecteren, de wijzigingen te bevestigen en vervolgens opnieuw op te slaan.

>[!WARNING]
> Het uitvoeren van directe productie wijzigingen via de gebruikers interface of CLI wordt niet aanbevolen. u moet gebruikmaken van [doorlopende integratie (CI) en doorlopende implementatie (cd) best practices](kubernetes-action.md). De Azure Portal Kubernetes-beheer mogelijkheden en de YAML-editor zijn gebouwd voor het leren en testen van nieuwe implementaties in een ontwikkelings-en test instelling.

## <a name="troubleshooting"></a>Problemen oplossen

Deze sectie heeft betrekking op veelvoorkomende problemen en stappen voor probleem oplossing.

### <a name="unauthorized-access"></a>Onbevoegde toegang

Om toegang te krijgen tot de Kubernetes-resources, moet u toegang hebben tot het AKS-cluster, de Kubernetes-API en de Kubernetes-objecten. Zorg ervoor dat u een cluster beheerder of een gebruiker met de juiste machtigingen hebt voor toegang tot het AKS-cluster. Zie [toegangs-en identiteits opties voor AKS][concepts-identity]voor meer informatie over de beveiliging van het cluster.

>[!NOTE]
> De resource weergave kubernetes in azure portal wordt alleen ondersteund door [beheerde Aad-clusters](managed-aad.md) of clusters die niet zijn ingeschakeld voor Aad. Als u gebruikmaakt van een cluster waarvoor Managed AAD is ingeschakeld, moet uw AAD-gebruiker of-identiteit de respectieve rollen/rollen bindingen hebben om toegang te krijgen tot de kubernetes-API, naast de machtiging voor het ophalen van de [gebruiker `kubeconfig` ](control-kubeconfig-access.md).

### <a name="enable-resource-view"></a>Resource weergave inschakelen

Voor bestaande clusters moet u mogelijk de resource weergave Kubernetes inschakelen. Als u de resource weergave wilt inschakelen, volgt u de aanwijzingen in de portal voor uw cluster.

:::image type="content" source="media/kubernetes-portal/enable-resource-view.png" alt-text="Azure Portal bericht om de resource weergave Kubernetes in te scha kelen." lightbox="media/kubernetes-portal/enable-resource-view.png":::

> [!TIP]
> De AKS-functie voor door de [**API server geautoriseerde IP-adresbereiken**](api-server-authorized-ip-ranges.md) kan worden toegevoegd om de API-server toegang alleen te beperken tot het open bare eind punt van de firewall. Een andere optie voor dergelijke clusters is het bijwerken `--api-server-authorized-ip-ranges` om toegang te bieden voor een lokale client computer of een IP-adres bereik (van waaruit de portal wordt gebladerd). U hebt het open bare IPv4-adres van de computer nodig om deze toegang toe te staan. U kunt dit adres vinden met de onderstaande opdracht of door te zoeken in ' wat is mijn IP-adres ' in een Internet browser.
```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKSNAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt uitgelegd hoe u toegang krijgt tot Kubernetes-resources voor uw AKS-cluster. Zie [implementaties en yaml-manifesten][deployments] voor een dieper inzicht in cluster bronnen en de YAML-bestanden die worden gebruikt met de Kubernetes resource viewer.

<!-- LINKS - internal -->
[kubernetes-dashboard]: kubernetes-dashboard.md
[concepts-identity]: concepts-identity.md
[portal-quickstart]: kubernetes-walkthrough-portal.md#run-the-application
[deployments]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[aks-managed-aad]: managed-aad.md
[cli-aad-upgrade]: managed-aad.md#upgrading-to-aks-managed-azure-ad-integration
[enable-monitor]: ../azure-monitor/insights/container-insights-enable-existing-clusters.md
