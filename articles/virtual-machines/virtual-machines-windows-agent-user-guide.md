---
title: "aaaAzure översikt över virtuella | Microsoft Docs"
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
ms.date: 03/28/2017
ms.author: nepeters
ms.openlocfilehash: 178766925673419cd661dbb460b8427bbfaf54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a><span data-ttu-id="d2015-103">Översikt över Azure virtuella datorns Agent</span><span class="sxs-lookup"><span data-stu-id="d2015-103">Azure Virtual Machine Agent overview</span></span>

<span data-ttu-id="d2015-104">hello virtuella Microsoft Azure-agenten (VM-Agent) är en säker, enkel process som hanterar VM interaktion med hello Azure-Infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="d2015-104">hello Microsoft Azure Virtual Machine Agent (VM Agent) is a secured, lightweight process that manages VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="d2015-105">hello VM-agenten har en primär roll i att aktivera och köra tillägg för virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2015-105">hello VM Agent has a primary role in enabling and executing Azure virtual machine extensions.</span></span> <span data-ttu-id="d2015-106">VM-tillägg som aktiverar efter distributionskonfigurationen av virtuella datorer, till exempel installera och konfigurera programvara.</span><span class="sxs-lookup"><span data-stu-id="d2015-106">VM Extensions enabling post deployment configuration of virtual machines, such as installing and configuring software.</span></span> <span data-ttu-id="d2015-107">Tillägg för virtuell dator kan du även aktivera återställningsfunktioner, till exempel återställer hello administratörslösenord för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2015-107">Virtual machine extensions also enable recovery features such as resetting hello administrative password of a virtual machine.</span></span> <span data-ttu-id="d2015-108">Tillägg för virtuell dator utan hello Azure VM-agenten kan inte köras.</span><span class="sxs-lookup"><span data-stu-id="d2015-108">Without hello Azure VM Agent, virtual machine extensions cannot be run.</span></span>

<span data-ttu-id="d2015-109">Det här dokumentet beskriver installation, identifiering och borttagning av hello Azure Virtual Machine-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-109">This document details installation, detection, and removal of hello Azure Virtual Machine Agent.</span></span>

## <a name="install-hello-vm-agent"></a><span data-ttu-id="d2015-110">Installera hello VM-Agent</span><span class="sxs-lookup"><span data-stu-id="d2015-110">Install hello VM Agent</span></span>

### <a name="azure-gallery-image"></a><span data-ttu-id="d2015-111">Bild av Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="d2015-111">Azure gallery image</span></span>

<span data-ttu-id="d2015-112">hello Azure VM-agenten installeras som standard på alla Windows-dator som distribueras från en avbildning i Azure-galleriet.</span><span class="sxs-lookup"><span data-stu-id="d2015-112">hello Azure VM Agent is installed by default on any Windows virtual machine deployed from an Azure Gallery image.</span></span> <span data-ttu-id="d2015-113">När du distribuerar en Azure-galleriet bild från hello Portal, PowerShell, kommandoradsgränssnittet eller en Azure Resource Manager-mall installeras hello Azure VM-agenten är också.</span><span class="sxs-lookup"><span data-stu-id="d2015-113">When deploying an Azure gallery image from hello Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template, hello Azure VM Agent is also be installed.</span></span> 

### <a name="manual-installation"></a><span data-ttu-id="d2015-114">Manuell installation</span><span class="sxs-lookup"><span data-stu-id="d2015-114">Manual installation</span></span>

<span data-ttu-id="d2015-115">hello Windows VM-agenten kan installeras manuellt med hjälp av Windows installer-paket.</span><span class="sxs-lookup"><span data-stu-id="d2015-115">hello Windows VM agent can be manually installed using a Windows installer package.</span></span> <span data-ttu-id="d2015-116">Manuell installation kan vara nödvändigt när du skapar en virtuell datoravbildning som ska distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2015-116">Manual installation may be necessary when creating a custom virtual machine image that will be deployed in Azure.</span></span> <span data-ttu-id="d2015-117">toomanually installera hello Windows VM-agenten, hämta installationsprogrammet för hello VM-agenten från den här platsen [Windows Azure VM-agenten hämta](http://go.microsoft.com/fwlink/?LinkID=394789).</span><span class="sxs-lookup"><span data-stu-id="d2015-117">toomanually install hello Windows VM Agent, download hello VM Agent installer from this location [Windows Azure VM Agent Download](http://go.microsoft.com/fwlink/?LinkID=394789).</span></span> 

<span data-ttu-id="d2015-118">hello VM-agenten kan installeras genom att dubbelklicka på hello windows installer-filen.</span><span class="sxs-lookup"><span data-stu-id="d2015-118">hello VM Agent can be installed by double-clicking hello windows installer file.</span></span> <span data-ttu-id="d2015-119">Kör hello följande kommando för en automatiserad eller obevakad installation av hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-119">For an automated or unattended installation of hello VM agent, run hello following command.</span></span>

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a><span data-ttu-id="d2015-120">Identifiera hello VM-Agent</span><span class="sxs-lookup"><span data-stu-id="d2015-120">Detect hello VM Agent</span></span>

### <a name="powershell"></a><span data-ttu-id="d2015-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2015-121">PowerShell</span></span>

<span data-ttu-id="d2015-122">hello Azure Resource Manager PowerShell-modulen kan vara används tooretrieve information om Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2015-122">hello Azure Resource Manager PowerShell module can be used tooretrieve information about Azure Virtual Machines.</span></span> <span data-ttu-id="d2015-123">Kör `Get-AzureRmVM` returnerar lite inklusive hello Etableringsstatus för hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-123">Running `Get-AzureRmVM` returns quite a bit of information including hello provisioning state for hello Azure VM Agent.</span></span>

```PowerShell
Get-AzureRmVM
```

<span data-ttu-id="d2015-124">hello följande är bara en delmängd av hello `Get-AzureRmVM` utdata.</span><span class="sxs-lookup"><span data-stu-id="d2015-124">hello following is just a subset of hello `Get-AzureRmVM` output.</span></span> <span data-ttu-id="d2015-125">Meddelande hello `ProvisionVMAgent` egenskapen inuti `OSProfile`, den här egenskapen kan vara används toodetermine om hello VM-agenten har distribuerade toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d2015-125">Notice hello `ProvisionVMAgent` property nested inside `OSProfile`, this property can be used toodetermine if hello VM agent has been deployed toohello virtual machine.</span></span>

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

<span data-ttu-id="d2015-126">Följande skript hello kan vara används tooreturn en kortfattad lista över namn på virtuella datorer och hello tillståndet för hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-126">hello following script can be used tooreturn a concise list of virtual machine names and hello state of hello VM Agent.</span></span>

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a><span data-ttu-id="d2015-127">Manuell identifiering</span><span class="sxs-lookup"><span data-stu-id="d2015-127">Manual Detection</span></span>

<span data-ttu-id="d2015-128">När du loggade in tooa Windows Azure VM vara Aktivitetshanteraren används tooexamine processer som körs.</span><span class="sxs-lookup"><span data-stu-id="d2015-128">When logged in tooa Windows Azure VM, task manager can be used tooexamine running processes.</span></span> <span data-ttu-id="d2015-129">toocheck för hello Azure VM-agenten, öppna Aktivitetshanteraren > Klicka på fliken för hello information och leta efter ett processnamn `WindowsAzureGuestAgent.exe`.</span><span class="sxs-lookup"><span data-stu-id="d2015-129">toocheck for hello Azure VM Agent, open Task Manager > click hello details tab, and look for a process name `WindowsAzureGuestAgent.exe`.</span></span> <span data-ttu-id="d2015-130">hello förekomst av den här processen anger den hello VM-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="d2015-130">hello presence of this process indicates that hello VM agent is installed.</span></span>

## <a name="upgrade-hello-vm-agent"></a><span data-ttu-id="d2015-131">Uppgradera hello VM-Agent</span><span class="sxs-lookup"><span data-stu-id="d2015-131">Upgrade hello VM Agent</span></span>

<span data-ttu-id="d2015-132">hello Azure VM-agenten för Windows uppgraderas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d2015-132">hello Azure VM Agent for Windows is automatically upgraded.</span></span> <span data-ttu-id="d2015-133">Nya virtuella datorer är distribuerade tooAzure, kan de få hello senaste VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-133">As new virtual machines are deployed tooAzure, they receive hello latest VM agent.</span></span> <span data-ttu-id="d2015-134">Anpassade VM-avbildningar ska vara uppdaterade tooinclude hello nya VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="d2015-134">Custom VM images should be manually updated tooinclude hello new VM agent.</span></span>
