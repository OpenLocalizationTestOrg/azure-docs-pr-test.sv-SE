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
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="084fe-103">Felsöka din modell i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="084fe-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="084fe-104">Den här artikeln förklarar hello varför antingen hello efter två fel kan uppstå när du kör en modell för orsaker:</span><span class="sxs-lookup"><span data-stu-id="084fe-104">This article explains hello potential reasons why either of hello following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="084fe-105">Hej [Träningsmodell] [ train-model] modulen genererar ett fel</span><span class="sxs-lookup"><span data-stu-id="084fe-105">hello [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="084fe-106">Hej [Poängmodell] [ score-model] modulen ger felaktiga resultat</span><span class="sxs-lookup"><span data-stu-id="084fe-106">hello [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="084fe-107">Modulen för Train-modell genererar ett fel</span><span class="sxs-lookup"><span data-stu-id="084fe-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="084fe-109">Hej [Träningsmodell] [ train-model] modulen förväntar två indata:</span><span class="sxs-lookup"><span data-stu-id="084fe-109">hello [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="084fe-110">hello typ av maskininlärningsmodell från hello samling av modeller som tillhandahålls av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="084fe-110">hello type of machine learning model from hello collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="084fe-111">hello utbildningsdata med en angiven etikett-kolumn som anger hello variabeln toopredict (hello andra kolumner antas toobe funktioner).</span><span class="sxs-lookup"><span data-stu-id="084fe-111">hello training data with a specified Label column which specifies hello variable toopredict (hello other columns are assumed toobe Features).</span></span>

<span data-ttu-id="084fe-112">Den här modulen kan producera ett fel i hello följande fall:</span><span class="sxs-lookup"><span data-stu-id="084fe-112">This module can produce an error in hello following cases:</span></span>

1. <span data-ttu-id="084fe-113">hello etikett kolumn har angetts felaktigt.</span><span class="sxs-lookup"><span data-stu-id="084fe-113">hello Label column is specified incorrectly.</span></span> <span data-ttu-id="084fe-114">Detta kan inträffa om mer än en kolumn har markerats som hello etikett eller en felaktig kolumnindex är markerad.</span><span class="sxs-lookup"><span data-stu-id="084fe-114">This can happen if either more than one column is selected as hello Label or an incorrect column index is selected.</span></span> <span data-ttu-id="084fe-115">Till exempel skulle hello andra fall tillämpas om ett index på 30 används med en inkommande datamängd som har bara 25 kolumner.</span><span class="sxs-lookup"><span data-stu-id="084fe-115">For example, hello second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="084fe-116">hello dataset innehåller inte några kolumner i funktionen.</span><span class="sxs-lookup"><span data-stu-id="084fe-116">hello dataset does not contain any Feature columns.</span></span> <span data-ttu-id="084fe-117">Till exempel om hello inkommande datamängden har endast en kolumn som har markerats som hello etikett kolumn vore det inga funktioner med vilken toobuild hello-modell.</span><span class="sxs-lookup"><span data-stu-id="084fe-117">For example, if hello input dataset has only one column, which is marked as hello Label column, there would be no features with which toobuild hello model.</span></span> <span data-ttu-id="084fe-118">I det här fallet hello [Träningsmodell] [ train-model] modulen genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="084fe-118">In this case, hello [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="084fe-119">hello inkommande datauppsättningen (funktioner eller etikett) innehåller oändligt som ett-värde.</span><span class="sxs-lookup"><span data-stu-id="084fe-119">hello input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="084fe-120">Modulen poängsätta modell ger felaktiga resultat</span><span class="sxs-lookup"><span data-stu-id="084fe-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="084fe-122">I en typisk utbildning/testning experimentet för övervakad inlärning hello [dela Data] [ split] modulen delas hello ursprungliga datauppsättningen i två delar: en del används tootrain hello modell och en del används tooscore hur väl hello tränade modellen utför.</span><span class="sxs-lookup"><span data-stu-id="084fe-122">In a typical training/testing experiment for supervised learning, hello [Split Data][split] module divides hello original dataset into two parts: one part is used tootrain hello model and one part is used tooscore how well hello trained model performs.</span></span> <span data-ttu-id="084fe-123">hello tränad modell är och sedan använda tooscore hello testdata, efter vilken hello resultatet är utvärderade toodetermine hello riktighet hello modellen.</span><span class="sxs-lookup"><span data-stu-id="084fe-123">hello trained model is then used tooscore hello test data, after which hello results are evaluated toodetermine hello accuracy of hello model.</span></span>

<span data-ttu-id="084fe-124">Hej [Poängmodell] [ score-model] modulen kräver två indata:</span><span class="sxs-lookup"><span data-stu-id="084fe-124">hello [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="084fe-125">En tränad modell utdata från hello [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="084fe-125">A trained model output from hello [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="084fe-126">En bedömningsprofil datamängd som skiljer sig från hello dataset används tootrain hello modellen.</span><span class="sxs-lookup"><span data-stu-id="084fe-126">A scoring dataset that is different from hello dataset used tootrain hello model.</span></span>

<span data-ttu-id="084fe-127">Det är möjligt att även om hello försöket lyckas hello [Poängmodell] [ score-model] modulen ger felaktiga resultat.</span><span class="sxs-lookup"><span data-stu-id="084fe-127">It's possible that even though hello experiment succeeds, hello [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="084fe-128">Flera scenarier kan orsaka det här toohappen:</span><span class="sxs-lookup"><span data-stu-id="084fe-128">Several scenarios may cause this toohappen:</span></span>

1. <span data-ttu-id="084fe-129">Om hello angivna etikett kategoriska och en regressionsmodell tränats på hello data, felaktig utdata skulle genereras av hello [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="084fe-129">If hello specified Label is categorical and a regression model is trained on hello data, an incorrect output would be produced by hello [Score Model][score-model] module.</span></span> <span data-ttu-id="084fe-130">Det beror på att regression kräver en kontinuerlig svar-variabel.</span><span class="sxs-lookup"><span data-stu-id="084fe-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="084fe-131">Det vore i det här fallet lämpligare toouse en modell för klassificering.</span><span class="sxs-lookup"><span data-stu-id="084fe-131">In this case, it would be more suitable toouse a classification model.</span></span> 

2. <span data-ttu-id="084fe-132">På samma sätt kan kan en klassificering modell tränas på en datamängd med flyttal i hello etikett kolumn, det ge oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="084fe-132">Similarly, if a classification model is trained on a dataset having floating point numbers in hello Label column, it may produce undesirable results.</span></span> <span data-ttu-id="084fe-133">Det beror på att klassificering kräver en diskret svar-variabel som endast tillåter att värden intervallet över en begränsad och vanligtvis något liten uppsättning klasser.</span><span class="sxs-lookup"><span data-stu-id="084fe-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="084fe-134">Om inte hello bedömningen dataset saknar modellen hello tootrain för funktioner som används för alla hello hello [Poängmodell] [ score-model] genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="084fe-134">If hello scoring dataset does not contain all hello features used tootrain hello model, hello [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="084fe-135">Om en rad i hello bedömningen datamängden innehåller ett saknat värde eller ett oändligt värde för någon av dess funktioner, hello [Poängmodell] [ score-model] kommer inte att generera utdata motsvarande toothat rader.</span><span class="sxs-lookup"><span data-stu-id="084fe-135">If a row in hello scoring dataset contains a missing value or an infinite value for any of its features, hello [Score Model][score-model] will not produce any output corresponding toothat row.</span></span>

5. <span data-ttu-id="084fe-136">Hej [Poängmodell] [ score-model] kan ge identiska utdata för alla rader i hello bedömningen dataset.</span><span class="sxs-lookup"><span data-stu-id="084fe-136">hello [Score Model][score-model] may produce identical outputs for all rows in hello scoring dataset.</span></span> <span data-ttu-id="084fe-137">Detta kan exempelvis inträffa när försök klassificering med beslut skogar om hello minsta antalet prov per lövnod väljs toobe mer än hello antalet utbildning exempel tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="084fe-137">This could occur, for example, when attempting classification using Decision Forests if hello minimum number of samples per leaf node is chosen toobe more than hello number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

