---
title: Gepland onderhoud-Azure Database for PostgreSQL flexibele server
description: In dit artikel wordt de functie voor gepland onderhoud in Azure Database for PostgreSQL-flexibele server beschreven.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: ffee15776a48b6495f78b6becf81c620e1dc4d69
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336306"
---
# <a name="scheduled-maintenance-in-azure-database-for-postgresql--flexible-server"></a>Gepland onderhoud in Azure Database for PostgreSQL: flexibele server
 
Azure Database for PostgreSQL-flexibele server voert periodiek onderhoud uit om uw beheerde data base veilig, stabiel en up-to-date te houden. Tijdens het onderhoud krijgt de server nieuwe functies, updates en patches.
 
> [!IMPORTANT]
> Azure Database for PostgreSQL-flexibele server is als preview-versie beschikbaar
 
## <a name="selecting-a-maintenance-window"></a>Een onderhouds venster selecteren
 
U kunt onderhoud plannen gedurende een specifieke dag van de week en een tijd venster binnen die dag. Of u kunt ervoor zorgen dat het systeem een dag en tijd venster tijd automatisch voor u kiest. In beide gevallen waarschuwt het systeem u vijf dagen voordat er onderhoud wordt uitgevoerd. Het systeem laat u ook weten wanneer het onderhoud wordt gestart en wanneer het is voltooid.
 
Meldingen over gepland onderhoud kunnen het volgende zijn:
 
* Verzonden naar een specifiek adres
* E-mail verzonden naar een Azure Resource Manager rol
* Verzonden in een tekst bericht (SMS) naar mobiele apparaten
* Gepusht als een melding naar een Azure-app
* Bezorgd als een voicemail bericht
 
Wanneer u voorkeuren voor het onderhoudsschema opgeeft, kunt u een dag van de week en een tijdvenster kiezen. Als u geen voorkeuren opgeeft, kiest het systeem een tijd tussen 23:00 uur en 07:00 uur in de regiotijd van uw server. U kunt verschillende schema's definiëren voor elke flexibele server in uw Azure-abonnement. 
 
> [!IMPORTANT]
> Normaal gesproken zijn er ten minste 30 dagen tussen geslaagde geplande onderhoudsgebeurtenissen voor een server.
>
> In het geval van een essentiële nood-update, zoals een ernstig beveiligingsprobleem, kan het meldingsvenster echter korter zijn dan vijf dagen. De essentiële update kan worden toegepast op uw server, zelfs als in de afgelopen 30 dagen geslaagd gepland onderhoud is uitgevoerd.

U kunt de plannings instellingen op elk gewenst moment bijwerken. Als er een onderhouds planning is gepland voor uw flexibele server en u de plannings voorkeuren bijwerkt, wordt de huidige gebeurtenis uitgevoerd zoals gepland en wordt de wijziging in de plannings instellingen van kracht wanneer de voltooiing is voltooid. 

Als de onderhouds gebeurtenis door het systeem wordt geannuleerd of niet kan worden voltooid, wordt er door het systeem een melding over geannuleerde of mislukte onderhouds gebeurtenissen gemaakt. De volgende poging om onderhoud uit te voeren, wordt gepland volgens de huidige plannings instellingen en ontvangt vijf dagen vooraf een melding.
 
## <a name="next-steps"></a>Volgende stappen
 
* Meer informatie over [het wijzigen van de onderhouds planning](how-to-maintenance-portal.md)
* Meer informatie [over het verkrijgen van meldingen over aanstaande onderhouds werkzaamheden](../../service-health/service-notifications.md) met Azure service Health
* Meer informatie [over het instellen van waarschuwingen over aanstaande geplande onderhouds gebeurtenissen](../../service-health/resource-health-alert-monitor-guide.md)
