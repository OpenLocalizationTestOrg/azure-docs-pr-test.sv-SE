---
title: aaaCreate Machine Learning-arbetsytan | Microsoft Docs
description: "Hur toocreate en arbetsyta för Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a><span data-ttu-id="37f5e-103">Skapa och dela en Azure Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="37f5e-103">Create and share an Azure Machine Learning workspace</span></span>
<span data-ttu-id="37f5e-104">Den här menyn länkar tootopics som beskriver hur tooset in hello olika datavetenskap miljöer används av hello Cortana Analytics processen (CAP).</span><span class="sxs-lookup"><span data-stu-id="37f5e-104">This menu links tootopics that describe how tooset up hello various data science environments used by hello Cortana Analytics Process (CAPS).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

<span data-ttu-id="37f5e-105">toouse Azure Machine Learning Studio, behöver du toohave Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="37f5e-105">toouse Azure Machine Learning Studio, you need toohave a Machine Learning workspace.</span></span> <span data-ttu-id="37f5e-106">Den här arbetsytan innehåller hello verktyg du behöver toocreate, hantera och publicera experiment.</span><span class="sxs-lookup"><span data-stu-id="37f5e-106">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a><span data-ttu-id="37f5e-107">toocreate en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="37f5e-107">toocreate a workspace</span></span>
1. <span data-ttu-id="37f5e-108">Logga in toohello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="37f5e-108">Sign in toohello [Azure portal](https://portal.azure.com/)</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f5e-109">toosign i och skapa en arbetsyta måste du toobe administratör Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37f5e-109">toosign in and create a workspace, you need toobe an Azure subscription administrator.</span></span> 
    >
    > 

2. <span data-ttu-id="37f5e-110">Klicka på **+ nytt**</span><span class="sxs-lookup"><span data-stu-id="37f5e-110">Click **+New**</span></span>

3. <span data-ttu-id="37f5e-111">Välj **Intelligence + analys**, klickar du på **Machine Learning-arbetsytan**, klicka på **skapa**</span><span class="sxs-lookup"><span data-stu-id="37f5e-111">Select **Intelligence + analytics**, click **Machine Learning Workspace**, then click **Create**</span></span>

4. <span data-ttu-id="37f5e-112">Ange arbetsyteinformation om</span><span class="sxs-lookup"><span data-stu-id="37f5e-112">Enter your workspace information</span></span>

    - <span data-ttu-id="37f5e-113">Hej *Arbetsytenamn* kan vara upp too260 tecken, inte slutar med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="37f5e-113">hello *workspace name* may be up too260 characters, not ending in a space.</span></span> <span data-ttu-id="37f5e-114">hello-namnet får inte innehålla följande tecken:`< > * % & : \ ? + /`</span><span class="sxs-lookup"><span data-stu-id="37f5e-114">hello name can't include these characters: `< > * % & : \ ? + /`</span></span>
    - <span data-ttu-id="37f5e-115">Hej *web service-plan* du välja (eller skapa), tillsammans med hello associerade *prisnivån* du väljer, används om du distribuerar webbtjänster från den här arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="37f5e-115">hello *web service plan* you choose (or create), along with hello associated *pricing tier* you select, is used if you deploy web services from this workspace.</span></span>

    ![Skapa en ny arbetsyta](media/machine-learning-create-workspace/create-new-workspace.png)

5. <span data-ttu-id="37f5e-117">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="37f5e-117">Click **Create**</span></span>

<span data-ttu-id="37f5e-118">När hello arbetsytan har distribuerats kan öppna du den i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="37f5e-118">Once hello workspace is deployed, you can open it in Machine Learning Studio.</span></span>

1. <span data-ttu-id="37f5e-119">Bläddra tooMachine Learning Studio på [https://studio.azureml.net/](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="37f5e-119">Browse tooMachine Learning Studio at [https://studio.azureml.net/](https://studio.azureml.net/).</span></span>

2. <span data-ttu-id="37f5e-120">Välj din arbetsyta i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="37f5e-120">Select your workspace in hello upper-right-hand corner.</span></span>

    ![Välj arbetsyta](media/machine-learning-create-workspace/open-workspace.png)

3. <span data-ttu-id="37f5e-122">Klicka på **Mina experiment**.</span><span class="sxs-lookup"><span data-stu-id="37f5e-122">Click **my experiments**.</span></span>

    ![Öppna experiment](media/machine-learning-create-workspace/my-experiments.png)

<span data-ttu-id="37f5e-124">Information om hur du hanterar din arbetsyta finns [hantera en Azure Machine Learning-arbetsytan](machine-learning-manage-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="37f5e-124">For information about managing your workspace, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).</span></span>
<span data-ttu-id="37f5e-125">Om det uppstår ett problem uppstod när din arbetsyta, se [felsökningsguide: skapa och ansluta tooa Machine Learning-arbetsytan](machine-learning-troubleshooting-creating-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="37f5e-125">If you encounter a problem creating your workspace, see [Troubleshooting guide: Create and connect tooa Machine Learning workspace](machine-learning-troubleshooting-creating-ml-workspace.md).</span></span>


## <a name="sharing-an-azure-machine-learning-workspace"></a><span data-ttu-id="37f5e-126">Dela en Azure Machine Learning-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="37f5e-126">Sharing an Azure Machine Learning workspace</span></span>
<span data-ttu-id="37f5e-127">När Machine Learning-arbetsytan har skapats kan erbjuda du användare tooyour arbetsytan tooshare åtkomst tooyour arbetsytan och alla dess experiment, datauppsättningar, bärbara datorer och så vidare. Du kan lägga till användare i en av två roller:</span><span class="sxs-lookup"><span data-stu-id="37f5e-127">Once a Machine Learning workspace is created, you can invite users tooyour workspace tooshare access tooyour workspace and all its experiments, datasets, notebooks, etc. You can add users in one of two roles:</span></span>

* <span data-ttu-id="37f5e-128">**Användaren** -arbetsytan användare kan skapa, öppna, ändra och ta bort experiment, datauppsättningar, etc. hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="37f5e-128">**User** - A workspace user can create, open, modify, and delete experiments, datasets, etc. in hello workspace.</span></span>
* <span data-ttu-id="37f5e-129">**Ägare** – bjuda in en ägare och ta bort användare hello arbetsytan i tillägg toowhat en användare kan göra.</span><span class="sxs-lookup"><span data-stu-id="37f5e-129">**Owner** - An owner can invite and remove users in hello workspace, in addition toowhat a user can do.</span></span>

> [!NOTE]
> <span data-ttu-id="37f5e-130">hello administratörskontot som skapar hello arbetsyta läggs automatiskt toohello arbetsytan som arbetsyta ägare.</span><span class="sxs-lookup"><span data-stu-id="37f5e-130">hello administrator account that creates hello workspace is automatically added toohello workspace as workspace Owner.</span></span> <span data-ttu-id="37f5e-131">Men andra administratörer eller användare i att prenumerationen inte är automatiskt beviljas åtkomst till toohello arbetsytan – du behöver tooinvite dem explicit.</span><span class="sxs-lookup"><span data-stu-id="37f5e-131">However, other administrators or users in that subscription are not automatically granted access toohello workspace - you need tooinvite them explicitly.</span></span>
> 
> 

### <a name="tooshare-a-workspace"></a><span data-ttu-id="37f5e-132">tooshare en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="37f5e-132">tooshare a workspace</span></span>

1. <span data-ttu-id="37f5e-133">Logga in tooMachine Learning Studio på [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span><span class="sxs-lookup"><span data-stu-id="37f5e-133">Sign in tooMachine Learning Studio at [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span></span>

2. <span data-ttu-id="37f5e-134">I hello vänstra panelen, klickar du på **inställningar**</span><span class="sxs-lookup"><span data-stu-id="37f5e-134">In hello left panel, click **SETTINGS**</span></span>

3. <span data-ttu-id="37f5e-135">Klicka på hello **användare** fliken</span><span class="sxs-lookup"><span data-stu-id="37f5e-135">Click hello **USERS** tab</span></span>

4. <span data-ttu-id="37f5e-136">Klicka på **bjuda in fler användare** på hello hello sidans nederkant</span><span class="sxs-lookup"><span data-stu-id="37f5e-136">Click **INVITE MORE USERS** at hello bottom of hello page</span></span>

    ![Inställningar för Studio](media/machine-learning-create-workspace/settings.png)

5. <span data-ttu-id="37f5e-138">Ange en eller flera e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="37f5e-138">Enter one or more email addresses.</span></span> <span data-ttu-id="37f5e-139">hello krävs ett giltigt microsoftkonto eller ett organisationskonto (från Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="37f5e-139">hello users need a valid Microsoft account or an organizational account (from Azure Active Directory).</span></span>

6. <span data-ttu-id="37f5e-140">Välj om du vill tooadd hello användare som ägare eller användare.</span><span class="sxs-lookup"><span data-stu-id="37f5e-140">Select whether you want tooadd hello users as Owner or User.</span></span>

7. <span data-ttu-id="37f5e-141">Klicka på hello **OK** markering knappen.</span><span class="sxs-lookup"><span data-stu-id="37f5e-141">Click hello **OK** checkmark button.</span></span>

<span data-ttu-id="37f5e-142">Varje användare som du lägger till kommer att få ett e-postmeddelande med instruktioner om hur toosign i toohello delas arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="37f5e-142">Each user you add will receive an email with instructions on how toosign in toohello shared workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="37f5e-143">För användare toobe kan toodeploy eller hantera webbtjänster i den här arbetsytan, de måste vara en deltagare eller en administratör i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37f5e-143">For users toobe able toodeploy or manage web services in this workspace, they must be a contributor or administrator in hello Azure subscription.</span></span> 



