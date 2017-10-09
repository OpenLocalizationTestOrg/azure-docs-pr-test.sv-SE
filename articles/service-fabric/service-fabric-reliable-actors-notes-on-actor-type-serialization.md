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
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Information om Service Fabric Reliable Actors skriver serialisering
hello argument över alla metoder, resultattyper hello uppgifter som returneras av varje metod i ett gränssnitt för aktören och objekt som lagras i en aktör tillståndshanterare måste vara [data minimera serialiserbara](https://msdn.microsoft.com/library/ms731923.aspx). Detta gäller även toohello argument för hello metoderna som definieras i [aktören händelsegränssnitt](service-fabric-reliable-actors-events.md). (Aktören händelse gränssnittsmetoder alltid returnerar void.)

## <a name="custom-data-types"></a>Anpassade datatyper
I det här exemplet hello följande aktören-gränssnittet definierar en metod som returnerar en anpassad datatyp som kallas `VoicemailBox`:

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

hello gränssnittet implementeras av en aktör som använder hello tillstånd manager toostore en `VoicemailBox` objekt:

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

I det här exemplet hello `VoicemailBox` objektet serialiseras när:

* hello objekt överförs mellan en aktören-instans och en anropare.
* hello-objekt sparas i hello tillståndshanterare där den beständiga toodisk och replikeras tooother noder.

hello tillförlitliga aktören framework använder DataContract-serialisering. Därför hello anpassade dataobjekt och deras medlemmar måste vara försedd med hello **DataContract** och **DataMember** respektive attribut.

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


## <a name="next-steps"></a>Nästa steg
* [Aktören livscykel och skräp samling](service-fabric-reliable-actors-lifecycle.md)
* [Aktören timers och påminnelser](service-fabric-reliable-actors-timers-reminders.md)
* [Aktören händelser](service-fabric-reliable-actors-events.md)
* [Aktören återinträde](service-fabric-reliable-actors-reentrancy.md)
* [Aktören polymorfism och objektorienterad designmönster](service-fabric-reliable-actors-polymorphism.md)
* [Aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md)
