---
title: "aaaManage dina virtuella datorer med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig kommandon som du kan använda tooautomate uppgifter vid hantering av virtuella datorer."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a><span data-ttu-id="973e2-103">Hantera dina virtuella datorer med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="973e2-103">Manage your virtual machines by using Azure PowerShell</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="973e2-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="973e2-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="973e2-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="973e2-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="973e2-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="973e2-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="973e2-107">Vanliga PowerShell-kommandon med hjälp av hello Resource Manager-modellen, se [här](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="973e2-107">For common PowerShell commands using hello Resource Manager model, see [here](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="973e2-108">Många uppgifter du utför varje dag toomanage dina virtuella datorer kan automatiseras med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="973e2-108">Many tasks you do each day toomanage your VMs can be automated by using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="973e2-109">Den här artikeln innehåller exempel på kommandon för enklare aktiviteter och länkar tooarticles som visar hello-kommandon för mer avancerade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="973e2-109">This article gives you example commands for simpler tasks, and links tooarticles that show hello commands for more complex tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="973e2-110">Om du inte har installerat och konfigurerat Azure PowerShell men du kan hämta instruktioner i hello artikel [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="973e2-110">If you haven't installed and configured Azure PowerShell yet, you can get instructions in hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
> 
> 

## <a name="how-toouse-hello-example-commands"></a><span data-ttu-id="973e2-111">Hur toouse hello exempelkommandon</span><span class="sxs-lookup"><span data-stu-id="973e2-111">How toouse hello example commands</span></span>
<span data-ttu-id="973e2-112">Du behöver tooreplace vissa hello text i hello kommandon med text som är lämpliga för din miljö.</span><span class="sxs-lookup"><span data-stu-id="973e2-112">You'll need tooreplace some of hello text in hello commands with text that's appropriate for your environment.</span></span> <span data-ttu-id="973e2-113">Ange text som du behöver tooreplace Hej < och > symboler.</span><span class="sxs-lookup"><span data-stu-id="973e2-113">hello < and > symbols indicate text you need tooreplace.</span></span> <span data-ttu-id="973e2-114">När du ersätter hello text, ta bort hello symboler men lämna hello citattecken på plats.</span><span class="sxs-lookup"><span data-stu-id="973e2-114">When you replace hello text, remove hello symbols but leave hello quote marks in place.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="973e2-115">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="973e2-115">Get a VM</span></span>
<span data-ttu-id="973e2-116">Detta är en grundläggande uppgift som du ofta använder.</span><span class="sxs-lookup"><span data-stu-id="973e2-116">This is a basic task you'll use often.</span></span> <span data-ttu-id="973e2-117">Använd den tooget information om en virtuell dator, utföra aktiviteter på en virtuell dator eller hämta utdata toostore i en variabel.</span><span class="sxs-lookup"><span data-stu-id="973e2-117">Use it tooget information about a VM, perform tasks on a VM, or get output toostore in a variable.</span></span>

<span data-ttu-id="973e2-118">tooget information om hello VM, kör detta kommando ersätter allt i hello citattecken, inklusive Hej < och > tecken:</span><span class="sxs-lookup"><span data-stu-id="973e2-118">tooget information about hello VM, run this command, replacing everything in hello quotes, including hello < and > characters:</span></span>

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

<span data-ttu-id="973e2-119">toostore hello utdata i en variabel i $vm, kör:</span><span class="sxs-lookup"><span data-stu-id="973e2-119">toostore hello output in a $vm variable, run:</span></span>

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a><span data-ttu-id="973e2-120">Logga in tooa Windows-baserade Virtuella</span><span class="sxs-lookup"><span data-stu-id="973e2-120">Log on tooa Windows-based VM</span></span>
<span data-ttu-id="973e2-121">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="973e2-121">Run these commands:</span></span>

> [!NOTE]
> <span data-ttu-id="973e2-122">Du kan hämta hello virtuell dator och molntjänstnamnet från hello visningen av hello **Get-AzureVM** kommando.</span><span class="sxs-lookup"><span data-stu-id="973e2-122">You can get hello virtual machine and cloud service name from hello display of hello **Get-AzureVM** command.</span></span>
> 
> <span data-ttu-id="973e2-123">$svcName = ”<cloud service name>” $vmName = ”<virtual machine name>” $localPath = ”< enhet och mapp plats toostore hello ned RDP-filen, exempel: c:\temp >” $localFile = $localPath + ”\" + $vmname +” .rdp ”Get-AzureRemoteDesktopFile - ServiceName $svcName-Name $vmName - LocalPath $localFile-starta</span><span class="sxs-lookup"><span data-stu-id="973e2-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<drive and folder location toostore hello downloaded RDP file, example: c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch</span></span>
> 
> 

## <a name="stop-a-vm"></a><span data-ttu-id="973e2-124">Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="973e2-124">Stop a VM</span></span>
<span data-ttu-id="973e2-125">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="973e2-125">Run this command:</span></span>

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> <span data-ttu-id="973e2-126">Använd den här parametern tookeep hello virtuella IP (VIP) för hello moln tjänsten om det är hello senaste VM i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="973e2-126">Use this parameter tookeep hello virtual IP (VIP) of hello cloud service in case it's hello last VM in that cloud service.</span></span> <br><br> <span data-ttu-id="973e2-127">Om du använder hello StayProvisioned parameter kan faktureras du fortfarande för hello VM.</span><span class="sxs-lookup"><span data-stu-id="973e2-127">If you use hello StayProvisioned parameter, you'll still be billed for hello VM.</span></span>
> 
> 

## <a name="start-a-vm"></a><span data-ttu-id="973e2-128">Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="973e2-128">Start a VM</span></span>
<span data-ttu-id="973e2-129">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="973e2-129">Run this command:</span></span>

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a><span data-ttu-id="973e2-130">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="973e2-130">Attach a data disk</span></span>
<span data-ttu-id="973e2-131">Den här uppgiften krävs några steg.</span><span class="sxs-lookup"><span data-stu-id="973e2-131">This task requires a few steps.</span></span> <span data-ttu-id="973e2-132">Använd först hello *** Add-AzureDataDisk *** cmdlet tooadd hello toohello $vm diskobjektet.</span><span class="sxs-lookup"><span data-stu-id="973e2-132">First, you use hello ****Add-AzureDataDisk**** cmdlet tooadd hello disk toohello $vm object.</span></span> <span data-ttu-id="973e2-133">Sedan kan du använda **uppdatering AzureVM** cmdlet tooupdate hello konfigurationen av hello VM.</span><span class="sxs-lookup"><span data-stu-id="973e2-133">Then, you use **Update-AzureVM** cmdlet tooupdate hello configuration of hello VM.</span></span>

<span data-ttu-id="973e2-134">Du måste också toodecide om tooattach en ny disk eller en som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="973e2-134">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="973e2-135">För en ny disk hello skapar hello VHD-filen och bifogas.</span><span class="sxs-lookup"><span data-stu-id="973e2-135">For a new disk, hello command creates hello .vhd file and attaches it.</span></span>

<span data-ttu-id="973e2-136">tooattach kör det här kommandot för en ny disk:</span><span class="sxs-lookup"><span data-stu-id="973e2-136">tooattach a new disk, run this command:</span></span>

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

<span data-ttu-id="973e2-137">tooattach en befintlig datadisk kör detta kommando:</span><span class="sxs-lookup"><span data-stu-id="973e2-137">tooattach an existing data disk, run this command:</span></span>

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

<span data-ttu-id="973e2-138">tooattach datadiskar från en befintlig VHD-fil i blob storage kör detta kommando:</span><span class="sxs-lookup"><span data-stu-id="973e2-138">tooattach data disks from an existing .vhd file in blob storage, run this command:</span></span>

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a><span data-ttu-id="973e2-139">Skapa en Windows-baserad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="973e2-139">Create a Windows-based VM</span></span>
<span data-ttu-id="973e2-140">toocreate en ny Windows-baserad virtuell dator i Azure, Använd hello instruktionerna i [använda Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="973e2-140">toocreate a new Windows-based virtual machine in Azure, use hello instructions in [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](create-powershell.md).</span></span> <span data-ttu-id="973e2-141">Det här avsnittet steg du via hello skapandet av en Azure PowerShell kommandouppsättning som skapar ett Windows-baserade Virtuella kan förkonfigureras:</span><span class="sxs-lookup"><span data-stu-id="973e2-141">This topic steps you through hello creation of an Azure PowerShell command set that creates a Windows-based VM that can be preconfigured:</span></span>

* <span data-ttu-id="973e2-142">Med medlemskap i Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="973e2-142">With Active Directory domain membership.</span></span>
* <span data-ttu-id="973e2-143">Med ytterligare diskar.</span><span class="sxs-lookup"><span data-stu-id="973e2-143">With additional disks.</span></span>
* <span data-ttu-id="973e2-144">Anger som en medlem av en befintlig belastningsutjämnad.</span><span class="sxs-lookup"><span data-stu-id="973e2-144">As a member of an existing load-balanced set.</span></span>
* <span data-ttu-id="973e2-145">Med en statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="973e2-145">With a static IP address.</span></span>

