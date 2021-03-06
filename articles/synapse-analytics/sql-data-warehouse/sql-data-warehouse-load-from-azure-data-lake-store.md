---
title: Zelf studie gegevens laden van Azure Data Lake Storage
description: Gebruik de instructie COPY om gegevens te laden uit Azure Data Lake Storage voor Synapse SQL.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 06/07/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: c634b4f7ac3aa1fe83e6ab3f863e998b8d25232c
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533934"
---
# <a name="load-data-from-azure-data-lake-storage-for-synapse-sql"></a>Gegevens laden uit Azure Data Lake Storage voor Synapse SQL

In deze hand leiding wordt beschreven hoe u de [instructie Copy](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) kunt gebruiken om gegevens uit Azure data Lake Storage te laden. Ga naar de volgende documentatie voor snelle voor beelden over het gebruik van de instructie COPY over alle verificatie methoden: [gegevens veilig laden met Synapse SQL](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples).

> [!NOTE]  
> Als u feedback wilt geven of problemen wilt melden voor de instructie COPY, stuurt u een e-mail naar de volgende distributie lijst: sqldwcopypreview@service.microsoft.com .
>
> [!div class="checklist"]
>
> * Maak de doel tabel om gegevens van Azure Data Lake Storage te laden.
> * Maak de instructie COPY om gegevens naar het Data Warehouse te laden.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

Download en installeer voordat u met deze zelfstudie begint de nieuwste versie van [SSMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SQL Server Management Studio).

U hebt het volgende nodig om deze zelfstudie te volgen:

* Een SQL-groep. Zie [een SQL-groep en query gegevens maken](create-data-warehouse-portal.md).
* Een Data Lake Storage-account. Zie [aan de slag met Azure data Lake Storage](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Voor dit opslag account moet u een van de volgende referenties configureren of opgeven om te laden: een sleutel voor een opslag account, een SAS-sleutel (Shared Access Signature), een Azure Directory-toepassings gebruiker of een AAD-gebruiker die de juiste Azure-rol heeft voor het opslag account.

## <a name="create-the-target-table"></a>De doeltabel maken

Maak verbinding met uw SQL-groep en wijs de doel tabel aan waarnaar u wilt laden. In dit voor beeld maken we een tabel met product dimensies.

```sql
-- A: Create the target table
-- DimProduct
CREATE TABLE [dbo].[DimProduct]
(
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    DISTRIBUTION = HASH([ProductKey]),
    CLUSTERED COLUMNSTORE INDEX
    --HEAP
);
```


## <a name="create-the-copy-statement"></a>De instructie COPY maken

Maak verbinding met uw SQL-groep en voer de instructie COPY uit. Voor een volledige lijst met voor beelden gaat u naar de volgende documentatie: [gegevens veilig laden met Synapse SQL](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples).

```sql
-- B: Create and execute the COPY statement

COPY INTO [dbo].[DimProduct] 
--The column list allows you map, omit, or reorder input file columns to target table columns. 
--You can also specify the default value when there is a NULL value in the file.
--When the column list is not specified, columns will be mapped based on source and target ordinality
(
    ProductKey default -1 1,
    ProductLabel default 'myStringDefaultWhenNull' 2,
    ProductName default 'myStringDefaultWhenNull' 3
)
--The storage account location where you data is staged
FROM 'https://storageaccount.blob.core.windows.net/container/directory/'
WITH 
(
   --CREDENTIAL: Specifies the authentication method and credential access your storage account
   CREDENTIAL (IDENTITY = '', SECRET = '')
   --FILE_TYPE: Specifies the file type in your storage account location
   FILE_TYPE = 'CSV',
   --FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text (CSV) file
   FIELDTERMINATOR = '|',
   --ROWTERMINATOR: Marks the end of a record in the file
   ROWTERMINATOR = '0x0A',
   --FIELDQUOTE: Specifies the delimiter for data of type string in a delimited text (CSV) file
   FIELDQUOTE = '',
   ENCODING = 'UTF8',
   DATEFORMAT = 'ymd',
   --MAXERRORS: Maximum number of reject rows allowed in the load before the COPY operation is canceled
   MAXERRORS = 10,
   --ERRORFILE: Specifies the directory where the rejected rows and the corresponding error reason should be written
   ERRORFILE = '/errorsfolder',
) OPTION (LABEL = 'COPY: ADLS tutorial');
```

## <a name="optimize-columnstore-compression"></a>Column Store-compressie optimaliseren

Tabellen worden standaard gedefinieerd als een geclusterde column store-index. Nadat het laden is voltooid, zijn sommige gegevens rijen mogelijk niet gecomprimeerd naar de column Store.  Er zijn verschillende redenen waarom dit kan gebeuren. Zie [Column Store-indexen beheren](sql-data-warehouse-tables-index.md)voor meer informatie.

Als u de query prestaties en column Store-compressie wilt optimaliseren na een laad opdracht, bouwt u de tabel opnieuw op om ervoor te zorgen dat de column store-index alle rijen kan comprimeren.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>Statistieken optimaliseren

Het is aan te raden om statistieken voor één kolom direct na een belasting te maken. Er zijn enkele keuzes voor statistieken. Als u bijvoorbeeld statistieken voor één kolom maakt voor elke kolom, kan het lang duren om alle statistieken opnieuw samen te stellen. Als u weet dat bepaalde kolommen zich niet in query predikaten bevinden, kunt u het maken van statistieken voor die kolommen overs Laan.

Als u besluit om met één kolom statistieken te maken voor elke kolom van elke tabel, kunt u het voor beeld van de opgeslagen procedure code `prc_sqldw_create_stats` in het [statistiek](sql-data-warehouse-tables-statistics.md) artikel gebruiken.

Het volgende voor beeld is een goed uitgangs punt voor het maken van statistieken. Er worden statistieken voor één kolom gemaakt voor elke kolom in de dimensie tabel en voor elke join-kolom in de feiten tabellen. U kunt later altijd statistieken met één of meerdere kolommen toevoegen aan andere feiten tabel kolommen.

## <a name="achievement-unlocked"></a>Prestaties vergren delen.

U hebt gegevens geladen in uw data warehouse. Helemaal goed!

## <a name="next-steps"></a>Volgende stappen
Het laden van gegevens is de eerste stap bij het ontwikkelen van een Data Warehouse-oplossing met behulp van Azure Synapse Analytics. Bekijk onze ontwikkelings bronnen.

> [!div class="nextstepaction"]
> [Meer informatie over het ontwikkelen van tabellen voor gegevens opslag](sql-data-warehouse-tables-overview.md)

Raadpleeg de volgende documentatie voor meer informatie over het laden van voor beelden en verwijzingen:
- [Naslag documentatie over het kopiëren van instructies](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#syntax)
- [Voor beelden kopiëren voor elke authenticatie methode](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [Quick start voor één tabel kopiëren](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
