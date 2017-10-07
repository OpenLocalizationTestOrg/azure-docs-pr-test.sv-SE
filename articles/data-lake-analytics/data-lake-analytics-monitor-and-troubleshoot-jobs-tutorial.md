---
title: "aaaTroubleshoot Azure Data Lake Analytics-jobb med hjälp av Azure Portal | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Portal tootroubleshoot Data Lake Analytics-jobb. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e810d56bab8f1a8254721ec9906bb6a4508dc22a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Felsöka Azure Data Lake Analytics-jobb med hjälp av Azure Portal
Lär dig hur toouse hello Azure Portal tootroubleshoot Data Lake Analytics-jobb.

I den här kursen ska du konfigurera saknas källa filen problem och använder hello Azure Portal tootroubleshoot hello problem.

## <a name="submit-a-data-lake-analytics-job"></a>Skicka ett Data Lake Analytics-jobb

Skicka hello följande U-SQL-jobb:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   too"/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
hello käll-filen som definierats i hello skript är **/Samples/Data/SearchLog.tsv1**, där det ska vara **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-hello-job"></a>Felsöka hello jobb

**toosee alla hello jobb**

1. Hello Azure-portalen, klicka på **Microsoft Azure** i hello övre vänstra hörnet.
2. Klicka på panelen hello med ditt Data Lake Analytics-kontonamn.  hello jobb sammanfattning visas på hello **jobbhantering** panelen.

    ![Hantering av Azure Data Lake Analytics-jobbet](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    hello jobbet Management ger dig en överblick över av hello jobbstatus. Observera att det är ett jobb som misslyckades.
3. Klicka på hello **jobbhantering** panelen toosee hello jobb. hello jobb kategoriseras i **kör**, **i kö**, och **avslutat**. Du bör se din misslyckade jobb i hello **avslutat** avsnitt. Den är förstnämnda hello-listan. När du har många jobb kan du klicka på **Filter** toohelp du toolocate jobb.

    ![Filtrera Azure Data Lake Analytics-jobb](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klicka på hello misslyckade jobbet från jobbinformation för hello listan tooopen hello i ett nytt blad:

    ![Azure Data Lake Analytics misslyckades jobb](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Meddelande hello **skicka** knappen. När du har åtgärdat hello problem kan du skicka hello jobb.
5. Klicka på markerade del från hello föregående skärmbild tooopen hello felinformation.  Du bör se något som liknar:

    ![Azure Data Lake Analytics misslyckades jobbinformation](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Den visar hello källmapp inte hittas.
6. Klicka på **duplicera skriptet**.
7. Uppdatera hello **FROM** sökväg toohello följande:

    ”/ Samples/Data/SearchLog.tsv”
8. Klicka på **Skicka jobb**.

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Kom igång med Azure Data Lake Analytics och U-SQL med Visual Studio](data-lake-analytics-u-sql-get-started.md)
* [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md)
