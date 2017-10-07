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
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Kör STAR-CCM + med Microsoft HPC Pack på en Linux RDMA kluster i Azure
Den här artikeln visar hur toodeploy Microsoft HPC Pack kluster på Azure och kör en [CD-simuleringar STAR-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) jobb på flera Linux compute-noder som är sammankopplade med InfiniBand.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack ger funktioner toorun olika storskaliga HPC och parallella program, inklusive MPI program på kluster av Microsoft Azure-datorer. HPC Pack stöder också Linux HPC-program som körs på Linux-beräkningsnod virtuella datorer som distribueras i ett HPC Pack-kluster. En introduktion toousing Linux compute-noder med HPC Pack, finns [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Konfigurera ett HPC Pack-kluster
Hämta hello HPC Pack IaaS distribution skript från hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) och extrahera dem lokalt.

Azure PowerShell är en förutsättning. Läs hello artikeln om PowerShell inte har konfigurerats på den lokala datorn [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

När hello detta skrivs är hello Linux bilder från hello Azure Marketplace (som innehåller hello InfiniBand drivrutiner för Azure) för SLES 12, CentOS 6.5 och CentOS 7.1. Den här artikeln är baserad på hello användning av SLES 12. tooretrieve hello namnet på alla Linux-bilder som stöder HPC i hello Marketplace, kan du köra hello följande PowerShell-kommando:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

hello utdata visar hello plats där dessa avbildningar är tillgängliga och hello avbildningsnamn (**avbildning**) toobe som används i mallen för distribution av hello senare.

Innan du distribuerar hello klustret har toobuild en mallfil för distribution av HPC Pack. Eftersom vi riktad ett litet kluster tas hello huvudnod hello domänkontrollant och vara värd för en lokal SQL-databas.

hello vid följande mall distribuera en huvudnod, skapa en XML-fil med namnet **MyCluster.xml**, och Ersätt hello värdena för **SubscriptionId**, **StorageAccount**,  **Plats**, **VMName**, och **ServiceName** med dina.

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

Starta hello-huvudnod skapas genom att köra hello PowerShell-kommando i en upphöjd kommandotolk:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Efter 20 minuter för too30 hello huvudnod vara redo. Du kan ansluta tooit från hello Azure-portalen genom att klicka på hello **Anslut** ikon för hello virtuell dator.

Du kanske slutligen toofix hello DNS-vidarebefordrare. toodo starta så DNS-hanteraren.

1. Högerklicka på hello-servernamnet i DNS-hanteraren, Välj **egenskaper**, och klicka sedan på hello **vidarebefordrare** fliken.
2. Klicka på hello **redigera** knappen tooremove alla vidarebefordrare och klicka sedan på **OK**.
3. Kontrollera att hello **använda rottips om det finns inga vidarebefordrare** kryssrutan är markerad och klicka sedan på **OK**.

## <a name="set-up-linux-compute-nodes"></a>Ställ in Linux compute-noder
Du distribuerar hello Linux compute-noder med hello samma Distributionsmall som du använde toocreate hello huvudnod.

Kopiera hello fil **MyCluster.xml** från din lokala dator toohello huvudnod och uppdatera hello **NodeCount** tagg med hello antalet noder som du vill toodeploy (< = 20). Vara försiktig toohave tillräckligt med tillgängliga kärnor i din Azure kvot eftersom varje A9-instansen kommer att använda 16 kärnor i prenumerationen. Du kan använda A8 instanser (8 kärnor) i stället för A9 om du vill toouse flera virtuella datorer i hello samma budget.

Kopiera hello HPC Pack IaaS distribution skript på hello huvudnod.

Kör hello följande Azure PowerShell-kommandon i en upphöjd kommandotolk:

1. Kör **Add-AzureAccount** tooconnect tooyour Azure-prenumeration.
2. Om du har flera prenumerationer kör **Get-AzureSubscription** toolist dem.
3. Ange en standard-prenumeration genom att köra hello **Välj AzureSubscription - SubscriptionName xxxx-standard** kommando.
4. Kör **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart distribuera datornoderna Linux.
   
   ![Huvudnod distribution i åtgärd][hndeploy]

Öppna hello HPC Pack Cluster Manager verktyget. Efter några minuter visas regelbundet Linux compute-noder i listan över compute-noder i klustret. Med hello klassisk distribution läge skapas IaaS-VM sekventiellt. Så om hello antalet noder är viktigt, kan hämtar alla distribuerade ta lång tid.

![Linux-noder i HPC Pack Cluster Manager][clustermanager]

Nu när alla noder är igång i hello klustret, finns det ytterligare infrastruktur inställningar toomake.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Konfigurera en Azure-filresurs för Windows och Linux-noder
Du kan använda hello Azure File service toostore skript och programpaket datafiler. Azure-filen innehåller CIFS ovanpå Azure Blob storage som beständiga arkivet. Tänk på att detta inte är hello mest skalbar lösning men den är hello enklaste en och kräver inte dedikerade virtuella datorer.

Skapa en filresurs i Azure genom att följa hello anvisningarna i hello artikeln [Kom igång med Azure File storage i Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).

Behåll hello namnet på ditt lagringskonto som **saname**, hello filresursnamn som **sharename**, och hello lagringskontonyckel som **sakey**.

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a>Montera hello Azure-filresurs på hello huvudnod
Öppna en upphöjd kommandotolk och kör hello efter kommandot toostore hello autentiseringsuppgifter i hello lokal dator valv:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Sedan toomount hello Azure-filresurs, kör:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a>Montera hello Azure-filresurs på Linux-datornoder
En användbar verktyget som medföljer HPC Pack är hello clusrun. Du kan använda det här kommandoradsverktyget toorun hello samma kommando samtidigt på en uppsättning compute-noder. I vårt fall har använt toomount hello Azure-filresurs och spara den toosurvive omstarter.
Kör hello följande kommandon i en upphöjd kommandotolk på hello huvudnod.

toocreate hello monteringskatalog:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

toomount hello Azure-filresurs:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

toopersist hello montera filresursen:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installera STAR-CCM +
Azure VM-instanser A8- och A9 tillhandahåller InfiniBand-stöd och RDMA-funktioner. hello kernel-drivrutiner som aktiverar dessa funktioner är tillgängliga för Windows Server 2012 R2, SUSE 12, CentOS 6.5 och CentOS 7.1 bilder i hello Azure Marketplace. Microsoft MPI och Intel MPI (utgåva 5.x) är hello två MPI bibliotek som har stöd för de drivrutinerna i Azure.

CD-simuleringar STAR-CCM + släpper 11.x och senare paketeras med Intel MPI version 5.x, så ingår InfiniBand-stöd för Azure.

Hämta hello Linux64 STAR-CCM + paket från hello [CD-simuleringar portal](https://steve.cd-adapco.com). I det här fallet används det version 11.02.010 i blandade precision.

I hello huvudnod i hello **/hpcdata** Azure File delar, skapa ett kommandoskript som heter **setupstarccm.sh** med hello följande innehåll. Det här skriptet ska köras på varje compute-nod tooset in STAR-CCM + lokalt.

#### <a name="sample-setupstarcmsh-script"></a>Exempelskript för setupstarcm.sh
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
Nu tooset in STAR-CCM + på alla Linux compute-noder, öppna en upphöjd kommandotolk och kör följande kommando hello:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Du kan övervaka hello CPU-användning med hjälp av hello termisk karta av Klusterhanterare medan hello kommandot körs. Efter några minuter måste alla noder vara korrekt inställt.

## <a name="run-star-ccm-jobs"></a>Kör STAR-CCM + jobb
HPC Pack används för jobbet Schemaläggaren i ordning toorun STAR-CCM + jobb. toodo så vi behöver hello stöd för några skript som används toostart hello jobb och köra STAR-CCM +. hello indata sparas på hello Azure-filresurs första för enkelhetens skull.

hello följande PowerShell-skript har använt tooqueue en stjärna-CCM + jobb. Det tar tre argument:

* hello modellnamn
* hello antalet noder toobe används
* hello antalet kärnor på varje nod toobe används

Eftersom STAR-CCM + kan fylla hello minnesbandbredd, det är oftast bättre toouse färre kärnor per datornoder och lägga till nya noder. hello exakta antalet kärnor per nod beror på hello processortypen och hello sammankoppling hastighet.

hello noder allokeras endast för hello jobbet och kan inte delas med andra jobb. hello jobbet startas inte som ett MPI-jobb direkt. Hej **runstarccm.sh** kommandoskript startar hello MPI starta.

hello ange modellen och hello **runstarccm.sh** skriptet lagras i hello **/hpcdata** resurs som tidigare har monterats.

Loggfilerna namnges med hello jobb-ID och lagras i hello **/hpcdata resursen**, tillsammans med hello STAR-CCM + utgående filer.

#### <a name="sample-submitstarccmjobps1-script"></a>Exempelskript för SubmitStarccmJob.ps1
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
Ersätt **runner.java** med din önskade stjärna-CCM + Java modellen programstart och loggning kod.

#### <a name="sample-runstarccmsh-script"></a>Exempelskript för runstarccm.sh
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

I vår test använde vi en licens Power på begäran-token. Denna token har tooset hello **$CDLMD_LICENSE_FILE** miljövariabeln för **1999@flex.cd-adapco.com**  och hello nyckel i hello **- podkey** alternativet hello kommandoraden .

Efter vissa initiering hello skript extraherar--från hello **$CCP_NODES_CORES** miljövariabler som HPC Pack in--hello listan över noder toobuild en hostfile som hello MPI programstart använder. Den här hostfile innehåller hello namnlista compute-nod som används för hello jobb, ett namn per rad.

hello format **$CCP_NODES_CORES** följer detta mönster:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Där:

* `<Number of nodes>`är hello antalet noder som har allokerats toothis jobb.
* `<Name of node_n_...>`är hello namnet på varje nod som allokerats toothis jobb.
* `<Cores of node_n_...>`är hello antalet kärnor på hello nod allokerade toothis jobb.

Hej antal kärnor (**$NBCORES**) är också beräknat utifrån hello antalet noder (**$NBNODES**) och hello antalet kärnor per nod (som anges som parameter **$NBCORESPERNODE**).

För hello MPI alternativ är hello de som används med Intel MPI på Azure

* `-mpi intel`toospecify Intel MPI.
* `-fabric UDAPL`toouse Azure InfiniBand verb.
* `-cpubind bandwidth,v`toooptimize bandbredd för MPI med stjärna-CCM +.
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`toomake Intel MPI arbeta med Azure InfiniBand och tooset hello krävs antalet kärnor per nod.
* `-batch`toostart STAR-CCM + i batchläge med något användargränssnitt.

Slutligen toostart ett jobb, se till att noderna är igång och körs och är online i hanteraren för. Från en PowerShell-Kommandotolken kör detta:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Stoppa noder
Vid ett senare tillfälle när du är klar med dina tester kan användas för följande HPC Pack PowerShell-kommandon toostop hello och starta noder:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Nästa steg
Försök att köra andra Linux-arbetsbelastningar. Se exempelvis:

* [Kör NAMD med Microsoft HPC Pack på Linux compute-noder i Azure](hpcpack-cluster-namd.md)
* [Köra OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
