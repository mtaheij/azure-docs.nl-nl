---
author: mattchenderson
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: mahender
ms.openlocfilehash: 3b282a99bb7f6f107717d9c265a46d285d03b849
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83649105"
---
Met een beheerde identiteit van Azure Active Directory (Azure AD) kan uw app eenvoudig toegang krijgen tot andere met Azure AD beveiligde resources, zoals Azure Key Vault. De identiteit wordt beheerd door het Azure-platform en u hoeft geen geheimen in te richten of te draaien. Zie [beheerde identiteiten voor Azure-resources](../articles/active-directory/managed-identities-azure-resources/overview.md)voor meer informatie over beheerde identiteiten in azure AD.

Aan uw toepassing kunnen twee typen identiteiten worden verleend:

- Een door het **systeem toegewezen identiteit** is gekoppeld aan uw toepassing en wordt verwijderd als uw app wordt verwijderd. Een app kan slechts één door het systeem toegewezen identiteit hebben.
- Een door de **gebruiker toegewezen identiteit** is een zelfstandige Azure-resource die aan uw app kan worden toegewezen. Een app kan meerdere door de gebruiker toegewezen identiteiten hebben.