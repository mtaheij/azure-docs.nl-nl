---
title: Blauwdrukvoorbeeld voor Media implementeren
description: Implementeer stappen voor het blauwdrukvoorbeeld voor Media, inclusief de parametergegevens voor blauwdrukartefacten.
ms.date: 08/13/2020
ms.topic: sample
ms.openlocfilehash: c8ac1726bc5007c1790f5efbc45c72f9712fcdf2
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2020
ms.locfileid: "88208425"
---
# <a name="deploy-the-media-blueprint-sample"></a>Het blauwdrukvoorbeeld voor Media implementeren

Voor het implementeren van het blauwdrukvoorbeeld voor Media moeten de volgende stappen worden uitgevoerd:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken op basis van het voorbeeld
> - Uw kopie van het voorbeeld markeren als **Gepubliceerd**
> - Uw kopie van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-blueprint-from-sample"></a>Een blauwdruk maken op basis van een voorbeeld

Implementeer eerst het blauwdrukvoorbeeld door een nieuwe blauwdruk in uw omgeving te maken op basis van het voorbeeld.

1. Selecteer **Alle services** en zoek en selecteer **Beleid** in het linkerdeelvenster. Op de pagina **Beleid** selecteert u **Blauwdrukken**.

1. Op de pagina **Aan de slag** aan de linkerkant selecteert u de knop **Maken** onder _Een blauwdruk maken_.

1. Zoek het blauwdrukvoorbeeld voor **Media** onder _Andere voorbeelden_ en selecteer **Dit voorbeeld gebruiken**.

1. Voer de _Basisinstellingen_ van het blauwdrukvoorbeeld in:

   - **Naam van blauwdruk**: Geef een naam op voor uw kopie van het blauwdrukvoorbeeld.
   - **Definitielocatie**: Gebruik het weglatingsteken en selecteer de beheergroep waarin u uw kopie van het voorbeeld wilt opslaan.

1. Selecteer het tabblad _Artefacten_ boven aan de pagina of kies **Volgende: Artefacten** onder aan de pagina.

1. Controleer de lijst met artefacten die samen het blauwdrukvoorbeeld vormen. Veel van de artefacten bevatten parameters die we later zullen definiëren. Selecteer **Concept opslaan** wanneer u klaar bent met het controleren van het blauwdrukvoorbeeld.

## <a name="publish-the-sample-copy"></a>De voorbeeldkopie publiceren

Uw kopie van het blauwdrukvoorbeeld is nu gemaakt in uw omgeving. De kopie is gemaakt in de **Concept**-modus en moet worden **Gepubliceerd** voordat deze kan worden toegewezen en geïmplementeerd. De kopie van het blauwdrukvoorbeeld kan worden aangepast aan uw omgeving en behoeften, maar door die aanpassing is het mogelijk dat de kopie niet meer is afgestemd op de standaard.

1. Selecteer **Alle services** en zoek en selecteer **Beleid** in het linkerdeelvenster. Op de pagina **Beleid** selecteert u **Blauwdrukken**.

1. Selecteer de pagina **Blauwdrukdefinities** aan de linkerkant. Gebruik de filters om uw kopie van het blauwdrukvoorbeeld te zoeken en selecteer vervolgens uw kopie.

1. Selecteer **Blauwdruk publiceren** boven aan de pagina. Op de nieuwe pagina aan de rechterkant geeft u een **Versie** voor uw kopie van het blauwdrukvoorbeeld op. Deze eigenschap is handig als u later een aanpassing wilt maken. Geef **Notities over wijzigingen** op, zoals 'Eerste gepubliceerde versie op basis van het blauwdrukvoorbeeld voor Media.' Selecteer vervolgens **Publiceren** onder aan de pagina.

## <a name="assign-the-sample-copy"></a>De voorbeeldkopie toewijzen

Zodra de kopie van het blauwdrukvoorbeeld is **Gepubliceerd**, kan het worden toegewezen aan een abonnement binnen de beheergroep waarin de kopie is opgeslagen. Dit is de stap waarin parameters worden opgegeven om elke implementatie van de kopie van het blauwdrukvoorbeeld uniek te maken.

1. Selecteer **Alle services** en zoek en selecteer **Beleid** in het linkerdeelvenster. Op de pagina **Beleid** selecteert u **Blauwdrukken**.

1. Selecteer de pagina **Blauwdrukdefinities** aan de linkerkant. Gebruik de filters om uw kopie van het blauwdrukvoorbeeld te zoeken en selecteer vervolgens uw kopie.

1. Selecteer **Blauwdruk toewijzen** boven aan de pagina Blauwdrukdefinitie.

1. Geef de parameterwaarden op voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen in de beheergroep waarin u uw kopie van het blauwdrukvoorbeeld hebt opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing gemaakt voor elk abonnement waarvoor de ingevoerde parameters worden gebruikt.
     - **Naam van toewijzing**: De naam wordt vooraf voor u ingevuld op basis van de naam van de blauwdruk.
       Wijzig de naam als dat nodig is of gebruik de opgegeven naam.
     - **Locatie**: Selecteer een regio waarin u de beheerde identiteit wilt maken. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **Blauwdrukdefinitieversie**: Kies een **gepubliceerde** versie van uw kopie van het blauwdrukvoorbeeld.

   - Toewijzing vergrendelen

     Selecteer de instelling voor het vergrendelen van de blauwdruk voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de optie voor de standaard door het _systeem toegewezen_ beheerde identiteit staan.

   - Artefactparameters

     De parameters die in deze sectie worden gedefinieerd, zijn van toepassing op het artefact waaronder de parameter is gedefinieerd. Deze parameters zijn [dynamische parameters](../../concepts/parameters.md#dynamic-parameters), aangezien ze zijn gedefinieerd tijdens de toewijzing van de blauwdruk. Zie de [tabel met artefactparameters](#artifact-parameters-table) voor een volledige lijst met artefactparameters en hun beschrijvingen.

1. Zodra alle parameters zijn ingevoerd, selecteert u **Toewijzen** onder aan de pagina. De blauwdruktoewijzing wordt gemaakt en de implementatie van het artefact begint. De implementatie duurt circa één uur. Open de blauwdruktoewijzing om de status van de implementatie te controleren.

> [!WARNING]
> De Azure Blueprints-service en de ingebouwde blauwdrukvoorbeelden zijn **gratis**. Azure-resources zijn [geprijsd per product](https://azure.microsoft.com/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om de kosten te schatten voor het uitvoeren van resources die door dit blauwdrukvoorbeeld zijn geïmplementeerd.

## <a name="artifact-parameters-table"></a>Tabel met artefactparameters

In de volgende tabel ziet u een lijst met de blauwdrukartefactparameters:

Naam van het artefact|Type artefact|Parameternaam|Beschrijving|
|-|-|-|-|
|\[Preview\]: Log Analytics-agent voor virtuele Linux-machines implementeren |Beleidstoewijzing |Log Analytics-werkruimte voor virtuele Linux-machines |Raadpleeg [Een Log Analytics-werkruimte maken in Azure Portal](../../../../azure-monitor/learn/quick-create-workspace.md) voor meer informatie. |
|\[Preview\]: Log Analytics-agent voor virtuele Linux-machines implementeren |Beleidstoewijzing |Optioneel: Lijst met VM-installatiekopieën met ondersteuning van het Linux-besturingssysteem die aan het bereik kunnen worden toegevoegd |Een lege matrix kan worden gebruikt om aan te geven dat er geen optionele parameters zijn: `[]` |
|\[Preview\]: Log Analytics-agent voor Windows-VM's implementeren |Beleidstoewijzing |Optioneel: Lijst met VM-installatiekopieën met ondersteuning van het Windows-besturingssysteem die aan het bereik kunnen worden toegevoegd |Een lege matrix kan worden gebruikt om aan te geven dat er geen optionele parameters zijn: `[]` |
|\[Preview\]: Log Analytics-agent voor Windows-VM's implementeren |Beleidstoewijzing |Log Analytics-werkruimte voor Windows-VM's |Raadpleeg [Een Log Analytics-werkruimte maken in de Azure-portal](../../../../azure-monitor/learn/quick-create-workspace.md) voor meer informatie. |
|\[Preview\]: Media-controles en implementatie van specifieke VM-extensies implementeren ter ondersteuning van de controlevereisten controleren |Beleidstoewijzing |Id van de Log Analytics-werkruimte waarvoor VM's moeten worden geconfigureerd |Dit is de id (GUID) van de Log Analytics-werkruimte waarvoor de VM's moeten worden geconfigureerd. |
|\[Preview\]: Media-controles en implementatie van specifieke VM-extensies implementeren ter ondersteuning van de controlevereisten controleren |Beleidstoewijzing |Lijst met resourcetypen waarvoor diagnostische logboeken moeten zijn ingeschakeld |Lijst met resourcetypen die moeten worden gecontroleerd als de instelling voor diagnostische logboeken niet is ingeschakeld. Acceptabele waarden vindt u in de [schema's van diagnostische logboeken van Azure Monitor](../../../../azure-monitor/platform/resource-logs-schema.md#service-specific-schemas). |
|\[Preview\]: Media-controles en implementatie van specifieke VM-extensies implementeren ter ondersteuning van de controlevereisten controleren |Beleidstoewijzing |Beheerdersgroep |Groep. Voorbeeld: `Administrator; myUser1; myUser2` |
|\[Preview\]: Media-controles en implementatie van specifieke VM-extensies implementeren ter ondersteuning van de controlevereisten controleren |Beleidstoewijzing |Lijst met gebruikers die moeten worden opgenomen in de groep Beheerders voor Windows-VM's |Een lijst met leden die moeten worden opgenomen in de lokale groep beheerders, gescheiden door puntkomma's. Voorbeeld: `Administrator; myUser1; myUser2` |
|Advanced Threat Protection implementeren voor opslagaccounts |Beleidstoewijzing |Effect |Informatie over de effecten van het beleid vindt u bij [Inzicht in de effecten van Azure Policy](../../../policy/concepts/effects.md). |
|Controle op SQL-servers implementeren |Beleidstoewijzing |De waarde in dagen van de bewaarperiode (0 betekent een onbeperkte bewaarperiode) |Bewaarperiode in dagen (optioneel, _180_ dagen als u niets opgeeft) |
|Controle op SQL-servers implementeren |Beleidstoewijzing |De resourcegroepnaam voor het opslagaccount voor SQL Server-controles |De controle schrijft databasegebeurtenissen naar een auditlogboek in uw Azure Storage-account (er wordt een opslagaccount aangemaakt in elke regio waarin een SQL Server wordt gemaakt die wordt gedeeld door alle servers in die regio). Belangrijk: voor de juiste werking van de controle mag u geen resourcegroepen of opslagaccounts verwijderen of hernoemen. |
|Diagnostische instellingen voor netwerkbeveiligingsgroepen implementeren |Beleidstoewijzing |Het voorvoegsel van het opslagaccount voor netwerkbeveiligingsgroepdiagnoses |Dit voorvoegsel vormt in combinatie met de locatie van de netwerkbeveiligingsgroep de naam van het gemaakte opslagaccount. |
|Diagnostische instellingen voor netwerkbeveiligingsgroepen implementeren |Beleidstoewijzing |De resourcegroepnaam voor het opslagaccount voor netwerkbeveiligingsgroepdiagnoses (moet bestaan) |De resourcegroep waarin het opslagaccount wordt gemaakt. Deze resourcegroep moet al bestaan. |

## <a name="next-steps"></a>Volgende stappen

Nu u de stappen voor het implementeren van het blauwdrukvoorbeeld voor Media hebt bekeken, raadpleegt u de volgende artikelen voor informatie over het overzicht en de toewijzing van besturingselementen:

> [!div class="nextstepaction"]
> [Blauwdrukvoorbeelden voor Media - Overzicht](./index.md)
> [Blauwdrukvoorbeelden voor Media - Toewijzing van besturingselementen](./control-mapping.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
