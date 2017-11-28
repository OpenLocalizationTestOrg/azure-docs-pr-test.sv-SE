---
title: aaaAutoscale HPC Pack klusternoder | Microsoft Docs
description: "Automatiskt växa eller krympa hello antalet noder i HPC Pack beräkning i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a><span data-ttu-id="c6b35-103">Automatiskt växa eller krympa hello HPC Pack klusterresurser i Azure enligt toohello klusterarbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="c6b35-103">Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload</span></span>
<span data-ttu-id="c6b35-104">Om du distribuerar Azure ”burst” noder i klustret HPC Pack, eller skapar du ett HPC Pack-kluster i Azure Virtual Machines, du kan öka eller minska hello klusterresurser, till exempel noder eller kärnor enligt hello arbetsbelastningen på hello klustret automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6b35-104">If you deploy Azure “burst” nodes in your HPC Pack cluster, or you create an HPC Pack cluster in Azure VMs, you may want a way to automatically grow or shrink hello cluster resources such as nodes or cores according to hello workload on hello cluster.</span></span> <span data-ttu-id="c6b35-105">Skalning hello klusterresurser på så sätt kan du toouse Azure-resurser mer effektivt och kontrollera deras kostnader.</span><span class="sxs-lookup"><span data-stu-id="c6b35-105">Scaling hello cluster resources in this way allows you toouse your Azure resources more efficiently and control their costs.</span></span>

<span data-ttu-id="c6b35-106">Den här artikeln visar HPC Pack innehåller beräkningsresurser tooautoscale på två sätt:</span><span class="sxs-lookup"><span data-stu-id="c6b35-106">This article shows you two ways that HPC Pack provides tooautoscale compute resources:</span></span>

* <span data-ttu-id="c6b35-107">hello HPC Pack klustret egenskapen **AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="c6b35-107">hello HPC Pack cluster property **AutoGrowShrink**</span></span>

* <span data-ttu-id="c6b35-108">Hej **AzureAutoGrowShrink.ps1** HPC PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c6b35-108">hello **AzureAutoGrowShrink.ps1** HPC PowerShell script</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="c6b35-109">För närvarande kan endast automatiskt växa och krympa HPC Pack compute-noder som kör ett Windows Server-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c6b35-109">Currently you can only automatically grow and shrink HPC Pack compute nodes that are running a Windows Server operating system.</span></span>


## <a name="set-hello-autogrowshrink-cluster-property"></a><span data-ttu-id="c6b35-110">Egenskapen hello AutoGrowShrink kluster</span><span class="sxs-lookup"><span data-stu-id="c6b35-110">Set hello AutoGrowShrink cluster property</span></span>
### <a name="prerequisites"></a><span data-ttu-id="c6b35-111">Krav</span><span class="sxs-lookup"><span data-stu-id="c6b35-111">Prerequisites</span></span>

* <span data-ttu-id="c6b35-112">**HPC Pack 2012 R2 uppdatering 2 eller ett senare kluster** -hello klustrets huvudnod kan distribueras antingen lokalt eller i en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="c6b35-112">**HPC Pack 2012 R2 Update 2 or later cluster** - hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="c6b35-113">Se [ställer in en hybrid-kluster med HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget igång med en lokal huvudnod och Azure ”burst” noder.</span><span class="sxs-lookup"><span data-stu-id="c6b35-113">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="c6b35-114">Se hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) tooquickly distribuera ett kluster som HPC Pack i virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="c6b35-114">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs.</span></span>

* <span data-ttu-id="c6b35-115">**För ett kluster med en huvudnod i Azure (Resource Manager-modellen)** – med början i HPC Pack 2016 certifikatautentisering i ett Azure Active Directory-program används för att automatiskt växer och krympande klustrets virtuella datorer distribueras via Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c6b35-115">**For a cluster with a head node in Azure (Resource Manager deployment model)** - Starting in HPC Pack 2016, certificate authentication in an Azure Active Directory application is used for automatically growing and shrinking cluster VMs deployed using Azure Resource Manager.</span></span> <span data-ttu-id="c6b35-116">Konfigurera ett certifikat på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c6b35-116">Configure a certificate as follows:</span></span>

  1. <span data-ttu-id="c6b35-117">Anslut med fjärrskrivbord tooone huvudnod efter klusterdistribution.</span><span class="sxs-lookup"><span data-stu-id="c6b35-117">After cluster deployment, connect by Remote Desktop tooone head node.</span></span>

  2. <span data-ttu-id="c6b35-118">Ladda upp certifikatet (PFX-format med privat nyckel) hello tooeach huvudnod och installera tooCert:\LocalMachine\My och Cert: \LocalMachine\Root.</span><span class="sxs-lookup"><span data-stu-id="c6b35-118">Upload hello certificate (PFX format with private key) tooeach head node and install tooCert:\LocalMachine\My and Cert:\LocalMachine\Root.</span></span>

  3. <span data-ttu-id="c6b35-119">Starta Azure PowerShell som administratör och kör följande kommandon i en huvudnod hello:</span><span class="sxs-lookup"><span data-stu-id="c6b35-119">Start Azure PowerShell as an administrator and run hello following commands on one head node:</span></span>

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    <span data-ttu-id="c6b35-120">Om ditt konto finns i fler än en Azure Active Directory-klient eller Azure-prenumeration, kan du köra följande hello kommandot tooselect hello rätt klient och prenumeration:</span><span class="sxs-lookup"><span data-stu-id="c6b35-120">If your account is in more than one Azure Active Directory tenant or Azure subscription, you can run hello following command tooselect hello correct tenant and subscription:</span></span>
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    <span data-ttu-id="c6b35-121">Kör följande kommando tooview hello markerade hello klient och prenumeration:</span><span class="sxs-lookup"><span data-stu-id="c6b35-121">Run hello following command tooview hello currently selected tenant and subscription:</span></span>
    
    ```powershell
        Get-AzureRMContext
    ```

  4. <span data-ttu-id="c6b35-122">Kör följande skript hello</span><span class="sxs-lookup"><span data-stu-id="c6b35-122">Run hello following script</span></span>

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    <span data-ttu-id="c6b35-123">där</span><span class="sxs-lookup"><span data-stu-id="c6b35-123">where</span></span>

    <span data-ttu-id="c6b35-124">**DisplayName** -Azure Active programmets visningsnamn.</span><span class="sxs-lookup"><span data-stu-id="c6b35-124">**DisplayName** - Azure Active Application display name.</span></span> <span data-ttu-id="c6b35-125">Om programmet hello inte finns, skapas den i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c6b35-125">If hello application does not exist, it is created in Azure Active Directory.</span></span>

    <span data-ttu-id="c6b35-126">**Webbsida** -hello programmet hello startsida.</span><span class="sxs-lookup"><span data-stu-id="c6b35-126">**HomePage** - hello home page of hello application.</span></span> <span data-ttu-id="c6b35-127">Du kan konfigurera en dummy URL, som i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="c6b35-127">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="c6b35-128">**IdentifierUri** -identifierare för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="c6b35-128">**IdentifierUri** - Identifier of hello application.</span></span> <span data-ttu-id="c6b35-129">Du kan konfigurera en dummy URL, som i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="c6b35-129">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="c6b35-130">**CertificateThumbprint** -tumavtryck hello certifikat du installerade på hello huvudnod i steg 1.</span><span class="sxs-lookup"><span data-stu-id="c6b35-130">**CertificateThumbprint** - Thumbprint of hello certificate you installed on hello head node in Step 1.</span></span>

    <span data-ttu-id="c6b35-131">**TenantId** -klient-ID för din Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c6b35-131">**TenantId** - Tenant ID of your Azure Active Directory.</span></span> <span data-ttu-id="c6b35-132">Du kan hämta hello klient-ID från hello Azure Active Directory-portalen **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="c6b35-132">You can get hello Tenant ID from hello Azure Active Directory portal **Properties** page.</span></span>

    <span data-ttu-id="c6b35-133">Mer information om **ConfigARMAutoGrowShrinkCert.ps1**kör `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span><span class="sxs-lookup"><span data-stu-id="c6b35-133">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span></span>


* <span data-ttu-id="c6b35-134">**För ett kluster med en huvudnod i Azure (klassiska distributionsmodellen)** - om du använder hello HPC Pack IaaS distribution skriptet toocreate hello kluster i hello klassiska distributionsmodellen, aktivera hello **AutoGrowShrink** kluster Egenskapen genom att ange hello AutoGrowShrink alternativet i konfigurationsfilen för hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="c6b35-134">**For a cluster with a head node in Azure (classic deployment model)** - If you use hello HPC Pack IaaS deployment script toocreate hello cluster in hello classic deployment model, enable hello **AutoGrowShrink** cluster property by setting hello AutoGrowShrink option in hello cluster configuration file.</span></span> <span data-ttu-id="c6b35-135">Mer information finns i hello-dokumentationen som medföljer hello [hämtning av](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="c6b35-135">For details, see hello documentation accompanying hello [script download](https://www.microsoft.com/download/details.aspx?id=44949).</span></span>

    <span data-ttu-id="c6b35-136">Du kan också aktivera hello **AutoGrowShrink** klustret egenskapen när du har distribuerat hello kluster med hjälp av HPC PowerShell-kommandon beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="c6b35-136">Alternatively, enable hello **AutoGrowShrink** cluster property after you deploy hello cluster by using HPC PowerShell commands described in hello following section.</span></span> <span data-ttu-id="c6b35-137">tooprepare för den här första fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c6b35-137">tooprepare for this, first complete hello following steps:</span></span>

  1. <span data-ttu-id="c6b35-138">Konfigurera ett Azure-hanteringscertifikat i hello huvudnod och i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6b35-138">Configure an Azure management certificate on hello head node and in hello Azure subscription.</span></span> <span data-ttu-id="c6b35-139">En test-distribution kan du använda hello standard Microsoft HPC Azure självsignerade certifikat som HPC Pack installeras på hello huvudnod och ladda upp det certifikatet tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6b35-139">For a test deployment, you can use hello Default Microsoft HPC Azure self-signed certificate that HPC Pack installs on hello head node, and then upload that certificate tooyour Azure subscription.</span></span> <span data-ttu-id="c6b35-140">Alternativ och steg finns hello [TechNet-biblioteket vägledning](https://technet.microsoft.com/library/gg481759.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6b35-140">For options and steps, see hello [TechNet Library guidance](https://technet.microsoft.com/library/gg481759.aspx).</span></span>

  2. <span data-ttu-id="c6b35-141">Kör **regedit** på hello huvudnod, gå tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo och Lägg till ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="c6b35-141">Run **regedit** on hello head node, go tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, and add a string value.</span></span> <span data-ttu-id="c6b35-142">Ange namnet på hello värdet för ”tumavtryck” och värdet data toohello hello certifikatets tumavtryck i steg 1.</span><span class="sxs-lookup"><span data-stu-id="c6b35-142">Set hello Value name too“ThumbPrint”, and Value data toohello thumbprint of hello certificate in Step 1.</span></span>

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a><span data-ttu-id="c6b35-143">HPC PowerShell-kommandon tooset hello AutoGrowShrink-egenskap</span><span class="sxs-lookup"><span data-stu-id="c6b35-143">HPC PowerShell commands tooset hello AutoGrowShrink property</span></span>
<span data-ttu-id="c6b35-144">Följande är exempel HPC PowerShell-kommandon tooset **AutoGrowShrink** och tootune sitt beteende med ytterligare parametrar.</span><span class="sxs-lookup"><span data-stu-id="c6b35-144">Following are sample HPC PowerShell commands tooset **AutoGrowShrink** and tootune its behavior with additional parameters.</span></span> <span data-ttu-id="c6b35-145">Se [AutoGrowShrink parametrar](#AutoGrowShrink-parameters) senare i den här artikeln hello fullständig lista över inställningar.</span><span class="sxs-lookup"><span data-stu-id="c6b35-145">See [AutoGrowShrink parameters](#AutoGrowShrink-parameters) later in this article for hello complete list of settings.</span></span>

<span data-ttu-id="c6b35-146">toorun dessa kommandon starta HPC PowerShell på hello klustrets huvudnod som administratör.</span><span class="sxs-lookup"><span data-stu-id="c6b35-146">toorun these commands, start HPC PowerShell on hello cluster head node as an administrator.</span></span>

<span data-ttu-id="c6b35-147">**tooenable hello AutoGrowShrink egenskapen**</span><span class="sxs-lookup"><span data-stu-id="c6b35-147">**tooenable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

<span data-ttu-id="c6b35-148">**toodisable hello AutoGrowShrink egenskapen**</span><span class="sxs-lookup"><span data-stu-id="c6b35-148">**toodisable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

<span data-ttu-id="c6b35-149">**toochange hello växer intervall i minuter**</span><span class="sxs-lookup"><span data-stu-id="c6b35-149">**toochange hello grow interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

<span data-ttu-id="c6b35-150">**toochange hello krympa intervall i minuter**</span><span class="sxs-lookup"><span data-stu-id="c6b35-150">**toochange hello shrink interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

<span data-ttu-id="c6b35-151">**tooview hello aktuella konfigureringen av AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="c6b35-151">**tooview hello current configuration of AutoGrowShrink**</span></span>

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

<span data-ttu-id="c6b35-152">**tooexclude noden grupper från AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="c6b35-152">**tooexclude node groups from AutoGrowShrink**</span></span>

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> <span data-ttu-id="c6b35-153">Den här parametern stöds från och med HPC Pack 2016</span><span class="sxs-lookup"><span data-stu-id="c6b35-153">This parameter is supported starting in HPC Pack 2016</span></span>
>

### <a name="autogrowshrink-parameters"></a><span data-ttu-id="c6b35-154">AutoGrowShrink parametrar</span><span class="sxs-lookup"><span data-stu-id="c6b35-154">AutoGrowShrink parameters</span></span>
<span data-ttu-id="c6b35-155">hello följande är AutoGrowShrink parametrar som du kan ändra med hjälp av hello **Set HpcClusterProperty** kommando.</span><span class="sxs-lookup"><span data-stu-id="c6b35-155">hello following are AutoGrowShrink parameters that you can modify by using hello **Set-HpcClusterProperty** command.</span></span>

* <span data-ttu-id="c6b35-156">**EnableGrowShrink** – växla tooenable eller inaktivera hello **AutoGrowShrink** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-156">**EnableGrowShrink** - Switch tooenable or disable hello **AutoGrowShrink** property.</span></span>
* <span data-ttu-id="c6b35-157">**ParamSweepTasksPerCore** -antal parametrisk rensning uppgifter toogrow en kärna.</span><span class="sxs-lookup"><span data-stu-id="c6b35-157">**ParamSweepTasksPerCore** - Number of parametric sweep tasks toogrow one core.</span></span> <span data-ttu-id="c6b35-158">hello standardvärdet är en kärna med toogrow per aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c6b35-158">hello default is toogrow one core per task.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c6b35-159">HPC Pack QFE KB3134307 ändringar **ParamSweepTasksPerCore** för**TasksPerResourceUnit**.</span><span class="sxs-lookup"><span data-stu-id="c6b35-159">HPC Pack QFE KB3134307 changes **ParamSweepTasksPerCore** too**TasksPerResourceUnit**.</span></span> <span data-ttu-id="c6b35-160">Den baseras på resurstypen för hello jobb och kan vara nod, socket eller kärnor.</span><span class="sxs-lookup"><span data-stu-id="c6b35-160">It is based on hello job resource type and can be node, socket, or core.</span></span>
  >
  >
* <span data-ttu-id="c6b35-161">**GrowThreshold** -tröskelvärdet köade aktiviteter tootrigger automatisk ökning.</span><span class="sxs-lookup"><span data-stu-id="c6b35-161">**GrowThreshold** - Threshold of queued tasks tootrigger automatic growth.</span></span> <span data-ttu-id="c6b35-162">hello standardvärdet är 1, vilket innebär att om det finns 1 eller flera aktiviteter i hello köade tillstånd, utöka noder automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6b35-162">hello default is 1, which means that if there are 1 or more tasks in hello queued state, automatically grow nodes.</span></span>
* <span data-ttu-id="c6b35-163">**GrowInterval** -intervallet i minuter tootrigger automatisk ökning.</span><span class="sxs-lookup"><span data-stu-id="c6b35-163">**GrowInterval** - Interval in minutes tootrigger automatic growth.</span></span> <span data-ttu-id="c6b35-164">hello Standardintervallet är 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="c6b35-164">hello default interval is 5 minutes.</span></span>
* <span data-ttu-id="c6b35-165">**ShrinkInterval** -intervallet i minuter tootrigger automatisk krymper.</span><span class="sxs-lookup"><span data-stu-id="c6b35-165">**ShrinkInterval** - Interval in minutes tootrigger automatic shrinking.</span></span> <span data-ttu-id="c6b35-166">hello Standardintervallet är 5 minuter. |</span><span class="sxs-lookup"><span data-stu-id="c6b35-166">hello default interval is 5 minutes.|</span></span>
* <span data-ttu-id="c6b35-167">**ShrinkIdleTimes** -antalet kontinuerlig kontrollerar tooshrink tooindicate hello noder är inaktiva.</span><span class="sxs-lookup"><span data-stu-id="c6b35-167">**ShrinkIdleTimes** - Number of continuous checks tooshrink tooindicate hello nodes are idle.</span></span> <span data-ttu-id="c6b35-168">hello standardinställningen är 3 gånger.</span><span class="sxs-lookup"><span data-stu-id="c6b35-168">hello default is 3 times.</span></span> <span data-ttu-id="c6b35-169">Till exempel, om hello **ShrinkInterval** är 5 minuter, HPC Pack kontrollerar om hello nod är inaktiv var femte minut.</span><span class="sxs-lookup"><span data-stu-id="c6b35-169">For example, if hello **ShrinkInterval** is 5 minutes, HPC Pack checks whether hello node is idle every 5 minutes.</span></span> <span data-ttu-id="c6b35-170">Om hello noder är i viloläge hello efter 3 kontinuerlig kontrolleras (15 minuter), krymper HPC Pack noden.</span><span class="sxs-lookup"><span data-stu-id="c6b35-170">If hello nodes are in hello idle state after 3 continuous checks (15 minutes), then HPC Pack shrinks that node.</span></span>
* <span data-ttu-id="c6b35-171">**ExtraNodesGrowRatio** -ytterligare procentandelen noder toogrow för Message Passing Interface (MPI) jobb.</span><span class="sxs-lookup"><span data-stu-id="c6b35-171">**ExtraNodesGrowRatio** - Additional percentage of nodes toogrow for Message Passing Interface (MPI) jobs.</span></span> <span data-ttu-id="c6b35-172">hello standardvärdet är 1, vilket innebär att HPC Pack växer noder 1% för MPI-jobb.</span><span class="sxs-lookup"><span data-stu-id="c6b35-172">hello default value is 1, which means that HPC Pack grows nodes 1% for MPI jobs.</span></span>
* <span data-ttu-id="c6b35-173">**GrowByMin** -växla tooindicate om hello autogrow princip baseras på hello minsta resurser som krävs för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="c6b35-173">**GrowByMin** - Switch tooindicate whether hello autogrow policy is based on hello minimum resources required for hello job.</span></span> <span data-ttu-id="c6b35-174">hello standard är FALSKT, vilket innebär att HPC Pack växer noder för jobb baserat på hello maximala resurser som krävs för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="c6b35-174">hello default is false, which means that HPC Pack grows nodes for jobs based on hello maximum resources required for hello jobs.</span></span>
* <span data-ttu-id="c6b35-175">**SoaJobGrowThreshold** -tröskelvärdet för inkommande SOA-begäranden tootrigger hello automatisk växa processen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-175">**SoaJobGrowThreshold** - Threshold of incoming SOA requests tootrigger hello automatic grow process.</span></span> <span data-ttu-id="c6b35-176">hello standardvärdet är 50000.</span><span class="sxs-lookup"><span data-stu-id="c6b35-176">hello default value is 50000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c6b35-177">Den här parametern stöds från och med HPC Pack 2012 R2 uppdatering 3.</span><span class="sxs-lookup"><span data-stu-id="c6b35-177">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
  >
* <span data-ttu-id="c6b35-178">**SoaRequestsPerCore** -antal inkommande SOA-begäranden toogrow en kärna.</span><span class="sxs-lookup"><span data-stu-id="c6b35-178">**SoaRequestsPerCore** -Number of incoming SOA requests toogrow one core.</span></span> <span data-ttu-id="c6b35-179">hello standardvärdet är 20000.</span><span class="sxs-lookup"><span data-stu-id="c6b35-179">hello default value is 20000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c6b35-180">Den här parametern stöds från och med HPC Pack 2012 R2 uppdatering 3.</span><span class="sxs-lookup"><span data-stu-id="c6b35-180">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
* <span data-ttu-id="c6b35-181">**ExcludeNodeGroups** – noder i hello angetts noden grupper inte automatiskt växa eller krympa.</span><span class="sxs-lookup"><span data-stu-id="c6b35-181">**ExcludeNodeGroups** – Nodes in hello specified node groups do not automatically grow and shrink.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="c6b35-182">Den här parametern stöds från och med HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="c6b35-182">This parameter is supported starting in HPC Pack 2016.</span></span>
  >

### <a name="mpi-example"></a><span data-ttu-id="c6b35-183">MPI-exempel</span><span class="sxs-lookup"><span data-stu-id="c6b35-183">MPI example</span></span>
<span data-ttu-id="c6b35-184">Som standard HPC Pack växer 1% extra noder för MPI-jobb (**ExtraNodesGrowRatio** anges too1).</span><span class="sxs-lookup"><span data-stu-id="c6b35-184">By default HPC Pack grows 1% extra nodes for MPI jobs (**ExtraNodesGrowRatio** is set too1).</span></span> <span data-ttu-id="c6b35-185">hello orsaken är att MPI kan kräva flera noder och hello jobb kan endast köra när alla noder är klar.</span><span class="sxs-lookup"><span data-stu-id="c6b35-185">hello reason is that MPI may require multiple nodes, and hello job can only run when all nodes are ready.</span></span> <span data-ttu-id="c6b35-186">När Azure startar noder behöva ibland en nod mer tid toostart än andra orsakar andra noder toobe inaktiv under väntan på att noden tooget redo.</span><span class="sxs-lookup"><span data-stu-id="c6b35-186">When Azure starts nodes, occasionally one node might need more time toostart than others, causing other nodes toobe idle while waiting for that node tooget ready.</span></span> <span data-ttu-id="c6b35-187">HPC Pack av växande extra noder minskar väntetiden för den här resursen och kostnaderna potentiellt.</span><span class="sxs-lookup"><span data-stu-id="c6b35-187">By growing extra nodes, HPC Pack reduces this resource waiting time, and potentially saves costs.</span></span> <span data-ttu-id="c6b35-188">tooincrease hello procentandelen extra noder för MPI-jobb (till exempel too10%), köra ett kommando som liknar</span><span class="sxs-lookup"><span data-stu-id="c6b35-188">tooincrease hello percentage of extra nodes for MPI jobs (for example, too10%), run a command similar to</span></span>

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a><span data-ttu-id="c6b35-189">SOA-exempel</span><span class="sxs-lookup"><span data-stu-id="c6b35-189">SOA example</span></span>
<span data-ttu-id="c6b35-190">Som standard **SoaJobGrowThreshold** anges too50000 och **SoaRequestsPerCore** too200000 anges.</span><span class="sxs-lookup"><span data-stu-id="c6b35-190">By default, **SoaJobGrowThreshold** is set too50000 and **SoaRequestsPerCore** is set too200000.</span></span> <span data-ttu-id="c6b35-191">Om du skickar ett SOA-jobb med 70000 begäranden, det finns en köad aktivitet och inkommande begäranden 70000.</span><span class="sxs-lookup"><span data-stu-id="c6b35-191">If you submit one SOA job with 70000 requests, there is one queued task and incoming requests are 70000.</span></span> <span data-ttu-id="c6b35-192">I det här fallet HPC Pack växer 1 kärna för hello i kö aktivitet och växer (70000 50000) efter inkommande begäranden / 20000 = 1 core i totalt växer 2 kärnor för denna SOA-jobb.</span><span class="sxs-lookup"><span data-stu-id="c6b35-192">In this case HPC Pack grows 1 core for hello queued task, and for incoming requests, grows (70000 - 50000)/20000 = 1 core, so in total grows 2 cores for this SOA job.</span></span>

## <a name="run-hello-azureautogrowshrinkps1-script"></a><span data-ttu-id="c6b35-193">Kör hello AzureAutoGrowShrink.ps1 skript</span><span class="sxs-lookup"><span data-stu-id="c6b35-193">Run hello AzureAutoGrowShrink.ps1 script</span></span>
### <a name="prerequisites"></a><span data-ttu-id="c6b35-194">Krav</span><span class="sxs-lookup"><span data-stu-id="c6b35-194">Prerequisites</span></span>

* <span data-ttu-id="c6b35-195">**HPC Pack 2012 R2 Update 1 eller ett senare kluster** - hello **AzureAutoGrowShrink.ps1** skript är installerat i hello % CCP_HOME % bin-mappen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-195">**HPC Pack 2012 R2 Update 1 or later cluster** - hello **AzureAutoGrowShrink.ps1** script is installed in hello %CCP_HOME%bin folder.</span></span> <span data-ttu-id="c6b35-196">hello klustrets huvudnod kan distribueras antingen lokalt eller i en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="c6b35-196">hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="c6b35-197">Se [ställer in en hybrid-kluster med HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget igång med en lokal huvudnod och Azure ”burst” noder.</span><span class="sxs-lookup"><span data-stu-id="c6b35-197">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="c6b35-198">Se hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) tooquickly distribuera ett kluster som HPC Pack Azure-dator eller Använd en [Azure quickstart mallen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span><span class="sxs-lookup"><span data-stu-id="c6b35-198">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs, or use an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span></span>
* <span data-ttu-id="c6b35-199">**Azure PowerShell 1.4.0** -hello skript för närvarande är beroende av den här specifika versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c6b35-199">**Azure PowerShell 1.4.0** - hello script currently depends on this specific version of Azure PowerShell.</span></span>
* <span data-ttu-id="c6b35-200">**För ett kluster med Azure burst-noder** -kör hello skriptet på en klientdator där HPC Pack är installerad eller på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6b35-200">**For a cluster with Azure burst nodes** - Run hello script on a client computer where HPC Pack is installed, or on hello head node.</span></span> <span data-ttu-id="c6b35-201">Om körs på en klientdator, kontrollerar du att du angett hello variabeln $env: CCP_SCHEDULER toopoint toohello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6b35-201">If running on a client computer, ensure that you set hello variable $env:CCP_SCHEDULER toopoint toohello head node.</span></span> <span data-ttu-id="c6b35-202">hello Azure ”burst” noder måste läggas till toohello kluster, men de kan vara i hello inte distribuerats tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c6b35-202">hello Azure “burst” nodes must be added toohello cluster, but they may be in hello Not-Deployed state.</span></span>
* <span data-ttu-id="c6b35-203">**För ett kluster som distribueras på virtuella Azure-datorer (Resource Manager-distributionsmodellen)** -för ett kluster med Azure virtuella datorer som distribueras i hello Resource Manager-distributionsmodellen hello skript stöder två metoder för Azure-autentisering: Logga in tooyour Azure-konto toorun hello skript varje gång (genom att köra `Login-AzureRmAccount`, eller konfigurera en service principal tooauthenticate med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="c6b35-203">**For a cluster deployed in Azure VMs (Resource Manager deployment model)** - For a cluster of Azure VMs deployed in hello Resource Manager deployment model, hello script supports two methods for Azure authentication: sign in tooyour Azure account toorun hello script every time (by running `Login-AzureRmAccount`, or configure a service principal tooauthenticate with a certificate.</span></span> <span data-ttu-id="c6b35-204">HPC Pack innehåller hello skriptet **ConfigARMAutoGrowShrinkCert.ps** toocreate ett huvudnamn för tjänsten med certifikat.</span><span class="sxs-lookup"><span data-stu-id="c6b35-204">HPC Pack provides hello script **ConfigARMAutoGrowShrinkCert.ps** toocreate a service principal with certificate.</span></span> <span data-ttu-id="c6b35-205">hello skriptet skapar ett program för Azure Active Directory (Azure AD) och ett huvudnamn för tjänsten och tilldelar hello deltagare rollen toohello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="c6b35-205">hello script creates an Azure Active Directory (Azure AD) application and a service principal, and assigns hello Contributor role toohello service principal.</span></span> <span data-ttu-id="c6b35-206">toorun hello skript, starta Azure PowerShell som administratör och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="c6b35-206">toorun hello script, start Azure PowerShell  as administrator and run hello following commands:</span></span>

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    <span data-ttu-id="c6b35-207">Mer information om **ConfigARMAutoGrowShrinkCert.ps1**kör `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span><span class="sxs-lookup"><span data-stu-id="c6b35-207">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span></span>

* <span data-ttu-id="c6b35-208">**För ett kluster som distribueras på virtuella Azure-datorer (klassiska distributionsmodellen)** -körning hello skript på hello huvudnod VM, eftersom den är beroende av hello **Start HpcIaaSNode.ps1** och **Stop-HpcIaaSNode.ps1**skript som är installerade det.</span><span class="sxs-lookup"><span data-stu-id="c6b35-208">**For a cluster deployed in Azure VMs (classic deployment model)** - Run hello script on hello head node VM, because it depends on hello **Start-HpcIaaSNode.ps1** and **Stop-HpcIaaSNode.ps1** scripts that are installed there.</span></span> <span data-ttu-id="c6b35-209">Dessa skript Dessutom kräver ett Azure-hanteringscertifikat eller inställningsfilen för publicering (se [hantera compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster-node-manage.md)).</span><span class="sxs-lookup"><span data-stu-id="c6b35-209">Those scripts additionally require an Azure management certificate or publish settings file (see [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)).</span></span> <span data-ttu-id="c6b35-210">Kontrollera att alla hello compute-nod virtuella datorer som du redan har lagts till toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="c6b35-210">Make sure all hello compute node VMs you need are already added toohello cluster.</span></span> <span data-ttu-id="c6b35-211">De kan vara i hello stoppat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c6b35-211">They may be in hello Stopped state.</span></span>



### <a name="syntax"></a><span data-ttu-id="c6b35-212">Syntax</span><span class="sxs-lookup"><span data-stu-id="c6b35-212">Syntax</span></span>
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="c6b35-213">Parametrar</span><span class="sxs-lookup"><span data-stu-id="c6b35-213">Parameters</span></span>
* <span data-ttu-id="c6b35-214">**NodeTemplates** -namnen på hello noden mallar toodefine hello omfång för hello noder toogrow och minska.</span><span class="sxs-lookup"><span data-stu-id="c6b35-214">**NodeTemplates** - Names of hello node templates toodefine hello scope for hello nodes toogrow and shrink.</span></span> <span data-ttu-id="c6b35-215">Om inget annat anges (hello standardvärdet är @()), alla noder i hello **AzureNodes** nodgrupp är i omfattning när **NodeType** har värdet AzureNodes och alla noder i hello **ComputeNodes** nodgrupp är i omfattning när **NodeType** har värdet ComputeNodes.</span><span class="sxs-lookup"><span data-stu-id="c6b35-215">If not specified (hello default value is @()), all nodes in hello **AzureNodes** node group are in scope when **NodeType** has a value of AzureNodes, and all nodes in hello **ComputeNodes** node group are in scope when **NodeType** has a value of ComputeNodes.</span></span>
* <span data-ttu-id="c6b35-216">**JobTemplates** -namnen på hello jobbet mallar toodefine hello omfång för hello noder toogrow.</span><span class="sxs-lookup"><span data-stu-id="c6b35-216">**JobTemplates** - Names of hello job templates toodefine hello scope for hello nodes toogrow.</span></span>
* <span data-ttu-id="c6b35-217">**NodeType** – hello typ av noden toogrow och minska.</span><span class="sxs-lookup"><span data-stu-id="c6b35-217">**NodeType** - hello type of node toogrow and shrink.</span></span> <span data-ttu-id="c6b35-218">Värden som stöds är:</span><span class="sxs-lookup"><span data-stu-id="c6b35-218">Supported values are:</span></span>

  * <span data-ttu-id="c6b35-219">**AzureNodes** – för Azure PaaS (burst) noder i ett lokalt eller Azure IaaS-kluster.</span><span class="sxs-lookup"><span data-stu-id="c6b35-219">**AzureNodes** – for Azure PaaS (burst) nodes in an on-premises or Azure IaaS cluster.</span></span>
  * <span data-ttu-id="c6b35-220">**ComputeNodes** - för beräkningsnod virtuella datorer i ett Azure IaaS-kluster.</span><span class="sxs-lookup"><span data-stu-id="c6b35-220">**ComputeNodes** - for compute node VMs only in an Azure IaaS cluster.</span></span>

* <span data-ttu-id="c6b35-221">**NumOfQueuedJobsPerNodeToGrow** -antalet köade jobb krävs toogrow en nod.</span><span class="sxs-lookup"><span data-stu-id="c6b35-221">**NumOfQueuedJobsPerNodeToGrow** - Number of queued jobs required toogrow one node.</span></span>
* <span data-ttu-id="c6b35-222">**NumOfQueuedJobsToGrowThreshold** -hello tröskelvärdet för antal köade jobb toostart hello växa processen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-222">**NumOfQueuedJobsToGrowThreshold** - hello threshold number of queued jobs toostart hello grow process.</span></span>
* <span data-ttu-id="c6b35-223">**NumOfActiveQueuedTasksPerNodeToGrow** -hello antalet aktiva köade aktiviteter krävs toogrow en nod.</span><span class="sxs-lookup"><span data-stu-id="c6b35-223">**NumOfActiveQueuedTasksPerNodeToGrow** - hello number of active queued tasks required toogrow one node.</span></span> <span data-ttu-id="c6b35-224">Om **NumOfQueuedJobsPerNodeToGrow** anges med ett värde som är större än 0, ignoreras den här parametern.</span><span class="sxs-lookup"><span data-stu-id="c6b35-224">If **NumOfQueuedJobsPerNodeToGrow** is specified with a value greater than 0, this parameter is ignored.</span></span>
* <span data-ttu-id="c6b35-225">**NumOfActiveQueuedTasksToGrowThreshold** -hello tröskelvärdet för antal aktiva köade aktiviteter toostart hello växa processen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-225">**NumOfActiveQueuedTasksToGrowThreshold** - hello threshold number of active queued tasks toostart hello grow process.</span></span>
* <span data-ttu-id="c6b35-226">**NumOfInitialNodesToGrow** - hello inledande minsta antal noder toogrow om alla hello-noder i omfånget är **inte distribuerade** eller **Stoppad (Deallocated)**.</span><span class="sxs-lookup"><span data-stu-id="c6b35-226">**NumOfInitialNodesToGrow** - hello initial minimum number of nodes toogrow if all hello nodes in scope are **Not-Deployed** or **Stopped (Deallocated)**.</span></span>
* <span data-ttu-id="c6b35-227">**GrowCheckIntervalMins** -hello-intervall i minuter mellan kontrollerar toogrow.</span><span class="sxs-lookup"><span data-stu-id="c6b35-227">**GrowCheckIntervalMins** - hello interval in minutes between checks toogrow.</span></span>
* <span data-ttu-id="c6b35-228">**ShrinkCheckIntervalMins** -hello-intervall i minuter mellan kontrollerar tooshrink.</span><span class="sxs-lookup"><span data-stu-id="c6b35-228">**ShrinkCheckIntervalMins** - hello interval in minutes between checks tooshrink.</span></span>
* <span data-ttu-id="c6b35-229">**ShrinkCheckIdleTimes** -hello antal kontinuerlig förminskas kontroller (avgränsade med **ShrinkCheckIntervalMins**) tooindicate hello noder är inaktiv.</span><span class="sxs-lookup"><span data-stu-id="c6b35-229">**ShrinkCheckIdleTimes** - hello number of continuous shrink checks (separated by **ShrinkCheckIntervalMins**) tooindicate hello nodes are idle.</span></span>
* <span data-ttu-id="c6b35-230">**UseLastConfigurations** -hello tidigare konfigurationer i hello argumentet-filen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-230">**UseLastConfigurations** - hello previous configurations saved in hello argument file.</span></span>
* <span data-ttu-id="c6b35-231">**ArgFile**- hello namnet på hello argumentet filen används toosave och uppdatera hello toorun hello-konfigurationsskript.</span><span class="sxs-lookup"><span data-stu-id="c6b35-231">**ArgFile**- hello name of hello argument file used toosave and update hello configurations toorun hello script.</span></span>
* <span data-ttu-id="c6b35-232">**LogFilePrefix** -hello-prefixet i hello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-232">**LogFilePrefix** - hello prefix name of hello log file.</span></span> <span data-ttu-id="c6b35-233">Du kan ange en sökväg.</span><span class="sxs-lookup"><span data-stu-id="c6b35-233">You can specify a path.</span></span> <span data-ttu-id="c6b35-234">Som standard är hello loggen skriftliga toohello aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c6b35-234">By default hello log is written toohello current working directory.</span></span>

### <a name="example-1"></a><span data-ttu-id="c6b35-235">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="c6b35-235">Example 1</span></span>
<span data-ttu-id="c6b35-236">hello konfigureras följande exempel hello Azure burst-noder som distribueras med standardmall AzureNode toogrow och minska automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6b35-236">hello following example configures hello Azure burst nodes deployed with the Default AzureNode Template toogrow and shrink automatically.</span></span> <span data-ttu-id="c6b35-237">Om alla noder är från början hello **inte distribuerade** tillstånd, minst 3 noder har startats.</span><span class="sxs-lookup"><span data-stu-id="c6b35-237">If all the nodes are initially in hello **Not-Deployed** state, at least 3 nodes are started.</span></span> <span data-ttu-id="c6b35-238">Om hello antalet köade jobb överskrider 8, hello skriptet börjar noder tills deras nummer överskrider hello förhållandet mellan jobb i kön till **NumOfQueuedJobsPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="c6b35-238">If hello number of queued jobs exceeds 8, hello script starts nodes until their number exceeds hello ratio of queued jobs to **NumOfQueuedJobsPerNodeToGrow**.</span></span> <span data-ttu-id="c6b35-239">Om en nod har hittats toobe inaktiv i 3 på varandra följande ledig tid, stoppas.</span><span class="sxs-lookup"><span data-stu-id="c6b35-239">If a node is found toobe idle in 3 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a><span data-ttu-id="c6b35-240">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="c6b35-240">Example 2</span></span>
<span data-ttu-id="c6b35-241">hello konfigureras följande exempel hello Azure compute-nod virtuella datorer som distribueras med hello standardmall ComputeNode toogrow och minska automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6b35-241">hello following example configures hello Azure compute node VMs deployed with hello Default ComputeNode Template toogrow and shrink automatically.</span></span>
<span data-ttu-id="c6b35-242">hello jobb som konfigurerats av hello standardmallen för jobbet definiera hello omfattning arbetsbelastningen på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c6b35-242">hello jobs configured by hello Default job template define hello scope of the workload on hello cluster.</span></span> <span data-ttu-id="c6b35-243">Om alla noder i hello ursprungligen har stoppats, startas minst 5 noder.</span><span class="sxs-lookup"><span data-stu-id="c6b35-243">If all hello nodes are initially stopped, at least 5 nodes are started.</span></span> <span data-ttu-id="c6b35-244">Om hello antalet aktiva köade aktiviteter överskrider 15, hello skriptet börjar noder tills deras nummer överskrider hello förhållandet mellan aktiva köade aktiviteter för**NumOfActiveQueuedTasksPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="c6b35-244">If hello number of active queued tasks exceeds 15, hello script starts nodes until their number exceeds hello ratio of active queued tasks too**NumOfActiveQueuedTasksPerNodeToGrow**.</span></span> <span data-ttu-id="c6b35-245">Om en nod har hittats toobe inaktiv i 10 på varandra följande ledig tid, stoppas.</span><span class="sxs-lookup"><span data-stu-id="c6b35-245">If a node is found toobe idle in 10 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
