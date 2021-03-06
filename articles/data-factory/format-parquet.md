---
title: De indeling Parquet in Azure Data Factory
description: In dit onderwerp wordt beschreven hoe u kunt omgaan met de Parquet-indeling in Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/27/2020
ms.author: jingwang
ms.openlocfilehash: 13a0966413519e56199cfce150d9dd7973d634ec
ms.sourcegitcommit: ada9a4a0f9d5dbb71fc397b60dc66c22cf94a08d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2020
ms.locfileid: "91404716"
---
# <a name="parquet-format-in-azure-data-factory"></a>De indeling Parquet in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Volg dit artikel als u **de Parquet-bestanden wilt parseren of de gegevens wilt schrijven naar de Parquet-indeling**. 

De Parquet-indeling wordt ondersteund voor de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestands systeem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)en [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel [gegevens sets](concepts-datasets-linked-services.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevens sets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Parquet-gegevensset.

| Eigenschap         | Beschrijving                                                  | Vereist |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | De eigenschap type van de DataSet moet worden ingesteld op **Parquet**. | Yes      |
| location         | Locatie-instellingen van bestand (en). Elke connector op basis van bestanden heeft een eigen locatie type en ondersteunde eigenschappen onder `location` . **Zie de sectie Details in connector artikel-> eigenschappen van gegevensset**. | Yes      |
| compressionCodec | De compressie-codec die moet worden gebruikt bij het schrijven naar Parquet-bestanden. Bij het lezen van Parquet-bestanden, bepalen gegevens fabrieken de compressie-codec automatisch op basis van de meta gegevens van het bestand.<br>Ondersteunde typen zijn '**none**', '**gzip**', '**Snappy**' (standaard) en '**lzo**'. Houd er rekening mee dat de activiteit die momenteel wordt gekopieerd, geen ondersteuning biedt voor LZO wanneer Parquet bestanden lezen/schrijven. | No       |

> [!NOTE]
> Spaties in kolom naam wordt niet ondersteund voor Parquet-bestanden.

Hieronder ziet u een voor beeld van een Parquet-gegevensset op Azure Blob Storage:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie het artikel [pijp lijnen](concepts-pipelines-activities.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. In deze sectie vindt u een lijst met eigenschappen die worden ondersteund door de Parquet-bron en Sink.

### <a name="parquet-as-source"></a>Parquet als bron

De volgende eigenschappen worden ondersteund in de sectie *** \* bron \* *** van de Kopieer activiteit.

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **ParquetSource**. | Yes      |
| storeSettings | Een groep eigenschappen voor het lezen van gegevens uit een gegevens archief. Elke connector op basis van een bestand heeft zijn eigen ondersteunde Lees instellingen onder `storeSettings` . **Zie de sectie Details in connector artikel-> eigenschappen van de Kopieer activiteit**. | No       |

### <a name="parquet-as-sink"></a>Parquet als Sink

De volgende eigenschappen worden ondersteund in het gedeelte *** \* sink \* *** van de Kopieer activiteit.

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de Sink voor kopieer activiteiten moet worden ingesteld op **ParquetSink**. | Yes      |
| formatSettings | Een groep eigenschappen. Raadpleeg de tabel **Parquet write Settings** hieronder. |    No      |
| storeSettings | Een groep eigenschappen voor het schrijven van gegevens naar een gegevens archief. Elke connector op basis van bestanden heeft eigen ondersteunde schrijf instellingen onder `storeSettings` . **Zie de sectie Details in connector artikel-> eigenschappen van de Kopieer activiteit**. | No       |

Ondersteunde **Parquet-schrijf instellingen** onder `formatSettings` :

| Eigenschap      | Beschrijving                                                  | Vereist                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | Het type formatSettings moet zijn ingesteld op **ParquetWriteSettings**. | Yes                                                   |
| maxRowsPerFile | Wanneer u gegevens naar een map schrijft, kunt u ervoor kiezen om naar meerdere bestanden te schrijven en het maximum aantal rijen per bestand op te geven.  | No |
| fileNamePrefix | Van toepassing wanneer `maxRowsPerFile` is geconfigureerd.<br> Geef het voor voegsel van de bestands naam op wanneer u gegevens naar meerdere bestanden schrijft, wat resulteert in dit patroon: `<fileNamePrefix>_00000.<fileExtension>` . Als u niets opgeeft, wordt het voor voegsel van de bestands naam automatisch gegenereerd. Deze eigenschap is niet van toepassing wanneer een bron archief of een [gegevens archief met partitie opties](copy-activity-performance-features.md)is gebaseerd op het bestand.  | No |

## <a name="mapping-data-flow-properties"></a>Eigenschappen van gegevens stroom toewijzen

In het toewijzen van gegevens stromen kunt u lezen en schrijven naar Parquet-indeling in de volgende gegevens archieven: [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure data Lake Storage gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties)en [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties).

### <a name="source-properties"></a>Bron eigenschappen

De onderstaande tabel geeft een lijst van de eigenschappen die door een Parquet-bron worden ondersteund. U kunt deze eigenschappen bewerken op het tabblad **bron opties** .

| Naam | Beschrijving | Vereist | Toegestane waarden | Eigenschap gegevens stroom script |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Indeling | Indeling moet `parquet` | ja | `parquet` | indeling |
| Joker tekens | Alle bestanden die overeenkomen met het pad naar Joker tekens worden verwerkt. Onderdrukt de map en het bestandspad die in de gegevensset zijn ingesteld. | nee | Teken reeks [] | wildcardPaths |
| Basispad voor partitie | Voor bestands gegevens die zijn gepartitioneerd, kunt u een basispad opgeven om gepartitioneerde mappen als kolommen te kunnen lezen | nee | Tekenreeks | partitionRootPath |
| Lijst met bestanden | Hiermee wordt aangegeven of uw bron verwijst naar een tekst bestand met de bestanden die moeten worden verwerkt | nee | `true` of `false` | File List |
| Kolom voor het opslaan van de bestands naam | Een nieuwe kolom maken met de naam en het pad van het bron bestand | nee | Tekenreeks | rowUrlColumn |
| Na voltooiing | De bestanden na de verwerking verwijderen of verplaatsen. Bestandspad wordt gestart vanuit de hoofdmap van de container | nee | Verwijderen: `true` of `false` <br> Ga `[<from>, <to>]` | purgeFiles <br> moveFiles |
| Filteren op laatst gewijzigd | Kiezen of bestanden moeten worden gefilterd op basis van het tijdstip waarop deze voor het laatst zijn gewijzigd | nee | Tijdstempel | modifiedAfter <br> modifiedBefore |
| Geen bestanden gevonden | Als deze eigenschap waar is, wordt er geen fout gegenereerd als er geen bestanden worden gevonden | nee | `true` of `false` | ignoreNoFilesFound |

### <a name="source-example"></a>Bron voorbeeld

De onderstaande afbeelding is een voor beeld van een configuratie van een Parquet-bron in het toewijzen van gegevens stromen.

![Parquet-bron](media/data-flow/parquet-source.png)

Het gekoppelde gegevensstroom script is:

```
source(allowSchemaDrift: true,
    validateSchema: false,
    rowUrlColumn: 'fileName',
    format: 'parquet') ~> ParquetSource
```

### <a name="sink-properties"></a>Eigenschappen van Sink

De onderstaande tabel geeft een lijst van de eigenschappen die worden ondersteund door een Parquet-sink. U kunt deze eigenschappen bewerken op het tabblad **instellingen** .

| Naam | Beschrijving | Vereist | Toegestane waarden | Eigenschap gegevens stroom script |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Indeling | Indeling moet `parquet` | ja | `parquet` | indeling |
| De map wissen | Als de doelmap vóór het schrijven is gewist | nee | `true` of `false` | afkappen |
| Optie Bestands naam | De naamgevings indeling van de gegevens die zijn geschreven. Standaard één bestand per partitie in de indeling `part-#####-tid-<guid>` | nee | Patroon: teken reeks <br> Per partitie: teken reeks [] <br> Als gegevens in kolom: teken reeks <br> Uitvoer naar één bestand: `['<fileName>']` | filePattern <br> partitionFileNames <br> rowUrlColumn <br> partitionFileNames |

### <a name="sink-example"></a>Sink-voor beeld

De onderstaande afbeelding is een voor beeld van een Parquet-Sink-configuratie in het toewijzen van gegevens stromen.

![Parquet-Sink](media/data-flow/parquet-sink.png)

Het gekoppelde gegevensstroom script is:

```
ParquetSource sink(
    format: 'parquet',
    filePattern:'output[n].parquet',
    truncate: true,
    allowSchemaDrift: true,
    validateSchema: false,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> ParquetSink
```

## <a name="data-type-support"></a>Ondersteuning voor gegevens typen

Parquet complexe gegevens typen worden momenteel niet ondersteund (bijvoorbeeld kaart, lijst, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Zelf-hostende Integration Runtime gebruiken

> [!IMPORTANT]
> Als u de kopie wilt voorzien van zelf-hostende Integration Runtime bijvoorbeeld tussen on-premises en gegevens opslag in de Cloud, moet u het pakket **64-bits jre 8 (Java Runtime Environment) of openjdk** en **micro soft Visual C++ 2010 Redistributable** installeren op uw **IR-computer**. Raadpleeg de volgende alinea met meer informatie.

Voor een kopie die wordt uitgevoerd op een zelf-hostende IR met Parquet-serialisatie/deserialisatie, zoekt ADF de Java-runtime door het REGI ster *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* voor jre te controleren, indien niet gevonden, ter controle van de systeem variabele *`JAVA_HOME`* voor openjdk.

- **Jre gebruiken**: de 64-bits IR vereist een 64-bits jre. U kunt deze [hier](https://go.microsoft.com/fwlink/?LinkId=808605)vinden.
- **Als u openjdk wilt gebruiken**, wordt dit ondersteund sinds IR-versie 3,13. Verpak de jvm.dll met alle andere vereiste assembly's van OpenJDK op een zelf-hostende IR-computer en stel de systeem omgevings variabele in JAVA_HOME dienovereenkomstig in.
- **Het herdistribueerbare pakket Visual c++ 2010 installeren: het**herdistribueerbare pakket visual c++ 2010 wordt niet geïnstalleerd met de zelf-HOSTende IR-installaties. U kunt deze [hier](https://www.microsoft.com/download/details.aspx?id=14632)vinden.

> [!TIP]
> Als u gegevens kopieert naar/van Parquet-indeling met behulp van zelf-hostende Integration Runtime en de fout melding ' er is een fout opgetreden bij het aanroepen van Java, bericht: **Java. lang. OutOfMemoryError: Java-heap-ruimte**', kunt u een omgevings variabele toevoegen aan `_JAVA_OPTIONS` de computer die als host fungeert voor de zelf-hostende IR om een dergelijke kopie te maken,

![JVM-Heap-grootte instellen op zelf-hostende IR](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Voor beeld: Stel variabele `_JAVA_OPTIONS` met waarde in `-Xms256m -Xmx16g` . Met de vlag `Xms` wordt de eerste geheugen toewijzings groep opgegeven voor een Java Virtual Machine (JVM), terwijl `Xmx` de maximale geheugen toewijzings groep wordt opgegeven. Dit betekent dat JVM wordt gestart met `Xms` de hoeveelheid geheugen en dat er een maximum hoeveelheid geheugen kan worden gebruikt `Xmx` . ADF gebruikt standaard min 64 MB en Max 1G.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van kopieeractiviteiten](copy-activity-overview.md)
- [Gegevens stroom toewijzen](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [GetMetadata-activiteit](control-flow-get-metadata-activity.md)
