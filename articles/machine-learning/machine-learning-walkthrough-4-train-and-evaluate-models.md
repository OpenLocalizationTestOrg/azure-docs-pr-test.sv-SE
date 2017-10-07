---
title: "Steg 4: Träna och utvärdera hello analytiska förutsägelsemodeller | Microsoft Docs"
description: "Steg 4 i hello utveckla en förutsägelselösning genomgång: tåg och poängsätta och utvärdera flera modeller i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a>Genomgång steg 4: Träna och utvärdera hello analytiska förutsägelsemodeller
Det här avsnittet innehåller hello fjärde steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. **Träna och utvärdera hello modeller**
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. [Få åtkomst till hello-webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)

- - -
En av hello fördelarna med att använda Azure Machine Learning Studio för att skapa machine learning-modeller är hello möjlighet tootry mer än en typ av modellen samtidigt i en enda experiment och jämföra hello resultat. Den här typen av experiment hjälper dig att hitta hello bästa lösningen för ditt problem.

I hello experiment Vi utvecklar i den här genomgången, vi skapa två olika typer av modeller och jämför deras bedömningsprofil resultat toodecide vilken algoritm vi vill toouse i vår slutliga experimentet.  

Det finns olika modeller som vi kan välja från. toosee hello modeller är tillgängliga, expandera hello **Maskininlärning** nod i hello modulpaletten och expandera sedan **initiera modell** och hello noder under den. Hello enligt experimentet vi välja hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) och hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] moduler.    

> [!TIP]
> tooget hjälp för att bestämma vilka Machine Learning-algoritmen bäst passar hello viss problemet du försöker toosolve, se [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
> 
> 

## <a name="train-hello-models"></a>Träna hello modeller

Vi lägger till båda hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen och [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modul i detta experimentet.

### <a name="two-class-boosted-decision-tree"></a>Two-Class Tvåklassförhöjda beslutsträdet

Först ska vi ställa in hello ökat beslut trädet modellen.

1. Hitta hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modul i hello modulpaletten och drar den till hello arbetsytan.

2. Hitta hello [Träningsmodell] [ train-model] modulen, drar den till arbetsytan hello och ansluter sedan hello utdata från hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree]modulen toohello kvar indataport av hello [Träningsmodell] [ train-model] modul.
   
   Hej [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen initierar hello allmän modell, och [Träningsmodell] [ train-model] använder träningsdata tootrain hello modell. 

3. Ansluta hello vänstra utdata från hello vänster [köra R-skriptet] [ execute-r-script] modulen toohello höger inkommande port för hello [Träningsmodell] [ train-model] modul (vi valt i [steg3](machine-learning-walkthrough-3-create-new-experiment.md) för den här genomgången toouse hello data från vänster sida av hello dela Data-modulen för utbildning hello).
   
   > [!TIP]
   > Vi behöver inte två hello indata och en av hello utdata för hello [köra R-skriptet] [ execute-r-script] modul för experimentet, så vi kan lämna dem. 
   > 
   > 

Den här delen av hello experiment nu ser ut ungefär så här:  

![En modell][1]

Nu måste vi tootell hello [Träningsmodell] [ train-model] modul som vi vill hello modellen toopredict hello kreditrisk värde.

1. Välj hello [Träningsmodell] [ train-model] modul. I hello **egenskaper** rutan klickar du på **starta kolumnväljaren**.

2. I hello **Markera en kolumn** dialogrutan Skriv ”kredit risk” i hello sökfältet under **tillgängliga kolumner**markerar ”kredit risk” nedan och klicka på högerpilen för hello ( **>** ) toomove ”kredit risk” för**valda kolumner**. 

    ![Markera hello kreditrisk kolumnen för hello träningsmodellmodulen][0]

3. Klicka på hello **OK** är markerat.

### <a name="two-class-support-vector-machine"></a>Tvåklassig dator för vektorstöd

Nu ska ställa vi in hello SVM modellen.  

Första, en förklaring om SVM. Förstärkta beslutsträd fungerar bra med funktioner för alla typer. Men eftersom hello SVM modulen genererar en linjär klassificerare, hello som genereras har hello bästa testfel när alla numeriska funktioner har hello samma skala. tooconvert alla numeriska funktioner toohello samma skala måste vi använder en ”Tanh”-omvandling (med hello [normalisera Data] [ normalize-data] module). Detta omvandlar våra siffror till intervallet för hello [0,1]. hello SVM modulen konverterar sträng funktioner toocategorical funktioner och sedan toobinary 0-1-funktioner, så vi inte behöver toomanually Omforma strängen funktioner. Dessutom bör inte tootransform hello kreditrisk kolumn (21) – är det numeriska, men det är hello värdet vi tränar hello modellen toopredict tooleave måste den enbart.  

tooset in hello SVM modell hello följande:

1. Hitta hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modul i hello modulpaletten och drar den till hello arbetsytan.

2. Högerklicka på hello [Träningsmodell] [ train-model] modulen, Välj **kopiera**, och högerklicka hello arbetsytan och välj **klistra in**. Hej kopia av hello [Träningsmodell] [ train-model] modulen har hello samma Kolumnurval av hello ursprungliga.

3. Ansluta hello utdata från hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen toohello kvar indataport av hello andra [Träningsmodell] [ train-model] modul.

4. Hitta hello [normalisera Data] [ normalize-data] modulen och drar den till hello arbetsytan.

5. Ansluta hello vänstra utdata från hello vänster [köra R-skriptet] [ execute-r-script] toohello modulindata till den här modulen (Observera att hello utdataporten för en modul kan vara anslutna toomore än en modul).

6. Ansluta hello kvar utdataporten för hello [normalisera Data] [ normalize-data] modulen toohello höger inkommande port för hello andra [Träningsmodell] [ train-model] modul.

Den här delen av vår experimentet bör nu se ut ungefär så här:  

![Utbildning hello andra modell][2]  

Nu konfigurera hello [normalisera Data] [ normalize-data] modulen:

1. Klicka på tooselect hello [normalisera Data] [ normalize-data] modul. I hello **egenskaper** väljer **Tanh** för hello **omvandling metoden** parameter.

2. Klicka på **starta kolumnväljaren**, Välj ”inga kolumner” för **börjar med**väljer **inkludera** i hello första listrutan väljer **kolumntypen**i hello andra listrutan och välj **numeriska** i hello tredje listrutan. Anger att alla hello numeriska kolumner (och endast numeriskt) omvandlas.

3. Klicka på hello plustecken (+) toohello höger i den här raden - detta skapar en rad av nedrullningsbara listorna. Välj **undanta** i hello första listrutan väljer **kolumnnamn** i hello andra listrutan och ange ”kredit risk” i hello textfältet. Detta anger hello kreditrisk kolumnen ska ignoreras (vi måste toodo detta eftersom den här kolumnen är numeriska och så skulle omvandlas om vi inte utesluter den).

4. Klicka på hello **OK** är markerat.  

    ![Välj kolumner för hello normalisera Data modulen][5]

Hej [normalisera Data] [ normalize-data] modulen är nu set tooperform en Tanh omvandling på alla numeriska kolumner utom hello kreditrisk.  

## <a name="score-and-evaluate-hello-models"></a>Poängsätta och utvärdera hello modeller

Vi använder hello testa data som separeras av hello [dela Data] [ split] modulen tooscore våra tränade modeller. Vi kan sedan jämföra hello resultaten av hello två modeller toosee som genererade bättre resultat.  

### <a name="add-hello-score-model-modules"></a>Lägga till hello poängsätta modell moduler

1. Hitta hello [Poängmodell] [ score-model] modulen och drar den till hello arbetsytan.

2. Ansluta hello [Träningsmodell] [ train-model] modul som har anslutit toohello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen toohello vänstra indata porten för hello [Poängmodell] [ score-model] modul.

3. Ansluta hello höger [köra R-skriptet] [ execute-r-script] modul (våra tester data) toohello höger inkommande port för hello [Poängmodell] [ score-model] modul.

    ![Ansluten att modulen poängsätta modell][6]
   
   Hej [Poängmodell] [ score-model] modul kan nu ta hello kredit information från hello testning informationen genom att köra via hello modellen, och jämföra hello förutsägelser hello modellen genererar med hello faktiska kredit risk kolumnen i hello testa data.

4. Kopiera och klistra in hello [Poängmodell] [ score-model] modulen toocreate en andra kopia.

5. Ansluta hello utdata från hello SVM modellen (det vill säga hello utgående port för hello [Träningsmodell] [ train-model] modul som har anslutit toohello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen) toohello inkommande port för hello andra [Poängmodell] [ score-model] modul.

6. För hello SVM modellen har vi toodo hello samma omvandling toohello testdata som vi gjorde toohello utbildningsdata. Kopiera och klistra in hello så [normalisera Data] [ normalize-data] modulen toocreate en andra kopia och koppla den högra toohello [köra R-skriptet] [ execute-r-script] modul.

7. Ansluta hello vänstra utdata från hello andra [normalisera Data] [ normalize-data] modulen toohello höger inkommande port för hello andra [Poängmodell] [ score-model] modul.

    ![Båda poängsätta modell-moduler som är ansluten][7]

### <a name="add-hello-evaluate-model-module"></a>Lägg till hello utvärdera modell

tooevaluate hello två bedömningsprofil resultat och jämför dem, vi använder en [utvärdera modell] [ evaluate-model] modul.  

1. Hitta hello [utvärdera modell] [ evaluate-model] modulen och drar den till hello arbetsytan.

2. Ansluta hello utdataporten för hello [Poängmodell] [ score-model] modulen som hör till hello ökat beslut trädet modellen vänster toohello inkommande port för hello [utvärdera modell] [ evaluate-model] modul.

3. Ansluta hello andra [Poängmodell] [ score-model] modulen toohello höger inkommande port.  

    ![Utvärdera modellen modulen ansluten][8]

### <a name="run-hello-experiment-and-check-hello-results"></a>Kör experimentet hello och kontrollera hello resultat

toorun Hej experiment, klicka på hello **kör** nedan hello arbetsytan. Det kan ta några minuter. En snurrande indikator för varje modul visar att det körs och sedan en grön bock visas när hello modulen är klar. När alla hello-moduler har markerat, har hello experimentet slutförts.

hello experimentet bör nu se ut ungefär så här:  

![Utvärdering av båda modellerna][3]

toocheck hello resultaten klickar du på hello utdataporten för hello [utvärdera modell] [ evaluate-model] modulen och välj **visualisera**.  

Hej [utvärdera modell] [ evaluate-model] modulen genererar ett par med kurvor och mått som gör att du toocompare hello resultaten av hello två poängsatta modeller. Du kan visa hello resultat som mottagaren operatorn egenskap (ROC) kurvor, Precision/återkalla kurvor eller Lift kurvor. Ytterligare data som visas innehåller en matris med förvirring kumulativa värden för hello område under hello kurvan (AUC) och andra mått. Du kan ändra hello tröskelvärdet genom att flytta skjutreglaget hello åt vänster eller höger och se hur den påverkar hello uppsättning mått.  

toohello direkt av hello diagram, klickar du på **bedömas dataset** eller **bedömas dataset toocompare** toohighlight hello associerade kurva och toodisplay hello associerade mått nedan. Hello förklaring för hello kurvor, ”bedömas dataset” motsvarar toohello kvar indataport av hello [utvärdera modell] [ evaluate-model] modul - i vårt fall är hello ökat beslut trädet modellen. ”Bedömas dataset toocompare” motsvarar toohello högra indataporten - hello SVM modellen i vårt fall. När du klickar på en av etiketterna hello kurva för den modellen som är markerad och hello motsvarande mått visas som visas i följande bild hello.  

![ROC kurvor för modeller][4]

Du kan välja vilken modell är närmaste toogiving du hello resultat som du letar efter genom att undersöka dessa värden. Du kan gå tillbaka och iterera experimentet genom att ändra parametervärden i hello olika modeller. 

hello vetenskap och bilder av tolka resultaten och justera hello modellen prestanda är utanför hello omfånget för den här genomgången. För ytterligare hjälp kan du läsa hello följande artiklar:
- [Hur tooevaluate modell prestanda i Azure Machine Learning](machine-learning-evaluate-model-performance.md)
- [Välj parametrar toooptimize algoritmerna i Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md)
- [Tolka modellen resultaten i Azure Machine Learning](machine-learning-interpret-model-results.md)

> [!TIP]
> Varje gång du kör hello experiment en post på den iterationen sparas i hello Körningshistorik. Du kan visa dessa iterationer och returnera tooany av dem genom att klicka på **visa KÖRNINGSHISTORIK** nedan hello arbetsytan. Du kan också klicka på **tidigare kör** i hello **egenskaper** fönstret tooreturn toohello iteration omedelbart före hello en öppen.
> 
> Du kan göra en kopia av eventuella iterationer av experimentet genom att klicka på **Spara som** nedan hello arbetsytan. 
> Använd hello experiment **sammanfattning** och **beskrivning** egenskaper tookeep en post på vad du har gjort i iterationer av experiment.
> 
> Mer information finns i [hantera iterationer av experiment i Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).  
> 
> 

- - -
**Nästa: [distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)**

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
