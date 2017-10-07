---
title: aaaHow tootag Windows VM resurs i Azure | Microsoft Docs
description: "Lär dig mer om en Windows-dator som skapats i Azure med hjälp av Resource Manager-modellen hello-märkning"
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
ms.openlocfilehash: 160416ddc35998b3c98c6e579668a6a5eb6de6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="4e52a-103">Hur tootag en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="4e52a-103">How tootag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="4e52a-104">Den här artikeln beskrivs olika sätt tootag en Windows-dator i Azure med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="4e52a-104">This article describes different ways tootag a Windows virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="4e52a-105">Taggar är användardefinierade nyckel/värde-par som kan placeras direkt på en resurs eller en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e52a-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="4e52a-106">Azure stöder för närvarande in too15 taggar per resurs och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4e52a-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="4e52a-107">Taggar kan placeras på en resurs för närvarande hello skapas eller lägga tooan befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="4e52a-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="4e52a-108">Observera att taggar stöds för resurser som skapats via hello Resource Manager distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4e52a-108">Please note that tags are supported for resources created via hello Resource Manager deployment model only.</span></span> <span data-ttu-id="4e52a-109">Om du vill tootag en Linux-dator, se [hur tootag en Linux-dator i Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4e52a-109">If you want tootag a Linux virtual machine, see [How tootag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="4e52a-110">Tagga med PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e52a-110">Tagging with PowerShell</span></span>
<span data-ttu-id="4e52a-111">toocreate, lägga till och ta bort taggar via PowerShell, måste du först tooset upp din [PowerShell-miljö med Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="4e52a-111">toocreate, add, and delete tags through PowerShell, you first need tooset up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="4e52a-112">När du har slutfört installationen hello, kan du placera taggar på beräkning, nätverk och lagring resurser vid skapandet eller när hello resursen har skapats via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4e52a-112">Once you have completed hello setup, you can place tags on Compute, Network, and Storage resources at creation or after hello resource is created via PowerShell.</span></span> <span data-ttu-id="4e52a-113">Den här artikeln kommer att inriktas på Visa/redigera taggar som placeras på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4e52a-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="4e52a-114">Först gå tooa virtuella datorn via hello `Get-AzureRmVM` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4e52a-114">First, navigate tooa Virtual Machine through hello `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="4e52a-115">Om den virtuella datorn redan innehåller taggar kan sedan visas alla hello taggar på resursen:</span><span class="sxs-lookup"><span data-stu-id="4e52a-115">If your Virtual Machine already contains tags, you will then see all hello tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="4e52a-116">Om du vill tooadd taggar via PowerShell kan du använda hello `Set-AzureRmResource` kommando.</span><span class="sxs-lookup"><span data-stu-id="4e52a-116">If you would like tooadd tags through PowerShell, you can use hello `Set-AzureRmResource` command.</span></span> <span data-ttu-id="4e52a-117">Observera vid uppdatering av taggarna via PowerShell taggar uppdateras som helhet.</span><span class="sxs-lookup"><span data-stu-id="4e52a-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="4e52a-118">Så om du lägger till en tagg tooa resurs som redan har taggar måste tooinclude alla hello-taggar som du vill toobe placerad hello resurs.</span><span class="sxs-lookup"><span data-stu-id="4e52a-118">So if you are adding one tag tooa resource that already has tags, you will need tooinclude all hello tags that you want toobe placed on hello resource.</span></span> <span data-ttu-id="4e52a-119">Nedan visas ett exempel på hur tooadd ytterligare taggar tooa resursen via PowerShell-Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4e52a-119">Below is an example of how tooadd additional tags tooa resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="4e52a-120">Den här första cmdlet anger alla hello taggarna placerad *MyTestVM* toohello *$tags* variabel, med hjälp av hello `Get-AzureRmResource` och `Tags` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="4e52a-120">This first cmdlet sets all of hello tags placed on *MyTestVM* toohello *$tags* variable, using hello `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="4e52a-121">hello andra kommandot visar hello taggar för hello angivna variabeln.</span><span class="sxs-lookup"><span data-stu-id="4e52a-121">hello second command displays hello tags for hello given variable.</span></span>

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

<span data-ttu-id="4e52a-122">hello tredje kommandot lägger till en ytterligare taggen toohello *$tags* variabeln.</span><span class="sxs-lookup"><span data-stu-id="4e52a-122">hello third command adds an additional tag toohello *$tags* variable.</span></span> <span data-ttu-id="4e52a-123">Observera hello användning av hello  **+=**  tooappend hello nytt nyckel/värde-par toohello *$tags* lista.</span><span class="sxs-lookup"><span data-stu-id="4e52a-123">Note hello use of hello **+=** tooappend hello new key/value pair toohello *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="4e52a-124">hello fjärde kommando anges alla hello-taggar som definierats i hello *$tags* variabeln toohello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="4e52a-124">hello fourth command sets all of hello tags defined in hello *$tags* variable toohello given resource.</span></span> <span data-ttu-id="4e52a-125">I det här fallet är det MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="4e52a-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="4e52a-126">hello femte kommando visar alla hello taggarna på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="4e52a-126">hello fifth command displays all of hello tags on hello resource.</span></span> <span data-ttu-id="4e52a-127">Som du kan se *plats* nu har definierats som en tagg med *MyLocation* som hello-värde.</span><span class="sxs-lookup"><span data-stu-id="4e52a-127">As you can see, *Location* is now defined as a tag with *MyLocation* as hello value.</span></span>

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

<span data-ttu-id="4e52a-128">Mer om toolearn taggning via PowerShell, kolla hello [Cmdlets för Azure-resurs][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="4e52a-128">toolearn more about tagging through PowerShell, check out hello [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="4e52a-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e52a-129">Next steps</span></span>
* <span data-ttu-id="4e52a-130">toolearn mer information om resurserna i Azure-märkning finns [översikt över Azure Resource Manager] [ Azure Resource Manager Overview] och [med hjälp av taggar tooorganize dina Azure-resurser] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="4e52a-130">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="4e52a-131">toosee hur taggar kan hjälpa dig att hantera din användning av Azure-resurser, se [förstå fakturan Azure] [ Understanding your Azure Bill] och [få insikter om dina Microsoft Azure-resursförbrukning] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="4e52a-131">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
