---
title: "ett jobb förloppet genom att räkna uppgifter efter status - Azure Batch aaaMonitor | Microsoft Docs"
description: "Övervaka hello ett jobb genom att anropa hello hämta uppgift räknar åtgärden toocount uppgifter för ett jobb. Du kan hämta ett antal aktiva, körs och slutförda uppgifter och aktiviteter som har lyckades eller misslyckades."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: tamram
ms.openlocfilehash: 03957d8a3d678bf44587f3bc7f988a76885c2af0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="count-tasks-by-state-toomonitor-a-jobs-progress-preview"></a>Antal aktiviteter genom att tillståndet toomonitor jobbförloppet (förhandsgranskning)

Azure Batch tillhandahåller ett effektivt sätt toomonitor hello förloppet för ett jobb som körs tillhörande uppgifter. Du kan anropa hello [hämta uppgift räknar] [ rest_get_task_counts] åtgärden toofind ut hur många aktiviteter är i tillståndet active, körs, eller slutfördes och hur många har har lyckats eller misslyckats. Genom att räkna hello antalet aktiviteter i varje du enklare visa hello jobbets förlopp tooa användaren eller oväntade fördröjningar eller fel som kan påverka hello jobb identifieras.

> [!IMPORTANT]
> hello hämta uppgift räknar åtgärden är för närvarande i förhandsvisning och finns inte ännu i Azure Government, Azure Kina och Azure Tyskland. 
>
>

## <a name="how-tasks-are-counted"></a>Hur åtgärder ska räknas

hello hämta uppgift räknar åtgärden räknar uppgifter efter tillstånd, enligt följande:

- En uppgift räknas som **active** när den är i kö och kan toorun, men inte är tilldelad tooa compute-nod. En uppgift också räknas som **active** om det är beroende av en överordnad aktivitet som inte har slutförts ännu. Mer information om aktiviteten beroenden finns [och skapa aktiviteten beroenden toorun uppgifter som är beroende av andra aktiviteter](batch-task-dependencies.md). 
- En uppgift räknas som **kör** när den har tilldelats tooa compute-nod, men har inte slutförts ännu. En uppgift räknas som **kör** när dess tillstånd är antingen `preparing` eller `running`, som anges av hello [hämta information om en aktivitet] [ rest_get_task] igen.
- En uppgift räknas som **slutförts** när den inte längre är berättigad toorun. En uppgift räknas som **slutförts** har vanligtvis antingen har slutförts, har slutförts utan fel eller också har uttömt gränsen för återförsök. 

hello hämta uppgift räknar åtgärden rapporterar också hur många aktiviteter har har lyckats eller misslyckats. Batch avgör om en aktivitet har lyckats eller misslyckats genom att kontrollera hello **resultatet** -egenskapen för hello [executionInfo] [https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo] egenskap:

    - En uppgift räknas som **lyckades** om hello resultatet för körning av aktiviteten är `success`.
    - En uppgift räknas som **misslyckades** om hello resultatet för körning av aktiviteten är `failure`.

Mer information om uppgiften tillstånd finns [hämta information om en aktivitet][rest_get_task].

hello visar följande .NET-kodexempel hur tooretrieve aktivitet räknar efter tillstånd: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> Du kan använda ett liknande mönster för REST och andra språk som stöds tooget aktivitet räknar för ett jobb. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Konsekvenskontrollen för aktiviteten antal

hello Batch-tjänsten aggregerar antal aktiviteten genom att samla in data från flera delar av en asynkron distribuerade system. tooensure som uppgift antal är korrekta Batch tillhandahåller ytterligare verifiering för tillstånd antal genom att utföra konsekvenskontroller mot flera komponenter av hello system. Batch utför dessa konsekvenskontroller så länge det finns färre än 200 000 uppgifter i hello-jobbet. Hello osannolika att hello konsekvenskontroll hittar fel, korrigerar Batch hello resultatet av hello hämta uppgifter räknar utifrån hello resultat av hello konsekvenskontroll. hello är konsekvenskontroll en extra mått tooensure som kunder som förlitar sig på hello hämta uppgift räknar åtgärden hämta hello rätt information de behöver för lösningen.

Hej **validationStatus** egenskapen hello svar anger om Batch har utfört hello konsekvenskontroll. Om inte Batch har kan toocheck tillstånd antal mot hello faktiska tillstånd lagras i hello system sedan hello **validationStatus** egenskapen för`unvalidated`. Av prestandaskäl Batch kommer inte att utföra hello konsekvenskontroll om hello jobbet innehåller mer än 200 000 uppgifter, så hello **validationStatus** egenskapen kan anges för`unvalidated` i det här fallet. Antal för hello aktivitet är dock inte nödvändigtvis fel i det här fallet även en mycket begränsad dataförlust är högst osannolikt. 

När status ändras i en aktivitet bearbetar hello aggregering pipeline hello ändra inom några sekunder. hello hämta uppgift räknar åtgärden visar hello uppdateras uppgiften antal inom denna period. Men om hello aggregering pipeline missar en ändring i en aktivitet tillstånd, sedan ändring inte är registrerad tills hello nästa validering pass. Uppgiften antal kan vara något fel på grund av toohello missade händelse under denna tid, men de korrigerade på hello nästa validering pass.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>Metodtips för inventering uppgifter för ett jobb

Anropar hello hämta uppgift räknar åtgärden är hello mest effektiva sättet tooreturn grundläggande antalet aktiviteter i ett jobb av tillstånd. Om du använder Batch-tjänstversion 2017-06-01.5.1, rekommenderar vi skrivning eller uppdatera din kod toouse hämta uppgift räknar.

hello hämta uppgift räknar åtgärden är inte tillgänglig i Batch-tjänsten-versioner tidigare än 2017-06-01.5.1. Om du använder en äldre version av hello-tjänsten sedan använda en lista över frågan toocount aktiviteter i ett jobb i stället. Mer information finns i [skapa frågor toolist Batch resurser effektivt](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Nästa steg

* Se hello [Batch funktionsöversikt](batch-api-basics.md) toolearn mer om Batch-koncept och funktioner. hello artikeln beskriver hello primära Batch resurser, till exempel pooler, compute-noder, jobb och uppgifter och ger en översikt över hello tjänstens funktioner.
* Hello grundläggande information om hur utvecklingen av ett Batch-aktiverade program som använder hello [Batch .NET-klientbibliotek](batch-dotnet-get-started.md) eller [Python](batch-python-tutorial.md). De här inledande artiklarna leder dig igenom ett fungerande program som använder hello Batch-tjänsten tooexecute en arbetsbelastning på flera compute-noder.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job
