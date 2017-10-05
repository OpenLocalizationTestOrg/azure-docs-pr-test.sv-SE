---
title: "Skapa eller ändra labs automatiskt med Azure Resource Manager-mallar med PowerShell | Microsoft Docs"
description: "Lär dig hur du använder Azure Resource Manager-mallar med PowerShell för att skapa eller ändra labs automatiskt i ett DevTest-labb"
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
ms.openlocfilehash: cea4531175df2cc39790497dc049d27e23ffa0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="63dea-103">Skapa eller ändra labs automatiskt med Azure Resource Manager-mallar och PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="63dea-104">DevTest Labs innehåller mallar för Azure Resource Manager och PowerShell-skript som kan hjälpa dig att snabbt och automatiskt skapa nya övningar eller ändra befintliga labs och sedan distribuera dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="63dea-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="63dea-105">Den här artikeln får du stegvisa anvisningar genom att använda dessa mallar och skript för att automatisera skapas, ändras eller distribution av ditt labb.</span><span class="sxs-lookup"><span data-stu-id="63dea-105">This article helps guide you through the process of using these templates and scripts to automate the creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="63dea-106">Den här artikeln visar också var du hittar mer information om hur du använder PowerShell för att utföra vissa vanliga aktiviteter i DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="63dea-106">This article also shows you where you can find more information about how to use PowerShell to perform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="63dea-107">Steg 1: Samla in dina mallar och skript</span><span class="sxs-lookup"><span data-stu-id="63dea-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="63dea-108">Du kan hitta färdiga [Azure Resource Manager-mallar](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) och [PowerShell-skript](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) på vårt offentliga [Github-lagringsplatsen](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="63dea-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="63dea-109">Använda dem som-är, eller anpassa dem efter dina behov och lagra dem i din egen [privata Git repo](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="63dea-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="63dea-110">Steg 2: Ändra Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="63dea-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="63dea-111">Du kan följa stegen i [skapa din första Azure Resource Manager-mallen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) om du aldrig har skapat en mall innan.</span><span class="sxs-lookup"><span data-stu-id="63dea-111">You can follow the steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="63dea-112">Dessutom [bästa praxis för att skapa mallar för Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) erbjuder många riktlinjer och förslag på hur du skapar Azure Resource Manager-mallar som är tillförlitliga och enkla att använda.</span><span class="sxs-lookup"><span data-stu-id="63dea-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions to help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="63dea-113">Du kommer normalt använda en variant av en av de metoder eller exempel tillhandahålls och ändra mallen för dina behov.</span><span class="sxs-lookup"><span data-stu-id="63dea-113">Typically, you will use a variation of one of the approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="63dea-114">Steg 3: Distribuera resurser med PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="63dea-115">När du har anpassat dina mallar och skript, följer du stegen för att [distribuera resurser med Resource Manager-mallar och Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="63dea-115">After you have customized your templates and scripts, follow the steps necessary to [deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="63dea-116">Artikeln innehåller allmän information om hur du använder Azure PowerShell med Azure Resource Manager-mallar för att distribuera resurserna till Azure.</span><span class="sxs-lookup"><span data-stu-id="63dea-116">The article provides general information about using Azure PowerShell with Azure Resource Manager templates to deploy your resources to Azure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="63dea-117">Vanliga uppgifter som du kan utföra i DevTest labs med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="63dea-118">Det finns många andra vanliga aktiviteter som du kan automatisera med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63dea-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="63dea-119">I dokumentationen till följande avsnitt beskriver de steg som krävs för att utföra dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="63dea-119">The following sections of the documentation outline the steps required to perform these tasks.</span></span>

* [<span data-ttu-id="63dea-120">Skapa en anpassad avbildning från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="63dea-121">Överföra VHD-filen till labblagringskontot med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-121">Upload VHD file to lab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="63dea-122">Lägg till en extern användare i ett testlabb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-122">Add an external user to a lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="63dea-123">Skapa en anpassad labb-roll med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63dea-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="63dea-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63dea-124">Next steps</span></span>
* <span data-ttu-id="63dea-125">Lär dig hur du skapar en [privata Git-lagringsplats](devtest-lab-add-artifact-repo.md) där du vill lagra din anpassade mallar eller skript.</span><span class="sxs-lookup"><span data-stu-id="63dea-125">Learn how to create a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="63dea-126">Utforska den [Azure Resource Manager-mallar från Azure Quickstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="63dea-126">Explore the [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
