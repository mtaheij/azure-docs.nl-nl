---
title: 'Zelfstudie: Integratie van eenmalige aanmelding (SSO) van Azure Active Directory met Salesforce | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Salesforce.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/17/2020
ms.author: jeedes
ms.openlocfilehash: 7228f4fbf348b8112654ece91aa5e9e831ac1201
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543562"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce"></a>Zelfstudie: Integratie van eenmalige aanmelding (SSO) van Azure Active Directory met Salesforce

In deze zelfstudie leert u hoe u Salesforce integreert met Azure Active Directory (Azure AD). Wanneer u Salesforce integreert met Azure AD, kunt u:

* in Azure AD bepalen wie er toegang heeft tot Salesforce.
* ervoor zorgen dat gebruikers zich automatisch met hun Azure AD-account kunnen aanmelden bij Salesforce.
* uw accounts op één centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op Salesforce waarvoor eenmalige aanmelding (SSO) is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Salesforce biedt ondersteuning voor met **SP** geïnitieerde eenmalige aanmelding

* Salesforce biedt ondersteuning voor het [**automatisch** inrichten van gebruikers](salesforce-provisioning-tutorial.md) (aanbevolen)

* Salesforce biedt ondersteuning voor het **Just-In-Time** inrichten van gebruikers

* De mobiele Salesforce-toepassing kan nu worden geconfigureerd met Azure AD voor het inschakelen van eenmalige aanmelding. In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.
* Zodra u Salesforce hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer, uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce toevoegen vanuit de galerie

Voor het configureren van de integratie van Salesforce in Azure AD moet u Salesforce vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **Toevoegen vanuit de galerie**, **Salesforce** in het zoekvak.
1. Selecteer **Salesforce** in het resultatenvenster en voeg de app toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce"></a>Eenmalige aanmelding van Azure AD configureren en testen voor Salesforce

Configureer en test eenmalige aanmelding van Azure AD met Salesforce met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Salesforce.

Voltooi de volgende bouwstenen om eenmalige aanmelding van Azure AD met Salesforce te configureren en te testen:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    * **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : om eenmalige aanmelding van Azure AD te testen met B. Simon.
    * **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** : zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Salesforce-eenmalige aanmelding configureren](#configure-salesforce-sso)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
    * **[Een Salesforce-testgebruiker maken](#create-salesforce-test-user)** : als u een equivalent van B.Simon in Salesforce wilt hebben dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in de Azure-portal.

1. Zoek in de [Azure-portal](https://portal.azure.com/) op de integratiepagina van de toepassing **Salesforce** de sectie **Beheren** en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **Standaard-SAML-configuratie** voert u de waarden in voor de volgende velden:

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met de volgende indeling:

    Bedrijfsaccount: `https://<subdomain>.my.salesforce.com`

    Ontwikkelaarsaccount: `https://<subdomain>-dev-ed.my.salesforce.com`
    
    b. Typ in het tekstvak **Antwoord-URL** een URL met de volgende indeling:

    Bedrijfsaccount: `https://<subdomain>.my.salesforce.com`

    Ontwikkelaarsaccount: `https://<subdomain>-dev-ed.my.salesforce.com`

    c. Typ in het tekstvak **Id** een waarde met de volgende indeling:

    Bedrijfsaccount: `https://<subdomain>.my.salesforce.com`

    Ontwikkelaarsaccount: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en -id. Neem contact op met het [Salesforce-klantondersteuningsteam](https://help.salesforce.com/support) om deze waarden te verkrijgen.

1. Ga op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** naar **XML-bestand met federatieve metagegevens** en selecteer **Downloaden** om het certificaat te downloaden. Sla dit vervolgens op de computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

1. In de sectie **Salesforce instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in de Azure-portal.

1. Selecteer in het linkerdeelvenster van de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Volg de volgende stappen bij de eigenschappen voor **Gebruiker**:
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer username@companydomain.extension in het veld **Gebruikersnaam** in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u B.Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot Salesforce.

1. Selecteer in de Azure-portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Salesforce** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-salesforce-sso"></a>Eenmalige aanmelding configureren voor Salesforce

1. Als u de configuratie in Salesforce wilt automatiseren, moet u **My Apps-browserextensie voor veilig aanmelden** installeren door op **De extensie installeren** te klikken.

    ![Uitbreiding van Mijn apps](common/install-myappssecure-extension.png)

1. Na de toevoeging van de extensie aan de browser, klikt u op **Salesforce instellen** om naar de toepassing voor eenmalige aanmelding in Salesforce te worden geleid. Geef hier de Administrator-referenties op om u aan te melden bij Eenmalige aanmelding in Salesforce. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 3-13 geautomatiseerd.

    ![Instelling configureren](common/setup-sso.png)

1. Als u Salesforce handmatig wilt instellen, opent u een nieuw browservenster en meldt u zich als beheerder aan bij de Salesforce-bedrijfssite. Voer daarna de volgende stappen uit:

1. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/configure1.png)

1. Schuif in het navigatiedeelvenster omlaag naar **INSTELLINGEN** en klik op **Identiteit** om het bijbehorende gedeelte uit te vouwen. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso.png)

1. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Als u eenmalige aanmelding niet kunt inschakelen voor uw Salesforce-account, moet u mogelijk contact opnemen met het [Salesforce-klantondersteuningsteam](https://help.salesforce.com/support).

1. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-enable-saml.png)

1. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-new.png)

1. Klik op **Bestand kiezen** om het XML-bestand met metagegevens te uploaden dat u hebt gedownload uit Azure Portal. Klik dan op **Maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/xmlchoose.png)

1. Op de pagina **SAML-instellingen voor eenmalige aanmelding** worden de velden automatisch ingevuld. Selecteer de **Gebruikers inrichten ingeschakeld** en klik dan op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/salesforcexml.png)

1. Klik in het navigatiedeelvenster links in Salesforce op **Bedrijfsinstellingen** om het bijbehorende gedeelte uit te vouwen. Klik dan op **Mijn domein**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-my-domain.png)

1. Schuif omlaag naar het gedeelte **Verificatieconfiguratie** en klik op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-edit-auth-config.png)

1. In het gedeelte **Verificatieconfiguratie** schakelt u **AzureSSO** in als **verificatieservice** voor de SAML-configuratie voor eenmalige aanmelding. Klik dan op **Opslaan** .

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Als er meer dan één verificatieservice wordt geselecteerd, wordt gebruikers gevraagd met welke verificatieservice ze zich graag willen aanmelden om gebruik te maken van eenmalige aanmelding in uw Salesforce-omgeving. Als u niet wilt dat dit gebeurt, moet u **andere verificatieservices uitgeschakeld laten**.

### <a name="create-salesforce-test-user"></a>Een Salesforce-testgebruiker maken

In deze sectie wordt een gebruiker met de naam B.Simon gemaakt in Salesforce. Salesforce biedt ondersteuning voor just-in-time inrichten, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet bestaat in Salesforce, wordt er een nieuwe gemaakt wanneer u Salesforce opent. Salesforce ondersteunt ook automatische inrichting van gebruikers. Meer details over het configureren van automatische inrichting van gebruikers vindt u [hier](salesforce-provisioning-tutorial.md).

## <a name="test-sso"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de Salesforce-tegel in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van Salesforce waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="test-sso-for-salesforce-mobile"></a>Test voor eenmalige aanmelding in Salesforce (mobiel)

1. Open de mobiele Salesforce-toepassing. Klik op de aanmeldpagina op **Aangepast domein gebruiken**.

    ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app1.png)

1. Voer in het tekstvak **Aangepast domein** uw geregistreerde aangepaste domeinnaam in en klik op **Doorgaan**.

    ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app2.png)

1. Voer uw Azure AD-referenties in om u aan te melden bij de Salesforce-toepassing en klik op **Volgende**.

    ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app3.png)

1. Klik op de pagina **Toegang toestaan** op **Toestaan** om toegang te verlenen tot de Salesforce-toepassing.

    ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app4.png)

1. Als het aanmelden is gelukt, wordt de startpagina van de toepassing weergegeven.

    ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app5.png) ![Mobiele Salesforce-toepassing](media/salesforce-tutorial/mobile-app6.png)

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](salesforce-provisioning-tutorial.md)

- [Salesforce proberen met Azure AD](https://aad.portal.azure.com)

- [Wat is sessiebeheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Salesforce beveiligen met geavanceerde zichtbaarheid en besturingselementen](https://docs.microsoft.com/cloud-app-security/protect-salesforce)
