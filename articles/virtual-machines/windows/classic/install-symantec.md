---
title: "aaaInstall Symantec Endpoint Protection på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera tillägg för hello Symantec Endpoint Protection säkerhet på en ny eller befintlig virtuell Azure-dator som skapats med hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a><span data-ttu-id="85f92-103">Hur tooinstall och konfigurera Symantec Endpoint Protection på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="85f92-103">How tooinstall and configure Symantec Endpoint Protection on a Windows VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="85f92-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="85f92-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="85f92-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="85f92-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="85f92-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="85f92-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="85f92-107">Den här artikeln beskrivs hur du tooinstall och konfigurera hello Symantec Endpoint Protection-klienten på en befintlig virtuell dator (VM) med Windows Server.</span><span class="sxs-lookup"><span data-stu-id="85f92-107">This article shows you how tooinstall and configure hello Symantec Endpoint Protection client on an existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="85f92-108">Fullständig klienten inkluderar tjänster, till exempel virus och spionprogram skydd, brandvägg och intrångsskydd.</span><span class="sxs-lookup"><span data-stu-id="85f92-108">This full client includes services such as virus and spyware protection, firewall, and intrusion prevention.</span></span> <span data-ttu-id="85f92-109">hello-klienten installeras som ett tillägg för säkerhet via hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="85f92-109">hello client is installed as a security extension by using hello VM Agent.</span></span>

<span data-ttu-id="85f92-110">Om du har en befintlig prenumeration från Symantec för en lokal lösning kan du använda den tooprotect Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="85f92-110">If you have an existing subscription from Symantec for an on-premises solution, you can use it tooprotect your Azure virtual machines.</span></span> <span data-ttu-id="85f92-111">Om du inte är en kund ännu kan registrera du dig för en utvärderingsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="85f92-111">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="85f92-112">Mer information om den här lösningen finns [Symantec Endpoint Protection på Microsoft Azure-plattformen][Symantec].</span><span class="sxs-lookup"><span data-stu-id="85f92-112">For more information about this solution, see [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec].</span></span> <span data-ttu-id="85f92-113">Sidan innehåller också länkar toolicensing information och anvisningar för att installera hello klienten om du redan en Symantec-kund.</span><span class="sxs-lookup"><span data-stu-id="85f92-113">This page also has links toolicensing information and instructions for installing hello client if you're already a Symantec customer.</span></span>

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a><span data-ttu-id="85f92-114">Installera Symantec Endpoint Protection på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="85f92-114">Install Symantec Endpoint Protection on an existing VM</span></span>
<span data-ttu-id="85f92-115">Innan du börjar behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="85f92-115">Before you begin, you need hello following:</span></span>

* <span data-ttu-id="85f92-116">hello Azure PowerShell-modulen, version 0.8.2 eller senare på datorn fungerar.</span><span class="sxs-lookup"><span data-stu-id="85f92-116">hello Azure PowerShell module, version 0.8.2 or later, on your work computer.</span></span> <span data-ttu-id="85f92-117">Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med hello **Get-Module azure | format-table version** kommando.</span><span class="sxs-lookup"><span data-stu-id="85f92-117">You can check hello version of Azure PowerShell that you have installed with hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="85f92-118">Instruktioner och en länk toohello senaste versionen finns [hur tooInstall och konfigurera Azure PowerShell][PS].</span><span class="sxs-lookup"><span data-stu-id="85f92-118">For instructions and a link toohello latest version, see [How tooInstall and Configure Azure PowerShell][PS].</span></span> <span data-ttu-id="85f92-119">Logga in tooyour Azure-prenumeration med `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="85f92-119">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="85f92-120">hello VM-agenten som körs på hello Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="85f92-120">hello VM Agent running on hello Azure Virtual Machine.</span></span>

<span data-ttu-id="85f92-121">Kontrollera först att hello VM-agenten är installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="85f92-121">First, verify that hello VM Agent is already installed on hello virtual machine.</span></span> <span data-ttu-id="85f92-122">Fyll i hello molntjänstnamnet och namn på virtuell dator och kör sedan följande kommandon i en administratörsnivå Azure PowerShell-kommandotolk hello.</span><span class="sxs-lookup"><span data-stu-id="85f92-122">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="85f92-123">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="85f92-123">Replace everything within hello quotes, including hello < and > characters.</span></span>

> [!TIP]
> <span data-ttu-id="85f92-124">Om du inte vet hello Molntjänsten och namn på virtuella datorer, kör **Get-AzureVM** toolist hello namn för alla virtuella datorer i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="85f92-124">If you don't know hello cloud service and virtual machine names, run **Get-AzureVM** toolist hello names for all virtual machines in your current subscription.</span></span>

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="85f92-125">Om hello **write-host** kommando visar **SANT**, hello VM-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="85f92-125">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="85f92-126">Om det visar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello Azure blogginlägget [Agent för VM-tillägg - del 2][Agent].</span><span class="sxs-lookup"><span data-stu-id="85f92-126">If it displays **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2][Agent].</span></span>

<span data-ttu-id="85f92-127">Om hello VM-agenten är installerad kan du köra dessa kommandon tooinstall hello Symantec Endpoint Protection-agenten.</span><span class="sxs-lookup"><span data-stu-id="85f92-127">If hello VM Agent is installed, run these commands tooinstall hello Symantec Endpoint Protection agent.</span></span>

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

<span data-ttu-id="85f92-128">tooverify som hello Symantec security tillägg har installerats och är uppdaterad:</span><span class="sxs-lookup"><span data-stu-id="85f92-128">tooverify that hello Symantec security extension has been installed and is up-to-date:</span></span>

1. <span data-ttu-id="85f92-129">Logga in toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="85f92-129">Log on toohello virtual machine.</span></span> <span data-ttu-id="85f92-130">Instruktioner finns i [hur tooLog på tooa virtuell dator kör Windows Server][Logon].</span><span class="sxs-lookup"><span data-stu-id="85f92-130">For instructions, see [How tooLog on tooa Virtual Machine Running Windows Server][Logon].</span></span>
2. <span data-ttu-id="85f92-131">Windows Server 2008 R2, klickar du på **Start > Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="85f92-131">For Windows Server 2008 R2, click **Start > Symantec Endpoint Protection**.</span></span> <span data-ttu-id="85f92-132">Windows Server 2012 eller Windows Server 2012 R2 från startskärmen hello skriver **Symantec**, och klicka sedan på **Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="85f92-132">For Windows Server 2012 or Windows Server 2012 R2, from hello start screen, type **Symantec**, and then click **Symantec Endpoint Protection**.</span></span>
3. <span data-ttu-id="85f92-133">Från hello **Status** för hello **Status Symantec Endpoint Protection** fönster, tillämpa uppdateringar eller starta om vid behov.</span><span class="sxs-lookup"><span data-stu-id="85f92-133">From hello **Status** tab of hello **Status-Symantec Endpoint Protection** window, apply updates or restart if needed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85f92-134">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="85f92-134">Additional resources</span></span>
<span data-ttu-id="85f92-135">[Hur tooLog på tooa virtuell dator kör Windows Server][Logon]</span><span class="sxs-lookup"><span data-stu-id="85f92-135">[How tooLog on tooa Virtual Machine Running Windows Server][Logon]</span></span>

<span data-ttu-id="85f92-136">[Azure VM-tillägg och funktioner][Ext]</span><span class="sxs-lookup"><span data-stu-id="85f92-136">[Azure VM Extensions and Features][Ext]</span></span>

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
