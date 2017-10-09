---
title: "aaaWorking med tillförlitlig samlingar | Microsoft Docs"
description: "Lär dig hello bästa praxis för att arbeta med tillförlitlig samlingar."
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: 
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: 41ba0b257da8493c1fc2e99ad7565593dc7cbcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-reliable-collections"></a><span data-ttu-id="d3e2d-103">Arbeta med tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="d3e2d-103">Working with Reliable Collections</span></span>
<span data-ttu-id="d3e2d-104">Service Fabric erbjuder en tillståndskänslig programming modell tillgänglig too.NET utvecklare via tillförlitliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-104">Service Fabric offers a stateful programming model available too.NET developers via Reliable Collections.</span></span> <span data-ttu-id="d3e2d-105">Mer specifikt ger Service Fabric tillförlitliga ordlista och tillförlitlig kön klasser.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="d3e2d-106">När du använder dessa klasser är ditt tillstånd partitionerad (för skalbarhet) replikeras (för tillgänglighet) och överförd inom en partition (för ACID-semantik).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="d3e2d-107">Nu ska vi titta på en vanlig användning av en tillförlitlig Ordlisteobjekt och se vilka dess faktiskt gör.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record toolog & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record toolog & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

<span data-ttu-id="d3e2d-108">Alla åtgärder på tillförlitliga ordlista objekt (förutom ClearAsync som inte går att ångra), kräver ett ITransaction-objekt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="d3e2d-109">Det här objektet är kopplad till den eventuella och alla ändringar som du försöker toomake tooany tillförlitliga ordlista och/eller tillförlitlig köobjekt inom en partition.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-109">This object has associated with it any and all changes you’re attempting toomake tooany reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="d3e2d-110">Du skaffar ett ITransaction objekt genom att anropa hello partition har Statemanager's CreateTransaction metod.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-110">You acquire an ITransaction object by calling hello partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="d3e2d-111">I hello koden ovan skickas hello ITransaction objektet tooa tillförlitliga ordlista AddAsync metod.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-111">In hello code above, hello ITransaction object is passed tooa reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="d3e2d-112">Internt, ta ordlista metoder som accepterar en nyckel reader/writer lås som är associerade med hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with hello key.</span></span> <span data-ttu-id="d3e2d-113">Om hello metoden ändrar hello nyckelvärde, hello metoden tar ett skrivlås på hello nyckel och om hello metoden läser endast från hello nyckelvärde, är ett läslås vidtas på hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-113">If hello method modifies hello key’s value, hello method takes a write lock on hello key and if hello method only reads from hello key’s value, then a read lock is taken on hello key.</span></span> <span data-ttu-id="d3e2d-114">Eftersom AddAsync ändrar hello nyckeln värdet toohello nya, skickas i värdet hello nyckeln skrivskydd hämtas.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-114">Since AddAsync modifies hello key’s value toohello new, passed-in value, hello key’s write lock is taken.</span></span> <span data-ttu-id="d3e2d-115">Så om 2 (eller högre) trådar försöker tooadd värden med hello samma nyckel på hello samma time, en tråd skaffar hello skrivskydd och hello andra trådar blockeras.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-115">So, if 2 (or more) threads attempt tooadd values with hello same key at hello same time, one thread will acquire hello write lock and hello other threads will block.</span></span> <span data-ttu-id="d3e2d-116">Som standard metoder blockera för in too4 sekunder tooacquire hello Lås; efter 4 sekunder hello metoder som resulterar i en TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-116">By default, methods block for up too4 seconds tooacquire hello lock; after 4 seconds, hello methods throw a TimeoutException.</span></span> <span data-ttu-id="d3e2d-117">Metoden överlagringar finns vilket gör att du toopass ett explicit timeout-värde om du föredrar.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-117">Method overloads exist allowing you toopass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="d3e2d-118">Vanligtvis kan du skriva din kod tooreact tooa TimeoutException genom fångar in den och du gör hello hela åtgärden (som visas i hello koden ovan).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-118">Usually, you write your code tooreact tooa TimeoutException by catching it and retrying hello entire operation (as shown in hello code above).</span></span> <span data-ttu-id="d3e2d-119">I enkla kod ansluter jag bara Task.Delay skicka 100 millisekunder varje gång.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="d3e2d-120">Men i verkligheten kan du använda någon typ av exponentiell inte fördröjning i stället.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="d3e2d-121">När hello Lås förvärvas AddAsync lägger till hello nyckel och värdeobjekt refererar till tooan interna tillfälliga ordlista associerad med hello ITransaction-objekt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-121">Once hello lock is acquired, AddAsync adds hello key and value object references tooan internal temporary dictionary associated with hello ITransaction object.</span></span> <span data-ttu-id="d3e2d-122">Detta görs tooprovide du med Läs-your-äger-skrivningar semantik.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-122">This is done tooprovide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="d3e2d-123">Som är när du anropar AddAsync, en senare anropet tooTryGetValueAsync (med hjälp av hello samma ITransaction-objekt) returnerar hello värde även om du inte har ännu har allokerats hello transaktion.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-123">That is, after you call AddAsync, a later call tooTryGetValueAsync (using hello same ITransaction object) will return hello value even if you have not yet committed hello transaction.</span></span> <span data-ttu-id="d3e2d-124">Därefter AddAsync Serialiserar din nyckel och värde objekt toobyte matriser och lägger till dessa byte-matriser tooa loggfilen på hello lokala noden.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-124">Next, AddAsync serializes your key and value objects toobyte arrays and appends these byte arrays tooa log file on hello local node.</span></span> <span data-ttu-id="d3e2d-125">Slutligen skickar AddAsync hello byte-matriser tooall hello sekundära repliker så att de har hello samma nyckel/värde-information.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-125">Finally, AddAsync sends hello byte arrays tooall hello secondary replicas so they have hello same key/value information.</span></span> <span data-ttu-id="d3e2d-126">Även om hello nyckel/värde-information har skrivits tooa loggfil, betraktas hello information inte som en del av hello ordlistan tills hello transaktion som de är associerade med har bekräftats.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-126">Even though hello key/value information has been written tooa log file, hello information is not considered part of hello dictionary until hello transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="d3e2d-127">Hello koden ovan genomför hello anropet tooCommitAsync alla hello transaktion-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-127">In hello code above, hello call tooCommitAsync commits all of hello transaction’s operations.</span></span> <span data-ttu-id="d3e2d-128">Mer specifikt lägger till information commit toohello loggfilen på hello lokala noden och skickar även hello commit poster tooall hello sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-128">Specifically, it appends commit information toohello log file on hello local node and also sends hello commit record tooall hello secondary replicas.</span></span> <span data-ttu-id="d3e2d-129">När ett kvorum (majoritet) av hello repliker har svarat alla data ändringar anses permanent och eventuella lås som är associerade med nycklar som har ändras via hello ITransaction objekt släpps så att andra trådar/transaktioner kan ändra hello samma nycklar och deras värden.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-129">Once a quorum (majority) of hello replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via hello ITransaction object are released so other threads/transactions can manipulate hello same keys and their values.</span></span>

<span data-ttu-id="d3e2d-130">Om CommitAsync inte anropas (vanligtvis på grund av tooan undantag som uppstod), hämtar hello ITransaction objektet bort.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-130">If CommitAsync is not called (usually due tooan exception being thrown), then hello ITransaction object gets disposed.</span></span> <span data-ttu-id="d3e2d-131">När avyttring objektets ogenomförda ITransaction Service Fabric läggs Avbryt information toohello lokala nodens loggfilen och behövs inga toobe skickas tooany av hello sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information toohello local node’s log file and nothing needs toobe sent tooany of hello secondary replicas.</span></span> <span data-ttu-id="d3e2d-132">Och sedan alla som är associerade med nycklar som har ändras via hello transaktion låsen släpps.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-132">And then, any locks associated with keys that were manipulated via hello transaction are released.</span></span>

## <a name="common-pitfalls-and-how-tooavoid-them"></a><span data-ttu-id="d3e2d-133">Vanliga fallgropar och hur tooavoid dem.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-133">Common pitfalls and how tooavoid them</span></span>
<span data-ttu-id="d3e2d-134">Nu när du förstår hur hello tillförlitliga samlingar fungerar internt ska vi titta på några vanliga missbruk av dem.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-134">Now that you understand how hello reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="d3e2d-135">Se hello koden nedan:</span><span class="sxs-lookup"><span data-stu-id="d3e2d-135">See hello code below:</span></span>

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes hello name/user, logs hello bytes,
   // & sends hello bytes toohello secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // hello line below updates hello property’s value in memory only; the
   // new value is NOT serialized, logged, & sent toosecondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

<span data-ttu-id="d3e2d-136">När du arbetar med en vanlig .NET-ordlista du lägger till en nyckel/värde-toohello ordlista och ändra sedan hello-värdet för en egenskap (till exempel LastLogin).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-136">When working with a regular .NET dictionary, you can add a key/value toohello dictionary and then change hello value of a property (such as LastLogin).</span></span> <span data-ttu-id="d3e2d-137">Men fungerar den här koden inte med en tillförlitlig ordlista.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="d3e2d-138">Kom ihåg från hello tidigare diskussion, hello anropet tooAddAsync Serialiserar hello nyckel/värde-objekt toobyte matriser, och sedan sparar hello matriser tooa lokal fil och skickar dem också toohello sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-138">Remember from hello earlier discussion, hello call tooAddAsync serializes hello key/value objects toobyte arrays and then saves hello arrays tooa local file and also sends them toohello secondary replicas.</span></span> <span data-ttu-id="d3e2d-139">Om du senare ändrar en egenskap ändras hello egenskapens värde i minnet. det påverkar inte hello lokal fil eller hello data som skickas toohello repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-139">If you later change a property, this changes hello property’s value in memory only; it does not impact hello local file or hello data sent toohello replicas.</span></span> <span data-ttu-id="d3e2d-140">Om hello processen kraschar genereras i minnet direkt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-140">If hello process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="d3e2d-141">När en ny process startas eller om en annan replik blir primära, sedan hello gamla egenskapsvärdet är vad som är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-141">When a new process starts or if another replica becomes primary, then hello old property value is what is available.</span></span>

<span data-ttu-id="d3e2d-142">Jag kan inte vara påfrestande tillräckligt hur lätt det är toomake hello typ av fel som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-142">I cannot stress enough how easy it is toomake hello kind of mistake shown above.</span></span> <span data-ttu-id="d3e2d-143">Och du kommer endast Lär dig mer om hello fel när hello processen kraschar.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-143">And, you will only learn about hello mistake if/when hello process goes down.</span></span> <span data-ttu-id="d3e2d-144">hello rätt metod toowrite hello koden är helt enkelt tooreverse hello två rader:</span><span class="sxs-lookup"><span data-stu-id="d3e2d-144">hello correct way toowrite hello code is simply tooreverse hello two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="d3e2d-145">Här är ett annat exempel som visar vanliga fel:</span><span class="sxs-lookup"><span data-stu-id="d3e2d-145">Here is another example showing a common mistake:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (user.HasValue) {
      // hello line below updates hello property’s value in memory only; the
      // new value is NOT serialized, logged, & sent toosecondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

<span data-ttu-id="d3e2d-146">Igen med vanlig .NET ordlistor hello koden ovan fungerar bra och är ett vanligt mönster: hello utvecklare använder en nyckel toolook upp ett värde.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-146">Again, with regular .NET dictionaries, hello code above works fine and is a common pattern: hello developer uses a key toolook up a value.</span></span> <span data-ttu-id="d3e2d-147">Om hello värdet finns ändras hello utvecklare ett egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-147">If hello value exists, hello developer changes a property’s value.</span></span> <span data-ttu-id="d3e2d-148">Med tillförlitlig samlingar uppvisar men den här koden hello samma problem som redan beskrivs: **du inte ändra ett objekt när du har angett det tooa tillförlitliga samling.**</span><span class="sxs-lookup"><span data-stu-id="d3e2d-148">However, with reliable collections, this code exhibits hello same problem as already discussed: **you MUST not modify an object once you have given it tooa reliable collection.**</span></span>

<span data-ttu-id="d3e2d-149">hello korrekt sätt tooupdate ett värde i en tillförlitlig samling tooget ett toohello befintliga referensvärde och Överväg hello-objektet som anges tooby denna referens som inte ändras.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-149">hello correct way tooupdate a value in a reliable collection, is tooget a reference toohello existing value and consider hello object referred tooby this reference immutable.</span></span> <span data-ttu-id="d3e2d-150">Skapa sedan ett nytt objekt som är en exakt kopia av hello ursprungliga objektet.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-150">Then, create a new object which is an exact copy of hello original object.</span></span> <span data-ttu-id="d3e2d-151">Nu kan du ändra hello tillståndet för den här nya objekt och skriva hello nytt objekt i samlingen hello så att den hämtar serialiseras toobyte matriser, tillagda toohello lokal fil och skickas toohello repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-151">Now, you can modify hello state of this new object and write hello new object into hello collection so that it gets serialized toobyte arrays, appended toohello local file and sent toohello replicas.</span></span> <span data-ttu-id="d3e2d-152">När genomför hello change(s) hello InMemory-objekt, hello lokal fil, och alla hello repliker har hello exakt samma tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-152">After committing hello change(s), hello in-memory objects, hello local file, and all hello replicas have hello same exact state.</span></span> <span data-ttu-id="d3e2d-153">Alla är bra!</span><span class="sxs-lookup"><span data-stu-id="d3e2d-153">All is good!</span></span>

<span data-ttu-id="d3e2d-154">hello koden nedan visar hello rätt metod tooupdate ett värde i en tillförlitlig samling:</span><span class="sxs-lookup"><span data-stu-id="d3e2d-154">hello code below shows hello correct way tooupdate a value in a reliable collection:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with hello same state as hello current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In hello new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update hello key’s value toohello updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a><span data-ttu-id="d3e2d-155">Definiera ändras typer tooprevent programmerare datafel</span><span class="sxs-lookup"><span data-stu-id="d3e2d-155">Define immutable data types tooprevent programmer error</span></span>
<span data-ttu-id="d3e2d-156">Vi rekommenderar vill vi gärna hello tooreport Kompilatorfel när du av misstag skapa kod som mutates tillståndet för ett objekt som du bör tooconsider ändras.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-156">Ideally, we’d like hello compiler tooreport errors when you accidentally produce code that mutates state of an object that you are supposed tooconsider immutable.</span></span> <span data-ttu-id="d3e2d-157">Men hello C#-kompileraren saknar hello möjlighet toodo detta.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-157">But, hello C# compiler does not have hello ability toodo this.</span></span> <span data-ttu-id="d3e2d-158">I så fall tooavoid potentiella programmerare buggar, rekommenderar vi att du definierar hello-typer som du använder med tillförlitlig samlingar toobe ändras typer.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-158">So, tooavoid potential programmer bugs, we highly recommend that you define hello types you use with reliable collections toobe immutable types.</span></span> <span data-ttu-id="d3e2d-159">Det innebär mer specifikt kan du behålla toocore värdetyper (till exempel siffror [Int32, UInt64, etc.], DateTime, Guid, TimeSpan och hello som).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-159">Specifically, this means that you stick toocore value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and hello like).</span></span> <span data-ttu-id="d3e2d-160">Och naturligtvis kan du också använda sträng.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-160">And, of course, you can also use String.</span></span> <span data-ttu-id="d3e2d-161">Det är bästa tooavoid samlingsegenskaper som serialisering och avserialisering av dem kan ofta kan försämra prestanda.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-161">It is best tooavoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="d3e2d-162">Men om du vill toouse samlingsegenskaper rekommenderar vi starkt hello användning av. ändras samlingar nätverksbibliotek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-162">However, if you want toouse collection properties, we highly recommend hello use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="d3e2d-163">Det här biblioteket är tillgängliga för nedladdning från http://nuget.org. Vi rekommenderar också försegling klasser och skrivskydda fält när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-163">This library is available for download from http://nuget.org. We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="d3e2d-164">hello användarinformationen typ nedan visar hur toodefine en oföränderlig skriver dra fördel av ovan nämnda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-164">hello UserInfo type below demonstrates how toodefine an immutable type taking advantage of aforementioned recommendations.</span></span>

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert hello deserialized collection tooan immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has tooset it. So instead, hello getter is public and hello setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId toohello ItemsBidding
   // collection by creating a new immutable UserInfo object with hello added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

<span data-ttu-id="d3e2d-165">hello ItemId typ är också en oåterkalleliga typen som visas här:</span><span class="sxs-lookup"><span data-stu-id="d3e2d-165">hello ItemId type is also an immutable type as shown here:</span></span>

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="d3e2d-166">Schemaversionshantering (uppgraderingar)</span><span class="sxs-lookup"><span data-stu-id="d3e2d-166">Schema versioning (upgrades)</span></span>
<span data-ttu-id="d3e2d-167">Internt, serialisera tillförlitliga samlingar med objekten. NET'S DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-167">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="d3e2d-168">hello serialiserade objekt är beständiga toohello primära repliken lokal disk och är också överförda toohello sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-168">hello serialized objects are persisted toohello primary replica’s local disk and are also transmitted toohello secondary replicas.</span></span> <span data-ttu-id="d3e2d-169">Då tjänsten utvecklas troligen kommer du toochange hello slags data (schema) kräver att din tjänst.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-169">As your service matures, it’s likely you’ll want toochange hello kind of data (schema) your service requires.</span></span> <span data-ttu-id="d3e2d-170">Du måste närma versioning av dina data med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-170">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="d3e2d-171">Först och främst kan måste du alltid vara kan toodeserialize gamla data.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-171">First and foremost, you must always be able toodeserialize old data.</span></span> <span data-ttu-id="d3e2d-172">Mer specifikt det innebär att koden deserialisering måste vara oändligt bakåtkompatibla: Version 333 av koden för tjänsten måste vara kan toooperate på data som placeras i en tillförlitlig samling av version 1 av koden för tjänsten 5 år sedan.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-172">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able toooperate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="d3e2d-173">Service-koden är dessutom uppgraderade en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-173">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="d3e2d-174">Under en uppgradering kan du alltså ha två olika versioner av koden för tjänsten körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-174">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="d3e2d-175">Du måste undvika hello ny version av koden för tjänsten använder hello nya schemat som äldre versioner av koden för tjänsten inte kanske kan toohandle hello nya schemat.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-175">You must avoid having hello new version of your service code use hello new schema as old versions of your service code might not be able toohandle hello new schema.</span></span> <span data-ttu-id="d3e2d-176">När det är möjligt bör du utforma varje version av din tjänst toobe framåtkompatibla av version 1.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-176">When possible, you should design each version of your service toobe forward compatible by 1 version.</span></span> <span data-ttu-id="d3e2d-177">Mer specifikt detta innebär att V1 av koden för tjänsten ska kunna toosimply ignorera eventuella schemaelement det explicit inte hanteras.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-177">Specifically, this means that V1 of your service code should be able toosimply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="d3e2d-178">Det måste dock vara kan toosave alla data som det inte uttryckligen veta om och bara skriva tillbaka när du uppdaterar en ordlistans nyckel eller ett värde.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-178">However, it must be able toosave any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="d3e2d-179">Medan du kan ändra hello schemat för en nyckel, måste du kontrollera att din nyckel-hash-kod och som är lika med algoritmer är stabil.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-179">While you can modify hello schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="d3e2d-180">Om du ändrar hur något av dessa algoritmer fungerar, kommer du inte att kan toolook hello nyckeln inom hello tillförlitliga ordlista någonsin igen.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-180">If you change how either of these algorithms operate, you will not be able toolook up hello key within hello reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="d3e2d-181">Du kan också utföra vad är vanligtvis enligt tooas 2-fas uppgradera.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-181">Alternatively, you can perform what is typically referred tooas a 2-phase upgrade.</span></span> <span data-ttu-id="d3e2d-182">Med en 2-fas i uppgraderingen uppgradera tjänsten från V1 tooV2: V2 innehåller hello-kod som vet hur kör inte toodeal med hello ny schemaändring, men den här koden.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-182">With a 2-phase upgrade, you upgrade your service from V1 tooV2: V2 contains hello code that knows how toodeal with hello new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="d3e2d-183">När hello V2 koden läser V1 data, fungerar på den och skriver V1-data.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-183">When hello V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="d3e2d-184">Sedan efter hello uppgraderingen är klar alla uppgradera domäner, kan du på något sätt signal toohello V2-instanser som hello uppgraderingen är klar.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-184">Then, after hello upgrade is complete across all upgrade domains, you can somehow signal toohello running V2 instances that hello upgrade is complete.</span></span> <span data-ttu-id="d3e2d-185">(Ett sätt toosignal detta är tooroll ut en uppgradering av configuration; det här är vad är detta en 2-fas uppgradering.) Nu kan hello V2 instanser läsa V1 data, konvertera den tooV2 data, fungerar på det och skriva ut den som V2-data.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-185">(One way toosignal this is tooroll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, hello V2 instances can read V1 data, convert it tooV2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="d3e2d-186">När andra instanser läsa V2 data eftersom de inte behöver tooconvert, de bara fungerar på det och skriva ut V2-data.</span><span class="sxs-lookup"><span data-stu-id="d3e2d-186">When other instances read V2 data, they do not need tooconvert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3e2d-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3e2d-187">Next Steps</span></span>
<span data-ttu-id="d3e2d-188">toolearn om hur du skapar vidarebefordra kompatibel datakontrakt finns [framåt-kompatibel datakontrakt](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-188">toolearn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="d3e2d-189">toolearn Metodtips om versionshantering datakontrakt finns [Data kontraktet versionshantering](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-189">toolearn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="d3e2d-190">toolearn hur tooimplement version feltoleranta data kontrakt, finns i [Version feltoleranta serialisering återanrop](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-190">toolearn how tooimplement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="d3e2d-191">hur tooprovide en datastruktur som kan samverka för flera versioner Se toolearn [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3e2d-191">toolearn how tooprovide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
