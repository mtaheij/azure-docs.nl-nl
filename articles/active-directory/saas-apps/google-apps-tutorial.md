---
title: 'Zelfstudie: Integratie van eenmalige aanmelding van Azure Active Directory met Google Cloud (G Suite) Connector | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Google Cloud (G Suite) Connector.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/06/2020
ms.author: jeedes
ms.openlocfilehash: 7e6b4524523d0659126bcd6cbe1294d700e79ed9
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707812"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-google-cloud-g-suite-connector"></a>Zelfstudie: Integratie van eenmalige aanmelding van Azure Active Directory met Google Cloud (G Suite) Connector

In deze zelfstudie leert u hoe u Google Cloud (G Suite) Connector kunt integreren met Azure Active Directory (Azure AD). Wanneer u Google Cloud (G Suite) Connector integreert met Azure AD, kunt u het volgende doen:

* In Azure AD beheren wie toegang heeft tot Google Cloud (G Suite) Connector.
* Ervoor zorgen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Google Cloud (G Suite) Connector.
* Uw accounts op een centrale locatie beheren: Azure Portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

- Een Azure AD-abonnement
- Een abonnement op Google Cloud (G Suite) Connector waarvoor eenmalige aanmelding is ingeschakeld.
- Een Google Apps-abonnement of Google Cloud Platform-abonnement.

> [!NOTE]
> Als u de stappen in deze zelfstudie wilt testen, is het raadzaam om niet de productieomgeving te gebruiken. Dit document is gemaakt met behulp van de nieuwe ervaring voor eenmalige aanmelding. Als u nog steeds gebruikmaakt van de oude versie, ziet de installatie er anders uit. U kunt de nieuwe ervaring inschakelen in de instellingen voor eenmalige aanmelding van de G Suite-toepassing. Ga naar **Azure AD, bedrijfstoepassingen**, selecteer **Google Cloud (G Suite) Connector**, selecteer **Eenmalige aanmelding** en klik vervolgens op **Nieuwe ervaring uitproberen**.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **V: Biedt deze integratie ondersteuning voor Google Cloud Platform-integratie voor eenmalige aanmelding met Azure AD?**

    A: Ja. Google Cloud Platform en Google Apps delen hetzelfde verificatieplatform. Voor de GCP-integratie moet u dus de eenmalige aanmelding met Google Apps configureren.

2. **V: Zijn Chromebooks en andere Chrome-apparaten compatibel met eenmalige aanmelding in Azure AD?**
  
    A: Ja, gebruikers kunnen zich met behulp van hun Azure AD-referenties aanmelden bij hun Chromebook-apparaten. Bekijk dit [ondersteuningsartikel over Google Cloud (G Suite) Connector](https://support.google.com/chrome/a/answer/6060880) voor informatie over waarom gebruikers mogelijk tweemaal om hun referenties wordt gevraagd.

3. **V: Kunnen gebruikers, als ik eenmalige aanmelding inschakel, hun Azure AD-referenties gebruiken om zich aan te melden bij Google-producten, zoals Google Classroom, GMail, Google Drive, YouTube, enzovoort?**

    A: Ja, afhankelijk van [de Google Cloud (G Suite) Connector](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) die u inschakelt of uitschakelt voor uw organisatie.

4. **V: Kan ik eenmalige aanmelding voor alleen een subset van gebruikers van Google Cloud (G Suite) Connector inschakelen?**

    A: Nee, als u eenmalige aanmelding inschakelt, moeten onmiddellijk alle gebruikers van Google Cloud (G Suite) Connector zich verifiëren met hun Azure AD-referenties. Omdat Google Cloud (G Suite) Connector geen ondersteuning biedt voor meerdere id-providers, kan de id-provider voor uw Google Cloud (G Suite) Connector-omgeving Azure AD of Google zijn, maar niet beide tegelijkertijd.

5. **V: Wordt een gebruiker die is aangemeld via Windows, automatisch gevraagd om zich te verifiëren bij Google Cloud (G Suite) Connector, zonder dat om een wachtwoord wordt gevraagd?**

    A: Er zijn twee opties waarbij dit scenario zich voordoet. Ten eerste kan een gebruiker zich via [Deelnemen aan Azure Active Directory](../device-management-introduction.md) aanmelden bij een Windows 10-apparaat. Gebruikers kunnen zich ook aanmelden bij Windows-apparaten die zijn toegevoegd aan een domein in on-premises Active Directory waarvoor eenmalige aanmelding bij Azure AD is ingeschakeld via een [AD FS-implementatie (Active Directory Federation Services)](../hybrid/plan-connect-user-signin.md). Voor beide opties moet u de stappen in de volgende zelfstudie uitvoeren om eenmalige aanmelding tussen Azure AD en Google Cloud (G Suite) Connector in te schakelen.

6. **V: Wat moet ik doen als ik een foutbericht over ongeldige e-mail krijg?**

    A: Voor deze installatie is het e-mailkenmerk vereist voor de gebruikers om zich te kunnen aanmelden. Dit kenmerk kan niet handmatig worden ingesteld.

    Het e-mailkenmerk wordt automatisch ingevuld voor elke gebruiker met een geldige Exchange-licentie. Als e-mail niet is ingeschakeld voor een gebruiker, wordt dit foutbericht ontvangen omdat de toepassing dit kenmerk moet ophalen om toegang te kunnen verlenen.

    U kunt met een beheerdersaccount naar portal.office.com gaan, in het beheercentrum klikken op Facturering > Abonnementen, uw Microsoft 365-abonnement selecteren en vervolgens klikken op Toewijzen aan gebruikers. Selecteer de gebruikers van wie u het abonnement wilt controleren, en klik in het rechterdeelvenster op Licenties bewerken.

    Nadat de Microsoft 365-licentie is toegewezen, kan het enkele minuten duren voordat deze is toegepast. Hierna wordt het kenmerk user.mail automatisch ingevuld en is het probleem zeer waarschijnlijk opgelost.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* De Google Cloud (G Suite) Connector biedt ondersteuning voor door **SP** geïnitieerde eenmalige aanmelding

* Google Cloud (G Suite) Connector biedt ondersteuning voor [**Geautomatiseerde** gebruikersinrichting](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)
* Zodra u Google Cloud (G Suite) Connector hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer is een uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-google-cloud-g-suite-connector-from-the-gallery"></a>Google Cloud (G Suite) Connector toevoegen vanuit de galerie

Om de integratie van Google Cloud (G Suite) Connector in Azure AD te configureren, moet u Google Cloud (G Suite) Connector vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het linkernavigatiedeelvenster de service **Azure Active Directory**.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een nieuwe toepassing toe te voegen.
1. Typ **Google Cloud (G Suite) Connector** in het zoekvak in de sectie **Toevoegen uit de galerie**.
1. Selecteer **Google Cloud (G Suite) Connector** in het resultatenvenster en voeg de app vervolgens toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-single-sign-on-for-google-cloud-g-suite-connector"></a>Eenmalige aanmelding van Azure AD configureren en testen voor Google Cloud (G Suite) Connector

Configureer en test eenmalige aanmelding van Azure AD met Google Cloud (G Suite) Connector met behulp van een testgebruiker met de naam **B.Simon**. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Google Cloud (G Suite) Connector.

Voltooi de volgende stappen om eenmalige aanmelding van Azure AD met Google Cloud (G Suite) Connector te configureren en te testen:

1. **[Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-sso)** : zodat uw gebruikers deze functie kunnen gebruiken.
    1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : om eenmalige aanmelding van Azure AD te testen met B.Simon.
    1. **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** zodat B.Simon eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Eenmalige aanmelding van Google Cloud (G Suite) Connector configureren](#configure-google-cloud-g-suite-connector-sso)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
    1. **[Testgebruiker maken voor Google Cloud (G Suite) Connector](#create-google-cloud-g-suite-connector-test-user)** : als u een tegenhanger van Britta Simon in Google Cloud (G Suite) Connector wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in Azure Portal.

1. Zoek in de [Azure-portal](https://portal.azure.com/) op de integratiepagina van de toepassing **Google Cloud (G Suite) Connector** de sectie **Beheren** en selecteer **Eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Op de pagina **Eenmalige aanmelding instellen met SAML** klikt u op het bewerkings-/penpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u wilt configureren voor **Gmail**:

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon:

    ```http
    google.com/a/<yourdomain.com>
    google.com
    https://google.com
    https://google.com/a/<yourdomain.com>
    ```

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: 

    ```http
    https://www.google.com
    https://www.google.com/a/<yourdomain.com>
    ```

1. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u wilt configureren voor **Google Cloud Platform**:

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon:
    
    ```http
    google.com/a/<yourdomain.com>
    google.com
    https://google.com
    https://google.com/a/<yourdomain.com>
    ```
    
    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: 
    
    ```http
    https://www.google.com
    https://www.google.com/a/<yourdomain.com>
    ```

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Google Cloud (G Suite) Connector biedt geen waarde voor Entiteits-id/id-waarde voor de configuratie van eenmalige aanmelding. Wanneer u dus de optie voor **domeinspecifieke uitgever** uitschakelt, wordt de id-waarde `google.com`. Als u de optie voor de **domeinspecifieke uitgever** inschakelt, wordt deze `google.com/a/<yourdomainname.com>`. Als u de optie voor de **domeinspecifieke uitgever** wilt in-/uitschakelen, gaat u naar het gedeelte **Eenmalige aanmelding bij Google Cloud (G Suite) Connector configureren**, dat verderop in de zelfstudie wordt besproken. Neem contact op met het [ondersteuningsteam van Google Cloud (G Suite) Connector](https://www.google.com/contact/) voor meer informatie.

1. De toepassing Google Cloud (G Suite) Connector verwacht de SAML-asserties in een specifieke indeling. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken. In de volgende schermopname ziet u een voorbeeld hiervan. De standaardwaarde van **Unieke gebruikers-id** is **user.userprincipalname**, maar in Google Cloud (G Suite) Connector wordt verwacht dat aan deze waarde het e-mailadres van de gebruiker is toegewezen. Hiervoor kunt u het kenmerk **user.mail** in de lijst gebruiken of de juiste kenmerkwaarde op basis van uw organisatieconfiguratie.

    ![image](common/default-attributes.png)


1. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** gaat u naar **Certificaat (Base64)** en selecteert u **Downloaden** om het certificaat te downloaden en op te slaan op uw computer.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. In de sectie **Google Cloud (G Suite) Connector instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in Azure Portal.

1. Selecteer in het linkerdeelvenster van Azure Portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Volg de volgende stappen bij de eigenschappen voor **Gebruiker**:
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer username@companydomain.extension in het veld **Gebruikersnaam** in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u B.Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot Google Cloud (G Suite) Connector.

1. Selecteer in Azure Portal de optie **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Google Cloud (G Suite) Connector** in de lijst met toepassingen.
1. Zoek op de overzichtspagina van de app de sectie **Beheren** en selecteer **Gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![De koppeling Gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B.Simon** in de lijst Gebruikers. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Als u een waarde voor een rol verwacht in de SAML-assertie, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren. Klik vervolgens op de knop **Selecteren** onderaan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-google-cloud-g-suite-connector-sso"></a>Eenmalige aanmelding bij Google Cloud (G Suite) Connector configureren

1. Open een nieuw tabblad in uw browser en meld u met uw beheerdersaccount aan bij de [beheerconsole van Google Cloud (G Suite) Connector](https://admin.google.com/).

2. Klik op **Beveiliging**. Als u de koppeling niet ziet, is deze mogelijk verborgen in het menu **Meer besturingselementen** onder aan het scherm.

    ![Klik op Beveiliging.][10]

3. Klik op de pagina **Beveiliging** op **Eenmalige aanmelding (SSO) instellen.**

    ![Klik op Eenmalige aanmelding.][11]

4. Voer de volgende configuratiewijzigingen uit:

    ![Eenmalige aanmelding configureren][12]

    a. Selecteer **Installatie van eenmalige aanmelding met id-provider van derden**.

    b. Plak in het veld **URL van aanmeldingspagina** in Google Cloud (G Suite) Connector de waarde van **Aanmeldings-URL** die u hebt gekopieerd in de Azure-portal.

    c. Plak in het veld **URL van afmeldingspagina** in Google Cloud (G Suite) Connector de waarde van **Aanmeldings-URL** die u hebt gekopieerd in de Azure-portal.

    > [!NOTE]
    > Google Cloud (G suite) is gebaseerd op het SAML-afmeldingsprotocol. Daarom moet in het veld **URL van afmeldingspagina** de SAML-afmeldings-URL (dat is de afmeldings-URL) worden gebruikt als dezelfde waarde.

    d. Upload in Google Cloud (G Suite) Connector voor **Verificatiecertificaat** het certificaat dat u hebt gedownload in de Azure-portal.   

    e. Schakel de optie **Een domeinspecifieke uitgever gebruiken** optie in/uit volgens de opmerking in de bovenstaande sectie **Standaard SAML-configuratie** in Azure AD.

    f. Plak in het veld **URL voor wachtwoord wijzigen** in Google Cloud (G Suite) Connector de waarde van **URL voor wachtwoord wijzigen** die u hebt gekopieerd in de Azure-portal.

    g. Klik op **Opslaan**.

### <a name="create-google-cloud-g-suite-connector-test-user"></a>Testgebruiker voor Google Cloud (G Suite) Connector maken

Het doel van deze sectie is het [maken van een gebruiker in Google Cloud (G Suite) Connector](https://support.google.com/a/answer/33310?hl=en) met de naam B.Simon. Nadat de gebruiker handmatig is gemaakt in Google Cloud (G Suite) Connector, kan de gebruiker zich nu aanmelden met haar aanmeldingsreferenties voor Microsoft 365.

Google Cloud (G Suite) Connector biedt ook ondersteuning voor automatische gebruikersinrichting. Als u het automatisch inrichten van gebruikers wilt configureren, moet u de [Google Cloud (G Suite) Connector eerst configureren voor automatische gebruikersinrichting](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial).

> [!NOTE]
> Zorg ervoor dat uw gebruiker al bestaat in Google Cloud (G Suite) Connector, als inrichten in Azure AD niet is ingeschakeld vóór het testen van eenmalige aanmelding.

> [!NOTE]
> Neem contact op met het [ondersteuningsteam van Google](https://www.google.com/contact/) als u handmatig een gebruiker moet maken.

## <a name="test-sso"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel van Google Cloud (G Suite) Connector klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van Google Cloud (G Suite) Connector waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

- [Google Cloud (G Suite) Connector gebruiken met Azure AD](https://aad.portal.azure.com/)

- [Wat is sessiebeheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Google Cloud (G Suite) Connector beveiligen met geavanceerde zichtbaarheid en besturingselementen](https://docs.microsoft.com/cloud-app-security/protect-gsuite)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png