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
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a><span data-ttu-id="aa066-103">Genomgång steg 1: Skapa en arbetsyta i Machine Learning</span><span class="sxs-lookup"><span data-stu-id="aa066-103">Walkthrough Step 1: Create a Machine Learning workspace</span></span>
<span data-ttu-id="aa066-104">Detta är det första steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="aa066-104">This is the first step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

1. <span data-ttu-id="aa066-105">**Skapa en Machine Learning-arbetsyta**</span><span class="sxs-lookup"><span data-stu-id="aa066-105">**Create a Machine Learning workspace**</span></span>
2. [<span data-ttu-id="aa066-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="aa066-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="aa066-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="aa066-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="aa066-108">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="aa066-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="aa066-109">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="aa066-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="aa066-110">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="aa066-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

<span data-ttu-id="aa066-111">Om du vill använda Machine Learning Studio, behöver du en Microsoft Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="aa066-111">To use Machine Learning Studio, you need to have a Microsoft Azure Machine Learning workspace.</span></span> <span data-ttu-id="aa066-112">Den här arbetsytan innehåller de verktyg du behöver för att skapa, hantera och publicera experiment.</span><span class="sxs-lookup"><span data-stu-id="aa066-112">This workspace contains the tools you need to create, manage, and publish experiments.</span></span>  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

<span data-ttu-id="aa066-113">För din Azure-prenumeration måste administratören skapa arbetsytan och lägga till dig som ägare eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="aa066-113">The administrator for your Azure subscription needs to create the workspace and then add you as an owner or contributor.</span></span> <span data-ttu-id="aa066-114">Mer information finns i [skapa och dela en Azure Machine Learning-arbetsytan](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="aa066-114">For details, see [Create and share an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

<span data-ttu-id="aa066-115">När ditt arbetsområde har skapats kan du öppna Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span><span class="sxs-lookup"><span data-stu-id="aa066-115">After your workspace is created, open Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)).</span></span> <span data-ttu-id="aa066-116">Om du har mer än en arbetsyta kan välja du arbetsytan i verktygsfältet i det övre högra hörnet i fönstret.</span><span class="sxs-lookup"><span data-stu-id="aa066-116">If you have more than one workspace, you can select the workspace in the toolbar in the upper-right corner of the window.</span></span>

![Välj arbetsyta i Studio][2]

> [!TIP]
> <span data-ttu-id="aa066-118">Om du har gjort en ägare av arbetsytan kan dela du experiment som du arbetar med genom att bjuda in andra till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="aa066-118">If you were made an owner of the workspace, you can share the experiments you're working on by inviting others to the workspace.</span></span> <span data-ttu-id="aa066-119">Du kan göra detta i Machine Learning Studio på den **inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="aa066-119">You can do this in Machine Learning Studio on the **SETTINGS** page.</span></span> <span data-ttu-id="aa066-120">Du behöver bara Microsoft-konto eller organisationskonto för varje användare.</span><span class="sxs-lookup"><span data-stu-id="aa066-120">You just need the Microsoft account or organizational account for each user.</span></span>
> 
> <span data-ttu-id="aa066-121">På den **inställningar** klickar du på **användare**, klicka på **bjuda in fler användare** längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="aa066-121">On the **SETTINGS** page, click **USERS**, then click **INVITE MORE USERS** at the bottom of the window.</span></span>
> 
> 

- - -
<span data-ttu-id="aa066-122">**Nästa: [överför befintliga data](machine-learning-walkthrough-2-upload-data.md)**</span><span class="sxs-lookup"><span data-stu-id="aa066-122">**Next: [Upload existing data](machine-learning-walkthrough-2-upload-data.md)**</span></span>

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
