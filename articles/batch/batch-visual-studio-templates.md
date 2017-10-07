---
title: "aaaStart skapar Batch-lösningar med Visual Studio-projektmallar - Azure | Microsoft Docs"
description: "Lär dig mer om Visual Studio-projektmallar hur du kan implementera och köra din beräkningsintensiva arbetsbelastningar i Azure Batch."
services: batch
documentationcenter: .net
author: fayora
manager: timlt
editor: 
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61c480ddc4dffd66c01220a137a3e852e39c338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-project-templates-toojump-start-batch-solutions"></a>Batch-lösningar för Visual Studio-projekt mallar toojump-start

hello **Jobbhanteraren** och **aktivitet Processor Visual Studio mallar** ange koden toohelp för Batch du tooimplement och kör din beräkningsintensiva arbetsbelastningar på Batch med hello minst mängd arbete. Det här dokumentet beskriver dessa mallar och vägledning för hur toouse dem.

> [!IMPORTANT]
> Den här artikeln beskrivs bara information tillämpliga toothese två mallar och förutsätter att du är bekant med hello Batch-tjänsten och viktiga begrepp relaterade tooit: pooler, compute-noder, jobb och uppgifter, job manager-uppgifter, miljövariabler och andra relevant information. Du hittar mer information finns i [grunderna i Azure Batch](batch-technical-overview.md), [Batch funktionsöversikt för utvecklare](batch-api-basics.md), och [komma igång med hello Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md).
> 
> 

## <a name="high-level-overview"></a>Översikt på hög nivå
hello Job Manager och uppgiften Processor mallar kan vara användbart två komponenter som används toocreate:

* En manager projektaktivitet som implementerar en förgrening för jobbet som kan dela ett jobb upp på flera aktiviteter som kan köras fristående parallellt.
* En uppgift processor som kan vara används tooperform föregående bearbetning och efter bearbetning runt en kommandorad för programmet.

Till exempel i ett scenario med film återgivning skulle hello jobbet delningslisten göra en enda film jobbet till hundratals eller tusentals olika uppgifter som behandlar enskilda ramar separat. På motsvarande sätt skulle hello uppgiften processor anropa hello återgivningen programmet och alla beroende processer som är nödvändiga toorender varje ram, samt utföra eventuella ytterligare åtgärder (till exempel kopiering hello renderas ram tooa lagringsplats).

> [!NOTE]
> hello Job Manager och uppgiften Processor mallar är oberoende av varandra, så du kan välja toouse båda eller bara en av dem, beroende på hello krav för din beräknings-jobb och på dina inställningar.
> 
> 

I hello diagrammet nedan visas ett jobb för beräkning som använder de här mallarna går igenom tre steg:

1. hello klientkod (t.ex. program, webbtjänsten, etc.) skickar ett jobb toohello Batch-tjänsten på Azure som anger att dess jobbet manager hello job manager aktivitetsprogram.
2. hello Batch-tjänsten kör hello projektaktivitet manager på en beräkningsnod och hello jobbet delningslisten startar hello angetts antal aktiviteten processor uppgifter på som många beräkningsnoderna som krävs, baserat på hello parametrar och specifikationer i hello jobbet delningslisten kod.
3. hello uppgiften processor åtgärder körs oberoende av varandra, parallellt, tooprocess hello indata och generera hello utdata.

![Diagram över hur klientkod samverkar med hello Batch-tjänsten][diagram01]

## <a name="prerequisites"></a>Krav
toouse hello Batch mallar, behöver du hello följande:

* En dator med Visual Studio 2015 installerat. Batch-mallar är för närvarande stöds endast för Visual Studio 2015.
* hello Batch mallar, som är tillgängliga från hello [Visual Studio-galleriet] [ vs_gallery] som Visual Studio-tillägg. Det finns två sätt tooget hello mallar:
  
  * Installera hello mallar med hjälp av hello **tillägg och uppdateringar** dialogrutan i Visual Studio (Mer information finns i [söka och med hjälp av Visual Studio-tillägg][vs_find_use_ext]). I hello **tillägg och uppdateringar** dialogrutan, söka efter och hämta hello följande två tillägg:
    
    * Azure Batch Job Manager med jobbet delningslisten
    * Azure Batch uppgiften Processor
  * Ladda ned hello mallar från hello online galleriet för Visual Studio: [projektmallar för Microsoft Azure Batch][vs_gallery_templates]
* Om du planerar toouse hello [programpaket](batch-application-packages.md) funktionen toodeploy hello job manager och uppgiften processor toohello Batch compute-noder måste du toolink en storage-konto tooyour Batch-kontot.

## <a name="preparation"></a>Förberedelse
Vi rekommenderar att du skapar en lösning som kan innehålla din jobbhanteraren samt aktivitet processorn, eftersom detta kan göra det enklare tooshare kod mellan jobbhanteraren och uppgiften processor program. toocreate den här lösningen gör du följande:

1. Öppna Visual Studio och välj **filen** > **ny** > **projekt**.
2. Under **mallar**, expandera **andra projekttyper**, klickar du på **Visual Studio-lösningar**, och välj sedan **tomt lösning**.
3. Ange ett namn som beskriver ditt program och hello syftet med den här lösningen (t.ex. ”LitwareBatchTaskPrograms”).
4. toocreate hello ny lösning, klickar du på **OK**.

## <a name="job-manager-template"></a>Job Manager-mall
hello Job Manager mall kan tooimplement en projektaktivitet manager som kan utföra hello följande åtgärder:

* Dela upp ett jobb i flera uppgifter.
* Skicka dessa uppgifter toorun på Batch.

> [!NOTE]
> Läs mer om jobb manager uppgifter [Batch funktionsöversikt för utvecklare](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-hello-template"></a>Skapa en Job Manager med hjälp av hello mall
tooadd en job manager toohello lösning som du skapade tidigare, så här:

1. Öppna din befintliga lösning i Visual Studio.
2. I Solution Explorer, högerklicka på hello lösning, klickar du på **Lägg till** > **nytt projekt**.
3. Under **Visual C#**, klickar du på **moln**, och klicka sedan på **Azure Batch Job Manager med jobbet delningslisten**.
4. Ange ett namn som beskriver ditt program och identifierar det här projektet som hello jobbhanteraren (t.ex.) ”LitwareJobManager”).
5. toocreate hello projektet klickar du på **OK**.
6. Slutligen build hello projektet tooforce Visual Studio tooload alla refererar till NuGet-paket och tooverify som hello projektet är giltigt innan du börjar att ändra den.

### <a name="job-manager-template-files-and-their-purpose"></a>Job Manager mallfilerna och deras syfte
När du skapar ett projekt med hello Job Manager mallen genererar tre grupper av kodfiler:

* hello huvudsakliga programfilen (Program.cs). Innehåller hello startpunkt för programmet och översta undantagshantering. Du bör inte normalt behöver toomodify detta.
* hello Framework-katalogen. Innehåller hello-filer som är ansvarig för hello-formaterad' arbete som utförs av hello job manager-program – uppackning parametrar, lägga till uppgifter toohello batchjobb osv. Du bör inte normalt måste toomodify dessa filer.
* hello delningslisten fil (JobSplitter.cs). Detta är där kommer att placeras din programspecifika logik för att dela upp ett jobb i aktiviteter.

Naturligtvis du kan lägga till ytterligare filer som krävs toosupport jobbet delningslisten koden, beroende på hello komplexitet hello jobbet dela logik.

hello mallen skapas också standard .NET projektfiler, till exempel en .csproj filen app.config, Packages.config-fil, osv.

hello resten av det här avsnittet beskrivs hello andra filer och deras kodstruktur och förklaras vad varje klass gör.

![Visual Studio Solution Explorer visar hello Job Manager mallen lösning][solution_explorer01]

**Framework-filer**

* `Configuration.cs`: Kapslar in hello inläsning av jobb konfigurationsdata som Batch-kontoinformation, länkade lagringskontouppgifter, projekt- och information och jobbparametrar. Det ger också tillgång tooBatch definierats miljövariabler (se miljöinställningar för aktiviteter i hello Batch dokumentationen) via hello Configuration.EnvironmentVariable klass.
* `IConfiguration.cs`: Sammanfattning Hej implementering av hello Configuration klass, så att du kan testa enheten ditt jobb delningslisten med hjälp av en förfalskad eller fingerad konfigurationsobjektet.
* `JobManager.cs`: Samordnar hello komponenter i hello job manager-program. Den är ansvarig för hello initierar hello jobbet delningslisten, anropar hello jobbet delningslisten och sändning hello uppgifter som returneras av hello jobbet delningslisten toohello migreringsaktivitetens.
* `JobManagerException.cs`: Representerar ett fel som kräver hello job manager tooterminate. JobManagerException är används toowrap 'förväntades' fel där specifika diagnostisk information kan anges som en del av avslutning.
* `TaskSubmitter.cs`: Den här klassen är ansvarig tooadding uppgifter som returneras av hello jobbet delningslisten toohello Batch-jobbet. hello JobManager klassen aggregerar hello aktivitetssekvensen i batchar för effektiv men rimlig tillägg toohello jobbet och sedan anropar TaskSubmitter.SubmitTasks på en bakgrundstråd för varje grupp.

**Jobbet delningslisten**

`JobSplitter.cs`: Den här klassen innehåller programspecifika logik för att dela hello jobbet i aktiviteter. hello framework anropar hello JobSplitter.Split metoden tooobtain en serie uppgifter som läggs till toohello jobb som hello-metoden returnerar dem. Detta är hello klassen där du matar in hello logiken för ditt jobb. Implementera hello dela metoden tooreturn en sekvens av CloudTask instanser som representerar hello aktiviteter som du vill toopartition hello jobb.

**Standard projektfiler för .NET-kommandorad**

* `App.config`: Standard .NET-programkonfigurationsfilen.
* `Packages.config`: Standard NuGet-paketet beroende fil.
* `Program.cs`: Innehåller hello startpunkt för programmet och översta undantagshantering.

### <a name="implementing-hello-job-splitter"></a>Implementera hello jobbet delningslisten
När du öppnar hello Job Manager mallprojekt har hello projektet hello JobSplitter.cs filen öppnas som standard. Du kan implementera hello dela logik för hello aktiviteter i din arbetsbelastning med hjälp av hello Split() metoden visas nedan:

```csharp
/// <summary>
/// Gets hello tasks into which toosplit hello job. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello job manager framework invokes hello Split method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and hello "yield return" statement; this allows tasks toobe added
/// and toostart running while splitting is still in progress.
/// </summary>
/// <returns>hello tasks toobe added toohello job. Tasks are added automatically
/// by hello job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for hello split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> hello kommenterade avsnitt i hello `Split()` metoden är endast hello-avsnitt i hello Job Manager kod som är avsedd för du toomodify genom att lägga till hello logik toosplit jobb i olika aktiviteter. Om du vill toomodify ett annat avsnitt i hello mall, kontrollera att du har bekantat med hur Batch fungerar och prova några av hello [Batch-kodexempel][github_samples].
> 
> 

Implementeringen Split() har åtkomst till:

* Hej jobbparametrar via hello `_parameters` fältet.
* Hej CloudJob objekt som representerar hello jobb via hello `_job` fältet.
* Hej CloudTask objekt som representerar hello manager projektaktivitet via hello `_jobManagerTask` fältet.

Din `Split()` implementering behöver inte tooadd uppgifter toohello jobbet direkt. I stället koden ska returnera en sekvens av CloudTask objekt och de läggas automatiskt till toohello jobb av hello framework klasser som anropar hello jobbet delningslisten. Det är vanligt toouse C# 's iterator (`yield return`) funktion tooimplement jobbet delarna eftersom detta gör att hello uppgifter toostart körs så snart som möjligt, i stället för att väntar på att alla aktiviteter toobe beräknas.

**Jobbet delningslisten misslyckades**

Om jobbet-delningslisten påträffar ett fel, bör det antingen:

* Avsluta hello sekvens med hello C# `yield break` instruktionen, där case hello jobbhanteraren behandlas som slutförd, eller
* Utlös ett undantag, där case hello jobbhanteraren behandlas som misslyckad och kan göras beroende på hur hello-klienten har konfigurerats).

I båda fallen måste alla uppgifter returnerades redan av hello jobbet delningslisten och tillagda toohello Batch-jobbet kommer att vara berättigad toorun. Klicka om du inte vill att den här toohappen, kan du:

* Avsluta hello jobb innan man returneras från hello jobbet delningslisten
* Formulera hello hela aktiviteten till samlingen innan det returneras (det vill säga returnera en `ICollection<CloudTask>` eller `IList<CloudTask>` i stället för att implementera din jobbet delningslisten med C#-iterator)
* Använd aktiviteten beroenden toomake alla aktiviteter är beroende av hello slutförande av hello job manager

**Jobbet manager återförsök**

Om hello jobbhanteraren misslyckas utförs den igen av hello Batch-tjänsten beroende på hello klientinställningar försök igen. Detta är generellt sett är säkra, eftersom när hello framework lägger till aktiviteter toohello jobb, ignoreras alla aktiviteter som redan finns. Men om beräkning av uppgifter är dyr, kanske vill du inte tooincur hello kostnaden för att beräkna aktiviteter som redan har lagts toohello jobb. däremot om hello kör är inte garanterat toogenerate hello samma aktivitets-ID och sedan hello 'Ignorera dubbletter' beteende inte kommer startar. I dessa fall bör du utforma jobbet delningslisten toodetect hello arbetet som redan har utförts och inte upprepa den, till exempel genom att utföra en CloudJob.ListTasks innan du startar tooyield uppgifter.

### <a name="exit-codes-and-exceptions-in-hello-job-manager-template"></a>Avsluta koder och undantag i hello Job Manager-mall
Slutkoder och undantag tillhandahåller en mekanism toodetermine hello resultatet av att köra ett program och de kan hjälpa tooidentify eventuella problem med hello hello program ska köras. hello Job Manager mallen implementerar hello slutkoder och undantag som beskrivs i det här avsnittet.

En projektaktivitet manager som har implementerats med hello Job Manager mall kan returnera tre möjliga slutkoder:

| Kod | Beskrivning |
| --- | --- |
| 0 |Hej jobbhanteraren har slutförts. Jobbet delningslisten koden körde toocompletion och alla aktiviteter har lagts till toohello jobb. |
| 1 |hello projektaktivitet manager misslyckades med ett undantag i en '' del av programmet hello. hello undantaget var översatta tooa JobManagerException med diagnostisk information och, om möjligt hello förslag för att lösa felet. |
| 2 |hello projektaktivitet manager misslyckades med ett 'oväntat-undantag. hello undantaget var loggade toostandard utdata, men hello jobbhanteraren kunde tooadd eventuell ytterligare information för diagnostik eller reparation. |

Hello gäller jobbet manager uppgift misslyckades, kan vissa aktiviteter fortfarande har lagts toohello service innan hello-fel uppstod. Dessa aktiviteter kommer att köras som vanligt. Se ”jobbet delningslisten misslyckades” ovan beskrivning av den här kodsökvägen.

Alla hello information som returnerades av undantag skrivs i stdout.txt och stderr.txt-filer. Mer information finns i [felhantering](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Överväganden för klienten
Det här avsnittet beskrivs vissa implementering klientkrav när anropar en jobbhanteraren baserat på den här mallen. Se [hur toopass parametrar och miljövariabler från hello klientkod](#pass-environment-settings) information om skicka parametrar och inställningar.

**Obligatoriska autentiseringsuppgifter**

I ordning tooadd uppgifter toohello Azure Batch-jobbet kräver hello projektaktivitet manager din URL: en för Azure Batch-konto och nyckel. Du måste skicka dessa i miljövariabler som heter YOUR_BATCH_URL och YOUR_BATCH_KEY. Du kan ange dessa i hello Job Manager aktivitetsinställningar i miljön. Till exempel i C# klient:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Autentiseringsuppgifter för lagring**

Normalt behöver hello klienten inte tooprovide hello länkade storage-konto autentiseringsuppgifter toohello manager projektaktivitet eftersom (a) de flesta jobbet cheferna behöver inte tooexplicitly åtkomst hello länkade storage-konto och (b) hello länkade storage-konto används ofta tooall aktiviteter som en gemensam miljö för hello jobb. Om du inte tillhandahåller hello länkade storage-konto via hello vanliga miljöinställningar och hello jobbhanteraren kräver åtkomst toolinked lagring och sedan du ska ange autentiseringsuppgifter för hello länkad lagring på följande sätt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Jobbet manager aktivitetsinställningar**

hello klienten bör ange hello jobbhanteraren *killJobOnCompletion* flaggan för**FALSKT**.

Det är oftast säkert för hello klienten tooset *runExclusive* för**FALSKT**.

hello klienten bör använda hello *resourceFiles* eller *applicationPackageReferences* samling toohave hello jobbhanteraren körbara (och dess nödvändiga DLL-filer) distribueras toohello compute-nod.

Som standard kommer hello jobbhanteraren inte göras om det misslyckas. Beroende på dina jobb manager logik hello klienter kanske tooenable återförsök via *begränsningar*/*maxTaskRetryCount*.

**Inställningar för**

Om hello jobbet delningslisten avger åtgärder med beroenden, ange hello klienten hello jobbet usesTaskDependencies tootrue.

Det är ovanligt för klienter toowish tooadd uppgifter toojobs utöver vilka hello jobbet delningslisten skapar i hello jobbet delningslisten modellen. hello klienten bör normalt därför ange hello jobbet *onAllTasksComplete* för**terminatejob**.

## <a name="task-processor-template"></a>Uppgiftsmallar för Processor
En uppgift Processor-mall kan du tooimplement en aktivitet processor som kan utföra följande åtgärder hello:

* Ställ in hello information som krävs för varje Batch uppgiften toorun.
* Kör alla åtgärder som krävs för varje Batch-aktivitet.
* Spara aktiviteten utdata toopersistent lagring.

Även om en aktivitet processorn inte är nödvändiga toorun uppgifter på Batch är hello stor fördel med att använda en aktivitet processor att det ger en wrapper tooimplement alla-körningen åtgärder på en plats. Till exempel om du behöver toorun flera program i hello kontexten för varje aktivitet, eller om du behöver toocopy toopersistent datalagring när du har slutfört varje uppgift.

hello-åtgärder som utförs av hello uppgiften processor kan vara som enkla eller komplexa, och många eller få som krävs av din arbetsbelastning. Dessutom genom att implementera alla-åtgärder i en aktivitet processor, kan du lätt uppdatera eller lägga till åtgärder baserat på ändringar tooapplications eller arbetsbelastning krav. Men i vissa fall kanske en uppgift processor inte hello optimal lösning för din implementering som den kan lägga till onödiga komplexitet, till exempel när du kör jobb som går snabbt att starta från en enkel kommandorad.

### <a name="create-a-task-processor-using-hello-template"></a>Skapa en uppgift-Processor med hello mall
tooadd en aktivitet processor toohello lösning som du skapade tidigare, så här:

1. Öppna din befintliga lösning i Visual Studio.
2. I Solution Explorer, högerklicka på hello lösning, klickar du på **Lägg till**, och klicka sedan på **nytt projekt**.
3. Under **Visual C#**, klickar du på **moln**, och klicka sedan på **Azure Batch uppgiften Processor**.
4. Ange ett namn som beskriver ditt program och identifierar det här projektet som hello uppgiften processor (t.ex.) ”LitwareTaskProcessor”).
5. toocreate hello projektet klickar du på **OK**.
6. Slutligen build hello projektet tooforce Visual Studio tooload alla refererar till NuGet-paket och tooverify som hello projektet är giltigt innan du börjar att ändra den.

### <a name="task-processor-template-files-and-their-purpose"></a>Uppgiften Processor mallfilerna och deras syfte
När du skapar ett projekt med hello uppgiften processor mallen genererar tre grupper av kodfiler:

* hello huvudsakliga programfilen (Program.cs). Innehåller hello startpunkt för programmet och översta undantagshantering. Du bör inte normalt behöver toomodify detta.
* hello Framework-katalogen. Innehåller hello-filer som är ansvarig för hello-formaterad' arbete som utförs av hello job manager-program – uppackning parametrar, lägga till uppgifter toohello batchjobb osv. Du bör inte normalt måste toomodify dessa filer.
* hello uppgiften processor-fil (TaskProcessor.cs). Detta är där kommer att placeras din programspecifika logik för att köra en aktivitet (vanligtvis genom att anropa ut tooan befintliga executable). Före och efterbearbetning kod, till exempel ladda ned ytterligare data eller överför resultat filer även här.

Naturligtvis du kan lägga till ytterligare filer som krävs toosupport aktivitet processor koden, beroende på hello komplexitet hello jobbet dela logik.

hello mallen skapas också standard .NET projektfiler, till exempel en .csproj filen app.config, Packages.config-fil, osv.

hello resten av det här avsnittet beskrivs hello andra filer och deras kodstruktur och förklaras vad varje klass gör.

![Visual Studio Solution Explorer visar hello uppgiften Processor mallen lösning][solution_explorer02]

**Framework-filer**

* `Configuration.cs`: Kapslar in hello inläsning av jobb konfigurationsdata som Batch-kontoinformation, länkade lagringskontouppgifter, projekt- och information och jobbparametrar. Det ger också tillgång tooBatch definierats miljövariabler (se miljöinställningar för aktiviteter i hello Batch dokumentationen) via hello Configuration.EnvironmentVariable klass.
* `IConfiguration.cs`: Sammanfattning Hej implementering av hello Configuration klass, så att du kan testa enheten ditt jobb delningslisten med hjälp av en förfalskad eller fingerad konfigurationsobjektet.
* `TaskProcessorException.cs`: Representerar ett fel som kräver hello job manager tooterminate. TaskProcessorException är används toowrap 'förväntades' fel där specifika diagnostisk information kan anges som en del av avslutning.

**Uppgiften-Processor**

* `TaskProcessor.cs`: Kör hello aktiviteten. hello framework anropar hello TaskProcessor.Run metod. Detta är hello klassen där du matar in hello programspecifika logiken för uppgiften. Implementera hello Kör metod för att:
  * Analysera och kontrollera eventuella parametrar för aktiviteten
  * Skapa hello kommandoraden för alla externa program du vill tooinvoke
  * Logga alla diagnostisk information som du kan behöva för felsökning
  * Starta en process som kommandoraden
  * Vänta tills hello processen tooexit
  * Avbilda hello avslutningskoden hello processen toodetermine om den har lyckats eller misslyckats
  * Spara alla utdatafiler som du vill tookeep toopersistent lagring

**Standard projektfiler för .NET-kommandorad**

* `App.config`: Standard .NET-programkonfigurationsfilen.
* `Packages.config`: Standard NuGet-paketet beroende fil.
* `Program.cs`: Innehåller hello startpunkt för programmet och översta undantagshantering.

## <a name="implementing-hello-task-processor"></a>Implementera hello uppgiften processor
När du öppnar hello uppgiften Processor mallprojekt har hello projektet hello TaskProcessor.cs filen öppnas som standard. Du kan implementera hello kör logik för hello aktiviteter i din arbetsbelastning med metoden hello Run() visas nedan:

```csharp
/// <summary>
/// Runs hello task processing logic. This is where you inject
/// your application-specific logic for decomposing hello job into tasks.
///
/// hello task processor framework invokes hello Run method for you; you need
/// only tooimplement it, not toocall it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check hello exit code of that program and
/// save output files toopersistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for hello task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> hello är kommenterade avsnitt i hello Run()-metoden hello endast avsnitt i hello uppgiften Processor mall-koden som är avsedd för du toomodify genom att lägga till hello kör logik för hello aktiviteter i din arbetsbelastning. Om du vill toomodify ett annat avsnitt i hello mall måste du först bekanta dig med hur Batch fungerar genom att granska hello Batch dokumentation och testar några av hello Batch-kodexempel.
> 
> 

hello Run() metoden ansvarar för att starta hello kommandoraden, starta en eller flera processer, väntar på att alla processen toocomplete, spara hello resultat och slutligen returneras med slutkoden. hello Run()-metoden är där du implementera hello bearbetning av logik för aktiviteter. hello uppgiften processor framework anropar hello Run()-metoden. Du behöver inte toocall den själv.

Implementeringen Run() har åtkomst till:

* Hej uppgiftsparametrar via hello `_parameters` fältet.
* Hej projekt- och ID: n via hello `_jobId` och `_taskId` fält.
* Hej uppgiftskonfigurationen via hello `_configuration` fältet.

**Aktivitet, fel**

Du kan avsluta hello Run()-metoden genom att ett undantag om fel uppstår, men detta lämnar hello översta nivå undantagshanterare kontrollen över hello sluttid för aktiviteten. Om du behöver toocontrol hello slutkoden så att du kan skilja mellan olika typer av fel, till exempel för att ställa diagnoser eller eftersom vissa feltillstånd bör avsluta hello jobb och andra bör inte sedan ska du avsluta hello Run() metoden genom att returnera en slutkoden noll. Detta blir hello sluttid för aktiviteten.

### <a name="exit-codes-and-exceptions-in-hello-task-processor-template"></a>Avsluta koder och undantag i hello uppgiften Processor mall
Slutkoder och undantag tillhandahåller en mekanism toodetermine hello resultatet av att köra ett program och de kan hjälpa dig identifiera eventuella problem med hello hello program ska köras. hello uppgiften Processor mallen implementerar hello slutkoder och undantag som beskrivs i det här avsnittet.

En uppgift processor uppgift som implementeras med hello uppgiften Processor mall kan returnera tre möjliga slutkoder:

| Kod | Beskrivning |
| --- | --- |
| [Process.ExitCode][process_exitcode] |hello uppgiften processor körde toocompletion. Observera att detta inte innebär programmet hello du anropade lyckades – endast hello uppgiftsprocessorn anropa den och utföra eventuell efter bearbetning utan undantag. hello innebörden av hello slutkoden beror på programmet hello anropas – normalt slutkoden 0 betyder hello program har slutförts och andra slutkoden hello programmet misslyckades. |
| 1 |hello uppgiften processor misslyckades med ett undantag i en '' del av programmet hello. hello undantaget var översatta tooa `TaskProcessorException` med diagnostisk information och, om möjligt hello förslag för att lösa felet. |
| 2 |hello uppgiften processor misslyckades med ett 'oväntat-undantag. hello undantaget var loggade toostandard utdata, men hello uppgiften processor var tooadd eventuell ytterligare information för diagnostik eller reparation. |

> [!NOTE]
> Om hello du anropa används avsluta koder 1 och 2 tooindicate specifika felet lägen, är sedan använda slutkoder 1 och 2 för aktiviteten processor fel tvetydig. Du kan ändra dessa uppgiften processor koder toodistinctive avsluta felkoder genom att redigera hello undantag fall i hello Program.cs-filen.
> 
> 

Alla hello information som returnerades av undantag skrivs i stdout.txt och stderr.txt-filer. Mer information finns i felhantering, i hello Batch-dokumentationen.

### <a name="client-considerations"></a>Överväganden för klienten
**Autentiseringsuppgifter för lagring**

Om aktiviteten processorn använder Azure blob storage toopersist utdata, till exempel använda hello filen konventioner hjälpbibliotek, så den behöver åtkomst för*antingen* hello molnet lagringskontouppgifter *eller* en blob-behållaren URL som innehåller en signatur för delad åtkomst (SAS). hello mallen innehåller stöd för att tillhandahålla autentiseringsuppgifter via vanliga miljövariabler. Klienten kan skicka hello lagring autentiseringsuppgifter på följande sätt:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

hello storage-konto är sedan tillgängliga i hello TaskProcessor klassen via hello `_configuration.StorageAccount` egenskapen.

Om du vill toouse behållar-URL med SAS kan du kan också skicka detta via en gemensam miljöinställning jobb, men hello uppgiften processor mallen innehåller för närvarande inte inbyggt stöd för detta.

**Lagringsinställningar**

Du rekommenderas hello klient- eller manager uppgiften skapa behållare som krävs av uppgifter innan du lägger till hello uppgifter toohello jobb. Detta är obligatoriskt om du använder en behållar-URL med SAS som en URL inte innehåller behörighet toocreate hello behållare. Det rekommenderas även om du skickar lagringskontouppgifter, som sparas varje aktivitet med toocall CloudBlobContainer.CreateIfNotExistsAsync om hello behållare.

## <a name="pass-parameters-and-environment-variables"></a>Skicka parametrar och miljövariabler
### <a name="pass-environment-settings"></a>Skicka miljöinställningar
En klient kan skicka information toohello projektaktivitet manager i hello form av inställningarna för miljön. Den här informationen kan sedan användas av hello projektaktivitet manager när genererar hello aktivitet processor aktiviteter som körs som en del av hello compute jobb. Exempel på hello information som du kan överföra som miljöinställningarna är:

* Namn och lagringskontonycklar
* URL: en för batch-konto
* Batch-kontonyckel

hello Batch-tjänsten har en enkel mekanism toopass miljö inställningar tooa manager projektaktivitet genom att använda hello `EnvironmentSettings` egenskap i [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Till exempel tooget hello `BatchClient` instans för Batch-kontot du skickar som miljövariabler från hello klient code hello URL och delade nyckel autentiseringsuppgifter för hello Batch-kontot. På samma sätt tooaccess hello storage-konto som är länkade toohello Batch-kontot, kan du överföra hello lagringskontonamn och hello lagringskontonyckel som miljövariabler.

### <a name="pass-parameters-toohello-job-manager-template"></a>Skicka parametrar toohello Job Manager-mall
I många fall är det användbart toopass per projekt parametrar toohello manager projektaktivitet, antingen toocontrol hello jobbet dela process eller tooconfigure hello uppgifter för hello jobb. Du kan göra detta genom att ladda upp en JSON-fil med namnet parameters.json som en resurs för hello projektaktivitet manager. hello parametrar sedan kan bli tillgängliga i hello `JobSplitter._parameters` i hello Job Manager-mall.

> [!NOTE]
> hello inbyggda parametern hanteraren stöder endast string-string ordlistor. Om du vill toopass komplexa JSON-värden som parametervärden du kommer behöver toopass dem som strängar och analysera dem i hello jobbet delningslisten eller ändra hello framework `Configuration.GetJobParameters` metod.
> 
> 

### <a name="pass-parameters-toohello-task-processor-template"></a>Skicka parametrar toohello aktivitet Processor mall
Du kan också skicka parametrar tooindividual uppgifter implementeras med hjälp av hello uppgiften Processor mallen. Precis som med hello job manager-mall söker hello uppgiften processor mallen efter en resursfil med namnet

parameters.JSON och om att hitta den läser in den som hello parametrar ordlistan. Det finns ett par olika alternativ för hur toopass parametrar toohello uppgifter för processorn:

* Återanvända hello jobbparametrar JSON. Detta fungerar bra om hello endast parametrar är jobbet hela (till exempel en render höjd och bredd). toodo, när du skapar en CloudTask i hello jobbet delningslisten kan lägga till ett toohello parameters.json resurs filen referensobjekt från hello job manager aktivitetens ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) toohello CloudTask ResourceFiles samling.
* Generera och överför en uppgiftsspecifika parameters.json dokument som en del av delningslisten jobbkörningen och referera till blobben i hello uppgiften resource files samlingen. Detta är nödvändigt om olika aktiviteter har olika parametrar. Ett exempel kan vara ett 3D-rendering scenario där hello ram index skickas toohello aktiviteten som en parameter.

> [!NOTE]
> hello inbyggda parametern hanteraren stöder endast string-string ordlistor. Om du vill toopass komplexa JSON-värden som parametervärden du kommer behöver toopass dem som strängar och analysera dem i hello uppgiften processor eller ändra hello framework `Configuration.GetTaskParameters` metod.
> 
> 

## <a name="next-steps"></a>Nästa steg
### <a name="persist-job-and-task-output-tooazure-storage"></a>Spara jobbet och uppgift utdata tooAzure lagring
Ett annat bra verktyg i utvecklingen av Batch-lösningar är [Azure Batch filen konventioner][nuget_package]. Använd denna .NET-klassbiblioteket (för närvarande under förhandsgranskning) i din Batch .NET program tooeasily store och hämta uppgift utdata tooand från Azure Storage. [Spara Azure Batch-jobb- och utdata](batch-task-output.md) innehåller en fullständig beskrivning av hello bibliotek och dess användning.

### <a name="batch-forum"></a>Batch-Forum
Hej [Azure Batch-Forum] [ forum] är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN. HEAD på över för användbara ”Fäst” inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
