---
title: Diagnostisera artefakt fel i Azure DevTest Labs VM | Microsoft Docs
description: "Lär dig hur du felsöker artefakt fel i DevTest Labs"
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
ms.openlocfilehash: e4f2946d0ba0756f36622aded0e8594acabb9527
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="diagnose-artifact-failures-in-the-lab"></a><span data-ttu-id="5d6f6-103">Diagnostisera artefakt fel i labbet</span><span class="sxs-lookup"><span data-stu-id="5d6f6-103">Diagnose artifact failures in the lab</span></span> 
<span data-ttu-id="5d6f6-104">När du har skapat en artefakt, kan du kontrollera om det har lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-104">After you have created an artifact, you can check to see if it succeeded or failed.</span></span> <span data-ttu-id="5d6f6-105">Artefakt loggar i DevTest Labs innehåller information som du kan använda för att diagnostisera ett artefakt-fel.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-105">Artifact logs in DevTest Labs provide information you can use to diagnose an artifact failure.</span></span> <span data-ttu-id="5d6f6-106">Det finns ett par olika sätt som du kan visa artefakt logginformation för en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-106">There are a couple different ways you can view the artifact log information for a Windows VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5d6f6-107">För att säkerställa att fel identifieras korrekt och förklaras, är det viktigt att artefakten är korrekt strukturerad.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-107">To ensure that failures are correctly identified and explained, it is important that the artifact is properly structured.</span></span> <span data-ttu-id="5d6f6-108">Information om hur du skapar en artefakt korrekt finns [skapa anpassade artefakter](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="5d6f6-108">For information about how to correctly construct an artifact, see [Create custom artifacts](devtest-lab-artifact-author.md).</span></span> <span data-ttu-id="5d6f6-109">Och om du vill se ett exempel på en korrekt strukturerad artefakt kolla detta [Test parametertyper](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefakt.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-109">And to see an example of a properly structured artifact, check out this [Test Parameter Types](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artifact.</span></span>

## <a name="troubleshoot-artifact-failures-using-the-azure-portal"></a><span data-ttu-id="5d6f6-110">Felsöka artefakt fel med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5d6f6-110">Troubleshoot artifact failures using the Azure portal</span></span>
<span data-ttu-id="5d6f6-111">Om du vill använda Azure portal för att diagnostisera fel under skapande av artefakt, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5d6f6-111">To use the Azure portal to diagnose failures during artifact creation, follow these steps:</span></span>

1. <span data-ttu-id="5d6f6-112">Välj ditt labb i listan över resurser.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-112">From the list of resources, select your lab.</span></span>

2. <span data-ttu-id="5d6f6-113">Välj den Windows-VM som innehåller den artefakt som du vill undersöka.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-113">Choose the Windows VM that includes the artifact you want to investigate.</span></span>

3. <span data-ttu-id="5d6f6-114">I den vänstra rutan under **allmänna**, Välj **artefakter**.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-114">In the left panel under **GENERAL**, choose **Artifacts**.</span></span> <span data-ttu-id="5d6f6-115">En lista över artefakter som är associerade med den virtuella datorn visas som anger namnet på artefakten och dess status.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-115">A list of artifacts associated with that VM appears, indicating the name of the artifact and its status.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. <span data-ttu-id="5d6f6-117">Välj en artefakt som visar statusen **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-117">Choose an artifact that shows a status of **Failed**.</span></span> <span data-ttu-id="5d6f6-118">Artefakten öppnas och visar ett tillägg meddelande som innehåller information om felet för artefakten.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-118">The artifact opens and shows an extension message that includes details about the failure of the artifact.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-vm"></a><span data-ttu-id="5d6f6-120">Felsöka artefakt fel från den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5d6f6-120">Troubleshoot artifact failures from within the VM</span></span>
<span data-ttu-id="5d6f6-121">Följ dessa steg om du vill visa artefakt-loggar från den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="5d6f6-121">To view the artifact logs from within the virtual machine, follow these steps:</span></span>

1. <span data-ttu-id="5d6f6-122">Logga in på den virtuella datorn som innehåller den artefakt som du vill felsöka.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-122">Log in to the VM that contains the artifact you want to diagnose.</span></span>

2. <span data-ttu-id="5d6f6-123">Navigera till C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status där ”1,9 är CSE-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-123">Navigate to C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status where "1.9 is the CSE version number.</span></span>

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. <span data-ttu-id="5d6f6-125">Öppna den **status** fil att visa information som hjälper dig att diagnostisera artefakt fel för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5d6f6-125">Open the **status** file to view information that helps diagnose artifact failures for that VM.</span></span>




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="5d6f6-126">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="5d6f6-126">Related blog posts</span></span>
* [<span data-ttu-id="5d6f6-127">Anslut en virtuell dator till befintliga AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5d6f6-127">Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="5d6f6-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d6f6-128">Next steps</span></span>
* <span data-ttu-id="5d6f6-129">Lär dig hur du [lägga till en Git-lagringsplats i ett labb](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="5d6f6-129">Learn how to [add a Git repository to a lab](devtest-lab-add-artifact-repo.md).</span></span>

