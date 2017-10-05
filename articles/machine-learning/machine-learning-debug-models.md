---
title: "Felsöka din modell i Azure Machine Learning | Microsoft Docs"
description: "Så här felsöker du fel som genereras av modulerna Träningsmodell och Poängmodell i Azure Machine Learning."
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
ms.openlocfilehash: d4cc94a6395ea45bccf65d9a9f3118ec98cb258d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="b6a35-103">Felsöka din modell i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b6a35-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="b6a35-104">Den här artikeln beskrivs möjliga orsaker till något av följande två fel kan uppstå när du kör en modell:</span><span class="sxs-lookup"><span data-stu-id="b6a35-104">This article explains the potential reasons why either of the following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="b6a35-105">den [Träningsmodell] [ train-model] modulen genererar ett fel</span><span class="sxs-lookup"><span data-stu-id="b6a35-105">the [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="b6a35-106">den [Poängmodell] [ score-model] modulen ger felaktiga resultat</span><span class="sxs-lookup"><span data-stu-id="b6a35-106">the [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="b6a35-107">Modulen för Train-modell genererar ett fel</span><span class="sxs-lookup"><span data-stu-id="b6a35-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="b6a35-109">Den [Träningsmodell] [ train-model] modulen förväntar två indata:</span><span class="sxs-lookup"><span data-stu-id="b6a35-109">The [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="b6a35-110">Typ av maskininlärningsmodell för insamling av modeller som tillhandahålls av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="b6a35-110">The type of machine learning model from the collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="b6a35-111">Utbildning data med en angiven etikett-kolumn som anger variabeln för att förutsäga (de andra kolumnerna antas vara funktioner).</span><span class="sxs-lookup"><span data-stu-id="b6a35-111">The training data with a specified Label column which specifies the variable to predict (the other columns are assumed to be Features).</span></span>

<span data-ttu-id="b6a35-112">Den här modulen kan producera ett fel i följande fall:</span><span class="sxs-lookup"><span data-stu-id="b6a35-112">This module can produce an error in the following cases:</span></span>

1. <span data-ttu-id="b6a35-113">Kolumnen etikett har angetts felaktigt.</span><span class="sxs-lookup"><span data-stu-id="b6a35-113">The Label column is specified incorrectly.</span></span> <span data-ttu-id="b6a35-114">Detta kan inträffa om mer än en kolumn har markerats som etikett eller en felaktig kolumnindex är markerad.</span><span class="sxs-lookup"><span data-stu-id="b6a35-114">This can happen if either more than one column is selected as the Label or an incorrect column index is selected.</span></span> <span data-ttu-id="b6a35-115">Det andra fallet skulle till exempel gälla om ett index på 30 används med en inkommande datamängd som har bara 25 kolumner.</span><span class="sxs-lookup"><span data-stu-id="b6a35-115">For example, the second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="b6a35-116">Dataset innehåller inte några kolumner i funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6a35-116">The dataset does not contain any Feature columns.</span></span> <span data-ttu-id="b6a35-117">Till exempel om den inkommande datamängden har endast en kolumn som har markerats som kolumnen etikett, är det inga funktioner som att skapa modellen.</span><span class="sxs-lookup"><span data-stu-id="b6a35-117">For example, if the input dataset has only one column, which is marked as the Label column, there would be no features with which to build the model.</span></span> <span data-ttu-id="b6a35-118">I det här fallet den [Träningsmodell] [ train-model] modulen genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="b6a35-118">In this case, the [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="b6a35-119">Inkommande datauppsättningen (funktioner eller etikett) innehåller oändligt som ett-värde.</span><span class="sxs-lookup"><span data-stu-id="b6a35-119">The input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="b6a35-120">Modulen poängsätta modell ger felaktiga resultat</span><span class="sxs-lookup"><span data-stu-id="b6a35-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="b6a35-122">I en typisk utbildning/testning experiment för övervakad inlärning av [dela Data] [ split] modulen delas den ursprungliga datauppsättningen i två delar: en del används för att träna modellen och en del används för att poängsätta hur väl utför den tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="b6a35-122">In a typical training/testing experiment for supervised learning, the [Split Data][split] module divides the original dataset into two parts: one part is used to train the model and one part is used to score how well the trained model performs.</span></span> <span data-ttu-id="b6a35-123">Den tränade modellen används sedan för att samla in testdata, efter vilken resultaten utvärderas för att fastställa korrektheten i modellen.</span><span class="sxs-lookup"><span data-stu-id="b6a35-123">The trained model is then used to score the test data, after which the results are evaluated to determine the accuracy of the model.</span></span>

<span data-ttu-id="b6a35-124">Den [Poängmodell] [ score-model] modulen kräver två indata:</span><span class="sxs-lookup"><span data-stu-id="b6a35-124">The [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="b6a35-125">En tränad modell utdata från den [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="b6a35-125">A trained model output from the [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="b6a35-126">En bedömningsprofil datamängd som skiljer sig från den datamängd som används för att träna modellen.</span><span class="sxs-lookup"><span data-stu-id="b6a35-126">A scoring dataset that is different from the dataset used to train the model.</span></span>

<span data-ttu-id="b6a35-127">Det är möjligt att även om försöket lyckas den [Poängmodell] [ score-model] modulen ger felaktiga resultat.</span><span class="sxs-lookup"><span data-stu-id="b6a35-127">It's possible that even though the experiment succeeds, the [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="b6a35-128">Flera scenarier kan orsaka detta ske:</span><span class="sxs-lookup"><span data-stu-id="b6a35-128">Several scenarios may cause this to happen:</span></span>

1. <span data-ttu-id="b6a35-129">Om den angivna etiketten är kategoriska och en regressionsmodell tränats på data, felaktig utdata skulle genereras av den [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="b6a35-129">If the specified Label is categorical and a regression model is trained on the data, an incorrect output would be produced by the [Score Model][score-model] module.</span></span> <span data-ttu-id="b6a35-130">Det beror på att regression kräver en kontinuerlig svar-variabel.</span><span class="sxs-lookup"><span data-stu-id="b6a35-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="b6a35-131">I så fall skulle vara mer lämpligt att använda en modell för klassificering.</span><span class="sxs-lookup"><span data-stu-id="b6a35-131">In this case, it would be more suitable to use a classification model.</span></span> 

2. <span data-ttu-id="b6a35-132">På samma sätt kan kan en klassificering modell tränas på en datamängd med flyttal i kolumnen etikett, det ge oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="b6a35-132">Similarly, if a classification model is trained on a dataset having floating point numbers in the Label column, it may produce undesirable results.</span></span> <span data-ttu-id="b6a35-133">Det beror på att klassificering kräver en diskret svar-variabel som endast tillåter att värden intervallet över en begränsad och vanligtvis något liten uppsättning klasser.</span><span class="sxs-lookup"><span data-stu-id="b6a35-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="b6a35-134">Om bedömningsprofil datamängden inte innehåller alla funktioner som används för att träna modellen, den [Poängmodell] [ score-model] genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="b6a35-134">If the scoring dataset does not contain all the features used to train the model, the [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="b6a35-135">Om en rad i bedömningsprofil datamängden innehåller ett saknat värde eller ett oändligt värde för någon av dess funktioner i [Poängmodell] [ score-model] kommer inte producerar några utdata som motsvarar den raden.</span><span class="sxs-lookup"><span data-stu-id="b6a35-135">If a row in the scoring dataset contains a missing value or an infinite value for any of its features, the [Score Model][score-model] will not produce any output corresponding to that row.</span></span>

5. <span data-ttu-id="b6a35-136">Den [Poängmodell] [ score-model] kan ge identiska utdata för alla rader i dataset bedömningsprofil.</span><span class="sxs-lookup"><span data-stu-id="b6a35-136">The [Score Model][score-model] may produce identical outputs for all rows in the scoring dataset.</span></span> <span data-ttu-id="b6a35-137">Detta kan exempelvis inträffa när försök klassificering med beslut skogar om det minsta antalet prov per lövnod väljs vara fler än antalet utbildning exempel tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b6a35-137">This could occur, for example, when attempting classification using Decision Forests if the minimum number of samples per leaf node is chosen to be more than the number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

