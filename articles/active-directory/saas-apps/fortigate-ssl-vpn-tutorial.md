---
title: 'Zelfstudie: Integratie van eenmalige aanmelding van Azure AD met FortiGate SSL VPN'
description: In deze zelfstudie leert u hoe u eenmalige aanmelding configureert tussen Azure Active Directory en FortiGate SSL VPN.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: abe92218d6bb20274e916089c15df8c1f44c4fd6
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90986432"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fortigate-ssl-vpn"></a>Zelfstudie: Eenmalige aanmelding van Azure Active Directory integreren met FortiGate SSL VPN

In deze zelfstudie leert u hoe u FortiGate SSL VPN integreert met Azure Active Directory (Azure AD). Wanneer u FortiGate SSL VPN integreert met Azure AD, kunt u het volgende doen:

* Gebruik Azure AD om te bepalen wie toegang heeft tot FortiGate SSL VPN.
* Zorg ervoor dat gebruikers zich automatisch met hun Azure AD-account kunnen aanmelden bij FortiGate SSL VPN.
* Uw accounts op één centrale locatie beheren: de Azure-portal.

Zie [Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) voor meer informatie over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een abonnement op FortiGate SSL VPN waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="tutorial-description"></a>Beschrijving van zelfstudie

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

FortiGate SSL VPN biedt ondersteuning voor met SP geïnitieerde eenmalige aanmelding.

Nadat u FortiGate SSL VPN hebt geconfigureerd, kunt u sessiebeheer afdwingen, waardoor exfiltratie en infiltratie van gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessiebeheer is een uitbreiding van voorwaardelijke toegang. [Meer informatie over het afdwingen van sessiebeheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-fortigate-ssl-vpn-from-the-gallery"></a>FortiGate SSL VPN toevoegen vanuit de galerie

Voor het configureren van de integratie van FortiGate SSL VPN met Azure AD moet u FortiGate SSL VPN uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de knop **Azure Active Directory** in het linkerdeelvenster.
1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.
1. Selecteer **Nieuwe toepassing** om een toepassing toe te voegen.
1. Typ in het gedeelte **Toevoegen uit de galerie** **FortiGate SSL VPN** in het zoekvak.
1. Selecteer **FortiGate SSL VPN** in het resultatenvenster en voeg vervolgens de app toe. Wacht enkele seconden tot de app is toegevoegd aan de tenant.

## <a name="configure-and-test-azure-ad-sso-for-fortigate-ssl-vpn"></a>Eenmalige aanmelding van Azure AD voor FortiGate SSL VPN configureren en testen

U configureert en test eenmalige aanmelding van Azure AD met FortiGate SSL VPN met behulp van een testgebruiker met de naam B. Simon. Eenmalige aanmelding werkt alleen als u een koppelingsrelatie tot stand brengt tussen een Azure AD-gebruiker en de bijbehorende gebruiker in FortiGate SSL VPN.

Voltooi de volgende stappen van hoog niveau om eenmalige aanmelding van Azure AD met FortiGate SSL VPN te configureren en te testen:

1. **[Eenmalige aanmelding via Azure AD configureren](#configure-azure-ad-sso)** zodat uw gebruikers deze functie kunnen gebruiken.
    1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** om eenmalige aanmelding van Azure AD te testen.
    1. **[Toegang verlenen aan de testgebruiker](#grant-access-to-the-test-user)** zodat de gebruiker eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Eenmalige aanmelding voor FortiGate SSL VPN configureren](#configure-fortigate-ssl-vpn-sso)** aan de toepassingszijde.
    1. **Een testgebruiker maken voor FortiGate SSL VPN** als een tegenhanger van de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)** om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Eenmalige aanmelding van Azure AD configureren

Volg deze stappen om eenmalige aanmelding van Azure AD in te schakelen in de Azure-portal:

1. Selecteer in [Azure Portal](https://portal.azure.com/), op de integratiepagina van de toepassing **FortiGate SSL VPN**, het gedeelte **Beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer **SAML** op de pagina **Selecteer een methode voor eenmalige aanmelding**.
1. Selecteer op de pagina **Eenmalige aanmelding instellen met SAML** het potloodpictogram voor **Standaard-SAML-configuratie** om de instellingen te bewerken:

   ![Schermopname van het potloodpictogram voor het bewerken van de standaard-SAML-configuratie.](common/edit-urls.png)

1. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende waarden in:

    a. Voer in het vak **Aanmeldings-URL** een URL met het patroon `https://<FQDN>/remote/login` in.

    b. Voer in het vak **Id** een URL met het patroon `https://<FQDN>/remote/saml/metadata` in.

    c. Voer in het vak **Antwoord-URL** een URL met het patroon `https://<FQDN>/remote/saml/login` in.

    d. Voer in het vak **Afmeldings-URL** een URL met het patroon `https://<FQDN>/remote/saml/logout` in.

    > [!NOTE]
    > Deze waarden zijn slechts patronen. U moet de werkelijke **Aanmeldings-id**, **Id**, **Antwoord-URL** en **Afmeldings-URL** gebruiken. Neem contact op met het [ondersteuningsteam van FortiGate SSL VPN](mailto:tac_amer@fortinet.com) om de werkelijke waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. In FortiGate SSL VPN worden de SAML-asserties in een specifieke indeling verwacht. U dient aangepaste kenmerktoewijzingen toe te voegen aan de configuratie van uw SAML-tokenkenmerken. Deze schermopname toont de standaardkenmerken:

    ![Schermopname met de standaardkenmerken.](common/default-attributes.png)

1. Bovendien worden in FortiGate SSL VPN nog enkele kenmerken verwacht die als SAML-antwoord moeten worden doorgestuurd. Deze kenmerken worden weergegeven in de volgende tabel. Ze worden ook vooraf ingevuld, maar u kunt ze indien nodig herzien.
    
    | Naam |  Bronkenmerk|
    | ------------ | --------- |
    | gebruikersnaam | user.userprincipalname |
    | group | user.groups |

1. Ga op de pagina **Eenmalige aanmelding met SAML instellen** naar de sectie **SAML-handtekeningcertificaat** en selecteer de koppeling **Downloaden** naast **Certificaat (Base64)** om het certificaat te downloaden en op te slaan op de computer:

    ![Schermopname met de downloadkoppeling voor het certificaat.](common/certificatebase64.png)

1. In het gedeelte **FortiGate SSL VPN instellen** kopieert u de juiste URL('s) op basis van uw behoeften:

    ![Schermopname met de configuratie-URL's.](common/copy-configuration-urls.png)

#### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam B.Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory**. Selecteer **Gebruikers** en daarna **Alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Ga als volgt te werk in het venster dat**** verschijnt:
   1. Voer in het vak **Naam** de naam **B.Simon** in.  
   1. Voer in het vak **Gebruikersnaam** \<username>@\<companydomain> in.\<extension>. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Selecteer **Maken**.

#### <a name="grant-access-to-the-test-user"></a>De testgebruiker toegang geven

In deze sectie geeft u B.Simon toestemming om eenmalige aanmelding van Azure te gebruiken door die gebruiker toegang te verlenen tot FortiGate SSL VPN.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen** en selecteer **Alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **FortiGate SSL VPN**.
1. Selecteer op de overzichtspagina van de app in de sectie **Beheren** de optie **Gebruikers en groepen**:

   ![Schermopname van de optie Gebruikers en groepen.](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**:

    ![Schermopname van de knop Gebruiker toevoegen.](common/add-assign-user.png)

1. Selecteer in het dialoogvenster **Gebruikers en groepen** **B.Simon** in de lijst **Gebruikers** en selecteer vervolgens de knop **Selecteren** onderaan het scherm.
1. Als u een rolwaarde verwacht in de SAML-assertie, selecteert u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst. Klik onder aan het scherm op de knop **Selecteren**.
1. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

#### <a name="create-a-security-group-for-the-test-user"></a>Maak een beveiligingsgroep voor de testgebruiker

In dit gedeelte maakt u een beveiligingsgroep in Azure Active Directory voor de testgebruiker. Deze beveiligingsgroep wordt door FortiGate gebruikt om de gebruiker netwerktoegang via de VPN te verlenen.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory**. Selecteer vervolgens **Groepen**.
1. Selecteer bovenaan het scherm **Nieuwe groep**.
1. Voltooi in de eigenschappen van **Nieuwe groep** de volgende stappen:
   1. In de lijst **Groepstype** selecteert u **Beveiliging**.
   1. In het vak **Groepsnaam** voert u **FortiGateAccess** in.
   1. In het vak **Beschrijving van groep**voert u **Groep om FortiGate VPN toegang te verlenen**.
   1. Selecteer **Nee** voor de **Azure AD-rollen kunnen worden toegewezen aan de groep (Preview)** -instellingen.
   1. Selecteer in het vak **Lidmaatschapstype** de optie **Toegewezen**.
   1. Selecteer onder **Leden** de optie **Geen leden geselecteerd**.
   1. Selecteer in het dialoogvenster **Gebruikers en groepen** de optie **B. Simon** in de lijst **Gebruikers**. Klik vervolgens op de knop **Selecteren** onder aan het scherm.
   1. Selecteer **Maken**.
1. Nadat u weer in het gedeelte **Groepen** in Azure Active Directory bent, gaat u naar de groep **FortiGate Access** en noteert u de **object-id**. U hebt deze later nodig.

### <a name="configure-fortigate-ssl-vpn-sso"></a>Eenmalige aanmelding voor FortiGate SSL VPN configureren

#### <a name="upload-the-base64-saml-certificate-to-the-fortigate-appliance"></a>Upload het Base64 SAML-certificaat naar het FortiGate-apparaat

Nadat u de SAML-configuratie van de FortiGate-app in uw tenant hebt voltooid, hebt u het met Base64 gecodeerde SAML-certificaat gedownload. U dient dit certificaat naar het FortiGate-apparaat te uploaden:

1. Log in op het beheerportaal van uw FortiGate-apparaat.
1. In het linkerdeelvenster selecteert u **Systeem**.
1. Onder **System** selecteert u **Certificates**.
1. Selecteer **Import** > **Remote Certificate**.
1. Blader naar het certificaat dat u hebt gedownload uit de implementatie van de FortiGate-app in de Azure-tenant, selecteer het certificaat en selecteer vervolgens **OK**.

Nadat het certificaat is geüpload, noteert u de naam onder **System** > **Certificates** > **Remote Certificate**. Het krijgt standaard de naam REMOTE_Cert_ *N*, waarbij *N* een geheel getal is.

#### <a name="complete-fortigate-command-line-configuration"></a>FortiGate-opdrachtregelconfiguratie voltooien

Voor de volgende stappen dient u de afmeldings-URL van Azure te configureren. Deze URL bevat een vraagteken (?). U dient specifieke stappen uit te voeren om dit teken te kunnen verzenden. U kunt deze stappen niet uitvoeren vanuit de FortiGate CLI-console. Breng in plaats daarvan een SSH-sessie tot stand met het FortiGate-apparaat met behulp van een hulpprogramma als PuTTY. Als uw FortiGate-apparaat een virtuele machine van Azure is, kunt u de volgende stappen voltooien vanaf de seriële console voor de virtuele Azure-machine.

Als u deze stappen wilt uitvoeren, hebt u de eerder genoteerde waarden nodig:

- Entiteits-id
- Antwoord-URL
- Afmeldings-URL
- URL voor aanmelden bij Azure
- Azure AD-id
- URL voor afmelden bij Azure
- Base64 SAML-certificaatnaam (REMOTE_Cert_*N*)

1. Breng een SSH-sessie tot stand met het FortiGate-apparaat en meld u aan met een FortiGate-administratoraccount.
1. Voer deze opdrachten uit:

   ```console
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert <Base64 SAML Certificate Name>
    set user-name username
    set group-name group
    end

   ```

   > [!NOTE]
   > De Azure-afmeldings-URL bevat een `?`-teken. U dient een speciale toetsenreeks in te voeren om de URL correct op te geven voor de seriële FortiGate-console. De URL is doorgaans `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`.
   >
   > Voer `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation` in om de Azure-afmeldings-URL in de seriële console in te voeren
   > 
   > Selecteer vervolgens CTRL+V en plak de rest van de URL om de regel te voltooien: `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`.

#### <a name="configure-fortigate-for-group-matching"></a>FortiGate configureren om groepen te laten matchen

In deze sectie configureert u FortiGate zodanig dat de object-id van de beveiligingsgroep wordt herkend waarin de testgebruiker zich bevindt. Dankzij de configuratie kan FortiGate toegangsbeslissingen nemen op basis van het groepslidmaatschap.

Als u deze stappen wilt voltooien, hebt u de object-id nodig van de FortiGateAccess-beveiligingsgroep die u eerder in deze zelfstudie hebt gemaakt.

1. Breng een SSH-sessie tot stand met het FortiGate-apparaat en meld u aan met een FortiGate-administratoraccount.
1. Voer deze opdrachten uit:

   ```
    config user group
    edit FortiGateAccess
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <Object Id>
    next
    end
    next
    end
   ```

#### <a name="create-a-fortigate-vpn-portals-and-firewall-policy"></a>FortiGate VPN-portals en firewallbeleid maken

In deze sectie configureert u FortiGate VPN-portals en een firewallbeleid, zodat toegang wordt verleend tot de FortiGateAccess-beveiligingsgroep die u eerder in deze zelfstudie hebt gemaakt.

Werk samen met het  [FortiGate-ondersteuningsteam](mailto:tac_amer@fortinet.com) om de VPN-portals en het firewallbeleid toe te voegen aan het FortiGate VPN-platform. U moet deze stap voltooien voordat u eenmalige aanmelding gebruikt.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie test u de configuratie voor eenmalige aanmelding van Azure AD met behulp van het toegangsvenster.

Wanneer u het toegangsvenster op de tegel FortiGate SSL VPN selecteert, wordt u automatisch aangemeld bij de instantie van FortiGate SSL VPN waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Inleiding tot het toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) voor meer informatie over het toegangsvenster.

Voor de beste eindgebruikerservaring wordt u aangeraden de Fortinet VPN-client, FortiClient, te gebruiken.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [FortiGate SSL VPN proberen met Azure AD](https://aad.portal.azure.com/)

- [Wat is sessiebeheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
