---
title: Prestaties afstemmen door resultatensets op te slaan in de cache
description: Overzicht van de cache functie voor resultaat sets voor Synapse SQL-pool in azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.custom: azure-synapse
ms.openlocfilehash: aeeca38afb82e2dcd86e111d1ae5dcb2e7499f42
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91362262"
---
# <a name="performance-tuning-with-result-set-caching"></a>Prestaties afstemmen door resultatensets op te slaan in de cache

Als de cache voor het instellen van de resultatenset is ingeschakeld, slaat Synapse SQL de query resultaten in de gebruikers database automatisch op voor herhaaldelijk gebruik.  Op deze manier kunnen volgende query's worden uitgevoerd om resultaten rechtstreeks uit de persistente cache te halen, zodat herberekening niet nodig is.   Caching van resultaten sets verbetert de query prestaties en vermindert het gebruik van reken resources.  Daarnaast gebruiken query's die in de cache opgeslagen resultaten zijn ingesteld geen gelijktijdigheids sleuven en worden dus niet met bestaande gelijktijdigheids limieten geteld. Voor de beveiliging hebben gebruikers alleen toegang tot de resultaten in de cache als ze dezelfde machtigingen voor gegevens toegang hebben als de gebruikers die de in de cache opgeslagen resultaten maken.  

## <a name="key-commands"></a>Belangrijkste opdrachten

[IN-en uitschakelen van de cache voor de resultatenset voor een gebruikers database](/sql/t-sql/statements/alter-database-transact-sql-set-options?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[IN-en uitschakelen van de cache voor de resultatenset voor een sessie](/sql/t-sql/statements/set-result-set-caching-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[De grootte van de resultatenset in de cache controleren](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)  

[De cache opschonen](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="whats-not-cached"></a>Wat wordt niet in cache opgeslagen  

Zodra de caching van de resultatenset voor een Data Base is ingeschakeld, worden de resultaten in de cache opgeslagen voor alle query's totdat de cache vol is, met uitzonde ring van de volgende query's:

- Query's die gebruikmaken van niet-deterministische functies, zoals DateTime. Now ()
- Query's met door de gebruiker gedefinieerde functies
- Query's die gebruikmaken van tabellen met beveiliging op rijniveau of beveiliging op kolom niveau ingeschakeld
- Query's die gegevens retour neren met een Rijgrootte groter dan 64 KB
- Query's waarbij grote gegevens worden geretourneerd (>10GB) 

> [!IMPORTANT]
> De bewerkingen voor het maken van een cache met resultaten en het ophalen van gegevens uit de cache vindt plaats op het knoop punt beheer van een exemplaar van een Synapse SQL-groep.
> Wanneer het in cache plaatsen van de resultatenset is ingeschakeld, kan het uitvoeren van query's die een grote resultatenset retour neren (bijvoorbeeld >1GB), hoge vertraging veroorzaken op het beheer knooppunt en de algehele query respons op het exemplaar vertragen.  Deze query's worden vaak gebruikt tijdens het verkennen van gegevens of ETL-bewerkingen. Om te voor komen dat het controle knooppunt stressert en het prestatie probleem veroorzaakt, moeten gebruikers de resultatenset cache uitschakelen in de Data Base voordat deze typen query's worden uitgevoerd.  

Voer deze query uit voor de tijd die nodig is voor de cache bewerkingen van de resultatenset voor een query:

```sql
SELECT step_index, operation_type, location_type, status, total_elapsed_time, command
FROM sys.dm_pdw_request_steps
WHERE request_id  = <'request_id'>;
```

Hier volgt een voorbeeld uitvoer voor een query die wordt uitgevoerd met de cache voor de resultatenset uitgeschakeld.

![Scherm opname toont query resultaten, inclusief locatie type en opdracht.](./media/performance-tuning-result-set-caching/query-steps-with-rsc-disabled.png)

Hier volgt een voorbeeld uitvoer voor een query die wordt uitgevoerd met de cache voor de resultatenset ingeschakeld.

![Scherm afbeelding toont query resultaten met de geselecteerde opdracht * van [D W ResultCache D b] dot d b o aangeroepen.](./media/performance-tuning-result-set-caching/query-steps-with-rsc-enabled.png)

## <a name="when-cached-results-are-used"></a>Wanneer de resultaten in de cache worden gebruikt

De resultatenset in de cache wordt opnieuw gebruikt voor een query als aan alle volgende vereisten wordt voldaan:

- De gebruiker die de query uitvoert, heeft toegang tot alle tabellen waarnaar wordt verwezen in de query.
- Er is een exacte overeenkomst tussen de nieuwe query en de vorige query die de cache met resultaten sets heeft gegenereerd.
- Er zijn geen gegevens of schema wijzigingen in de tabellen waarin de resultatenset in de cache is gegenereerd.

Voer deze opdracht uit om te controleren of een query is uitgevoerd met een treffer voor de resultaten cache of ontbreekt. De kolom result_cache_hit retourneert 1 voor cache treffer, 0 voor Missing in cache en negatieve waarden om redenen waarom het instellen van een cache voor de resultatenset niet is gebruikt. Raadpleeg [sys. dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) voor meer informatie.

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>In cache opgeslagen resultaten beheren

De maximale grootte van de cache voor de resultatenset is 1 TB per data base.  De resultaten in de cache worden automatisch ongeldig gemaakt wanneer de onderliggende query gegevens veranderen.  

Het verwijderen van de cache wordt door Synapse SQL automatisch na deze planning beheerd:

- Elke 48 uur als de resultatenset niet is gebruikt of ongeldig is gemaakt.
- Wanneer de cache voor de resultatenset de maximum grootte nadert.

Gebruikers kunnen de volledige cache voor de resultatenset hand matig legen door een van de volgende opties te gebruiken:

- De cache functie voor het instellen van resultaten voor de data base uitschakelen
- DBCC DROPRESULTSETCACHE uitvoeren terwijl er verbinding is gemaakt met de data base

Wanneer een Data Base wordt onderbroken, wordt de resultatenset in de cache niet leeg gelaten.  

## <a name="next-steps"></a>Volgende stappen

Zie [ontwikkelings overzicht](sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkel aars.
