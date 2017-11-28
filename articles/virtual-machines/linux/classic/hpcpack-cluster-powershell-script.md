---
title: aaaPowerShell skript toodeploy Linux HPC-kluster | Microsoft Docs
description: "Köra ett PowerShell-skript toodeploy ett Linux HPC Pack 2012 R2-kluster i virtuella Azure-datorer"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 885b03fa2fd604827dc388803fc21debab730979
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="3a931-103">Skapa ett Linux högpresterande datorbearbetning (HPC) kluster med hello HPC Pack IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="3a931-103">Create a Linux high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="3a931-104">Kör hello HPC Pack IaaS distribution PowerShell-skriptet toodeploy en fullständig HPC Pack 2012 R2-kluster för Linux-arbetsbelastningar på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="3a931-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Linux workloads in Azure virtual machines.</span></span> <span data-ttu-id="3a931-105">hello klustret består av en Active Directory-anslutna huvudnod som kör Windows Server och Microsoft HPC Pack och compute-noder som kör något av hello Linux-distributioner som stöds av HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="3a931-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and compute nodes that run one of hello Linux distributions supported by HPC Pack.</span></span> <span data-ttu-id="3a931-106">Om du vill toodeploy ett HPC Pack kluster i Azure för Windows-arbetsbelastningar, se [skapa ett Windows HPC-kluster med hello HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a931-106">If you want toodeploy an HPC Pack cluster in Azure for Windows workloads, see [Create a Windows HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="3a931-107">Du kan också använda en Azure Resource Manager mallen toodeploy ett HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="3a931-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="3a931-108">Ett exempel finns [skapa ett HPC-kluster med Linux datornoderna](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span><span class="sxs-lookup"><span data-stu-id="3a931-108">For an example, see [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3a931-109">hello PowerShell-skriptet som beskrivs i den här artikeln skapar ett Microsoft HPC Pack 2012 R2-kluster i Azure med hjälp av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3a931-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="3a931-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3a931-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="3a931-111">Dessutom stöder inte hello-skriptet som beskrivs i den här artikeln HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="3a931-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a><span data-ttu-id="3a931-112">Exempel konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="3a931-112">Example configuration file</span></span>
<span data-ttu-id="3a931-113">hello följande konfigurationsfil skapar en domänkontrollant och domänskog och distribuerar ett HPC Pack-kluster som har en huvudnod med lokala databaser och 10 Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="3a931-113">hello following configuration file creates a domain controller and domain forest and deploys an HPC Pack cluster which has one head node with local databases and 10 Linux compute nodes.</span></span> <span data-ttu-id="3a931-114">Alla hello molntjänster skapas direkt i hello Östasien plats.</span><span class="sxs-lookup"><span data-stu-id="3a931-114">All hello cloud services are created directly in hello East Asia location.</span></span> <span data-ttu-id="3a931-115">hello Linux datornoderna skapas i två molntjänster och två storage-konton (det vill säga *MyLnxCN 0001* till *MyLnxCN 0005* i *MyLnxCNService01* och *mylnxstorage01*, och *MyLnxCN 0006* till *MyLnxCN 0010* i *MyLnxCNService02* och *mylnxstorage02* ).</span><span class="sxs-lookup"><span data-stu-id="3a931-115">hello Linux compute nodes are created in two cloud services and two storage accounts (that is, *MyLnxCN-0001* to *MyLnxCN-0005* in *MyLnxCNService01* and *mylnxstorage01*, and *MyLnxCN-0006* to *MyLnxCN-0010* in *MyLnxCNService02* and *mylnxstorage02*).</span></span> <span data-ttu-id="3a931-116">hello compute-noder har skapats från en OpenLogic CentOS version 7.0 Linux-bild.</span><span class="sxs-lookup"><span data-stu-id="3a931-116">hello compute nodes are created from an OpenLogic CentOS version 7.0 Linux image.</span></span> 

<span data-ttu-id="3a931-117">Ersätt värdena för prenumerationsnamn och hello namn på kontot och tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3a931-117">Substitute your own values for your subscription name and hello account and service names.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a><span data-ttu-id="3a931-118">Felsökning</span><span class="sxs-lookup"><span data-stu-id="3a931-118">Troubleshooting</span></span>
* <span data-ttu-id="3a931-119">**”Virtuella nätverk finns inte” fel**.</span><span class="sxs-lookup"><span data-stu-id="3a931-119">**“VNet doesn’t exist” error**.</span></span> <span data-ttu-id="3a931-120">Om du kör hello HPC Pack IaaS distribution skriptet toodeploy flera kluster i Azure samtidigt under en prenumeration, en eller flera distributioner kan misslyckas med felet hello ”VNet *VNet\_namn* finns inte”.</span><span class="sxs-lookup"><span data-stu-id="3a931-120">If you run hello HPC Pack IaaS deployment script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="3a931-121">Om det här felet uppstår kör hello skript för distribution av hello misslyckades.</span><span class="sxs-lookup"><span data-stu-id="3a931-121">If this error occurs, rerun hello script for hello failed deployment.</span></span>
* <span data-ttu-id="3a931-122">**Problem att komma åt hello Internet från hello virtuella Azure-nätverket**.</span><span class="sxs-lookup"><span data-stu-id="3a931-122">**Problem accessing hello Internet from hello Azure virtual network**.</span></span> <span data-ttu-id="3a931-123">Om skapar du ett HPC Pack kluster med en ny domänkontrollant med hjälp av hello distributionsskriptet eller om du befordrar en huvudnod VM toodomain domänkontrollant manuellt, kan det uppstå problem med att ansluta hello virtuella datorer i hello virtuella Azure-nätverket toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="3a931-123">If you create an HPC Pack cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs in hello Azure virtual network toohello Internet.</span></span> <span data-ttu-id="3a931-124">Detta kan inträffa om en vidarebefordrare DNS-server konfigureras automatiskt på hello domänkontrollant och DNS-servern vidarebefordraren löses inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="3a931-124">This can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="3a931-125">toowork problemet, logga in toohello domänkontrollanten och antingen ta bort hello vidarebefordrare Konfigurationsinställningen eller konfigurera en giltig vidarebefordrare DNS-server.</span><span class="sxs-lookup"><span data-stu-id="3a931-125">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="3a931-126">detta, i Serverhanteraren klickar du på toodo **verktyg** > **DNS** tooopen DNS-hanteraren och dubbelklicka sedan på **vidarebefordrare**.</span><span class="sxs-lookup"><span data-stu-id="3a931-126">toodo this, in Server Manager click **Tools** > **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a931-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a931-127">Next steps</span></span>
* <span data-ttu-id="3a931-128">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) för information om Linux-distributioner som stöds, flyttar data och skicka jobb tooan HPC Pack kluster med Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="3a931-128">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for information about supported Linux distributions, moving data, and submitting jobs tooan HPC Pack cluster with Linux compute nodes.</span></span>
* <span data-ttu-id="3a931-129">Självstudier som använder hello skriptet toocreate ett kluster och köra en Linux HPC-arbetsbelastning finns:</span><span class="sxs-lookup"><span data-stu-id="3a931-129">For tutorials that use hello script toocreate a cluster and run a Linux HPC workload, see:</span></span>
  * [<span data-ttu-id="3a931-130">Kör NAMD med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="3a931-130">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
  * [<span data-ttu-id="3a931-131">Kör OpenFOAM med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="3a931-131">Run OpenFOAM with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-openfoam.md)
  * [<span data-ttu-id="3a931-132">Kör STAR-CCM + med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="3a931-132">Run STAR-CCM+ with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-starccm.md)

