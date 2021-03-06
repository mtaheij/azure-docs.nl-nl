---
title: Beperkingen-Azure Database for MySQL flexibele server
description: In dit artikel worden de beperkingen in Azure Database for MySQL flexibele server beschreven, zoals het aantal opties voor verbinding en opslag engine.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 9/21/2020
ms.openlocfilehash: 64f4b6e87f038b265fbd2c3e13f3779fb4c24f74
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90936157"
---
# <a name="limitations-in-azure-database-for-mysql---flexible-server-preview"></a>Beperkingen in Azure Database for MySQL flexibele server (preview-versie)

> [!IMPORTANT] 
> Azure Database for MySQL - Flexible Server is momenteel beschikbaar als openbare preview.

In dit artikel worden de beperkingen in de Azure Database for MySQL flexibele Server-service beschreven. [Algemene beperkingen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.7/en/limits.html) in de MySQL-data base-engine zijn ook van toepassing. Als u meer wilt weten over resource-lagen (compute, geheugen, opslag), raadpleegt u het artikel [Compute and Storage](concepts-compute-storage.md) .

## <a name="server-parameters"></a>Serverparameters

> [!NOTE]
> Als u op zoek bent naar de minimale/maximale waarden voor server parameters zoals `max_connections` en `innodb_buffer_pool_size` , wordt deze informatie verplaatst naar de concepten van server parameters <!-- **[server parameters](./concepts-server-parameters.md)** --> .

Azure Database for MySQL biedt ondersteuning voor het afstemmen van de waarden van server parameters. De minimum-en maximum waarde van sommige para meters (bijvoorbeeld `max_connections`, `join_buffer_size` , `query_cache_size` ) wordt bepaald door de compute-laag en de reken grootte van de server. Raadpleeg de concepten van server parameters <!-- [server parameters](./concepts-server-parameters.md)--> voor meer informatie over deze limieten.

## <a name="storage-engines"></a>Opslag engines

MySQL ondersteunt veel opslag engines. Op Azure Database for MySQL flexibele server worden de volgende opslag engines ondersteund en niet ondersteund:

### <a name="supported"></a>Ondersteund
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [GEHEUGENMETABASE](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Niet ondersteund
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [FAXBERICHTEN](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [Federatie](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privileges--data-manipulation-support"></a>Bevoegdheden & ondersteuning voor gegevens manipulatie

Veel server parameters en-instellingen kunnen per ongeluk de prestaties van de server afnemen of de ACID-eigenschappen van de MySQL-server negatien. Voor het onderhouden van de service-integriteit en SLA op het niveau van een product, worden met deze service niet meerdere rollen beschikbaar. 

De MySQL-service staat geen directe toegang tot het onderliggende bestands systeem toe. Sommige opdrachten voor het bewerken van gegevens worden niet ondersteund. 

### <a name="unsupported"></a>Niet ondersteund

Het volgende wordt niet ondersteund:
- Rol van DBA: beperkt. U kunt ook de gebruiker beheerder (gemaakt tijdens het maken van een nieuwe server) gebruiken om de meeste DDL-en DML-instructies uit te voeren. 
- SUPER bevoegdheid: op dezelfde manier is [Super privilege](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) beperkt.
- DEFINE: vereist Super privileges om te maken en beperkt. Als u gegevens importeert met behulp van een back-up, verwijdert u de `CREATE DEFINER` opdrachten hand matig of gebruikt u de `--skip-definer` opdracht bij het uitvoeren van een mysqldump.
- Systeem databases: de [MySQL-systeem database](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) is alleen-lezen en wordt gebruikt ter ondersteuning van verschillende PaaS-functionaliteit. U kunt geen wijzigingen aanbrengen in de `mysql` systeem database.
- `SELECT ... INTO OUTFILE`: Wordt niet ondersteund in de service.

### <a name="supported"></a>Ondersteund
- `LOAD DATA INFILE` wordt ondersteund, maar de `[LOCAL]` para meter moet worden opgegeven en worden omgeleid naar een UNC-pad (Azure-opslag gekoppeld aan SMB).

## <a name="functional-limitations"></a>Functionele beperkingen

### <a name="zone-redundant-ha"></a>Zone redundante HA
- Deze configuratie kan alleen worden ingesteld tijdens het maken van de server.
- Niet ondersteund in een Burstive Compute-laag.

### <a name="networking"></a>Netwerken
- De verbindings methode kan niet worden gewijzigd na het maken van de server. Als de server is gemaakt met *persoonlijke toegang (VNet-integratie)*, kan deze niet meer worden gewijzigd in *open bare toegang (toegestane IP-adressen)* na het maken, en andersom
- TLS/SSL is standaard ingeschakeld en kan niet worden uitgeschakeld.
- De minimale TLS-versie die op de server wordt ondersteund, is TLS 1.2. Raadpleeg [verbinding maken met behulp van TLS/SSL](./how-to-connect-tls-ssl.md) voor meer informatie.

### <a name="stopstart-operation"></a>Stoppen/starten-bewerking
- Niet ondersteund met zone-redundante HA-configuraties (primair en stand-by).
- Niet ondersteund bij het lezen van replica configuraties (zowel bron-als replica).

### <a name="scale-operations"></a>Schaal bewerkingen
- Het verminderen van de ingerichte Server opslag wordt niet ondersteund.

### <a name="read-replicas"></a>Leesreplica's
- Niet ondersteund met zone-redundante HA-configuraties (primair en stand-by).

### <a name="server-version-upgrades"></a>Server versie-upgrades
- Automatische migratie tussen de primaire data base-engine versies wordt niet ondersteund. Als u een upgrade wilt uitvoeren voor de primaire versie, neemt u een [dump op en herstelt](../concepts-migrate-dump-restore.md) u deze op een server die is gemaakt met de nieuwe engine versie.

### <a name="restoring-a-server"></a>Een server herstellen
- Bij herstel naar een bepaald tijdstip worden nieuwe servers gemaakt met dezelfde Compute-en opslag configuraties als de bron server waarop deze is gebaseerd. De herstelde server Compute kan worden geschaald nadat de server is gemaakt.
- Het herstellen van een verwijderde server wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

- Inzicht [in wat er beschikbaar is voor de berekenings-en opslag opties](concepts-compute-storage.md)
- Meer informatie over [ondersteunde mysql-versies](concepts-supported-versions.md)
- Lees [hoe u een back-up maakt en een server herstelt met behulp van de Azure Portal](how-to-restore-server-portal.md)