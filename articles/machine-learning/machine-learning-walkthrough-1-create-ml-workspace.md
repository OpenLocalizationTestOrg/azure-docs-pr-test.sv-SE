---
title: 'Steg 1: Skapa en arbetsyta i Machine Learning | Microsoft Docs'
description: "Steg 1 av hello utveckla en förutsägelselösning genomgång: Lär dig hur tooset upp en ny Azure Machine Learning Studio-arbetsyta."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 93d2e240826db9768e85b00cab0eb62510b4efb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Genomgång steg 1: Skapa en arbetsyta i Machine Learning
Detta är hello första steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Skapa en Machine Learning-arbetsyta**
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. [Få åtkomst till hello-webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

toouse Machine Learning Studio, behöver du toohave en Microsoft Azure Machine Learning-arbetsytan. Den här arbetsytan innehåller hello verktyg du behöver toocreate, hantera och publicera experiment.  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Hej administratör för din Azure-prenumeration måste toocreate hello arbetsytan och lägga till dig som ägare eller deltagare. Mer information finns i [skapa och dela en Azure Machine Learning-arbetsytan](machine-learning-create-workspace.md).

När ditt arbetsområde har skapats kan du öppna Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Om du har mer än en arbetsyta, kan du välja hello arbetsytan i hello verktygsfältet i hello övre högra hörnet av hello-fönstret.

![Välj arbetsyta i Studio][2]

> [!TIP]
> Om du har gjort en ägare av hello arbetsytan kan du dela hello experiment som du arbetar med genom att bjuda in andra toohello arbetsytan. Du kan göra detta i Machine Learning Studio på hello **inställningar** sidan. Du behöver bara hello Microsoft-konto eller organisationskonto för varje användare.
> 
> På hello **inställningar** klickar du på **användare**, klicka på **bjuda in fler användare** längst hello hello-fönstret.
> 
> 

- - -
**Nästa: [överför befintliga data](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
