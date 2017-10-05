---
title: "Arbeta med tillförlitlig samlingar | Microsoft Docs"
description: "Läs om bästa praxis för att arbeta med tillförlitlig samlingar."
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
ms.openlocfilehash: f53f13e4fb83b1cd370ec673e86e5311cd93055f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-reliable-collections"></a><span data-ttu-id="83276-103">Arbeta med tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="83276-103">Working with Reliable Collections</span></span>
<span data-ttu-id="83276-104">Service Fabric erbjuder en tillståndskänslig programmeringsmodell som är tillgängliga för .NET-utvecklare via tillförlitliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="83276-104">Service Fabric offers a stateful programming model available to .NET developers via Reliable Collections.</span></span> <span data-ttu-id="83276-105">Mer specifikt ger Service Fabric tillförlitliga ordlista och tillförlitlig kön klasser.</span><span class="sxs-lookup"><span data-stu-id="83276-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="83276-106">När du använder dessa klasser är ditt tillstånd partitionerad (för skalbarhet) replikeras (för tillgänglighet) och överförd inom en partition (för ACID-semantik).</span><span class="sxs-lookup"><span data-stu-id="83276-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="83276-107">Nu ska vi titta på en vanlig användning av en tillförlitlig Ordlisteobjekt och se vilka dess faktiskt gör.</span><span class="sxs-lookup"><span data-stu-id="83276-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

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

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

<span data-ttu-id="83276-108">Alla åtgärder på tillförlitliga ordlista objekt (förutom ClearAsync som inte går att ångra), kräver ett ITransaction-objekt.</span><span class="sxs-lookup"><span data-stu-id="83276-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="83276-109">Det här objektet är kopplad till den eventuella och alla ändringar som du försöker göra att alla tillförlitliga ordlista och/eller tillförlitlig kö objekt inom en partition.</span><span class="sxs-lookup"><span data-stu-id="83276-109">This object has associated with it any and all changes you’re attempting to make to any reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="83276-110">Du skaffar ett ITransaction objekt genom att anropa partitionen har Statemanager's CreateTransaction metod.</span><span class="sxs-lookup"><span data-stu-id="83276-110">You acquire an ITransaction object by calling the partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="83276-111">ITransaction-objektet har skickats till en tillförlitlig ordlista AddAsync metod i koden ovan.</span><span class="sxs-lookup"><span data-stu-id="83276-111">In the code above, the ITransaction object is passed to a reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="83276-112">Internt, ta ordlista metoder som accepterar en nyckel reader/writer lås som är associerade med nyckeln.</span><span class="sxs-lookup"><span data-stu-id="83276-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with the key.</span></span> <span data-ttu-id="83276-113">Om metoden ändrar värdet för nyckeln, metoden tar ett skrivlås på nyckeln och om metoden läser endast från den nyckelvärde, är ett läslås vidtas på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="83276-113">If the method modifies the key’s value, the method takes a write lock on the key and if the method only reads from the key’s value, then a read lock is taken on the key.</span></span> <span data-ttu-id="83276-114">Eftersom AddAsync ändrar nyckelvärde till det nya, skickas i värdet, tas den nyckeln skrivskydd.</span><span class="sxs-lookup"><span data-stu-id="83276-114">Since AddAsync modifies the key’s value to the new, passed-in value, the key’s write lock is taken.</span></span> <span data-ttu-id="83276-115">Om 2 (eller högre) trådar försöker lägga till värden med samma nyckel samtidigt, en tråd kommer aktivera skrivskydd och andra trådar blockeras på Internet.</span><span class="sxs-lookup"><span data-stu-id="83276-115">So, if 2 (or more) threads attempt to add values with the same key at the same time, one thread will acquire the write lock and the other threads will block.</span></span> <span data-ttu-id="83276-116">Som standard blockerar metoder för upp till 4 sekunder att låsa; metoderna utlösa en TimeoutException efter 4 sekunder.</span><span class="sxs-lookup"><span data-stu-id="83276-116">By default, methods block for up to 4 seconds to acquire the lock; after 4 seconds, the methods throw a TimeoutException.</span></span> <span data-ttu-id="83276-117">Metoden överlagringar finns så att du kan skicka ett explicit timeout-värde om du föredrar.</span><span class="sxs-lookup"><span data-stu-id="83276-117">Method overloads exist allowing you to pass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="83276-118">Vanligtvis kan du skriva koden för att ta hänsyn till en TimeoutException som fångar in den och försök hela (som visas i koden ovan).</span><span class="sxs-lookup"><span data-stu-id="83276-118">Usually, you write your code to react to a TimeoutException by catching it and retrying the entire operation (as shown in the code above).</span></span> <span data-ttu-id="83276-119">I enkla kod ansluter jag bara Task.Delay skicka 100 millisekunder varje gång.</span><span class="sxs-lookup"><span data-stu-id="83276-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="83276-120">Men i verkligheten kan du använda någon typ av exponentiell inte fördröjning i stället.</span><span class="sxs-lookup"><span data-stu-id="83276-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="83276-121">När låset förvärvas läggs AddAsync objektreferenser nyckel och värde i en internt tillfälliga ordlista som är associerade med ITransaction-objekt.</span><span class="sxs-lookup"><span data-stu-id="83276-121">Once the lock is acquired, AddAsync adds the key and value object references to an internal temporary dictionary associated with the ITransaction object.</span></span> <span data-ttu-id="83276-122">Detta görs för att ge dig med Läs-your-äger-skrivningar semantik.</span><span class="sxs-lookup"><span data-stu-id="83276-122">This is done to provide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="83276-123">Det vill säga när du anropar AddAsync returneras ett senare anrop till TryGetValueAsync (med samma ITransaction-objekt) värdet även om du inte har ännu har allokerats transaktionen.</span><span class="sxs-lookup"><span data-stu-id="83276-123">That is, after you call AddAsync, a later call to TryGetValueAsync (using the same ITransaction object) will return the value even if you have not yet committed the transaction.</span></span> <span data-ttu-id="83276-124">Därefter AddAsync Serialiserar din nyckel och värde objekt till byte-matriser och lägger till dessa byte-matriser i en loggfil på den lokala noden.</span><span class="sxs-lookup"><span data-stu-id="83276-124">Next, AddAsync serializes your key and value objects to byte arrays and appends these byte arrays to a log file on the local node.</span></span> <span data-ttu-id="83276-125">Slutligen skickar AddAsync byte-matriser till de sekundära replikerna så att de har samma nyckel/värde-information.</span><span class="sxs-lookup"><span data-stu-id="83276-125">Finally, AddAsync sends the byte arrays to all the secondary replicas so they have the same key/value information.</span></span> <span data-ttu-id="83276-126">Även om nyckel/värde-information har skrivits till en loggfil, anses inte informationen om en del av ordlistan tills de är associerade med transaktionen genomförts.</span><span class="sxs-lookup"><span data-stu-id="83276-126">Even though the key/value information has been written to a log file, the information is not considered part of the dictionary until the transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="83276-127">I koden ovan genomför anropet till CommitAsync alla åtgärder i transaktionen.</span><span class="sxs-lookup"><span data-stu-id="83276-127">In the code above, the call to CommitAsync commits all of the transaction’s operations.</span></span> <span data-ttu-id="83276-128">Mer specifikt läggs commit information i loggfilen på den lokala noden och skickar även commit-post till de sekundära replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-128">Specifically, it appends commit information to the log file on the local node and also sends the commit record to all the secondary replicas.</span></span> <span data-ttu-id="83276-129">När ett kvorum (majoritet) repliker har svarat alla dataändringar betraktas som permanenta och alla som är associerade med nycklar som har ändras via objektet ITransaction läslås frigörs så att andra trådar/transaktioner kan ändra samma nycklar och deras värden.</span><span class="sxs-lookup"><span data-stu-id="83276-129">Once a quorum (majority) of the replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via the ITransaction object are released so other threads/transactions can manipulate the same keys and their values.</span></span>

<span data-ttu-id="83276-130">Om CommitAsync inte anropas (vanligtvis på grund av ett undantag som genereras), hämtar ITransaction objektet bort.</span><span class="sxs-lookup"><span data-stu-id="83276-130">If CommitAsync is not called (usually due to an exception being thrown), then the ITransaction object gets disposed.</span></span> <span data-ttu-id="83276-131">Vid avyttring objektets ogenomförda ITransaction Service Fabric läggs Avbryt information till den lokala noden loggfil och ingenting måste skickas till någon av de sekundära replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information to the local node’s log file and nothing needs to be sent to any of the secondary replicas.</span></span> <span data-ttu-id="83276-132">Och sedan alla som är associerade med nycklar som har ändras via transaktionen låsen släpps.</span><span class="sxs-lookup"><span data-stu-id="83276-132">And then, any locks associated with keys that were manipulated via the transaction are released.</span></span>

## <a name="common-pitfalls-and-how-to-avoid-them"></a><span data-ttu-id="83276-133">Vanliga fallgropar och hur du undviker dem</span><span class="sxs-lookup"><span data-stu-id="83276-133">Common pitfalls and how to avoid them</span></span>
<span data-ttu-id="83276-134">Nu när du förstår hur tillförlitliga samlingar fungerar internt ska vi titta på några vanliga missbruk av dem.</span><span class="sxs-lookup"><span data-stu-id="83276-134">Now that you understand how the reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="83276-135">Se koden nedan:</span><span class="sxs-lookup"><span data-stu-id="83276-135">See the code below:</span></span>

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

<span data-ttu-id="83276-136">När du arbetar med en vanlig .NET-ordlista kan du lägga till ett nyckelvärde i ordlistan och ändra sedan värdet för en egenskap (till exempel LastLogin).</span><span class="sxs-lookup"><span data-stu-id="83276-136">When working with a regular .NET dictionary, you can add a key/value to the dictionary and then change the value of a property (such as LastLogin).</span></span> <span data-ttu-id="83276-137">Men fungerar den här koden inte med en tillförlitlig ordlista.</span><span class="sxs-lookup"><span data-stu-id="83276-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="83276-138">Kom ihåg från den tidigare diskussionen anropet till AddAsync Serialiserar nyckel/värde-objekt till byte-matriser och sparar sedan matriser till en lokal fil och också skickas till de sekundära replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-138">Remember from the earlier discussion, the call to AddAsync serializes the key/value objects to byte arrays and then saves the arrays to a local file and also sends them to the secondary replicas.</span></span> <span data-ttu-id="83276-139">Om du senare ändrar en egenskap ändras egenskapsvärdet i minnet. det påverkar inte den lokala filen eller data som skickas till replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-139">If you later change a property, this changes the property’s value in memory only; it does not impact the local file or the data sent to the replicas.</span></span> <span data-ttu-id="83276-140">Om processen kraschar genereras i minnet direkt.</span><span class="sxs-lookup"><span data-stu-id="83276-140">If the process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="83276-141">När en ny process startas eller en annan replik blir primära är gamla egenskapens värde för vad som är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="83276-141">When a new process starts or if another replica becomes primary, then the old property value is what is available.</span></span>

<span data-ttu-id="83276-142">Jag kan inte vara påfrestande tillräckligt hur lätt det är att se vilken typ av fel som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="83276-142">I cannot stress enough how easy it is to make the kind of mistake shown above.</span></span> <span data-ttu-id="83276-143">Och du bara lära dig om fel när processen kraschar.</span><span class="sxs-lookup"><span data-stu-id="83276-143">And, you will only learn about the mistake if/when the process goes down.</span></span> <span data-ttu-id="83276-144">Det korrekta sättet att skriva koden är att återföra två rader:</span><span class="sxs-lookup"><span data-stu-id="83276-144">The correct way to write the code is simply to reverse the two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="83276-145">Här är ett annat exempel som visar vanliga fel:</span><span class="sxs-lookup"><span data-stu-id="83276-145">Here is another example showing a common mistake:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

<span data-ttu-id="83276-146">Igen med vanlig .NET ordlistor koden ovan fungerar bra och är ett vanligt mönster: utvecklaren använder en nyckel för att leta upp ett värde.</span><span class="sxs-lookup"><span data-stu-id="83276-146">Again, with regular .NET dictionaries, the code above works fine and is a common pattern: the developer uses a key to look up a value.</span></span> <span data-ttu-id="83276-147">Om värdet finns ändrar utvecklaren värdet för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="83276-147">If the value exists, the developer changes a property’s value.</span></span> <span data-ttu-id="83276-148">Med tillförlitlig samlingar uppvisar men den här koden samma problem som redan diskuteras: **du inte ändra ett objekt när du har tilldelat till en tillförlitlig samling.**</span><span class="sxs-lookup"><span data-stu-id="83276-148">However, with reliable collections, this code exhibits the same problem as already discussed: **you MUST not modify an object once you have given it to a reliable collection.**</span></span>

<span data-ttu-id="83276-149">Det korrekta sättet att uppdatera ett värde i en tillförlitlig samling är att hämta en referens till det befintliga värdet och Överväg att objektet som refereras till av den här referensen inte ändras.</span><span class="sxs-lookup"><span data-stu-id="83276-149">The correct way to update a value in a reliable collection, is to get a reference to the existing value and consider the object referred to by this reference immutable.</span></span> <span data-ttu-id="83276-150">Skapa sedan ett nytt objekt som är en exakt kopia av det ursprungliga objektet.</span><span class="sxs-lookup"><span data-stu-id="83276-150">Then, create a new object which is an exact copy of the original object.</span></span> <span data-ttu-id="83276-151">Nu kan du ändra tillståndet för den här nya objekt och Skriv det nya objektet i samlingen så att den hämtar serialiseras till byte-matriser läggs till en lokal fil och skickas till replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-151">Now, you can modify the state of this new object and write the new object into the collection so that it gets serialized to byte arrays, appended to the local file and sent to the replicas.</span></span> <span data-ttu-id="83276-152">När du utför ändringar har i minnet objekt, den lokala filen och alla repliker exakt samma tillstånd.</span><span class="sxs-lookup"><span data-stu-id="83276-152">After committing the change(s), the in-memory objects, the local file, and all the replicas have the same exact state.</span></span> <span data-ttu-id="83276-153">Alla är bra!</span><span class="sxs-lookup"><span data-stu-id="83276-153">All is good!</span></span>

<span data-ttu-id="83276-154">Koden nedan visar det korrekta sättet att uppdatera ett värde i en tillförlitlig samling:</span><span class="sxs-lookup"><span data-stu-id="83276-154">The code below shows the correct way to update a value in a reliable collection:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a><span data-ttu-id="83276-155">Definiera ändras datatyper för att förhindra programmerare fel</span><span class="sxs-lookup"><span data-stu-id="83276-155">Define immutable data types to prevent programmer error</span></span>
<span data-ttu-id="83276-156">Vi rekommenderar vill vi gärna kompilatorn att rapportera fel när du av misstag skapa kod som mutates tillståndet för ett objekt som du bör du överväga att ändras.</span><span class="sxs-lookup"><span data-stu-id="83276-156">Ideally, we’d like the compiler to report errors when you accidentally produce code that mutates state of an object that you are supposed to consider immutable.</span></span> <span data-ttu-id="83276-157">Men C#-kompilatorn har inte möjlighet att göra detta.</span><span class="sxs-lookup"><span data-stu-id="83276-157">But, the C# compiler does not have the ability to do this.</span></span> <span data-ttu-id="83276-158">Så för att undvika potentiell programmerare buggar, rekommenderar vi starkt att du definierar vilka du använder med tillförlitlig samlingar som inte ändras typer.</span><span class="sxs-lookup"><span data-stu-id="83276-158">So, to avoid potential programmer bugs, we highly recommend that you define the types you use with reliable collections to be immutable types.</span></span> <span data-ttu-id="83276-159">Det innebär mer specifikt kan du hålla dig till core värdetyper (till exempel siffror [Int32, UInt64, etc.], DateTime, Guid, TimeSpan och liknande).</span><span class="sxs-lookup"><span data-stu-id="83276-159">Specifically, this means that you stick to core value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and the like).</span></span> <span data-ttu-id="83276-160">Och naturligtvis kan du också använda sträng.</span><span class="sxs-lookup"><span data-stu-id="83276-160">And, of course, you can also use String.</span></span> <span data-ttu-id="83276-161">Det är bäst att undvika samlingsegenskaper som serialisering och avserialisering av dem kan ofta kan försämra prestanda.</span><span class="sxs-lookup"><span data-stu-id="83276-161">It is best to avoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="83276-162">Men om du vill använda samlingsegenskaper rekommenderar vi användning av. ändras samlingar nätverksbibliotek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="83276-162">However, if you want to use collection properties, we highly recommend the use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="83276-163">Det här biblioteket är tillgängliga för nedladdning från http://nuget.org.</span><span class="sxs-lookup"><span data-stu-id="83276-163">This library is available for download from http://nuget.org.</span></span> <span data-ttu-id="83276-164">Vi rekommenderar också försegling klasser och skrivskydda fält när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="83276-164">We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="83276-165">Typen användarinformationen nedan visar hur du definierar en oåterkalleliga typen genom att dra nytta av dessa rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="83276-165">The UserInfo type below demonstrates how to define an immutable type taking advantage of aforementioned recommendations.</span></span>

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
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

<span data-ttu-id="83276-166">ItemId typen är också en oåterkalleliga typen som visas här:</span><span class="sxs-lookup"><span data-stu-id="83276-166">The ItemId type is also an immutable type as shown here:</span></span>

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

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="83276-167">Schemaversionshantering (uppgraderingar)</span><span class="sxs-lookup"><span data-stu-id="83276-167">Schema versioning (upgrades)</span></span>
<span data-ttu-id="83276-168">Internt, serialisera tillförlitliga samlingar med objekten. NET'S DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="83276-168">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="83276-169">De serialiserade objekt sparas på den primära repliken lokal disk och även skickas till de sekundära replikerna.</span><span class="sxs-lookup"><span data-stu-id="83276-169">The serialized objects are persisted to the primary replica’s local disk and are also transmitted to the secondary replicas.</span></span> <span data-ttu-id="83276-170">Då tjänsten utvecklas, är det troligt att du vill ändra typ av data (schema) din tjänst kräver.</span><span class="sxs-lookup"><span data-stu-id="83276-170">As your service matures, it’s likely you’ll want to change the kind of data (schema) your service requires.</span></span> <span data-ttu-id="83276-171">Du måste närma versioning av dina data med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="83276-171">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="83276-172">Först och främst kan kunna du alltid avbryta serialiseringen gamla data.</span><span class="sxs-lookup"><span data-stu-id="83276-172">First and foremost, you must always be able to deserialize old data.</span></span> <span data-ttu-id="83276-173">Mer specifikt det innebär att koden deserialisering måste vara oändligt bakåtkompatibla: Version 333 av koden för tjänsten måste kunna fungerar på data som placeras i en tillförlitlig samling av version 1 av koden för tjänsten 5 år sedan.</span><span class="sxs-lookup"><span data-stu-id="83276-173">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able to operate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="83276-174">Service-koden är dessutom uppgraderade en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="83276-174">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="83276-175">Under en uppgradering kan du alltså ha två olika versioner av koden för tjänsten körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="83276-175">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="83276-176">Måste undviker du att den nya versionen av koden för tjänsten använder det nya schemat som äldre versioner av koden för tjänsten inte kanske kan hantera det nya schemat.</span><span class="sxs-lookup"><span data-stu-id="83276-176">You must avoid having the new version of your service code use the new schema as old versions of your service code might not be able to handle the new schema.</span></span> <span data-ttu-id="83276-177">När det är möjligt bör du utforma varje version av din tjänst ska framåtkompatibla av version 1.</span><span class="sxs-lookup"><span data-stu-id="83276-177">When possible, you should design each version of your service to be forward compatible by 1 version.</span></span> <span data-ttu-id="83276-178">Det innebär mer specifikt att V1 av koden för tjänsten ska kunna bortse från alla schemaelement det explicit inte hanteras.</span><span class="sxs-lookup"><span data-stu-id="83276-178">Specifically, this means that V1 of your service code should be able to simply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="83276-179">Det måste dock att kunna spara alla data som inte uttryckligen behöver veta om och bara skriva tillbaka ut när du uppdaterar en ordlistans nyckel eller ett värde.</span><span class="sxs-lookup"><span data-stu-id="83276-179">However, it must be able to save any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="83276-180">Medan du kan ändra schemat för en nyckel, måste du kontrollera att din nyckel-hash-kod och som är lika med algoritmer är stabil.</span><span class="sxs-lookup"><span data-stu-id="83276-180">While you can modify the schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="83276-181">Om du ändrar hur något av dessa algoritmer fungerar kan du inte slå upp nyckeln i tillförlitliga ordlistan någonsin igen.</span><span class="sxs-lookup"><span data-stu-id="83276-181">If you change how either of these algorithms operate, you will not be able to look up the key within the reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="83276-182">Du kan också utföra vad vanligtvis kallas en 2-fas uppgradering.</span><span class="sxs-lookup"><span data-stu-id="83276-182">Alternatively, you can perform what is typically referred to as a 2-phase upgrade.</span></span> <span data-ttu-id="83276-183">Med en 2-fas i uppgraderingen du uppgradera din tjänst från V1 till V2: V2 innehåller koden som vet hur du arbetar med ny schemaändring men det här programmet inte köras.</span><span class="sxs-lookup"><span data-stu-id="83276-183">With a 2-phase upgrade, you upgrade your service from V1 to V2: V2 contains the code that knows how to deal with the new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="83276-184">När koden V2 läser V1 data, fungerar på den och skriver V1-data.</span><span class="sxs-lookup"><span data-stu-id="83276-184">When the V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="83276-185">Sedan, när uppgraderingen är klar över alla uppgraderingsdomäner du kan på något sätt signalerar till instanser för V2 att uppgraderingen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="83276-185">Then, after the upgrade is complete across all upgrade domains, you can somehow signal to the running V2 instances that the upgrade is complete.</span></span> <span data-ttu-id="83276-186">(Detta är ett sätt att signal att distribuera en uppgradering av configuration; det här är vad är detta en 2-fas uppgradering.) Nu, V2-instanser kan läsa V1 data, konvertera den till V2 data, fungerar på det och skriva ut den som V2-data.</span><span class="sxs-lookup"><span data-stu-id="83276-186">(One way to signal this is to roll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, the V2 instances can read V1 data, convert it to V2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="83276-187">När andra instanser läsa V2 data, de behöver inte att konvertera den, de bara fungerar på den och skriva ut V2-data.</span><span class="sxs-lookup"><span data-stu-id="83276-187">When other instances read V2 data, they do not need to convert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83276-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83276-188">Next Steps</span></span>
<span data-ttu-id="83276-189">Läs om hur du skapar vidarebefordra kompatibel datakontrakt i [framåt-kompatibel datakontrakt](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="83276-189">To learn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="83276-190">Information om metodtips för versionshantering datakontrakt finns [Data kontraktet versionshantering](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="83276-190">To learn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="83276-191">Information om hur du implementerar version feltoleranta datakontrakt finns [Version feltoleranta serialisering återanrop](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="83276-191">To learn how to implement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="83276-192">Information om hur du skapar en datastruktur som kan samverka för flera versioner finns [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="83276-192">To learn how to provide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
