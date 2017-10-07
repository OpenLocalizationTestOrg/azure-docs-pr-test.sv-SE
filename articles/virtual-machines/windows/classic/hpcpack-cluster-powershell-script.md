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
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a>Skapa ett högpresterande datorbearbetning (HPC) Windows-kluster med hello HPC Pack IaaS-distributionsskriptet
Kör hello HPC Pack IaaS distribution PowerShell-skriptet toodeploy en fullständig HPC Pack 2012 R2-kluster för Windows-arbetsbelastningar på virtuella Azure-datorer. hello klustret består av en Active Directory-anslutna huvudnod som kör Windows Server och Microsoft HPC Pack och ytterligare Windows beräkningsresurser som du anger. Om du vill toodeploy ett HPC Pack kluster i Azure för Linux-arbetsbelastningar, se [skapar ett Linux-HPC-kluster med hello HPC Pack IaaS distributionsskriptet](../../linux/classic/hpcpack-cluster-powershell-script.md). Du kan också använda en Azure Resource Manager mallen toodeploy ett HPC Pack-kluster. Exempel finns i [skapa ett HPC-kluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) och [skapa ett HPC-kluster med en nod avbildning för anpassade beräkningen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

> [!IMPORTANT] 
> hello PowerShell-skriptet som beskrivs i den här artikeln skapar ett Microsoft HPC Pack 2012 R2-kluster i Azure med hjälp av hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.
> Dessutom stöder inte hello-skriptet som beskrivs i den här artikeln HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Exempel konfigurationsfiler
I hello ersätta egna värden för ditt prenumerations-Id eller namn och hello kontot och tjänsten namn i följande exempel.

### <a name="example-1"></a>Exempel 1
hello distribuerar följande konfigurationsfil ett HPC Pack-kluster som har en huvudnod med lokala databaser och fem compute-noder som kör operativsystemet Windows Server 2012 R2 för hello. Alla hello molntjänster skapas direkt i hello västra USA plats. hello huvudnod fungerar som domänkontrollant för hello domänskog.

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

### <a name="example-2"></a>Exempel 2
hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog. hello-klustret har 1 huvudnod med lokala databaser och 12 compute-noder med hello BGInfo VM-tillägget används.
Automatisk installation av Windows-uppdateringar är inaktiverad för alla hello virtuella datorer i hello domänskog. Alla hello molntjänster skapas direkt i Östasien plats. hello datornoderna skapas i tre molntjänster och tre storage-konton: *MyHPCCN 0001* för*MyHPCCN 0005* i *MyHPCCNService01* och  *mycnstorage01*; *MyHPCCN 0006* för*MyHPCCN0010* i *MyHPCCNService02* och *mycnstorage02*; och  *MyHPCCN-0011* för*MyHPCCN 0012* i *MyHPCCNService03* och *mycnstorage03*). hello compute-noder har skapats från en befintlig privat avbildning som hämtats från en beräkningsnod. hello automatiskt växa eller krympa-tjänsten är aktiverad med standard växa eller krympa intervall.

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

### <a name="example-3"></a>Exempel 3
hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog. hello-kluster som innehåller en huvudnod, en databasserver med en disk på 500 GB data, två broker noder som kör operativsystemet Windows Server 2012 R2 för hello och fem compute-noder som kör operativsystemet Windows Server 2012 R2 för hello. Hej Molntjänsten MyHPCCNService skapas i hello tillhörighetsgrupp *MyIBAffinityGroup*, och hello andra molntjänster har skapats i hello tillhörighetsgrupp *MyAffinityGroup*. hello HPC Job Scheduler REST-API och HPC-webbportalens är aktiverade på hello huvudnod.

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



### <a name="example-4"></a>Exempel 4
hello följande konfigurationsfilen distribuerar ett HPC Pack kluster i en befintlig domänskog. hello klustret har två huvudnod med lokala databaser två Azure noden mallar skapas och tre storlek medel Azure noder har skapats för Azure nodmallen *AzureTemplate1*. En skriptfil som körs på hello huvudnod när hello huvudnod har konfigurerats.

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

## <a name="troubleshooting"></a>Felsökning
* **”Virtuella nätverk finns inte” fel** -om du kör hello skriptet toodeploy flera kluster i Azure samtidigt under en prenumeration, en eller flera distributioner kan misslyckas med felet hello ”VNet *VNet\_namn* inte Det finns ”.
  Om det här felet uppstår kör hello skriptet igen för distribution av hello misslyckades.
* **Problem att komma åt hello Internet från hello virtuella Azure-nätverket** – om du skapar ett kluster med en ny domänkontrollant med hjälp av hello distributionsskriptet eller du befordrar en domänkontrollant med huvudnod VM toodomain manuellt kan det uppstå problem ansluter hello VMs toohello Internet. Det här problemet kan inträffa om en vidarebefordrare DNS-server konfigureras automatiskt på hello domänkontrollant och DNS-servern vidarebefordraren löses inte korrekt.
  
    toowork problemet, logga in toohello domänkontrollanten och antingen ta bort hello vidarebefordrare Konfigurationsinställningen eller konfigurera en giltig vidarebefordrare DNS-server. inställningen i Serverhanteraren klickar du på tooconfigure **verktyg** >
    **DNS** tooopen DNS-hanteraren och dubbelklicka sedan på **vidarebefordrare**.
* **Fel vid åtkomst av RDMA-nätverk från beräkningsintensiva VMs** – om du lägger till Windows Server-beräkning eller Meddelandenod virtuella datorer med en RDMA-kompatibla storlek, till exempel A8 eller A9, du får problem med ansluta nätverket för virtuella datorer toohello RDMA-programmet. En beror problemet uppstår på om hello HpcVmDrivers tillägget inte har installerats korrekt när hello virtuella datorer läggs toohello klustret. Tillägget kan till exempel ha fastnat i hello installerar tillstånd.
  
    toowork runt problemet första hello markeringstillstånd hello tillägget hello virtuella datorer. Om hello tillägget inte har installerats korrekt, tar du bort hello noder från hello HPC-kluster och Lägg sedan till hello noder. Exempelvis kan du lägga till Beräkningsnoden virtuella datorer genom att köra hello HpcIaaSNode.ps1 Lägg till skript i hello huvudnod.

## <a name="next-steps"></a>Nästa steg
* Försök att köra en test-arbetsbelastning på hello klustret. Ett exempel finns hello HPC Pack [Kom igång med](https://technet.microsoft.com/library/jj884144).
* En självstudiekurs tooscript hello Klusterdistribution och kör ett HPC-arbetsbelastning finns [komma igång med ett HPC Pack kluster i Azure toorun Excel- och SOA-arbetsbelastningar](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Försök HPC Pack verktyg toostart, stoppa, lägga till och ta bort compute-noder från ett kluster som du skapar. Se [hantera compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster-node-manage.md).
* Konfigurera toosubmit jobb toohello kluster från en lokal dator tooget finns [skicka HPC-jobb från en lokal dator tooan HPC Pack kluster i Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

