---
title: 'Zelf studie: Medius flow configureren voor het automatisch inrichten van gebruikers met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het automatisch inrichten en ongedaan maken van de inrichting van gebruikers accounts van Azure AD naar Medius flow.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 04/30/2020
ms.author: Zhchia
ms.openlocfilehash: fae6c9f76f5c502d3a31c76330bd5b348d523e37
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91360579"
---
# <a name="tutorial-configure-mediusflow-for-automatic-user-provisioning"></a>Zelf studie: Medius flow configureren voor automatische gebruikers inrichting

In deze zelf studie worden de stappen beschreven die u moet uitvoeren in zowel Medius flow als Azure Active Directory (Azure AD) voor het configureren van automatische gebruikers inrichting. Wanneer de configuratie is geconfigureerd, worden gebruikers en groepen door Azure AD automatisch ingericht en [Gemedius flowd](https://www.mediusflow.com/) met behulp van de Azure AD-inrichtings service. Zie voor belangrijke details over wat deze service doet, hoe het werkt en veelgestelde vragen [Inrichting en ongedaan maken van inrichting van gebruikers automatiseren naar SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Ondersteunde mogelijkheden
> [!div class="checklist"]
> * Gebruikers maken in Medius flow
> * Gebruikers in Medius flow verwijderen wanneer ze niet meer toegang nodig hebben
> * Gebruikers kenmerken gesynchroniseerd laten tussen Azure AD en Medius flow
> * Inrichtings groepen en groepslid maatschappen in Medius flow
> * Eenmalige aanmelding bij Medius flow (aanbevolen)

## <a name="prerequisites"></a>Vereisten

In het scenario dat in deze zelfstudie wordt beschreven, wordt ervan uitgegaan dat u al beschikt over de volgende vereisten:

* [Een Azure AD-Tenant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Een gebruikers account in azure AD met [toestemming](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) voor het configureren van inrichting (bijvoorbeeld toepassings beheerder, Cloud toepassings beheerder, eigenaar van de toepassing of globale beheerder). 
* Een actief Medius flow-abonnement met een kwaliteits garantie of productie Tenant.
* Een gebruikers account in Medius flow met beheerders toegangs rechten om de configuratie in Medius flow te kunnen uitvoeren.
* De bedrijven die zijn toegevoegd aan de Medius flow-Tenant waarbij de gebruikers moeten worden ingericht.

## <a name="step-1-plan-your-provisioning-deployment"></a>Stap 1. Implementatie van de inrichting plannen
1. Lees [hoe de inrichtingsservice werkt](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bepaal wie u wilt opnemen in het [bereik voor inrichting](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Bepaal welke gegevens moeten worden [toegewezen tussen Azure AD en Medius flow](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-mediusflow-to-support-provisioning-with-azure-ad"></a>Stap 2. Medius flow configureren voor ondersteuning bij het inrichten met Azure AD

### <a name="activate-the-microsoft-365-app-within-mediusflow"></a>De Microsoft 365-app activeren binnen Medius flow
Begin met het inschakelen van de toegang tot de Azure AD-aanmelding en de Azure AD-configuratie functie in Medius Flow door de volgende stappen uit te voeren:

#### <a name="user-login"></a>Gebruikers aanmelding
Als u de aanmeldings stroom naar Microsoft 365/Azure AD wilt inschakelen, raadpleegt u [dit] ( https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#user-login-setup) artikel.

#### <a name="user-transfer-configuration"></a>Configuratie van gebruikers overdracht
Raadpleeg [Dit](
https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#user-sync-setup) artikel voor het inschakelen van de configuratie portal van de gebruikers die u wilt inrichten vanuit Azure AD.

#### <a name="configure-user-provisioning"></a>Gebruikersinrichting configureren

1.  Meld u aan bij de [Medius flow-beheer console](https://office365.cloudapp.mediusflow.com/) door de Tenant-id op te geven.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/1-auth.png)

2. Controleer de verbinding met Medius flow.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/2-verify-connection.png)

3. Geef de Azure AD-Tenant-ID op.

    ![Tenant-ID opgeven](./media/mediusflow-provisioning-tutorial/3-provide-azuread-tenantid.png)

   Meer informatie vindt u in de [Veelgestelde vragen](https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#how-do-i-get-the-azure-tenant-id) over hoe u deze kunt vinden.

4. Sla de configuratie op.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/4-save-config.png)

5. Selecteer gebruikers inrichten en klik op **OK**.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/5-select-user-provisioning.png)

6. Klik op **geheime sleutel genereren**. Kopieer deze waarde en sla deze op. Deze waarde wordt ingevoerd in het veld **geheime token** op het tabblad **inrichten** van uw Medius flow-toepassing in de Azure Portal.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/6-create-secret-1.png)

7. Klik op **OK**.

    ![Verifiëren](./media/mediusflow-provisioning-tutorial/7-confirm-secret.png)

8. Als u wilt dat de gebruikers worden geïmporteerd met een vooraf gedefinieerde set rollen, bedrijven en andere algemene configuraties in Medius flow, moet u deze eerst configureren. Begin door de configuratie toe te voegen door te klikken op **nieuwe configuratie toevoegen**.

    ![Gebruikers configuratie](./media/mediusflow-provisioning-tutorial/8-configure-user-configuration-1.png)

9. Geef de standaard instellingen voor de gebruikers op. In deze weer gave kunt u het standaard kenmerk instellen. Als de standaard instellingen in orde zijn, is het voldoende om alleen een geldige bedrijfs naam op te geven. Omdat deze configuratie-instellingen zijn opgehaald van Medius flow, moeten ze eerst worden geconfigureerd. Zie de sectie **vereisten** van dit artikel voor meer informatie.

    ![Gebruikers configuratie](./media/mediusflow-provisioning-tutorial/9-configure-user-config-detail-1.png)

10. Klik op **Opslaan** om de gebruikers configuratie op te slaan.

    ![Gebruikers configuratie](./media/mediusflow-provisioning-tutorial/10-done-1.png)

11. Als u de koppeling voor het inrichten van gebruikers wilt ophalen, klikt u op de **koppeling kopiëren van scim**. Kopieer deze waarde en sla deze op. Deze waarde wordt ingevoerd in het veld **Tenant-URL** op het tabblad **inrichten** van uw Medius flow-toepassing in de Azure Portal.
 
    ![Gebruikers configuratie](./media/mediusflow-provisioning-tutorial/11-get-scim-link.png)

## <a name="step-3-add-mediusflow-from-the-azure-ad-application-gallery"></a>Stap 3. Medius flow toevoegen vanuit de Azure AD-toepassings galerie

Voeg Medius flow toe vanuit de Azure AD-toepassings galerie om het beheren van de inrichting van Medius flow te starten. Als u eerder Medius flow voor SSO hebt ingesteld, kunt u dezelfde toepassing gebruiken. U wordt echter aangeraden een afzonderlijke app te maken wanneer u de integratie voor het eerst test. Klik [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) voor meer informatie over het toevoegen van een toepassing uit de galerie. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Stap 4. Definiëren wie u wilt opnemen in het bereik voor inrichting 

Met de Azure AD-inrichtingsservice kunt u bepalen wie worden ingericht op basis van toewijzing aan de toepassing en/of op basis van kenmerken van de gebruiker/groep. Als u ervoor kiest om te bepalen wie wordt ingericht voor uw app op basis van toewijzing, kunt u de volgende [stappen](../manage-apps/assign-user-or-group-access-portal.md) gebruiken om gebruikers en groepen aan de toepassing toe te wijzen. Als u ervoor kiest om uitsluitend te bepalen wie wordt ingericht op basis van kenmerken van de gebruiker of groep, kunt u een bereikfilter gebruiken zoals [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) wordt beschreven. 

* Wanneer u gebruikers en groepen toewijst aan Medius flow, moet u een andere rol dan **standaard toegang**selecteren. Gebruikers met de rol Standaardtoegang worden uitgesloten van inrichting en worden gemarkeerd als niet-effectief gerechtigd in de inrichtingslogboeken. Als Standaardtoegang de enige beschikbare rol voor de toepassing is, kunt u [het manifest van de toepassing bijwerken](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) om extra rollen toe te voegen. 

* Begin klein. Test de toepassing met een kleine set gebruikers en groepen voordat u de toepassing naar iedereen uitrolt. Wanneer het bereik voor inrichting is ingesteld op toegewezen gebruikers en groepen, kunt u dit beheren door een of twee gebruikers of groepen toe te wijzen aan de app. Wanneer het bereik is ingesteld op alle gebruikers en groepen, kunt u een [bereikfilter op basis van kenmerken](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) opgeven. 


## <a name="step-5-configure-automatic-user-provisioning-to-mediusflow"></a>Stap 5. Automatische gebruikers inrichting configureren voor Medius flow 

In deze sectie wordt u begeleid bij de stappen voor het configureren van de Azure AD-inrichtings service om gebruikers en/of groepen in TestApp te maken, bij te werken en uit te scha kelen op basis van gebruikers-en/of groeps toewijzingen in azure AD.

### <a name="to-configure-automatic-user-provisioning-for-mediusflow-in-azure-ad"></a>Automatische gebruikers inrichting configureren voor Medius flow in azure AD:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Selecteer **Bedrijfstoepassingen** en vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Medius flow**.

    ![De koppeling Medius flow in de lijst met toepassingen](common/all-applications.png)

3. Selecteer het tabblad **Inrichten**.

    ![Scherm opname van de opties voor beheer met de inrichtings optie.](common/provisioning.png)

4. Stel de **Inrichtingsmodus** in op **Automatisch**.

    ![Scherm afbeelding van de vervolg keuzelijst voor de inrichtings modus met de automatische optie aangeroepen.](common/provisioning-automatic.png)

5. Geef in het gedeelte **beheerders referenties** de waarde voor de TENANT-URL op die u eerder hebt opgehaald in de **Tenant-URL**. Voer de geheime token waarde in die eerder is opgehaald in het **geheime token**. Klik op **verbinding testen** om te controleren of Azure AD verbinding kan maken met Medius flow. Als de verbinding mislukt, zorg er dan voor dat uw Medius flow-account beheerders machtigingen heeft en probeer het opnieuw.

      ![Scherm afbeelding toont het dialoog venster beheerders referenties, waarin u uw Tenant U R L en geheime token kunt invoeren.](./media/mediusflow-provisioning-tutorial/provisioning.png)

6. Voer in het veld **E-mailadres voor meldingen** het e-mailadres in van een persoon of groep die de inrichtingsfoutmeldingen zou moeten ontvangen en schakel het selectievakje **Een e-mailmelding verzenden als een fout optreedt** in.

    ![E-mailadres voor meldingen](common/provisioning-notification-email.png)

7. Selecteer **Opslaan**.

8. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory gebruikers synchroniseren met Medius flow**.

9. Controleer de gebruikers kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Medius flow in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt om te voldoen aan de gebruikers accounts in Medius flow voor bijwerk bewerkingen. Als u ervoor kiest om het [overeenkomende doel kenmerk](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)te wijzigen, moet u ervoor zorgen dat de MEDIUS flow-API het filteren van gebruikers op basis van dat kenmerk ondersteunt. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

   |Kenmerk|Type|
   |---|---|
   |userName|Tekenreeks|
   |emails[type eq "work"].value|Tekenreeks|
   |naam. displayName|Tekenreeks|
   |actief|Booleaans|
   |name.givenName|Tekenreeks|
   |name.familyName|Tekenreeks|
   |naam. opgemaakt|Tekenreeks|
   |externalID|Tekenreeks|
   |urn: IETF: params: scim: schemas: extensie: Enter prise: 2.0: gebruiker: Manager|Naslaginformatie|


10. Selecteer in de sectie **toewijzingen** de optie **Azure Active Directory groepen synchroniseren met Medius flow**.

11. Controleer de groeps kenmerken die zijn gesynchroniseerd vanuit Azure AD naar Medius flow in de sectie **kenmerk toewijzing** . De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen, worden gebruikt om de groepen in Medius flow te vergelijken voor bijwerk bewerkingen. Selecteer de knop **Opslaan** om eventuele wijzigingen door te voeren.

      |Kenmerk|Type|
      |---|---|
      |displayName|Tekenreeks|
      |externalID|Tekenreeks|
      |leden|Naslaginformatie|

12. Als u bereikfilters wilt configureren, raadpleegt u de volgende instructies in de [zelfstudie Bereikfilter](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Als u de Azure AD-inrichtings service voor **Medius flow wilt inschakelen, wijzigt u de** **inrichtings status** in in het gedeelte **instellingen** .

    ![Inrichtingsstatus ingeschakeld](common/provisioning-toggle-on.png)

14. Definieer de gebruikers en/of groepen die u wilt inrichten voor Medius Flow door de gewenste waarden in het **bereik** te kiezen in de sectie **instellingen** .

    ![Inrichtingsbereik](common/provisioning-scope.png)

15. Wanneer u klaar bent om in te richten, klikt u op **Opslaan**.

    ![Inrichtingsconfiguratie opslaan](common/provisioning-configuration-save.png)

Met deze bewerking wordt de eerste synchronisatiecyclus gestart van alle gebruikers en groepen die zijn gedefinieerd onder **Bereik** in de sectie **Instellingen**. De initiële cyclus duurt langer dan volgende cycli, die ongeveer om de 40 minuten plaatsvinden zolang de Azure AD-inrichtingsservice wordt uitgevoerd. 

## <a name="step-6-monitor-your-deployment"></a>Stap 6. Uw implementatie bewaken
Nadat u het inrichten hebt geconfigureerd, gebruikt u de volgende resources om uw implementatie te bewaken:

1. Gebruik de [inrichtingslogboeken](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) om te bepalen welke gebruikers al dan niet met succes zijn ingericht
2. Controleer de [voortgangsbalk](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) om de status van de inrichtingscyclus weer te geven en te zien of deze al bijna is voltooid
3. Als het configureren van de inrichting een foutieve status lijkt te hebben, wordt de toepassing in quarantaine geplaatst. [Klik hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) voor meer informatie over quarantainestatussen.

## <a name="additional-resources"></a>Aanvullende resources

* [Gebruikersaccountinrichting voor zakelijke apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van logboeken en het ophalen van rapporten over de inrichtingsactiviteit](../manage-apps/check-status-user-account-provisioning.md)
