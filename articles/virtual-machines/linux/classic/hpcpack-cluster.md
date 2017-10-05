---
title: Linux compute virtuella datorer i ett kluster med HPC Pack | Microsoft Docs
description: "Lär dig hur du skapar och använder ett HPC Pack-kluster i Azure för Linux högpresterande datorbearbetning (HPC) arbetsbelastningar"
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
ms.openlocfilehash: 809d3944311badf265117d353b65642e044d900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="77f72-103">Kom igång med beräkningsnoder för Linux i ett HPC Pack-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="77f72-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="77f72-104">Konfigurera en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) kluster i Azure som innehåller en huvudnod som kör Windows Server och flera compute-noder som kör ett Linux-distribution som stöds.</span><span class="sxs-lookup"><span data-stu-id="77f72-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="77f72-105">Utforska alternativ för att flytta data mellan noder för Linux och Windows huvudnod i klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-105">Explore options to move data among the Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="77f72-106">Lär dig mer om att skicka Linux HPC-jobb till klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-106">Learn how to submit Linux HPC jobs to the cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="77f72-107">Följande diagram visar HPC Pack kluster du skapar och arbetar med på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="77f72-107">At a high level, the following diagram shows the HPC Pack cluster you create and work with.</span></span>

![HPC Pack kluster med noder för Linux][scenario]

<span data-ttu-id="77f72-109">Andra alternativ att köra Linux HPC arbetsbelastningar i Azure finns [tekniska resurser för batch- och högpresterande datorbearbetning](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="77f72-109">For other options to run Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="77f72-110">Distribuera ett HPC Pack kluster med beräkningsnoder för Linux</span><span class="sxs-lookup"><span data-stu-id="77f72-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="77f72-111">Den här artikeln lär du två alternativ för att distribuera ett HPC Pack kluster i Azure med Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-111">This article shows you two options to deploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="77f72-112">Båda metoderna kan du använda en Marketplace-avbildning av Windows Server med HPC Pack för att skapa huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="77f72-112">Both methods use a Marketplace image of Windows Server with HPC Pack to create the head node.</span></span> 

* <span data-ttu-id="77f72-113">**Azure Resource Manager-mall** -använda en mall från Azure Marketplace, eller en mall för Snabbstart från gemenskapen, för att automatisera skapandet av klustret i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="77f72-113">**Azure Resource Manager template** - Use a template from the Azure Marketplace, or a quickstart template from the community, to automate creation of the cluster in the Resource Manager deployment model.</span></span> <span data-ttu-id="77f72-114">Till exempel den [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mallen i Azure Marketplace skapar en komplett HPC Pack klustret infrastruktur för Linux HPC arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="77f72-114">For example, the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="77f72-115">**PowerShell-skriptet** -användning i [Microsoft HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**ny HpcIaaSCluster.ps1**) att automatisera en distribution för hela klustret i den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="77f72-115">**PowerShell script** - Use the [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) to automate a complete cluster deployment in the classic deployment model.</span></span> <span data-ttu-id="77f72-116">Azure PowerShell-skript använder ett HPC Pack VM-avbildning i Azure Marketplace för snabb distribution och ger en omfattande uppsättning konfigurationsparametrar för att distribuera datornoderna Linux.</span><span class="sxs-lookup"><span data-stu-id="77f72-116">This Azure PowerShell script uses an HPC Pack VM image in the Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters to deploy Linux compute nodes.</span></span>

<span data-ttu-id="77f72-117">Mer information om HPC Pack distributionsalternativ för kluster i Azure finns [alternativ för att skapa och hantera högpresterande datorbearbetning (HPC) kluster i Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77f72-117">For more information about HPC Pack cluster deployment options in Azure, see [Options to create and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="77f72-118">Krav</span><span class="sxs-lookup"><span data-stu-id="77f72-118">Prerequisites</span></span>
* <span data-ttu-id="77f72-119">**Azure-prenumeration** -du kan använda en prenumeration i Azure Global eller Azure Kina service.</span><span class="sxs-lookup"><span data-stu-id="77f72-119">**Azure subscription** - You can use a subscription in either the Azure Global or Azure China service.</span></span> <span data-ttu-id="77f72-120">Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="77f72-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="77f72-121">**Kärnor kvoten** – du kan behöva öka kvoten för kärnor, särskilt om du väljer att distribuera flera klusternoder med flera kärnor VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="77f72-121">**Cores quota** - You might need to increase the quota of cores, especially if you choose to deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="77f72-122">Om du vill öka en kvot öppnar du en online-kund supportbegäran utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="77f72-122">To increase a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="77f72-123">**Linux-distributioner** -för närvarande HPC Pack stöder följande Linux-distributioner för compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-123">**Linux distributions** - Currently HPC Pack supports the following Linux distributions for compute nodes.</span></span> <span data-ttu-id="77f72-124">Du kan använda dessa distributioner Marketplace-versioner där det är tillgängligt eller ange egna.</span><span class="sxs-lookup"><span data-stu-id="77f72-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="77f72-125">**CentOS-baserade**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span><span class="sxs-lookup"><span data-stu-id="77f72-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="77f72-126">**Red Hat Enterprise Linux**: 6.7, 6.8 och 7.2</span><span class="sxs-lookup"><span data-stu-id="77f72-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="77f72-127">**SUSE Linux Enterprise Server**: SLES 12 SLES 12 (Premium) SLES 12 SP1, SLES 12 SP1 (Premium) SLES 12 för HPC SLES 12 för HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="77f72-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="77f72-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="77f72-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="77f72-129">Ange en SUSE Linux Enterprise Server 12 HPC eller CentOS-baserade HPC-avbildning från Azure Marketplace för att använda RDMA-Azure-nätverk med en storlek på RDMA-kompatibla Virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="77f72-129">To use the Azure RDMA network with one of the RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from the Azure Marketplace.</span></span> <span data-ttu-id="77f72-130">Mer information finns i [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77f72-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="77f72-131">Ytterligare krav för att distribuera klustret med hjälp av distributionsskriptet HPC Pack IaaS:</span><span class="sxs-lookup"><span data-stu-id="77f72-131">Additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="77f72-132">**Klientdatorn** -behöver du en Windows-baserad klient-dator för att köra skriptet för distribution av klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-132">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="77f72-133">**Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="77f72-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="77f72-134">**HPC Pack IaaS distributionsskriptet** – hämta och packa upp den senaste versionen av skriptet från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="77f72-134">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="77f72-135">Du kan kontrollera vilken version av skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="77f72-135">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="77f72-136">Den här artikeln är baserat på version 4.4.1 eller senare av skriptet.</span><span class="sxs-lookup"><span data-stu-id="77f72-136">This article is based on version 4.4.1 or later of the script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="77f72-137">Distributionsalternativ 1.</span><span class="sxs-lookup"><span data-stu-id="77f72-137">Deployment option 1.</span></span> <span data-ttu-id="77f72-138">Använd en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="77f72-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="77f72-139">Gå till den [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mall i Azure Marketplace och klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="77f72-139">Go to the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="77f72-140">Granska informationen i Azure-portalen och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="77f72-140">In the Azure portal, review the information and then click **Create**.</span></span>
   
    ![Skapa Portal][portal]
3. <span data-ttu-id="77f72-142">På den **grunderna** bladet anger du ett namn för klustret, vilket även namnen huvudnod VM.</span><span class="sxs-lookup"><span data-stu-id="77f72-142">On the **Basics** blade, enter a name for the cluster, which also names the head node VM.</span></span> <span data-ttu-id="77f72-143">Du kan välja en befintlig resursgrupp eller skapa en grupp för distribution på en plats som är tillgängliga för dig.</span><span class="sxs-lookup"><span data-stu-id="77f72-143">You can choose an existing resource group or create a group for the deployment in a location that's available to you.</span></span> <span data-ttu-id="77f72-144">Platsen påverkar tillgängligheten för vissa VM-storlekar och andra Azure-tjänster (se [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="77f72-144">The location affects the availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="77f72-145">På den **Head noden inställningar** bladet för en första distribution, kan du oftast acceptera standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="77f72-145">On the **Head node settings** blade, for a first deployment, you can generally accept the default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="77f72-146">Den **efter konfigurationsskript URL** är en valfri inställning och anger ett offentligt tillgängliga Windows PowerShell-skript som du vill köra på huvudnoden VM när den körs.</span><span class="sxs-lookup"><span data-stu-id="77f72-146">The **Post-configuration script URL** is an optional setting to specify a publicly available Windows PowerShell script that you want to run on the head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="77f72-147">På den **Compute-nod inställningar** bladet Välj en namnkonflikt mönstret för noder, antal och storleken på noderna och Linux-distribution ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="77f72-147">On the **Compute node settings** blade, select a naming pattern for the nodes, the number and size of the nodes, and the Linux distribution to deploy.</span></span>
6. <span data-ttu-id="77f72-148">På den **infrastrukturinställningar** bladet ange namn för virtuellt nätverk och Active Directory domän, domän och VM-administratörsbehörighet och ett namngivningsmönster för storage-konton.</span><span class="sxs-lookup"><span data-stu-id="77f72-148">On the **Infrastructure settings** blade, enter names for the virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for the storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="77f72-149">HPC Pack använder Active Directory-domänen för att autentisera användare för klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-149">HPC Pack uses the Active Directory domain to authenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="77f72-150">När verifieringstesterna köra och granska av användningsvillkoren, klickar du på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="77f72-150">After the validation tests run and you review the terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a><span data-ttu-id="77f72-151">Distributionsalternativ 2.</span><span class="sxs-lookup"><span data-stu-id="77f72-151">Deployment option 2.</span></span> <span data-ttu-id="77f72-152">Använd skript för IaaS-distribution</span><span class="sxs-lookup"><span data-stu-id="77f72-152">Use the IaaS deployment script</span></span>
<span data-ttu-id="77f72-153">Följande är ytterligare krav för att distribuera klustret med hjälp av distributionsskriptet HPC Pack IaaS:</span><span class="sxs-lookup"><span data-stu-id="77f72-153">Following are additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="77f72-154">**Klientdatorn** -behöver du en Windows-baserad klient-dator för att köra skriptet för distribution av klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-154">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="77f72-155">**Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="77f72-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="77f72-156">**HPC Pack IaaS distributionsskriptet** – hämta och packa upp den senaste versionen av skriptet från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="77f72-156">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="77f72-157">Du kan kontrollera vilken version av skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="77f72-157">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="77f72-158">Den här artikeln är baserat på version 4.4.1 eller senare av skriptet.</span><span class="sxs-lookup"><span data-stu-id="77f72-158">This article is based on version 4.4.1 or later of the script.</span></span>

<span data-ttu-id="77f72-159">**XML-konfigurationsfilen**</span><span class="sxs-lookup"><span data-stu-id="77f72-159">**XML configuration file**</span></span>

<span data-ttu-id="77f72-160">HPC Pack IaaS-distributionsskriptet använder en XML-konfigurationsfil som indata för att beskriva HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="77f72-160">The HPC Pack IaaS deployment script uses an XML configuration file as input to describe the  HPC cluster.</span></span> <span data-ttu-id="77f72-161">Följande exempel konfigurationsfil anger ett mindre kluster som består av en huvudnod i HPC Pack och två storlek A7 CentOS 7.0 Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-161">The following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="77f72-162">Ändra den här filen behövs för din miljö och önskade klusterkonfigurationen och spara den med ett namn, till exempel HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="77f72-162">Modify the file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="77f72-163">Till exempel måste ange namnet på din prenumeration och en unik lagringskontonamnet och molntjänster tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="77f72-163">For example, you need to supply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="77f72-164">Dessutom kanske du vill välja en annan Linux avbildning som stöds för compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-164">Additionally, you might want to choose a different supported Linux image for the compute nodes.</span></span> <span data-ttu-id="77f72-165">Mer information om elementen i konfigurationsfilen finns i filen Manual.rtf i mappen skript och [skapa ett HPC-kluster med HPC Pack IaaS-distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77f72-165">For more information about the elements in the configuration file, see the Manual.rtf file in the script folder and [Create an HPC cluster with the HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

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

<span data-ttu-id="77f72-166">**Att köra skriptet HPC Pack IaaS-distribution**</span><span class="sxs-lookup"><span data-stu-id="77f72-166">**To run the HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="77f72-167">Öppna Windows PowerShell på datorn som administratör.</span><span class="sxs-lookup"><span data-stu-id="77f72-167">Open Windows PowerShell on the client computer as an administrator.</span></span>
2. <span data-ttu-id="77f72-168">Ändra katalogen till mappen där skriptet är installerat (E:\IaaSClusterScript i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="77f72-168">Change directory to the folder where the script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="77f72-169">Kör följande kommando för att distribuera HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="77f72-169">Run the following command to deploy the HPC Pack cluster.</span></span> <span data-ttu-id="77f72-170">Det här exemplet förutsätter att konfigurationsfilen finns i E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="77f72-170">This example assumes that the configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="77f72-171">a.</span><span class="sxs-lookup"><span data-stu-id="77f72-171">a.</span></span> <span data-ttu-id="77f72-172">Eftersom den **AdminPassword** har inte angetts i kommandot ovan uppmanas du att ange lösenord för användaren *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="77f72-172">Because the **AdminPassword** is not specified in the preceding command, you are prompted to enter the password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="77f72-173">b.</span><span class="sxs-lookup"><span data-stu-id="77f72-173">b.</span></span> <span data-ttu-id="77f72-174">Skriptet startar sedan att verifiera konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="77f72-174">The script then starts to validate the configuration file.</span></span> <span data-ttu-id="77f72-175">Det kan ta upp till flera minuter beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="77f72-175">It can take up to several minutes depending on the network connection.</span></span>
   
    ![Validering][validate]
   
    <span data-ttu-id="77f72-177">c.</span><span class="sxs-lookup"><span data-stu-id="77f72-177">c.</span></span> <span data-ttu-id="77f72-178">När verifieringar skicka visar skriptet klusterresurserna som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="77f72-178">After validations pass, the script lists the cluster resources to create.</span></span> <span data-ttu-id="77f72-179">Ange *Y* att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="77f72-179">Enter *Y* to continue.</span></span>
   
    ![Resurser][resources]
   
    <span data-ttu-id="77f72-181">d.</span><span class="sxs-lookup"><span data-stu-id="77f72-181">d.</span></span> <span data-ttu-id="77f72-182">Skriptet börjar distribuera HPC Pack klustret och slutför konfigurationen utan ytterligare manuella steg.</span><span class="sxs-lookup"><span data-stu-id="77f72-182">The script starts to deploy the HPC Pack cluster and completes the configuration without further manual steps.</span></span> <span data-ttu-id="77f72-183">Skriptet kan köras i flera minuter.</span><span class="sxs-lookup"><span data-stu-id="77f72-183">The script can run for several minutes.</span></span>
   
    ![Distribuera][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="77f72-185">I det här exemplet skriptet genererar en loggfil automatiskt eftersom den **- LogFile** parameter har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="77f72-185">In this example, the script generates a log file automatically since the **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="77f72-186">Loggarna inte är skriven i realtid men har samlats in i slutet av verifieringen och distribution.</span><span class="sxs-lookup"><span data-stu-id="77f72-186">The logs aren't written in real time, but are collected at the end of the validation and the deployment.</span></span> <span data-ttu-id="77f72-187">Vissa loggar går förlorade om PowerShell processen stoppas när skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="77f72-187">If the PowerShell process is stopped while the script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-to-the-head-node"></a><span data-ttu-id="77f72-188">Ansluta till huvudnoden</span><span class="sxs-lookup"><span data-stu-id="77f72-188">Connect to the head node</span></span>
<span data-ttu-id="77f72-189">När du har distribuerat HPC Pack klustret i Azure, [ansluta med Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) till huvudnod VM med domänen autentiseringsuppgifter du anges när du distribuerade klustret (till exempel *hpc\\clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="77f72-189">After you deploy the HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to the head node VM using the domain credentials you provided when you deployed the cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="77f72-190">Du kan hantera klustret från huvudnod.</span><span class="sxs-lookup"><span data-stu-id="77f72-190">You manage the cluster from the head node.</span></span>

<span data-ttu-id="77f72-191">Starta HPC Cluster Manager för att kontrollera status för HPC Pack klustret på noden head.</span><span class="sxs-lookup"><span data-stu-id="77f72-191">On the head node, start HPC Cluster Manager to check the status of the HPC Pack cluster.</span></span> <span data-ttu-id="77f72-192">Du kan hantera och övervaka Linux compute-noder på samma sätt som du arbetar med Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-192">You can manage and monitor Linux compute nodes the same way you work with Windows compute nodes.</span></span> <span data-ttu-id="77f72-193">Se exempelvis Linux-noder som anges i **resurshantering** (dessa noder distribueras med den **LinuxNode** mall).</span><span class="sxs-lookup"><span data-stu-id="77f72-193">For example, you see the Linux nodes listed in **Resource Management** (these nodes are deployed with the **LinuxNode** template).</span></span>

![Hantering][management]

<span data-ttu-id="77f72-195">Du också se Linux-noder i den **termisk karta** vyn.</span><span class="sxs-lookup"><span data-stu-id="77f72-195">You also see the Linux nodes in the **Heat Map** view.</span></span>

![Termisk karta][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="77f72-197">Hur du flyttar data i ett kluster med noder som Linux</span><span class="sxs-lookup"><span data-stu-id="77f72-197">How to move data in a cluster with Linux nodes</span></span>
<span data-ttu-id="77f72-198">Du har flera alternativ för att flytta data mellan noder för Linux och Windows huvudnod i klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-198">You have several choices to move data among Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="77f72-199">Här följer tre vanliga metoder som beskrivs i detalj i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="77f72-199">Here are three common methods, described in more detail in the following sections:</span></span>

* <span data-ttu-id="77f72-200">**Azure File** -visar den hantera SMB-filresursen som lagrar datafiler i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="77f72-200">**Azure File** - Exposes a managed SMB file share to store data files in Azure storage.</span></span> <span data-ttu-id="77f72-201">Windows-noderna och Linux-noder kan montera en filresurs på Azure som en enhet eller mapp på samma gång, även om de har distribuerats i olika virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="77f72-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at the same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="77f72-202">**Huvudnod SMB dela** -monterar en standard Windows delad mapp för huvudnoden på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-202">**Head node SMB share** - Mounts a standard Windows shared folder of the head node on Linux nodes.</span></span>
* <span data-ttu-id="77f72-203">**HEAD nod NFS-servern** -är en lösning för fildelning för en blandad miljö för Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="77f72-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="77f72-204">Azure File storage</span><span class="sxs-lookup"><span data-stu-id="77f72-204">Azure File storage</span></span>
<span data-ttu-id="77f72-205">Den [Azure File](https://azure.microsoft.com/services/storage/files/) service exponerar filresurser med hjälp av SMB 2.1 standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="77f72-205">The [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using the standard SMB 2.1 protocol.</span></span> <span data-ttu-id="77f72-206">Virtuella Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via File storage API.</span><span class="sxs-lookup"><span data-stu-id="77f72-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through the File storage API.</span></span> 

<span data-ttu-id="77f72-207">Detaljerade anvisningar om hur du skapar en filresurs på Azure och montera på huvudnoden finns [Kom igång med Azure File storage i Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="77f72-207">For detailed steps to create an Azure File share and mount it on the head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="77f72-208">Montera filresursen Azure på Linux-noder, se [använda Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="77f72-208">To mount the Azure File share on the Linux nodes, see [How to use Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="77f72-209">Om du vill konfigurera spara anslutningar, se [Persisting anslutningar till Microsoft Azure-filer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="77f72-209">To set up persisting connections, see [Persisting connections to Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="77f72-210">I följande exempel skapar du en filresurs på Azure på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="77f72-210">In the following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="77f72-211">Om du vill ansluta till resursen på huvudnoden, öppna en kommandotolk och ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="77f72-211">To mount the share on the head node, open a Command Prompt and enter the following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="77f72-212">I det här exemplet allvhdsje är namnet på ditt lagringskonto, storageaccountkey är din lagringskontonyckel och rdma är Azure File resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="77f72-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is the Azure File share name.</span></span> <span data-ttu-id="77f72-213">Azure-filresursen är monterad som Z: på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="77f72-213">The Azure File share is mounted as Z: on the head node.</span></span>

<span data-ttu-id="77f72-214">Montera filresursen Azure på Linux-noder, kör en **clusrun** på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="77f72-214">To mount the Azure File share on Linux nodes, run a **clusrun** command on the head node.</span></span> <span data-ttu-id="77f72-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  är HPC Pack användbart att utföra administrativa uppgifter på flera noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool to carry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="77f72-216">(Se även [Clusrun för Linux-noder](#Clusrun-for-Linux-nodes) i den här artikeln.)</span><span class="sxs-lookup"><span data-stu-id="77f72-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="77f72-217">Öppna Windows PowerShell-fönstret och ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="77f72-217">Open a Windows PowerShell window and enter the following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="77f72-218">Det första kommandot skapar en mapp med namnet /rdma på alla noder i gruppen LinuxNodes.</span><span class="sxs-lookup"><span data-stu-id="77f72-218">The first command creates a folder named /rdma on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="77f72-219">Det andra kommandot monterar Azure File share allvhdsjw.file.core.windows.net/rdma till mappen /rdma med dir och filen bitar till 777.</span><span class="sxs-lookup"><span data-stu-id="77f72-219">The second command mounts the Azure File share allvhdsjw.file.core.windows.net/rdma onto the /rdma folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="77f72-220">I det andra kommandot allvhdsje är namnet på ditt lagringskonto och storageaccountkey är din lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="77f72-220">In the second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="77f72-221">Den ”\\`” symbol i det andra kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="77f72-221">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="77f72-222">”\\`”, innebär att ””, (kommatecken tecken) är en del av kommandot.</span><span class="sxs-lookup"><span data-stu-id="77f72-222">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="77f72-223">Huvudnod resursen</span><span class="sxs-lookup"><span data-stu-id="77f72-223">Head node share</span></span>
<span data-ttu-id="77f72-224">Du kan också montera en delad mapp för huvudnoden på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-224">Alternatively, mount a shared folder of the head node on Linux nodes.</span></span> <span data-ttu-id="77f72-225">En resurs som innehåller det enklaste sättet att dela filer, men huvudnoden och alla Linux-noder måste distribueras i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="77f72-225">A share provides the simplest way to share files, but the head node and all Linux nodes must be deployed in the same virtual network.</span></span> <span data-ttu-id="77f72-226">Här följer stegen.</span><span class="sxs-lookup"><span data-stu-id="77f72-226">Here are the steps.</span></span>

1. <span data-ttu-id="77f72-227">Skapa en mapp på huvudnoden och dela den för alla med läs-/ skrivbehörighet.</span><span class="sxs-lookup"><span data-stu-id="77f72-227">Create a folder on the head node and share it to Everyone with Read/Write permissions.</span></span> <span data-ttu-id="77f72-228">Till exempel dela D:\OpenFOAM på huvudnoden som \\CentOS7RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="77f72-228">For example, share D:\OpenFOAM on the head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="77f72-229">Här är CentOS7RDMA HN värdnamnet för huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="77f72-229">Here CentOS7RDMA-HN is the hostname of the head node.</span></span>
   
    ![Filresursbehörigheter][fileshareperms]
   
    ![Fildelning][filesharing]
2. <span data-ttu-id="77f72-232">Öppna Windows PowerShell-fönstret och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="77f72-232">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="77f72-233">Det första kommandot skapar en mapp med namnet /openfoam på alla noder i gruppen LinuxNodes.</span><span class="sxs-lookup"><span data-stu-id="77f72-233">The first command creates a folder named /openfoam on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="77f72-234">Det andra kommandot monterar delade mappen //CentOS7RDMA-HN/OpenFOAM till mappen med dir och filen bitar till 777.</span><span class="sxs-lookup"><span data-stu-id="77f72-234">The second command mounts the shared folder //CentOS7RDMA-HN/OpenFOAM onto the folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="77f72-235">Användarnamnet och lösenordet i kommandot ska användarnamn och lösenord för en kluster-användare i huvudnod.</span><span class="sxs-lookup"><span data-stu-id="77f72-235">The username and password in the command should be the username and password of a cluster user on the head node.</span></span> <span data-ttu-id="77f72-236">(Se [Lägg till eller ta bort klustret användare](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="77f72-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="77f72-237">Den ”\\`” symbol i det andra kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="77f72-237">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="77f72-238">”\\`”, innebär att ””, (kommatecken tecken) är en del av kommandot.</span><span class="sxs-lookup"><span data-stu-id="77f72-238">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="77f72-239">NFS-server</span><span class="sxs-lookup"><span data-stu-id="77f72-239">NFS server</span></span>
<span data-ttu-id="77f72-240">NFS-tjänsten kan du dela och flytta filer mellan datorer som kör operativsystemet Windows Server 2012 med SMB-protokollet och Linux-baserade datorer med hjälp av NFS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="77f72-240">The NFS service enables you to share and migrate files between computers running the Windows Server 2012 operating system using the SMB protocol and Linux-based computers using the NFS protocol.</span></span> <span data-ttu-id="77f72-241">NFS-servern och alla andra noder måste distribueras i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="77f72-241">The NFS server and all other nodes have to be deployed in the same virtual network.</span></span> <span data-ttu-id="77f72-242">Det ger bättre kompatibilitet med Linux-noder jämfört med en SMB-resurs.</span><span class="sxs-lookup"><span data-stu-id="77f72-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="77f72-243">Till exempel stöder filen länkar.</span><span class="sxs-lookup"><span data-stu-id="77f72-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="77f72-244">Om du vill installera och konfigurera NFS-servern, följer du stegen i [servern för System första nätverksfilresurs slutpunkt till slutpunkt](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="77f72-244">To install and set up an NFS server, follow the steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="77f72-245">Till exempel skapa en NFS-resurs med namnet nfs med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="77f72-245">For example, create an NFS share named nfs with the following properties:</span></span>
   
    ![NFS-auktorisering][nfsauth]
   
    ![NFS-resursbehörigheter][nfsshare]
   
    ![NFS-NTFS-behörigheter][nfsperm]
   
    ![Egenskaper för NFS][nfsmanage]
2. <span data-ttu-id="77f72-250">Öppna Windows PowerShell-fönstret och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="77f72-250">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="77f72-251">Det första kommandot skapar en mapp med namnet /nfsshared på alla noder i gruppen LinuxNodes.</span><span class="sxs-lookup"><span data-stu-id="77f72-251">The first command creates a folder named /nfsshared on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="77f72-252">Det andra kommandot monterar NFS-resurs CentOS7RDMA HN: / nfs till mappen.</span><span class="sxs-lookup"><span data-stu-id="77f72-252">The second command mounts the NFS share CentOS7RDMA-HN:/nfs onto the folder.</span></span> <span data-ttu-id="77f72-253">Här CentOS7RDMA HN: / nfs är fjärrsökväg NFS-resursen.</span><span class="sxs-lookup"><span data-stu-id="77f72-253">Here CentOS7RDMA-HN:/nfs is the remote path of your NFS share.</span></span>

## <a name="how-to-submit-jobs"></a><span data-ttu-id="77f72-254">Hur du skickar in jobb</span><span class="sxs-lookup"><span data-stu-id="77f72-254">How to submit jobs</span></span>
<span data-ttu-id="77f72-255">Det finns flera sätt att skicka jobb till klustret HPC Pack:</span><span class="sxs-lookup"><span data-stu-id="77f72-255">There are several ways to submit jobs to the HPC Pack cluster:</span></span>

* <span data-ttu-id="77f72-256">HPC Cluster Manager eller HPC Job Manager GUI</span><span class="sxs-lookup"><span data-stu-id="77f72-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="77f72-257">HPC-webbportalens</span><span class="sxs-lookup"><span data-stu-id="77f72-257">HPC web portal</span></span>
* <span data-ttu-id="77f72-258">REST API</span><span class="sxs-lookup"><span data-stu-id="77f72-258">REST API</span></span>

<span data-ttu-id="77f72-259">Skicka jobb till klustret i Azure via HPC Pack Gränssnittsbaserade verktyg och HPC-webbportalens är desamma som för Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="77f72-259">Job submission to the cluster in Azure via HPC Pack GUI tools and the HPC web portal are the same as for Windows compute nodes.</span></span> <span data-ttu-id="77f72-260">Se [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) och [så att skicka jobb från en klientdator för lokala](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77f72-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How to submit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="77f72-261">För att skicka jobb via REST-API, referera till [skapa och skicka jobb med hjälp av REST-API i Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="77f72-261">To submit jobs via the REST API, refer to [Creating and Submitting Jobs by Using the REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="77f72-262">För att skicka jobb från en Linux-klient, även gå till Python-exempel i den [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="77f72-262">To submit jobs from a Linux client, also refer to the Python sample in the [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="77f72-263">Clusrun för Linux-noder</span><span class="sxs-lookup"><span data-stu-id="77f72-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="77f72-264">HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) verktyget kan användas för att köra kommandon på Linux-noder antingen via en kommandotolk eller HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="77f72-264">The HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used to execute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="77f72-265">Följande är några grundläggande exempel.</span><span class="sxs-lookup"><span data-stu-id="77f72-265">Following are some basic examples.</span></span>

* <span data-ttu-id="77f72-266">Visa aktuella användarnamn på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-266">Show current user names on all nodes in the cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="77f72-267">Installera den **gdb** felsökningsverktyget med **yum** på alla noder i linuxnodes gruppen och starta sedan om noderna efter 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="77f72-267">Install the **gdb** debugger tool with **yum** on all nodes in the linuxnodes group and then restart the nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="77f72-268">Skapa ett kommandoskript som visar varje tal 1 till 10 för en sekund på varje Linux-nod i klustret, kör den och visa utdata från noderna omedelbart.</span><span class="sxs-lookup"><span data-stu-id="77f72-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in the cluster, run it, and show output from the nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="77f72-269">Du kan behöva använda vissa escape-tecken i **clusrun** kommandon.</span><span class="sxs-lookup"><span data-stu-id="77f72-269">You might need to use certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="77f72-270">I det här exemplet visas när du använder ^ i en kommandotolk för att undanta den ”>” symbol.</span><span class="sxs-lookup"><span data-stu-id="77f72-270">As shown in this example, use ^ in a Command Prompt to escape the ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="77f72-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77f72-271">Next steps</span></span>
* <span data-ttu-id="77f72-272">Prova att skala upp kluster till ett större antal noder eller körs en Linux-arbetsbelastning i klustret.</span><span class="sxs-lookup"><span data-stu-id="77f72-272">Try scaling up the cluster to a larger number of nodes, or try running a Linux workload on the cluster.</span></span> <span data-ttu-id="77f72-273">Ett exempel finns [kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="77f72-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="77f72-274">Försök med ett kluster med [RDMA-kompatibla, beräkningsintensiva VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) att köra MPI arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="77f72-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to run MPI workloads.</span></span> <span data-ttu-id="77f72-275">Ett exempel finns [kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-klustret i Azure](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="77f72-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="77f72-276">Om du vill arbeta med Linux-noder i ett lokalt HPC Pack kluster finns i [TechNet vägledning](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="77f72-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see the [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

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
