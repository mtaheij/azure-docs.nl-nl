---
title: Paginastijl aanpassen voor de verouderde ontwikkelaarsportal van API Management
titleSuffix: Azure API Management
description: Volg de stappen van deze snelstart voor het aanpassen van de stijl van de elementen in de ontwikkelaarsportal van Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 664686511df8f310295a9f6ed6bc689b3a999544
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/25/2020
ms.locfileid: "75430740"
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>De stijl van de pagina's van de ontwikkelaarsportal aanpassen

Er zijn drie gebruikelijke manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:
 
* [De inhoud van statische pagina's en pagina-indelingselementen bewerken](api-management-modify-content-layout.md)
* De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal (uitgelegd in deze handleiding)
* [De sjablonen bewerken die worden gebruikt voor pagina's die worden gegenereerd door de portal](api-management-developer-portal-templates.md) (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enzovoort)

In dit artikel leert u hoe u de stijl aanpast van de elementen op pagina's van de verouderde **ontwikkelaarsportal** en hoe u de wijzigingen weergeeft.

![stijl aanpassen](./media/modify-developer-portal-style/developer_portal.png)

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Vereisten

+ Informatie over de [terminologie van Azure API Management](api-management-terminology.md).
+ Voltooi de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi ook de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="customize-the-developer-portal"></a>De ontwikkelaarsportal aanpassen

1. Selecteer **Overzicht**.
2. Klik op de knop **Ontwikkelaarsportal (verouderd)** boven aan het venster **Overzicht**.
3. In de linkerbovenhoek van het scherm ziet u een pictogram dat bestaat uit twee schilderkwasten. Beweeg de muisaanwijzer over dit pictogram om het menu Portalaanpassing te openen.

    ![stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Selecteer **Stijlen** in het menu om het deelvenster Stijl aanpassen te openen.

    Alle elementen die u kunt aanpassen met behulp van **Stijlen** worden weergegeven op de pagina
5. Voer 'headings-color' in het veld **Wijzigen van de waarden om de weergave van het ontwikkelaarsportal aan te passen:** in.

    Het element **\@headings-color** wordt op de pagina weergegeven. Deze variabele bepaalt de kleur van de tekst.

    ![stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Klik op het veld voor de variabele **\@headings-color**. 
    
    De vervolgkeuzelijst Kleurenkiezer wordt geopend.
7. Selecteer een nieuwe kleur uit de vervolgkeuzelijst Kleurenkiezer.

    > [!TIP]
    > Een realtime-voorbeeld is beschikbaar voor alle wijzigingen. Er verschijnt een voortgangsindicator aan de bovenkant van het deelvenster Aanpassen. Na een paar seconden verandert de koptekst naar de zojuist geselecteerde kleur.

8. Selecteer **Publiceren** linksonder in het menu van het deelvenster Aanpassen.
9. Selecteer **Aanpassingen publiceren** om de wijzigingen openbaar beschikbaar te maken.

## <a name="view-your-change"></a>Uw wijziging bekijken

1. Ga naar de ontwikkelaarsportal.
2. Hier ziet u de wijziging die u hebt aangebracht.

## <a name="next-steps"></a>Volgende stappen

Misschien wilt u wel weten hoe u [de Azure API Management-ontwikkelaarsportal aanpast met behulp van sjablonen](api-management-developer-portal-templates.md).