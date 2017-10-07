---
title: aaaExtend U-SQL-skript med Python i Azure Data Lake Analytics | Microsoft Docs
description: "Lär dig hur toorun Python code i U-SQL-skript"
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
ms.openlocfilehash: f051f56f67522d4f2b8e6e54fd21a5c95ce3ba92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="73e0c-103">Självstudier: Kom igång med att utöka U-SQL med Python</span><span class="sxs-lookup"><span data-stu-id="73e0c-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="73e0c-104">Python-tilläggen för U-SQL kan utvecklare tooperform massivt parallell körning av Python-kod.</span><span class="sxs-lookup"><span data-stu-id="73e0c-104">Python Extensions for U-SQL enable developers tooperform massively parallel execution of Python code.</span></span> <span data-ttu-id="73e0c-105">hello som följande exempel visar hello grundläggande steg:</span><span class="sxs-lookup"><span data-stu-id="73e0c-105">hello following example illustrates hello basic steps:</span></span>

* <span data-ttu-id="73e0c-106">Använd hello `REFERENCE ASSEMBLY` instruktionen tooenable Python-tillägg för hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="73e0c-106">Use hello `REFERENCE ASSEMBLY` statement tooenable Python extensions for hello U-SQL Script</span></span>
* <span data-ttu-id="73e0c-107">Med hjälp av hello `REDUCE` åtgärden toopartition hello mata in data på en nyckel</span><span class="sxs-lookup"><span data-stu-id="73e0c-107">Using hello `REDUCE` operation toopartition hello input data on a key</span></span>
* <span data-ttu-id="73e0c-108">hello Python-tillägg för U-SQL är en inbyggd reducer (`Extension.Python.Reducer`) som kör Python-kod på varje nod som tilldelats toohello reducer</span><span class="sxs-lookup"><span data-stu-id="73e0c-108">hello Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned toohello reducer</span></span>
* <span data-ttu-id="73e0c-109">hello U-SQL-skript som innehåller inbäddade hello Python-kod som har en funktion som kallas `usqlml_main` som accepterar pandas DataFrame som indata och returnerar pandas DataFrame som utdata.</span><span class="sxs-lookup"><span data-stu-id="73e0c-109">hello U-SQL script contains hello embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

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
        too"/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="73e0c-110">Hur Python kan integreras med U-SQL</span><span class="sxs-lookup"><span data-stu-id="73e0c-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="73e0c-111">Datatyper</span><span class="sxs-lookup"><span data-stu-id="73e0c-111">Datatypes</span></span>

* <span data-ttu-id="73e0c-112">Konverteras som sträng och numeriska kolumner från U-SQL-mellan Pandas och U-SQL</span><span class="sxs-lookup"><span data-stu-id="73e0c-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="73e0c-113">U-SQL null-värden är konverterade tooand från Pandas `NA` värden</span><span class="sxs-lookup"><span data-stu-id="73e0c-113">U-SQL Nulls are converted tooand from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="73e0c-114">Scheman</span><span class="sxs-lookup"><span data-stu-id="73e0c-114">Schemas</span></span>

* <span data-ttu-id="73e0c-115">Index angreppsmetoderna i Pandas stöds inte i U-SQL.</span><span class="sxs-lookup"><span data-stu-id="73e0c-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="73e0c-116">Alla indata ramar i hello Python-funktionen har alltid ett numeriskt index för 64-bitars från 0 till hello antalet rader minus 1.</span><span class="sxs-lookup"><span data-stu-id="73e0c-116">All input data frames in hello Python function always have a 64-bit numerical index from 0 through hello number of rows minus 1.</span></span> 
* <span data-ttu-id="73e0c-117">U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="73e0c-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="73e0c-118">U-SQL datauppsättningar kolumnnamn som inte är strängar.</span><span class="sxs-lookup"><span data-stu-id="73e0c-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="73e0c-119">Python-versioner</span><span class="sxs-lookup"><span data-stu-id="73e0c-119">Python Versions</span></span>
<span data-ttu-id="73e0c-120">Endast Python 3.5.1 (sammanställd för Windows) stöds.</span><span class="sxs-lookup"><span data-stu-id="73e0c-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="73e0c-121">Standard Python-moduler</span><span class="sxs-lookup"><span data-stu-id="73e0c-121">Standard Python modules</span></span>
<span data-ttu-id="73e0c-122">Alla hello standard Python-moduler ingår.</span><span class="sxs-lookup"><span data-stu-id="73e0c-122">All hello standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="73e0c-123">Ytterligare Python-moduler</span><span class="sxs-lookup"><span data-stu-id="73e0c-123">Additional Python modules</span></span>
<span data-ttu-id="73e0c-124">Utöver hello Python-standardbibliotek ingår flera vanliga python-bibliotek:</span><span class="sxs-lookup"><span data-stu-id="73e0c-124">Besides hello standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="73e0c-125">Undantag meddelanden</span><span class="sxs-lookup"><span data-stu-id="73e0c-125">Exception Messages</span></span>
<span data-ttu-id="73e0c-126">För närvarande visas ett undantag i Python-kod som ett allmänt vertex fel.</span><span class="sxs-lookup"><span data-stu-id="73e0c-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="73e0c-127">I framtida hello visar hello U-SQL-jobbet felmeddelanden hello Python Undantagsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="73e0c-127">In hello future, hello U-SQL Job error messages will display hello Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="73e0c-128">Indata och utdata storleksbegränsningar</span><span class="sxs-lookup"><span data-stu-id="73e0c-128">Input and Output size limitations</span></span>
<span data-ttu-id="73e0c-129">Varje nod har en begränsad mängd minne som tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="73e0c-129">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="73e0c-130">Denna gräns är för närvarande 6 GB för en AU.</span><span class="sxs-lookup"><span data-stu-id="73e0c-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="73e0c-131">Eftersom hello inkommande och utgående DataFrames måste finnas i minnet i hello Python-kod, får inte hello total storlek för hello indata och utdata överskrida 6 GB.</span><span class="sxs-lookup"><span data-stu-id="73e0c-131">Because hello input and output DataFrames must exist in memory in hello Python code, hello total size for hello input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="73e0c-132">Se även</span><span class="sxs-lookup"><span data-stu-id="73e0c-132">See also</span></span>
* [<span data-ttu-id="73e0c-133">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="73e0c-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="73e0c-134">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73e0c-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="73e0c-135">Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="73e0c-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

