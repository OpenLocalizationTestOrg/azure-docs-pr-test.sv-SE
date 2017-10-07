---
title: "aaaRetrain en klassisk webbtjänst | Microsoft Docs"
description: "Lär dig hur tooprogrammatically träna om en modell och uppdatera hello web service toouse hello nyligen trained model i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a>Omtrimma en klassisk webbtjänst
hello förutsägande webbtjänst som du har distribuerat är hello standard bedömningsslutpunkten. Slutpunkter hålls synkroniserade med hello ursprungliga träning och bedömningen experiment, och därför hello tränad modell för hello standardslutpunkten kan inte ersättas. webbtjänst för tooretrain hello, måste du lägga till en ny slutpunkt toohello-webbtjänst. 

## <a name="prerequisites"></a>Krav
Du måste ha konfigurerat en träningsexperiment och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> Hej prediktivt experiment måste distribueras som klassisk machine learning-webbtjänst. 
> 
> 

Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Lägg till en ny slutpunkt
hello förutsägande webbtjänst som du har distribuerat innehåller en bedömningsslutpunkten som hålls synkroniserade med hello ursprungliga utbildning och bedömningsprofil experiment tränad modell. tooupdate din web service toowith en ny tränad modell, måste du skapa en ny bedömningsprofil slutpunkt. 

toocreate en ny bedömningsprofil slutpunkt på hello förutsägande webbtjänst som kan uppdateras med hello tränade modellen:

> [!NOTE]
> Var noga med att du lägger till hello endpoint toohello förutsägande webbtjänst, inte hello utbildning webbtjänsten. Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges. hello förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.
> 
> 

Det finns tre sätt som du kan lägga till en ny slutpunkt tooa webbtjänst:

1. Programmässigt
2. Använd hello webbtjänster för Microsoft Azure-portalen
3. Använd hello klassiska Azure-portalen

### <a name="programmatically-add-an-endpoint"></a>Lägga till en slutpunkt
Du kan lägga till bedömningsprofil slutpunkter som använder hello exempelkoden i detta [github-lagringsplatsen](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a>Använd hello webbtjänster för Microsoft Azure portal tooadd en slutpunkt
1. Klicka på webbtjänster på hello navigeringen till vänster kolumn i Machine Learning Studio.
2. Hello längst ned på hello web service instrumentpanelen, klickar du på **hantera slutpunkter preview**.
3. Klicka på **Lägg till**.
4. Skriv ett namn och beskrivning för hello ny slutpunkt. Välj hello loggningsnivån och om exempeldata är aktiverad. Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a>Använd hello Azure klassiska portal tooadd en slutpunkt
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Hello vänstra menyn klickar du på **Maskininlärning**.
3. Klicka på arbetsytan under namn och klicka sedan på **Web Services**.
4. Under namn, klickar du på **inventering modellen [förutsägande exp].** .
5. Hello längst hello-sidan, klickar du på **Lägg till slutpunkt**. Mer information om att lägga till slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md). 

## <a name="update-hello-added-endpoints-trained-model"></a>Uppdatera hello läggs slutpunktens Trained Model
toocomplete hello omtränings-processen, måste du uppdatera hello tränad modell för hello ny slutpunkt som du har lagt till.

* Om du har lagt till hello ny slutpunkt med hello klassiska Azure-portalen kan du klicka på hello ny slutpunkt i hello-portalen och sedan hello **UpdateResource** länka tooget hello URL måste tooupdate hello endpoint modellen.
* Om du har lagt till hello slutpunkten med hjälp av hello exempelkod inkluderar platsen för hello Hjälpsidans URL som identifieras av hello *HelpLocationURL* värdet i hello utdata.

URL till tooretrieve hello-sökväg:

1. Kopiera och klistra in hello URL i webbläsaren.
2. Klicka på länken för hello Update resurs.
3. Kopiera hello POST URL för hello PATCH-begäran. Exempel:
   
     KORRIGERING AV URL: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2

Du kan nu använda hello tränade modellen tooupdate hello bedömningsslutpunkten som du skapade tidigare.

hello följande exempelkod visar hur toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, och KORRIGERA URL tooupdate hello slutpunkt.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

Hej *apiKey* och hello *endpointUrl* för hello-anropet kan hämtas från slutpunkten instrumentpanelen.

Hej värdet för hello *namn* parameter i *resurser* bör matcha hello resursnamn av hello sparas Trained Model i hello prediktivt experiment. tooget hello resursnamn:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Hello vänstra menyn klickar du på **Maskininlärning**.
3. Klicka på arbetsytan under namn och klicka sedan på **Web Services**.
4. Under namn, klickar du på **inventering modellen [förutsägande exp].** .
5. Klicka på ny hello slutpunkt som du har lagt till.
6. Hello endpoint instrumentpanelen, klicka på **uppdatering resurs**.
7. Hello Update resurs API-dokumentationen sidan för hello webbtjänsten hittar hello **resursnamnet** under **uppdateras resurser**.

Om din SAS-token upphör att gälla innan du har uppdaterat hello slutpunkt, måste du utföra GET med hello jobb-Id tooobtain en ny token.

När hello koden har körts ska hello ny slutpunkt börja använda hello retrained modellen i cirka 30 sekunder.

## <a name="summary"></a>Sammanfattning
Med hjälp av Hej Omtränings-API: er, kan du uppdatera hello tränad modell för en förutsägbar webbtjänst som aktiverar scenarier:

* Periodiska modell via programmering med nya data.
* Distribution av en modell toocustomers med hello målet för att låta dem träna om hello modellen med sina egna data.

## <a name="next-steps"></a>Nästa steg
[Felsöka hello omtränings av en klassisk Azure Machine Learning-webbtjänst](machine-learning-troubleshooting-retraining-models.md)

