---
title: aaaManage HPC Pack klustret datornoder | Microsoft Docs
description: "Lär dig mer om PowerShell-skript, verktyg tooadd, ta bort, starta och stoppa HPC Pack 2012 R2-kluster compute-noder i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="a7ee2-103">Hantera hello antal och tillgängligheten för compute-noder i ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="a7ee2-103">Manage hello number and availability of compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="a7ee2-104">Om du har skapat ett HPC Pack 2012 R2-kluster i virtuella Azure-datorer, kanske du vill sätt tooeasily lägga till, ta bort, starta (tillhandahålla) eller stoppa (avetablering) vissa compute-nod virtuella datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-104">If you created an HPC Pack 2012 R2 cluster in Azure VMs, you might want ways tooeasily add, remove, start (provision), or stop (deprovision) some compute node VMs in the cluster.</span></span> <span data-ttu-id="a7ee2-105">toodo dessa uppgifter köra Azure PowerShell-skript som är installerade på hello huvudnod VM.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-105">toodo these tasks, run Azure PowerShell scripts that are installed on hello head node VM.</span></span> <span data-ttu-id="a7ee2-106">Dessa skript som hjälper dig styra hello antal och tillgängligheten för din HPC Pack klusterresurser så du kan styra kostnader.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-106">These scripts help you control hello number and availability of your HPC Pack cluster resources so you can control costs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="a7ee2-107">Den här artikeln gäller bara tooHPC Pack 2012 R2-kluster i Azure som skapats med hjälp av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-107">This article applies only tooHPC Pack 2012 R2 clusters in Azure created using hello classic deployment model.</span></span> <span data-ttu-id="a7ee2-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="a7ee2-109">Dessutom är hello PowerShell-skript som beskrivs i den här artikeln inte tillgängliga i HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-109">In addition, hello PowerShell scripts described in this article are not available in HPC Pack 2016.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7ee2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a7ee2-110">Prerequisites</span></span>
* <span data-ttu-id="a7ee2-111">**HPC Pack 2012 R2-kluster i Azure Virtual Machines**: skapa ett HPC Pack 2012 R2-kluster i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-111">**HPC Pack 2012 R2 cluster in Azure VMs**: Create an HPC Pack 2012 R2 cluster in hello classic deployment model.</span></span> <span data-ttu-id="a7ee2-112">Exempelvis kan du automatisera hello distribution med hjälp av hello HPC Pack 2012 R2 VM-avbildning i hello Azure Marketplace och ett Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-112">For example, you can automate hello deployment by using hello HPC Pack 2012 R2 VM image in hello Azure Marketplace and an Azure PowerShell script.</span></span> <span data-ttu-id="a7ee2-113">Information och krav finns i [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="a7ee2-113">For information and prerequisites, see [Create an HPC Cluster with hello HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md).</span></span>
  
    <span data-ttu-id="a7ee2-114">Efter distributionen kan hitta hello nod hanteringsskript i hello % CCP\_hem % bin-mappen i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-114">After deployment, find hello node management scripts in hello %CCP\_HOME%bin folder on hello head node.</span></span> <span data-ttu-id="a7ee2-115">Kör alla hello skript som administratör.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-115">Run each of hello scripts as an administrator.</span></span>
* <span data-ttu-id="a7ee2-116">**Azure publicera certifikat för filen eller hantering av inställningar**: du behöver toodo något av följande hello i hello huvudnod:</span><span class="sxs-lookup"><span data-stu-id="a7ee2-116">**Azure publish settings file or management certificate**: You need toodo one of hello following on hello head node:</span></span>
  
  * <span data-ttu-id="a7ee2-117">**Importera hello Azure inställningsfilen för publicering**.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-117">**Import hello Azure publish settings file**.</span></span> <span data-ttu-id="a7ee2-118">toodo detta, kör hello följande Azure PowerShell-cmdlets i hello huvudnod:</span><span class="sxs-lookup"><span data-stu-id="a7ee2-118">toodo this, run hello following Azure PowerShell cmdlets on hello head node:</span></span>
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * <span data-ttu-id="a7ee2-119">**Konfigurera hello Azure-hanteringscertifikatet på hello huvudnod**.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-119">**Configure hello Azure management certificate on hello head node**.</span></span> <span data-ttu-id="a7ee2-120">Om du har hello .cer-fil, importera det i certifikatarkivet för hello CurrentUser\My och kör sedan hello följande Azure PowerShell-cmdleten för Azure-miljön (AzureCloud eller AzureChinaCloud):</span><span class="sxs-lookup"><span data-stu-id="a7ee2-120">If you have hello .cer file, import it in hello CurrentUser\My certificate store and then run hello following Azure PowerShell cmdlet for your Azure environment (either AzureCloud or AzureChinaCloud):</span></span>
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a><span data-ttu-id="a7ee2-121">Lägg till Beräkningsnoden virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a7ee2-121">Add compute node VMs</span></span>
<span data-ttu-id="a7ee2-122">Lägg till compute-noder med hello **Lägg till HpcIaaSNode.ps1** skript.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-122">Add compute nodes with hello **Add-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="a7ee2-123">Syntax</span><span class="sxs-lookup"><span data-stu-id="a7ee2-123">Syntax</span></span>
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a><span data-ttu-id="a7ee2-124">Parametrar</span><span class="sxs-lookup"><span data-stu-id="a7ee2-124">Parameters</span></span>
* <span data-ttu-id="a7ee2-125">**ServiceName**: namnet på hello molntjänst som nya compute-nod virtuella datorer läggs till.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-125">**ServiceName**: Name of hello cloud service that new compute node VMs are added to.</span></span>
* <span data-ttu-id="a7ee2-126">**Avbildning**: Azure VM avbildningens namn, som kan hämtas via hello klassiska Azure-portalen eller Azure PowerShell-cmdleten **Get-AzureVMImage**.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-126">**ImageName**: Azure VM image name, which can be obtained through hello Azure classic portal or Azure PowerShell cmdlet **Get-AzureVMImage**.</span></span> <span data-ttu-id="a7ee2-127">hello avbildningen måste uppfylla följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="a7ee2-127">hello image must meet hello following requirements:</span></span>
  
  1. <span data-ttu-id="a7ee2-128">Ett Windows-operativsystem installeras.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-128">A Windows operating system must be installed.</span></span>
  2. <span data-ttu-id="a7ee2-129">HPC Pack måste vara installerad i hello beräkningsrollen nod.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-129">HPC Pack must be installed in hello compute node role.</span></span>
  3. <span data-ttu-id="a7ee2-130">hello bilden måste vara en privat avbildning i hello användarkategorin, inte en offentlig Azure VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-130">hello image must be a private image in hello User category, not a public Azure VM image.</span></span>
* <span data-ttu-id="a7ee2-131">**Antal**: antal compute-nod VMs toobe lagts till.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-131">**Quantity**: Number of compute node VMs toobe added.</span></span>
* <span data-ttu-id="a7ee2-132">**InstanceSize**: storleken på hello compute-nod virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-132">**InstanceSize**: Size of hello compute node VMs.</span></span>
* <span data-ttu-id="a7ee2-133">**DomainUserName**: användaren domännamn som är används toojoin hello ny VMs toohello domän.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-133">**DomainUserName**: Domain user name, which is used toojoin hello new VMs toohello domain.</span></span>
* <span data-ttu-id="a7ee2-134">**DomainUserPassword**: lösenordet för hello domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-134">**DomainUserPassword**: Password of hello domain user.</span></span>
* <span data-ttu-id="a7ee2-135">**NodeNameSeries** (valfritt): namnge mönster för hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-135">**NodeNameSeries** (optional): Naming pattern for hello compute nodes.</span></span> <span data-ttu-id="a7ee2-136">hello formatet måste vara &lt; *rot\_namn*&gt;&lt;*starta\_nummer*&gt;%.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-136">hello format must be &lt;*Root\_Name*&gt;&lt;*Start\_Number*&gt;%.</span></span> <span data-ttu-id="a7ee2-137">Till exempel beräkna MyCN % 10%: en serie hello nodnamn från MyCN11.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-137">For example, MyCN%10% means a series of hello compute node names starting from MyCN11.</span></span> <span data-ttu-id="a7ee2-138">Om inget anges konfigurerats hello skriptet använder hello nod namngivning serien i hello HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-138">If not specified, hello script uses hello configured node naming series in hello HPC cluster.</span></span>

### <a name="example"></a><span data-ttu-id="a7ee2-139">Exempel</span><span class="sxs-lookup"><span data-stu-id="a7ee2-139">Example</span></span>
<span data-ttu-id="a7ee2-140">hello följande exempel lägger till 20 storlek stora beräkningsnod virtuella datorer i hello Molntjänsten *hpcservice1*, baserat på hello VM-avbildning *hpccnimage1*.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-140">hello following example adds 20 size Large compute node VMs in hello cloud service *hpcservice1*, based on hello VM image *hpccnimage1*.</span></span>

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a><span data-ttu-id="a7ee2-141">Ta bort beräkningsnod virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a7ee2-141">Remove compute node VMs</span></span>
<span data-ttu-id="a7ee2-142">Ta bort compute-noder med hello **ta bort HpcIaaSNode.ps1** skript.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-142">Remove compute nodes with hello **Remove-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="a7ee2-143">Syntax</span><span class="sxs-lookup"><span data-stu-id="a7ee2-143">Syntax</span></span>
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="a7ee2-144">Parametrar</span><span class="sxs-lookup"><span data-stu-id="a7ee2-144">Parameters</span></span>
* <span data-ttu-id="a7ee2-145">**Namnet**: namn på klustret noder toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-145">**Name**: Names of cluster nodes toobe removed.</span></span> <span data-ttu-id="a7ee2-146">Jokertecken stöds.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-146">Wildcards are supported.</span></span> <span data-ttu-id="a7ee2-147">hello parameternamn är namn.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-147">hello parameter set name is Name.</span></span> <span data-ttu-id="a7ee2-148">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-148">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="a7ee2-149">**Noden**: hello HpcNode objekt för hello noder toobe tas bort, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7ee2-149">**Node**: hello HpcNode object for hello nodes toobe removed, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="a7ee2-150">hello parameternamn är noden.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-150">hello parameter set name is Node.</span></span> <span data-ttu-id="a7ee2-151">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-151">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="a7ee2-152">**DeleteVHD** (valfritt): Ange toodelete hello associerade diskar för hello virtuella datorer som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-152">**DeleteVHD** (optional): Setting toodelete hello associated disks for hello VMs that are removed.</span></span>
* <span data-ttu-id="a7ee2-153">**Framtvinga** (valfritt): Ange tooforce HPC noder offline innan du tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-153">**Force** (optional): Setting tooforce HPC nodes offline before removing them.</span></span>
* <span data-ttu-id="a7ee2-154">**Bekräfta** (valfritt): fråga efter bekräftelse innan hello kommando körs.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-154">**Confirm** (optional): Prompt for confirmation before executing hello command.</span></span>
* <span data-ttu-id="a7ee2-155">**WhatIf**: Ange toodescribe vad som skulle hända om du körde kommandot hello utan verkligen körs hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-155">**WhatIf**: Setting toodescribe what would happen if you executed hello command without actually executing hello command.</span></span>

### <a name="example"></a><span data-ttu-id="a7ee2-156">Exempel</span><span class="sxs-lookup"><span data-stu-id="a7ee2-156">Example</span></span>
<span data-ttu-id="a7ee2-157">hello följande exempel tvingar offline hello noder med namn som börjar *HPCNode-CN -* och de tar bort hello noder och deras associerade diskar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-157">hello following example forces offline hello nodes with names beginning *HPCNode-CN-* and them removes hello nodes and their associated disks.</span></span>

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a><span data-ttu-id="a7ee2-158">Starta beräkningsnod virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a7ee2-158">Start compute node VMs</span></span>
<span data-ttu-id="a7ee2-159">Starta compute-noder med hello **Start HpcIaaSNode.ps1** skript.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-159">Start compute nodes with hello **Start-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="a7ee2-160">Syntax</span><span class="sxs-lookup"><span data-stu-id="a7ee2-160">Syntax</span></span>
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="a7ee2-161">Parametrar</span><span class="sxs-lookup"><span data-stu-id="a7ee2-161">Parameters</span></span>
* <span data-ttu-id="a7ee2-162">**Namnet**: namnen på hello klustret noder toobe igång.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-162">**Name**: Names of hello cluster nodes toobe started.</span></span> <span data-ttu-id="a7ee2-163">Jokertecken stöds.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-163">Wildcards are supported.</span></span> <span data-ttu-id="a7ee2-164">hello parameternamn är namn.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-164">hello parameter set name is Name.</span></span> <span data-ttu-id="a7ee2-165">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-165">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="a7ee2-166">**Noden**-hello HpcNode objekt för hello noder toobe igång, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7ee2-166">**Node**- hello HpcNode object for hello nodes toobe started, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="a7ee2-167">hello parameternamn är noden.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-167">hello parameter set name is Node.</span></span> <span data-ttu-id="a7ee2-168">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-168">You cannot specify both hello **Name** and **Node** parameters.</span></span>

### <a name="example"></a><span data-ttu-id="a7ee2-169">Exempel</span><span class="sxs-lookup"><span data-stu-id="a7ee2-169">Example</span></span>
<span data-ttu-id="a7ee2-170">hello följande exempel startar noder med namn som börjar *HPCNode-CN -*.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-170">hello following example starts nodes with names beginning *HPCNode-CN-*.</span></span>

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a><span data-ttu-id="a7ee2-171">Stoppa beräkningsnod virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a7ee2-171">Stop compute node VMs</span></span>
<span data-ttu-id="a7ee2-172">Stoppa compute-noder med hello **stoppa HpcIaaSNode.ps1** skript.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-172">Stop compute nodes with hello **Stop-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="a7ee2-173">Syntax</span><span class="sxs-lookup"><span data-stu-id="a7ee2-173">Syntax</span></span>
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="a7ee2-174">Parametrar</span><span class="sxs-lookup"><span data-stu-id="a7ee2-174">Parameters</span></span>
* <span data-ttu-id="a7ee2-175">**Namnet**-namnen på hello klustret noder toobe stoppades.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-175">**Name**- Names of hello cluster nodes toobe stopped.</span></span> <span data-ttu-id="a7ee2-176">Jokertecken stöds.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-176">Wildcards are supported.</span></span> <span data-ttu-id="a7ee2-177">hello parameternamn är namn.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-177">hello parameter set name is Name.</span></span> <span data-ttu-id="a7ee2-178">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-178">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="a7ee2-179">**Noden**: hello HpcNode objekt för hello noder toobe stoppas, som kan hämtas via hello HPC PowerShell-cmdleten [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7ee2-179">**Node**: hello HpcNode object for hello nodes toobe stopped, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="a7ee2-180">hello parameternamn är noden.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-180">hello parameter set name is Node.</span></span> <span data-ttu-id="a7ee2-181">Du kan inte ange båda hello **namn** och **nod** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-181">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="a7ee2-182">**Framtvinga** (valfritt): Ange tooforce HPC noder offline innan du stoppar dem.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-182">**Force** (optional): Setting tooforce HPC nodes offline before stopping them.</span></span>

### <a name="example"></a><span data-ttu-id="a7ee2-183">Exempel</span><span class="sxs-lookup"><span data-stu-id="a7ee2-183">Example</span></span>
<span data-ttu-id="a7ee2-184">hello följande exempel tvingar offline noder med namn som börjar *HPCNode-CN -* och sedan stoppas hello noder.</span><span class="sxs-lookup"><span data-stu-id="a7ee2-184">hello following example forces offline nodes with names beginning *HPCNode-CN-* and then stops hello nodes.</span></span>

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a><span data-ttu-id="a7ee2-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7ee2-185">Next steps</span></span>
* <span data-ttu-id="a7ee2-186">tooautomatically växa eller krympa hello klusternoder enligt hello nuvarande arbetsbelastningen för jobb och aktiviteter på hello kluster, se [automatiskt växa eller krympa hello HPC Pack klusterresurser i Azure bl.a toohello klusterarbetsbelastning](hpcpack-cluster-node-autogrowshrink.md).</span><span class="sxs-lookup"><span data-stu-id="a7ee2-186">tooautomatically grow or shrink hello cluster nodes according to hello current workload of jobs and tasks on hello cluster, see [Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload](hpcpack-cluster-node-autogrowshrink.md).</span></span>

