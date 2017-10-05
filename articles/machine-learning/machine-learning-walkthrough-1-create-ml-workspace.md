---
title: 'Steg 1: Skapa en arbetsyta i Machine Learning | Microsoft Docs'
description: "Steg 1 av utveckla en förutsägelselösning genomgång: Lär dig hur du ställer in en ny Azure Machine Learning Studio-arbetsyta."
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
ms.openlocfilehash: 8ca42ef8f5314866301f5c9e93caa90dc837a66e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Genomgång steg 1: Skapa en arbetsyta i Machine Learning
Detta är det första steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Skapa en Machine Learning-arbetsyta**
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera modellerna](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera webbtjänsten](machine-learning-walkthrough-5-publish-web-service.md)
6. [Få åtkomst till webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

Om du vill använda Machine Learning Studio, behöver du en Microsoft Azure Machine Learning-arbetsytan. Den här arbetsytan innehåller de verktyg du behöver för att skapa, hantera och publicera experiment.  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

För din Azure-prenumeration måste administratören skapa arbetsytan och lägga till dig som ägare eller deltagare. Mer information finns i [skapa och dela en Azure Machine Learning-arbetsytan](machine-learning-create-workspace.md).

När ditt arbetsområde har skapats kan du öppna Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Om du har mer än en arbetsyta kan välja du arbetsytan i verktygsfältet i det övre högra hörnet i fönstret.

![Välj arbetsyta i Studio][2]

> [!TIP]
> Om du har gjort en ägare av arbetsytan kan dela du experiment som du arbetar med genom att bjuda in andra till arbetsytan. Du kan göra detta i Machine Learning Studio på den **inställningar** sidan. Du behöver bara Microsoft-konto eller organisationskonto för varje användare.
> 
> På den **inställningar** klickar du på **användare**, klicka på **bjuda in fler användare** längst ned i fönstret.
> 
> 

- - -
**Nästa: [överför befintliga data](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
