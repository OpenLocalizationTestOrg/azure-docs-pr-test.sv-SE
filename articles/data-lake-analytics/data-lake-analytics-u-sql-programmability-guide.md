---
title: "U-SQL-programmering guide för Azure Data Lake | Microsoft Docs"
description: "Mer information om en uppsättning tjänster i Azure Data Lake som gör att du kan skapa en molnbaserad stordataplattform."
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
ms.openlocfilehash: e4e298475d7be7d51c8bd55be498371ed6ce77a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="u-sql-programmability-guide"></a><span data-ttu-id="45416-103">U-SQL-programmering guide</span><span class="sxs-lookup"><span data-stu-id="45416-103">U-SQL programmability guide</span></span>

<span data-ttu-id="45416-104">U-SQL är ett frågespråk som är utformad för stora datatypen för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="45416-104">U-SQL is a query language that's designed for big data-type of workloads.</span></span> <span data-ttu-id="45416-105">En av de unika funktionerna i U-SQL är en kombination av SQL-liknande deklarativ språk med utökningsbarhet och programmering som tillhandahålls av C#.</span><span class="sxs-lookup"><span data-stu-id="45416-105">One of the unique features of U-SQL is the combination of the SQL-like declarative language with the extensibility and programmability that's provided by C#.</span></span> <span data-ttu-id="45416-106">I den här handboken inriktas på utökningsbarhet och U-SQL-språket som är aktiverad som C#-programmering.</span><span class="sxs-lookup"><span data-stu-id="45416-106">In this guide, we concentrate on the extensibility and programmability of the U-SQL language that's enabled by C#.</span></span>

## <a name="requirements"></a><span data-ttu-id="45416-107">Krav</span><span class="sxs-lookup"><span data-stu-id="45416-107">Requirements</span></span>

<span data-ttu-id="45416-108">Hämta och installera [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="45416-108">Download and install [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="get-started-with-u-sql"></a><span data-ttu-id="45416-109">Kom igång med U-SQL</span><span class="sxs-lookup"><span data-stu-id="45416-109">Get started with U-SQL</span></span>  

<span data-ttu-id="45416-110">Nu ska vi titta på följande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="45416-110">Let’s look at the following U-SQL script:</span></span>

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

<span data-ttu-id="45416-111">Den definierar en raduppsättning kallas @a och skapar en raduppsättning kallas @results från @a.</span><span class="sxs-lookup"><span data-stu-id="45416-111">It defines a RowSet called @a and creates a RowSet called @results from @a.</span></span>

## <a name="c-types-and-expressions-in-u-sql-script"></a><span data-ttu-id="45416-112">C#-typer och uttryck i U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="45416-112">C# types and expressions in U-SQL script</span></span>

<span data-ttu-id="45416-113">Ett U-SQL-uttryck är ett C#-uttryck i kombination med U-SQL logiska operationer sådana `AND`, `OR`, och `NOT`.</span><span class="sxs-lookup"><span data-stu-id="45416-113">A U-SQL Expression is a C# expression combined with U-SQL logical operations such `AND`, `OR`, and `NOT`.</span></span> <span data-ttu-id="45416-114">U-SQL-uttryck kan användas med SELECT-, EXTRAHERA, där, med, gruppera efter och DEKLARERA.</span><span class="sxs-lookup"><span data-stu-id="45416-114">U-SQL Expressions can be used with SELECT, EXTRACT, WHERE, HAVING, GROUP BY and DECLARE.</span></span>

<span data-ttu-id="45416-115">Följande skript Parsar till exempel en sträng ett DateTime-värde i SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="45416-115">For example, the following script parses a string a DateTime value in the SELECT clause.</span></span>

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

<span data-ttu-id="45416-116">Följande skript Parsar en sträng ett DateTime-värde i en DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="45416-116">The following script parses a string a DateTime value in a DECLARE statement.</span></span>

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a><span data-ttu-id="45416-117">Använd C#-uttryck för datatypkonverteringar</span><span class="sxs-lookup"><span data-stu-id="45416-117">Use C# expressions for data type conversions</span></span>
<span data-ttu-id="45416-118">Exemplet nedan visar hur du kan göra en konvertering för datetime-data med hjälp av C#-uttryck.</span><span class="sxs-lookup"><span data-stu-id="45416-118">The following example demonstrates how you can do a datetime data conversion by using C# expressions.</span></span> <span data-ttu-id="45416-119">I det här scenariot konverteras datetime strängdata till standard datetime med Tidformatet för midnatt 00:00:00.</span><span class="sxs-lookup"><span data-stu-id="45416-119">In this particular scenario, string datetime data is converted to standard datetime with midnight 00:00:00 time notation.</span></span>

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a><span data-ttu-id="45416-120">Använd C#-uttryck för dagens datum</span><span class="sxs-lookup"><span data-stu-id="45416-120">Use C# expressions for today’s date</span></span>
<span data-ttu-id="45416-121">Hämta dagens datum, använder vi följande C#-uttryck:</span><span class="sxs-lookup"><span data-stu-id="45416-121">To pull today’s date, we can use the following C# expression:</span></span>

```
DateTime.Now.ToString("M/d/yyyy")
```

<span data-ttu-id="45416-122">Här är ett exempel på hur du använder det här uttrycket i ett skript:</span><span class="sxs-lookup"><span data-stu-id="45416-122">Here's an example of how to use this expression in a script:</span></span>

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



## <a name="using-net-assemblies"></a><span data-ttu-id="45416-123">Med hjälp av .NET-sammansättningar</span><span class="sxs-lookup"><span data-stu-id="45416-123">Using .NET assemblies</span></span>
<span data-ttu-id="45416-124">U-SQL-modellen för utökning beroende kraftigt möjligheten att lägga till anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="45416-124">U-SQL’s extensibility model relies heavily on the ability to add custom code.</span></span> <span data-ttu-id="45416-125">För närvarande ger U-SQL dig enkelt sätt att lägga till egna Microsoft. NET-baserade kod (särskilt C#).</span><span class="sxs-lookup"><span data-stu-id="45416-125">Currently, U-SQL provides you with easy ways to add your own Microsoft .NET-based code (in particular, C#).</span></span> <span data-ttu-id="45416-126">Du kan också lägga till egen kod som skrivs i andra .NET-språk, till exempel VB.NET eller F #.</span><span class="sxs-lookup"><span data-stu-id="45416-126">However, you can also add custom code that's written in other .NET languages, such as VB.NET or F#.</span></span> 

### <a name="register-a-net-assembly"></a><span data-ttu-id="45416-127">Registrera en .NET-sammansättning</span><span class="sxs-lookup"><span data-stu-id="45416-127">Register a .NET assembly</span></span>

<span data-ttu-id="45416-128">Använd instruktionen CREATE ASSEMBLY för att placera en .NET-sammansättning i en U-SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="45416-128">Use the CREATE ASSEMBLY statement to place a .NET assembly into a U-SQL Database.</span></span> <span data-ttu-id="45416-129">När en sammansättning i en databas kan kan U-SQL-skript använda dessa sammansättningar med hjälp av instruktionen REFERENSSAMMANSÄTTNING.</span><span class="sxs-lookup"><span data-stu-id="45416-129">Once an assembly is in a database, U-SQL scripts can use those assemblies by using the REFERENCE ASSEMBLY statement.</span></span> 

<span data-ttu-id="45416-130">Följande kod visar hur du registrerar en sammansättning:</span><span class="sxs-lookup"><span data-stu-id="45416-130">The following code shows how to register an assembly:</span></span>

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

<span data-ttu-id="45416-131">Följande kod visar hur du refererar till en sammansättning:</span><span class="sxs-lookup"><span data-stu-id="45416-131">The following code shows how to reference an assembly:</span></span>

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

<span data-ttu-id="45416-132">Läs den [sammansättningen instruktioner](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) som omfattar det här avsnittet i detalj.</span><span class="sxs-lookup"><span data-stu-id="45416-132">Consult the [assembly registration instructions](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) that covers this topic in greater detail.</span></span>


### <a name="use-assembly-versioning"></a><span data-ttu-id="45416-133">Använd sammansättningen versionshantering</span><span class="sxs-lookup"><span data-stu-id="45416-133">Use assembly versioning</span></span>
<span data-ttu-id="45416-134">U-SQL används för närvarande, .NET Framework version 4.5.</span><span class="sxs-lookup"><span data-stu-id="45416-134">Currently, U-SQL uses the .NET Framework version 4.5.</span></span> <span data-ttu-id="45416-135">Kontrollera att din egen sammansättningar är kompatibla med den här versionen av körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="45416-135">So ensure that your own assemblies are compatible with that version of the runtime.</span></span>

<span data-ttu-id="45416-136">Som tidigare nämnts U-SQL kör kod i en 64-bitars (x 64)-format.</span><span class="sxs-lookup"><span data-stu-id="45416-136">As mentioned earlier, U-SQL runs code in a 64-bit (x64) format.</span></span> <span data-ttu-id="45416-137">Kontrollera så att din kod kompileras om du vill köra x64.</span><span class="sxs-lookup"><span data-stu-id="45416-137">So make sure that your code is compiled to run on x64.</span></span> <span data-ttu-id="45416-138">Annars felmeddelande felaktigt format visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="45416-138">Otherwise you get the incorrect format error shown earlier.</span></span>

<span data-ttu-id="45416-139">Varje överförs sammansättningen DLL-filen och resursfilen som en annan runtime, en inbyggd sammansättning eller en .config-fil kan innehålla högst 400 MB.</span><span class="sxs-lookup"><span data-stu-id="45416-139">Each uploaded assembly DLL and resource file, such as a different runtime, a native assembly, or a config file, can be at most 400 MB.</span></span> <span data-ttu-id="45416-140">Den totala storleken på distribuerade resurser via DISTRIBUERA resurs eller via referenser till sammansättningar och deras ytterligare filer får inte överstiga 3 GB.</span><span class="sxs-lookup"><span data-stu-id="45416-140">The total size of deployed resources, either via DEPLOY RESOURCE or via references to assemblies and their additional files, cannot exceed 3 GB.</span></span>

<span data-ttu-id="45416-141">Slutligen, Observera att varje U-SQL-databas kan bara innehålla en version av alla angivna sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="45416-141">Finally, note that each U-SQL database can only contain one version of any given assembly.</span></span> <span data-ttu-id="45416-142">Om du behöver både 7 och versionen 8 NewtonSoft Json.Net bibliotekets måste registrera dem i två olika databaser.</span><span class="sxs-lookup"><span data-stu-id="45416-142">For example, if you need both version 7 and version 8 of the NewtonSoft Json.Net library, you need to register them in two different databases.</span></span> <span data-ttu-id="45416-143">Varje skript kan bara hänvisa till en version av en angiven sammansättning DLL-filen.</span><span class="sxs-lookup"><span data-stu-id="45416-143">Furthermore, each script can only refer to one version of a given assembly DLL.</span></span> <span data-ttu-id="45416-144">I detta avseende följer U-SQL C# sammansättningen hantering och versionshantering semantiken.</span><span class="sxs-lookup"><span data-stu-id="45416-144">In this respect, U-SQL follows the C# assembly management and versioning semantics.</span></span>


## <a name="use-user-defined-functions-udf"></a><span data-ttu-id="45416-145">Använda användardefinierade funktioner: UDF</span><span class="sxs-lookup"><span data-stu-id="45416-145">Use user-defined functions: UDF</span></span>
<span data-ttu-id="45416-146">U-SQL-användardefinierade funktioner eller UDF, programmerar rutiner som accepterar parametrar, utföra en åtgärd (till exempel en komplicerad beräkning) och returnerar resultatet av den åtgärden som ett-värde.</span><span class="sxs-lookup"><span data-stu-id="45416-146">U-SQL user-defined functions, or UDF, are programming routines that accept parameters, perform an action (such as a complex calculation), and return the result of that action as a value.</span></span> <span data-ttu-id="45416-147">Returvärdet för UDF kan endast vara en enskild skalär.</span><span class="sxs-lookup"><span data-stu-id="45416-147">The return value of UDF can only be a single scalar.</span></span> <span data-ttu-id="45416-148">U-SQL-UDF kan anropas i grundläggande U-SQL-skript som andra C# skalära funktioner.</span><span class="sxs-lookup"><span data-stu-id="45416-148">U-SQL UDF can be called in U-SQL base script like any other C# scalar function.</span></span>

<span data-ttu-id="45416-149">Vi rekommenderar att du initiera U-SQL-användardefinierade funktioner som **offentliga** och **statiska**.</span><span class="sxs-lookup"><span data-stu-id="45416-149">We recommend that you initialize U-SQL user-defined functions as **public** and **static**.</span></span>

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

<span data-ttu-id="45416-150">Först ska vi titta på det enkelt exemplet på hur du skapar en UDF.</span><span class="sxs-lookup"><span data-stu-id="45416-150">First let’s look at the simple example of creating a UDF.</span></span>

<span data-ttu-id="45416-151">I detta scenario användningsfall måste fastställa räkenskapsårets perioden, inklusive räkenskapskvartal och räkenskapsmånad på den första inloggningen för den angivna användaren.</span><span class="sxs-lookup"><span data-stu-id="45416-151">In this use-case scenario, we need to determine the fiscal period, including the fiscal quarter and fiscal month of the first sign-in for the specific user.</span></span> <span data-ttu-id="45416-152">Räkenskapsårets första månaden på året i vårt scenario är juni.</span><span class="sxs-lookup"><span data-stu-id="45416-152">The first fiscal month of the year in our scenario is June.</span></span>

<span data-ttu-id="45416-153">Om du vill beräkna räkenskapsårets period, introducerar vi följande C#-funktion:</span><span class="sxs-lookup"><span data-stu-id="45416-153">To calculate fiscal period, we introduce the following C# function:</span></span>

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

<span data-ttu-id="45416-154">Bara beräknar räkenskapsmånad och kvartal och returnerar ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="45416-154">It simply calculates fiscal month and quarter and returns a string value.</span></span> <span data-ttu-id="45416-155">Juni använda en månad efter de första räkenskapskvartal vi ”Q1:P1”.</span><span class="sxs-lookup"><span data-stu-id="45416-155">For June, the first month of the first fiscal quarter, we use "Q1:P1".</span></span> <span data-ttu-id="45416-156">Vi använder ”Q1:P2” för juli och så vidare.</span><span class="sxs-lookup"><span data-stu-id="45416-156">For July, we use "Q1:P2", and so on.</span></span>

<span data-ttu-id="45416-157">Detta är en vanlig C#-funktion som vi ska använda i vår U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="45416-157">This is a regular C# function that we are going to use in our U-SQL project.</span></span>

<span data-ttu-id="45416-158">Här är hur avsnittet bakomliggande kod visas i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="45416-158">Here is how the code-behind section looks in this scenario:</span></span>

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

<span data-ttu-id="45416-159">Nu det dags att anropa funktionen från grundläggande U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="45416-159">Now we are going to call this function from the base U-SQL script.</span></span> <span data-ttu-id="45416-160">För att göra detta, måste vi du ange ett fullständigt kvalificerat namn för funktionen, inklusive namnområdet, som i det här fallet är NameSpace.Class.Function(parameter).</span><span class="sxs-lookup"><span data-stu-id="45416-160">To do this, we have to provide a fully qualified name for the function, including the namespace, which in this case is NameSpace.Class.Function(parameter).</span></span>

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

<span data-ttu-id="45416-161">Följande är det faktiska grundläggande U-SQL-skriptet:</span><span class="sxs-lookup"><span data-stu-id="45416-161">Following is the actual U-SQL base script:</span></span>

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
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="45416-162">Nedan följer utdatafilen för körning av skript:</span><span class="sxs-lookup"><span data-stu-id="45416-162">Following is the output file of the script execution:</span></span>

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

<span data-ttu-id="45416-163">Det här exemplet visar en enkel användning av infogade UDF i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="45416-163">This example demonstrates a simple usage of inline UDF in U-SQL.</span></span>

### <a name="keep-state-between-udf-invocations"></a><span data-ttu-id="45416-164">Behåll tillstånd mellan UDF-anrop</span><span class="sxs-lookup"><span data-stu-id="45416-164">Keep state between UDF invocations</span></span>
<span data-ttu-id="45416-165">U-SQL C#-programmering objekt kan vara mer förfinade använder interaktivitet via bakomliggande kod globala variabler.</span><span class="sxs-lookup"><span data-stu-id="45416-165">U-SQL C# programmability objects can be more sophisticated, utilizing interactivity through the code-behind global variables.</span></span> <span data-ttu-id="45416-166">Nu ska vi titta på följande användningsfall affärsscenariot.</span><span class="sxs-lookup"><span data-stu-id="45416-166">Let’s look at the following business use-case scenario.</span></span>

<span data-ttu-id="45416-167">Användare kan växla mellan av interna program i stora organisationer.</span><span class="sxs-lookup"><span data-stu-id="45416-167">In large organizations, users can switch between varieties of internal applications.</span></span> <span data-ttu-id="45416-168">Det kan vara Microsoft Dynamics CRM, PowerBI och så vidare.</span><span class="sxs-lookup"><span data-stu-id="45416-168">These can include Microsoft Dynamics CRM, PowerBI, and so on.</span></span> <span data-ttu-id="45416-169">Kunder kan också använda en telemetri analys av hur användarna växlar mellan olika program, vilka användningstrender är, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="45416-169">Customers might want to apply a telemetry analysis of how users switch between different applications, what the usage trends are, and so on.</span></span> <span data-ttu-id="45416-170">Målet för företag är att optimera användning av programmet.</span><span class="sxs-lookup"><span data-stu-id="45416-170">The goal for the business is to optimize application usage.</span></span> <span data-ttu-id="45416-171">De kan också kombinera olika program eller specifika inloggning rutiner.</span><span class="sxs-lookup"><span data-stu-id="45416-171">They also might want to combine different applications or specific sign-on routines.</span></span>

<span data-ttu-id="45416-172">För att åstadkomma detta behöver vi fastställa sessions-ID. och fördröjning mellan den sista sessionen som inträffat.</span><span class="sxs-lookup"><span data-stu-id="45416-172">To achieve this goal, we have to determine session IDs and lag time between the last session that occurred.</span></span>

<span data-ttu-id="45416-173">Vi behöver söka efter en tidigare inloggning och tilldela sedan den här inloggningen till alla sessioner som skapas för samma program.</span><span class="sxs-lookup"><span data-stu-id="45416-173">We need to find a previous sign-in and then assign this sign-in to all sessions that are being generated to the same application.</span></span> <span data-ttu-id="45416-174">Den första kontrollen är att inte grundläggande U-SQL-skript kan vi använda beräkningar över redan beräknade kolumner med funktionen LAG.</span><span class="sxs-lookup"><span data-stu-id="45416-174">The first challenge is that U-SQL base script doesn't allow us to apply calculations over already-calculated columns with LAG function.</span></span> <span data-ttu-id="45416-175">Andra utmaningen är att vi behöver ha viss session för alla sessioner inom samma tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="45416-175">The second challenge is that we have to keep the specific session for all sessions within the same time period.</span></span>

<span data-ttu-id="45416-176">Lös problemet, vi använder en global variabel inuti en bakomliggande kod: `static public string globalSession;`.</span><span class="sxs-lookup"><span data-stu-id="45416-176">To solve this problem, we use a global variable inside a code-behind section: `static public string globalSession;`.</span></span>

<span data-ttu-id="45416-177">Den globala variabeln används för hela raduppsättningen under våra körning av skript.</span><span class="sxs-lookup"><span data-stu-id="45416-177">This global variable is applied to the entire rowset during our script execution.</span></span>

<span data-ttu-id="45416-178">Här är avsnittet bakomliggande kod i vårt U-SQL-program:</span><span class="sxs-lookup"><span data-stu-id="45416-178">Here is the code-behind section of our U-SQL program:</span></span>

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

<span data-ttu-id="45416-179">Det här exemplet visar den globala variabeln `static public string globalSession;` används inuti den `getStampUserSession` funktionen och få initieras på nytt varje gång parametern Session ändras.</span><span class="sxs-lookup"><span data-stu-id="45416-179">This example shows the global variable `static public string globalSession;` used inside the `getStampUserSession` function and getting reinitialized each time the Session parameter is changed.</span></span>

<span data-ttu-id="45416-180">Grundläggande U-SQL-skriptet är följande:</span><span class="sxs-lookup"><span data-stu-id="45416-180">The U-SQL base script is as follows:</span></span>

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
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

<span data-ttu-id="45416-181">Funktionen `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` anropas här under andra minne raduppsättningen beräkningen.</span><span class="sxs-lookup"><span data-stu-id="45416-181">Function `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` is called here during the second memory rowset calculation.</span></span> <span data-ttu-id="45416-182">Passerar den `UserSessionTimestamp` kolumn och returnerar värdet tills `UserSessionTimestamp` har ändrats.</span><span class="sxs-lookup"><span data-stu-id="45416-182">It passes the `UserSessionTimestamp` column and returns the value until `UserSessionTimestamp` has changed.</span></span>

<span data-ttu-id="45416-183">Utdatafilen är följande:</span><span class="sxs-lookup"><span data-stu-id="45416-183">The output file is as follows:</span></span>

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

<span data-ttu-id="45416-184">Det här exemplet visas en mer komplicerad användningsfall scenario där vi använder en global variabel inuti en bakomliggande kod som gäller för hela minne raduppsättningen.</span><span class="sxs-lookup"><span data-stu-id="45416-184">This example demonstrates a more complicated use-case scenario in which we use a global variable inside a code-behind section that's applied to the entire memory rowset.</span></span>

## <a name="use-user-defined-types-udt"></a><span data-ttu-id="45416-185">Använda användardefinierade typer: UDT</span><span class="sxs-lookup"><span data-stu-id="45416-185">Use user-defined types: UDT</span></span>
<span data-ttu-id="45416-186">Användardefinierade typer eller UDT, är en annan programmeringsfunktionen av U-SQL.</span><span class="sxs-lookup"><span data-stu-id="45416-186">User-defined types, or UDT, is another programmability feature of U-SQL.</span></span> <span data-ttu-id="45416-187">U-SQL-UDT fungerar som en vanlig C# användardefinierad datatyp.</span><span class="sxs-lookup"><span data-stu-id="45416-187">U-SQL UDT acts like a regular C# user-defined type.</span></span> <span data-ttu-id="45416-188">C# är ett starkt typifierad språk som tillåter användning av inbyggda och anpassade användardefinierade typer.</span><span class="sxs-lookup"><span data-stu-id="45416-188">C# is a strongly typed language that allows the use of built-in and custom user-defined types.</span></span>

<span data-ttu-id="45416-189">U-SQL kan inte implicit serialisera eller deserialisera godtycklig UDT när UDT överförs mellan formhörnen i raduppsättningar.</span><span class="sxs-lookup"><span data-stu-id="45416-189">U-SQL cannot implicitly serialize or de-serialize arbitrary UDTs when the UDT is passed between vertices in rowsets.</span></span> <span data-ttu-id="45416-190">Det innebär att användaren måste ange ett explicit formaterare IFormatter-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="45416-190">This means that the user has to provide an explicit formatter by using the IFormatter interface.</span></span> <span data-ttu-id="45416-191">Detta ger U-SQL med serialiserat och deserialisera metoder för UDT.</span><span class="sxs-lookup"><span data-stu-id="45416-191">This provides U-SQL with the serialize and de-serialize methods for the UDT.</span></span>

> [!NOTE]
> <span data-ttu-id="45416-192">U-SQL kan inte inbyggda juicepressar outputters för närvarande serialisera och deserialisera UDT data till eller från filer även med IFormatter.</span><span class="sxs-lookup"><span data-stu-id="45416-192">U-SQL’s built-in extractors and outputters currently cannot serialize or de-serialize UDT data to or from files even with the IFormatter set.</span></span> <span data-ttu-id="45416-193">Så när du skriver UDT data till en fil med instruktionen utdata, eller läsa med en extraktor, måste du skicka den som en sträng eller byte-matris.</span><span class="sxs-lookup"><span data-stu-id="45416-193">So when you're writing UDT data to a file with the OUTPUT statement, or reading it with an extractor, you have to pass it as a string or byte array.</span></span> <span data-ttu-id="45416-194">Sedan anropa serialisering och avserialisering kod (det vill säga den UDT metoden ToString()) explicit.</span><span class="sxs-lookup"><span data-stu-id="45416-194">Then you call the serialization and deserialization code (that is, the UDT’s ToString() method) explicitly.</span></span> <span data-ttu-id="45416-195">Användardefinierade juicepressar och outputters, å andra sidan kan läsa och skriva UDT-typer.</span><span class="sxs-lookup"><span data-stu-id="45416-195">User-defined extractors and outputters, on the other hand, can read and write UDTs.</span></span>

<span data-ttu-id="45416-196">Om vi försöker använda UDT i EXTRAKTOR eller OUTPUTTER (från föregående SELECT-), som visas här:</span><span class="sxs-lookup"><span data-stu-id="45416-196">If we try to use UDT in EXTRACTOR or OUTPUTTER (out of previous SELECT), as shown here:</span></span>

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="45416-197">Vi får du följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="45416-197">We receive the following error:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

<span data-ttu-id="45416-198">Om du vill arbeta med UDT i outputter, har vi antingen serialisera den sträng med metoden ToString() eller skapa en anpassad outputter.</span><span class="sxs-lookup"><span data-stu-id="45416-198">To work with UDT in outputter, we either have to serialize it to string with the ToString() method or create a custom outputter.</span></span>

<span data-ttu-id="45416-199">Användardefinierade typer som kan för närvarande inte användas i GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="45416-199">UDTs currently cannot be used in GROUP BY.</span></span> <span data-ttu-id="45416-200">Om UDT används i GROUP BY, returneras följande fel:</span><span class="sxs-lookup"><span data-stu-id="45416-200">If UDT is used in GROUP BY, the following error is thrown:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

<span data-ttu-id="45416-201">Om du vill definiera en UDT behöver vi:</span><span class="sxs-lookup"><span data-stu-id="45416-201">To define a UDT, we have to:</span></span>

* <span data-ttu-id="45416-202">Lägg till följande namnområden:</span><span class="sxs-lookup"><span data-stu-id="45416-202">Add the following namespaces:</span></span>

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* <span data-ttu-id="45416-203">Lägg till `Microsoft.Analytics.Interfaces`, vilket krävs för UDT-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-203">Add `Microsoft.Analytics.Interfaces`, which is required for the UDT interfaces.</span></span> <span data-ttu-id="45416-204">Dessutom `System.IO` kan behövas för att definiera IFormatter-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="45416-204">In addition, `System.IO` might be needed to define the IFormatter interface.</span></span>

* <span data-ttu-id="45416-205">Definiera en användardefinierade typ med SqlUserDefinedType attribut.</span><span class="sxs-lookup"><span data-stu-id="45416-205">Define a used-defined type with SqlUserDefinedType attribute.</span></span>

<span data-ttu-id="45416-206">**SqlUserDefinedType** används för att markera en typdefinition i en sammansättning som en användardefinierad typ (UDT) i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="45416-206">**SqlUserDefinedType** is used to mark a type definition in an assembly as a user-defined type (UDT) in U-SQL.</span></span> <span data-ttu-id="45416-207">Egenskaper för attributet avspeglar UDT fysiska egenskaper.</span><span class="sxs-lookup"><span data-stu-id="45416-207">The properties on the attribute reflect the physical characteristics of the UDT.</span></span> <span data-ttu-id="45416-208">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-208">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-209">SqlUserDefinedType är ett obligatoriskt attribut för UDT-definition.</span><span class="sxs-lookup"><span data-stu-id="45416-209">SqlUserDefinedType is a required attribute for UDT definition.</span></span>

<span data-ttu-id="45416-210">Konstruktorn för klassen:</span><span class="sxs-lookup"><span data-stu-id="45416-210">The constructor of the class:</span></span>  

* <span data-ttu-id="45416-211">SqlUserDefinedTypeAttribute (typ formaterare)</span><span class="sxs-lookup"><span data-stu-id="45416-211">SqlUserDefinedTypeAttribute (type formatter)</span></span>

* <span data-ttu-id="45416-212">Skriv formaterare: krävs för parametern för att definiera en UDT-formaterare--specifikt typ av den `IFormatter` interface måste överföras här.</span><span class="sxs-lookup"><span data-stu-id="45416-212">Type formatter: Required parameter to define an UDT formatter--specifically, the type of the `IFormatter` interface must be passed here.</span></span>

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* <span data-ttu-id="45416-213">Vanliga UDT kräver även definitionen av gränssnittet IFormatter som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="45416-213">Typical UDT also requires definition of the IFormatter interface, as shown in the following example:</span></span>

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

<span data-ttu-id="45416-214">Den `IFormatter` gränssnittet Serialiserar och frigör Serialiserar ett objektdiagram med rot \<typeparamref name = ”T” >.</span><span class="sxs-lookup"><span data-stu-id="45416-214">The `IFormatter` interface serializes and de-serializes an object graph with the root type of \<typeparamref name="T">.</span></span>

<span data-ttu-id="45416-215">\<typeparam name = ”T” > rottyp för objektdiagrammet serialisera och deserialisera.</span><span class="sxs-lookup"><span data-stu-id="45416-215">\<typeparam name="T">The root type for the object graph to serialize and de-serialize.</span></span>

* <span data-ttu-id="45416-216">**Deserialisera**: Frigör Serialiserar data på den angivna dataströmmen och reconstitutes diagram över objekt.</span><span class="sxs-lookup"><span data-stu-id="45416-216">**Deserialize**: De-serializes the data on the provided stream and reconstitutes the graph of objects.</span></span>

* <span data-ttu-id="45416-217">**Serialisera**: Serialiserar ett objekt eller diagram över objekt med den angivna roten till den angivna dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="45416-217">**Serialize**: Serializes an object, or graph of objects, with the given root to the provided stream.</span></span>

<span data-ttu-id="45416-218">`MyType`instans: instans av typen.</span><span class="sxs-lookup"><span data-stu-id="45416-218">`MyType` instance: Instance of the type.</span></span>  
<span data-ttu-id="45416-219">`IColumnWriter`skrivaren / `IColumnReader` reader: den underliggande kolumn strömmen.</span><span class="sxs-lookup"><span data-stu-id="45416-219">`IColumnWriter` writer / `IColumnReader` reader: The underlying column stream.</span></span>  
<span data-ttu-id="45416-220">`ISerializationContext`kontexten: uppräkning som definierar en uppsättning flaggor som anger källan eller målet kontext för dataströmmen under serialiseringen.</span><span class="sxs-lookup"><span data-stu-id="45416-220">`ISerializationContext` context: Enum that defines a set of flags that specifies the source or destination context for the stream during serialization.</span></span>

* <span data-ttu-id="45416-221">**Mellanliggande**: Anger att kontexten källan eller målet inte är beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="45416-221">**Intermediate**: Specifies that the source or destination context is not a persisted store.</span></span>

* <span data-ttu-id="45416-222">**Beständiga**: Anger att kontexten källan eller målet är en beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="45416-222">**Persistence**: Specifies that the source or destination context is a persisted store.</span></span>

<span data-ttu-id="45416-223">Som en vanlig C# typ, en definition av U-SQL-UDT kan omfatta åsidosättningar för operatörer som +/ == /! =.</span><span class="sxs-lookup"><span data-stu-id="45416-223">As a regular C# type, a U-SQL UDT definition can include overrides for operators such as +/==/!=.</span></span> <span data-ttu-id="45416-224">Det kan också innefatta statiska metoder.</span><span class="sxs-lookup"><span data-stu-id="45416-224">It can also include static methods.</span></span> <span data-ttu-id="45416-225">Till exempel om vi ska använda den här UDT som en parameter för en mängdfunktion för U-SQL MIN vi behöver ange < operatorn åsidosättning.</span><span class="sxs-lookup"><span data-stu-id="45416-225">For example, if we are going to use this UDT as a parameter to a U-SQL MIN aggregate function, we have to define < operator override.</span></span>

<span data-ttu-id="45416-226">Tidigare i den här handboken visas vi ett exempel på räkenskapsårets period identifiering från ett visst datum i formatet Qn:Pn (Q1:P10).</span><span class="sxs-lookup"><span data-stu-id="45416-226">Earlier in this guide, we demonstrated an example for fiscal period identification from the specific date in the format Qn:Pn (Q1:P10).</span></span> <span data-ttu-id="45416-227">I följande exempel visas hur du definierar en anpassad typ för räkenskapsårets tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="45416-227">The following example shows how to define a custom type for fiscal period values.</span></span>

<span data-ttu-id="45416-228">Följande är ett exempel på en bakomliggande kod avsnitt med anpassade UDT och IFormatter gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="45416-228">Following is an example of a code-behind section with custom UDT and IFormatter interface:</span></span>

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

<span data-ttu-id="45416-229">Den angivna typen innehåller två tal: kvartal och månader.</span><span class="sxs-lookup"><span data-stu-id="45416-229">The defined type includes two numbers: quarter and month.</span></span> <span data-ttu-id="45416-230">Operatörer == /! = / > / < och statisk metod ToString() definieras här.</span><span class="sxs-lookup"><span data-stu-id="45416-230">Operators ==/!=/>/< and static method ToString() are defined here.</span></span>

<span data-ttu-id="45416-231">Som tidigare nämnts kan användas i SELECT-uttryck UDT, men kan inte användas i OUTPUTTER-EXTRAKTOR utan anpassad serialisering.</span><span class="sxs-lookup"><span data-stu-id="45416-231">As mentioned earlier, UDT can be used in SELECT expressions, but cannot be used in OUTPUTTER/EXTRACTOR without custom serialization.</span></span> <span data-ttu-id="45416-232">Det måste serialiseras som en sträng med ToString() eller användas med en anpassad OUTPUTTER/EXTRAKTOR.</span><span class="sxs-lookup"><span data-stu-id="45416-232">It either has to be serialized as a string with ToString() or used with a custom OUTPUTTER/EXTRACTOR.</span></span>

<span data-ttu-id="45416-233">Nu ska vi diskutera användning av UDT.</span><span class="sxs-lookup"><span data-stu-id="45416-233">Now let’s discuss usage of UDT.</span></span> <span data-ttu-id="45416-234">I en bakomliggande kod avsnittet ändrat vi våra GetFiscalPeriod-funktionen på följande:</span><span class="sxs-lookup"><span data-stu-id="45416-234">In a code-behind section, we changed our GetFiscalPeriod function to the following:</span></span>

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

<span data-ttu-id="45416-235">Som du ser returnerar värdet för våra FiscalPeriod-typen.</span><span class="sxs-lookup"><span data-stu-id="45416-235">As you can see, it returns the value of our FiscalPeriod type.</span></span>

<span data-ttu-id="45416-236">Här kan vi ge ett exempel på hur du använder den ytterligare i grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="45416-236">Here we provide an example of how to use it further in U-SQL base script.</span></span> <span data-ttu-id="45416-237">Det här exemplet visar olika former av UDT-anrop från U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="45416-237">This example demonstrates different forms of UDT invocation from U-SQL script.</span></span>

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

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="45416-238">Här är ett exempel på en fullständig bakgrundskod avsnitt:</span><span class="sxs-lookup"><span data-stu-id="45416-238">Here's an example of a full code-behind section:</span></span>

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

## <a name="use-user-defined-aggregates-udagg"></a><span data-ttu-id="45416-239">Använda användardefinierade aggregeringar: UDAGG</span><span class="sxs-lookup"><span data-stu-id="45416-239">Use user-defined aggregates: UDAGG</span></span>
<span data-ttu-id="45416-240">Användardefinierade aggregeringar är aggregeringen-relaterade funktioner som inte levereras out-of-the-box med U-SQL.</span><span class="sxs-lookup"><span data-stu-id="45416-240">User-defined aggregates are any aggregation-related functions that are not shipped out-of-the-box with U-SQL.</span></span> <span data-ttu-id="45416-241">Exemplet kan vara en aggregering att utföra anpassad matematiska beräkningar, sträng sammanfogningar, ändringar med strängar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="45416-241">The example can be an aggregate to perform custom math calculations, string concatenations, manipulations with strings, and so on.</span></span>

<span data-ttu-id="45416-242">Användardefinierade sammanställd basklass definitionen är följande:</span><span class="sxs-lookup"><span data-stu-id="45416-242">The user-defined aggregate base class definition is as follows:</span></span>

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

<span data-ttu-id="45416-243">**SqlUserDefinedAggregate** anger att typen ska registreras som en användardefinierad funktion.</span><span class="sxs-lookup"><span data-stu-id="45416-243">**SqlUserDefinedAggregate** indicates that the type should be registered as a user-defined aggregate.</span></span> <span data-ttu-id="45416-244">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-244">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-245">Attributet SqlUserDefinedType **valfria** UDAGG definition.</span><span class="sxs-lookup"><span data-stu-id="45416-245">SqlUserDefinedType attribute is **optional** for UDAGG definition.</span></span>


<span data-ttu-id="45416-246">Basklassen kan du skicka abstrakt tre parametrar: två som indataparametrar och en som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="45416-246">The base class allows you to pass three abstract parameters: two as input parameters and one as the result.</span></span> <span data-ttu-id="45416-247">Datatyperna är variabeln och ska definieras under klassarv.</span><span class="sxs-lookup"><span data-stu-id="45416-247">The data types are variable and should be defined during class inheritance.</span></span>

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

* <span data-ttu-id="45416-248">**Init** anropar en gång för varje grupp under beräkningen.</span><span class="sxs-lookup"><span data-stu-id="45416-248">**Init** invokes once for each group during computation.</span></span> <span data-ttu-id="45416-249">Det ger en initieringsrutinen för varje grupp aggregering.</span><span class="sxs-lookup"><span data-stu-id="45416-249">It provides an initialization routine for each aggregation group.</span></span>  
* <span data-ttu-id="45416-250">**Ackumulera** körs en gång för varje värde.</span><span class="sxs-lookup"><span data-stu-id="45416-250">**Accumulate** is executed once for each value.</span></span> <span data-ttu-id="45416-251">Den innehåller de flesta funktioner för algoritmen aggregering.</span><span class="sxs-lookup"><span data-stu-id="45416-251">It provides the main functionality for the aggregation algorithm.</span></span> <span data-ttu-id="45416-252">Den kan användas för att Aggregera värden med olika datatyper som definieras under klassarv.</span><span class="sxs-lookup"><span data-stu-id="45416-252">It can be used to aggregate values with various data types that are defined during class inheritance.</span></span> <span data-ttu-id="45416-253">Använder två parametrar av varierande datatyper.</span><span class="sxs-lookup"><span data-stu-id="45416-253">It can accept two parameters of variable data types.</span></span>
* <span data-ttu-id="45416-254">**Avsluta** körs en gång per aggregering grupp i slutet av bearbetning till utdata resultatet för varje grupp.</span><span class="sxs-lookup"><span data-stu-id="45416-254">**Terminate** is executed once per aggregation group at the end of processing to output the result for each group.</span></span>

<span data-ttu-id="45416-255">Använd klassdefinitionen enligt följande för att deklarera rätt indata och utdata datatyper:</span><span class="sxs-lookup"><span data-stu-id="45416-255">To declare correct input and output data types, use the class definition as follows:</span></span>

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* <span data-ttu-id="45416-256">T1: Den första parametern i ackumuleras</span><span class="sxs-lookup"><span data-stu-id="45416-256">T1: First parameter to accumulate</span></span>
* <span data-ttu-id="45416-257">T2: Den första parametern i ackumuleras</span><span class="sxs-lookup"><span data-stu-id="45416-257">T2: First parameter to accumulate</span></span>
* <span data-ttu-id="45416-258">TResult: Returtypen för Avsluta</span><span class="sxs-lookup"><span data-stu-id="45416-258">TResult: Return type of terminate</span></span>

<span data-ttu-id="45416-259">Exempel:</span><span class="sxs-lookup"><span data-stu-id="45416-259">For example:</span></span>

```
public class GuidAggregate : IAggregate<string, int, int>
```

<span data-ttu-id="45416-260">eller</span><span class="sxs-lookup"><span data-stu-id="45416-260">or</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a><span data-ttu-id="45416-261">Använd UDAGG i U-SQL</span><span class="sxs-lookup"><span data-stu-id="45416-261">Use UDAGG in U-SQL</span></span>
<span data-ttu-id="45416-262">Om du vill använda UDAGG definieras i bakomliggande kod eller refereras till från den befintliga programmering DLL enligt tidigare diskussion.</span><span class="sxs-lookup"><span data-stu-id="45416-262">To use UDAGG, first define it in code-behind or reference it from the existent programmability DLL as discussed earlier.</span></span>

<span data-ttu-id="45416-263">Sedan använder du följande syntax:</span><span class="sxs-lookup"><span data-stu-id="45416-263">Then use the following syntax:</span></span>

```
AGG<UDAGG_functionname>(param1,param2)
```

<span data-ttu-id="45416-264">Här är ett exempel på UDAGG:</span><span class="sxs-lookup"><span data-stu-id="45416-264">Here is an example of UDAGG:</span></span>

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

<span data-ttu-id="45416-265">Och grundläggande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="45416-265">And base U-SQL script:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

<span data-ttu-id="45416-266">I detta scenario användningsfall sammanfoga vi klass GUID för specifika användare.</span><span class="sxs-lookup"><span data-stu-id="45416-266">In this use-case scenario, we concatenate class GUIDs for the specific users.</span></span>

## <a name="use-user-defined-objects-udo"></a><span data-ttu-id="45416-267">Använda användardefinierade objekt: UDO</span><span class="sxs-lookup"><span data-stu-id="45416-267">Use user-defined objects: UDO</span></span>
<span data-ttu-id="45416-268">U-SQL kan du definiera anpassade programmering objekt, vilket kallas användardefinierade objekt eller UDO.</span><span class="sxs-lookup"><span data-stu-id="45416-268">U-SQL enables you to define custom programmability objects, which are called user-defined objects or UDO.</span></span>

<span data-ttu-id="45416-269">Följande är en lista över UDO i U-SQL:</span><span class="sxs-lookup"><span data-stu-id="45416-269">The following is a list of UDO in U-SQL:</span></span>

* <span data-ttu-id="45416-270">Användardefinierade juicepressar</span><span class="sxs-lookup"><span data-stu-id="45416-270">User-defined extractors</span></span>
    * <span data-ttu-id="45416-271">Extrahera rad för rad</span><span class="sxs-lookup"><span data-stu-id="45416-271">Extract row by row</span></span>
    * <span data-ttu-id="45416-272">Används för att implementera extrahering av data från anpassade strukturerade filer</span><span class="sxs-lookup"><span data-stu-id="45416-272">Used to implement data extraction from custom structured files</span></span>

* <span data-ttu-id="45416-273">Användardefinierade outputters</span><span class="sxs-lookup"><span data-stu-id="45416-273">User-defined outputters</span></span>
    * <span data-ttu-id="45416-274">Utdata rad för rad</span><span class="sxs-lookup"><span data-stu-id="45416-274">Output row by row</span></span>
    * <span data-ttu-id="45416-275">Används för att anpassade utdatatyper eller anpassade format</span><span class="sxs-lookup"><span data-stu-id="45416-275">Used to output custom data types or custom file formats</span></span>

* <span data-ttu-id="45416-276">Användardefinierade processorer</span><span class="sxs-lookup"><span data-stu-id="45416-276">User-defined processors</span></span>
    * <span data-ttu-id="45416-277">Tar en rad och skapar en rad</span><span class="sxs-lookup"><span data-stu-id="45416-277">Take one row and produce one row</span></span>
    * <span data-ttu-id="45416-278">Används för att minska antalet kolumner eller skapa nya kolumner med värden som härleds från en befintlig kolumn</span><span class="sxs-lookup"><span data-stu-id="45416-278">Used to reduce the number of columns or produce new columns with values that are derived from an existing column set</span></span>

* <span data-ttu-id="45416-279">Användardefinierade appliers</span><span class="sxs-lookup"><span data-stu-id="45416-279">User-defined appliers</span></span>
    * <span data-ttu-id="45416-280">Tar en rad och skapar 0 till n rader</span><span class="sxs-lookup"><span data-stu-id="45416-280">Take one row and produce 0 to n rows</span></span>
    * <span data-ttu-id="45416-281">Används med yttre/mellan Använd</span><span class="sxs-lookup"><span data-stu-id="45416-281">Used with OUTER/CROSS APPLY</span></span>

* <span data-ttu-id="45416-282">Användardefinierade combiners</span><span class="sxs-lookup"><span data-stu-id="45416-282">User-defined combiners</span></span>
    * <span data-ttu-id="45416-283">Kombinerar raduppsättningar--användardefinierade kopplingar</span><span class="sxs-lookup"><span data-stu-id="45416-283">Combines rowsets--user-defined JOINs</span></span>

* <span data-ttu-id="45416-284">Användardefinierade förminskningsapparater</span><span class="sxs-lookup"><span data-stu-id="45416-284">User-defined reducers</span></span>
    * <span data-ttu-id="45416-285">Tar n rader och skapar en rad</span><span class="sxs-lookup"><span data-stu-id="45416-285">Take n rows and produce one row</span></span>
    * <span data-ttu-id="45416-286">Används för att minska antalet rader</span><span class="sxs-lookup"><span data-stu-id="45416-286">Used to reduce the number of rows</span></span>

<span data-ttu-id="45416-287">UDO kallas vanligtvis explicit i U-SQL-skript som en del av följande U-SQL-uttryck:</span><span class="sxs-lookup"><span data-stu-id="45416-287">UDO is typically called explicitly in U-SQL script as part of the following U-SQL statements:</span></span>

* <span data-ttu-id="45416-288">EXTRAHERA</span><span class="sxs-lookup"><span data-stu-id="45416-288">EXTRACT</span></span>
* <span data-ttu-id="45416-289">UTDATA</span><span class="sxs-lookup"><span data-stu-id="45416-289">OUTPUT</span></span>
* <span data-ttu-id="45416-290">PROCESSEN</span><span class="sxs-lookup"><span data-stu-id="45416-290">PROCESS</span></span>
* <span data-ttu-id="45416-291">KOMBINERA</span><span class="sxs-lookup"><span data-stu-id="45416-291">COMBINE</span></span>
* <span data-ttu-id="45416-292">MINSKA</span><span class="sxs-lookup"><span data-stu-id="45416-292">REDUCE</span></span>

> [!NOTE]  
> <span data-ttu-id="45416-293">UDOS är begränsad för att använda 0,5 Gb minne.</span><span class="sxs-lookup"><span data-stu-id="45416-293">UDO’s are limited to consume 0.5Gb memory.</span></span>  <span data-ttu-id="45416-294">Den här minne begränsningen gäller inte för lokala körningar.</span><span class="sxs-lookup"><span data-stu-id="45416-294">This memory limitation does not apply to local executions.</span></span>

## <a name="use-user-defined-extractors"></a><span data-ttu-id="45416-295">Använda användardefinierade juicepressar</span><span class="sxs-lookup"><span data-stu-id="45416-295">Use user-defined extractors</span></span>
<span data-ttu-id="45416-296">U-SQL kan du importera externa data med hjälp av en EXTRAHERA-instruktion.</span><span class="sxs-lookup"><span data-stu-id="45416-296">U-SQL allows you to import external data by using an EXTRACT statement.</span></span> <span data-ttu-id="45416-297">Uttrycket EXTRAHERA kan använda inbyggda UDO juicepressar:</span><span class="sxs-lookup"><span data-stu-id="45416-297">An EXTRACT statement can use built-in UDO extractors:</span></span>  

* <span data-ttu-id="45416-298">*Extractors.Text()*: ger extrahering från avgränsade textfiler i olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-298">*Extractors.Text()*: Provides extraction from delimited text files of different encodings.</span></span>

* <span data-ttu-id="45416-299">*Extractors.Csv()*: ger extrahering från CSV-filer (CSV) av olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-299">*Extractors.Csv()*: Provides extraction from comma-separated value (CSV) files of different encodings.</span></span>

* <span data-ttu-id="45416-300">*Extractors.Tsv()*: ger extrahering från fliken med kommaseparerade värden (TVS) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-300">*Extractors.Tsv()*: Provides extraction from tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="45416-301">Det kan vara praktiskt att utveckla anpassade extraktor.</span><span class="sxs-lookup"><span data-stu-id="45416-301">It can be useful to develop a custom extractor.</span></span> <span data-ttu-id="45416-302">Det kan vara användbart när data importeras om vi vill göra något av följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="45416-302">This can be helpful during data import if we want to do any of the following tasks:</span></span>

* <span data-ttu-id="45416-303">Ändra inkommande data genom att dela kolumner och ändra enskilda värden.</span><span class="sxs-lookup"><span data-stu-id="45416-303">Modify input data by splitting columns and modifying individual values.</span></span> <span data-ttu-id="45416-304">PROCESSOR-funktioner är bättre om du vill kombinera kolumner.</span><span class="sxs-lookup"><span data-stu-id="45416-304">The PROCESSOR functionality is better for combining columns.</span></span>
* <span data-ttu-id="45416-305">Parsa Ostrukturerade data, till exempel webbsidor och e-postmeddelanden eller delvis Ostrukturerade data, till exempel XML/JSON.</span><span class="sxs-lookup"><span data-stu-id="45416-305">Parse unstructured data such as Web pages and emails, or semi-unstructured data such as XML/JSON.</span></span>
* <span data-ttu-id="45416-306">Tolka data i kodning stöds inte.</span><span class="sxs-lookup"><span data-stu-id="45416-306">Parse data in unsupported encoding.</span></span>

<span data-ttu-id="45416-307">Om du vill definiera en användardefinierad extraktor eller Undanta måste vi skapa en `IExtractor` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-307">To define a user-defined extractor, or UDE, we need to create an `IExtractor` interface.</span></span> <span data-ttu-id="45416-308">Alla indataparametrar till extraktor, till exempel kolumn/rad avgränsare och kodning, måste definieras i konstruktorn för klassen.</span><span class="sxs-lookup"><span data-stu-id="45416-308">All input parameters to the extractor, such as column/row delimiters, and encoding, need to be defined in the constructor of the class.</span></span> <span data-ttu-id="45416-309">Den `IExtractor` interface måste även innehålla en definition för den `IEnumerable<IRow>` åsidosätta på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="45416-309">The `IExtractor`  interface should also contain a definition for the `IEnumerable<IRow>` override as follows:</span></span>

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

<span data-ttu-id="45416-310">Den **SqlUserDefinedExtractor** attributet anger att typen ska registreras som en användardefinierad extraktor.</span><span class="sxs-lookup"><span data-stu-id="45416-310">The **SqlUserDefinedExtractor** attribute indicates that the type should be registered as a user-defined extractor.</span></span> <span data-ttu-id="45416-311">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-311">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-312">SqlUserDefinedExtractor är ett valfritt attribut för Undanta definition.</span><span class="sxs-lookup"><span data-stu-id="45416-312">SqlUserDefinedExtractor is an optional attribute for UDE definition.</span></span> <span data-ttu-id="45416-313">Den används för att definiera AtomicFileProcessing-egenskapen för Undanta.</span><span class="sxs-lookup"><span data-stu-id="45416-313">It used to define AtomicFileProcessing property for the UDE object.</span></span>

* <span data-ttu-id="45416-314">bool AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="45416-314">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="45416-315">**SANT** = anger att den här extraktor kräver atomiska indatafiler (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="45416-315">**true** = Indicates that this extractor requires atomic input files (JSON, XML, ...)</span></span>
* <span data-ttu-id="45416-316">**FALSKT** = anger den här extraktor kan hantera delade / distribuerade filer (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="45416-316">**false** = Indicates that this extractor can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="45416-317">De huvudsakliga Undanta programmering objekten är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="45416-317">The main UDE programmability objects are **input** and **output**.</span></span> <span data-ttu-id="45416-318">Indataobjektet används för att räkna upp indata som `IUnstructuredReader`.</span><span class="sxs-lookup"><span data-stu-id="45416-318">The input object is used to enumerate input data as `IUnstructuredReader`.</span></span> <span data-ttu-id="45416-319">Utdata-objektet används för att ange utdata till följd av aktiviteten extraktor.</span><span class="sxs-lookup"><span data-stu-id="45416-319">The output object is used to set output data as a result of the extractor activity.</span></span>

<span data-ttu-id="45416-320">Indata är tillgänglig via `System.IO.Stream` och `System.IO.StreamReader`.</span><span class="sxs-lookup"><span data-stu-id="45416-320">The input data is accessed through `System.IO.Stream` and `System.IO.StreamReader`.</span></span>

<span data-ttu-id="45416-321">För indatakolumnerna uppräkningen dela vi först Indataströmmen med hjälp av en avgränsare för en rad.</span><span class="sxs-lookup"><span data-stu-id="45416-321">For input columns enumeration, we first split the input stream by using a row delimiter.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

<span data-ttu-id="45416-322">Sedan ytterligare delats inkommande raden kolumnen.</span><span class="sxs-lookup"><span data-stu-id="45416-322">Then, further split input row into column parts.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

<span data-ttu-id="45416-323">Om du vill ställa in datautflödet vi använder den `output.Set` metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-323">To set output data, we use the `output.Set` method.</span></span>

<span data-ttu-id="45416-324">Det är viktigt att förstå att anpassade extraktor endast matar ut kolumner och värden som definieras med utdata.</span><span class="sxs-lookup"><span data-stu-id="45416-324">It's important to understand that the custom extractor only outputs columns and values that are defined with the output.</span></span> <span data-ttu-id="45416-325">Ange metodanrop.</span><span class="sxs-lookup"><span data-stu-id="45416-325">Set method call.</span></span>

```
output.Set<string>(count, part);
```

<span data-ttu-id="45416-326">Den faktiska extraktor utdatan utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="45416-326">The actual extractor output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="45416-327">Följande är extraktor-exempel:</span><span class="sxs-lookup"><span data-stu-id="45416-327">Following is the extractor example:</span></span>

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
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
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
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
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

<span data-ttu-id="45416-328">Extraktor återskapar GUID för kolumnen ”guid” i detta scenario användningsfall och konverterar värdena i kolumnen ”användare” till versaler.</span><span class="sxs-lookup"><span data-stu-id="45416-328">In this use-case scenario, the extractor regenerates the GUID for “guid” column and converts the values of “user” column to upper case.</span></span> <span data-ttu-id="45416-329">Anpassade juicepressar kan ge mer komplicerad resultat genom att parsa indata och ändrar den.</span><span class="sxs-lookup"><span data-stu-id="45416-329">Custom extractors can produce more complicated results by parsing input data and manipulating it.</span></span>

<span data-ttu-id="45416-330">Nedan följer grundläggande U-SQL-skript som använder en anpassad extraktor:</span><span class="sxs-lookup"><span data-stu-id="45416-330">Following is base U-SQL script that uses a custom extractor:</span></span>

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

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a><span data-ttu-id="45416-331">Använda användardefinierade outputters</span><span class="sxs-lookup"><span data-stu-id="45416-331">Use user-defined outputters</span></span>
<span data-ttu-id="45416-332">Användardefinierade outputter är en annan U-SQL-UDO som gör det möjligt att utöka inbyggda funktioner för U-SQL.</span><span class="sxs-lookup"><span data-stu-id="45416-332">User-defined outputter is another U-SQL UDO that allows you to extend built-in U-SQL functionality.</span></span> <span data-ttu-id="45416-333">Liknar extraktor, det finns flera inbyggda outputters.</span><span class="sxs-lookup"><span data-stu-id="45416-333">Similar to the extractor, there are several built-in outputters.</span></span>

* <span data-ttu-id="45416-334">*Outputters.Text()*: skriver data till avgränsade textfiler i olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-334">*Outputters.Text()*: Writes data to delimited text files of different encodings.</span></span>
* <span data-ttu-id="45416-335">*Outputters.Csv()*: skriver data till en fil med kommaavgränsade värden (CSV) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-335">*Outputters.Csv()*: Writes data to comma-separated value (CSV) files of different encodings.</span></span>
* <span data-ttu-id="45416-336">*Outputters.Tsv()*: skriver data till fliken med kommaseparerade värden (TVS) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-336">*Outputters.Tsv()*: Writes data to tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="45416-337">Anpassade outputter kan du skriva data i ett anpassat definierade format.</span><span class="sxs-lookup"><span data-stu-id="45416-337">Custom outputter allows you to write data in a custom defined format.</span></span> <span data-ttu-id="45416-338">Detta kan vara användbart för följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="45416-338">This can be useful for the following tasks:</span></span>

* <span data-ttu-id="45416-339">Data skrivs till delvis strukturerade eller Ostrukturerade filer.</span><span class="sxs-lookup"><span data-stu-id="45416-339">Writing data to semi-structured or unstructured files.</span></span>
* <span data-ttu-id="45416-340">Stöd för skrivning av data inte kodningar.</span><span class="sxs-lookup"><span data-stu-id="45416-340">Writing data not supported encodings.</span></span>
* <span data-ttu-id="45416-341">Ändra utdata eller lägga till anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="45416-341">Modifying output data or adding custom attributes.</span></span>

<span data-ttu-id="45416-342">Om du vill definiera användardefinierade outputter måste vi skapa den `IOutputter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-342">To define user-defined outputter, we need to create the `IOutputter` interface.</span></span>

<span data-ttu-id="45416-343">Följande är grundläggande `IOutputter` klassen implementering:</span><span class="sxs-lookup"><span data-stu-id="45416-343">Following is the base `IOutputter` class implementation:</span></span>

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

<span data-ttu-id="45416-344">Alla indataparametrar för outputter som kolumn/rad avgränsare, kodning och så vidare måste definieras i konstruktorn för klassen.</span><span class="sxs-lookup"><span data-stu-id="45416-344">All input parameters to the outputter, such as column/row delimiters, encoding, and so on, need to be defined in the constructor of the class.</span></span> <span data-ttu-id="45416-345">Den `IOutputter` interface måste även innehålla en definition för `void Output` åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="45416-345">The `IOutputter` interface should also contain a definition for `void Output` override.</span></span> <span data-ttu-id="45416-346">Attributet `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` (valfritt) kan anges för behandling av atomiska.</span><span class="sxs-lookup"><span data-stu-id="45416-346">The attribute `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` can optionally be set for atomic file processing.</span></span> <span data-ttu-id="45416-347">Mer information finns i följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="45416-347">For more information, see the following details.</span></span>

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

* <span data-ttu-id="45416-348">`Output`kallas för varje inkommande rad.</span><span class="sxs-lookup"><span data-stu-id="45416-348">`Output` is called for each input row.</span></span> <span data-ttu-id="45416-349">Returnerar den `IUnstructuredWriter output` raduppsättningen.</span><span class="sxs-lookup"><span data-stu-id="45416-349">It returns the `IUnstructuredWriter output` rowset.</span></span>
* <span data-ttu-id="45416-350">Klassen konstruktor används för att skicka parametrar till användardefinierade outputter.</span><span class="sxs-lookup"><span data-stu-id="45416-350">The Constructor class is used to pass parameters to the user-defined outputter.</span></span>
* <span data-ttu-id="45416-351">`Close`används för att åsidosätta (valfritt) om du vill frigöra dyra tillstånd eller kontrollera när den sista raden har skrivits.</span><span class="sxs-lookup"><span data-stu-id="45416-351">`Close` is used to optionally override to release expensive state or determine when the last row was written.</span></span>

<span data-ttu-id="45416-352">**SqlUserDefinedOutputter** attributet anger att typen ska registreras som en användardefinierad outputter.</span><span class="sxs-lookup"><span data-stu-id="45416-352">**SqlUserDefinedOutputter** attribute indicates that the type should be registered as a user-defined outputter.</span></span> <span data-ttu-id="45416-353">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-353">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-354">SqlUserDefinedOutputter är ett valfritt attribut för en användardefinierad outputter definition.</span><span class="sxs-lookup"><span data-stu-id="45416-354">SqlUserDefinedOutputter is an optional attribute for a user-defined outputter definition.</span></span> <span data-ttu-id="45416-355">Den används för att definiera egenskapen AtomicFileProcessing.</span><span class="sxs-lookup"><span data-stu-id="45416-355">It's used to define the AtomicFileProcessing property.</span></span>

* <span data-ttu-id="45416-356">bool AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="45416-356">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="45416-357">**SANT** = anger att den här outputter kräver atomiska utdatafilerna (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="45416-357">**true** = Indicates that this outputter requires atomic output files (JSON, XML, ...)</span></span>
* <span data-ttu-id="45416-358">**FALSKT** = anger den här outputter kan hantera delade / distribuerade filer (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="45416-358">**false** = Indicates that this outputter can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="45416-359">De huvudsakliga programmering objekten är **raden** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="45416-359">The main programmability objects are **row** and **output**.</span></span> <span data-ttu-id="45416-360">Den **raden** objektet används för att räkna upp utdata som `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-360">The **row** object is used to enumerate output data as `IRow` interface.</span></span> <span data-ttu-id="45416-361">**Utdata** används för att ställa in datautflödet målfilen.</span><span class="sxs-lookup"><span data-stu-id="45416-361">**Output** is used to set output data to the target file.</span></span>

<span data-ttu-id="45416-362">Utdata är tillgänglig via den `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-362">The output data is accessed through the `IRow` interface.</span></span> <span data-ttu-id="45416-363">Utdata skickas en rad i taget.</span><span class="sxs-lookup"><span data-stu-id="45416-363">Output data is passed a row at a time.</span></span>

<span data-ttu-id="45416-364">Enskilda värden räknas upp genom att anropa metoden Get för IRow-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="45416-364">The individual values are enumerated by calling the Get method of the IRow interface:</span></span>

```
row.Get<string>("column_name")
```

<span data-ttu-id="45416-365">Enskilda kolumnnamn kan fastställas genom att anropa `row.Schema`:</span><span class="sxs-lookup"><span data-stu-id="45416-365">Individual column names can be determined by calling `row.Schema`:</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="45416-366">Den här metoden låter dig skapa en flexibel outputter för alla metadata-scheman.</span><span class="sxs-lookup"><span data-stu-id="45416-366">This approach enables you to build a flexible outputter for any metadata schema.</span></span>

<span data-ttu-id="45416-367">Utdata skrivs till filen med hjälp av `System.IO.StreamWriter`.</span><span class="sxs-lookup"><span data-stu-id="45416-367">The output data is written to file by using `System.IO.StreamWriter`.</span></span> <span data-ttu-id="45416-368">Stream-parametern anges till `output.BaseStrea` som en del av `IUnstructuredWriter output`.</span><span class="sxs-lookup"><span data-stu-id="45416-368">The stream parameter is set to `output.BaseStrea` as part of `IUnstructuredWriter output`.</span></span>

<span data-ttu-id="45416-369">Observera att det är viktigt att tömma Databufferten för filen efter varje iteration raden.</span><span class="sxs-lookup"><span data-stu-id="45416-369">Note that it's important to flush the data buffer to the file after each row iteration.</span></span> <span data-ttu-id="45416-370">Dessutom kan den `StreamWriter` objektet måste användas med tillgänglig attributet aktiverad (standard) och den **med** nyckelordet:</span><span class="sxs-lookup"><span data-stu-id="45416-370">In addition, the `StreamWriter` object must be used with the Disposable attribute enabled (default) and with the **using** keyword:</span></span>

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

<span data-ttu-id="45416-371">Annars kan anropa uttryckligen Flush() metoden efter varje iteration.</span><span class="sxs-lookup"><span data-stu-id="45416-371">Otherwise, call Flush() method explicitly after each iteration.</span></span> <span data-ttu-id="45416-372">Detta visar vi i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="45416-372">We show this in the following example.</span></span>

### <a name="set-headers-and-footers-for-user-defined-outputter"></a><span data-ttu-id="45416-373">Ange sidhuvuden och sidfötter för användardefinierade outputter</span><span class="sxs-lookup"><span data-stu-id="45416-373">Set headers and footers for user-defined outputter</span></span>
<span data-ttu-id="45416-374">Använd enstaka upprepning körning flödet för att ange en rubrik.</span><span class="sxs-lookup"><span data-stu-id="45416-374">To set a header, use single iteration execution flow.</span></span>

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

<span data-ttu-id="45416-375">Koden i först `if (isHeaderRow)` block körs bara en gång.</span><span class="sxs-lookup"><span data-stu-id="45416-375">The code in the first `if (isHeaderRow)` block is executed only once.</span></span>

<span data-ttu-id="45416-376">Använd för sidfoten referensen till instansen av `System.IO.Stream` objekt (`output.BaseStream`).</span><span class="sxs-lookup"><span data-stu-id="45416-376">For the footer, use the reference to the instance of `System.IO.Stream` object (`output.BaseStream`).</span></span> <span data-ttu-id="45416-377">Skriva sidfoten i metoden Close() i den `IOutputter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-377">Write the footer in the Close() method of the `IOutputter` interface.</span></span>  <span data-ttu-id="45416-378">(Mer information finns i följande exempel.)</span><span class="sxs-lookup"><span data-stu-id="45416-378">(For more information, see the following example.)</span></span>

<span data-ttu-id="45416-379">Följande är ett exempel på en användardefinierad outputter:</span><span class="sxs-lookup"><span data-stu-id="45416-379">Following is an example of a user-defined outputter:</span></span>

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

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close().
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
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
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
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
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

<span data-ttu-id="45416-380">Och grundläggande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="45416-380">And U-SQL base script:</span></span>

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
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="45416-381">Det här är en HTML-outputter som skapar en HTML-fil med tabelldata.</span><span class="sxs-lookup"><span data-stu-id="45416-381">This is an HTML outputter, which creates an HTML file with table data.</span></span>

### <a name="call-outputter-from-u-sql-base-script"></a><span data-ttu-id="45416-382">Anropa outputter från grundläggande U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="45416-382">Call outputter from U-SQL base script</span></span>
<span data-ttu-id="45416-383">För att anropa en anpassad outputter från grundläggande U-SQL-skript, har den nya instansen av outputter-objektet skapas.</span><span class="sxs-lookup"><span data-stu-id="45416-383">To call a custom outputter from the base U-SQL script, the new instance of the outputter object has to be created.</span></span>

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="45416-384">För att undvika att skapa en instans av objektet i grundläggande skript kan skapa vi en funktion omslutning som visas i det tidigare exemplet:</span><span class="sxs-lookup"><span data-stu-id="45416-384">To avoid creating an instance of the object in base script, we can create a function wrapper, as shown in our earlier example:</span></span>

```c#
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

<span data-ttu-id="45416-385">I det här fallet ursprungliga anropet ser ut som följande:</span><span class="sxs-lookup"><span data-stu-id="45416-385">In this case, the original call looks like the following:</span></span>

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a><span data-ttu-id="45416-386">Använda användardefinierade processorer</span><span class="sxs-lookup"><span data-stu-id="45416-386">Use user-defined processors</span></span>
<span data-ttu-id="45416-387">Användardefinierade är UDP, en typ av U-SQL-UDO som gör det möjligt att bearbeta inkommande rader genom att använda programmeringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="45416-387">User-defined processor, or UDP, is a type of U-SQL UDO that enables you to process the incoming rows by applying programmability features.</span></span> <span data-ttu-id="45416-388">UDP kan du kombinera kolumner, ändra värdena och lägga till nya kolumner, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="45416-388">UDP enables you to combine columns, modify values, and add new columns if necessary.</span></span> <span data-ttu-id="45416-389">I princip hjälper det för att bearbeta en raduppsättning för att skapa nödvändiga dataelement.</span><span class="sxs-lookup"><span data-stu-id="45416-389">Basically, it helps to process a rowset to produce required data elements.</span></span>

<span data-ttu-id="45416-390">Om du vill definiera en UDP måste vi skapa en `IProcessor` gränssnitt med den `SqlUserDefinedProcessor` attribut som är valfritt för UDP.</span><span class="sxs-lookup"><span data-stu-id="45416-390">To define a UDP, we need to create an `IProcessor` interface with the `SqlUserDefinedProcessor` attribute, which is optional for UDP.</span></span>

<span data-ttu-id="45416-391">Det här gränssnittet ska innehålla definitionen för den `IRow` gränssnittet raduppsättningen åsidosätta, enligt följande exempel:</span><span class="sxs-lookup"><span data-stu-id="45416-391">This interface should contain the definition for the `IRow` interface rowset override, as shown in the following example:</span></span>

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

<span data-ttu-id="45416-392">**SqlUserDefinedProcessor** anger att typen ska registreras som en användardefinierad processor.</span><span class="sxs-lookup"><span data-stu-id="45416-392">**SqlUserDefinedProcessor** indicates that the type should be registered as a user-defined processor.</span></span> <span data-ttu-id="45416-393">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-393">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-394">Attributet SqlUserDefinedProcessor **valfria** för UDP-definition.</span><span class="sxs-lookup"><span data-stu-id="45416-394">The SqlUserDefinedProcessor attribute is **optional** for UDP definition.</span></span>

<span data-ttu-id="45416-395">De huvudsakliga programmering objekten är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="45416-395">The main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="45416-396">Indataobjektet används för att räkna upp indatakolumnerna och utdata och för att ange utdata till följd av processoraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="45416-396">The input object is used to enumerate input columns and output, and to set output data as a result of the processor activity.</span></span>

<span data-ttu-id="45416-397">För indatakolumnerna uppräkningen vi använder den `input.Get` metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-397">For input columns enumeration, we use the `input.Get` method.</span></span>

```
string column_name = input.Get<string>("column_name");
```

<span data-ttu-id="45416-398">Parametern för `input.Get` metoden är en kolumn som skickas som en del av den `PRODUCE` -satsen i den `PROCESS` instruktionen av grundläggande U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="45416-398">The parameter for `input.Get` method is a column that's passed as part of the `PRODUCE` clause of the `PROCESS` statement of the U-SQL base script.</span></span> <span data-ttu-id="45416-399">Vi behöver använda rätt datatyp här.</span><span class="sxs-lookup"><span data-stu-id="45416-399">We need to use the correct data type here.</span></span>

<span data-ttu-id="45416-400">Utdata, använda den `output.Set` metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-400">For output, use the `output.Set` method.</span></span>

<span data-ttu-id="45416-401">Det är viktigt att Observera att anpassade producent matar ut endast kolumner och värden som definieras med de `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="45416-401">It's important to note that custom producer only outputs columns and values that are defined with the `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", mycolumn);
```

<span data-ttu-id="45416-402">Den faktiska utdatan utlöses genom att anropa `return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="45416-402">The actual processor output is triggered by calling `return output.AsReadOnly();`.</span></span>

<span data-ttu-id="45416-403">Följande är ett exempel på processor:</span><span class="sxs-lookup"><span data-stu-id="45416-403">Following is a processor example:</span></span>

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

<span data-ttu-id="45416-404">I detta scenario användningsfall genererar processorn en ny kolumn som kallas ”full_description” genom att kombinera de befintliga kolumnerna – i det här fallet ”användare” i versaler och ”des”.</span><span class="sxs-lookup"><span data-stu-id="45416-404">In this use-case scenario, the processor is generating a new column called “full_description” by combining the existing columns--in this case, “user” in upper case, and “des”.</span></span> <span data-ttu-id="45416-405">Den genererar ett GUID och returnerar de ursprungliga och nya GUID-värdena.</span><span class="sxs-lookup"><span data-stu-id="45416-405">It also regenerates a GUID and returns the original and new GUID values.</span></span>

<span data-ttu-id="45416-406">Som du ser i exemplet ovan, kan du anropa C#-metoder under `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="45416-406">As you can see from the previous example, you can call C# methods during `output.Set` method call.</span></span>

<span data-ttu-id="45416-407">Följande är ett exempel på grundläggande U-SQL-skript som använder en anpassad processor:</span><span class="sxs-lookup"><span data-stu-id="45416-407">Following is an example of base U-SQL script that uses a custom processor:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a><span data-ttu-id="45416-408">Använda användardefinierade appliers</span><span class="sxs-lookup"><span data-stu-id="45416-408">Use user-defined appliers</span></span>
<span data-ttu-id="45416-409">Ett U-SQL-användardefinierade applier kan du anropa en anpassad C#-funktion för varje rad som returneras av det yttre tabelluttrycket i en fråga.</span><span class="sxs-lookup"><span data-stu-id="45416-409">A U-SQL user-defined applier enables you to invoke a custom C# function for each row that's returned by the outer table expression of a query.</span></span> <span data-ttu-id="45416-410">Rätt indata ska utvärderas för varje rad från den vänstra indata och de rader som produceras kombineras för det slutgiltiga resultatet.</span><span class="sxs-lookup"><span data-stu-id="45416-410">The right input is evaluated for each row from the left input, and the rows that are produced are combined for the final output.</span></span> <span data-ttu-id="45416-411">Listan över kolumner som genereras av APPLY-operatorn är en kombination av en uppsättning kolumner i vänstra och högra indata.</span><span class="sxs-lookup"><span data-stu-id="45416-411">The list of columns that are produced by the APPLY operator are the combination of the set of columns in the left and the right input.</span></span>

<span data-ttu-id="45416-412">Användardefinierade applier anropas som en del av uttrycket MARKERAR du USQL.</span><span class="sxs-lookup"><span data-stu-id="45416-412">User-defined applier is being invoked as part of the USQL SELECT expression.</span></span>

<span data-ttu-id="45416-413">Vanliga anropet till de användardefinierade applier ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="45416-413">The typical call to the user-defined applier looks like the following:</span></span>

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

<span data-ttu-id="45416-414">Mer information om hur du använder appliers i ett SELECT-uttryck finns [U-SQL väljer att välja mellan tillämpa och yttre tillämpa](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span><span class="sxs-lookup"><span data-stu-id="45416-414">For more information about using appliers in a SELECT expression, see [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span></span>

<span data-ttu-id="45416-415">Användardefinierade applier basklass definition är följande:</span><span class="sxs-lookup"><span data-stu-id="45416-415">The user-defined applier base class definition is as follows:</span></span>

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

<span data-ttu-id="45416-416">Om du vill definiera en användardefinierad applier måste vi skapa den `IApplier` gränssnitt med den [`SqlUserDefinedApplier`]-attribut som är valfri för en användardefinierad applier definition.</span><span class="sxs-lookup"><span data-stu-id="45416-416">To define a user-defined applier, we need to create the `IApplier` interface with the [`SqlUserDefinedApplier`] attribute, which is optional for a user-defined applier definition.</span></span>

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

* <span data-ttu-id="45416-417">Tillämpa anropas för varje rad i tabellen yttre.</span><span class="sxs-lookup"><span data-stu-id="45416-417">Apply is called for each row of the outer table.</span></span> <span data-ttu-id="45416-418">Returnerar den `IUpdatableRow` utdata raduppsättningen.</span><span class="sxs-lookup"><span data-stu-id="45416-418">It returns the `IUpdatableRow` output rowset.</span></span>
* <span data-ttu-id="45416-419">Klassen konstruktor används för att skicka parametrar till användardefinierade applier.</span><span class="sxs-lookup"><span data-stu-id="45416-419">The Constructor class is used to pass parameters to the user-defined applier.</span></span>

<span data-ttu-id="45416-420">**SqlUserDefinedApplier** anger att typen ska registreras som en användardefinierad applier.</span><span class="sxs-lookup"><span data-stu-id="45416-420">**SqlUserDefinedApplier** indicates that the type should be registered as a user-defined applier.</span></span> <span data-ttu-id="45416-421">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-421">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-422">**SqlUserDefinedApplier** är **valfria** för en användardefinierad applier definition.</span><span class="sxs-lookup"><span data-stu-id="45416-422">**SqlUserDefinedApplier** is **optional** for a user-defined applier definition.</span></span>


<span data-ttu-id="45416-423">De huvudsakliga programmering objekten är följande:</span><span class="sxs-lookup"><span data-stu-id="45416-423">The main programmability objects are as follows:</span></span>

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

<span data-ttu-id="45416-424">Inkommande raduppsättningar skickas `IRow` indata.</span><span class="sxs-lookup"><span data-stu-id="45416-424">Input rowsets are passed as `IRow` input.</span></span> <span data-ttu-id="45416-425">Utdata rader genereras som `IUpdatableRow` utdata-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="45416-425">The output rows are generated as `IUpdatableRow` output interface.</span></span>

<span data-ttu-id="45416-426">Enskilda kolumnnamn kan fastställas genom att anropa den `IRow` Schema-metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-426">Individual column names can be determined by calling the `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="45416-427">Att hämta värden för faktiska data från inkommande `IRow`, vi använda metoden Get() för `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-427">To get the actual data values from the incoming `IRow`, we use the Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="45416-428">Eller vi använda kolumnnamn schemat:</span><span class="sxs-lookup"><span data-stu-id="45416-428">Or we use the schema column name:</span></span>

```
row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="45416-429">Utdatavärden måste anges med `IUpdatableRow` utdata:</span><span class="sxs-lookup"><span data-stu-id="45416-429">The output values must be set with `IUpdatableRow` output:</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="45416-430">Det är viktigt att förstå att anpassade appliers endast utgående kolumner och värden som definieras med `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="45416-430">It is important to understand that custom appliers only output columns and values that are defined with `output.Set` method call.</span></span>

<span data-ttu-id="45416-431">Den faktiska utdatan utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="45416-431">The actual output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="45416-432">Användardefinierade applier parametrar kan skickas till konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="45416-432">The user-defined applier parameters can be passed to the constructor.</span></span> <span data-ttu-id="45416-433">Applier kan returnera en variabel antalet kolumner som måste definieras under applier-anropet i grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="45416-433">Applier can return a variable number of columns that need to be defined during the applier call in base U-SQL Script.</span></span>

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

<span data-ttu-id="45416-434">Här är det användardefinierade applier exemplet:</span><span class="sxs-lookup"><span data-stu-id="45416-434">Here is the user-defined applier example:</span></span>

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

<span data-ttu-id="45416-435">Nedan följer grundläggande U-SQL-skriptet för den här användardefinierade applier:</span><span class="sxs-lookup"><span data-stu-id="45416-435">Following is the base U-SQL script for this user-defined applier:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

<span data-ttu-id="45416-436">I den här Användarscenario fungerar användardefinierade applier som en kommaavgränsad parser för bil flottan egenskaper.</span><span class="sxs-lookup"><span data-stu-id="45416-436">In this use case scenario, user-defined applier acts as a comma-delimited value parser for the car fleet properties.</span></span> <span data-ttu-id="45416-437">Indatafilen rader ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="45416-437">The input file rows look like the following:</span></span>

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

<span data-ttu-id="45416-438">Det är en typisk tabbavgränsad TVS fil med en kolumn för egenskaper som innehåller bil egenskaper, till exempel märke och modell.</span><span class="sxs-lookup"><span data-stu-id="45416-438">It is a typical tab-delimited TSV file with a properties column that contains car properties such as make and model.</span></span> <span data-ttu-id="45416-439">Dessa egenskaper måste parsas till tabellkolumner.</span><span class="sxs-lookup"><span data-stu-id="45416-439">Those properties must be parsed to the table columns.</span></span> <span data-ttu-id="45416-440">Applier som har angetts kan du generera en dynamisk antal egenskaper i raduppsättningen resultat baserat på den parameter som skickas.</span><span class="sxs-lookup"><span data-stu-id="45416-440">The applier that's provided also enables you to generate a dynamic number of properties in the result rowset, based on the parameter that's passed.</span></span> <span data-ttu-id="45416-441">Du kan skapa alla egenskaper eller en specifik uppsättning endast egenskaper.</span><span class="sxs-lookup"><span data-stu-id="45416-441">You can generate either all properties or a specific set of properties only.</span></span>

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

<span data-ttu-id="45416-442">Användardefinierade applier kan anropas som en ny instans av applier objekt:</span><span class="sxs-lookup"><span data-stu-id="45416-442">The user-defined applier can be called as a new instance of applier object:</span></span>

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

<span data-ttu-id="45416-443">Eller med anrop av en wrapper standardmetod:</span><span class="sxs-lookup"><span data-stu-id="45416-443">Or with the invocation of a wrapper factory method:</span></span>

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a><span data-ttu-id="45416-444">Använda användardefinierade combiners</span><span class="sxs-lookup"><span data-stu-id="45416-444">Use user-defined combiners</span></span>
<span data-ttu-id="45416-445">Användardefinierade combiner eller UDC, kan du kombinera rader från vänster och höger raduppsättningar baserat på egen kod.</span><span class="sxs-lookup"><span data-stu-id="45416-445">User-defined combiner, or UDC, enables you to combine rows from left and right rowsets, based on custom logic.</span></span> <span data-ttu-id="45416-446">Användardefinierade combiner används med KOMBINERA uttryck.</span><span class="sxs-lookup"><span data-stu-id="45416-446">User-defined combiner is used with COMBINE expression.</span></span>

<span data-ttu-id="45416-447">En combiner anropas med KOMBINERA uttryck som innehåller den nödvändiga informationen om både den inkommande raduppsättningar, grupperade kolumner, förväntat resultat schemat och ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="45416-447">A combiner is being invoked with the COMBINE expression that provides the necessary information about both the input rowsets, the grouping columns, the expected result schema, and additional information.</span></span>

<span data-ttu-id="45416-448">För att anropa en combiner i ett grundläggande U-SQL-skript, använder vi följande syntax:</span><span class="sxs-lookup"><span data-stu-id="45416-448">To call a combiner in a base U-SQL script, we use the following syntax:</span></span>

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

<span data-ttu-id="45416-449">Mer information finns i [KOMBINERA uttryck (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span><span class="sxs-lookup"><span data-stu-id="45416-449">For more information, see [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span></span>

<span data-ttu-id="45416-450">Om du vill definiera en användardefinierad combiner måste vi skapa den `ICombiner` gränssnitt med den [`SqlUserDefinedCombiner`]-attribut som är valfri för en användardefinierad Combiner definition.</span><span class="sxs-lookup"><span data-stu-id="45416-450">To define a user-defined combiner, we need to create the `ICombiner` interface with the [`SqlUserDefinedCombiner`] attribute, which is optional for a user-defined Combiner definition.</span></span>

<span data-ttu-id="45416-451">Base `ICombiner` klassen definition:</span><span class="sxs-lookup"><span data-stu-id="45416-451">Base `ICombiner` class definition:</span></span>

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

<span data-ttu-id="45416-452">Den anpassa implementeringen av en `ICombiner` gränssnitt ska innehålla definitionen för en `IEnumerable<IRow>` kombinera åsidosättning.</span><span class="sxs-lookup"><span data-stu-id="45416-452">The custom implementation of an `ICombiner` interface should contain the definition for an `IEnumerable<IRow>` Combine override.</span></span>

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

<span data-ttu-id="45416-453">Den **SqlUserDefinedCombiner** attributet anger att typen ska registreras som en användardefinierad combiner.</span><span class="sxs-lookup"><span data-stu-id="45416-453">The **SqlUserDefinedCombiner** attribute indicates that the type should be registered as a user-defined combiner.</span></span> <span data-ttu-id="45416-454">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-454">This class cannot be inherited.</span></span>

<span data-ttu-id="45416-455">**SqlUserDefinedCombiner** används för att definiera egenskapen Combiner läge.</span><span class="sxs-lookup"><span data-stu-id="45416-455">**SqlUserDefinedCombiner** is used to define the Combiner mode property.</span></span> <span data-ttu-id="45416-456">Det är ett valfritt attribut för en användardefinierad combiner definition.</span><span class="sxs-lookup"><span data-stu-id="45416-456">It is an optional attribute for a user-defined combiner definition.</span></span>

<span data-ttu-id="45416-457">CombinerMode läge</span><span class="sxs-lookup"><span data-stu-id="45416-457">CombinerMode     Mode</span></span>

<span data-ttu-id="45416-458">CombinerMode uppräkning kan ha följande värden:</span><span class="sxs-lookup"><span data-stu-id="45416-458">CombinerMode enum can take the following values:</span></span>

* <span data-ttu-id="45416-459">Fullständig (0) varje rad i utdata potentiellt beror på alla inkommande rader från vänster och höger med samma nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="45416-459">Full  (0) Every output row potentially depends on all the input rows from left and right       with the same key value.</span></span>

* <span data-ttu-id="45416-460">Vänster (1) varje rad i utdata är beroende av en rad från vänster (och potentiellt alla rader från höger med samma nyckelvärde) som indata.</span><span class="sxs-lookup"><span data-stu-id="45416-460">Left  (1) Every output row depends on a single input row from the left (and potentially all rows       from the right with the same key value).</span></span>

* <span data-ttu-id="45416-461">Höger (2) varje rad i utdata är beroende av en rad från höger (och potentiellt alla rader från vänster med samma nyckelvärde) som indata.</span><span class="sxs-lookup"><span data-stu-id="45416-461">Right (2)     Every output row depends on a single input row from the right (and potentially all rows       from the left with the same key value).</span></span>

* <span data-ttu-id="45416-462">Den inre (3) varje rad i utdata beror på en inkommande rad från vänster och höger med samma värde.</span><span class="sxs-lookup"><span data-stu-id="45416-462">Inner (3) Every output row depends on a single input row from left and right with the same value.</span></span>

<span data-ttu-id="45416-463">Exempel: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span><span class="sxs-lookup"><span data-stu-id="45416-463">Example:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span></span>


<span data-ttu-id="45416-464">De huvudsakliga programmering objekten är:</span><span class="sxs-lookup"><span data-stu-id="45416-464">The main programmability objects are:</span></span>

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

<span data-ttu-id="45416-465">Inkommande raduppsättningar skickas **vänstra** och **rätt** `IRowset` typ av gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-465">Input rowsets are passed as **left** and **right** `IRowset` type of interface.</span></span> <span data-ttu-id="45416-466">Båda raduppsättningar måste räknas upp för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="45416-466">Both rowsets must be enumerated for processing.</span></span> <span data-ttu-id="45416-467">Du kan räkna upp alla gränssnitt en gång, så att vi räknar upp och cachelagra den vid behov.</span><span class="sxs-lookup"><span data-stu-id="45416-467">You can only enumerate each interface once, so we have to enumerate and cache it if necessary.</span></span>

<span data-ttu-id="45416-468">För cachelagring, kan vi skapa en lista\<T\> typ av minne som ett resultat av en LINQ Frågekörningen, specifikt lista <`IRow`>.</span><span class="sxs-lookup"><span data-stu-id="45416-468">For caching purposes, we can create a List\<T\> type of memory structure as a result of a LINQ query execution, specifically List<`IRow`>.</span></span> <span data-ttu-id="45416-469">Anonym datatypen kan användas under uppräkning samt.</span><span class="sxs-lookup"><span data-stu-id="45416-469">The anonymous data type can be used during enumeration as well.</span></span>

<span data-ttu-id="45416-470">Se [introduktion till LINQ-frågor (C#)](https://msdn.microsoft.com/library/bb397906.aspx) för mer information om LINQ-frågor och [IEnumerable\<T\> gränssnittet](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) för mer information om IEnumerable\<T\> gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-470">See [Introduction to LINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) for more information about LINQ queries, and [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) for more information about IEnumerable\<T\> interface.</span></span>

<span data-ttu-id="45416-471">Att hämta värden för faktiska data från inkommande `IRowset`, vi använda metoden Get() för `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-471">To get the actual data values from the incoming `IRowset`, we use the Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="45416-472">Enskilda kolumnnamn kan fastställas genom att anropa den `IRow` Schema-metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-472">Individual column names can be determined by calling the `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="45416-473">Eller med hjälp av kolumnen schemanamnet:</span><span class="sxs-lookup"><span data-stu-id="45416-473">Or by using the schema column name:</span></span>

```
c# row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="45416-474">Allmän uppräkning med LINQ ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="45416-474">The general enumeration with LINQ looks like the following:</span></span>

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

<span data-ttu-id="45416-475">Efter att räkna upp båda raduppsättningar ska vi gå igenom alla rader.</span><span class="sxs-lookup"><span data-stu-id="45416-475">After enumerating both rowsets, we are going to loop through all rows.</span></span> <span data-ttu-id="45416-476">För varje rad i raduppsättningen vänstra ska vi söka efter alla rader som uppfyller villkoret för våra combiner.</span><span class="sxs-lookup"><span data-stu-id="45416-476">For each row in the left rowset, we are going to find all rows that satisfy the condition of our combiner.</span></span>

<span data-ttu-id="45416-477">Utdatavärden måste anges med `IUpdatableRow` utdata.</span><span class="sxs-lookup"><span data-stu-id="45416-477">The output values must be set with `IUpdatableRow` output.</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="45416-478">Den faktiska utdatan utlöses av anrop till `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="45416-478">The actual output is triggered by calling to `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="45416-479">Följande är ett exempel på combiner:</span><span class="sxs-lookup"><span data-stu-id="45416-479">Following is a combiner example:</span></span>

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

<span data-ttu-id="45416-480">I detta scenario användningsfall skapar vi en analytics-rapport för återförsäljaren.</span><span class="sxs-lookup"><span data-stu-id="45416-480">In this use-case scenario, we are building an analytics report for the retailer.</span></span> <span data-ttu-id="45416-481">Målet är att hitta alla produkter som kostar mer än 20 000 och som sälja via webbplatsen snabbare än via vanliga återförsäljare inom ett bestämt tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="45416-481">The goal is to find all products that cost more than $20,000 and that sell through the website faster than through the regular retailer within a certain time frame.</span></span>

<span data-ttu-id="45416-482">Det här är det grundläggande U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="45416-482">Here is the base U-SQL script.</span></span> <span data-ttu-id="45416-483">Du kan jämföra logiken mellan en vanlig koppling och en combiner:</span><span class="sxs-lookup"><span data-stu-id="45416-483">You can compare the logic between a regular JOIN and a combiner:</span></span>

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

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

<span data-ttu-id="45416-484">En användardefinierad combiner kan anropas som en ny instans av objektet applier:</span><span class="sxs-lookup"><span data-stu-id="45416-484">A user-defined combiner can be called as a new instance of the applier object:</span></span>

```
USING new MyNameSpace.MyCombiner();
```


<span data-ttu-id="45416-485">Eller med anrop av en wrapper standardmetod:</span><span class="sxs-lookup"><span data-stu-id="45416-485">Or with the invocation of a wrapper factory method:</span></span>

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a><span data-ttu-id="45416-486">Använda användardefinierade förminskningsapparater</span><span class="sxs-lookup"><span data-stu-id="45416-486">Use user-defined reducers</span></span>

<span data-ttu-id="45416-487">U-SQL kan du skriva anpassade raduppsättningen förminskningsapparater i C# med hjälp av ramverket användardefinierade operatorn utökningsbarhet och implementera ett IReducer gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="45416-487">U-SQL enables you to write custom rowset reducers in C# by using the user-defined operator extensibility framework and implementing an IReducer interface.</span></span>

<span data-ttu-id="45416-488">Användardefinierade reducer eller UDR, kan användas till att eliminera onödiga raderna under Extraheringen av data (importera).</span><span class="sxs-lookup"><span data-stu-id="45416-488">User-defined reducer, or UDR, can be used to eliminate unnecessary rows during data extraction (import).</span></span> <span data-ttu-id="45416-489">Det kan också användas att manipulera och utvärdera rader och kolumner.</span><span class="sxs-lookup"><span data-stu-id="45416-489">It also can be used to manipulate and evaluate rows and columns.</span></span> <span data-ttu-id="45416-490">Baserat på logik för programmering, kan det också definiera vilka rader måste ska extraheras.</span><span class="sxs-lookup"><span data-stu-id="45416-490">Based on programmability logic, it can also define which rows need to be extracted.</span></span>

<span data-ttu-id="45416-491">Om du vill definiera en UDR klass måste vi skapa en `IReducer` gränssnitt med en valfri `SqlUserDefinedReducer` attribut.</span><span class="sxs-lookup"><span data-stu-id="45416-491">To define a UDR class, we need to create an `IReducer` interface with an optional `SqlUserDefinedReducer` attribute.</span></span>

<span data-ttu-id="45416-492">Den här klassen-gränssnittet ska innehålla en definition för det `IEnumerable` gränssnittet raduppsättningen åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="45416-492">This class interface should contain a definition for the `IEnumerable` interface rowset override.</span></span>

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

<span data-ttu-id="45416-493">Den **SqlUserDefinedReducer** attributet anger att typen ska registreras som en användardefinierad reducer.</span><span class="sxs-lookup"><span data-stu-id="45416-493">The **SqlUserDefinedReducer** attribute indicates that the type should be registered as a user-defined reducer.</span></span> <span data-ttu-id="45416-494">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="45416-494">This class cannot be inherited.</span></span>
<span data-ttu-id="45416-495">**SqlUserDefinedReducer** är ett valfritt attribut för en användardefinierad reducer definition.</span><span class="sxs-lookup"><span data-stu-id="45416-495">**SqlUserDefinedReducer** is an optional attribute for a user-defined reducer definition.</span></span> <span data-ttu-id="45416-496">Den används för att definiera IsRecursive egenskap.</span><span class="sxs-lookup"><span data-stu-id="45416-496">It's used to define IsRecursive property.</span></span>

* <span data-ttu-id="45416-497">bool IsRecursive</span><span class="sxs-lookup"><span data-stu-id="45416-497">bool     IsRecursive</span></span>    
* <span data-ttu-id="45416-498">**SANT** = anger om den här Reducer är idempotent</span><span class="sxs-lookup"><span data-stu-id="45416-498">**true**  = Indicates whether this Reducer is idempotent</span></span>

<span data-ttu-id="45416-499">De huvudsakliga programmering objekten är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="45416-499">The main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="45416-500">Indataobjektet används för att räkna upp inkommande rader.</span><span class="sxs-lookup"><span data-stu-id="45416-500">The input object is used to enumerate input rows.</span></span> <span data-ttu-id="45416-501">Utdata används för att ange utdata rader på grund av att minska aktivitet.</span><span class="sxs-lookup"><span data-stu-id="45416-501">Output is used to set output rows as a result of reducing activity.</span></span>

<span data-ttu-id="45416-502">För inkommande rader uppräkningen vi använder den `Row.Get` metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-502">For input rows enumeration, we use the `Row.Get` method.</span></span>

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

<span data-ttu-id="45416-503">Parametern för den `Row.Get` metoden är en kolumn som skickas som en del av den `PRODUCE` klassen för den `REDUCE` instruktionen av grundläggande U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="45416-503">The parameter for the `Row.Get` method is a column that's passed as part of the `PRODUCE` class of the `REDUCE` statement of the U-SQL base script.</span></span> <span data-ttu-id="45416-504">Vi behöver använda rätt datatyp som här.</span><span class="sxs-lookup"><span data-stu-id="45416-504">We need to use the correct data type here as well.</span></span>

<span data-ttu-id="45416-505">Utdata, använda den `output.Set` metoden.</span><span class="sxs-lookup"><span data-stu-id="45416-505">For output, use the `output.Set` method.</span></span>

<span data-ttu-id="45416-506">Det är viktigt att förstå den anpassade reducer endast matar ut värden som definieras med de `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="45416-506">It is important to understand that custom reducer only outputs values that are defined with the `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", guid);
```

<span data-ttu-id="45416-507">Den faktiska reducer utdatan utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="45416-507">The actual reducer output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="45416-508">Följande är ett exempel på reducer:</span><span class="sxs-lookup"><span data-stu-id="45416-508">Following is a reducer example:</span></span>

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

<span data-ttu-id="45416-509">I detta scenario användningsfall reducer hoppar över rader med ett tomt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="45416-509">In this use-case scenario, the reducer is skipping rows with an empty user name.</span></span> <span data-ttu-id="45416-510">För varje rad i raduppsättningen läser varje obligatorisk kolumn, därefter utvärderar längden på användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="45416-510">For each row in rowset, it reads each required column, then evaluates the length of the user name.</span></span> <span data-ttu-id="45416-511">Den matar ut den faktiska raden bara om värdet för användarnamnet är större än 0.</span><span class="sxs-lookup"><span data-stu-id="45416-511">It outputs the actual row only if user name value length is more than 0.</span></span>

<span data-ttu-id="45416-512">Nedan följer grundläggande U-SQL-skript som använder en anpassad reducer:</span><span class="sxs-lookup"><span data-stu-id="45416-512">Following is base U-SQL script that uses a custom reducer:</span></span>

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
    TO @output_file 
    USING Outputters.Text();
```
