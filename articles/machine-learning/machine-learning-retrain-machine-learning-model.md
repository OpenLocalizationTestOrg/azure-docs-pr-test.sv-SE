---
title: "aaaRetrain en Maskininlärningsmodell | Microsoft Docs"
description: "Lär dig hur tooretrain en modell och uppdatera hello Web service toouse hello nyligen tränade modellen i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a>Träna om Machine Learning-modellen
Som en del av hello av operationalization för machine learning-modeller i Azure Machine Learning din modell tränas och sparas. Du sedan använda den toocreate predicative webbtjänsten. hello webbtjänsten kan sedan användas i webbplatser, instrumentpaneler och mobilappar. 

Modeller som du skapar med Machine Learning finns vanligtvis inte statisk. När nya data blir tillgängliga eller när hello konsumenter av hello API har måste sina egna data hello modellen toobe retrained. 

Omtränings kan inträffa ofta. Med hello programmässiga Omtränings-API-funktionen, kan du programmässigt träna om hello modellen webbtjänsten hello Omtränings-API: er och uppdatera hello med hello nyligen tränad modell. 

Det här dokumentet beskriver hello omtränings processen och visar hur toouse hello Omtränings-API: er.

## <a name="why-retrain-defining-hello-problem"></a>Träna om varför: definiera hello problem
Som en del av hello maskininlärning utbildning process, är en modell tränas med hjälp av en uppsättning data. Modeller som du skapar med Machine Learning finns vanligtvis inte statisk. När nya data blir tillgängliga eller när hello konsumenter av hello API har måste sina egna data hello modellen toobe retrained.

I dessa scenarier programmässiga API ger ett bekvämt sätt tooallow du eller hello konsumenter av din API: er toocreate en klient som kan, på en gång eller regelbunden basis träna om hello modellen med sina egna data. De kan sedan utvärdera hello resultaten av omtränings och uppdatera hello Web API toouse hello nyligen tränad modell.

> [!NOTE]
> Om du har en befintlig Träningsexperiment och nya webbtjänsten, kanske toocheck ut omtrimning en befintlig förutsägande webbtjänst i stället för följande hello genomgång som nämns i hello efter avsnittet.
> 
> 

## <a name="end-to-end-workflow"></a>Arbetsflödet slutpunkt till slutpunkt
hello innebär hello följande komponenter: A Träningsexperiment och Prediktivt Experiment publiceras som en webbtjänst. tooenable omtränings av en tränad modell, hello Träningsexperiment måste publiceras som en webbtjänst med hello utdata från en tränad modell. Detta gör att API toohello modell för omtränings. 

hello följande steg gäller tooboth nya och klassiska Web services:

Skapa hello inledande förutsägande webbtjänst:

* Skapa ett experiment utbildning
* Skapa en prediktiv web experiment
* Distribuera en prediktiv webbtjänst

Träna om hello-webbtjänsten:

* Uppdatera utbildning experiment tooallow för omtränings
* Distribuera hello omtränings-webbtjänst
* Använda hello Batch Execution Service code tooretrain hello modellen

En genomgång av hello föregående steg finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

Om du har distribuerat en klassiska webbtjänst:

* Skapa en ny slutpunkt på hello förutsägande webbtjänst
* Hämta hello korrigering URL och kod
* Använd hello korrigering URL toopoint hello ny slutpunkt på hello retrained modellen 

En genomgång av hello föregående steg finns [träna om en klassiska webbtjänst](machine-learning-retrain-a-classic-web-service.md).

Om du stöter på problem med omtränings klassiska webbtjänsten finns [felsökning hello omtränings av en webbtjänst för Azure Machine Learning klassiska](machine-learning-troubleshooting-retraining-models.md).

Om du har distribuerat en ny webbtjänst:

* Logga in tooyour Azure Resource Manager-konto
* Hämta hello Web service definition
* Exportera hello Web Service Definition som JSON
* Uppdatera hello referens toohello `ilearner` blob i hello JSON
* Importera hello JSON till en Web Service Definition
* Uppdatera hello-webbtjänsten med nya Web Service Definition

En genomgång av hello föregående steg finns [träna om en ny webbtjänst med Machine Learning Management PowerShell-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).

hello innebär för att ställa in omtränings för det klassiska hello följande steg:

![Översikt över omtränings][1]

hello innebär för att ställa in omtränings för en ny webbtjänst hello följande steg:

![Översikt över omtränings][7]

## <a name="other-resources"></a>Andra resurser
* [Omtränings och uppdaterar Azure Machine Learning-modeller med Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Skapa många Machine Learning-modeller och web service slutpunkter från ett experiment med hjälp av PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
* Hej [AML Omtränings modeller med hjälp av API: er](https://www.youtube.com/watch?v=wwjglA8xllg) videon visar hur tooretrain Machine Learning-modeller skapas i Azure Machine Learning med hello Omtränings-API: er och PowerShell.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

