---
title: "aaaQuery exempel på vanliga användningsmönster i Stream Analytics | Microsoft Docs"
description: "Vanliga frågemönster för Azure Stream Analytics"
keywords: "frågan exempel"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Exempel på vanliga Stream Analytics användningsmönster fråga
## <a name="introduction"></a>Introduktion
Frågorna i Azure Stream Analytics uttrycks i ett SQL-liknande frågespråk. De här frågorna dokumenteras i hello [Stream Analytics fråga Språkreferens](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide. Den här artikeln beskrivs lösningar tooseveral vanliga frågemönster, baserat på verkliga scenarier. Det är ett pågående arbete och fortsätter toobe uppdateras med nya mönster kontinuerligt.

## <a name="query-example-convert-data-types"></a>Frågan exempel: konvertera-datatyper
**Beskrivning**: definiera hello typer av egenskaper på hello Indataströmmen.
Till exempel hello bil vikt kommer på hello Indataströmmen som strängar och behöver toobe konverteras för**INT** tooperform **SUMMAN** den.

**Indata**:

| Kontrollera | Tid | Vikt |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Utdata**:

| Kontrollera | Vikt |
| --- | --- |
| Honda |3000 |

**Lösningen**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Förklaring**: Använd ett **OMVANDLINGEN** instruktionen i hello **vikt** fältet toospecify dess datatyp. Visa hello lista över vilka datatyper i [datatyper (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a>Frågan exempel: Använd liknande ej som toodo matchning
**Beskrivning**: kontrollerar att ett fältvärde på hello händelse matchar ett visst mönster.
Kontrollera exempelvis att hello resultatet returnerar licens nivåer som börjar med ett och slutar med 9.

**Indata**:

| Kontrollera | LicensePlate | Tid |
| --- | --- | --- |
| Honda |ABC 123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC 369 |2015-01-01T00:00:03.0000000Z |

**Utdata**:

| Kontrollera | LicensePlate | Tid |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC 369 |2015-01-01T00:00:03.0000000Z |

**Lösningen**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Förklaring**: Använd hello **som** instruktionen toocheck hello **LicensePlate** fältet värde. Det måste börja med en A-, och sedan har ett antal noll eller fler tecken och sedan avslutas med en 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Frågan exempel: Ange logik för olika fall/värden (CASE-satser)
**Beskrivning**: Ange en annan beräkning för ett fält baserat på ett visst kriterium.
Ange till exempel en sträng beskrivning för hur många bilar av hello samma skickas med ett specialfall för 1.

**Indata**:

| Kontrollera | Tid |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Utdata**:

| CarsPassed | Tid |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Lösningen**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Förklaring**: hello **FALLET** satsen gör att vi tooprovide olika beräkning, baserat på vissa kriterier (i vårt fall hello antal hello bilar i hello sammanställd fönstret).

## <a name="query-example-send-data-toomultiple-outputs"></a>Frågan exempel: skicka data toomultiple matar ut
**Beskrivning**: skicka data toomultiple utdata mål från ett enda utskriftsjobb.
Till exempel analysera data för en avisering om tröskelvärdesbaserad och arkivlagring alla händelser tooblob.

**Indata**:

| Kontrollera | Tid |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Kontrollera | Tid |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Kontrollera | Tid | Antal |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Lösningen**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Förklaring**: hello **INTO** satsen instruerar Stream Analytics som hello matar ut toowrite hello data toofrom den här instruktionen.
hello första frågan är en direktlagringsdisk hello data togs emot tooan utdata som vi med namnet **ArchiveOutput**.
hello andra frågan har några enkla aggregering och filtrering och skickar resultatet hello tooa underordnade aviseringar system.

Observera att du kan också återanvända hello resultaten av hello cte (cte-referenser) (som **WITH** instruktioner) i flera instruktioner i utdata. Det här alternativet har hello ytterligare fördelen med att öppna färre läsare toohello Indatakällan.
Exempel: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Frågan exempel: Räkna antalet unika värden
**Beskrivning**: antal hello unikt fältvärden som visas i hello dataströmmen inom ett tidsintervall.
Till exempel hur många unika gör bilar passerat hello avgift monter i ett fönster med 2 sekunder?

**Indata**:

| Kontrollera | Tid |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Utdata:**

| Antal | Tid |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Lösning:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Förklaring:**
**COUNT (DISTINKTA gör)** returnerar hello antalet distinkta värden i hello **Se** kolumnen inom ett tidsintervall.

## <a name="query-example-determine-if-a-value-has-changed"></a>Frågan exempel: fastställa om ett värde har ändrats
**Beskrivning**: Titta på en tidigare värde toodetermine om den skiljer sig hello aktuellt värde.
Exempelvis är hello tidigare bil på hello avgift väg hello samma göra hello aktuella bilen?

**Indata**:

| Kontrollera | Tid |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Utdata**:

| Kontrollera | Tid |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Lösningen**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Förklaring**: Använd **FÖRDRÖJNING** toopeek till hello inkommande dataström tillbaka en händelse och få hello **Se** värde. Jämför det toohello **Se** värdet på hello händelsen och hello utdatahändelse om de är olika.

## <a name="query-example-find-hello-first-event-in-a-window"></a>Frågan exempel: Sök hello första händelsen i ett fönster
**Beskrivning**: hitta hello första bil i varje 10 minuters intervall.

**Indata**:

| LicensePlate | Kontrollera | Tid |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Utdata**:

| LicensePlate | Kontrollera | Tid |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Lösningen**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nu ska vi ändra hello problemet och hitta hello första bil av en viss gör i varje 10 minuters intervall.

| LicensePlate | Kontrollera | Tid |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Lösningen**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a>Frågan exempel: Sök hello sista händelsen i ett fönster
**Beskrivning**: hitta hello senaste bil i varje 10 minuters intervall.

**Indata**:

| LicensePlate | Kontrollera | Tid |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Utdata**:

| LicensePlate | Kontrollera | Tid |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Lösningen**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Förklaring**: det finns två steg i hello-frågan. hello första en hittar hello senaste tidsstämpel i windows 10 minuter. hello andra steg kopplingar hello resultaten av hello första frågan med hello ursprungliga dataströmmen toofind hello händelser som matchar hello senaste tidsstämplar i varje fönster. 

## <a name="query-example-detect-hello-absence-of-events"></a>Frågan exempel: identifiera hello frånvaro av händelser
**Beskrivning**: Kontrollera att en ström som inte har något värde som matchar vissa villkor.
Till exempel 2 på varandra följande bilar från samma hello angett hello avgift väg inom hello sista 90 sekunder?

**Indata**:

| Kontrollera | LicensePlate | Tid |
| --- | --- | --- |
| Honda |ABC 123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF 987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI 345 |2015-01-01T00:00:04.0000000Z |

**Utdata**:

| Kontrollera | Tid | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC 123 |2015-01-01T00:00:01.0000000Z |

**Lösningen**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Förklaring**: Använd **FÖRDRÖJNING** toopeek till hello inkommande dataström tillbaka en händelse och få hello **Se** värde. Jämför det toohello **Se** värdet i hello händelsen och hello utdatahändelse om de är hello samma. Du kan också använda **FÖRDRÖJNING** tooget data om hello tidigare bil.

## <a name="query-example-detect-hello-duration-between-events"></a>Frågan exempel: identifiera hello varaktigheten mellan händelser
**Beskrivning**: hitta hello varaktighet för en given händelse. Till exempel ges en web clickstream bestämma hello tidsåtgången för en funktion.

**Indata**:  

| Användare | Funktion | Händelse | Tid |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Start |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |End |2015-01-01T00:00:08.0000000Z |

**Utdata**:  

| Användare | Funktion | Varaktighet |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Lösningen**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Förklaring**: Använd hello **senaste** fungerar tooretrieve hello senast **tid** värde när hello händelsetyp **starta**. Hej **senaste** använder funktionen **PARTITION BY [användare]** tooindicate som hello resultatet beräknas per unika användare. hello-frågan har en maximal tröskel 1 timme för hello tidsskillnaden mellan **starta** och **stoppa** händelser, men kan konfigureras vid behov **(GRÄNSEN DURATION(hour, 1)**.

## <a name="query-example-detect-hello-duration-of-a-condition"></a>Frågan exempel: identifiera hello varaktighet för ett villkor
**Beskrivning**: ta reda på hur länge ett villkor inträffade.
Anta exempelvis att en bugg resulterade i alla bilar med en felaktig vikt (ovanför 20 000 pund). Vi vill toocompute hello varaktighet hello programfel.

**Indata**:

| Kontrollera | Tid | Vikt |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Utdata**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Lösningen**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Förklaring**: Använd **FÖRDRÖJNING** tooview hello Indataströmmen under 24 timmar och leta efter instanser där **StartFault** och **StopFault** omfattas av hello vikt < 20000.

## <a name="query-example-fill-missing-values"></a>Frågan exempel: fylla i saknade värden
**Beskrivning**: för hello ström av händelser som saknar värden, skapar du en dataström med händelser med jämna mellanrum.
Till exempel generera en händelse var femte sekund som rapporter hello senast visade datapunkt.

**Indata**:

| T | värde |
| --- | --- |
| ”2014-01-01T06:01:00” |1 |
| ”2014-01-01T06:01:05” |2 |
| ”2014-01-01T06:01:10” |3 |
| ”2014-01-01T06:01:15” |4 |
| ”2014-01-01T06:01:30” |5 |
| ”2014-01-01T06:01:35” |6 |

**Utdata (första 10 raderna)**:

| windowend | lastevent.t | lastevent.Value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Lösningen**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Förklaring**: den här frågan genererar händelser var femte sekund och utdata hello senaste händelse som du fick tidigare. Hej [Hopping fönstret](https://msdn.microsoft.com/library/dn835041.aspx "Hopping fönstret--Azure Stream Analytics") varaktighet anger hur långt tillbaka hello fråga Se toofind hello senaste händelse (300 sekunder i det här exemplet).

## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

