---
title: aaaDebug U-SQL-jobb | Microsoft Docs
description: "Lär dig hur toodebug U-SQL misslyckades vertex med Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Felsöka användardefinierade C#-kod för misslyckade U-SQL-jobb

U-SQL ger en modellen för utökning med C#, så att du kan skriva din kod tooadd funktioner, till exempel en anpassad extraktor eller reducer. Det finns fler toolearn [U-SQL-programmering guiden](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). I praktiken någon kod måste felsökning och ge endast system för stordata begränsad runtime felsökningsinformation som loggfiler.

Azure Data Lake-verktyg för Visual Studio innehåller en funktion som kallas **misslyckades Vertex felsöka**, vilket gör att du klona en misslyckade jobbet från hello molnet tooyour lokal dator för felsökning. hello lokala klona avbildar hello hela molnmiljö, inklusive alla indata och användarkod.

hello följande videoklipp visar misslyckades Vertex Debug i Azure Data Lake-verktyg för Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Visual Studio kräver hello följande två uppdateringar om de inte redan är installerade: [Microsoft Visual C++ 2015 Redistributable uppdatering 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) och [Universal C Runtime för Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-toolocal-machine"></a>Gick inte att hämta vertex toolocal datorn

När du öppnar ett jobb som misslyckades i Azure Data Lake-verktyg för Visual Studio kan du se ett gult avisering fält med detaljerade felmeddelanden hello fel på fliken.

1. Klicka på **hämta** toodownload alla hello nödvändiga resurser och inkommande dataströmmar. Om hello hämtningen inte slutförs, klicka på **försök**.

2. Klicka på **öppna** när hello nedladdningen är klar toogenerate lokala felsökningsmiljö. En ny Visual Studio-instans med en lösning för felsökning skapas och öppnas automatiskt.

![Azure Data Lake Analytics U-SQL felsökning i visual studio download vertex](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Jobb kan inkludera bakomliggande kod källfiler eller registrerade sammansättningar, och dessa två typer har olika scenarier för felsökning.

- [Felsöka ett jobb som misslyckades med bakomliggande kod](#debug-job-failed-with-code-behind)
- [Felsöka ett jobb som misslyckades med sammansättningar](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Felsöka jobb som misslyckades med bakomliggande kod

Om ett U-SQL-jobb misslyckas och hello arbetsuppgifter inbegriper användarkod (vanligtvis namnet `Script.usql.cs` i ett U-SQL-projekt), att källkoden har importerats till hello felsökning lösning.  Därifrån kan du använda hello Visual Studio felsökning verktyg (titta på, variabler, etc.) tootroubleshoot hello problem.

> [!NOTE]
> Innan du felsökning, vara säker på att toocheck **Common Language Runtime undantag** i hello undantag inställningar fönster (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL felsökning i visual studio-inställning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Tryck på **F5** toorun hello bakomliggande kod kod. Det kan köras förrän den har stoppats av ett undantagsfel.

2. Öppna hello `ADLTool_Codebehind.usql.cs` filen och ange brytpunkter, tryck på **F5** toodebug hello kod steg för steg.

    ![Azure Data Lake Analytics U-SQL debug-undantag](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Felsöka jobb som misslyckades med sammansättningar

Om du använder registrerade sammansättningar i U-SQL-skript kan hello system kan inte hämta hello källkoden automatiskt. I det här fallet manuellt lägga till hello sammansättningar källa kod filer toohello lösning.

### <a name="configure-hello-solution"></a>Konfigurera hello lösning

1. Högerklicka på **lösning 'VertexDebug' > Lägg till > befintligt projekt...**  toofind hello sammansättningar källkoden och Lägg till hello projektet toohello felsökning lösning.

    ![Azure Data Lake Analytics U-SQL-debug lägga till projekt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Högerklicka på **LocalVertexHost > Egenskaper** i hello lösning och kopiera hello **Working Directory** sökväg.

3. Högerklicka på **sammansättningsprojekt källa kod > Egenskaper**väljer hello **skapa** fliken längst till vänster och klistra in hello kopieras sökväg som **utdata > Utdatasökvägen**.

    ![Ange sökväg till pdb för Azure Data Lake Analytics U-SQL-felsökning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Tryck på **Ctrl + Alt + E**, kontrollera **Common Language Runtime undantag** i inställningar för undantag.

### <a name="start-debug"></a>Starta felsökning

1. Högerklicka på **Kodprojekt för sammansättningen källa > återskapa** toooutput .pdb filer toohello `LocalVertexHost` arbetskatalogen.

2. Tryck på **F5** och hello projektet kommer att köras tills den har stoppats av ett undantagsfel. Du kan se hello följande varningsmeddelande som du kan ignorera. Det kan ta upp tooa minut tooget toohello debug skärmen.

    ![Azure Data Lake Analytics U-SQL felsökning i visual studio-varning](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Öppna källkoden och ange brytpunkter, tryck på **F5** toodebug hello kod steg för steg.

Du kan också använda hello Visual Studio felsökning verktyg (titta på, variabler, etc.) tootroubleshoot hello problem.

> [!NOTE]
> Återskapa hello sammansättningsprojekt källa kod varje gång när du har ändrat hello kodfiler toogenerate uppdateras .pdb.

Efter felsökning visar hello utdatafönstret hello efter meddelande om hello projekt slutförs:

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL-debug lyckas](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a>Skicka hello jobb

När du har slutfört felsökning, skicka hello misslyckade jobbet.

1. Kopiera C#-kod för jobb med bakomliggande kod lösningar till hello bakomliggande kod källfilen (vanligtvis `Script.usql.cs`).
2. Registrera hello uppdateras .dll sammansättningar i databasen ADLA för jobb med sammansättningar:
    1. Från Server Explorer eller Cloud Explorer expanderar du hello **ADLA konto > databaser** nod.
    2. Högerklicka på **sammansättningar** och registrera ditt nya .dll-sammansättningar med hello ADLA databasen: ![Azure Data Lake Analytics U-SQL-debug registrera sammansättningen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Skicka jobbet.

## <a name="next-steps"></a>Nästa steg

- [U-SQL-programmering guide](data-lake-analytics-u-sql-programmability-guide.md)
- [Utveckla U-SQL-användardefinierade operatorer för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
