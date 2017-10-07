---
title: "aaaService Fabric säkerhetskopiering och återställning | Microsoft Docs"
description: "Konceptuell dokumentationen för Service Fabric-säkerhetskopiering och återställning"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,jessebenson
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: mcoskun
ms.openlocfilehash: e502b59c84999c3fe825167383f00a5ebd70c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-restore-reliable-services-and-reliable-actors"></a>Säkerhetskopiera och återställa Reliable Services och Reliable Actors
Azure Service Fabric är en plattform för hög tillgänglighet som replikerar hello tillstånd över flera noder toomaintain hög tillgänglighet.  Även om en nod i klustret hello misslyckas, fortsätter hello tjänster därför toobe som är tillgängliga. Den här inbyggda redundans som tillhandahålls av hello plattformen kan vara tillräcklig för vissa, i vissa fall är det lämpligt att hello service tooback data (tooan externa store).

> [!NOTE]
> Den kritiska toobackup och återställa dina data (och kontrollera att den fungerar som förväntat) så att du kan återställa från data går förlorade.
> 
> 

En tjänst kan exempelvis vilja tooback upp data i ordning tooprotect från hello följande scenarier:

- Hej för händelsen hello permanent förlorade en hela Service Fabric-klustret.
- Permanent förlorade en majoritet av hello repliker för en partition för tjänsten
- Administrativa fel där hello tillstånd hämtar eller förstörs. T.ex, kan detta inträffa om en administratör med tillräcklig behörighet för felaktigt tar bort hello tjänst.
- Programfel i hello service som orsakar att data skadas. T.ex, kan det hända när en tjänst koduppgradering startas skrivning felaktiga data tooa tillförlitliga samling. I sådana fall hello både koden och hello data kan ha toobe återställs tooan tidigare tillstånd.
- Offline databearbetning. Det kan vara praktiskt toohave offline bearbetning av data för business intelligence som sker separat från hello-tjänsten som genererar hello data.

hello säkerhetskopiering/återställning funktionen tillåter tjänster som bygger på hello Reliable Services API toocreate och återställning av säkerhetskopior. hello att säkerhetskopiering API: er som tillhandahålls av hello plattform säkerhetskopiorna av en tjänst partition tillstånd, utan blockerar Läs- eller skrivåtgärder. hello återställning API: er kan en tjänst partition tillstånd toobe återställts från en säkerhetskopia av valda.

## <a name="types-of-backup"></a>Typer av säkerhetskopiering
Det finns två alternativ för säkerhetskopiering: fullständig och inkrementell.
En fullständig säkerhetskopia är en säkerhetskopia som innehåller alla hello data som krävs för toorecreate hello status hello replik: kontrollpunkter och alla loggposter.
Eftersom den har hello kontrollpunkter och hello loggen kan du återställa en fullständig säkerhetskopia ensamt.

hello problem med fullständiga säkerhetskopieringar uppstår när hello kontrollpunkter är stora.
En replik som har 16 GB tillstånd har till exempel kontrollpunkter som utgör cirka too16 GB.
Om vi har ett mål för återställningspunkt fem minuter behöver hello replica toobe säkerhetskopieras var femte minut.
Varje gång den säkerhetskopierar måste toocopy 16 GB kontrollpunkter dessutom too50 MB (kan konfigureras med hjälp av `CheckpointThresholdInMB`) med loggar.

![Fullständig säkerhetskopiering exempel.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

hello lösning toothis problemet är inkrementella säkerhetskopieringar där säkerhetskopian bara innehåller loggposter hello ändrats sedan den senaste säkerhetskopieringen av hello.

![Exempel för inkrementell säkerhetskopiering.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Eftersom säkerhetskopior är endast ändringar sedan hello senaste säkerhetskopieringen (inte omfattar hello kontrollpunkter) är de tenderar toobe snabbare, men de kan inte återställas på egen hand.
toorestore en inkrementell säkerhetskopia hello hela loggsäkerhetskopieringssekvensen krävs.
En säkerhetskopiering kedja följt av ett antal sammanhängande inkrementella säkerhetskopieringar är en kedja av säkerhetskopior som börjar med en fullständig säkerhetskopia.

## <a name="backup-reliable-services"></a>Säkerhetskopiering Reliable Services
hello service författare har fullständig kontroll över när toomake säkerhetskopieringar och där säkerhetskopiorna lagras.

toostart en säkerhetskopia hello-tjänsten måste tooinvoke hello ärvda medlemmen funktionen `BackupAsync`.  
Säkerhetskopieringar kan göras från primära repliker och de behöver skriva status toobe beviljas.

Enligt nedan, `BackupAsync` tar in en `BackupDescription` objekt, där en kan ange en fullständig eller inkrementell säkerhetskopiering, samt en Återanropsfunktionen, `Func<< BackupInfo, CancellationToken, Task<bool>>>` som anropas när hello säkerhetskopieringsmappen har skapats lokalt och flyttas redo toobe ut toosome extern lagring.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Begäran tootake en inkrementell säkerhetskopia kan misslyckas med `FabricMissingFullBackupException`. Det här undantaget anger att en av följande saker hello sker:

- hello replik har aldrig tagit en fullständig säkerhetskopiering eftersom det har blivit primära,
- Vissa av hello loggposter eftersom hello senaste säkerhetskopieringen har trunkerats eller
- replik skickades hello `MaxAccumulatedBackupLogSizeInMB` gränsen.

Användare kan öka hello sannolikheten för att kunna toodo inkrementella säkerhetskopieringar genom att konfigurera `MinLogSizeInMB` eller `TruncationThresholdFactor`.
Observera att om du ökar värdena ökas hello per replik diskanvändning.
Mer information finns i [Reliable Services-konfiguration](service-fabric-reliable-services-configuration.md)

`BackupInfo`innehåller information om hello säkerhetskopiering, inklusive hello platsen för hello mappen där hello runtime sparade hello säkerhetskopiering (`BackupInfo.Directory`). hello Återanropsfunktionen kan flytta hello `BackupInfo.Directory` tooan extern butik eller en annan plats.  Den här funktionen returnerar också bool som anger om det var kan toosuccessfully flytta hello tooits mål mapp för säkerhetskopiering.

hello följande kod visar hur hello `BackupCallbackAsync` metoden kan vara används tooupload hello säkerhetskopiering tooAzure lagring:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

I exemplet som föregår hello `ExternalBackupStore` är hello exempel klass som används toointerface med Azure Blob storage och `UploadBackupFolderAsync` är hello-metod som komprimerar hello mapp och placerar den i hello Azure Blob store.

Tänk på följande:

  - Det kan finnas endast en säkerhetskopiering relä per replikering vid en given tidpunkt. Mer än en `BackupAsync` samtal i taget genereras `FabricBackupInProgressException` toolimit inflight säkerhetskopieringar tooone.
  - Om en replik misslyckas över när en säkerhetskopiering pågår kanske hello-säkerhetskopiering inte har slutförts. Därför när hello växling vid fel är klar, är det hello service ansvar toorestart hello säkerhetskopiering genom att anropa `BackupAsync` efter behov.

## <a name="restore-reliable-services"></a>Återställa Reliable Services
I allmänhet indelas Hej då du kan behöva tooperform en återställningsåtgärd i dessa kategorier:

  - hello service partitionera förlorade data. Hello disk för två av tre repliker för en partition (inklusive hello primära repliken) hämtar skadad eller rensas. hello nya primära behöva toorestore data från en säkerhetskopia.
  - hello hela tjänsten går förlorad. Till exempel en administratör tar bort hello hela tjänsten och hello-tjänsten och hello data måste därför toobe återställs.
  - hello service replikerade skadad programdata (t.ex. på grund av ett fel i programmet). I det här fallet hello-tjänsten har toobe uppgraderas eller återställts tooremove hello orsaken till hello skador och icke-skadade data har återställts toobe.

Medan flera metoder är möjligt, vi erbjuder några exempel på med `RestoreAsync` toorecover från hello ovan scenarier.

## <a name="partition-data-loss-in-reliable-services"></a>Partitionen dataförlust i Reliable Services
I det här fallet hello runtime skulle automatiskt identifiera hello dataförlust och anropa hello `OnDataLossAsync` API.

hello service författare måste tooperform hello följande toorecover:

  - Åsidosätt hello virtuella basklassmetoden `OnDataLossAsync`.
  - Hitta hello senaste säkerhetskopiering hello externa platsen som innehåller säkerhetskopior hello-tjänsten.
  - Hämta senaste säkerhetskopian av hello (och packa hello säkerhetskopiering i hello säkerhetskopieringsmappen om den har komprimerats).
  - Hej `OnDataLossAsync` metoden ger ett `RestoreContext`. Anropa hello `RestoreAsync` API på hello tillhandahålls `RestoreContext`.
  - Returnera true om hello återställningen lyckades.

Följande är exempel på implementering av hello `OnDataLossAsync` metoden:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription`skickade toohello `RestoreContext.RestoreAsync` anrop innehåller en medlem som kallas `BackupFolderPath`.
När du återställer en fullständig säkerhetskopia, detta `BackupFolderPath` ska anges toohello lokal sökväg till hello-mapp som innehåller en fullständig säkerhetskopiering.
När du återställer en fullständig säkerhetskopia och ett antal inkrementella säkerhetskopieringar `BackupFolderPath` ska anges toohello lokal sökväg på hello-mapp som innehåller inte bara hello fullständig säkerhetskopia, men även alla hello inkrementella säkerhetskopieringar.
`RestoreAsync`Anropet kan utlösa `FabricMissingFullBackupException` om hello `BackupFolderPath` som inte innehåller en fullständig säkerhetskopia.
Den kan också ge `ArgumentException` om `BackupFolderPath` har brutits kedja för säkerhetskopior.
Om den innehåller hello fullständig säkerhetskopiering hello först inkrementell och hello tredje inkrementell säkerhetskopiering men ingen hälsningspaket andra inkrementell säkerhetskopiering.

> [!NOTE]
> Hej RestorePolicy är tooSafe som standard.  Det innebär att hello `RestoreAsync` API misslyckas med ArgumentException om den identifierar hello säkerhetskopiering mappen innehåller ett tillstånd som är äldre än eller lika med toohello tillstånd som finns i den här repliken.  `RestorePolicy.Force`kan vara används tooskip säkerhet kontrollen. Det har angetts som en del av `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Borttagna eller förlorade service
Om en tjänst har tagits bort, måste du först återskapa hello-tjänsten innan hello data kan återställas.  Det är viktigt toocreate hello tjänst med samma konfiguration, t.ex. partitioneringsschema så som hello data kan återställas sömlöst hello.  När hello-tjänsten är igång, hello API toorestore data (`OnDataLossAsync` ovan) har toobe anropas för varje partition för den här tjänsten. Ett sätt att uppnå detta är med hjälp av `[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)` för varje partition.  

Från och med nu, är implementeringen hello samma som hello ovan scenario. Varje partition måste toorestore hello senaste relevanta säkerhetskopiering från hello extern butik. Ett villkor är hello partitionen ID kan ha nu ändrats, eftersom hello runtime skapar partitions-ID: N dynamiskt. Hello-tjänsten måste därför toostore hello lämplig partitionsinformation och service name tooidentify hello rätt senaste säkerhetskopiering toorestore från för varje partition.

> [!NOTE]
> Det rekommenderas inte toouse `FabricClient.ServiceManager.InvokeDataLossAsync` på varje partition toorestore hello hela tjänsten, eftersom som kan skada din klustertillstånd.
> 

## <a name="replication-of-corrupt-application-data"></a>Replikering av skadade programdata
Om uppgradering av hello nyligen distribuerade programmet har ett programfel, som kan orsaka skadade data. Till exempel kan en uppgradering av programmet starta tooupdate alla telefonpost i en tillförlitlig ordlista med ett ogiltigt riktnummer.  I det här fallet replikeras hello ogiltiga telefonnummer sedan Service Fabric inte är medveten om hello slags hello data som lagras.

hello först öppna toodo när du identifiera sådana en flagranta programfel som gör att data skadas är toofreeze hello-tjänst på programnivå hello och, om möjligt uppgradera toohello version av hello programkod som saknar hello programfel.  Även efter hello service-koden är fast, hello data kan fortfarande vara skadade och därmed data måste toobe återställs.  I sådana fall kan kanske det inte tillräckligt toorestore hello senaste säkerhetskopiering, eftersom hello senaste säkerhetskopieringar kan också vara skadad.  Du har alltså toofind hello senaste säkerhetskopia som togs innan hello data har skadats.

Om du inte är säker på vilken säkerhetskopior är skadade kan du distribuera ett nytt Service Fabric-kluster och återställa hello säkerhetskopior av berörda partitioner precis som hello ovan ”Borttaget eller förlorade service” scenario.  Börja återställa hello säkerhetskopior från hello senaste toohello minst för varje partition. När du har hittat en säkerhetskopiering som inte har hello skadade flytta/ta bort alla säkerhetskopior för den här partitionen som var (än att säkerhetskopiering). Upprepa proceduren för varje partition. Nu när `OnDataLossAsync` anropas på hello partitionen i hello produktionskluster hello senaste säkerhetskopia finns hello externa arkivet ska vara hello en utvald av hello ovan processen.

Nu hello stegen i hello ”Deleted eller förlorade service” kan vara användas toorestore hello tillstånd hello Tjänststatus toohello innan hello buggy kod skadad hello tillstånd.

Tänk på följande:

  - När du återställer är det används en risk att hello säkerhetskopia återställs äldre än hello tillståndet för hello partitionen innan hello data gick förlorade. Därför bör du återställa endast som en sista utväg toorecover så mycket data som möjligt.
  - hello sträng som representerar hello säkerhetskopiering mappsökvägen och hello sökvägar för filer i hello säkerhetskopieringsmappen kan vara större än 255 tecken, beroende på hello FabricDataRoot sökväg och programtyp namnlängd. Detta kan orsaka vissa .NET-metoder som `Directory.Move`, toothrow hello `PathTooLongException` undantag. En lösning är toodirectly anropa kernel32 API: er, som `CopyFile`.

## <a name="backup-and-restore-reliable-actors"></a>Säkerhetskopiering och återställning Reliable Actors


Tillförlitliga aktörer Framework bygger på Reliable Services. Hej ActorService som är värd för hello actor(s) är en tillståndskänslig tillförlitlig tjänst. Därför måste alla hello säkerhetskopiering och återställning tillgängliga funktioner i Reliable Services är också tillgängliga tooReliable aktörer (utom beteenden som är specifika för tillståndsprovidern). Eftersom säkerhetskopior kommer att vidtas på grundval av per partition, tillstånd för alla aktörer i att partition ska säkerhetskopieras (och återställningen liknar och sker på grundval av per partition). tooperform säkerhetskopiering/återställning hello tjänstens ägare ska skapa en anpassad aktören service-klass som härleds från klassen ActorService och sedan säkerhetskopiering/återställning liknande tooReliable tjänster enligt beskrivningen ovan i föregående avsnitt.

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo)
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

När du skapar en anpassad aktören tjänsteklass måste tooregister som också när du registrerar hello aktören.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
   (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

hello tillstånd standardprovidern för Reliable Actors är `KvsActorStateProvider`. Inkrementell säkerhetskopiering är inte aktiverad som standard för `KvsActorStateProvider`. Du kan aktivera inkrementell säkerhetskopiering genom att skapa `KvsActorStateProvider` med hello lämpliga inställningen i sin konstruktor och sedan överföra tooActorService konstruktorn enligt följande kodavsnitt:

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

När inkrementell säkerhetskopiering har aktiverats, tar en inkrementell säkerhetskopia kan misslyckas med FabricMissingFullBackupException av något av följande skäl och tootake en fullständig säkerhetskopiering behöver innan du tar inkrementella säkerhetskopiorna:

  - hello replik har aldrig tagit en fullständig säkerhetskopiering eftersom den blev primära.
  - Vissa av hello loggposter trunkerades eftersom senaste säkerhetskopian skapades.

När inkrementell säkerhetskopiering är aktiverad `KvsActorStateProvider` använder inte cirkulär buffert toomanage loggen innehåller information och trunkerar den regelbundet. Om ingen säkerhetskopia är upptaget av användaren under 45 minuter, trunkerar hello system automatiskt hello loggposter. Intervallet kan konfigureras genom att ange `logTrunctationIntervalInMinutes` i `KvsActorStateProvider` konstruktor (liknande toowhen aktiverar inkrementell säkerhetskopiering). också kan hämta trunkerade hello loggposter om primära repliken behöver toobuild en annan replik genom att skicka alla data.

När du gör återställningen från en säkerhetskopiering kedja liknande tooReliable tjänster ska hello BackupFolderPath innehålla underkataloger med en underkatalog med fullständig säkerhetskopiering och andra underkataloger som innehåller inkrementella säkerhetskopiorna. hello återställning API genereras FabricException med ett meddelande om hello loggsäkerhetskopieringssekvensen valideringen misslyckas. 

> [!NOTE]
> `KvsActorStateProvider`ignorerar för tillfället hello alternativet RestorePolicy.Safe. Stöd för den här funktionen är planerad i en kommande version.
> 

## <a name="testing-backup-and-restore"></a>Testa säkerhetskopiering och återställning
Det är viktigt tooensure som kritiska data säkerhetskopieras och återställas från. Detta kan göras genom att anropa hello `Start-ServiceFabricPartitionDataLoss` cmdlet i PowerShell som kan orsaka dataförlust i en viss partition tootest om hello data säkerhetskopiera och återställa funktioner för tjänsten fungerar som förväntat.  Det är också möjligt tooprogrammatically anropa förlust av data och återställa från händelsen samt.

> [!NOTE]
> Du kan hitta ett exempel på implementering av säkerhetskopiering och återställning av funktioner i hello referens webbprogrammet på GitHub. Tittar du på hello `Inventory.Service` tjänsten för mer information.
> 
> 

## <a name="under-hello-hood-more-details-on-backup-and-restore"></a>Under huven hello: Mer information om säkerhetskopiering och återställning
Här är några mer information om säkerhetskopiering och återställning.

### <a name="backup"></a>Säkerhetskopiering
hello tillförlitliga Tillståndshanterare ger hello möjlighet toocreate konsekvent säkerhetskopieringar utan att blockera alla läsa eller skrivåtgärder. toodo så, den använder en mekanism för datapersistence kontrollpunkts- och loggfiler.  hello tillförlitliga Tillståndshanterare tar fuzzy (lightweight) kontrollpunkter vid vissa punkter toorelieve tryck från hello transaktionella loggen och förbättra återställningstiden.  När `BackupAsync` anropas, hello tillförlitliga Tillståndshanterare instruerar alla tillförlitliga objekt toocopy deras senaste kontrollpunkten tooa lokala backup-mappen.  Hello tillförlitliga Tillståndshanterare kopierar sedan alla loggposter från hello ”start pekaren” toohello senaste loggpost till hello säkerhetskopieringsmappen.  Eftersom alla hello loggposter in toohello senaste loggpost ingår i hello säkerhetskopiering och hello tillförlitliga Tillståndshanterare bevarar write-ahead loggning, hello tillförlitliga Tillståndshanterare garanterar att alla transaktioner som genomförs (`CommitAsync` returnerade har) ingår i hello säkerhetskopiering.

En transaktion som sparar efter `BackupAsync` har anropats kanske eller kanske inte hello säkerhetskopiering.  När hello lokala säkerhetskopieringsmappen har fyllts av hello-plattformen (d.v.s. lokal säkerhetskopiering har slutförts av hello runtime) hello service säkerhetskopiering återanropet anropas.  Den här återanrop ansvarar för att flytta hello säkerhetskopieringsmappen tooan extern plats, till exempel Azure Storage.

### <a name="restore"></a>Återställ
hello tillförlitliga Tillståndshanterare innehåller hello möjlighet toorestore från en säkerhetskopia med hello `RestoreAsync` API.  
Hej `RestoreAsync` metod på `RestoreContext` kan endast anropas inuti hello `OnDataLossAsync` metod.
Hej bool som returneras av `OnDataLossAsync` anger om hello tjänsten återställd dess tillstånd från en extern källa.
Om hello `OnDataLossAsync` returnerar true, Service Fabric återskapar alla andra repliker från denna primära Server. Service Fabric säkerställer att repliker som tar emot `OnDataLossAsync` anropa första övergången toohello primära rollen men inte beviljas läses status eller skriva status.
Detta innebär att för StatefulService implementerare `RunAsync` inte anropas förrän `OnDataLossAsync` har slutförts.
Sedan `OnDataLossAsync` kommer att anropas på hello nya primära.
Tills en tjänst är slutförd detta API har (genom att returnera true eller false) och är klar hello relevanta omkonfiguration, kommer hello API att anropas i taget.

`RestoreAsync`först utelämnar alla befintliga tillstånd i hello primära repliken som den anropades på.  
Hello tillförlitliga Tillståndshanterare skapar sedan alla tillförlitliga hello-objekt som finns i hello säkerhetskopieringsmappen.  
Hello tillförlitliga objekt är sedan instrueras toorestore från deras kontrollpunkter i hello säkerhetskopieringsmappen.  
Slutligen hello tillförlitliga Tillståndshanterare återställer dess egna tillstånd från hello loggposter i hello säkerhetskopieringsmappen och utför återställningen.  
Som en del av återställningsprocessen hello är åtgärder från hello ”startpunkt” som har commit loggposter i hello säkerhetskopieringsmappen upprepat toohello tillförlitliga objekt.  
Det här steget säkerställer att hello återställda är konsekvent.

## <a name="next-steps"></a>Nästa steg
  - [Tillförlitliga samlingar](service-fabric-work-with-reliable-collections.md)
  - [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
  - [Reliable Services-meddelanden](service-fabric-reliable-services-notifications.md)
  - [Tillförlitliga tjänstkonfiguration](service-fabric-reliable-services-configuration.md)
  - [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

