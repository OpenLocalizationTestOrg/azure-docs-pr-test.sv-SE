---
title: aaaReliable Services-meddelanden | Microsoft Docs
description: "Konceptuell dokumentationen för Service Fabric Reliable Services meddelanden"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: 8c43190d31dbe82d1dc7fa1c228128bdcc3684f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-notifications"></a>Reliable Services-meddelanden
Meddelanden tillåter klienter tootrack hello ändringar som görs tooan objekt som de är intresserad av. Två typer av objekt som stöder meddelanden: *tillförlitliga Tillståndshanterare* och *tillförlitliga ordlista*.

Vanliga orsaker till med hjälp av meddelanden är:

* Skapa materialiserade vyer, till exempel sekundärindex eller aggregeras filtrerade datavyer hello replikens tillstånd. Ett exempel är en sorterade index över alla nycklar i tillförlitliga ordlistan.
* Skicka övervakningsdata, till exempel hello antalet användare som lagts till i hello senaste timmen.

Meddelanden skickas som en del av åtgärder. På grund av att ska meddelanden hanteras så snabbt som möjligt och synkron händelser inte får innehåller några kostsamma åtgärder.

## <a name="reliable-state-manager-notifications"></a>Tillstånd för tillförlitlig Manager meddelanden
Tillförlitlig Tillståndshanterare visar meddelanden för hello följande händelser:

* Transaktionen
  * Checka in
* Tillstånd manager
  * Återskapa
  * Lägga till en tillförlitlig tillstånd
  * Borttagning av en tillförlitlig tillstånd

Tillförlitlig Tillståndshanterare spårar hello aktuella inflight transaktioner. hello enda förändringen i transaktionstillstånd som orsakar en avisering toobe utlöses är en transaktion som allokeras.

Tillförlitlig Tillståndshanterare upprätthåller en samling av tillförlitliga tillstånd som tillförlitliga ordlista och tillförlitlig kön. Tillförlitlig Tillståndshanterare utlöses meddelanden när den här samlingen ändras: tillståndet tillförlitliga läggs till eller tas bort eller återskapas hello hela samlingen.
hello tillförlitliga Tillståndshanterare samling återskapas i tre fall:

* Återställning: När en replik startar den återställer dess tidigare tillstånd från hello disken. Hello slutet av återställning, används **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello återställda tillförlitliga tillstånd.
* Fullständig kopia: innan en replik kan ansluta till hello konfigurationsuppsättning, den har inbyggda toobe. Ibland kan kräver detta en fullständig kopia av tillförlitliga Tillståndshanterare tillstånd från hello primära repliken toobe tillämpade toohello inaktiv sekundär replik. Tillförlitlig Tillståndshanterare på hello sekundär replik använder **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello tillförlitliga tillstånd som det har fått från hello primära repliken.
* Återställ: I scenarier med haveriberedskap hello replikens tillstånd kan återställas från en säkerhetskopia via **RestoreAsync**. I sådana fall tillförlitliga Tillståndshanterare på hello primära repliken använder **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello tillförlitliga tillstånd som den har återställts från hello säkerhetskopian.

tooregister för meddelanden och/eller tillstånd manager meddelanden behöver du tooregister med hello **TransactionChanged** eller **StateManagerChanged** händelser på tillförlitliga Tillståndshanterare. En gemensam plats tooregister med dessa händelsehanterare är hello konstruktorn för tillståndskänsliga tjänsten. När du registrerar på hello-konstruktorn kan du inte missar alla aviseringar som orsakas av en ändring under hello livstid **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Hej **TransactionChanged** händelsehanteraren använder **NotifyTransactionChangedEventArgs** tooprovide information om hello-händelse. Den innehåller hello Åtgärdsegenskap (till exempel **NotifyTransactionChangedAction.Commit**) som anger hello typ av ändring. Den innehåller också hello transaktionsegenskap som ger en referens toohello transaktion som har ändrats.

> [!NOTE]
> Idag **TransactionChanged** händelser aktiveras bara om genomförs hello transaktionen. hello åtgärd är lika för**NotifyTransactionChangedAction.Commit**. Men i hello framtida, händelser kan aktiveras för andra typer av tillståndsändringar för transaktionen. Vi rekommenderar kontrollerar hello åtgärd och bearbetning av hello-händelse endast om det är något som du förväntar dig.
> 
> 

Följande är ett exempel **TransactionChanged** händelsehanterare.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Hej **StateManagerChanged** händelsehanteraren använder **NotifyStateManagerChangedEventArgs** tooprovide information om hello-händelse.
**NotifyStateManagerChangedEventArgs** har två underklasser: **NotifyStateManagerRebuildEventArgs** och **NotifyStateManagerSingleEntityChangedEventArgs**.
Du använder hello Åtgärdsegenskap i **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello rätt underklass:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** och **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Följande är ett exempel **StateManagerChanged** meddelandehanteraren.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Tillförlitliga ordlista meddelanden
Tillförlitliga ordlistan innehåller meddelanden för hello följande händelser:

* Återskapa: Anropas om **ReliableDictionary** dess tillstånd har återställts från en återställda eller kopierade lokala tillstånd eller säkerhetskopiering.
* Rensa: Anropas om hello tillståndet för **ReliableDictionary** har rensats via hello **ClearAsync** metod.
* Lägg till: Anropas när ett objekt har lagts till för**ReliableDictionary**.
* Uppdatering: Anropas när ett objekt i **IReliableDictionary** har uppdaterats.
* Ta bort: Anropas när ett objekt i **IReliableDictionary** har tagits bort.

tooget tillförlitliga ordlista meddelanden behöver du tooregister med hello **DictionaryChanged** händelsehanteraren på **IReliableDictionary**. En gemensam plats tooregister med dessa händelsehanterare är i hello **ReliableStateManager.StateManagerChanged** lägga till meddelanden.
Registrera när **IReliableDictionary** har lagts till för**IReliableStateManager** garanterar att du inte missar eventuella meddelanden.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** är hello exempelmetoden den föregående hello **OnStateManagerChangedHandler** exempel anrop.
> 
> 

hello föregående kod anger hello **IReliableNotificationAsyncCallback** gränssnitt, tillsammans med **DictionaryChanged**. Eftersom **NotifyDictionaryRebuildEventArgs** innehåller en **IAsyncEnumerable** gränssnitt--som måste toobe räknas upp asynkront--återskapa meddelanden skickas via  **RebuildNotificationAsyncCallback** i stället för **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> I föregående kod, som en del av bearbetning hello återskapa meddelande hello underhålls första hello sammansatt tillstånd rensas. Eftersom hello tillförlitliga samling återskapas med ett nytt tillstånd, är alla tidigare meddelanden irrelevanta.
> 
> 

Hej **DictionaryChanged** händelsehanteraren använder **NotifyDictionaryChangedEventArgs** tooprovide information om hello-händelse.
**NotifyDictionaryChangedEventArgs** har fem underklasser. Använd hello Åtgärdsegenskap i **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello rätt underklass:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add** och **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Rekommendationer
* *Gör* slutföra meddelandehändelser så snabbt som möjligt.
* *Inte* köra alla kostsamma åtgärder (till exempel i/o-operationer) som en del av synkron händelser.
* *Gör* Kontrollera hello åtgärdstyp innan du bearbetar hello-händelse. Nya åtgärdstyper kan läggas till i hello framtida.

Här följer några saker tookeep i åtanke:

* Meddelanden skickas som en del av hello körningen av en åtgärd. Till exempel utlöses ett meddelande om återställning som hello sista steget i en återställning. En återställning slutförs inte förrän händelsen för hello-meddelande bearbetas.
* Eftersom meddelanden skickas som en del av hello tillämpa operations Se klienter bara meddelanden för lokalt allokerat åtgärder. Och eftersom åtgärder är garanterat endast toobe lokalt allokerat (med andra ord inloggad), de kan eller inte kan ångras i hello framtida.
* I hello gör om sökvägen utlöses ett enda meddelande för varje tillämpade åtgärd. Det innebär att om transaktionen T1 omfattar Create(X), Delete(X) och Create(X), får du en avisering för hello skapandet av X, ett för hello borttagning och ett för att skapa en hello igen, i den ordningen.
* Operations tillämpas för transaktioner som innehåller flera åtgärder i hello ordning som de togs emot på hello primära repliken från hello användare.
* Som en del av FALSKT pågår, kan vissa åtgärder att ångra. Meddelanden har aktiverats för dessa ångra-åtgärder, rullande hello tillstånd hello replik tillbaka tooa stabil punkt. En viktig skillnad Ångra meddelanden är att händelser som har dubblettnycklar aggregeras. Till exempel om är som ångras transaktionen T1, visas ett enda meddelande tooDelete(X).

## <a name="next-steps"></a>Nästa steg
* [Tillförlitliga samlingar](service-fabric-work-with-reliable-collections.md)
* [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Tillförlitlig säkerhetskopiering och återställning (återställning)](service-fabric-reliable-services-backup-restore.md)
* [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

