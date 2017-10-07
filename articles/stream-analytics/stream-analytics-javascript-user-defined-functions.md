---
title: "aaaAzure Stream Analytics JavaScript användardefinierade funktioner | Microsoft Docs"
description: "Utföra en avancerad fråga säkerhetsnivån med JavaScript användardefinierade funktioner"
keywords: "JavaScript, användardefinierade funktioner, udf"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Azure Stream Analytics JavaScript användardefinierade funktioner
Azure Stream Analytics stöder användardefinierade funktioner som skrivits i JavaScript. Med hello omfattande uppsättning **sträng**, **RegExp**, **matematiska**, **matris**, och **datum** metoder som Ger JavaScript, komplexa Datatransformationer med Stream Analytics-jobb blir enklare toocreate.

## <a name="javascript-user-defined-functions"></a>JavaScript användardefinierade funktioner
JavaScript användardefinierade funktioner stöder tillståndslösa, endast beräkning skalärfunktioner som inte kräver extern anslutning. hello returnera värdet för en funktion kan endast vara ett skalärvärde (enkel). När du lägger till ett JavaScript användardefinierad funktion tooa jobb, kan du använda hello funktionen var som helst i hello-frågan som en inbyggd skalärfunktion.

Här följer några scenarier där användbara JavaScript användardefinierade funktioner:
* Tolkning och manipulera strängar som har funktioner för reguljära uttryck, till exempel **Regexp_Replace()** och **Regexp_Extract()**
* Avkoda och kodning data, till exempel binary-hex-konvertering
* Utföra matematiska beräkningar med JavaScript **matematiska** funktioner
* Utför åtgärder i matrisen som sortera, koppling, Sök och fill

Här följer några saker som du inte kan göra med en JavaScript-användardefinierad funktion i Stream Analytics:
* Anropet ut externa REST-slutpunkter, till exempel utför omvänd sökning av IP- eller datahämtning referensdata från en extern källa
* Utför anpassade händelsen format serialisering eller avserialisering på indata/utdata
* Skapa anpassade mängder

Även om fungerar som **Date.GetDate()** eller **Math.random()** inte är blockerade i hello funktioner definition du bör undvika att använda dem. Dessa funktioner **inte** returnerade hello samma varje gång du anropar dem och hello Azure Stream Analytics-tjänsten inte har en journal av funktionsanrop och kan medföra returnerade resultat. Om en funktion returnerar olika resultat på hello samma händelser, garanteras inte repeterbarhet när ett jobb startas av dig eller hello Stream Analytics-tjänsten.

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a>Lägg till JavaScript användardefinierad funktion i hello Azure-portalen
toocreate en enkel JavaScript användardefinierad funktion under ett befintligt Stream Analytics-jobb, utföra de här stegen:

1.  Hello Azure-portalen, hitta Stream Analytics-jobbet.
2.  Under **jobbet TOPOLOGI**, Välj din funktion. En tom lista över funktioner visas.
3.  toocreate en ny användardefinierad funktion, Välj **Lägg till**.
4.  På hello **nya funktionen** bladet för **funktionstyp**väljer **JavaScript**. En standardmall för funktionen visas i hello-redigeraren.
5.  För hello **UDF alias**, ange **hex2Int**, och ändra hello funktionen implementering på följande sätt:

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Välj **Spara**. Funktionen visas i hello lista över funktioner.
7.  Välj ny hello **hex2Int** fungerar och kontrollera hello funktionsdefinitionen. Alla funktioner har en **UDF** prefix tillagda toohello funktionen alias. Du behöver för*hello prefixet* när du anropar funktionen hello i Stream Analytics-fråga. I så fall måste du anropa **UDF.hex2Int**.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Anropa en användardefinierad JavaScript-funktion i en fråga

1. I hello frågan editor under **jobbet TOPOLOGI**väljer **frågan**.
2.  Redigera frågan och sedan anropa hello användardefinierad funktion, så här:

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  tooupload hello exempeldatafil, högerklicka på hello jobbet indata.
4.  tootest frågan, Välj **Test**.


## <a name="supported-javascript-objects"></a>Stöds JavaScript-objekt
Azure Stream Analytics JavaScript användardefinierade funktioner stöder standard, inbyggda JavaScript-objekt. En lista över de här objekten finns [globala objekt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics och JavaScript typkonvertering

Det finns skillnader i hello-typer som har stöd för hello Stream Analytics-frågespråket och JavaScript. Den här tabellen visar hello konvertering mappningar mellan hello två:

Stream Analytics | JavaScript
--- | ---
bigint | Antal (JavaScript kan endast representera heltal in tooprecisely 2 ^ 53)
Datum och tid | Datum (JavaScript endast stöder millisekunder)
dubbla | Tal
nvarchar(max) | Sträng
Post | Objekt
Matris | Matris
NULL | Null


Här är JavaScript-Stream Analytics-konvertering:


JavaScript | Stream Analytics
--- | ---
Tal | Bigint (om hello numret är runda och mellan lång. MinValue och lång. MaxValue; Annars är det dubbla)
Date | Datum och tid
Sträng | nvarchar(max)
Objekt | Post
Matris | Matris
Null, Odefinierad | NULL
En annan typ (till exempel en funktion eller fel) | Stöds inte (resulterar i körningsfel)

## <a name="troubleshooting"></a>Felsökning
JavaScript-körningsfel betraktas som allvarligt och är anslutna via hello aktivitetsloggen. tooretrieve hello logg i hello Azure-portalen, gå tooyour jobb och välj **aktivitetsloggen**.


## <a name="other-javascript-user-defined-function-patterns"></a>Andra JavaScript användardefinierad funktion mönster

### <a name="write-nested-json-toooutput"></a>Skriva den kapslade JSON toooutput
Om du har en uppföljning bearbetningssteg som använder en Stream Analytics-jobbet utdata som indata, och det krävs en JSON-format, kan du skriva toooutput en JSON-sträng. hello nästa exempel anropar hello **JSON.stringify()** fungerar toopack alla namn/värde-par av hello indata och skriva dem som ett enda strängvärde i utdata.

**Definition av JavaScript användardefinierad funktion:**

```
function main(x) {
return JSON.stringify(x);
}
```

**Exempelfråga:**
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a>Få hjälp
För mer hjälp kan du prova vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referens](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
