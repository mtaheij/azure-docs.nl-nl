---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793329"
---
Als u de [eindpunt status](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) in de rest API de status van de eind punten en de laatste bekende fout geeft om de reden voor het identificeren van een eind punt niet in orde is. De volgende tabel bevat de meest voorkomende fouten.

|Laatst bekende fout|Beschrijving/wanneer deze plaatsvindt|Mogelijke beperking|
|-----|-----|-----|
|Wijk|Er is een tijdelijke fout opgetreden en IoT Hub voert de bewerking opnieuw uit.|Bekijk routes [Diagnostische logboeken](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes).|
|InternalError|Er is een fout opgetreden tijdens het leveren van een bericht aan een eind punt.|Dit is een interne uitzonde ring, maar ook de routes [Diagnostische logboeken](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)bekijken.|
|Niet geautoriseerd|IoT Hub is niet gemachtigd om berichten naar het opgegeven eind punt te verzenden.|Controleer of de connection string up-to-date is voor het eind punt. Als deze is gewijzigd, kunt u een update op uw IoT Hub. Als het eind punt beheerde identiteit gebruikt, controleert u of de IoT Hub principal de vereiste machtigingen voor het doel heeft.|
|Beperkt|IoT Hub wordt beperkt tijdens het schrijven van berichten naar het eind punt.|Controleer de beperkings limieten voor het betreffende eind punt. Pas de configuraties voor het eind punt zodanig aan dat deze zo nodig worden geschaald.|
|Time-out|Time-out van bewerking.|Voer de bewerking opnieuw uit.|
|Niet gevonden|Doel resource bestaat niet.|Zorg ervoor dat de doel resource bestaat.|
|Kan de container niet vinden|De opslag container bestaat niet.|Zorg ervoor dat de opslag container bestaat.|
|Container uitgeschakeld|De opslag container is uitgeschakeld.|Zorg ervoor dat de opslag container is ingeschakeld.|
|MaxMessageSizeExceeded|Bericht routering heeft een maximale bericht grootte van 256 KB. de bericht grootte die wordt gerouteerd, overschrijdt deze limiet.|Controleer of de bericht grootte kan worden verminderd met minder toepassings eigenschappen of minder bericht verrijkingen.|
|PartitioningAndDuplicateDetectionNotSupported|Er is geen duplicaten detectie ingeschakeld voor service bus.|Schakel duplicaten detectie uit Service Bus uit of overweeg het gebruik van een entiteit zonder duplicaten detectie.|
|SessionfulEntityNotSupported|Voor service bus zijn mogelijk geen sessies ingeschakeld.|Schakel de sessie uit Service Bus of overweeg het gebruik van een entiteit zonder sessies.|
|NoMatchingSubscriptionsForMessage|Er is geen abonnement op het schrijven van een bericht in het service bus-onderwerp.|Een abonnement maken voor IoT Hub berichten waarnaar moet worden doorgestuurd.|
|EndpointExternallyDisabled|Het eind punt bevindt zich niet in een actieve status, dus IoT Hub kan berichten verzenden.|Schakel het eind punt in om het terug te zetten naar de actieve status.|
|DeviceMaximumQueueDepthExceeded|De service bus-grootte limiet is bereikt.|Overweeg het verwijderen van berichten van de doel Event Hubs zodat nieuwe berichten kunnen worden opgenomen in de Event Hubs.|