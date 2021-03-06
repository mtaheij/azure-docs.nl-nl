---
title: Failover van virtuele VMware-machines naar Azure met Site Recovery
description: Meer informatie over het uitvoeren van een failover van virtuele VMware-machines naar Azure in Azure Site Recovery
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/16/2019
ms.custom: MVC
ms.openlocfilehash: bf47f08ac555cf60f59ba2b1a84750b6a9e2e0a1
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86132007"
---
# <a name="fail-over--vmware-vms"></a>Failover uitvoeren van virtuele VMware-machines

In dit artikel wordt beschreven hoe u een failover uitvoert van een on-premises virtuele VMware-machine (VM) naar Azure met [Azure Site Recovery](site-recovery-overview.md).

Dit is de vijfde zelfstudie in een reeks waarin u ziet hoe u herstel na noodgeval naar Azure kunt instellen voor on-premises machines.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of de eigenschappen van de VMware-VM in overeenstemming zijn met de Azure-vereisten.
> * Failover uitvoeren van specifieke VM's naar Azure.

> [!NOTE]
> In zelfstudies ziet u steeds het eenvoudigste implementatiepad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en worden niet alle mogelijke instellingen en paden weergegeven. Zie [Failover van VM's en fysieke servers](site-recovery-failover.md)voor meer informatie over failover.

[Meer informatie](failover-failback-overview.md#types-of-failover) over verschillende typen failover. Als u een failover wilt uitvoeren voor meerdere VM's in een herstelplan, raadpleegt u [dit artikel](site-recovery-failover.md).

## <a name="before-you-start"></a>Voordat u begint

Voltooi de vorige zelfstudies:

1. Zorg ervoor dat u [Azure hebt ingesteld](tutorial-prepare-azure.md) voor on-premises noodherstel van VMware-VM’s, Hyper-V-VM’s en fysieke machines naar Azure.
2. Bereid uw on-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md)-omgeving voor op herstel na noodgeval. 
3. Stel herstel na noodgeval in voor [VMware-VM's](vmware-azure-tutorial.md).
4. Voer een [DR-herstelanalyse](tutorial-dr-drill-azure.md) uit om ervoor te zorgen dat alles werkt zoals verwacht.

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failover uitvoert, controleert u de eigenschappen van de VM om ervoor te zorgen dat de VM's voldoen aan de [Azure-vereisten](vmware-physical-azure-support-matrix.md#replicated-machines).

Controleer de eigenschappen als volgt:

1. Selecteer in **Beveiligde items**de optie **Gerepliceerde items** en selecteer vervolgens de VM die u wilt controleren.

2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Selecteer **Eigenschappen** om meer details te bekijken.

3. In **Compute en netwerk** kunt u deze eigenschappen naar behoefte wijzigen:
    * Azure-naam
    * Resourcegroep
    * Doelgrootte
    * [Beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md)
    * Instellingen voor beheerde schijven

4. U kunt netwerk instellingen weergeven en wijzigen, waaronder:

    * Het netwerk en subnet waarin de Azure-VM zich na een failover bevindt.
    * Het IP-adres dat eraan wordt toegewezen.

5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. Selecteer in **Instellingen** > **Gerepliceerde items** de VM waarvoor u een failover wilt uitvoeren en selecteer vervolgens **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   * **Laatste**: Met deze optie worden eerst alle gegevens verwerkt die naar Site Recovery zijn verzonden. Dit biedt het laagste RPO (Recovery Point Objective), omdat de na de failover gemaakte Azure-VM alle gegevens bevat die naar Site Recovery zijn gerepliceerd toen de failover werd geactiveerd.
   * **Laatst verwerkt**: Met deze optie wordt een failover van de VM uitgevoerd naar het laatste door Site Recovery verwerkte herstelpunt. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
   * **Laatste toepassingsconsistente punt**: Met deze optie wordt er een failover uitgevoerd van de VM naar het laatste door Site Recovery verwerkte herstelpunt dat consistent is met de app.
   * **Aangepast**: Met deze optie kunt u een herstelpunt opgeven.

3. Selecteer **Sluit de computer af voordat de failover wordt gestart** om bron-VM's af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet, ook als het afsluiten mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. Er kunnen langere failover-tijden voor het testen nodig zijn:

* VMware-VM's met een versie van Mobility-service ouder dan 9.8.
* Fysieke servers.
* VMware Linux-VMs.
* Hyper-V-VM's die worden beveiligd als fysieke servers.
* VMware-VM's waarvoor de DHCP-service niet is ingeschakeld.
* VMware-VM's die niet over de volgende opstartstuurprogramma's beschikken: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> Annuleer nooit een failover die in uitvoering is. De VM-replicatie wordt gestopt voordat de failover is gestart. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

## <a name="connect-to-failed-over-vm"></a>Verbinding maken met VM waarvoor failover is uitgevoerd

1. Als u na een failover verbinding wilt maken met Azure-VM's met behulp van Remote Desktop Protocol (RDP) en Secure Shell (SSH), [controleert u of aan de vereisten is voldaan](failover-failback-overview.md#connect-to-azure-after-failover).
2. Na de failover gaat u naar de VM en valideert u door [verbinding te maken](../virtual-machines/windows/connect-logon.md) met deze machine.
3. Gebruik **Herstelpunt wijzigen** als u na een failover een ander herstelpunt wilt gebruiken. Nadat u de failover in de volgende stap hebt doorgevoerd, is deze optie niet meer beschikbaar.
4. Na validatie selecteert u **Doorvoeren** om het herstelpunt van de VM na een failover te voltooien.
5. Na het doorvoeren worden alle andere herstelpunten verwijderd. Met deze stap wordt de failover voltooid.

>[!TIP]
> Volg de handleiding voor het [oplossen van problemen](site-recovery-failover-to-azure-troubleshoot.md) als u na een failover connectiviteitsproblemen ondervindt.

## <a name="next-steps"></a>Volgende stappen

Na een failover kunt u de Azure-VM's opnieuw beveiligen in de on-premises omgeving. Nadat de VM's opnieuw zijn beveiligd en gerepliceerd naar de on-premises site, kunt u een failback uitvoeren vanuit Azure wanneer u klaar bent.

> [!div class="nextstepaction"]
> [Azure VM's opnieuw beveiligen](vmware-azure-reprotect.md)
> [Failback uitvoeren vanuit Azure](vmware-azure-failback.md)
