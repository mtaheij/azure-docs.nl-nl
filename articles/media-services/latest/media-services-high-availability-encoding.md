---
title: Hoge Beschik baarheid met Media Services en video on demand (VOD)
description: Dit artikel bevat een overzicht van de Azure-Services die u kunt gebruiken om maximale Beschik baarheid voor de VOD-toepassing te vergemakkelijken.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: conceptual
ms.custom: ''
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: c3aaba6939f9e5e3f5d7c169cd3a199cc93f527d
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89296767"
---
# <a name="high-availability-with-media-services-and-video-on-demand-vod"></a>Hoge Beschik baarheid met Media Services en video on demand (VOD)

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="high-availability-for-vod"></a>Hoge Beschik baarheid voor VOD

Er is een ontwerp patroon met hoge Beschik baarheid met de naam [Geodes](/azure/architecture/patterns/geodes) in de documentatie van de Azure-architectuur. Hierin wordt beschreven hoe dubbele resources worden geïmplementeerd in verschillende geografische regio's om te voorzien in schaal baarheid en tolerantie.  U kunt Azure-Services gebruiken om een dergelijke architectuur te maken voor een groot aantal ontwerp overwegingen voor hoge Beschik baarheid, zoals redundantie, status controle, taak verdeling en gegevens back-up en herstel.  Hieronder wordt een van deze architectuur beschreven, met details over elke service die in de oplossing wordt gebruikt en hoe de afzonderlijke services kunnen worden gebruikt voor het maken van een architectuur met hoge Beschik baarheid voor uw VOD-toepassing.

### <a name="sample"></a>Voorbeeld

Er is een voor beeld dat u kunt gebruiken om vertrouwd te raken met hoge Beschik baarheid met Media Services en video on demand (VOD). Er wordt ook meer informatie over hoe de services worden gebruikt voor een VOD-scenario.  Het voor beeld is niet bedoeld om te worden gebruikt in de huidige vorm van productie.  Bekijk zorgvuldig de voorbeeld code en het Leesmij-bestand, met name de sectie over [fout modi](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/HighAvailabilityEncodingStreaming) voordat u deze integreert in een productie toepassing.  Voor een productie-implementatie van hoge Beschik baarheid voor video on demand (VOD) moet ook de CDN-strategie (Content Delivery Network) zorgvuldig worden geëvalueerd.  Bekijk de [code op github](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/HighAvailabilityEncodingStreaming).

## <a name="overview-of-services"></a>Overzicht van services

De services die in deze voorbeeld architectuur worden gebruikt, zijn:

| Pictogram | Naam | Beschrijving |
| :--: | ---- | ----------- |
|![image](media/media-services-high-availability-encoding/azure-media-services.svg)| Media Services-account | **Beschrijving:**<br>Een Media Services account is het begin punt voor het beheren, versleutelen, coderen, analyseren en streaming van media-inhoud in Azure. Het is gekoppeld aan een Azure Storage-account bron. Het account en alle gekoppelde opslag moeten zich in hetzelfde Azure-abonnement bevallen.<br><br>**VOD gebruik:**<br>Dit zijn de services die u gebruikt om uw video-en audio-assets te coderen en te leveren.  Voor maximale Beschik baarheid moet u ten minste twee Media Services accounts instellen, elk in een andere regio. Meer [informatie over Azure Media Services](media-services-overview.md). |
|![image](media/media-services-high-availability-encoding/storage-account.svg)| Storage-account | **Beschrijving:**<br>Een Azure-opslag account bevat al uw Azure Storage gegevens objecten: blobs, bestanden, wacht rijen, tabellen en schijven. Gegevens zijn overal ter wereld toegankelijk via HTTP of HTTPS.<br><br>Elk Media Services account heeft in elke regio een opslag account in dezelfde regio.<br><br>**VOD gebruik:**<br>U kunt invoer-en uitvoer gegevens opslaan voor VOD verwerking en streaming. Meer [informatie over Azure Storage](../../storage/common/storage-introduction.md). |
|![image](media/media-services-high-availability-encoding/storage-account-queue.svg)| Azure Storage-wachtrij | **Beschrijving:**<br>Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen.<br><br>**VOD gebruik:**<br>Wacht rijen kunnen worden gebruikt voor het verzenden en ontvangen van berichten om activiteiten tussen verschillende modules te coördineren. In het voor beeld wordt gebruikgemaakt van een Azure Storage wachtrij, maar Azure biedt ook andere typen wacht rijen, zoals Service Bus en Service Fabric betrouw bare wacht rijen die beter aansluiten bij uw behoeften. [Meer informatie over Azure Queue](../../storage/queues/storage-queues-introduction.md). |
|![image](media/media-services-high-availability-encoding/azure-cosmos-db.svg)| Azure Cosmos DB  | **Beschrijving:**<br>Azure Cosmos DB is de wereld wijd gedistribueerde, multi-model database service van micro soft die de door Voer en opslag onafhankelijk van elk wille keurig aantal Azure-regio's wereld wijd schaalt.<br><br>**VOD gebruik:**<br>Tabellen kunnen worden gebruikt voor het opslaan van taak uitvoer status records en voor het bijhouden van de status van elk Media Services exemplaar. U kunt ook de status van elke aanroep bijhouden/vastleggen in de Media Services-API. Meer [informatie over Azure Cosmos DB](../../cosmos-db/introduction.md).  |
|![image](media/media-services-high-availability-encoding/managed-identity.svg)| Beheerde identiteit | **Beschrijving:**<br>Beheerde identiteit is een functie van Azure AD die automatisch beheerde identiteiten in azure AD biedt. Het kan worden gebruikt voor verificatie bij elke service die ondersteuning biedt voor Azure AD-verificatie, met inbegrip van Key Vault, zonder dat u referenties in code hoeft op te slaan.<br><br>**VOD gebruik:**<br>Azure Functions kunt beheerde identiteit gebruiken om te verifiëren Media Services exemplaren om verbinding te maken met Key Vault. Meer [informatie over beheerde identiteit](../../active-directory/managed-identities-azure-resources/overview.md). |
|![image](media/media-services-high-availability-encoding/key-vault.svg)| Key Vault | **Beschrijving:**<br>Azure Key Vault kan worden gebruikt om de toegang tot tokens, wacht woorden, certificaten, API-sleutels en andere geheimen veilig op te slaan en nauw keurig te beheren. Het kan ook worden gebruikt als een oplossing voor sleutel beheer. Met Azure Key Vault kunt u eenvoudig de versleutelingssleutels maken en beheren waarmee uw gegevens worden versleuteld. Het kan eenvoudig open bare en persoonlijke Transport Layer Security/Secure Sockets Layer (TLS/SSL)-certificaten inrichten, beheren en implementeren voor gebruik met Azure en interne verbonden resources. Geheimen en sleutels kunnen worden beveiligd door software of het FIPS 140-2 level 2-gevalideerde Hsm's.<br><br>**VOD gebruik:**<br>Key Vault kunnen worden gebruikt voor het instellen van toegangs beleid voor de service-principal voor uw toepassing.  Het kan worden gebruikt om de connection string op te slaan voor opslag accounts. We gebruiken Key Vault om verbindings reeksen op te slaan in opslag accounts en Cosmos db. U kunt ook Key Vault gebruiken om de algehele cluster configuratie op te slaan. Voor elk exemplaar van media service kunt u de abonnements-ID, de naam van de resource groep en de account naam opslaan. Zie hoe het wordt gebruikt in het voor beeld voor meer informatie. Meer [informatie over Key Vault](../../key-vault/general/overview.md). |
|![image](media/media-services-high-availability-encoding/function-app.svg)| Azure Functions | **Beschrijving:**<br>Voer kleine stukjes code (' functions ') uit zonder dat u zich zorgen hoeft te maken over de infra structuur van toepassingen met Azure Functions. Meer [informatie over Azure functions](../../azure-functions/functions-overview.md).<br><br>**VOD gebruik:**<br>Azure Functions kan worden gebruikt om host op te slaan van de modules van uw VOD-toepassing.  Modules voor een VOD-toepassing kunnen het volgende omvatten:<br><br>**Module voor taak planning**<br>De module taak planning is voor het indienen van nieuwe taken in een Media Services cluster (twee of meer exemplaren in verschillende regio's). Hiermee wordt de status van elk Media Services exemplaar gevolgd en wordt een nieuwe taak verzonden naar het volgende gezonde exemplaar.<br><br>**Module taak status**<br>De taak status module luistert naar taak uitvoer status gebeurtenissen die afkomstig zijn van Azure Event Grid Service. De gebeurtenissen worden opgeslagen in het gebeurtenis archief om het aantal aanroepen naar Media Services-Api's door andere modules te minimaliseren.<br><br>**Status module exemplaar**<br>Met deze module worden de ingediende taken gevolgd en wordt de status van elke Media Services-instantie bepaald. U kunt voltooide taken, mislukte taken en taken die nog niet zijn voltooid, bijhouden.<br><br>**Inrichtings module**<br>Met deze module worden verwerkte assets ingericht. De Asset-gegevens worden naar alle Media Services-exemplaren gekopieerd en de Azure front-deur service ingesteld om ervoor te zorgen dat activa kunnen worden gestreamd, zelfs als sommige Media Services exemplaren niet beschikbaar zijn. Er worden ook streaming-Locators ingesteld.<br><br>**Module voor taak verificatie**<br>Met deze module kunt u elke ingediende taak volgen, mislukte taken opnieuw verzenden en taak gegevens opschonen zodra een taak is voltooid.  |
|![image](media/media-services-high-availability-encoding/application-service.svg)| App Service (en plan)  | **Beschrijving:**<br>Azure App Service is een op HTTP gebaseerde service voor het hosten van webtoepassingen, REST API's en mobiele back-ends. .NET, .NET core, Java, Ruby, Node.js, PHP of python worden ondersteund. Toepassingen worden op zowel Windows-als Linux-omgevingen uitgevoerd en geschaald.<br><br>**VOD gebruik:**<br>Elke module wordt gehost door een App Service. Meer [informatie over app service](../../app-service/overview.md). |
|![image](media/media-services-high-availability-encoding/azure-front-door.svg)| Azure Front Door | **Beschrijving:**<br>Azure front deur wordt gebruikt voor het definiëren, beheren en bewaken van de wereld wijde route ring van webverkeer door te optimaliseren voor de beste prestaties en snelle globale failover voor hoge Beschik baarheid.<br><br>**VOD gebruik:**<br>De voor deur van Azure kan worden gebruikt voor het routeren van verkeer naar streaming-eind punten. Meer [informatie over de voor deur van Azure](../../frontdoor/front-door-overview.md).  |
|![image](media/media-services-high-availability-encoding/event-grid-subscription.svg)| Azure Event Grid | **Beschrijving:**<br>Event Grid is gemaakt voor architecturen op basis van gebeurtenissen, heeft ingebouwde ondersteuning voor gebeurtenissen die afkomstig zijn van Azure-Services, zoals opslag-blobs en resource groepen. Het biedt ook ondersteuning voor aangepaste topic-gebeurtenissen. Filters kunnen worden gebruikt voor het routeren van specifieke gebeurtenissen naar verschillende eind punten, multi cast naar meerdere eind punten en om ervoor te zorgen dat gebeurtenissen op een betrouw bare manier worden bezorgd. Het maximaliseert de beschik baarheid door systeem eigen verspreiding uit te breiden over meerdere fout domeinen in elke regio en in verschillende beschikbaarheids zones.<br><br>**VOD gebruik:**<br>Event Grid kan worden gebruikt om alle toepassings gebeurtenissen bij te houden en op te slaan om de taak status te behouden. Meer [informatie over Azure Event grid](../../event-grid/overview.md). |
|![image](media/media-services-high-availability-encoding/application-insights.svg)| Application Insights | **Beschrijving:** <br>Application Insights, een functie van Azure Monitor, is een flexibele APM-service (Application Performance Management) voor ontwikkelaars en DevOps-professionals. Het wordt gebruikt voor het bewaken van Live-toepassingen. Het detecteert prestatie afwijkingen en bevat hulpprogram ma's voor analyse om problemen te diagnosticeren en te begrijpen wat gebruikers met een app doen. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.<br><br>**VOD gebruik:**<br>Alle logboeken kunnen worden verzonden naar Application Insights. Het is mogelijk om te zien welk exemplaar elke taak heeft verwerkt door te zoeken naar geslaagde taak berichten. Dit kan alle verzonden taak-meta gegevens bevatten, inclusief de unieke id en de naam van de exemplaar gegevens. Meer [informatie over Application Insights](../../azure-monitor/app/app-insights-overview.md). |
## <a name="architecture"></a>Architectuur

Dit diagram op hoog niveau toont de architectuur van het voor beeld om u op weg te helpen met hoge Beschik baarheid en Media Services.

[![Video on demand (VOD)-architectuur diagram ](media/media-services-high-availability-encoding/high-availability-architecture.svg) op hoog niveau](media/media-services-high-availability-encoding/high-availability-architecture.svg#lightbox)

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="regions"></a>Regio's

* [Maak](https://review.docs.microsoft.com/azure/media-services/latest/create-account-cli-how-to) twee (of meer) Azure Media Services-accounts. De twee accounts moeten zich in verschillende regio's behoeven. Zie [regio's waarin de Azure Media Services-service is geïmplementeerd](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)voor meer informatie.
* Upload uw media naar de regio van waaruit u de taak wilt indienen. Zie [een taak invoer maken op basis van een HTTPS-URL](https://review.docs.microsoft.com/azure/media-services/latest/job-input-from-http-how-to) of [een taak invoer maken op basis van een lokaal bestand](https://review.docs.microsoft.com/azure/media-services/latest/job-input-from-local-file-how-to)voor meer informatie over het starten van code ring.
* Als u de [taak](https://review.docs.microsoft.com/azure/media-services/latest/transforms-jobs-concept) vervolgens opnieuw moet verzenden naar een andere regio, kunt u gebruiken `JobInputHttp` of gebruiken `Copy-Blob` om de gegevens van de bron-Asset-container te kopiëren naar een activa container in de alternatieve regio.

### <a name="monitoring"></a>Bewaking

* Abonneer u op `JobStateChange` berichten in elk account via Azure Event grid.
    * [Registreer u voor gebeurtenissen](https://review.docs.microsoft.com/azure/media-services/latest/reacting-to-media-services-events) via de Azure portal of cli (u kunt dit ook doen met de Event Grid Management SDK)
    * Gebruik de [SDK van micro soft. Azure. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (die systeem eigen ondersteuning biedt voor Media Services gebeurtenissen).
    * U kunt ook Event Grid gebeurtenissen gebruiken via Azure Functions.

    Voor meer informatie:

    * Zie het [Audio Analytics](https://review.docs.microsoft.com/azure/media-services/latest/transforms-jobs-concept) -voor beeld waarin wordt getoond hoe u een job bewaakt met Azure Event grid, inclusief het toevoegen van een terugval als de Azure Event grid berichten om een of andere reden worden vertraagd.
    * Bekijk de [Azure Event grid schema's voor Media Services-gebeurtenissen](https://review.docs.microsoft.com/azure/media-services/latest/media-services-event-schemas).

* Wanneer u een [taak](https://review.docs.microsoft.com/azure/media-services/latest/transforms-jobs-concept)maakt:
    * Selecteer een wille keurig account in de lijst met momenteel gebruikte accounts (deze lijst bevat normaal gesp roken beide accounts, maar als er problemen zijn gedetecteerd, kan het slechts één account bevatten). Als de lijst leeg is, moet u een waarschuwing genereren zodat een operator kan onderzoeken.
    * Maak een record voor het bijhouden van elke invlucht-taak en de regio/het account dat wordt gebruikt.
* Wanneer uw `JobStateChange` handler een melding krijgt dat een taak de geplande status heeft bereikt, noteert u de tijd die wordt ingevoerd in de geplande status en de regio/het account dat wordt gebruikt.
* Als uw `JobStateChange` handler een melding krijgt dat een taak de verwerkings status heeft bereikt, markeert u de record voor de taak als verwerking en noteert u de tijd die de verwerkings status heeft ingevoerd.
* Als uw `JobStateChange` handler een melding krijgt dat een taak een definitieve status heeft bereikt (voltooid/fout/geannuleerd), markeert u de record voor de taak op de juiste manier.
* Een afzonderlijk proces hebben dat regel matig naar uw records van de taken zoekt
    * Als u taken in de geplande status hebt die binnen een redelijke hoeveelheid tijd voor een bepaalde regio nog niet in de verwerkings status zijn gevorderd, verwijdert u die regio uit de lijst met momenteel gebruikte accounts. Afhankelijk van uw bedrijfs vereisten kunt u ervoor kiezen om deze taken direct te annuleren en ze opnieuw naar de andere regio te verzenden. Of u kunt ze nog een paar tijd geven om over te gaan naar de volgende status.
    * Als een regio uit de lijst met accounts is verwijderd, controleert u deze voor herstel voordat u deze weer aan de lijst toevoegt. De regionale status kan worden bewaakt via de bestaande taken in de regio (als deze niet zijn geannuleerd en opnieuw worden ingediend) door het account na een bepaalde periode opnieuw aan de lijst toe te voegen en door Opera tors die de Azure-communicatie over storingen controleren die van invloed kunnen zijn op Azure Media Services.

## <a name="next-steps"></a>Volgende stappen

* [Code voorbeelden](/samples/browse/?products=azure-media-services) bekijken
