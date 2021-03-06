---
title: Farms beheren
description: Hierin wordt beschreven hoe u Farms beheert
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 09144c4c35ab911b60931849807123608f2c3cdd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85847475"
---
# <a name="manage-farms"></a>Een boerderij beheren

U kunt uw farms beheren in azure FarmBeats. In dit artikel vindt u informatie over het maken van Farms, het installeren van apparaten, Sens oren en drones waarmee u uw Farms kunt beheren.

## <a name="create-farms"></a>Farms maken

Voer de volgende stappen uit:

1. Meld u aan bij de farm Accelerator. de pagina **Farms** wordt weer gegeven.
    Op de pagina **Farms** wordt de lijst met farms weer gegeven als deze al zijn gemaakt in het abonnement.

    Hier volgt een voor beeld van de afbeelding:

    ![Maten van project-Farm](./media/create-farms-in-azure-farmbeats/create-farm-main-page-1.png)


2. Selecteer **farm maken** en geef **naam**, **bijsnijds** en **adres**op.
3. Selecteer in de grens van de **Farm definiëren**(verplicht veld) de optie **voor het koppelen** of **geojson-code**markeren.

Dit zijn de twee manieren om een farm grens te definiëren:

1. **Op kaart markeren**: gebruik het kaart beheer programma om de grens van de farm te tekenen en te markeren. Als u de grenzen wilt markeren, ![ project Farm maten ](./media/create-farms-in-azure-farmbeats/pencil-icon-1.png) en markeert u de exacte grenzen.

    ![Maten van project-Farm](./media/create-farms-in-azure-farmbeats/create-farm-mark-on-map-1.png)

2. **Geojson-code plakken**: de geojson is een indeling voor het coderen van geografische gegevens structuren, met behulp van JavaScript object NOTATION (JSON). Met deze optie wordt een tekstvak weer gegeven waarin een geojson-teken reeks kan worden ingevoerd om de grenzen van de farm te markeren. U kunt ook geojson-code maken op basis van GeoJSON.io.
Gebruik de knop Info om de informatie te vullen.

    ![Maten van project-Farm](./media/create-farms-in-azure-farmbeats/create-new-farm-1.png)

3.  Selecteer **verzenden** om een farm te maken. Er wordt een nieuwe farm gemaakt en weer gegeven op de pagina **Farms** .

## <a name="view-farm"></a>Farm weer geven

Op de lijst pagina met farms wordt een lijst met gemaakte Farms weer gegeven. Selecteer een farm om de lijst met te bekijken:

 - **Aantal** apparaten: geeft het aantal en de status weer van apparaten die in de farm zijn geïmplementeerd.
 - **Kaart** : kaart van de farm met de apparaten die in de farm zijn geïmplementeerd.
 - **Telemetrie** : geeft de telemetrie weer van de Sens oren die in de farm zijn geïmplementeerd.
 - **Nieuwste precisie kaarten** : geeft de meest recente satelliet indices-kaart (Evi, NDWI), bodem vocht heatmap en sensor plaatsing kaart.

## <a name="edit-farm"></a>Farm bewerken

Op de pagina **Farms** wordt een lijst met gemaakte Farms weer gegeven.

1.  Selecteer een farm om de farm weer te geven en te bewerken.
2.  Selecteer **Farm bewerken** om de farm gegevens te bewerken. In het venster **Farm Details** kunt u de grens velden voor de **naam**, het **gewas**, het **adres**en de **Farm** definitie bewerken.

    ![Maten van project-Farm](./media/create-farms-in-azure-farmbeats/edit-farm-1.png)

3. Selecteer **verzenden** om de bewerkte details op te slaan.

## <a name="delete-farm"></a>Farm verwijderen

Op de pagina **Farms** wordt een lijst met gemaakte Farms weer gegeven. Gebruik de volgende stappen om een farm te verwijderen:

1.  Selecteer een farm in de lijst om de farm gegevens te verwijderen.
2.  Selecteer **farm verwijderen** om de farm te verwijderen.

    ![Maten van project-Farm](./media/create-farms-in-azure-farmbeats/delete-farm-1.png)

    > [!NOTE]
    > Wanneer u een farm verwijdert, worden de apparaten en toewijzingen die zijn gekoppeld aan de farm niet verwijderd. Alle farm gegevens die aan het apparaat en de kaarten zijn gekoppeld, zijn niet relevant. U kunt door gaan met het weer geven van apparaten, telemetrie en kaarten van de FarmBeats-service.


## <a name="next-steps"></a>Volgende stappen

Nu u uw farm hebt gemaakt, leert u hoe u [sensor gegevens kunt ophalen](get-sensor-data-from-sensor-partner.md) in uw farm.
