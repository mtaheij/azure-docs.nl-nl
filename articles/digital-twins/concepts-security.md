---
title: Beveiliging voor Azure Digital Twins-oplossingen
titleSuffix: Azure Digital Twins
description: Meer informatie over best practices voor beveiliging met Azure Digital Apparaatdubbels.
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 4dc768b92a9ffbeafc31ef81d065237f47fb645e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91331876"
---
# <a name="secure-azure-digital-twins"></a>Azure Digital Apparaatdubbels beveiligen

Voor de beveiliging biedt Azure Digital Apparaatdubbels nauw keurige toegangs controle over specifieke gegevens, resources en acties in uw implementatie. Dit wordt gedaan via een gedetailleerde beheer strategie voor rollen en machtigingen die **op rollen gebaseerd toegangs beheer (RBAC)** wordt genoemd. Meer informatie over de algemene principes van RBAC voor Azure vindt u [hier](../role-based-access-control/overview.md).

Azure Digital Apparaatdubbels biedt ook ondersteuning voor versleuteling van gegevens in rust.

## <a name="granting-permissions-with-rbac"></a>Machtigingen verlenen met RBAC

RBAC wordt geleverd aan Azure Digital Apparaatdubbels via integratie met [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD).

U kunt RBAC gebruiken om machtigingen te verlenen aan een *beveiligingsprincipal*, die een gebruiker, een groep of een Application Service-Principal kan zijn. De beveiligingsprincipal wordt geverifieerd door Azure AD en ontvangt een OAuth 2,0-token als resultaat. Dit token kan worden gebruikt voor het autoriseren van een toegangs aanvraag aan een Azure Digital Apparaatdubbels-exemplaar.

### <a name="authentication-and-authorization"></a>Verificatie en autorisatie

In azure AD is Access een proces dat uit twee stappen bestaat. Wanneer een beveiligingsprincipal (een gebruiker, groep of toepassing) probeert toegang te krijgen tot Azure Digital Apparaatdubbels, moet de aanvraag worden *geverifieerd* en *geautoriseerd*. 

1. Eerst wordt de identiteit van de beveiligingsprincipal *geverifieerd*en wordt een OAuth 2,0-token geretourneerd.
2. Vervolgens wordt het token door gegeven als onderdeel van een aanvraag voor de Azure Digital Apparaatdubbels-service om toegang tot de opgegeven bron te *autoriseren* .

Voor de verificatie stap moet elke toepassings aanvraag een OAuth 2,0-toegangs token tijdens runtime bevatten. Als een toepassing wordt uitgevoerd binnen een Azure-entiteit, zoals een [Azure functions](../azure-functions/functions-overview.md) -app, kan deze een **beheerde identiteit** gebruiken om toegang te krijgen tot de resources. Meer informatie over beheerde identiteiten vindt u in de volgende sectie.

De autorisatie stap vereist dat een Azure-rol wordt toegewezen aan de beveiligingsprincipal. De rollen die zijn toegewezen aan een beveiligingsprincipal, bepalen de machtigingen die de principal heeft. Azure Digital Apparaatdubbels biedt Azure-rollen die sets machtigingen voor Azure Digital Apparaatdubbels-resources omvatten. Deze rollen worden verderop in dit artikel beschreven.

Zie [*inzicht in de verschillende rollen*](../role-based-access-control/rbac-and-directory-admin-roles.md) in de documentatie van Azure RBAC voor meer informatie over rollen en roltoewijzingen die worden ondersteund in Azure.

#### <a name="authentication-with-managed-identities"></a>Verificatie met beheerde identiteiten

[Beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) is een functie van meerdere Azure waarmee u een beveiligde identiteit kunt maken die is gekoppeld aan de implementatie waarin de toepassings code wordt uitgevoerd. U kunt deze identiteit vervolgens koppelen aan toegangs beheer rollen om aangepaste machtigingen te verlenen voor toegang tot specifieke Azure-resources die uw toepassing nodig heeft.

Met beheerde identiteiten beheert het Azure-platform deze runtime-identiteit. U hoeft geen toegangs sleutels op te slaan en te beveiligen in uw toepassings code of configuratie, hetzij voor de identiteit zelf, hetzij voor de resources die u nodig hebt. Een Azure Digital Apparaatdubbels-client-app die binnen een Azure App Service-toepassing wordt uitgevoerd, hoeft geen SAS-regels en sleutels of andere toegangs tokens te verwerken. De client-app heeft alleen het eindpunt adres van de Azure Digital Apparaatdubbels-naam ruimte nodig. Wanneer de app verbinding maakt, koppelt Azure Digital Apparaatdubbels de context van de beheerde entiteit aan de client. Zodra deze is gekoppeld aan een beheerde identiteit, kan uw Azure Digital Apparaatdubbels-client alle geautoriseerde bewerkingen uitvoeren. De autorisatie wordt vervolgens verleend door een beheerde entiteit te koppelen aan een Azure Digital Apparaatdubbels Azure-rol (hieronder beschreven).

#### <a name="authorization-azure-roles-for-azure-digital-twins"></a>Autorisatie: Azure-functies voor Azure Digital Apparaatdubbels

Azure biedt de onderstaande ingebouwde Azure-rollen voor het verlenen van toegang tot een Azure Digital Apparaatdubbels-resource:
* *Azure Digital apparaatdubbels-eigenaar (preview)* : gebruik deze rol om volledige toegang te geven tot Azure Digital apparaatdubbels-resources.
* *Azure Digital Apparaatdubbels Reader (preview)* : gebruik deze functie om alleen-lezen toegang te verlenen aan Azure Digital apparaatdubbels-resources.

> [!TIP]
> De rol *Azure Digital Apparaatdubbels Reader (preview)* biedt nu ook ondersteuning voor Browse relaties.

Zie voor meer informatie over hoe ingebouwde rollen worden gedefinieerd [*begrijpen functie definities*](../role-based-access-control/role-definitions.md) in de documentatie van Azure RBAC. Zie [*aangepaste rollen in azure*](../role-based-access-control/custom-roles.md)voor meer informatie over het maken van aangepaste Azure-rollen.

U kunt rollen op twee manieren toewijzen:
* via het deel venster toegangs beheer (IAM) voor Azure Digital Apparaatdubbels in de Azure Portal (Zie [*roltoewijzingen toevoegen of verwijderen met behulp van Azure RBAC en de Azure Portal*](../role-based-access-control/role-assignments-portal.md))
* via CLI-opdrachten om een rol toe te voegen of te verwijderen

Voor meer gedetailleerde stappen over hoe u dit doet, kunt u het uitproberen in de zelf studie over Azure Digital Apparaatdubbels [*: verbinding maken met een end-to-end oplossing*](tutorial-end-to-end.md).

### <a name="permission-scopes"></a>Machtigingsbereiken

Voordat u een Azure-rol toewijst een beveiligingsprincipal, moet u het toegangsbereik bepalen dat de beveiligingsprincipal moet hebben. Aanbevolen procedures bepalen dat het het beste is om alleen het smalle mogelijke bereik toe te kennen.

In de volgende lijst worden de niveaus beschreven waarmee u toegang tot Azure Digital Apparaatdubbels-resources kunt bereiken.
* Modellen: met de acties voor deze resource bepaalt u de controle over [modellen](concepts-models.md) die zijn geüpload in azure Digital apparaatdubbels.
* Query Digital Apparaatdubbels Graph: de acties voor deze resource bepalen de mogelijkheid om [query bewerkingen](concepts-query-language.md) uit te voeren op digitale apparaatdubbels in de Azure Digital apparaatdubbels Graph.
* Digitale dubbele: de acties voor deze resource bieden controle over ruwe bewerkingen op [digitale apparaatdubbels](concepts-twins-graph.md) in de dubbele grafiek.
* Digitale dubbele relatie: de acties voor deze bron bepalen de controle over ruwe bewerkingen op [relaties](concepts-twins-graph.md) tussen digitale apparaatdubbels in het dubbele diagram.
* Gebeurtenis route: de acties voor deze bron bepalen de machtigingen voor het [routeren van gebeurtenissen](concepts-route-events.md) van Azure Digital apparaatdubbels naar een eindpunt service, zoals [Event hub](../event-hubs/event-hubs-about.md), [Event grid](../event-grid/overview.md)of [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

### <a name="troubleshooting-permissions"></a>Machtigingen voor probleem oplossing

Als een gebruiker probeert een actie uit te voeren die niet is toegestaan door hun rol, wordt mogelijk een fout bericht weer gegeven bij het lezen van de service aanvraag `403 (Forbidden)` . Zie [*probleem oplossing: Azure Digital apparaatdubbels-aanvraag is mislukt met status: 403 (verboden)*](troubleshoot-error-403.md)voor meer informatie en stappen voor probleem oplossing.

## <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest

Azure Digital Apparaatdubbels zorgt voor versleuteling van gegevens in rust en in transit als deze zijn geschreven in onze data centers en ontsleutelt deze voor u wanneer u er toegang toe hebt. Deze versleuteling vindt plaats met behulp van een door micro soft beheerde versleutelings sleutel.

## <a name="next-steps"></a>Volgende stappen

* Zie de volgende concepten in actie [*: een exemplaar en authenticatie instellen*](how-to-set-up-instance-portal.md).

* Bekijk hoe u met deze concepten communiceert vanuit de code van de client toepassing in [*How-to: app-verificatie code schrijven*](how-to-authenticate-client.md).

* Lees meer over [RBAC voor Azure](../role-based-access-control/overview.md).
