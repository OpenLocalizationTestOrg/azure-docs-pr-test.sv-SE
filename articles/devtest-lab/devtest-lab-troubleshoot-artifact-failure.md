---
title: aaaDiagnose artefakt fel i Azure DevTest Labs VM | Microsoft Docs
description: "Lär dig hur tootroubleshoot artefakt fel i DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a><span data-ttu-id="1dab3-103">Diagnostisera artefakt fel i hello labb</span><span class="sxs-lookup"><span data-stu-id="1dab3-103">Diagnose artifact failures in hello lab</span></span> 
<span data-ttu-id="1dab3-104">När du har skapat en artefakt, kan du kontrollera toosee om den har lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="1dab3-104">After you have created an artifact, you can check toosee if it succeeded or failed.</span></span> <span data-ttu-id="1dab3-105">Artefakt loggar i DevTest Labs innehåller information som du kan använda toodiagnose ett artefakt-fel.</span><span class="sxs-lookup"><span data-stu-id="1dab3-105">Artifact logs in DevTest Labs provide information you can use toodiagnose an artifact failure.</span></span> <span data-ttu-id="1dab3-106">Det finns ett par olika sätt som du kan visa hello artefakt logginformation för en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="1dab3-106">There are a couple different ways you can view hello artifact log information for a Windows VM.</span></span>

> [!NOTE]
> <span data-ttu-id="1dab3-107">tooensure att fel identifieras korrekt och förklaras, är det viktigt att hello artefakt är korrekt strukturerad.</span><span class="sxs-lookup"><span data-stu-id="1dab3-107">tooensure that failures are correctly identified and explained, it is important that hello artifact is properly structured.</span></span> <span data-ttu-id="1dab3-108">Information om hur toocorrectly konstruera en artefakt finns [skapa anpassade artefakter](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="1dab3-108">For information about how toocorrectly construct an artifact, see [Create custom artifacts](devtest-lab-artifact-author.md).</span></span> <span data-ttu-id="1dab3-109">Och toosee ett exempel på en korrekt strukturerad artefakt kolla detta [Test parametertyper](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefakt.</span><span class="sxs-lookup"><span data-stu-id="1dab3-109">And toosee an example of a properly structured artifact, check out this [Test Parameter Types](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artifact.</span></span>

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a><span data-ttu-id="1dab3-110">Felsöka artefakt fel med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1dab3-110">Troubleshoot artifact failures using hello Azure portal</span></span>
<span data-ttu-id="1dab3-111">toouse hello Azure portal toodiagnose fel vid skapande av artefakt, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="1dab3-111">toouse hello Azure portal toodiagnose failures during artifact creation, follow these steps:</span></span>

1. <span data-ttu-id="1dab3-112">Hello lista över resurser, Välj ditt labb.</span><span class="sxs-lookup"><span data-stu-id="1dab3-112">From hello list of resources, select your lab.</span></span>

2. <span data-ttu-id="1dab3-113">Välj hello Windows VM som innehåller hello artefakt som du vill tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="1dab3-113">Choose hello Windows VM that includes hello artifact you want tooinvestigate.</span></span>

3. <span data-ttu-id="1dab3-114">Hello vänstra panelen under **allmänna**, Välj **artefakter**.</span><span class="sxs-lookup"><span data-stu-id="1dab3-114">In hello left panel under **GENERAL**, choose **Artifacts**.</span></span> <span data-ttu-id="1dab3-115">En lista över artefakter som är associerade med den virtuella datorn visas, som anger hello namn hello artefakt och dess status.</span><span class="sxs-lookup"><span data-stu-id="1dab3-115">A list of artifacts associated with that VM appears, indicating hello name of hello artifact and its status.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. <span data-ttu-id="1dab3-117">Välj en artefakt som visar statusen **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="1dab3-117">Choose an artifact that shows a status of **Failed**.</span></span> <span data-ttu-id="1dab3-118">hello artefakt öppnas och visar ett tillägg meddelande som innehåller information om hello fel hello artefakt.</span><span class="sxs-lookup"><span data-stu-id="1dab3-118">hello artifact opens and shows an extension message that includes details about hello failure of hello artifact.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a><span data-ttu-id="1dab3-120">Felsöka artefakt-fel från hello VM</span><span class="sxs-lookup"><span data-stu-id="1dab3-120">Troubleshoot artifact failures from within hello VM</span></span>
<span data-ttu-id="1dab3-121">tooview hello artefakt loggar från hello virtuella datorn så här:</span><span class="sxs-lookup"><span data-stu-id="1dab3-121">tooview hello artifact logs from within hello virtual machine, follow these steps:</span></span>

1. <span data-ttu-id="1dab3-122">Logga in toohello VM som innehåller hello artefakt som du vill toodiagnose.</span><span class="sxs-lookup"><span data-stu-id="1dab3-122">Log in toohello VM that contains hello artifact you want toodiagnose.</span></span>

2. <span data-ttu-id="1dab3-123">Navigera tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status där ”1,9 är hello CSE-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="1dab3-123">Navigate tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status where "1.9 is hello CSE version number.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. <span data-ttu-id="1dab3-125">Öppna hello **status** filen tooview information som hjälper dig att diagnostisera artefakt fel för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1dab3-125">Open hello **status** file tooview information that helps diagnose artifact failures for that VM.</span></span>




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="1dab3-126">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="1dab3-126">Related blog posts</span></span>
* [<span data-ttu-id="1dab3-127">Ansluta till en VM-tooexisting AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1dab3-127">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="1dab3-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dab3-128">Next steps</span></span>
* <span data-ttu-id="1dab3-129">Lär dig hur för[lägga till ett Git-lagringsplatsen tooa labb](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="1dab3-129">Learn how too[add a Git repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

