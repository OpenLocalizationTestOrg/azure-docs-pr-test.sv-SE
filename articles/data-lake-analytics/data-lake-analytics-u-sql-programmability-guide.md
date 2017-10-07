---
title: "aaaU SQL-programmering guide för Azure Data Lake | Microsoft Docs"
description: "Läs mer om hello uppsättning tjänster i Azure Data Lake som gör att du toocreate en molnbaserad stordataplattform."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: saveenr
ms.openlocfilehash: cc8f126234c6106a0dc633ce85a1d9ab1e634e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-programmability-guide"></a>U-SQL-programmering guide

U-SQL är ett frågespråk som är utformad för stora datatypen för arbetsbelastningar. En av hello unika funktionerna i U-SQL är hello kombination av hello SQL-liknande deklarativ språk med hello utökningsbarhet och programmering som tillhandahålls av C#. I den här handboken vi koncentrera dig på hello utökningsbarhet och hello U-SQL-språket som är aktiverad som C#-programmering.

## <a name="requirements"></a>Krav

Hämta och installera [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>Kom igång med U-SQL  

Nu ska vi titta på hello följande U-SQL-skript:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso",   1500.0, "2017-03-39"),
            ("Woodgrove", 2700.0, "2017-04-10")
        ) AS 
              D( customer, amount );
@results =
    SELECT
        customer,
    amount,
    date
    FROM @a;    
```

Den definierar en raduppsättning kallas @a och skapar en raduppsättning kallas @results från @a.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C#-typer och uttryck i U-SQL-skript

Ett U-SQL-uttryck är ett C#-uttryck i kombination med U-SQL logiska operationer sådana `AND`, `OR`, och `NOT`. U-SQL-uttryck kan användas med SELECT-, EXTRAHERA, där, med, gruppera efter och DEKLARERA.

Följande skript hello Parsar till exempel en sträng ett DateTime-värde i hello SELECT-satsen.

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

hello Parsar följande skript en sträng ett DateTime-värde i en DECLARE-sats.

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Använd C#-uttryck för datatypkonverteringar
hello som följande exempel visar hur du kan göra en konvertering för datetime-data med hjälp av C#-uttryck. I det här scenariot är datetime strängdata konverterade toostandard datetime med Tidformatet för midnatt 00:00:00.

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 too@output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>Använd C#-uttryck för dagens datum
toopull dagens datum, vi kan använda hello följande C#-uttryck:

```
DateTime.Now.ToString("M/d/yyyy")
```

Här är ett exempel på hur toouse uttrycket i ett skript:

```
@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```



## <a name="using-net-assemblies"></a>Med hjälp av .NET-sammansättningar
U-SQL-modellen för utökning beroende kraftigt hello möjlighet tooadd anpassad kod. För närvarande ger U-SQL dig enkla sätt tooadd egna Microsoft. NET-baserade kod (särskilt C#). Du kan också lägga till egen kod som skrivs i andra .NET-språk, till exempel VB.NET eller F #. 

### <a name="register-a-net-assembly"></a>Registrera en .NET-sammansättning

Använd hello CREATE ASSEMBLY instruktionen tooplace en .NET-sammansättning i en U-SQL-databas. När en sammansättning i en databas kan kan U-SQL-skript använda dessa sammansättningar med hello REFERENSSAMMANSÄTTNING-sats. 

Hej följande kod visar hur tooregister en sammansättning:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

Hej följande kod visar hur tooreference en sammansättning:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Kontakta hello [sammansättningen instruktioner](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) som omfattar det här avsnittet i detalj.


### <a name="use-assembly-versioning"></a>Använd sammansättningen versionshantering
För närvarande använder U-SQL hello .NET Framework version 4.5. Kontrollera att din egen sammansättningar är kompatibla med den här versionen av hello körning.

Som tidigare nämnts U-SQL kör kod i en 64-bitars (x 64)-format. Kontrollera så att koden är kompilerade toorun på x64. Annars kan du hämta hello felaktigt formatfel visades tidigare.

Varje överförs sammansättningen DLL-filen och resursfilen som en annan runtime, en inbyggd sammansättning eller en .config-fil kan innehålla högst 400 MB. hello total storlek för distribuerade resurser via DISTRIBUERA resurs eller via referenser tooassemblies och deras ytterligare filer får inte överstiga 3 GB.

Slutligen, Observera att varje U-SQL-databas kan bara innehålla en version av alla angivna sammansättningen. Till exempel om du behöver både 7 och versionen 8 av hello NewtonSoft Json.Net bibliotek, måste tooregister dem i två olika databaser. Dessutom kan varje skript endast hänvisa tooone version av en angiven sammansättning DLL. I detta avseende följer U-SQL hello C# sammansättningen hantering och versionshantering semantik.


## <a name="use-user-defined-functions-udf"></a>Använda användardefinierade funktioner: UDF
U-SQL-användardefinierade funktioner eller UDF, programmerar rutiner som accepterar parametrar, utföra en åtgärd (till exempel en komplicerad beräkning) och returnera hello resultatet av den åtgärden som ett-värde. hello returnera värdet för UDF kan endast vara en enskild skalär. U-SQL-UDF kan anropas i grundläggande U-SQL-skript som andra C# skalära funktioner.

Vi rekommenderar att du initiera U-SQL-användardefinierade funktioner som **offentliga** och **statiska**.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

Först ska vi titta på hello enkelt exempel på hur du skapar en UDF.

I detta scenario användningsfall måste toodetermine hello räkenskapsårets period, inklusive hello räkenskapskvartal och räkenskapsmånad på hello första inloggningen för hello specifika användare. hello är första räkenskapsmånad på hello år i vårt scenario juni.

toocalculate räkenskapsårets period, introducerar vi följande C#-funktionen hello:

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Bara beräknar räkenskapsmånad och kvartal och returnerar ett strängvärde. Vi använder ”Q1:P1” för juni hello första månad hello första räkenskapskvartal. Vi använder ”Q1:P2” för juli och så vidare.

Det här är en vanlig C#-funktion som vi är pågående toouse i vår U-SQL-projekt.

Här är hur hello bakomliggande kod avsnitt ser ut i det här scenariot:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }

    }

}
```

Nu vi toocall funktionen från hello grundläggande U-SQL-skript. toodo, har vi tooprovide ett fullständigt kvalificerat namn för hello funktion, inklusive hello namnområdet, som i det här fallet är NameSpace.Class.Function(parameter).

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Följande är hello faktiska U-SQL bas-skript:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

Nedan följer hello utdatafilen för körning av skript hello:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Det här exemplet visar en enkel användning av infogade UDF i U-SQL.

### <a name="keep-state-between-udf-invocations"></a>Behåll tillstånd mellan UDF-anrop
U-SQL C#-programmering objekt kan vara mer förfinade använder interaktivitet via hello bakomliggande kod globala variabler. Nu ska vi titta på hello följande affärsscenario användningsfall.

Användare kan växla mellan av interna program i stora organisationer. Det kan vara Microsoft Dynamics CRM, PowerBI och så vidare. Kunder vill kanske tooapply en telemetri analys av hur användarna växlar mellan olika program, vilka hello användning trender är och så vidare. hello målet för hello företag är toooptimize programanvändning. De kan också toocombine olika program eller specifika inloggning rutiner.

tooachieve det här målet, har vi toodetermine sessions-ID. och fördröjningen mellan hello senaste sessionen som inträffat.

Vi behöver toofind en tidigare inloggning och sedan tilldela den här inloggningen tooall-sessioner som har genererat toohello samma program. hello första kontrollen är att grundläggande U-SQL-skript inte kan vi tooapply beräkningar över redan beräknade kolumner med funktionen LAG. hello andra challenge är att det finns specifika tookeep hello-session för alla sessioner inom hello samma tidsperiod.

toosolve det här problemet kan vi använda den globala inuti en bakomliggande kod: `static public string globalSession;`.

Den globala variabeln är tillämpade toohello hela raduppsättning under våra körning av skript.

Här är hello bakomliggande kod avsnitt i vårt U-SQL-program:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

Det här exemplet visar hello globala variabeln `static public string globalSession;` används inuti hello `getStampUserSession` funktion och hämtar initieras på nytt varje gång hello Session parameter ändras.

hello grundläggande U-SQL-skriptet är följande:

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    too@out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

Funktionen `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` anropas här under hello andra minne raduppsättningen beräkning. Hello överförs `UserSessionTimestamp` kolumn och returnerar hello värdet förrän `UserSessionTimestamp` har ändrats.

hello utdatafilen är följande:

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Det här exemplet visar en mer komplicerad användningsfall scenario där vi använder en global variabel i en bakomliggande kod avsnitt som är kopplade toohello hela minne raduppsättning.

## <a name="use-user-defined-types-udt"></a>Använda användardefinierade typer: UDT
Användardefinierade typer eller UDT, är en annan programmeringsfunktionen av U-SQL. U-SQL-UDT fungerar som en vanlig C# användardefinierad datatyp. C# är ett starkt typifierad språk som tillåter hello användning av inbyggda och anpassade användardefinierade typer.

U-SQL kan inte implicit serialisera eller deserialisera godtycklig UDT när hello UDT överförs mellan formhörnen i raduppsättningar. Detta innebär att hello användaren tooprovide en explicit formaterare hello IFormatter-gränssnittet. Detta ger U-SQL hello serialisera och deserialisera metoder för hello UDT.

> [!NOTE]
> U-SQL kan inte inbyggda juicepressar outputters för närvarande serialisera och deserialisera UDT data tooor från filer även med hello IFormatter uppsättningen. När du skriver UDT tooa datafilen med hello utdata-instruktion, eller läsa med en extraktor du toopass den som en sträng eller byte-matris. Sedan anropa hello serialisering och avserialisering kod (det vill säga hello UDT metoden ToString()) explicit. Användardefinierade juicepressar och outputters på hello andra kan lämnar, läsa och skriva UDT-typer.

Om vi försöker toouse UDT i EXTRAKTOR eller OUTPUTTER (från föregående SELECT-), som visas här:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

Vi tar emot hello följande fel:

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used toooutput column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how tooserialize this type, or call a serialization method on hello type in
hello preceding SELECT. C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

toowork med UDT i outputter antingen har vi tooserialize det toostring med hello metoden ToString() eller skapa en anpassad outputter.

Användardefinierade typer som kan för närvarande inte användas i GROUP BY. Om UDT används i GROUP BY, uppstod hello följande fel:

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want toouse with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

toodefine en UDT vi behöver:

* Lägg till hello följande namnområden:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* Lägg till `Microsoft.Analytics.Interfaces`, vilket krävs för hello UDT-gränssnitt. Dessutom `System.IO` kan vara nödvändiga toodefine hello IFormatter gränssnitt.

* Definiera en användardefinierade typ med SqlUserDefinedType attribut.

**SqlUserDefinedType** är används toomark en typdefinition i en sammansättning som användardefinierade typen (UDT) i U-SQL. hello egenskaper för hello attributet avspeglar hello hello UDT fysiska egenskaper. Den här klassen kan inte ärvas.

SqlUserDefinedType är ett obligatoriskt attribut för UDT-definition.

hello konstruktor för hello klass:  

* SqlUserDefinedTypeAttribute (typ formaterare)

* Skriv formaterare: krävs för parametern toodefine UDT-Formateraren--specifikt hello typ av hello `IFormatter` interface måste överföras här.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Vanliga UDT kräver även definitionen av hello IFormatter gränssnitt som visas i följande exempel hello:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

Hej `IFormatter` gränssnittet Serialiserar och frigör Serialiserar ett objektdiagram med hello rottyp av \<typeparamref name = ”T” >.

\<typeparam name = ”T” > Hej rottyp för hello objektet diagram tooserialize och deserialisera.

* **Deserialisera**: Frigör Serialiserar hello data på hello angivna dataströmmen och reconstitutes hello diagram över objekt.

* **Serialisera**: Serialiserar ett objekt eller diagram över objekt med hello angivna roten toohello angivna dataströmmen.

`MyType`instans: instans av typen hello.  
`IColumnWriter`skrivaren / `IColumnReader` reader: hello underliggande kolumnen dataströmmen.  
`ISerializationContext`kontexten: uppräkning som definierar en uppsättning flaggor som anger hello källan eller målet kontext för hello ström under serialiseringen.

* **Mellanliggande**: Anger den hello källa eller målet kontexten inte är beständiga arkivet.

* **Beständiga**: Anger att hello källan eller målet kontext är beständiga arkivet.

Som en vanlig C# typ, en definition av U-SQL-UDT kan omfatta åsidosättningar för operatörer som +/ == /! =. Det kan också innefatta statiska metoder. Till exempel om vi toouse denna UDT som en parameter tooa mängdfunktionen MIN U-SQL, har vi toodefine < operatorn åsidosättning.

Tidigare i den här handboken visas vi ett exempel på räkenskapsårets period identifiering från hello specifika datum i hello format Qn:Pn (Q1:P10). hello som följande exempel visar hur toodefine en anpassad skriver för räkenskapsårets tidsperioden.

Följande är ett exempel på en bakomliggande kod avsnitt med anpassade UDT och IFormatter gränssnitt:

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

hello definierade typen innehåller två tal: kvartal och månader. Operatörer == /! = / > / < och statisk metod ToString() definieras här.

Som tidigare nämnts kan användas i SELECT-uttryck UDT, men kan inte användas i OUTPUTTER-EXTRAKTOR utan anpassad serialisering. Det har antingen toobe serialiserad som en sträng med ToString() eller används tillsammans med en anpassad OUTPUTTER/EXTRAKTOR.

Nu ska vi diskutera användning av UDT. I en bakomliggande kod avsnittet ändrat vi våra GetFiscalPeriod funktionen toohello följande:

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Som du ser returnerar hello värde av våra FiscalPeriod-typen.

Här kan vi ge ett exempel på hur toouse det ytterligare i grundläggande U-SQL-skript. Det här exemplet visar olika former av UDT-anrop från U-SQL-skript.

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in hello prior SELECT.  Passing hello UDT toothis subsequent SELECT would have failed if hello UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```

Här är ett exempel på en fullständig bakgrundskod avsnitt:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Använda användardefinierade aggregeringar: UDAGG
Användardefinierade aggregeringar är aggregeringen-relaterade funktioner som inte levereras out-of-the-box med U-SQL. hello exempel kan en sammanställd tooperform anpassade matematiska beräkningar, sträng sammanfogningar ändringar med strängar, och så vidare.

hello användardefinierade sammanställd basklass definition är följande:

```c#
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** anger att hello typ ska registreras som en användardefinierad funktion. Den här klassen kan inte ärvas.

Attributet SqlUserDefinedType **valfria** UDAGG definition.


hello basklass kan du toopass tre abstrakta parametrar: två som indataparametrar och en som hello resultat. hello-datatyper kan variera och definieras under klassarv.

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init** anropar en gång för varje grupp under beräkningen. Det ger en initieringsrutinen för varje grupp aggregering.  
* **Ackumulera** körs en gång för varje värde. Det ger hello huvudsakliga funktioner för hello aggregering algoritmen. Det kan vara används tooaggregate värden med olika datatyper som definieras under klassarv. Använder två parametrar av varierande datatyper.
* **Avsluta** körs en gång per aggregering grupp hello slutet av bearbetning toooutput hello resultatet för varje grupp.

toodeclare rätt indata och utdata-datatyperna, Använd hello klassdefinitionen enligt följande:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: Första parametern tooaccumulate
* T2: Första parametern tooaccumulate
* TResult: Returtypen för Avsluta

Exempel:

```
public class GuidAggregate : IAggregate<string, int, int>
```

eller

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>Använd UDAGG i U-SQL
toouse UDAGG, först definiera den i den bakomliggande koden eller refereras till från befintliga hello-programmering DLL enligt tidigare diskussion.

Använd sedan hello följande syntax:

```
AGG<UDAGG_functionname>(param1,param2)
```

Här är ett exempel på UDAGG:

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

Och grundläggande U-SQL-skript:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

I detta scenario användningsfall sammanfoga vi klass GUID för hello specifika användare.

## <a name="use-user-defined-objects-udo"></a>Använda användardefinierade objekt: UDO
U-SQL kan toodefine anpassade programmering objekt, vilket kallas användardefinierade objekt eller UDO.

hello nedan följer en lista över UDO i U-SQL:

* Användardefinierade juicepressar
    * Extrahera rad för rad
    * Använda tooimplement data extrahering från anpassade strukturerade filer

* Användardefinierade outputters
    * Utdata rad för rad
    * Använda toooutput anpassade datatyper eller anpassade filformat

* Användardefinierade processorer
    * Tar en rad och skapar en rad
    * Använda tooreduce hello antalet kolumner eller skapa nya kolumner med värden som härleds från en befintlig kolumn

* Användardefinierade appliers
    * Tar en rad och skapar 0 toon rader
    * Används med yttre/mellan Använd

* Användardefinierade combiners
    * Kombinerar raduppsättningar--användardefinierade kopplingar

* Användardefinierade förminskningsapparater
    * Tar n rader och skapar en rad
    * Används tooreduce hello antalet rader

UDO kallas vanligtvis explicit i U-SQL-skript som en del av hello följande U-SQL-uttryck:

* EXTRAHERA
* UTDATA
* PROCESSEN
* KOMBINERA
* MINSKA

> [!NOTE]  
> UDOS är begränsad tooconsume 0,5 Gb minne.  Den här begränsningen minne gäller inte toolocal körningar.

## <a name="use-user-defined-extractors"></a>Använda användardefinierade juicepressar
U-SQL kan tooimport externa data med hjälp av en EXTRAHERA-instruktion. Uttrycket EXTRAHERA kan använda inbyggda UDO juicepressar:  

* *Extractors.Text()*: ger extrahering från avgränsade textfiler i olika kodningar.

* *Extractors.Csv()*: ger extrahering från CSV-filer (CSV) av olika kodningar.

* *Extractors.Tsv()*: ger extrahering från fliken med kommaseparerade värden (TVS) filer med olika kodningar.

Det kan vara användbart toodevelop anpassade extraktor. Det kan vara användbart när data importeras om vi vill toodo någon hello följande uppgifter:

* Ändra inkommande data genom att dela kolumner och ändra enskilda värden. hello PROCESSOR funktioner är bättre om du vill kombinera kolumner.
* Parsa Ostrukturerade data, till exempel webbsidor och e-postmeddelanden eller delvis Ostrukturerade data, till exempel XML/JSON.
* Tolka data i kodning stöds inte.

toodefine en användardefinierad extraktor eller Undanta, behöver vi toocreate en `IExtractor` gränssnitt. Alla indata parametrar toohello extraktor som kolumn/rad avgränsare och kodning, måste toobe som definierats i hello konstruktorn av hello-klassen. Hej `IExtractor` interface måste även innehålla en definition för hello `IEnumerable<IRow>` åsidosätta på följande sätt:

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

Hej **SqlUserDefinedExtractor** attributet anger att hello typ ska registreras som en användardefinierad extraktor. Den här klassen kan inte ärvas.

SqlUserDefinedExtractor är ett valfritt attribut för Undanta definition. Det använda toodefine AtomicFileProcessing egenskapen för hello Undanta objekt.

* bool AtomicFileProcessing   

* **SANT** = anger att den här extraktor kräver atomiska indatafiler (JSON, XML,...)
* **FALSKT** = anger den här extraktor kan hantera delade / distribuerade filer (CSV, SEQ,...)

hello huvudsakliga Undanta programmering objekt är **inkommande** och **utdata**. Hej indataobjektet är används tooenumerate indata som `IUnstructuredReader`. hello utdata objektet är används tooset utdata till följd av hello extraktor aktivitet.

hello indata nås via `System.IO.Stream` och `System.IO.StreamReader`.

För indatakolumnerna uppräkningen dela vi först hello Indataströmmen med hjälp av en avgränsare för en rad.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Sedan ytterligare delats inkommande raden kolumnen.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

tooset utdata använder vi hello `output.Set` metod.

Det är viktigt toounderstand som hello anpassade extraktor endast matar ut kolumner och värden som definieras med hello utdata. Ange metodanrop.

```
output.Set<string>(count, part);
```

hello faktiska extraktor utdata utlöses genom att anropa `yield return output.AsReadOnly();`.

Följande är hello extraktor exempel:

```
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read hello input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split hello input by hello column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert tooUPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep hello rest of hello columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

I detta scenario användningsfall hello extraktor återskapar hello GUID för ”guid” kolumn och konverterar hello värdena för ”användare” kolumnen tooupper fallet. Anpassade juicepressar kan ge mer komplicerad resultat genom att parsa indata och ändrar den.

Nedan följer grundläggande U-SQL-skript som använder en anpassad extraktor:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a>Använda användardefinierade outputters
Användardefinierade outputter är en annan U-SQL-UDO som gör att du tooextend inbyggd U-SQL-funktion. Liknande toohello extraktor det finns flera inbyggda outputters.

* *Outputters.Text()*: skriver data toodelimited textfiler i olika kodningar.
* *Outputters.Csv()*: skriver data toocomma med kommaseparerade värden (CSV) filer med olika kodningar.
* *Outputters.Tsv()*: skriver data tootab med kommaseparerade värden (TVS) filer med olika kodningar.

Anpassade outputter tillåter toowrite data i ett anpassat definierade format. Detta kan vara användbart för hello följande uppgifter:

* Skrivning toosemi strukturerade eller Ostrukturerade datafiler.
* Stöd för skrivning av data inte kodningar.
* Ändra utdata eller lägga till anpassade attribut.

toodefine användardefinierade outputter måste toocreate hello `IOutputter` gränssnitt.

Följande är ett grundläggande hello `IOutputter` klassen implementering:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Alla indata parametrar toohello outputter som behöver toobe som definierats i hello konstruktorn för klassen hello kolumn/rad avgränsare, kodning och så vidare. Hej `IOutputter` interface måste även innehålla en definition för `void Output` åsidosätta. hello attributet `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` (valfritt) kan anges för behandling av atomiska. Mer information finns i hello följande information.

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output`kallas för varje inkommande rad. Den returnerar hello `IUnstructuredWriter output` raduppsättningen.
* hello konstruktorn klassen används toopass parametrar toohello användardefinierade outputter.
* `Close`används toooptionally åsidosätta toorelease dyra tillstånd eller avgöra när hello sista raden har skrivits.

**SqlUserDefinedOutputter** attributet anger att hello typ ska registreras som en användardefinierad outputter. Den här klassen kan inte ärvas.

SqlUserDefinedOutputter är ett valfritt attribut för en användardefinierad outputter definition. Den används toodefine hello AtomicFileProcessing egenskapen.

* bool AtomicFileProcessing   

* **SANT** = anger att den här outputter kräver atomiska utdatafilerna (JSON, XML,...)
* **FALSKT** = anger den här outputter kan hantera delade / distribuerade filer (CSV, SEQ,...)

hello huvudsakliga programmering objekt är **raden** och **utdata**. Hej **raden** objektet är används tooenumerate utdata som `IRow` gränssnitt. **Utdata** är används tooset toohello mål utdatafilen.

hello utdata nås via hello `IRow` gränssnitt. Utdata skickas en rad i taget.

hello enskilda värden räknas upp genom att anropa hello Get-metoden för hello IRow-gränssnittet:

```
row.Get<string>("column_name")
```

Enskilda kolumnnamn kan fastställas genom att anropa `row.Schema`:

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Den här metoden kan du toobuild en flexibel outputter för alla metadata-scheman.

hello utdata skrivs toofile med hjälp av `System.IO.StreamWriter`. hello dataströmmen är angiven för`output.BaseStrea` som en del av `IUnstructuredWriter output`.

Observera att det är viktigt tooflush hello data buffert toohello fil efter varje iteration raden. Dessutom hello `StreamWriter` objektet måste användas med hello tillgänglig attributet aktiverad (standard) och hello **med** nyckelordet:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Annars kan anropa uttryckligen Flush() metoden efter varje iteration. Detta visar vi i följande exempel hello.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Ange sidhuvuden och sidfötter för användardefinierade outputter
tooset ett huvud använda enstaka upprepning körning flödet.

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

Hej koden i hello först `if (isHeaderRow)` block körs bara en gång.

Använd för hello sidfot hello referens toohello instans av `System.IO.Stream` objekt (`output.BaseStream`). Skriva hello sidfot i hello Close() metod för hello `IOutputter` gränssnitt.  (Mer information finns i följande exempel hello.)

Följande är ett exempel på en användardefinierad outputter:

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // hello Close method is used toowrite hello footer toohello file. It's executed only once, after all rows
    public override void Close().
    {
    //Reference tooIO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization tooenumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required toomatch hello distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference toohello instance of hello IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define hello factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

Och grundläggande U-SQL-skript:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    too@output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Det här är en HTML-outputter som skapar en HTML-fil med tabelldata.

### <a name="call-outputter-from-u-sql-base-script"></a>Anropa outputter från grundläggande U-SQL-skript
toocall en anpassad outputter från hello grundläggande U-SQL-skript, hello ny instans av hello outputter objektet har toobe skapas.

```sql
OUTPUT @rs0 too@output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Skapa en instans av hello tooavoid objekt i grundläggande skript kan vi skapa en funktion omslutning som visas i det tidigare exemplet:

```c#
        // Define hello factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

I det här fallet hello ursprungliga anropet ser ut så hello följande:

```
OUTPUT @rs0 
too@output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Använda användardefinierade processorer
Användardefinierade är UDP, en typ av U-SQL-UDO som gör att du tooprocess hello inkommande rader genom att använda programmeringsfunktioner. UDP möjliggör toocombine kolumner, ändra värdena och lägga till nya kolumner, om det behövs. I princip hjälper tooprocess dataelement för tooproduce krävs en raduppsättning.

toodefine en UDP, behöver vi toocreate en `IProcessor` gränssnittet med hello `SqlUserDefinedProcessor` attribut som är valfritt för UDP.

Det här gränssnittet ska innehålla hello definitionen för hello `IRow` gränssnittet raduppsättningen åsidosätta, enligt följande exempel hello:

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor** anger att hello typ ska registreras som en användardefinierad processor. Den här klassen kan inte ärvas.

Hej SqlUserDefinedProcessor attributet **valfria** för UDP-definition.

hello huvudsakliga programmering objekt är **inkommande** och **utdata**. hello indataobjektet är att använda tooenumerate indatakolumnerna och utdata tooset utdata till följd av hello processoraktivitet.

För indatakolumnerna uppräkningen vi använder hello `input.Get` metod.

```
string column_name = input.Get<string>("column_name");
```

Hej parametern för `input.Get` metoden är en kolumn som skickas som en del av hello `PRODUCE` -satsen i hello `PROCESS` instruktionen på grundläggande hello U-SQL-skript. Vi behöver toouse hello rätt datatyp här.

Använd hello för utdata `output.Set` metod.

Det är viktigt toonote att anpassade producent endast matar ut kolumner och värden som definieras med hello `output.Set` metodanrop.

```
output.Set<string>("mycolumn", mycolumn);
```

hello faktiska utdata utlöses genom att anropa `return output.AsReadOnly();`.

Följande är ett exempel på processor:

```
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

I detta scenario användningsfall genererar hello processor en ny kolumn som kallas ”full_description” genom att kombinera hello befintliga kolumner – i det här fallet ”användare” i versaler och ”des”. Den genererar ett GUID och returnerar hello ursprungliga och nya GUID-värden.

Som du kan se hello föregående exempel kan du anropa C#-metoder under `output.Set` metodanrop.

Följande är ett exempel på grundläggande U-SQL-skript som använder en anpassad processor:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a>Använda användardefinierade appliers
Ett U-SQL-användardefinierade applier kan tooinvoke anpassade C#-funktionen för varje rad som returneras av hello yttre tabelluttryck i en fråga. hello rätt indata ska utvärderas för varje rad från hello vänstra indata och hello rader som genereras kombineras hello slutversionen. hello lista med kolumner som genereras av hello APPLY-operatorn är hello kombination av hello uppsättning kolumner i hello vänster och hello höger indata.

Användardefinierade applier anropas som en del av hello USQL Välj uttryck.

hello vanliga anropa toohello användardefinierade applier ser ut som följande hello:

```
SELECT …
FROM …
CROSS APPLYis used toopass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Mer information om hur du använder appliers i ett SELECT-uttryck finns [U-SQL väljer att välja mellan tillämpa och yttre tillämpa](https://msdn.microsoft.com/library/azure/mt621307.aspx).

hello användardefinierade applier basklass definition är följande:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

toodefine en användardefinierad applier måste toocreate hello `IApplier` gränssnittet med hello [`SqlUserDefinedApplier`]-attribut som är valfri för en användardefinierad applier definition.

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Tillämpa anropas för varje rad i hello yttre tabell. Den returnerar hello `IUpdatableRow` utdata raduppsättningen.
* hello konstruktorn klassen är används toopass parametrar toohello användardefinierade applier.

**SqlUserDefinedApplier** anger att hello typ ska registreras som en användardefinierad applier. Den här klassen kan inte ärvas.

**SqlUserDefinedApplier** är **valfria** för en användardefinierad applier definition.


hello huvudsakliga programmering objekt är följande:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Inkommande raduppsättningar skickas `IRow` indata. hello utdata rader genereras som `IUpdatableRow` utdata-gränssnittet.

Enskilda kolumnnamn kan fastställas genom att anropa hello `IRow` Schema-metoden.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

tooget hello faktiska värden från hello inkommande `IRow`, vi använda hello Get()-metoden i `IRow` gränssnitt.

```
mycolumn = row.Get<int>("mycolumn")
```

Eller vi använda hello schemat kolumnnamn:

```
row.Get<int>(row.Schema[0].Name)
```

hello utdatavärden måste anges med `IUpdatableRow` utdata:

```
output.Set<int>("mycolumn", mycolumn)
```

Det är viktigt toounderstand att anpassade appliers endast utgående kolumner och värden som definieras med `output.Set` metodanrop.

hello faktiska utdata utlöses genom att anropa `yield return output.AsReadOnly();`.

hello användardefinierade applier parametrar kan skickas toohello konstruktor. Applier kan returnera en variabel antalet kolumner som behöver toobe som definierats under hello applier anrop i grundläggande U-SQL-skript.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Här är hello användardefinierade applier exempel:

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Nedan följer hello grundläggande U-SQL-skript för den här användardefinierade applier:

```
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

I den här Användarscenario, användardefinierade applier fungerar som en kommaavgränsad parser för hello bil flottan egenskaper. Det ser ut som följande hello hello indatafilen rader:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Det är en typisk tabbavgränsad TVS fil med en kolumn för egenskaper som innehåller bil egenskaper, till exempel märke och modell. Dessa egenskaper måste vara parsade toohello tabellkolumner. Hej applier som har angetts kan du också toogenerate dynamiska antal egenskaper i hello leda raduppsättningen, baserat på hello-parameter som skickas. Du kan skapa alla egenskaper eller en specifik uppsättning endast egenskaper.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

hello användardefinierade applier kan anropas som en ny instans av applier objekt:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Eller med hello anrop av en wrapper standardmetod:

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Använda användardefinierade combiners
Användardefinierade combiner eller UDC, kan du toocombine rader från vänster och höger raduppsättningar baserat på egen kod. Användardefinierade combiner används med KOMBINERA uttryck.

En combiner anropas med hello KOMBINERA uttryck som innehåller hello nödvändig information om både inkommande raduppsättningar hello, hello gruppering kolumner, hello förväntat resultat schemat och ytterligare information.

toocall en combiner i ett grundläggande U-SQL-skript, vi använda hello följande syntax:

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Mer information finns i [KOMBINERA uttryck (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).

toodefine en användardefinierad combiner måste toocreate hello `ICombiner` gränssnittet med hello [`SqlUserDefinedCombiner`]-attribut som är valfri för en användardefinierad Combiner definition.

Base `ICombiner` klassen definition:

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

Hej anpassade implementering av en `ICombiner` gränssnittet ska innehålla hello definitionen för en `IEnumerable<IRow>` kombinera åsidosättning.

```
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

Hej **SqlUserDefinedCombiner** attributet anger att hello typ ska registreras som en användardefinierad combiner. Den här klassen kan inte ärvas.

**SqlUserDefinedCombiner** är används toodefine hello Combiner egenskap. Det är ett valfritt attribut för en användardefinierad combiner definition.

CombinerMode läge

CombinerMode uppräkning kan ta hello följande värden:

* Fullständig (0) varje rad i utdata potentiellt beror på alla hello inkommande rader från vänster och höger med hello samma nyckelvärde.

* Vänster (1) varje rad i utdata beror på en inkommande rad från hello vänster (och potentiellt alla rader från hello hello med samma nyckelvärde).

* Höger (2) varje rad i utdata beror på en inkommande rad från hello rätt (och potentiellt alla rader från hello vänster med hello samma nyckelvärde).

* Inre (3) varje rad i utdata beror på en enda inmatning rad från vänster och höger med hello samma värde.

Exempel: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


hello huvudsakliga programmering objekt är:

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Inkommande raduppsättningar skickas **vänstra** och **rätt** `IRowset` typ av gränssnitt. Båda raduppsättningar måste räknas upp för bearbetning. Du kan räkna upp alla gränssnitt en gång, så att vi har tooenumerate och cachelagra den vid behov.

För cachelagring, kan vi skapa en lista\<T\> typ av minne som ett resultat av en LINQ Frågekörningen, specifikt lista <`IRow`>. hello anonym datatyp kan användas under uppräkning samt.

Se [introduktion tooLINQ frågor (C#)](https://msdn.microsoft.com/library/bb397906.aspx) för mer information om LINQ-frågor och [IEnumerable\<T\> gränssnittet](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) mer information om IEnumerable\<T\> gränssnitt.

tooget hello faktiska värden från hello inkommande `IRowset`, vi använda hello Get()-metoden i `IRow` gränssnitt.

```
mycolumn = row.Get<int>("mycolumn")
```

Enskilda kolumnnamn kan fastställas genom att anropa hello `IRow` Schema-metoden.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Eller genom att använda hello schemat kolumnnamn:

```
c# row.Get<int>(row.Schema[0].Name)
```

hello allmän uppräkning med LINQ ser ut så hello följande:

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Efter att räkna upp båda raduppsättningar vi tooloop via alla rader. För varje rad i hello vänstra raduppsättningen vi toofind alla rader som uppfyller våra combiner hello villkor.

hello utdatavärden måste anges med `IUpdatableRow` utdata.

```
output.Set<int>("mycolumn", mycolumn)
```

hello faktiska utdata utlöses genom att anropa för`yield return output.AsReadOnly();`.

Följande är ett exempel på combiner:

```
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

I detta scenario användningsfall skapar vi en analytics-rapport för hello återförsäljare. hello målet är toofind alla produkter som kostar mer än 20 000 och att sälja via hello webbplats snabbare än via hello reguljära återförsäljare inom ett bestämt tidsintervall.

Här är hello grundläggande U-SQL-skript. Du kan jämföra hello logik mellan en vanlig koppling och en combiner:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 too@output_file1 USING Outputters.Tsv();
OUTPUT @rs2 too@output_file2 USING Outputters.Tsv();
```

En användardefinierad combiner kan anropas som en ny instans av hello applier objekt:

```
USING new MyNameSpace.MyCombiner();
```


Eller med hello anrop av en wrapper standardmetod:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Använda användardefinierade förminskningsapparater

U-SQL kan du toowrite anpassade raduppsättningen förminskningsapparater i C# med hjälp av hello användardefinierade operatorn utökningsbarhet framework och implementera ett IReducer gränssnitt.

Användardefinierade reducer eller UDR, kan vara används tooeliminate onödiga raderna under Extraheringen av data (importera). Den kan även använda toomanipulate och utvärdera rader och kolumner. Baserat på logik för programmering, kan det också definiera vilka rader måste toobe extraheras.

toodefine en UDR-klass vi behöver toocreate en `IReducer` gränssnitt med en valfri `SqlUserDefinedReducer` attribut.

Den här klassen-gränssnittet ska innehålla en definition för hello `IEnumerable` gränssnittet raduppsättningen åsidosätta.

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

Hej **SqlUserDefinedReducer** attributet anger att hello typ ska registreras som en användardefinierad reducer. Den här klassen kan inte ärvas.
**SqlUserDefinedReducer** är ett valfritt attribut för en användardefinierad reducer definition. Den används toodefine IsRecursive egenskapen.

* bool IsRecursive    
* **SANT** = anger om den här Reducer är idempotent

hello huvudsakliga programmering objekt är **inkommande** och **utdata**. Hej indataobjektet är används tooenumerate inkommande rader. Utdata är används tooset utdata rader på grund av att minska aktivitet.

För inkommande rader uppräkningen vi använder hello `Row.Get` metod.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

Hej parametern för hello `Row.Get` metoden är en kolumn som skickas som en del av hello `PRODUCE` objektklass hello `REDUCE` instruktionen på grundläggande hello U-SQL-skript. Vi behöver toouse hello korrekta data skriver samt.

Använd hello för utdata `output.Set` metod.

Det är viktigt toounderstand som anpassade reducer endast utdata värden som definieras med hello `output.Set` metodanrop.

```
output.Set<string>("mycolumn", guid);
```

hello faktiska reducer utdata utlöses genom att anropa `yield return output.AsReadOnly();`.

Följande är ett exempel på reducer:

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

I detta scenario användningsfall hello reducer hoppar över rader med ett tomt användarnamn. För varje rad i raduppsättningen läser varje obligatorisk kolumn, därefter utvärderar hello tidslängd hello användarnamn. Den matar ut hello faktiska raden bara om värdet för användarnamnet är större än 0.

Nedan följer grundläggande U-SQL-skript som använder en anpassad reducer:

```
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```
