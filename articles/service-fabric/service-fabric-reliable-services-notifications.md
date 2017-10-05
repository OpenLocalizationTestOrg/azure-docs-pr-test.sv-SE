---
title: Reliable Services-meddelanden | Microsoft Docs
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
ms.openlocfilehash: c6a53d851510ed5e6eec1f3ac0f636ad034a6d4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-services-notifications"></a><span data-ttu-id="0d7e7-103">Reliable Services-meddelanden</span><span class="sxs-lookup"><span data-stu-id="0d7e7-103">Reliable Services notifications</span></span>
<span data-ttu-id="0d7e7-104">Meddelanden tillåter klienter att spåra ändringar som görs i ett objekt som de är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-104">Notifications allow clients to track the changes that are being made to an object that they're interested in.</span></span> <span data-ttu-id="0d7e7-105">Två typer av objekt som stöder meddelanden: *tillförlitliga Tillståndshanterare* och *tillförlitliga ordlista*.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="0d7e7-106">Vanliga orsaker till med hjälp av meddelanden är:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="0d7e7-107">Skapa materialiserade vyer, till exempel sekundärindex eller aggregeras filtrerade datavyer repliken tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-107">Building materialized views, such as secondary indexes or aggregated filtered views of the replica's state.</span></span> <span data-ttu-id="0d7e7-108">Ett exempel är en sorterade index över alla nycklar i tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="0d7e7-109">Skicka övervakningsdata, till exempel hur många användare som lagts till i den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-109">Sending monitoring data, such as the number of users added in the last hour.</span></span>

<span data-ttu-id="0d7e7-110">Meddelanden skickas som en del av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="0d7e7-111">På grund av att ska meddelanden hanteras så snabbt som möjligt och synkron händelser inte får innehåller några kostsamma åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="0d7e7-112">Tillstånd för tillförlitlig Manager meddelanden</span><span class="sxs-lookup"><span data-stu-id="0d7e7-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="0d7e7-113">Tillförlitlig Tillståndshanterare visar meddelanden för följande händelser:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-113">Reliable State Manager provides notifications for the following events:</span></span>

* <span data-ttu-id="0d7e7-114">Transaktionen</span><span class="sxs-lookup"><span data-stu-id="0d7e7-114">Transaction</span></span>
  * <span data-ttu-id="0d7e7-115">Checka in</span><span class="sxs-lookup"><span data-stu-id="0d7e7-115">Commit</span></span>
* <span data-ttu-id="0d7e7-116">Tillstånd manager</span><span class="sxs-lookup"><span data-stu-id="0d7e7-116">State manager</span></span>
  * <span data-ttu-id="0d7e7-117">Återskapa</span><span class="sxs-lookup"><span data-stu-id="0d7e7-117">Rebuild</span></span>
  * <span data-ttu-id="0d7e7-118">Lägga till en tillförlitlig tillstånd</span><span class="sxs-lookup"><span data-stu-id="0d7e7-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="0d7e7-119">Borttagning av en tillförlitlig tillstånd</span><span class="sxs-lookup"><span data-stu-id="0d7e7-119">Removal of a reliable state</span></span>

<span data-ttu-id="0d7e7-120">Tillförlitlig Tillståndshanterare spårar de aktuella inflight transaktionerna.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-120">Reliable State Manager tracks the current inflight transactions.</span></span> <span data-ttu-id="0d7e7-121">Den enda förändringen i transaktionstillstånd som orsakar en avisering till att utlösa är en transaktion som allokeras.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-121">The only change in transaction state that causes a notification to be fired is a transaction being committed.</span></span>

<span data-ttu-id="0d7e7-122">Tillförlitlig Tillståndshanterare upprätthåller en samling av tillförlitliga tillstånd som tillförlitliga ordlista och tillförlitlig kön.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="0d7e7-123">Tillförlitlig Tillståndshanterare utlöses meddelanden när den här samlingen ändras: tillståndet tillförlitliga läggs till eller tas bort eller återskapas hela samlingen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or the entire collection is rebuilt.</span></span>
<span data-ttu-id="0d7e7-124">Samlingen tillförlitliga Tillståndshanterare återskapas i tre fall:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-124">The Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="0d7e7-125">Återställning: När en replik startar den återställer dess tidigare tillstånd från disken.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-125">Recovery: When a replica starts, it recovers its previous state from the disk.</span></span> <span data-ttu-id="0d7e7-126">I slutet av recovery används **NotifyStateManagerChangedEventArgs** eller en händelse som innehåller de återställda tillförlitliga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-126">At the end of recovery, it uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of recovered reliable states.</span></span>
* <span data-ttu-id="0d7e7-127">Fullständig kopia: innan konfigurationen kan delta i en replik, den har skapas.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-127">Full copy: Before a replica can join the configuration set, it has to be built.</span></span> <span data-ttu-id="0d7e7-128">Ibland kan kräver detta en fullständig kopia av tillförlitliga Tillståndshanterare tillstånd från den primära repliken ska tillämpas på den sekundära repliken som inaktiv.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-128">Sometimes, this requires a full copy of Reliable State Manager's state from the primary replica to be applied to the idle secondary replica.</span></span> <span data-ttu-id="0d7e7-129">Tillstånd för tillförlitlig Manager på den sekundära repliken använder **NotifyStateManagerChangedEventArgs** eller en händelse som innehåller de tillförlitliga tillstånd som det har fått från den primära repliken.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-129">Reliable State Manager on the secondary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it acquired from the primary replica.</span></span>
* <span data-ttu-id="0d7e7-130">Återställ: I scenarier med haveriberedskap repliken kan återställas från en säkerhetskopia via **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-130">Restore: In disaster recovery scenarios, the replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="0d7e7-131">I sådana fall kan den tillförlitliga tillstånd Manager på den primära repliken använder **NotifyStateManagerChangedEventArgs** eller en händelse som innehåller de tillförlitliga tillstånd som den har återställts från säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-131">In such cases, Reliable State Manager on the primary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it restored from the backup.</span></span>

<span data-ttu-id="0d7e7-132">För att registrera för meddelanden och/eller tillstånd manager meddelanden måste du registrera den **TransactionChanged** eller **StateManagerChanged** händelser på tillförlitliga Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-132">To register for transaction notifications and/or state manager notifications, you need to register with the **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="0d7e7-133">En gemensam plats att registrera med dessa händelsehanterare är konstruktören för tillståndskänsliga tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-133">A common place to register with these event handlers is the constructor of your stateful service.</span></span> <span data-ttu-id="0d7e7-134">När du registrerar på konstruktorn kan du inte missar alla aviseringar som orsakas av en ändring under livslängden för **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-134">When you register on the constructor, you won't miss any notification that's caused by a change during the lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="0d7e7-135">Den **TransactionChanged** händelsehanteraren använder **NotifyTransactionChangedEventArgs** att ge information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-135">The **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** to provide details about the event.</span></span> <span data-ttu-id="0d7e7-136">Den innehåller egenskapen action (till exempel **NotifyTransactionChangedAction.Commit**) som anger vilken typ av ändring.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-136">It contains the action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies the type of change.</span></span> <span data-ttu-id="0d7e7-137">Den innehåller också transaktionsegenskapen som ger en referens till den transaktion som har ändrats.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-137">It also contains the transaction property that provides a reference to the transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="0d7e7-138">Idag **TransactionChanged** händelser aktiveras bara om genomförs transaktionen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-138">Today, **TransactionChanged** events are raised only if the transaction is committed.</span></span> <span data-ttu-id="0d7e7-139">Åtgärden är lika med **NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-139">The action is then equal to **NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="0d7e7-140">Men i framtiden, händelser kan aktiveras för andra typer av tillståndsändringar för transaktionen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-140">But in the future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="0d7e7-141">Vi rekommenderar att kontroll av åtgärden och bearbetar händelsen endast om det är något som du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-141">We recommend checking the action and processing the event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="0d7e7-142">Följande är ett exempel **TransactionChanged** händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-142">Following is an example **TransactionChanged** event handler.</span></span>

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

<span data-ttu-id="0d7e7-143">Den **StateManagerChanged** händelsehanteraren använder **NotifyStateManagerChangedEventArgs** att ge information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-143">The **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="0d7e7-144">**NotifyStateManagerChangedEventArgs** har två underklasser: **NotifyStateManagerRebuildEventArgs** och **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="0d7e7-145">Du använder egenskapen action i **NotifyStateManagerChangedEventArgs** att omvandla **NotifyStateManagerChangedEventArgs** till rätt underklass:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-145">You use the action property in **NotifyStateManagerChangedEventArgs** to cast **NotifyStateManagerChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="0d7e7-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="0d7e7-147">**NotifyStateManagerChangedAction.Add** och **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="0d7e7-148">Följande är ett exempel **StateManagerChanged** meddelandehanteraren.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-148">Following is an example **StateManagerChanged** notification handler.</span></span>

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

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="0d7e7-149">Tillförlitliga ordlista meddelanden</span><span class="sxs-lookup"><span data-stu-id="0d7e7-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="0d7e7-150">Tillförlitliga ordlista visar meddelanden för följande händelser:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-150">Reliable Dictionary provides notifications for the following events:</span></span>

* <span data-ttu-id="0d7e7-151">Återskapa: Anropas om **ReliableDictionary** dess tillstånd har återställts från en återställda eller kopierade lokala tillstånd eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="0d7e7-152">Rensa: Anropas om tillståndet för **ReliableDictionary** har rensats via den **ClearAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-152">Clear: Called when the state of **ReliableDictionary** has been cleared through the **ClearAsync** method.</span></span>
* <span data-ttu-id="0d7e7-153">Lägg till: Anropas när ett objekt har lagts till **ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-153">Add: Called when an item has been added to **ReliableDictionary**.</span></span>
* <span data-ttu-id="0d7e7-154">Uppdatering: Anropas när ett objekt i **IReliableDictionary** har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="0d7e7-155">Ta bort: Anropas när ett objekt i **IReliableDictionary** har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="0d7e7-156">För att få tillförlitliga ordlista meddelanden, måste du registrera med den **DictionaryChanged** händelsehanteraren på **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-156">To get Reliable Dictionary notifications, you need to register with the **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="0d7e7-157">En gemensam plats att registrera med dessa händelsehanterare är i den **ReliableStateManager.StateManagerChanged** lägga till meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-157">A common place to register with these event handlers is in the **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="0d7e7-158">Registrera när **IReliableDictionary** har lagts till i **IReliableStateManager** garanterar att du inte missar eventuella meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-158">Registering when **IReliableDictionary** is added to **IReliableStateManager** ensures that you won't miss any notifications.</span></span>

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
> <span data-ttu-id="0d7e7-159">**ProcessStateManagerSingleEntityNotification** är exempelmetoden som den föregående **OnStateManagerChangedHandler** exempel anrop.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-159">**ProcessStateManagerSingleEntityNotification** is the sample method that the preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="0d7e7-160">Föregående koduppsättningar den **IReliableNotificationAsyncCallback** gränssnitt, tillsammans med **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-160">The preceding code sets the **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="0d7e7-161">Eftersom **NotifyDictionaryRebuildEventArgs** innehåller en **IAsyncEnumerable** gränssnitt--som behöver räknas asynkront--återskapa meddelanden skickas via  **RebuildNotificationAsyncCallback** i stället för **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs to be enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

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
> <span data-ttu-id="0d7e7-162">I föregående kod, som en del av meddelandet återskapa är först behålla sammansatt tillstånd avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-162">In the preceding code, as part of processing the rebuild notification, first the maintained aggregated state is cleared.</span></span> <span data-ttu-id="0d7e7-163">Eftersom samlingen tillförlitliga återskapas med ett nytt tillstånd, är alla tidigare meddelanden irrelevanta.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-163">Because the reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="0d7e7-164">Den **DictionaryChanged** händelsehanteraren använder **NotifyDictionaryChangedEventArgs** att ge information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-164">The **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="0d7e7-165">**NotifyDictionaryChangedEventArgs** har fem underklasser.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="0d7e7-166">Använda egenskapen action i **NotifyDictionaryChangedEventArgs** att omvandla **NotifyDictionaryChangedEventArgs** till rätt underklass:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-166">Use the action property in **NotifyDictionaryChangedEventArgs** to cast **NotifyDictionaryChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="0d7e7-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="0d7e7-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="0d7e7-169">**NotifyDictionaryChangedAction.Add** och **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="0d7e7-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="0d7e7-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="0d7e7-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

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

## <a name="recommendations"></a><span data-ttu-id="0d7e7-172">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="0d7e7-172">Recommendations</span></span>
* <span data-ttu-id="0d7e7-173">*Gör* slutföra meddelandehändelser så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="0d7e7-174">*Inte* köra alla kostsamma åtgärder (till exempel i/o-operationer) som en del av synkron händelser.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="0d7e7-175">*Gör* Kontrollera åtgärdstypen innan du bearbetar händelsen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-175">*Do* check the action type before you process the event.</span></span> <span data-ttu-id="0d7e7-176">Nya åtgärdstyper kan läggas till i framtiden.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-176">New action types might be added in the future.</span></span>

<span data-ttu-id="0d7e7-177">Här följer några saker att tänka på:</span><span class="sxs-lookup"><span data-stu-id="0d7e7-177">Here are some things to keep in mind:</span></span>

* <span data-ttu-id="0d7e7-178">Meddelanden skickas som en del av en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-178">Notifications are fired as part of the execution of an operation.</span></span> <span data-ttu-id="0d7e7-179">Till exempel utlöses ett meddelande om återställning som det sista steget i en återställning.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-179">For example, a restore notification is fired as the last step of a restore operation.</span></span> <span data-ttu-id="0d7e7-180">En återställning slutförs inte förrän händelsen meddelande bearbetas.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-180">A restore will not finish until the notification event is processed.</span></span>
* <span data-ttu-id="0d7e7-181">Eftersom meddelanden skickas som en del av åtgärderna använder finns klienter endast meddelanden för lokalt allokerat åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-181">Because notifications are fired as part of the applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="0d7e7-182">Och eftersom åtgärder garanterat bara genomföras lokalt (med andra ord inloggad), de kan eller kan inte ångras i framtiden.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-182">And because operations are guaranteed only to be locally committed (in other words, logged), they might or might not be undone in the future.</span></span>
* <span data-ttu-id="0d7e7-183">Gör om, för utlöses ett enda meddelande för varje tillämpade åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-183">On the redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="0d7e7-184">Det innebär att om transaktionen T1 omfattar Create(X), Delete(X) och Create(X), får du en avisering för att skapa X, en för borttagning och en för att skapa igen, i den ordningen.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for the creation of X, one for the deletion, and one for the creation again, in that order.</span></span>
* <span data-ttu-id="0d7e7-185">Åtgärder tillämpas för transaktioner som innehåller flera åtgärder i den ordning som de togs emot på den primära repliken från användaren.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-185">For transactions that contain multiple operations, operations are applied in the order in which they were received on the primary replica from the user.</span></span>
* <span data-ttu-id="0d7e7-186">Som en del av FALSKT pågår, kan vissa åtgärder att ångra.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="0d7e7-187">Aviseringar genereras för dessa ångra-åtgärder, återställer tillståndet för repliken till en stabil.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-187">Notifications are raised for such undo operations, rolling the state of the replica back to a stable point.</span></span> <span data-ttu-id="0d7e7-188">En viktig skillnad Ångra meddelanden är att händelser som har dubblettnycklar aggregeras.</span><span class="sxs-lookup"><span data-stu-id="0d7e7-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="0d7e7-189">Till exempel om är som ångras transaktionen T1, visas ett enda meddelande Delete(X).</span><span class="sxs-lookup"><span data-stu-id="0d7e7-189">For example, if transaction T1 is being undone, you'll see a single notification to Delete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d7e7-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d7e7-190">Next steps</span></span>
* [<span data-ttu-id="0d7e7-191">Tillförlitliga samlingar</span><span class="sxs-lookup"><span data-stu-id="0d7e7-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="0d7e7-192">Snabbstart för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="0d7e7-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="0d7e7-193">Tillförlitlig säkerhetskopiering och återställning (återställning)</span><span class="sxs-lookup"><span data-stu-id="0d7e7-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="0d7e7-194">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="0d7e7-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

