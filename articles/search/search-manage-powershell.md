---
title: Power shell-scripts met de module AZ. Search
titleSuffix: Azure Cognitive Search
description: Een Azure Cognitive Search-service maken en configureren met Power shell. U kunt een service omhoog of omlaag schalen, beheer-en query-API-sleutels beheren en query's uitvoeren op systeem gegevens.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 9f189d1889f3ca3a3aa3234432452b1b3d696c04
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935089"
---
# <a name="manage-your-azure-cognitive-search-service-with-powershell"></a>Uw Azure Cognitive Search-service beheren met Power shell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST API](/rest/api/searchmanagement/)
> * [.NET SDK](/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

U kunt Power shell-cmdlets en-scripts uitvoeren op Windows, Linux of in [Azure Cloud shell](../cloud-shell/overview.md) om Azure Cognitive Search te maken en te configureren. De module **AZ. Search** breidt [Azure PowerShell](/powershell/) uit met volledige pariteit van de [rest api's voor zoek beheer](/rest/api/searchmanagement) en de mogelijkheid om de volgende taken uit te voeren:

> [!div class="checklist"]
> * [Zoek Services in een abonnement weer geven](#list-search-services)
> * [Service-informatie retour neren](#get-search-service-information)
> * [Een service maken of verwijderen](#create-or-delete-a-service)
> * [Beheer-API opnieuw genereren-sleutels](#regenerate-admin-keys)
> * [Query-API-sleutels maken of verwijderen](#create-or-delete-query-keys)
> * [Omhoog of omlaag schalen met replica's en partities](#scale-replicas-and-partitions)

Af en toe worden vragen gesteld over taken die *niet* voor komen in de bovenstaande lijst. Op dit moment kunt u niet de module **AZ. Search** of het beheer rest API gebruiken om een server naam,-regio of-laag te wijzigen. Toegewezen resources worden toegewezen wanneer een service wordt gemaakt. Voor het wijzigen van de onderliggende hardware (locatie of type knoop punt) is een nieuwe service vereist. Op dezelfde manier zijn er geen hulpprogram ma's of Api's voor het overdragen van inhoud, zoals een index, van de ene service naar de andere.

Binnen een service is het maken en beheren van inhoud via [Search Service rest API](/rest/api/searchservice/) of [.NET SDK](/dotnet/api/?term=microsoft.azure.search). Hoewel er geen speciale Power shell-opdrachten zijn voor inhoud, kunt u Power shell-script schrijven dat REST-of .NET-Api's aanroept om indexen te maken en te laden.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Versies controleren en modules laden

De voor beelden in dit artikel zijn interactief en vereisen verhoogde machtigingen. Azure PowerShell (de **AZ** -module) moet zijn geïnstalleerd. Zie [Azure PowerShell installeren](/powershell/azure/) voor meer informatie.

### <a name="powershell-version-check-51-or-later"></a>Controle van Power shell-versie (5,1 of hoger)

Lokale Power shell moet 5,1 of hoger zijn, op elk ondersteund besturings systeem.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Azure PowerShell laden

Als u niet zeker weet of **AZ** is geïnstalleerd, voert u de volgende opdracht uit als een verificatie stap. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Sommige systemen laden modules niet automatisch. Als er een fout optreedt bij de vorige opdracht, laadt u de module en als dat niet het geval is, gaat u terug naar de installatie-instructies om te zien of u een stap hebt gemist.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Verbinding maken met Azure met een browser aanmeldings token

U kunt uw aanmeldings referenties voor uw portal gebruiken om verbinding te maken met een abonnement in Power shell. U kunt ook [niet-interactief verifiëren met een Service-Principal](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

```azurepowershell-interactive
Connect-AzAccount
```

Als u meerdere Azure-abonnementen hebt, stelt u uw Azure-abonnement in. Voer deze opdracht uit om een lijst met uw huidige abonnementen weer te geven.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Voer de volgende opdracht uit om het abonnement op te geven. In het volgende voor beeld is de naam van het abonnement `ContosoSubscription` .

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-services-in-a-subscription"></a>Services in een abonnement weer geven

De volgende opdrachten zijn afkomstig van [**AZ. resources**](/powershell/module/az.resources/?view=azps-1.4.0#resources), die informatie over bestaande resources en services retour neren die al zijn ingericht in uw abonnement. Als u niet weet hoeveel Zoek Services al zijn gemaakt, retour neren deze opdrachten die informatie en bespaart u een reis naar de portal.

Met de eerste opdracht worden alle zoek services geretourneerd.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

Ga in de lijst met Services naar informatie over een specifieke resource.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

De resultaten moeten er ongeveer uitzien als in de volgende uitvoer.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Importeren AZ. Search

Opdrachten van [**AZ. Search**](/powershell/module/az.search/?view=azps-1.4.0#search) zijn pas beschikbaar als u de module laadt.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Alle opdrachten van AZ. Search weer geven

Als een verificatie stap retourneert een lijst met opdrachten die in de module zijn opgenomen.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

De resultaten moeten er ongeveer uitzien als in de volgende uitvoer.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Informatie over de zoek service ophalen

Nadat **AZ. Search** is geïmporteerd en u de resource groep met uw zoek service kent, voert u [Get-AzSearchService](/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) uit om de service definitie te retour neren, inclusief naam, regio, laag en aantal partities.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

De resultaten moeten er ongeveer uitzien als in de volgende uitvoer.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Een service maken of verwijderen

[**New-AzSearchService**](/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) wordt gebruikt om [een nieuwe zoek service te maken](search-create-service-portal.md).

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
De resultaten moeten er ongeveer uitzien als in de volgende uitvoer.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Beheer sleutels opnieuw genereren

[**New-AzSearchAdminKey**](/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) wordt gebruikt om de beheer- [API-sleutels](search-security-api-keys.md)te herstellen. Voor geverifieerde toegang worden twee beheer sleutels gemaakt met elke service. Sleutels zijn vereist voor elke aanvraag. Beide beheerders sleutels zijn functioneel gelijkwaardig, waarbij volledige schrijf toegang wordt verleend aan een zoek service met de mogelijkheid om gegevens op te halen of om een wille keurig object te maken en te verwijderen. Er bestaan twee sleutels, zodat u deze kunt gebruiken terwijl u de andere vervangt. 

U kunt slechts één keer opnieuw genereren, opgegeven als de `primary` of- `secondary` sleutel. Voor een ononderbroken service moet u alle client code bijwerken om een secundaire sleutel te gebruiken terwijl u de primaire sleutel rolt. Vermijd het wijzigen van de sleutels tijdens de vlucht.

Zoals u zou verwachten, zullen aanvragen die gebruikmaken van de oude sleutel, mislukken als u sleutels opnieuw genereert zonder de client code bij te werken. Door alle nieuwe sleutels te genereren, wordt uw service niet permanent vergrendeld en hebt u nog steeds toegang tot de service via de portal. Nadat u de primaire en secundaire sleutels opnieuw hebt gegenereerd, kunt u de client code bijwerken voor het gebruik van de nieuwe sleutels en bewerkingen worden hervat.

De waarden voor de API-sleutels worden gegenereerd door de service. U kunt geen aangepaste sleutel opgeven voor Azure Cognitive Search te gebruiken. Op dezelfde manier is er geen door de gebruiker gedefinieerde naam voor beheer-API-sleutels. Verwijzingen naar de sleutel zijn vaste teken reeksen, ofwel `primary` of `secondary` . 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

De resultaten moeten er ongeveer uitzien als in de volgende uitvoer. Beide sleutels worden geretourneerd, zelfs als u slechts één per keer wijzigt.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Query sleutels maken of verwijderen

[**New-AzSearchQueryKey**](/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) wordt gebruikt voor het maken van query- [API-sleutels](search-security-api-keys.md) voor alleen-lezen toegang vanuit client-apps naar een Azure Cognitive search-index. Query sleutels worden gebruikt om te verifiëren bij een specifieke index om Zoek resultaten op te halen. Query sleutels geven geen alleen-lezen toegang tot andere items in de service, zoals een index, gegevens bron of Indexeer functie.

U kunt geen sleutel opgeven voor Azure Cognitive Search te gebruiken. De API-sleutels worden gegenereerd door de service.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Replica's en partities schalen

[**Set-AzSearchService**](/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) wordt gebruikt om [replica's en partities te verg Roten of verkleinen](search-capacity-planning.md) om factureer bare resources binnen uw service opnieuw aan te passen. Het verg Roten van replica's of partities wordt toegevoegd aan uw factuur, met zowel vaste als variabele kosten. Als u een tijdelijke behoefte hebt aan extra verwerkings kracht, kunt u replica's en partities verg Roten om de werk belasting te verwerken. Het bewakings gebied op de overzichts portal pagina bevat tegels op query latentie, query's per seconde en beperking, om aan te geven of de huidige capaciteit voldoende is.

Het kan even duren om een item toe te voegen of te verwijderen. Aanpassingen van de capaciteit worden op de achtergrond uitgevoerd, waardoor bestaande workloads kunnen worden voortgezet. Er wordt extra capaciteit gebruikt voor inkomende aanvragen zodra deze klaar zijn, zonder dat er aanvullende configuratie is vereist. 

Het verwijderen van capaciteit kan storend zijn. Het wordt aanbevolen om te voor komen dat alle indexerings-en indexerings taken worden gestopt voordat de capaciteit wordt verminderd. Als dat niet mogelijk is, kunt u overwegen om de capaciteit incrementeel, één replica en partitie tegelijk te verlagen, totdat de nieuwe doel niveaus zijn bereikt.

Wanneer u de opdracht hebt verzonden, is het niet mogelijk om deze halverwege te beëindigen. U moet wachten tot de opdracht is voltooid voordat u de aantallen wijzigt.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

De resultaten moeten er ongeveer uitzien als in de volgende uitvoer.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="next-steps"></a>Volgende stappen

Bouw een [index](search-what-is-an-index.md), [Zoek een query op een index](search-query-overview.md) met behulp van de portal, rest-API'S of de .NET SDK.

* [Een Azure Cognitive Search-index maken in de Azure Portal](search-get-started-portal.md)
* [Een Indexeer functie instellen voor het laden van gegevens uit andere services](search-indexer-overview.md)
* [Query's uitvoeren op een Azure Cognitive Search-index met behulp van Search Explorer in de Azure Portal](search-explorer.md)
* [Azure Cognitive Search gebruiken in .NET](search-howto-dotnet-sdk.md)