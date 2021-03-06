---
title: Azure Media Services v2-accounts beheren | Microsoft Docs
description: Als u media-inhoud in azure wilt beheren, versleutelen, coderen, analyseren en streamen, moet u een Media Services-account maken. In dit artikel wordt uitgelegd hoe u Azure Media Services v2-accounts beheert.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: 167028cda6962445373b22c5f3d0a9e56c3e608a
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2020
ms.locfileid: "89255407"
---
# <a name="manage-azure-media-services-v2-accounts"></a>Azure Media Services v2-accounts beheren

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Als u media-inhoud in azure wilt beheren, versleutelen, coderen, analyseren en streamen, moet u een Media Services-account maken. Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. Het opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden.  

## <a name="moving-a-media-services-account-between-subscriptions"></a>Een Media Services account verplaatsen tussen abonnementen

Als u een Media Services account naar een nieuw abonnement wilt verplaatsen, moet u eerst de hele resource groep met het Media Services account naar het nieuwe abonnement verplaatsen. U moet alle gekoppelde resources verplaatsen: Azure Storage accounts, Azure CDN profielen, enzovoort. Zie [resources verplaatsen naar een nieuwe resource groep of een nieuw abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md)voor meer informatie. Net als bij resources in azure kan het enige tijd duren om de resource groep te verplaatsen.

Media Services v2 biedt geen ondersteuning voor het model multitenancy. Als u een Media Services account naar een abonnement in een nieuwe Tenant moet verplaatsen, maakt u een nieuwe Azure Active Directory (Azure AD)-toepassing in de nieuwe Tenant. Verplaats uw account vervolgens naar het abonnement in de nieuwe Tenant. Nadat de Tenant is verplaatst, kunt u beginnen met het gebruik van een Azure AD-toepassing van de nieuwe Tenant om toegang te krijgen tot het Media Services-account met behulp van de v2-Api's.

> [!IMPORTANT]
> U moet de [Azure AD-verificatie](media-services-portal-get-started-with-aad.md) gegevens opnieuw instellen om toegang te krijgen tot Media Services v2 API.
  
### <a name="considerations"></a>Overwegingen

* Maak back-ups van alle gegevens in uw account voordat u migreert naar een ander abonnement.
* U moet alle streaming-eind punten en live streaming-resources stoppen. Uw gebruikers hebben geen toegang tot uw inhoud voor de duur van het verplaatsen van de resource groep.

> [!IMPORTANT]
> Start het streaming-eind punt niet voordat de verplaatsing is voltooid.

### <a name="troubleshoot"></a>Problemen oplossen

Als een Media Services account of een gekoppeld Azure Storage account wordt ' losgekoppeld ' nadat de resource groep is verplaatst, roteert u de sleutels van het opslag account. Als door het draaien van de sleutels van het opslag account de status ' verbinding verbroken ' van het Media Services account niet wordt opgelost, wordt er een nieuwe ondersteunings aanvraag in het menu ' ondersteuning en probleem oplossing ' in het Media Services account opgeslagen.  

## <a name="next-steps"></a>Volgende stappen

[Een account maken](media-services-portal-create-account.md)
