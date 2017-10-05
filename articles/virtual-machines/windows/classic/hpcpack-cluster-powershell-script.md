---
title: "PowerShell-skript för att distribuera Windows HPC-kluster | Microsoft Docs"
description: "Köra ett PowerShell-skript för att distribuera ett Windows HPC Pack 2012 R2-kluster i Azure-datorer"
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
ms.openlocfilehash: 85b125ab19671b61d2541af6378c95feb88bf952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="7dd24-103">Skapa Windows kluster för högpresterande datorbearbetning (HPC) med HPC Pack IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="7dd24-103">Create a Windows high-performance computing (HPC) cluster with the HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="7dd24-104">Kör PowerShell-skript för att distribuera en komplett HPC Pack 2012 R2-kluster för Windows-arbetsbelastningar på virtuella Azure-datorer för distributionen av HPC Pack IaaS.</span><span class="sxs-lookup"><span data-stu-id="7dd24-104">Run the HPC Pack IaaS deployment PowerShell script to deploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="7dd24-105">Klustret består av en Active Directory-anslutna huvudnod som kör Windows Server och Microsoft HPC Pack och ytterligare Windows beräkningsresurser som du anger.</span><span class="sxs-lookup"><span data-stu-id="7dd24-105">The cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="7dd24-106">Om du vill distribuera ett HPC Pack kluster i Azure för Linux-arbetsbelastningar, se [skapar ett Linux-HPC-kluster med HPC Pack IaaS-distributionsskriptet](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="7dd24-106">If you want to deploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with the HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="7dd24-107">Du kan också använda en Azure Resource Manager-mall för att distribuera ett kluster med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="7dd24-107">You can also use an Azure Resource Manager template to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="7dd24-108">Exempel finns i [skapa ett HPC-kluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) och [skapa ett HPC-kluster med en nod avbildning för anpassade beräkningen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="7dd24-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7dd24-109">PowerShell-skriptet som beskrivs i den här artikeln skapar ett Microsoft HPC Pack 2012 R2-kluster i Azure med hjälp av den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7dd24-109">The PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using the classic deployment model.</span></span> <span data-ttu-id="7dd24-110">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="7dd24-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="7dd24-111">Dessutom stöder inte i skriptet som beskrivs i den här artikeln HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="7dd24-111">In addition, the script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="7dd24-112">Exempel konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="7dd24-112">Example configuration files</span></span>
<span data-ttu-id="7dd24-113">Ersätt värdena för ditt prenumerations-Id eller namn och namnen på kontot och tjänsten i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="7dd24-113">In the following examples, substitute your own values for your subscription Id or name and the account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="7dd24-114">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="7dd24-114">Example 1</span></span>
<span data-ttu-id="7dd24-115">Följande konfigurationsfil distribuerar ett HPC Pack-kluster som har en huvudnod med lokala databaser och fem compute-noder som kör operativsystemet Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="7dd24-115">The following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="7dd24-116">Alla molntjänster skapas direkt i USA, västra plats.</span><span class="sxs-lookup"><span data-stu-id="7dd24-116">All the cloud services are created directly in the West US location.</span></span> <span data-ttu-id="7dd24-117">Huvudnoden fungerar som domänkontrollant för domänskogen.</span><span class="sxs-lookup"><span data-stu-id="7dd24-117">The head node acts as domain controller of the domain forest.</span></span>

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

### <a name="example-2"></a><span data-ttu-id="7dd24-118">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="7dd24-118">Example 2</span></span>
<span data-ttu-id="7dd24-119">Följande konfigurationsfil distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="7dd24-119">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="7dd24-120">Klustret har 1 huvudnod med lokala databaser och 12 compute-noder med filnamnstillägget BGInfo VM tillämpas.</span><span class="sxs-lookup"><span data-stu-id="7dd24-120">The cluster has 1 head node with local databases and 12 compute nodes with the BGInfo VM extension applied.</span></span>
<span data-ttu-id="7dd24-121">Automatisk installation av Windows-uppdateringar är inaktiverad för alla virtuella datorer i domänskogen.</span><span class="sxs-lookup"><span data-stu-id="7dd24-121">Automatic installation of Windows updates is disabled for all the VMs in the domain forest.</span></span> <span data-ttu-id="7dd24-122">Alla molntjänster skapas direkt i Östasien plats.</span><span class="sxs-lookup"><span data-stu-id="7dd24-122">All the cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="7dd24-123">Compute-noderna skapas i tre molntjänster och tre storage-konton: *MyHPCCN 0001* till *MyHPCCN 0005* i *MyHPCCNService01* och  *mycnstorage01*; *MyHPCCN 0006* till *MyHPCCN0010* i *MyHPCCNService02* och *mycnstorage02*; och  *MyHPCCN-0011* till *MyHPCCN 0012* i *MyHPCCNService03* och *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="7dd24-123">The compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* to *MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* to *MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* to *MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="7dd24-124">Compute-noder har skapats från en befintlig privat avbildning som hämtats från en beräkningsnod.</span><span class="sxs-lookup"><span data-stu-id="7dd24-124">The compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="7dd24-125">Automatisk växa eller krympa-tjänsten är aktiverad med standard växa eller krympa intervall.</span><span class="sxs-lookup"><span data-stu-id="7dd24-125">The auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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

### <a name="example-3"></a><span data-ttu-id="7dd24-126">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="7dd24-126">Example 3</span></span>
<span data-ttu-id="7dd24-127">Följande konfigurationsfil distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="7dd24-127">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="7dd24-128">Klustret innehåller en huvudnod, en databasserver med en disk på 500 GB data, två broker noder som kör operativsystemet Windows Server 2012 R2 och fem compute-noder som kör operativsystemet Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="7dd24-128">The cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running the Windows Server 2012 R2 operating system, and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="7dd24-129">Molntjänsten MyHPCCNService skapas i tillhörighetsgruppen *MyIBAffinityGroup*, och de andra molntjänsterna har skapats i tillhörighetsgruppen *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="7dd24-129">The cloud service MyHPCCNService is created in the affinity group *MyIBAffinityGroup*, and the other cloud services are created in the affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="7dd24-130">HPC-jobbet Scheduler REST-API och HPC-webbportalens är aktiverade på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="7dd24-130">The HPC Job Scheduler REST API and HPC web portal are enabled on the head node.</span></span>

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



### <a name="example-4"></a><span data-ttu-id="7dd24-131">Exempel 4</span><span class="sxs-lookup"><span data-stu-id="7dd24-131">Example 4</span></span>
<span data-ttu-id="7dd24-132">Följande konfigurationsfil distribuerar ett HPC Pack kluster i en befintlig domänskog.</span><span class="sxs-lookup"><span data-stu-id="7dd24-132">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="7dd24-133">Klustret har två huvudnod med lokala databaser, två Azure noden mallar skapas och tre storlek medel Azure noder har skapats för Azure nodmallen *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="7dd24-133">The cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="7dd24-134">En skriptfil som körs på huvudnoden när huvudnoden har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="7dd24-134">A script file runs on the head node after the head node is configured.</span></span>

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

## <a name="troubleshooting"></a><span data-ttu-id="7dd24-135">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7dd24-135">Troubleshooting</span></span>
* <span data-ttu-id="7dd24-136">**”Virtuella nätverk finns inte” fel** -om du kör skriptet för att distribuera flera kluster i Azure samtidigt under en prenumeration, en eller flera distributioner kan misslyckas med fel ”VNet *VNet\_namn* finns inte ”.</span><span class="sxs-lookup"><span data-stu-id="7dd24-136">**“VNet doesn’t exist” error** - If you run the script to deploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with the error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="7dd24-137">Om det här felet inträffar, kör du skriptet igen för misslyckad distribution.</span><span class="sxs-lookup"><span data-stu-id="7dd24-137">If this error occurs, run the script again for the failed deployment.</span></span>
* <span data-ttu-id="7dd24-138">**Problem som har åtkomst till Internet från virtuella Azure-nätverket** – om du skapar ett kluster med en ny domänkontrollant med hjälp av skriptet för distribution eller manuellt upphöja huvudnod VM till domänkontrollant kan det uppstå problem med att ansluta den Virtuella datorer till Internet.</span><span class="sxs-lookup"><span data-stu-id="7dd24-138">**Problem accessing the Internet from the Azure virtual network** - If you create a cluster with a new domain controller by using the deployment script, or you manually promote a head node VM to domain controller, you may experience problems connecting the VMs to the Internet.</span></span> <span data-ttu-id="7dd24-139">Det här problemet kan inträffa om en vidarebefordrare DNS-server konfigureras automatiskt på domänkontrollanten och DNS-servern vidarebefordraren löses inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="7dd24-139">This problem can occur if a forwarder DNS server is automatically configured on the domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="7dd24-140">Att undvika det här problemet, logga in på domänkontrollanten och antingen ta bort inställningen vidarebefordrare konfiguration eller konfigurera en giltig vidarebefordrare DNS-server.</span><span class="sxs-lookup"><span data-stu-id="7dd24-140">To work around this problem, log on to the domain controller and either remove the forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="7dd24-141">Konfigurera den här inställningen i Serverhanteraren klickar du på **verktyg** >
    **DNS** att öppna DNS-hanteraren och dubbelklicka sedan på **vidarebefordrare**.</span><span class="sxs-lookup"><span data-stu-id="7dd24-141">To configure this setting, in Server Manager click **Tools** >
    **DNS** to open DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="7dd24-142">**Fel vid åtkomst av RDMA-nätverk från beräkningsintensiva VMs** – om du lägger till Windows Server-beräkning eller Meddelandenod virtuella datorer med en RDMA-kompatibelt storlek exempelvis A8 eller A9 kan det uppstå problem med att ansluta dessa virtuella datorer till nätverkets RDMA program.</span><span class="sxs-lookup"><span data-stu-id="7dd24-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs to the RDMA application network.</span></span> <span data-ttu-id="7dd24-143">En beror problemet uppstår på om HpcVmDrivers tillägget inte har installerats korrekt när de virtuella datorerna har lagts till i klustret.</span><span class="sxs-lookup"><span data-stu-id="7dd24-143">One reason this problem occurs is if the HpcVmDrivers extension is not properly installed when the VMs are added to the cluster.</span></span> <span data-ttu-id="7dd24-144">Tillägget kan till exempel ha fastnat i tillståndet installation.</span><span class="sxs-lookup"><span data-stu-id="7dd24-144">For example, the extension might be stuck in the installing state.</span></span>
  
    <span data-ttu-id="7dd24-145">Undvik problemet genom att kontrollera tillståndet för tillägget i de virtuella datorerna först.</span><span class="sxs-lookup"><span data-stu-id="7dd24-145">To work around this problem, first check the state of the extension in the VMs.</span></span> <span data-ttu-id="7dd24-146">Om tillägget inte har installerats korrekt, tar du bort noder från HPC-kluster och Lägg sedan till noderna.</span><span class="sxs-lookup"><span data-stu-id="7dd24-146">If the extension is not properly installed, try removing the nodes from the HPC cluster and then add the nodes again.</span></span> <span data-ttu-id="7dd24-147">Exempelvis kan du lägga till Beräkningsnoden virtuella datorer genom att köra skriptet Lägg till HpcIaaSNode.ps1 på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="7dd24-147">For example, you can add compute node VMs by running the Add-HpcIaaSNode.ps1 script on the head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dd24-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7dd24-148">Next steps</span></span>
* <span data-ttu-id="7dd24-149">Försök att köra en test-arbetsbelastning i klustret.</span><span class="sxs-lookup"><span data-stu-id="7dd24-149">Try running a test workload on the cluster.</span></span> <span data-ttu-id="7dd24-150">Ett exempel finns i HPC Pack [Kom igång med](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="7dd24-150">For an example, see the HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="7dd24-151">Ett skript för kluster-distributionen och köra ett HPC-arbetsbelastning, finns [komma igång med ett HPC Pack kluster i Azure för att köra arbetsbelastningar för Excel och SOA-](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7dd24-151">For a tutorial to script the cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure to run Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="7dd24-152">Försök HPC Pack verktyg för att starta, stoppa, lägga till och ta bort compute-noder från ett kluster som du skapar.</span><span class="sxs-lookup"><span data-stu-id="7dd24-152">Try HPC Pack's tools to start, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="7dd24-153">Se [hantera compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="7dd24-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="7dd24-154">Om du vill konfigurera att skicka jobb till klustret från en lokal dator, se [skicka HPC-jobb från en lokal dator till ett HPC Pack kluster i Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7dd24-154">To get set up to submit jobs to the cluster from a local computer, see [Submit HPC jobs from an on-premises computer to an HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

