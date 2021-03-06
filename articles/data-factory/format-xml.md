---
title: XML-indeling in Azure Data Factory
description: In dit onderwerp wordt beschreven hoe u kunt omgaan met XML-indeling in Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/23/2020
ms.author: jingwang
ms.openlocfilehash: e0fadf4ac8cea1c8804b17f5549a99bc360e2950
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334290"
---
# <a name="xml-format-in-azure-data-factory"></a>XML-indeling in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Volg dit artikel als u **de XML-bestanden wilt parseren**. 

De XML-indeling wordt ondersteund voor de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestands systeem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)en [SFTP](connector-sftp.md). Het wordt ondersteund als bron, maar niet op sink.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel [gegevens sets](concepts-datasets-linked-services.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevens sets. Deze sectie bevat een lijst met eigenschappen die door de XML-gegevensset worden ondersteund.

| Eigenschap         | Beschrijving                                                  | Vereist |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | De eigenschap type van de gegevensset moet worden ingesteld op **XML**. | Yes      |
| location         | Locatie-instellingen van bestand (en). Elke connector op basis van bestanden heeft een eigen locatie type en ondersteunde eigenschappen onder `location` . **Zie de sectie Details in connector artikel-> eigenschappen van gegevensset**. | Yes      |
| encodingName     | Het coderings type dat wordt gebruikt voor het lezen/schrijven van test bestanden. <br>Toegestane waarden zijn als volgt: ' UTF-8 ', ' UTF-16 ', ' UTF-16BE ', ' UTF-32 ', ' UTF-32BE ', ' VS-ASCII ', ' UTF-7 ', ' BIG5 ', ' EUC-JP ', ' EUC-KR ', ' GB2312 "," GB18030 "," JOHAB "," SHIFT-JIS "," CP875 "," CP866 "," IBM00858 "," IBM037 "," IBM273 "," IBM437 "," IBM500 "," IBM737 "," IBM775 "," IBM850 "," IBM852 "," IBM855 "," IBM857 "," IBM860 "," IBM861 ', ' IBM863 ', ' IBM864 ', ' IBM865 ', ' IBM869 ', ' IBM870 ', ' IBM01140 ', ' IBM01141 ', ' IBM01142 ', ' IBM01143 ', ' IBM01144 ', IBM01145 "," IBM01146 "," IBM01147 "," IBM01148 "," IBM01149 "," ISO-2022-JP "," ISO-2022-KR "," ISO-8859-1 "," ISO-8859-2 "," ISO-8859-3 "," ISO-8859-4 "," ISO-8859-5 "," ISO-8859-6 "," ISO-8859-7 "," ISO-8859-8 "," ISO-8859-9 "," ISO-8859-13 " , "ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1256", "WINDOWS-1257", "WINDOWS-1258".| No       |
| nullValue | Hiermee wordt de teken reeks representatie van een null-waarde opgegeven.<br/>De standaard waarde is een **lege teken reeks**. | No |
| compressie | Groep eigenschappen voor het configureren van bestands compressie. Configureer deze sectie als u compressie/decompressie wilt uitvoeren tijdens de uitvoering van de activiteit. | No |
| type<br>(*onder `compression` *) | De compressie-codec die wordt gebruikt voor het lezen/schrijven van XML-bestanden. <br>Toegestane waarden zijn **bzip2**, **gzip**, **Deflate**, **ZipDeflate**, **TarGzip**, **Snappy**of **LZ4**. De standaard waarde is niet gecomprimeerd.<br>**Houd er rekening mee** dat het kopiëren van gegevens op dit moment geen ondersteuning biedt voor ' snappy ' & ' LZ4 ' en dat de toewijzing van data stroom niet wordt ondersteund ' ZipDeflate '.<br>**Opmerking** wanneer u de Kopieer activiteit gebruikt om **ZipDeflate** / **TarGzip** -bestand (en) te decomprimeren en te schrijven naar Sink-gegevens archief op basis van een bestand, worden standaard bestanden uitgepakt naar de map:. `<path specified in dataset>/<folder named as source compressed file>/` Gebruik de `preserveZipFileNameAsFolder` / `preserveCompressionFileNameAsFolder` bron van de [Kopieer activiteit](#xml-as-source) om te bepalen of de naam van de gecomprimeerde bestanden als mapstructuur moet worden behouden. | Nee.  |
| niveau<br/>(*onder `compression` *) | De compressie ratio. <br>Toegestane waarden zijn **optimaal** of **snelst**.<br>- **Snelst:** De compressie bewerking moet zo snel mogelijk worden voltooid, zelfs als het resulterende bestand niet optimaal is gecomprimeerd.<br>- **Optimaal**: de compressie bewerking moet optimaal worden gecomprimeerd, zelfs als het volt ooien van de bewerking langer duurt. Zie het onderwerp [compressie niveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) voor meer informatie. | No       |

Hieronder ziet u een voor beeld van XML-gegevensset op Azure Blob Storage:

```json
{
    "name": "XMLDataset",
    "properties": {
        "type": "Xml",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "ZipDeflate"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie het artikel [pijp lijnen](concepts-pipelines-activities.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Deze sectie bevat een lijst met eigenschappen die door de XML-bron worden ondersteund.

Meer informatie over het toewijzen van XML-gegevens en Sink-gegevens opslag/-indeling van [schema toewijzing](copy-activity-schema-and-type-mapping.md). Wanneer u een voor beeld van XML-bestanden bekijkt, worden de gegevens weer gegeven met JSON-hiërarchie en wordt u gebruikgemaakt van JSON-pad om naar de velden te verwijzen.

### <a name="xml-as-source"></a>XML als bron

De volgende eigenschappen worden ondersteund in de sectie *** \* bron \* *** van de Kopieer activiteit. Meer informatie over het [gedrag van XML-Connector](#xml-connector-behavior).

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **XmlSource**. | Yes      |
| formatSettings | Een groep eigenschappen. Raadpleeg de onderstaande tabel **XML-Lees instellingen** . | No       |
| storeSettings | Een groep eigenschappen voor het lezen van gegevens uit een gegevens archief. Elke connector op basis van een bestand heeft zijn eigen ondersteunde Lees instellingen onder `storeSettings` . **Zie de sectie Details in connector artikel-> eigenschappen van de Kopieer activiteit**. | No       |

Ondersteunde **XML-Lees instellingen** onder `formatSettings` :

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Het type formatSettings moet zijn ingesteld op **XmlReadSettings**. | Yes      |
| validationMode | Hiermee wordt opgegeven of het XML-schema moet worden gevalideerd.<br>Toegestane waarden zijn **geen** (standaard instelling, geen validatie), **xsd** (valideren met XSD), **DTD** (valideren met DTD). | No |
| naam ruimten | Hiermee wordt aangegeven of naam ruimte moet worden ingeschakeld bij het parseren van de XML-bestanden. Toegestane waarden zijn: **True** (standaard), **False**. | No |
| namespacePrefixes | Naam ruimte-URI naar voorvoegsel toewijzing, die wordt gebruikt om velden te benoemen tijdens het parseren van het XML-bestand.<br/>Als een XML-bestand naam ruimte en naam ruimte is ingeschakeld, is de veld naam standaard hetzelfde als in het XML-document.<br>Als er een item is gedefinieerd voor de naam ruimte-URI in deze kaart, is de veld naam `prefix:fieldName` . | No |
| detectDataType | Hiermee wordt aangegeven of integer-, double-en Boolean-gegevens typen moeten worden gedetecteerd. Toegestane waarden zijn: **True** (standaard), **False**.| No |
| compressionProperties | Een groep eigenschappen voor het decomprimeren van gegevens voor een bepaalde compressie-codec. | No       |
| preserveZipFileNameAsFolder<br>(*onder `compressionProperties` -> `type` als `ZipDeflateReadSettings` *)  | Van toepassing wanneer de invoer-gegevensset is geconfigureerd met **ZipDeflate** -compressie. Hiermee wordt aangegeven of de naam van het bron-zip-bestand tijdens het kopiëren moet worden bewaard als mapstructuur.<br>-Als deze eigenschap is ingesteld op **True (standaard)**, wordt in Data Factory ongecomprimeerde bestanden naar `<path specified in dataset>/<folder named as source zip file>/` .<br>-Als deze eigenschap is ingesteld op **Onwaar**, wordt door Data Factory ongecomprimeerde bestanden rechtstreeks naar te schrijven `<path specified in dataset>` . Zorg ervoor dat u geen dubbele bestands namen in verschillende zip-bron bestanden hebt om te voor komen dat u racet of onverwacht gedrag.  | No |
| preserveCompressionFileNameAsFolder<br>(*onder `compressionProperties` -> `type` als `TarGZipReadSettings` *) | Van toepassing wanneer de invoer-gegevensset is geconfigureerd met **TarGzip** -compressie. Hiermee wordt aangegeven of de gecomprimeerde bestands naam van de bron moet worden behouden als mapstructuur tijdens het kopiëren.<br>-Als deze eigenschap is ingesteld op **True (standaard)**, schrijft Data Factory gedecomprimeerde bestanden naar `<path specified in dataset>/<folder named as source compressed file>/` . <br>-Als deze eigenschap is ingesteld op **Onwaar**, schrijft Data Factory de gedecomprimeerde bestanden rechtstreeks naar `<path specified in dataset>` . Zorg ervoor dat u geen dubbele bestands namen in verschillende bron bestanden hebt om te voor komen dat u racet of onverwacht gedrag. | No |

## <a name="mapping-data-flow-properties"></a>Eigenschappen van gegevens stroom toewijzen

In het toewijzen van gegevens stromen kunt u lezen en schrijven naar XML-indeling in de volgende gegevens archieven: [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure data Lake Storage gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties)en [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties). U kunt naar XML-bestanden verwijzen met behulp van XML-gegevensset of met behulp van een [inline-gegevensset](data-flow-source.md#inline-datasets).

### <a name="source-properties"></a>Bron eigenschappen

De onderstaande tabel geeft een lijst van de eigenschappen die worden ondersteund door een XML-bron. U kunt deze eigenschappen bewerken op het tabblad **bron opties** . Meer informatie over het [gedrag van XML-Connector](#xml-connector-behavior). Bij gebruik van een inline-gegevensset worden extra Bestands instellingen weer geven, die hetzelfde zijn als de eigenschappen die worden beschreven in de sectie [Eigenschappen van gegevensset](#dataset-properties) . 

| Naam | Beschrijving | Vereist | Toegestane waarden | Eigenschap gegevens stroom script |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Joker tekens | Alle bestanden die overeenkomen met het pad naar Joker tekens worden verwerkt. Onderdrukt de map en het bestandspad die in de gegevensset zijn ingesteld. | No | Teken reeks [] | wildcardPaths |
| Basispad voor partitie | Voor bestands gegevens die zijn gepartitioneerd, kunt u een basispad opgeven om gepartitioneerde mappen als kolommen te kunnen lezen | Nee | Tekenreeks | partitionRootPath |
| Lijst met bestanden | Hiermee wordt aangegeven of uw bron verwijst naar een tekst bestand met de bestanden die moeten worden verwerkt | No | `true` of `false` | File List |
| Kolom voor het opslaan van de bestands naam | Een nieuwe kolom maken met de naam en het pad van het bron bestand | Nee | Tekenreeks | rowUrlColumn |
| Na voltooiing | De bestanden na de verwerking verwijderen of verplaatsen. Bestandspad wordt gestart vanuit de hoofdmap van de container | No | Verwijderen: `true` of `false` <br> Ga `['<from>', '<to>']` | purgeFiles <br>moveFiles |
| Filteren op laatst gewijzigd | Kiezen of bestanden moeten worden gefilterd op basis van het tijdstip waarop deze voor het laatst zijn gewijzigd | No | Tijdstempel | modifiedAfter <br>modifiedBefore |
| Validatie modus | Hiermee wordt opgegeven of het XML-schema moet worden gevalideerd. | No | `None` (standaard, geen validatie)<br>`xsd` (valideren met XSD)<br>`dtd` (valideren met DTD). | validationMode |
| Naamruimten | Hiermee wordt aangegeven of naam ruimte moet worden ingeschakeld bij het parseren van de XML-bestanden. | No | `true` (standaard) of `false` | naam ruimten |
| Naam ruimte voorvoegsel paren | Naam ruimte-URI naar voorvoegsel toewijzing, die wordt gebruikt om velden te benoemen tijdens het parseren van het XML-bestand.<br/>Als een XML-bestand naam ruimte en naam ruimte is ingeschakeld, is de veld naam standaard hetzelfde als in het XML-document.<br>Als er een item is gedefinieerd voor de naam ruimte-URI in deze kaart, is de veld naam `prefix:fieldName` . | No | Matrix met patroon`['URI1'->'prefix1','URI2'->'prefix2']` | namespacePrefixes |
| Geen bestanden gevonden | Als deze eigenschap waar is, wordt er geen fout gegenereerd als er geen bestanden worden gevonden | nee | `true` of `false` | ignoreNoFilesFound |

### <a name="xml-source-script-example"></a>Voor beeld van XML-bron script

Het onderstaande script is een voor beeld van een configuratie van een XML-bron in het toewijzen van gegevens stromen met de gegevensset-modus.

```
source(allowSchemaDrift: true,
    validateSchema: false,
    validationMode: 'xsd',
    namespaces: true) ~> XMLSource
```

Het onderstaande script is een voor beeld van een XML-bron configuratie met behulp van de inline-gegevensset modus.

```
source(allowSchemaDrift: true,
    validateSchema: false,
    format: 'xml',
    fileSystem: 'filesystem',
    folderPath: 'folder',
    validationMode: 'xsd',
    namespaces: true) ~> XMLSource
```

## <a name="xml-connector-behavior"></a>Gedrag van XML-Connector

Let op het volgende wanneer u XML als bron gebruikt.

- XML-kenmerken:

    - Kenmerken van een element worden geparseerd als de subvelden van het element in de hiërarchie.
    - De naam van het kenmerk veld volgt het patroon `@attributeName` .

- Validatie van XML-schema:

    - U kunt ervoor kiezen om schema niet te valideren of schema te valideren met XSD of DTD.
    - Wanneer u XSD of DTD gebruikt voor het valideren van XML-bestanden, moet de XSD/DTD worden aangeduid in de XML-bestanden via een relatief pad.

- Verwerking van naam ruimten:

    - De naam ruimte kan worden uitgeschakeld bij het gebruik van de gegevens stroom. in dat geval worden de kenmerken die de naam ruimte definiëren, geparseerd als normale kenmerken.
    - Wanneer de naam ruimte is ingeschakeld, volgen de namen van het element en de kenmerken het patroon       `namespaceUri,elementName` en `namespaceUri,@attributeName` standaard. U kunt een naam ruimte voorvoegsel definiëren voor elke naam ruimte-URI in de bron. in dat geval volgen de namen van het element en de kenmerken het patroon `definedPrefix:elementName` of `definedPrefix:@attributeName` in plaats daarvan.

- Kolom waarde:

    - Als een XML-element zowel eenvoudige tekst waarde als attributen/onderliggende elementen heeft, wordt de eenvoudige tekst waarde geparseerd als de waarde van een kolom waarde met ingebouwde veld naam `_value_` . En de naam ruimte van het element wordt ook overgenomen als dit van toepassing is.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van kopieeractiviteiten](copy-activity-overview.md)
- [Gegevens stroom toewijzen](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [GetMetadata-activiteit](control-flow-get-metadata-activity.md)
