---
title: Hur du tagga en Windows VM-resurs i Azure | Microsoft Docs
description: "Lär dig mer om en Windows-dator som skapats i Azure med hjälp av Resource Manager-distributionsmodellen-märkning"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 5f00c4265cea3db02dbb09a7f81be636a3fdd3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="9ff83-103">Hur du tagga en virtuell Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="9ff83-103">How to tag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="9ff83-104">Den här artikeln beskrivs olika sätt att märka en virtuell Windows-dator i Azure via Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-104">This article describes different ways to tag a Windows virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="9ff83-105">Taggar är användardefinierade nyckel/värde-par som kan placeras direkt på en resurs eller en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9ff83-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="9ff83-106">Azure stöder för närvarande upp till 15 taggar per resurs och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="9ff83-107">Taggar kan placeras på en resurs vid tidpunkten för skapandet eller lägga till en befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="9ff83-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="9ff83-108">Observera att taggar stöds för resurser som skapats via den Resource Manager distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-108">Please note that tags are supported for resources created via the Resource Manager deployment model only.</span></span> <span data-ttu-id="9ff83-109">Om du vill att märka en Linux-dator, se [så att märka en Linux-dator i Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ff83-109">If you want to tag a Linux virtual machine, see [How to tag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="9ff83-110">Tagga med PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ff83-110">Tagging with PowerShell</span></span>
<span data-ttu-id="9ff83-111">För att skapa, lägga till och ta bort taggar via PowerShell kan du första behovet av att ställa in din [PowerShell-miljö med Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="9ff83-111">To create, add, and delete tags through PowerShell, you first need to set up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="9ff83-112">När du har slutfört installationen kan placera du taggar på beräkning, nätverk och lagring resurser vid skapandet eller när resursen har skapats via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ff83-112">Once you have completed the setup, you can place tags on Compute, Network, and Storage resources at creation or after the resource is created via PowerShell.</span></span> <span data-ttu-id="9ff83-113">Den här artikeln kommer att inriktas på Visa/redigera taggar som placeras på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9ff83-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="9ff83-114">Först gå till en virtuell dator via den `Get-AzureRmVM` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9ff83-114">First, navigate to a Virtual Machine through the `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="9ff83-115">Om den virtuella datorn redan innehåller taggar kan sedan visas alla taggar på resursen:</span><span class="sxs-lookup"><span data-stu-id="9ff83-115">If your Virtual Machine already contains tags, you will then see all the tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="9ff83-116">Om du vill lägga till taggar via PowerShell kan du använda den `Set-AzureRmResource` kommando.</span><span class="sxs-lookup"><span data-stu-id="9ff83-116">If you would like to add tags through PowerShell, you can use the `Set-AzureRmResource` command.</span></span> <span data-ttu-id="9ff83-117">Observera vid uppdatering av taggarna via PowerShell taggar uppdateras som helhet.</span><span class="sxs-lookup"><span data-stu-id="9ff83-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="9ff83-118">Så om du lägger till en tagg till en resurs som redan har taggar måste att inkludera alla taggar som du vill ska placeras på resursen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-118">So if you are adding one tag to a resource that already has tags, you will need to include all the tags that you want to be placed on the resource.</span></span> <span data-ttu-id="9ff83-119">Nedan visas ett exempel på hur du lägger till ytterligare taggar till en resurs via PowerShell-Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="9ff83-119">Below is an example of how to add additional tags to a resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="9ff83-120">Den här första cmdlet anger alla taggar placerade på *MyTestVM* till den *$tags* variabel, med hjälp av den `Get-AzureRmResource` och `Tags` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-120">This first cmdlet sets all of the tags placed on *MyTestVM* to the *$tags* variable, using the `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="9ff83-121">Det andra kommandot visar etiketter för den angivna variabeln.</span><span class="sxs-lookup"><span data-stu-id="9ff83-121">The second command displays the tags for the given variable.</span></span>

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

<span data-ttu-id="9ff83-122">Tredje kommandot lägger till en ytterligare så att den *$tags* variabeln.</span><span class="sxs-lookup"><span data-stu-id="9ff83-122">The third command adds an additional tag to the *$tags* variable.</span></span> <span data-ttu-id="9ff83-123">Observera användningen av den  **+=**  ska läggas till på nytt nyckel/värde-par till den *$tags* lista.</span><span class="sxs-lookup"><span data-stu-id="9ff83-123">Note the use of the **+=** to append the new key/value pair to the *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="9ff83-124">Kommandot fjärde anger alla taggar som definierats i den *$tags* variabeln till den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-124">The fourth command sets all of the tags defined in the *$tags* variable to the given resource.</span></span> <span data-ttu-id="9ff83-125">I det här fallet är det MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="9ff83-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="9ff83-126">Det femte kommandot visar alla taggar på resursen.</span><span class="sxs-lookup"><span data-stu-id="9ff83-126">The fifth command displays all of the tags on the resource.</span></span> <span data-ttu-id="9ff83-127">Som du kan se *plats* nu har definierats som en tagg med *MyLocation* som värde.</span><span class="sxs-lookup"><span data-stu-id="9ff83-127">As you can see, *Location* is now defined as a tag with *MyLocation* as the value.</span></span>

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

<span data-ttu-id="9ff83-128">Om du vill veta mer om märkning via PowerShell kan ta en titt på [Cmdlets för Azure-resurs][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="9ff83-128">To learn more about tagging through PowerShell, check out the [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="9ff83-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ff83-129">Next steps</span></span>
* <span data-ttu-id="9ff83-130">Läs mer om resurserna i Azure-märkning i [översikt över Azure Resource Manager] [ Azure Resource Manager Overview] och [med taggar för att organisera dina Azure-resurser][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="9ff83-130">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="9ff83-131">Om du vill se hur taggar kan hjälpa dig att hantera din användning av Azure-resurser, se [förstå fakturan Azure] [ Understanding your Azure Bill] och [få insikter om dina Microsoft Azure-resursförbrukning][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="9ff83-131">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
