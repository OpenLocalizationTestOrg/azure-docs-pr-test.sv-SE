---
title: "aaaDesign effektivt listan frågor - Azure Batch | Microsoft Docs"
description: "Öka prestandan genom att filtrera dina frågor när du begär information om Batch-resurser som pooler, jobb, uppgifter och beräkningsnoder."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7e554119ec9d0e9e8007ccfb1ca80fe142a5e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-queries-toolist-batch-resources-efficiently"></a>Skapa frågor toolist Batch resurser effektivt

Här lär du dig hur tooincrease Azure Batch-programmets prestanda genom att minska hello mängden data som returneras av hello-tjänsten när du frågar projekt, uppgifter och beräkningsnoder med hello [Batch .NET] [ api_net] bibliotek.

Nästan alla program i batchen måste tooperform vissa typer av övervakning eller andra åtgärder som frågar ofta hello Batch-tjänsten med jämna mellanrum. Till exempel toodetermine om det finns kvar i ett jobb köade aktiviteter, måste du hämta data för varje aktivitet i hello-jobbet. toodetermine hello status för noder i din pool, måste du hämta data på varje nod i hello pool. Den här artikeln förklarar hur tooexecute sådana frågor i hello mest effektiva sättet.

> [!NOTE]
> hello Batch-tjänsten innehåller särskilda API-stöd för hello vanligt scenario för inventering aktiviteter i ett projekt. Du kan anropa hello istället för att använda en fråga för dessa [hämta uppgift räknar] [ rest_get_task_counts] igen. Hämta uppgift antal anger hur många aktiviteter väntar, kör eller slutföra och hur många aktiviteter har lyckades eller misslyckades. Hämta uppgift räknar är effektivare än en fråga. Mer information finns i [antal uppgifter för ett jobb efter status (förhandsgranskning)](batch-get-task-counts.md). 
>
> hello hämta uppgift räknar åtgärden är inte tillgänglig i Batch-tjänsten-versioner tidigare än 2017-06-01.5.1. Om du använder en äldre version av hello-tjänsten sedan använda en lista över frågan toocount aktiviteter i ett jobb i stället.
>
> 

## <a name="meet-hello-detaillevel"></a>Uppfyller hello DetailLevel
I produktion Batch program numrera entiteter t.ex projekt, uppgifter och beräkningsnoder i hello tusentalsavgränsare. När du begär information om dessa resurser kan måste potentiellt stora mängder data ”över hello överföring” från hello Batch-tooyour tjänstprogrammet på varje fråga. Genom att begränsa hello antalet objekt och typ av information som returneras av en fråga kan du öka hello hastigheten på dina frågor och därför hello prestanda för ditt program.

Detta [Batch .NET] [ api_net] API kodfragment visar *varje* aktivitet som är associerad med ett jobb, tillsammans med *alla* hello egenskaper för varje uppgift:

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Du kan utföra ett mycket effektivare listfrågan dock genom att använda en ”detaljnivån” tooyour fråga. Du kan göra detta genom att tillhandahålla en [ODATADetailLevel] [ odata] objekt toohello [JobOperations.ListTasks] [ net_list_tasks] metod. Den här fragment returnerar endast hello-ID, kommandoraden och compute-nod information egenskaper för slutförda aktiviteter:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties tooreturn
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply hello ODATADetailLevel toohello ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

I det här exemplet, om det finns tusentals uppgifter i jobbet hello hello från hello andra frågan vanligtvis blir resultatet returneras mycket snabbare än hello först. Mer information om hur du använder ODATADetailLevel när listobjekt med hello Batch .NET API ingår [under](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Vi rekommenderar starkt att du *alltid* varor en ODATADetailLevel objektet tooyour .NET API lista anropar tooensure högsta effektivitet och prestanda för ditt program. Genom att ange en detaljnivå kan hjälpa du toolower Batch-tjänsten svarstider, förbättra belastningen på nätverket och minimera minnesanvändning av klientprogram.
> 
> 

## <a name="filter-select-and-expand"></a>Filtrera, markera och utöka
Hej [Batch .NET] [ api_net] och [Batch REST] [ api_rest] API: er ger hello möjlighet tooreduce båda hello antal objekt som returneras i en lista samt hello mängden information som returneras för varje. Det gör du genom att ange **filter**, **Välj**, och **Expandera strängar** när du utför listan frågor.

### <a name="filter"></a>Filter
hello filtersträngen är ett uttryck som minskar hello antal objekt som returneras. Till exempel listan endast hello pågående aktiviteter för ett jobb eller listan endast compute-noder som är redo toorun uppgifter.

* hello Filtersträngen består av en eller flera uttryck med ett uttryck som består av en egenskapsnamn, en operator och ett värde. hello egenskaper som kan anges är specifika tooeach enhetstyp som du fråga som är hello operatorer som stöds för varje egenskap.
* Du kan kombinera flera uttryck med hjälp av hello logiska operatorer `and` och `or`.
* Det här exemplet filtrera strängar endast hello kör ”återge” uppgifter: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Välj
hello väljer sträng begränsar hello egenskapsvärden som returneras för varje objekt. Du anger en lista över egenskapsnamn och endast de värdena som returneras för hello objekt i hello frågeresultat.

* hello väljer sträng består av en kommaavgränsad lista med egenskapsnamn. Du kan ange hello egenskaper för hello entitetstypen du frågar.
* Det här exemplet väljer sträng som anger att endast tre värden ska returneras för varje aktivitet: `id, state, stateTransitionTime`.

### <a name="expand"></a>Visa
hello Expandera sträng minskar hello antalet API-anrop som är nödvändiga tooobtain viss information. När du använder en sträng visa mer information om varje objekt kan erhållas med ett enda API-anrop. I stället för första erhålla hello-listan för entiteter och begär information för varje objekt i listan hello du använder en Expandera sträng tooobtain hello samma information i ett enda API-anrop. Mindre API-anrop innebär bättre prestanda.

* Liknande toohello väljer sträng, hello Expandera sträng kontroller om vissa data tas med i listan frågeresultat.
* hello Expandera sträng stöds endast när den används i visar en lista över jobb, jobbscheman, uppgifter och pooler. Den stöder för närvarande bara statistik.
* När alla egenskaper som krävs och ingen väljer sträng anges, hello expanderar sträng *måste* vara används tooget statistikinformation. Om en väljer sträng är används tooobtain en delmängd av egenskaper, sedan `stats` kan anges i hello väljer sträng och hello Expandera sträng behöver inte toobe som angetts.
* Det här exemplet Expandera sträng Anger att statistik ska returneras för varje objekt i listan hello: `stats`.

> [!NOTE]
> När någon av hello tre fråga strängtyper (filtrera, markera och expandera), måste du se till att hello egenskapsnamn och fallet matcha motsvarigheterna REST API-element. Till exempel när du arbetar med hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) klass, måste du ange **tillstånd** i stället för **tillstånd**, även om hello egenskap för .NET är [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Finns hello tabellerna nedan för egenskapsmappningar mellan hello .NET eller REST API: er.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Regler för filter, markera och utöka strängar
* Egenskaper för namnen i filtret, markera och utöka strängar som ska visas som i hello [Batch REST] [ api_rest] API – även när du använder [Batch .NET] [ api_net] eller en av hello andra Batch-SDK.
* Alla egenskapsnamn är skiftlägeskänsliga, men egenskapsvärden är skiftlägeskänsligt.
* Tidsvärdet strängar kan vara något av två format och måste föregås av `DateTime`.
  
  * W3C-DTF format exempel:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * RFC 1123 format exempel:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Booleskt strängar är antingen `true` eller `false`.
* Om en ogiltig egenskap eller en operator har angetts en `400 (Bad Request)` fel returneras.

## <a name="efficient-querying-in-batch-net"></a>Effektiva frågor skickas i Batch .NET
Inom hello [Batch .NET] [ api_net] API, hello [ODATADetailLevel] [ odata] klassen används för att tillhandahålla filter och välj utöka strängar toolist åtgärder. Hej ODataDetailLevel klass har tre offentliga strängegenskaper som kan anges i hello konstruktorn eller ange direkt på hello-objektet. Du sedan skicka hello ODataDetailLevel-objektet som en parameter toohello olika Liståtgärder som [ListPools][net_list_pools], [ListJobs][net_list_jobs], och [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: begränsa hello antal objekt som returneras.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: Ange vilka egenskapsvärden som returneras med varje objekt.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: hämta data för alla objekt i ett enda API-anrop i stället för anrop för varje objekt.

hello använder följande kodavsnitt hello Batch .NET API tooefficiently frågan hello Batch-tjänsten för hello statistik för en specifik uppsättning pooler. I det här scenariot har hello Batch användaren både test- och pooler. hello test pool-ID: N föregås ”test” och ”produktprenumeration” föregås hello produktionspool-ID: N. Hello kodutdrag *myBatchClient* är en korrekt initierad instans av hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) klass.

```csharp
// First we need an ODATADetailLevel instance on which tooset hello filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want toopull only hello "test" pools, so we limit hello number of items returned
// by using a FilterClause and specifying that hello pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// toofurther limit hello data that crosses hello wire, configure hello SelectClause to
// limit hello properties that are returned on each CloudPool object tooonly
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify hello ExpandClause so that hello .NET API pulls hello statistics for the
// CloudPools in a single underlying REST API call. Note that we use hello pool's
// REST API element name "stats" here as opposed too"Statistics" as it appears in
// hello .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing hello amount of data that is returned
// by specifying hello detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> En instans av [ODATADetailLevel] [ odata] som har konfigurerats med utvalda och expandera-satser kan också skickas tooappropriate Get-metoder, exempelvis [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello mängden data som returneras.
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a>Batch REST API för too.NET mappningar
Egenskapsnamn i filtret, markera och expandera strängar *måste* återspeglar motsvarigheterna REST API både namn och fallet. hello tabeller nedan innehåller mappningar mellan hello .NET och REST API-motsvarigheter.

### <a name="mappings-for-filter-strings"></a>Mappningar för filtersträngar
* **.NET-metoder för listan**: hello .NET API-metoderna i den här kolumnen accepterar ett [ODATADetailLevel] [ odata] objekt som parameter.
* **Begäranden om REST**: varje REST API-sidan länkade tooin som den här kolumnen innehåller en tabell som anger hello egenskaper och åtgärder som tillåts i *filter* strängar. Du använder dessa egenskapsnamn och åtgärder när du skapar en [ODATADetailLevel.FilterClause] [ odata_filter] sträng.

| Metoder för .NET-lista | Begäranden för REST-lista |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Visa hello certifikat på ett konto][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Lista hello filer som hör till en aktivitet][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[Status för hello hello jobbförberedelseuppgiften och jobbet versionen uppgifter för ett jobb][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Lista hello jobb i ett konto][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Lista hello filer på en nod][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Lista hello uppgifter som är kopplad till ett jobb][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Lista hello jobbscheman på ett konto][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Lista hello jobb som är associerade med ett Jobbschema][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Lista hello compute-noder i en pool][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Lista hello pooler i ett konto][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Mappningar för väljer strängar
* **Batch-.NET-typer**: Batch .NET API-typer.
* **REST API-entiteter**: varje sida i den här kolumnen innehåller en eller flera tabeller som visar hello REST API-egenskapsnamn för hello-typen. Dessa egenskapsnamn som används när du skapar *Välj* strängar. Du ska använda samma egenskapsnamn när du skapar en [ODATADetailLevel.SelectClause] [ odata_select] sträng.

| Batch .NET-typer | REST API-enheter |
| --- | --- |
| [Certifikat][net_cert] |[Hämta information om ett certifikat][rest_get_cert] |
| [CloudJob][net_job] |[Hämta information om ett jobb][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Hämta information om ett Jobbschema][rest_get_schedule] |
| [ComputeNode][net_node] |[Hämta information om en nod][rest_get_node] |
| [CloudPool][net_pool] |[Hämta information om en pool][rest_get_pool] |
| [CloudTask][net_task] |[Hämta information om en aktivitet][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Exempel: skapa en Filtersträng
När du skapar en Filtersträng för [ODATADetailLevel.FilterClause][odata_filter], kontakta hello tabellen ovan under ”mappningar för filtersträngar” toofind hello REST API-dokumentationssida som motsvarar toohello lista åtgärden tooperform gärna. Du hittar hello filtrera egenskaper och deras stöds operatorer i hello första försök tabellen på sidan. Om du inte vill tooretrieve alla aktiviteter vars avslutskoden var inte är noll, till exempel detta rad på [listan hello uppgifter som är kopplad till ett jobb] [ rest_list_tasks] anger hello tillämpliga egenskapssträng och tillåtna operatorer:

| Egenskap | Tillåtna åtgärder | Typ |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Därför är hello Filtersträngen för att lista alla aktiviteter med slutkoden noll:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Exempel: skapa en väljer sträng
tooconstruct [ODATADetailLevel.SelectClause][odata_select], kontakta hello tabellen ovan under ”mappningar för väljer strängar” och navigera toohello REST API-sidan som motsvarar toohello typ av enhet som du är en lista med. Du hittar hello valbar egenskaper och deras stöds operatorer i hello första försök tabellen på sidan. Om du inte vill tooretrieve endast hello-ID och kommandoraden för varje aktivitet i en lista, t.ex, du hittar dessa rader i hello tillämpliga tabellen på [hämta information om en aktivitet][rest_get_task]:

| Egenskap | Typ | Anteckningar |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

hello väljer sträng för att inkludera endast hello-ID och kommandoraden med uppgifterna listan blir:

`id, commandLine`

## <a name="code-samples"></a>Kodexempel
### <a name="efficient-list-queries-code-sample"></a>Kodexempel för effektiv listan frågor
Kolla in hello [EfficientListQueries] [ efficient_query_sample] exempelprojektet på GitHub toosee hur effektivt listan frågor kan påverka prestanda i ett program. Den här C#-konsolprogram skapar och lägger till ett stort antal uppgifter tooa jobb. Sedan gör det flera anrop toohello [JobOperations.ListTasks] [ net_list_tasks] metoden och överför [ODATADetailLevel] [ odata] objekt som är konfigurerad med en annan egenskap värden toovary hello mängden data toobe returneras. Den genererar utdata liknande toohello följande:

```
Adding 5000 tasks toojob jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER tooquery tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER toocontinue...
```

Som visas i hello förfluten tid minskar du avsevärt svarstider för frågan genom att begränsa hello egenskaper och hello antal objekt som returneras. Du hittar det här och andra exempelprojekt i hello [azure-batch-samples] [ github_samples] databasen på GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics bibliotek och koden exempel
Dessutom toohello EfficientListQueries kodexemplet ovan, du kan hitta hello [BatchMetrics] [ batch_metrics] projekt i hello [azure-batch-samples] [ github_samples] GitHub-lagringsplatsen. Hej BatchMetrics exempelprojektet visar hur tooefficiently övervaka jobbförloppet i Azure Batch med hello Batch-API.

Hej [BatchMetrics] [ batch_metrics] exempel innehåller en .NET-klassbiblioteksprojektet som du kan införliva i dina projekt och ett enkelt kommandoradsverktyg programmet tooexercise och visar hello använder hello bibliotek.

hello exempelprogrammet i hello projekt visar hello följande åtgärder:

1. Att markera specifika attribut i ordning toodownload endast hello egenskaper behöver du
2. Filtrering på tillstånd övergången gånger i ordning toodownload endast ändringar sedan senaste hello-fråga

Exempelvis visas hello följande metod i hello BatchMetrics bibliotek. Returnerar en ODATADetailLevel som anger att endast hello `id` och `state` egenskaper som ska hämtas för hello entiteter som efterfrågas. Det även anger att endast enheter vars tillstånd har ändrats sedan hello anges `DateTime` parametern ska returneras.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Nästa steg
### <a name="parallel-node-tasks"></a>Parallel-nod uppgifter
[Maximera Azure Batch beräkning Resursanvändning med samtidiga nod uppgifter](batch-parallel-node-tasks.md) är en annan artikel relaterade tooBatch programprestanda. Vissa typer av arbetsbelastningar kan dra nytta av köra parallella aktiviteter på större-- men färre--compute-noder. Kolla in hello [Exempelscenario](batch-parallel-node-tasks.md#example-scenario) i hello artikeln för information om sådant scenario.

### <a name="batch-forum"></a>Batch-Forum
Hej [Azure Batch-Forum] [ forum] är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN. HEAD på över för användbara ”Fäst” inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job