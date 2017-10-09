---
title: aaaAutomatically skala compute-noder i en Azure Batch-pool | Microsoft Docs
description: "Aktivera automatisk skalning på ett moln poolen toodynamically justera hello antalet compute-noder i hello pool."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6d1e0c5d8e0e56e15a4d3588150f2466a689f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Skapa en automatisk skalning formel för att skala beräkningsnoder i en Batch-pool

Azure Batch skala automatiskt poolerna baserat på parametrarna som du definierar. Automatisk skalning Batch lägger till dynamiskt noder tooa poolen som aktiviteten krav och tar bort compute-noder som de minska. Du kan spara både tid och pengar genom att automatiskt justera hello antalet datornoderna används av Batch-program. 

Du aktiverar automatisk skalning på en pool av datornoderna genom att associera med en *Autoskala formeln* som du definierar. hello Batch-tjänsten använder hello Autoskala formeln toodetermine hello antal compute-noder som är nödvändiga tooexecute din arbetsbelastning. Compute-noder kan vara dedikerade noder eller [låg prioritet noder](batch-low-pri-vms.md). Batch svarar tooservice mått data som samlas in regelbundet. Med den här mätvärdesdata kan justeras Batch hello antalet beräkningsnoder i hello poolen baserat på din formel och en konfigurerbar intervall.

Du kan aktivera automatisk skalning när poolen har skapats, eller på en befintlig adresspool. Du kan även ändra en befintlig formel på en pool som har konfigurerats för autoskalning. Batch kan tooevaluate formlerna innan du tilldelar dem toopools och toomonitor hello status autoskalning körs.

Den här artikeln beskrivs hello olika enheter som utgör ditt Autoskala formler, inklusive variabler, operatorer, funktioner och funktioner. Diskuterar hur tooobtain olika compute resurs- och mått i Batch. Du kan använda dessa mått tooadjust antalet för din pool-noder baserat på resursen användnings- och status. Sedan beskrivs hur tooconstruct en formel och aktivera automatisk skalning på en pool med hjälp av både hello Batch REST och .NET-API: er. Slutligen vi med några formler.

> [!IMPORTANT]
> När du skapar ett Batch-konto kan du ange hello [kontokonfigurationen](batch-api-basics.md#account), som bestämmer om pooler tilldelas i en prenumeration på tjänsten Batch (hello standard) eller i din prenumeration för användaren. Om du har skapat Batch-kontot med hello standardkonfiguration för Batch-tjänsten är begränsad tooa högsta antalet kärnor som kan användas för bearbetning med ditt konto. hello Batch-tjänsten skalas datornoderna bara in toothat core gränsen. Därför nå hello Batch-tjänsten inte hello mål antalet compute-noder som anges av en Autoskala formel. Se [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) information för att visa och öka kvoter för ditt konto.
>
>Om du har skapat ditt konto med hello användarens prenumeration konfigurationen delar ditt konto i hello kärnkvot för hello prenumeration. Mer information finns i [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Gränser för virtuella datorer) i [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Prenumerations- och tjänstgränser, kvoter och begränsningar i Azure).
>
>

## <a name="automatic-scaling-formulas"></a>Automatisk skalning formler
En automatisk skalning formeln är ett strängvärde som du definierar som innehåller en eller flera instruktioner. hello Autoskala formeln tilldelas tooa pool [autoScaleFormula] [ rest_autoscaleformula] element (Batch REST) eller [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] Egenskapen (Batch .NET). hello Batch-tjänsten använder formeln toodetermine hello mål antal compute-noder i hello pool för hello nästa intervall för bearbetning. hello formeln strängen får inte överskrida 8 KB, kan innehålla upp too100 instruktioner som avgränsas med semikolon och kan innehålla radbrytningar och kommentarer.

Du kan se automatisk skalning formler som en Batch Autoskala ”språk”. Formeln satser är kostnadsfri utformad uttryck som kan innehålla både tjänsten användardefinierade variabler (variabler som definieras av hello Batch-tjänsten) och användardefinierade variabler (variabler som du definierar). De kan utföra olika åtgärder på dessa värden med hjälp av inbyggda typer, operatorer och funktioner. En instruktion kan till exempel ta hello följande format:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formler innehåller vanligtvis flera instruktioner som utför åtgärder på värden som hämtas i tidigare rapporter. Till exempel först vi hämta ett värde för `variable1`, överför den tooa funktionen toopopulate `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Med de här uttrycken i din Autoskala formeln tooarrive på ett mål som antal compute-noder. Dedikerad noder och låg prioritet noder har sina egna Målinställningar så att du kan definiera ett mål för varje nod. En Autoskala formel kan innehålla ett målvärde för dedikerade noder, ett målvärde för låg prioritet noder eller båda.

hello mål antalet noder kan vara högre eller lägre eller hello lika hello aktuellt antal noder av typen i hello poolen. Batch utvärderar en pool Autoskala formel med ett specifikt intervall (se [autoskalning intervall](#automatic-scaling-interval)). Batch justerar hello målnumret för varje typ av nod i hello poolen toohello tal som Autoskala formeln anger vid hello tiden för utvärderingen.

### <a name="sample-autoscale-formula"></a>Autoskala formeln

Här är ett exempel på en formel för Autoskala som kan vara justerade toowork för de flesta scenarier. Hej variabler `startingNumberOfVMs` och `maxNumberofVMs` i hello exempel formeln kan vara justerade tooyour behov. Den här formeln skalas dedikerade noder, men kan vara ändrade tooapply tooscale låg prioritet noder. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Med den här formeln Autoskala skapas hello poolen med en enda virtuell dator. Hej `$PendingTasks` mått definierar hello antalet aktiviteter som körs eller står i kö. hello formeln hittar hello Genomsnittligt antal väntande aktiviteter i hello senaste 180 sekunder och anger hello `$TargetDedicatedNodes` variabeln därefter. hello formeln säkerställer att hello mål antalet dedicerade noder aldrig överstiger 25 virtuella datorer. När nya aktiviteter skickas växer hello poolen automatiskt. Som slutförda uppgifter, virtuella datorer blir ledigt i taget och hello autoskalning formeln krymper hello poolen.

## <a name="variables"></a>Variabler
Du kan använda båda **tjänst definierade** och **användardefinierade** variabler i Autoskala formlerna. hello tjänst definierade variabler är inbyggda i toohello Batch-tjänsten. Några variabler som definieras av tjänsten är skrivskyddad och vissa är skrivskyddad. Användardefinierade variabler är variabler som du definierar. I hello Exempelformel visas i föregående avsnitt hello `$TargetDedicatedNodes` och `$PendingTasks` är variabler som definieras av tjänsten. Variabler `startingNumberOfVMs` och `maxNumberofVMs` är användardefinierade variabler.

> [!NOTE]
> Tjänst-definierade variabler föregås alltid av ett dollartecken ($). För användardefinierade variabler är hello dollartecken valfritt.
>
>

hello följande tabeller visar både läs-och skrivbara och skrivskyddade variabler som definieras av hello Batch-tjänsten.

Du kan hämta och ange hello värdena för dessa variabler som definieras av tjänsten toomanage hello antalet compute-noder i en pool:

| Läsa / skriva-tjänst-definierade variabler | Beskrivning |
| --- | --- |
| $TargetDedicatedNodes |hello mål antal dedikerade compute-noder för hello poolen. hello antalet dedicerade noder har angetts som mål eftersom en pool inte kan alltid får hello önskad antalet noder. Till exempel hello mål antalet dedicerade noder har ändrats av en Autoskala utvärdering innan hello poolen har nått hello inledande mål och hello poolen kan inte nå hello mål. <br /><br /> Poolen på ett konto som har skapats med hello Batch-tjänsten konfiguration kan inte uppnå målet om hello mål är längre än en nod eller core batchkonton. Poolen på ett konto som har skapats med hello användarens prenumeration konfiguration kan inte uppnå målet om hello mål överskrider hello delade kärnkvot för hello prenumeration.|
| $TargetLowPriorityNodes |hello mål antalet låg prioritet compute-noder för hello poolen. hello antal låg prioritet noder har angetts som mål eftersom en pool inte kan alltid får hello önskad antalet noder. Till exempel hello mål antalet noder med låg prioritet har ändrats av en Autoskala utvärdering innan hello poolen har nått hello inledande mål och hello poolen kan inte nå hello mål. En pool kan också inte uppnå målet om hello mål är längre än en nod eller core batchkonton. <br /><br /> Mer information på låg prioritet datornoder finns [använda VM med låg prioritet med Batch (förhandsgranskning)](batch-low-pri-vms.md). |
| $NodeDeallocationOption |hello vad som händer när datornoderna tas bort från poolen. Möjliga värden:<ul><li>**meddelanden**--avslutar aktiviteter omedelbart och placerar dem på hello jobbkön så att de schemaläggs.<li>**Avsluta**--avslutar aktiviteter omedelbart och tar bort dem från hello jobbkön.<li>**taskcompletion**--väntar för pågående aktiviteter toofinish och sedan tar bort hello noden från hello poolen.<li>**retaineddata**--väntar på alla hello lokala uppgift behålls data på hello nod toobe rensas innan du tar bort hello noden från hello poolen.</ul> |

Du kan få hello värdet för dessa variabler som definieras av tjänsten toomake justeringar som baseras på mått för hello Batch-tjänsten:

| Skrivskyddade tjänst definierade variabler | Beskrivning |
| --- | --- |
| $CPUPercent |hello genomsnittliga procentandelen av CPU-användning. |
| $WallClockSeconds |hello antal sekunder som används. |
| $MemoryBytes |hello Genomsnittligt antal megabyte som används. |
| $DiskBytes |hello Genomsnittligt antal gigabyte används på hello lokala diskar. |
| $DiskReadBytes |hello antal lästa byte. |
| $DiskWriteBytes |hello antalet skrivna byte. |
| $DiskReadOps |hello antal lästa diskåtgärder utföras. |
| $DiskWriteOps |hello antal skrivåtgärder på disken utförs. |
| $NetworkInBytes |hello antal inkommande byte. |
| $NetworkOutBytes |hello antal utgående byte. |
| $SampleNodeCount |hello antal compute-noder. |
| $ActiveTasks |hello antalet aktiviteter som är redo tooexecute men ännu inte körs. Hej $ActiveTasks antal omfattar alla aktiviteter som är aktiv för hello och vars förutsättningar är uppfyllda. Alla aktiviteter som finns i aktivt läge för hello men vars beroenden är inte uppfyllda undantas från hello $ActiveTasks count.|
| $RunningTasks |hello antal aktiviteter körs. |
| $PendingTasks |hello summan av $ActiveTasks och $RunningTasks. |
| $SucceededTasks |hello antalet uppgifter som har slutförts. |
| $FailedTasks |hello antalet uppgifter som misslyckades. |
| $CurrentDedicatedNodes |hello aktuellt antal dedikerade compute-noder. |
| $CurrentLowPriorityNodes |hello aktuellt antal låg prioritet compute-noder, inklusive alla noder som är försenat. |
| $PreemptedNodeCount | hello antalet noder i hello poolen som är i tillståndet försenat. |

> [!TIP]
> hello skrivskyddad, tjänst-definierade variabler som visas i hello föregående tabell är *objekt* som ger olika metoder tooaccess data som är associerade med varje. Mer information finns i [Hämta exempeldata](#getsampledata) senare i den här artikeln.
>
>

## <a name="types"></a>Typer
Dessa typer stöds i en formel:

* dubbla
* doubleVec
* doubleVecList
* Sträng
* tidsstämpeln--tidsstämpel är en sammansatt struktur som innehåller hello följande medlemmar:

  * År
  * månaden (1-12)
  * dag (1-31)
  * veckodag (hello formatet för tal, till exempel 1 för måndag)
  * timme (i 24-timmarsformat format, till exempel 13 innebär 1 PM)
  * minuter (00-59)
  * andra (00-59)
* TimeInterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Åtgärder
Dessa åtgärder är tillåtna på hello-typer som anges i hello föregående avsnitt.

| Åtgärd | Stöds operatorer | Typ av resultat |
| --- | --- | --- |
| dubbla *operatorn* dubbla |+, -, *, / |dubbla |
| dubbla *operatorn* timeinterval |* |TimeInterval |
| doubleVec *operatorn* dubbla |+, -, *, / |doubleVec |
| doubleVec *operatorn* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *operatorn* dubbla |*, / |TimeInterval |
| TimeInterval *operatorn* timeinterval |+, - |TimeInterval |
| TimeInterval *operatorn* tidsstämpel |+ |tidsstämpel |
| tidsstämpel *operatorn* timeinterval |+ |tidsstämpel |
| tidsstämpel *operatorn* tidsstämpel |- |TimeInterval |
| *operatorn*dubbla |-, ! |dubbla |
| *operatorn*timeinterval |- |TimeInterval |
| dubbla *operatorn* dubbla |<, <=, ==, >=, >, != |dubbla |
| strängen *operatorn* sträng |<, <=, ==, >=, >, != |dubbla |
| tidsstämpel *operatorn* tidsstämpel |<, <=, ==, >=, >, != |dubbla |
| TimeInterval *operatorn* timeinterval |<, <=, ==, >=, >, != |dubbla |
| dubbla *operatorn* dubbla |&&, &#124;&#124; |dubbla |

När du testar ett double med en ternär operator (`double ? statement1 : statement2`) inte är noll är **SANT**, och noll är **FALSKT**.

## <a name="functions"></a>Funktioner
De här fördefinierade **funktioner** är tillgängliga för toouse för att definiera en automatisk skalning formel.

| Funktionen | Returtyp | Beskrivning |
| --- | --- | --- |
| AVG(doubleVecList) |dubbla |Returnerar hello medelvärdet för alla värden i hello doubleVecList. |
| Len(doubleVecList) |dubbla |Returnerar hello hello-vektorn som skapas från hello doubleVecList längd. |
| LG(Double) |dubbla |Returnerar hello loggen bas 2 av hello dubbla. |
| LG(doubleVecList) |doubleVec |Returnerar hello component-wise loggen bas 2 av hello doubleVecList. En vec(double) måste uttryckligen skickades för hello-parametern. Annars antas hello dubbla lg(double) version. |
| LN(Double) |dubbla |Returnerar hello naturlig log hello dubbla. |
| LN(doubleVecList) |doubleVec |Returnerar hello component-wise loggen bas 2 av hello doubleVecList. En vec(double) måste uttryckligen skickades för hello-parametern. Annars antas hello dubbla lg(double) version. |
| log(Double) |dubbla |Returnerar hello loggen bas 10 av hello dubbla. |
| log(doubleVecList) |doubleVec |Returnerar hello component-wise loggen bas 10 av hello doubleVecList. En vec(double) måste uttryckligen skickades för hello enda dubbla parameter. Annars antas hello dubbla log(double) version. |
| Max(doubleVecList) |dubbla |Returnerar hello maxvärdet i hello doubleVecList. |
| min(doubleVecList) |dubbla |Returnerar hello minimivärdet i hello doubleVecList. |
| norm(doubleVecList) |dubbla |Returnerar hello två normen av hello-vektorn som skapas från hello doubleVecList. |
| percentil (doubleVec v dubbla p) |dubbla |Returnerar hello percentil element av hello vector v. |
| SLUMP() |dubbla |Returnerar ett slumpmässigt värde mellan 0,0 och 1,0. |
| Range(doubleVecList) |dubbla |Returnerar hello skillnaden mellan hello lägsta och högsta värden i hello doubleVecList. |
| Std(doubleVecList) |dubbla |Returnerar hello exempel standardavvikelsen för hello värden i hello doubleVecList. |
| Stop) | |Stoppar utvärderingen av uttrycket för hello autoskalning. |
| SUM(doubleVecList) |dubbla |Returnerar hello summan av alla hello komponenter i hello doubleVecList. |
| tid (string dateTime = ””) |tidsstämpel |Returnerar hello tidsstämpeln för hello aktuell tid om inga parametrar skickas eller hello tidsstämpeln för hello dateTime-sträng om den skickas. Stöds dateTime-format är W3C DTF och RFC 1123. |
| val (doubleVec v dubbla i) |dubbla |Returnerar värdet hello hello-element som är på plats i i vector v, med startIndex noll. |

Vissa av hello-funktioner som beskrivs i föregående tabell för hello kan acceptera en lista som ett argument. hello kommaavgränsad lista är en kombination av *dubbla* och *doubleVec*. Exempel:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Hej *doubleVecList* värdet är konverterade tooa enda *doubleVec* för utvärdering. Till exempel om `v = [1,2,3]`, sedan anropa `avg(v)` är likvärdiga toocalling `avg(1,2,3)`. Anropar `avg(v, 7)` är likvärdiga toocalling `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Hämta exempeldata
Autoskala formler fungerar på mätvärdesdata (exempel) som tillhandahålls av hello Batch-tjänsten. En formel förstoras eller förminskas poolstorlek utifrån hello värden som hämtas från hello-tjänsten. hello tjänst definierade variabler som beskrivs är tidigare objekt som ger olika metoder tooaccess data som är associerade med objektet. Hello visar följande uttryck exempelvis en begäran tooget hello senaste fem minuterna av CPU-användning:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Metod | Beskrivning |
| --- | --- |
| GetSample() |Hej `GetSample()` metoden returnerar en vector av insamlade data.<br/><br/>Ett exempel är 30 sekunder värt mått data. Med andra ord hämtas exempel var 30: e sekund. Men som anges nedan, det finns en fördröjning mellan när ett exempel som samlas in och när den är tillgänglig tooa formel. Därför kanske inte alla prover för en viss tidsperiod för utvärdering av en formel.<ul><li>`doubleVec GetSample(double count)`<br/>Anger hello antal prover tooobtain hello senaste prover som samlades in.<br/><br/>`GetSample(1)`Returnerar hello senaste tillgängliga exempel. För mått som `$CPUPercent`, men detta bör inte användas eftersom det är omöjligt tooknow *när* hello exempel samlades in. Det kan vara senaste eller på grund av problem med systemet, kan det vara mycket äldre. Det är bättre i sådana fall toouse ett tidsintervall som visas nedan.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Anger en tid för att samla in exempeldata. Du kan också anger den också hello procentandelen exempel som måste vara tillgängliga i hello begärt tidsintervall.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Returnerar 20 prov om alla prover för hello senaste 10 minuterna finns i hello CPUPercent historik. Om hello senaste minuten tidigare inte var tillgänglig, men skulle endast 18 exempel returneras. I det här fallet:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`misslyckas eftersom endast 90 procent av hello prover är tillgängliga.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`fungerar.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Anger en tid för att samla in data med både en starttid och en sluttid.<br/><br/>Som nämnts ovan är det en fördröjning mellan när ett exempel som samlas in och när den är tillgänglig tooa formel. Tänk fördröjningen när du använder hello `GetSample` metod. Se `GetSamplePercent` nedan. |
| GetSamplePeriod() |Returnerar hello antal prover som har tagits i historiska data exempeldata. |
| Count() |Returnerar hello Totalt antal prover i hello mått historik. |
| HistoryBeginTime() |Returnerar hello tidsstämpeln för hello äldsta tillgängliga datasampel för hello mått. |
| GetSamplePercent() |Returnerar hello procentandelen prover som är tillgängliga för ett visst tidsintervall. Exempel:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Eftersom hello `GetSample` metoden misslyckas om hello procentandelen prover som returnerades är mindre än hello `samplePercent` har angetts, kan du använda hello `GetSamplePercent` metoden toocheck första. Du kan sedan utföra åtgärden alternativ om det finns inte tillräckligt prover, utan att avbryta hello automatisk skalning utvärdering. |

### <a name="samples-sample-percentage-and-hello-getsample-method"></a>Exempel, exempel procent och hello *GetSample()* metod
hello grundläggande drift ett Autoskala formeln är tooobtain aktivitets- och mätvärden och justera poolstorlek baserat på dessa data. Det är därför viktigt toohave förstår hur Autoskala formler interagera med mätvärdesdata (exempel).

**Exempel**

hello Batch-tjänsten med jämna mellanrum tar prover av aktivitets- och mått och gör dem tillgängliga tooyour Autoskala formler. Exemplen är registrerade med 30 sekunders mellanrum av hello Batch-tjänsten. Det finns dock vanligtvis en fördröjning mellan när de exempel som har registrerats och när de blir tillgängliga för (och kan läsas av) Autoskala formler. På grund av toovarious faktorer, till exempel nätverks- eller andra problem med infrastruktur, kan dessutom prover inte registreras för ett visst intervall.

**Exempel procent**

När `samplePercent` skickas toohello `GetSample()` metod eller hello `GetSamplePercent()` metoden anropas _procent_ refererar tooa jämförelse mellan hello totala antalet möjliga exempel som är registrerade av hello Batch-tjänsten och hello antal prover som är tillgängliga tooyour Autoskala formel.

Nu ska vi titta på timespan 10 minuters som exempel. Eftersom prover har registrerats med 30 sekunders mellanrum inom timespan 10 minuters, är hello maximalt antal prover som registreras av Batch 20 prov (2 per minut). På grund av toohello inbyggd svarstiden för hello reporting mekanism och andra problem i Azure, kan det dock endast 15 prover som är tillgängliga tooyour Autoskala formeln för läsning. Så, exempelvis för den 10-minutersperioden endast 75% av hello totala antalet prov som registreras kanske tillgängliga tooyour formel.

**Intervall för GetSample() och exempel**

Autoskala formlerna är pågående toobe växande och minska storleken på din pooler &mdash; att lägga till noder eller ta bort noder. Eftersom noder kostar pengar, vill du tooensure med formler ett intelligent sätt att analys som baseras på uppgifter. Därför rekommenderar vi att du använder en trender typen analys i formler. Den här typen växer och krymper din poolerna baserat på ett intervall av insamlade prover.

toodo så Använd `GetSample(interval look-back start, interval look-back end)` tooreturn en vector prover:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

När hello över linjen utvärderas av Batch returnerar en uppsättning exempel som en vektor med värden. Exempel:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

När du har lagrat hello vector prover, kan du använda funktioner som `min()`, `max()`, och `avg()` tooderive meningsfulla värden från hello samlas in intervallet.

För ytterligare säkerhet kan du tvinga en formel utvärdering toofail om mindre än en viss procentandel av exemplet är tillgänglig för en viss tidsperiod. Om du framtvingar en formel utvärdering toofail instruera du Batch toocease ytterligare utvärdering av hello formeln om hello angivna procentandelen av prover inte är tillgänglig. Ingen ändring görs i det här fallet toohello poolstorlek. toospecify krävs procent av prover för hello utvärdering toosucceed, ange den som hello tredje parametern för`GetSample()`. Här är har ett krav för 75 procent av prover angetts:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Eftersom det kan finnas en fördröjning i exempel tillgänglighet, är det viktigt tooalways ange ett tidsintervall med föregående titt starttid som är äldre än en minut. Det tar ungefär en minut för prover toopropagate via hello system så exempel i hello intervallet `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` kanske inte tillgänglig. Igen och du kan använda hello procentandel parametern för `GetSample()` tooforce ett visst exempel procentandel krav.

> [!IMPORTANT]
> Vi **rekommenderar** som du **förlita dig inte *endast* på `GetSample(1)` i Autoskala formlerna**. Detta beror på att `GetSample(1)` i stort sett står toohello Batch-tjänsten ”ge mig hello sista exemplet som du har, oavsett hur länge sedan den hämtades”. Eftersom det är bara ett exempel på en enda, och det kan vara ett äldre prov, kanske inte representativ för hello större bild av aktivitet eller resource tillstånd. Om du använder `GetSample(1)`och kontrollera att den är del av en större instruktion inte hello endast datapunkt som är beroende av din formel.
>
>

## <a name="metrics"></a>Mått
Du kan använda både resurs- och mått när du definierar en formel. Du kan justera hello mål antalet dedicerade noder i hello poolen baserat på hello mått data som du vill hämta och utvärdera. Se hello [variabler](#variables) avsnittet ovan för mer information om varje mått.

<table>
  <tr>
    <th>Mått</th>
    <th>Beskrivning</th>
  </tr>
  <tr>
    <td><b>Resurs</b></td>
    <td><p>Resursen mått baseras på hello CPU, hello bandbredd, hello minnesanvändningen för datornoder och hello antalet noder.</p>
        <p> Dessa definieras av tjänsten variabler är användbara för att göra justeringar baserat på antalet noder:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Dessa definieras av tjänsten variabler är användbara för att göra justeringar baserat på noden Resursanvändning:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Aktivitet</b></td>
    <td><p>Uppgiften mått baserat på hello status för aktiviteter, till exempel aktiv, väntar, och slutförs. hello följande tjänst definierade variabler är användbara för att göra poolstorleken justeringar baserat på uppgiften mått:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Skriv en Autoskala formel
Du skapa en formel Autoskala genom instruktioner som använder hello ovan komponenter och sedan kombinera dessa instruktioner till en fullständig formel. I det här avsnittet skapar vi ett exempel Autoskala formeln som kan utföra vissa verkliga skalning beslut.

Först ska vi definiera hello krav för vår nya Autoskala formel. hello formeln bör:

1. Öka hello mål antalet dedicerade beräkningsnoder i poolen om CPU-användningen är hög.
2. Minska hello mål antal dedikerade beräkningsnoder i en pool när CPU-användningen är låg.
3. Begränsa alltid hello maximalt antal noder dedikerade too400.

tooincrease hello antalet noder under hög CPU-användning, definiera hello-uttryck som fyller på en användardefinierad variabel (`$totalDedicatedNodes`) med ett värde som är 110 procent av hello aktuella mål antal dedikerade noder, men endast om hello minsta Genomsnittlig CPU-användning under hello var senaste 10 minuterna ovan 70 procent. Annars Använd hello värdet för hello aktuellt antal dedikerade noder.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

för*minska* Hej antalet dedicerade noder under låg CPU-användning, hello nästa instruktionen i vår formeln anger hello samma `$totalDedicatedNodes` variabeln too90 procent av hello aktuellt mål antal dedikerade noder om hello Genomsnittlig CPU-användning Hej var senaste 60 minuter under 20 procent. Annars använder hello aktuellt värde för `$totalDedicatedNodes` som vi har fyllts i hello instruktionen ovan.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Nu begränsa hello mål antal dedikerade compute-noder tooa högst 400:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Här är hello hela formeln:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>Skapa en pool med Autoskala-aktiverade med .NET

toocreate en pool med autoskalning aktiverat i .NET, Följ dessa steg:

1. Skapa hello-pool med [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Ange hello [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) egenskapen för`true`.
3. Ange hello [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) egenskap med Autoskala-formel.
4. (Valfritt) Ange hello [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) egenskapen (standardvärdet är 15 minuter).
5. Genomför hello-pool med [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) eller [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

hello skapar nedanstående kodutdrag en pool med Autoskala-aktiverade i .NET. hello anger poolens Autoskala formeln hello mål antal dedikerade noder too5 på måndagen och 1 för varje dag i veckan hello. Hej [intervall för automatisk skalning](#automatic-scaling-interval) är ange too30 minuter. I det här och andra C#-kodavsnitt i den här artikeln hello `myBatchClient` är en korrekt initierad instans av hello [BatchClient] [ net_batchclient] klass.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> När du skapar en Autoskala-aktiverade pool kan inte ange hello _targetDedicatedComputeNodes_ parameter eller hello _targetLowPriorityComputeNodes_ parameter på hello anropa för **CreatePool**. Ange i stället hello **AutoScaleEnabled** och **AutoScaleFormula** egenskaper i hello poolen. hello värden för dessa egenskaper avgör hello målnumret för varje typ av noden. Dessutom toomanually ändra storlek på en Autoskala-aktiverade pool (med exempelvis [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), första **inaktivera** automatisk skalning på Hej poolen och sedan ändrar den.
>
>

Dessutom tooBatch .NET, du kan använda någon av hello andra [Batch SDK](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md), och hello [Batch CLI](batch-cli-get-started.md)tooconfigure autoskalning.


### <a name="automatic-scaling-interval"></a>Intervall för automatisk skalning
Som standard justeras hello Batch-tjänsten en poolstorlek enligt tooits Autoskala formeln var 15: e minut. Intervallet kan konfigureras med hjälp av hello följande egenskaper för programpoolen:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST-API)

hello minsta intervall är fem minuter och högsta hello är 168 timmar. Om ett intervall som är utanför intervallet har angetts returnerar hello Batch-tjänsten en felaktig begäran (400) fel.

> [!NOTE]
> Autoskalning är inte avsedd för närvarande toorespond toochanges på mindre än en minut, men är snarare avsedd tooadjust hello storleken på din pool gradvis när du kör en arbetsbelastning.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Aktivera autoskalning på en befintlig adresspool

Varje Batch-SDK tillhandahåller en sätt tooenable autoskalning. Exempel:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)
* [Aktivera automatisk skalning på poolen] [ rest_enableautoscale] (REST-API)

När du aktiverar autoskalning på en befintlig adresspool Kom ihåg hello följande punkter:

* Om automatisk skalning är inaktiverat på hello pool när du skickar hello begäran tooenable autoskalning, måste du ange en giltig Autoskala formel när du skickar hello-begäran. Du kan också ange ett intervall för utvärdering av Autoskala. Om du inte anger ett intervall används hello standardvärdet 15 minuter.
* Om Autoskala är aktiverat i hello pool, kan du ange en Autoskala formel, ett intervall för utvärdering eller båda. Du måste ange minst en av dessa egenskaper.

  * Om du anger ett nytt Autoskala utvärdering intervall hello befintliga utvärderingsschema stoppas och startas ett nytt schema. Starttid för hello nytt schema är hello tid vid vilken hello begäran tooenable autoskalning har utfärdats.
  * Om du utelämnar antingen hello Autoskala formeln eller utvärdering intervall fortsätter hello Batch-tjänsten toouse hello aktuellt värde för den här inställningen.

> [!NOTE]
> Om du har angett värden för hello *targetDedicatedComputeNodes* eller *targetLowPriorityComputeNodes* parametrarna för hello **CreatePool** metoden när du skapade hello pool i .NET eller för hello jämförbara parametrar i ett annat språk och sedan dessa värden ignoreras när hello autoskalning formeln utvärderas.
>
>

Den här C#-kodstycke använder hello [Batch .NET] [ net_api] biblioteket tooenable autoskalning på en befintlig adresspool:

```csharp
// Define hello autoscaling formula. This formula sets hello target number of nodes
// too5 on Mondays, and 1 on every other day of hello week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set hello autoscale formula on hello existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Uppdatera en Autoskala formel

tooupdate hello formeln på en befintlig Autoskala-aktiverade adresspool, anrop hello åtgärden tooenable autoskalning igen med hello ny formel. Om exempelvis autoskalning är redan aktiverat på `myexistingpool` när hello efter .NET-kod körs dess Autoskala formel ersätts med hello innehållet i `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-hello-autoscale-interval"></a>Hello Autoskala uppdateringsintervall

tooupdate hello Autoskala intervallet utvärdering av en befintlig Autoskala-aktiverade pool, anrop hello åtgärden tooenable autoskalning igen med hello nytt intervall. Till exempel tooset hello Autoskala utvärdering too60 minuter för en pool som redan är aktiverat Autoskala i .NET:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Utvärdera en Autoskala formel

Du kan utvärdera en formel innan den tillämpas tooa pool. På så sätt kan testa du hello formeln toosee hur dess rapporter utvärdera innan du publicerar hello formeln till produktionen.

tooevaluate en Autoskala formel, måste du först aktivera autoskalning på hello-pool med en giltig formel. tootest en formel i en pool som ännu inte har autoskalning aktiverad, Använd hello enradiga formel `$TargetDedicatedNodes = 0` när du först aktivera autoskalning. Använd sedan någon av följande tooevaluate hello formel som du vill använda tootest hello:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) eller [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Metoderna Batch .NET kräver hello-ID för en befintlig adresspool och en sträng som innehåller hello Autoskala formeln tooevaluate.

* [Utvärdera en automatisk skalning formel](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    Ange hello programpools-ID i hello URI i denna REST API-begäran och hello Autoskala formeln i hello *autoScaleFormula* element av hello frågans brödtext. hello svaret hello åtgärdens innehåller någon information om fel som kan vara relaterade toohello formel.

I det här [Batch .NET] [ net_api] kodfragmentet vi utvärderar en Autoskala formel. Om hello poolen inte har aktiverat autoskalning det att aktivera den först.

```csharp
// First obtain a reference tooan existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on hello pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula tooenable autoscaling on the
    // pool. This formula is valid, but won't resize hello pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls tooonce every 30 seconds.
    // Because we want tooapply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here tooensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh hello properties of hello pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on hello pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // hello formula tooevaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform hello autoscale formula evaluation. Note that this code does not
    // actually apply hello formula toohello pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print hello results of hello AutoScaleRun.
        // This will display hello values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply hello formula toohello pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output hello message associated with hello error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Lyckad utvärdering av hello formeln i det här kodstycket ger resultat som liknar:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Hämta information om Autoskala körs

tooensure formeln fungerar som förväntat, rekommenderar vi att du regelbundet kontrollerar hello resultaten av hello autoskalning körs som Batch utför på din pool. toodo så få (eller uppdatera) en referens toohello poolen och granska den senaste Autoskala kör hello egenskaper.

I Batch .NET hello [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) -egenskap har flera egenskaper som innehåller information om hello senaste autoskalning kör utförs på hello pool:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

I hello REST-API, hello [få information om poolen](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) begäran returnerar information om hello poolen, vilket innefattar hello senaste autoskalning körs information i hello [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) egenskapen.

hello följande kodfragment för C# används hello Batch .NET-biblioteket tooprint information om hello senaste autoskalning körs på poolen _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Utdata från föregående kodfragment hello:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Autoskala formler
Nu ska vi titta på några formler som visar olika sätt tooadjust hello mängden beräkningsresurser i poolen.

### <a name="example-1-time-based-adjustment"></a>Exempel 1: Tidsbaserade justering
Anta att du vill tooadjust hello poolstorlek baserat på hello hello veckodag och tid på dagen. Det här exemplet visar hur tooincrease eller minska hello antalet noder i hello poolen därefter.

hello formeln hämtar först hello aktuell tid. Om det är en vardag (1-5) och inom arbetstid (too6 8: 00 PM), hello poolen Målstorlek anges too20 noder. I annat fall har den värdet too10 noder.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Exempel 2: Uppgiftsbaserade justering
I det här exemplet justeras hello poolstorlek utifrån hello antalet aktiviteter i hello kö. Både kommentarer och radbrytningar accepteras i formeln strängar.

```csharp
// Get pending tasks for hello past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use hello last sample point,
// otherwise we use hello maximum of last sample point and hello history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM toopending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// hello pool size is capped at 20, if target VM value is more than that, set it
// too20. This value should be adjusted according tooyour use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Exempel 3: Redovisning för parallella aktiviteter
Det här exemplet justerar hello poolstorlek baserat på hello antal uppgifter. Den här formeln beaktas även konto hello [MaxTasksPerComputeNode] [ net_maxtasks] värdet som har angetts för hello poolen. Den här metoden är användbar i situationer där [parallell körning av aktiviteten](batch-parallel-node-tasks.md) har aktiverats på din pool.

```csharp
// Determine whether 70 percent of hello samples have been recorded in hello past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set hello number of nodes tooadd tooone-fourth hello number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set too4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt toogrow hello number of compute nodes toomatch hello number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep hello nodes active until hello tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Exempel 4: Ange ett inledande poolstorlek
Det här exemplet visar en C# code fragment med en Autoskala formeln som anger hello poolen storlek tooa ange antalet noder för en inledande tidsperiod. Sedan justeras den hello poolstorlek utifrån hello antalet körs och aktiva uppgifter efter hello inledande tidsperiod har gått ut.

hello formeln i hello följande kodutdrag:

* Anger hello inledande adresspool storlek toofour noder.
* Justeras inte hello poolstorlek inom hello 10 minuter i hello poolen livscykeln.
* Efter 10 minuter hämtar hello maxvärdet för hello antal körs och aktiva aktiviteter inom hello senaste 60 minuter.
  * Om båda värdena är 0 (som anger att inga aktiviteter körs eller är aktiv i hello senaste 60 minuter), anges hello poolstorlek too0.
  * Ingen ändring görs om antingen värdet är större än noll.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Nästa steg
* [Maximera Azure Batch beräkning Resursanvändning med samtidiga nod uppgifter](batch-parallel-node-tasks.md) innehåller information om hur du kan köra flera aktiviteter samtidigt i hello beräkningsnoder i din pool. Dessutom tooautoscaling, den här funktionen kan hjälpa toolower jobbet varaktighet för vissa arbetsbelastningar, spara pengar.
* Se till att dina frågor för Batch-programmet hello Batch-tjänsten i hello de flesta optimalt sätt för en annan effektivitet booster. Se [fråga hello Azure Batch-tjänsten effektivt](batch-efficient-list-queries.md) toolearn hur toolimit hello mängden data som passerar hello överföring när du frågar hello status för tusentals compute-noder eller uppgifter.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
