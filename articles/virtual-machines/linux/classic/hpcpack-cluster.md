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
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Kom igång med beräkningsnoder för Linux i ett HPC Pack-kluster i Azure
Konfigurera en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) kluster i Azure som innehåller en huvudnod som kör Windows Server och flera compute-noder som kör ett Linux-distribution som stöds. Utforska alternativ toomove data mellan hello Linux noder och hello Windows huvudnod hello-klustret. Lär dig hur toosubmit Linux HPC jobb toohello klustret.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

På en hög nivå hello följande diagram visar hello HPC Pack kluster du skapar och arbetar med.

![HPC Pack kluster med noder för Linux][scenario]

För andra alternativ toorun Linux HPC arbetsbelastningar i Azure, se [tekniska resurser för batch- och högpresterande datorbearbetning](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Distribuera ett HPC Pack kluster med beräkningsnoder för Linux
Den här artikeln innehåller två alternativ toodeploy ett HPC Pack-kluster i Azure med Linux compute-noder. Båda metoderna kan du använda en Marketplace-avbildning av Windows Server med HPC Pack toocreate hello huvudnod. 

* **Azure Resource Manager-mall** -använda en mall från hello Azure Marketplace, eller en mall för Snabbstart hello gemenskapen, tooautomate skapandet av hello klustret i hello Resource Manager-distributionsmodellen. Till exempel hello [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mallen i hello Azure Marketplace skapar en komplett HPC Pack klustret infrastruktur för Linux HPC arbetsbelastningar.
* **PowerShell-skriptet** -Använd hello [Microsoft HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**ny HpcIaaSCluster.ps1**) tooautomate en fullständig Klusterdistribution i hello klassiska distributionsmodellen. Azure PowerShell-skript använder ett HPC Pack VM-avbildning i hello Azure Marketplace för snabb distribution och ger en omfattande uppsättning configuration parametrar toodeploy Linux compute-noder.

Mer information om HPC Pack distributionsalternativ för kluster i Azure finns [alternativ toocreate och hantera ett kluster för högpresterande datorbearbetning (HPC) i Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Krav
* **Azure-prenumeration** -du kan använda en prenumeration i antingen hello Azure Global eller Azure Kina service. Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) på bara några minuter.
* **Kärnor kvoten** – du kan behöva tooincrease hello kvoten för kärnor, särskilt om du väljer toodeploy flera klusternoder med flera kärnor VM-storlekar. tooincrease en kvot öppnar en online-kund supportbegäran utan kostnad.
* **Linux-distributioner** -för närvarande HPC Pack stöder hello följande Linux-distributioner för compute-noder. Du kan använda dessa distributioner Marketplace-versioner där det är tillgängligt eller ange egna.
  
  * **CentOS-baserade**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6.8 och 7.2
  * **SUSE Linux Enterprise Server**: SLES 12 SLES 12 (Premium) SLES 12 SP1, SLES 12 SP1 (Premium) SLES 12 för HPC SLES 12 för HPC (Premium)
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > toouse hello Azure RDMA nätverk med en av RDMA-kompatibla hello VM-storlekar, ange en SUSE Linux Enterprise Server 12 HPC eller CentOS-baserade HPC-avbildning från hello Azure Marketplace. Mer information finns i [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

Ytterligare krav toodeploy hello-kluster med hjälp av hello HPC Pack IaaS-skriptet:

* **Klientdatorn** -du behöver ett Windows-baserad klient datorn toorun hello klustret skript för distribution.
* **Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.
* **HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Du kan kontrollera hello version hello skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`. Den här artikeln är baserat på version 4.4.1 eller senare av hello skript.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Distributionsalternativ 1. Använd en Resource Manager-mall
1. Gå toohello [HPC Pack kluster för Linux arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) mallen i hello Azure Marketplace och klicka på **distribuera**.
2. Hello Azure-portalen, granska hello information och klicka sedan på **skapa**.
   
    ![Skapa Portal][portal]
3. På hello **grunderna** bladet, ange ett namn för hello-kluster, vilket även namnen hello huvudnod VM. Du kan välja en befintlig resursgrupp eller skapa en grupp för hello distribution på en plats som är tillgängliga tooyou. hello plats påverkar hello tillgängligheten för vissa VM-storlekar och andra Azure-tjänster (se [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/)).
4. På hello **Head noden inställningar** bladet en första distribution kan du oftast acceptera standardinställningarna för hello. 
   
   > [!NOTE]
   > Hej **efter konfigurationsskript URL** är en valfri inställning toospecify ett offentligt tillgängliga Windows PowerShell-skript som du vill toorun i hello huvudnod VM när den körs. 
   > 
   > 
5. På hello **Compute-nod inställningar** bladet välj namnmönstret för hello noder, hello antalet och storleken på hello noder och hello toodeploy för Linux-distribution.
6. På hello **infrastrukturinställningar** bladet ange namn för hello virtuella nätverk och Active Directory domän, domän och VM-administratörsautentiseringsuppgifter och ett namngivningsmönster för hello storage-konton.
   
   > [!NOTE]
   > HPC Pack använder hello Active Directory-domän tooauthenticate klustret användare. 
   > 
   > 
7. När du kör verifieringstester hello och du granska hello villkor för användning, klickar du på **inköp**.

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a>Distributionsalternativ 2. Använd hello IaaS-distributionsskriptet
Följande är ytterligare förutsättningar toodeploy hello kluster med hjälp av hello HPC Pack IaaS-skriptet:

* **Klientdatorn** -du behöver ett Windows-baserad klient datorn toorun hello klustret skript för distribution.
* **Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.
* **HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Du kan kontrollera hello version hello skriptet genom att köra `.\New-HPCIaaSCluster.ps1 –Version`. Den här artikeln är baserat på version 4.4.1 eller senare av hello skript.

**XML-konfigurationsfilen**

hello HPC Pack IaaS distributionsskriptet använder en XML-konfigurationsfil som inkommande toodescribe hello HPC-kluster. hello anger följande exempel konfigurationsfil ett mindre kluster som består av en huvudnod i HPC Pack och två storlek A7 CentOS 7.0 Linux compute-noder. 

Ändra hello-filen som behövs för din miljö och önskade klusterkonfigurationen och spara den med ett namn, till exempel HPCDemoConfig.xml. Du måste till exempel toosupply namnet på din prenumeration och en unik lagringskontonamn och molntjänstnamnet. Dessutom kanske du vill att en annan toochoose stöds Linux avbildningen för hello compute-noder. Mer information om hello element i hello-konfigurationsfilen finns hello Manual.rtf fil i mappen för hello-skript och [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

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

**toorun hello HPC Pack IaaS distributionsskriptet**

1. Öppna Windows PowerShell på hello klientdator som administratör.
2. Ändra toohello katalogmapp där hello skript är installerat (E:\IaaSClusterScript i det här exemplet).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. Kör följande kommando toodeploy hello HPC Pack klustret hello. Det här exemplet förutsätter att hello-konfigurationsfilen finns i E:\HPCDemoConfig.xml
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Eftersom hello **AdminPassword** har inte angetts i hello före kommandot, ange tooenter hello lösenord för användare anges *MyAdminName*.
   
    b. hello skript startar sedan toovalidate hello konfigurationsfilen. Det kan ta upp tooseveral minuter beroende på hur hello nätverksanslutning.
   
    ![Validering][validate]
   
    c. När verifieringar skicka visar hello skript hello klustret resurser toocreate. Ange *Y* toocontinue.
   
    ![Resurser][resources]
   
    d. hello skript startar toodeploy hello HPC Pack kluster och har slutförts hello-konfiguration utan några ytterligare manuella steg. hello-skript kan köras i flera minuter.
   
    ![Distribuera][deploy]
   
   > [!NOTE]
   > I det här exemplet hello skriptet genererar en loggfil automatiskt eftersom hello **- LogFile** parameter har inte angetts. hello loggar inte är skriven i realtid men har samlats in hello slutet av hello validering och hello-distribution. Vissa loggar går förlorade om hello PowerShell-process stoppas medan hello skript körs.
   > 
   > 

## <a name="connect-toohello-head-node"></a>Ansluta toohello huvudnod
När du har distribuerat hello HPC Pack kluster i Azure, [ansluta med Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello huvudnod VM med hello autentiseringsuppgifter för domänen som du angav när du distribuerade klustret hello (till exempel *hpc\\ clusteradmin*). Du kan hantera hello kluster från hello huvudnod.

Starta HPC Cluster Manager toocheck hello status för hello HPC Pack kluster på hello huvudnod. Du kan hantera och övervaka Linux datornoderna hello samma sätt som du arbetar med Windows compute-noder. Se exempelvis hello Linux noder som anges i **resurshantering** (dessa noder distribueras med hello **LinuxNode** mall).

![Hantering][management]

Du också se hello Linux noder i hello **termisk karta** vyn.

![Termisk karta][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a>Hur toomove data i ett kluster med noder som Linux
Du har flera alternativ toomove data mellan noder för Linux och hello Windows huvudnod hello klustret. Här följer tre vanliga metoder som beskrivs i detalj i följande avsnitt hello:

* **Azure File** -visar filer som en hanterad SMB-filresurs toostore-data i Azure storage. Windows-noderna och Linux-noder kan montera en filresurs på Azure som en enhet eller mapp på hello samma tid, även om de har distribuerats i olika virtuella nätverk.
* **Huvudnod SMB dela** -monterar en standard Windows delad mapp för hello huvudnod på Linux-noder.
* **HEAD nod NFS-servern** -är en lösning för fildelning för en blandad miljö för Windows och Linux.

### <a name="azure-file-storage"></a>Azure File storage
Hej [Azure File](https://azure.microsoft.com/services/storage/files/) service exponerar filresurser med hjälp av SMB 2.1 hello standardprotokoll. Virtuella Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via hello fillagring API. 

Detaljerade anvisningar toocreate en Azure-fil dela och montera på hello huvudnod finns [Kom igång med Azure File storage i Windows](../../../storage/files/storage-how-to-use-files-windows.md). toomount hello Azure-filresurs på hello Linux noder finns [hur toouse Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md). tooset in spara anslutningar finns [Persisting anslutningar tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

I följande exempel hello, skapar du en Azure-filresurs på ett lagringskonto. toomount hello-resursen på hello huvudnod, öppna en kommandotolk och ange hello följande kommandon:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

I det här exemplet allvhdsje är namnet på ditt lagringskonto, storageaccountkey är din lagringskontonyckel och rdma är hello Azure filresursnamn. hello Azure-filresurs monteras som Z: hello huvudnod.

toomount hello Azure-filresurs på Linux-noder som kör en **clusrun** på hello huvudnod. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  är en användbar HPC Pack verktyget toocarry administrativa uppgifter på flera noder. (Se även [Clusrun för Linux-noder](#Clusrun-for-Linux-nodes) i den här artikeln.)

Öppna Windows PowerShell-fönstret och ange hello följande kommandon:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

hello första kommandot skapar en mapp med namnet /rdma på alla noder i hello LinuxNodes grupp. hello andra kommandot monterar hello Azure File share allvhdsjw.file.core.windows.net/rdma till hello /rdma mapp med dir och filen läge bits set too777. I andra hello-kommandot allvhdsje är namnet på ditt lagringskonto och storageaccountkey är din lagringskontonyckel.

> [!NOTE]
> Hej ”\`” symbol i andra hello-kommandot är en symbolen för PowerShell. ”\`,” innebär att hello ””, (kommatecken tecken) är en del av hello-kommando.
> 
> 

### <a name="head-node-share"></a>Huvudnod resursen
Du kan också montera en delad mapp för hello huvudnod på Linux-noder. En resurs ger hello enklaste sättet tooshare filer, men hello huvudnod och alla Linux-noder måste distribueras i hello samma virtuella nätverk. Här är hello åtgärder.

1. Skapa en mapp på hello huvudnod och dela den tooEveryone med läs-/ skrivbehörighet. Till exempel dela D:\OpenFOAM i hello huvudnod som \\CentOS7RDMA HN\OpenFOAM. Här är CentOS7RDMA HN hello värdnamnet för hello huvudnod.
   
    ![Filresursbehörigheter][fileshareperms]
   
    ![Fildelning][filesharing]
2. Öppna Windows PowerShell-fönstret och kör följande kommandon hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

hello första kommandot skapar en mapp med namnet /openfoam på alla noder i hello LinuxNodes grupp. hello andra kommandot monterar hello delade mappen //CentOS7RDMA-HN/OpenFOAM till hello mapp med dir och filen läge bits set too777. hello ska användarnamn och lösenord i hello-kommandot hello användarnamnet och lösenordet för en kluster-användare på hello huvudnod. (Se [Lägg till eller ta bort klustret användare](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> Hej ”\`” symbol i andra hello-kommandot är en symbolen för PowerShell. ”\`,” innebär att hello ””, (kommatecken tecken) är en del av hello-kommando.
> 
> 

### <a name="nfs-server"></a>NFS-server
hello NFS-tjänsten kan du tooshare och flytta filer mellan datorer som kör Windows Server 2012 för hello hello SMB-protokollet och Linux-baserade datorer med hjälp av hello NFS-protokollet. hello NFS-servern och alla andra noder har distribuerats i hello toobe samma virtuella nätverk. Det ger bättre kompatibilitet med Linux-noder jämfört med en SMB-resurs. Till exempel stöder filen länkar.

1. tooinstall och skapa en NFS-server gör hello i [servern för System första nätverksfilresurs slutpunkt till slutpunkt](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Till exempel skapa en NFS-resurs som heter nfs med hello följande egenskaper:
   
    ![NFS-auktorisering][nfsauth]
   
    ![NFS-resursbehörigheter][nfsshare]
   
    ![NFS-NTFS-behörigheter][nfsperm]
   
    ![Egenskaper för NFS][nfsmanage]
2. Öppna Windows PowerShell-fönstret och kör följande kommandon hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   hello första kommandot skapar en mapp med namnet /nfsshared på alla noder i hello LinuxNodes grupp. hello andra kommandot monteringar hello NFS dela CentOS7RDMA HN: / nfs till hello mapp. Här CentOS7RDMA HN: / nfs är hello fjärrsökväg NFS-resursen.

## <a name="how-toosubmit-jobs"></a>Hur toosubmit jobb
Det finns flera sätt toosubmit jobb toohello HPC Pack klustret:

* HPC Cluster Manager eller HPC Job Manager GUI
* HPC-webbportalens
* REST API

Jobbet toohello kluster i Azure via HPC Pack Gränssnittsbaserade verktyg och hello HPC-webbportalens är hello samma som för Windows compute-noder. Se [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) och [hur toosubmit jobb från en klientdator för lokala](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

toosubmit jobb via hello REST-API finns för[skapa och skicka jobb genom att använda hello REST API i Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). toosubmit jobb från en klient för Linux finns också toohello Python provet i hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Clusrun för Linux-noder
hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) verktyget kan använda tooexecute kommandon på Linux-noder antingen via en kommandotolk eller HPC Cluster Manager. Följande är några grundläggande exempel.

* Visa aktuella användarnamn på alla noder i klustret hello.
  
    ```command
    clusrun whoami
    ```
* Installera hello **gdb** felsökningsverktyget med **yum** på alla noder i hello linuxnodes gruppen och starta sedan om hello noder efter 10 minuter.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Skapa ett kommandoskript som visar varje tal 1 till 10 för en sekund på varje Linux-nod i klustret hello köra det och visa utdata från hello noder omedelbart.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Du kan behöva toouse vissa escape-tecken i **clusrun** kommandon. I det här exemplet visas när du använder ^ i en kommandotolk tooescape hello ”>” symbol.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Försök att skala upp hello klustret tooa större antal noder eller försök att köra en Linux-arbetsbelastning på hello klustret. Ett exempel finns [kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure](hpcpack-cluster-namd.md).
* Försök med ett kluster med [RDMA-kompatibla, beräkningsintensiva VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI arbetsbelastningar. Ett exempel finns [kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-klustret i Azure](hpcpack-cluster-openfoam.md).
* Om du vill arbeta med Linux-noder i ett kluster för lokala HPC Pack finns hello [TechNet vägledning](https://technet.microsoft.com/library/mt595803.aspx).

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
