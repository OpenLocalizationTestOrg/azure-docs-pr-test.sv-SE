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
# <a name="working-with-reliable-collections"></a>Arbeta med tillförlitlig samlingar
Service Fabric erbjuder en tillståndskänslig programming modell tillgänglig too.NET utvecklare via tillförlitliga samlingar. Mer specifikt ger Service Fabric tillförlitliga ordlista och tillförlitlig kön klasser. När du använder dessa klasser är ditt tillstånd partitionerad (för skalbarhet) replikeras (för tillgänglighet) och överförd inom en partition (för ACID-semantik). Nu ska vi titta på en vanlig användning av en tillförlitlig Ordlisteobjekt och se vilka dess faktiskt gör.

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

Alla åtgärder på tillförlitliga ordlista objekt (förutom ClearAsync som inte går att ångra), kräver ett ITransaction-objekt. Det här objektet är kopplad till den eventuella och alla ändringar som du försöker toomake tooany tillförlitliga ordlista och/eller tillförlitlig köobjekt inom en partition. Du skaffar ett ITransaction objekt genom att anropa hello partition har Statemanager's CreateTransaction metod.

I hello koden ovan skickas hello ITransaction objektet tooa tillförlitliga ordlista AddAsync metod. Internt, ta ordlista metoder som accepterar en nyckel reader/writer lås som är associerade med hello nyckel. Om hello metoden ändrar hello nyckelvärde, hello metoden tar ett skrivlås på hello nyckel och om hello metoden läser endast från hello nyckelvärde, är ett läslås vidtas på hello nyckel. Eftersom AddAsync ändrar hello nyckeln värdet toohello nya, skickas i värdet hello nyckeln skrivskydd hämtas. Så om 2 (eller högre) trådar försöker tooadd värden med hello samma nyckel på hello samma time, en tråd skaffar hello skrivskydd och hello andra trådar blockeras. Som standard metoder blockera för in too4 sekunder tooacquire hello Lås; efter 4 sekunder hello metoder som resulterar i en TimeoutException. Metoden överlagringar finns vilket gör att du toopass ett explicit timeout-värde om du föredrar.

Vanligtvis kan du skriva din kod tooreact tooa TimeoutException genom fångar in den och du gör hello hela åtgärden (som visas i hello koden ovan). I enkla kod ansluter jag bara Task.Delay skicka 100 millisekunder varje gång. Men i verkligheten kan du använda någon typ av exponentiell inte fördröjning i stället.

När hello Lås förvärvas AddAsync lägger till hello nyckel och värdeobjekt refererar till tooan interna tillfälliga ordlista associerad med hello ITransaction-objekt. Detta görs tooprovide du med Läs-your-äger-skrivningar semantik. Som är när du anropar AddAsync, en senare anropet tooTryGetValueAsync (med hjälp av hello samma ITransaction-objekt) returnerar hello värde även om du inte har ännu har allokerats hello transaktion. Därefter AddAsync Serialiserar din nyckel och värde objekt toobyte matriser och lägger till dessa byte-matriser tooa loggfilen på hello lokala noden. Slutligen skickar AddAsync hello byte-matriser tooall hello sekundära repliker så att de har hello samma nyckel/värde-information. Även om hello nyckel/värde-information har skrivits tooa loggfil, betraktas hello information inte som en del av hello ordlistan tills hello transaktion som de är associerade med har bekräftats.

Hello koden ovan genomför hello anropet tooCommitAsync alla hello transaktion-åtgärder. Mer specifikt lägger till information commit toohello loggfilen på hello lokala noden och skickar även hello commit poster tooall hello sekundära repliker. När ett kvorum (majoritet) av hello repliker har svarat alla data ändringar anses permanent och eventuella lås som är associerade med nycklar som har ändras via hello ITransaction objekt släpps så att andra trådar/transaktioner kan ändra hello samma nycklar och deras värden.

Om CommitAsync inte anropas (vanligtvis på grund av tooan undantag som uppstod), hämtar hello ITransaction objektet bort. När avyttring objektets ogenomförda ITransaction Service Fabric läggs Avbryt information toohello lokala nodens loggfilen och behövs inga toobe skickas tooany av hello sekundära repliker. Och sedan alla som är associerade med nycklar som har ändras via hello transaktion låsen släpps.

## <a name="common-pitfalls-and-how-tooavoid-them"></a>Vanliga fallgropar och hur tooavoid dem.
Nu när du förstår hur hello tillförlitliga samlingar fungerar internt ska vi titta på några vanliga missbruk av dem. Se hello koden nedan:

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

När du arbetar med en vanlig .NET-ordlista du lägger till en nyckel/värde-toohello ordlista och ändra sedan hello-värdet för en egenskap (till exempel LastLogin). Men fungerar den här koden inte med en tillförlitlig ordlista. Kom ihåg från hello tidigare diskussion, hello anropet tooAddAsync Serialiserar hello nyckel/värde-objekt toobyte matriser, och sedan sparar hello matriser tooa lokal fil och skickar dem också toohello sekundära repliker. Om du senare ändrar en egenskap ändras hello egenskapens värde i minnet. det påverkar inte hello lokal fil eller hello data som skickas toohello repliker. Om hello processen kraschar genereras i minnet direkt. När en ny process startas eller om en annan replik blir primära, sedan hello gamla egenskapsvärdet är vad som är tillgängligt.

Jag kan inte vara påfrestande tillräckligt hur lätt det är toomake hello typ av fel som visas ovan. Och du kommer endast Lär dig mer om hello fel när hello processen kraschar. hello rätt metod toowrite hello koden är helt enkelt tooreverse hello två rader:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Här är ett annat exempel som visar vanliga fel:

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

Igen med vanlig .NET ordlistor hello koden ovan fungerar bra och är ett vanligt mönster: hello utvecklare använder en nyckel toolook upp ett värde. Om hello värdet finns ändras hello utvecklare ett egenskapsvärde. Med tillförlitlig samlingar uppvisar men den här koden hello samma problem som redan beskrivs: **du inte ändra ett objekt när du har angett det tooa tillförlitliga samling.**

hello korrekt sätt tooupdate ett värde i en tillförlitlig samling tooget ett toohello befintliga referensvärde och Överväg hello-objektet som anges tooby denna referens som inte ändras. Skapa sedan ett nytt objekt som är en exakt kopia av hello ursprungliga objektet. Nu kan du ändra hello tillståndet för den här nya objekt och skriva hello nytt objekt i samlingen hello så att den hämtar serialiseras toobyte matriser, tillagda toohello lokal fil och skickas toohello repliker. När genomför hello change(s) hello InMemory-objekt, hello lokal fil, och alla hello repliker har hello exakt samma tillstånd. Alla är bra!

hello koden nedan visar hello rätt metod tooupdate ett värde i en tillförlitlig samling:

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

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a>Definiera ändras typer tooprevent programmerare datafel
Vi rekommenderar vill vi gärna hello tooreport Kompilatorfel när du av misstag skapa kod som mutates tillståndet för ett objekt som du bör tooconsider ändras. Men hello C#-kompileraren saknar hello möjlighet toodo detta. I så fall tooavoid potentiella programmerare buggar, rekommenderar vi att du definierar hello-typer som du använder med tillförlitlig samlingar toobe ändras typer. Det innebär mer specifikt kan du behålla toocore värdetyper (till exempel siffror [Int32, UInt64, etc.], DateTime, Guid, TimeSpan och hello som). Och naturligtvis kan du också använda sträng. Det är bästa tooavoid samlingsegenskaper som serialisering och avserialisering av dem kan ofta kan försämra prestanda. Men om du vill toouse samlingsegenskaper rekommenderar vi starkt hello användning av. ändras samlingar nätverksbibliotek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Det här biblioteket är tillgängliga för nedladdning från http://nuget.org. Vi rekommenderar också försegling klasser och skrivskydda fält när det är möjligt.

hello användarinformationen typ nedan visar hur toodefine en oföränderlig skriver dra fördel av ovan nämnda rekommendationer.

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

hello ItemId typ är också en oåterkalleliga typen som visas här:

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

## <a name="schema-versioning-upgrades"></a>Schemaversionshantering (uppgraderingar)
Internt, serialisera tillförlitliga samlingar med objekten. NET'S DataContractSerializer. hello serialiserade objekt är beständiga toohello primära repliken lokal disk och är också överförda toohello sekundära repliker. Då tjänsten utvecklas troligen kommer du toochange hello slags data (schema) kräver att din tjänst. Du måste närma versioning av dina data med försiktighet. Först och främst kan måste du alltid vara kan toodeserialize gamla data. Mer specifikt det innebär att koden deserialisering måste vara oändligt bakåtkompatibla: Version 333 av koden för tjänsten måste vara kan toooperate på data som placeras i en tillförlitlig samling av version 1 av koden för tjänsten 5 år sedan.

Service-koden är dessutom uppgraderade en domän i taget. Under en uppgradering kan du alltså ha två olika versioner av koden för tjänsten körs samtidigt. Du måste undvika hello ny version av koden för tjänsten använder hello nya schemat som äldre versioner av koden för tjänsten inte kanske kan toohandle hello nya schemat. När det är möjligt bör du utforma varje version av din tjänst toobe framåtkompatibla av version 1. Mer specifikt detta innebär att V1 av koden för tjänsten ska kunna toosimply ignorera eventuella schemaelement det explicit inte hanteras. Det måste dock vara kan toosave alla data som det inte uttryckligen veta om och bara skriva tillbaka när du uppdaterar en ordlistans nyckel eller ett värde.

> [!WARNING]
> Medan du kan ändra hello schemat för en nyckel, måste du kontrollera att din nyckel-hash-kod och som är lika med algoritmer är stabil. Om du ändrar hur något av dessa algoritmer fungerar, kommer du inte att kan toolook hello nyckeln inom hello tillförlitliga ordlista någonsin igen.
>
>

Du kan också utföra vad är vanligtvis enligt tooas 2-fas uppgradera. Med en 2-fas i uppgraderingen uppgradera tjänsten från V1 tooV2: V2 innehåller hello-kod som vet hur kör inte toodeal med hello ny schemaändring, men den här koden. När hello V2 koden läser V1 data, fungerar på den och skriver V1-data. Sedan efter hello uppgraderingen är klar alla uppgradera domäner, kan du på något sätt signal toohello V2-instanser som hello uppgraderingen är klar. (Ett sätt toosignal detta är tooroll ut en uppgradering av configuration; det här är vad är detta en 2-fas uppgradering.) Nu kan hello V2 instanser läsa V1 data, konvertera den tooV2 data, fungerar på det och skriva ut den som V2-data. När andra instanser läsa V2 data eftersom de inte behöver tooconvert, de bara fungerar på det och skriva ut V2-data.

## <a name="next-steps"></a>Nästa steg
toolearn om hur du skapar vidarebefordra kompatibel datakontrakt finns [framåt-kompatibel datakontrakt](https://msdn.microsoft.com/library/ms731083.aspx).

toolearn Metodtips om versionshantering datakontrakt finns [Data kontraktet versionshantering](https://msdn.microsoft.com/library/ms731138.aspx).

toolearn hur tooimplement version feltoleranta data kontrakt, finns i [Version feltoleranta serialisering återanrop](https://msdn.microsoft.com/library/ms733734.aspx).

hur tooprovide en datastruktur som kan samverka för flera versioner Se toolearn [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
