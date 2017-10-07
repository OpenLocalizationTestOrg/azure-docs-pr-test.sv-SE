---
title: "aaaDeploy en Machine Learning-webbtjänst | Microsoft Docs"
description: "Hur tooconvert en utbildning experimentera tooa prediktivt experiment, förbereda för distribution och sedan distribuera det som en Azure Machine Learning-webbtjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9cb7af637632b2c3688c11483f29cf24df8fd065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Distribuera en Azure Machine Learning-webbtjänst
Azure Machine Learning kan du toobuild, testa och distribuera förutsägelseanalyslösningar.

Detta görs från en punkt-för-översikt, i tre steg:

* **[Skapa en träningsexperiment]**  -Azure Machine Learning Studio är en gemensam visual utvecklingsmiljö om du använder tootrain och testa en förutsägelseanalys modellen med hjälp av utbildningsdata som du anger.
* **[Konvertera den tooa prediktivt experiment]**  – när din modell har tränats med befintliga data och du är redo toouse den tooscore nya data, du förbereda och förenkla experimentet för förutsägelser.
* **[Distribuera den som en webbtjänst]**  -du kan distribuera din prediktivt experiment som en [nya] eller [klassiska] Azure-webbtjänsten. Användare kan skicka tooyour datamodellen och ta emot din modell förutsägelser.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Skapa ett experiment utbildning
tootrain en förutsägelseanalysmodell du använder Azure Machine Learning Studio toocreate träningsexperiment där du inkluderar olika moduler tooload utbildningsdata, förbereda hello data vid behov, tillämpa maskininlärningsalgoritmer och utvärdera hello resultat . Du kan iterera ett experiment och prova olika toocompare algoritmer för maskininlärning och utvärdera hello resultat.

hello processen att skapa och hantera utbildning experiment beskrivs mer i detalj någon annanstans. Mer information finns i följande artiklar:

* [Skapa ett enkelt experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md)
* [Utveckla en förutsägelselösning med Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
* [Importera utbildningsdata till Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
* [Hantera iterationer av experiment i Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Konvertera hello utbildning experiment tooa prediktivt experiment
När du har tränat modellen, är du redo tooconvert din utbildning experimentera till en prediktivt experiment tooscore data.

Genom att konvertera tooa förutsägande experiment, du får den tränade modellen redo toobe distribueras som en bedömningsprofil webbtjänst. Användare av hello-webbtjänsten kan skicka indata tooyour modell och din modell skickar tillbaka hello förutsägelse resultat. När du konverterar tooa förutsägande experiment, Tänk på hur du förväntar dig din modell toobe som används av andra.

tooconvert din utbildning experiment tooa förutsägande experiment, klickar du på **kör** hello längst ned på hello experimentet klickar du på **konfigurera Web Service**och välj **förutsägande webbtjänst** .

![Konvertera tooscoring experiment](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Mer information om hur tooperform den här konverteringen finns [hur tooprepare modellen för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).

hello följande steg beskriver hur du distribuerar en prediktivt experiment som en ny webbtjänst. Du kan också distribuera hello experiment som klassisk webbtjänst.

## <a name="deploy-it-as-a-web-service"></a>Distribuera den som en webbtjänst

Du kan distribuera hello prediktivt experiment som en ny webbtjänst eller som en klassisk webbtjänst.

### <a name="deploy-hello-predictive-experiment-as-a-new-web-service"></a>Distribuera hello prediktivt experiment som en ny webbtjänst
Nu när hello prediktivt experiment har förberetts, kan du distribuera den som en ny Azure-webbtjänst. Med hjälp av hello-webbtjänsten, användare kan skicka tooyour datamodellen och hello modellen returnerar dess förutsägelser.

toodeploy din förutsägande experimentera klickar du på **kör** längst hello hello experimentera arbetsytan. När hello experimentet har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [ny]**.  hello öppnas distribution av hello Machine Learning-webbtjänst portalen.

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning-webbtjänst portal distribuera Experiment sida
Ange ett namn för webbtjänsten hello hello distribuera Experiment på sidan.
Välj en prisnivå plan. Om du har en befintlig prisavtal kan du välja det annars skapa du en ny prisplan för hello-tjänsten.

1. I hello **prisplan** listrutan väljer du ett befintligt schema eller hello **Välj ny plan** alternativet.
2. I **schemanamn**, Skriv ett namn som identifierar hello plan på fakturan.
3. Välj en av hello **månatliga planera nivåer**. hello plan nivåerna standard toohello planer för standardregion och webbtjänsten är distribuerade toothat region.

Klicka på **distribuera** och hello **Quickstart** öppnas sidan för webbtjänsten.

hello-webbtjänstsida Quickstart ger dig åtkomst och vägledning på hello vanligaste uppgifter som du utför när du har skapat en webbtjänst. Härifrån kan du enkelt kan komma åt både hello Test och förbruka sida.

<!-- ![Deploy hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Testa din nya webbtjänsten
tootest din nya webbtjänsten klickar du på **testa webbtjänsten** under vanliga uppgifter. Du kan testa din webbtjänsten som ett svar på begäranden tjänsten (RR) eller en Batch Execution service (BES) hello Test på sidan.

hello Resursposter testsida visar hello indata, utdata och eventuella globala parametrar som du har definierat för hello experiment. tootest hello-webbtjänsten, du kan manuellt ange lämpliga värden för hello indata eller ange en fil med kommaavgränsade värden (CSV) formaterad fil som innehåller hello testvärden.

tootest med RRS, ange lämpliga värden för hello indata från läget för hello listan och klicka på **Test-svar på begäranden**. Förutsägelse resultat visas i hello utdata kolumnen toohello kvar.

![Distribuera hello-webbtjänst](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

tootest din BES Klicka **Batch**. Klicka på Bläddra under dina indata och välj en CSV-fil som innehåller rätt exempelvärden hello Batch test på sidan. Om du inte har en CSV-fil och du har skapat din prediktivt experiment med Machine Learning Studio, kan du hämta hello datauppsättning för experimentet förutsägbara och använda den.

toodownload hello datamängd, öppna Machine Learning Studio. Öppna din prediktivt experiment och högerklicka på hello indata för experimentet. Hello snabbmenyn, Välj **dataset** och välj sedan **hämta**.

![Distribuera hello-webbtjänst](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klicka på **Test**. hello status för jobbet Batch Execution visar toohello höger under **Test batchjobb**.

![Distribuera hello-webbtjänst](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

På hello **CONFIGURATION** sidan kan du ändra hello beskrivning, avdelning, uppdatera hello lagringskontonyckel och aktivera exempeldata för webbtjänsten.

![Konfigurera hello-webbtjänsten](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

När du har distribuerat hello-webbtjänsten, kan du:

* **Åtkomst** det via hello-webbtjänstens API.
* **Hantera** den via Azure Machine Learning web services-portalen eller hello klassiska Azure-portalen.
* **Uppdatera** det om din modell ändras.

#### <a name="access-your-new-web-service"></a>Få åtkomst till din nya webbtjänsten
När du distribuerar webbtjänsten från Machine Learning Studio, kan du skicka data toohello service och ta emot svar programmässigt.

Hej **förbruka** innehåller alla hello information som du behöver tooaccess webbtjänsten. Till exempel tillhandahålls hello API-nyckeln toohello tooallow behörighet access-tjänsten.

Mer information om åtkomst till en Machine Learning-webbtjänst finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Hantera nya webbtjänsten
Du kan hantera nya tjänster Machine Learning-webbtjänster webbportalen. Från hello [huvudsidan för Företagsportalen](https://services.azureml-test.net/), klickar du på **Web Services**. Du kan ta bort eller kopiera en tjänst från hello för tjänster webbsida. toomonitor en specifik tjänst på hello-tjänsten och klicka sedan på **instrumentpanelen**. toomonitor batchjobb som är associerade med hello webbtjänsten klickar du på **Batch begära loggen**.

### <a name="deploy-hello-predictive-experiment-as-a-classic-web-service"></a>Distribuera hello prediktivt experiment som ett klassiskt-webbtjänst

Nu när hello prediktivt experiment tillräckligt har förberetts, kan du distribuera den som en klassiska Azure-webbtjänst. Med hjälp av hello-webbtjänsten, användare kan skicka tooyour datamodellen och hello modellen returnerar dess förutsägelser.

toodeploy din förutsägande experimentera klickar du på **kör** längst hello hello experimentera arbetsytan och klicka sedan på **distribuera webbtjänsten**. hello-webbtjänsten har konfigurerats och du placeras i hello web service-instrumentpanelen.

![Distribuera hello-webbtjänst](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Testa din klassiska-webbtjänst

Du kan testa hello-webbtjänsten i hello Machine Learning-webbtjänster portalen eller Machine Learning Studio.

tootest hello Request Response-webbtjänsten, klicka på hello **Test** knappen hello web service instrumentpanelen. En dialogruta visas tooask du för hello indata för hello-tjänsten. Dessa är hello kolumner som förväntades av hello bedömningen experiment. Ange en uppsättning data och klicka sedan på **OK**. hello resultat som genererats av hello webbtjänsten visas längst ned hello hello instrumentpanelen.

Du kan klicka på hello **Test** Förhandsgranska länken tootest din tjänst i hello Azure Machine Learning-webbtjänster portal enligt tidigare hello nytt web service-avsnitt.

tootest hello Batch Execution Service klickar du på **Test** preview länk. Klicka på Bläddra under dina indata och välj en CSV-fil som innehåller rätt exempelvärden hello Batch test på sidan. Om du inte har en CSV-fil och du har skapat din prediktivt experiment med Machine Learning Studio, kan du hämta hello datauppsättning för experimentet förutsägbara och använda den.

![Testa hello-webbtjänst](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

På hello **CONFIGURATION** sidan kan du ändra hello service hello visningsnamn och en beskrivning. hello namn och beskrivning visas i hello [klassiska Azure-portalen](http://manage.windowsazure.com/) där du hanterar din webbtjänster.

Du kan ange en beskrivning för indata, utdata och web Serviceparametrar genom att ange en sträng för varje kolumn under **INMATNINGSSCHEMAT**, **UTDATASCHEMA**, och **Web SERVICE PARAMETERN**. Dessa beskrivningar används i hello exempel kod dokumentationen för hello-webbtjänsten.

Du kan aktivera loggning toodiagnose fel som du se när webbtjänsten används. Mer information finns i [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).

![Konfigurera hello-webbtjänsten](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Du kan också konfigurera hello slutpunkter för hello-webbtjänsten i hello Azure Machine Learning-webbtjänster portal liknande toohello procedur visas tidigare i hello nytt web service-avsnitt. hello alternativ som är tillgängliga kan du lägga till eller ändra hello beskrivning, aktivera loggning och aktivera exempeldata för testning.

#### <a name="access-your-classic-web-service"></a>Åtkomst till webbtjänsten klassisk
När du distribuerar webbtjänsten från Machine Learning Studio, kan du skicka data toohello service och ta emot svar programmässigt.

hello instrumentpanelen innehåller alla hello information du behöver tooaccess webbtjänsten. Till exempel tillhandahålls hello API-nyckeln tooallow behörighet toohello-tjänsten för dataåtkomst och API-artiklar finns toohelp du börjar skriva koden.

Mer information om åtkomst till en Machine Learning-webbtjänst finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Hantera webbtjänsten klassisk
Det finns olika åtgärder du kan utföra toomonitor en webbtjänst. Du kan uppdatera och ta bort den. Du kan också lägga till ytterligare slutpunkter tooa klassisk webbtjänsten i tillägg toohello standardslutpunkten som skapas när du distribuerar den.

Mer information finns i [hantera en Azure Machine Learning-arbetsytan](machine-learning-manage-workspace.md) och [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix hello link and uncomment
For more information on how toomanage Azure Machine Learning web service endpoints using hello REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-hello-web-service"></a>Uppdatera hello-webbtjänst
Du kan ändra tooyour webbtjänst, till exempel uppdatering hello modellen med ytterligare utbildningsdata, och distribuera den igen, skriver över hello ursprungliga webbtjänsten.

webbtjänst för tooupdate hello, öppna hello ursprungliga prediktivt experiment du använde toodeploy hello-webbtjänsten och göra en redigerbar kopia genom att klicka på **Spara som**. Gör ändringarna och klicka sedan på **distribuera webbtjänsten**.

Eftersom du har distribuerat experimentet innan tillfrågas du om du vill toooverwrite (klassiska webbtjänst) eller (ny webbtjänst) hello befintliga uppdateringstjänsten. Klicka på **Ja** eller **uppdatering** stoppar hello befintlig webbtjänst och distribuerar hello nya prediktivt experiment distribueras i dess ställe.

> [!NOTE]
> Om du har gjort konfigurationsändringar i hello ursprungliga webbtjänsten kan exempelvis behöver att ange ett nytt namn eller beskrivning, du tooenter dessa värden igen.
> 
> 

Ett alternativ för att uppdatera webbtjänsten är tooretrain hello modellen programmässigt. Mer information finns i [Träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).

<!-- internal links -->
[Skapa en träningsexperiment]: #create-a-training-experiment
[Konvertera den tooa prediktivt experiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Distribuera den som en webbtjänst]: #deploy-it-as-a-web-service
[nya]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klassiska]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
