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
# <a name="reliable-services-notifications"></a><span data-ttu-id="6b3a4-103">Reliable Services-meddelanden</span><span class="sxs-lookup"><span data-stu-id="6b3a4-103">Reliable Services notifications</span></span>
<span data-ttu-id="6b3a4-104">Meddelanden tillåter klienter tootrack hello ändringar som görs tooan objekt som de är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-104">Notifications allow clients tootrack hello changes that are being made tooan object that they're interested in.</span></span> <span data-ttu-id="6b3a4-105">Två typer av objekt som stöder meddelanden: *tillförlitliga Tillståndshanterare* och *tillförlitliga ordlista*.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="6b3a4-106">Vanliga orsaker till med hjälp av meddelanden är:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="6b3a4-107">Skapa materialiserade vyer, till exempel sekundärindex eller aggregeras filtrerade datavyer hello replikens tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-107">Building materialized views, such as secondary indexes or aggregated filtered views of hello replica's state.</span></span> <span data-ttu-id="6b3a4-108">Ett exempel är en sorterade index över alla nycklar i tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="6b3a4-109">Skicka övervakningsdata, till exempel hello antalet användare som lagts till i hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-109">Sending monitoring data, such as hello number of users added in hello last hour.</span></span>

<span data-ttu-id="6b3a4-110">Meddelanden skickas som en del av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="6b3a4-111">På grund av att ska meddelanden hanteras så snabbt som möjligt och synkron händelser inte får innehåller några kostsamma åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="6b3a4-112">Tillstånd för tillförlitlig Manager meddelanden</span><span class="sxs-lookup"><span data-stu-id="6b3a4-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="6b3a4-113">Tillförlitlig Tillståndshanterare visar meddelanden för hello följande händelser:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-113">Reliable State Manager provides notifications for hello following events:</span></span>

* <span data-ttu-id="6b3a4-114">Transaktionen</span><span class="sxs-lookup"><span data-stu-id="6b3a4-114">Transaction</span></span>
  * <span data-ttu-id="6b3a4-115">Checka in</span><span class="sxs-lookup"><span data-stu-id="6b3a4-115">Commit</span></span>
* <span data-ttu-id="6b3a4-116">Tillstånd manager</span><span class="sxs-lookup"><span data-stu-id="6b3a4-116">State manager</span></span>
  * <span data-ttu-id="6b3a4-117">Återskapa</span><span class="sxs-lookup"><span data-stu-id="6b3a4-117">Rebuild</span></span>
  * <span data-ttu-id="6b3a4-118">Lägga till en tillförlitlig tillstånd</span><span class="sxs-lookup"><span data-stu-id="6b3a4-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="6b3a4-119">Borttagning av en tillförlitlig tillstånd</span><span class="sxs-lookup"><span data-stu-id="6b3a4-119">Removal of a reliable state</span></span>

<span data-ttu-id="6b3a4-120">Tillförlitlig Tillståndshanterare spårar hello aktuella inflight transaktioner.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-120">Reliable State Manager tracks hello current inflight transactions.</span></span> <span data-ttu-id="6b3a4-121">hello enda förändringen i transaktionstillstånd som orsakar en avisering toobe utlöses är en transaktion som allokeras.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-121">hello only change in transaction state that causes a notification toobe fired is a transaction being committed.</span></span>

<span data-ttu-id="6b3a4-122">Tillförlitlig Tillståndshanterare upprätthåller en samling av tillförlitliga tillstånd som tillförlitliga ordlista och tillförlitlig kön.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="6b3a4-123">Tillförlitlig Tillståndshanterare utlöses meddelanden när den här samlingen ändras: tillståndet tillförlitliga läggs till eller tas bort eller återskapas hello hela samlingen.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or hello entire collection is rebuilt.</span></span>
<span data-ttu-id="6b3a4-124">hello tillförlitliga Tillståndshanterare samling återskapas i tre fall:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-124">hello Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="6b3a4-125">Återställning: När en replik startar den återställer dess tidigare tillstånd från hello disken.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-125">Recovery: When a replica starts, it recovers its previous state from hello disk.</span></span> <span data-ttu-id="6b3a4-126">Hello slutet av återställning, används **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello återställda tillförlitliga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-126">At hello end of recovery, it uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of recovered reliable states.</span></span>
* <span data-ttu-id="6b3a4-127">Fullständig kopia: innan en replik kan ansluta till hello konfigurationsuppsättning, den har inbyggda toobe.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-127">Full copy: Before a replica can join hello configuration set, it has toobe built.</span></span> <span data-ttu-id="6b3a4-128">Ibland kan kräver detta en fullständig kopia av tillförlitliga Tillståndshanterare tillstånd från hello primära repliken toobe tillämpade toohello inaktiv sekundär replik.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-128">Sometimes, this requires a full copy of Reliable State Manager's state from hello primary replica toobe applied toohello idle secondary replica.</span></span> <span data-ttu-id="6b3a4-129">Tillförlitlig Tillståndshanterare på hello sekundär replik använder **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello tillförlitliga tillstånd som det har fått från hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-129">Reliable State Manager on hello secondary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it acquired from hello primary replica.</span></span>
* <span data-ttu-id="6b3a4-130">Återställ: I scenarier med haveriberedskap hello replikens tillstånd kan återställas från en säkerhetskopia via **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-130">Restore: In disaster recovery scenarios, hello replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="6b3a4-131">I sådana fall tillförlitliga Tillståndshanterare på hello primära repliken använder **NotifyStateManagerChangedEventArgs** toofire en händelse som innehåller hello tillförlitliga tillstånd som den har återställts från hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-131">In such cases, Reliable State Manager on hello primary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it restored from hello backup.</span></span>

<span data-ttu-id="6b3a4-132">tooregister för meddelanden och/eller tillstånd manager meddelanden behöver du tooregister med hello **TransactionChanged** eller **StateManagerChanged** händelser på tillförlitliga Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-132">tooregister for transaction notifications and/or state manager notifications, you need tooregister with hello **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="6b3a4-133">En gemensam plats tooregister med dessa händelsehanterare är hello konstruktorn för tillståndskänsliga tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-133">A common place tooregister with these event handlers is hello constructor of your stateful service.</span></span> <span data-ttu-id="6b3a4-134">När du registrerar på hello-konstruktorn kan du inte missar alla aviseringar som orsakas av en ändring under hello livstid **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-134">When you register on hello constructor, you won't miss any notification that's caused by a change during hello lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="6b3a4-135">Hej **TransactionChanged** händelsehanteraren använder **NotifyTransactionChangedEventArgs** tooprovide information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-135">hello **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** tooprovide details about hello event.</span></span> <span data-ttu-id="6b3a4-136">Den innehåller hello Åtgärdsegenskap (till exempel **NotifyTransactionChangedAction.Commit**) som anger hello typ av ändring.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-136">It contains hello action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies hello type of change.</span></span> <span data-ttu-id="6b3a4-137">Den innehåller också hello transaktionsegenskap som ger en referens toohello transaktion som har ändrats.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-137">It also contains hello transaction property that provides a reference toohello transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="6b3a4-138">Idag **TransactionChanged** händelser aktiveras bara om genomförs hello transaktionen.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-138">Today, **TransactionChanged** events are raised only if hello transaction is committed.</span></span> <span data-ttu-id="6b3a4-139">hello åtgärd är lika för**NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-139">hello action is then equal too**NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="6b3a4-140">Men i hello framtida, händelser kan aktiveras för andra typer av tillståndsändringar för transaktionen.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-140">But in hello future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="6b3a4-141">Vi rekommenderar kontrollerar hello åtgärd och bearbetning av hello-händelse endast om det är något som du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-141">We recommend checking hello action and processing hello event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="6b3a4-142">Följande är ett exempel **TransactionChanged** händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-142">Following is an example **TransactionChanged** event handler.</span></span>

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

<span data-ttu-id="6b3a4-143">Hej **StateManagerChanged** händelsehanteraren använder **NotifyStateManagerChangedEventArgs** tooprovide information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-143">hello **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="6b3a4-144">**NotifyStateManagerChangedEventArgs** har två underklasser: **NotifyStateManagerRebuildEventArgs** och **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="6b3a4-145">Du använder hello Åtgärdsegenskap i **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello rätt underklass:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-145">You use hello action property in **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="6b3a4-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="6b3a4-147">**NotifyStateManagerChangedAction.Add** och **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="6b3a4-148">Följande är ett exempel **StateManagerChanged** meddelandehanteraren.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-148">Following is an example **StateManagerChanged** notification handler.</span></span>

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

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="6b3a4-149">Tillförlitliga ordlista meddelanden</span><span class="sxs-lookup"><span data-stu-id="6b3a4-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="6b3a4-150">Tillförlitliga ordlistan innehåller meddelanden för hello följande händelser:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-150">Reliable Dictionary provides notifications for hello following events:</span></span>

* <span data-ttu-id="6b3a4-151">Återskapa: Anropas om **ReliableDictionary** dess tillstånd har återställts från en återställda eller kopierade lokala tillstånd eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="6b3a4-152">Rensa: Anropas om hello tillståndet för **ReliableDictionary** har rensats via hello **ClearAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-152">Clear: Called when hello state of **ReliableDictionary** has been cleared through hello **ClearAsync** method.</span></span>
* <span data-ttu-id="6b3a4-153">Lägg till: Anropas när ett objekt har lagts till för**ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-153">Add: Called when an item has been added too**ReliableDictionary**.</span></span>
* <span data-ttu-id="6b3a4-154">Uppdatering: Anropas när ett objekt i **IReliableDictionary** har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="6b3a4-155">Ta bort: Anropas när ett objekt i **IReliableDictionary** har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="6b3a4-156">tooget tillförlitliga ordlista meddelanden behöver du tooregister med hello **DictionaryChanged** händelsehanteraren på **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-156">tooget Reliable Dictionary notifications, you need tooregister with hello **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="6b3a4-157">En gemensam plats tooregister med dessa händelsehanterare är i hello **ReliableStateManager.StateManagerChanged** lägga till meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-157">A common place tooregister with these event handlers is in hello **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="6b3a4-158">Registrera när **IReliableDictionary** har lagts till för**IReliableStateManager** garanterar att du inte missar eventuella meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-158">Registering when **IReliableDictionary** is added too**IReliableStateManager** ensures that you won't miss any notifications.</span></span>

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
> <span data-ttu-id="6b3a4-159">**ProcessStateManagerSingleEntityNotification** är hello exempelmetoden den föregående hello **OnStateManagerChangedHandler** exempel anrop.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-159">**ProcessStateManagerSingleEntityNotification** is hello sample method that hello preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="6b3a4-160">hello föregående kod anger hello **IReliableNotificationAsyncCallback** gränssnitt, tillsammans med **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-160">hello preceding code sets hello **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="6b3a4-161">Eftersom **NotifyDictionaryRebuildEventArgs** innehåller en **IAsyncEnumerable** gränssnitt--som måste toobe räknas upp asynkront--återskapa meddelanden skickas via  **RebuildNotificationAsyncCallback** i stället för **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs toobe enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

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
> <span data-ttu-id="6b3a4-162">I föregående kod, som en del av bearbetning hello återskapa meddelande hello underhålls första hello sammansatt tillstånd rensas.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-162">In hello preceding code, as part of processing hello rebuild notification, first hello maintained aggregated state is cleared.</span></span> <span data-ttu-id="6b3a4-163">Eftersom hello tillförlitliga samling återskapas med ett nytt tillstånd, är alla tidigare meddelanden irrelevanta.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-163">Because hello reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="6b3a4-164">Hej **DictionaryChanged** händelsehanteraren använder **NotifyDictionaryChangedEventArgs** tooprovide information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-164">hello **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="6b3a4-165">**NotifyDictionaryChangedEventArgs** har fem underklasser.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="6b3a4-166">Använd hello Åtgärdsegenskap i **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello rätt underklass:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-166">Use hello action property in **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="6b3a4-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="6b3a4-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="6b3a4-169">**NotifyDictionaryChangedAction.Add** och **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="6b3a4-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="6b3a4-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="6b3a4-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

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

## <a name="recommendations"></a><span data-ttu-id="6b3a4-172">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="6b3a4-172">Recommendations</span></span>
* <span data-ttu-id="6b3a4-173">*Gör* slutföra meddelandehändelser så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="6b3a4-174">*Inte* köra alla kostsamma åtgärder (till exempel i/o-operationer) som en del av synkron händelser.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="6b3a4-175">*Gör* Kontrollera hello åtgärdstyp innan du bearbetar hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-175">*Do* check hello action type before you process hello event.</span></span> <span data-ttu-id="6b3a4-176">Nya åtgärdstyper kan läggas till i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-176">New action types might be added in hello future.</span></span>

<span data-ttu-id="6b3a4-177">Här följer några saker tookeep i åtanke:</span><span class="sxs-lookup"><span data-stu-id="6b3a4-177">Here are some things tookeep in mind:</span></span>

* <span data-ttu-id="6b3a4-178">Meddelanden skickas som en del av hello körningen av en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-178">Notifications are fired as part of hello execution of an operation.</span></span> <span data-ttu-id="6b3a4-179">Till exempel utlöses ett meddelande om återställning som hello sista steget i en återställning.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-179">For example, a restore notification is fired as hello last step of a restore operation.</span></span> <span data-ttu-id="6b3a4-180">En återställning slutförs inte förrän händelsen för hello-meddelande bearbetas.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-180">A restore will not finish until hello notification event is processed.</span></span>
* <span data-ttu-id="6b3a4-181">Eftersom meddelanden skickas som en del av hello tillämpa operations Se klienter bara meddelanden för lokalt allokerat åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-181">Because notifications are fired as part of hello applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="6b3a4-182">Och eftersom åtgärder är garanterat endast toobe lokalt allokerat (med andra ord inloggad), de kan eller inte kan ångras i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-182">And because operations are guaranteed only toobe locally committed (in other words, logged), they might or might not be undone in hello future.</span></span>
* <span data-ttu-id="6b3a4-183">I hello gör om sökvägen utlöses ett enda meddelande för varje tillämpade åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-183">On hello redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="6b3a4-184">Det innebär att om transaktionen T1 omfattar Create(X), Delete(X) och Create(X), får du en avisering för hello skapandet av X, ett för hello borttagning och ett för att skapa en hello igen, i den ordningen.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for hello creation of X, one for hello deletion, and one for hello creation again, in that order.</span></span>
* <span data-ttu-id="6b3a4-185">Operations tillämpas för transaktioner som innehåller flera åtgärder i hello ordning som de togs emot på hello primära repliken från hello användare.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-185">For transactions that contain multiple operations, operations are applied in hello order in which they were received on hello primary replica from hello user.</span></span>
* <span data-ttu-id="6b3a4-186">Som en del av FALSKT pågår, kan vissa åtgärder att ångra.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="6b3a4-187">Meddelanden har aktiverats för dessa ångra-åtgärder, rullande hello tillstånd hello replik tillbaka tooa stabil punkt.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-187">Notifications are raised for such undo operations, rolling hello state of hello replica back tooa stable point.</span></span> <span data-ttu-id="6b3a4-188">En viktig skillnad Ångra meddelanden är att händelser som har dubblettnycklar aggregeras.</span><span class="sxs-lookup"><span data-stu-id="6b3a4-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="6b3a4-189">Till exempel om är som ångras transaktionen T1, visas ett enda meddelande tooDelete(X).</span><span class="sxs-lookup"><span data-stu-id="6b3a4-189">For example, if transaction T1 is being undone, you'll see a single notification tooDelete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b3a4-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b3a4-190">Next steps</span></span>
* [<span data-ttu-id="6b3a4-191">Tillförlitliga samlingar</span><span class="sxs-lookup"><span data-stu-id="6b3a4-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="6b3a4-192">Snabbstart för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="6b3a4-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="6b3a4-193">Tillförlitlig säkerhetskopiering och återställning (återställning)</span><span class="sxs-lookup"><span data-stu-id="6b3a4-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="6b3a4-194">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="6b3a4-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

