---
title: Server opnieuw starten-Azure Portal-Azure Database for MySQL
description: In dit artikel wordt beschreven hoe u een Azure Database for MySQL server opnieuw kunt opstarten met behulp van de Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: d885cc64eeebd4873ad5993b39b48845d1365c23
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902749"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Azure Database for MySQL server opnieuw opstarten met Azure Portal
In dit onderwerp wordt beschreven hoe u een Azure Database for MySQL server opnieuw kunt starten. Mogelijk moet u de server opnieuw opstarten om onderhouds redenen te zorgen, waardoor er een korte storing optreedt terwijl de server de bewerking uitvoert.

Het opnieuw opstarten van de server wordt geblokkeerd als de service bezet is. De service kan bijvoorbeeld een eerder aangevraagde bewerking verwerken, zoals het schalen van vCores.

De tijd die nodig is om opnieuw op te starten, is afhankelijk van het MySQL-herstel proces. Om de herstarttijd te verlagen, raden we u aan om de hoeveelheid activiteit die op de server plaatsvindt, te minimaliseren voordat de computer opnieuw wordt opgestart.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze hand leiding te volt ooien:
- Een [Azure database for mysql server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Server opnieuw opstarten uitvoeren

De MySQL-server wordt opnieuw gestart met de volgende stappen:

1. Selecteer uw Azure Database for MySQL server in het Azure Portal.

2. Klik op de werk balk van de pagina **overzicht** van de server op **opnieuw opstarten**.

   :::image type="content" source="./media/howto-restart-server-portal/2-server.png" alt-text="Azure Database for MySQL-overzicht-knop opnieuw opstarten":::

3. Klik op **Ja** om te bevestigen dat de server opnieuw wordt opgestart.

   :::image type="content" source="./media/howto-restart-server-portal/3-restart-confirm.png" alt-text="Azure Database for MySQL-opnieuw opstarten bevestigen":::

4. Houd er rekening mee dat de server status wordt gewijzigd in opnieuw opstarten.

   :::image type="content" source="./media/howto-restart-server-portal/4-restarting-status.png" alt-text="Azure Database for MySQL-start status":::

5. Het opnieuw opstarten van de server is voltooid.

   :::image type="content" source="./media/howto-restart-server-portal/5-restart-success.png" alt-text="Azure Database for MySQL: opnieuw opstarten geslaagd":::

## <a name="next-steps"></a>Volgende stappen

[Snelstartgids: Azure Database for MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
