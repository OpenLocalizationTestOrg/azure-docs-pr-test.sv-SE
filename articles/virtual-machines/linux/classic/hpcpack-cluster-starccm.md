---
title: "aaaRun STAR-CCM + med HPC Pack på virtuella Linux-datorer | Microsoft Docs"
description: "Distribuera ett kluster för Microsoft HPC Pack på Azure och köra en stjärna-CCM + jobb på flera Linux compute-noder i ett nätverk med RDMA."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: 8265013cb295f53d6d4354ab2f100ef20d9f4c8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="1bba7-103">Kör STAR-CCM + med Microsoft HPC Pack på en Linux RDMA kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="1bba7-103">Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="1bba7-104">Den här artikeln visar hur toodeploy Microsoft HPC Pack kluster på Azure och kör en [CD-simuleringar STAR-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) jobb på flera Linux compute-noder som är sammankopplade med InfiniBand.</span><span class="sxs-lookup"><span data-stu-id="1bba7-104">This article shows you how toodeploy a Microsoft HPC Pack cluster on Azure and run a [CD-adapco STAR-CCM+](http://www.cd-adapco.com/products/star-ccm%C2%AE) job on multiple Linux compute nodes that are interconnected with InfiniBand.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="1bba7-105">Microsoft HPC Pack ger funktioner toorun olika storskaliga HPC och parallella program, inklusive MPI program på kluster av Microsoft Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="1bba7-105">Microsoft HPC Pack provides features toorun a variety of large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="1bba7-106">HPC Pack stöder också Linux HPC-program som körs på Linux-beräkningsnod virtuella datorer som distribueras i ett HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="1bba7-106">HPC Pack also supports running Linux HPC applications on Linux compute-node VMs that are deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="1bba7-107">En introduktion toousing Linux compute-noder med HPC Pack, finns [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="1bba7-107">For an introduction toousing Linux compute nodes with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="set-up-an-hpc-pack-cluster"></a><span data-ttu-id="1bba7-108">Konfigurera ett HPC Pack-kluster</span><span class="sxs-lookup"><span data-stu-id="1bba7-108">Set up an HPC Pack cluster</span></span>
<span data-ttu-id="1bba7-109">Hämta hello HPC Pack IaaS distribution skript från hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) och extrahera dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-109">Download hello HPC Pack IaaS deployment scripts from hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) and extract them locally.</span></span>

<span data-ttu-id="1bba7-110">Azure PowerShell är en förutsättning.</span><span class="sxs-lookup"><span data-stu-id="1bba7-110">Azure PowerShell is a prerequisite.</span></span> <span data-ttu-id="1bba7-111">Läs hello artikeln om PowerShell inte har konfigurerats på den lokala datorn [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1bba7-111">If PowerShell is not configured on your local machine, please read hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="1bba7-112">När hello detta skrivs är hello Linux bilder från hello Azure Marketplace (som innehåller hello InfiniBand drivrutiner för Azure) för SLES 12, CentOS 6.5 och CentOS 7.1.</span><span class="sxs-lookup"><span data-stu-id="1bba7-112">At hello time of this writing, hello Linux images from hello Azure Marketplace (which contains hello InfiniBand drivers for Azure) are for SLES 12, CentOS 6.5, and CentOS 7.1.</span></span> <span data-ttu-id="1bba7-113">Den här artikeln är baserad på hello användning av SLES 12.</span><span class="sxs-lookup"><span data-stu-id="1bba7-113">This article is based on hello usage of SLES 12.</span></span> <span data-ttu-id="1bba7-114">tooretrieve hello namnet på alla Linux-bilder som stöder HPC i hello Marketplace, kan du köra hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="1bba7-114">tooretrieve hello name of all Linux images that support HPC in hello Marketplace, you can run hello following PowerShell command:</span></span>

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

<span data-ttu-id="1bba7-115">hello utdata visar hello plats där dessa avbildningar är tillgängliga och hello avbildningsnamn (**avbildning**) toobe som används i mallen för distribution av hello senare.</span><span class="sxs-lookup"><span data-stu-id="1bba7-115">hello output lists hello location in which these images are available and hello image name (**ImageName**) toobe used in hello deployment template later.</span></span>

<span data-ttu-id="1bba7-116">Innan du distribuerar hello klustret har toobuild en mallfil för distribution av HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="1bba7-116">Before you deploy hello cluster, you have toobuild an HPC Pack deployment template file.</span></span> <span data-ttu-id="1bba7-117">Eftersom vi riktad ett litet kluster tas hello huvudnod hello domänkontrollant och vara värd för en lokal SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="1bba7-117">Because we're targeting a small cluster, hello head node will be hello domain controller and host a local SQL database.</span></span>

<span data-ttu-id="1bba7-118">hello vid följande mall distribuera en huvudnod, skapa en XML-fil med namnet **MyCluster.xml**, och Ersätt hello värdena för **SubscriptionId**, **StorageAccount**,  **Plats**, **VMName**, och **ServiceName** med dina.</span><span class="sxs-lookup"><span data-stu-id="1bba7-118">hello following template will deploy such a head node, create an XML file named **MyCluster.xml**, and replace hello values of **SubscriptionId**, **StorageAccount**, **Location**, **VMName**, and **ServiceName** with yours.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

<span data-ttu-id="1bba7-119">Starta hello-huvudnod skapas genom att köra hello PowerShell-kommando i en upphöjd kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="1bba7-119">Start hello head-node creation by running hello PowerShell command in an elevated command prompt:</span></span>

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

<span data-ttu-id="1bba7-120">Efter 20 minuter för too30 hello huvudnod vara redo.</span><span class="sxs-lookup"><span data-stu-id="1bba7-120">After 20 too30 minutes, hello head node should be ready.</span></span> <span data-ttu-id="1bba7-121">Du kan ansluta tooit från hello Azure-portalen genom att klicka på hello **Anslut** ikon för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1bba7-121">You can connect tooit from hello Azure portal by clicking hello **Connect** icon of hello virtual machine.</span></span>

<span data-ttu-id="1bba7-122">Du kanske slutligen toofix hello DNS-vidarebefordrare.</span><span class="sxs-lookup"><span data-stu-id="1bba7-122">You might eventually have toofix hello DNS forwarder.</span></span> <span data-ttu-id="1bba7-123">toodo starta så DNS-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="1bba7-123">toodo so, start DNS Manager.</span></span>

1. <span data-ttu-id="1bba7-124">Högerklicka på hello-servernamnet i DNS-hanteraren, Välj **egenskaper**, och klicka sedan på hello **vidarebefordrare** fliken.</span><span class="sxs-lookup"><span data-stu-id="1bba7-124">Right-click hello server name in DNS Manager, select **Properties**, and then click hello **Forwarders** tab.</span></span>
2. <span data-ttu-id="1bba7-125">Klicka på hello **redigera** knappen tooremove alla vidarebefordrare och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bba7-125">Click hello **Edit** button tooremove any forwarders, and then click **OK**.</span></span>
3. <span data-ttu-id="1bba7-126">Kontrollera att hello **använda rottips om det finns inga vidarebefordrare** kryssrutan är markerad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bba7-126">Make sure that hello **Use root hints if no forwarders are available** check box is selected, and then click **OK**.</span></span>

## <a name="set-up-linux-compute-nodes"></a><span data-ttu-id="1bba7-127">Ställ in Linux compute-noder</span><span class="sxs-lookup"><span data-stu-id="1bba7-127">Set up Linux compute nodes</span></span>
<span data-ttu-id="1bba7-128">Du distribuerar hello Linux compute-noder med hello samma Distributionsmall som du använde toocreate hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1bba7-128">You deploy hello Linux compute nodes by using hello same deployment template that you used toocreate hello head node.</span></span>

<span data-ttu-id="1bba7-129">Kopiera hello fil **MyCluster.xml** från din lokala dator toohello huvudnod och uppdatera hello **NodeCount** tagg med hello antalet noder som du vill toodeploy (< = 20).</span><span class="sxs-lookup"><span data-stu-id="1bba7-129">Copy hello file **MyCluster.xml** from your local machine toohello head node, and update hello **NodeCount** tag with hello number of nodes that you want toodeploy (<=20).</span></span> <span data-ttu-id="1bba7-130">Vara försiktig toohave tillräckligt med tillgängliga kärnor i din Azure kvot eftersom varje A9-instansen kommer att använda 16 kärnor i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="1bba7-130">Be careful toohave enough available cores in your Azure quota, because each A9 instance will consume 16 cores in your subscription.</span></span> <span data-ttu-id="1bba7-131">Du kan använda A8 instanser (8 kärnor) i stället för A9 om du vill toouse flera virtuella datorer i hello samma budget.</span><span class="sxs-lookup"><span data-stu-id="1bba7-131">You can use A8 instances (8 cores) instead of A9 if you want toouse more VMs in hello same budget.</span></span>

<span data-ttu-id="1bba7-132">Kopiera hello HPC Pack IaaS distribution skript på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1bba7-132">On hello head node, copy hello HPC Pack IaaS deployment scripts.</span></span>

<span data-ttu-id="1bba7-133">Kör hello följande Azure PowerShell-kommandon i en upphöjd kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="1bba7-133">Run hello following Azure PowerShell commands in an elevated command prompt:</span></span>

1. <span data-ttu-id="1bba7-134">Kör **Add-AzureAccount** tooconnect tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1bba7-134">Run **Add-AzureAccount** tooconnect tooyour Azure subscription.</span></span>
2. <span data-ttu-id="1bba7-135">Om du har flera prenumerationer kör **Get-AzureSubscription** toolist dem.</span><span class="sxs-lookup"><span data-stu-id="1bba7-135">If you have multiple subscriptions, run **Get-AzureSubscription** toolist them.</span></span>
3. <span data-ttu-id="1bba7-136">Ange en standard-prenumeration genom att köra hello **Välj AzureSubscription - SubscriptionName xxxx-standard** kommando.</span><span class="sxs-lookup"><span data-stu-id="1bba7-136">Set a default subscription by running hello **Select-AzureSubscription -SubscriptionName xxxx -Default** command.</span></span>
4. <span data-ttu-id="1bba7-137">Kör **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart distribuera datornoderna Linux.</span><span class="sxs-lookup"><span data-stu-id="1bba7-137">Run **.\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml** toostart deploying Linux compute nodes.</span></span>
   
   ![Huvudnod distribution i åtgärd][hndeploy]

<span data-ttu-id="1bba7-139">Öppna hello HPC Pack Cluster Manager verktyget.</span><span class="sxs-lookup"><span data-stu-id="1bba7-139">Open hello HPC Pack Cluster Manager tool.</span></span> <span data-ttu-id="1bba7-140">Efter några minuter visas regelbundet Linux compute-noder i listan över compute-noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="1bba7-140">After few minutes, Linux compute nodes will regularly appear in list of cluster compute nodes.</span></span> <span data-ttu-id="1bba7-141">Med hello klassisk distribution läge skapas IaaS-VM sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-141">With hello classic deployment mode, IaaS VMs are created sequentially.</span></span> <span data-ttu-id="1bba7-142">Så om hello antalet noder är viktigt, kan hämtar alla distribuerade ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="1bba7-142">So if hello number of nodes is important, getting them all deployed can take a significant amount of time.</span></span>

![Linux-noder i HPC Pack Cluster Manager][clustermanager]

<span data-ttu-id="1bba7-144">Nu när alla noder är igång i hello klustret, finns det ytterligare infrastruktur inställningar toomake.</span><span class="sxs-lookup"><span data-stu-id="1bba7-144">Now that all nodes are up and running in hello cluster, there are additional infrastructure settings toomake.</span></span>

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a><span data-ttu-id="1bba7-145">Konfigurera en Azure-filresurs för Windows och Linux-noder</span><span class="sxs-lookup"><span data-stu-id="1bba7-145">Set up an Azure File share for Windows and Linux nodes</span></span>
<span data-ttu-id="1bba7-146">Du kan använda hello Azure File service toostore skript och programpaket datafiler.</span><span class="sxs-lookup"><span data-stu-id="1bba7-146">You can use hello Azure File service toostore scripts, application packages, and data files.</span></span> <span data-ttu-id="1bba7-147">Azure-filen innehåller CIFS ovanpå Azure Blob storage som beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="1bba7-147">Azure File provides CIFS capabilities on top of Azure Blob storage as a persistent store.</span></span> <span data-ttu-id="1bba7-148">Tänk på att detta inte är hello mest skalbar lösning men den är hello enklaste en och kräver inte dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1bba7-148">Be aware that this is not hello most scalable solution, but it is hello simplest one and doesn’t require dedicated VMs.</span></span>

<span data-ttu-id="1bba7-149">Skapa en filresurs i Azure genom att följa hello anvisningarna i hello artikeln [Kom igång med Azure File storage i Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="1bba7-149">Create an Azure File share by following hello instructions in hello article [Get started with Azure File storage on Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="1bba7-150">Behåll hello namnet på ditt lagringskonto som **saname**, hello filresursnamn som **sharename**, och hello lagringskontonyckel som **sakey**.</span><span class="sxs-lookup"><span data-stu-id="1bba7-150">Keep hello name of your storage account as **saname**, hello file share name as **sharename**, and hello storage account key as **sakey**.</span></span>

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a><span data-ttu-id="1bba7-151">Montera hello Azure-filresurs på hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="1bba7-151">Mount hello Azure File share on hello head node</span></span>
<span data-ttu-id="1bba7-152">Öppna en upphöjd kommandotolk och kör hello efter kommandot toostore hello autentiseringsuppgifter i hello lokal dator valv:</span><span class="sxs-lookup"><span data-stu-id="1bba7-152">Open an elevated command prompt and run hello following command toostore hello credentials in hello local machine vault:</span></span>

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

<span data-ttu-id="1bba7-153">Sedan toomount hello Azure-filresurs, kör:</span><span class="sxs-lookup"><span data-stu-id="1bba7-153">Then, toomount hello Azure File share, run:</span></span>

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a><span data-ttu-id="1bba7-154">Montera hello Azure-filresurs på Linux-datornoder</span><span class="sxs-lookup"><span data-stu-id="1bba7-154">Mount hello Azure File share on Linux compute nodes</span></span>
<span data-ttu-id="1bba7-155">En användbar verktyget som medföljer HPC Pack är hello clusrun.</span><span class="sxs-lookup"><span data-stu-id="1bba7-155">One useful tool that comes with HPC Pack is hello clusrun tool.</span></span> <span data-ttu-id="1bba7-156">Du kan använda det här kommandoradsverktyget toorun hello samma kommando samtidigt på en uppsättning compute-noder.</span><span class="sxs-lookup"><span data-stu-id="1bba7-156">You can use this command-line tool toorun hello same command simultaneously on a set of compute nodes.</span></span> <span data-ttu-id="1bba7-157">I vårt fall har använt toomount hello Azure-filresurs och spara den toosurvive omstarter.</span><span class="sxs-lookup"><span data-stu-id="1bba7-157">In our case, it's used toomount hello Azure File share and persist it toosurvive reboots.</span></span>
<span data-ttu-id="1bba7-158">Kör hello följande kommandon i en upphöjd kommandotolk på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1bba7-158">In an elevated command prompt on hello head node, run hello following commands.</span></span>

<span data-ttu-id="1bba7-159">toocreate hello monteringskatalog:</span><span class="sxs-lookup"><span data-stu-id="1bba7-159">toocreate hello mount directory:</span></span>

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

<span data-ttu-id="1bba7-160">toomount hello Azure-filresurs:</span><span class="sxs-lookup"><span data-stu-id="1bba7-160">toomount hello Azure File share:</span></span>

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

<span data-ttu-id="1bba7-161">toopersist hello montera filresursen:</span><span class="sxs-lookup"><span data-stu-id="1bba7-161">toopersist hello mount share:</span></span>

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a><span data-ttu-id="1bba7-162">Installera STAR-CCM +</span><span class="sxs-lookup"><span data-stu-id="1bba7-162">Install STAR-CCM+</span></span>
<span data-ttu-id="1bba7-163">Azure VM-instanser A8- och A9 tillhandahåller InfiniBand-stöd och RDMA-funktioner.</span><span class="sxs-lookup"><span data-stu-id="1bba7-163">Azure VM instances A8 and A9 provide InfiniBand support and RDMA capabilities.</span></span> <span data-ttu-id="1bba7-164">hello kernel-drivrutiner som aktiverar dessa funktioner är tillgängliga för Windows Server 2012 R2, SUSE 12, CentOS 6.5 och CentOS 7.1 bilder i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1bba7-164">hello kernel drivers that enable those capabilities are available for Windows Server 2012 R2, SUSE 12, CentOS 6.5, and CentOS 7.1 images in hello Azure Marketplace.</span></span> <span data-ttu-id="1bba7-165">Microsoft MPI och Intel MPI (utgåva 5.x) är hello två MPI bibliotek som har stöd för de drivrutinerna i Azure.</span><span class="sxs-lookup"><span data-stu-id="1bba7-165">Microsoft MPI and Intel MPI (release 5.x) are hello two MPI libraries that support those drivers in Azure.</span></span>

<span data-ttu-id="1bba7-166">CD-simuleringar STAR-CCM + släpper 11.x och senare paketeras med Intel MPI version 5.x, så ingår InfiniBand-stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="1bba7-166">CD-adapco STAR-CCM+ release 11.x and later is bundled with Intel MPI version 5.x, so InfiniBand support for Azure is included.</span></span>

<span data-ttu-id="1bba7-167">Hämta hello Linux64 STAR-CCM + paket från hello [CD-simuleringar portal](https://steve.cd-adapco.com).</span><span class="sxs-lookup"><span data-stu-id="1bba7-167">Get hello Linux64 STAR-CCM+ package from hello [CD-adapco portal](https://steve.cd-adapco.com).</span></span> <span data-ttu-id="1bba7-168">I det här fallet används det version 11.02.010 i blandade precision.</span><span class="sxs-lookup"><span data-stu-id="1bba7-168">In our case, we used version 11.02.010 in mixed precision.</span></span>

<span data-ttu-id="1bba7-169">I hello huvudnod i hello **/hpcdata** Azure File delar, skapa ett kommandoskript som heter **setupstarccm.sh** med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="1bba7-169">On hello head node, in hello **/hpcdata** Azure File share, create a shell script named **setupstarccm.sh** with hello following content.</span></span> <span data-ttu-id="1bba7-170">Det här skriptet ska köras på varje compute-nod tooset in STAR-CCM + lokalt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-170">This script will be run on each compute node tooset up STAR-CCM+ locally.</span></span>

#### <a name="sample-setupstarcmsh-script"></a><span data-ttu-id="1bba7-171">Exempelskript för setupstarcm.sh</span><span class="sxs-lookup"><span data-stu-id="1bba7-171">Sample setupstarcm.sh script</span></span>
```
    #!/bin/bash
    # setupstarcm.sh tooset up STAR-CCM+ locally

    # Create hello CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy hello STAR-CCM package from hello file share toohello local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract hello package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without hello FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
<span data-ttu-id="1bba7-172">Nu tooset in STAR-CCM + på alla Linux compute-noder, öppna en upphöjd kommandotolk och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1bba7-172">Now, tooset up STAR-CCM+ on all your Linux compute nodes, open an elevated command prompt and run hello following command:</span></span>

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

<span data-ttu-id="1bba7-173">Du kan övervaka hello CPU-användning med hjälp av hello termisk karta av Klusterhanterare medan hello kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="1bba7-173">While hello command is running, you can monitor hello CPU usage by using hello heat map of Cluster Manager.</span></span> <span data-ttu-id="1bba7-174">Efter några minuter måste alla noder vara korrekt inställt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-174">After few minutes, all nodes should be correctly set up.</span></span>

## <a name="run-star-ccm-jobs"></a><span data-ttu-id="1bba7-175">Kör STAR-CCM + jobb</span><span class="sxs-lookup"><span data-stu-id="1bba7-175">Run STAR-CCM+ jobs</span></span>
<span data-ttu-id="1bba7-176">HPC Pack används för jobbet Schemaläggaren i ordning toorun STAR-CCM + jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-176">HPC Pack is used for its job scheduler capabilities in order toorun STAR-CCM+ jobs.</span></span> <span data-ttu-id="1bba7-177">toodo så vi behöver hello stöd för några skript som används toostart hello jobb och köra STAR-CCM +.</span><span class="sxs-lookup"><span data-stu-id="1bba7-177">toodo so, we need hello support of a few scripts that are used toostart hello job and run STAR-CCM+.</span></span> <span data-ttu-id="1bba7-178">hello indata sparas på hello Azure-filresurs första för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="1bba7-178">hello input data is kept on hello Azure File share first for simplicity.</span></span>

<span data-ttu-id="1bba7-179">hello följande PowerShell-skript har använt tooqueue en stjärna-CCM + jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-179">hello following PowerShell script is used tooqueue a STAR-CCM+ job.</span></span> <span data-ttu-id="1bba7-180">Det tar tre argument:</span><span class="sxs-lookup"><span data-stu-id="1bba7-180">It takes three arguments:</span></span>

* <span data-ttu-id="1bba7-181">hello modellnamn</span><span class="sxs-lookup"><span data-stu-id="1bba7-181">hello model name</span></span>
* <span data-ttu-id="1bba7-182">hello antalet noder toobe används</span><span class="sxs-lookup"><span data-stu-id="1bba7-182">hello number of nodes toobe used</span></span>
* <span data-ttu-id="1bba7-183">hello antalet kärnor på varje nod toobe används</span><span class="sxs-lookup"><span data-stu-id="1bba7-183">hello number of cores on each node toobe used</span></span>

<span data-ttu-id="1bba7-184">Eftersom STAR-CCM + kan fylla hello minnesbandbredd, det är oftast bättre toouse färre kärnor per datornoder och lägga till nya noder.</span><span class="sxs-lookup"><span data-stu-id="1bba7-184">Because STAR-CCM+ can fill hello memory bandwidth, it's usually better toouse fewer cores per compute nodes and add new nodes.</span></span> <span data-ttu-id="1bba7-185">hello exakta antalet kärnor per nod beror på hello processortypen och hello sammankoppling hastighet.</span><span class="sxs-lookup"><span data-stu-id="1bba7-185">hello exact number of cores per node will depend on hello processor family and hello interconnect speed.</span></span>

<span data-ttu-id="1bba7-186">hello noder allokeras endast för hello jobbet och kan inte delas med andra jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-186">hello nodes are allocated exclusively for hello job and can’t be shared with other jobs.</span></span> <span data-ttu-id="1bba7-187">hello jobbet startas inte som ett MPI-jobb direkt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-187">hello job is not started as an MPI job directly.</span></span> <span data-ttu-id="1bba7-188">Hej **runstarccm.sh** kommandoskript startar hello MPI starta.</span><span class="sxs-lookup"><span data-stu-id="1bba7-188">hello **runstarccm.sh** shell script will start hello MPI launcher.</span></span>

<span data-ttu-id="1bba7-189">hello ange modellen och hello **runstarccm.sh** skriptet lagras i hello **/hpcdata** resurs som tidigare har monterats.</span><span class="sxs-lookup"><span data-stu-id="1bba7-189">hello input model and hello **runstarccm.sh** script are stored in hello **/hpcdata** share that was previously mounted.</span></span>

<span data-ttu-id="1bba7-190">Loggfilerna namnges med hello jobb-ID och lagras i hello **/hpcdata resursen**, tillsammans med hello STAR-CCM + utgående filer.</span><span class="sxs-lookup"><span data-stu-id="1bba7-190">Log files are named with hello job ID and are stored in hello **/hpcdata share**, along with hello STAR-CCM+ output files.</span></span>

#### <a name="sample-submitstarccmjobps1-script"></a><span data-ttu-id="1bba7-191">Exempelskript för SubmitStarccmJob.ps1</span><span class="sxs-lookup"><span data-stu-id="1bba7-191">Sample SubmitStarccmJob.ps1 script</span></span>
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us hello job ID that's used tooidentify hello name of hello uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit hello job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
<span data-ttu-id="1bba7-192">Ersätt **runner.java** med din önskade stjärna-CCM + Java modellen programstart och loggning kod.</span><span class="sxs-lookup"><span data-stu-id="1bba7-192">Replace **runner.java** with your preferred STAR-CCM+ Java model launcher and logging code.</span></span>

#### <a name="sample-runstarccmsh-script"></a><span data-ttu-id="1bba7-193">Exempelskript för runstarccm.sh</span><span class="sxs-lookup"><span data-stu-id="1bba7-193">Sample runstarccm.sh script</span></span>
```
    #!/bin/bash
    echo "start"
    # hello path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set hello mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into hello hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with hello hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

<span data-ttu-id="1bba7-194">I vår test använde vi en licens Power på begäran-token.</span><span class="sxs-lookup"><span data-stu-id="1bba7-194">In our test, we used a Power-On-Demand license token.</span></span> <span data-ttu-id="1bba7-195">Denna token har tooset hello **$CDLMD_LICENSE_FILE** miljövariabeln för **1999@flex.cd-adapco.com**  och hello nyckel i hello **- podkey** alternativet hello kommandoraden .</span><span class="sxs-lookup"><span data-stu-id="1bba7-195">For that token, you have tooset hello **$CDLMD_LICENSE_FILE** environment variable too**1999@flex.cd-adapco.com** and hello key in hello **-podkey** option of hello command line.</span></span>

<span data-ttu-id="1bba7-196">Efter vissa initiering hello skript extraherar--från hello **$CCP_NODES_CORES** miljövariabler som HPC Pack in--hello listan över noder toobuild en hostfile som hello MPI programstart använder.</span><span class="sxs-lookup"><span data-stu-id="1bba7-196">After some initialization, hello script extracts--from hello **$CCP_NODES_CORES** environment variables that HPC Pack set--hello list of nodes toobuild a hostfile that hello MPI launcher uses.</span></span> <span data-ttu-id="1bba7-197">Den här hostfile innehåller hello namnlista compute-nod som används för hello jobb, ett namn per rad.</span><span class="sxs-lookup"><span data-stu-id="1bba7-197">This hostfile will contain hello list of compute node names that are used for hello job, one name per line.</span></span>

<span data-ttu-id="1bba7-198">hello format **$CCP_NODES_CORES** följer detta mönster:</span><span class="sxs-lookup"><span data-stu-id="1bba7-198">hello format of **$CCP_NODES_CORES** follows this pattern:</span></span>

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

<span data-ttu-id="1bba7-199">Där:</span><span class="sxs-lookup"><span data-stu-id="1bba7-199">Where:</span></span>

* <span data-ttu-id="1bba7-200">`<Number of nodes>`är hello antalet noder som har allokerats toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-200">`<Number of nodes>` is hello number of nodes allocated toothis job.</span></span>
* <span data-ttu-id="1bba7-201">`<Name of node_n_...>`är hello namnet på varje nod som allokerats toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-201">`<Name of node_n_...>` is hello name of each node allocated toothis job.</span></span>
* <span data-ttu-id="1bba7-202">`<Cores of node_n_...>`är hello antalet kärnor på hello nod allokerade toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-202">`<Cores of node_n_...>` is hello number of cores on hello node allocated toothis job.</span></span>

<span data-ttu-id="1bba7-203">Hej antal kärnor (**$NBCORES**) är också beräknat utifrån hello antalet noder (**$NBNODES**) och hello antalet kärnor per nod (som anges som parameter **$NBCORESPERNODE**).</span><span class="sxs-lookup"><span data-stu-id="1bba7-203">hello number of cores (**$NBCORES**) is also calculated based on hello number of nodes (**$NBNODES**) and hello number of cores per node (provided as parameter **$NBCORESPERNODE**).</span></span>

<span data-ttu-id="1bba7-204">För hello MPI alternativ är hello de som används med Intel MPI på Azure</span><span class="sxs-lookup"><span data-stu-id="1bba7-204">For hello MPI options, hello ones that are used with Intel MPI on Azure are:</span></span>

* <span data-ttu-id="1bba7-205">`-mpi intel`toospecify Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="1bba7-205">`-mpi intel` toospecify Intel MPI.</span></span>
* <span data-ttu-id="1bba7-206">`-fabric UDAPL`toouse Azure InfiniBand verb.</span><span class="sxs-lookup"><span data-stu-id="1bba7-206">`-fabric UDAPL` toouse Azure InfiniBand verbs.</span></span>
* <span data-ttu-id="1bba7-207">`-cpubind bandwidth,v`toooptimize bandbredd för MPI med stjärna-CCM +.</span><span class="sxs-lookup"><span data-stu-id="1bba7-207">`-cpubind bandwidth,v` toooptimize bandwidth for MPI with STAR-CCM+.</span></span>
* <span data-ttu-id="1bba7-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`toomake Intel MPI arbeta med Azure InfiniBand och tooset hello krävs antalet kärnor per nod.</span><span class="sxs-lookup"><span data-stu-id="1bba7-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"` toomake Intel MPI work with Azure InfiniBand, and tooset hello required number of cores per node.</span></span>
* <span data-ttu-id="1bba7-209">`-batch`toostart STAR-CCM + i batchläge med något användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1bba7-209">`-batch` toostart STAR-CCM+ in batch mode with no UI.</span></span>

<span data-ttu-id="1bba7-210">Slutligen toostart ett jobb, se till att noderna är igång och körs och är online i hanteraren för.</span><span class="sxs-lookup"><span data-stu-id="1bba7-210">Finally, toostart a job, make sure that your nodes are up and running and are online in Cluster Manager.</span></span> <span data-ttu-id="1bba7-211">Från en PowerShell-Kommandotolken kör detta:</span><span class="sxs-lookup"><span data-stu-id="1bba7-211">Then from a PowerShell command prompt, run this:</span></span>

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a><span data-ttu-id="1bba7-212">Stoppa noder</span><span class="sxs-lookup"><span data-stu-id="1bba7-212">Stop nodes</span></span>
<span data-ttu-id="1bba7-213">Vid ett senare tillfälle när du är klar med dina tester kan användas för följande HPC Pack PowerShell-kommandon toostop hello och starta noder:</span><span class="sxs-lookup"><span data-stu-id="1bba7-213">Later on, after you're done with your tests, you can use hello following HPC Pack PowerShell commands toostop and start nodes:</span></span>

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a><span data-ttu-id="1bba7-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1bba7-214">Next steps</span></span>
<span data-ttu-id="1bba7-215">Försök att köra andra Linux-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="1bba7-215">Try running other Linux workloads.</span></span> <span data-ttu-id="1bba7-216">Se exempelvis:</span><span class="sxs-lookup"><span data-stu-id="1bba7-216">For example, see:</span></span>

* [<span data-ttu-id="1bba7-217">Kör NAMD med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="1bba7-217">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
* [<span data-ttu-id="1bba7-218">Köra OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="1bba7-218">Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
