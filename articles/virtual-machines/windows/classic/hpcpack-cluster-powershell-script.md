---
title: aaaPowerShell skript toodeploy Windows HPC-kluster | Microsoft Docs
description: "Köra ett PowerShell-skript toodeploy ett Windows HPC Pack 2012 R2-kluster i virtuella Azure-datorer"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 10ce1e9bc4e98954b955549bd72aaaf6106c69fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="514e2-103">Skapa ett högpresterande datorbearbetning (HPC) Windows-kluster med hello HPC Pack IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="514e2-103">Create a Windows high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="514e2-104">Kör hello HPC Pack IaaS distribution PowerShell-skriptet toodeploy en fullständig HPC Pack 2012 R2-kluster för Windows-arbetsbelastningar på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="514e2-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="514e2-105">hello klustret består av en Active Directory-anslutna huvudnod som kör Windows Server och Microsoft HPC Pack och ytterligare Windows beräkningsresurser som du anger.</span><span class="sxs-lookup"><span data-stu-id="514e2-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="514e2-106">Om du vill toodeploy ett HPC Pack kluster i Azure för Linux-arbetsbelastningar, se [skapar ett Linux-HPC-kluster med hello HPC Pack IaaS distributionsskriptet](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="514e2-106">If you want toodeploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with hello HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="514e2-107">Du kan också använda en Azure Resource Manager mallen toodeploy ett HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="514e2-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="514e2-108">Exempel finns i [skapa ett HPC-kluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) och [skapa ett HPC-kluster med en nod avbildning för anpassade beräkningen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="514e2-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="514e2-109">hello PowerShell-skriptet som beskrivs i den här artikeln skapar ett Microsoft HPC Pack 2012 R2-kluster i Azure med hjälp av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="514e2-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="514e2-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="514e2-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="514e2-111">Dessutom stöder inte hello-skriptet som beskrivs i den här artikeln HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="514e2-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="514e2-112">Exempel konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="514e2-112">Example configuration files</span></span>
<span data-ttu-id="514e2-113">I hello ersätta egna värden för ditt prenumerations-Id eller namn och hello kontot och tjänsten namn i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="514e2-113">In hello following examples, substitute your own values for your subscription Id or name and hello account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="514e2-114">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="514e2-114">Example 1</span></span>
<span data-ttu-id="514e2-115">hello distribuerar följande konfigurationsfil ett HPC Pack-kluster som har en huvudnod med lokala databaser och fem compute-noder som kör operativsystemet Windows Server 2012 R2 för hello.</span><span class="sxs-lookup"><span data-stu-id="514e2-115">hello following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="514e2-116">Alla hello molntjänster skapas direkt i hello västra USA plats.</span><span class="sxs-lookup"><span data-stu-id="514e2-116">All hello cloud services are created directly in hello West US location.</span></span> <span data-ttu-id="514e2-117">hello huvudnod fungerar som domänkontrollant för hello domänskog.</span><span class="sxs-lookup"><span data-stu-id="514e2-117">hello head node acts as domain controller of hello domain forest.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a><span data-ttu-id="514e2-118">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="514e2-118">Example 2</span></span>
<span data-ttu-id="514e2-119">hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="514e2-119">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="514e2-120">hello-klustret har 1 huvudnod med lokala databaser och 12 compute-noder med hello BGInfo VM-tillägget används.</span><span class="sxs-lookup"><span data-stu-id="514e2-120">hello cluster has 1 head node with local databases and 12 compute nodes with hello BGInfo VM extension applied.</span></span>
<span data-ttu-id="514e2-121">Automatisk installation av Windows-uppdateringar är inaktiverad för alla hello virtuella datorer i hello domänskog.</span><span class="sxs-lookup"><span data-stu-id="514e2-121">Automatic installation of Windows updates is disabled for all hello VMs in hello domain forest.</span></span> <span data-ttu-id="514e2-122">Alla hello molntjänster skapas direkt i Östasien plats.</span><span class="sxs-lookup"><span data-stu-id="514e2-122">All hello cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="514e2-123">hello datornoderna skapas i tre molntjänster och tre storage-konton: *MyHPCCN 0001* för*MyHPCCN 0005* i *MyHPCCNService01* och  *mycnstorage01*; *MyHPCCN 0006* för*MyHPCCN0010* i *MyHPCCNService02* och *mycnstorage02*; och  *MyHPCCN-0011* för*MyHPCCN 0012* i *MyHPCCNService03* och *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="514e2-123">hello compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* too*MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* too*MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* too*MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="514e2-124">hello compute-noder har skapats från en befintlig privat avbildning som hämtats från en beräkningsnod.</span><span class="sxs-lookup"><span data-stu-id="514e2-124">hello compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="514e2-125">hello automatiskt växa eller krympa-tjänsten är aktiverad med standard växa eller krympa intervall.</span><span class="sxs-lookup"><span data-stu-id="514e2-125">hello auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a><span data-ttu-id="514e2-126">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="514e2-126">Example 3</span></span>
<span data-ttu-id="514e2-127">hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="514e2-127">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="514e2-128">hello-kluster som innehåller en huvudnod, en databasserver med en disk på 500 GB data, två broker noder som kör operativsystemet Windows Server 2012 R2 för hello och fem compute-noder som kör operativsystemet Windows Server 2012 R2 för hello.</span><span class="sxs-lookup"><span data-stu-id="514e2-128">hello cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running hello Windows Server 2012 R2 operating system, and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="514e2-129">Hej Molntjänsten MyHPCCNService skapas i hello tillhörighetsgrupp *MyIBAffinityGroup*, och hello andra molntjänster har skapats i hello tillhörighetsgrupp *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="514e2-129">hello cloud service MyHPCCNService is created in hello affinity group *MyIBAffinityGroup*, and hello other cloud services are created in hello affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="514e2-130">hello HPC Job Scheduler REST-API och HPC-webbportalens är aktiverade på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="514e2-130">hello HPC Job Scheduler REST API and HPC web portal are enabled on hello head node.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a><span data-ttu-id="514e2-131">Exempel 4</span><span class="sxs-lookup"><span data-stu-id="514e2-131">Example 4</span></span>
<span data-ttu-id="514e2-132">hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="514e2-132">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="514e2-133">hello klustret har två huvudnod med lokala databaser två Azure noden mallar skapas och tre storlek medel Azure noder har skapats för Azure nodmallen *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="514e2-133">hello cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="514e2-134">En skriptfil som körs på hello huvudnod när hello huvudnod har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="514e2-134">A script file runs on hello head node after hello head node is configured.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a><span data-ttu-id="514e2-135">Felsökning</span><span class="sxs-lookup"><span data-stu-id="514e2-135">Troubleshooting</span></span>
* <span data-ttu-id="514e2-136">**”Virtuella nätverk finns inte” fel** -om du kör hello skriptet toodeploy flera kluster i Azure samtidigt under en prenumeration, en eller flera distributioner kan misslyckas med felet hello ”VNet *VNet\_namn* inte Det finns ”.</span><span class="sxs-lookup"><span data-stu-id="514e2-136">**“VNet doesn’t exist” error** - If you run hello script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="514e2-137">Om det här felet uppstår kör hello skriptet igen för distribution av hello misslyckades.</span><span class="sxs-lookup"><span data-stu-id="514e2-137">If this error occurs, run hello script again for hello failed deployment.</span></span>
* <span data-ttu-id="514e2-138">**Problem att komma åt hello Internet från hello virtuella Azure-nätverket** – om du skapar ett kluster med en ny domänkontrollant med hjälp av hello distributionsskriptet eller du befordrar en domänkontrollant med huvudnod VM toodomain manuellt kan det uppstå problem ansluter hello VMs toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="514e2-138">**Problem accessing hello Internet from hello Azure virtual network** - If you create a cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs toohello Internet.</span></span> <span data-ttu-id="514e2-139">Det här problemet kan inträffa om en vidarebefordrare DNS-server konfigureras automatiskt på hello domänkontrollant och DNS-servern vidarebefordraren löses inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="514e2-139">This problem can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="514e2-140">toowork problemet, logga in toohello domänkontrollanten och antingen ta bort hello vidarebefordrare Konfigurationsinställningen eller konfigurera en giltig vidarebefordrare DNS-server.</span><span class="sxs-lookup"><span data-stu-id="514e2-140">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="514e2-141">inställningen i Serverhanteraren klickar du på tooconfigure **verktyg** >
    **DNS** tooopen DNS-hanteraren och dubbelklicka sedan på **vidarebefordrare**.</span><span class="sxs-lookup"><span data-stu-id="514e2-141">tooconfigure this setting, in Server Manager click **Tools** >
    **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="514e2-142">**Fel vid åtkomst av RDMA-nätverk från beräkningsintensiva VMs** – om du lägger till Windows Server-beräkning eller Meddelandenod virtuella datorer med en RDMA-kompatibla storlek, till exempel A8 eller A9, du får problem med ansluta nätverket för virtuella datorer toohello RDMA-programmet.</span><span class="sxs-lookup"><span data-stu-id="514e2-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs toohello RDMA application network.</span></span> <span data-ttu-id="514e2-143">En beror problemet uppstår på om hello HpcVmDrivers tillägget inte har installerats korrekt när hello virtuella datorer läggs toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="514e2-143">One reason this problem occurs is if hello HpcVmDrivers extension is not properly installed when hello VMs are added toohello cluster.</span></span> <span data-ttu-id="514e2-144">Tillägget kan till exempel ha fastnat i hello installerar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="514e2-144">For example, the extension might be stuck in hello installing state.</span></span>
  
    <span data-ttu-id="514e2-145">toowork runt problemet första hello markeringstillstånd hello tillägget hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="514e2-145">toowork around this problem, first check hello state of hello extension in hello VMs.</span></span> <span data-ttu-id="514e2-146">Om hello tillägget inte har installerats korrekt, tar du bort hello noder från hello HPC-kluster och Lägg sedan till hello noder.</span><span class="sxs-lookup"><span data-stu-id="514e2-146">If hello extension is not properly installed, try removing hello nodes from hello HPC cluster and then add hello nodes again.</span></span> <span data-ttu-id="514e2-147">Exempelvis kan du lägga till Beräkningsnoden virtuella datorer genom att köra hello HpcIaaSNode.ps1 Lägg till skript i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="514e2-147">For example, you can add compute node VMs by running hello Add-HpcIaaSNode.ps1 script on hello head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="514e2-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="514e2-148">Next steps</span></span>
* <span data-ttu-id="514e2-149">Försök att köra en test-arbetsbelastning på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="514e2-149">Try running a test workload on hello cluster.</span></span> <span data-ttu-id="514e2-150">Ett exempel finns hello HPC Pack [Kom igång med](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="514e2-150">For an example, see hello HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="514e2-151">En självstudiekurs tooscript hello Klusterdistribution och kör ett HPC-arbetsbelastning finns [komma igång med ett HPC Pack kluster i Azure toorun Excel- och SOA-arbetsbelastningar](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="514e2-151">For a tutorial tooscript hello cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="514e2-152">Försök HPC Pack verktyg toostart, stoppa, lägga till och ta bort compute-noder från ett kluster som du skapar.</span><span class="sxs-lookup"><span data-stu-id="514e2-152">Try HPC Pack's tools toostart, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="514e2-153">Se [hantera compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="514e2-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="514e2-154">Konfigurera toosubmit jobb toohello kluster från en lokal dator tooget finns [skicka HPC-jobb från en lokal dator tooan HPC Pack kluster i Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="514e2-154">tooget set up toosubmit jobs toohello cluster from a local computer, see [Submit HPC jobs from an on-premises computer tooan HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

