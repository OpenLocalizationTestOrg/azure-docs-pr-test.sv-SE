---
title: "aaaDevelop U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig hur tooinstall Data Lake-verktyg för Visual Studio och hur toodevelop och testa U-SQL-skript."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Utveckla U-SQL-skript med hjälp av Data Lake Tools för Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Lär dig hur toouse Visual Studio toocreate Azure Data Lake Analytics-konton, definiera jobb i [U-SQL](data-lake-analytics-u-sql-get-started.md), och skicka jobb toohello Data Lake Analytics-tjänsten. Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).


## <a name="prerequisites"></a>Krav

* **Visual Studio**: Alla utgåvor utom Express stöds.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK för .NET** version 2.7.1 eller senare.  Installera den med hjälp av hello [installationsprogram för webbplattform](http://www.microsoft.com/web/downloads/platform.aspx).
* Ett **Data Lake Analytics**-konto. toocreate ett konto, se [Kom igång med Azure Data Lake Analytics med hjälp av Azure portal](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Installera Azure Data Lake Tools för Visual Studio 

Hämta och installera Azure Data Lake-verktyg för Visual Studio [från hello Download Center](http://aka.ms/adltoolsvs). Efter installationen kontrollerar du att:
* Hej **Server Explorer** > **Azure** noden innehåller en **Datasjöanalys** nod. 
* Hej **verktyg** menyn har en **Data Lake** objekt.

## <a name="connect-tooan-azure-data-lake-analytics-account"></a>Ansluta tooan Azure Data Lake Analytics-konto

1. Öppna Visual Studio.
2. Öppna Server Explorer genom att välja **Visa** > **Server Explorer**.
3. Högerklicka på **Azure**. Välj sedan **ansluta tooMicrosoft Azure-prenumeration** och följer instruktionerna för hello.
4. Välj **Azure** > **Data Lake Analytics** i Server Explorer. En lista över dina Data Lake Analytics-konton visas.


## <a name="write-your-first-u-sql-script"></a>Skriv ditt första U-SQL-skript

hello efter texten är ett enkelt U-SQL-skript. Den definierar en liten datamängd och skrivningar som dataset toohello standard Data Lake Store som en fil som kallas `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Skicka ett Data Lake Analytics-jobb

1. Välj **Arkiv** > **Nytt** > **Projekt**.

2. Välj hello **U-SQL-projekt** Skriv och klicka på **OK**. Visual Studio skapar en lösning med filen **Script.usql**.

3. Klistra in hello föregående skript i hello **Script.usql** fönster.

4. I hello övre vänstra hörnet i hello **Script.usql** fönstret Ange hello Data Lake Analytics-konto.

    ![Skicka U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. I hello övre vänstra hörnet i hello **Script.usql** väljer **skicka**.
6. Kontrollera hello **Analytics-konto**, och välj sedan **skicka**. Resultat för skicka är tillgängliga i hello Data Lake-verktyg för Visual Studio-resultat när hello ansökan har slutförts.

    ![Skicka U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. toosee hello senaste jobb status och uppdatera hello-skärmen, klickar du på **uppdatera**. När hello jobb lyckas, den visar hello **Jobbdiagram**, **metadataåtgärder**, **Tillståndshistorik**, och **diagnostik**:

    ![Prestandadiagram för U-SQL Visual Studio Data Lake Analytics-jobbet](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Jobbet sammanfattning** visar hello sammanfattning av hello jobb.   
   * **Jobbinformation** visar mer detaljerad information om hello jobbet, inklusive hello skript, resurser och formhörnen.
   * **Jobbdiagram** visualizes hello förloppet för jobbet hello.
   * **Metadataåtgärder** visar alla hello-åtgärder som vidtagits hello U-SQL-katalogen.
   * **Data** visar alla hello indata och utdata.
   * **Diagnostik** ger en avancerad analys för jobbkörning och prestandaoptimering.

### <a name="toocheck-job-state"></a>toocheck jobbets status

1. Välj **Azure** > **Data Lake Analytics** i Server Explorer. 
2. Expandera hello Data Lake Analytics-kontonamnet.
3. Dubbelklicka på **Jobb**.
4. Välj hello jobb som du redan har skickat.

### <a name="toosee-hello-output-of-a-job"></a>toosee hello utdata för ett jobb

1. Bläddra i Server Explorer toohello jobbet du skickat.
2. Klicka på hello **Data** fliken.
3. I hello **utdata för jobbet** fliken, väljer hello `"/data.csv"` fil.

## <a name="next-steps"></a>Nästa steg

* [Kör U-SQL-skript på din egen arbetsstation för testning och felsökning](data-lake-analytics-data-lake-tools-local-run.md)
* [Felsöka C#-kod i U-SQL-jobb](data-lake-analytics-debug-u-sql-jobs.md)
* [Använd hello Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
