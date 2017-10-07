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
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a>Självstudier: Kom igång med att utöka U-SQL med Python

Python-tilläggen för U-SQL kan utvecklare tooperform massivt parallell körning av Python-kod. hello som följande exempel visar hello grundläggande steg:

* Använd hello `REFERENCE ASSEMBLY` instruktionen tooenable Python-tillägg för hello U-SQL-skript
* Med hjälp av hello `REDUCE` åtgärden toopartition hello mata in data på en nyckel
* hello Python-tillägg för U-SQL är en inbyggd reducer (`Extension.Python.Reducer`) som kör Python-kod på varje nod som tilldelats toohello reducer
* hello U-SQL-skript som innehåller inbäddade hello Python-kod som har en funktion som kallas `usqlml_main` som accepterar pandas DataFrame som indata och returnerar pandas DataFrame som utdata.

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

## <a name="how-python-integrates-with-u-sql"></a>Hur Python kan integreras med U-SQL

### <a name="datatypes"></a>Datatyper

* Konverteras som sträng och numeriska kolumner från U-SQL-mellan Pandas och U-SQL
* U-SQL null-värden är konverterade tooand från Pandas `NA` värden

### <a name="schemas"></a>Scheman

* Index angreppsmetoderna i Pandas stöds inte i U-SQL. Alla indata ramar i hello Python-funktionen har alltid ett numeriskt index för 64-bitars från 0 till hello antalet rader minus 1. 
* U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn
* U-SQL datauppsättningar kolumnnamn som inte är strängar. 

### <a name="python-versions"></a>Python-versioner
Endast Python 3.5.1 (sammanställd för Windows) stöds. 

### <a name="standard-python-modules"></a>Standard Python-moduler
Alla hello standard Python-moduler ingår.

### <a name="additional-python-modules"></a>Ytterligare Python-moduler
Utöver hello Python-standardbibliotek ingår flera vanliga python-bibliotek:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Undantag meddelanden
För närvarande visas ett undantag i Python-kod som ett allmänt vertex fel. I framtida hello visar hello U-SQL-jobbet felmeddelanden hello Python Undantagsmeddelande.

### <a name="input-and-output-size-limitations"></a>Indata och utdata storleksbegränsningar
Varje nod har en begränsad mängd minne som tilldelats tooit. Denna gräns är för närvarande 6 GB för en AU. Eftersom hello inkommande och utgående DataFrames måste finnas i minnet i hello Python-kod, får inte hello total storlek för hello indata och utdata överskrida 6 GB.

## <a name="see-also"></a>Se även
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb](data-lake-analytics-use-window-functions.md)

