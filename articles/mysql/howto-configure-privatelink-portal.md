---
title: Privé-koppeling-Azure Portal-Azure Database for MySQL
description: Meer informatie over het configureren van een persoonlijke koppeling voor Azure Database for MySQL van Azure Portal
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 01/09/2020
ms.openlocfilehash: 1a99a91152f8308af122677ad3b8df3fb5005dbb
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90896177"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mysql-using-portal"></a>Een persoonlijke koppeling voor Azure Database for MySQL maken en beheren met behulp van portal

Een privé-eindpunt is de fundamentele bouwsteen voor een Private Link in Azure. Het biedt Azure-resources, zoals virtuele machines, de mogelijkheid om Private Link-resources te gebruiken om privé met elkaar communiceren. In dit artikel leert u hoe u de Azure Portal kunt gebruiken om een virtuele machine te maken in een Azure-Virtual Network en een Azure Database for MySQL-server met een persoonlijk Azure-eind punt.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> De functie voor persoonlijke koppelingen is alleen beschikbaar voor Azure Database for MySQL servers in de prijs Categorieën Algemeen of geoptimaliseerd voor geheugen. Zorg ervoor dat de database server zich in een van deze prijs categorieën bevindt.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de [Azure-portal](https://portal.azure.com).

## <a name="create-an-azure-vm"></a>Een Azure-VM maken

In deze sectie maakt u een virtueel netwerk en het subnet voor het hosten van de virtuele machine die wordt gebruikt voor toegang tot uw persoonlijke koppelings bron (een MySQL-server in Azure).

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken
In deze sectie maakt u een virtueel netwerk en het subnet om de VM te hosten die wordt gebruikt om op een veilige manier toegang te krijgen tot uw Private Link-resource.

1. Selecteer in de linkerbovenhoek van het scherm **een resource**  >  **netwerk**  >  **virtueel netwerk**maken.
2. Typ of selecteer in **Virtueel netwerk maken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Naam | Voer *MyVirtualNetwork*in. |
    | Adresruimte | Voer *10.1.0.0/16*in. |
    | Abonnement | Selecteer uw abonnement.|
    | Resourcegroep | Selecteer **Nieuwe maken**, voer *myResourceGroup* in en selecteer vervolgens **OK**. |
    | Locatie | Selecteer **Europa - west**.|
    | Subnet - Naam | Voer *mySubnet*in. |
    | Subnet - adresbereik | Voer *10.1.0.0/24*in. |
    |||
3. Laat voor de rest de standaardwaarden staan en selecteer **Maken**.

### <a name="create-virtual-machine"></a>Virtuele machine maken

1. Selecteer in de linkerbovenhoek van het scherm in de Azure Portal de optie **Een resource maken** > **Compute** > **Virtuele machine**.

2. Typ of selecteer in **Een virtuele machine maken - Basisprincipes** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | **PROJECTGEGEVENS** | |
    | Abonnement | Selecteer uw abonnement. |
    | Resourcegroep | Selecteer **myResourceGroup**. U hebt deze in de vorige sectie gemaakt.  |
    | **EXEMPLAARDETAILS** |  |
    | Naam van de virtuele machine | Voer *myVm* in. |
    | Regio | Selecteer **Europa - west**. |
    | Beschikbaarheidsopties | Laat de standaardwaarde **Geen infrastructuurredundantie vereist** staan. |
    | Installatiekopie | Selecteer **Windows Server 2019 Datacenter**. |
    | Grootte | Laat de standaardwaarde **Standard DS1 v2** staan. |
    | **ADMINISTRATOR-ACCOUNT** |  |
    | Gebruikersnaam | Voer een gebruikersnaam naar keuze in. |
    | Wachtwoord | Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Wachtwoord bevestigen | Voer het wachtwoord opnieuw in. |
    | **REGELS VOOR BINNENKOMENDE POORT** |  |
    | Openbare poorten voor inkomend verkeer | Laat de standaardwaarde **Geen** staan. |
    | **GELD BESPAREN** |  |
    | Hebt u al een Windows-licentie? | Laat de standaardwaarde **Nee** staan. |
    |||

1. Selecteer **Volgende: Schijven**.

1. Behoud de standaardinstellingen in **Een virtuele machine maken – schijven** en selecteer **Volgende: Netwerken**.

1. Selecteer in **Een virtuele machine maken - Netwerken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Virtueel netwerk | Laat de standaardwaarde **MyVirtualNetwork** staan.  |
    | Adresruimte | Laat de standaardwaarde **10.1.0.0/24** staan.|
    | Subnet | Laat de standaardwaarde **mySubnet (10.1.0.0/24)** staan.|
    | Openbare IP | Handhaaf de standaardinstelling **(new) myVm-ip**. |
    | Openbare poorten voor inkomend verkeer | Selecteer **Geselecteerde poorten toestaan**. |
    | Binnenkomende poorten selecteren | Selecteer **HTTP** en **RDP**.|
    |||


1. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven, waar uw configuratie wordt gevalideerd in Azure.

1. Als u het bericht **Validatie geslaagd** ziet, selecteert u **Maken**.

## <a name="create-an-azure-database-for-mysql"></a>Een Azure Database voor MySQL maken

In deze sectie maakt u een Azure Database for MySQL-server in Azure. 

1. Selecteer in de linkerbovenhoek van het scherm in de Azure Portal **een resource**  >  **databases**maken  >  **Azure database for MySQL**.

1. Geef in **Azure database for MySQL** deze informatie op:

    | Instelling | Waarde |
    | ------- | ----- |
    | **Projectgegevens** | |
    | Abonnement | Selecteer uw abonnement. |
    | Resourcegroep | Selecteer **myResourceGroup**. U hebt deze in de vorige sectie gemaakt.|
    | **Servergegevens** |  |
    |Servernaam  | Voer *mijn server*in. Als deze naam al wordt gebruikt, maakt u een unieke naam.|
    | Gebruikersnaam van beheerder| Voer de naam van de beheerder van uw keuze in. |
    | Wachtwoord | Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 8 tekens lang zijn en moet voldoen aan de vooraf gedefinieerde vereisten. |
    | Locatie | Selecteer een Azure-regio waar u wilt dat de MySQL-server zich bevindt. |
    |Versie  | Selecteer de database versie van de MySQL-server die is vereist.|
    | Compute en opslag| Selecteer de prijs categorie die nodig is voor de server op basis van de werk belasting. |
    |||
 
7. Selecteer **OK**. 
8. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven, waar uw configuratie wordt gevalideerd in Azure. 
9. Wanneer u het bericht door gegeven validatie ziet, selecteert u **maken**. 
10. Wanneer u het bericht door gegeven validatie ziet, selecteert u maken. 

> [!NOTE]
> In sommige gevallen bevinden de Azure Database for MySQL en het VNet-subnet zich in verschillende abonnementen. In deze gevallen moet u ervoor zorgen dat u de volgende configuraties hebt:
> - Zorg ervoor dat voor beide abonnementen de resource provider **micro soft. DBforMySQL** is geregistreerd. Raadpleeg [Resource-Manager-registratie][resource-manager-portal] voor meer informatie

## <a name="create-a-private-endpoint"></a>Een privé-eindpunt maken

In deze sectie maakt u een MySQL-server en voegt u hieraan een persoonlijk eind punt toe. 

1. Selecteer in de linkerbovenhoek van het scherm in het Azure Portal de optie **een resource maken**  >  **Networking**  >  **persoonlijke netwerk koppeling**.

2. Selecteer in **Private Link-centrum – Overzicht** bij de optie **Een particuliere verbinding met een service maken** de optie **Start**.

    :::image type="content" source="media/concepts-data-access-and-security-private-link/privatelink-overview.png" alt-text="Overzicht van persoonlijke koppelingen":::

1. Voer in **een persoonlijk eind punt maken-basis beginselen**de volgende gegevens in of Selecteer deze:

    | Instelling | Waarde |
    | ------- | ----- |
    | **Projectgegevens** | |
    | Abonnement | Selecteer uw abonnement. |
    | Resourcegroep | Selecteer **myResourceGroup**. U hebt deze in de vorige sectie gemaakt.|
    | **Exemplaar Details** |  |
    | Naam | Voer *myPrivateEndpoint* in. Als deze naam al wordt gebruikt, maakt u een unieke naam. |
    |Regio|Selecteer **Europa - west**.|
    |||

5. Selecteer **Volgende: Resource**.
6. Typ of selecteer in **Een privé-eindpunt maken – Resource** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    |Verbindingsmethode  | Selecteer Verbinding maken met een Azure-resource in mijn directory.|
    | Abonnement| Selecteer uw abonnement. |
    | Resourcetype | Selecteer **micro soft. DBforMySQL/servers**. |
    | Resource |Selecteer *myServer*|
    |Stel subresource in |*MysqlServer* selecteren|
    |||
7. Selecteer **Volgende: Configuratie**.
8. Voer in **een persoonlijk eind punt maken-configuratie**de volgende gegevens in of Selecteer deze:

    | Instelling | Waarde |
    | ------- | ----- |
    |**NETWERKEN**| |
    | Virtueel netwerk| Selecteer *MyVirtualNetwork*. |
    | Subnet | Selecteer *mySubnet*. |
    |**INTEGRATIE VAN PRIVÉ-DNS**||
    |Integreren met privé-DNS-zone |Selecteer **Ja**. |
    |Privé-DNS-zone |Selecteer *(nieuw) privatelink. mysql. data base. Azure. com* |
    |||

    > [!Note] 
    > Gebruik de vooraf gedefinieerde privé-DNS-zone voor uw service of geef de naam van uw voor Keurs-DNS-zone op. Raadpleeg de configuratie van de [DNS-zone van Azure-Services](../private-link/private-endpoint-dns.md) voor meer informatie.

1. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven, waar uw configuratie wordt gevalideerd in Azure. 
2. Als u het bericht **Validatie geslaagd** ziet, selecteert u **Maken**. 

    :::image type="content" source="media/concepts-data-access-and-security-private-link/show-mysql-private-link.png" alt-text="Privé koppeling gemaakt":::

    > [!NOTE] 
    > De FQDN in de DNS-instelling van de klant wordt niet omgezet naar het geconfigureerde particuliere IP-adres. U moet een DNS-zone voor de geconfigureerde FQDN instellen, zoals [hier](../dns/dns-operations-recordsets-portal.md)wordt weer gegeven.

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Verbinding maken met een virtuele machine met behulp van Extern bureaublad (RDP)


Nadat u **myVm** hebt gemaakt, maakt u hiermee als volgt verbinding via internet: 

1. Voer in de zoekbalk van de portal *myVm* in.

1. Selecteer de knop **Verbinding maken**. Na het selecteren van de knop **Verbinden** wordt **Verbinden met virtuele machine** geopend.

1. Selecteer **RDP-bestand downloaden**. In Azure wordt een *RDP*-bestand (Remote Desktop Protocol) gemaakt en het bestand wordt gedownload naar de computer.

1. Open het *downloaded.rdp*-bestand.

    1. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.

    1. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine.

        > [!NOTE]
        > Mogelijk moet u **Meer opties** > **Een ander account gebruiken** selecteren om de referenties op te geven die u hebt ingevoerd tijdens het maken van de VM.

1. Selecteer **OK**.

1. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als er een certificaatwaarschuwing wordt weergegeven, selecteert u **Ja** of **Doorgaan**.

1. Wanneer het VM-bureaublad wordt weergegeven, minimaliseert u het om terug te gaan naar het lokale bureaublad.

## <a name="access-the-mysql-server-privately-from-the-vm"></a>De MySQL-server privé openen vanuit de VM

1. Open PowerShell in het extern bureaublad van *myVM*.

2. Voer in  `nslookup  myServer.privatelink.mysql.database.azure.com` . 

    U ontvangt een bericht dat er ongeveer als volgt uitziet:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myServer.privatelink.mysql.database.azure.com
    Address:  10.1.3.4
    ```

3. Test de verbinding van de particuliere verbinding voor de MySQL-server met behulp van een beschik bare client. In het onderstaande voor beeld heb ik [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) gebruikt om de bewerking uit te voeren.

4. In **nieuwe verbinding**voert u de volgende gegevens in of selecteert u deze:

    | Instelling | Waarde |
    | ------- | ----- |
    | Servertype| Selecteer **MySQL**.|
    | Servernaam| *MyServer.privatelink.mysql.database.Azure.com* selecteren |
    | Gebruikersnaam | Voer de gebruikers naam in, username@servername die wordt opgegeven tijdens het maken van de mysql-server. |
    |Wachtwoord |Voer een wacht woord in dat is opgegeven tijdens het maken van de MySQL-server. |
    |SSL|Selecteer **vereist**.|
    ||

5. Selecteer Verbinding maken.

6. Blader in het menu aan de linkerkant door databases.

7. Eventueel Informatie van de MySQL-server maken of er een query op uitvoeren.

8. Sluit de verbinding met extern bureau blad met myVm.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met het persoonlijke eind punt, MySQL-server en de virtuele machine, verwijdert u de resource groep en alle resources die deze bevat:

1. Typ *myResourceGroup* in het vak **Zoeken** bovenaan de portal en selecteer *myResourceGroup* in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer myResourceGroup in bij **Typ de naam van de resource groep** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze procedure hebt u een VM gemaakt in een virtueel netwerk, een Azure Database for MySQL en een persoonlijk eind punt voor persoonlijke toegang. U hebt verbinding gemaakt met één virtuele machine via internet en u kunt een beveiligde verbinding met de MySQL-server met behulp van een persoonlijke koppeling. Zie [Wat is Azure private endpoint](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)? voor meer informatie over privé-eind punten.

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md