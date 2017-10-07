---
title: "aaaAzure logganalys söka referens | Microsoft Docs"
description: "hello logganalys Sök referens beskriver hello sökspråk och hello allmänna syntax frågealternativ du kan använda när du söker efter data och filtrera uttryck toohelp begränsa din sökning."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7478a1139b88a1ce76ebb7b76027a6ccd66f4f27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-search-reference"></a>Logganalys söka referens

>[!NOTE]
> Den här artikeln beskriver loggen sökningar med hello aktuella frågespråket i logganalys.  Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), läser du bör för[hello Språkreferens för hello nytt språk](https://go.microsoft.com/fwlink/?linkid=856079).

hello beskrivs följande referens om sökningen language hello allmänna syntax frågealternativ du kan använda när du söker efter data och filtrera uttryck toohelp begränsa din sökning. Här beskrivs också kommandon som du kan använda tootake åtgärd på hello data som hämtats.

Du kan läsa om hello fält som returneras i sökningar och hello facets som hjälper dig identifiera mer om liknande kategorier av data i hello [sökfält och aspekten refererar till avsnittet](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Allmän frågesyntaxen
hello syntaxen för allmänna frågor är följande:

```
filterExpression | command1 | command2 …
```

Hej filteruttrycket (`filterExpression`) definierar hello ”där” villkor för hello frågan. hello kommandon gäller toohello resultat som returneras av hello frågan. Flera kommandon måste avgränsas med hello vertikalstreck (|).

### <a name="general-syntax-examples"></a>Allmän syntaxexemplen
Exempel:

```
system
```

Den här frågan returnerar resultat som innehåller hello word *system* i alla fält som har indexerats för fulltext eller villkor sökning.

> [!NOTE]
> Inte alla fält som indexeras sätt, men de vanligaste textrepresentation fält (till exempel beskrivningar och namn) vanligtvis hello finns.
>
>

```
system error
```

Den här frågan returnerar resultat som innehåller hello ord *system* och *fel*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Den här frågan returnerar resultat som innehåller hello ord *system* och *fel*. Den sorterar hello resultaten efter hello *ManagementGroupName* (i stigande ordning), och sedan efter hello *TimeGenerated* (i fallande ordning). Det tar bara hello första 10 resultat.

> [!IMPORTANT]
> Alla hello fältnamn och hello värden för hello sträng och texten är skiftlägeskänslig.
>
>

## <a name="filter-expressions"></a>Filtrera uttryck
hello följande underavsnitt beskrivs hello filteruttryck.

### <a name="string-literals"></a>Stränglitteraler
En stränglitteral är valfri sträng som inte kan identifieras av hello parsern som ett nyckelord eller en fördefinierad datatyp (till exempel ett tal eller datum).

Exempel:

```
These all are string literals
```

Den här frågan söker efter förekomster av alla fem ord. tooperform en komplex sträng sökning omge hello sträng inom citattecken. Exempel:

```
"Windows Server"
```

Detta returnerar endast resultat med exakt matchar för *Windows Server*.

### <a name="numbers"></a>Siffror
hello-parsern stöder hello heltal och flyttal nummer syntax för numeriska fält.

Exempel:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Datum och tid
Alla delar av informationen i hello system har en *TimeGenerated* -egenskap som representerar hello ursprungliga datum och tid för hello-post. Vissa typer av data kan ha ytterligare datum- och tidsfälten (till exempel *LastModified*).

Hej tidslinjen **diagram tidsvärdet** Väljaren i Azure Log Analytics visar en fördelning av resultaten över tid (bl.a toohello aktuella frågan körs). Detta baseras på hello *TimeGenerated* fältet. Datum och tid har en specifik sträng-format som kan användas i frågor toorestrict hello frågan tooa viss tidsram. Du kan också använda syntax toorefer toorelative tidsintervall (till exempel ”mellan tre dagar sedan och 2 timmar sedan”).

hello följande är giltiga typer av syntaxen för datum och tid:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Exempel:

```
TimeGenerated:2013-10-01T12:20
```

hello föregående kommando returnerar bara poster med en *TimeGenerated* värdet exakt 12:20 på 1 oktober 2013.

hello-parsern stöder också hello mnemoteknisk datum/tid-värde, nu. (Det är troligt att detta kommer att ge något resultat, eftersom data inte göra det via hello system som snabb.)

Exemplen är byggblocken toouse för relativa och absoluta datum. Hej i följande tre avsnitt ser du hur toouse dem i mer avancerade filter med exempel på relativa datumintervall.

### <a name="datetime-math"></a>Tidsvärdet math
Använd hello tidsvärdet matematiska operatorer toooffset eller avrunda hello datum/tid-värde med hjälp av enkla beräkningar för datum/tid.

Syntax:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operatorn | Beskrivning |
| --- | --- |
| / |Avrundar tidsvärdet toohello angetts enhet. Till exempel nu / dag Avrundar hello aktuella tidsvärdet toomidnight av hello aktuell dag. |
| + eller - |Förskjutningar tidsvärdet av hello anges antalet enheter. Till exempel nu + 1 timme förskjuts hello aktuellt datum/tid för en timme framåt. 2013-10-01T12:00-10 dagar förskjuts hello datumvärde tillbaka med 10 dagar. |

Du kan länka hello tidsvärdet matematiska operatorer tillsammans. Exempel:

```
NOW+1HOUR-10MONTHS/MINUTE
```

hello visar följande tabell hello stöds datum/tid-enheter.

| Datum/tid-enhet | Beskrivning |
| --- | --- |
| ÅR, ÅR |Ange antalet år Avrundar toocurrent år eller förskjutningarna av hello. |
| MÅNADEN, MÅNADER |Avrundar toocurrent månad eller förskjutningarna av hello angivna antalet månader. |
| DAGAR, DATUM |Avrundar toocurrent dagen i månaden hello eller förskjutningarna av hello angivet antal dagar. |
| TIMME, TIMMAR |Ange antalet timmar Avrundar toocurrent timme eller förskjutningarna av hello. |
| MINUT, MINUTER |Ange antalet minuter Avrundar toocurrent minut eller förskjutningarna av hello. |
| ANDRA, I SEKUNDER |Avrundar toocurrent andra eller förskjuts med hello angivna antalet sekunder. |
| MILLISEKUND MILLISEKUNDER MILLIEKVIVALENTER, MILLIS |Avrundar toocurrent millisekunder eller förskjutningarna av hello anges i millisekunder. |

### <a name="field-facets"></a>Fältet facets
Du kan ange hello sökvillkor för specifika fält och deras exakta värden genom att använda fältet facets. Detta skiljer sig från att skriva ”fritext” frågor för olika villkor i hela hello index. Du har redan sett den här tekniken i flera föregående exempel. hello följande är mer komplexa exempel.

**Syntax**

```
field:value
```

```
field=value
```

**Beskrivning**

Sökningar hello fält för hello specifikt värde. hello-värdet kan vara en stränglitteral, tal eller datum och tid.

Exempel:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntax**

*fältet > värde*

*fältet < värde*

*fältet > = värde*

*fältet < = värde*

*fältet! = värde*

**Beskrivning**

Innehåller jämförelser.

Exempel:

```
TimeGenerated>NOW+2HOURS
```

**Syntax**

```
field:[from..to]
```

**Beskrivning**

Innehåller intervallet faceting.

Exempel:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>I
Hej **IN** nyckelordet tillåter tooselect från en lista med värden. Beroende på hello-syntax som du använder, kan det vara en enkel lista med värden som du anger, eller en lista med värden från en aggregering.

Syntax 1:

```
field IN {value1,value2,value3,...}
```

Den här syntaxen kan du tooinclude alla värden i listan.



Exempel:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Syntax 2:

```
(Outer query) (Field toouse with inner query results) IN {Inner query | measure count() by (Field toosend tooouter query)} (rest  of outer query)  
```

Den här syntaxen kan toocreate en aggregering. Du kan sedan feed hello lista med värden från den sammansättning i en annan yttre (primära) sökning som söker efter händelser med dessa värde. Du gör detta genom omslutande hello inre Sök i klammerparenteser och mata resultaten som möjliga värden för ett fält i hello yttre Sök med hjälp av hello i-operatorn.

Inre frågan exempel: *datorer som för närvarande saknar säkerhetsuppdateringar* med hello följande aggregering fråga:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

sista hello-fråga som söker efter *alla Windows-händelser för datorer som för närvarande saknar säkerhetsuppdateringar* hello följande slag:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
Hej **innehåller** nyckelordet tillåter toofilter efter poster med ett fält som innehåller en angiven sträng. Detta är skiftlägeskänslig, fungerar bara med strängfält och får inte innehålla några escape-tecken.

Syntax:

```
field:contains("string")
```

Exempel:

```
Type:contains("Event")
```

Detta returnerar poster med en typ som innehåller hello sträng ”Event”. Exempel inkluderar **händelse**, **SecurityEvent**, och **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Reguljära uttryck
Du kan ange ett sökvillkor för ett fält med ett reguljärt uttryck med hjälp av hello **Regex** nyckelord. En fullständig beskrivning av hello-syntax som du kan använda i reguljära uttryck, se [med reguljära uttryck toofilter loggen sökningar i logganalys](log-analytics-log-searches-regex.md).

Syntax:

```
field:Regex("Regular Expression")
```

Exempel:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Logiska operatorer
hello frågan språk stöder hello logiska operatorer (*och*, *eller*, och *inte*) och deras alias C-format (*&&*,  *||* , och *!*respektive). Du kan använda parenteser toogroup operatorerna.

Exempel:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Du kan utelämna hello logisk operator för hello översta filter argument. I det här fallet antas hello och-operatorn.

| Filteruttrycket | Motsvarande för|
| --- | --- |
| systemfel |system och fel |
| System ”Windows Server” eller allvarlighetsgrad: 1 |system- och (”Windows Server” eller allvarlighetsgrad: 1) |

### <a name="wildcarding"></a>Jokertecken
hello-frågespråket stöder användning av hello ( \* ) tecken för representerar ett eller flera tecken för ett värde i en fråga.

Exempel:

 Hitta alla datorer med ”SQL” i hello namn, till exempel ”Redmond SQL”.

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> Jokertecken kan inte användas i offerterna för tillfället. Till exempel hello-meddelande `"*This text*"` anser hello (\*) används som en literal (\*) tecken.


## <a name="commands"></a>Kommandon


hello kommandon gäller toohello resultat som returneras av hello frågan. Använd hello pipe tecken (|) tooapply toohello en kommandot Hämta resultaten. Flera kommandon måste avgränsas med hello vertikalstrecket.

> [!NOTE]
> Kommandonamn kan skrivas i versaler eller gemener, till skillnad från hello fältnamn och hello data.
>
>

### <a name="sort"></a>Sortera
Syntax:

    sort field1 asc|desc, field2 asc|desc, …

Sorterar hello resultaten efter specifika fält. hello asc/desc suffix toosort hello resulterar i stigande eller fallande ordning är valfritt. Om det utelämnas hello *asc* sorteringsordning antas. För hello **TimeGenerated** fältet *desc* sorteringsordning antas så att den returnerar först hello senaste resultat som standard.

### <a name="toplimit"></a>Upp/gränsen
Syntax:

    top number


    limit number
Gränser hello svar toohello övre N resultat.

Exempel:

    Type:Alert errors detected | top 10

Returnerar hello topp 10 matchande resultat.

### <a name="skip"></a>Hoppa över
Syntax:

    skip number

Hoppar över hello antalet resultat som visas.

Exempel:

    Type:Alert errors detected | top 10 | skip 200

Returnerar de översta 10 matchande resultat från resultatet 200.

### <a name="select"></a>Välj
Syntax:

    select field1, field2, ...

Begränsar resultat toohello fält som du väljer.

Exempel:

    Type:Alert errors detected | select Name, Severity

Gränser hello returnerade resultat fält för*namn* och *allvarlighetsgrad*.

### <a name="measure"></a>Mått
Hej *mått* kommandot är används tooapply statistikfunktioner toohello raw sökresultat. Detta är mycket användbar tooget *Gruppera efter* vyer över hello data. När du använder hello mått kommandot innehåller logganalys sökresultatet en tabell med sammanlagda resultat.

**Syntax:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Sammanställer hello resultaten efter *grupperingsfält*, och beräknar hello samman måttvärden med hjälp av *aggregatedField*.

| Måttet statistisk funktion | Beskrivning |
| --- | --- |
| *aggregateFunction* |hello namnet på hello mängdfunktion (skiftlägesokänslig). hello följande mängdfunktioner stöds: antal, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, percentil ## eller PCT ## (## är ett tal mellan 1 och 99). |
| *aggregatedField* |hello fält som är att aggregeras. Det här fältet är valfritt för hello antal mängdfunktionen, men har toobe ett befintligt numeriska fält för SUMMAN, MAX, MIN, AVG, STDDEV, percentil ## eller PCT ## (## är ett tal mellan 1 och 99). Hej aggregatedField kan också vara någon av hello **utöka** funktioner som stöds. |
| *fieldAlias* |(valfritt) hello-alias för hello beräknade insamlat värde. Om inget anges hello fältnamnet är **AggregatedValue**. |
| *Grupperingsfält* |hello namnet på hello fält som hello resultatuppsättningen grupperas efter. |
| *Intervall* |hello tidsintervall i hello-format:**nnnNAME**. **nnn**är hello positivt heltal. **NAMNET** är hello intervall namn. Stöds intervall namn är skiftlägeskänsliga och innehålla: MILLISEKUNDER [S] sekund [S] minut [S] timme [S] dag [S] [S] för MÅNADEN och ÅRET [S]. |

hello intervall alternativet kan endast användas i fält för datum/tid-grupp (exempelvis *TimeGenerated* och *TimeCreated*). För närvarande är detta verkställs inte av hello service, men ett fält utan datum/tid som skickas toohello serverdel orsakar ett fel under körning. När hello schemavalidering implementeras avvisar hello service API som använder fält utan datum/tid för intervall aggregering. hello aktuella *mått* implementeringen stöder intervall gruppering för alla mängdfunktioner.

Om hello BY-satsen utelämnas, men ett intervall har angetts (som andra syntax), hello *TimeGenerated* fältet antas som standard.

Exempel:

**Exempel 1**

    Type:Alert | measure count() as Count by ObjectId

Grupper hello aviseringar efter *ObjectID*, och beräknar hello antalet aviseringar för varje grupp. hello aggregerade returneras värdet som hello *antal* fält (alias).

**Exempel 2**

    Type:Alert | measure count() interval 1HOUR

Grupper hello aviseringar per 1 timme med hjälp av hello *TimeGenerated* fältet och returnerar hello antal varningar i varje intervall.

**Exempel 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Samma som hello föregående exempel, men ett sammansatt fältalias (*AlertsPerHour*).

**Exempel 4**

    * | måttet count() av TimeCreated intervall 5DAYS

Grupperar hello resultaten efter 5 dagars intervall genom att använda hello *TimeCreated* fältet och returnerar hello antalet resultat i varje intervall.

**Exempel 5**

    Type:Alert | measure max(Severity) by WorkflowName

Grupper hello aviseringar efter arbetsbelastning namn och returnerar hello maximal allvarlighetsgrad värde för varje arbetsflöde.

**Exempel 6**

    Type:Alert | measure min(Severity) by WorkflowName

Samma som hello föregående exempel, men hello *min* samman funktion.

**Exempel 7**

    Type:Perf | measure avg(CounterValue) by Computer

Grupper Perf per dator, och beräknar medelvärdet hello (medel).

**Exempel 8**

    Type:Perf | measure sum(CounterValue) by Computer

Samma som hello föregående exempel, men använder *summan*.

**Exempel 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Samma som hello föregående exempel, men använder *stddev*.

**Exempel 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Samma som hello föregående exempel, men använder *percentile70*.

**Exempel 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Samma som hello föregående exempel, men använder *pct70*. Observera att *PCT ##* är endast ett alias för *percentil ##* funktion.

**Exempel 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Grupper Perf först per dator och sedan CounterName och beräknar medelvärdet hello (medel).

**Exempel 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Hämtar hello översta fem arbetsflöden med hello maximalt antal varningar.

**Exempel 14**

    * | måttet countdistinct(Computer) efter typ

Räknar hello antal unika datorer som rapporterar för varje typ av.

**Exempel 15**

    * | måttet countdistinct(Computer) intervall 1 timme

Räknar hello antal unika datorer som rapporterar varje timme.

**Exempel 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

Grupper processortid i procent av datorn, och returnerar hello medelvärdet för varje timme.

**Exempel 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

Grupper W3CIISLog av metoden, och returnerar hello maximala för var femte minut.

**Exempel 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

Grupper processortid i procent av datorn, och returnerar hello minsta, medel, 75: e percentilen och högsta gräns för varje timme.

**Exempel 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Grupper % processortid först efter dator och sedan efter instans namn och returnerar hello minimum, medel, 75: e percentilen och högsta gräns för varje timme.

**Exempel 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Beräknar hello högst Diskskrivningar per minut för alla diskar på datorn.

### <a name="where"></a>där
Syntax:

```
**where** AggregatedValue>20
```

Kan endast användas när en *mått* kommandot toofurther filter hello samman resulterar det hello *mått* aggregeringsfunktionen har skapats.

Exempel:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Dedupliceringen
Syntax:

    Dedup FieldName

Returnerar hello första dokument för varje unikt värde i hello baserat fältet.

Exempel:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Det här exemplet returnerar en händelse (hello senaste händelse) per EventID.

### <a name="join"></a>Slå ihop
Kopplingar hello resultaten av två frågor tooform en enda resultatuppsättning.  Har stöd för flera kopplingstyper beskrivs i hello Följ tabell.

| Kopplingstyp | Beskrivning |
|:--|:--|
| inre | Returnera bara poster med ett matchande värde i båda frågorna. |
| yttre | Returnera alla poster från båda frågorna.  |
| vänster  | Returnera alla poster från vänster frågan och matchande poster från rätt fråga. |


- Kopplingar för närvarande inte stöder frågor som innehåller hello **IN** nyckelord, hello **mått** kommando eller hello **utöka** kommandot om det riktar sig till ett fält från hello rätt fråga.
- Du kan för närvarande innehålla bara ett enda fält i en koppling.
- En sökning får inte innehålla mer än en koppling.

**Syntax**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Exempel**

tooillustrate hello olika kopplingstyper, bör du ansluta till en datatyp som samlas in från en anpassad logg med namnet MyBackup_CL och pulsslag hello för varje dator.  Dessa datatyper har hello efter data.

`Type = MyBackup_CL`

| TimeGenerated | Dator | LastBackupStatus |
|:---|:---|:---|
| 4/20/2017 01:26:32.137 AM | Srv01.contoso.com | Lyckades |
| 4/20/2017 02:13:12.381 AM | SRV02.contoso.com | Lyckades |
| 4/20/2017 02:13:12.381 AM | srv03.contoso.com | Fel |

`Type = Hearbeat`(Endast en delmängd av fält som visas)

| TimeGenerated | Dator | ComputerIP |
|:---|:---|:---|
| 2017-4/21 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 2017-4/21 12:02:21.916 PM | SRV02.contoso.com | 10.10.100.2 |
| 2017-4/21 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>inre koppling

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Returnerar hello följande poster där hello datorn fält matchar för båda datatyper.

| Dator| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Lyckades | 2017-4/21 12:01:34.482 PM | 10.10.100.1 | Pulsslag |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Lyckades | 2017-4/21 12:02:21.916 PM | 10.10.100.2 | Pulsslag |


#### <a name="outer-join"></a>yttre koppling

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Returnerar hello följande poster för båda datatyper.

| Dator| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Lyckades  | 2017-4/21 12:01:34.482 PM | 10.10.100.1 | Pulsslag |
| SRV02.contoso.com | 4/20/2017 02:14:12.381 AM | Lyckades  | 2017-4/21 12:02:21.916 PM | 10.10.100.2 | Pulsslag |
| srv03.contoso.com | 4/20/2017 01:33:35.974 AM | Fel  | 2017-4/21 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 2017-4/21 12:01:47.373 PM | 10.10.100.2 | Pulsslag |



#### <a name="left-join"></a>koppling till vänster

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Returnerar följande poster från MyBackup_CL med alla matchande fält från pulsslag hello.

| Dator| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Lyckades | 2017-4/21 12:01:34.482 PM | 10.10.100.1 | Pulsslag |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Lyckades | 2017-4/21 12:02:21.916 PM | 10.10.100.2 | Pulsslag |
| srv03.contoso.com | 4/20/2017 02:13:12.381 AM | Fel | | | |


### <a name="extend"></a>Utöka
Kan du toocreate körning fält i frågor. Observera att Runtime-fält inte kan användas med hello mått kommandot tooperform aggregering.

**Exempel 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Visar viktade rekommendation poäng för SQL-bedömning rekommendationer.

**Exempel 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Visar räknarvärdet i KBs i stället för byte.

**Exempel 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Skalor hello värdet för WireData TotalBytes så att alla resultat mellan 0 och 100.

**Exempel 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Taggar Perf värde som är mindre än 50 procent som låg, och andra som hög.

**Exempel 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Beräknar hello högst Diskskrivningar per minut för alla diskar på datorn.

**Funktioner som stöds**

| Funktionen | Beskrivning | Syntaxexemplen |
| --- | --- | --- |
| ABS |Returnerar hello absolutvärdet för hello anges värdet eller funktion. |`abs(x)` <br> `abs(-5)` |
| ARCCOS |Returnerar arcus cosinus av ett värde eller en funktion. |`acos(x)` |
| och |Returnerar värdet true om alla operanderna utvärdera tootrue. |`and(not(exists(popularity)),exists(price))` |
| ARCSIN |Returnerar sinus för båge för ett värde eller en funktion. |`asin(x)` |
| ARCTAN |Returnerar arctangens för ett värde eller en funktion. |`atan(x)` |
| ARCTAN2 |Returnerar hello vinkel följd hello konvertering av hello rectangular koordinaterna x, y-koordinaterna för toopolar. |`atan2(x,y)` |
| cbrt |Kuben rot. |`cbrt(x)` |
| ceil |Avrundar uppåt tooan heltal. |`ceil(x)`  <br> `ceil(5.6)`Returnerar 6 |
| COS |Returnerar cosinus för en vinkel. |`cos(x)` |
| COSH |Returnerar hyperboliskt cosinus för en vinkel. |`cosh(x)` |
| def |Förkortning för standard. Returnerar hello värdet för fältet ”Filter”. Om hello fältet inte finns, returnerar det angivna standardvärdet hello och ger hello första värde där: `exists()==true`. |`def(rating,5)`. Den här funktionen def() returnerar hello klassificering eller om ingen klassificering anges i hello dokumentet returnerar 5. <br> `def(myfield, 1.0)`motsvarar för`if(exists(myfield),myfield,1.0)`. |
| gr |Konverterar radianer toodegrees. |`deg(x)` |
| div |`div(x,y)`dividerar x av y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| Dist |Returnerar hello avståndet mellan två angreppsmetoderna, (poäng) i ett n-dimensionell utrymme. Hämtar i hello power plus två eller fler instanser av ValueSource och beräknar hello avstånd mellan hello t.ex. Varje ValueSource måste vara ett tal. Det måste vara ett jämnt antal ValueSource instanser skickades och hello metoden förutsätter att hello första hälften representerar hello första vektorn och andra hälften av hello representerar hello andra vector. |`dist(2, x, y, 0, 0)`Beräknar hello Euclidean avståndet mellan (0,0) och (x, y) för varje dokument. <br> `dist(1, x, y, 0, 0)`Beräknar hello Manhattan (Tag taxi) avståndet mellan (0,0) och (x, y) för varje dokument. <br> `dist(2,,x,y,z,0,0,0)`Euclidean avståndet mellan (0,0,0) och (x, y, z) för varje dokument.<br>`dist(1,x,y,z,e,f,g)`Manhattan avståndet mellan (x, y, z) och (e, f, g), där varje är ett fältnamn. |
| Det finns |Returnerar TRUE om något tillhör hello fältet finns. |`exists(author)`Returnerar TRUE för alla dokument som har ett värde i fältet för hello ”författare”.<br>`exists(query(price:5.00))`Returnerar SANT om ”pris” matchar ”5,00”. |
| EXP |Returnerar Eulers tal upphöjt toopower x. |`exp(x)` |
| våning |Avrundar ned tooan heltal. |`floor(x)`  <br> `floor(5.6)`Returnerar 5 |
| hypo |Returnerar sqrt(sum(pow(x,2),pow(y,2))) utan mellanliggande Dataspill eller -underskott. |`hypo(x,y)`  <br> ` |
| Om |Möjliggör villkorlig funktionen frågor. I `if(test,value1,value2)`, test eller så refererar tooa logiskt värde eller uttryck som returnerar ett logiskt värde (SANT eller FALSKT). `value1`hello-värdet returneras av hello-funktionen om test ger TRUE. `value2`hello-värdet returneras av hello-funktionen om test ger FALSE. Ett uttryck kan vara en funktion som tillämpar booleska värden. Det kan också vara en funktion som returnerar numeriska värden, i vilket fall värdet 0 tolkas som false, eller returnera strängar, i vilket fall tom sträng tolkas som false. |`if(termfreq(cat,'electronics'),popularity,42)`Den här funktionen kontrollerar varje dokument toosee om den innehåller hello termen ”electronics” hello cat fältet. Om det matchar, sedan hello returneras värdet för hello popularitet fältet. I annat fall returneras värdet hello 42. |
| linjär |Implementerar `m*x+c`där m och c är konstanter och x är en valfri funktion. Detta motsvarar för`sum(product(m,x),c)`, men något mer effektiv som det implementeras som en funktion. |`linear(x,m,c) linear(x,2,4)`Returnerar`2*x+4` |
| LN |Returnerar hello naturlig log hello specificerade funktion. |`ln(x)` |
| Logg |Returnerar hello loggen bas 10 av hello specificerade funktion. |`log(x)   log(sum(x,100))` |
| karta |Mappar alla värden i en inkommande funktion x som faller inom min och max, restriktiva toohello angivna målet. hello argument min och max måste vara konstanter. hello argument mål- och standard kan vara konstanter eller funktioner. Om hello värdet för x inte faller mellan min och max sedan antingen hello-värdet för x returneras eller ett standardvärde returneras om anges som en 5 argument. |`map(x,min,max,target) map(x,0,0,1)`Ändras alla värden för 0 too1. Detta kan vara användbart för att hantera standardvärden 0.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Ändras alla värden mellan 0 och 100 too1 och alla andra värden för-1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Ändrar värden mellan 0 och 100 toox + 599 och alla andra värden toofrequency löper hello 'solr' hello fältet text. |
| Max |Returnerar hello största numeriska värdet för flera kapslade funktioner eller konstanter som angetts som argument: `max(x,y,...)`. Hej max-funktionen kan också användas för ”bottoming” en annan funktion eller fältet på några angetts konstant.  Använd hello `field(myfield,max)` syntax för att välja hello maximivärdet för ett enda flervärdesfält. |`max(myfield,myotherfield,0)` |
| min. |Returnerar hello minsta numeriska värdet för flera kapslade funktioner av konstanter som angetts som argument: `min(x,y,...)`. hello min-funktionen kan också användas för att tillhandahålla en ”övre gränsen” på en funktion med en konstant. Använd hello `field(myfield,min)` syntax för att välja hello minimivärdet för ett enda flervärdesfält. |`min(myfield,myotherfield,0)` |
| MOD |Beräknar hello modulus av hello funktionen x av hello funktionen y. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Returnerar skillnaden mellan argumenten millisekunder. Datum är relativ toohello Unix- eller POSIX tid epok, midnatt den 1 januari 1970 UTC. Argument kan vara hello namnet på en indexerad TrieDateField eller datum matematiska baserat på ett konstant datum eller nu. `ms()`motsvarar för`ms(NOW)`, i millisekunder sedan hello epok. `ms(a)`Returnerar hello antalet millisekunder sedan hello epok som representerar hello argumentet. `ms(a,b)`Returnerar hello antalet millisekunder att b inträffar före en, vilket är `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| inte |hello logiskt negated värdet för hello omslutas funktion. |`not(exists(author))`GÄLLER endast när `exists(author)` är false. |
| eller |En logisk disjunktion. |`or(value1,value2)`TRUE om endera value1 eller value2 är true. |
| Pow |Aktiverar hello anges grundläggande toohello angetts power. `pow(x,y)`genererar x toohello kraften hos y. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Hej samma som rot. |
| Produkten |Returnerar hello produkten av flera värden eller funktioner som anges i en kommaavgränsad lista. `mul(...)`kan också användas som ett alias för den här funktionen. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Utför en ömsesidigt funktion med `recip(x,m,a,b)` implementera `a/(m*x+b)`, där m, a, b är konstanter och x är en godtyckligt komplexa funktion. När en och b är lika med- och x > = 0, den här funktionen har ett maximalt värde 1 släpper som x ökar. Öka hello-värdet för en och b tillsammans resulterar i en flödet av hello hela funktionen tooa plattare tillhör hello kurvan. De här egenskaperna kan göra detta till en perfekt funktion för att öka nyare dokument när x är `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad |Konverterar grader tooradians. |`rad(x)` |
| Skriv ut |Avrundar toohello närmsta heltal. |`rint(x)`  <br> `rint(5.6)`Returnerar 6 |
| sin |Returnerar sinus för en vinkel. |`sin(x)` |
| SINH |Returnerar hyperboliskt sinus för en vinkel. |`sinh(x)` |
| Skala |Skalor värden för hello funktionen x, så att de ligger mellan hello angivna minTarget och maxTarget inklusiva. hello aktuella implementeringen passerar alla hello funktionen värden tooobtain hello min och max, så den kan välja hello rätt skala. hello aktuella implementeringen kan inte skilja när dokument har tagits bort, eller dokument som har inget värde. Den använder 0.0 värden för dessa fall. Det innebär att om värdena är normalt alla större än 0,0, en kan fortfarande få 0,0 som hello min värdet toomap från. I dessa fall måste en lämplig `map()` funktionen kan användas som en lösning toochange 0,0 tooa värde i hello verkliga intervallet, som visas här:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`Skalor hello värdena på x, så att alla värden är mellan 1 och 2 inklusiva. |
| rot |Returnerar hello kvadratroten ur hello anges värdet eller funktion. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Beräknar hello avståndet mellan två strängar. Använder hello Lucene stavningskontroll checker StringDistance gränssnitt och stöder alla hello implementeringar tillgängligt i paketet. Kan också program tooplug i sina egna via Solrs resurs inläsning funktioner. strdist tar `(string1, string2, distance measure)`. Möjliga värden för avståndet måttet är:<ul><li>jw: Jaro Winkler</li><li>Redigera: Levenstein eller redigera avstånd</li><li>ngram: hello NGramDistance, om du kan om du vill skicka in hello ngram storlek för. Standardvärdet är 2.</li><li>FQN: Fullt kvalificerad klassnamn för en implementering av hello StringDistance gränssnitt. Måste ha en Nej %d{arg/ konstruktor.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub |Returnerar x-y från `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Summa |Returnerar hello summan av flera värden eller funktioner som anges i en kommaavgränsad lista. `add(...)`kan användas som ett alias för den här funktionen. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Returnerar hello antalet gånger hello termen visas i hello-fältet för dokumentet. |termfreq(text,'memory') |
| TAN |Returnerar tangens för en vinkel. |`tan(x)` |
| TANH |Returnerar hyperbolisk tangens för en vinkel. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Sök fältet och aspekten referens
När du använder loggen Sök toofind data visas resultatet olika fält och facets. Vissa av hello information visas inte mycket beskrivande. Använd hello följande information toohelp du förstår hello resultat.

| Fält | Söktyp | Beskrivning |
| --- | --- | --- |
| Klient-ID |Alla |Använda toopartition. |
| TimeGenerated |Alla |Använda toodrive hello tidslinjen timeselectors (i sökningen och i andra skärmar). Representerar när hello datadel genererades (vanligtvis på hello agent). hello tid uttryckt i ISO-format och är alltid UTC. Hello gäller typer som är baserade på befintliga instrumentation (det vill säga händelser i en logg), är det vanligtvis hello riktigt tid att hello post/rad/loggposten loggades. För vissa av hello hello andra typer som skapas via hanteringspaket eller i hello molnet (exempelvis rekommendationer eller varningar), tid representerar något annat. Detta är hello tiden när denna ny typ av data med en ögonblicksbild av en konfiguration av något slag samlades in, eller en rekommendation/avisering har producerats baserat på den. |
| Händelse-ID |Händelse |Händelse-ID hello i händelseloggen i Windows. |
| Händelseloggen |Händelse |Händelseloggen där hello händelsen loggades i Windows. |
| EventLevelName |Händelse |Kritisk/varning/information/lyckades |
| EventLevel |Händelse |Numeriskt värde för kritiska/varning/information/lyckad (Använd EventLevelName i stället för enklare/mer lättläst frågor). |
| SourceSystem |Alla |Där hello data kommer från (i bifoga läge toohello service). Exempel: Microsoft System Center Operations Manager och Azure Storage. |
| Objektnamn |PerfHourly |Objektnamn för Windows-prestanda. |
| Instansnamn |PerfHourly |Windows-instansnamn på prestandaräknare. |
| CounteName |PerfHourly |Namn på Windows prestandaräknare. |
| ObjectDisplayName |PerfHourly ConfigurationAlert ConfigurationObject, ConfigurationObjectProperty |Visningsnamnet för hello-objekt som mål för en regel för prestandainsamling i Operations Manager. Kan också vara hello visningsnamnet för hello-objekt som identifieras av åtgärdsinformation eller mot vilken hello aviseringen genererades. |
| RootObjectName |PerfHourly ConfigurationAlert ConfigurationObject, ConfigurationObjectProperty |Visningsnamn för hello överordnade hello överordnad (i en dubbel värdrelation) hello-objekt som mål för en regel för prestandainsamling i Operations Manager. Kan också vara hello visningsnamnet för hello-objekt som identifieras av åtgärdsinformation eller mot vilken hello aviseringen genererades. |
| Dator |De flesta typer |Datornamnet som hello data som hör till. |
| Enhetsnamn |ProtectionStatus |Datorns namn hello data tillhör för (samma som ”dator”). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Hot status rang är en numerisk representation av hello hot status. Liknande tooHTTP svarskoder hello rangordning har glapp mellan hello siffror (vilket är anledningen till att inga hot är 150 och inte 100 eller 0), och lämna utrymme tooadd nya tillstånd. Hello avsikt är en sammanfattning av status för hot och skyddsstatus tooshow hello sämsta tillstånd som hello datorn har varit i under hello tidsperiod. hello siffror rangordnas hello olika lägen, så du kan söka efter hello post med hello högsta antalet. |
| ThreatStatus |ProtectionStatus |Beskrivning av ThreatStatus, mappar 1 till 1 med ThreatStatusRank. |
| TypeofProtection |ProtectionStatus |Produkten för program mot skadlig kod som har identifierats i hello datorn: none, verktyget Borttagning av skadlig kod för Microsoft Forefront och så vidare. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus RequiredUpdate |HealthService-ID för den här datorns agent. |
| HealthServiceId |De flesta typer |HealthService-ID för den här datorns agent. |
| ManagementGroupName |De flesta typer |Namn på Hanteringsgrupp för Operations Manager-anslutna agenter. Annars är null/tomt. |
| Objekttyp |ConfigurationObject |Ange (som Operations Manager management pack-typ /-klass) för det här objektet har identifierats av logganalys configuration assessment. |
| UpdateTitle |RequiredUpdate |Namnet på hello uppdatering som inte hittades installerad. |
| PublishDate |RequiredUpdate |När hello uppdateringen publicerades på Microsoft Update. |
| Server |RequiredUpdate |Datorns namn hello data tillhör för (samma som ”dator”). |
| Produkt |RequiredUpdate |Produkten som hello uppdateringen gäller. |
| UpdateClassification |RequiredUpdate |Typ av uppdatering (till exempel Samlad uppdatering eller service pack). |
| KBID |RequiredUpdate |KB artikel-ID som beskriver det här bästa praxis eller update. |
| WorkflowName |ConfigurationAlert |Namnet på hello regeln eller övervakaren som producerade hello avisering. |
| Allvarsgrad |ConfigurationAlert |Allvarlighetsgraden hello avisering. |
| Prioritet |ConfigurationAlert |Prioriteten för hello avisering. |
| IsMonitorAlert |ConfigurationAlert |Den här aviseringen som genererats av Övervakare (SANT) eller en regel (FALSKT)? |
| AlertParameters |ConfigurationAlert |XML med hello parametrar för hello logganalys avisering. |
| Kontext |ConfigurationAlert |XML med hello kontext för hello logganalys avisering. |
| Arbetsbelastning |ConfigurationAlert |Teknik eller arbetsbelastning som hello avisering refererar till. |
| AdvisorWorkload |Rekommendation |Teknik eller arbetsbelastning som hello rekommendation refererar till. |
| Beskrivning |ConfigurationAlert |Aviseringsbeskrivningen (kort). |
| DaysSinceLastUpdate |UpdateAgent |Hur många dagar sedan (relativa tooTimeGenerated av den här posten) denna agent installerades uppdateringar från Windows Server Update Service (WSUS) eller Microsoft Update? |
| DaysSinceLastUpdateBucket |UpdateAgent |Baserat på DaysSinceLastUpdate, en kategorisering i tid buckets av hur länge sedan en dator senast installerade uppdateringar från WSUS-/ Microsoft Update. |
| AutomaticUpdateEnabled |UpdateAgent |Kontroll av automatisk uppdatering aktiveras eller inaktiveras på den här agenten? |
| AutomaticUpdateValue |UpdateAgent |Är automatisk uppdatering kontrollerar set tooautomatically hämta och installera, endast hämta eller bara kontrollera? |
| WindowsUpdateAgentVersion |UpdateAgent |Versionsnummer för hello Microsoft Update-agenten. |
| WSUSServer |UpdateAgent |Vilken WSUS-server är den här update-agenten på? |
| OSVersion |UpdateAgent |Version av operativsystemet för hello update-agenten körs på. |
| Namn |Rekommendation ConfigurationObjectProperty |/ Titel på hello rekommendation eller namnet på hello egenskap från logganalys configuration assessment. |
| Värde |ConfigurationObjectProperty |Värdet för en egenskap från logganalys configuration assessment. |
| KBLink |Rekommendation |URL: en toohello KB-artikel som beskriver det här bästa praxis eller en uppdatering. |
| RecommendationStatus |Rekommendation |Rekommendationerna finns bland hello några typer vars poster få uppdaterade, inte nyligen tillagda toohello sökindex. Den här statusen ändras om hello rekommendation är aktiv/öppna, eller om logganalys upptäcker att den har lösts. |
| RenderedDescription |Händelse |Renderade beskrivning (återanvända text ifyllda parametrar) av en Windows-händelse. |
| ParameterXml |Händelse |XML med hello parametrar under hello data i en Windows-händelse (som visas i Loggboken). |
| EventData |Händelse |XML med hello hela dataavsnittet i en Windows-händelse (som visas i Loggboken). |
| Källa |Händelse |Källan för händelseloggning som genererade hello-händelse. |
| EventCategory |Händelse |Kategori hello händelse direkt från hello Windows-händelseloggen. |
| Användarnamn |Händelse |Användarnamnet på hello Windows-händelse (normalt NT AUTHORITY\LOCALSYSTEM). |
| SampleValue |PerfHourly |Genomsnittligt värde för hello timvis aggregering för en prestandaräknare. |
| Min |PerfHourly |Minsta värdet i hello timintervall av en prestandaräknaren timvis aggregering. |
| Max. |PerfHourly |Maximalt värde i hello timintervall av en prestandaräknaren timvis aggregering. |
| Percentile95 |PerfHourly |hello 95 percentilvärdet för hello timintervall av en prestandaräknaren timvis aggregering. |
| SampleCount |PerfHourly |Hur många rådataprestanda räknaren prover har använt tooproduce detta varje timme aggregera post. |
| Hot |ProtectionStatus |Namn på skadlig kod. |
| StorageAccount |W3CIISLog |Hello Azure lagringskontologg lästes från. |
| AzureDeploymentID |W3CIISLog |Azure-distribution-ID för hello molnet hello loggfiler som hör till. |
| Roll |W3CIISLog |Rollen för hello Azure cloud service hello loggen tillhör. |
| RoleInstance |W3CIISLog |RoleInstance av hello Azure roll som hello loggen tillhör. |
| sSiteName |W3CIISLog |IIS-webbplatsen som hello loggen tillhör too(metabase notation); hello s-sitename fält i hello ursprungliga loggen. |
| sComputerName |W3CIISLog |hello s-computername fält i hello ursprungliga loggen. |
| sIP |W3CIISLog |Servern IP-adress hello HTTP-begäran var riktar sig till. hello s-IP-fält i hello ursprungliga loggen. |
| csMethod |W3CIISLog |HTTP-metoden (till exempel GET/POST) används av hello klient i hello HTTP-begäran. hello cs-metod i hello ursprungliga loggen. |
| cIP |W3CIISLog |Klientens IP-adress hello HTTP-begäran kommer från. hello c-IP-fält i hello ursprungliga loggen. |
| csUserAgent |W3CIISLog |HTTP-användaragent deklareras av hello klienten (webbläsaren eller på annat sätt). hello cs-användaragenten i hello ursprungliga loggen. |
| scStatus |W3CIISLog |HTTP-statuskoden (till exempel 200/403/500) returnerades av hello server toohello-klienten. hello cs-status i hello ursprungliga loggen. |
| timeTaken |W3CIISLog |Hur länge (i millisekunder) tog som hello-begäran toocomplete. Hej timetaken fält i hello ursprungliga loggen. |
| csUriStem |W3CIISLog |Relativ URI (utan värdadress, som är, / söka) som begärdes. hello cs uristem fält i hello ursprungliga loggen. |
| csUriQuery |W3CIISLog |URI-fråga. URI-frågor krävs endast för dynamiska sidor, till exempel ASP-sidor, så att det här fältet innehåller vanligtvis ett bindestreck för statiska sidor. |
| sPort |W3CIISLog |Serverport som hello HTTP-begäran har skickats för (och som IIS lyssnar på, eftersom den skaffat). |
| csUserName |W3CIISLog |Autentisera användarnamnet, om hello-begäran är autentiserad och inte anonym. |
| csVersion |W3CIISLog |HTTP-protokollversion som används i hello begäran (till exempel HTTP/1.1). |
| csCookie |W3CIISLog |Cookie-information. |
| csReferer |W3CIISLog |Plats för hello användaren senast besökte. Denna plats innehöll en länk toohello aktuella plats. |
| csHost |W3CIISLog |Värdadressen (till exempel www.mysite.com) som begärdes. |
| scSubStatus |W3CIISLog |Understatus. |
| scWin32Status |W3CIISLog |Windows-statuskod. |
| csBytes |W3CIISLog |Byte som skickats i hello begäran från hello klient toohello server. |
| scBytes |W3CIISLog |Byte returnerade tillbaka i hello-svar från hello server toohello-klienten. |
| ConfigChangeType |ConfigurationChange |Typ av ändring (till exempel WindowsServices/program). |
| ChangeCategory |ConfigurationChange |Ändringskategori hello (Modified/lagts till/tagits bort). |
| SoftwareType |ConfigurationChange |Typ av programvara (uppdateringsprogrammet). |
| SoftwareName |ConfigurationChange |Namn på hello programvara (endast tillämpliga toosoftware ändringar). |
| Utgivare |ConfigurationChange |Leverantören som publicerar hello programvara (endast tillämpliga toosoftware ändringar). |
| SvcChangeType |ConfigurationChange |Typ av ändring som tillämpades på en Windows-tjänst (tillstånd/StartupType/sökväg/tjänstkonto). Detta är endast tillämplig tooWindows tjänsten ändras. |
| SvcDisplayName |ConfigurationChange |Visningsnamnet för hello-tjänst som har ändrats. |
| SvcName |ConfigurationChange |Namnet på hello-tjänst som har ändrats. |
| SvcState |ConfigurationChange |Nya () av tjänstens aktuella tillstånd hello. |
| SvcPreviousState |ConfigurationChange |Tidigare fungerande tillstånd för hello-tjänst (gäller endast om tjänstens tillstånd ändras). |
| SvcStartupType |ConfigurationChange |Starttyp för tjänsten. |
| SvcPreviousStartupType |ConfigurationChange |Tidigare tjänststarttyp (gäller endast om tjänstens starttyp ändras). |
| SvcAccount |ConfigurationChange |Tjänstkontot. |
| SvcPreviousAccount |ConfigurationChange |Tidigare tjänstkontot (gäller endast om tjänstkontot har ändrats). |
| SvcPath |ConfigurationChange |Sökvägen toohello körbar fil för Windows hello-tjänst. |
| SvcPreviousPath |ConfigurationChange |Föregående sökväg hello körbara för hello Windows-tjänsten (gäller endast om det ändras). |
| SvcDescription |ConfigurationChange |Beskrivning av hello-tjänsten. |
| Föregående |ConfigurationChange |Föregående status för den här programvaran (installerad ej installerad/föregående versionen). |
| Aktuella |ConfigurationChange |Senaste status för den här programvaran (installerad ej installerad/current version). |

## <a name="next-steps"></a>Nästa steg
Mer information om loggen sökningar:

* Bekanta dig med [logga sökningar](log-analytics-log-searches.md) tooview detaljerad information som samlas in av lösningar.
* Använd [anpassade fält i logganalys](log-analytics-custom-fields.md) tooextend loggen sökningar.
