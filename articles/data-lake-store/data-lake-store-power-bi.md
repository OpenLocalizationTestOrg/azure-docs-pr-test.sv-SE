---
title: "aaaAnalyze data i Data Lake Store med hjälp av Power BI | Microsoft Docs"
description: "Använd Power BI tooanalyze data som lagras i Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 6a1bfa80fd1b0dda59b7eaaae9ca1585ba42783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analysera data i Data Lake Store med hjälp av Power BI
I den här artikeln får du lära dig hur toouse Power BI Desktop tooanalyze och visualisera data som lagras i Azure Data Lake Store.

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-konto**. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md). Den här artikeln förutsätter att du redan har skapat ett Data Lake Store-konto, som kallas **mybidatalakestore**, och överföra en exempeldatafil (**Drivers.txt**) tooit. Den här exempelfilen är tillgängliga för nedladdning från [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. Du kan ladda ned det från [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Skapa en rapport i Power BI Desktop
1. Starta Power BI Desktop på datorn.
2. Från hello **Start** band klickar du på **hämta Data**, och klicka sedan på mer. I hello **hämta Data** dialogrutan klickar du på **Azure**, klickar du på **Azure Data Lake Store**, och klicka sedan på **Anslut**.
   
    ![Anslut tooData Datasjölager](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Anslut tooData Lake Store")
3. Om du ser en dialogruta om hello connector som i en utvecklingsfasen, välja toocontinue.
4. I hello **Microsoft Azure Data Lake Store** dialogrutan Ange hello URL tooyour Data Lake Store-konto och klicka sedan på **OK**.
   
    ![URL för Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL för Data Lake Store")
5. Hello nästa i dialogrutan klickar du på **logga in** toosign till Data Lake Store-konto. Du omdirigeras tooyour organisation inloggningssidan. Följ hello prompter toosign hello hänsyn.
   
    ![Logga in på Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "logga in på Data Lake Store")
6. När du har loggat in, klickar du på **Anslut**.
   
    ![Anslut tooData Datasjölager](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Anslut tooData Lake Store")
7. hello nästa dialogruta visar hello filen du överförde tooyour Data Lake Store-konto. Kontrollera hello information och klickar sedan på **belastningen**.
   
    ![Läs in data från Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "läsa in data från Data Lake Store")
8. När hello data har lästs in Power BI, visas följande fält i hello hello **fält** fliken.
   
    ![Importera fält](./media/data-lake-store-power-bi/imported-fields.png "importeras fält")
   
    Dock toovisualize och analysera hello data, vi föredrar hello data toobe tillgängliga per hello följande fält
   
    ![Önskade fält](./media/data-lake-store-power-bi/desired-fields.png "önskade fält")
   
    I hello nästa steg ska uppdaterar vi hello frågan tooconvert hello importerade data i hello önskade format.
9. Från hello **Start** band klickar du på **redigera frågor**.
   
    ![Redigera frågor](./media/data-lake-store-power-bi/edit-queries.png "redigera frågor")
10. I hello Query Editor under hello **innehåll** kolumnen, klickar du på **binär**.
    
    ![Redigera frågor](./media/data-lake-store-power-bi/convert-query1.png "redigera frågor")
11. Du ser en filikon som representerar hello **Drivers.txt** -fil som du har överfört. Högerklicka på hello-filen och klicka på **CSV**.    
    
    ![Redigera frågor](./media/data-lake-store-power-bi/convert-query2.png "redigera frågor")
12. Du bör se utdata som visas nedan. Dina data finns nu i ett format som du kan använda toocreate visualiseringar.
    
    ![Redigera frågor](./media/data-lake-store-power-bi/convert-query3.png "redigera frågor")
13. Från hello **Start** band klickar du på **stängs och**, och klicka sedan på **stängs och**.
    
    ![Redigera frågor](./media/data-lake-store-power-bi/load-edited-query.png "redigera frågor")
14. När hello frågan uppdateras hello **fält** fliken visar hello nya fält som är tillgängliga för visualisering.
    
    ![Uppdatera fält](./media/data-lake-store-power-bi/updated-query-fields.png "uppdatera fält")
15. Låt oss skapa ett cirkeldiagram toorepresent hello drivrutiner i varje ort för ett visst land. toodo, se hello följande val.
    
    1. Klicka på hello symbolen för ett cirkeldiagram hello visualiseringar på fliken.
       
        ![Skapa cirkeldiagram](./media/data-lake-store-power-bi/create-pie-chart.png "skapa cirkeldiagram")
    2. hello kolumner som vi toouse är **kolumner 4** (namn på hello ort) och **kolumn 7** (namnet på hello land). Dra dessa kolumner från **fält** fliken för**visualiseringar** fliken enligt nedan.
       
        ![Skapa grafik](./media/data-lake-store-power-bi/create-visualizations.png "skapa grafik")
    3. hello cirkeldiagram bör nu se ut ungefär som hello som visas nedan.
       
        ![Cirkeldiagram](./media/data-lake-store-power-bi/pie-chart.png "skapa grafik")
16. Du kan nu se hello antal drivrutiner i varje ort i hello valt land genom att välja ett visst land från hello sidfilter nivå. Till exempel under hello **visualiseringar** fliken, under **sidan nivå filter**väljer **Brasilien**.
    
    ![Välj ett land](./media/data-lake-store-power-bi/select-country.png "Välj land")
17. hello cirkeldiagram är uppdateras automatiskt toodisplay hello drivrutiner i hello orter av Brasilien.
    
    ![Drivrutiner i ett land](./media/data-lake-store-power-bi/driver-per-country.png "drivrutiner per land")
18. Från hello **filen** -menyn klickar du på **spara** toosave hello visualiseringen som en Power BI Desktop-fil.

## <a name="publish-report-toopower-bi-service"></a>Publicera rapporten tooPower BI-tjänsten
När du har skapat hello visualiseringar i Power BI Desktop kan dela du den med andra genom att publicera den toohello Power BI-tjänsten. Anvisningar för hur toodo som finns i [publicera från Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Se även
* [Analysera data i Data Lake Store med hjälp av Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

