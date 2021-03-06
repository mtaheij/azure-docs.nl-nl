---
title: Problemen met gedeelde installatie kopieën in azure oplossen
description: Meer informatie over het oplossen van problemen met gedeelde afbeeldings galerieën.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 06/15/2020
ms.author: cynthn
ms.reviewer: cynthn
ms.openlocfilehash: 3a206a7aabee9f75524ab4715afa30ec05c612bf
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328060"
---
# <a name="troubleshooting-shared-image-galleries-in-azure"></a>Problemen met gedeelde afbeeldings galerieën in azure oplossen

Als u problemen ondervindt tijdens het uitvoeren van bewerkingen op galerieën met gedeelde installatiekopieën, definities van installatiekopieën en versies van installatiekopieën, voert u de mislukte opdracht opnieuw uit in de foutopsporingsmodus. De foutopsporingsmodus wordt geactiveerd door de `--debug` Switch door te geven met CLI en de `-Debug` Switch met Power shell. Wanneer u de fout hebt gevonden, volgt u dit document om de fouten op te lossen.


## <a name="unable-to-create-a-shared-image-gallery"></a>Kan geen galerie met gedeelde installatiekopieën maken

Mogelijke oorzaken:

*De naam van de galerie is ongeldig.*

De naam van de galerie kan bestaan uit hoofdletters en kleine letters, cijfers en punten. De naam van de galerie kan geen liggende streepjes bevatten. Wijzig de naam van de galerie en probeer het opnieuw. 

*De naam van de galerie is niet uniek in uw abonnement.*

Kies een andere galerie naam en probeer het opnieuw.


## <a name="unable-to-create-an-image-definition"></a>Kan geen definitie voor de installatiekopie maken 

Mogelijke oorzaken:

*de naam van de definitie van de installatie kopie is ongeldig.*

Toegestane tekens voor de definitie van de installatie kopie zijn hoofd letters, kleine letters, cijfers, punten, streepjes en punten. Wijzig de naam van de definitie van de installatie kopie en probeer het opnieuw.

*De verplichte eigenschappen voor het maken van een definitie van een installatie kopie zijn niet ingevuld.*

De eigenschappen, zoals naam, uitgever, aanbieding, SKU en type besturings systeem, zijn verplicht. Controleer of alle eigenschappen worden door gegeven.

Zorg ervoor dat de **OSTYPE**, Linux of Windows, van de definitie van de installatie kopie hetzelfde is als de bron die u gebruikt om de installatie kopie versie te maken. 


## <a name="unable-to-create-an-image-version"></a>Kan geen versie voor de installatiekopie maken 

Mogelijke oorzaken:

*De naam van de installatie kopie versie is ongeldig.*

Toegestane tekens voor een installatiekopieversie zijn cijfers en punten. Cijfers moeten binnen het bereik van een 32-bits geheel getal zijn. Indeling: *MajorVersion. MinorVersion. patch*. Wijzig de versie naam van de installatie kopie en probeer het opnieuw.

*Een door de bron beheerde installatie kopie van waaruit de installatie kopie versie wordt gemaakt, is niet gevonden.* 

Controleer of de bron installatie kopie bestaat en zich in dezelfde regio bevindt als de versie van de installatie kopie.

*De beheerde installatie kopie wordt niet ingericht.*

Zorg ervoor dat de inrichtings status van de door de bron beheerde installatie kopie is **geslaagd**.

*De lijst met doel regio's bevat niet de bron regio.*

De doel regio lijst moet de bron regio van de versie van de installatie kopie bevatten. Zorg ervoor dat u de bron regio hebt opgenomen in de lijst met doel regio's waarvoor Azure de versie van de installatie kopie moet repliceren.

*De replicatie naar alle doel regio's is niet voltooid.*

Gebruik de markering **--expand ReplicationStatus** om te controleren of de replicatie naar alle opgegeven doel regio's is voltooid. Als dat niet het geval is, wacht u tot 6 uur totdat de taak is voltooid. Als dit mislukt, voert u de opdracht opnieuw uit om de installatie kopie versie te maken en te repliceren. Als er sprake is van een groot aantal doel regio's, wordt de versie van de installatie kopie gerepliceerd naar. Overweeg de replicatie in fasen uit te voeren.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>Kan geen VM of schaalset maken 

Mogelijke oorzaken:

*De gebruiker die probeert een virtuele machine of VM-schaalset te maken, heeft geen lees toegang tot de versie van de installatie kopie.*

Neem contact op met de eigenaar van het abonnement en vraag hen om Lees toegang te verlenen aan de installatie kopie versie of de bovenliggende resources (zoals de galerie met gedeelde installatie kopieën of de definitie van de installatie kopie) via [Azure op rollen gebaseerd toegangs beheer (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles). 

*De versie van de installatie kopie is niet gevonden.*

Controleer of de regio waarvoor u een VM of virtuele machine wilt maken, is opgenomen in de lijst met doel regio's van de versie van de installatie kopie. Als de regio al in de lijst met doel regio's staat, controleert u of de replicatie taak is voltooid. U kunt de vlag **-ReplicationStatus** gebruiken om te controleren of de replicatie naar alle opgegeven doel regio's is voltooid. 

*Het maken van de virtuele machine of de VM-schaalset neemt veel tijd in beslag.*

Controleer of de **OSTYPE** van de versie van de installatie kopie die u probeert te maken van de virtuele machine en de VM-schaalset hetzelfde **OSTYPE** heeft van de bron die u hebt gebruikt voor het maken van de installatie kopie versie. 

## <a name="unable-to-share-resources"></a>Kan geen resources delen

Het delen van de galerie met gedeelde installatie kopieën, de definitie van de installatie kopie en de versie van de installatie kopie van de resources tussen abonnementen is ingeschakeld met behulp [van Azure op rollen gebaseerd toegangs beheer (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles). 

## <a name="replication-is-slow"></a>De replicatie is traag

Gebruik de markering **--expand ReplicationStatus** om te controleren of de replicatie naar alle opgegeven doel regio's is voltooid. Als dat niet het geval is, wacht u tot 6 uur totdat de taak is voltooid. Als dit mislukt, moet u de opdracht opnieuw activeren om de installatie kopie versie te maken en te repliceren. Als er sprake is van een groot aantal doel regio's, wordt de versie van de installatie kopie gerepliceerd naar. Overweeg de replicatie in fasen uit te voeren.

## <a name="azure-limits-and-quotas"></a>Limieten en quota in Azure 

[Azure-limieten en-quota](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) zijn van toepassing op alle resources van de gedeelde installatie kopie, de afbeeldings definitie en de versie van de installatie kopie. Zorg ervoor dat u zich binnen de limieten voor uw abonnementen bevindt. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Galerie met gedeelde installatie kopieën](./linux/shared-image-galleries.md).
