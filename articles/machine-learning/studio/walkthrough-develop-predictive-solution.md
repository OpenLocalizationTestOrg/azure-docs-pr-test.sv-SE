---
title: "Förutsägelseanalys för kreditrisk med Machine Learning | Microsoft Docs"
description: "En detaljerad genomgång av hur du skapar en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning Studio."
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
ms.openlocfilehash: fe504d826b6c40099f1f8706ef7e8780eed5cf9a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Genomgång: Utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning

I den här genomgången ska vi titta närmare på hur du utvecklar en lösning för förutsägelseanalys i Machine Learning Studio. Vi ska skapa en enkel modell i Machine Learning Studio som vi sedan distribuerar som en Azure Machine Learning-webbtjänst, där modellen kan göra förutsägelser med nya data. 

I den här genomgången förutsätter vi att du har använt Machine Learning Studio åtminstone någon gång och att du är någorlunda insatt i vad maskininlärning är. Men vi förväntar oss inte att du är en expert.

Om du aldrig har använt **Azure Machine Learning Studio** innan kanske du vill börja med självstudierna, [Skapa ditt första dataexperiment i Azure Machine Learning Studio](create-experiment.md). Den självstudiekursen vänder sig till nybörjare och vägleder dig genom Machine Learning Studio. Vi går vi igenom grunderna och förklarar hur du drar och släpper moduler i ett experiment, hur du kopplar ihop dem och hur du kör experimentet. Vi avslutar med att titta på resultatet. Ett annat verktyg som hjälper dig att komma igång är ett diagram med en översikt över funktionerna i Machine Learning Studio. Du kan hämta och skriva ut det här: [Översiktsdiagram över funktionerna i Azure Machine Learning Studio ](studio-overview-diagram.md).
 
Om du inte har erfarenhet av maskininlärning finns det en bra videoserie som vi rekommenderar. Den heter [Datavetenskap för nybörjare](data-science-for-beginners-the-5-questions-data-science-answers.md) och är en bra introduktion som med enkelt språk förklarar vad maskininlärning är.


[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Problemet

Anta att du behöver förutsäga kreditrisken för en person baserat på den information som han eller hon fyller i på en kreditansökan.  

Kreditriskbedömning är ett komplext problem, men vi ska förenkla det lite i den här genomgången. Vi använder det som ett exempel på hur du skapar en lösning för förutsägelseanalys i Microsoft Azure Machine Learning. För att göra det använder vi Azure Machine Learning Studio och en Machine Learning-webbtjänst.  

## <a name="the-solution"></a>Lösningen

I den här detaljerade genomgången börjar vi med offentligt tillgängliga kreditriskdata och utvecklar och tränar en förutsägbar modell baserat på dessa data. Därefter distribuerar vi modellen som en webbtjänst så att den kan användas av andra för kreditriskbedömning.

Vi skapar lösningen för kreditriskbedömning genom att följa dessa steg:  

1. [Skapa en Machine Learning-arbetsyta](walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](walkthrough-2-upload-data.md)
3. [Skapa ett experiment](walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera modellerna](walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera webbtjänsten](walkthrough-5-publish-web-service.md)
6. [Få åtkomst till webbtjänsten](walkthrough-6-access-web-service.md)

> [!TIP] 
> Du hittar en fungerande kopia av experimentet som vi skapar i den här genomgången i [Cortana Intelligence-galleriet](https://gallery.cortanaintelligence.com). Gå till **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** (Genomgång –kreditriskbedömning) och klicka på **Öppna i Studio** för att ladda ned en kopia av experimentet till din Machine Learning Studio-arbetsyta.
> 
> Den här genomgången baseras på en förenklad version av exempelexperimentet [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Binär klassificering: kreditriskbedömning), som också finns i [galleriet](http://gallery.cortanaintelligence.com/).
