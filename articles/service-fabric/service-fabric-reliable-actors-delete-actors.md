---
title: Azure Service Fabric-actors verwijderen
description: Meer informatie over het hand matig en volledig verwijderen van Reliable Actors en hun status in een Azure Service Fabric-toepassing.
author: amanbha
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: amanbha
ms.custom: devx-track-csharp
ms.openlocfilehash: 80192aef564317e36fba56025aa31c787676d974
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2020
ms.locfileid: "89006853"
---
# <a name="delete-reliable-actors-and-their-state"></a>Reliable Actors en hun status verwijderen
Garbagecollection van gedeactiveerde actors reinigt alleen het actor-object, maar verwijdert geen gegevens die zijn opgeslagen in de status Manager van een actor. Wanneer een actor opnieuw wordt geactiveerd, worden de bijbehorende gegevens opnieuw beschikbaar gemaakt via de status Manager. In gevallen waarin actors gegevens opslaan in de status Manager en worden gedeactiveerd, maar nooit opnieuw worden geactiveerd, kan het nodig zijn om de gegevens op te schonen.

De [actor-service](service-fabric-reliable-actors-platform.md) biedt een functie voor het verwijderen van actors van een externe beller:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Het verwijderen van een actor heeft de volgende gevolgen, afhankelijk van het feit of de actor momenteel actief is:

* **Actieve actor**
  * Actor is verwijderd uit de lijst met actieve actors en wordt gedeactiveerd.
  * De status wordt definitief verwijderd.
* **Inactieve actor**
  * De status wordt definitief verwijderd.

Een actor kan geen verwijdering op zichzelf aanroepen vanuit een van de actor-methoden omdat de actor niet kan worden verwijderd tijdens het uitvoeren van een actor-aanroep context, waarbij de runtime een vergren deling heeft behaald rond de actor-aanroep om toegang met één thread af te dwingen.

Lees het volgende voor meer informatie over Reliable Actors:
* [Actor timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Actor herbetreedbaarheid](service-fabric-reliable-actors-reentrancy.md)
* [De functie voor het controleren van actor en prestaties](service-fabric-reliable-actors-diagnostics.md)
* [Referentie documentatie voor actor-API](/previous-versions/azure/dn971626(v=azure.100))
* [C#-voorbeeld code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeld code](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
