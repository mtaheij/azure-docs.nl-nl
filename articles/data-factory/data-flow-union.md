---
title: Data flow-vakbonds transformatie toewijzen
description: Nieuwe vertakkings transformatie Azure Data Factory toewijzings gegevens stroom
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 04/27/2020
ms.openlocfilehash: 1874e3536a3dda123c7fff9726c02e5d92d88804
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82204335"
---
# <a name="azure-data-factory-mapping-data-flow-union-transformation"></a>Data flow-vakbonds transformatie Azure Data Factory koppelen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Samen voegen meerdere gegevens stromen met elkaar samen, met de SQL-Unie van deze stromen als de nieuwe uitvoer van de samenvoeg transformatie. Alle schema's van elke invoer stroom worden gecombineerd in uw gegevens stroom, zonder dat hiervoor een koppelings sleutel nodig is.

U kunt n-aantal streams combi neren in de instellingen tabel door het pictogram ' + ' te selecteren naast elke geconfigureerde rij, inclusief zowel bron gegevens als stromen van bestaande trans formaties in uw gegevens stroom.

Hier volgt een korte video-overzicht van de samenvoeg transformatie in de stroom voor het toewijzen van gegevens van ADF:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4vngz]

![Union-transformatie](media/data-flow/union.png "Union")

In dit geval kunt u ongelijksoortige meta gegevens van meerdere bronnen combi neren (in dit voor beeld drie verschillende bron bestanden) en deze combi neren in één stroom:

![Overzicht van Union-trans formatie](media/data-flow/union111.png "Vereniging 1")

U kunt dit doen door extra rijen toe te voegen aan de vakbonds instellingen door alle bron toe te voegen die u wilt toevoegen. Er is geen algemene zoek-of koppelings sleutel nodig:

![Instellingen voor de trans formatie van vakbond](media/data-flow/unionsettings.png "Instellingen voor samen voegen")

Als u een SELECT-trans formatie hebt ingesteld na de samen voeging, kunt u de namen van overlappende velden of velden die niet zijn benoemd vanuit bronnen zonder koptekst. Klik op controleren om de kolommen gecombineerde meta gegevens met 132 totaal in dit voor beeld uit drie verschillende bronnen weer te geven:

![Voltooide Union-trans formatie](media/data-flow/union333.png "Vereniging 3")

## <a name="name-and-position"></a>Naam en positie

Wanneer u "Union op naam" kiest, wordt elke kolom waarde uit de bijbehorende kolom van elke bron verwijderd, met een nieuw samengevoegd meta gegevens schema.

Als u vereniging op positie kiest, wordt elke kolom waarde uit de oorspronkelijke positie van elke overeenkomende bron verwijderd, wat resulteert in een nieuwe gecombineerde stroom van gegevens, waarbij de gegevens van elke bron aan dezelfde stroom worden toegevoegd:

![Uitvoer Union](media/data-flow/unionoutput.png "Uitvoer Union")

## <a name="next-steps"></a>Volgende stappen

Verken vergelijk bare trans formaties, waaronder [samen voegen](data-flow-join.md) en [bestaan](data-flow-exists.md).
