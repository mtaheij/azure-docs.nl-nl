---
title: Apache Spark toepassingen bewaken in Synapse Studio
description: Meer informatie over het bewaken van uw Apache Spark-toepassingen met behulp van Synapse Studio.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 2b8dbd20e79b9a6f48ca2d39079ebb452a3b46b2
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530808"
---
# <a name="use-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Synapse Studio (preview) gebruiken om uw Apache Spark-toepassingen te bewaken

Met Azure Synapse Analytics kunt u Spark gebruiken om notitie blokken, taken en andere soorten toepassingen uit te voeren in uw Spark-Pools in uw werk ruimte.

In dit artikel wordt uitgelegd hoe u uw Apache Spark-toepassingen kunt bewaken, zodat u de meest recente status, problemen en voortgang kunt blijven gebruiken.

## <a name="access-apache-spark-applications-list"></a>Lijst met toegang tot Apache Spark toepassingen

Als u de lijst met Apache Spark toepassingen in uw werk ruimte wilt zien, opent u eerst [de Synapse Studio](https://web.azuresynapse.net/) en selecteert u uw werk ruimte.

![Aanmelden bij werk ruimte](./media/common/login-workspace.png)

Wanneer u uw werk ruimte hebt geopend, selecteert u de sectie **monitor** aan de linkerkant.

![Hub bewaken selecteren](./media/common/left-nav.png)

Selecteer **Apache Spark toepassingen** om de lijst met Apache Spark toepassingen weer te geven.

 ![Spark-toepassingen selecteren](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filter-your-apache-spark-applications"></a>Uw Apache Spark-toepassingen filteren

U kunt de lijst met Apache Spark toepassingen filteren op degene die u wilt. Met de filters boven aan het scherm kunt u een veld opgeven waarop u wilt filteren.

U kunt bijvoorbeeld de weer gave filteren om alleen de Apache Spark toepassingen te zien die de naam ' verkoop ' bevatten:

![Knop filteren](./media/common/filter-button.png)

![Voorbeeld filter](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="view-details-about-a-specific-apache-spark-application"></a>Details over een specifieke Apache Spark-toepassing weer geven

Als u de details van een van uw Apache Spark toepassingen wilt weer geven, selecteert u de Apache Spark toepassing en bekijkt u de details. Als de Apache Spark toepassing nog steeds wordt uitgevoerd, kunt u de voortgang bewaken. [Meer informatie](apache-spark-applications.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het bewaken van pipeline-uitvoeringen het artikel [Synapse Studio](how-to-monitor-pipeline-runs.md) . 

Zie voor meer informatie over het opsporen van fouten Apache Spark toepassing het artikel [Apache Spark toepassingen bewaken in Synapse Studio](apache-spark-applications.md) .