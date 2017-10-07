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
# <a name="u-sql-programmability-guide"></a><span data-ttu-id="74ede-103">U-SQL-programmering guide</span><span class="sxs-lookup"><span data-stu-id="74ede-103">U-SQL programmability guide</span></span>

<span data-ttu-id="74ede-104">U-SQL är ett frågespråk som är utformad för stora datatypen för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-104">U-SQL is a query language that's designed for big data-type of workloads.</span></span> <span data-ttu-id="74ede-105">En av hello unika funktionerna i U-SQL är hello kombination av hello SQL-liknande deklarativ språk med hello utökningsbarhet och programmering som tillhandahålls av C#.</span><span class="sxs-lookup"><span data-stu-id="74ede-105">One of hello unique features of U-SQL is hello combination of hello SQL-like declarative language with hello extensibility and programmability that's provided by C#.</span></span> <span data-ttu-id="74ede-106">I den här handboken vi koncentrera dig på hello utökningsbarhet och hello U-SQL-språket som är aktiverad som C#-programmering.</span><span class="sxs-lookup"><span data-stu-id="74ede-106">In this guide, we concentrate on hello extensibility and programmability of hello U-SQL language that's enabled by C#.</span></span>

## <a name="requirements"></a><span data-ttu-id="74ede-107">Krav</span><span class="sxs-lookup"><span data-stu-id="74ede-107">Requirements</span></span>

<span data-ttu-id="74ede-108">Hämta och installera [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="74ede-108">Download and install [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="get-started-with-u-sql"></a><span data-ttu-id="74ede-109">Kom igång med U-SQL</span><span class="sxs-lookup"><span data-stu-id="74ede-109">Get started with U-SQL</span></span>  

<span data-ttu-id="74ede-110">Nu ska vi titta på hello följande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="74ede-110">Let’s look at hello following U-SQL script:</span></span>

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

<span data-ttu-id="74ede-111">Den definierar en raduppsättning kallas @a och skapar en raduppsättning kallas @results från @a.</span><span class="sxs-lookup"><span data-stu-id="74ede-111">It defines a RowSet called @a and creates a RowSet called @results from @a.</span></span>

## <a name="c-types-and-expressions-in-u-sql-script"></a><span data-ttu-id="74ede-112">C#-typer och uttryck i U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="74ede-112">C# types and expressions in U-SQL script</span></span>

<span data-ttu-id="74ede-113">Ett U-SQL-uttryck är ett C#-uttryck i kombination med U-SQL logiska operationer sådana `AND`, `OR`, och `NOT`.</span><span class="sxs-lookup"><span data-stu-id="74ede-113">A U-SQL Expression is a C# expression combined with U-SQL logical operations such `AND`, `OR`, and `NOT`.</span></span> <span data-ttu-id="74ede-114">U-SQL-uttryck kan användas med SELECT-, EXTRAHERA, där, med, gruppera efter och DEKLARERA.</span><span class="sxs-lookup"><span data-stu-id="74ede-114">U-SQL Expressions can be used with SELECT, EXTRACT, WHERE, HAVING, GROUP BY and DECLARE.</span></span>

<span data-ttu-id="74ede-115">Följande skript hello Parsar till exempel en sträng ett DateTime-värde i hello SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="74ede-115">For example, hello following script parses a string a DateTime value in hello SELECT clause.</span></span>

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

<span data-ttu-id="74ede-116">hello Parsar följande skript en sträng ett DateTime-värde i en DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="74ede-116">hello following script parses a string a DateTime value in a DECLARE statement.</span></span>

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a><span data-ttu-id="74ede-117">Använd C#-uttryck för datatypkonverteringar</span><span class="sxs-lookup"><span data-stu-id="74ede-117">Use C# expressions for data type conversions</span></span>
<span data-ttu-id="74ede-118">hello som följande exempel visar hur du kan göra en konvertering för datetime-data med hjälp av C#-uttryck.</span><span class="sxs-lookup"><span data-stu-id="74ede-118">hello following example demonstrates how you can do a datetime data conversion by using C# expressions.</span></span> <span data-ttu-id="74ede-119">I det här scenariot är datetime strängdata konverterade toostandard datetime med Tidformatet för midnatt 00:00:00.</span><span class="sxs-lookup"><span data-stu-id="74ede-119">In this particular scenario, string datetime data is converted toostandard datetime with midnight 00:00:00 time notation.</span></span>

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 too@output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a><span data-ttu-id="74ede-120">Använd C#-uttryck för dagens datum</span><span class="sxs-lookup"><span data-stu-id="74ede-120">Use C# expressions for today’s date</span></span>
<span data-ttu-id="74ede-121">toopull dagens datum, vi kan använda hello följande C#-uttryck:</span><span class="sxs-lookup"><span data-stu-id="74ede-121">toopull today’s date, we can use hello following C# expression:</span></span>

```
DateTime.Now.ToString("M/d/yyyy")
```

<span data-ttu-id="74ede-122">Här är ett exempel på hur toouse uttrycket i ett skript:</span><span class="sxs-lookup"><span data-stu-id="74ede-122">Here's an example of how toouse this expression in a script:</span></span>

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



## <a name="using-net-assemblies"></a><span data-ttu-id="74ede-123">Med hjälp av .NET-sammansättningar</span><span class="sxs-lookup"><span data-stu-id="74ede-123">Using .NET assemblies</span></span>
<span data-ttu-id="74ede-124">U-SQL-modellen för utökning beroende kraftigt hello möjlighet tooadd anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="74ede-124">U-SQL’s extensibility model relies heavily on hello ability tooadd custom code.</span></span> <span data-ttu-id="74ede-125">För närvarande ger U-SQL dig enkla sätt tooadd egna Microsoft. NET-baserade kod (särskilt C#).</span><span class="sxs-lookup"><span data-stu-id="74ede-125">Currently, U-SQL provides you with easy ways tooadd your own Microsoft .NET-based code (in particular, C#).</span></span> <span data-ttu-id="74ede-126">Du kan också lägga till egen kod som skrivs i andra .NET-språk, till exempel VB.NET eller F #.</span><span class="sxs-lookup"><span data-stu-id="74ede-126">However, you can also add custom code that's written in other .NET languages, such as VB.NET or F#.</span></span> 

### <a name="register-a-net-assembly"></a><span data-ttu-id="74ede-127">Registrera en .NET-sammansättning</span><span class="sxs-lookup"><span data-stu-id="74ede-127">Register a .NET assembly</span></span>

<span data-ttu-id="74ede-128">Använd hello CREATE ASSEMBLY instruktionen tooplace en .NET-sammansättning i en U-SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="74ede-128">Use hello CREATE ASSEMBLY statement tooplace a .NET assembly into a U-SQL Database.</span></span> <span data-ttu-id="74ede-129">När en sammansättning i en databas kan kan U-SQL-skript använda dessa sammansättningar med hello REFERENSSAMMANSÄTTNING-sats.</span><span class="sxs-lookup"><span data-stu-id="74ede-129">Once an assembly is in a database, U-SQL scripts can use those assemblies by using hello REFERENCE ASSEMBLY statement.</span></span> 

<span data-ttu-id="74ede-130">Hej följande kod visar hur tooregister en sammansättning:</span><span class="sxs-lookup"><span data-stu-id="74ede-130">hello following code shows how tooregister an assembly:</span></span>

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

<span data-ttu-id="74ede-131">Hej följande kod visar hur tooreference en sammansättning:</span><span class="sxs-lookup"><span data-stu-id="74ede-131">hello following code shows how tooreference an assembly:</span></span>

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

<span data-ttu-id="74ede-132">Kontakta hello [sammansättningen instruktioner](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) som omfattar det här avsnittet i detalj.</span><span class="sxs-lookup"><span data-stu-id="74ede-132">Consult hello [assembly registration instructions](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) that covers this topic in greater detail.</span></span>


### <a name="use-assembly-versioning"></a><span data-ttu-id="74ede-133">Använd sammansättningen versionshantering</span><span class="sxs-lookup"><span data-stu-id="74ede-133">Use assembly versioning</span></span>
<span data-ttu-id="74ede-134">För närvarande använder U-SQL hello .NET Framework version 4.5.</span><span class="sxs-lookup"><span data-stu-id="74ede-134">Currently, U-SQL uses hello .NET Framework version 4.5.</span></span> <span data-ttu-id="74ede-135">Kontrollera att din egen sammansättningar är kompatibla med den här versionen av hello körning.</span><span class="sxs-lookup"><span data-stu-id="74ede-135">So ensure that your own assemblies are compatible with that version of hello runtime.</span></span>

<span data-ttu-id="74ede-136">Som tidigare nämnts U-SQL kör kod i en 64-bitars (x 64)-format.</span><span class="sxs-lookup"><span data-stu-id="74ede-136">As mentioned earlier, U-SQL runs code in a 64-bit (x64) format.</span></span> <span data-ttu-id="74ede-137">Kontrollera så att koden är kompilerade toorun på x64.</span><span class="sxs-lookup"><span data-stu-id="74ede-137">So make sure that your code is compiled toorun on x64.</span></span> <span data-ttu-id="74ede-138">Annars kan du hämta hello felaktigt formatfel visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="74ede-138">Otherwise you get hello incorrect format error shown earlier.</span></span>

<span data-ttu-id="74ede-139">Varje överförs sammansättningen DLL-filen och resursfilen som en annan runtime, en inbyggd sammansättning eller en .config-fil kan innehålla högst 400 MB.</span><span class="sxs-lookup"><span data-stu-id="74ede-139">Each uploaded assembly DLL and resource file, such as a different runtime, a native assembly, or a config file, can be at most 400 MB.</span></span> <span data-ttu-id="74ede-140">hello total storlek för distribuerade resurser via DISTRIBUERA resurs eller via referenser tooassemblies och deras ytterligare filer får inte överstiga 3 GB.</span><span class="sxs-lookup"><span data-stu-id="74ede-140">hello total size of deployed resources, either via DEPLOY RESOURCE or via references tooassemblies and their additional files, cannot exceed 3 GB.</span></span>

<span data-ttu-id="74ede-141">Slutligen, Observera att varje U-SQL-databas kan bara innehålla en version av alla angivna sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="74ede-141">Finally, note that each U-SQL database can only contain one version of any given assembly.</span></span> <span data-ttu-id="74ede-142">Till exempel om du behöver både 7 och versionen 8 av hello NewtonSoft Json.Net bibliotek, måste tooregister dem i två olika databaser.</span><span class="sxs-lookup"><span data-stu-id="74ede-142">For example, if you need both version 7 and version 8 of hello NewtonSoft Json.Net library, you need tooregister them in two different databases.</span></span> <span data-ttu-id="74ede-143">Dessutom kan varje skript endast hänvisa tooone version av en angiven sammansättning DLL.</span><span class="sxs-lookup"><span data-stu-id="74ede-143">Furthermore, each script can only refer tooone version of a given assembly DLL.</span></span> <span data-ttu-id="74ede-144">I detta avseende följer U-SQL hello C# sammansättningen hantering och versionshantering semantik.</span><span class="sxs-lookup"><span data-stu-id="74ede-144">In this respect, U-SQL follows hello C# assembly management and versioning semantics.</span></span>


## <a name="use-user-defined-functions-udf"></a><span data-ttu-id="74ede-145">Använda användardefinierade funktioner: UDF</span><span class="sxs-lookup"><span data-stu-id="74ede-145">Use user-defined functions: UDF</span></span>
<span data-ttu-id="74ede-146">U-SQL-användardefinierade funktioner eller UDF, programmerar rutiner som accepterar parametrar, utföra en åtgärd (till exempel en komplicerad beräkning) och returnera hello resultatet av den åtgärden som ett-värde.</span><span class="sxs-lookup"><span data-stu-id="74ede-146">U-SQL user-defined functions, or UDF, are programming routines that accept parameters, perform an action (such as a complex calculation), and return hello result of that action as a value.</span></span> <span data-ttu-id="74ede-147">hello returnera värdet för UDF kan endast vara en enskild skalär.</span><span class="sxs-lookup"><span data-stu-id="74ede-147">hello return value of UDF can only be a single scalar.</span></span> <span data-ttu-id="74ede-148">U-SQL-UDF kan anropas i grundläggande U-SQL-skript som andra C# skalära funktioner.</span><span class="sxs-lookup"><span data-stu-id="74ede-148">U-SQL UDF can be called in U-SQL base script like any other C# scalar function.</span></span>

<span data-ttu-id="74ede-149">Vi rekommenderar att du initiera U-SQL-användardefinierade funktioner som **offentliga** och **statiska**.</span><span class="sxs-lookup"><span data-stu-id="74ede-149">We recommend that you initialize U-SQL user-defined functions as **public** and **static**.</span></span>

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

<span data-ttu-id="74ede-150">Först ska vi titta på hello enkelt exempel på hur du skapar en UDF.</span><span class="sxs-lookup"><span data-stu-id="74ede-150">First let’s look at hello simple example of creating a UDF.</span></span>

<span data-ttu-id="74ede-151">I detta scenario användningsfall måste toodetermine hello räkenskapsårets period, inklusive hello räkenskapskvartal och räkenskapsmånad på hello första inloggningen för hello specifika användare.</span><span class="sxs-lookup"><span data-stu-id="74ede-151">In this use-case scenario, we need toodetermine hello fiscal period, including hello fiscal quarter and fiscal month of hello first sign-in for hello specific user.</span></span> <span data-ttu-id="74ede-152">hello är första räkenskapsmånad på hello år i vårt scenario juni.</span><span class="sxs-lookup"><span data-stu-id="74ede-152">hello first fiscal month of hello year in our scenario is June.</span></span>

<span data-ttu-id="74ede-153">toocalculate räkenskapsårets period, introducerar vi följande C#-funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="74ede-153">toocalculate fiscal period, we introduce hello following C# function:</span></span>

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

<span data-ttu-id="74ede-154">Bara beräknar räkenskapsmånad och kvartal och returnerar ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="74ede-154">It simply calculates fiscal month and quarter and returns a string value.</span></span> <span data-ttu-id="74ede-155">Vi använder ”Q1:P1” för juni hello första månad hello första räkenskapskvartal.</span><span class="sxs-lookup"><span data-stu-id="74ede-155">For June, hello first month of hello first fiscal quarter, we use "Q1:P1".</span></span> <span data-ttu-id="74ede-156">Vi använder ”Q1:P2” för juli och så vidare.</span><span class="sxs-lookup"><span data-stu-id="74ede-156">For July, we use "Q1:P2", and so on.</span></span>

<span data-ttu-id="74ede-157">Det här är en vanlig C#-funktion som vi är pågående toouse i vår U-SQL-projekt.</span><span class="sxs-lookup"><span data-stu-id="74ede-157">This is a regular C# function that we are going toouse in our U-SQL project.</span></span>

<span data-ttu-id="74ede-158">Här är hur hello bakomliggande kod avsnitt ser ut i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="74ede-158">Here is how hello code-behind section looks in this scenario:</span></span>

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

<span data-ttu-id="74ede-159">Nu vi toocall funktionen från hello grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-159">Now we are going toocall this function from hello base U-SQL script.</span></span> <span data-ttu-id="74ede-160">toodo, har vi tooprovide ett fullständigt kvalificerat namn för hello funktion, inklusive hello namnområdet, som i det här fallet är NameSpace.Class.Function(parameter).</span><span class="sxs-lookup"><span data-stu-id="74ede-160">toodo this, we have tooprovide a fully qualified name for hello function, including hello namespace, which in this case is NameSpace.Class.Function(parameter).</span></span>

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

<span data-ttu-id="74ede-161">Följande är hello faktiska U-SQL bas-skript:</span><span class="sxs-lookup"><span data-stu-id="74ede-161">Following is hello actual U-SQL base script:</span></span>

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

<span data-ttu-id="74ede-162">Nedan följer hello utdatafilen för körning av skript hello:</span><span class="sxs-lookup"><span data-stu-id="74ede-162">Following is hello output file of hello script execution:</span></span>

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

<span data-ttu-id="74ede-163">Det här exemplet visar en enkel användning av infogade UDF i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="74ede-163">This example demonstrates a simple usage of inline UDF in U-SQL.</span></span>

### <a name="keep-state-between-udf-invocations"></a><span data-ttu-id="74ede-164">Behåll tillstånd mellan UDF-anrop</span><span class="sxs-lookup"><span data-stu-id="74ede-164">Keep state between UDF invocations</span></span>
<span data-ttu-id="74ede-165">U-SQL C#-programmering objekt kan vara mer förfinade använder interaktivitet via hello bakomliggande kod globala variabler.</span><span class="sxs-lookup"><span data-stu-id="74ede-165">U-SQL C# programmability objects can be more sophisticated, utilizing interactivity through hello code-behind global variables.</span></span> <span data-ttu-id="74ede-166">Nu ska vi titta på hello följande affärsscenario användningsfall.</span><span class="sxs-lookup"><span data-stu-id="74ede-166">Let’s look at hello following business use-case scenario.</span></span>

<span data-ttu-id="74ede-167">Användare kan växla mellan av interna program i stora organisationer.</span><span class="sxs-lookup"><span data-stu-id="74ede-167">In large organizations, users can switch between varieties of internal applications.</span></span> <span data-ttu-id="74ede-168">Det kan vara Microsoft Dynamics CRM, PowerBI och så vidare.</span><span class="sxs-lookup"><span data-stu-id="74ede-168">These can include Microsoft Dynamics CRM, PowerBI, and so on.</span></span> <span data-ttu-id="74ede-169">Kunder vill kanske tooapply en telemetri analys av hur användarna växlar mellan olika program, vilka hello användning trender är och så vidare.</span><span class="sxs-lookup"><span data-stu-id="74ede-169">Customers might want tooapply a telemetry analysis of how users switch between different applications, what hello usage trends are, and so on.</span></span> <span data-ttu-id="74ede-170">hello målet för hello företag är toooptimize programanvändning.</span><span class="sxs-lookup"><span data-stu-id="74ede-170">hello goal for hello business is toooptimize application usage.</span></span> <span data-ttu-id="74ede-171">De kan också toocombine olika program eller specifika inloggning rutiner.</span><span class="sxs-lookup"><span data-stu-id="74ede-171">They also might want toocombine different applications or specific sign-on routines.</span></span>

<span data-ttu-id="74ede-172">tooachieve det här målet, har vi toodetermine sessions-ID. och fördröjningen mellan hello senaste sessionen som inträffat.</span><span class="sxs-lookup"><span data-stu-id="74ede-172">tooachieve this goal, we have toodetermine session IDs and lag time between hello last session that occurred.</span></span>

<span data-ttu-id="74ede-173">Vi behöver toofind en tidigare inloggning och sedan tilldela den här inloggningen tooall-sessioner som har genererat toohello samma program.</span><span class="sxs-lookup"><span data-stu-id="74ede-173">We need toofind a previous sign-in and then assign this sign-in tooall sessions that are being generated toohello same application.</span></span> <span data-ttu-id="74ede-174">hello första kontrollen är att grundläggande U-SQL-skript inte kan vi tooapply beräkningar över redan beräknade kolumner med funktionen LAG.</span><span class="sxs-lookup"><span data-stu-id="74ede-174">hello first challenge is that U-SQL base script doesn't allow us tooapply calculations over already-calculated columns with LAG function.</span></span> <span data-ttu-id="74ede-175">hello andra challenge är att det finns specifika tookeep hello-session för alla sessioner inom hello samma tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="74ede-175">hello second challenge is that we have tookeep hello specific session for all sessions within hello same time period.</span></span>

<span data-ttu-id="74ede-176">toosolve det här problemet kan vi använda den globala inuti en bakomliggande kod: `static public string globalSession;`.</span><span class="sxs-lookup"><span data-stu-id="74ede-176">toosolve this problem, we use a global variable inside a code-behind section: `static public string globalSession;`.</span></span>

<span data-ttu-id="74ede-177">Den globala variabeln är tillämpade toohello hela raduppsättning under våra körning av skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-177">This global variable is applied toohello entire rowset during our script execution.</span></span>

<span data-ttu-id="74ede-178">Här är hello bakomliggande kod avsnitt i vårt U-SQL-program:</span><span class="sxs-lookup"><span data-stu-id="74ede-178">Here is hello code-behind section of our U-SQL program:</span></span>

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

<span data-ttu-id="74ede-179">Det här exemplet visar hello globala variabeln `static public string globalSession;` används inuti hello `getStampUserSession` funktion och hämtar initieras på nytt varje gång hello Session parameter ändras.</span><span class="sxs-lookup"><span data-stu-id="74ede-179">This example shows hello global variable `static public string globalSession;` used inside hello `getStampUserSession` function and getting reinitialized each time hello Session parameter is changed.</span></span>

<span data-ttu-id="74ede-180">hello grundläggande U-SQL-skriptet är följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-180">hello U-SQL base script is as follows:</span></span>

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

<span data-ttu-id="74ede-181">Funktionen `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` anropas här under hello andra minne raduppsättningen beräkning.</span><span class="sxs-lookup"><span data-stu-id="74ede-181">Function `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` is called here during hello second memory rowset calculation.</span></span> <span data-ttu-id="74ede-182">Hello överförs `UserSessionTimestamp` kolumn och returnerar hello värdet förrän `UserSessionTimestamp` har ändrats.</span><span class="sxs-lookup"><span data-stu-id="74ede-182">It passes hello `UserSessionTimestamp` column and returns hello value until `UserSessionTimestamp` has changed.</span></span>

<span data-ttu-id="74ede-183">hello utdatafilen är följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-183">hello output file is as follows:</span></span>

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

<span data-ttu-id="74ede-184">Det här exemplet visar en mer komplicerad användningsfall scenario där vi använder en global variabel i en bakomliggande kod avsnitt som är kopplade toohello hela minne raduppsättning.</span><span class="sxs-lookup"><span data-stu-id="74ede-184">This example demonstrates a more complicated use-case scenario in which we use a global variable inside a code-behind section that's applied toohello entire memory rowset.</span></span>

## <a name="use-user-defined-types-udt"></a><span data-ttu-id="74ede-185">Använda användardefinierade typer: UDT</span><span class="sxs-lookup"><span data-stu-id="74ede-185">Use user-defined types: UDT</span></span>
<span data-ttu-id="74ede-186">Användardefinierade typer eller UDT, är en annan programmeringsfunktionen av U-SQL.</span><span class="sxs-lookup"><span data-stu-id="74ede-186">User-defined types, or UDT, is another programmability feature of U-SQL.</span></span> <span data-ttu-id="74ede-187">U-SQL-UDT fungerar som en vanlig C# användardefinierad datatyp.</span><span class="sxs-lookup"><span data-stu-id="74ede-187">U-SQL UDT acts like a regular C# user-defined type.</span></span> <span data-ttu-id="74ede-188">C# är ett starkt typifierad språk som tillåter hello användning av inbyggda och anpassade användardefinierade typer.</span><span class="sxs-lookup"><span data-stu-id="74ede-188">C# is a strongly typed language that allows hello use of built-in and custom user-defined types.</span></span>

<span data-ttu-id="74ede-189">U-SQL kan inte implicit serialisera eller deserialisera godtycklig UDT när hello UDT överförs mellan formhörnen i raduppsättningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-189">U-SQL cannot implicitly serialize or de-serialize arbitrary UDTs when hello UDT is passed between vertices in rowsets.</span></span> <span data-ttu-id="74ede-190">Detta innebär att hello användaren tooprovide en explicit formaterare hello IFormatter-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="74ede-190">This means that hello user has tooprovide an explicit formatter by using hello IFormatter interface.</span></span> <span data-ttu-id="74ede-191">Detta ger U-SQL hello serialisera och deserialisera metoder för hello UDT.</span><span class="sxs-lookup"><span data-stu-id="74ede-191">This provides U-SQL with hello serialize and de-serialize methods for hello UDT.</span></span>

> [!NOTE]
> <span data-ttu-id="74ede-192">U-SQL kan inte inbyggda juicepressar outputters för närvarande serialisera och deserialisera UDT data tooor från filer även med hello IFormatter uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="74ede-192">U-SQL’s built-in extractors and outputters currently cannot serialize or de-serialize UDT data tooor from files even with hello IFormatter set.</span></span> <span data-ttu-id="74ede-193">När du skriver UDT tooa datafilen med hello utdata-instruktion, eller läsa med en extraktor du toopass den som en sträng eller byte-matris.</span><span class="sxs-lookup"><span data-stu-id="74ede-193">So when you're writing UDT data tooa file with hello OUTPUT statement, or reading it with an extractor, you have toopass it as a string or byte array.</span></span> <span data-ttu-id="74ede-194">Sedan anropa hello serialisering och avserialisering kod (det vill säga hello UDT metoden ToString()) explicit.</span><span class="sxs-lookup"><span data-stu-id="74ede-194">Then you call hello serialization and deserialization code (that is, hello UDT’s ToString() method) explicitly.</span></span> <span data-ttu-id="74ede-195">Användardefinierade juicepressar och outputters på hello andra kan lämnar, läsa och skriva UDT-typer.</span><span class="sxs-lookup"><span data-stu-id="74ede-195">User-defined extractors and outputters, on hello other hand, can read and write UDTs.</span></span>

<span data-ttu-id="74ede-196">Om vi försöker toouse UDT i EXTRAKTOR eller OUTPUTTER (från föregående SELECT-), som visas här:</span><span class="sxs-lookup"><span data-stu-id="74ede-196">If we try toouse UDT in EXTRACTOR or OUTPUTTER (out of previous SELECT), as shown here:</span></span>

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

<span data-ttu-id="74ede-197">Vi tar emot hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="74ede-197">We receive hello following error:</span></span>

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

<span data-ttu-id="74ede-198">toowork med UDT i outputter antingen har vi tooserialize det toostring med hello metoden ToString() eller skapa en anpassad outputter.</span><span class="sxs-lookup"><span data-stu-id="74ede-198">toowork with UDT in outputter, we either have tooserialize it toostring with hello ToString() method or create a custom outputter.</span></span>

<span data-ttu-id="74ede-199">Användardefinierade typer som kan för närvarande inte användas i GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="74ede-199">UDTs currently cannot be used in GROUP BY.</span></span> <span data-ttu-id="74ede-200">Om UDT används i GROUP BY, uppstod hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="74ede-200">If UDT is used in GROUP BY, hello following error is thrown:</span></span>

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

<span data-ttu-id="74ede-201">toodefine en UDT vi behöver:</span><span class="sxs-lookup"><span data-stu-id="74ede-201">toodefine a UDT, we have to:</span></span>

* <span data-ttu-id="74ede-202">Lägg till hello följande namnområden:</span><span class="sxs-lookup"><span data-stu-id="74ede-202">Add hello following namespaces:</span></span>

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* <span data-ttu-id="74ede-203">Lägg till `Microsoft.Analytics.Interfaces`, vilket krävs för hello UDT-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-203">Add `Microsoft.Analytics.Interfaces`, which is required for hello UDT interfaces.</span></span> <span data-ttu-id="74ede-204">Dessutom `System.IO` kan vara nödvändiga toodefine hello IFormatter gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-204">In addition, `System.IO` might be needed toodefine hello IFormatter interface.</span></span>

* <span data-ttu-id="74ede-205">Definiera en användardefinierade typ med SqlUserDefinedType attribut.</span><span class="sxs-lookup"><span data-stu-id="74ede-205">Define a used-defined type with SqlUserDefinedType attribute.</span></span>

<span data-ttu-id="74ede-206">**SqlUserDefinedType** är används toomark en typdefinition i en sammansättning som användardefinierade typen (UDT) i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="74ede-206">**SqlUserDefinedType** is used toomark a type definition in an assembly as a user-defined type (UDT) in U-SQL.</span></span> <span data-ttu-id="74ede-207">hello egenskaper för hello attributet avspeglar hello hello UDT fysiska egenskaper.</span><span class="sxs-lookup"><span data-stu-id="74ede-207">hello properties on hello attribute reflect hello physical characteristics of hello UDT.</span></span> <span data-ttu-id="74ede-208">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-208">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-209">SqlUserDefinedType är ett obligatoriskt attribut för UDT-definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-209">SqlUserDefinedType is a required attribute for UDT definition.</span></span>

<span data-ttu-id="74ede-210">hello konstruktor för hello klass:</span><span class="sxs-lookup"><span data-stu-id="74ede-210">hello constructor of hello class:</span></span>  

* <span data-ttu-id="74ede-211">SqlUserDefinedTypeAttribute (typ formaterare)</span><span class="sxs-lookup"><span data-stu-id="74ede-211">SqlUserDefinedTypeAttribute (type formatter)</span></span>

* <span data-ttu-id="74ede-212">Skriv formaterare: krävs för parametern toodefine UDT-Formateraren--specifikt hello typ av hello `IFormatter` interface måste överföras här.</span><span class="sxs-lookup"><span data-stu-id="74ede-212">Type formatter: Required parameter toodefine an UDT formatter--specifically, hello type of hello `IFormatter` interface must be passed here.</span></span>

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* <span data-ttu-id="74ede-213">Vanliga UDT kräver även definitionen av hello IFormatter gränssnitt som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="74ede-213">Typical UDT also requires definition of hello IFormatter interface, as shown in hello following example:</span></span>

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

<span data-ttu-id="74ede-214">Hej `IFormatter` gränssnittet Serialiserar och frigör Serialiserar ett objektdiagram med hello rottyp av \<typeparamref name = ”T” >.</span><span class="sxs-lookup"><span data-stu-id="74ede-214">hello `IFormatter` interface serializes and de-serializes an object graph with hello root type of \<typeparamref name="T">.</span></span>

<span data-ttu-id="74ede-215">\<typeparam name = ”T” > Hej rottyp för hello objektet diagram tooserialize och deserialisera.</span><span class="sxs-lookup"><span data-stu-id="74ede-215">\<typeparam name="T">hello root type for hello object graph tooserialize and de-serialize.</span></span>

* <span data-ttu-id="74ede-216">**Deserialisera**: Frigör Serialiserar hello data på hello angivna dataströmmen och reconstitutes hello diagram över objekt.</span><span class="sxs-lookup"><span data-stu-id="74ede-216">**Deserialize**: De-serializes hello data on hello provided stream and reconstitutes hello graph of objects.</span></span>

* <span data-ttu-id="74ede-217">**Serialisera**: Serialiserar ett objekt eller diagram över objekt med hello angivna roten toohello angivna dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="74ede-217">**Serialize**: Serializes an object, or graph of objects, with hello given root toohello provided stream.</span></span>

<span data-ttu-id="74ede-218">`MyType`instans: instans av typen hello.</span><span class="sxs-lookup"><span data-stu-id="74ede-218">`MyType` instance: Instance of hello type.</span></span>  
<span data-ttu-id="74ede-219">`IColumnWriter`skrivaren / `IColumnReader` reader: hello underliggande kolumnen dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="74ede-219">`IColumnWriter` writer / `IColumnReader` reader: hello underlying column stream.</span></span>  
<span data-ttu-id="74ede-220">`ISerializationContext`kontexten: uppräkning som definierar en uppsättning flaggor som anger hello källan eller målet kontext för hello ström under serialiseringen.</span><span class="sxs-lookup"><span data-stu-id="74ede-220">`ISerializationContext` context: Enum that defines a set of flags that specifies hello source or destination context for hello stream during serialization.</span></span>

* <span data-ttu-id="74ede-221">**Mellanliggande**: Anger den hello källa eller målet kontexten inte är beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="74ede-221">**Intermediate**: Specifies that hello source or destination context is not a persisted store.</span></span>

* <span data-ttu-id="74ede-222">**Beständiga**: Anger att hello källan eller målet kontext är beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="74ede-222">**Persistence**: Specifies that hello source or destination context is a persisted store.</span></span>

<span data-ttu-id="74ede-223">Som en vanlig C# typ, en definition av U-SQL-UDT kan omfatta åsidosättningar för operatörer som +/ == /! =.</span><span class="sxs-lookup"><span data-stu-id="74ede-223">As a regular C# type, a U-SQL UDT definition can include overrides for operators such as +/==/!=.</span></span> <span data-ttu-id="74ede-224">Det kan också innefatta statiska metoder.</span><span class="sxs-lookup"><span data-stu-id="74ede-224">It can also include static methods.</span></span> <span data-ttu-id="74ede-225">Till exempel om vi toouse denna UDT som en parameter tooa mängdfunktionen MIN U-SQL, har vi toodefine < operatorn åsidosättning.</span><span class="sxs-lookup"><span data-stu-id="74ede-225">For example, if we are going toouse this UDT as a parameter tooa U-SQL MIN aggregate function, we have toodefine < operator override.</span></span>

<span data-ttu-id="74ede-226">Tidigare i den här handboken visas vi ett exempel på räkenskapsårets period identifiering från hello specifika datum i hello format Qn:Pn (Q1:P10).</span><span class="sxs-lookup"><span data-stu-id="74ede-226">Earlier in this guide, we demonstrated an example for fiscal period identification from hello specific date in hello format Qn:Pn (Q1:P10).</span></span> <span data-ttu-id="74ede-227">hello som följande exempel visar hur toodefine en anpassad skriver för räkenskapsårets tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="74ede-227">hello following example shows how toodefine a custom type for fiscal period values.</span></span>

<span data-ttu-id="74ede-228">Följande är ett exempel på en bakomliggande kod avsnitt med anpassade UDT och IFormatter gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="74ede-228">Following is an example of a code-behind section with custom UDT and IFormatter interface:</span></span>

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

<span data-ttu-id="74ede-229">hello definierade typen innehåller två tal: kvartal och månader.</span><span class="sxs-lookup"><span data-stu-id="74ede-229">hello defined type includes two numbers: quarter and month.</span></span> <span data-ttu-id="74ede-230">Operatörer == /! = / > / < och statisk metod ToString() definieras här.</span><span class="sxs-lookup"><span data-stu-id="74ede-230">Operators ==/!=/>/< and static method ToString() are defined here.</span></span>

<span data-ttu-id="74ede-231">Som tidigare nämnts kan användas i SELECT-uttryck UDT, men kan inte användas i OUTPUTTER-EXTRAKTOR utan anpassad serialisering.</span><span class="sxs-lookup"><span data-stu-id="74ede-231">As mentioned earlier, UDT can be used in SELECT expressions, but cannot be used in OUTPUTTER/EXTRACTOR without custom serialization.</span></span> <span data-ttu-id="74ede-232">Det har antingen toobe serialiserad som en sträng med ToString() eller används tillsammans med en anpassad OUTPUTTER/EXTRAKTOR.</span><span class="sxs-lookup"><span data-stu-id="74ede-232">It either has toobe serialized as a string with ToString() or used with a custom OUTPUTTER/EXTRACTOR.</span></span>

<span data-ttu-id="74ede-233">Nu ska vi diskutera användning av UDT.</span><span class="sxs-lookup"><span data-stu-id="74ede-233">Now let’s discuss usage of UDT.</span></span> <span data-ttu-id="74ede-234">I en bakomliggande kod avsnittet ändrat vi våra GetFiscalPeriod funktionen toohello följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-234">In a code-behind section, we changed our GetFiscalPeriod function toohello following:</span></span>

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

<span data-ttu-id="74ede-235">Som du ser returnerar hello värde av våra FiscalPeriod-typen.</span><span class="sxs-lookup"><span data-stu-id="74ede-235">As you can see, it returns hello value of our FiscalPeriod type.</span></span>

<span data-ttu-id="74ede-236">Här kan vi ge ett exempel på hur toouse det ytterligare i grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-236">Here we provide an example of how toouse it further in U-SQL base script.</span></span> <span data-ttu-id="74ede-237">Det här exemplet visar olika former av UDT-anrop från U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-237">This example demonstrates different forms of UDT invocation from U-SQL script.</span></span>

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

<span data-ttu-id="74ede-238">Här är ett exempel på en fullständig bakgrundskod avsnitt:</span><span class="sxs-lookup"><span data-stu-id="74ede-238">Here's an example of a full code-behind section:</span></span>

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

## <a name="use-user-defined-aggregates-udagg"></a><span data-ttu-id="74ede-239">Använda användardefinierade aggregeringar: UDAGG</span><span class="sxs-lookup"><span data-stu-id="74ede-239">Use user-defined aggregates: UDAGG</span></span>
<span data-ttu-id="74ede-240">Användardefinierade aggregeringar är aggregeringen-relaterade funktioner som inte levereras out-of-the-box med U-SQL.</span><span class="sxs-lookup"><span data-stu-id="74ede-240">User-defined aggregates are any aggregation-related functions that are not shipped out-of-the-box with U-SQL.</span></span> <span data-ttu-id="74ede-241">hello exempel kan en sammanställd tooperform anpassade matematiska beräkningar, sträng sammanfogningar ändringar med strängar, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="74ede-241">hello example can be an aggregate tooperform custom math calculations, string concatenations, manipulations with strings, and so on.</span></span>

<span data-ttu-id="74ede-242">hello användardefinierade sammanställd basklass definition är följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-242">hello user-defined aggregate base class definition is as follows:</span></span>

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

<span data-ttu-id="74ede-243">**SqlUserDefinedAggregate** anger att hello typ ska registreras som en användardefinierad funktion.</span><span class="sxs-lookup"><span data-stu-id="74ede-243">**SqlUserDefinedAggregate** indicates that hello type should be registered as a user-defined aggregate.</span></span> <span data-ttu-id="74ede-244">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-244">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-245">Attributet SqlUserDefinedType **valfria** UDAGG definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-245">SqlUserDefinedType attribute is **optional** for UDAGG definition.</span></span>


<span data-ttu-id="74ede-246">hello basklass kan du toopass tre abstrakta parametrar: två som indataparametrar och en som hello resultat.</span><span class="sxs-lookup"><span data-stu-id="74ede-246">hello base class allows you toopass three abstract parameters: two as input parameters and one as hello result.</span></span> <span data-ttu-id="74ede-247">hello-datatyper kan variera och definieras under klassarv.</span><span class="sxs-lookup"><span data-stu-id="74ede-247">hello data types are variable and should be defined during class inheritance.</span></span>

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

* <span data-ttu-id="74ede-248">**Init** anropar en gång för varje grupp under beräkningen.</span><span class="sxs-lookup"><span data-stu-id="74ede-248">**Init** invokes once for each group during computation.</span></span> <span data-ttu-id="74ede-249">Det ger en initieringsrutinen för varje grupp aggregering.</span><span class="sxs-lookup"><span data-stu-id="74ede-249">It provides an initialization routine for each aggregation group.</span></span>  
* <span data-ttu-id="74ede-250">**Ackumulera** körs en gång för varje värde.</span><span class="sxs-lookup"><span data-stu-id="74ede-250">**Accumulate** is executed once for each value.</span></span> <span data-ttu-id="74ede-251">Det ger hello huvudsakliga funktioner för hello aggregering algoritmen.</span><span class="sxs-lookup"><span data-stu-id="74ede-251">It provides hello main functionality for hello aggregation algorithm.</span></span> <span data-ttu-id="74ede-252">Det kan vara används tooaggregate värden med olika datatyper som definieras under klassarv.</span><span class="sxs-lookup"><span data-stu-id="74ede-252">It can be used tooaggregate values with various data types that are defined during class inheritance.</span></span> <span data-ttu-id="74ede-253">Använder två parametrar av varierande datatyper.</span><span class="sxs-lookup"><span data-stu-id="74ede-253">It can accept two parameters of variable data types.</span></span>
* <span data-ttu-id="74ede-254">**Avsluta** körs en gång per aggregering grupp hello slutet av bearbetning toooutput hello resultatet för varje grupp.</span><span class="sxs-lookup"><span data-stu-id="74ede-254">**Terminate** is executed once per aggregation group at hello end of processing toooutput hello result for each group.</span></span>

<span data-ttu-id="74ede-255">toodeclare rätt indata och utdata-datatyperna, Använd hello klassdefinitionen enligt följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-255">toodeclare correct input and output data types, use hello class definition as follows:</span></span>

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* <span data-ttu-id="74ede-256">T1: Första parametern tooaccumulate</span><span class="sxs-lookup"><span data-stu-id="74ede-256">T1: First parameter tooaccumulate</span></span>
* <span data-ttu-id="74ede-257">T2: Första parametern tooaccumulate</span><span class="sxs-lookup"><span data-stu-id="74ede-257">T2: First parameter tooaccumulate</span></span>
* <span data-ttu-id="74ede-258">TResult: Returtypen för Avsluta</span><span class="sxs-lookup"><span data-stu-id="74ede-258">TResult: Return type of terminate</span></span>

<span data-ttu-id="74ede-259">Exempel:</span><span class="sxs-lookup"><span data-stu-id="74ede-259">For example:</span></span>

```
public class GuidAggregate : IAggregate<string, int, int>
```

<span data-ttu-id="74ede-260">eller</span><span class="sxs-lookup"><span data-stu-id="74ede-260">or</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a><span data-ttu-id="74ede-261">Använd UDAGG i U-SQL</span><span class="sxs-lookup"><span data-stu-id="74ede-261">Use UDAGG in U-SQL</span></span>
<span data-ttu-id="74ede-262">toouse UDAGG, först definiera den i den bakomliggande koden eller refereras till från befintliga hello-programmering DLL enligt tidigare diskussion.</span><span class="sxs-lookup"><span data-stu-id="74ede-262">toouse UDAGG, first define it in code-behind or reference it from hello existent programmability DLL as discussed earlier.</span></span>

<span data-ttu-id="74ede-263">Använd sedan hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="74ede-263">Then use hello following syntax:</span></span>

```
AGG<UDAGG_functionname>(param1,param2)
```

<span data-ttu-id="74ede-264">Här är ett exempel på UDAGG:</span><span class="sxs-lookup"><span data-stu-id="74ede-264">Here is an example of UDAGG:</span></span>

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

<span data-ttu-id="74ede-265">Och grundläggande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="74ede-265">And base U-SQL script:</span></span>

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

<span data-ttu-id="74ede-266">I detta scenario användningsfall sammanfoga vi klass GUID för hello specifika användare.</span><span class="sxs-lookup"><span data-stu-id="74ede-266">In this use-case scenario, we concatenate class GUIDs for hello specific users.</span></span>

## <a name="use-user-defined-objects-udo"></a><span data-ttu-id="74ede-267">Använda användardefinierade objekt: UDO</span><span class="sxs-lookup"><span data-stu-id="74ede-267">Use user-defined objects: UDO</span></span>
<span data-ttu-id="74ede-268">U-SQL kan toodefine anpassade programmering objekt, vilket kallas användardefinierade objekt eller UDO.</span><span class="sxs-lookup"><span data-stu-id="74ede-268">U-SQL enables you toodefine custom programmability objects, which are called user-defined objects or UDO.</span></span>

<span data-ttu-id="74ede-269">hello nedan följer en lista över UDO i U-SQL:</span><span class="sxs-lookup"><span data-stu-id="74ede-269">hello following is a list of UDO in U-SQL:</span></span>

* <span data-ttu-id="74ede-270">Användardefinierade juicepressar</span><span class="sxs-lookup"><span data-stu-id="74ede-270">User-defined extractors</span></span>
    * <span data-ttu-id="74ede-271">Extrahera rad för rad</span><span class="sxs-lookup"><span data-stu-id="74ede-271">Extract row by row</span></span>
    * <span data-ttu-id="74ede-272">Använda tooimplement data extrahering från anpassade strukturerade filer</span><span class="sxs-lookup"><span data-stu-id="74ede-272">Used tooimplement data extraction from custom structured files</span></span>

* <span data-ttu-id="74ede-273">Användardefinierade outputters</span><span class="sxs-lookup"><span data-stu-id="74ede-273">User-defined outputters</span></span>
    * <span data-ttu-id="74ede-274">Utdata rad för rad</span><span class="sxs-lookup"><span data-stu-id="74ede-274">Output row by row</span></span>
    * <span data-ttu-id="74ede-275">Använda toooutput anpassade datatyper eller anpassade filformat</span><span class="sxs-lookup"><span data-stu-id="74ede-275">Used toooutput custom data types or custom file formats</span></span>

* <span data-ttu-id="74ede-276">Användardefinierade processorer</span><span class="sxs-lookup"><span data-stu-id="74ede-276">User-defined processors</span></span>
    * <span data-ttu-id="74ede-277">Tar en rad och skapar en rad</span><span class="sxs-lookup"><span data-stu-id="74ede-277">Take one row and produce one row</span></span>
    * <span data-ttu-id="74ede-278">Använda tooreduce hello antalet kolumner eller skapa nya kolumner med värden som härleds från en befintlig kolumn</span><span class="sxs-lookup"><span data-stu-id="74ede-278">Used tooreduce hello number of columns or produce new columns with values that are derived from an existing column set</span></span>

* <span data-ttu-id="74ede-279">Användardefinierade appliers</span><span class="sxs-lookup"><span data-stu-id="74ede-279">User-defined appliers</span></span>
    * <span data-ttu-id="74ede-280">Tar en rad och skapar 0 toon rader</span><span class="sxs-lookup"><span data-stu-id="74ede-280">Take one row and produce 0 toon rows</span></span>
    * <span data-ttu-id="74ede-281">Används med yttre/mellan Använd</span><span class="sxs-lookup"><span data-stu-id="74ede-281">Used with OUTER/CROSS APPLY</span></span>

* <span data-ttu-id="74ede-282">Användardefinierade combiners</span><span class="sxs-lookup"><span data-stu-id="74ede-282">User-defined combiners</span></span>
    * <span data-ttu-id="74ede-283">Kombinerar raduppsättningar--användardefinierade kopplingar</span><span class="sxs-lookup"><span data-stu-id="74ede-283">Combines rowsets--user-defined JOINs</span></span>

* <span data-ttu-id="74ede-284">Användardefinierade förminskningsapparater</span><span class="sxs-lookup"><span data-stu-id="74ede-284">User-defined reducers</span></span>
    * <span data-ttu-id="74ede-285">Tar n rader och skapar en rad</span><span class="sxs-lookup"><span data-stu-id="74ede-285">Take n rows and produce one row</span></span>
    * <span data-ttu-id="74ede-286">Används tooreduce hello antalet rader</span><span class="sxs-lookup"><span data-stu-id="74ede-286">Used tooreduce hello number of rows</span></span>

<span data-ttu-id="74ede-287">UDO kallas vanligtvis explicit i U-SQL-skript som en del av hello följande U-SQL-uttryck:</span><span class="sxs-lookup"><span data-stu-id="74ede-287">UDO is typically called explicitly in U-SQL script as part of hello following U-SQL statements:</span></span>

* <span data-ttu-id="74ede-288">EXTRAHERA</span><span class="sxs-lookup"><span data-stu-id="74ede-288">EXTRACT</span></span>
* <span data-ttu-id="74ede-289">UTDATA</span><span class="sxs-lookup"><span data-stu-id="74ede-289">OUTPUT</span></span>
* <span data-ttu-id="74ede-290">PROCESSEN</span><span class="sxs-lookup"><span data-stu-id="74ede-290">PROCESS</span></span>
* <span data-ttu-id="74ede-291">KOMBINERA</span><span class="sxs-lookup"><span data-stu-id="74ede-291">COMBINE</span></span>
* <span data-ttu-id="74ede-292">MINSKA</span><span class="sxs-lookup"><span data-stu-id="74ede-292">REDUCE</span></span>

> [!NOTE]  
> <span data-ttu-id="74ede-293">UDOS är begränsad tooconsume 0,5 Gb minne.</span><span class="sxs-lookup"><span data-stu-id="74ede-293">UDO’s are limited tooconsume 0.5Gb memory.</span></span>  <span data-ttu-id="74ede-294">Den här begränsningen minne gäller inte toolocal körningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-294">This memory limitation does not apply toolocal executions.</span></span>

## <a name="use-user-defined-extractors"></a><span data-ttu-id="74ede-295">Använda användardefinierade juicepressar</span><span class="sxs-lookup"><span data-stu-id="74ede-295">Use user-defined extractors</span></span>
<span data-ttu-id="74ede-296">U-SQL kan tooimport externa data med hjälp av en EXTRAHERA-instruktion.</span><span class="sxs-lookup"><span data-stu-id="74ede-296">U-SQL allows you tooimport external data by using an EXTRACT statement.</span></span> <span data-ttu-id="74ede-297">Uttrycket EXTRAHERA kan använda inbyggda UDO juicepressar:</span><span class="sxs-lookup"><span data-stu-id="74ede-297">An EXTRACT statement can use built-in UDO extractors:</span></span>  

* <span data-ttu-id="74ede-298">*Extractors.Text()*: ger extrahering från avgränsade textfiler i olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-298">*Extractors.Text()*: Provides extraction from delimited text files of different encodings.</span></span>

* <span data-ttu-id="74ede-299">*Extractors.Csv()*: ger extrahering från CSV-filer (CSV) av olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-299">*Extractors.Csv()*: Provides extraction from comma-separated value (CSV) files of different encodings.</span></span>

* <span data-ttu-id="74ede-300">*Extractors.Tsv()*: ger extrahering från fliken med kommaseparerade värden (TVS) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-300">*Extractors.Tsv()*: Provides extraction from tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="74ede-301">Det kan vara användbart toodevelop anpassade extraktor.</span><span class="sxs-lookup"><span data-stu-id="74ede-301">It can be useful toodevelop a custom extractor.</span></span> <span data-ttu-id="74ede-302">Det kan vara användbart när data importeras om vi vill toodo någon hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="74ede-302">This can be helpful during data import if we want toodo any of hello following tasks:</span></span>

* <span data-ttu-id="74ede-303">Ändra inkommande data genom att dela kolumner och ändra enskilda värden.</span><span class="sxs-lookup"><span data-stu-id="74ede-303">Modify input data by splitting columns and modifying individual values.</span></span> <span data-ttu-id="74ede-304">hello PROCESSOR funktioner är bättre om du vill kombinera kolumner.</span><span class="sxs-lookup"><span data-stu-id="74ede-304">hello PROCESSOR functionality is better for combining columns.</span></span>
* <span data-ttu-id="74ede-305">Parsa Ostrukturerade data, till exempel webbsidor och e-postmeddelanden eller delvis Ostrukturerade data, till exempel XML/JSON.</span><span class="sxs-lookup"><span data-stu-id="74ede-305">Parse unstructured data such as Web pages and emails, or semi-unstructured data such as XML/JSON.</span></span>
* <span data-ttu-id="74ede-306">Tolka data i kodning stöds inte.</span><span class="sxs-lookup"><span data-stu-id="74ede-306">Parse data in unsupported encoding.</span></span>

<span data-ttu-id="74ede-307">toodefine en användardefinierad extraktor eller Undanta, behöver vi toocreate en `IExtractor` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-307">toodefine a user-defined extractor, or UDE, we need toocreate an `IExtractor` interface.</span></span> <span data-ttu-id="74ede-308">Alla indata parametrar toohello extraktor som kolumn/rad avgränsare och kodning, måste toobe som definierats i hello konstruktorn av hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="74ede-308">All input parameters toohello extractor, such as column/row delimiters, and encoding, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="74ede-309">Hej `IExtractor` interface måste även innehålla en definition för hello `IEnumerable<IRow>` åsidosätta på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="74ede-309">hello `IExtractor`  interface should also contain a definition for hello `IEnumerable<IRow>` override as follows:</span></span>

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

<span data-ttu-id="74ede-310">Hej **SqlUserDefinedExtractor** attributet anger att hello typ ska registreras som en användardefinierad extraktor.</span><span class="sxs-lookup"><span data-stu-id="74ede-310">hello **SqlUserDefinedExtractor** attribute indicates that hello type should be registered as a user-defined extractor.</span></span> <span data-ttu-id="74ede-311">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-311">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-312">SqlUserDefinedExtractor är ett valfritt attribut för Undanta definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-312">SqlUserDefinedExtractor is an optional attribute for UDE definition.</span></span> <span data-ttu-id="74ede-313">Det använda toodefine AtomicFileProcessing egenskapen för hello Undanta objekt.</span><span class="sxs-lookup"><span data-stu-id="74ede-313">It used toodefine AtomicFileProcessing property for hello UDE object.</span></span>

* <span data-ttu-id="74ede-314">bool AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="74ede-314">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="74ede-315">**SANT** = anger att den här extraktor kräver atomiska indatafiler (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="74ede-315">**true** = Indicates that this extractor requires atomic input files (JSON, XML, ...)</span></span>
* <span data-ttu-id="74ede-316">**FALSKT** = anger den här extraktor kan hantera delade / distribuerade filer (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="74ede-316">**false** = Indicates that this extractor can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="74ede-317">hello huvudsakliga Undanta programmering objekt är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="74ede-317">hello main UDE programmability objects are **input** and **output**.</span></span> <span data-ttu-id="74ede-318">Hej indataobjektet är används tooenumerate indata som `IUnstructuredReader`.</span><span class="sxs-lookup"><span data-stu-id="74ede-318">hello input object is used tooenumerate input data as `IUnstructuredReader`.</span></span> <span data-ttu-id="74ede-319">hello utdata objektet är används tooset utdata till följd av hello extraktor aktivitet.</span><span class="sxs-lookup"><span data-stu-id="74ede-319">hello output object is used tooset output data as a result of hello extractor activity.</span></span>

<span data-ttu-id="74ede-320">hello indata nås via `System.IO.Stream` och `System.IO.StreamReader`.</span><span class="sxs-lookup"><span data-stu-id="74ede-320">hello input data is accessed through `System.IO.Stream` and `System.IO.StreamReader`.</span></span>

<span data-ttu-id="74ede-321">För indatakolumnerna uppräkningen dela vi först hello Indataströmmen med hjälp av en avgränsare för en rad.</span><span class="sxs-lookup"><span data-stu-id="74ede-321">For input columns enumeration, we first split hello input stream by using a row delimiter.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

<span data-ttu-id="74ede-322">Sedan ytterligare delats inkommande raden kolumnen.</span><span class="sxs-lookup"><span data-stu-id="74ede-322">Then, further split input row into column parts.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

<span data-ttu-id="74ede-323">tooset utdata använder vi hello `output.Set` metod.</span><span class="sxs-lookup"><span data-stu-id="74ede-323">tooset output data, we use hello `output.Set` method.</span></span>

<span data-ttu-id="74ede-324">Det är viktigt toounderstand som hello anpassade extraktor endast matar ut kolumner och värden som definieras med hello utdata.</span><span class="sxs-lookup"><span data-stu-id="74ede-324">It's important toounderstand that hello custom extractor only outputs columns and values that are defined with hello output.</span></span> <span data-ttu-id="74ede-325">Ange metodanrop.</span><span class="sxs-lookup"><span data-stu-id="74ede-325">Set method call.</span></span>

```
output.Set<string>(count, part);
```

<span data-ttu-id="74ede-326">hello faktiska extraktor utdata utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="74ede-326">hello actual extractor output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="74ede-327">Följande är hello extraktor exempel:</span><span class="sxs-lookup"><span data-stu-id="74ede-327">Following is hello extractor example:</span></span>

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

<span data-ttu-id="74ede-328">I detta scenario användningsfall hello extraktor återskapar hello GUID för ”guid” kolumn och konverterar hello värdena för ”användare” kolumnen tooupper fallet.</span><span class="sxs-lookup"><span data-stu-id="74ede-328">In this use-case scenario, hello extractor regenerates hello GUID for “guid” column and converts hello values of “user” column tooupper case.</span></span> <span data-ttu-id="74ede-329">Anpassade juicepressar kan ge mer komplicerad resultat genom att parsa indata och ändrar den.</span><span class="sxs-lookup"><span data-stu-id="74ede-329">Custom extractors can produce more complicated results by parsing input data and manipulating it.</span></span>

<span data-ttu-id="74ede-330">Nedan följer grundläggande U-SQL-skript som använder en anpassad extraktor:</span><span class="sxs-lookup"><span data-stu-id="74ede-330">Following is base U-SQL script that uses a custom extractor:</span></span>

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

## <a name="use-user-defined-outputters"></a><span data-ttu-id="74ede-331">Använda användardefinierade outputters</span><span class="sxs-lookup"><span data-stu-id="74ede-331">Use user-defined outputters</span></span>
<span data-ttu-id="74ede-332">Användardefinierade outputter är en annan U-SQL-UDO som gör att du tooextend inbyggd U-SQL-funktion.</span><span class="sxs-lookup"><span data-stu-id="74ede-332">User-defined outputter is another U-SQL UDO that allows you tooextend built-in U-SQL functionality.</span></span> <span data-ttu-id="74ede-333">Liknande toohello extraktor det finns flera inbyggda outputters.</span><span class="sxs-lookup"><span data-stu-id="74ede-333">Similar toohello extractor, there are several built-in outputters.</span></span>

* <span data-ttu-id="74ede-334">*Outputters.Text()*: skriver data toodelimited textfiler i olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-334">*Outputters.Text()*: Writes data toodelimited text files of different encodings.</span></span>
* <span data-ttu-id="74ede-335">*Outputters.Csv()*: skriver data toocomma med kommaseparerade värden (CSV) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-335">*Outputters.Csv()*: Writes data toocomma-separated value (CSV) files of different encodings.</span></span>
* <span data-ttu-id="74ede-336">*Outputters.Tsv()*: skriver data tootab med kommaseparerade värden (TVS) filer med olika kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-336">*Outputters.Tsv()*: Writes data tootab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="74ede-337">Anpassade outputter tillåter toowrite data i ett anpassat definierade format.</span><span class="sxs-lookup"><span data-stu-id="74ede-337">Custom outputter allows you toowrite data in a custom defined format.</span></span> <span data-ttu-id="74ede-338">Detta kan vara användbart för hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="74ede-338">This can be useful for hello following tasks:</span></span>

* <span data-ttu-id="74ede-339">Skrivning toosemi strukturerade eller Ostrukturerade datafiler.</span><span class="sxs-lookup"><span data-stu-id="74ede-339">Writing data toosemi-structured or unstructured files.</span></span>
* <span data-ttu-id="74ede-340">Stöd för skrivning av data inte kodningar.</span><span class="sxs-lookup"><span data-stu-id="74ede-340">Writing data not supported encodings.</span></span>
* <span data-ttu-id="74ede-341">Ändra utdata eller lägga till anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="74ede-341">Modifying output data or adding custom attributes.</span></span>

<span data-ttu-id="74ede-342">toodefine användardefinierade outputter måste toocreate hello `IOutputter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-342">toodefine user-defined outputter, we need toocreate hello `IOutputter` interface.</span></span>

<span data-ttu-id="74ede-343">Följande är ett grundläggande hello `IOutputter` klassen implementering:</span><span class="sxs-lookup"><span data-stu-id="74ede-343">Following is hello base `IOutputter` class implementation:</span></span>

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

<span data-ttu-id="74ede-344">Alla indata parametrar toohello outputter som behöver toobe som definierats i hello konstruktorn för klassen hello kolumn/rad avgränsare, kodning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="74ede-344">All input parameters toohello outputter, such as column/row delimiters, encoding, and so on, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="74ede-345">Hej `IOutputter` interface måste även innehålla en definition för `void Output` åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="74ede-345">hello `IOutputter` interface should also contain a definition for `void Output` override.</span></span> <span data-ttu-id="74ede-346">hello attributet `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` (valfritt) kan anges för behandling av atomiska.</span><span class="sxs-lookup"><span data-stu-id="74ede-346">hello attribute `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` can optionally be set for atomic file processing.</span></span> <span data-ttu-id="74ede-347">Mer information finns i hello följande information.</span><span class="sxs-lookup"><span data-stu-id="74ede-347">For more information, see hello following details.</span></span>

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

* <span data-ttu-id="74ede-348">`Output`kallas för varje inkommande rad.</span><span class="sxs-lookup"><span data-stu-id="74ede-348">`Output` is called for each input row.</span></span> <span data-ttu-id="74ede-349">Den returnerar hello `IUnstructuredWriter output` raduppsättningen.</span><span class="sxs-lookup"><span data-stu-id="74ede-349">It returns hello `IUnstructuredWriter output` rowset.</span></span>
* <span data-ttu-id="74ede-350">hello konstruktorn klassen används toopass parametrar toohello användardefinierade outputter.</span><span class="sxs-lookup"><span data-stu-id="74ede-350">hello Constructor class is used toopass parameters toohello user-defined outputter.</span></span>
* <span data-ttu-id="74ede-351">`Close`används toooptionally åsidosätta toorelease dyra tillstånd eller avgöra när hello sista raden har skrivits.</span><span class="sxs-lookup"><span data-stu-id="74ede-351">`Close` is used toooptionally override toorelease expensive state or determine when hello last row was written.</span></span>

<span data-ttu-id="74ede-352">**SqlUserDefinedOutputter** attributet anger att hello typ ska registreras som en användardefinierad outputter.</span><span class="sxs-lookup"><span data-stu-id="74ede-352">**SqlUserDefinedOutputter** attribute indicates that hello type should be registered as a user-defined outputter.</span></span> <span data-ttu-id="74ede-353">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-353">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-354">SqlUserDefinedOutputter är ett valfritt attribut för en användardefinierad outputter definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-354">SqlUserDefinedOutputter is an optional attribute for a user-defined outputter definition.</span></span> <span data-ttu-id="74ede-355">Den används toodefine hello AtomicFileProcessing egenskapen.</span><span class="sxs-lookup"><span data-stu-id="74ede-355">It's used toodefine hello AtomicFileProcessing property.</span></span>

* <span data-ttu-id="74ede-356">bool AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="74ede-356">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="74ede-357">**SANT** = anger att den här outputter kräver atomiska utdatafilerna (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="74ede-357">**true** = Indicates that this outputter requires atomic output files (JSON, XML, ...)</span></span>
* <span data-ttu-id="74ede-358">**FALSKT** = anger den här outputter kan hantera delade / distribuerade filer (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="74ede-358">**false** = Indicates that this outputter can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="74ede-359">hello huvudsakliga programmering objekt är **raden** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="74ede-359">hello main programmability objects are **row** and **output**.</span></span> <span data-ttu-id="74ede-360">Hej **raden** objektet är används tooenumerate utdata som `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-360">hello **row** object is used tooenumerate output data as `IRow` interface.</span></span> <span data-ttu-id="74ede-361">**Utdata** är används tooset toohello mål utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="74ede-361">**Output** is used tooset output data toohello target file.</span></span>

<span data-ttu-id="74ede-362">hello utdata nås via hello `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-362">hello output data is accessed through hello `IRow` interface.</span></span> <span data-ttu-id="74ede-363">Utdata skickas en rad i taget.</span><span class="sxs-lookup"><span data-stu-id="74ede-363">Output data is passed a row at a time.</span></span>

<span data-ttu-id="74ede-364">hello enskilda värden räknas upp genom att anropa hello Get-metoden för hello IRow-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="74ede-364">hello individual values are enumerated by calling hello Get method of hello IRow interface:</span></span>

```
row.Get<string>("column_name")
```

<span data-ttu-id="74ede-365">Enskilda kolumnnamn kan fastställas genom att anropa `row.Schema`:</span><span class="sxs-lookup"><span data-stu-id="74ede-365">Individual column names can be determined by calling `row.Schema`:</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="74ede-366">Den här metoden kan du toobuild en flexibel outputter för alla metadata-scheman.</span><span class="sxs-lookup"><span data-stu-id="74ede-366">This approach enables you toobuild a flexible outputter for any metadata schema.</span></span>

<span data-ttu-id="74ede-367">hello utdata skrivs toofile med hjälp av `System.IO.StreamWriter`.</span><span class="sxs-lookup"><span data-stu-id="74ede-367">hello output data is written toofile by using `System.IO.StreamWriter`.</span></span> <span data-ttu-id="74ede-368">hello dataströmmen är angiven för`output.BaseStrea` som en del av `IUnstructuredWriter output`.</span><span class="sxs-lookup"><span data-stu-id="74ede-368">hello stream parameter is set too`output.BaseStrea` as part of `IUnstructuredWriter output`.</span></span>

<span data-ttu-id="74ede-369">Observera att det är viktigt tooflush hello data buffert toohello fil efter varje iteration raden.</span><span class="sxs-lookup"><span data-stu-id="74ede-369">Note that it's important tooflush hello data buffer toohello file after each row iteration.</span></span> <span data-ttu-id="74ede-370">Dessutom hello `StreamWriter` objektet måste användas med hello tillgänglig attributet aktiverad (standard) och hello **med** nyckelordet:</span><span class="sxs-lookup"><span data-stu-id="74ede-370">In addition, hello `StreamWriter` object must be used with hello Disposable attribute enabled (default) and with hello **using** keyword:</span></span>

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

<span data-ttu-id="74ede-371">Annars kan anropa uttryckligen Flush() metoden efter varje iteration.</span><span class="sxs-lookup"><span data-stu-id="74ede-371">Otherwise, call Flush() method explicitly after each iteration.</span></span> <span data-ttu-id="74ede-372">Detta visar vi i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="74ede-372">We show this in hello following example.</span></span>

### <a name="set-headers-and-footers-for-user-defined-outputter"></a><span data-ttu-id="74ede-373">Ange sidhuvuden och sidfötter för användardefinierade outputter</span><span class="sxs-lookup"><span data-stu-id="74ede-373">Set headers and footers for user-defined outputter</span></span>
<span data-ttu-id="74ede-374">tooset ett huvud använda enstaka upprepning körning flödet.</span><span class="sxs-lookup"><span data-stu-id="74ede-374">tooset a header, use single iteration execution flow.</span></span>

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

<span data-ttu-id="74ede-375">Hej koden i hello först `if (isHeaderRow)` block körs bara en gång.</span><span class="sxs-lookup"><span data-stu-id="74ede-375">hello code in hello first `if (isHeaderRow)` block is executed only once.</span></span>

<span data-ttu-id="74ede-376">Använd för hello sidfot hello referens toohello instans av `System.IO.Stream` objekt (`output.BaseStream`).</span><span class="sxs-lookup"><span data-stu-id="74ede-376">For hello footer, use hello reference toohello instance of `System.IO.Stream` object (`output.BaseStream`).</span></span> <span data-ttu-id="74ede-377">Skriva hello sidfot i hello Close() metod för hello `IOutputter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-377">Write hello footer in hello Close() method of hello `IOutputter` interface.</span></span>  <span data-ttu-id="74ede-378">(Mer information finns i följande exempel hello.)</span><span class="sxs-lookup"><span data-stu-id="74ede-378">(For more information, see hello following example.)</span></span>

<span data-ttu-id="74ede-379">Följande är ett exempel på en användardefinierad outputter:</span><span class="sxs-lookup"><span data-stu-id="74ede-379">Following is an example of a user-defined outputter:</span></span>

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

<span data-ttu-id="74ede-380">Och grundläggande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="74ede-380">And U-SQL base script:</span></span>

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

<span data-ttu-id="74ede-381">Det här är en HTML-outputter som skapar en HTML-fil med tabelldata.</span><span class="sxs-lookup"><span data-stu-id="74ede-381">This is an HTML outputter, which creates an HTML file with table data.</span></span>

### <a name="call-outputter-from-u-sql-base-script"></a><span data-ttu-id="74ede-382">Anropa outputter från grundläggande U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="74ede-382">Call outputter from U-SQL base script</span></span>
<span data-ttu-id="74ede-383">toocall en anpassad outputter från hello grundläggande U-SQL-skript, hello ny instans av hello outputter objektet har toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="74ede-383">toocall a custom outputter from hello base U-SQL script, hello new instance of hello outputter object has toobe created.</span></span>

```sql
OUTPUT @rs0 too@output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="74ede-384">Skapa en instans av hello tooavoid objekt i grundläggande skript kan vi skapa en funktion omslutning som visas i det tidigare exemplet:</span><span class="sxs-lookup"><span data-stu-id="74ede-384">tooavoid creating an instance of hello object in base script, we can create a function wrapper, as shown in our earlier example:</span></span>

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

<span data-ttu-id="74ede-385">I det här fallet hello ursprungliga anropet ser ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-385">In this case, hello original call looks like hello following:</span></span>

```
OUTPUT @rs0 
too@output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a><span data-ttu-id="74ede-386">Använda användardefinierade processorer</span><span class="sxs-lookup"><span data-stu-id="74ede-386">Use user-defined processors</span></span>
<span data-ttu-id="74ede-387">Användardefinierade är UDP, en typ av U-SQL-UDO som gör att du tooprocess hello inkommande rader genom att använda programmeringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="74ede-387">User-defined processor, or UDP, is a type of U-SQL UDO that enables you tooprocess hello incoming rows by applying programmability features.</span></span> <span data-ttu-id="74ede-388">UDP möjliggör toocombine kolumner, ändra värdena och lägga till nya kolumner, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="74ede-388">UDP enables you toocombine columns, modify values, and add new columns if necessary.</span></span> <span data-ttu-id="74ede-389">I princip hjälper tooprocess dataelement för tooproduce krävs en raduppsättning.</span><span class="sxs-lookup"><span data-stu-id="74ede-389">Basically, it helps tooprocess a rowset tooproduce required data elements.</span></span>

<span data-ttu-id="74ede-390">toodefine en UDP, behöver vi toocreate en `IProcessor` gränssnittet med hello `SqlUserDefinedProcessor` attribut som är valfritt för UDP.</span><span class="sxs-lookup"><span data-stu-id="74ede-390">toodefine a UDP, we need toocreate an `IProcessor` interface with hello `SqlUserDefinedProcessor` attribute, which is optional for UDP.</span></span>

<span data-ttu-id="74ede-391">Det här gränssnittet ska innehålla hello definitionen för hello `IRow` gränssnittet raduppsättningen åsidosätta, enligt följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="74ede-391">This interface should contain hello definition for hello `IRow` interface rowset override, as shown in hello following example:</span></span>

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

<span data-ttu-id="74ede-392">**SqlUserDefinedProcessor** anger att hello typ ska registreras som en användardefinierad processor.</span><span class="sxs-lookup"><span data-stu-id="74ede-392">**SqlUserDefinedProcessor** indicates that hello type should be registered as a user-defined processor.</span></span> <span data-ttu-id="74ede-393">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-393">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-394">Hej SqlUserDefinedProcessor attributet **valfria** för UDP-definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-394">hello SqlUserDefinedProcessor attribute is **optional** for UDP definition.</span></span>

<span data-ttu-id="74ede-395">hello huvudsakliga programmering objekt är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="74ede-395">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="74ede-396">hello indataobjektet är att använda tooenumerate indatakolumnerna och utdata tooset utdata till följd av hello processoraktivitet.</span><span class="sxs-lookup"><span data-stu-id="74ede-396">hello input object is used tooenumerate input columns and output, and tooset output data as a result of hello processor activity.</span></span>

<span data-ttu-id="74ede-397">För indatakolumnerna uppräkningen vi använder hello `input.Get` metod.</span><span class="sxs-lookup"><span data-stu-id="74ede-397">For input columns enumeration, we use hello `input.Get` method.</span></span>

```
string column_name = input.Get<string>("column_name");
```

<span data-ttu-id="74ede-398">Hej parametern för `input.Get` metoden är en kolumn som skickas som en del av hello `PRODUCE` -satsen i hello `PROCESS` instruktionen på grundläggande hello U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-398">hello parameter for `input.Get` method is a column that's passed as part of hello `PRODUCE` clause of hello `PROCESS` statement of hello U-SQL base script.</span></span> <span data-ttu-id="74ede-399">Vi behöver toouse hello rätt datatyp här.</span><span class="sxs-lookup"><span data-stu-id="74ede-399">We need toouse hello correct data type here.</span></span>

<span data-ttu-id="74ede-400">Använd hello för utdata `output.Set` metod.</span><span class="sxs-lookup"><span data-stu-id="74ede-400">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="74ede-401">Det är viktigt toonote att anpassade producent endast matar ut kolumner och värden som definieras med hello `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="74ede-401">It's important toonote that custom producer only outputs columns and values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", mycolumn);
```

<span data-ttu-id="74ede-402">hello faktiska utdata utlöses genom att anropa `return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="74ede-402">hello actual processor output is triggered by calling `return output.AsReadOnly();`.</span></span>

<span data-ttu-id="74ede-403">Följande är ett exempel på processor:</span><span class="sxs-lookup"><span data-stu-id="74ede-403">Following is a processor example:</span></span>

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

<span data-ttu-id="74ede-404">I detta scenario användningsfall genererar hello processor en ny kolumn som kallas ”full_description” genom att kombinera hello befintliga kolumner – i det här fallet ”användare” i versaler och ”des”.</span><span class="sxs-lookup"><span data-stu-id="74ede-404">In this use-case scenario, hello processor is generating a new column called “full_description” by combining hello existing columns--in this case, “user” in upper case, and “des”.</span></span> <span data-ttu-id="74ede-405">Den genererar ett GUID och returnerar hello ursprungliga och nya GUID-värden.</span><span class="sxs-lookup"><span data-stu-id="74ede-405">It also regenerates a GUID and returns hello original and new GUID values.</span></span>

<span data-ttu-id="74ede-406">Som du kan se hello föregående exempel kan du anropa C#-metoder under `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="74ede-406">As you can see from hello previous example, you can call C# methods during `output.Set` method call.</span></span>

<span data-ttu-id="74ede-407">Följande är ett exempel på grundläggande U-SQL-skript som använder en anpassad processor:</span><span class="sxs-lookup"><span data-stu-id="74ede-407">Following is an example of base U-SQL script that uses a custom processor:</span></span>

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

## <a name="use-user-defined-appliers"></a><span data-ttu-id="74ede-408">Använda användardefinierade appliers</span><span class="sxs-lookup"><span data-stu-id="74ede-408">Use user-defined appliers</span></span>
<span data-ttu-id="74ede-409">Ett U-SQL-användardefinierade applier kan tooinvoke anpassade C#-funktionen för varje rad som returneras av hello yttre tabelluttryck i en fråga.</span><span class="sxs-lookup"><span data-stu-id="74ede-409">A U-SQL user-defined applier enables you tooinvoke a custom C# function for each row that's returned by hello outer table expression of a query.</span></span> <span data-ttu-id="74ede-410">hello rätt indata ska utvärderas för varje rad från hello vänstra indata och hello rader som genereras kombineras hello slutversionen.</span><span class="sxs-lookup"><span data-stu-id="74ede-410">hello right input is evaluated for each row from hello left input, and hello rows that are produced are combined for hello final output.</span></span> <span data-ttu-id="74ede-411">hello lista med kolumner som genereras av hello APPLY-operatorn är hello kombination av hello uppsättning kolumner i hello vänster och hello höger indata.</span><span class="sxs-lookup"><span data-stu-id="74ede-411">hello list of columns that are produced by hello APPLY operator are hello combination of hello set of columns in hello left and hello right input.</span></span>

<span data-ttu-id="74ede-412">Användardefinierade applier anropas som en del av hello USQL Välj uttryck.</span><span class="sxs-lookup"><span data-stu-id="74ede-412">User-defined applier is being invoked as part of hello USQL SELECT expression.</span></span>

<span data-ttu-id="74ede-413">hello vanliga anropa toohello användardefinierade applier ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="74ede-413">hello typical call toohello user-defined applier looks like hello following:</span></span>

```
SELECT …
FROM …
CROSS APPLYis used toopass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

<span data-ttu-id="74ede-414">Mer information om hur du använder appliers i ett SELECT-uttryck finns [U-SQL väljer att välja mellan tillämpa och yttre tillämpa](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span><span class="sxs-lookup"><span data-stu-id="74ede-414">For more information about using appliers in a SELECT expression, see [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span></span>

<span data-ttu-id="74ede-415">hello användardefinierade applier basklass definition är följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-415">hello user-defined applier base class definition is as follows:</span></span>

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

<span data-ttu-id="74ede-416">toodefine en användardefinierad applier måste toocreate hello `IApplier` gränssnittet med hello [`SqlUserDefinedApplier`]-attribut som är valfri för en användardefinierad applier definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-416">toodefine a user-defined applier, we need toocreate hello `IApplier` interface with hello [`SqlUserDefinedApplier`] attribute, which is optional for a user-defined applier definition.</span></span>

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

* <span data-ttu-id="74ede-417">Tillämpa anropas för varje rad i hello yttre tabell.</span><span class="sxs-lookup"><span data-stu-id="74ede-417">Apply is called for each row of hello outer table.</span></span> <span data-ttu-id="74ede-418">Den returnerar hello `IUpdatableRow` utdata raduppsättningen.</span><span class="sxs-lookup"><span data-stu-id="74ede-418">It returns hello `IUpdatableRow` output rowset.</span></span>
* <span data-ttu-id="74ede-419">hello konstruktorn klassen är används toopass parametrar toohello användardefinierade applier.</span><span class="sxs-lookup"><span data-stu-id="74ede-419">hello Constructor class is used toopass parameters toohello user-defined applier.</span></span>

<span data-ttu-id="74ede-420">**SqlUserDefinedApplier** anger att hello typ ska registreras som en användardefinierad applier.</span><span class="sxs-lookup"><span data-stu-id="74ede-420">**SqlUserDefinedApplier** indicates that hello type should be registered as a user-defined applier.</span></span> <span data-ttu-id="74ede-421">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-421">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-422">**SqlUserDefinedApplier** är **valfria** för en användardefinierad applier definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-422">**SqlUserDefinedApplier** is **optional** for a user-defined applier definition.</span></span>


<span data-ttu-id="74ede-423">hello huvudsakliga programmering objekt är följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-423">hello main programmability objects are as follows:</span></span>

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

<span data-ttu-id="74ede-424">Inkommande raduppsättningar skickas `IRow` indata.</span><span class="sxs-lookup"><span data-stu-id="74ede-424">Input rowsets are passed as `IRow` input.</span></span> <span data-ttu-id="74ede-425">hello utdata rader genereras som `IUpdatableRow` utdata-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="74ede-425">hello output rows are generated as `IUpdatableRow` output interface.</span></span>

<span data-ttu-id="74ede-426">Enskilda kolumnnamn kan fastställas genom att anropa hello `IRow` Schema-metoden.</span><span class="sxs-lookup"><span data-stu-id="74ede-426">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="74ede-427">tooget hello faktiska värden från hello inkommande `IRow`, vi använda hello Get()-metoden i `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-427">tooget hello actual data values from hello incoming `IRow`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="74ede-428">Eller vi använda hello schemat kolumnnamn:</span><span class="sxs-lookup"><span data-stu-id="74ede-428">Or we use hello schema column name:</span></span>

```
row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="74ede-429">hello utdatavärden måste anges med `IUpdatableRow` utdata:</span><span class="sxs-lookup"><span data-stu-id="74ede-429">hello output values must be set with `IUpdatableRow` output:</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="74ede-430">Det är viktigt toounderstand att anpassade appliers endast utgående kolumner och värden som definieras med `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="74ede-430">It is important toounderstand that custom appliers only output columns and values that are defined with `output.Set` method call.</span></span>

<span data-ttu-id="74ede-431">hello faktiska utdata utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="74ede-431">hello actual output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="74ede-432">hello användardefinierade applier parametrar kan skickas toohello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="74ede-432">hello user-defined applier parameters can be passed toohello constructor.</span></span> <span data-ttu-id="74ede-433">Applier kan returnera en variabel antalet kolumner som behöver toobe som definierats under hello applier anrop i grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-433">Applier can return a variable number of columns that need toobe defined during hello applier call in base U-SQL Script.</span></span>

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

<span data-ttu-id="74ede-434">Här är hello användardefinierade applier exempel:</span><span class="sxs-lookup"><span data-stu-id="74ede-434">Here is hello user-defined applier example:</span></span>

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

<span data-ttu-id="74ede-435">Nedan följer hello grundläggande U-SQL-skript för den här användardefinierade applier:</span><span class="sxs-lookup"><span data-stu-id="74ede-435">Following is hello base U-SQL script for this user-defined applier:</span></span>

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

<span data-ttu-id="74ede-436">I den här Användarscenario, användardefinierade applier fungerar som en kommaavgränsad parser för hello bil flottan egenskaper.</span><span class="sxs-lookup"><span data-stu-id="74ede-436">In this use case scenario, user-defined applier acts as a comma-delimited value parser for hello car fleet properties.</span></span> <span data-ttu-id="74ede-437">Det ser ut som följande hello hello indatafilen rader:</span><span class="sxs-lookup"><span data-stu-id="74ede-437">hello input file rows look like hello following:</span></span>

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

<span data-ttu-id="74ede-438">Det är en typisk tabbavgränsad TVS fil med en kolumn för egenskaper som innehåller bil egenskaper, till exempel märke och modell.</span><span class="sxs-lookup"><span data-stu-id="74ede-438">It is a typical tab-delimited TSV file with a properties column that contains car properties such as make and model.</span></span> <span data-ttu-id="74ede-439">Dessa egenskaper måste vara parsade toohello tabellkolumner.</span><span class="sxs-lookup"><span data-stu-id="74ede-439">Those properties must be parsed toohello table columns.</span></span> <span data-ttu-id="74ede-440">Hej applier som har angetts kan du också toogenerate dynamiska antal egenskaper i hello leda raduppsättningen, baserat på hello-parameter som skickas.</span><span class="sxs-lookup"><span data-stu-id="74ede-440">hello applier that's provided also enables you toogenerate a dynamic number of properties in hello result rowset, based on hello parameter that's passed.</span></span> <span data-ttu-id="74ede-441">Du kan skapa alla egenskaper eller en specifik uppsättning endast egenskaper.</span><span class="sxs-lookup"><span data-stu-id="74ede-441">You can generate either all properties or a specific set of properties only.</span></span>

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

<span data-ttu-id="74ede-442">hello användardefinierade applier kan anropas som en ny instans av applier objekt:</span><span class="sxs-lookup"><span data-stu-id="74ede-442">hello user-defined applier can be called as a new instance of applier object:</span></span>

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

<span data-ttu-id="74ede-443">Eller med hello anrop av en wrapper standardmetod:</span><span class="sxs-lookup"><span data-stu-id="74ede-443">Or with hello invocation of a wrapper factory method:</span></span>

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a><span data-ttu-id="74ede-444">Använda användardefinierade combiners</span><span class="sxs-lookup"><span data-stu-id="74ede-444">Use user-defined combiners</span></span>
<span data-ttu-id="74ede-445">Användardefinierade combiner eller UDC, kan du toocombine rader från vänster och höger raduppsättningar baserat på egen kod.</span><span class="sxs-lookup"><span data-stu-id="74ede-445">User-defined combiner, or UDC, enables you toocombine rows from left and right rowsets, based on custom logic.</span></span> <span data-ttu-id="74ede-446">Användardefinierade combiner används med KOMBINERA uttryck.</span><span class="sxs-lookup"><span data-stu-id="74ede-446">User-defined combiner is used with COMBINE expression.</span></span>

<span data-ttu-id="74ede-447">En combiner anropas med hello KOMBINERA uttryck som innehåller hello nödvändig information om både inkommande raduppsättningar hello, hello gruppering kolumner, hello förväntat resultat schemat och ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="74ede-447">A combiner is being invoked with hello COMBINE expression that provides hello necessary information about both hello input rowsets, hello grouping columns, hello expected result schema, and additional information.</span></span>

<span data-ttu-id="74ede-448">toocall en combiner i ett grundläggande U-SQL-skript, vi använda hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="74ede-448">toocall a combiner in a base U-SQL script, we use hello following syntax:</span></span>

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

<span data-ttu-id="74ede-449">Mer information finns i [KOMBINERA uttryck (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span><span class="sxs-lookup"><span data-stu-id="74ede-449">For more information, see [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span></span>

<span data-ttu-id="74ede-450">toodefine en användardefinierad combiner måste toocreate hello `ICombiner` gränssnittet med hello [`SqlUserDefinedCombiner`]-attribut som är valfri för en användardefinierad Combiner definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-450">toodefine a user-defined combiner, we need toocreate hello `ICombiner` interface with hello [`SqlUserDefinedCombiner`] attribute, which is optional for a user-defined Combiner definition.</span></span>

<span data-ttu-id="74ede-451">Base `ICombiner` klassen definition:</span><span class="sxs-lookup"><span data-stu-id="74ede-451">Base `ICombiner` class definition:</span></span>

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

<span data-ttu-id="74ede-452">Hej anpassade implementering av en `ICombiner` gränssnittet ska innehålla hello definitionen för en `IEnumerable<IRow>` kombinera åsidosättning.</span><span class="sxs-lookup"><span data-stu-id="74ede-452">hello custom implementation of an `ICombiner` interface should contain hello definition for an `IEnumerable<IRow>` Combine override.</span></span>

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

<span data-ttu-id="74ede-453">Hej **SqlUserDefinedCombiner** attributet anger att hello typ ska registreras som en användardefinierad combiner.</span><span class="sxs-lookup"><span data-stu-id="74ede-453">hello **SqlUserDefinedCombiner** attribute indicates that hello type should be registered as a user-defined combiner.</span></span> <span data-ttu-id="74ede-454">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-454">This class cannot be inherited.</span></span>

<span data-ttu-id="74ede-455">**SqlUserDefinedCombiner** är används toodefine hello Combiner egenskap.</span><span class="sxs-lookup"><span data-stu-id="74ede-455">**SqlUserDefinedCombiner** is used toodefine hello Combiner mode property.</span></span> <span data-ttu-id="74ede-456">Det är ett valfritt attribut för en användardefinierad combiner definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-456">It is an optional attribute for a user-defined combiner definition.</span></span>

<span data-ttu-id="74ede-457">CombinerMode läge</span><span class="sxs-lookup"><span data-stu-id="74ede-457">CombinerMode     Mode</span></span>

<span data-ttu-id="74ede-458">CombinerMode uppräkning kan ta hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="74ede-458">CombinerMode enum can take hello following values:</span></span>

* <span data-ttu-id="74ede-459">Fullständig (0) varje rad i utdata potentiellt beror på alla hello inkommande rader från vänster och höger med hello samma nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="74ede-459">Full  (0) Every output row potentially depends on all hello input rows from left and right       with hello same key value.</span></span>

* <span data-ttu-id="74ede-460">Vänster (1) varje rad i utdata beror på en inkommande rad från hello vänster (och potentiellt alla rader från hello hello med samma nyckelvärde).</span><span class="sxs-lookup"><span data-stu-id="74ede-460">Left  (1) Every output row depends on a single input row from hello left (and potentially all rows       from hello right with hello same key value).</span></span>

* <span data-ttu-id="74ede-461">Höger (2) varje rad i utdata beror på en inkommande rad från hello rätt (och potentiellt alla rader från hello vänster med hello samma nyckelvärde).</span><span class="sxs-lookup"><span data-stu-id="74ede-461">Right (2)     Every output row depends on a single input row from hello right (and potentially all rows       from hello left with hello same key value).</span></span>

* <span data-ttu-id="74ede-462">Inre (3) varje rad i utdata beror på en enda inmatning rad från vänster och höger med hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="74ede-462">Inner (3) Every output row depends on a single input row from left and right with hello same value.</span></span>

<span data-ttu-id="74ede-463">Exempel: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span><span class="sxs-lookup"><span data-stu-id="74ede-463">Example:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span></span>


<span data-ttu-id="74ede-464">hello huvudsakliga programmering objekt är:</span><span class="sxs-lookup"><span data-stu-id="74ede-464">hello main programmability objects are:</span></span>

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

<span data-ttu-id="74ede-465">Inkommande raduppsättningar skickas **vänstra** och **rätt** `IRowset` typ av gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-465">Input rowsets are passed as **left** and **right** `IRowset` type of interface.</span></span> <span data-ttu-id="74ede-466">Båda raduppsättningar måste räknas upp för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="74ede-466">Both rowsets must be enumerated for processing.</span></span> <span data-ttu-id="74ede-467">Du kan räkna upp alla gränssnitt en gång, så att vi har tooenumerate och cachelagra den vid behov.</span><span class="sxs-lookup"><span data-stu-id="74ede-467">You can only enumerate each interface once, so we have tooenumerate and cache it if necessary.</span></span>

<span data-ttu-id="74ede-468">För cachelagring, kan vi skapa en lista\<T\> typ av minne som ett resultat av en LINQ Frågekörningen, specifikt lista <`IRow`>.</span><span class="sxs-lookup"><span data-stu-id="74ede-468">For caching purposes, we can create a List\<T\> type of memory structure as a result of a LINQ query execution, specifically List<`IRow`>.</span></span> <span data-ttu-id="74ede-469">hello anonym datatyp kan användas under uppräkning samt.</span><span class="sxs-lookup"><span data-stu-id="74ede-469">hello anonymous data type can be used during enumeration as well.</span></span>

<span data-ttu-id="74ede-470">Se [introduktion tooLINQ frågor (C#)](https://msdn.microsoft.com/library/bb397906.aspx) för mer information om LINQ-frågor och [IEnumerable\<T\> gränssnittet](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) mer information om IEnumerable\<T\> gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-470">See [Introduction tooLINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) for more information about LINQ queries, and [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) for more information about IEnumerable\<T\> interface.</span></span>

<span data-ttu-id="74ede-471">tooget hello faktiska värden från hello inkommande `IRowset`, vi använda hello Get()-metoden i `IRow` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-471">tooget hello actual data values from hello incoming `IRowset`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="74ede-472">Enskilda kolumnnamn kan fastställas genom att anropa hello `IRow` Schema-metoden.</span><span class="sxs-lookup"><span data-stu-id="74ede-472">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="74ede-473">Eller genom att använda hello schemat kolumnnamn:</span><span class="sxs-lookup"><span data-stu-id="74ede-473">Or by using hello schema column name:</span></span>

```
c# row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="74ede-474">hello allmän uppräkning med LINQ ser ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="74ede-474">hello general enumeration with LINQ looks like hello following:</span></span>

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

<span data-ttu-id="74ede-475">Efter att räkna upp båda raduppsättningar vi tooloop via alla rader.</span><span class="sxs-lookup"><span data-stu-id="74ede-475">After enumerating both rowsets, we are going tooloop through all rows.</span></span> <span data-ttu-id="74ede-476">För varje rad i hello vänstra raduppsättningen vi toofind alla rader som uppfyller våra combiner hello villkor.</span><span class="sxs-lookup"><span data-stu-id="74ede-476">For each row in hello left rowset, we are going toofind all rows that satisfy hello condition of our combiner.</span></span>

<span data-ttu-id="74ede-477">hello utdatavärden måste anges med `IUpdatableRow` utdata.</span><span class="sxs-lookup"><span data-stu-id="74ede-477">hello output values must be set with `IUpdatableRow` output.</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="74ede-478">hello faktiska utdata utlöses genom att anropa för`yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="74ede-478">hello actual output is triggered by calling too`yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="74ede-479">Följande är ett exempel på combiner:</span><span class="sxs-lookup"><span data-stu-id="74ede-479">Following is a combiner example:</span></span>

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

<span data-ttu-id="74ede-480">I detta scenario användningsfall skapar vi en analytics-rapport för hello återförsäljare.</span><span class="sxs-lookup"><span data-stu-id="74ede-480">In this use-case scenario, we are building an analytics report for hello retailer.</span></span> <span data-ttu-id="74ede-481">hello målet är toofind alla produkter som kostar mer än 20 000 och att sälja via hello webbplats snabbare än via hello reguljära återförsäljare inom ett bestämt tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="74ede-481">hello goal is toofind all products that cost more than $20,000 and that sell through hello website faster than through hello regular retailer within a certain time frame.</span></span>

<span data-ttu-id="74ede-482">Här är hello grundläggande U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-482">Here is hello base U-SQL script.</span></span> <span data-ttu-id="74ede-483">Du kan jämföra hello logik mellan en vanlig koppling och en combiner:</span><span class="sxs-lookup"><span data-stu-id="74ede-483">You can compare hello logic between a regular JOIN and a combiner:</span></span>

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

<span data-ttu-id="74ede-484">En användardefinierad combiner kan anropas som en ny instans av hello applier objekt:</span><span class="sxs-lookup"><span data-stu-id="74ede-484">A user-defined combiner can be called as a new instance of hello applier object:</span></span>

```
USING new MyNameSpace.MyCombiner();
```


<span data-ttu-id="74ede-485">Eller med hello anrop av en wrapper standardmetod:</span><span class="sxs-lookup"><span data-stu-id="74ede-485">Or with hello invocation of a wrapper factory method:</span></span>

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a><span data-ttu-id="74ede-486">Använda användardefinierade förminskningsapparater</span><span class="sxs-lookup"><span data-stu-id="74ede-486">Use user-defined reducers</span></span>

<span data-ttu-id="74ede-487">U-SQL kan du toowrite anpassade raduppsättningen förminskningsapparater i C# med hjälp av hello användardefinierade operatorn utökningsbarhet framework och implementera ett IReducer gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="74ede-487">U-SQL enables you toowrite custom rowset reducers in C# by using hello user-defined operator extensibility framework and implementing an IReducer interface.</span></span>

<span data-ttu-id="74ede-488">Användardefinierade reducer eller UDR, kan vara används tooeliminate onödiga raderna under Extraheringen av data (importera).</span><span class="sxs-lookup"><span data-stu-id="74ede-488">User-defined reducer, or UDR, can be used tooeliminate unnecessary rows during data extraction (import).</span></span> <span data-ttu-id="74ede-489">Den kan även använda toomanipulate och utvärdera rader och kolumner.</span><span class="sxs-lookup"><span data-stu-id="74ede-489">It also can be used toomanipulate and evaluate rows and columns.</span></span> <span data-ttu-id="74ede-490">Baserat på logik för programmering, kan det också definiera vilka rader måste toobe extraheras.</span><span class="sxs-lookup"><span data-stu-id="74ede-490">Based on programmability logic, it can also define which rows need toobe extracted.</span></span>

<span data-ttu-id="74ede-491">toodefine en UDR-klass vi behöver toocreate en `IReducer` gränssnitt med en valfri `SqlUserDefinedReducer` attribut.</span><span class="sxs-lookup"><span data-stu-id="74ede-491">toodefine a UDR class, we need toocreate an `IReducer` interface with an optional `SqlUserDefinedReducer` attribute.</span></span>

<span data-ttu-id="74ede-492">Den här klassen-gränssnittet ska innehålla en definition för hello `IEnumerable` gränssnittet raduppsättningen åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="74ede-492">This class interface should contain a definition for hello `IEnumerable` interface rowset override.</span></span>

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

<span data-ttu-id="74ede-493">Hej **SqlUserDefinedReducer** attributet anger att hello typ ska registreras som en användardefinierad reducer.</span><span class="sxs-lookup"><span data-stu-id="74ede-493">hello **SqlUserDefinedReducer** attribute indicates that hello type should be registered as a user-defined reducer.</span></span> <span data-ttu-id="74ede-494">Den här klassen kan inte ärvas.</span><span class="sxs-lookup"><span data-stu-id="74ede-494">This class cannot be inherited.</span></span>
<span data-ttu-id="74ede-495">**SqlUserDefinedReducer** är ett valfritt attribut för en användardefinierad reducer definition.</span><span class="sxs-lookup"><span data-stu-id="74ede-495">**SqlUserDefinedReducer** is an optional attribute for a user-defined reducer definition.</span></span> <span data-ttu-id="74ede-496">Den används toodefine IsRecursive egenskapen.</span><span class="sxs-lookup"><span data-stu-id="74ede-496">It's used toodefine IsRecursive property.</span></span>

* <span data-ttu-id="74ede-497">bool IsRecursive</span><span class="sxs-lookup"><span data-stu-id="74ede-497">bool     IsRecursive</span></span>    
* <span data-ttu-id="74ede-498">**SANT** = anger om den här Reducer är idempotent</span><span class="sxs-lookup"><span data-stu-id="74ede-498">**true**  = Indicates whether this Reducer is idempotent</span></span>

<span data-ttu-id="74ede-499">hello huvudsakliga programmering objekt är **inkommande** och **utdata**.</span><span class="sxs-lookup"><span data-stu-id="74ede-499">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="74ede-500">Hej indataobjektet är används tooenumerate inkommande rader.</span><span class="sxs-lookup"><span data-stu-id="74ede-500">hello input object is used tooenumerate input rows.</span></span> <span data-ttu-id="74ede-501">Utdata är används tooset utdata rader på grund av att minska aktivitet.</span><span class="sxs-lookup"><span data-stu-id="74ede-501">Output is used tooset output rows as a result of reducing activity.</span></span>

<span data-ttu-id="74ede-502">För inkommande rader uppräkningen vi använder hello `Row.Get` metod.</span><span class="sxs-lookup"><span data-stu-id="74ede-502">For input rows enumeration, we use hello `Row.Get` method.</span></span>

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

<span data-ttu-id="74ede-503">Hej parametern för hello `Row.Get` metoden är en kolumn som skickas som en del av hello `PRODUCE` objektklass hello `REDUCE` instruktionen på grundläggande hello U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="74ede-503">hello parameter for hello `Row.Get` method is a column that's passed as part of hello `PRODUCE` class of hello `REDUCE` statement of hello U-SQL base script.</span></span> <span data-ttu-id="74ede-504">Vi behöver toouse hello korrekta data skriver samt.</span><span class="sxs-lookup"><span data-stu-id="74ede-504">We need toouse hello correct data type here as well.</span></span>

<span data-ttu-id="74ede-505">Använd hello för utdata `output.Set` metod.</span><span class="sxs-lookup"><span data-stu-id="74ede-505">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="74ede-506">Det är viktigt toounderstand som anpassade reducer endast utdata värden som definieras med hello `output.Set` metodanrop.</span><span class="sxs-lookup"><span data-stu-id="74ede-506">It is important toounderstand that custom reducer only outputs values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", guid);
```

<span data-ttu-id="74ede-507">hello faktiska reducer utdata utlöses genom att anropa `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="74ede-507">hello actual reducer output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="74ede-508">Följande är ett exempel på reducer:</span><span class="sxs-lookup"><span data-stu-id="74ede-508">Following is a reducer example:</span></span>

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

<span data-ttu-id="74ede-509">I detta scenario användningsfall hello reducer hoppar över rader med ett tomt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="74ede-509">In this use-case scenario, hello reducer is skipping rows with an empty user name.</span></span> <span data-ttu-id="74ede-510">För varje rad i raduppsättningen läser varje obligatorisk kolumn, därefter utvärderar hello tidslängd hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="74ede-510">For each row in rowset, it reads each required column, then evaluates hello length of hello user name.</span></span> <span data-ttu-id="74ede-511">Den matar ut hello faktiska raden bara om värdet för användarnamnet är större än 0.</span><span class="sxs-lookup"><span data-stu-id="74ede-511">It outputs hello actual row only if user name value length is more than 0.</span></span>

<span data-ttu-id="74ede-512">Nedan följer grundläggande U-SQL-skript som använder en anpassad reducer:</span><span class="sxs-lookup"><span data-stu-id="74ede-512">Following is base U-SQL script that uses a custom reducer:</span></span>

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
