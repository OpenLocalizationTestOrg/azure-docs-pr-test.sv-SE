---
title: aaaAzure integration Stream Analytics och Machine Learning | Microsoft Docs
description: "Hur toouse en användardefinierad funktion och Machine Learning i ett Stream Analytics-jobb"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Utföra sentiment analys med hjälp av Azure Stream Analytics och Azure Machine Learning
Den här artikeln beskriver hur tooquickly inställt på ett enkelt Azure Stream Analytics-jobb som integrerar Azure Machine Learning. Du använder en Machine Learning sentiment analytics modell från hello Cortana Intelligence Gallery tooanalyze strömmande textdata och fastställa hello sentiment poäng i realtid. Med hello Cortana Intelligence Suite kan du utföra den här uppgiften utan att oroa hello krångligheter för att skapa en sentiment analytics modell.

Du kan använda det du lära dig från den här artikeln tooscenarios som dessa:

* Analysera realtid sentiment direktuppspelning Twitter-data.
* Analysera poster kunden chattar med supportpersonalen.
* Utvärderar kommentarer om forum, bloggar och videor. 
* Många andra realtid, förutsägbara bedömningsprofil scenarier.

Du skulle få hello data direkt från en Twitter-dataström i ett verkligt scenario. toosimplify hello självstudiekursen kommer vi har skrivit det så att hello Streaming Analytics-jobbet hämtar tweets från en CSV-fil i Azure Blob storage. Du kan skapa egna CSV-fil eller du kan använda en exempel-CSV-fil som visas i följande bild hello:

![exempel tweets i en CSV-fil](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

hello Streaming Analytics-jobbet som du skapar gäller hello sentiment analytics modellen som en användardefinierad funktion (UDF) på hello text exempeldata från hello blobstore. hello (hello resultatet av hello sentiment analys) skrivs toohello samma blobstore i en annan CSV-fil. 

hello visar följande bild den här konfigurationen. Som anges för en mer realistisk scenario ersätta du blob-lagring med streaming Twitter-data från Azure Event Hubs indata. Dessutom kan du bygga en [Microsoft Power BI](https://powerbi.microsoft.com/) realtid visualisering av hello sammanställd sentiment.    

![Stream Analytics Machine Learning-integration: översikt](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Krav
Innan du börjar bör du kontrollera att du har hello följande:

* En aktiv Azure-prenumeration.
* En CSV-fil med vissa data i den. Du kan hämta hello-filen som visades tidigare från [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), eller skapa en egen fil. I den här artikeln förutsätter att du använder hello-filen från GitHub.

På en hög nivå toocomplete hello aktiviteter visas i den här artikeln får du hello följande:

1. Skapa ett Azure storage-konto och en behållare för blob storage och överför en CSV-formaterad indatafilen toohello behållare.
3. Lägga till en sentiment analytics modell från hello Cortana Intelligence Gallery tooyour Azure Machine Learning-arbetsytan och distribuera den här modellen som en webbtjänst i hello Machine Learning-arbetsytan.
5. Skapa ett Stream Analytics-jobb som anropar den här webbtjänsten som en funktion i ordning toodetermine sentiment för hello textinmatning.
6. Starta hello Stream Analytics-jobbet och kontrollera hello utdata.

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a>Skapa en lagringsbehållare och överför inkommande hello CSV-fil
Du kan använda en CSV-fil, till exempel hello en tillgänglig från GitHub för det här steget.

1. I hello Azure-portalen klickar du på **ny** &gt; **lagring** &gt; **lagringskonto**.

   ![Skapa nytt lagringskonto](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. Ange ett namn (`samldemo` i hello exemplet). hello namn kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure. 

3. Ange en befintlig resursgrupp och ange en plats. Plats, rekommenderar vi att alla hello resurser som skapats i denna självstudiekurs Använd hello samma plats.

    ![Ange information om lagringskonto](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. Välj hello storage-konto i hello Azure-portalen. I hello lagring-kontoblad klickar du på **behållare** och klicka sedan på  **+ &nbsp;behållare** toocreate blob-lagring.

    ![Skapa blob-behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. Ange ett namn för hello behållare (`azuresamldemoblob` i hello exempel) och kontrollera att **komma åt typen** har angetts för**Blob**. Klicka på **OK** när du är klar.

    ![Ange information om blob-behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. I hello **behållare** bladet, Välj hello ny behållare, vilket öppnar bladet hello för behållaren.

7. Klicka på **Överför**.

    ![Överför knapp för en behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. I hello **överför blob** bladet ange hello CSV-fil som du vill toouse för den här självstudiekursen. För **Blob typen**väljer **blockblob** och ange hello blockera storlek too4 MB, vilket är tillräcklig för den här kursen.

    ![Överför blob-fil](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. Klicka på hello **överför** knappen längst ned hello hello-bladet.

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a>Lägga till hello sentiment analytics modell från hello Cortana Intelligence Gallery

Nu när hello exempeldata i en blob, kan du aktivera hello sentiment analysmodell i Cortana Intelligence Gallery.

1. Gå toohello [förutsägande sentiment analytics modellen](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) sida i hello Cortana Intelligence Gallery.  

2. Klicka på **öppna i Studio**.  
   
   ![Strömma Analytics Machine Learning, öppna Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Logga in toogo toohello arbetsytan. Välj en plats.

4. Klicka på **kör** på hello hello sidans nederkant. hello processen körs som tar ungefär en minut.

   ![Kör experiment i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. När hello processen har körts, Välj **distribuera webbtjänsten** på hello hello sidans nederkant.

   ![distribuera experiment i Machine Learning Studio som en webbtjänst](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. toovalidate som hello sentiment analytics modellen är klar toouse klickar du på hello **Test** knappen. Ange text som indata som ”jag älskar Microsoft”. 

   ![Testa experiment i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Om hello test fungerar, visas en liknande toohello resultat som följande exempel:

   ![testresultaten i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. I hello **appar** kolumn, och klicka på hello **Excel 2010 eller tidigare arbetsboken** länk toodownload en Excel-arbetsbok. hello arbetsboken innehåller hello en API-nyckeln och hello-URL som du behöver senare tooset in hello Stream Analytics-jobbet.

    ![Stream Analytics Machine Learning, snabböversikten](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a>Skapa ett Stream Analytics-jobb som använder hello Machine Learning-modellen

Du kan nu skapa ett Stream Analytics-jobb som läser hello exempel tweets från hello CSV-fil i blob storage. 

### <a name="create-hello-job"></a>Skapa hello jobb

1. Gå toohello [Azure-portalen](https://portal.azure.com).  

2. Klicka på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**. 

   ![Azure portal sökvägen för att hämta tooa nya Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. Hello jobb `azure-sa-ml-demo`, ange en prenumeration, ange en befintlig resursgrupp eller skapa en ny och välj hello plats för hello jobb.

   ![Ange inställningar för nya Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a>Konfigurera hello jobbet indata
hello jobbet hämtar indata från hello CSV-fil som du överfört tidigare tooblob lagring.

1. När hello jobbet har skapats under **jobbet topologi** i hello jobb-bladet, klickar du på hello **indata** rutan.  
   
   !['Indata-rutan i Stream Analytics-jobbet bladet](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. I hello **indata** bladet, klickar du på **+ Lägg till**.

   ![”Knappen Lägg till' för att lägga till ett inkommande toohello Stream Analytics-jobb](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. Fyll i hello **nya indata** bladet med dessa värden:

    * **Ett inmatat alias**: Använd hello namn `datainput`.
    * **Typ av datakälla**: Välj **dataströmmen**.
    * **Källan**: Välj **Blob storage**.
    * **Importera alternativet**: Välj **använda blob storage från aktuell prenumeration**. 
    * **Lagringskontot**. Välj hello storage-konto som du skapade tidigare.
    * **Behållaren**. Välj hello-behållare som du skapade tidigare (`azuresamldemoblob`).
    * **Händelsen serialiseringsformat**. Välj **CSV**.

    ![Inställningar för nya jobb indata](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. Klicka på **Skapa**.

### <a name="configure-hello-job-output"></a>Konfigurera hello jobbutdata
hello jobbet skickar resultaten toohello samma blob storage där den hämtar indata. 

1. Under **jobbet topologi** i hello jobb-bladet, klickar du på hello **utdata** rutan.  
  
   ![Skapa nya utdata för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. I hello **utdata** bladet, klickar du på **+ Lägg till**, och sedan lägga till utdata med hello-alias `datamloutput`. 

3. För **Sink**väljer **Blob storage**. Fyller i hello resten av hello utdata inställningar med hjälp av hello samma värden som du använde för hello blob-lagring för indata:

    * **Lagringskontot**. Välj hello storage-konto som du skapade tidigare.
    * **Behållaren**. Välj hello-behållare som du skapade tidigare (`azuresamldemoblob`).
    * **Händelsen serialiseringsformat**. Välj **CSV**.

   ![Inställningar för nya jobbutdata](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. Klicka på **Skapa**.   


### <a name="add-hello-machine-learning-function"></a>Lägg till hello Machine Learning-funktion 
Tidigare du har publicerat en webbtjänsten för Machine Learning modellen tooa. I vårt scenario, när hello dataströmmen analys jobbet körs, skickas varje prov tweet från hello inkommande toohello webbtjänst för sentiment analys. hello Machine Learning-webbtjänst returnerar en sentiment (`positive`, `neutral`, eller `negative`) och sannolikhet hello tweet vara positivt. 

I det här avsnittet av kursen hello definierar du en funktion i hello dataströmmen analys jobbet. hello-funktionen kan anropade toosend en tweet toohello webbtjänst och komma hello svar. 

1. Kontrollera att du har hello web service URL: en och API-nyckeln som du hämtade tidigare i hello Excel-arbetsboken.

2. Returnera toohello jobbet översikt bladet.

3. Under **inställningar**väljer **funktioner** och klicka sedan på **+ Lägg till**.

   ![Lägga till en funktion toohello Stream Analytics-jobb](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. Ange `sentiment` som hello fungera alias och fylla i hello resten av hello blad med hjälp av dessa värden:

    * **Funktionen typen**: Välj **Azure ML**.
    * **Importera alternativet**: Välj **importera från en annan prenumeration**. Detta ger dig en möjlighet tooenter hello URL och en nyckel.
    * **URL: en**: klistra in hello webbtjänst-URL.
    * **Nyckeln**: klistra in hello API-nyckel.
  
    ![Inställningar för att lägga till en Machine Learning-funktionen toohello Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. Klicka på **Skapa**.

### <a name="create-a-query-tootransform-hello-data"></a>Skapa en fråga tootransform hello data

Stream Analytics använder en deklarativ, SQL-baserade frågan tooexamine hello indata och bearbetar den. I det här avsnittet skapar du en fråga som läser varje tweet från indata och anropar sedan hello Machine Learning-funktionen tooperform sentiment analys. hello frågan skickar sedan hello resultatet toohello utdata du definierat (blobblagring).

1. Returnera toohello jobbet översikt bladet.

2.  Under **jobbet topologi**, klicka på hello **frågan** rutan.

    ![Skapa en fråga för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. Ange hello följande fråga:

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    hello frågan anropar hello-funktionen som du skapade tidigare (`sentiment`) i ordning tooperform sentiment analys på varje tweet i hello indata. 

4. Klicka på **spara** toosave hello frågan.


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a>Starta hello Stream Analytics-jobbet och kontrollera hello-utdata

Du kan nu starta hello Stream Analytics-jobbet.

### <a name="start-hello-job"></a>Starta hello jobbet
1. Returnera toohello jobbet översikt bladet.

2. Klicka på **starta** hello överst i hello-bladet.

    ![Skapa en fråga för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. I hello **startjobb**väljer **anpassade**, och välj sedan en dag tidigare toowhen du överförde hello CSV tooblob fillagring. När du är klar klickar du på **starta**.  


### <a name="check-hello-output"></a>Kontrollera hello-utdata
1. Låt hello-jobbet körs i några minuter tills du ser aktivitet i hello **övervakning** rutan. 

2. Om du har ett verktyg som du vanligtvis använder tooexamine hello innehållet i blob-lagring kan använda den verktyget tooexamine hello `azuresamldemoblob` behållare. Du kan också hello följande i hello Azure-portalen:

    1. Hitta hello i hello portal `samldemo` lagring kontot och hitta hello i hello konto `azuresamldemoblob` behållare. Du ser två filer i hello behållare: hello-fil som innehåller hello exempel tweets och en CSV-fil som genererats av hello Stream Analytics-jobbet.
    2. Högerklicka på filen hello genereras och välj sedan **hämta**. 

   ![Ladda ned CSV jobbutdata från Blob storage](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Öppna hello genereras CSV-fil. Du ser något som liknar följande exempel hello:  
   
   ![Visa Stream Analytics, Machine Learning, CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Visa mått
Du kan också visa Azure Machine Learning-funktionen-relaterade mått. hello följande funktion-relaterade mått visas i hello **övervakning** rutan hello jobb-bladet:

* **Funktionen begäranden** anger hello antal förfrågningar som skickas tooa Machine Learning-webbtjänst.  
* **Funktionen händelser** anger hello antalet händelser i hello-begäran. Som standard varje begäran tooa Machine Learning-webbtjänst som innehåller too1 000 händelser.  


## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Integrera REST-API och Maskininlärning](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)



