---
title: "Utöka U-SQL-skript med Python i Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur du kör Python-kod i U-SQL-skript"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d18ef1f747aee2fa01cef9891432d0461031ee4c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="0a7e5-103">Självstudier: Kom igång med att utöka U-SQL med Python</span><span class="sxs-lookup"><span data-stu-id="0a7e5-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="0a7e5-104">Python-tillägg för U-SQL ger utvecklare möjligheten att utföra massivt parallell körning av Python-kod.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-104">Python Extensions for U-SQL enable developers to perform massively parallel execution of Python code.</span></span> <span data-ttu-id="0a7e5-105">I följande exempel visas de grundläggande steg:</span><span class="sxs-lookup"><span data-stu-id="0a7e5-105">The following example illustrates the basic steps:</span></span>

* <span data-ttu-id="0a7e5-106">Använd den `REFERENCE ASSEMBLY` -instruktionen för att aktivera Python-tillägg för U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="0a7e5-106">Use the `REFERENCE ASSEMBLY` statement to enable Python extensions for the U-SQL Script</span></span>
* <span data-ttu-id="0a7e5-107">Med den `REDUCE` åtgärden att partitionera indata för en nyckel</span><span class="sxs-lookup"><span data-stu-id="0a7e5-107">Using the `REDUCE` operation to partition the input data on a key</span></span>
* <span data-ttu-id="0a7e5-108">Python-tillägg för U-SQL är en inbyggd reducer (`Extension.Python.Reducer`) som kör Python-kod på varje nod som tilldelats reducer</span><span class="sxs-lookup"><span data-stu-id="0a7e5-108">The Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned to the reducer</span></span>
* <span data-ttu-id="0a7e5-109">U-SQL-skriptet innehåller en inbäddad Python-kod som har en funktion som kallas `usqlml_main` som accepterar pandas DataFrame som indata och returnerar pandas DataFrame som utdata.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-109">The U-SQL script contains the embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="0a7e5-110">Hur Python kan integreras med U-SQL</span><span class="sxs-lookup"><span data-stu-id="0a7e5-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="0a7e5-111">Datatyper</span><span class="sxs-lookup"><span data-stu-id="0a7e5-111">Datatypes</span></span>

* <span data-ttu-id="0a7e5-112">Konverteras som sträng och numeriska kolumner från U-SQL-mellan Pandas och U-SQL</span><span class="sxs-lookup"><span data-stu-id="0a7e5-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="0a7e5-113">U-SQL null-värden konverteras till och från Pandas `NA` värden</span><span class="sxs-lookup"><span data-stu-id="0a7e5-113">U-SQL Nulls are converted to and from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="0a7e5-114">Scheman</span><span class="sxs-lookup"><span data-stu-id="0a7e5-114">Schemas</span></span>

* <span data-ttu-id="0a7e5-115">Index angreppsmetoderna i Pandas stöds inte i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="0a7e5-116">Alla indata ramar i Python-funktionen har alltid ett numeriskt index för 64-bitars mellan 0 och antalet rader minus 1.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-116">All input data frames in the Python function always have a 64-bit numerical index from 0 through the number of rows minus 1.</span></span> 
* <span data-ttu-id="0a7e5-117">U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="0a7e5-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="0a7e5-118">U-SQL datauppsättningar kolumnnamn som inte är strängar.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="0a7e5-119">Python-versioner</span><span class="sxs-lookup"><span data-stu-id="0a7e5-119">Python Versions</span></span>
<span data-ttu-id="0a7e5-120">Endast Python 3.5.1 (sammanställd för Windows) stöds.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="0a7e5-121">Standard Python-moduler</span><span class="sxs-lookup"><span data-stu-id="0a7e5-121">Standard Python modules</span></span>
<span data-ttu-id="0a7e5-122">Alla Python-moduler som standard ingår.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-122">All the standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="0a7e5-123">Ytterligare Python-moduler</span><span class="sxs-lookup"><span data-stu-id="0a7e5-123">Additional Python modules</span></span>
<span data-ttu-id="0a7e5-124">Förutom standardbibliotek Python ingår flera vanliga python-bibliotek:</span><span class="sxs-lookup"><span data-stu-id="0a7e5-124">Besides the standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="0a7e5-125">Undantag meddelanden</span><span class="sxs-lookup"><span data-stu-id="0a7e5-125">Exception Messages</span></span>
<span data-ttu-id="0a7e5-126">För närvarande visas ett undantag i Python-kod som ett allmänt vertex fel.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="0a7e5-127">U-SQL-jobbet felmeddelanden visas i framtiden, Undantagsmeddelande Python.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-127">In the future, the U-SQL Job error messages will display the Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="0a7e5-128">Indata och utdata storleksbegränsningar</span><span class="sxs-lookup"><span data-stu-id="0a7e5-128">Input and Output size limitations</span></span>
<span data-ttu-id="0a7e5-129">Varje nod har en begränsad mängd minne som tilldelats den.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-129">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="0a7e5-130">Denna gräns är för närvarande 6 GB för en AU.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="0a7e5-131">Eftersom inkommande och utgående DataFrames måste finnas i minnet i Python-kod, får inte den totala storleken för ingående och utgående överskrida 6 GB.</span><span class="sxs-lookup"><span data-stu-id="0a7e5-131">Because the input and output DataFrames must exist in memory in the Python code, the total size for the input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="0a7e5-132">Se även</span><span class="sxs-lookup"><span data-stu-id="0a7e5-132">See also</span></span>
* [<span data-ttu-id="0a7e5-133">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="0a7e5-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="0a7e5-134">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a7e5-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="0a7e5-135">Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="0a7e5-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

