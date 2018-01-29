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
ms.openlocfilehash: a8acaa16265070308753c2a0df3a9e7b8a3a841a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a>Självstudier: Kom igång med att utöka U-SQL med Python

## <a name="prerequisites"></a>Krav

Innan du börjar, kontrollera Python-tillägg installeras i ditt Azure Data Lake Analytics-konto.

* Navigera till dig Data Lake Analytics-konto i Azure-portalen
* I den vänstra menyn under **komma igång** klickar du på **exempel på skript**
* Klicka på **installera U-SQL-tillägg** sedan **OK**

## <a name="overview"></a>Översikt 

Python-tillägg för U-SQL ger utvecklare möjligheten att utföra massivt parallell körning av Python-kod. I följande exempel visas de grundläggande steg:

* Använd den `REFERENCE ASSEMBLY` -instruktionen för att aktivera Python-tillägg för U-SQL-skript
* Med den `REDUCE` åtgärden att partitionera indata för en nyckel
* Python-tillägg för U-SQL är en inbyggd reducer (`Extension.Python.Reducer`) som kör Python-kod på varje nod som tilldelats reducer
* U-SQL-skriptet innehåller en inbäddad Python-kod som har en funktion som kallas `usqlml_main` som accepterar pandas DataFrame som indata och returnerar pandas DataFrame som utdata.

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

## <a name="how-python-integrates-with-u-sql"></a>Hur Python kan integreras med U-SQL

### <a name="datatypes"></a>Datatyper

* Konverteras som sträng och numeriska kolumner från U-SQL-mellan Pandas och U-SQL
* U-SQL null-värden konverteras till och från Pandas `NA` värden

### <a name="schemas"></a>Scheman

* Index angreppsmetoderna i Pandas stöds inte i U-SQL. Alla indata ramar i Python-funktionen har alltid ett numeriskt index för 64-bitars mellan 0 och antalet rader minus 1. 
* U-SQL-datauppsättningar kan inte ha dubbletter av kolumnnamn
* U-SQL datauppsättningar kolumnnamn som inte är strängar. 

### <a name="python-versions"></a>Python-versioner
Endast Python 3.5.1 (sammanställd för Windows) stöds. 

### <a name="standard-python-modules"></a>Standard Python-moduler
Alla Python-moduler som standard ingår.

### <a name="additional-python-modules"></a>Ytterligare Python-moduler
Förutom standardbibliotek Python ingår flera vanliga python-bibliotek:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Undantag meddelanden
För närvarande visas ett undantag i Python-kod som ett allmänt vertex fel. U-SQL-jobbet felmeddelanden visas i framtiden, Undantagsmeddelande Python.

### <a name="input-and-output-size-limitations"></a>Indata och utdata storleksbegränsningar
Varje nod har en begränsad mängd minne som tilldelats den. Denna gräns är för närvarande 6 GB för en AU. Eftersom inkommande och utgående DataFrames måste finnas i minnet i Python-kod, får inte den totala storleken för ingående och utgående överskrida 6 GB.

## <a name="see-also"></a>Se även
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb](data-lake-analytics-use-window-functions.md)
* [Använda Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
