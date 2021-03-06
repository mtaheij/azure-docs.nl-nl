---
title: Active Directory-verificatie-Azure Database for MySQL
description: Meer informatie over de concepten van Azure Active Directory voor verificatie met Azure Database for MySQL
author: lfittl-msft
ms.author: lufittl
ms.service: mysql
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 942789d5541e796247af2dbdf9371cce4701105e
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171902"
---
# <a name="use-azure-active-directory-for-authenticating-with-mysql"></a>Azure Active Directory gebruiken voor verificatie met MySQL

Microsoft Azure Active Directory-verificatie (Azure AD) is een mechanisme om verbinding te maken met Azure Database for MySQL met behulp van identiteiten gedefinieerd in azure AD.
Met Azure AD-verificatie kunt u gebruikers identiteiten van data bases en andere micro soft-Services beheren op een centrale locatie, waardoor het beheer van machtigingen wordt vereenvoudigd.

Voor delen van het gebruik van Azure AD zijn:

- Verificatie van gebruikers op een uniforme manier door Azure-Services
- Beheer van wachtwoord beleid en het draaien van wacht woorden op één plek
- Meerdere vormen van verificatie die door Azure Active Directory worden ondersteund, waarmee de nood zaak om wacht woorden op te slaan, kan worden voor komen
- Klanten kunnen database machtigingen beheren met externe groepen (Azure AD).
- Azure AD-verificatie maakt gebruik van MySQL-database gebruikers om identiteiten op database niveau te verifiëren
- Ondersteuning van verificatie op basis van tokens voor toepassingen die verbinding maken met Azure Database for MySQL

Gebruik het volgende proces om Azure Active Directory-verificatie te configureren en gebruiken:

1. Maak en vul zo nodig Azure Active Directory met gebruikers identiteiten in.
2. U kunt eventueel de Active Directory koppelen of wijzigen die momenteel is gekoppeld aan uw Azure-abonnement.
3. Maak een Azure AD-beheerder voor de Azure Database for MySQL-server.
4. Maak database gebruikers in uw data base die zijn toegewezen aan Azure AD-identiteiten.
5. Maak verbinding met uw data base door een token op te halen voor een Azure AD-identiteit en u aan te melden.

> [!NOTE]
> Zie [configureren en aanmelden met Azure AD voor Azure database for MySQL](howto-configure-sign-in-azure-ad-authentication.md)voor meer informatie over het maken en vullen van Azure AD en het configureren van Azure ad met Azure database for MySQL.

## <a name="architecture"></a>Architectuur

In het volgende diagram op hoog niveau wordt een overzicht gegeven van hoe verificatie werkt met Azure AD-verificatie met Azure Database for MySQL. De pijlen geven communicatiepaden aan.

![verificatie stroom][1]

## <a name="administrator-structure"></a>Beheerdersstructuur

Wanneer u Azure AD-verificatie gebruikt, zijn er twee beheerders accounts voor de MySQL-server. de oorspronkelijke MySQL-beheerder en de Azure AD-beheerder. Alleen de beheerder op basis van een Azure AD-account kan de eerste Azure AD-databasegebruiker in een gebruikersdatabase maken. De aanmelding van de Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Wanneer de beheerder een groeps account is, kan dit worden gebruikt door elk groepslid om meerdere Azure AD-beheerders voor de MySQL-server in te scha kelen. Het gebruik van een groeps account als beheerder verbetert de beheer baarheid, zodat u groeps leden in azure AD centraal kunt toevoegen en verwijderen zonder dat u de gebruikers of machtigingen op de MySQL-server hoeft te wijzigen. Er kan maar één Azure AD-beheerder (een gebruiker of groep) tegelijk worden geconfigureerd.

![beheerdersstructuur][2]

## <a name="permissions"></a>Machtigingen

Als u nieuwe gebruikers wilt maken die kunnen worden geverifieerd met Azure AD, moet u de Azure AD-beheerder zijn ontworpen. Deze gebruiker wordt toegewezen door de Azure AD-beheerders account voor een specifieke Azure Database for MySQL server te configureren.

Als u een nieuwe Azure AD-database gebruiker wilt maken, moet u verbinding maken als de Azure AD-beheerder. Dit wordt uitgelegd in [configureren en aanmelden met Azure AD voor Azure database for MySQL](howto-configure-sign-in-azure-ad-authentication.md).

Een Azure AD-verificatie is alleen mogelijk als de Azure AD-beheerder is gemaakt voor Azure Database for MySQL. Als de Azure Active Directory-beheerder van de server is verwijderd, kunnen bestaande Azure Active Directory gebruikers die eerder zijn gemaakt, geen verbinding meer maken met de data base met behulp van hun Azure Active Directory referenties.

## <a name="connecting-using-azure-ad-identities"></a>Verbinding maken met Azure AD-identiteiten

Azure Active Directory-verificatie ondersteunt de volgende methoden om verbinding te maken met een database met behulp van Azure AD-identiteiten:

- Azure Active Directory – wachtwoord
- Azure Active Directory – geïntegreerd
- Azure Active Directory – Universal met MFA
- Active Directory toepassings certificaten of client geheimen gebruiken
- [Beheerde identiteit](howto-connect-with-managed-identity.md)

Zodra u hebt geverifieerd op basis van de Active Directory, haalt u een token op. Dit token is uw wacht woord voor het aanmelden.

Houd er rekening mee dat beheer bewerkingen, zoals het toevoegen van nieuwe gebruikers, alleen worden ondersteund voor Azure AD-gebruikers rollen op dit moment.

> [!NOTE]
> Zie voor meer informatie over het maken van verbinding met een Active Directory-token [configureren en aanmelden met Azure AD voor Azure database for MySQL](howto-configure-sign-in-azure-ad-authentication.md).

## <a name="additional-considerations"></a>Aanvullende overwegingen

- Azure Active Directory-verificatie is alleen beschikbaar voor MySQL 5,7 en nieuwer.
- Er kan slechts één Azure AD-beheerder op elk gewenst moment worden geconfigureerd voor een Azure Database for MySQL-server.
- Alleen een Azure AD-beheerder voor MySQL kan eerst verbinding maken met de Azure Database for MySQL met behulp van een Azure Active Directory-account. De Active Directory-beheerder kan vervolgens nieuwe Azure AD-databasegebruikers configureren.
- Als een gebruiker uit Azure AD wordt verwijderd, kan die gebruiker niet langer worden geverifieerd bij Azure AD, waardoor het niet langer mogelijk is om een toegangs token voor die gebruiker te verkrijgen. In dit geval, hoewel de overeenkomende gebruiker zich nog steeds in de Data Base bevindt, is het niet mogelijk om verbinding te maken met de server met die gebruiker.
> [!NOTE]
> Aanmelding met de verwijderde Azure AD-gebruiker kan nog steeds worden uitgevoerd, totdat het token verloopt (Maxi maal 60 minuten van het uitgeven van tokens).  Als u de gebruiker ook verwijdert uit Azure Database for MySQL wordt deze toegang onmiddellijk ingetrokken.
- Als de Azure AD-beheerder wordt verwijderd van de server, wordt de server niet meer gekoppeld aan een Azure AD-Tenant en daarom worden alle Azure AD-aanmeldingen voor de server uitgeschakeld. Als u een nieuwe Azure AD-beheerder van dezelfde Tenant toevoegt, worden de Azure AD-aanmeldingen opnieuw ingeschakeld.
- Azure Database for MySQL overeenkomt met de toegangs Azure Database for MySQL tokens van de gebruiker met de unieke gebruikers-ID van de gebruiker, in plaats van de gebruikers naam te gebruiken. Dit betekent dat als een Azure AD-gebruiker wordt verwijderd in azure AD en een nieuwe gebruiker is gemaakt met dezelfde naam, Azure Database for MySQL van mening is dat een andere gebruiker. Daarom kan de nieuwe gebruiker geen verbinding maken met de bestaande gebruiker als een gebruiker uit Azure AD wordt verwijderd en vervolgens een nieuwe gebruiker met dezelfde naam wordt toegevoegd.

## <a name="next-steps"></a>Volgende stappen

- Zie [configureren en aanmelden met Azure AD voor Azure database for MySQL](howto-configure-sign-in-azure-ad-authentication.md)voor meer informatie over het maken en vullen van Azure AD en het configureren van Azure ad met Azure database for MySQL.
- Zie [gebruikers maken in azure database for MySQL](howto-create-users.md)voor een overzicht van aanmeldingen en database gebruikers voor Azure database for MySQL.

<!--Image references-->

[1]: ./media/concepts-azure-ad-authentication/authentication-flow.png
[2]: ./media/concepts-azure-ad-authentication/admin-structure.png
