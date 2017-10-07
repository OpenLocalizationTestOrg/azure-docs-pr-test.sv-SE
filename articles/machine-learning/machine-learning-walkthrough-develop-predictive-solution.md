---
title: "aaaA förutsägelselösning för kreditrisk med Machine Learning | Microsoft Docs"
description: "En detaljerad genomgång som visar hur toocreate en förutsägelseanalys för kredit riskbedömning i Azure Machine Learning Studio."
keywords: "kreditrisk, lösning för förutsägelseanalys, riskbedömning"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Genomgång: Utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning

I den här genomgången ska titta vi en utökad på hello arbetet med att utveckla en förutsägelseanalys i Machine Learning Studio. Vi utvecklar en enkel modell i Machine Learning Studio och sedan distribuera det som en Azure Machine Learning-webbtjänst där hello modell kan göra förutsägelser med hjälp av nya data. 

I den här genomgången förutsätter vi att du har använt Machine Learning Studio åtminstone någon gång och att du är någorlunda insatt i vad maskininlärning är. Men vi förväntar oss inte att du är en expert.

Om du aldrig har använt **Azure Machine Learning Studio** innan du kanske vill toostart med hello kursen [skapa din första datavetenskap experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md). Den här kursen tar dig igenom Machine Learning Studio för hello första gången. Den visar hello grunderna för hur toodrag och släpp-moduler på experimentet, ansluta dem till varandra, köra hello experiment och titta på hello resultat. Ett annat verktyg som kan vara till hjälp för att komma igång är ett diagram som ger en översikt över hello funktionerna i Machine Learning Studio. Du kan hämta och skriva ut det här: [Översiktsdiagram över funktionerna i Azure Machine Learning Studio ](machine-learning-studio-overview-diagram.md).
 
Om du är ny toohello fältet för maskininlärning i allmänhet är det videoserie som kan vara användbara tooyou. Den kallas [datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) och den kan ge dig en bra överblick toomachine learning med vanliga ord och begrepp.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a>hello-problem

Anta att du behöver toopredict kreditrisken för en person baserat på hello information de gav på en kreditansökan.  

Kreditriskbedömning är ett komplext problem, men vi ska förenkla det lite i den här genomgången. Vi använder det som ett exempel på hur du skapar en lösning för förutsägelseanalys i Microsoft Azure Machine Learning. toodo kan vi använda Azure Machine Learning Studio och Machine Learning-webbtjänst.  

## <a name="hello-solution"></a>hello-lösning

I den här detaljerade genomgången börjar vi med offentligt tillgängliga kreditriskdata och utvecklar och tränar en förutsägbar modell baserat på dessa data. Vi distribuera hello modellen som en webbtjänst så att den kan användas av andra för kreditriskbedömning.

toocreate denna lösning, vi gör du följande:  

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. [Åtkomst hello-webbtjänst](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Du hittar en fungerande kopia av hello experiment som vi utvecklar i den här genomgången i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Gå för**[genomgången - kredit risk förutsägelse](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  och på **öppna i Studio** toodownload en kopia av hello experiment i Machine Learning Studio-arbetsytan.
> 
> Den här genomgången är baserad på en förenklad version av hello exempelexperimentet [binär klassificering: kredit risk förutsägelse](http://go.microsoft.com/fwlink/?LinkID=525270), även tillgänglig i hello [galleriet](http://gallery.cortanaintelligence.com/).
