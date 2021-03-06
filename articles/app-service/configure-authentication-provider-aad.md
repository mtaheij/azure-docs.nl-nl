---
title: Azure AD-verificatie configureren
description: Meer informatie over het configureren van Azure Active Directory-verificatie als een id-provider voor uw App Service of Azure Functions-app.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 04/14/2020
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: 4022ca97f88e2f06d3b4c1eeb103bb60d1856eda
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962142"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-azure-ad-login"></a>Uw App Service of Azure Functions app configureren voor het gebruik van Azure AD-aanmelding

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit artikel wordt beschreven hoe u Azure App Service of Azure Functions kunt configureren om Azure Active Directory (Azure AD) als verificatie provider te gebruiken.

> [!NOTE]
> Met de Express-instellingen stroom wordt een AAD v1-toepassing geregistreerd. Als u [Azure Active Directory v 2.0](../active-directory/develop/v2-overview.md) (inclusief [MSAL](../active-directory/develop/msal-overview.md)) wilt gebruiken, volgt u de [Geavanceerde configuratie-instructies](#advanced).

Volg deze aanbevolen procedures bij het instellen van uw app en verificatie:

- Geef elke App Service-app eigen machtigingen en toestemming.
- Configureer elke App Service-app met een eigen registratie.
- Vermijd het delen van machtigingen tussen omgevingen door afzonderlijke app-registraties voor afzonderlijke implementatie sleuven te gebruiken. Bij het testen van nieuwe code kunt u deze procedure gebruiken om te voor komen dat problemen van invloed zijn op de productie-app.

> [!NOTE]
> Deze functie is momenteel niet beschikbaar in het verbruiks abonnement voor Linux voor Azure Functions

## <a name="configure-with-express-settings"></a><a name="express"> </a>Configureren met Express-instellingen

> [!NOTE]
> De **Express** -optie is niet beschikbaar voor overheids Clouds.

1. In de [Azure Portal]zoekt en selecteert u **app Services**en selecteert u vervolgens uw app.
2. Selecteer in de linkernavigatiebalk **verificatie/autorisatie**  >  **in**.
3. Selecteer **Azure Active Directory**  >  **Express**.

   Als u in plaats daarvan een bestaande app-registratie wilt kiezen:

   1. Kies **bestaande AD-app selecteren**en klik vervolgens op **Azure AD-App**.
   2. Kies een bestaande app-registratie en klik op **OK**.

3. Selecteer **OK** om de app app service te registreren in azure Active Directory. Er wordt een nieuwe app-registratie gemaakt.

    ![Snelle instellingen in Azure Active Directory](./media/configure-authentication-provider-aad/express-settings.png)

4. Beschrijving App Service biedt standaard verificatie, maar beperkt geen geautoriseerde toegang tot uw site-inhoud en Api's. U moet gebruikers in uw app-code autoriseren. Als u app-toegang alleen wilt beperken voor gebruikers die zijn geverifieerd door Azure Active Directory, stelt **u een actie in die moet worden uitgevoerd wanneer de aanvraag niet is geverifieerd** om u aan te **melden met Azure Active Directory**. Wanneer u deze functionaliteit instelt, vereist uw app dat alle aanvragen worden geverifieerd. Ook worden alle niet-geverifieerde omgeleid naar Azure Active Directory voor authenticatie.

    > [!CAUTION]
    > Het beperken van de toegang op deze manier is van toepassing op alle aanroepen naar uw app. Dit is mogelijk niet wenselijk voor apps met een openbaar beschik bare start pagina, zoals in veel toepassingen met één pagina. Voor dergelijke toepassingen kunt u **anonieme aanvragen (geen actie)** de voor keur geven, waarbij de app de aanmelding zelf hand matig start. Zie voor meer informatie [verificatie stroom](overview-authentication-authorization.md#authentication-flow).
5. Selecteer **Opslaan**.

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Configureren met geavanceerde instellingen

U kunt app-instellingen hand matig configureren als u een app-registratie wilt gebruiken vanuit een andere Azure AD-Tenant. Deze aangepaste configuratie volt ooien:

1. Een registratie maken in azure AD.
2. Geef een aantal van de registratie gegevens op App Service.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>Een app-registratie maken in azure AD voor uw app service-app

U hebt de volgende informatie nodig wanneer u uw App Service-app configureert:

- Client-id
- Tenant-id
- Client geheim (optioneel)
- URI voor de toepassings-ID

Voer de volgende stappen uit:

1. Meld u aan bij de [Azure Portal], zoek en selecteer **app Services**en selecteer vervolgens uw app. Noteer de **URL**van uw app. U gebruikt deze om de registratie van uw Azure Active Directory-app te configureren.
1. Selecteer **Azure Active Directory**  >  **app-registraties**  >  **nieuwe registratie**.
1. Voer op de pagina **een toepassing registreren** een **naam** in voor de registratie van uw app.
1. In **omleidings-URI**selecteert u **Web** en type `<app-url>/.auth/login/aad/callback` . Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. Selecteer **Maken**.
1. Nadat de app-registratie is gemaakt, kopieert u de **toepassings-id** en de **Directory (Tenant)-ID** voor later.
1. Selecteer **Verificatie**. Schakel onder **impliciete toekenning** **id-tokens** in om OpenID Connect te verbinden met gebruikers aanmeldingen van app service.
1. Beschrijving Selecteer **huis stijl**. Voer de URL van uw App Service-app in op de URL van de **Start pagina**en selecteer **Opslaan**.
1. Selecteer **een API**-  >  **set**beschikbaar maken. Plak de URL van uw App Service-app in een app met één Tenant en selecteer **Opslaan** en voor de app voor meerdere tenants, plak in de URL die is gebaseerd op een van de geverifieerde Tenant domeinen en selecteer vervolgens **Opslaan**.

   > [!NOTE]
   > Deze waarde is de **URI van de toepassings-id** van de app-registratie. Als uw web-app toegang tot een API in de Cloud vereist, hebt u de URI van de **toepassings-id** van de web-app nodig wanneer u de Cloud app Service Resource configureert. U kunt dit bijvoorbeeld gebruiken als u wilt dat de Cloud service expliciet toegang tot de Web-App verleent.

1. Selecteer **Een bereik toevoegen**.
   1. Voer *user_impersonation*in **Scope naam**in.
   1. Voer in de tekst vakken de naam en beschrijving van het toestemming bereik in die gebruikers op de pagina toestemming moeten zien. Voer bijvoorbeeld *toegang tot mijn app*in.
   1. Selecteer **bereik toevoegen**.
1. Beschrijving Als u een client geheim wilt maken, selecteert u **certificaten & geheimen**  >  **Nieuw client geheim**  >  **toevoegen**. Kopieer de waarde van het client geheim dat op de pagina wordt weer gegeven. Het wordt niet meer weer gegeven.
1. Beschrijving Selecteer **verificatie**om meerdere **antwoord-url's**toe te voegen.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>Azure Active Directory in uw app service-app inschakelen

1. In de [Azure Portal]zoekt en selecteert u **app Services**en selecteert u vervolgens uw app.
1. Selecteer in het linkerdeel venster onder **instellingen**de optie **verificatie/autorisatie**  >  **On**.
1. Beschrijving Standaard staat App Service verificatie niet-geverifieerde toegang tot uw app toe. Als u gebruikers verificatie wilt afdwingen, stelt **u de actie in die moet worden uitgevoerd wanneer de aanvraag niet is geverifieerd** om u aan te **melden met Azure Active Directory**.
1. Selecteer onder **Azure Active Directory** de optie **Verificatieproviders**.
1. Selecteer in de **beheer modus** **geavanceerd** en configureer app service verificatie volgens de volgende tabel:

    |Veld|Beschrijving|
    |-|-|
    |Client-id| Gebruik de **toepassings-id (client)** van de app-registratie. |
    |URL van de uitgever| Gebruik `<authentication-endpoint>/<tenant-id>/v2.0` en vervang door *\<authentication-endpoint>* het [verificatie-eind punt voor uw cloud omgeving](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) (bijvoorbeeld ' https://login.microsoft.com "voor wereld wijd Azure), vervang ook door *\<tenant-id>* de **Directory-ID (Tenant)** waarin de app-registratie is gemaakt. Deze waarde wordt gebruikt om gebruikers om te leiden naar de juiste Azure AD-Tenant, en om de juiste meta gegevens te downloaden om de juiste sleutels voor token-ondertekening en claim waarde voor token uitgever te bepalen. Voor toepassingen die gebruikmaken van Azure AD v1 en voor Azure Functions-apps, laat u `/v2.0` de URL weg.|
    |Client geheim (optioneel)| Gebruik het client geheim dat u hebt gegenereerd in de app-registratie.|
    |Toegestane token doel groepen| Als dit een Cloud-of server-app is en u verificatie tokens van een web-app wilt toestaan, voegt u hier de URI voor de **toepassings-id** van de web-app toe. De geconfigureerde **client-id** wordt *altijd* impliciet beschouwd als een toegestane doel groep. |

2. Selecteer **OK**en selecteer vervolgens **Opslaan**.

U bent nu klaar om Azure Active Directory te gebruiken voor verificatie in uw App Service-app.

## <a name="configure-a-native-client-application"></a>Een systeem eigen client toepassing configureren

U kunt systeem eigen clients registreren om verificatie toe te staan voor de host van de Web-API in uw app met behulp van een client bibliotheek zoals de **Active Directory Authentication Library**.

1. Selecteer in de [Azure Portal] **Active Directory**  >  **app-registraties**  >  **nieuwe registratie**.
1. Voer op de pagina **een toepassing registreren** een **naam** in voor de registratie van uw app.
1. Selecteer in de **omleidings-URI** **open bare client (mobiele & bureau blad)** en typ de URL `<app-url>/.auth/login/aad/callback` . Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Gebruik in plaats daarvan de SID van het [pakket](/previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library#package-sid) als URI voor een Microsoft Store toepassing.
1. Selecteer **Maken**.
1. Nadat de app-registratie is gemaakt, kopieert u de waarde van de **toepassings-id (client)**.
1. Selecteer **API-machtigingen**  >  **een machtiging**  >  **mijn api's**toevoegen.
1. Selecteer de app-registratie die u eerder hebt gemaakt voor uw App Service-app. Als u de app-registratie niet ziet, zorg er dan voor dat u het **user_impersonation** bereik hebt toegevoegd in [een app-registratie in azure AD maken voor uw app service-app](#register).
1. Selecteer onder **gedelegeerde machtigingen**de optie **user_impersonation**en selecteer vervolgens **machtigingen toevoegen**.

U hebt nu een systeem eigen client toepassing geconfigureerd die toegang heeft tot uw App Service-app namens een gebruiker.

## <a name="configure-a-daemon-client-application-for-service-to-service-calls"></a>Een daemon-client toepassing configureren voor service-naar-service-aanroepen

Uw toepassing kan een token verkrijgen voor het aanroepen van een web-API die wordt gehost in uw App Service of functie-app namens zichzelf (niet namens een gebruiker). Dit scenario is nuttig voor niet-interactieve daemon-toepassingen die taken uitvoeren zonder een aangemelde gebruiker. De standaard OAuth 2,0- [client referenties](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md) worden verleend.

1. Selecteer in de [Azure Portal] **Active Directory**  >  **app-registraties**  >  **nieuwe registratie**.
1. Voer op de pagina **een toepassing registreren** een **naam** in voor de registratie van uw daemon-app.
1. Voor een daemon-toepassing hebt u geen omleidings-URI nodig, zodat u die leeg kunt laten.
1. Selecteer **Maken**.
1. Nadat de app-registratie is gemaakt, kopieert u de waarde van de **toepassings-id (client)**.
1. Selecteer **certificaten & geheimen**  >  **Nieuw client geheim**  >  **toevoegen**. Kopieer de waarde van het client geheim dat op de pagina wordt weer gegeven. Het wordt niet meer weer gegeven.

U kunt nu [een toegangs token aanvragen met behulp van de client-id en het client geheim](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) door de `resource` para meter in te stellen op de URI van de **toepassings-id** van de doel-app. Het resulterende toegangs token kan vervolgens worden weer gegeven aan de doel-app met behulp van de standaard [OAuth 2,0-autorisatie-header](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#use-the-access-token-to-access-the-secured-resource), en app service verificatie/autorisatie het token te valideren en gebruiken, zoals gebruikelijk, wordt aangegeven dat de aanroeper (een toepassing in dit geval, niet een gebruiker) wordt geverifieerd.

Op dit moment kan _elke_ client toepassing in uw Azure AD-Tenant een toegangs token aanvragen en zich verifiëren bij de doel-app. Als u ook _autorisatie_ wilt afdwingen om alleen bepaalde client toepassingen toe te staan, moet u een extra configuratie uitvoeren.

1. [Definieer een app-rol](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) in het manifest van de app-registratie die de app service of functie-app vertegenwoordigt die u wilt beveiligen.
1. Selecteer **API-machtigingen**voor het  >  **toevoegen van een machtiging**voor  >  **mijn api's**in de app-registratie die de client vertegenwoordigt die moet worden geautoriseerd.
1. Selecteer de app-registratie die u eerder hebt gemaakt. Als u de app-registratie niet ziet, zorg er dan voor dat u [een app-functie hebt toegevoegd](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md).
1. Selecteer onder **toepassings machtigingen**de app-functie die u eerder hebt gemaakt en selecteer vervolgens **machtigingen toevoegen**.
1. Zorg ervoor dat u op toestemming van de **beheerder geven** klikt om de client toepassing te machtigen om de machtiging aan te vragen.
1. Net als bij het vorige scenario (voordat de functies zijn toegevoegd), kunt u nu [een toegangs token aanvragen](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) voor hetzelfde doel `resource` . het toegangs token bevat een `roles` claim met de app-rollen die voor de client toepassing zijn geautoriseerd.
1. In de doel-App Service of functie-app code kunt u nu valideren of de verwachte rollen aanwezig zijn in het token (dit wordt niet uitgevoerd door App Service verificatie/autorisatie). Zie voor meer informatie [gebruikers claims voor toegang](app-service-authentication-how-to.md#access-user-claims).

U hebt nu een daemon-client toepassing geconfigureerd die toegang heeft tot uw App Service-app met behulp van een eigen identiteit.

## <a name="next-steps"></a><a name="related-content"> </a>Volgende stappen

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]
* [Zelfstudie: Zelfstudie: Gebruikers eind-tot-eind verifiëren en autoriseren in Azure App Service](tutorial-auth-aad.md)
<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/