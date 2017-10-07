---
title: aaaLinux compute virtuella datorer i ett kluster med HPC Pack | Microsoft Docs
description: "Lär dig hur toocreate och använda ett HPC Pack kluster i Azure för Linux högpresterande datorbearbetning (HPC) arbetsbelastningar"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 9ed20d6cd69a6472a00666caf8965e9d022698a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="4d118-103">Kom igång med beräkningsnoder för Linux i ett HPC Pack-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="4d118-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="4d118-104">Konfigurera en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) kluster i Azure som innehåller en huvudnod som kör Windows Server och flera compute-noder som kör ett Linux-distribution som stöds.</span><span class="sxs-lookup"><span data-stu-id="4d118-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="4d118-105">Utforska alternativ toomove data mellan hello Linux noder och hello Windows huvudnod hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="4d118-105">Explore options toomove data among hello Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="4d118-106">Lär dig hur toosubmit Linux HPC jobb toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="4d118-106">Learn how toosubmit Linux HPC jobs toohello cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="4d118-107">På en hög nivå hello följande diagram visar hello HPC Pack kluster du skapar och arbetar med.</span><span class="sxs-lookup"><span data-stu-id="4d118-107">At a high level, hello following diagram shows hello HPC Pack cluster you create and work with.</span></span>

![HPC Pack kluster med noder för Linux][scenario]

<span data-ttu-id="4d118-109">För andra alternativ toorun Linux HPC arbetsbelastningar i Azure, se [tekniska resurser för batch- och högpresterande datorbearbetning](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="4d118-109">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="4d118-110">Distribuera ett HPC Pack kluster med beräkningsnoder för Linux</span><span class="sxs-lookup"><span data-stu-id="4d118-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="4d118-111">Den här artikeln innehåller två alternativ toodeploy ett HPC Pack-kluster i Azure med Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-111">This article shows you two options toodeploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="4d118-112">Båda metoderna kan du använda en Marketplace-avbildning av Windows Server med HPC Pack toocreate hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-112">Both methods use a Marketplace image of Windows Server with HPC Pack toocreate hello head node.</span></span> 

* <span data-ttu-id="4d118-113">**Azure Resource Manager-mall** -använda en mall från hello Azure Marketplace, eller en mall för Snabbstart hello gemenskapen, tooautomate skapandet av hello klustret i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4d118-113">**Azure Resource Manager template** - Use a template from hello Azure Marketplace, or a quickstart template from hello community, tooautomate creation of hello cluster in hello Resource Manager deployment model.</span></span> <span data-ttu-id="4d118-114">Till exempel hello [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mallen i hello Azure Marketplace skapar en komplett HPC Pack klustret infrastruktur för Linux HPC arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4d118-114">For example, hello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="4d118-115">**PowerShell-skriptet** -Använd hello [Microsoft HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**ny HpcIaaSCluster.ps1**) tooautomate en fullständig Klusterdistribution i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4d118-115">**PowerShell script** - Use hello [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate a complete cluster deployment in hello classic deployment model.</span></span> <span data-ttu-id="4d118-116">Azure PowerShell-skript använder ett HPC Pack VM-avbildning i hello Azure Marketplace för snabb distribution och ger en omfattande uppsättning configuration parametrar toodeploy Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-116">This Azure PowerShell script uses an HPC Pack VM image in hello Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters toodeploy Linux compute nodes.</span></span>

<span data-ttu-id="4d118-117">Mer information om HPC Pack distributionsalternativ för kluster i Azure finns [alternativ toocreate och hantera ett kluster för högpresterande datorbearbetning (HPC) i Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d118-117">For more information about HPC Pack cluster deployment options in Azure, see [Options toocreate and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4d118-118">Krav</span><span class="sxs-lookup"><span data-stu-id="4d118-118">Prerequisites</span></span>
* <span data-ttu-id="4d118-119">**Azure-prenumeration** -du kan använda en prenumeration i antingen hello Azure Global eller Azure Kina service.</span><span class="sxs-lookup"><span data-stu-id="4d118-119">**Azure subscription** - You can use a subscription in either hello Azure Global or Azure China service.</span></span> <span data-ttu-id="4d118-120">Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4d118-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="4d118-121">**Kärnor kvoten** – du kan behöva tooincrease hello kvoten för kärnor, särskilt om du väljer toodeploy flera klusternoder med flera kärnor VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="4d118-121">**Cores quota** - You might need tooincrease hello quota of cores, especially if you choose toodeploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="4d118-122">tooincrease en kvot öppnar en online-kund supportbegäran utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="4d118-122">tooincrease a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="4d118-123">**Linux-distributioner** -för närvarande HPC Pack stöder hello följande Linux-distributioner för compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-123">**Linux distributions** - Currently HPC Pack supports hello following Linux distributions for compute nodes.</span></span> <span data-ttu-id="4d118-124">Du kan använda dessa distributioner Marketplace-versioner där det är tillgängligt eller ange egna.</span><span class="sxs-lookup"><span data-stu-id="4d118-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="4d118-125">**CentOS-baserade**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span><span class="sxs-lookup"><span data-stu-id="4d118-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="4d118-126">**Red Hat Enterprise Linux**: 6.7, 6.8 och 7.2</span><span class="sxs-lookup"><span data-stu-id="4d118-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="4d118-127">**SUSE Linux Enterprise Server**: SLES 12 SLES 12 (Premium) SLES 12 SP1, SLES 12 SP1 (Premium) SLES 12 för HPC SLES 12 för HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="4d118-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="4d118-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="4d118-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="4d118-129">toouse hello Azure RDMA nätverk med en av RDMA-kompatibla hello VM-storlekar, ange en SUSE Linux Enterprise Server 12 HPC eller CentOS-baserade HPC-avbildning från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4d118-129">toouse hello Azure RDMA network with one of hello RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from hello Azure Marketplace.</span></span> <span data-ttu-id="4d118-130">Mer information finns i [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d118-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="4d118-131">Ytterligare krav toodeploy hello-kluster med hjälp av hello HPC Pack IaaS-skriptet:</span><span class="sxs-lookup"><span data-stu-id="4d118-131">Additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="4d118-132">**Klientdatorn** -du behöver ett Windows-baserad klient datorn toorun hello klustret skript för distribution.</span><span class="sxs-lookup"><span data-stu-id="4d118-132">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="4d118-133">**Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="4d118-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="4d118-134">**HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="4d118-134">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="4d118-135">Du kan kontrollera hello version hello skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="4d118-135">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="4d118-136">Den här artikeln är baserat på version 4.4.1 eller senare av hello skript.</span><span class="sxs-lookup"><span data-stu-id="4d118-136">This article is based on version 4.4.1 or later of hello script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="4d118-137">Distributionsalternativ 1.</span><span class="sxs-lookup"><span data-stu-id="4d118-137">Deployment option 1.</span></span> <span data-ttu-id="4d118-138">Använd en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="4d118-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="4d118-139">Gå toohello [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mallen i hello Azure Marketplace och klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="4d118-139">Go toohello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="4d118-140">Hello Azure-portalen, granska hello information och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d118-140">In hello Azure portal, review hello information and then click **Create**.</span></span>
   
    ![Skapa Portal][portal]
3. <span data-ttu-id="4d118-142">På hello **grunderna** bladet, ange ett namn för hello-kluster, vilket även namnen hello huvudnod VM.</span><span class="sxs-lookup"><span data-stu-id="4d118-142">On hello **Basics** blade, enter a name for hello cluster, which also names hello head node VM.</span></span> <span data-ttu-id="4d118-143">Du kan välja en befintlig resursgrupp eller skapa en grupp för hello distribution på en plats som är tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="4d118-143">You can choose an existing resource group or create a group for hello deployment in a location that's available tooyou.</span></span> <span data-ttu-id="4d118-144">hello plats påverkar hello tillgängligheten för vissa VM-storlekar och andra Azure-tjänster (se [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="4d118-144">hello location affects hello availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="4d118-145">På hello **Head noden inställningar** bladet en första distribution kan du oftast acceptera standardinställningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="4d118-145">On hello **Head node settings** blade, for a first deployment, you can generally accept hello default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="4d118-146">Hej **efter konfigurationsskript URL** är en valfri inställning toospecify ett offentligt tillgängliga Windows PowerShell-skript som du vill toorun i hello huvudnod VM när den körs.</span><span class="sxs-lookup"><span data-stu-id="4d118-146">hello **Post-configuration script URL** is an optional setting toospecify a publicly available Windows PowerShell script that you want toorun on hello head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="4d118-147">På hello **Compute-nod inställningar** bladet välj namnmönstret för hello noder, hello antalet och storleken på hello noder och hello toodeploy för Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="4d118-147">On hello **Compute node settings** blade, select a naming pattern for hello nodes, hello number and size of hello nodes, and hello Linux distribution toodeploy.</span></span>
6. <span data-ttu-id="4d118-148">På hello **infrastrukturinställningar** bladet ange namn för hello virtuella nätverk och Active Directory domän, domän och VM-administratörsautentiseringsuppgifter och ett namngivningsmönster för hello storage-konton.</span><span class="sxs-lookup"><span data-stu-id="4d118-148">On hello **Infrastructure settings** blade, enter names for hello virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for hello storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4d118-149">HPC Pack använder hello Active Directory-domän tooauthenticate klustret användare.</span><span class="sxs-lookup"><span data-stu-id="4d118-149">HPC Pack uses hello Active Directory domain tooauthenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="4d118-150">När du kör verifieringstester hello och du granska hello villkor för användning, klickar du på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="4d118-150">After hello validation tests run and you review hello terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a><span data-ttu-id="4d118-151">Distributionsalternativ 2.</span><span class="sxs-lookup"><span data-stu-id="4d118-151">Deployment option 2.</span></span> <span data-ttu-id="4d118-152">Använd hello IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="4d118-152">Use hello IaaS deployment script</span></span>
<span data-ttu-id="4d118-153">Följande är ytterligare förutsättningar toodeploy hello kluster med hjälp av hello HPC Pack IaaS-skriptet:</span><span class="sxs-lookup"><span data-stu-id="4d118-153">Following are additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="4d118-154">**Klientdatorn** -du behöver ett Windows-baserad klient datorn toorun hello klustret skript för distribution.</span><span class="sxs-lookup"><span data-stu-id="4d118-154">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="4d118-155">**Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="4d118-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="4d118-156">**HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="4d118-156">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="4d118-157">Du kan kontrollera hello version hello skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="4d118-157">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="4d118-158">Den här artikeln är baserat på version 4.4.1 eller senare av hello skript.</span><span class="sxs-lookup"><span data-stu-id="4d118-158">This article is based on version 4.4.1 or later of hello script.</span></span>

<span data-ttu-id="4d118-159">**XML-konfigurationsfilen**</span><span class="sxs-lookup"><span data-stu-id="4d118-159">**XML configuration file**</span></span>

<span data-ttu-id="4d118-160">hello HPC Pack IaaS distributionsskriptet använder en XML-konfigurationsfil som inkommande toodescribe hello HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="4d118-160">hello HPC Pack IaaS deployment script uses an XML configuration file as input toodescribe hello  HPC cluster.</span></span> <span data-ttu-id="4d118-161">hello anger följande exempel konfigurationsfil ett mindre kluster som består av en huvudnod i HPC Pack och två storlek A7 CentOS 7.0 Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-161">hello following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="4d118-162">Ändra hello-filen som behövs för din miljö och önskade klusterkonfigurationen och spara den med ett namn, till exempel HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="4d118-162">Modify hello file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="4d118-163">Du måste till exempel toosupply namnet på din prenumeration och en unik lagringskontonamn och molntjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="4d118-163">For example, you need toosupply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="4d118-164">Dessutom kanske du vill att en annan toochoose stöds Linux avbildningen för hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-164">Additionally, you might want toochoose a different supported Linux image for hello compute nodes.</span></span> <span data-ttu-id="4d118-165">Mer information om hello element i hello-konfigurationsfilen finns hello Manual.rtf fil i mappen för hello-skript och [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d118-165">For more information about hello elements in hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="4d118-166">**toorun hello HPC Pack IaaS distributionsskriptet**</span><span class="sxs-lookup"><span data-stu-id="4d118-166">**toorun hello HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="4d118-167">Öppna Windows PowerShell på hello klientdator som administratör.</span><span class="sxs-lookup"><span data-stu-id="4d118-167">Open Windows PowerShell on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="4d118-168">Ändra toohello katalogmapp där hello skript är installerat (E:\IaaSClusterScript i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="4d118-168">Change directory toohello folder where hello script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="4d118-169">Kör följande kommando toodeploy hello HPC Pack klustret hello.</span><span class="sxs-lookup"><span data-stu-id="4d118-169">Run hello following command toodeploy hello HPC Pack cluster.</span></span> <span data-ttu-id="4d118-170">Det här exemplet förutsätter att hello-konfigurationsfilen finns i E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="4d118-170">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="4d118-171">a.</span><span class="sxs-lookup"><span data-stu-id="4d118-171">a.</span></span> <span data-ttu-id="4d118-172">Eftersom hello **AdminPassword** har inte angetts i hello före kommandot, ange tooenter hello lösenord för användare anges *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="4d118-172">Because hello **AdminPassword** is not specified in hello preceding command, you are prompted tooenter hello password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="4d118-173">b.</span><span class="sxs-lookup"><span data-stu-id="4d118-173">b.</span></span> <span data-ttu-id="4d118-174">hello skript startar sedan toovalidate hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="4d118-174">hello script then starts toovalidate hello configuration file.</span></span> <span data-ttu-id="4d118-175">Det kan ta upp tooseveral minuter beroende på hur hello nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="4d118-175">It can take up tooseveral minutes depending on hello network connection.</span></span>
   
    ![Validering][validate]
   
    <span data-ttu-id="4d118-177">c.</span><span class="sxs-lookup"><span data-stu-id="4d118-177">c.</span></span> <span data-ttu-id="4d118-178">När verifieringar skicka visar hello skript hello klustret resurser toocreate.</span><span class="sxs-lookup"><span data-stu-id="4d118-178">After validations pass, hello script lists hello cluster resources toocreate.</span></span> <span data-ttu-id="4d118-179">Ange *Y* toocontinue.</span><span class="sxs-lookup"><span data-stu-id="4d118-179">Enter *Y* toocontinue.</span></span>
   
    ![Resurser][resources]
   
    <span data-ttu-id="4d118-181">d.</span><span class="sxs-lookup"><span data-stu-id="4d118-181">d.</span></span> <span data-ttu-id="4d118-182">hello skript startar toodeploy hello HPC Pack kluster och har slutförts hello-konfiguration utan några ytterligare manuella steg.</span><span class="sxs-lookup"><span data-stu-id="4d118-182">hello script starts toodeploy hello HPC Pack cluster and completes hello configuration without further manual steps.</span></span> <span data-ttu-id="4d118-183">hello-skript kan köras i flera minuter.</span><span class="sxs-lookup"><span data-stu-id="4d118-183">hello script can run for several minutes.</span></span>
   
    ![Distribuera][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="4d118-185">I det här exemplet hello skriptet genererar en loggfil automatiskt eftersom hello **- LogFile** parameter har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="4d118-185">In this example, hello script generates a log file automatically since hello **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="4d118-186">hello loggar inte är skriven i realtid men har samlats in hello slutet av hello validering och hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="4d118-186">hello logs aren't written in real time, but are collected at hello end of hello validation and hello deployment.</span></span> <span data-ttu-id="4d118-187">Vissa loggar går förlorade om hello PowerShell-process stoppas medan hello skript körs.</span><span class="sxs-lookup"><span data-stu-id="4d118-187">If hello PowerShell process is stopped while hello script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-toohello-head-node"></a><span data-ttu-id="4d118-188">Ansluta toohello huvudnod</span><span class="sxs-lookup"><span data-stu-id="4d118-188">Connect toohello head node</span></span>
<span data-ttu-id="4d118-189">När du har distribuerat hello HPC Pack kluster i Azure, [ansluta med Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello huvudnod VM med hello autentiseringsuppgifter för domänen som du angav när du distribuerade klustret hello (till exempel *hpc\\ clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="4d118-189">After you deploy hello HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="4d118-190">Du kan hantera hello kluster från hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-190">You manage hello cluster from hello head node.</span></span>

<span data-ttu-id="4d118-191">Starta HPC Cluster Manager toocheck hello status för hello HPC Pack kluster på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-191">On hello head node, start HPC Cluster Manager toocheck hello status of hello HPC Pack cluster.</span></span> <span data-ttu-id="4d118-192">Du kan hantera och övervaka Linux datornoderna hello samma sätt som du arbetar med Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-192">You can manage and monitor Linux compute nodes hello same way you work with Windows compute nodes.</span></span> <span data-ttu-id="4d118-193">Se exempelvis hello Linux noder som anges i **resurshantering** (dessa noder distribueras med hello **LinuxNode** mall).</span><span class="sxs-lookup"><span data-stu-id="4d118-193">For example, you see hello Linux nodes listed in **Resource Management** (these nodes are deployed with hello **LinuxNode** template).</span></span>

![Hantering][management]

<span data-ttu-id="4d118-195">Du också se hello Linux noder i hello **termisk karta** vyn.</span><span class="sxs-lookup"><span data-stu-id="4d118-195">You also see hello Linux nodes in hello **Heat Map** view.</span></span>

![Termisk karta][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="4d118-197">Hur toomove data i ett kluster med noder som Linux</span><span class="sxs-lookup"><span data-stu-id="4d118-197">How toomove data in a cluster with Linux nodes</span></span>
<span data-ttu-id="4d118-198">Du har flera alternativ toomove data mellan noder för Linux och hello Windows huvudnod hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4d118-198">You have several choices toomove data among Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="4d118-199">Här följer tre vanliga metoder som beskrivs i detalj i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="4d118-199">Here are three common methods, described in more detail in hello following sections:</span></span>

* <span data-ttu-id="4d118-200">**Azure File** -visar filer som en hanterad SMB-filresurs toostore-data i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="4d118-200">**Azure File** - Exposes a managed SMB file share toostore data files in Azure storage.</span></span> <span data-ttu-id="4d118-201">Windows-noderna och Linux-noder kan montera en filresurs på Azure som en enhet eller mapp på hello samma tid, även om de har distribuerats i olika virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d118-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at hello same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="4d118-202">**Huvudnod SMB dela** -monterar en standard Windows delad mapp för hello huvudnod på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-202">**Head node SMB share** - Mounts a standard Windows shared folder of hello head node on Linux nodes.</span></span>
* <span data-ttu-id="4d118-203">**HEAD nod NFS-servern** -är en lösning för fildelning för en blandad miljö för Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="4d118-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="4d118-204">Azure File storage</span><span class="sxs-lookup"><span data-stu-id="4d118-204">Azure File storage</span></span>
<span data-ttu-id="4d118-205">Hej [Azure File](https://azure.microsoft.com/services/storage/files/) service exponerar filresurser med hjälp av SMB 2.1 hello standardprotokoll.</span><span class="sxs-lookup"><span data-stu-id="4d118-205">hello [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using hello standard SMB 2.1 protocol.</span></span> <span data-ttu-id="4d118-206">Virtuella Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via hello fillagring API.</span><span class="sxs-lookup"><span data-stu-id="4d118-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through hello File storage API.</span></span> 

<span data-ttu-id="4d118-207">Detaljerade anvisningar toocreate en Azure-fil dela och montera på hello huvudnod finns [Kom igång med Azure File storage i Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="4d118-207">For detailed steps toocreate an Azure File share and mount it on hello head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="4d118-208">toomount hello Azure-filresurs på hello Linux noder finns [hur toouse Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4d118-208">toomount hello Azure File share on hello Linux nodes, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="4d118-209">tooset in spara anslutningar finns [Persisting anslutningar tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d118-209">tooset up persisting connections, see [Persisting connections tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="4d118-210">I följande exempel hello, skapar du en Azure-filresurs på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4d118-210">In hello following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="4d118-211">toomount hello-resursen på hello huvudnod, öppna en kommandotolk och ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4d118-211">toomount hello share on hello head node, open a Command Prompt and enter hello following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="4d118-212">I det här exemplet allvhdsje är namnet på ditt lagringskonto, storageaccountkey är din lagringskontonyckel och rdma är hello Azure filresursnamn.</span><span class="sxs-lookup"><span data-stu-id="4d118-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is hello Azure File share name.</span></span> <span data-ttu-id="4d118-213">hello Azure-filresurs monteras som Z: hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-213">hello Azure File share is mounted as Z: on hello head node.</span></span>

<span data-ttu-id="4d118-214">toomount hello Azure-filresurs på Linux-noder som kör en **clusrun** på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-214">toomount hello Azure File share on Linux nodes, run a **clusrun** command on hello head node.</span></span> <span data-ttu-id="4d118-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  är en användbar HPC Pack verktyget toocarry administrativa uppgifter på flera noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool toocarry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="4d118-216">(Se även [Clusrun för Linux-noder](#Clusrun-for-Linux-nodes) i den här artikeln.)</span><span class="sxs-lookup"><span data-stu-id="4d118-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="4d118-217">Öppna Windows PowerShell-fönstret och ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4d118-217">Open a Windows PowerShell window and enter hello following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="4d118-218">hello första kommandot skapar en mapp med namnet /rdma på alla noder i hello LinuxNodes grupp.</span><span class="sxs-lookup"><span data-stu-id="4d118-218">hello first command creates a folder named /rdma on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="4d118-219">hello andra kommandot monterar hello Azure File share allvhdsjw.file.core.windows.net/rdma till hello /rdma mapp med dir och filen läge bits set too777.</span><span class="sxs-lookup"><span data-stu-id="4d118-219">hello second command mounts hello Azure File share allvhdsjw.file.core.windows.net/rdma onto hello /rdma folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="4d118-220">I andra hello-kommandot allvhdsje är namnet på ditt lagringskonto och storageaccountkey är din lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="4d118-220">In hello second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="4d118-221">Hej ”\\`” symbol i andra hello-kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4d118-221">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="4d118-222">”\\`,” innebär att hello ””, (kommatecken tecken) är en del av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="4d118-222">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="4d118-223">Huvudnod resursen</span><span class="sxs-lookup"><span data-stu-id="4d118-223">Head node share</span></span>
<span data-ttu-id="4d118-224">Du kan också montera en delad mapp för hello huvudnod på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-224">Alternatively, mount a shared folder of hello head node on Linux nodes.</span></span> <span data-ttu-id="4d118-225">En resurs ger hello enklaste sättet tooshare filer, men hello huvudnod och alla Linux-noder måste distribueras i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d118-225">A share provides hello simplest way tooshare files, but hello head node and all Linux nodes must be deployed in hello same virtual network.</span></span> <span data-ttu-id="4d118-226">Här är hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4d118-226">Here are hello steps.</span></span>

1. <span data-ttu-id="4d118-227">Skapa en mapp på hello huvudnod och dela den tooEveryone med läs-/ skrivbehörighet.</span><span class="sxs-lookup"><span data-stu-id="4d118-227">Create a folder on hello head node and share it tooEveryone with Read/Write permissions.</span></span> <span data-ttu-id="4d118-228">Till exempel dela D:\OpenFOAM i hello huvudnod som \\CentOS7RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="4d118-228">For example, share D:\OpenFOAM on hello head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="4d118-229">Här är CentOS7RDMA HN hello värdnamnet för hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-229">Here CentOS7RDMA-HN is hello hostname of hello head node.</span></span>
   
    ![Filresursbehörigheter][fileshareperms]
   
    ![Fildelning][filesharing]
2. <span data-ttu-id="4d118-232">Öppna Windows PowerShell-fönstret och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4d118-232">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="4d118-233">hello första kommandot skapar en mapp med namnet /openfoam på alla noder i hello LinuxNodes grupp.</span><span class="sxs-lookup"><span data-stu-id="4d118-233">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="4d118-234">hello andra kommandot monterar hello delade mappen //CentOS7RDMA-HN/OpenFOAM till hello mapp med dir och filen läge bits set too777.</span><span class="sxs-lookup"><span data-stu-id="4d118-234">hello second command mounts hello shared folder //CentOS7RDMA-HN/OpenFOAM onto hello folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="4d118-235">hello ska användarnamn och lösenord i hello-kommandot hello användarnamnet och lösenordet för en kluster-användare på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4d118-235">hello username and password in hello command should be hello username and password of a cluster user on hello head node.</span></span> <span data-ttu-id="4d118-236">(Se [Lägg till eller ta bort klustret användare](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="4d118-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="4d118-237">Hej ”\\`” symbol i andra hello-kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4d118-237">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="4d118-238">”\\`,” innebär att hello ””, (kommatecken tecken) är en del av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="4d118-238">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="4d118-239">NFS-server</span><span class="sxs-lookup"><span data-stu-id="4d118-239">NFS server</span></span>
<span data-ttu-id="4d118-240">hello NFS-tjänsten kan du tooshare och flytta filer mellan datorer som kör Windows Server 2012 för hello hello SMB-protokollet och Linux-baserade datorer med hjälp av hello NFS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="4d118-240">hello NFS service enables you tooshare and migrate files between computers running hello Windows Server 2012 operating system using hello SMB protocol and Linux-based computers using hello NFS protocol.</span></span> <span data-ttu-id="4d118-241">hello NFS-servern och alla andra noder har distribuerats i hello toobe samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d118-241">hello NFS server and all other nodes have toobe deployed in hello same virtual network.</span></span> <span data-ttu-id="4d118-242">Det ger bättre kompatibilitet med Linux-noder jämfört med en SMB-resurs.</span><span class="sxs-lookup"><span data-stu-id="4d118-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="4d118-243">Till exempel stöder filen länkar.</span><span class="sxs-lookup"><span data-stu-id="4d118-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="4d118-244">tooinstall och skapa en NFS-server gör hello i [servern för System första nätverksfilresurs slutpunkt till slutpunkt](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d118-244">tooinstall and set up an NFS server, follow hello steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="4d118-245">Till exempel skapa en NFS-resurs som heter nfs med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="4d118-245">For example, create an NFS share named nfs with hello following properties:</span></span>
   
    ![NFS-auktorisering][nfsauth]
   
    ![NFS-resursbehörigheter][nfsshare]
   
    ![NFS-NTFS-behörigheter][nfsperm]
   
    ![Egenskaper för NFS][nfsmanage]
2. <span data-ttu-id="4d118-250">Öppna Windows PowerShell-fönstret och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4d118-250">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="4d118-251">hello första kommandot skapar en mapp med namnet /nfsshared på alla noder i hello LinuxNodes grupp.</span><span class="sxs-lookup"><span data-stu-id="4d118-251">hello first command creates a folder named /nfsshared on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="4d118-252">hello andra kommandot monteringar hello NFS dela CentOS7RDMA HN: / nfs till hello mapp.</span><span class="sxs-lookup"><span data-stu-id="4d118-252">hello second command mounts hello NFS share CentOS7RDMA-HN:/nfs onto hello folder.</span></span> <span data-ttu-id="4d118-253">Här CentOS7RDMA HN: / nfs är hello fjärrsökväg NFS-resursen.</span><span class="sxs-lookup"><span data-stu-id="4d118-253">Here CentOS7RDMA-HN:/nfs is hello remote path of your NFS share.</span></span>

## <a name="how-toosubmit-jobs"></a><span data-ttu-id="4d118-254">Hur toosubmit jobb</span><span class="sxs-lookup"><span data-stu-id="4d118-254">How toosubmit jobs</span></span>
<span data-ttu-id="4d118-255">Det finns flera sätt toosubmit jobb toohello HPC Pack klustret:</span><span class="sxs-lookup"><span data-stu-id="4d118-255">There are several ways toosubmit jobs toohello HPC Pack cluster:</span></span>

* <span data-ttu-id="4d118-256">HPC Cluster Manager eller HPC Job Manager GUI</span><span class="sxs-lookup"><span data-stu-id="4d118-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="4d118-257">HPC-webbportalens</span><span class="sxs-lookup"><span data-stu-id="4d118-257">HPC web portal</span></span>
* <span data-ttu-id="4d118-258">REST API</span><span class="sxs-lookup"><span data-stu-id="4d118-258">REST API</span></span>

<span data-ttu-id="4d118-259">Jobbet toohello kluster i Azure via HPC Pack Gränssnittsbaserade verktyg och hello HPC-webbportalens är hello samma som för Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4d118-259">Job submission toohello cluster in Azure via HPC Pack GUI tools and hello HPC web portal are hello same as for Windows compute nodes.</span></span> <span data-ttu-id="4d118-260">Se [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) och [hur toosubmit jobb från en klientdator för lokala](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d118-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How toosubmit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="4d118-261">toosubmit jobb via hello REST-API finns för[skapa och skicka jobb genom att använda hello REST API i Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d118-261">toosubmit jobs via hello REST API, refer too[Creating and Submitting Jobs by Using hello REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="4d118-262">toosubmit jobb från en klient för Linux finns också toohello Python provet i hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="4d118-262">toosubmit jobs from a Linux client, also refer toohello Python sample in hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="4d118-263">Clusrun för Linux-noder</span><span class="sxs-lookup"><span data-stu-id="4d118-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="4d118-264">hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) verktyget kan använda tooexecute kommandon på Linux-noder antingen via en kommandotolk eller HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="4d118-264">hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used tooexecute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="4d118-265">Följande är några grundläggande exempel.</span><span class="sxs-lookup"><span data-stu-id="4d118-265">Following are some basic examples.</span></span>

* <span data-ttu-id="4d118-266">Visa aktuella användarnamn på alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="4d118-266">Show current user names on all nodes in hello cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="4d118-267">Installera hello **gdb** felsökningsverktyget med **yum** på alla noder i hello linuxnodes gruppen och starta sedan om hello noder efter 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="4d118-267">Install hello **gdb** debugger tool with **yum** on all nodes in hello linuxnodes group and then restart hello nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="4d118-268">Skapa ett kommandoskript som visar varje tal 1 till 10 för en sekund på varje Linux-nod i klustret hello köra det och visa utdata från hello noder omedelbart.</span><span class="sxs-lookup"><span data-stu-id="4d118-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in hello cluster, run it, and show output from hello nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="4d118-269">Du kan behöva toouse vissa escape-tecken i **clusrun** kommandon.</span><span class="sxs-lookup"><span data-stu-id="4d118-269">You might need toouse certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="4d118-270">I det här exemplet visas när du använder ^ i en kommandotolk tooescape hello ”>” symbol.</span><span class="sxs-lookup"><span data-stu-id="4d118-270">As shown in this example, use ^ in a Command Prompt tooescape hello ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4d118-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d118-271">Next steps</span></span>
* <span data-ttu-id="4d118-272">Försök att skala upp hello klustret tooa större antal noder eller försök att köra en Linux-arbetsbelastning på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4d118-272">Try scaling up hello cluster tooa larger number of nodes, or try running a Linux workload on hello cluster.</span></span> <span data-ttu-id="4d118-273">Ett exempel finns [kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="4d118-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="4d118-274">Försök med ett kluster med [RDMA-kompatibla, beräkningsintensiva VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4d118-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI workloads.</span></span> <span data-ttu-id="4d118-275">Ett exempel finns [kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-klustret i Azure](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="4d118-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="4d118-276">Om du vill arbeta med Linux-noder i ett kluster för lokala HPC Pack finns hello [TechNet vägledning](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d118-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see hello [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
