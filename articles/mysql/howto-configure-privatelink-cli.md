---
title: Persoonlijke koppeling-Azure CLI-Azure Database for MySQL
description: Meer informatie over het configureren van een persoonlijke koppeling voor Azure Database for MySQL van Azure CLI
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 01/09/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8948ed51a9cda3d308046f3b85b6ce338462e82c
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2020
ms.locfileid: "87826157"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mysql-using-cli"></a>Een persoonlijke koppeling voor Azure Database for MySQL maken en beheren met CLI

Een privé-eindpunt is de fundamentele bouwsteen voor een Private Link in Azure. Het biedt Azure-resources, zoals virtuele machines, de mogelijkheid om Private Link-resources te gebruiken om privé met elkaar communiceren. In dit artikel leert u hoe u de Azure CLI gebruikt om een virtuele machine te maken in een Azure-Virtual Network en een Azure Database for MySQL-server met een persoonlijk Azure-eind punt.

> [!NOTE]
> De functie voor persoonlijke koppelingen is alleen beschikbaar voor Azure Database for MySQL servers in de prijs Categorieën Algemeen of geoptimaliseerd voor geheugen. Zorg ervoor dat de database server zich in een van deze prijs categorieën bevindt.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om Azure CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) voor installatie- of upgrade-informatie.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u een resource kunt maken, moet u een resourcegroep maken die het Virtual Network host. Maak een resourcegroep maken met [az group create](/cli/azure/group). In dit voor beeld wordt een resource groep met de naam *myResourceGroup* gemaakt op de locatie *Europa West* :

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Een Virtual Network maken
Maak een Virtual Network met [az network vnet create](/cli/azure/network/vnet). In dit voorbeeld wordt een standaard Virtual Network gemaakt met de naam *myVirtualNetwork* met één subnet genaamd *mySubnet*:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Beleid voor privé-eindpunten van subnet uitschakelen 
Azure implementeert resources in een subnet binnen een virtueel netwerk, dus u moet het subnet maken of bijwerken om beleid voor privé-eindpunt [netwerk](../private-link/disable-private-endpoint-network-policy.md)uit te scha kelen. Update een subnet-configuratie met de naam *mySubnet* met [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>De virtuele machine maken 
Maak een VM met az vm create. Wanneer u hierom wordt gevraagd, geeft u een wachtwoord op dat moet worden gebruikt als de aanmeldingsreferenties voor de VM. In het volgende voorbeeld wordt een VM gemaakt met de naam *myVm*: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
Noteer het open bare IP-adres van de virtuele machine. U gebruikt dit adres om in de volgende stap verbinding te maken met de virtuele machine via internet.

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken 
Maak een Azure Database for MySQL met de opdracht AZ mysql server Create. Houd er rekening mee dat de naam van uw MySQL-server uniek moet zijn in azure, dus Vervang de waarde van de tijdelijke aanduiding tussen vier Kante haken door uw eigen unieke waarde: 

```azurecli-interactive
# Create a server in the resource group 
az mysql server create \
--name mydemoserver \
--resource-group myResourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

> [!NOTE]
> In sommige gevallen bevinden de Azure Database for MySQL en het VNet-subnet zich in verschillende abonnementen. In deze gevallen moet u ervoor zorgen dat u de volgende configuraties hebt:
> - Zorg ervoor dat voor beide abonnementen de resource provider **micro soft. DBforMySQL** is geregistreerd. Raadpleeg [Resource-Manager-registratie][resource-manager-portal] voor meer informatie

## <a name="create-the-private-endpoint"></a>Privé-eindpunt maken 
Maak een persoonlijk eind punt voor de MySQL-server in uw Virtual Network: 

```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id $(az resource show -g myResourcegroup -n mydemoserver --resource-type "Microsoft.DBforMySQL/servers" --query "id") \    
    --group-id mysqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Privé-DNS-zone configureren 
Maak een Privé-DNS zone voor het MySQL-Server domein en maak een koppelings koppeling met de Virtual Network. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.mysql.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.mysql.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for MySQL name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.mysql.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.mysql.database.azure.com --resource-group myResourceGroup -a <Private IP Address>
```

> [!NOTE] 
> De FQDN in de DNS-instelling van de klant wordt niet omgezet naar het geconfigureerde particuliere IP-adres. U moet een DNS-zone voor de geconfigureerde FQDN instellen, zoals [hier](../dns/dns-operations-recordsets-portal.md)wordt weer gegeven.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Maak als volgt verbinding met de VM *myVm* van Internet:

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

1. Open Power shell in de Extern bureaublad van *myVM*.

2. Voer  `nslookup mydemomysqlserver.privatelink.mysql.database.azure.com` in. 

    U ontvangt een bericht dat er ongeveer als volgt uitziet:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemomysqlserver.privatelink.mysql.database.azure.com
    Address:  10.1.3.4
    ```

3. Test de verbinding van de particuliere verbinding voor de MySQL-server met behulp van een beschik bare client. In het onderstaande voor beeld heb ik [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) gebruikt om de bewerking uit te voeren.


4. In **nieuwe verbinding**voert u de volgende gegevens in of selecteert u deze:

    | Instelling | Waarde |
    | ------- | ----- |
    | Verbindingsnaam| Selecteer de verbindings naam van uw keuze.|
    | Hostnaam | *Mydemoserver.privatelink.mysql.database.Azure.com* selecteren |
    | Gebruikersnaam | Voer de gebruikers naam in, *username@servername* die wordt opgegeven tijdens het maken van de mysql-server. |
    | Wachtwoord | Voer een wacht woord in dat is opgegeven tijdens het maken van de MySQL-server. |
    ||

5. Selecteer Verbinding maken.

6. Blader in het menu aan de linkerkant door databases.

7. Eventueel Gegevens uit de MySQL-data base maken of er query's op uitvoeren.

8. Sluit de verbinding met extern bureau blad met myVm.

## <a name="clean-up-resources"></a>Resources opschonen 
U kunt az group delete gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, als deze niet meer nodig zijn: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Wat is Azure persoonlijk eind punt](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) ?

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md
