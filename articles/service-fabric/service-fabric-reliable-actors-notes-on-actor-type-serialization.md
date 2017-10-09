---
title: "aaaReliable aktörer not aktören skriver serialisering | Microsoft Docs"
description: "Innehåller information om grundläggande för att definiera serialiserbara klasser som kan använda toodefine Service Fabric Reliable Actors tillstånd och gränssnitt"
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
ms.openlocfilehash: d8584e7d90fe1c68af38983e71e5d0a7554689bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a><span data-ttu-id="c7ac7-103">Information om Service Fabric Reliable Actors skriver serialisering</span><span class="sxs-lookup"><span data-stu-id="c7ac7-103">Notes on Service Fabric Reliable Actors type serialization</span></span>
<span data-ttu-id="c7ac7-104">hello argument över alla metoder, resultattyper hello uppgifter som returneras av varje metod i ett gränssnitt för aktören och objekt som lagras i en aktör tillståndshanterare måste vara [data minimera serialiserbara](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="c7ac7-104">hello arguments of all methods, result types of hello tasks returned by each method in an actor interface, and objects stored in an actor's state manager must be [data contract serializable](https://msdn.microsoft.com/library/ms731923.aspx).</span></span> <span data-ttu-id="c7ac7-105">Detta gäller även toohello argument för hello metoderna som definieras i [aktören händelsegränssnitt](service-fabric-reliable-actors-events.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac7-105">This also applies toohello arguments of hello methods defined in [actor event interfaces](service-fabric-reliable-actors-events.md).</span></span> <span data-ttu-id="c7ac7-106">(Aktören händelse gränssnittsmetoder alltid returnerar void.)</span><span class="sxs-lookup"><span data-stu-id="c7ac7-106">(Actor event interface methods always return void.)</span></span>

## <a name="custom-data-types"></a><span data-ttu-id="c7ac7-107">Anpassade datatyper</span><span class="sxs-lookup"><span data-stu-id="c7ac7-107">Custom data types</span></span>
<span data-ttu-id="c7ac7-108">I det här exemplet hello följande aktören-gränssnittet definierar en metod som returnerar en anpassad datatyp som kallas `VoicemailBox`:</span><span class="sxs-lookup"><span data-stu-id="c7ac7-108">In this example, hello following actor interface defines a method that returns a custom data type called `VoicemailBox`:</span></span>

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

<span data-ttu-id="c7ac7-109">hello gränssnittet implementeras av en aktör som använder hello tillstånd manager toostore en `VoicemailBox` objekt:</span><span class="sxs-lookup"><span data-stu-id="c7ac7-109">hello interface is implemented by an actor that uses hello state manager toostore a `VoicemailBox` object:</span></span>

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

<span data-ttu-id="c7ac7-110">I det här exemplet hello `VoicemailBox` objektet serialiseras när:</span><span class="sxs-lookup"><span data-stu-id="c7ac7-110">In this example, hello `VoicemailBox` object is serialized when:</span></span>

* <span data-ttu-id="c7ac7-111">hello objekt överförs mellan en aktören-instans och en anropare.</span><span class="sxs-lookup"><span data-stu-id="c7ac7-111">hello object is transmitted between an actor instance and a caller.</span></span>
* <span data-ttu-id="c7ac7-112">hello-objekt sparas i hello tillståndshanterare där den beständiga toodisk och replikeras tooother noder.</span><span class="sxs-lookup"><span data-stu-id="c7ac7-112">hello object is saved in hello state manager where it is persisted toodisk and replicated tooother nodes.</span></span>

<span data-ttu-id="c7ac7-113">hello tillförlitliga aktören framework använder DataContract-serialisering.</span><span class="sxs-lookup"><span data-stu-id="c7ac7-113">hello Reliable Actor framework uses DataContract serialization.</span></span> <span data-ttu-id="c7ac7-114">Därför hello anpassade dataobjekt och deras medlemmar måste vara försedd med hello **DataContract** och **DataMember** respektive attribut.</span><span class="sxs-lookup"><span data-stu-id="c7ac7-114">Therefore, hello custom data objects and their members must be annotated with hello **DataContract** and **DataMember** attributes, respectively.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="c7ac7-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7ac7-115">Next steps</span></span>
* [<span data-ttu-id="c7ac7-116">Aktören livscykel och skräp samling</span><span class="sxs-lookup"><span data-stu-id="c7ac7-116">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="c7ac7-117">Aktören timers och påminnelser</span><span class="sxs-lookup"><span data-stu-id="c7ac7-117">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="c7ac7-118">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="c7ac7-118">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="c7ac7-119">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="c7ac7-119">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="c7ac7-120">Aktören polymorfism och objektorienterad designmönster</span><span class="sxs-lookup"><span data-stu-id="c7ac7-120">Actor polymorphism and object-oriented design patterns</span></span>](service-fabric-reliable-actors-polymorphism.md)
* [<span data-ttu-id="c7ac7-121">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="c7ac7-121">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
