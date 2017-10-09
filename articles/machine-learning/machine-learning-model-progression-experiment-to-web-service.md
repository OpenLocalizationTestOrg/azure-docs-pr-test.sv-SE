---
title: "aaaHow en Azure Machine Learning-modell blir en webbtjänst | Microsoft Docs"
description: "En översikt över hur din Azure Machine Learning modellen fortlöper från en utveckling experimentera tooan hello säkerhetsnivån operationalized webbtjänsten."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 25e0c025-f8b0-44ab-beaf-d0f2d485eb91
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 2bbe09fa8fa22662cf8ec4a8b6249d23c87c8ddf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-tooan-operationalized-web-service"></a>Hur en Machine Learning modellen fortlöper från ett experiment tooan operationalized Web service
Azure Machine Learning Studio tillhandahåller en interaktiv arbetsyta där du toodevelop, köra, testa och iterera ett ***experimentera*** som representerar en prediktiv analysmodell. Det finns en mängd olika moduler som kan:

* Indata i experimentet
* Ändra hello-data
* Träna en modell med hjälp av maskininlärningsalgoritmer
* Hej poängmodell
* Utvärdera hello resultat
* Sista utdatavärden

När du är nöjd med ditt experiment kan du distribuera den som en ***klassiska Azure Machine Learning-webbtjänst*** eller en ***nya Azure Machine Learning-webbtjänst*** så att användarna kan skicka det nya data och få tillbaka resultat.

Vi ger en översikt över hur din modell fortlöper i Machine Learning från en utveckling experiment tooan operationalized webbtjänsten hello säkerhetsnivån i den här artikeln.

> [!NOTE]
> Det finns andra sätt toodevelop och distribuera machine learning-modeller, men den här artikeln fokuserar på hur du använder Machine Learning Studio. Till exempel en beskrivning av hur toocreate en klassiska förutsägande webbtjänst med R, se hello blogginlägget tooread [Build & Distribuera förutsägande Web Apps med RStudio och Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Medan Azure Machine Learning Studio är utformad toohelp du utvecklar och distribuerar en *prediktiv analysmodell*, det är möjligt toouse Studio toodevelop ett experiment som inte innehåller en prediktiv analysmodell. Till exempel ett experiment kan bara inkommande data, bearbeta den och sedan utdata hello resultat. Du kan distribuera den här icke-prediktivt experiment som en webbtjänst precis som en förutsägbar analys experiment, men det är en enklare process eftersom hello experiment inte utbildning eller poängsättning av en modell för maskininlärning. När den inte är hello vanliga toouse Studio på det här sättet kommer vi inkludera den i hello diskussion så att vi kan ge en fullständig förklaring av hur Studio fungerar.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Utveckla och distribuera en prediktiv webbtjänst
Här följer hello faser som en typisk lösning följer du utvecklar och distribuerar den med hjälp av Machine Learning Studio:

![Distributionsflödet](media/machine-learning-model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Bild 1 - led i en typisk prediktiv analysmodell*

### <a name="hello-training-experiment"></a>Hej träningsexperiment
Hej ***träningsexperiment*** hello inledande fasen för att utveckla webbtjänsten i Machine Learning Studio. hello syftar hello träningsexperiment toogive du en plats toodevelop, testa, iterera och slutligen träna machine learning-modellen. Du kan även träna flera modeller samtidigt som du letar efter hello bästa lösningen, men när du är klar experimentera du väljer en enda tränats modell och eliminera hello resten från hello experiment. Ett exempel för att utveckla ett experiment förutsägbar analys finns [utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="hello-predictive-experiment"></a>Hej prediktivt experiment
När du har en tränad modell i experimentet utbildning klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänsten** i Machine Learning Studio tooinitiate hello att konvertera din utbildning Experimentera tooa ***prediktivt experiment***. Hej syftet hello prediktivt experiment är toouse tränade modellen tooscore nya data, med hello målet med slutligen blir operationalized som Azure-webbtjänst.

Den här konverteringen görs via hello följande steg:

* Konvertera hello uppsättning moduler som används för träning i en enda modul och spara den som en tränad modell
* Ta bort överflödig moduler som inte hör tooscoring
* Lägga till indata och utdata portar som hello eventuell Web-tjänsten ska använda

Det kan finnas fler ändringar som du vill toomake tooget din redo toodeploy prediktivt experiment som en webbtjänst. Till exempel om du vill hello Web service toooutput endast en delmängd av resultat, du kan lägga till en filtrering modul innan hello utdataporten.

I den här konverteringen hello träningsexperiment inte tas bort. När hello processen är klar har du två flikar i Studio: en för hello träningsexperiment och en för hello prediktivt experiment. Det här sättet kan du ändra toohello träningsexperiment innan du distribuerar webbtjänsten och återskapa hello prediktivt experiment. Eller spara en kopia av hello utbildning experiment toostart ytterligare en rad i experiment.

> [!NOTE]
> När du klickar på **förutsägande webbtjänsten** du startar en automatisk process tooconvert utbildning experiment tooa förutsägande experimentet och det fungerar bra i de flesta fall. Om experimentet utbildning är komplex (t.ex, du har flera sökvägar för träning som du ansluter tillsammans) kan du kanske föredrar toodo konverteringen manuellt. Mer information finns i [hur tooprepare modellen för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="hello-web-service"></a>hello-webbtjänst
När du är nöjd att experimentet förutsägande är klar, kan du distribuera din tjänst som antingen en klassiska webbtjänst eller en ny webbtjänst baserat på Azure Resource Manager. toooperationalize modellen genom att distribuera den som en *klassiska Machine Learning-webbtjänst*, klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]**. toodeploy som *nya Machine Learning-webbtjänst*, klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [ny]**. Användare kan nu skicka tooyour datamodellen med hjälp av webbtjänsten för hello REST-API och få tillbaka hello resultat. Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

## <a name="hello-non-typical-case-creating-a-non-predictive-web-service"></a>Hej icke vanligast: skapa en icke-förutsägande webbtjänst
Om experimentet inte tränar en prediktiv analysmodell och du behöver inte toocreate både en träningsexperiment och ett bedömningsprofil experiment – det finns bara ett experiment och du kan distribuera den som en webbtjänst. Machine Learning Studio identifierar om experimentet innehåller en förutsägelsemodell genom att analysera hello-moduler som du har använt.

När du har hävdade på experimentet och du är nöjd med den:

1. Klicka på **konfigurera Web Service** och välj **Omtränings webbtjänsten** - indata och utdata noder läggs till automatiskt
2. Klicka på **kör**
3. Klicka på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** beroende på hello miljö toowhich önskade toodeploy.

Webbtjänsten nu har distribuerats och du kan komma åt och hantera den precis som en förutsägbar webbtjänst.

## <a name="updating-your-web-service"></a>Uppdatera webbtjänsten
Nu när du har distribuerat experimentet som en webbtjänst, vad händer om du behöver tooupdate den?

Det beror på vad du behöver tooupdate:

**Du vill toochange hello indata eller utdata eller om du vill toomodify hur hello webbtjänsten manipulerar data**

Om du inte ändrar hello modell, men bara ändrar hur hello webbtjänsten hanterar data, du kan redigera hello prediktivt experiment och klicka sedan på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** igen. hello Web service har stoppats, hello uppdaterade prediktivt experiment distribueras och hello webbtjänsten har startats om.

Här är ett exempel: Anta att dina prediktivt experiment returnerar hello hela raden som indata med hello förutsade resultat. Du kan bestämma att du vill hello Web service toojust returnerade hello resultat. Så att du kan lägga till en **Projektkolumner** modul i hello prediktivt experiment precis före hello utgående port, tooexclude kolumner utom hello leda. När du klickar på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** igen, hello webbtjänsten har uppdaterats.

**Du vill tooretrain hello modellen med nya data**

Om du vill tookeep datorn Lär modell, men du vill att tooretrain den med nya data, har du två alternativ:

1. **Träna om hello modellen medan hello webbtjänsten körs** -om du vill tooretrain din modell medan hello förutsägande Web service körs, kan du göra detta genom att göra några ändringar toohello utbildning experiment toomake den en *** omtränings experiment***, och du kan distribuera den som en  ***omtränings web* tjänsten**. Anvisningar för hur toodo detta, se [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).
2. **Gå tillbaka toohello ursprungliga träningsexperiment och använda olika utbildning data toodevelop din modell** - experimentet förutsägande är länkade toohello webbtjänst, men hello träningsexperiment länkas inte direkt på detta sätt. Om du ändrar hello ursprungliga träningsexperiment och klicka på **konfigurera Web Service**, skapas en *nya* förutsägande experiment som, när de distribueras, skapar en *nya* Web tjänsten. Uppdaterar den inte bara hello ursprungliga webbtjänsten.
   
   Om du behöver toomodify hello träningsexperiment kan öppna det och klicka på **Spara som** toomake en kopia. Detta lämnar kvar hello ursprungliga träningsexperiment prediktivt experiment och webbtjänsten. Du kan nu skapa en ny webbtjänst med dina ändringar. När du har distribuerat hello ny webbtjänst sedan kan du bestämma om toostop hello tidigare webbtjänsten eller att det ska fungera tillsammans med hello ny.

**Du vill tootrain en annan modell**

Om du vill toomake ändras tooyour ursprungliga prediktivt experiment, till exempel att välja en annan maskininlärningsalgoritmen, måste försök med en annan utbildning metoden osv, toofollow hello andra proceduren ovan för omtränings din modell: Öppna hello träningsexperiment, klicka på **Spara som** toomake en kopia och sedan starta ned hello ny sökväg för att utveckla din modell, skapa hello prediktivt experiment och distribuera hello-webbtjänsten. Detta skapar en ny webbtjänst orelaterade toohello ursprungliga – kan du bestämma vilka tookeep för en eller båda, som körs.

## <a name="next-steps"></a>Nästa steg
Mer information om hello processen med att utveckla och experiment, finns i hello följande artiklar:

* Konvertera hello experiment - [hur tooprepare modellen för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md)
* distribuera webbtjänsten hello - [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md)
* omtränings hello modell - [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md)

För exempel på hello hela processen, se:

* [Självstudie om maskininlärning: skapa ditt första experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md)
* [Genomgång: Utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

