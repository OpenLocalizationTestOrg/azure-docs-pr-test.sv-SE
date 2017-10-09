---
title: "aaaHow tooprepare modellen för distribution i Azure Machine Learning Studio | Microsoft Docs"
description: "Hur tooprepare tränade modellen för distribution som en tjänst genom att konvertera experimentera tooa prediktivt experiment i Machine Learning Studio-utbildning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: garye
ms.openlocfilehash: d25bc68be63679a803bfc24a9e29e009a9263f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Hur tooprepare modellen för distribution i Azure Machine Learning Studio

Azure Machine Learning Studio ger du hello verktyg du behöver toodevelop en förutsägelseanalysmodell och sedan använda det genom att distribuera den som Azure-webbtjänst.

toodo kan du använda Studio toocreate ett experiment - kallas en *träningsexperiment* – där du träna poängsätta och redigera din modell. När du är nöjd kan du hämta din modell redo toodeploy genom att konvertera din utbildning experiment tooa *prediktivt experiment* som har konfigurerats tooscore användardata.

Du kan se ett exempel på hur [genomgång: utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

Den här artikeln tar en djupdykning i hello information om hur en träningsexperiment hämtar konverteras till en prediktivt experiment och hur det prediktivt experimentet distribueras. Förstå dessa uppgifter kan du lära dig hur tooconfigure distribuerade modell-toomake det mer effektivt.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Översikt 

hello att konvertera ett experiment tooa förutsägande träningsexperiment omfattar tre steg:

1. Ersätt hello maskininlärning algoritmen moduler med din tränad modell.
2. Trim hello experiment tooonly dessa moduler som krävs för resultatfunktioner. En träningsexperiment innehåller ett antal moduler som krävs för träning, men behövs inte när hello modell tränas.
3. Definiera hur din modell tar emot data från hello web Serviceanvändare och vilka data som ska returneras.

> [!TIP]
> Du har varit berörda med utbildning och bedömningen modellen med dina egna data i experimentet utbildning. Men distribution kan användare skickar nya tooyour datamodellen och förutsägelse resultat returneras. Så som du konverterar din utbildning experiment tooa prediktivt experiment tooget den klar för distribution, Tänk på hur hello modellen kommer att användas av andra.
> 
> 

## <a name="set-up-web-service-button"></a>Konfigurera Web Service-knappen
När du har kört experimentet (klickar du på **kör** längst hello hello experimentet), klicka på hello **konfigurera Web Service** knappen (Välj hello **förutsägande webbtjänsten** alternativet). **Konfigurera Web Service** utför för hello av tre steg för att konvertera din utbildning experiment tooa prediktivt experiment:

1. Sparar den tränade modellen i hello **tränade modeller** avsnitt i hello modulpaletten (toohello till vänster om hello experimentet). Ersätter den hello maskininlärningsalgoritmen och [Träningsmodell] [ train-model] moduler med hello sparade tränats modellen.
2. Analyseras experimentet och tar bort moduler som tydligt används endast för träning och inte längre behövs.
3. Infogas _Web service indata_ och _utdata_ moduler till standardplatser i experimentet (dessa moduler acceptera och returnera användardata).

Till exempel experimentera hello följande tåg en två-klass ökat beslut trädet modell med exempeldata inventering:

![Träningsexperiment][figure1]

hello moduler i experimentet utför i princip fyra olika funktioner:

![Modulfunktioner][figure2]

När du konverterar den här utbildning experiment tooa prediktivt experiment kan vissa av dessa moduler längre behövs inte eller de nu har olika syften:

* **Data** -hello data i det här exemplet dataset används inte vid bedömningen - hello användaren av webbtjänsten för hello tillhandahåller hello data toobe bedömas. Dock som hello metadata från den här datauppsättningen, till exempel datatyper, används av hello tränad modell. Du behöver tookeep hello datauppsättning i hello prediktivt experiment så att den kan ge dessa metadata.

* **Förbered** – beroende på hello användardata som ska skickas för poäng, dessa moduler kan eller inte nödvändiga tooprocess hello inkommande data. Hej **konfigurera Web Service** knappen inte touch dessa - toodecide måste du hur toohandle dem.
  
    Till exempel i det här exemplet hello exempeldatamängd kan ha värden som saknas, så en [Rensa Data som saknas] [ clean-missing-data] modulen kunde inkluderade toodeal med dem.. Dessutom innehåller hello exempeldatamängd kolumner som inte behövs tootrain hello modellen. Därför en [Välj kolumner i datauppsättning] [ select-columns] modulen kunde inkluderade tooexclude de extra kolumnerna från hello dataflöde. Om du vet att hello-data som ska skickas för resultatfunktioner via hello webbtjänsten inte värden som saknas och sedan kan du ta bort hello [Rensa Data som saknas] [ clean-missing-data] modul. Eftersom hello [Välj kolumner i datauppsättning] [ select-columns] modulen bidrar till att definiera hello kolumner med data den tränade modellen hello förväntar sig att modulen måste tooremain.

* **Train** -modulerna är används tootrain hello modellen. När du klickar på **konfigurera Web Service**, dessa moduler ersätts med en enda modul som innehåller hello modell du tränats. Den här nya modulen sparas i hello **tränade modeller** avsnitt i hello modulpaletten.

* **Poäng** – i det här exemplet hello [dela Data] [ split] modulen är används toodivide hello-dataströmmen till test- och utbildningsdata. I hello prediktivt experiment, vi inte tränar längre så [dela Data] [ split] kan tas bort. På samma sätt hello andra [Poängmodell] [ score-model] modulen och hello [utvärdera modell] [ evaluate-model] modulen används toocompare resultat från hello test data, så att dessa moduler inte behövs i hello förutsägande experiment. hello återstående [Poängmodell] [ score-model] modulen, men är nödvändiga tooreturn poäng innebär via hello-webbtjänst.

Här är hur vårt exempel ser ut när du klickar på **konfigurera Web Service**:

![Konvertera prediktivt experiment][figure3]

Hej arbete som utförs av **konfigurera Web Service** kan vara tillräckligt tooprepare dina experiment toobe distribueras som en webbtjänst. Men kanske du toodo vissa ytterligare specifika tooyour experiment.

### <a name="adjust-input-and-output-modules"></a>Justera inkommande och utgående moduler
Du använde en uppsättning träningsdata i experimentet utbildning och sedan har vissa bearbetning tooget hello data i ett formulär som hello maskininlärningsalgoritmen behövs. Om hello data som du förväntar dig tooreceive via hello-webbtjänst inte behöver denna bearbetning, du kan kringgå: ansluta hello utdata från hello **inkommande Web service-modulen** tooa annan modul i experimentet. hello användardata kommer nu i hello modellen på den här platsen.

Som standard **konfigurera Web Service** placeringar hello **Web service indata** modulen hello överst i dataflöde, som visas i hello bild ovan. Men vi manuellt kan placera hello **Web service indata** tidigare hello databearbetning moduler:

![Flytta hello web service indata][figure4]

hello indata för angivna hello webbtjänsten kommer nu passerar direkt i modulen poängsätta modell för hello utan någon bearbetning i förväg.

På samma sätt som standard **konfigurera Web Service** placeringar hello modulen för utdata längst hello-dataflöde. I det här exemplet returnerar hello webbtjänsten toohello användaren hello utdata från hello [Poängmodell] [ score-model] modulen som innehåller hello fullständig indata vector plus hello bedömningen resultat.
Om du föredrar tooreturn något annat, sedan du kan dock lägga till ytterligare moduler före hello **Web service utdata** modul. 

Till exempel tooreturn endast hello bedömningsprofil resultat och inte hello hela vector i indata, lägga till en [Välj kolumner i datauppsättning] [ select-columns] modulen tooexclude alla kolumner utom hello bedömningen resultat. Flytta hello **Web service utdata** modulen toohello utdata från hello [Välj kolumner i datauppsättning] [ select-columns] modul. hello experiment ser ut så här:

![Flytta hello web service-utdata][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Lägg till eller ta bort ytterligare rutiner för databehandling moduler
Om det finns flera moduler i experimentet som du vet inte behövs under bedömningen, kan dessa tas bort. Till exempel eftersom vi har flyttat hello **Web service indata** modulen tooa peka när hello databearbetningen moduler, vi kan ta bort hello [Rensa Data som saknas] [ clean-missing-data] modul från Hej prediktivt experiment.

Vår prediktivt experiment nu ser ut så här:

![Tar bort ytterligare modul][figure6]


### <a name="add-optional-web-service-parameters"></a>Lägga till valfria parametrar finns Web Service
I vissa fall kan kanske du vill tooallow hello användare av ditt webbprogram toochange hello tjänstbeteende av moduler när hello tjänsten används. *Webbtjänstparametrar* Tillåt toodo detta.

Ett vanligt exempel är att lägga in en [importera Data] [ import-data] modulen så hello användare hello distribuerade webbtjänsten kan ange en annan datakälla när hello webbtjänsten används. Eller konfigurera ett [exportera Data] [ export-data] modulen så att du kan ange ett annat mål.

Du kan definiera Webbtjänstparametrar och koppla dem till en eller flera parametrar, och du kan ange om de är obligatoriska eller valfria. hello användaren av webbtjänsten för hello visar värdena för dessa parametrar när komma åt hello-tjänsten och hello modulen åtgärder ändras.

För mer information om vilka Webbtjänstparametrar är och hur toouse dem, se [med Webbtjänstparametrar för Azure Machine Learning][webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-hello-predictive-experiment-as-a-web-service"></a>Distribuera hello prediktivt experiment som en webbtjänst
Nu när hello prediktivt experiment tillräckligt har förberetts, kan du distribuera den som en Azure-webbtjänst. Med hjälp av hello-webbtjänsten, användare kan skicka tooyour datamodellen och hello modellen returnerar dess förutsägelser.

Mer information om hello fullständig distributionsprocessen finns [distribuera en Azure Machine Learning-webbtjänst][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
