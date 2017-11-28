---
title: "Tillförlitliga aktörer not aktören skriver serialisering | Microsoft Docs"
description: "Innehåller information om grundläggande för att definiera serialiserbara klasser som kan användas för att definiera Service Fabric Reliable Actors tillstånd och gränssnitt"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4b48b893e5a3bf5620f00a336576efe1ad63def8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a><span data-ttu-id="a21b4-103">Information om Service Fabric Reliable Actors skriver serialisering</span><span class="sxs-lookup"><span data-stu-id="a21b4-103">Notes on Service Fabric Reliable Actors type serialization</span></span>
<span data-ttu-id="a21b4-104">Argumenten för alla metoder resultattyper uppgifter som returneras av varje metod i ett gränssnitt för aktören och objekt som lagras i en aktör tillståndshanterare måste vara [data minimera serialiserbara](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="a21b4-104">The arguments of all methods, result types of the tasks returned by each method in an actor interface, and objects stored in an actor's state manager must be [data contract serializable](https://msdn.microsoft.com/library/ms731923.aspx).</span></span> <span data-ttu-id="a21b4-105">Detta gäller även för argumenten metoderna som definieras i [aktören händelsegränssnitt](service-fabric-reliable-actors-events.md).</span><span class="sxs-lookup"><span data-stu-id="a21b4-105">This also applies to the arguments of the methods defined in [actor event interfaces](service-fabric-reliable-actors-events.md).</span></span> <span data-ttu-id="a21b4-106">(Aktören händelse gränssnittsmetoder alltid returnerar void.)</span><span class="sxs-lookup"><span data-stu-id="a21b4-106">(Actor event interface methods always return void.)</span></span>

## <a name="custom-data-types"></a><span data-ttu-id="a21b4-107">Anpassade datatyper</span><span class="sxs-lookup"><span data-stu-id="a21b4-107">Custom data types</span></span>
<span data-ttu-id="a21b4-108">I det här exemplet följande aktören-gränssnittet definierar en metod som returnerar en anpassad datatyp som kallas `VoicemailBox`:</span><span class="sxs-lookup"><span data-stu-id="a21b4-108">In this example, the following actor interface defines a method that returns a custom data type called `VoicemailBox`:</span></span>

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

<span data-ttu-id="a21b4-109">Gränssnittet implementeras av en aktör som använder tillståndshanterarens för att lagra en `VoicemailBox` objekt:</span><span class="sxs-lookup"><span data-stu-id="a21b4-109">The interface is implemented by an actor that uses the state manager to store a `VoicemailBox` object:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

<span data-ttu-id="a21b4-110">I detta exempel på `VoicemailBox` objektet serialiseras när:</span><span class="sxs-lookup"><span data-stu-id="a21b4-110">In this example, the `VoicemailBox` object is serialized when:</span></span>

* <span data-ttu-id="a21b4-111">Objektet överförs mellan en aktören-instans och en anropare.</span><span class="sxs-lookup"><span data-stu-id="a21b4-111">The object is transmitted between an actor instance and a caller.</span></span>
* <span data-ttu-id="a21b4-112">Objektet har sparats i hanteraren för tillstånd där den beständiga till disk och replikeras till andra noder.</span><span class="sxs-lookup"><span data-stu-id="a21b4-112">The object is saved in the state manager where it is persisted to disk and replicated to other nodes.</span></span>

<span data-ttu-id="a21b4-113">Tillförlitliga aktören framework använder DataContract-serialisering.</span><span class="sxs-lookup"><span data-stu-id="a21b4-113">The Reliable Actor framework uses DataContract serialization.</span></span> <span data-ttu-id="a21b4-114">Därför anpassade data-objekt och deras medlemmar måste vara försedd med den **DataContract** och **DataMember** respektive attribut.</span><span class="sxs-lookup"><span data-stu-id="a21b4-114">Therefore, the custom data objects and their members must be annotated with the **DataContract** and **DataMember** attributes, respectively.</span></span>

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a><span data-ttu-id="a21b4-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a21b4-115">Next steps</span></span>
* [<span data-ttu-id="a21b4-116">Aktören livscykel och skräp samling</span><span class="sxs-lookup"><span data-stu-id="a21b4-116">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="a21b4-117">Aktören timers och påminnelser</span><span class="sxs-lookup"><span data-stu-id="a21b4-117">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="a21b4-118">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="a21b4-118">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="a21b4-119">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="a21b4-119">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="a21b4-120">Aktören polymorfism och objektorienterad designmönster</span><span class="sxs-lookup"><span data-stu-id="a21b4-120">Actor polymorphism and object-oriented design patterns</span></span>](service-fabric-reliable-actors-polymorphism.md)
* [<span data-ttu-id="a21b4-121">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="a21b4-121">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
