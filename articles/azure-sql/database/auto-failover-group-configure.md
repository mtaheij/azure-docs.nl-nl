---
title: Een failovergroep configureren
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Meer informatie over het configureren van een groep voor automatische failover voor een Azure SQL Database (zowel één als gepoold) en een SQL-beheerd exemplaar met behulp van de Azure Portal, de Azure CLI en Power shell.
services: sql-database
ms.service: sql-db-mi
ms.subservice: high-availability
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein
ms.date: 08/14/2019
ms.openlocfilehash: ab057e1328efbff294faa1d68f2a27c5a1f03ade
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/30/2020
ms.locfileid: "91577506"
---
# <a name="configure-a-failover-group-for-azure-sql-database"></a>Een failovergroep voor Azure SQL Database configureren
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In dit onderwerp leert u hoe u een [groep voor automatische failover](auto-failover-group-overview.md) configureert voor Azure SQL database en Azure SQL Managed instance.

## <a name="single-database"></a>Individuele database

Maak de failovergroep en voeg er een toe aan een enkele data base met behulp van de Azure Portal of Power shell.

### <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten:

- De aanmeldings-en Firewall instellingen van de server voor de secundaire server moeten overeenkomen met die van de primaire server.

### <a name="create-failover-group"></a>Failovergroep maken

# <a name="portal"></a>[Portal](#tab/azure-portal)

Maak uw failovergroep en voeg uw afzonderlijke data base toe met behulp van de Azure Portal.

1. Selecteer **Azure SQL** in het linkermenu van de [Azure Portal](https://portal.azure.com). Als **Azure SQL** niet in de lijst staat, selecteert u **Alle services** en typt u Azure SQL in het zoekvak. Beschrijving Selecteer de ster naast **Azure SQL** om deze te favoriet en voeg deze toe als een item in de linkernavigatiebalk.
1. Selecteer de data base die u wilt toevoegen aan de failovergroep.
1. Selecteer de naam van de server onder **Server naam** om de instellingen voor de server te openen.

   ![Server openen voor één data base](./media/auto-failover-group-configure/open-sql-db-server.png)

1. Selecteer **failover-groepen** in het deel venster **instellingen** en selecteer vervolgens **groep toevoegen** om een nieuwe failovergroep te maken.

   ![Nieuwe failovergroep toevoegen](./media/auto-failover-group-configure/sqldb-add-new-failover-group.png)

1. Voer op de pagina **failover-groep** de vereiste waarden in of Selecteer deze en selecteer vervolgens **maken**.

   - **Data bases in de groep**: Kies de data base die u aan de failovergroep wilt toevoegen. Als u de data base aan de groep failover toevoegt, wordt het proces voor geo-replicatie automatisch gestart.

   ![SQL Database toevoegen aan failovergroep](./media/auto-failover-group-configure/add-sqldb-to-failover-group.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak uw failovergroep en voeg uw data base aan de groep toe met behulp van Power shell.

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a secondary server in the failover region
   Write-host "Creating a secondary server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup

   # Add the database to the failover group
   Write-host "Adding the database to the failover group..."
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..."
   ```

---

### <a name="test-failover"></a>Testfailover

Test de failover van de failovergroep met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Test de failover van de failovergroep met behulp van de Azure Portal.

1. Selecteer **Azure SQL** in het linkermenu van de [Azure Portal](https://portal.azure.com). Als **Azure SQL** niet voor komt in de lijst, selecteert u **alle services**en typt u ' Azure SQL ' in het zoekvak. Beschrijving Selecteer de ster naast **Azure SQL** om deze te favoriet en voeg deze toe als een item in de linkernavigatiebalk.
1. Selecteer de data base die u wilt toevoegen aan de failovergroep.

   ![Server openen voor één data base](./media/auto-failover-group-configure/open-sql-db-server.png)

1. Selecteer **failover-groepen** in het deel venster **instellingen** en kies vervolgens de groep die u zojuist hebt gemaakt.
  
   ![De groep failover selecteren in de portal](./media/auto-failover-group-configure/select-failover-group.png)

1. Controleer welke server primair is en welke server secundair is.
1. Selecteer **failover** in het taak venster om de failover-groep met uw data base bij te werken.
1. Selecteer **Ja** in de waarschuwing waarmee u wordt gewaarschuwd dat TDS-sessies worden losgekoppeld.

   ![Failover uitvoeren voor de failovergroep met uw data base](./media/auto-failover-group-configure/failover-sql-db.png)

1. Controleer welke server nu primair is en welke server secundair is. Als de failover is geslaagd, moeten de twee servers wisselende rollen hebben.
1. Selecteer **failover** opnieuw om de oorspronkelijke rollen van de servers te herstellen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Test de failover van de failovergroep met behulp van Power shell.  

Controleer de rol van de secundaire replica:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...."
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

Failover naar de secundaire server:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName
   ```

Herstel de failovergroep terug naar de primaire server:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to back to" $serverName
   ```

---

> [!IMPORTANT]
> Als u de secundaire data base moet verwijderen, verwijdert u deze uit de failover-groep voordat u deze verwijdert. Het verwijderen van een secundaire Data Base voordat deze wordt verwijderd uit de failovergroep kan onvoorspelbaar gedrag veroorzaken.

## <a name="elastic-pool"></a>Elastische pool

Maak de failovergroep en voeg er een elastische pool aan toe met behulp van de Azure Portal of Power shell.  

### <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten:

- De aanmeldings-en Firewall instellingen van de server voor de secundaire server moeten overeenkomen met die van de primaire server.

### <a name="create-the-failover-group"></a>De failover-groep maken

Maak de failovergroep voor uw elastische pool met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Maak uw failovergroep en voeg uw elastische groep toe met behulp van de Azure Portal.

1. Selecteer **Azure SQL** in het linkermenu van de [Azure Portal](https://portal.azure.com). Als **Azure SQL** niet voor komt in de lijst, selecteert u **alle services**en typt u ' Azure SQL ' in het zoekvak. Beschrijving Selecteer de ster naast **Azure SQL** om deze te favoriet en voeg deze toe als een item in de linkernavigatiebalk.
1. Selecteer de elastische pool die u aan de failovergroep wilt toevoegen.
1. Selecteer in het deel venster **overzicht** de naam van de server onder **Server naam** om de instellingen voor de server te openen.
  
   ![Server openen voor elastische pool](./media/auto-failover-group-configure/server-for-elastic-pool.png)

1. Selecteer **failover-groepen** in het deel venster **instellingen** en selecteer vervolgens **groep toevoegen** om een nieuwe failovergroep te maken.

   ![Nieuwe failovergroep toevoegen](./media/auto-failover-group-configure/sqldb-add-new-failover-group.png)

1. Voer op de pagina **failover-groep** de vereiste waarden in of Selecteer deze en selecteer vervolgens **maken**. Maak een nieuwe secundaire server of selecteer een bestaande secundaire server.

1. Selecteer **data bases in de groep** en kies vervolgens de elastische pool die u aan de failovergroep wilt toevoegen. Als er nog geen elastische pool aanwezig is op de secundaire server, verschijnt er een waarschuwing waarin u wordt gevraagd een elastische pool te maken op de secundaire server. Selecteer de waarschuwing en selecteer **OK** om de elastische pool te maken op de secundaire server.

   ![Elastische pool toevoegen aan failovergroep](./media/auto-failover-group-configure/add-elastic-pool-to-failover-group.png)

1. Selecteer **selecteren** om de instellingen voor de elastische groep toe te passen op de failovergroep en selecteer vervolgens **maken** om uw failovergroep te maken. Door de elastische pool toe te voegen aan de groep failover, wordt het proces voor geo-replicatie automatisch gestart.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak uw failovergroep en voeg uw elastische groep toe met behulp van Power shell.

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $poolName = "myElasticPool"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a failover group between the servers
   Write-host "Creating failover group..."
   New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -PartnerServerName $drServerName  `
       –FailoverGroupName $failoverGroupName `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2
   Write-host "Failover group created successfully."

   # Add elastic pool to the failover group
   Write-host "Enumerating databases in elastic pool...."
   $FailoverGroup = Get-AzSqlDatabaseFailoverGroup `
                    -ResourceGroupName $resourceGroupName `
                    -ServerName $serverName `
                    -FailoverGroupName $failoverGroupName
   $databases = Get-AzSqlElasticPoolDatabase `
               -ResourceGroupName $resourceGroupName `
               -ServerName $serverName `
               -ElasticPoolName $poolName
   Write-host "Adding databases to failover group..."
   $failoverGroup = $failoverGroup | Add-AzSqlDatabaseToFailoverGroup `
                                     -Database $databases
   Write-host "Databases added to failover group successfully."
  ```

---

### <a name="test-failover"></a>Testfailover

Test de failover van uw elastische pool met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Voer een failover uit naar de secundaire server en failback uit met behulp van de Azure Portal.

1. Selecteer **Azure SQL** in het linkermenu van de [Azure Portal](https://portal.azure.com). Als **Azure SQL** niet voor komt in de lijst, selecteert u **alle services**en typt u ' Azure SQL ' in het zoekvak. Beschrijving Selecteer de ster naast **Azure SQL** om deze te favoriet en voeg deze toe als een item in de linkernavigatiebalk.
1. Selecteer de elastische pool die u aan de failovergroep wilt toevoegen.
1. Selecteer in het deel venster **overzicht** de naam van de server onder **Server naam** om de instellingen voor de server te openen.

   ![Server openen voor elastische pool](./media/auto-failover-group-configure/server-for-elastic-pool.png)
1. Selecteer **failover-groepen** in het deel venster **instellingen** en kies vervolgens de failovergroep die u hebt gemaakt in sectie 2.
  
   ![De groep failover selecteren in de portal](./media/auto-failover-group-configure/select-failover-group.png)

1. Controleer welke server primair is en welke server secundair is.
1. Selecteer **failover** in het taak venster voor het uitvoeren van een failover van uw groep met elastische groepen.
1. Selecteer **Ja** in de waarschuwing waarmee u wordt gewaarschuwd dat TDS-sessies worden losgekoppeld.

   ![Failover uitvoeren voor de failovergroep met uw data base](./media/auto-failover-group-configure/failover-sql-db.png)

1. Controleer welke server primair is, welke server secundair is. Als de failover is geslaagd, moeten de twee servers wisselende rollen hebben.
1. Selecteer **failover** opnieuw om de failback-groep weer in te stellen op de oorspronkelijke instellingen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Test de failover van de failovergroep met behulp van Power shell.

Controleer de rol van de secundaire replica:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...."
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

Failover naar de secundaire server:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName
   ```

---

> [!IMPORTANT]
> Als u de secundaire data base moet verwijderen, verwijdert u deze uit de failover-groep voordat u deze verwijdert. Het verwijderen van een secundaire Data Base voordat deze wordt verwijderd uit de failovergroep kan onvoorspelbaar gedrag veroorzaken.

## <a name="sql-managed-instance"></a>SQL Managed Instance

Een failover-groep maken tussen twee beheerde exemplaren in een Azure SQL Managed instance met behulp van de Azure Portal of Power shell.

U moet [ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md) configureren of een gateway maken voor het virtuele netwerk van elk SQL Managed instance, de twee gateways verbinden en vervolgens de failovergroep maken. 

Implementeer beide beheerde instanties in [gekoppelde regio's](../../best-practices-availability-paired-regions.md) om prestatie redenen. Beheerde instanties die zich in geografische paar regio's bevinden, hebben veel betere prestaties ten opzichte van niet-gekoppelde regio's. 

### <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten:

- Het secundaire beheerde exemplaar moet leeg zijn.
- Het subnet-bereik voor het secundaire virtuele netwerk mag het subnet-bereik van het primaire virtuele netwerk niet overlappen.
- De sortering en tijd zone van het secundaire beheerde exemplaar moeten overeenkomen met die van het primaire beheerde exemplaar.
- Bij het verbinden van de twee gateways moet de **gedeelde sleutel** hetzelfde zijn voor beide verbindingen.

### <a name="create-primary-virtual-network-gateway"></a>Primaire virtuele netwerk gateway maken

Als u [ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md)niet hebt geconfigureerd, kunt u de primaire virtuele netwerk gateway maken met de Azure Portal, of Power shell.

> [!NOTE]
> De SKU van de gateway is van invloed op de doorvoer prestaties. In dit artikel wordt een gateway met de meest elementaire SKU ( `HwGw1` ) geïmplementeerd. Implementeer een hogere SKU (bijvoorbeeld: `VpnGw3` ) om een hogere door voer te krijgen. Zie [Gateway-sku's](../../vpn-gateway/vpn-gateway-about-vpngateways.md#benchmark) voor alle beschik bare opties. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Maak de primaire virtuele netwerk gateway met behulp van de Azure Portal.

1. Ga in het [Azure Portal](https://portal.azure.com)naar de resource groep en selecteer de **virtuele netwerk** resource voor uw primaire beheerde exemplaar.
1. Selecteer **subnetten** onder **instellingen** en selecteer vervolgens om een nieuw **Gateway-subnet**toe te voegen. De standaard waarden behouden.

   ![Gateway toevoegen voor primair beheerde exemplaar](./media/auto-failover-group-configure/add-subnet-gateway-primary-vnet.png)

1. Nadat de subnet gateway is gemaakt, selecteert u **een resource maken** in het navigatie deel venster links en typt u `Virtual network gateway` in het zoekvak. Selecteer de bron van de **virtuele netwerk gateway** die is gepubliceerd door **micro soft**.

   ![Een nieuwe gateway voor het virtuele netwerk maken](./media/auto-failover-group-configure/create-virtual-network-gateway.png)

1. Vul de vereiste velden in om de gateway te configureren voor uw primaire beheerde exemplaar.

   In de volgende tabel ziet u de waarden die nodig zijn voor de gateway voor het primaire beheerde exemplaar:

    | **Veld** | Waarde |
    | --- | --- |
    | **Abonnement** |  Het abonnement waar uw primaire beheerde instantie zich bevindt. |
    | **Naam** | De naam voor de gateway van uw virtuele netwerk. |
    | **Regio** | De regio waar uw primaire beheerde instantie zich bevindt. |
    | **Gatewaytype** | Selecteer **VPN**. |
    | **VPN-type** | **Op route gebaseerd** selecteren |
    | **SKU**| De standaard waarde van wijzigen `VpnGw1` . |
    | **Locatie**| De locatie waar uw secundaire beheerde instantie en secundaire virtuele netwerk zich bevindt.   |
    | **Virtueel netwerk**| Selecteer het virtuele netwerk voor uw secundaire beheerde exemplaar. |
    | **Openbaar IP-adres**| Selecteer **Nieuw maken**. |
    | **Naam openbaar IP-adres**| Voer een naam in voor uw IP-adres. |
    | &nbsp; | &nbsp; |

1. Wijzig de andere waarden als standaard en selecteer vervolgens **controleren + maken** om de instellingen voor de gateway van uw virtuele netwerk te controleren.

   ![Primaire gateway-instellingen](./media/auto-failover-group-configure/settings-for-primary-gateway.png)

1. Selecteer **maken** om de nieuwe virtuele netwerk gateway te maken.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak de primaire virtuele netwerk gateway met behulp van Power shell.

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryVnetName = "<Primary-Virtual-Network-Name>"
   $primaryGWName = "<Primary-Gateway-Name>"
   $primaryGWPublicIPAddress = $primaryGWName + "-ip"
   $primaryGWIPConfig = $primaryGWName + "-ipc"
   $primaryGWAsn = 61000

   # Get the primary virtual network
   $vnet1 = Get-AzVirtualNetwork -Name $primaryVnetName -ResourceGroupName $primaryResourceGroupName
   $primaryLocation = $vnet1.Location

   # Create primary gateway
   Write-host "Creating primary gateway..."
   $subnet1 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet1
   $gwpip1= New-AzPublicIpAddress -Name $primaryGWPublicIPAddress -ResourceGroupName $primaryResourceGroupName `
            -Location $primaryLocation -AllocationMethod Dynamic
   $gwipconfig1 = New-AzVirtualNetworkGatewayIpConfig -Name $primaryGWIPConfig `
            -SubnetId $subnet1.Id -PublicIpAddressId $gwpip1.Id

   $gw1 = New-AzVirtualNetworkGateway -Name $primaryGWName -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -IpConfigurations $gwipconfig1 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $primaryGWAsn
   $gw1
   ```

---

### <a name="create-secondary-virtual-network-gateway"></a>Secundaire virtuele netwerk gateway maken

Maak de secundaire virtuele netwerk gateway met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Herhaal de stappen in de vorige sectie om het subnet van het virtuele netwerk en de gateway voor het secundaire beheerde exemplaar te maken. Vul de vereiste velden in om de gateway te configureren voor uw secundaire beheerde exemplaar.

In de volgende tabel ziet u de waarden die nodig zijn voor de gateway voor het secundaire beheerde exemplaar:

   | **Veld** | Waarde |
   | --- | --- |
   | **Abonnement** |  Het abonnement waar uw secundaire beheerde instantie zich bevindt. |
   | **Naam** | De naam van de gateway van uw virtuele netwerk, zoals `secondary-mi-gateway` . |
   | **Regio** | De regio waar uw secundaire beheerde instantie zich bevindt. |
   | **Gatewaytype** | Selecteer **VPN**. |
   | **VPN-type** | **Op route gebaseerd** selecteren |
   | **SKU**| De standaard waarde van wijzigen `VpnGw1` . |
   | **Locatie**| De locatie waar uw secundaire beheerde instantie en secundaire virtuele netwerk zich bevindt.   |
   | **Virtueel netwerk**| Selecteer het virtuele netwerk dat is gemaakt in sectie 2, zoals `vnet-sql-mi-secondary` . |
   | **Openbaar IP-adres**| Selecteer **Nieuw maken**. |
   | **Naam openbaar IP-adres**| Voer een naam in voor uw IP-adres, zoals `secondary-gateway-IP` . |
   | &nbsp; | &nbsp; |

   ![Instellingen van secundaire gateway](./media/auto-failover-group-configure/settings-for-secondary-gateway.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak de secundaire virtuele netwerk gateway met behulp van Power shell.

   ```powershell-interactive
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryVnetName = "<Secondary-Virtual-Network-Name>"
   $secondaryGWName = "<Secondary-Gateway-Name>"
   $secondaryGWPublicIPAddress = $secondaryGWName + "-IP"
   $secondaryGWIPConfig = $secondaryGWName + "-ipc"
   $secondaryGWAsn = 62000

   # Get the secondary virtual network
   $vnet2 = Get-AzVirtualNetwork -Name $secondaryVnetName -ResourceGroupName $secondaryResourceGroupName
   $secondaryLocation = $vnet2.Location

   # Create the secondary gateway
   Write-host "Creating secondary gateway..."
   $subnet2 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet2
   $gwpip2= New-AzPublicIpAddress -Name $secondaryGWPublicIPAddress -ResourceGroupName $secondaryResourceGroupName `
            -Location $secondaryLocation -AllocationMethod Dynamic
   $gwipconfig2 = New-AzVirtualNetworkGatewayIpConfig -Name $secondaryGWIPConfig `
            -SubnetId $subnet2.Id -PublicIpAddressId $gwpip2.Id

   $gw2 = New-AzVirtualNetworkGateway -Name $secondaryGWName -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -IpConfigurations $gwipconfig2 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $secondaryGWAsn

   $gw2
   ```

---

### <a name="connect-the-gateways"></a>De gateways verbinden

Maak verbindingen tussen de twee gateways met behulp van de Azure Portal of Power shell.

Er moeten twee verbindingen worden gemaakt: de verbinding tussen de primaire gateway en de secundaire gateway en vervolgens de verbinding tussen de secundaire gateway en de primaire gateway.

De gedeelde sleutel die voor beide verbindingen wordt gebruikt, moet hetzelfde zijn voor elke verbinding.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Maak verbindingen tussen de twee gateways met behulp van de Azure Portal.

1. Selecteer **een resource maken** op basis van de [Azure Portal](https://portal.azure.com).
1. Typ `connection` in het zoekvak en druk vervolgens op ENTER om te zoeken. Hiermee gaat u naar de **verbindings** bron, gepubliceerd door micro soft.
1. Selecteer **maken** om uw verbinding te maken.
1. Selecteer op het tabblad **basis beginselen** de volgende waarden en selecteer vervolgens **OK**.
    1. Selecteer `VNet-to-VNet` voor het **verbindings type**.
    1. Selecteer uw abonnement in de vervolgkeuzelijst.
    1. Selecteer de resource groep voor uw beheerde instantie in de vervolg keuzelijst.
    1. Selecteer de locatie van uw primaire beheerde instantie in de vervolg keuzelijst.
1. Op het tabblad **instellingen** selecteert of voert u de volgende waarden in en selecteert u **OK**:
    1. Kies de primaire netwerk gateway voor de **eerste virtuele netwerk gateway**, zoals `Primary-Gateway` .  
    1. Kies de secundaire netwerk gateway voor de **tweede virtuele netwerk gateway**, zoals `Secondary-Gateway` .
    1. Schakel het selectie vakje in naast **bidirectionele connectiviteit instellen**.
    1. Wijzig de standaard naam van de primaire verbinding of geef deze de gewenste waarde.
    1. Geef een **gedeelde sleutel (PSK)** op voor de verbinding, zoals `mi1m2psk` .

   ![Gateway verbinding maken](./media/auto-failover-group-configure/create-gateway-connection.png)

1. Controleer op het tabblad **samen vatting** de instellingen voor uw bidirectionele verbinding en selecteer vervolgens **OK** om de verbinding te maken.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak verbindingen tussen de twee gateways met behulp van Power shell.

   ```powershell-interactive
   $vpnSharedKey = "mi1mi2psk"
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryGWConnection = "<Primary-connection-name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryGWConnection = "<Secondary-connection-name>"
   $secondaryLocation = "<Secondary-Region>"
  
   # Connect the primary to secondary gateway
   Write-host "Connecting the primary gateway"
   New-AzVirtualNetworkGatewayConnection -Name $primaryGWConnection -ResourceGroupName $primaryResourceGroupName `
       -VirtualNetworkGateway1 $gw1 -VirtualNetworkGateway2 $gw2 -Location $primaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $primaryGWConnection

   # Connect the secondary to primary gateway
   Write-host "Connecting the secondary gateway"

   New-AzVirtualNetworkGatewayConnection -Name $secondaryGWConnection -ResourceGroupName $secondaryResourceGroupName `
       -VirtualNetworkGateway1 $gw2 -VirtualNetworkGateway2 $gw1 -Location $secondaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $secondaryGWConnection
   ```

---

### <a name="create-the-failover-group"></a>De failover-groep maken

Maak de failovergroep voor uw beheerde instanties met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Maak de failovergroep voor uw SQL Managed instances met behulp van de Azure Portal.

1. Selecteer **Azure SQL** in het linkermenu van de [Azure Portal](https://portal.azure.com). Als **Azure SQL** niet in de lijst staat, selecteert u **Alle services** en typt u Azure SQL in het zoekvak. Beschrijving Selecteer de ster naast **Azure SQL** om deze te favoriet en voeg deze toe als een item in de linkernavigatiebalk.
1. Selecteer de primaire beheerde instantie die u wilt toevoegen aan de failovergroep.  
1. Ga onder **instellingen**naar **failover-groepen voor instanties** en kies vervolgens **groep toevoegen** om de pagina **failover-groep voor instanties** te openen.

   ![Een failovergroep toevoegen](./media/auto-failover-group-configure/add-failover-group.png)

1. Typ op de pagina **failover-groep voor exemplaar** de naam van de failovergroep en kies vervolgens het secundaire beheerde exemplaar in de vervolg keuzelijst. Selecteer **maken** om uw failovergroep te maken.

   ![Failovergroep maken](./media/auto-failover-group-configure/create-failover-group.png)

1. Zodra de implementatie van de failovergroep is voltooid, wordt u teruggeleid naar de pagina **failover-groep** .

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Maak de failovergroep voor uw beheerde instanties met behulp van Power shell.

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"

   # Create failover group
   Write-host "Creating the failover group..."
   $failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
        -Location $primaryLocation -ResourceGroupName $primaryResourceGroupName -PrimaryManagedInstanceName $primaryManagedInstance `
        -PartnerRegion $secondaryLocation -PartnerManagedInstanceName $secondaryManagedInstance `
        -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
   $failoverGroup
   ```

---

### <a name="test-failover"></a>Testfailover

Test de failover van de failovergroep met behulp van de Azure Portal of Power shell.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Test de failover van de failovergroep met behulp van de Azure Portal.

1. Navigeer naar uw _secundaire_ beheerde exemplaar binnen de [Azure Portal](https://portal.azure.com) en selecteer **failover-groepen voor instanties** onder instellingen.
1. Controleer welk beheerde exemplaar het primaire is en welk Managed instance het secundaire exemplaar is.
1. Selecteer **failover** en selecteer vervolgens **Ja** in de waarschuwing over TDS-sessies die worden losgekoppeld.

   ![Failover-groep](./media/auto-failover-group-configure/failover-mi-failover-group.png)

1. Controleer welk beheerd-exemplaar het primaire is en welk exemplaar het secundaire is. Als de failover is geslaagd, moeten de twee exemplaren geschakelde rollen hebben.

   ![Beheerde instanties hebben geswitcheerde rollen na een failover](./media/auto-failover-group-configure/mi-switched-after-failover.png)

1. Ga naar het nieuwe _secundaire_ beheerde exemplaar en selecteer eenmalig **failover** opnieuw om de primaire instantie terug te laten werken naar de primaire rol.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Test de failover van de failovergroep met behulp van Power shell.

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"

   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName

   # Failover the primary managed instance to the secondary role
   Write-host "Failing primary over to the secondary location"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to secondary location"

   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName

   # Fail primary managed instance back to primary role
   Write-host "Failing primary back to primary role"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to primary location"

   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   ```

---

## <a name="use-private-link"></a>Private Link gebruiken

Met behulp van een persoonlijke koppeling kunt u een logische server koppelen aan een specifiek privé IP-adres binnen het virtuele netwerk en subnet. 

Ga als volgt te werk om een persoonlijke koppeling met uw failover-groep te gebruiken:

1. Zorg ervoor dat de primaire en secundaire servers zich in een [gekoppelde regio](/azure/best-practices-availability-paired-regions)bevinden. 
1. Maak het virtuele netwerk en het subnet in elke regio voor het hosten van persoonlijke eind punten voor primaire en secundaire servers, zodat ze niet-overlappende IP-adres ruimten hebben. Het primaire adres bereik van het virtuele netwerk van 10.0.0.0/16 en het secundaire virtuele netwerk adres bereik van 10.0.0.1/16 overlapt bijvoorbeeld. Zie voor meer informatie over de adresbereiken van het virtuele netwerk de blog [Azure virtuele netwerken ontwerpen](https://devblogs.microsoft.com/premier-developer/understanding-cidr-notation-when-designing-azure-virtual-networks-and-subnets/).
1. Maak een [persoonlijk eind punt en Azure privé-DNS zone voor de primaire server](../../private-link/create-private-endpoint-portal.md#create-a-private-endpoint). 
1. Maak ook een persoonlijk eind punt voor de secundaire server, maar deze keer u ervoor kiest om dezelfde Privé-DNS zone die is gemaakt voor de primaire server, opnieuw te gebruiken. 
1. Zodra de persoonlijke koppeling tot stand is gebracht, kunt u de failovergroep maken volgens de stappen die eerder in dit artikel zijn beschreven. 


## <a name="locate-listener-endpoint"></a>Listener-eind punt zoeken

Als uw failovergroep is geconfigureerd, werkt u de connection string voor uw toepassing bij naar het listener-eind punt. Hierdoor blijft uw toepassing verbonden met de failover-groep-listener, in plaats van de primaire data base, elastische pool of instantie van de data base. Op die manier hoeft u de connection string niet hand matig bij te werken telkens wanneer er een failover voor de data base wordt uitgevoerd. verkeer wordt doorgestuurd naar de entiteit die momenteel primair is.

Het listener-eind punt bevindt zich in de vorm van `fog-name.database.windows.net` en wordt weer gegeven in de Azure portal bij het weer geven van de failovergroep:

![connection string failover-groep](./media/auto-failover-group-configure/find-failover-group-connection-string.png)

## <a name="remarks"></a>Opmerkingen

- Als u een failovergroep voor een enkele of gegroepeerde Data Base verwijdert, wordt de replicatie niet gestopt en wordt de gerepliceerde data base niet verwijderd. U moet de geo-replicatie hand matig stoppen en de data base van de secundaire server verwijderen als u één of een gegroepeerde Data Base wilt toevoegen aan een failovergroep nadat deze is verwijderd. Als u dit niet doet, treedt er een fout op die vergelijkbaar is bij het `The operation cannot be performed due to multiple errors` toevoegen van de data base aan de failovergroep.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende zelf studies voor gedetailleerde stappen voor het configureren van een failovergroep:

- [Een individuele database aan een failovergroep toevoegen](failover-group-add-single-database-tutorial.md)
- [Een elastische pool aan een failovergroep toevoegen](failover-group-add-elastic-pool-tutorial.md)
- [Een beheerd exemplaar toevoegen aan een failovergroep](../managed-instance/failover-group-add-instance-tutorial.md)

Zie [geo-replicatie](active-geo-replication-overview.md) en [groepen voor automatische failover](auto-failover-group-overview.md)voor een overzicht van Azure SQL database opties voor hoge Beschik baarheid.
