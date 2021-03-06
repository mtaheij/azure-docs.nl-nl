---
title: Azure-resources verplaatsen naar een andere regio
description: Hierin wordt een overzicht gegeven van het verplaatsen van Azure-resources tussen Azure-regio's.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 7a71502ec361004079e0962d8bc6433316a4ba81
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007635"
---
# <a name="moving-azure-resources-across-regions"></a>Azure-resources verplaatsen tussen regio's

Dit artikel bevat informatie over het verplaatsen van Azure-resources tussen Azure-regio's.

Azure-geografi,-regio's en-beschikbaarheids zones vormen de basis van de wereld wijde Azure-infra structuur. Azure- [geografi](https://azure.microsoft.com/global-infrastructure/geographies/) bevatten doorgaans twee of meer [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/). Een regio is een gebied binnen een geografie met Beschikbaarheidszones en meerdere data centers. 

Na het implementeren van resources in een specifieke Azure-regio, zijn er een aantal redenen waarom u resources naar een andere regio wilt verplaatsen.

- **Uitlijnen met een regio start**: Verplaats uw resources naar een nieuw geïntroduceerde Azure-regio die voorheen niet beschikbaar was.
- **Uitlijnen voor Services/onderdelen**: resources verplaatsen om te kunnen profiteren van services of functies die beschikbaar zijn in een bepaalde regio.
- **Reageer op bedrijfs ontwikkelingen**: Verplaats resources naar een regio in antwoord op bedrijfs wijzigingen, zoals fusies of acquisities.
- **Uitlijnen voor nabijheid**: resources verplaatsen naar een regio die lokaal is voor uw bedrijf.
- **Voldoen aan de vereisten voor gegevens**: Verplaats resources om uit te lijnen met vereisten voor gegevens locatie of gegevens classificatie behoeften. [Meer informatie](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Reageren op implementatie vereisten**: Verplaats resources die zijn geïmplementeerd, of verplaats als reactie op capaciteits behoeften. 
- **Reageren op buiten gebruik stellen**: resources verplaatsen vanwege het buiten gebruik stellen van regio's.

## <a name="move-resources-with-resource-mover"></a>Resources verplaatsen met resource-overdrijfing

U kunt resources verplaatsen naar een andere regio met [Azure resource](../../resource-mover/overview.md)-overzetten. Resource-overdrijfing biedt:

- Eén hub voor het verplaatsen van resources tussen regio's.
- Verkort de verplaatsings tijd en complexiteit. Alles wat u nodig hebt, bevindt zich op één locatie.
- Een eenvoudige en consistente ervaring voor het verplaatsen van verschillende typen Azure-resources.
- Een eenvoudige manier om afhankelijkheden te identificeren tussen resources die u wilt verplaatsen. Zo kunt u de gerelateerde resources samen verplaatsen, zodat alles naar verwachting in de doel regio werkt, na de verplaatsing.
- Automatisch opschonen van resources in de bron regio, als u deze na de verplaatsing wilt verwijderen.
- Testen. U kunt een verhuizing uitproberen en deze vervolgens verwijderen als u niet volledig wilt verplaatsen.

U kunt resources naar een andere regio verplaatsen met behulp van een aantal verschillende methoden:

- **Begin met het verplaatsen van resources van een resource groep**: met deze methode kunt u de regio verplaatsen vanuit een resource groep. Nadat u de resources die u wilt verplaatsen hebt geselecteerd, wordt het proces voortgezet in de resource-overschakeling hub om bron afhankelijkheden te controleren en het verplaatsings proces te organiseren. [Meer informatie](../../resource-mover/move-region-within-resource-group.md).
- **Begin met het verplaatsen van resources rechtstreeks vanuit de resource**-overschakeling hub: met deze methode kunt u de regio verplaatsen rechtstreeks in de hub. [Meer informatie](../../resource-mover/tutorial-move-region-virtual-machines.md).


## <a name="support-for-region-move"></a>Ondersteuning voor het verplaatsen van regio's

U kunt resource-overstap momenteel gebruiken om deze resources te verplaatsen naar een andere regio:

- Azure-Vm's en gekoppelde schijven
- NIC’s
- Beschikbaarheidssets
- Virtuele netwerken van Azure
- Openbare IP-adressen
- Netwerkbeveiligingsgroepen (NSG's)
- Interne en open bare load balancers
- Azure SQL-data bases en elastische Pools

## <a name="region-move-process"></a>Regio verplaatsings proces

Het feitelijke proces voor het verplaatsen van resources over regio's is afhankelijk van de resources die u wilt verplaatsen. Er zijn echter enkele veelvoorkomende belang rijke stappen:

1. **Vereisten controleren**: vereisten zijn onder andere het maken van de resources die u nodig hebt in de doel regio, het controleren of u voldoende quota hebt en te controleren of uw abonnement toegang heeft tot de doel regio.
2. **Afhankelijkheden analyseren**: uw resources hebben mogelijk afhankelijkheden van andere resources. Voordat u gaat verplaatsen, moet u afhankelijkheden bepalen, zodat de verplaatste resources na de verplaatsing blijven functioneren zoals verwacht.
3. **Voor bereiding voor verplaatsen**: Dit zijn de stappen die u in uw primaire regio hebt uitgevoerd vóór de verplaatsing. U moet bijvoorbeeld een Azure Resource Manager sjabloon exporteren of resources van de bron naar het doel repliceren.
4. **De resources verplaatsen**: hoe u resources verplaatst, is afhankelijk van wat ze zijn. Mogelijk moet u een sjabloon in de doel regio implementeren of de resources mislukken voor het doel.
5. **Doel resources negeren**: nadat u resources hebt verplaatst, kunt u de resources nu in de doel regio bekijken en besluiten of er iets wat u niet nodig hebt.
6. **De verplaatsing door voeren**: nadat u resources in de doel regio hebt gecontroleerd, is het mogelijk dat er voor sommige resources een laatste doorvoer actie is vereist. In een doel regio die nu de primaire regio is, moet u mogelijk herstel na nood geval instellen voor een nieuwe secundaire regio. 
7. **De bron opschonen**: tot slot kunt u de resources die u hebt gemaakt voor het verplaatsen en resources in uw primaire regio, opschonen en uit bedrijf nemen in de nieuwe regio.



## <a name="next-steps"></a>Volgende stappen

Meer [informatie](../../resource-mover/about-move-process.md) over het verplaatsings proces in resource verplaatsen.
