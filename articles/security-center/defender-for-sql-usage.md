---
title: Azure Defender voor SQL gebruiken
description: Meer informatie over het gebruik van het optionele Azure Defender voor SQL-abonnement van Azure Security Center
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 6d19027cfd406f81d31696c5cd2c1a235574e400
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/30/2020
ms.locfileid: "91577830"
---
# <a name="azure-defender-for-sql-servers-on-machines"></a>Azure Defender voor SQL-servers op computers 

Dit Azure Defender-abonnement detecteert afwijkende activiteiten die een ongebruikelijke en potentieel schadelijke pogingen om toegang te krijgen tot data bases of deze te gebruiken.

U ziet waarschuwingen wanneer er verdachte database activiteiten, potentiële kwetsbaar heden of SQL-injectie aanvallen en afwijkende database toegang en query patronen zijn.

## <a name="availability"></a>Beschikbaarheid

|Aspect|Details|
|----|:----|
|Release status:|Preview|
|Koers|**Azure Defender voor SQL-servers op computers** wordt gefactureerd zoals wordt weer gegeven op [de pagina met prijzen](security-center-pricing.md)|
|Beveiligde SQL-versies:|Azure SQL Server (alle versies die worden gedekt door micro soft support)|
|Clouds|![Ja](./media/icons/yes-icon.png) Commerciële Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) China gov, andere gov|
|||

## <a name="set-up-azure-defender-for-sql-servers-on-machines"></a>Azure Defender voor SQL-servers op computers instellen

Dit abonnement inschakelen:

* Richt de Log Analytics-agent in op de host van uw SQL-Server. Dit geeft de verbinding met Azure.

* Schakel het optionele abonnement in op de pagina prijzen en instellingen van Security Center.

Beide worden hieronder beschreven.

### <a name="step-1-provision-the-log-analytics-agent-on-your-sql-servers-host"></a>Stap 1. Richt de Log Analytics-agent in op de host van uw SQL-Server:

- **SQL Server op de Azure-VM** : als uw SQL-machine wordt gehost op een Azure-VM, kunt u [de log Analytics agent automatisch inrichten](security-center-enable-data-collection.md#workspace-configuration). U kunt ook de hand matige procedure volgen voor [het Onboarden van uw Azure stack-vm's](quickstart-onboard-machines.md#onboard-your-azure-stack-vms).
- **SQL Server op Azure Arc** : als uw SQL Server wordt gehost op een [Azure Arc](https://docs.microsoft.com/azure/azure-arc/) -computer, kunt u de log Analytics agent implementeren met behulp van de Security Center aanbeveling ' Log Analytics agent moet worden geïnstalleerd op uw op Windows gebaseerde Azure Arc-computers (preview) '. U kunt ook de hand matige procedure volgen in de [Azure Arc-documentatie](https://docs.microsoft.com/azure/azure-arc/servers/manage-vm-extensions#enable-extensions-from-the-portal).

- **SQL Server on-premises** : als uw SQL Server wordt gehost op een on-premises Windows-machine zonder Azure Arc, hebt u twee opties om deze te verbinden met Azure:
    
    - **Azure-Arc implementeren** : u kunt een Windows-machine verbinden met Security Center. Azure Arc biedt echter een diep gaande integratie in *al* uw Azure-omgeving. Als u Azure Arc hebt ingesteld, ziet u de pagina **SQL Server – Azure Arc** in de portal en worden uw beveiligings waarschuwingen weer gegeven op een specifiek tabblad **beveiliging** op die pagina. De eerste en aanbevolen optie is om [Azure Arc op de host in te stellen](https://docs.microsoft.com/azure/azure-arc/servers/onboard-portal#install-and-validate-the-agent-on-windows) en de instructies voor **SQL Server op Azure Arc**te volgen.
        
    - **De Windows-machine verbinden zonder Azure Arc** : als u ervoor kiest om verbinding te maken met een SQL Server die wordt uitgevoerd op een Windows-computer zonder Azure Arc te gebruiken, volgt u de instructies in [Windows-computers verbinden met Azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).


### <a name="step-2-enable-the-optional-plan-in-security-centers-pricing-and-settings-page"></a>Stap 2. Schakel het optionele abonnement in op de pagina prijzen en instellingen van Security Center:

1. Open in het menu van Security Center de pagina **& instellingen voor prijzen** .

    - Als u de **standaard werkruimte van Azure Security Center** gebruikt (met de naam ' defaultworkspace-[uw abonnement-id]-[regio] '), selecteert u het relevante **abonnement**.

    - Als u **een niet-standaard werk ruimte**gebruikt, selecteert u de relevante **werk ruimte** (Voer indien nodig de naam van de werk ruimte in het filter in):

        ![Uw niet-standaard werkruimte zoeken op titel](./media/security-center-advanced-iaas-data/pricing-and-settings-workspaces.png)

1. Stel de optie voor **Azure Defender voor SQL-servers op computers (preview)** in **op**aan. 

    ![Security Center prijs pagina met optionele abonnementen](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)

    Het plan wordt ingeschakeld op alle SQL-servers die zijn verbonden met de geselecteerde werk ruimte. De beveiliging is volledig actief na de eerste keer opnieuw opstarten van de SQL Server-instantie.

    >[!TIP] 
    > Als u een nieuwe werk ruimte wilt maken, volgt u de instructies in [een log Analytics-werk ruimte maken](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).


1. U kunt ook e-mail meldingen configureren voor beveiligings waarschuwingen. 
    U kunt een lijst met ontvangers instellen om een e-mail melding te ontvangen wanneer Security Center waarschuwingen worden gegenereerd. Het e-mail bericht bevat een directe koppeling naar de waarschuwing in Azure Security Center met alle relevante informatie. Zie [e-mail meldingen instellen voor beveiligings waarschuwingen](security-center-provide-security-contact-details.md)voor meer informatie.



## <a name="explore-vulnerability-assessment-reports"></a>Rapporten van evaluatie van beveiligings problemen verkennen

De service voor evaluatie van beveiligings problemen scant uw data bases eenmaal per week. De scans worden uitgevoerd op dezelfde dag van de week waarop u de service hebt ingeschakeld.

Het dash board evaluatie van beveiligings problemen biedt een overzicht van de evaluatie resultaten voor al uw data bases, samen met een samen vatting van gezonde en beschadigde data bases en een algehele samen vatting van mislukte controles volgens risico distributie.

U kunt de resultaten van de evaluatie van de beveiligings problemen rechtstreeks vanuit Security Center weer geven.

1. Open de pagina **aanbevelingen** vanuit de zijbalk van Security Center en selecteer de aanbevolen **beveiligings problemen op uw SQL-servers op computers moeten worden hersteld (preview)**. Zie [Security Center-aanbevelingen](security-center-recommendations.md)voor meer informatie. 

    :::image type="content" source="./media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png" alt-text="De resultaten van evaluatie van beveiligings problemen op uw SQL-servers op computers moeten worden hersteld (preview-versie)":::

    De gedetailleerde weer gave voor deze aanbeveling wordt weer gegeven.

    :::image type="content" source="./media/security-center-advanced-iaas-data/all-servers-view.png" alt-text="De resultaten van evaluatie van beveiligings problemen op uw SQL-servers op computers moeten worden hersteld (preview-versie)":::

1. Voor meer informatie, inzoomen:

    * Selecteer de gewenste server voor een overzicht van gescande resources (data bases) en de lijst met beveiligings controles die zijn getest.

    * Selecteer de gewenste Data Base voor een overzicht van de beveiligings problemen die zijn gegroepeerd op een specifieke SQL database.

    In elke weer gave worden de beveiligings controles gesorteerd op **Ernst**. Klik op een specifieke beveiligings controle om het deel venster Details te bekijken met een **Beschrijving**, het probleem te **verhelpen** en andere gerelateerde informatie, zoals **impact** of **Bench Mark**.

## <a name="azure-defender-for-sql-alerts"></a>Azure Defender voor SQL-waarschuwingen
Waarschuwingen worden gegenereerd door ongebruikelijke en mogelijk schadelijke pogingen om SQL-machines te openen of misbruik te maken. Deze gebeurtenissen kunnen waarschuwingen activeren die worden weer gegeven in de [sectie waarschuwingen voor SQL database en Azure Synapse Analytics (voorheen SQL Data Warehouse) van de pagina met waarschuwingen](alerts-reference.md#alerts-sql-db-and-warehouse).

## <a name="explore-and-investigate-security-alerts"></a>Beveiligings waarschuwingen verkennen en onderzoeken

Azure Defender-waarschuwingen zijn beschikbaar op de pagina waarschuwingen van Security Center, het tabblad Beveiliging van de resource, het [Azure Defender-dash board](azure-defender-dashboard.md)of via de directe koppeling in de e-mail waarschuwingen.

1. Als u waarschuwingen wilt weer geven, selecteert u **beveiligings waarschuwingen** in het menu van Security Center en selecteert u een waarschuwing.

1. Waarschuwingen zijn zodanig ontworpen dat ze zich op zichzelf bevinden, met gedetailleerde stappen voor herbemiddeling en informatie over onderzoek. U kunt verder onderzoeken door andere Azure Security Center en Azure-Sentinel-mogelijkheden te gebruiken voor een bredere weer gave:

    * Schakel de controle functie van SQL Server in voor verdere onderzoeken. Als u een Azure Sentinel-gebruiker bent, kunt u de SQL-controle logboeken uploaden van de gebeurtenissen van het Windows-beveiligings logboek naar Sentinel en een rijke onderzoek ervaring hebben. [Meer informatie over het controleren van SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification?view=sql-server-ver15).
    * Gebruik de aanbevelingen van Security Center voor de hostmachine die in elke waarschuwing worden aangegeven om uw beveiligings postuur te verbeteren. Hierdoor worden de Risico's van toekomstige aanvallen verminderd. 

    Meer [informatie over het beheren van en reageren op waarschuwingen](security-center-managing-and-responding-alerts.md).


## <a name="next-steps"></a>Volgende stappen

Zie het volgende artikel voor verwante materialen:

- [Beveiligings waarschuwingen voor SQL Database en Azure Synapse Analytics (voorheen SQL Data Warehouse)](alerts-reference.md#alerts-sql-db-and-warehouse)
- [E-mail meldingen instellen voor beveiligings waarschuwingen](security-center-provide-security-contact-details.md)
- [Meer informatie over Azure Sentinel](https://docs.microsoft.com/azure/sentinel/)
- [Gegevens beveiligings pakket van Azure Security Center](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)