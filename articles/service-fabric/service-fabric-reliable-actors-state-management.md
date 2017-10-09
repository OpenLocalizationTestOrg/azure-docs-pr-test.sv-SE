---
title: "aaaReliable aktörer tillstånd management | Microsoft Docs"
description: "Beskriver hur Reliable Actors tillstånd hanteras, beständiga och replikeras för hög tillgänglighet."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 346d92426b1890617d108a9504afb179e463bded
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-state-management"></a>Tillförlitliga aktörer tillståndshantering
Reliable Actors är enkeltrådad objekt som kan kapsla både logik och tillstånd. Eftersom aktörer körs på Reliable Services, de upprätthålla tillstånd på ett tillförlitligt sätt med hjälp av hello samma beständighet och replikering mekanismer som Reliable Services använder. Det här sättet aktörer förlorar inte deras tillstånd efter fel på återaktivering efter skräpinsamling eller när de flyttas mellan noder i ett kluster på grund av tooresource NLB eller uppgraderingar.

## <a name="state-persistence-and-replication"></a>Tillstånd beständighet och replikering
Alla Reliable Actors anses *stateful* eftersom varje instans aktören mappar tooa unikt ID. Det innebär att upprepade anrop toohello samma aktörs-ID är dirigeras toohello samma aktören-instans. I ett system med tillståndslös däremot klientanrop är inte garanterat toobe dirigeras toohello samma server varje gång. Därför är aktörstjänster alltid tillståndskänsliga tjänster.

Även om stateful som de måste lagra tillstånd på ett tillförlitligt sätt inte innebär betraktas som aktörer. Aktörer kan välja hello beständighet och replikering baserat på deras data lagringskraven:

* **Beständiga tillståndet**: tillstånd är beständiga toodisk och replikerade too3 eller flera repliker. Detta är hello mest beständiga tillståndet lagringsalternativ, där tillståndet kan spara genom hela klustret avbrott.
* **Temporära tillstånd**: tillstånd är replikerade too3 eller flera repliker och endast lagras i minnet. Detta ger återhämtning mot nodfel och aktören fel, och under uppgraderingar och resursen belastningsutjämning. Dock är tillstånd inte beständiga toodisk. Så om alla repliker går förlorade på samma gång, går hello tillstånd förlorad samt.
* **Inga beständiga tillståndet**: tillstånd är inte replikeras eller skrivs toodisk. Den här nivån är för aktörer som bara behöver toomaintain tillstånd på ett tillförlitligt sätt.

Varje nivå av beständiga är helt enkelt en annan *tillståndsprovidern* och *replikering* konfigurationen av din tjänst. Oavsett om tillståndet är skriven beroende toodisk hello tillståndsprovidern--hello-komponenten i en tillförlitlig tjänst som lagrar tillstånd. Replikeringen beror på hur många repliker för en tjänst har distribuerats med. Precis som med Reliable Services både hello tillståndsprovidern och kan enkelt ställas in replikantalet manuellt. hello aktören framework innehåller ett attribut att när används på en aktör väljer automatiskt en standardprovider för tillstånd och inställningar för replik antal tooachieve någon av dessa tre beständiga inställningar genereras automatiskt. Hej StatePersistence attributet ärvs inte av härledd klass, varje aktören måste tillhandahålla StatePersistence nivån.

### <a name="persisted-state"></a>Beständiga tillståndet
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Inställningen använder en tillståndsprovidern som lagrar data på disken och anger hello service replik antal too3 automatiskt.

### <a name="volatile-state"></a>Temporära tillstånd
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Den här inställningen använder en provider i minne – endast och anger hello replik antal too3.

### <a name="no-persisted-state"></a>Inga sparade tillstånd
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Den här inställningen använder en provider i minne – endast och anger hello replik antal too1.

### <a name="defaults-and-generated-settings"></a>Standardvärden och genererade inställningar
När du använder hello `StatePersistence` attribut, en tillståndsprovidern väljs automatiskt för dig vid körning när hello aktören-tjänsten startas. Hej replikantalet dock anges vid kompilering av hello Visual Studio aktören verktyg. hello verktyg automatiskt generera ett *standardtjänst* hello aktören tjänst i ApplicationManifest.xml. Parametrar skapas för **min replikuppsättningen storlek** och **mål replikuppsättningen storlek**.

Du kan ändra parametrarna manuellt. Men varje gång hello `StatePersistence` attribut ändras, hello har angetts toohello replik set storlek standardvärden för hello valt `StatePersistence` attribut, åsidosätter eventuella tidigare värden. Med andra ord hello värdena som du anger i ServiceManifest.xml är *endast* åsidosättas vid byggning när du ändrar hello `StatePersistence` attributvärdet.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Tillstånd manager
Varje instans aktören har sin egen tillståndshanterare: en ordlista-liknande datastruktur som lagras på ett tillförlitligt sätt nyckel/värde-par. hello tillstånd manager är en omslutning runt en tillståndsprovidern. Du kan använda den toostore data oavsett vilken beständiga inställning används. Det ger inte några garantier att en aktiv tjänst aktören kan ändras från en temporär (i minnet-endast) tillstånd inställningen tooa beständiga tillståndsinställningen via en löpande uppgradering och behålla data. Det är dock möjligt toochange replikantalet för en tjänst som körs.

Tillstånd manager nycklar måste vara strängar. Värden är generisk och kan ha olika typer, inklusive anpassade typer. Värden som lagras i hello tillståndshanterare måste vara datakontrakt serialiserbara eftersom de kan komma att överföras hello tooother nätverksnoder vid replikering och kan skrivas toodisk, beroende på en aktör tillståndsinställningen persistence.

Hej tillståndshanterare exponerar vanliga ordlista metoder för att hantera tillstånd, liknande toothose hittades i tillförlitliga ordlistan.

### <a name="accessing-state"></a>Åtkomst till tillstånd
Tillstånd kan nås via hello tillståndshanterare av nyckeln. Tillstånd manager metoder är alla asynkrona eftersom de kan kräva disk-i/o när aktörer beständiga tillstånd. Vid första åtkomst cachelagras tillstånd objekt i minnet. Upprepa åtkomst operations access-objekt direkt från minnet och returnera synkront utan att det medför i/o eller asynkrona kontexten-växla omkostnader. Ett tillstånd objekt tas bort från hello-cache i hello följande fall:

* En aktörsmetod utlöser ett undantag när det hämtar ett objekt från hello tillståndshanterare.
* En aktör återaktiveras när de har inaktiverats eller efter fel.
* hello tillstånd providern sidor tillstånd toodisk. Det här problemet beror på hello tillstånd providerns implementering. hello tillstånd standardprovidern för hello `Persisted` har problemet.

Du kan hämta tillstånd genom att använda ett standard- *hämta* åtgärden utlöser `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) om det inte finns en post för hello nyckeln:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Du kan också hämta tillstånd med hjälp av en *TryGet* metod som inte genereras om det inte finns en post för en nyckel:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a>Sparar tillstånd
metoder för hämtning av hello tillstånd manager returnera ett referensobjekt tooan i lokalt minne. Ändra det här objektet i lokala minnet enbart orsakar inte den toobe sparade varaktigt. När ett objekt som hämtats från hello tillståndshanterare och ändras, måste den igen i hello tillstånd manager toobe sparade varaktigt.

Du kan infoga tillstånd med hjälp av en ovillkorlig *ange*, vilket är hello motsvarar hello `dictionary["key"] = value` syntax:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Du kan lägga till tillstånd med hjälp av en *Lägg till* metod. Den här metoden genererar `InvalidOperationException`(C#) eller `IllegalStateException`(Java) när den försöker tooadd en nyckel som redan finns.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Du kan också lägga till tillstånd med hjälp av en *TryAdd* metod. Den här metoden genereras inget när den försöker tooadd en nyckel som redan finns.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Hello slutet av en aktörsmetod sparar hello tillståndshanterare automatiskt alla värden som har lagts till eller ändrats av en insert eller update-åtgärd. ”Spara” kan innehålla bestående toodisk och replikering, beroende på inställningarna för hello används. Värden som inte har ändrats beständiga eller replikeras inte. Om inga värden har ändrats, ingenting hello åtgärden för att spara. Om du sparar misslyckas hello ändrade tillstånd ignoreras och läsas in igen hello ursprungliga tillstånd.

Du kan också spara tillstånd manuellt genom att anropa hello `SaveStateAsync` metoden på hello aktören bas:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a>Ta bort tillstånd
Du kan ta bort tillstånd permanent från en aktör tillstånd manager genom att anropa hello *ta bort* metod. Den här metoden genererar `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) när den försöker tooremove en nyckel som inte finns.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Du kan också ta bort tillstånd permanent med hello *TryRemove* metod. Den här metoden genereras inget när den försöker tooremove en nyckel som inte finns.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Nästa steg

Tillstånd som är lagrad i Reliable Actors måste serialiseras innan dess skriftliga toodisk och replikeras för hög tillgänglighet. Lär dig mer om [aktören typserialiseringen](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Därefter lär dig mer om [aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md).
