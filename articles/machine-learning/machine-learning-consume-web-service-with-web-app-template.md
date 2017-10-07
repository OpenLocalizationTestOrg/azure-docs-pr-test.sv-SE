---
title: "aaaConsume en Machine Learning-webbtjänst med en mall för appen | Microsoft Docs"
description: "Använd en mall för app i Azure Marketplace tooconsume förutsägande webbtjänst i Azure Machine Learning."
keywords: "maskininlärning för webbtjänst, operationalization REST API"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Använda en Azure Machine Learning-webbtjänst med en webbappmall

När du har utvecklat din förutsägelsemodell och distribueras som en Azure-webbtjänst med hjälp av Machine Learning Studio eller med verktyg som R eller Python kan du komma åt hello operationalized modellen med hjälp av REST-API.

Det finns ett antal sätt tooconsume hello REST-API och åtkomst hello webbtjänsten. Exempelvis kan du skriva ett program i C#, R eller Python med hello exempelkod som skapas automatiskt när du distribuerade hello-webbtjänst (tillgänglig i hello [Machine Learning Web Services-portalen](https://services.azureml.net/quickstart) eller i hello web service instrumentpanelen i Machine Learning Studio). Du kan också använda hello exempel Microsoft Excel-arbetsbok på hello samtidigt.

Men hello snabbaste och enklaste sättet tooaccess webbtjänsten är via hello Web App mallar som är tillgängliga i hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a>hello mallar för Azure Machine Learning-App
hello web app mallar som är tillgängliga i hello Azure Marketplace kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och förväntat resultat. Allt du behöver toodo är att ge hello app åtkomst tooyour webbtjänst och data och hello mallen hello rest.

Två mallar är tillgängliga:

* [Azure ML-svar på begäranden tjänstmall Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML-Batch Execution Service Web App mall](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Varje mall skapar ett exempel ASP.NET-program med hjälp av hello API-URI och nyckel för webbtjänsten, och distribuerar den som en webbplats tooAzure. hello skapas svar på begäranden tjänsten (RR) en webbapp som du kan använda toosend en enda rad med data toohello web service tooget ett enskilt resultat. hello Batch Execution Service BES-mallen skapar ett webbprogram som du kan använda toosend många rader med data tooget flera resultat.

Ingen kodning är obligatoriska toouse dessa mallar. Du ange bara hello API-nyckel och URI och hello mallen skapar hello program åt dig.

tooget hello API-nyckel och Begärd URI för en webbtjänst:

1. I hello [Web Services-portalen](https://services.azureml.net/quickstart), en ny webbtjänst klickar du på **Web Services** hello överst. Eller för en klassisk web service klickar du på **klassiska webbtjänster**.
2. Klicka på hello-webbtjänst som du vill tooaccess.
3. Klicka på hello-slutpunkt som du vill använda tooaccess för klassisk-webbtjänsten.
4. Klicka på **förbruka** hello överst.
5. Kopiera hello **primära** eller **sekundärnyckeln** och spara den.
6. Om du skapar en mall för svar på begäranden tjänsten (RR) kan du kopiera hello **begäran och svar** URI och spara den. Om du skapar en mall för Batch Execution Service BES-, kopiera hello **gruppbegäranden** URI och spara den.


## <a name="how-toouse-hello-request-response-service-rrs-template"></a>Hur toouse hello svar på begäranden tjänsten (RR) mall
Följ dessa steg toouse hello Resursposter web app mall, som visas i följande diagram hello.

![Web processmall toouse Resursposter][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Gå toohello [Azure-portalen](https://portal.azure.com), **inloggning**, klickar du på **ny**, söka efter och välj **Azure ML-svar på begäranden Service Web App**, klicka på **Skapa**. 
   
   * Ge ett unikt namn för ditt webbprogram. hello-URL för hello webbprogrammet blir namnet följt av `.azurewebsites.net.` t.ex.`http://carprediction.azurewebsites.net.`
   * Välj hello Azure-prenumeration och tjänster som webbtjänsten körs under.
   * Klicka på **Skapa**.
     
     ![Skapa webbapp][image5]

4. När Azure distribution hello webbprogrammet har slutförts, klickar du på hello **URL** på hello inställningssidan för web app i Azure, eller ange hello URL i en webbläsare. Till exempel, `http://carprediction.azurewebsites.net.`
5. När hello web app första körs den ber dig om hello **API Post URL** och **API-nyckeln**.
   Ange hello-värden som du sparade tidigare (**Begärd URI** och **API-nyckel**respektive).
     
     Klicka på **skicka**.
     
     ![Ange Post URI och API-nyckel][image6]

6. Hej web app visar dess **Webbappkonfigurationen** sidan med hello aktuella webbtjänstinställningar. Här kan du ändra toohello inställningarna som används av hello webbprogrammet.
   
   > [!NOTE]
   > Hello inställningar här endast ändras dem för det här webbprogrammet. Hello standardinställningarna för webbtjänsten ändras inte. Till exempel om du ändrar hello **beskrivning** här ändringen inte hello beskrivning visas på hello web service instrumentpanelen i Machine Learning Studio.
   > 
   > 
   
    När du är klar klickar du på **spara ändringar**, och klicka sedan på **gå tooHome sidan**.

7. Startsidan kan du ange värden från hello toosend tooyour-webbtjänsten. Klicka på **skicka** när du är klar och hello resultat returneras.

Om du vill tooreturn toohello **Configuration** sidan finns toohello `setting.aspx` sidan av hello webbprogram. Till exempel: `http://carprediction.azurewebsites.net/setting.aspx.` du kommer att tillfrågas tooenter hello API-nyckeln igen – du behöver att tooaccess hello sidan och uppdatera inställningarna för hello.

Du kan stoppa, starta om eller ta bort hello webbprogram i hello Azure-portalen som andra webbprogram. Du kan bläddra toohello hem webbadressen och ange nya värden så länge som den körs.

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a>Hur toouse hello Batch Execution Service BES-mall
Du kan använda hello BES web app mall i hello samma sätt som hello RR-mall, förutom att hello-webbprogram som har skapats kan du toosubmit flera rader med data och ta emot flera resultat.

hello indatavärden för en webbtjänst för batch-körningen kan komma från Azure-lagring eller en lokal fil. hello resultat lagras i en Azure storage-behållare.
Därför ska du behöver ett Azure storage-behållare toohold hello resultaten som returnerades av hello webbprogram och du behöver tooget dina indata redo.

![Bearbeta toouse BES webbmall][image2]

1. Följ hello samma procedur toocreate hello BES webbapp som hello Resursposter mall, utom gå för[Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES-mall i Azure Marketplace och klicka på **skapa webbprogram** .

2. toospecify där du vill att hello resultatet lagras, ange hello mål behållaren information på startsidan för hello web app. Ange var hello webbprogrammet kan få hello indatavärden, antingen i en lokal fil eller en Azure storage-behållare.
   Klicka på **skicka**.
   
    ![Storage-informationen][image7]

hello webbprogrammet visas en sida med jobbstatus.
När hello jobbet har slutförts får du hello platsen för hello resultat i Azure blob storage. Du kan också ha hello alternativet för att hämta hello resultat tooa lokal fil.

## <a name="for-more-information"></a>Mer information
Mer information om toolearn...

* Skapa ett experiment i machine learning med Machine Learning Studio finns [skapa ditt första experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md)
* hur toodeploy din maskininlärning experiment som en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md)
* andra sätt tooaccess webbtjänsten, se [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md)

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
