---
title: "aaaUsing Import/Export av Data i Azure Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur toouse hello importera Data och exportera Data moduler toosend och ta emot data från en webbtjänst."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Distribuera Azure ML-webbtjänster som använder moduler för dataimport och dataexport

När du skapar en prediktivt experiment kan du vanligtvis lägga till en webbtjänst och utgående. När du distribuerar hello experiment kan konsumenterna skicka och ta emot data från webbtjänsten hello via hello indata och utdata. För vissa program, kan en konsument vara tillgänglig från en datafeed eller redan finns i en extern datakälla, till exempel Azure Blob storage. I dessa fall kan behöver de inte läsa och skriva data med hjälp av web service in- och utdataenheter. De kan i stället använda hello Batch Execution Service BES-tooread data från hello-datakälla med hjälp av en modul importera Data och skriva hello bedömningen resultat tooa olika plats med hjälp av en modul exportera Data.

hello importera Data och exportera data moduler kan läsa från och skriva toovarious uppgifter platser, till exempel en URL via HTTP, en Hive-fråga, en Azure SQL-databas, Azure Table storage, Azure Blob storage Data Feed tillhandahåller eller en lokal SQL-databas.

Det här avsnittet använder hello ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel och förutsätter datamängden hello har redan lästs i en Azure SQL-tabell med namnet censusdata.

## <a name="create-hello-training-experiment"></a>Skapa hello utbildning experiment
När du öppnar hello ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel den använder hello exempeldatamängd vuxna inventering intäkter binär klassificering. Och hello experiment hello arbetsytan ser liknande toohello följande bild:

![Inledande konfiguration av hello experiment.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

tooread hello data från hello Azure SQL-tabellen:

1. Ta bort hello dataset modul.
2. Skriv import i sökrutan för hello komponenter.
3. Hello resultatlistan lägga till en *importera Data* modulen toohello experimentera arbetsytan.
4. Ansluta utdata från hello *importera Data* hello modulindata av hello *Rensa Data som saknas* modul.
5. Välj i egenskapsrutan, **Azure SQL Database** i hello **datakällan** listrutan.
6. I hello **Databasservernamnet**, **databasnamnet**, **användarnamn**, och **lösenord** ange hello lämplig information för din databas.
7. Ange hello följande fråga i hello databasen fråga fältet.
   
     Välj [ålder]
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     från dbo.censusdata;
8. Hello längst ned på hello experimentet klickar du på **kör**.

## <a name="create-hello-predictive-experiment"></a>Skapa hello prediktivt experiment
Nästa ställa in hello prediktivt experiment som du distribuerar webbtjänsten.

1. Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänst (rekommenderas)**.
2. Ta bort hello *webbtjänst* och *Web Service utdata moduler* från hello prediktivt experiment. 
3. Skriv export i sökrutan för hello komponenter.
4. Hello resultatlistan lägga till en *exportera Data* modulen toohello experimentera arbetsytan.
5. Ansluta utdata från hello *Poängmodell* hello modulindata av hello *exportera Data* modul. 
6. Välj i egenskapsrutan, **Azure SQL Database** i hello data mål listrutan.
7. I hello **Databasservernamnet**, **databasnamnet**, **Server användarkontonamnet**, och **serverlösenord** ange hello lämplig information för din databas.
8. I hello **kommaavgränsad lista över kolumner toobe sparade** anger poängsatta etiketter.
9. I hello **Data tabell namnfältet**, Skriv dbo. ScoredLabels. Om hello tabellen inte finns, skapas den när hello experiment körs eller hello webbtjänst anropas.
10. I hello **kommaavgränsad lista med kolumner som datatable** anger ScoredLabels.

När du skriver ett program att anrop hello slutliga webbtjänst kanske du vill toospecify en annan fråga eller mål indatatabellen vid körning. tooconfigure dessa indata och utdata, använda hello Webbtjänstparametrar funktionen tooset hello *importera Data* modulen *datakällan* egenskap och hello *exportera Data* läge data mål-egenskap.  Mer information om Webbtjänstparametrar finns hello [AzureML Webbtjänstparametrar post](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) på hello Cortana Intelligence och Machine Learning-bloggen.

tooconfigure hello Webbtjänstparametrar för hello importera fråga och hello Måltabell:

1. I hello egenskaper för hello *importera Data* modulen, klicka på hello längst hello uppifrån höger om hello **databasfrågan** fältet och välj **anges som parameter för web service**.
2. I hello egenskaper för hello *exportera Data* modulen, klicka på hello längst hello uppifrån höger om hello **Data tabellnamn** fältet och välj **anges som parameter för web service**.
3. Längst ned hello hello *exportera Data* modulen egenskapsrutan i hello **Webbtjänstparametrar** , på databasfrågan och byta namn på den frågan.
4. Klicka på **Data tabellnamn** och Byt namn på den **tabellen**.

När du är klar experimentet bör se ut ungefär toohello följande bild:

![Sista utseendet på experimentet.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Du kan nu distribuera hello experiment som en webbtjänst.

## <a name="deploy-hello-web-service"></a>Distribuera hello-webbtjänst
Du kan distribuera tooeither en klassisk eller ny webbtjänst.

### <a name="deploy-a-classic-web-service"></a>Distribuera en klassiska webbtjänst
toodeploy som en klassiska webbtjänst och skapa ett program tooconsume den:

1. Klicka på Kör hello nedre kanten av hello experimentet.
2. När du kör hello har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]**.
3. Leta upp din API-nyckel på hello web service instrumentpanelen. Kopiera och spara den toouse senare.
4. I hello **standard Endpoint** tabell genom att klicka på hello **Batch Execution** länk tooopen hello API-hjälpsidan.
5. Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.
6. Hitta hello på hello API-hjälpsidan **exempelkod** avsnitt på hello hello sidans nederkant.
7. Kopiera och klistra in hello C#-exempelkod i Program.cs-filen och ta bort alla referenser toohello blob-lagring.
8. Uppdatera hello värdet för hello *apiKey* variabeln med hello API-nyckel som sparats tidigare.
9. Leta upp hello begäran deklaration och uppdatera hello värdena för Webbtjänstparametrar som skickas toohello *importera Data* och *exportera Data* moduler. I så fall måste du använda hello ursprungliga frågan, men definiera ett nytt tabellnamn.
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Kör hello program. 

För slutförande av hello kör läggs en ny tabell toohello databas som innehåller hello bedömningen resultat.

### <a name="deploy-a-new-web-service"></a>Distribuera en ny webbtjänst

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

toodeploy som en ny webbtjänst och skapa ett program tooconsume den:

1. Hello längst ned på hello experimentet klickar du på **kör**.
2. När du kör hello har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [ny]**.
3. Ange ett namn för din webbtjänsten hello distribuera Experiment på sidan och välja en prisavtal, och klicka sedan **distribuera**.
4. På hello **Quickstart** klickar du på **förbruka**.
5. I hello **exempelkod** klickar du på **Batch**.
6. Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.
7. Kopiera och klistra in hello C# exempelkoden i filen Program.cs.
8. Uppdatera hello värdet för hello *apiKey* variabeln med hello **primärnyckel** finns i hello **grundläggande förbrukning info** avsnitt.
9. Leta upp hello *scoreRequest* deklaration och uppdatera hello värdena för Webbtjänstparametrar som skickas toohello *importera Data* och *exportera Data* moduler. I så fall måste du använda hello ursprungliga frågan, men definiera ett nytt tabellnamn.
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Kör hello program. 

