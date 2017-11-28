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
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a><span data-ttu-id="a0ea8-103">Genomgång steg 1: Skapa en arbetsyta i Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a0ea8-103">Walkthrough Step 1: Create a Machine Learning workspace</span></span>
<span data-ttu-id="a0ea8-104">Detta är hello första steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="a0ea8-104">This is hello first step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

1. <span data-ttu-id="a0ea8-105">**Skapa en Machine Learning-arbetsyta**</span><span class="sxs-lookup"><span data-stu-id="a0ea8-105">**Create a Machine Learning workspace**</span></span>
2. [<span data-ttu-id="a0ea8-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="a0ea8-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="a0ea8-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="a0ea8-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="a0ea8-108">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="a0ea8-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="a0ea8-109">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a0ea8-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="a0ea8-110">Få åtkomst till hello-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="a0ea8-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

<span data-ttu-id="a0ea8-111">toouse Machine Learning Studio, behöver du toohave en Microsoft Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-111">toouse Machine Learning Studio, you need toohave a Microsoft Azure Machine Learning workspace.</span></span> <span data-ttu-id="a0ea8-112">Den här arbetsytan innehåller hello verktyg du behöver toocreate, hantera och publicera experiment.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-112">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

<span data-ttu-id="a0ea8-113">Hej administratör för din Azure-prenumeration måste toocreate hello arbetsytan och lägga till dig som ägare eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-113">hello administrator for your Azure subscription needs toocreate hello workspace and then add you as an owner or contributor.</span></span> <span data-ttu-id="a0ea8-114">Mer information finns i [skapa och dela en Azure Machine Learning-arbetsytan](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="a0ea8-114">For details, see [Create and share an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

<span data-ttu-id="a0ea8-115">När ditt arbetsområde har skapats kan du öppna Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span><span class="sxs-lookup"><span data-stu-id="a0ea8-115">After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span></span> <span data-ttu-id="a0ea8-116">Om du har mer än en arbetsyta, kan du välja hello arbetsytan i hello verktygsfältet i hello övre högra hörnet av hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-116">If you have more than one workspace, you can select hello workspace in hello toolbar in hello upper-right corner of hello window.</span></span>

![Välj arbetsyta i Studio][2]

> [!TIP]
> <span data-ttu-id="a0ea8-118">Om du har gjort en ägare av hello arbetsytan kan du dela hello experiment som du arbetar med genom att bjuda in andra toohello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-118">If you were made an owner of hello workspace, you can share hello experiments you're working on by inviting others toohello workspace.</span></span> <span data-ttu-id="a0ea8-119">Du kan göra detta i Machine Learning Studio på hello **inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-119">You can do this in Machine Learning Studio on hello **SETTINGS** page.</span></span> <span data-ttu-id="a0ea8-120">Du behöver bara hello Microsoft-konto eller organisationskonto för varje användare.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-120">You just need hello Microsoft account or organizational account for each user.</span></span>
> 
> <span data-ttu-id="a0ea8-121">På hello **inställningar** klickar du på **användare**, klicka på **bjuda in fler användare** längst hello hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="a0ea8-121">On hello **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at hello bottom of hello window.</span></span>
> 
> 

- - -
<span data-ttu-id="a0ea8-122">**Nästa: [överför befintliga data](machine-learning-walkthrough-2-upload-data.md)**</span><span class="sxs-lookup"><span data-stu-id="a0ea8-122">**Next: [Upload existing data](machine-learning-walkthrough-2-upload-data.md)**</span></span>

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
