---
title: "aaaCreate en kopia av en Azure-hanterade disken för tillbaka in | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av en Azure-hanterade disken toouse för tillbaka dig eller felsökning disken utfärdar."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="faaf8-103">Skapa en kopia av en virtuell Hårddisk lagras som en hanterad Azure-disken med hjälp av hanterade ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="faaf8-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="faaf8-104">Ta en ögonblicksbild av en hanterad Disk för säkerhetskopiering eller skapa en Disk som hanteras av hello ögonblicksbild och koppla den tooa test virtuella tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="faaf8-104">Take a snapshot of a Managed Disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="faaf8-105">En ögonblicksbild av en hanterad är en fullständig i tidpunkt kopia av en virtuell dator hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="faaf8-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="faaf8-106">Den skapar en skrivskyddad kopia av den virtuella Hårddisken och som standard lagras som en Standard hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="faaf8-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> <span data-ttu-id="faaf8-107">Mer information om hanterade diskar finns [översikt över Azure hanterade diskar](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="faaf8-107">For more information about Managed Disks, see [Azure Managed Disks overview](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="faaf8-108">Information om priser finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="faaf8-108">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="faaf8-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="faaf8-109">Before you begin</span></span>
<span data-ttu-id="faaf8-110">Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="faaf8-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="faaf8-111">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="faaf8-111">Run hello following command tooinstall it.</span></span>

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="faaf8-112">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="faaf8-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>

## <a name="copy-hello-vhd-with-a-snapshot"></a><span data-ttu-id="faaf8-113">Kopiera hello VHD med en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="faaf8-113">Copy hello VHD with a snapshot</span></span>
<span data-ttu-id="faaf8-114">Använd hello Azure-portalen eller PowerShell tootake en ögonblicksbild av hello hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="faaf8-114">Use either hello Azure portal or PowerShell tootake a snapshot of hello Managed Disk.</span></span>

### <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="faaf8-115">Använda Azure portal tootake en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="faaf8-115">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="faaf8-116">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="faaf8-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="faaf8-117">Från och med hello övre vänstra hörnet, klickar du på **ny** och Sök efter **ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="faaf8-117">Starting in hello upper left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="faaf8-118">I hello Snapshot-bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="faaf8-118">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="faaf8-119">Ange en **namn** för hello ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="faaf8-119">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="faaf8-120">Välj en befintlig [resursgruppen](../../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.</span><span class="sxs-lookup"><span data-stu-id="faaf8-120">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="faaf8-121">Välj ett Azure-datacenter plats.</span><span class="sxs-lookup"><span data-stu-id="faaf8-121">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="faaf8-122">För **källdisken**, Välj hello hanteras Disk toosnapshot.</span><span class="sxs-lookup"><span data-stu-id="faaf8-122">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="faaf8-123">Välj hello **kontotyp** toouse toostore hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="faaf8-123">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="faaf8-124">Vi rekommenderar **Standard_LRS** om du inte behöver den lagras på en disk med hög prestanda.</span><span class="sxs-lookup"><span data-stu-id="faaf8-124">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="faaf8-125">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="faaf8-125">Click **Create**.</span></span>

### <a name="use-powershell-tootake-a-snapshot"></a><span data-ttu-id="faaf8-126">Använd PowerShell tootake en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="faaf8-126">Use PowerShell tootake a snapshot</span></span>
<span data-ttu-id="faaf8-127">hello följande steg visar hur tooget hello VHD disk toobe kopieras, skapa hello konfigurationer av ögonblicksbilder och ta en ögonblicksbild av hello disk med hjälp av cmdlet hello ny AzureRmSnapshot<!--Add link toocmdlet when available-->.</span><span class="sxs-lookup"><span data-stu-id="faaf8-127">hello following steps show you how tooget hello VHD disk toobe copied, create hello snapshot configurations, and take a snapshot of hello disk by using hello New-AzureRmSnapshot cmdlet<!--Add link toocmdlet when available-->.</span></span> 

1. <span data-ttu-id="faaf8-128">Ange vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="faaf8-128">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  <span data-ttu-id="faaf8-129">Ersätt hello parametervärden:</span><span class="sxs-lookup"><span data-stu-id="faaf8-129">Replace hello parameter values:</span></span>
  -  <span data-ttu-id="faaf8-130">”myResourceGroup” hello VM resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="faaf8-130">"myResourceGroup" with hello VM's resource group.</span></span>
  -  <span data-ttu-id="faaf8-131">”southeastasia” med hello geografiska plats där du vill att din hanteras ögonblicksbild lagras.</span><span class="sxs-lookup"><span data-stu-id="faaf8-131">"southeastasia" with hello geographic location where you want your Managed Snapshot stored.</span></span> <!---How do you look these up? -->
  -  <span data-ttu-id="faaf8-132">”ContosoMD_datadisk1” med namnet hello av hello VHD-disk som du vill toocopy.</span><span class="sxs-lookup"><span data-stu-id="faaf8-132">"ContosoMD_datadisk1" with hello name of hello VHD disk that you want toocopy.</span></span>
  -  <span data-ttu-id="faaf8-133">”ContosoMD_datadisk1_snapshot1” med hello namn du vill använda toouse för hello ny ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="faaf8-133">"ContosoMD_datadisk1_snapshot1" with hello name you want toouse for hello new snapshot.</span></span>

2. <span data-ttu-id="faaf8-134">Hämta hello VHD disk toobe kopieras.</span><span class="sxs-lookup"><span data-stu-id="faaf8-134">Get hello VHD disk toobe copied.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. <span data-ttu-id="faaf8-135">Skapa hello konfigurationer av ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="faaf8-135">Create hello snapshot configurations.</span></span> 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. <span data-ttu-id="faaf8-136">Ta hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="faaf8-136">Take hello snapshot.</span></span>

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
<span data-ttu-id="faaf8-137">Om du planerar toouse hello ögonblicksbild toocreate en Disk som hanteras och koppla den till en virtuell dator som behöver toobe högpresterande, använda hello parametern `-AccountType Premium_LRS` med hello ny AzureRmSnapshot kommando.</span><span class="sxs-lookup"><span data-stu-id="faaf8-137">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="faaf8-138">hello-parametern skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="faaf8-138">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="faaf8-139">Premium-hanterade diskar är dyrare än Standard.</span><span class="sxs-lookup"><span data-stu-id="faaf8-139">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="faaf8-140">Vara säker på att du verkligen behöver Premium innan du använder parametern.</span><span class="sxs-lookup"><span data-stu-id="faaf8-140">So be sure you really need Premium before using that parameter.</span></span>


