---
title: "Steg 5: Distribuera hello webbtjänsten för Machine Learning | Microsoft Docs"
description: "Steg 5 i hello utveckla en förutsägelselösning genomgång: distribuera en prediktivt experiment i Machine Learning Studio som en webbtjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 76391010972ed1450bbda8bfb2352c7b22b51ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a>Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst
Det här är hello femte steg i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. **Distribuera hello-webbtjänst**
6. [Åtkomst hello-webbtjänst](machine-learning-walkthrough-6-access-web-service.md)

- - -
toogive andra en chansen toouse hello förutsägelsemodell som vi har utvecklat i den här genomgången, vi kan distribuera den som en webbtjänst på Azure.

Vi har experimentera med vår modell för in toothis punkt. Men hello distribuerade tjänsten kommer inte längre toodo utbildning - vad vi toogenerate nya förutsägelser av bedömningen hello användarens indata baserat på vår modell. Så vi toodo vissa förberedelser tooconvert detta experiment från en ***utbildning*** experimentera tooa ***förutsägande*** experiment. 

Detta är en process i tre steg:  

1. Ta bort en hello modeller
2. Konvertera hello *träningsexperiment* har vi skapat till en *prediktivt experiment*
3. Distribuera hello prediktivt experiment som en webbtjänst

## <a name="remove-one-of-hello-models"></a>Ta bort en hello modeller

Först måste vi tootrim experimentet ned lite. Vi har två olika modeller i hello experiment, men vi vill bara toouse en modell när vi distribuera den som en webbtjänst.  

Anta att vi har valt att hello ökat trädet modellen presterade bättre än hello SVM modellen. Så ta bort hello hello första sak toodo [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen och hello-moduler som användes för att träna den. Du kanske vill toomake en kopia av hello experiment först genom att klicka på **Spara som** längst hello hello experimentera arbetsytan.

Vi behöver toodelete hello följande moduler:  

* [Two-Class Support Vector Machine][two-class-support-vector-machine]
* [Träna modellen] [ train-model] och [Poängmodell] [ score-model] moduler som var anslutna tooit
* [Normalisera Data] [ normalize-data] (båda)
* [Utvärdera modellen] [ evaluate-model] (eftersom vi är klar utvärderar hello modeller)

Markera varje modul och tryck på hello del. eller högerklicka på hello modulen och välj **ta bort**. 

![Ta bort hello SVM modellen][3a]

Vår modell bör nu se ut ungefär så här:

![Ta bort hello SVM modellen][3]

Nu är klar toodeploy detta modellen med hello [två-Tvåklassförhöjt beslutsträd][two-class-boosted-decision-tree].

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Konvertera hello utbildning experiment tooa prediktivt experiment

tooget detta modellen är redo för distribution måste vi tooconvert detta utbildning experiment tooa prediktivt experiment. Detta omfattar tre steg:

1. Spara hello modell vi har tränat och Ersätt vår utbildningsmoduler
2. Trim hello experiment tooremove moduler som behövdes endast för utbildning
3. Definiera där hello-webbtjänsten ska ta emot indata och där det genererar hello-utdata

Vi kan göra detta manuellt, men som tur är alla tre stegen kan utföras genom att klicka på **konfigurera Web Service** längst hello hello experimentet (och välja hello **förutsägande webbtjänsten** alternativet).

> [!TIP]
> Om du vill ha mer information om vad som händer när du konverterar en utbildning experiment tooa förutsägande experiment, se [hur tooprepare modellen för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).

När du klickar på **konfigurera Web Service**, flera saker:

* hello tränad modell är konverterade tooa enda **Trained Model** modulen och lagras i hello modulen paletten toohello till vänster i hello experimentera arbetsytan (du hittar den under **tränade modeller**)
* Moduler som användes för träning tas bort; specifikt:
  * [Two-Class Tvåklassförhöjda beslutsträdet][two-class-boosted-decision-tree]
  * [Träningsmodell][train-model]
  * [Dela Data][split]
  * Hej andra [köra R-skriptet] [ execute-r-script] modul som användes för testdata
* hello sparade tränade modellen har lagts till hello experiment
* **Web service indata** och **Web service utdata** moduler läggs (dessa identifiera där hello användardata kommer att ange hello modellen och vilka data som returneras när hello webbtjänsten används)

> [!NOTE]
> Du kan se att hello experiment sparas i två delar under flikar som lagts till hello överst i hello experimentet. hello ursprungliga träningsexperiment är under hello fliken **träningsexperiment**, och hello nyskapad prediktivt experiment är **Prediktivt experiment**. Hej prediktivt experiment är hello något kommer vi att distribuera som en webbtjänst.

Vi behöver tootake ytterligare ett steg med viss experimentet.
Vi har lagt till två [köra R-skriptet] [ execute-r-script] moduler tooprovide en viktas fungera toohello data. Som var bara en lura vi behövs för träning och testning, så att vi kan ta reda på dessa moduler i hello sista modellen.
Machine Learning Studio bort en [köra R-skriptet] [ execute-r-script] modulen när den tas bort hello [dela] [ split] modul. Nu kan vi bort hello andra och ansluta [Metadata Editor] [ metadata-editor] direkt för[Poängmodell][score-model].    

Vår experimentet bör nu se ut så här:  

![Bedömningsprofil hello tränade modellen][4]  

> [!NOTE]
> Du kanske undrar varför vi kvar hello UCI tyska kreditkortdata dataset i hello prediktivt experiment. hello tjänsten kommer tooscore hello användarens data, inte hello ursprungliga datauppsättningen, så varför lämnar hello ursprungliga datauppsättningen i hello modellen?
> 
> Det gäller att hello-tjänsten inte behöver hello ursprungliga kreditkortdata. Men den behöver hello schemat för dessa data, som innehåller information, till exempel hur många kolumner finns och vilka kolumner som är numeriska. Den här schemainformationen är nödvändiga toointerpret hello användarens data. Vi lämnar komponenterna ansluten så att hello poängsättningsmodul har hello dataset schema när hello-tjänsten körs. hello data inte används bara hello schemat.  
> 
> 

Kör experimentet hello en sista gång (klicka på **kör**.) Om du vill tooverify som hello modellen fortfarande fungerar, klickar du på hello utdata från hello [Poängmodell] [ score-model] modulen och välj **visa resultat**. Du kan se att hello ursprungliga data visas, tillsammans med hello kredit risk värdet (”poängsatta etiketter”) och hello bedömningen sannolikhetsvärdet (”bedömas sannolikhet”.) 

## <a name="deploy-hello-web-service"></a>Distribuera hello-webbtjänst
Du kan distribuera hello experiment som antingen en klassisk webbtjänst eller som en ny webbtjänst som baseras på Azure Resource Manager.

### <a name="deploy-as-a-classic-web-service"></a>Distribuera som ett klassiskt-webbtjänst
toodeploy som ett klassiskt webbtjänsten som härletts från våra experiment, klickar du på **distribuera webbtjänsten** nedan hello arbetsytan och välj **distribuera webbtjänsten [klassisk]**. Machine Learning Studio distribuerar hello experiment som en webbtjänst och tar toohello instrumentpanel för webbtjänsten. Från den här sidan kan du returnera toohello experiment (**visa ögonblicksbild** eller **Visa senaste**) och köra en enkel hello-webbtjänsten (se **testa hello webbtjänsten** nedan). Det finns även information här för att skapa program som kan komma åt hello-webbtjänst (Mer information om det finns i hello nästa steg i den här genomgången).

![Web service-instrumentpanelen][6]

Du kan konfigurera hello tjänsten genom att klicka på hello **CONFIGURATION** fliken. Här kan du ändra hello tjänstnamn (det är angivet hello experiment namn som standard) och en beskrivning. Du kan också ge mer egna etiketter för hello indata och utdata.  

![Konfigurera hello-webbtjänsten][5]  

### <a name="deploy-as-a-new-web-service"></a>Distribuera som en ny webbtjänst

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

toodeploy en ny webbtjänst som härrör från våra experiment:

1. Klicka på **distribuera webbtjänsten** nedan hello arbetsytan och välj **distribuera webbtjänsten [ny]**. Machine Learning Studio överför toohello Azure Machine Learning-webbtjänster **distribuera Experiment** sidan.

2. Ange ett namn för hello-webbtjänsten. 

3. För **prisplan**, kan du välja en befintlig prisavtal eller välj ”Skapa nya” och ge hello nya planera ett namn och välj hello månatliga plan alternativet. hello plan nivåerna standard toohello planer för standardregion och webbtjänsten är distribuerade toothat region.

4. Klicka på **distribuera**.

Efter några minuter hello **Quickstart** öppnas sidan för webbtjänsten.

Du kan konfigurera hello tjänsten genom att klicka på hello **konfigurera** fliken. Här kan du ändra hello service title och en beskrivning. 

tootest hello webbtjänsten klickar du på hello **Test** fliken (se **testa hello webbtjänsten** nedan). Information om hur du skapar program som kan komma åt hello webbtjänsten klickar du på hello **förbruka** fliken (hello nästa steg i den här genomgången hamnar i detalj).

> [!TIP]
> När du har distribuerat den, kan du uppdatera hello-webbtjänsten. Till exempel om du vill toochange din modell kan sedan du kan redigera hello träningsexperiment, justera hello modellparametrarna och på **distribuera webbtjänsten**, välja **distribuera webbtjänsten [klassisk]** eller  **Distribuera webbtjänsten [ny]**. När du distribuerar hello experiment, ersätter hello webbtjänsten nu och Använd uppdaterade modellen.  
> 
> 

## <a name="test-hello-web-service"></a>Testa hello-webbtjänst

När hello webbtjänsten används hello användardata anger via hello **Web service indata** modulen där det har gått toohello [Poängmodell] [ score-model] modulen och poängsätts. hello sätt som vi har konfigurerat hello prediktivt experiment, hello modellen förväntar data i hello samma format som hello ursprungliga kredit risk dataset.
hello resultat returneras toohello användaren från hello webbtjänst via hello **Web service utdata** modul.

> [!TIP]
> hello sätt som vi har hello prediktivt experiment konfigurerad, hello hela fås hello [Poängmodell] [ score-model] modulen returneras. Detta omfattar alla hello indata plus hello kredit risk värdet och hello bedömningen sannolikheten. Men returnerar något annat om du vill, t.ex, du kan returnera bara hello kredit risk värde. toodo detta, infoga en [Projektkolumner] [ project-columns] modulen mellan [Poängmodell] [ score-model] och hello **Web service utdata** tooeliminate kolumner som du inte vill hello web service tooreturn. 
> 
> 

Du kan testa en klassisk web service antingen i **Machine Learning Studio** eller i hello **Azure Machine Learning-webbtjänster** portal.
Du kan testa en ny webbtjänst endast i hello **Machine Learning-webbtjänster** portal.

> [!TIP]
> Testa i hello Azure Machine Learning-webbtjänster portal, kan du ha hello portal Skapa exempeldata som du kan använda tootest hello svar på begäranden tjänsten. På hello **konfigurera** väljer ”Ja” för **exempel Data aktiverat?**. När du öppnar fliken hello begäran och svar på hello **Test** sidan hello portal fylls exempeldata från hello ursprungliga kredit risk dataset.

### <a name="test-a-classic-web-service"></a>Testa en klassisk-webbtjänst

Du kan testa en klassisk webbtjänst i Machine Learning Studio eller hello Machine Learning-webbtjänster portal. 

#### <a name="test-in-machine-learning-studio"></a>Testa i Machine Learning Studio

1. På hello **INSTRUMENTPANELEN** för hello-webbtjänsten, klickar du på hello **Test** knappen **standard Endpoint**. En dialogruta visas och du uppmanas att ange hello indata för hello-tjänsten. Dessa är hello samma kolumner som fanns i hello ursprungliga kredit risk dataset.  

2. Ange en uppsättning data och klicka sedan på **OK**. 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a>Testa i hello Machine Learning-webbtjänster portal

1. På hello **INSTRUMENTPANELEN** för hello-webbtjänsten, klickar du på hello **Test preview** länken under **standard Endpoint**. hello testsida hello Azure Machine Learning-webbtjänster för i portalen hello webbtjänstslutpunkt öppnas och du uppmanas att ange hello indata för hello-tjänsten. Dessa är hello samma kolumner som fanns i hello ursprungliga kredit risk dataset.

2. Klicka på **testa begäran och svar**. 

### <a name="test-a-new-web-service"></a>Testa en ny webbtjänst

Du kan testa en ny webbtjänst endast i hello Machine Learning-webbtjänster portal.

1. I hello [Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) klickar du på **Test** hello överst på hello sidan. Hej **Test** sidan öppnas och du kan ange data för hello-tjänsten. hello inmatningsfält visas överensstämmer toohello kolumner som fanns i hello ursprungliga kredit risk dataset. 

2. Ange en uppsättning data och klicka sedan på **Test-svar på begäranden**.

hello visas av hello test resultaten på hello höger hello-sidan i hello utdatakolumnen. 


## <a name="manage-hello-web-service"></a>Hantera hello-webbtjänst

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a>Hantera en klassisk webbtjänst i hello klassiska Azure-portalen

När du har distribuerat klassisk webbtjänsten kan du hantera den från hello [klassiska Azure-portalen](https://manage.windowsazure.com).

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com)
2. Klicka på hello Microsoft Azure-tjänster Kontrollpanelen **MACHINE LEARNING**
3. Klicka på arbetsytan
4. Klicka på hello **webbtjänster** fliken
5. Klicka på hello-webbtjänst som vi skapade
6. Klicka på hello ”default” slutpunkt

Här kan du till exempel övervaka hur hello webbtjänst fungerar och göra justeringar prestanda genom att ändra hur många samtidiga anrop hello-tjänsten kan hantera.

Mer information finns i:

* [Skapa slutpunkter](machine-learning-create-endpoint.md)
* [Skalning webbtjänst](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a>Hantera en klassisk eller ny webbtjänst i hello Azure Machine Learning-webbtjänster portal

När du har distribuerat din webbtjänst om klassiska eller nya, du kan hantera den från hello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal.

toomonitor hello prestanda för webbtjänsten:

1. Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal
2. Klicka på **webbtjänster**
3. Klicka på din web service
4. Klicka på hello **instrumentpanelen**

- - -
**Nästa: [komma åt hello-webbtjänst](machine-learning-walkthrough-6-access-web-service.md)**

[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
