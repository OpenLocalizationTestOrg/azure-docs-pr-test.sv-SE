---
title: "PowerShell-skript för att distribuera Linux HPC-kluster | Microsoft Docs"
description: "Köra ett PowerShell-skript för att distribuera ett Linux HPC Pack 2012 R2-kluster i Azure-datorer"
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
ms.openlocfilehash: c15dc66718a855e22f8109448cb8c8a23787b9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="ce2bb-103">Skapa en Linux högpresterande datorbearbetning (HPC) kluster med HPC Pack IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="ce2bb-103">Create a Linux high-performance computing (HPC) cluster with the HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="ce2bb-104">Kör PowerShell-skript för att distribuera en fullständig HPC Pack 2012 R2-kluster för Linux arbetsbelastningar på virtuella Azure-datorer för distributionen av HPC Pack IaaS.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-104">Run the HPC Pack IaaS deployment PowerShell script to deploy a complete HPC Pack 2012 R2 cluster for Linux workloads in Azure virtual machines.</span></span> <span data-ttu-id="ce2bb-105">Klustret består av en Active Directory-anslutna huvudnod som kör Windows Server och Microsoft HPC Pack och compute-noder som kör något av de Linux-distributioner som stöds av HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-105">The cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and compute nodes that run one of the Linux distributions supported by HPC Pack.</span></span> <span data-ttu-id="ce2bb-106">Om du vill distribuera ett HPC Pack kluster i Azure för Windows-arbetsbelastningar, se [skapa ett Windows HPC-kluster med HPC Pack IaaS-distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce2bb-106">If you want to deploy an HPC Pack cluster in Azure for Windows workloads, see [Create a Windows HPC cluster with the HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="ce2bb-107">Du kan också använda en Azure Resource Manager-mall för att distribuera ett kluster med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-107">You can also use an Azure Resource Manager template to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="ce2bb-108">Ett exempel finns [skapa ett HPC-kluster med Linux datornoderna](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span><span class="sxs-lookup"><span data-stu-id="ce2bb-108">For an example, see [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ce2bb-109">PowerShell-skriptet som beskrivs i den här artikeln skapar ett Microsoft HPC Pack 2012 R2-kluster i Azure med hjälp av den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-109">The PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using the classic deployment model.</span></span> <span data-ttu-id="ce2bb-110">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="ce2bb-111">Dessutom stöder inte i skriptet som beskrivs i den här artikeln HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-111">In addition, the script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a><span data-ttu-id="ce2bb-112">Exempel konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="ce2bb-112">Example configuration file</span></span>
<span data-ttu-id="ce2bb-113">Följande konfigurationsfil skapar en domänkontrollant och domänskog och distribuerar ett HPC Pack-kluster som har en huvudnod med lokala databaser och 10 Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-113">The following configuration file creates a domain controller and domain forest and deploys an HPC Pack cluster which has one head node with local databases and 10 Linux compute nodes.</span></span> <span data-ttu-id="ce2bb-114">Alla molntjänster skapas direkt i Östasien plats.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-114">All the cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="ce2bb-115">Linux-datornoderna skapas i två molntjänster och två storage-konton (det vill säga *MyLnxCN 0001* till *MyLnxCN 0005* i *MyLnxCNService01* och *mylnxstorage01*, och *MyLnxCN 0006* till *MyLnxCN 0010* i *MyLnxCNService02* och *mylnxstorage02* ).</span><span class="sxs-lookup"><span data-stu-id="ce2bb-115">The Linux compute nodes are created in two cloud services and two storage accounts (that is, *MyLnxCN-0001* to *MyLnxCN-0005* in *MyLnxCNService01* and *mylnxstorage01*, and *MyLnxCN-0006* to *MyLnxCN-0010* in *MyLnxCNService02* and *mylnxstorage02*).</span></span> <span data-ttu-id="ce2bb-116">Compute-noder har skapats från en OpenLogic CentOS version 7.0 Linux-bild.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-116">The compute nodes are created from an OpenLogic CentOS version 7.0 Linux image.</span></span> 

<span data-ttu-id="ce2bb-117">Ersätt värdena för din prenumerationsnamn och namnen på kontot och tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-117">Substitute your own values for your subscription name and the account and service names.</span></span>

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
## <a name="troubleshooting"></a><span data-ttu-id="ce2bb-118">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ce2bb-118">Troubleshooting</span></span>
* <span data-ttu-id="ce2bb-119">**”Virtuella nätverk finns inte” fel**.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-119">**“VNet doesn’t exist” error**.</span></span> <span data-ttu-id="ce2bb-120">Om du kör skriptet HPC Pack IaaS distribution för att distribuera flera kluster i Azure samtidigt under en prenumeration, en eller flera distributioner kan misslyckas med fel ”VNet *VNet\_namn* finns inte”.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-120">If you run the HPC Pack IaaS deployment script to deploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with the error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="ce2bb-121">Om det här felet inträffar, kör du skriptet för misslyckad distribution.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-121">If this error occurs, rerun the script for the failed deployment.</span></span>
* <span data-ttu-id="ce2bb-122">**Problem som har åtkomst till Internet från virtuella Azure-nätverket**.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-122">**Problem accessing the Internet from the Azure virtual network**.</span></span> <span data-ttu-id="ce2bb-123">Om du skapar ett HPC Pack kluster med en ny domänkontrollant med hjälp av skriptet för distribution, eller manuellt upphöja huvudnod VM till domänkontrollant, kan det uppstå problem med att ansluta virtuella datorer i Azure-nätverket till Internet.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-123">If you create an HPC Pack cluster with a new domain controller by using the deployment script, or you manually promote a head node VM to domain controller, you may experience problems connecting the VMs in the Azure virtual network to the Internet.</span></span> <span data-ttu-id="ce2bb-124">Detta kan inträffa om en vidarebefordrare DNS-server konfigureras automatiskt på en domänkontrollant och DNS-servern vidarebefordraren löses inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-124">This can occur if a forwarder DNS server is automatically configured on the domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="ce2bb-125">Att undvika det här problemet, logga in på domänkontrollanten och antingen ta bort inställningen vidarebefordrare konfiguration eller konfigurera en giltig vidarebefordrare DNS-server.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-125">To work around this problem, log on to the domain controller and either remove the forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="ce2bb-126">Du gör detta i Serverhanteraren klickar du på **verktyg** > **DNS** att öppna DNS-hanteraren och dubbelklicka sedan på **vidarebefordrare**.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-126">To do this, in Server Manager click **Tools** > **DNS** to open DNS Manager, and then double-click **Forwarders**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce2bb-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce2bb-127">Next steps</span></span>
* <span data-ttu-id="ce2bb-128">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) för information om Linux-distributioner som stöds, flyttar data och skicka jobb till ett HPC Pack kluster med Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="ce2bb-128">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for information about supported Linux distributions, moving data, and submitting jobs to an HPC Pack cluster with Linux compute nodes.</span></span>
* <span data-ttu-id="ce2bb-129">Självstudier som använder skriptet för att skapa ett kluster och köra en Linux HPC-arbetsbelastning finns:</span><span class="sxs-lookup"><span data-stu-id="ce2bb-129">For tutorials that use the script to create a cluster and run a Linux HPC workload, see:</span></span>
  * [<span data-ttu-id="ce2bb-130">Kör NAMD med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="ce2bb-130">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
  * [<span data-ttu-id="ce2bb-131">Kör OpenFOAM med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="ce2bb-131">Run OpenFOAM with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-openfoam.md)
  * [<span data-ttu-id="ce2bb-132">Kör STAR-CCM + med Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="ce2bb-132">Run STAR-CCM+ with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-starccm.md)

