---
title: "aaaOverview aktören-baserad Azure mikrotjänster livscykeln för hantering av | Microsoft Docs"
description: "Beskriver Service Fabric tillförlitliga aktören livscykel, skräpinsamling och manuellt ta bort aktörer och deras tillstånd"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktören livscykel, automatisk skräpinsamling och manuellt ta bort
En aktör aktiveras hello första gången ett anrop görs tooany av dess metoder. En aktör är inaktiverad (skräp som samlats in av hello aktörer runtime) om den inte används för en konfigurerbar tidsperiod. En aktör och dess tillstånd kan också tas bort manuellt när som helst.

## <a name="actor-activation"></a>Aktören aktivering
När en aktör aktiveras inträffar hello följande:

* När ett samtal kommer för en aktör och ingen sådan redan är aktiv, skapas en ny aktören.
* Hej aktörstillstånd har lästs in om den underhåller tillstånd.
* Hej `OnActivateAsync` (C#) eller `onActivateAsync` (Java)-metoden (som kan åsidosättas i hello aktören implementering) anropas.
* hello aktören anses nu aktiv.

## <a name="actor-deactivation"></a>Aktören avaktivering
När en aktör inaktiveras inträffar hello följande:

* När en aktör inte används för vissa tidsperiod, är det bort från hello Active aktörer tabell.
* Hej `OnDeactivateAsync` (C#) eller `onDeactivateAsync` (Java)-metoden (som kan åsidosättas i hello aktören implementering) anropas. Rensar alla hello timers för hello aktören. Aktören åtgärder som tillstånd ändringar inte ska anropas från den här metoden.

> [!TIP]
> hello aktörer Fabric runtime skickar vissa [händelser relaterade tooactor aktivering och inaktivering av](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). De är användbara i diagnostik- och prestandaövervakning.
>
>

### <a name="actor-garbage-collection"></a>Aktören skräpinsamling
När en aktör inaktiveras referenser toohello aktören objekt släpps och det kan vara skräpinsamlats normalt av hello CLR common language runtime () eller java virtual machine (JVM) skräpinsamlingen. Skräpinsamling endast rensar hello aktören objekt. Det gör **inte** ta bort tillstånd lagras i hello aktören tillstånd Manager. hello nästa gång hello aktören har aktiverats, skapas ett nytt aktören objekt och dess tillstånd har återställts.

Vad räknas som ”används” hello syfte inaktivering och skräpinsamling?

* Ta emot ett samtal
* `IRemindable.ReceiveReminderAsync`metoden anropas (gäller endast om hello aktör använder påminnelser)

> [!NOTE]
> Om hello aktör använder timers och dess timer-återanropet anropas, sker **inte** antal som ”används”.
>
>

Innan vi gå in hello information avaktivering är det viktigt toodefine hello följande villkor:

* *Skanna intervall*. Detta är hello intervall på vilka hello aktörer runtime söker igenom Active aktörer tabellen aktörer kan inaktiveras och skräpinsamlats. hello standardvärdet är 1 minut.
* *Inaktivitetstid*. Detta är hello tid att en aktör måste tooremain oanvända (inaktiv) innan den kan inaktiveras och skräpinsamlats. hello standardvärdet är 60 minuter.

Normalt behöver inte toochange dessa standardinställningar. Men om det behövs dessa intervall kan ändras via `ActorServiceSettings` när du registrerar din [aktören Service](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Hello aktören runtime håller reda på hello mängden tid som den har varit inaktiv (dvs. inte används) för varje active aktören. hello aktören runtime kontrollerar alla hello aktörer varje `ScanIntervalInSeconds` toosee om det kan vara skräp samlas in och samlar in om det har varit inaktiv i `IdleTimeoutInSeconds`.

Varje gång som en aktör används, är dess inaktivitetstid Återställ too0. Sedan kan hello aktören kan vara skräpinsamlats endast om den igen är vilande för `IdleTimeoutInSeconds`. Återkalla en aktör anses toohave har används om en gränssnittsmetod aktören eller ett aktören påminnelse motanrop körs. En aktör är **inte** anses vara toohave har används om dess timer motanrop körs.

hello följande diagram visar hello livscykeln för ett enda aktören tooillustrate dessa begrepp.

![Exempel på inaktivitetstid][1]

hello exemplet visar hello effekten av aktören metodanrop, påminnelser och timers på den här aktören hello livstid. följande punkter om hello exempel hello är värt att nämna:

* ScanInterval och IdleTimeout ställs too5 och 10. (Enheter inte roll här, eftersom exemplet är endast tooillustrate hello konceptet.)
* hello genomsökningen efter aktörer toobe skräpinsamlats sker på T = 0, 5, 10, 15, 20, 25, som definieras av hello genomsökning intervall på 5.
* En periodisk timer utlöses vid T = 4, 8, 12, 16, 20, 24, och dess motanrop körs. Hello inaktivitetstid av hello aktören påverkas inte.
* En aktören metodanrop på T = 7 återställer hello inaktivitetstid too0 och fördröjningar hello skräpinsamling av hello aktören.
* Ett återanrop för aktören påminnelse körs på T = 14 och ytterligare fördröjningar hello skräpinsamling av hello aktören.
* Under hello skräp samling sökningen T = 25 hello aktören inaktivitetstid slutligen överskrider hello timeout vid inaktivitet för 10 och hello aktören samlas in som skräp.

En aktör blir aldrig skräpinsamlats medan det körs en av dess metoder, oavsett hur lång tid det tar vid körning av den här metoden. Som tidigare nämnts förhindrar hello körning av aktören gränssnittsmetoder och påminnelse återanrop skräpinsamling genom att återställa hello aktören inaktivitetstid too0. hello körningen av timer återanrop återställs inte hello inaktivitetstid too0. Hello skräpinsamling av hello aktören skjuts tills hello timer återanrop har slutförts.

## <a name="deleting-actors-and-their-state"></a>Ta bort aktörer och deras tillstånd
Skräpinsamling inaktiverade aktörer endast rensar hello aktören objekt, men det tar inte bort data som lagras i en aktör tillstånd Manager. När en aktör aktiveras igen, sker dess data tillgängliga tooit via hello Tillståndshanterare. I fall där aktörer lagra data i Tillståndshanterare och är inaktiverad men aldrig aktiverats igen vara det nödvändigt tooclean av sina data.

Hej [aktören Service](service-fabric-reliable-actors-platform.md) innehåller en funktion för att ta bort aktörer från en fjärransluten anropare:

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

Om du tar bort en aktör har hello följande effekter beroende på om huruvida hello aktören är för närvarande är aktiva:

* **Aktiva aktören**
  * Aktören tas bort från listan med aktiva aktörer och inaktiveras.
  * Dess tillstånd tas bort permanent.
* **Inaktiva aktören**
  * Dess tillstånd tas bort permanent.

Observera att det går inte att anropa en aktör ta bort på sig själv från någon av dess metoder aktören eftersom hello aktören inte kan tas bort körs inom en aktören anropet kontext, i vilken hello runtime har fått ett lås runt hello aktören anropet tooenforce Enkeltrådig åtkomst.

## <a name="next-steps"></a>Nästa steg
* [Aktören timers och påminnelser](service-fabric-reliable-actors-timers-reminders.md)
* [Aktören händelser](service-fabric-reliable-actors-events.md)
* [Aktören återinträde](service-fabric-reliable-actors-reentrancy.md)
* [Aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md)
* [Aktören API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# exempelkod](https://github.com/Azure/servicefabric-samples)
* [Java-kodexempel](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
