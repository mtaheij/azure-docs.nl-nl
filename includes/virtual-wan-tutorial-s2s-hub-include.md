---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 105ab0c71d9e7e935842550ecdc4c8d2ff2a2d8c
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977915"
---
1. Zoek naar de virtuele WAN die u hebt gemaakt. Op de pagina Virtuele WAN, onder het gedeelte **Connectiviteit**, selecteert u **Hubs**.
2. Op de pagina Hubs selecteert u **+Nieuwe hub** om de pagina **Virtuele hub maken** te openen.

    ![Basisinstellingen](./media/virtual-wan-tutorial-hub-include/basics.png "Basisbeginselen")
3. Op de pagina **Basisinstellingen** van de pagina **Virtuele hub maken** vult u de volgende velden in:

    **Projectgegevens**

   * Regio (voorheen Locatie genoemd)
   * Naam
   * Privé-adresruimte van hub. De minimale adresruimte is /24 voor het maken van een hub, wat impliceert dat alles tussen /25 en /32 een fout produceert tijdens het maken. Azure Virtual WAN is een beheerde service van Microsoft en maakt de juiste subnetten in de virtuele hub voor de verschillende gateways/services (bijvoorbeeld VPN-gateways, ExpressRoute-gateways, gebruikers-VPN/punt-naar-site gateways, firewall, routering, enzovoort). De gebruiker hoeft niet uitdrukkelijk de adresruimte van het subnet voor de services in de virtuele hub in te plannen, aangezien Microsoft dit al doet als onderdeel van de service.
4. Selecteer **Volgende: Site-naar-site**

    ![Site-naar-site](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Site-naar-site")

5. Vul op het tabblad **Site-naar-site** de volgende velden in:

   * Selecteer **Ja** om een Site-naar-site VPN-verbinding te maken.
   * Het veld AS-nummer kan op dit moment niet in de virtuele hub worden bewerkt.
   * Selecteer de **Gateway-schaaleenheden** in de vervolgkeuzelijst. Met de schaaleenheid kunt u de geaggregeerde doorvoer van de VPN-gateway kiezen die wordt gemaakt in de virtuele hub waarmee sites worden verbonden. Als u 1 schaaleenheid = 500 Mbps kiest, betekent dit dat er twee exemplaren voor redundantie worden gemaakt, elk met een maximale doorvoer van 500 Mbps. Als u bijvoorbeeld vijf vertakkingen hebt, elk met een doorvoer van 10 Mbps, hebt u aan het eind van de vertakkingen een totaal van 50 Mbps nodig. Het plannen van de totale capaciteit van de Azure VPN-gateway moet worden uitgevoerd na het beoordelen van de capaciteit die nodig is om het aantal branches naar de hub te ondersteunen.
6. Selecteer **Beoordelen en maken** om de validatie uit te voeren.
7. Selecteer **Maken** om de hub te maken. Na 30 minuten selecteert u **Vernieuwen** om de hub op de pagina **Hubs** te bekijken. Selecteer **Naar resource gaan** om naar de resource te gaan.
