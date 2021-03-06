---
title: Probleem bij het opslaan van beheerders referenties de Azure AD Gallery-App configureren
description: Informatie over het oplossen van problemen bij het opslaan van beheerders referenties tijdens het configureren van de gebruikers inrichting voor een Azure Active Directory galerie-toepassing.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 7a3340e72499087dce7773264272601dfce8a50f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91266779"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Probleem bij het opslaan van de beheerders referenties tijdens het configureren van de gebruikers inrichting voor een Azure Active Directory galerie-toepassing 

Wanneer u de Azure Portal gebruikt voor het configureren van [automatische gebruikers inrichting](user-provisioning.md) voor een bedrijfs toepassing, kan er een situatie optreden waarin:

* De **beheerders referenties** die voor de toepassing zijn opgegeven, zijn geldig en de knop **verbinding testen** werkt. De referenties kunnen echter niet worden opgeslagen en de Azure Portal retourneert een algemeen fout bericht.

Als op SAML gebaseerde eenmalige aanmelding ook voor dezelfde toepassing is geconfigureerd, is de oorzaak van de fout waarschijnlijk dat de interne opslag limiet van Azure AD, per toepassing, voor certificaten en referenties is overschreden.

Azure AD heeft momenteel een maximale opslag capaciteit van 1024 bytes voor alle certificaten, geheime tokens, referenties en gerelateerde configuratie gegevens die zijn gekoppeld aan één exemplaar van een toepassing (ook wel een Service-Principal record genoemd in azure AD).

Wanneer op SAML gebaseerde eenmalige aanmelding is geconfigureerd, wordt het certificaat dat wordt gebruikt voor het ondertekenen van de SAML-tokens hier opgeslagen en verbruikt vaak meer dan 50% van de ruimte.

Alle geheime tokens, Uri's, e-mail adressen, gebruikers namen en wacht woorden die tijdens de installatie van gebruikers inrichten worden ingevoerd, kunnen ertoe leiden dat de opslag limiet wordt overschreden.

## <a name="how-to-work-around-this-issue"></a>Dit probleem omzeilen 

Er zijn twee mogelijke manieren om dit probleem vandaag nog te omzeilen:

1. **Gebruik twee galerie-toepassings exemplaren, één voor eenmalige aanmelding en één voor het inrichten van** de galerie Application [LinkedIn](../saas-apps/linkedinelevate-tutorial.md) als voor beeld. u kunt LinkedIn-verhoging toevoegen via de galerie en deze configureren voor eenmalige aanmelding. Voor het inrichten moet u een andere instantie van LinkedIn-uitbrei ding toevoegen vanuit de Azure AD-App-galerie en de naam ' LinkedIn-verhogen (inrichting) ' noemen. Voor dit tweede exemplaar configureert u [inrichting](../saas-apps/linkedinelevate-provisioning-tutorial.md), maar niet eenmalige aanmelding. Wanneer u deze tijdelijke oplossing gebruikt, moeten dezelfde gebruikers en groepen worden [toegewezen](../manage-apps/assign-user-or-group-access-portal.md) aan beide toepassingen. 

2. **De hoeveelheid opgeslagen configuratie gegevens verlagen** : alle gegevens die zijn ingevoerd in de sectie [beheerders referenties](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) van het tabblad inrichten, worden opgeslagen op dezelfde locatie als het SAML-certificaat. Hoewel het mogelijk niet mogelijk is de lengte van al deze gegevens te beperken, kunnen sommige optionele configuratie velden, zoals de **e-mail melding** , worden verwijderd.

## <a name="next-steps"></a>Volgende stappen
[Gebruikers inrichten en het ongedaan maken van de inrichting van SaaS-toepassingen configureren](user-provisioning.md)
