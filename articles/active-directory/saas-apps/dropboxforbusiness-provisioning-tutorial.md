---
title: 'Zelf studie: Dropbox voor bedrijven configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts in Dropbox voor bedrijven.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 05/20/2019
ms.author: jeedes
ms.openlocfilehash: f1ad698ccacc2fee94c797a20a43744d4cafba76
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91305637"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>Zelf studie: Dropbox voor bedrijven configureren voor automatische gebruikers inrichting

Het doel van deze zelf studie is te demonstreren welke stappen moeten worden uitgevoerd in Dropbox voor bedrijven en Azure Active Directory (Azure AD) om Azure AD te configureren voor het automatisch inrichten en ongedaan maken van de inrichting van gebruikers en/of groepen in Dropbox voor bedrijven.

> [!IMPORTANT]
> Micro soft en Dropbox veroudert de oude Dropbox-integratie met ingang van 04/01/2021. Om onderbrekingen van de service te voor komen, raden we u aan de migratie naar de nieuwe Dropbox-integratie te migreren, die groepen ondersteunt. Als u wilt migreren naar de nieuwe Dropbox-integratie, kunt u een nieuw exemplaar van Dropbox voor het inrichten in uw Azure AD-Tenant toevoegen en configureren met behulp van de onderstaande stappen. Wanneer u de nieuwe Dropbox-integratie hebt geconfigureerd, schakelt u inrichting in voor de oude Dropbox-integratie om het inrichten van conflicten te voor komen.

> [!NOTE]
> In deze zelfstudie wordt een connector beschreven die is gebaseerd op de Azure AD-service voor het inrichten van gebruikers. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* Een Azure AD-Tenant
* [Een Dropbox voor bedrijven-Tenant](https://www.dropbox.com/business/pricing)
* Een gebruikers account in Dropbox voor bedrijven met beheerders machtigingen.

## <a name="add-dropbox-for-business-from-the-gallery"></a>Dropbox voor bedrijven toevoegen vanuit de galerie

Voordat u Dropbox voor bedrijven configureert voor het automatisch inrichten van gebruikers met Azure AD, moet u Dropbox voor bedrijven vanuit de Azure AD-toepassings galerie toevoegen aan uw lijst met beheerde SaaS-toepassingen.

**Als u Dropbox voor bedrijven wilt toevoegen vanuit de Azure AD-toepassings galerie, voert u de volgende stappen uit:**

1. Selecteer in de **[Azure Portal](https://portal.azure.com)** in het navigatie venster links **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u de knop **nieuwe toepassing** boven aan het deel venster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Dropbox voor bedrijven**in, selecteer **Dropbox voor bedrijven** in het resultaten paneel en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

    ![Dropbox voor Bedrijven in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-dropbox-for-business"></a>Gebruikers toewijzen aan Dropbox voor bedrijven

Azure Active Directory gebruikt een concept met de naam *toewijzingen* om te bepalen welke gebruikers toegang moeten krijgen tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers worden alleen de gebruikers en/of groepen die zijn toegewezen aan een toepassing in azure AD gesynchroniseerd.

Voordat u automatische gebruikers inrichting configureert en inschakelt, moet u beslissen welke gebruikers en/of groepen in azure AD toegang nodig hebben tot Dropbox voor bedrijven. Nadat u hebt besloten, kunt u deze gebruikers en/of groepen toewijzen aan Dropbox voor bedrijven door de volgende instructies te volgen:

* [Een gebruiker of groep toewijzen aan een bedrijfs-app](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Belang rijke tips voor het toewijzen van gebruikers aan Dropbox voor bedrijven

* Het is raadzaam dat er één Azure AD-gebruiker wordt toegewezen aan Dropbox voor bedrijven om de configuratie van automatische gebruikers inrichting te testen. Extra gebruikers en/of groepen kunnen later worden toegewezen.

* Wanneer u een gebruiker toewijst aan Dropbox voor bedrijven, moet u een geldige toepassingsspecifieke rol (indien beschikbaar) selecteren in het dialoog venster toewijzing. Gebruikers met de rol **Standaard toegang** worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-dropbox-for-business"></a>Automatische gebruikers inrichting configureren voor Dropbox voor bedrijven 

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen in Dropbox voor bedrijven te maken, bij te werken en uit te scha kelen op basis van gebruikers-en/of groeps toewijzingen in azure AD.

> [!TIP]
> U kunt er ook voor kiezen om eenmalige aanmelding op basis van SAML voor Dropbox voor bedrijven in te scha kelen, gevolgd door de instructies in de [zelf studie Dropbox voor eenmalige aanmelding voor bedrijven](dropboxforbusiness-tutorial.md). Eenmalige aanmelding kan onafhankelijk van automatische gebruikers inrichting worden geconfigureerd, hoewel deze twee functies elkaar behoeven.

### <a name="to-configure-automatic-user-provisioning-for-dropbox-for-business-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor Dropbox voor bedrijven in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Dropbox voor bedrijven**.

    ![De koppeling Dropbox voor Bedrijven in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten**.

    ![Scherm opname van de opties voor beheer met de inrichtings optie.](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch**.

    ![Scherm afbeelding van de vervolg keuzelijst voor de inrichtings modus met de automatische optie aangeroepen.](common/provisioning-automatic.png)

5. Klik onder de sectie **Referenties voor beheerder** op **Autoriseren**. Er wordt een dialoog venster voor aanmelding bij Dropbox voor bedrijven geopend in een nieuw browser venster.

    ![Inrichten ](common/provisioning-oauth.png)

6. Meld u aan bij uw Dropbox voor Business Tenant en controleer uw identiteit op het dialoog venster **Aanmelden bij Dropbox voor bedrijven om te koppelen met Azure AD** .

    ![Dropbox voor bedrijven-aanmelding](media/dropboxforbusiness-provisioning-tutorial/dropbox01.png)

7. Klik na het volt ooien van stap 5 en 6 op **verbinding testen** om te controleren of Azure AD verbinding kan maken met Dropbox voor bedrijven. Als de verbinding mislukt, controleert u of uw Dropbox voor Business-account beheerders machtigingen heeft en probeer het opnieuw.

    ![Token](common/provisioning-testconnection-oauth.png)

8. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en vink het vakje **Een e-mailmelding verzenden als een fout optreedt** aan.

    ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

9. Klik op **Opslaan**.

10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met Dropbox**.

    ![Dropbox-gebruikers toewijzingen](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-mapping.png)

11. Controleer in de sectie **kenmerk toewijzing** de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Dropbox. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in Dropbox voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Gebruikers kenmerken van Dropbox](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-attributes.png)

12. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groepen synchroniseren met Dropbox**.

    ![Toewijzingen van Dropbox-groepen](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-mapping.png)

13. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Dropbox in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de groepen in Dropbox voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

    ![Kenmerken van Dropbox-groep](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-attributes.png)

14. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

15. Als u de Azure AD-inrichtings service voor Dropbox wilt inschakelen, **wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

16. Definieer de gebruikers en/of groepen die u wilt inrichten voor Dropbox door de gewenste waarden in het **bereik** te kiezen in de sectie **instellingen** .

    ![Inrichtingsbereik](common/provisioning-scope.png)

17. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatie gestart van alle gebruikers en/of groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen**. De initiële synchronisatie duurt langer dan volgende synchronisaties, die ongeveer om de 40 minuten plaatsvinden zolang de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de sectie **synchronisatie Details** gebruiken om de voortgang te bewaken en koppelingen naar het rapport inrichtings activiteiten te volgen, waarin alle acties worden beschreven die worden uitgevoerd door de Azure AD Provisioning-Service op Dropbox.

Zie [Rapportage over automatische inrichting van gebruikersaccounts](../app-provisioning/check-status-user-account-provisioning.md) voor informatie over het lezen van de Azure AD-inrichtingslogboeken.

## <a name="connector-limitations"></a>Connector beperkingen
 
* Dropbox biedt geen ondersteuning voor het onderbreken van uitgenodigde gebruikers. Als een uitgenodigde gebruiker wordt onderbroken, wordt die gebruiker verwijderd.

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../app-provisioning/check-status-user-account-provisioning.md)

