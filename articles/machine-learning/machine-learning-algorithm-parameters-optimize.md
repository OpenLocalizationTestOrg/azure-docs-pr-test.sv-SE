---
title: aaaOptimize algoritmerna i Azure Machine Learning | Microsoft Docs
description: "Förklarar hur toochoose hello optimala parameterinställning för en algoritm i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: fbf2f71abdbce19483fb048d67a39cbb368a928e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choose-parameters-toooptimize-your-algorithms-in-azure-machine-learning"></a>Välj parametrar toooptimize algoritmerna i Azure Machine Learning
Det här avsnittet beskrivs hur toochoose hello rätt hyperparameter anges för en algoritm i Azure Machine Learning. De flesta maskininlärningsalgoritmer har parametrar tooset. När du tränar en modell måste tooprovide värden för dessa parametrar. hello effekt hello tränade modellen beror på hello Modellparametrar som du väljer. hello processen hello optimala uppsättning parametrar kallas *modell markeringen*.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Det finns olika sätt toodo modellen val. Korsvalidering är en av hello används mest metoder för vald modell i machine learning och det är hello modellen markeringen standardmekanism i Azure Machine Learning. Eftersom Azure Machine Learning stöder både R och Python kan implementera du alltid egna mekanismer för val av modellen genom att använda R eller Python.

Det finns fyra steg i hello processen hello bästa parameteruppsättning:

1. **Definiera hello parametern utrymme**: hello algoritmen först bestämma exakt hello parametervärden som du vill tooconsider.
2. **Definiera hello korsvalidering inställningar**: Bestäm hur toochoose korsvalidering vikningar för hello dataset.
3. **Definiera hello mått**: Bestäm vilka mått toouse för att fastställa hello bästa uppsättning parametrar, till exempel noggrannhet, roten medelvärdet kvadrat fel, precision, återkalla eller f poäng.
4. **Träna, utvärdera och jämföra**: för varje unik kombination av hello parametervärden korsvalidering utförs av och baserat på hello fel mått som du definierar. Du kan välja hello bäst prestanda modellen efter utvärderingen och jämförelse.

hello följande bild illustrerar visar hur du kan göra detta i Azure Machine Learning.

![Hitta hello bästa parameteruppsättning](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-hello-parameter-space"></a>Definiera hello parametern utrymme
Du kan definiera hello parameteruppsättningen hello modellen initieringen steget. hello parametern rutan i alla maskininlärningsalgoritmer har två trainer lägen: *enskild Parameter* och *parametern intervallet*. Välj parametern intervallet läge. Du kan ange flera värden för varje parameter i intervallet för parametern-läge. Du kan ange kommaseparerade värden i hello textruta.

![Två-klass förstärkta beslutsträd, enskild parameter](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternativt kan du definiera hello högsta och lägsta punkter hello rutnätet och hello Totalt antal punkter toobe genereras med **Använd intervallet Builder**. Som standard genereras hello parametervärden på en linjär skala. Men om **logaritmisk skala** kontrolleras, hello värden genereras i hello logaritmisk skala (hello förhållandet mellan hello angränsande poäng är konstant i stället för deras skillnaden). Du kan definiera ett intervall för heltal parametrar, med bindestreck. Till exempel ”1-10” innebär att alla heltal mellan 1 och 10 (båda inkluderande) formuläret hello parameteruppsättning. Blandat läge stöds också. Till exempel hello parameteruppsättning ”1-10, 20, 50” innefattar heltal 1-10, 20, och 50.

![Två-klass förstärkta beslutsträd, intervallet för parametern](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Definiera korsvalidering vikningar
Hej [partitionera och ta prover] [ partition-and-sample] modul kan vara används toorandomly tilldela vikningar toohello data. I följande exempelkonfiguration för hello modulen hello, vi definiera fem gånger och tilldela slumpmässigt en vikning nummer toohello exempel instanser.

![Partitionera och ta prover](./media/machine-learning-algorithm-parameters-optimize/fig4.png)

## <a name="define-hello-metric"></a>Definiera hello mått
Hej [Justeringsmodeller] [ tune-model-hyperparameters] modulen ger stöd för empiriskt välja hello bästa uppsättning parametrar för en given algoritmen och dataset. Dessutom tooother information om utbildning hello modellen hello **egenskaper** rutan i den här modulen innehåller hello mått för att fastställa hello bästa parameteruppsättning. Den har två olika nedrullningsbara listrutor för klassificering och regression algoritmer respektive. Om hello algoritmen berörda är en klassificeringsalgoritm, hello regression mått ignoreras och vice versa. I det här exemplet hello mått är **noggrannhet**.   

![Svep parametrar](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Träna, utvärdera och jämföra
Hej samma [Justeringsmodeller] [ tune-model-hyperparameters] modulen tränar alla hello modeller som motsvarar toohello parameteruppsättning, utvärderar olika mått och skapar sedan hello bäst tränade modellen baserat på hello måttet som du väljer. Modulen har två obligatoriska indata:

* hello omdöme deltagaren
* hello dataset

hello-modulen har även en valfri datauppsättning som indata. Ansluta hello dataset med vikning information toohello obligatoriska dataset indata. Om hello dataset inte har tilldelats någon vikning information, utförs automatiskt en 10-fold korsvalidering som standard. Om sker inte hello vikning tilldelningen och en verifiering dataset tillhandahålls vid hello valfria dataset port, ett valt train-test och hello första datauppsättningen är används tootrain hello modellen för varje parameter-kombination.

![Förstärkta trädet klassificerare](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

hello modellen utvärderas sedan för hello validering datauppsättningen. hello lämnas utdataporten för hello-modulen visar olika mått som funktioner i parametervärden. hello rätt utdataporten ger hello tränade modellen som motsvarar toohello bäst prestanda modellen enligt toohello valt mått (**noggrannhet** i detta fall).  

![Verifieringen dataset](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Du kan se hello exakt parametrar som visualisera hello rätt utdataporten valt. Den här modellen kan användas i poängsättning av en uppsättning test eller en operationalized webbtjänst när du har sparat som en tränad modell.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
