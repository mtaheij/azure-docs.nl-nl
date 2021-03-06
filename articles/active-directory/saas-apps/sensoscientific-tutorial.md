---
title: 'Zelfstudie: Azure Active Directory-integratie met SensoScientific Wireless Temperature Monitoring System | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en SensoScientific Wireless Temperature Monitoring System.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.openlocfilehash: 61807f90caad9e6be2a25e54dcaa837595afc729
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543156"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Zelfstudie: Azure Active Directory-integratie met SensoScientific Wireless Temperature Monitoring System

In deze zelfstudie ontdekt u hoe u SensoScientific Wireless Temperature Monitoring System integreert met Azure Active Directory (Azure AD).
De integratie van SensoScientific Wireless Temperature Monitoring System met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot SensoScientific Wireless Temperature Monitoring System.
* U kunt ervoor zorgen dat uw gebruikers automatisch met hun Azure AD-account worden aangemeld bij SensoScientific Wireless Temperature Monitoring System (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om Azure AD-integratie met SensoScientific Wireless Temperature Monitoring System te configureren, hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) krijgen
* Een abonnement op SensoScientific Wireless Temperature Monitoring System waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* SensoScientific Wireless Temperature Monitoring System ondersteunt door **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>SensoScientific Wireless Temperature Monitoring System toevoegen uit de galerie

Om de integratie van SensoScientific Wireless Temperature Monitoring System in Azure AD te configureren, moet u SensoScientific Wireless Temperature Monitoring System uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om SensoScientific Wireless Temperature Monitoring System toe te voegen uit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SensoScientific Wireless Temperature Monitoring System**, selecteer **SensoScientific Wireless Temperature Monitoring System** in het resultatenvenster en klik op de knop **Toevoegen** om de toepassing toe te voegen.

    ![SensoScientific Wireless Temperature Monitoring System in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert en test u eenmalige aanmelding van Azure AD met SensoScientific Wireless Temperature Monitoring System op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in SensoScientific Wireless Temperature Monitoring System tot stand is gebracht.

Voltooi de volgende stappen om eenmalige aanmelding van Azure AD met SensoScientific Wireless Temperature Monitoring System te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor SensoScientific Wireless Temperature Monitoring System configureren](#configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on)** : als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor SensoScientific Wireless Temperature Monitoring System maken](#create-sensoscientific-wireless-temperature-monitoring-system-test-user)** : als u een tegenhanger van Britta Simon in SensoScientific Wireless Temperature Monitoring System wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD met SensoScientific Wireless Temperature Monitoring System te configureren:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de integratiepagina van de toepassing **SensoScientific Wireless Temperature Monitoring System** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **SAML-basisconfiguratie** hoeft de gebruiker geen enkele stap uit te voeren omdat de app al vooraf is geïntegreerd met Azure.

    ![Domein- en URL-gegevens voor eenmalige aanmelding bij SensoScientific Wireless Temperature Monitoring System](common/preintegrated.png)

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In de sectie **SensoScientific Wireless Temperature Monitoring System instellen** kopieert u de juiste URL(‘s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on"></a>Eenmalige aanmelding voor SensoScientific Wireless Temperature Monitoring System configureren

1. Meld u als beheerder aan bij uw SensoScientific Wireless Temperature Monitoring System-toepassing.

1. Klik in het navigatiemenu bovenaan op **Configuratie** en ga naar **Configureren** onder **Eenmalige aanmelding** om de instellingen voor eenmalige aanmelding te openen. Voer dan de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png)

    a. Selecteer **Naam van uitgever** als Azure AD.

    b. Plak in het tekstvak **URL van uitgever** de **Azure AD-id** die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het tekstvak **URL van service voor eenmalige aanmelding** de **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Plak in het tekstvak **URL van service voor eenmalige afmelding** de **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    e. Blader naar het certificaat dat u hebt gedownload uit de Azure-portal, en upload het hier.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie geeft u Britta Simon toestemming om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot SensoScientific Wireless Temperature Monitoring System.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **SensoScientific Wireless Temperature Monitoring System**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **SensoScientific Wireless Temperature Monitoring System** in de lijst met toepassingen.

    ![De koppeling naar SensoScientific Wireless Temperature Monitoring System in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Testgebruiker voor SensoScientific Wireless Temperature Monitoring System maken

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij SensoScientific Wireless Temperature Monitoring System, moeten ze worden ingericht in SensoScientific Wireless Temperature Monitoring System. Werk samen met het  [ondersteuningsteam van SensoScientific Wireless Temperature Monitoring System](https://www.sensoscientific.com/contact-us/)  om de gebruikers toe te voegen voor het SensoScientific Wireless Temperature Monitoring System-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel SensoScientific Wireless Temperature Monitoring System klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van SensoScientific Wireless Temperature Monitoring System waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

