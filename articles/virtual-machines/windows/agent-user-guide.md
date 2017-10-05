---
title: "Översikt över Azure-dator | Microsoft Docs"
description: "Översikt över Azure-dator"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: nepeters
ms.openlocfilehash: 24ad2c2d2872f844e32d3fae559683c3d992bd00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machine-agent-overview"></a><span data-ttu-id="ecc19-103">Översikt över Azure virtuella datorns Agent</span><span class="sxs-lookup"><span data-stu-id="ecc19-103">Azure Virtual Machine Agent overview</span></span>

<span data-ttu-id="ecc19-104">Den virtuella datorns Agent för Microsoft Azure (AM Agent) är en säker, enkel process som hanterar VM interaktion med Azure-Infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-104">The Microsoft Azure Virtual Machine Agent (AM Agent) is a secured, lightweight process that manages VM interaction with the Azure Fabric Controller.</span></span> <span data-ttu-id="ecc19-105">Den Virtuella Datoragenten har en primär roll i att aktivera och köra tillägg för virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc19-105">The VM Agent has a primary role in enabling and executing Azure virtual machine extensions.</span></span> <span data-ttu-id="ecc19-106">VM-tillägg som aktiverar efter distributionskonfigurationen av virtuella datorer, till exempel installera och konfigurera programvara.</span><span class="sxs-lookup"><span data-stu-id="ecc19-106">VM Extensions enabling post deployment configuration of virtual machines, such as installing and configuring software.</span></span> <span data-ttu-id="ecc19-107">Tillägg för virtuell dator kan du även aktivera återställningsfunktioner, till exempel när du återställer lösenordet för administratörer för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ecc19-107">Virtual machine extensions also enable recovery features such as resetting the administrative password of a virtual machine.</span></span> <span data-ttu-id="ecc19-108">Tillägg för virtuell dator utan Azure VM-agenten kan inte köras.</span><span class="sxs-lookup"><span data-stu-id="ecc19-108">Without the Azure VM Agent, virtual machine extensions cannot be run.</span></span>

<span data-ttu-id="ecc19-109">Det här dokumentet beskriver installation, identifiering och borttagning av virtuella Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-109">This document details installation, detection, and removal of the Azure Virtual Machine Agent.</span></span>

## <a name="install-the-vm-agent"></a><span data-ttu-id="ecc19-110">Installera den Virtuella Datoragenten</span><span class="sxs-lookup"><span data-stu-id="ecc19-110">Install the VM Agent</span></span>

### <a name="azure-gallery-image"></a><span data-ttu-id="ecc19-111">Bild av Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="ecc19-111">Azure gallery image</span></span>

<span data-ttu-id="ecc19-112">Azure VM-agenten installeras som standard på alla Windows-dator som distribueras från en avbildning i Azure-galleriet.</span><span class="sxs-lookup"><span data-stu-id="ecc19-112">The Azure VM Agent is installed by default on any Windows virtual machine deployed from an Azure Gallery image.</span></span> <span data-ttu-id="ecc19-113">När du distribuerar en Azure-galleriet bild från portalen, PowerShell, kommandoradsgränssnittet eller en Azure Resource Manager-mall, installeras även Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-113">When deploying an Azure gallery image from the Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template, the Azure VM Agent is also be installed.</span></span> 

### <a name="manual-installation"></a><span data-ttu-id="ecc19-114">Manuell installation</span><span class="sxs-lookup"><span data-stu-id="ecc19-114">Manual installation</span></span>

<span data-ttu-id="ecc19-115">Windows VM-agenten kan installeras manuellt med hjälp av Windows installer-paket.</span><span class="sxs-lookup"><span data-stu-id="ecc19-115">The Windows VM agent can be manually installed using a Windows installer package.</span></span> <span data-ttu-id="ecc19-116">Manuell installation kan vara nödvändigt när du skapar en virtuell datoravbildning som ska distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc19-116">Manual installation may be necessary when creating a custom virtual machine image that will be deployed in Azure.</span></span> <span data-ttu-id="ecc19-117">Installera Windows VM-agenten manuellt genom att hämta installationsprogrammet för VM-agenten från den här platsen [Windows Azure VM-agenten hämta](http://go.microsoft.com/fwlink/?LinkID=394789).</span><span class="sxs-lookup"><span data-stu-id="ecc19-117">To manually install the Windows VM Agent, download the VM Agent installer from this location [Windows Azure VM Agent Download](http://go.microsoft.com/fwlink/?LinkID=394789).</span></span> 

<span data-ttu-id="ecc19-118">VM-agenten kan installeras genom att dubbelklicka på den windows installer-fil.</span><span class="sxs-lookup"><span data-stu-id="ecc19-118">The VM Agent can be installed by double-clicking the windows installer file.</span></span> <span data-ttu-id="ecc19-119">Kör följande kommando för en automatiserad eller obevakad installation av VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-119">For an automated or unattended installation of the VM agent, run the following command.</span></span>

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a><span data-ttu-id="ecc19-120">Identifiera den Virtuella Datoragenten</span><span class="sxs-lookup"><span data-stu-id="ecc19-120">Detect the VM Agent</span></span>

### <a name="powershell"></a><span data-ttu-id="ecc19-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecc19-121">PowerShell</span></span>

<span data-ttu-id="ecc19-122">Azure Resource Manager PowerShell-modulen kan användas för att hämta information om Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ecc19-122">The Azure Resource Manager PowerShell module can be used to retrieve information about Azure Virtual Machines.</span></span> <span data-ttu-id="ecc19-123">Kör `Get-AzureRmVM` returnerar lite inklusive etableringsstatusen för Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-123">Running `Get-AzureRmVM` returns quite a bit of information including the provisioning state for the Azure VM Agent.</span></span>

```PowerShell
Get-AzureRmVM
```

<span data-ttu-id="ecc19-124">Följande är bara en delmängd av den `Get-AzureRmVM` utdata.</span><span class="sxs-lookup"><span data-stu-id="ecc19-124">The following is just a subset of the `Get-AzureRmVM` output.</span></span> <span data-ttu-id="ecc19-125">Observera den `ProvisionVMAgent` egenskapen inuti `OSProfile`, den här egenskapen kan användas för att avgöra om den Virtuella datoragenten har distribuerats till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecc19-125">Notice the `ProvisionVMAgent` property nested inside `OSProfile`, this property can be used to determine if the VM agent has been deployed to the virtual machine.</span></span>

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

<span data-ttu-id="ecc19-126">Följande skript kan användas för att returnera en kortfattad lista över namn på virtuella datorer och tillståndet för den Virtuella Datoragenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-126">The following script can be used to return a concise list of virtual machine names and the state of the VM Agent.</span></span>

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a><span data-ttu-id="ecc19-127">Manuell identifiering</span><span class="sxs-lookup"><span data-stu-id="ecc19-127">Manual Detection</span></span>

<span data-ttu-id="ecc19-128">När inloggad på en Windows Azure VM, kan du använda Aktivitetshanteraren för att granska processer som körs.</span><span class="sxs-lookup"><span data-stu-id="ecc19-128">When logged in to a Windows Azure VM, task manager can be used to examine running processes.</span></span> <span data-ttu-id="ecc19-129">För att kontrollera Azure VM-agenten, öppna Aktivitetshanteraren > Klicka på informationsfliken och leta efter ett processnamn `WindowsAzureGuestAgent.exe`.</span><span class="sxs-lookup"><span data-stu-id="ecc19-129">To check for the Azure VM Agent, open Task Manager > click the details tab, and look for a process name `WindowsAzureGuestAgent.exe`.</span></span> <span data-ttu-id="ecc19-130">Förekomst av den här processen anger att den Virtuella datoragenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="ecc19-130">The presence of this process indicates that the VM agent is installed.</span></span>

## <a name="upgrade-the-vm-agent"></a><span data-ttu-id="ecc19-131">Uppgradera den Virtuella Datoragenten</span><span class="sxs-lookup"><span data-stu-id="ecc19-131">Upgrade the VM Agent</span></span>

<span data-ttu-id="ecc19-132">Azure VM-agenten för Windows uppgraderas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ecc19-132">The Azure VM Agent for Windows is automatically upgraded.</span></span> <span data-ttu-id="ecc19-133">Allteftersom nya virtuella datorer distribueras till Azure, kan de få den senaste VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="ecc19-133">As new virtual machines are deployed to Azure, they receive the latest VM agent.</span></span> <span data-ttu-id="ecc19-134">Anpassade VM-avbildningar ska uppdateras för att inkludera den nya VM-agenten manuellt.</span><span class="sxs-lookup"><span data-stu-id="ecc19-134">Custom VM images should be manually updated to include the new VM agent.</span></span>