---
title: 'Zelfstudie: Een toepassing registreren'
titleSuffix: Azure AD B2C
description: Volg deze zelfstudie voor meer informatie over het registreren van een webtoepassing in Azure Active Directory B2C met behulp van Azure Portal.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 04/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0fd062bd0e58ecc714e4f450c93384e47e743b65
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922010"
---
# <a name="tutorial-register-a-web-application-in-azure-active-directory-b2c"></a>Zelfstudie: Een webtoepassing registreren in Azure Active Directory B2C

Voordat uw [toepassingen](application-types.md) kunnen communiceren met Azure Active Directory B2C (Azure AD B2C), moeten deze worden geregistreerd in een tenant die u beheert. Deze zelfstudie laat zien hoe u een webtoepassing registreert met behulp van de Azure-portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een web-app registreren
> * Een clientgeheim maken

Als u in plaats daarvan een systeemeigen app gebruikt (bijvoorbeeld iOS, Android, mobiel en bureaublad), leert u [hoe u een systeemeigen clienttoepassing kunt registreren](add-native-application.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u nog geen eigen [Azure AD B2C-tenant](tutorial-create-tenant.md) hebt gemaakt, maakt u er nu een. U kunt een bestaande Azure AD B2C-tenant gebruiken.

## <a name="register-a-web-application"></a>Een web-app registreren

Als u een toepassing wilt registreren in de Azure AD B2C-tenant, kunt u de nieuwe uniforme ervaring voor **App-registraties** of de verouderde ervaring **Toepassingen (verouderd)** gebruiken. [Meer informatie over de nieuwe ervaring](https://aka.ms/b2cappregtraining).

#### <a name="app-registrations"></a>[App-registraties](#tab/app-reg-ga/)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.
1. Zoek en selecteer **Azure AD B2C** in de Azure-portal.
1. Selecteer **App-registraties** en selecteer vervolgens **Nieuwe registratie**.
1. Voer een **naam** in voor de toepassing. Bijvoorbeeld *webapp1*.
1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst of een identiteitsprovider. Voor het verifiëren van gebruikers met Azure AD B2C**.
1. Selecteer onder **Omleidings-URI** de optie **Web** en voer vervolgens `https://jwt.ms` in het URL-tekstvak in.

    De omleidings-URI is het eindpunt waarnaar de gebruiker wordt gestuurd door de autorisatieserver (Azure AD B2C in dit geval) nadat de interactie met de gebruiker is voltooid en waaraan een toegangstoken of autorisatiecode wordt verzonden wanneer de autorisatie is geslaagd. In een productietoepassing is dit doorgaans een openbaar toegankelijk eindpunt waarop uw app wordt uitgevoerd, zoals `https://contoso.com/auth-response`. Voor testdoeleinden, zoals deze zelfstudie, kunt u dit instellen op `https://jwt.ms`, een webtoepassing van Microsoft die de gedecodeerde inhoud van een token weergeeft (de inhoud van het token verlaat nooit uw browser). Tijdens het ontwikkelen van apps kunt u het eindpunt daar toevoegen waar uw toepassing lokaal luistert, zoals `https://localhost:5000`. U kunt op elk gewenst moment omleidings-URI's toevoegen en wijzigen in uw geregistreerde toepassingen.

    De volgende beperkingen zijn van toepassing op omleidings-URI's:

    * De antwoord-URL moet beginnen met het schema `https`.
    * De antwoord-URL is hoofdlettergevoelig. Het hoofdlettergebruik moet overeenkomen met het URL-pad van de actieve toepassing. Als uw toepassing bijvoorbeeld `.../abc/response-oidc` als deel van het pad bevat, geeft u `.../ABC/response-oidc` niet op in de antwoord-URL. Omdat de webbrowser paden als hoofdlettergevoelig behandelt, kunnen cookies die zijn gekoppeld aan `.../abc/response-oidc` worden uitgesloten als ze worden omgeleid naar de qua hoofdlettergebruik niet-overeenkomende URL `.../ABC/response-oidc`.

1. Selecteer in **Machtigingen** het selectievakje *Beheerdersgoedkeuring verlenen aan machtigingen van OpenID en offline_access*.
1. Selecteer **Registreren**.

Zodra de registratie van de toepassing is voltooid, schakelt u de impliciete toekenningsstroom in:

1. Selecteer hiervoor **Verificatie** in het linkermenu onder **Beheren**.
1. Schakel onder **Impliciete toekenning** de selectievakjes **Toegangstokens** en **Id-tokens** in.
1. Selecteer **Opslaan**.

#### <a name="applications-legacy"></a>[Toepassingen (verouderd)](#tab/applications-legacy/)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.
1. Zoek en selecteer **Azure AD B2C** in de Azure-portal.
1. Selecteer **Toepassingen (verouderd)** , en selecteer vervolgens **Toevoegen**.
1. Voer een naam in voor de toepassing. Bijvoorbeeld *webapp1*.
1. Selecteer voor **Inclusief webtoepassing/ web-API** en **Impliciete stroom toestaan** **Ja**.
1. Voer voor de **Antwoord-URL** een eindpunt in waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. U kunt dit bijvoorbeeld instellen om lokaal te luisteren op `https://localhost:44316`. Als u het poortnummer nog niet weet, kunt u een waarde voor de tijdelijke aanduiding opgeven en deze later wijzigen.

    Voor testdoeleinden, zoals deze zelfstudie, kunt u deze instellen op `https://jwt.ms`, waarin de inhoud van een token voor inspectie wordt weergegeven. Voor deze zelfstudie stelt u de **Antwoord-URL** in op `https://jwt.ms`.

    De volgende beperkingen zijn van toepassing op antwoord-URL's:

    * De antwoord-URL moet beginnen met het schema `https`.
    * De antwoord-URL is hoofdlettergevoelig. Het hoofdlettergebruik moet overeenkomen met het URL-pad van de actieve toepassing. Als uw toepassing bijvoorbeeld `.../abc/response-oidc` als deel van het pad bevat, geeft u `.../ABC/response-oidc` niet op in de antwoord-URL. Omdat de webbrowser paden als hoofdlettergevoelig behandelt, kunnen cookies die zijn gekoppeld aan `.../abc/response-oidc` worden uitgesloten als ze worden omgeleid naar de qua hoofdlettergebruik niet-overeenkomende URL `.../ABC/response-oidc`.

1. Selecteer **Maken** om de registratie van de toepassing te voltooien.

* * *

## <a name="create-a-client-secret"></a>Een clientgeheim maken

Als uw toepassing een autorisatiecode voor een toegangstoken uitwisselt, moet u een toepassingsgeheim maken.


#### <a name="app-registrations"></a>[App-registraties](#tab/app-reg-ga/)

1. Selecteer op de pagina **Azure AD B2C - App-registraties** de toepassing die u hebt gemaakt, bijvoorbeeld *webapp1*.
1. Selecteer in het linkermenu onder **Beheren** de optie **Certificaten en geheimen**.
1. Selecteer **Nieuw clientgeheim**.
1. Voer een beschrijving voor het clientgeheim in het vak **Beschrijving** in. Bijvoorbeeld *clientsecret1*.
1. Selecteer onder **Verloopt** een duur waarvoor het geheim geldig is en selecteer vervolgens **Toevoegen**.
1. Noteer de **Waarde** van het geheim. U gebruikt deze waarde als het toepassingsgeheim in de code van uw toepassing.

#### <a name="applications-legacy"></a>[Toepassingen (verouderd)](#tab/applications-legacy/)

1. Selecteer op de pagina **Azure AD B2C - toepassingen** de toepassing die u hebt gemaakt, bijvoorbeeld *webapp1*.
1. Selecteer **Sleutels** en selecteer vervolgens **Sleutel genereren**.
1. Selecteer **Opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt deze waarde als het toepassingsgeheim in de code van uw toepassing.

* * *

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een web-app registreren
> * Een clientgeheim maken

U gaat nu leren hoe u gebruikersstromen kunt maken om gebruikers in staat te stellen zich te registreren, zich aan te melden en hun profielen te beheren.

> [!div class="nextstepaction"]
> [Gebruikersstromen maken in Azure Active Directory B2C >](tutorial-create-user-flows.md)
