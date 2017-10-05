---
title: "Hantera dina virtuella datorer med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig kommandon som du kan använda för att automatisera aktiviteter i hantering av virtuella datorer."
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
ms.openlocfilehash: fd2df7e1029ced11974d0b832258bed2cf3bbb27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a><span data-ttu-id="ba122-103">Hantera dina virtuella datorer med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba122-103">Manage your virtual machines by using Azure PowerShell</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="ba122-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ba122-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ba122-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ba122-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="ba122-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ba122-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="ba122-107">Vanliga PowerShell-kommandon med hjälp av Resource Manager-modellen, se [här](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba122-107">For common PowerShell commands using the Resource Manager model, see [here](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="ba122-108">Många uppgifter du utför varje dag för att hantera dina virtuella datorer kan automatiseras med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="ba122-108">Many tasks you do each day to manage your VMs can be automated by using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="ba122-109">Den här artikeln får du exempel på kommandon för enklare aktiviteter och länkar till artiklar som visar kommandon för mer avancerade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="ba122-109">This article gives you example commands for simpler tasks, and links to articles that show the commands for more complex tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="ba122-110">Om du inte har installerat och konfigurerat Azure PowerShell men du kan hämta anvisningarna i artikeln [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ba122-110">If you haven't installed and configured Azure PowerShell yet, you can get instructions in the article [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
> 
> 

## <a name="how-to-use-the-example-commands"></a><span data-ttu-id="ba122-111">Hur du använder kommandona exempel</span><span class="sxs-lookup"><span data-stu-id="ba122-111">How to use the example commands</span></span>
<span data-ttu-id="ba122-112">Du måste ersätta del av texten i kommandona med text som är lämplig för din miljö.</span><span class="sxs-lookup"><span data-stu-id="ba122-112">You'll need to replace some of the text in the commands with text that's appropriate for your environment.</span></span> <span data-ttu-id="ba122-113">Den < och > symbolerna anger du måste ersätta texten.</span><span class="sxs-lookup"><span data-stu-id="ba122-113">The < and > symbols indicate text you need to replace.</span></span> <span data-ttu-id="ba122-114">När du ersätter texten, ta bort symbolerna men lämna citattecken på plats.</span><span class="sxs-lookup"><span data-stu-id="ba122-114">When you replace the text, remove the symbols but leave the quote marks in place.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="ba122-115">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba122-115">Get a VM</span></span>
<span data-ttu-id="ba122-116">Detta är en grundläggande uppgift som du ofta använder.</span><span class="sxs-lookup"><span data-stu-id="ba122-116">This is a basic task you'll use often.</span></span> <span data-ttu-id="ba122-117">Du kan använda den för att hämta information om en virtuell dator, utföra aktiviteter på en virtuell dator eller hämta utdata för att lagra i en variabel.</span><span class="sxs-lookup"><span data-stu-id="ba122-117">Use it to get information about a VM, perform tasks on a VM, or get output to store in a variable.</span></span>

<span data-ttu-id="ba122-118">För att få information om den virtuella datorn kan köra det här kommandot ersätter allt inom citattecken, inklusive den < och > tecken:</span><span class="sxs-lookup"><span data-stu-id="ba122-118">To get information about the VM, run this command, replacing everything in the quotes, including the < and > characters:</span></span>

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

<span data-ttu-id="ba122-119">Om du vill lagra utdata i en variabel $vm, kör du:</span><span class="sxs-lookup"><span data-stu-id="ba122-119">To store the output in a $vm variable, run:</span></span>

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a><span data-ttu-id="ba122-120">Logga in på en Windows-baserad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba122-120">Log on to a Windows-based VM</span></span>
<span data-ttu-id="ba122-121">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ba122-121">Run these commands:</span></span>

> [!NOTE]
> <span data-ttu-id="ba122-122">Du kan hämta namnet på den virtuella datorn och moln från visningen av den **Get-AzureVM** kommando.</span><span class="sxs-lookup"><span data-stu-id="ba122-122">You can get the virtual machine and cloud service name from the display of the **Get-AzureVM** command.</span></span>
> 
> <span data-ttu-id="ba122-123">$svcName = ”<cloud service name>” $vmName = ”<virtual machine name>” $localPath = ”< enhet och mapp plats att lagra nedladdade RDP-filen, exempel: c:\temp >” $localFile = $localPath + ”\" + $vmname +” .rdp ”Get-AzureRemoteDesktopFile - ServiceName $svcName-namnet $vmName - LocalPath $localFile-starta</span><span class="sxs-lookup"><span data-stu-id="ba122-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch</span></span>
> 
> 

## <a name="stop-a-vm"></a><span data-ttu-id="ba122-124">Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba122-124">Stop a VM</span></span>
<span data-ttu-id="ba122-125">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba122-125">Run this command:</span></span>

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> <span data-ttu-id="ba122-126">Använd den här parametern om du vill behålla det virtuella IP (VIP) för Molntjänsten om det är den sista virtuella datorn i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba122-126">Use this parameter to keep the virtual IP (VIP) of the cloud service in case it's the last VM in that cloud service.</span></span> <br><br> <span data-ttu-id="ba122-127">Om du använder parametern StayProvisioned kan faktureras du fortfarande för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ba122-127">If you use the StayProvisioned parameter, you'll still be billed for the VM.</span></span>
> 
> 

## <a name="start-a-vm"></a><span data-ttu-id="ba122-128">Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba122-128">Start a VM</span></span>
<span data-ttu-id="ba122-129">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba122-129">Run this command:</span></span>

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a><span data-ttu-id="ba122-130">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="ba122-130">Attach a data disk</span></span>
<span data-ttu-id="ba122-131">Den här uppgiften krävs några steg.</span><span class="sxs-lookup"><span data-stu-id="ba122-131">This task requires a few steps.</span></span> <span data-ttu-id="ba122-132">Först måste du använda den *** Add-AzureDataDisk *** för att lägga till disken till objektet $vm.</span><span class="sxs-lookup"><span data-stu-id="ba122-132">First, you use the ****Add-AzureDataDisk**** cmdlet to add the disk to the $vm object.</span></span> <span data-ttu-id="ba122-133">Sedan kan du använda **uppdatering AzureVM** att uppdatera konfigurationen av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ba122-133">Then, you use **Update-AzureVM** cmdlet to update the configuration of the VM.</span></span>

<span data-ttu-id="ba122-134">Du måste också bestämma om du vill koppla en ny disk eller en som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="ba122-134">You'll also need to decide whether to attach a new disk or one that contains data.</span></span> <span data-ttu-id="ba122-135">För en ny disk kommandot skapar VHD-filen och bifogar den.</span><span class="sxs-lookup"><span data-stu-id="ba122-135">For a new disk, the command creates the .vhd file and attaches it.</span></span>

<span data-ttu-id="ba122-136">Om du vill koppla en ny disk, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba122-136">To attach a new disk, run this command:</span></span>

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

<span data-ttu-id="ba122-137">Om du vill koppla en befintlig disk kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba122-137">To attach an existing data disk, run this command:</span></span>

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

<span data-ttu-id="ba122-138">Om du vill koppla datadiskar från en befintlig VHD-fil i blob storage, kör du kommandot:</span><span class="sxs-lookup"><span data-stu-id="ba122-138">To attach data disks from an existing .vhd file in blob storage, run this command:</span></span>

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a><span data-ttu-id="ba122-139">Skapa en Windows-baserad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba122-139">Create a Windows-based VM</span></span>
<span data-ttu-id="ba122-140">Om du vill skapa en ny Windows-baserad virtuell dator i Azure, följ instruktionerna i [använda Azure PowerShell för att skapa och förkonfigurera Windows-baserade virtuella datorer](create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ba122-140">To create a new Windows-based virtual machine in Azure, use the instructions in [Use Azure PowerShell to create and preconfigure Windows-based virtual machines](create-powershell.md).</span></span> <span data-ttu-id="ba122-141">Det här avsnittet steg du genom att skapa en Azure PowerShell kommandouppsättning som skapar ett Windows-baserade Virtuella kan förkonfigureras:</span><span class="sxs-lookup"><span data-stu-id="ba122-141">This topic steps you through the creation of an Azure PowerShell command set that creates a Windows-based VM that can be preconfigured:</span></span>

* <span data-ttu-id="ba122-142">Med medlemskap i Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="ba122-142">With Active Directory domain membership.</span></span>
* <span data-ttu-id="ba122-143">Med ytterligare diskar.</span><span class="sxs-lookup"><span data-stu-id="ba122-143">With additional disks.</span></span>
* <span data-ttu-id="ba122-144">Anger som en medlem av en befintlig belastningsutjämnad.</span><span class="sxs-lookup"><span data-stu-id="ba122-144">As a member of an existing load-balanced set.</span></span>
* <span data-ttu-id="ba122-145">Med en statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ba122-145">With a static IP address.</span></span>

