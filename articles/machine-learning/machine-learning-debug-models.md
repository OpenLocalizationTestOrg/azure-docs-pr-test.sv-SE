---
title: aaaDebug modellen i Azure Machine Learning | Microsoft Docs
description: "Hur toodebug fel genereras av Träningsmodell och Poängmodell moduler i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: ee38ca8ce38d4fc7add5ba70c80ab9bb2ceaf1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a>Felsöka din modell i Azure Machine Learning

Den här artikeln förklarar hello varför antingen hello efter två fel kan uppstå när du kör en modell för orsaker:

* Hej [Träningsmodell] [ train-model] modulen genererar ett fel 
* Hej [Poängmodell] [ score-model] modulen ger felaktiga resultat 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Modulen för Train-modell genererar ett fel

![image1](./media/machine-learning-debug-models/train_model-1.png)

Hej [Träningsmodell] [ train-model] modulen förväntar två indata:

1. hello typ av maskininlärningsmodell från hello samling av modeller som tillhandahålls av Azure Machine Learning.
2. hello utbildningsdata med en angiven etikett-kolumn som anger hello variabeln toopredict (hello andra kolumner antas toobe funktioner).

Den här modulen kan producera ett fel i hello följande fall:

1. hello etikett kolumn har angetts felaktigt. Detta kan inträffa om mer än en kolumn har markerats som hello etikett eller en felaktig kolumnindex är markerad. Till exempel skulle hello andra fall tillämpas om ett index på 30 används med en inkommande datamängd som har bara 25 kolumner.

2. hello dataset innehåller inte några kolumner i funktionen. Till exempel om hello inkommande datamängden har endast en kolumn som har markerats som hello etikett kolumn vore det inga funktioner med vilken toobuild hello-modell. I det här fallet hello [Träningsmodell] [ train-model] modulen genererar ett fel.

3. hello inkommande datauppsättningen (funktioner eller etikett) innehåller oändligt som ett-värde.

## <a name="score-model-module-produces-incorrect-results"></a>Modulen poängsätta modell ger felaktiga resultat

![image2](./media/machine-learning-debug-models/train_test-2.png)

I en typisk utbildning/testning experimentet för övervakad inlärning hello [dela Data] [ split] modulen delas hello ursprungliga datauppsättningen i två delar: en del används tootrain hello modell och en del används tooscore hur väl hello tränade modellen utför. hello tränad modell är och sedan använda tooscore hello testdata, efter vilken hello resultatet är utvärderade toodetermine hello riktighet hello modellen.

Hej [Poängmodell] [ score-model] modulen kräver två indata:

1. En tränad modell utdata från hello [Träningsmodell] [ train-model] modul.
2. En bedömningsprofil datamängd som skiljer sig från hello dataset används tootrain hello modellen.

Det är möjligt att även om hello försöket lyckas hello [Poängmodell] [ score-model] modulen ger felaktiga resultat. Flera scenarier kan orsaka det här toohappen:

1. Om hello angivna etikett kategoriska och en regressionsmodell tränats på hello data, felaktig utdata skulle genereras av hello [Poängmodell] [ score-model] modul. Det beror på att regression kräver en kontinuerlig svar-variabel. Det vore i det här fallet lämpligare toouse en modell för klassificering. 

2. På samma sätt kan kan en klassificering modell tränas på en datamängd med flyttal i hello etikett kolumn, det ge oönskade resultat. Det beror på att klassificering kräver en diskret svar-variabel som endast tillåter att värden intervallet över en begränsad och vanligtvis något liten uppsättning klasser.

3. Om inte hello bedömningen dataset saknar modellen hello tootrain för funktioner som används för alla hello hello [Poängmodell] [ score-model] genererar ett fel.

4. Om en rad i hello bedömningen datamängden innehåller ett saknat värde eller ett oändligt värde för någon av dess funktioner, hello [Poängmodell] [ score-model] kommer inte att generera utdata motsvarande toothat rader.

5. Hej [Poängmodell] [ score-model] kan ge identiska utdata för alla rader i hello bedömningen dataset. Detta kan exempelvis inträffa när försök klassificering med beslut skogar om hello minsta antalet prov per lövnod väljs toobe mer än hello antalet utbildning exempel tillgängliga.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

