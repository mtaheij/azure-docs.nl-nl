---
title: Azure Security Bench Mark v2-Identity Management
description: Azure Security Bench Mark v2 Identity Management
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: a007b8e85440faab6f3d9f35c49ee207ee1597ff
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91295495"
---
# <a name="security-control-v2-identity-management"></a>Beveiligings controle v2: identiteits beheer

Identiteits beheer is van toepassing op besturings elementen voor het instellen van een beveiligde identiteits-en toegangs beheer met Azure Active Directory. Dit geldt ook voor het gebruik van eenmalige aanmelding, sterke verificaties, beheerde identiteiten (en service principes) voor toepassingen, voorwaardelijke toegang en het controleren van afwijkingen van accounts.

## <a name="id-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>ID-1: Azure Active Directory standaardiseren als het centrale identiteits-en verificatie systeem

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-1 | 16,1, 16,2, 16,4, 16,5 | IA-2, IA-8, AC-2, AC-3 |

Azure Active Directory (Azure AD) is de standaard service voor identiteits-en toegangs beheer van Azure. U moet de standaardisatie van Azure AD gebruiken om het identiteits-en toegangs beheer van uw organisatie in te bepalen:
- Micro soft-cloud resources, zoals de Azure Portal, Azure Storage, Azure Virtual Machines (Linux en Windows), Azure Key Vault, PaaS en SaaS-toepassingen.

- De resources van uw organisatie, zoals toepassingen op Azure of uw bedrijfs netwerk resources.

Het beveiligen van Azure AD moet een hoge prioriteit hebben in de Cloud beveiligings procedure van uw organisatie. Azure AD biedt een beveiligde score voor identiteiten om u te helpen bij het beoordelen van uw identiteits beveiligings postuur ten opzichte van de best practice aanbevelingen van micro soft. Gebruik de score om te meten hoe nauw keurig uw configuratie overeenkomt met best practice aanbevelingen en om verbeteringen aan te brengen in uw beveiligings postuur.

Opmerking: Azure AD biedt ondersteuning voor externe ID-providers, waarmee gebruikers zonder Microsoft-account zich kunnen aanmelden bij hun toepassingen en resources met hun externe identiteit.

- [Pacht in azure AD](../../active-directory/develop/single-and-multi-tenant-apps.md)

- [Een Azure AD-exemplaar maken en configureren](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure AD-tenants definiëren](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Externe ID-providers voor een toepassing gebruiken](/azure/active-directory/b2b/identity-providers)

- [Wat is de beveiligde score voor identiteiten in azure AD](../../active-directory/fundamentals/identity-secure-score.md)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Identiteits-en sleutel beheer](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys) 

- [Beveiligingsarchitectuur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Postuurbeheer](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-2-manage-application-identities-securely-and-automatically"></a>ID-2: toepassings identiteiten veilig en automatisch beheren

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-2 | N.v.t. | AC-2, AC-3, IA-2, IA-4, IA-9 |

Voor niet-humane accounts, zoals services of automatisering, gebruikt u Azure Managed Identities in plaats van een krachtig menselijk account te maken voor toegang tot resources of code uit te voeren. Door Azure beheerde identiteiten kunnen worden geverifieerd bij Azure-Services en-resources die ondersteuning bieden voor Azure AD-verificatie. Verificatie wordt ingeschakeld via vooraf gedefinieerde regels voor toegangs verlening, waardoor het niet mogelijk is om in code vastgelegde referenties in de bron code-of configuratie bestanden te gebruiken. 

Voor services die geen beheerde identiteiten ondersteunen, gebruikt u Azure AD om in plaats daarvan een service-principal te maken met beperkte machtigingen op het resource niveau.  Het is raadzaam Service-principals met certificaat referenties te configureren en terug te vallen op client geheimen. In beide gevallen kunnen Azure Key Vault worden gebruikt in combi natie met door Azure beheerde identiteiten, zodat de runtime-omgeving (zoals een Azure function) de referentie kan ophalen uit de sleutel kluis.

- [Door Azure beheerde identiteiten](../../active-directory/managed-identities-azure-resources/overview.md)

- [Services die beheerde identiteiten voor Azure-resources ondersteunen](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Azure-service-principal](/powershell/azure/create-azure-service-principal-azureps)

- [Een service-principal maken met certificaten](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

Azure Key Vault gebruiken voor registratie van beveiligings-principals: verificatie # autoriseren-a-Security-Principal-to-Access-Key-kluis

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Identiteits-en sleutel beheer](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>ID-3: eenmalige aanmelding (SSO) van Azure AD gebruiken voor toegang tot toepassingen

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-3 | 4.4 | IA-2, IA-4 |

Azure AD biedt identiteits-en toegangs beheer aan Azure-resources, Cloud toepassingen en on-premises toepassingen. Identiteits-en toegangs beheer is van toepassing op ondernemings identiteiten, zoals werk nemers, evenals externe identiteiten, zoals partners, leveranciers en leveranciers.

Gebruik eenmalige aanmelding (SSO) van Azure AD voor het beheren en beveiligen van de toegang tot de gegevens en resources van uw organisatie op locatie en in de Cloud. Verbind al uw gebruikers, toepassingen en apparaten met Azure AD voor naadloze, veilige toegang en meer zicht baarheid en controle. 

- [Meer informatie over de SSO van de toepassing met Azure AD](../../active-directory/manage-apps/what-is-single-sign-on.md)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Beveiligingsarchitectuur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Identiteits-en sleutel beheer](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>ID-4: gebruik sterke verificatie controles voor alle op Azure Active Directory gebaseerde toegang

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-4 | 4,2, 4,4 4,5, 11,5, 12,11, 16,3 | AC-2, AC-3, IA-2, IA-4 |

Azure AD biedt ondersteuning voor sterke verificatie controles via multi-factor Authentication (MFA) en krachtige methoden met een eigen wacht woord.  
- Multi-factor Authentication: Schakel Azure AD MFA in en volg Azure Security Center aanbevelingen voor identiteits-en toegangs beheer voor uw MFA-installatie. MFA kan worden afgedwongen voor alle gebruikers, gebruikers selecteren of op het niveau per gebruiker op basis van aanmeldings voorwaarden en risico factoren. 

- Verificatie met wacht woord: er zijn drie verificatie opties met een wacht woord beschikbaar: Windows hello voor bedrijven, Microsoft Authenticator-app en on-premises verificatie methoden zoals Smart Cards. 

Voor beheerders en bevoegde gebruikers zorgt u ervoor dat het hoogste niveau van de methode voor sterke verificatie wordt gebruikt, gevolgd door het juiste beleid voor sterke authenticatie voor andere gebruikers te implementeren.

Als er nog steeds verouderde verificatie op basis van wacht woorden wordt gebruikt voor Azure AD-verificatie, moet u er rekening mee houden dat alleen Cloud accounts (gebruikers accounts die rechtstreeks in azure zijn gemaakt) een standaard wachtwoord beleid voor de basis lijn hebben. En hybride accounts (gebruikers accounts die afkomstig zijn van on-premises Active Directory) volgen het on-premises wachtwoord beleid. Wanneer u gebruikmaakt van verificatie op basis van wacht woorden, biedt Azure AD een functie voor wachtwoord beveiliging waarmee wordt voor komen dat gebruikers wacht woorden kunnen instellen die gemakkelijk te raden zijn. Micro soft biedt een algemene lijst met verboden wacht woorden die worden bijgewerkt op basis van de telemetrie, en klanten kunnen de lijst uitbreiden op basis van hun behoeften (bijvoorbeeld branding, culturele verwijzingen, enzovoort). Deze wachtwoord beveiliging kan worden gebruikt voor Cloud-en hybride accounts. 

Opmerking: verificatie op basis van wachtwoord referenties is alleen vatbaar voor populaire aanvals methoden. Voor een betere beveiliging gebruikt u sterke verificatie zoals MFA en een sterk wachtwoord beleid. Voor toepassingen van derden en Marketplace-services die standaard wachtwoorden kunnen hebben, moet u deze wijzigen tijdens de initiële service-instelling. 

- [MFA inschakelen in azure](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Inleiding tot verificatie opties met een wacht woord voor Azure Active Directory](../../active-directory/authentication/concept-authentication-passwordless.md)

- [Standaard wachtwoord beleid voor Azure AD](../../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Verwijder beschadigde wacht woorden met Azure AD-wachtwoord beveiliging](../../active-directory/authentication/concept-password-ban-bad.md)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Beveiligingsarchitectuur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Identiteits-en sleutel beheer](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-5-monitor-and-alert-on-account-anomalies"></a>ID-5: controleren en waarschuwen voor afwijkingen van het account

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-5 | 4,8, 4,9, 16,12, 16,13 | AC-2, AC-3, AC-7, AU-6 |

Azure AD biedt de volgende gegevens bronnen: 
-   Aanmeldingen: het rapport met aanmeldingen bevat informatie over het gebruik van beheerde toepassingen en aanmeldings activiteiten voor gebruikers.

-   Audit logboeken: voorziet in traceer baarheid via Logboeken voor alle wijzigingen die zijn aangebracht via verschillende functies in azure AD. Voor beelden van audit Logboeken in vastgelegde wijzigingen zijn het toevoegen of verwijderen van gebruikers, apps, groepen, rollen en beleids regels.

-   Riskante aanmeldingen - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is.

-   Gebruikers voor wie wordt aangegeven dat ze risico lopen - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast.

Deze gegevens bronnen kunnen worden geïntegreerd met Azure Monitor, Azure Sentinel of SIEM Systems van derden.

Azure Security Center kan ook worden gewaarschuwd voor bepaalde verdachte activiteiten, zoals een uitzonderlijk aantal mislukte verificatie pogingen en afgeschafte accounts in het abonnement. 

Azure Advanced Threat Protection (ATP) is een beveiligings oplossing die on-premises Active Directory signalen kan gebruiken om geavanceerde bedreigingen, gemanipuleerde identiteiten en schadelijke Insider-acties te identificeren, te detecteren en te onderzoeken.

- [Activiteiten rapporten controleren in azure AD](../../active-directory/reports-monitoring/concept-audit-logs.md)

- [Risk ante aanmeldingen voor Azure AD weer geven](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Azure AD-gebruikers identificeren die zijn gemarkeerd voor Risk ante activiteiten](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Identiteits-en toegangs activiteiten van gebruikers controleren in Azure Security Center](../../security-center/security-center-identity-access.md)

- [Waarschuwingen in de module Threat Intelligence-beveiliging van Azure Security Center](//azure/security-center/alerts-reference)

- [Azure-activiteiten logboeken integreren in Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Verbinding maken met gegevens van Azure AD Identity Protection](../../sentinel/connect-azure-ad-identity-protection.md)

- [Azure Advanced Threat Protection](/azure-advanced-threat-protection/what-is-atp)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Postuurbeheer](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-6-restrict-azure-resource-access-based-on-conditions"></a>ID-6: toegang tot Azure-bronnen beperken op basis van voor waarden

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-6 | N.v.t. | AC-2, AC-3 |

Gebruik voorwaardelijke toegang voor Azure AD voor meer gedetailleerd toegangs beheer op basis van door de gebruiker gedefinieerde voor waarden, zoals het vereisen van gebruikers aanmeldingen vanuit bepaalde IP-bereiken voor het gebruik van MFA. Een gedetailleerd beheer van verificatie sessies kan ook worden gebruikt via beleid voor voorwaardelijke toegang van Azure AD voor verschillende use cases. 

- [Overzicht van voorwaardelijke toegang voor Azure](../../active-directory/conditional-access/overview.md)

- [Algemeen beleid voor voorwaardelijke toegang](../../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Beheer van verificatiesessies met voorwaardelijke toegang configureren](../../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Identiteits-en sleutel beheer](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Postuurbeheer](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Informatie over bedreigingen](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="id-7-eliminate-unintended-credential-exposure"></a>ID-7: onbedoelde referentie blootstelling elimineren

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-7 | 18,1, 18,7 | IA-5 |

Implementeer de Azure DevOps-referentie scanner om referenties in de code te identificeren. Credential scanner stimuleert ook het verplaatsen van gedetecteerde referenties naar veiliger locaties, zoals Azure Key Vault.

Voor GitHub kunt u de systeem eigen functie voor het scannen van geheime gegevens gebruiken om referenties of een andere vorm van geheimen binnen de code te identificeren.

- [Referentie scanner instellen](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [GitHub Secret Scanning](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Postuurbeheer](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-8-secure-user-access-to-legacy-applications"></a>ID-8: beveiligde gebruikers toegang tot verouderde toepassingen

| Azure-ID | CIS-besturings elementen v 7.1-ID ('s) | NIST SP800-53 R4-ID ('s) |
|--|--|--|--|
| ID-8 | 14,6 | AC-2, AC-3, SC-11 |

Zorg ervoor dat u beschikt over moderne toegangs beheer en sessie bewaking voor oudere toepassingen en de gegevens die ze opslaan en verwerken. Hoewel Vpn's vaak worden gebruikt voor toegang tot oudere toepassingen, hebben ze vaak alleen elementaire toegangs beheer en beperkte sessie bewaking.

Met Azure AD-toepassingsproxy kunt u oude on-premises toepassingen publiceren naar externe gebruikers met eenmalige aanmelding (SSO), waarbij u de betrouw baarheid van zowel externe gebruikers als apparaten expliciet valideert met voorwaardelijke toegang van Azure AD. 

Microsoft Cloud App Security is ook een CASB-service (Cloud Access Security Broker) die besturings elementen kan bieden voor het bewaken van toepassings sessies van een gebruiker en het blok keren van acties (voor zowel oudere on-premises toepassingen als voor de cloud software als een service (SaaS)-toepassingen). 

- [Azure AD-toepassingsproxy](../../active-directory/manage-apps/application-proxy.md#what-is-application-proxy)

- [Aanbevolen procedures Microsoft Cloud App Security](/cloud-app-security/best-practices)

**Verantwoordelijkheid**: klant

**Beveiligings belanghebbenden van klanten** ([meer informatie](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Beveiligingsarchitectuur](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Infrastructuur- en eindpuntbeveiliging](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Toepassingsbeveiliging en DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

