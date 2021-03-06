---
title: Een functie maken die wordt uitgevoerd volgens een planning in azure
description: Ontdek hoe u in Azure een functie maakt die wordt uitgevoerd op basis van een schema dat u definieert.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 04/16/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 9e542c493c02174364072f91d092f05ad9ec69cf
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90973090"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Maak een functie in Azure die wordt geactiveerd door een timer

Meer informatie over het gebruik van Azure Functions om een functie zonder [Server](https://azure.microsoft.com/solutions/serverless/) te maken die wordt uitgevoerd op basis van een schema dat u definieert.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Uw nieuwe functie-app is klaar voor gebruik. Vervolgens maakt u een functie in de nieuwe functie-app.

:::image type="content" source="./media/functions-create-scheduled-function/function-app-create-success.png" alt-text="De functie-app is gemaakt." border="true":::

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Een door een timer geactiveerde functie maken

1. Selecteer in de functie-app **functies**en selecteer vervolgens **+ toevoegen** 

   :::image type="content" source="./media/functions-create-scheduled-function/function-add-function.png" alt-text="Een functie toevoegen in de Azure Portal." border="true":::

1. Selecteer de **trigger** sjabloon voor timer. 

    :::image type="content" source="./media/functions-create-scheduled-function/function-select-timer-trigger.png" alt-text="Selecteer de trigger timer in het Azure Portal." border="true":::

1. Configureer de nieuwe trigger met de instellingen zoals opgegeven in de tabel onder de afbeelding en selecteer vervolgens **functie maken**.

    :::image type="content" source="./media/functions-create-scheduled-function/function-configure-timer-trigger.png" alt-text="Scherm afbeelding toont de nieuwe functie pagina waarop de timer trigger sjabloon is geselecteerd." border="true":::
    
    | Instelling | Voorgestelde waarde | Beschrijving |
    |---|---|---|
    | **Naam** | Standaard | Bepaalt de naam van de door de timer geactiveerde functie. |
    | **Schema** | 0 \* /1 \* \* \*\* | Een [CRON-expressie](functions-bindings-timer.md#ncrontab-expressions) met zes velden aan de hand waarvan uw functie elke minuut wordt uitgevoerd. |

## <a name="test-the-function"></a>De functie testen

1. Selecteer in uw functie **code + test** en vouw de logboeken uit.

    :::image type="content" source="./media/functions-create-scheduled-function/function-test-timer-trigger.png" alt-text="Test de timer trigger in het Azure Portal." border="true":::

1. Controleer de uitvoering door de informatie weer te geven die naar de logboeken wordt geschreven.

    :::image type="content" source="./media/functions-create-scheduled-function/function-view-timer-logs.png" alt-text="Bekijk de trigger timer in de Azure Portal." border="true":::

U kunt het schema van de functie nu wijzigen zodat deze één keer per uur wordt uitgevoerd in plaats van elke minuut.

## <a name="update-the-timer-schedule"></a>Het timerschema bijwerken

1. Selecteer in de functie **integratie**. Hier definieert u de invoer-en uitvoer bindingen voor uw functie en stelt u ook de planning in. 

1. Selecteer **Timer (myTimer)**.

    :::image type="content" source="./media/functions-create-scheduled-function/function-update-timer-schedule.png" alt-text="Werk de timer planning bij in de Azure Portal." border="true":::

1. Werk de **plannings** waarde bij naar `0 0 */1 * * *` en selecteer vervolgens **Opslaan**.  

    :::image type="content" source="./media/functions-create-scheduled-function/function-edit-timer-schedule.png" alt-text="Het timerschema voor het bijwerken van functies in Azure Portal." border="true":::

U hebt nu een functie die één keer per uur wordt uitgevoerd, op het uur.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie gemaakt die wordt uitgevoerd op basis van een schema. Zie [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Code-uitvoering plannen met Azure Functions) voor meer informatie over timeractiveringen.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
