---
title: Overzicht van blauwdrukvoorbeeld van Azure Security-benchmark
description: Overzicht van het blauwdrukvoorbeeld van de Azure Security-benchmark. Met dit blauwdrukvoorbeeld kunnen klanten specifieke beheeropties bekijken.
ms.date: 06/02/2020
ms.topic: sample
ms.openlocfilehash: 4113f350c21087dbda5f69d9c3981e2d169ffc00
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651904"
---
# <a name="azure-security-benchmark-blueprint-sample"></a>Blauwdrukvoorbeeld van Azure Security-benchmark

Het blauwdrukvoorbeeld voor de Azure Security-benchmark biedt handvatten voor governance met [Azure Policy](../../policy/overview.md), waarmee u bepaalde beheeropties van de [Azure Security-benchmark](../../../security/benchmarks/overview.md) kunt evalueren. Deze blauwdruk helpt klanten een kernset met beleid te implementeren voor architectuur die in Azure is geïmplementeerd en waarin zij beheeropties van de Azure Security-benchmark willen implementeren.

## <a name="control-mapping"></a>Toewijzing van beheeropties

De [mapping van het Azure Policy-besturingselement](../../policy/samples/azure-security-benchmark.md) bevat details over beleidsdefinities die in deze blauwdruk zijn opgenomen en hoe deze beleidsdefinities worden toegewezen aan de **nalevingsdomeinen** en **besturingselementen** in de Azure Security-benchmark. Wanneer resources worden toegewezen aan een architectuur, worden deze op niet-naleving van toegewezen beleidsregels geëvalueerd door Azure Policy. Zie [Azure Policy](../../policy/overview.md) voor meer informatie.

## <a name="deploy"></a>Implementeren

Als u het blauwdrukvoorbeeld Azure Security-benchmark van Azure Blueprints wilt implementeren, moet u de volgende stappen uitvoeren:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken op basis van het voorbeeld
> - Uw kopie van het voorbeeld markeren als **Gepubliceerd**
> - Uw kopie van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

### <a name="create-blueprint-from-sample"></a>Een blauwdruk maken op basis van een voorbeeld

Implementeer eerst het blauwdrukvoorbeeld door een nieuwe blauwdruk in uw omgeving te maken op basis van het voorbeeld.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Op de pagina **Aan de slag** aan de linkerkant selecteert u de knop **Maken** onder _Een blauwdruk maken_.

1. Zoek het blauwdrukvoorbeeld **Azure Security-benchmark** onder _Andere voorbeelden_ en selecteer de naam om dit voorbeeld te selecteren.

1. Voer de _Basisinstellingen_ van het blauwdrukvoorbeeld in:

   - **Naam van blauwdruk**: Geef een naam op voor uw kopie van het blauwdrukvoorbeeld Azure Security-benchmark.
   - **Definitielocatie**: Gebruik het weglatingsteken en selecteer de beheergroep waarin u uw kopie van het voorbeeld wilt opslaan.

1. Selecteer het tabblad _Artefacten_ boven aan de pagina of kies **Volgende: Artefacten** onder aan de pagina.

1. Controleer de lijst met artefacten die samen het blauwdrukvoorbeeld vormen. Veel van de artefacten bevatten parameters die we later zullen definiëren. Selecteer **Concept opslaan** wanneer u klaar bent met het controleren van het blauwdrukvoorbeeld.

### <a name="publish-the-sample-copy"></a>De voorbeeldkopie publiceren

Uw kopie van het blauwdrukvoorbeeld is nu gemaakt in uw omgeving. De kopie is gemaakt in de **Concept**-modus en moet worden **Gepubliceerd** voordat deze kan worden toegewezen en geïmplementeerd. De kopie van het blauwdrukvoorbeeld kan worden aangepast aan uw omgeving en behoeften, maar door die aanpassing is het mogelijk dat de kopie niet meer is afgestemd op de aanbevelingen van Azure Security-benchmark.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de pagina **Blauwdrukdefinities** aan de linkerkant. Gebruik de filters om uw kopie van het blauwdrukvoorbeeld te zoeken en selecteer vervolgens uw kopie.

1. Selecteer **Blauwdruk publiceren** boven aan de pagina. Op de nieuwe pagina aan de rechterkant geeft u een **Versie** voor uw kopie van het blauwdrukvoorbeeld op. Deze eigenschap is handig als u later een aanpassing wilt maken. Geef **Notities over wijzigingen** op, zoals "Eerste gepubliceerde versie op basis van het blauwdrukvoorbeeld Azure Security-benchmark." Selecteer vervolgens **Publiceren** onder aan de pagina.

### <a name="assign-the-sample-copy"></a>De voorbeeldkopie toewijzen

Zodra de kopie van het blauwdrukvoorbeeld is **Gepubliceerd**, kan het worden toegewezen aan een abonnement binnen de beheergroep waarin de kopie is opgeslagen. Dit is de stap waarin parameters worden opgegeven om elke implementatie van de kopie van het blauwdrukvoorbeeld uniek te maken.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de pagina **Blauwdrukdefinities** aan de linkerkant. Gebruik de filters om uw kopie van het blauwdrukvoorbeeld te zoeken en selecteer vervolgens uw kopie.

1. Selecteer **Blauwdruk toewijzen** boven aan de pagina Blauwdrukdefinitie.

1. Geef de parameterwaarden op voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen in de beheergroep waarin u uw kopie van het blauwdrukvoorbeeld hebt opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing gemaakt voor elk abonnement waarvoor de ingevoerde parameters worden gebruikt.
     - **Naam van toewijzing**: De naam wordt vooraf voor u ingevuld op basis van de naam van de blauwdruk.
       Wijzig de naam als dat nodig is of gebruik de opgegeven naam.
     - **Locatie**: Selecteer een regio waarin u de beheerde identiteit wilt maken. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **Blauwdrukdefinitieversie**: Kies een **gepubliceerde** versie van uw kopie van het blauwdrukvoorbeeld.

   - Toewijzing vergrendelen

     Selecteer de instelling voor het vergrendelen van de blauwdruk voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de optie voor de standaard door het _systeem toegewezen_ beheerde identiteit staan.

   - Artefactparameters

     De parameters die in deze sectie worden gedefinieerd, zijn van toepassing op het artefact waaronder de parameter is gedefinieerd. Deze parameters zijn [dynamische parameters](../concepts/parameters.md#dynamic-parameters), aangezien ze zijn gedefinieerd tijdens de toewijzing van de blauwdruk. Zie de [tabel met artefactparameters](#artifact-parameters-table) voor een volledige lijst met artefactparameters en hun beschrijvingen.

1. Zodra alle parameters zijn ingevoerd, selecteert u **Toewijzen** onder aan de pagina. De blauwdruktoewijzing wordt gemaakt en de implementatie van het artefact begint. De implementatie duurt circa één uur. Open de blauwdruktoewijzing om de status van de implementatie te controleren.

> [!WARNING]
> De Azure Blueprints-service en de ingebouwde blauwdrukvoorbeelden zijn **gratis**. Azure-resources zijn [geprijsd per product](https://azure.microsoft.com/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om de kosten te schatten voor het uitvoeren van resources die door dit blauwdrukvoorbeeld zijn geïmplementeerd.

### <a name="artifact-parameters-table"></a>Tabel met artefactparameters

In de volgende tabel ziet u een lijst met de blauwdrukartefactparameters:

|Naam van het artefact|Type artefact|Parameternaam|Beschrijving|
|-|-|-|-|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met gebruikers die zijn uitgesloten van de groep beheerders voor Windows-VM's|Een lijst met leden die niet moeten worden opgenomen in de lokale groep beheerders, gescheiden door puntkomma's. Bijvoorbeeld: Beheerder; myUser1; myUser2|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met gebruikers die moeten worden opgenomen in de groep beheerders voor Windows-VM's|Een lijst met leden die moeten worden opgenomen in de lokale groep beheerders, gescheiden door puntkomma's. Bijvoorbeeld: Beheerder; myUser1; myUser2|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met gebruikers die de groep beheerders voor Windows-VM's *alleen* mag bevatten|Een lijst met alle verwachte leden van de lokale beheerdersgroep, gescheiden door puntkomma's. Bijvoorbeeld: Beheerder; myUser1; myUser2|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met regio's waar Network Watcher moet worden ingeschakeld|Als u een volledige lijst met regio's wilt weergeven, gebruikt u Get-AzLocation|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Virtueel netwerk waarin VM's moeten worden verbonden|Voorbeeld: /subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroupName/providers/Microsoft.Network/virtualNetworks/Name|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Netwerkgateway die door virtuele netwerken moet worden gebruikt|Voorbeeld: /subscriptions/YourSubscriptionId/resourceGroups/YourResourceGroup/providers/Microsoft.Network/virtualNetworkGateways/Name|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met werkruimte-id's waar Log Analytics-agents verbinding mee moeten maken|Een door puntkomma's gescheiden lijst met de werkruimte-id's waarmee de Log Analytics-agent moet zijn verbonden|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Lijst met resourcetypen waarvoor diagnostische logboeken moeten zijn ingeschakeld|Diagnostische controle-instelling voor geselecteerde resourcetypen|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Meest recente PHP-versie|De nieuwste ondersteunde PHP-versie voor App Services|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Meest recente Java-versie|De nieuwste ondersteunde Java-versie voor App Services|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Meest recente Windows Python-versie|De nieuwste ondersteunde Python-versie voor App Services|
|Aanbevelingen voor Azure Security-benchmark controleren en specifieke ondersteunde VM-extensies implementeren|Beleidstoewijzing|Meest recente Linux Python-versie|De nieuwste ondersteunde Python-versie voor App Services|

## <a name="next-steps"></a>Volgende stappen

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md).