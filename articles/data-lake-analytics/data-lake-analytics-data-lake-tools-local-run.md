---
title: "aaaTest och felsökningsloggar U-SQL-jobb med hjälp av lokala kör och hello Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio och hello Azure Data Lake U-SQL-SDK tootest och debug U-SQL-jobb på din lokala arbetsstationen."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a>Testa och felsöka U-SQL-jobb med hjälp av lokal kör och hello Azure Data Lake U-SQL-SDK

Du kan använda Azure Data Lake-verktyg för Visual Studio och hello Azure Data Lake U-SQL-SDK toorun U-SQL-jobb på din arbetsstation, precis som i hello Azure Data Lake-tjänsten. Med de här två funktionerna som körs lokalt kan du spara tid när du testar och felsöker dina U-SQL-jobb.

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a>Förstå hello data-rotmappen och hello filsökväg

Både lokal körning och hello U-SQL-SDK kräver en data-rotmappen. hello data-rotmappen är ett ”lokalt Arkiv” för hello lokala beräkningskonto. Det är likvärdiga toohello Azure Data Lake Store-konto för Data Lake Analytics-konto. Växla tooa olika data-rotmappen som är precis som växlar tooa olika store-konto. Om du vill tooaccess ofta delas data med olika dataroten mappar, måste du använda absoluta sökvägar i skript. Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under hello-dataroten mappen toopoint toohello delade data.

hello data-rotmappen används för att:

- Lagra metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.
- Leta upp hello indata och utdata sökvägar som har definierats som relativa sökvägar i U-SQL. Använda relativa sökvägar gör det enklare toodeploy tooAzure dina U-SQL-projekt.

Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript. hello relativ sökväg är relativ toohello angivna data-sökvägen till rotmappen. Vi rekommenderar att du använder ”/” som hello sökväg avgränsare toomake skripten som är kompatibel med hello på serversidan. Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar. I det här exemplet är C:\LocalRunDataRoot hello data-rotmappen.

|Relativ sökväg|Absolut sökväg|
|-------------|-------------|
|/ABC/def/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/def/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/def/Input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Använd lokal kör från Visual Studio

Data Lake-verktyg för Visual Studio innehåller ett U-SQL lokalt körningen i Visual Studio. Med hjälp av den här funktionen kan du:

- Kör en U-SQL-skript lokalt, tillsammans med C#-sammansättningar.
- Felsöka en C#-sammansättning lokalt.
- Skapa, visa och ta bort kataloger för U-SQL (lokala databaser, sammansättningar, scheman och tabeller) från Server Explorer. Du kan också hitta hello lokala katalogen också från Server Explorer.

    ![Data Lake-verktyg för Visual Studio lokala kör lokal katalog](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

hello Data Lake-verktyg installationsprogrammet skapar en C:\LocalRunRoot mappen toobe som används som hello standard data-rotmappen. hello standard lokala kör parallellitet är 1.

### <a name="tooconfigure-local-run-in-visual-studio"></a>tooconfigure lokala kör i Visual Studio

1. Öppna Visual Studio.
2. Öppna **Server Explorer**.
3. Expandera **Azure** > **Datasjöanalys**.
4. Klicka på hello **Datasjö** -menyn och klicka sedan på **alternativ och inställningar för**.
5. Expandera i hello vänster träd **Azure Data Lake**, och expandera sedan **allmänna**.

    ![Konfigurera inställningar för data Lake-verktyg för Visual Studio kör lokalt](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Ett Visual Studio U-SQL-projekt krävs för att utföra lokala kör. Den här delen skiljer sig från att köra U-SQL-skript från Azure.

### <a name="toorun-a-u-sql-script-locally"></a>toorun U-SQL-skript lokalt
1. Öppna projektet U-SQL Visual Studio.   
2. Högerklicka på ett U-SQL-skript i Solution Explorer och klicka sedan på **skicka skript**.
3. Välj **(lokal)** som hello Analytics-kontot toorun skriptet lokalt.
Du kan också klicka på hello **(lokal)** konto på hello upp i skriptfönstret och klicka sedan på **skicka** (eller använda hello Ctrl + F5 kortkommandot).

    ![Data Lake-verktyg för Visual Studio lokala kör skicka jobb](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Felsök skript och C#-sammansättningar lokalt

Du kan felsöka C#-sammansättningar utan att skicka och registrera den tooAzure Data Lake Analytics-tjänsten. Du kan ange brytpunkter i både hello koden bakom filen och ett refererat C#-projekt.

#### <a name="toodebug-local-code-in-code-behind-file"></a>toodebug lokal kod i koden bakom filen

1. Ange brytpunkter i hello koden bakom filen.
2. Tryck på F5 toodebug hello skript lokalt.

> [!NOTE]
   > hello följande procedur fungerar bara i Visual Studio 2015. I äldre Visual Studio måste toomanually lägga till hello pdb-filer.  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a>toodebug lokal kod i ett refererat C#-projekt

1. Skapa ett C#-sammansättningsprojekt och skapa det toogenerate hello utdata DLL-filen.
2. Registrera hello DLL-filen med ett U-SQL-uttryck:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Ange brytpunkter i hello C#-kod.
4. Tryck på F5 toodebug hello skriptet med referens hello C# DLL-filen lokalt.

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a>Använd lokal kör från hello Data Lake U-SQL-SDK

Dessutom toorunning U-SQL-skript lokalt med hjälp av Visual Studio kan du använda hello Azure Data Lake U-SQL-SDK toorun U-SQL-skript lokalt med kommandoraden och API-gränssnitt. Du kan skala din lokal U-SQL-test via dessa.

Lär dig mer om [Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Nästa steg

* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).
* toouse hello vertex vy, se [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
