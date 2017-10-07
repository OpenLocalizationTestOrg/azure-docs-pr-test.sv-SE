---
title: "aaaReliable aktörer på Service Fabric | Microsoft Docs"
description: "Beskriver hur Reliable Actors ovanpå Reliable Services och använda hello funktioner för hello Service Fabric-plattformen."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: ecffb54139f1171c7839b77fed0be60950881198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a>Hur Reliable Actors använda hello Service Fabric-plattformen
Den här artikeln förklarar hur Reliable Actors fungerar på hello Azure Service Fabric-plattformen. Reliable Actors köras i ett ramverk som är värd för en implementering av en tillståndskänslig tillförlitlig tjänst som kallas hello *aktören tjänsten*. hello aktören tjänsten innehåller alla hello komponenter nödvändiga toomanage hello livscykel och meddelandet sändning för din aktörer:

* hello aktören Runtime hanterar livscykeln skräpinsamling, och tillämpar Enkeltrådig åtkomst.
* En aktören service fjärrkommunikation lyssnare accepterar fjärråtkomst anrop tooactors och skickar dem tooa dispatcher tooroute toohello lämplig aktören instans.
* hello aktören Tillståndsprovidern radbryts tillståndsprovidrar (till exempel hello tillförlitliga samlingar tillståndsprovidern) och ger ett nätverkskort för hantering av aktören tillstånd.

Dessa komponenter som tillsammans bildar hello tillförlitliga aktören framework.

## <a name="service-layering"></a>Tjänsten lager
Eftersom hello aktören själva tjänsten är en tillförlitlig tjänst, alla hello [programmodell](service-fabric-application-model.md), livscykel, [paketering](service-fabric-package-apps.md), [distribution](service-fabric-deploy-remove-applications.md), uppgradera och skalning begreppet Reliable Services gäller hello samma sätt tooactor tjänster. 

![Aktören service lager][1]

hello Visar föregående diagram hello förhållandet mellan hello Service Fabric-programramverk och användarkod. Blå element representerar hello Reliable Services application framework, orange representerar hello tillförlitliga aktören framework och grönt representerar användarkod.

I Reliable Services tjänsten ärver hello `StatefulService` klass. Den här klassen är härledd från `StatefulServiceBase` (eller `StatelessService` för tillståndslösa tjänster). I Reliable Actors använder du hello aktören service. hello aktören service är en annan implementering av hello `StatefulServiceBase` klassen implementerar hello aktören mönstret där din aktörer kör. Eftersom hello aktören själva tjänsten är bara en implementering av `StatefulServiceBase`, du kan skriva en egen tjänst som härleds från `ActorService` och implementera servicenivåer funktioner hello samma sätt som när arv `StatefulService`, som:

* Tjänsten säkerhetskopiering och återställning.
* Delade funktioner för alla aktörer, till exempel strömbrytare.
* RPC-anrop på hello aktören själva tjänsten och på varje enskild aktören.

> [!NOTE]
> Tillståndskänsliga tjänster stöds inte i Java-/ Linux.

### <a name="using-hello-actor-service"></a>Hello aktören-tjänsten
Aktören instanser har åtkomst toohello aktören de körs. Via hello aktören service hämta aktören instanser programmässigt kontext för hello-tjänster. hello service kontexten har hello partitions-ID, namn, programnamn och andra plattformsspecifika Service Fabric-information:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


Hello aktören service måste ha registrerats med en typ i hello Service Fabric runtime som alla Reliable Services. För hello aktören tjänsten toorun aktören-instanser måste också aktören-typen registreras med hello aktören tjänsten. Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer. Du kan registrera aktörstyp i hello enklaste fallet och hello aktören tjänsten med standardinställningarna används implicit:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Du kan också använda ett lambda-uttryck som tillhandahålls av hello metoden tooconstruct hello aktören Registreringstjänst för dig själv. Du kan konfigurera hello aktören service och uttryckligen skapa dina aktören instanser, där du kan mata in beroenden tooyour aktören via sin konstruktor:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a>Aktören Webbtjänstmetoder
Hej aktören tjänsten implementerar `IActorService` (C#) eller `ActorService` (Java), som i sin tur implementerar `IService` (C#) eller `Service` (Java). Det är hello-gränssnitt som används av Reliable Services fjärrkommunikation, vilket gör att RPC-anrop på Webbtjänstmetoder. Den innehåller servicenivå metoder som kan anropas via fjärranslutning via tjänsten fjärrkommunikation.

#### <a name="enumerating-actors"></a>Räknar upp aktörer
hello kan aktören en klient tooenumerate metadata om hello aktörer som är värd för hello-tjänsten. Eftersom hello aktören service är en partitionerad tillståndskänslig service, utförs uppräkningen per partition. Eftersom varje partition kan innehålla många aktörer, returneras hello uppräkning som en uppsättning växlingsbara resultat. hello sidor upprepas över tills alla sidor är skrivskyddade. följande exempel visar hur hello toocreate en lista över alla aktiva aktörer i en partition av en aktören-tjänst:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Om du tar bort aktörer
hello aktören tjänsten innehåller också en funktion för att ta bort aktörer:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Mer information om att radera aktörer och deras tillstånd finns hello [aktören livscykel dokumentationen](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Anpassade aktören service
Med hjälp av hello aktören registrering lambda, kan du registrera dina egna anpassade aktören-tjänst som härleds från `ActorService` (C#) och `FabricActorService` (Java). I den här anpassade aktören-tjänsten du kan implementera din egen servicenivå funktioner genom att skriva en service-klass som ärver `ActorService` (C#) eller `FabricActorService` (Java). En anpassad aktören tjänst ärver alla hello aktören runtime funktioner från `ActorService` (C#) eller `FabricActorService` (Java) och kan vara används tooimplement egna Webbtjänstmetoder.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
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
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a>Implementera aktören säkerhetskopiering och återställning
 I följande exempel hello, hello anpassade aktören service Exponerar en metod tooback aktören data genom att utnyttja hello fjärrkommunikation lyssnare finns redan i `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


I det här exemplet `IMyActorService` är ett kontrakt för fjärrkommunikation som implementerar `IService` (C#) och `Service` (Java) och sedan implementeras av `MyActorService`. Genom att lägga till avtalet remoting, metoder på `IMyActorService` är också tillgängliga tooa klienten genom att skapa en proxy för fjärrkommunikation via `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Programmodell
Aktören tjänsterna är tillförlitliga, så hello programmodell är hello samma. Dock generera hello aktören framework verktyg några hello programfiler modell du.

### <a name="service-manifest"></a>Tjänstmanifestet
hello aktören framework verktyg generera automatiskt hello innehållet i aktören tjänstens ServiceManifest.xml fil. Den här filen innehåller:

* Aktören tjänsttyp. namn på hello genereras baserat på din aktören projektnamn. Baserat på hello beständiga attribut i din aktören anges hello HasPersistedState flaggan också i enlighet med detta.
* Kodpaketet.
* Konfigurationspaketet.
* Resurser och slutpunkter.

### <a name="application-manifest"></a>Applikationsmanifestet
hello aktören framework verktyg skapa automatiskt en standard service definition för aktören tjänsten. hello verktyg fylla hello standardegenskaperna för tjänsten:

* Ange replikantalet bestäms av hello beständiga attribut i din aktören. Varje gång hello beständiga attribut på din aktören ändras, hello set replikantalet i tjänstdefinitionen för hello standard återställs därför.
* Partitionsschemat och intervallet ställs tooUniform Int64 med hello fullständig Int64 viktiga intervall.

## <a name="service-fabric-partition-concepts-for-actors"></a>Service Fabric-partition begrepp för aktörer
Aktörstjänster är partitionerad tillståndskänsliga tjänster. Varje partition för en tjänst aktören innehåller en uppsättning aktörer. Tjänsten partitioner distribueras automatiskt över flera noder i Service Fabric. Aktören instanser distribueras som ett resultat.

![Aktören partitionering och distribution][5]

Reliable Services kan skapas med annan partitionsscheman och partition nyckelintervall. hello aktören tjänsten använder hello Int64 partitioneringsschema med hello fullständig Int64 viktiga intervallet toomap aktörer toopartitions.

### <a name="actor-id"></a>Aktörs-ID
Varje aktören som skapas i hello-tjänsten har ett unikt ID som är associerade med den representeras av hello `ActorId` klass. `ActorId`är ett täckande ID-värde som kan användas för jämn fördelning av aktörer över hello service partitioner genom att generera slumpmässigt ID: N:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Varje `ActorId` är hashformaterats tooan Int64. Det är därför hello aktören tjänsten måste använda ett Int64 partitioneringsschema med hello fullständig Int64 viktiga intervall. Dock anpassade ID-värden kan användas för en `ActorID`, inklusive GUID/UUID: er, strängar och Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

När du använder GUID/UUID: er och strängar värdena hello hashformaterats tooan Int64. Men när du uttryckligen ger ett Int64 tooan `ActorId`, hello Int64 mappas direkt tooa partition utan ytterligare hash. Du kan använda den här tekniken toocontrol som partition hello aktörer är placerade i.

## <a name="next-steps"></a>Nästa steg
* [Aktören tillståndshantering](service-fabric-reliable-actors-state-management.md)
* [Aktören livscykel och skräp samling](service-fabric-reliable-actors-lifecycle.md)
* [Aktörer API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Exempelkod för .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-exempelkod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
