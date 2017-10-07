---
title: "aaaCreate eller ändra labs automatiskt med Azure Resource Manager-mallar med PowerShell | Microsoft Docs"
description: "Lär dig hur toouse Azure Resource Manager-mallar med PowerShell toocreate eller ändra labs automatiskt i ett DevTest-labb"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="b7c5e-103">Skapa eller ändra labs automatiskt med Azure Resource Manager-mallar och PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="b7c5e-104">DevTest Labs innehåller mallar för Azure Resource Manager och PowerShell-skript som kan hjälpa dig att snabbt och automatiskt skapa nya övningar eller ändra befintliga labs och sedan distribuera dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="b7c5e-105">Den här artikeln får du hjälp hello processen med att använda dessa mallar och skript tooautomate hello skapas, ändras eller distribution av ditt labb.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-105">This article helps guide you through hello process of using these templates and scripts tooautomate hello creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="b7c5e-106">Den här artikeln visar också var du hittar mer information om hur toouse PowerShell tooperform några vanliga uppgifter i DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-106">This article also shows you where you can find more information about how toouse PowerShell tooperform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="b7c5e-107">Steg 1: Samla in dina mallar och skript</span><span class="sxs-lookup"><span data-stu-id="b7c5e-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="b7c5e-108">Du kan hitta färdiga [Azure Resource Manager-mallar](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) och [PowerShell-skript](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) på vårt offentliga [Github-lagringsplatsen](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="b7c5e-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="b7c5e-109">Använda dem som-är, eller anpassa dem efter dina behov och lagra dem i din egen [privata Git repo](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="b7c5e-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="b7c5e-110">Steg 2: Ändra Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b7c5e-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="b7c5e-111">Du kan följa stegen hello i [skapa din första Azure Resource Manager-mallen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) om du aldrig har skapat en mall innan.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-111">You can follow hello steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="b7c5e-112">Dessutom [bästa praxis för att skapa mallar för Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) erbjuder många riktlinjer och förslag toohelp du skapa Azure Resource Manager-mallar som är tillförlitlig och enkel toouse.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions toohelp you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="b7c5e-113">Normalt använder du en variant av en av hello metoder eller exempel tillhandahålls och ändra mallen för dina behov.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-113">Typically, you will use a variation of one of hello approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="b7c5e-114">Steg 3: Distribuera resurser med PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="b7c5e-115">När du har anpassat dina mallar och skript, så hello behövs för[distribuera resurser med Resource Manager-mallar och Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="b7c5e-115">After you have customized your templates and scripts, follow hello steps necessary too[deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="b7c5e-116">hello artikeln innehåller allmän information om hur du använder Azure PowerShell med Azure Resource Manager mallar toodeploy tooAzure dina resurser.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-116">hello article provides general information about using Azure PowerShell with Azure Resource Manager templates toodeploy your resources tooAzure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="b7c5e-117">Vanliga uppgifter som du kan utföra i DevTest labs med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="b7c5e-118">Det finns många andra vanliga aktiviteter som du kan automatisera med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="b7c5e-119">hello beskriver hello-dokumentationen i följande avsnitt hello steg krävs tooperform dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-119">hello following sections of hello documentation outline hello steps required tooperform these tasks.</span></span>

* [<span data-ttu-id="b7c5e-120">Skapa en anpassad avbildning från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="b7c5e-121">Överföra VHD-filen toolab storage-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-121">Upload VHD file toolab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="b7c5e-122">Lägg till en extern användare tooa testlabb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-122">Add an external user tooa lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="b7c5e-123">Skapa en anpassad labb-roll med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c5e-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="b7c5e-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7c5e-124">Next steps</span></span>
* <span data-ttu-id="b7c5e-125">Lär dig hur toocreate en [privata Git-lagringsplats](devtest-lab-add-artifact-repo.md) där du vill lagra din anpassade mallar eller skript.</span><span class="sxs-lookup"><span data-stu-id="b7c5e-125">Learn how toocreate a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="b7c5e-126">Utforska hello [Azure Resource Manager-mallar från Azure Quickstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="b7c5e-126">Explore hello [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
