---
title: Cosmos DB migratie opties
description: In dit document worden de verschillende opties beschreven voor het migreren van uw on-premises of Cloud gegevens naar Azure Cosmos DB
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 4de6d4ba019af75b0f6179b2794ddb6c1e35e0c1
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030069"
---
# <a name="options-to-migrate-your-on-premises-or-cloud-data-to-azure-cosmos-db"></a>Opties voor het migreren van uw on-premises of Cloud gegevens naar Azure Cosmos DB

U kunt gegevens uit verschillende gegevens bronnen laden naar Azure Cosmos DB. Omdat Azure Cosmos DB meerdere Api's ondersteunt, kunnen de doelen een van de bestaande Api's zijn. Hier volgen enkele scenario's waarin u gegevens migreert naar Azure Cosmos DB:

* Gegevens verplaatsen van een Azure Cosmos-container naar een andere container in dezelfde data base of een andere data base. s
* Gegevens verplaatsen tussen toegewezen containers naar gedeelde data base-containers.
* Verplaats gegevens van een Azure Cosmos-account die zich in region1 bevindt, naar een ander Azure Cosmos-account in dezelfde of een andere regio.
* Verplaats gegevens van een bron, zoals Azure Blob Storage, een JSON-bestand, Oracle data base, Couch Base, DynamoDB naar Azure Cosmos DB.

Voor het ondersteunen van migratie paden van de verschillende bronnen naar de verschillende Azure Cosmos DB Api's, zijn er meerdere oplossingen die gespecialiseerde verwerking bieden voor elk migratie traject. In dit document vindt u een lijst met de beschik bare oplossingen en worden de voor delen en beperkingen beschreven.

## <a name="factors-affecting-the-choice-of-migration-tool"></a>Factoren die van invloed zijn op de keuze van het migratie hulpprogramma

De volgende factoren bepalen de keuze van het migratie programma:

* **Online versus offline migratie**: veel hulpprogram ma's voor migratie bieden een pad voor het uitvoeren van een eenmalige migratie. Dit betekent dat de toepassingen die toegang hebben tot de Data Base een periode van downtime kunnen ondervinden. Sommige migratie oplossingen bieden een manier om een Livemigratie uit te voeren waarbij een replicatie pijplijn is ingesteld tussen de bron en het doel.

* **Gegevens bron**: de bestaande gegevens kunnen zich in verschillende gegevens bronnen bevinden, zoals Oracle DB2, Datastax Cassanda, Azure SQL database, PostgreSQL, enzovoort. De gegevens kunnen zich ook in een bestaand Azure Cosmos DB account bevinden en het doel van de migratie kan het gegevens model wijzigen of de gegevens in een container opnieuw partitioneren met een andere partitie sleutel.

* **Azure Cosmos DB-API**: voor de SQL-api in azure Cosmos DB bestaan er diverse hulpprogram ma's die zijn ontwikkeld door het Azure Cosmos DB team en die ondersteuning bieden voor de verschillende migratie scenario's. Alle andere Api's hebben hun eigen gespecialiseerde set hulpprogram ma's die door de community zijn ontwikkeld en onderhouden. Omdat Azure Cosmos DB deze Api's ondersteunt op een niveau van een kabel protocol, moeten deze hulpprogram ma's werken zoals bij het migreren van gegevens naar Azure Cosmos DB. Het is echter mogelijk dat er aangepaste verwerking vereist is, omdat dit concept specifiek is voor Azure Cosmos DB.

* **Grootte van gegevens**: de meeste migratie hulpprogramma's werken zeer goed voor kleinere gegevens sets. Wanneer de gegevensset meer dan honderd GB overschrijdt, zijn de opties voor migratie hulpprogramma's beperkt. 

* **Verwachte migratie duur**: migraties kunnen worden geconfigureerd om te worden uitgevoerd met een traag, incrementeel tempo dat minder door Voer verbruikt of de volledige door Voer die is ingericht op de doel Azure Cosmos DB container verbruikt en de migratie in minder tijd kan volt ooien.

## <a name="azure-cosmos-db-sql-api"></a>SQL-API voor Azure Cosmos DB

|Type migratie|Oplossing|Ondersteunde bronnen|Ondersteunde doelen|Overwegingen|
|---------|---------|---------|---------|---------|
|Offline|[Hulpprogramma voor gegevensmigratie](import-data.md)| &bull;JSON/CSV-bestanden<br/>&bull;SQL-API voor Azure Cosmos DB<br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Table Storage<br/>&bull;AWS DynamoDB<br/>&bull;Azure Blob Storage|&bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB tabellen-API<br/>&bull;JSON-bestanden |&bull; Eenvoudig in te stellen en ondersteunt meerdere bronnen. <br/>&bull; Niet geschikt voor grote gegevens sets.|
|Offline|[Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)| &bull;JSON/CSV-bestanden<br/>&bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB-API voor MongoDB<br/>&bull;MongoDB <br/>&bull;SQL Server<br/>&bull;Table Storage<br/>&bull;Azure Blob Storage <br/> <br/>Raadpleeg het [Azure Data Factory](../data-factory/connector-overview.md) -artikel voor andere ondersteunde bronnen.|&bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB-API voor MongoDB<br/>&bull;JSON-bestanden <br/><br/> Zie het [Azure Data Factory](../data-factory/connector-overview.md) -artikel voor andere ondersteunde doelen. |&bull; Eenvoudig in te stellen en ondersteunt meerdere bronnen.<br/>&bull; Maakt gebruik van de bibliotheek van de Azure Cosmos DB bulk-uitvoerder. <br/>&bull; Geschikt voor grote gegevens sets. <br/>&bull; Geen controle punten: Dit betekent dat als er een probleem optreedt tijdens de migratie, het hele migratie proces opnieuw moet worden gestart.<br/>&bull; Geen wachtrij met onbestelbare berichten: Dit betekent dat een aantal foutieve bestanden het gehele migratie proces kan stoppen.|
|Offline|[Azure Cosmos DB Spark-connector](spark-connector.md)|Azure Cosmos DB SQL-API. <br/><br/>U kunt andere bronnen gebruiken met aanvullende connectors van het Spark-ecosysteem.| Azure Cosmos DB SQL-API. <br/><br/>U kunt andere doelen gebruiken met aanvullende connectors van het Spark-ecosysteem.| &bull; Maakt gebruik van de bibliotheek van de Azure Cosmos DB bulk-uitvoerder. <br/>&bull; Geschikt voor grote gegevens sets. <br/>&bull; Vereist een aangepaste Spark-installatie. <br/>&bull; Spark is gevoelig voor inconsistenties in het schema en dit kan een probleem zijn tijdens de migratie. |
|Offline|[Aangepast hulp programma met Cosmos DB bibliotheek voor bulksgewijs uitvoerder](migrate-cosmosdb-data.md)| De bron is afhankelijk van uw aangepaste code | SQL-API voor Azure Cosmos DB| &bull; Voorziet in controle punten voor onbestelbare berichten waardoor migratie tolerantie wordt verhoogd. <br/>&bull; Geschikt voor zeer grote gegevens sets (10 TB +).  <br/>&bull; Aangepaste Setup van dit hulp programma moet worden uitgevoerd als een App Service. |
|Online|[Cosmos DB functions + ChangeFeed-API](change-feed-functions.md)| SQL-API voor Azure Cosmos DB | SQL-API voor Azure Cosmos DB| &bull; Eenvoudig in te stellen. <br/>&bull; Werkt alleen als de bron een Azure Cosmos DB container is. <br/>&bull; Niet geschikt voor grote gegevens sets. <br/>&bull; Er worden geen verwijderingen van de bron container vastgelegd. |
|Online|[Aangepaste migratie service met ChangeFeed](https://github.com/Azure-Samples/azure-cosmosdb-live-data-migrator)| SQL-API voor Azure Cosmos DB | SQL-API voor Azure Cosmos DB| &bull; Biedt voortgangs tracering. <br/>&bull; Werkt alleen als de bron een Azure Cosmos DB container is. <br/>&bull; Kan ook worden gebruikt voor grotere gegevens sets.<br/>&bull; Vereist dat de gebruiker een App Service instelt voor het hosten van de wijzigings processor. <br/>&bull; Er worden geen verwijderingen van de bron container vastgelegd.|
|Online|[Realtimeplatform](cosmosdb-sql-api-migrate-data-striim.md)| &bull;Oracle <br/>&bull;Apache Cassandra<br/><br/> Zie de [realtimeplatform-website](https://www.striim.com/sources-and-targets/) voor andere ondersteunde bronnen. |&bull;SQL-API voor Azure Cosmos DB <br/>&bull; Azure Cosmos DB Cassandra-API<br/><br/> Zie de [realtimeplatform-website](https://www.striim.com/sources-and-targets/) voor andere ondersteunde doelen. | &bull; Werkt met een groot aantal verschillende bronnen, zoals Oracle, DB2, SQL Server.<br/>&bull; Maak eenvoudig ETL-pijp lijnen en biedt een dash board voor bewaking. <br/>&bull; Biedt ondersteuning voor grotere gegevens sets. <br/>&bull; Aangezien dit een hulp programma van derden is, moet het worden aangeschaft via Marketplace en worden geïnstalleerd in de omgeving van de gebruiker.|

## <a name="azure-cosmos-db-mongo-api"></a>Azure Cosmos DB Mongo-API

|Type migratie|Oplossing|Ondersteunde bronnen|Ondersteunde doelen|Overwegingen|
|---------|---------|---------|---------|---------|
|Online|[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB|Azure Cosmos DB-API voor MongoDB |&bull; Maakt gebruik van de bibliotheek van de Azure Cosmos DB bulk-uitvoerder. <br/>&bull; Geschikt voor grote gegevens sets en zorgt ervoor dat Live wijzigingen worden gerepliceerd. <br/>&bull; Werkt alleen met andere MongoDB-bronnen.|
|Offline|[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB| Azure Cosmos DB-API voor MongoDB| &bull; Maakt gebruik van de bibliotheek van de Azure Cosmos DB bulk-uitvoerder. <br/>&bull; Geschikt voor grote gegevens sets en zorgt ervoor dat Live wijzigingen worden gerepliceerd. <br/>&bull; Werkt alleen met andere MongoDB-bronnen.|
|Offline|[Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)| &bull;JSON/CSV-bestanden<br/>&bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB-API voor MongoDB <br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Table Storage<br/>&bull;Azure Blob Storage <br/><br/> Raadpleeg het [Azure Data Factory](../data-factory/connector-overview.md) -artikel voor andere ondersteunde bronnen. | &bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB-API voor MongoDB <br/>&bull; JSON-bestanden <br/><br/> Zie het [Azure Data Factory](../data-factory/connector-overview.md) -artikel voor andere ondersteunde doelen.| &bull; Eenvoudig in te stellen en ondersteunt meerdere bronnen. <br/>&bull; Maakt gebruik van de bibliotheek van de Azure Cosmos DB bulk-uitvoerder. <br/>&bull; Geschikt voor grote gegevens sets. <br/>&bull; Als er geen controle punt is, betekent dit dat er tijdens de migratie een probleem is met het opnieuw opstarten van het hele migratie proces.<br/>&bull; Het ontbreken van een wachtrij met onbestelbare berichten zou betekenen dat enkele foutieve bestanden het gehele migratie proces zouden kunnen stoppen. <br/>&bull; Aangepaste code nodig om de Lees doorvoer voor bepaalde gegevens bronnen te verg Roten.|
|Offline|[Bestaande Mongo-Hulpprogram Ma's (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|MongoDB | Azure Cosmos DB-API voor MongoDB| &bull; Eenvoudig in te stellen en te integreren. <br/>&bull; Aangepaste verwerking vereist voor gashendel.|

## <a name="azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra-API

|Type migratie|Oplossing|Ondersteunde bronnen|Ondersteunde doelen|Overwegingen|
|---------|---------|---------|---------|---------|
|Offline|[cqlsh kopiëren opdracht](cassandra-import-data.md#migrate-data-using-cqlsh-copy-command)|CSV-bestanden | Azure Cosmos DB Cassandra-API| &bull; Eenvoudig in te stellen. <br/>&bull; Niet geschikt voor grote gegevens sets. <br/>&bull; Werkt alleen wanneer de bron een Cassandra-tabel is.|
|Offline|[Tabel kopiëren met Spark](cassandra-import-data.md#migrate-data-using-spark) | &bull;Apache Cassandra<br/>&bull;Azure Cosmos DB Cassandra-API| Azure Cosmos DB Cassandra-API | &bull; Kan gebruik maken van Spark-mogelijkheden voor het parallelliseren van trans formatie en opname. <br/>&bull; Moet worden geconfigureerd met een aangepast beleid voor opnieuw proberen om beperkingen te verwerken.|
|Online|[Realtimeplatform (uit Oracle DB/Apache Cassandra)](cosmosdb-cassandra-api-migrate-data-striim.md)| &bull;Oracle<br/>&bull;Apache Cassandra<br/><br/> Zie de [realtimeplatform-website](https://www.striim.com/sources-and-targets/) voor andere ondersteunde bronnen.|&bull;SQL-API voor Azure Cosmos DB<br/>&bull;Azure Cosmos DB Cassandra-API <br/><br/> Zie de [realtimeplatform-website](https://www.striim.com/sources-and-targets/) voor andere ondersteunde doelen.| &bull; Werkt met een groot aantal verschillende bronnen, zoals Oracle, DB2, SQL Server. <br/>&bull; Maak eenvoudig ETL-pijp lijnen en biedt een dash board voor bewaking. <br/>&bull; Biedt ondersteuning voor grotere gegevens sets. <br/>&bull; Aangezien dit een hulp programma van derden is, moet het worden aangeschaft via Marketplace en worden geïnstalleerd in de omgeving van de gebruiker.|
|Online|[Blitzz (uit Oracle DB/Apache Cassandra)](oracle-migrate-cosmos-db-blitzz.md)|&bull;Oracle<br/>&bull;Apache Cassandra<br/><br/>Zie de [Blitzz-website](https://www.blitzz.io/) voor andere ondersteunde bronnen. |Azure Cosmos DB Cassandra-API. <br/><br/>Zie de [Blitzz-website](https://www.blitzz.io/) voor andere ondersteunde doelen. | &bull; Biedt ondersteuning voor grotere gegevens sets. <br/>&bull; Aangezien dit een hulp programma van derden is, moet het worden aangeschaft via Marketplace en worden geïnstalleerd in de omgeving van de gebruiker.|

## <a name="other-apis"></a>Overige API's

Voor andere Api's dan de SQL-API, Mongo-API en het Cassandra-API, zijn er verschillende hulpprogram ma's die worden ondersteund door elk van de bestaande ecosystemen van de API. 

**Table-API** 

* [Hulpprogramma voor gegevensmigratie](table-import.md#data-migration-tool)
* [AzCopy](table-import.md#migrate-data-by-using-azcopy)

**Gremlin-API**

* [Bibliotheek voor bulksgewijs uitvoeren grafiek](bulk-executor-graph-dotnet.md)
* [Gremlin Spark](https://github.com/Azure/azure-cosmosdb-spark/blob/2.4/samples/graphframes/main.scala) 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het uitproberen van de voorbeeld toepassingen die de bulk-uitvoerder bibliotheek in [.net](bulk-executor-dot-net.md) en [Java](bulk-executor-java.md)gebruiken. 
* De bibliotheek bulk-uitvoerder is geïntegreerd in de Cosmos DB Spark-connector, Zie [Azure Cosmos DB artikel Spark-connector](spark-connector.md) voor meer informatie.  
* Neem contact op met het product team van Azure Cosmos DB door een ondersteunings ticket te openen onder het probleem type ' algemeen advies ' en ' grote (TB +) ' subtype ' voor meer informatie over grootschalige migraties.
