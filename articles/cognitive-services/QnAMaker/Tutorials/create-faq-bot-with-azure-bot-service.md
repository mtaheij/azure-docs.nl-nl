---
title: 'Zelfstudie: Een FAQ-bot maken met Azure Bot Service'
description: In deze zelfstudie maakt u zonder code te schrijven een FAQ-bot met QnA Maker en Azure Bot Service.
ms.topic: tutorial
ms.date: 08/31/2020
ms.openlocfilehash: 0ec3b381c4361234b29717164b6b718aff0ae294
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652246"
---
# <a name="tutorial-create-an-faq-bot-with-azure-bot-service"></a>Zelfstudie: Een FAQ-bot maken met Azure Bot Service
Een FAQ-bot maken met QnA Maker en Azure [Bot Service](https://azure.microsoft.com/en-us/services/bot-service/) zonder code te schrijven.

In deze zelfstudie leert u het volgende:

<!-- green checkmark -->
> [!div class="checklist"]
> * Een QnA Maker Knowledge Base koppelen aan een Azure Bot Service
> * De bot implementeren
> * Chatten met de bot testen in een webgesprek
> * De bot in de ondersteunde kanalen oplichten

## <a name="create-and-publish-a-knowledge-base"></a>Een knowledge base maken

Volg de [quickstart](../Quickstarts/create-publish-knowledge-base.md) om een Knowledge Base te maken. Wanneer de Knowledge Base is gepubliceerd, komt u op de onderstaande pagina.

![Schermopname van geslaagde publicatie](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

## <a name="create-a-bot"></a>Een bot maken

Na het publiceren kunt u een bot maken op de pagina **Publiceren**:

* U kunt snel een aantal bots maken, die allemaal verwijzen naar dezelfde knowledge base voor verschillende regio's of abonnementen voor de individuele bots.
* Als u slechts één bot wilt gebruiken voor de knowledge base, gebruikt u de koppeling **Alle bots weergeven in Azure Portal** om een lijst met uw huidige bots weer te geven.

Wanneer u wijzigingen aanbrengt in de knowledge base en deze opnieuw publiceert, hoeft u geen verdere actie te ondernemen voor de bot. Deze is al geconfigureerd voor gebruik van de knowledge base en kan overweg met alle toekomstige wijzigingen in de knowledge base. Steeds wanneer u een knowledge base publiceert, worden alle bots die met de knowledge base zijn verbonden, automatisch bijgewerkt.

1. Selecteer in de QnA Maker-portal op de pagina **Publiceren** de optie **Bot maken**. Deze knop wordt alleen weergegeven nadat u de knowledge base hebt gepubliceerd.

    ![Schermopname van het maken van een bot](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Er wordt een nieuw browsertabblad geopend voor Azure Portal, met daarin de pagina Maken van de Azure Bot Service. De Azure Bot Service configureren. De bot en QnA Maker kunnen het abonnement voor de web-appservice delen, maar ze kunnen de web-app niet delen. Dit betekent dat de **naam van de app** voor de bot anders moet zijn dan de naam van de app voor de QnA Maker-service.

    * **Wel doen**
        * Wijzig de gebruikersnaam van de bot als deze niet uniek is.
        * Selecteer de SDK-taal. Nadat de bot is gemaakt, kunt u de code downloaden naar uw lokale ontwikkelomgeving en het ontwikkelingsproces voortzetten.
    * **Niet doen**
        * De volgende instellingen wijzigen in de Azure-portal als u de bot maakt. Deze zijn vooraf ingevuld voor uw bestaande knowledge base:
           * QnA-verificatiesleutel
           * App Service-plan en -locatie


1. Open de resource **Botservice** nadat de bot is gemaakt.
1. Selecteer onder **Botbeheer** **Testen in webchat**.
1. Voer in het chatbericht **Typ uw bericht** het volgende in:

    `Azure services?`

    De chatbot reageert met een antwoord uit uw knowledge base.

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/test-web-chat.png" alt-text="Voer een gebruikersquery in om de webchat testen.":::
1. De bot oplichten in aanvullende [ondersteunde kanalen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0&preserve-view=true).
