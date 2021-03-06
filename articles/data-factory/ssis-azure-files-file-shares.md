---
title: Bestanden openen en opslaan met SSIS-pakketten die zijn geïmplementeerd in azure
description: Meer informatie over het openen en opslaan van bestanden op locatie en in azure wanneer u SSIS-pakketten die gebruikmaken van lokale bestands systemen in SSIS optilt en verschuift in azure
ms.date: 06/27/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 36660854b9a7ae13431545392ef551694b48e97c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82628909"
---
# <a name="open-and-save-files-on-premises-and-in-azure-with-ssis-packages-deployed-in-azure"></a>Bestanden openen en opslaan op locatie en in azure met SSIS-pakketten die zijn geïmplementeerd in azure

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In dit artikel wordt beschreven hoe u bestanden on-premises en in azure opent en opslaat wanneer u SSIS-pakketten die gebruikmaken van lokale bestands systemen in SSIS opneemt en verschuift in Azure.

## <a name="save-temporary-files"></a>Tijdelijke bestanden opslaan

Als u tijdelijke bestanden wilt opslaan en verwerken tijdens één pakket uitvoering, kunnen pakketten gebruikmaken van de huidige werkmap ( `.` ) of tijdelijke map ( `%TEMP%` ) van uw Azure-SSIS Integration runtime knooppunten.

## <a name="use-on-premises-file-shares"></a>On-premises bestands shares gebruiken

Ga als volgt te werk om **on-premises bestands shares** te blijven gebruiken wanneer u pakketten die gebruikmaken van lokale bestands systemen in SSIS in azure optilt en verschuift:

1. Bestanden van lokale bestands systemen overdragen naar on-premises bestands shares.

2. De on-premises bestands shares koppelen aan een virtueel Azure-netwerk.

3. Voeg uw Azure-SSIS IR toe aan hetzelfde virtuele netwerk. Zie [een Azure-SSIS-integratie-runtime toevoegen aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie.

4. Verbind uw Azure-SSIS IR met de on-premises bestands shares in hetzelfde virtuele netwerk door toegangs referenties in te stellen die gebruikmaken van Windows-verificatie. Zie [verbinding maken met gegevens en bestands shares met Windows-verificatie](ssis-azure-connect-with-windows-auth.md)voor meer informatie.

5. Lokale bestands paden in uw pakketten bijwerken naar UNC-paden die verwijzen naar on-premises bestands shares. Bijvoorbeeld bijwerken `C:\abc.txt` naar `\\<on-prem-server-name>\<share-name>\abc.txt` .

## <a name="use-azure-file-shares"></a>Azure-bestands shares gebruiken

Ga als volgt te werk om **Azure files** te gebruiken wanneer u pakketten die gebruikmaken van lokale bestands systemen in SSIS in azure optilt en verschuift:

1. Bestanden van lokale bestands systemen overzetten naar Azure Files. Zie [Azure files](https://azure.microsoft.com/services/storage/files/)voor meer informatie.

2. Verbind uw Azure-SSIS IR met Azure Files door toegangs referenties in te stellen die gebruikmaken van Windows-verificatie. Zie [verbinding maken met gegevens en bestands shares met Windows-verificatie](ssis-azure-connect-with-windows-auth.md)voor meer informatie.

3. Lokale bestands paden in uw pakketten bijwerken naar UNC-paden die verwijzen naar Azure Files. Bijvoorbeeld bijwerken `C:\abc.txt` naar `\\<storage-account-name>.file.core.windows.net\<share-name>\abc.txt` .

## <a name="next-steps"></a>Volgende stappen

- Implementeer uw pakketten. Zie [een SSIS-project implementeren in azure met SSMS](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms)voor meer informatie.
- Voer uw pakketten uit. Zie [SSIS-pakketten uitvoeren in azure met SSMS](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms)voor meer informatie.
- Plan uw pakketten. Zie [SSIS-pakketten plannen in azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15)voor meer informatie.
