---
title: "aaaAnalyze webbplatsloggar med hjälp av Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur tooanalyze webbplatsloggar med hjälp av Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: d27aaca95ed2b643cfed7a17b0066bf7fa4a1bf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics
Lär dig hur tooanalyze webbplatsloggar med hjälp av Data Lake Analytics, särskilt för att ta reda på vilka referenter råkade ut för fel när de försöker toovisit hello webbplats.

## <a name="prerequisites"></a>Krav
* **Visual Studio 2015 eller Visual Studio 2013**.
* **[Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs)**.

    När Data Lake-verktyg för Visual Studio är installerat visas ett **Datasjö** objekt i hello **verktyg** -menyn i Visual Studio:

    ![U-SQL Visual Studio-menyn](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Grundläggande kunskaper om Data Lake Analytics och hello Data Lake-verktyg för Visual Studio**. tooget igång, se:

  * [Utveckla U-SQL-skript med Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* **Ett Data Lake Analytics-konto.**  Se [skapa ett Azure Data Lake Analytics-konto](data-lake-analytics-get-started-portal.md).
* **Överför hello exempel data toohello Data Lake Analytics-konto.** Se [toocopy exempeldatafiler](data-lake-analytics-get-started-portal.md).

    toorun ett Data Lake Analytics-jobb, måste vissa data. Även om hello Data Lake-verktygen stöder uppladdning av data, använder du hello portal tooupload hello exempel data toomake den här självstudiekursen enklare toofollow.

## <a name="connect-tooazure"></a>Ansluta tooAzure
Innan du kan skapa och testa alla U-SQL-skript, måste du först ansluta tooAzure.

**tooconnect tooData Lake Analytics**

1. Öppna Visual Studio.
2. Klicka på **Data Lake > Alternativ och inställningar för**.
3. Klicka på **logga In**, eller **ändra användare** om någon har loggat in och följ instruktionerna för hello.
4. Klicka på **OK** tooclose hello alternativ och inställningar för dialogrutan.

**toobrowse Data Lake Analytics-konton**

1. Visual Studio, öppna **Server Explorer** genom att klicka på **CTRL + ALT + S**.
2. Gå till **Server Explorer**, expandera **Azure** och expandera sedan **Data Lake Analytics**. En lista över dina Data Lake Analytics-konton visas om det finns några. Du kan inte skapa Data Lake Analytics-konton från hello studio. toocreate ett konto, se [Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md) eller [Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Utveckla U-SQL-program
Ett U-SQL-program är oftast ett U-SQL-skript. toolearn mer om U-SQL finns [Kom igång med U-SQL](data-lake-analytics-u-sql-get-started.md).

Du kan lägga till ytterligare användardefinierade operatorer toohello program.  Mer information finns i [utveckla U-SQL-operatörer för Data Lake Analytics-jobb för användardefinierade](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**toocreate och skicka ett Data Lake Analytics-jobb**

1. Klicka på hello **Arkiv > Nytt > projekt**.
2. Ange hello U-SQL-projekt.

    ![nytt U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klicka på **OK**. Visual studio skapar en lösning med en Script.usql-fil.
4. Ange följande skript i hello Script.usql fil hello:

        // Create a database for easy reuse, so you don't need tooread from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from hello weblog file with hello correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    toounderstand hello U-SQL finns [Kom igång med Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).    
5. Lägg till ett nytt U-SQL-skript tooyour projekt och ange hello följande:

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Växla tillbaka toohello första U-SQL-skript och nästa toohello **skicka** knappen, ange ditt Analytics-konto.
7. Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skapa skript**. Kontrollera hello resulterar i hello utdatafönstret.
8. Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skicka skript**.
9. Kontrollera hello **Analytics-konto** är hello en där du vill toorun hello jobbet och klicka sedan på **skicka**. Resultat för skicka och jobblänk är tillgängliga i hello Data Lake-verktyg för Visual Studio resultatfönstret när hello överföringen är klar.
10. Vänta tills hello jobbet har slutförts.  Om hello misslyckades, saknar troligen hello källfilen.  Se hello nödvändiga avsnitt i den här kursen. Ytterligare felsökningsinformation finns i [övervaka och felsöka Azure Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    När hello jobbet har slutförts, skall hello följande skärm visas:

    ![datasjöanalys analysera webbloggar webbplatsloggar](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Upprepa steg 7 – 10 för **Script1.usql**.

**toosee hello jobbutdata**

1. Från **Server Explorer**, expandera **Azure**, expandera **Datasjöanalys**, expandera Data Lake Analytics-kontot, expandera **Lagringskonton**, högerklicka på hello Data Lake-standardlagringskontot och klicka sedan på **Explorer**.
2. Dubbelklicka på **exempel** tooopen hello mapp och sedan dubbelklicka på **utdata**.
3. Dubbelklicka på **UnsuccessfulResponsees.log**.
4. Du kan också dubbelklicka hello utdatafilen inuti hello diagramvyn för hello jobbet i ordning toonavigate direkt toohello utdata.

## <a name="see-also"></a>Se även
tooget igång med Data Lake Analytics med hjälp av olika verktyg, se:

* [Kom igång med Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md)
* [Kom igång med Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Kom igång med Data Lake Analytics med hjälp av NET SDK](data-lake-analytics-get-started-net-sdk.md)
