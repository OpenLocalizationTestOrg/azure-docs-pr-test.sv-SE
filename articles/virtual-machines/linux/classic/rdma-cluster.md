---
title: aaaSet in en Linux RDMA klustret toorun MPI program | Microsoft Docs
description: "Skapa ett Linux-kluster i storlek H16r H16mr, A8 eller A9 VMs toouse hello Azure RDMA nätverk toorun MPI appar"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a>Ställa in en Linux RDMA klustret toorun MPI-program
Lär dig hur tooset in RDMA en Linux-kluster i Azure med [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun parallella Message Passing Interface (MPI) program. Den här artikeln innehåller steg tooprepare en Linux HPC avbildningen toorun Intel MPI på ett kluster. Efter förberedelse, kan du distribuera ett kluster för virtuella datorer med hjälp av den här avbildningen och en av hello RDMA-kompatibla Azure VM-storlekar (för närvarande H16r H16mr, A8 eller A9). Använd hello klustret toorun MPI program som effektiv kommunicerar över låg latens, hög genomströmning nätverk som baseras på direktåtkomst till fjärrminne (RDMA)-teknik.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

## <a name="cluster-deployment-options"></a>Distributionsalternativ för kluster
Följande är metoder du kan använda toocreate ett Linux RDMA-kluster med eller utan en jobbschemaläggaren.

* **Azure CLI-skript**: enligt nedan använder hello [Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md) (CLI) tooscript hello distributionen av ett kluster med RDMA-kompatibla virtuella datorer. hello CLI i Service Management-läge skapar hello klusternoder följd i hello klassiska distributionsmodellen så distribuerar många compute-noder kan ta flera minuter. tooenable hello RDMA-nätverksanslutning när du använder hello klassiska distributionsmodellen distribuera hello virtuella datorer i hello samma molntjänst.
* **Azure Resource Manager-mallar**: du kan också använda hello Resource Manager distribution modellen toodeploy ett kluster med RDMA-kompatibla virtuella datorer som ansluter toohello RDMA. Du kan [skapa en egen mall](../../../resource-group-authoring-templates.md), eller kontrollera hello [Azure quickstart mallar](https://azure.microsoft.com/documentation/templates/) för mallar som har bidragit med Microsoft eller hello community toodeploy hello lösningen som du söker. Resource Manager-mallar kan ge en snabb och tillförlitlig sätt toodeploy ett Linux-kluster. tooenable hello RDMA-nätverksanslutning när du använder hello Resource Manager-distributionsmodellen, distribuera hello virtuella datorer i hello samma tillgänglighetsuppsättning.
* **HPC Pack**: skapa ett Microsoft HPC Pack-kluster i Azure och lägga till RDMA-kompatibla compute-noder som kör ett stöds Linux distribution tooaccess hello RDMA-nätverk. Mer information finns i [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-hello-classic-model"></a>Exempel distributionsstegen i hello klassiska modellen
hello följande steg visar hur toouse hello Azure CLI toodeploy SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM från hello Azure Marketplace, anpassa den och skapa en anpassad VM-avbildning. Du kan använda hello tooscript hello distribution av avbildningar i ett kluster med RDMA-kompatibla virtuella datorer.

> [!TIP]
> Använd liknande steg toodeploy ett kluster med RDMA-kompatibla virtuella datorer baserat på CentOS-baserade HPC-avbildningar i hello Azure Marketplace. Vissa av stegen variera något som anges. 
>
>

### <a name="prerequisites"></a>Krav
* **Klientdatorn**: du behöver en Mac, Linux eller Windows-klienten datorn toocommunicate med Azure. De här stegen förutsätter att du använder en Linux-klient.
* **Azure-prenumeration**: Om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter. Överväg att en prenumeration med användningsbaserad betalning eller andra köpalternativ för större kluster.
* **Tillgänglighet för VM-storlek**: hello följande instanser är RDMA-kompatibla: H16r, H16mr, A8 och A9. Kontrollera [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/) för tillgänglighet i Azure-regioner.
* **Kärnor kvoten**: du kan behöva tooincrease hello kvoten för kärnor toodeploy ett kluster med beräkningsintensiva virtuella datorer. Du måste till exempel minst 128 kärnor, om du vill toodeploy 8 A9 virtuella datorer som visas i den här artikeln. Din prenumeration kan också begränsa hello antal kärnor som du kan distribuera i vissa VM storlek familjer, inklusive hello H-serien. toorequest en kvot ökar, [öppna en supportbegäran online customer](../../../azure-supportability/how-to-create-azure-support-request.md) utan kostnad.
* **Azure CLI**: [installera](../../../cli-install-nodejs.md) hello Azure CLI och [ansluta tooyour Azure-prenumeration](../../../xplat-cli-connect.md) från hello-klientdator.

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>Etablera en SLES 12 SP1 HPC-dator
När du har loggat in tooAzure med hello Azure CLI kör `azure config list` tooconfirm som hello utdata visar Service Management-läge. Om det inte ange hello läge genom att köra det här kommandot:

    azure config mode asm


Skriv följande toolist hello alla hello prenumerationer som du är behörig toouse:

    azure account list

hello aktuella aktiv prenumeration identifieras med `Current` ställa in också`true`. Om den här prenumerationen inte hello som du vill toouse toocreate hello klustret genom att ange hello lämpliga prenumerations-ID som hello aktiv prenumeration:

    azure account set <subscription-Id>

toosee hello offentligt tillgängliga SLES 12 SP1 HPC-avbildningar i Azure, köra ett kommando som hello efter, förutsatt att din miljö har stöd för shell **grep**:

    azure vm image list | grep "suse.*hpc"

Etablera en RDMA-kompatibla virtuell dator med en SLES 12 SP1 HPC-avbildning genom att köra ett kommando som hello följande:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Där:

* Hej storlek (A9 i det här exemplet) är en av RDMA-kompatibla hello VM-storlekar.
* hello externa SSH-portnumret (22 i det här exemplet är hello SSH standard) är ett giltigt portnummer. hello interna SSH-portnumret anges too22.
* En ny molntjänst skapas i hello Azure-region som anges av hello plats. Ange en plats som du väljer är tillgänglig i vilka hello VM.
* SUSE prioritet support (som debiteras ytterligare) hello SLES 12 SP1 avbildningsnamn för närvarande kan vara något av följande två alternativ: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a>Anpassa hello VM
När hello VM har slutförts etablering SSH toohello VM med hjälp av hello extern IP-adress för Virtuella datorer (eller DNS-namn) och hello Externt portnummer du konfigurerade och anpassa den. Anslutningsinformation finns [hur toolog på tooa virtuell dator som kör Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Utför kommandon som hello användaren du konfigurerat på hello VM, såvida inte rotåtkomst är obligatoriska toocomplete ett steg.

> [!IMPORTANT]
> Microsoft Azure tillhandahåller inte rotåtkomst tooLinux virtuella datorer. toogain administrativ åtkomst när du är ansluten som en användare toohello virtuell dator som kör kommandon med hjälp av `sudo`.
>
>

* **Uppdateringar**: Installera uppdateringar med hjälp av zypper. Du kan också tooinstall NFS verktyg.

  > [!IMPORTANT]
  > I en SLES 12 SP1 HPC VM rekommenderar vi att du inte installerar kernel-uppdateringar, vilket kan orsaka problem med hello Linux RDMA drivrutiner.
  >
  >
* **Intel MPI**: Slutför hello installationen av Intel MPI på hello SLES 12 SP1 HPC VM genom att köra följande kommando hello:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Låsa minne**: för MPI koder toolock hello minne tillgängligt för RDMA, lägga till eller ändra följande inställningar i hello /etc/security/limits.conf filen hello. Du måste roten åtkomst tooedit den här filen.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > I testsyfte kan ange du också memlock toounlimited. Till exempel: `<User or group name>    hard    memlock unlimited`. Mer information finns i [kända bästa metoder för inställningen låst minnesstorleken](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SSH-nycklar för virtuella datorer SLES**: Generera SSH-nycklar tooestablish förtroende för ditt användarkonto bland hello compute-noder i hello SLES kluster när du kör MPI-jobb. Om du har distribuerat en CentOS-baserade HPC VM inte följer du detta steg. Instruktioner senare i den här artikeln tooset passwordless SSH-förtroende mellan hello klusternoderna finns när du hello avbildning och distribuera hello-kluster.

    toocreate SSH-nycklar, kör följande kommando hello. När du uppmanas att ange indata väljer **RETUR** toogenerate hello nycklar i hello standardplatsen utan att ange ett lösenord.

        ssh-keygen

    Tilläggsfilen hello offentliga nyckel toohello authorized_keys för kända offentliga nycklar.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    I hello ~/.ssh directory, redigera eller skapa hello config-fil. Ange hello IP-adressintervall hello privat nätverk som du planerar toouse i Azure (10.32.0.0/16 i det här exemplet):

        host 10.32.0.*
        StrictHostKeyChecking no

    Du kan också visa hello privat nätverk IP-adressen för varje virtuell dator i klustret:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Konfigurera `StrictHostKeyChecking no` kan skapa en säkerhetsrisk om en specifik IP-adress eller intervall har angetts.
  >
  >
* **Program**: installera alla program som du behöver eller utföra andra anpassningar innan hello avbildning.

### <a name="capture-hello-image"></a>Hello avbildning
toocapture hello avbildning, kör följande kommando på hello Linux VM hello först. Det här kommandot deprovisions hello VM men underhåller användarkonton och SSH-nycklar som du skapat.

```
sudo waagent -deprovision
```

Kör följande Azure CLI-kommandon toocapture hello avbildningen hello från klientdatorn. Mer information finns i [hur toocapture en klassisk virtuell Linux-dator som en bild](capture-image.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

När du har kört dessa kommandon hello VM avbildningen har gjorts för din användning och hello VM tas bort. Nu har du en anpassad avbildning redo toodeploy ett kluster.

### <a name="deploy-a-cluster-with-hello-image"></a>Distribuera ett kluster med hello avbildning
Ändra hello följande Bash-skript med lämpliga värden för din miljö och köra den från din klientdator. Eftersom Azure distribuerar hello VMs följd i hello klassiska distributionsmodellen, tar det några minuter toodeploy hello åtta A9 virtuella datorer i det här skriptet.

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Överväganden för ett CentOS HPC-kluster
Tooset upp ett baserat på en av hello CentOS-baserade HPC-avbildningar i hello Azure Marketplace i stället för SLES 12 för HPC-kluster, så du hello allmänna steg i hello föregående avsnitt. Observera följande skillnader när du etablerar och konfigurerar hello VM hello:

- Intel MPI är redan installerad på en virtuell dator etablerats från en CentOS-baserade HPC-avbildning.
- Lås minnesinställningarna har redan lagts till i hello VM /etc/security/limits.conf fil.
- Generera inte SSH-nycklar på hello VM som du etablerar för avbildning. Vi rekommenderar i stället konfigurera användare för autentisering när du distribuerar hello-kluster. Mer information finns i följande avsnitt hello.  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a>Ställ in passwordless SSH-förtroende på hello klustret
Det finns två metoder för att upprätta förtroende mellan hello compute-noder på ett CentOS-baserade HPC-kluster: värd-baserad autentisering och användaren-baserad autentisering. Värd-baserad autentisering utanför hello omfånget för den här artikeln och vanligen måste göras via ett skript med filnamnstillägget under distributionen. Användare-baserad autentisering är praktiskt för att upprätta förtroende efter distributionen och kräver hello generation och delning av SSH-nycklar mellan hello compute-noder i klustret hello. Den här metoden kallas passwordless SSH-inloggning och krävs när du kör MPI-jobb.

Ett exempelskript som bidrog från hello community är tillgängligt på [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable enkelt användarautentisering på ett CentOS-baserade HPC-kluster. Hämta och använda det här skriptet med hjälp av hello följande steg. Du kan också ändra det här skriptet eller använda några andra metoden tooestablish passwordless SSH-autentisering mellan hello beräkning klusternoder.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

toorun hello skript, behöver du tooknow hello prefix för IP-adresser för undernät. Hämta hello prefix genom att köra följande kommando på en av klusternoderna hello hello. Din utdata ska se ut ungefär 10.1.3.5 och hello prefixet är hello 10.1.3 del.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Nu köra hello skript med tre parametrar: hello vanliga användarnamn på hello compute-noder, hello gemensamt lösenord för användaren på hello datornoder och hello undernätsprefixets som returnerades från föregående hello-kommando.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Det här skriptet hello följande:

* Skapar en katalog på hello värdnoden med namnet .ssh, vilket krävs för passwordless inloggningen.
* Skapar en konfigurationsfil i hello .ssh katalog som instruerar passwordless inloggning tooallow inloggning från alla noder i klustret hello.
* Skapa filer som innehåller hello nod-namn och IP-adresser i noden för alla hello-noder i klustret hello. De här filerna finns kvar efter hello skriptet körs för senare referens.
* Skapar en privata och offentliga nyckel för varje nod i klustret (inklusive hello värdnoden) och skapar poster i hello authorized_keys fil.

> [!WARNING]
> Kör skriptet kan du skapa en säkerhetsrisk. Se till att hello offentlig nyckelinformation i ~/.ssh inte distribueras.
>
>

## <a name="configure-intel-mpi"></a>Konfigurera Intel MPI
toorun MPI program på Azure Linux RDMA, måste tooconfigure vissa miljö variabler specifika tooIntel MPI. Här är ett exempel Bash-skript tooconfigure hello variabler som behövs toorun ett program. Ändra hello sökvägen toompivars.sh som behövs för installationen av Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

hello hello värden filens format är som följer. Lägg till en rad för varje nod i klustret. Ange den privata IP-adresser från hello virtuella nätverk som definierats tidigare, inte DNS-namn. Till exempel innehåller två värdar med IP-adresser 10.32.0.1 och 10.32.0.2 hello filen hello följande:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Kör MPI på en grundläggande kluster med två noder
Om du inte redan har gjort först ställa in hello miljö för Intel MPI.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Kör MPI-kommando
Kör MPI-kommando på en av hello compute-noder tooshow som MPI har installerats korrekt och kan kommunicera mellan minst två compute-noder. hello följande **mpirun** körs hello **värdnamn** på två noder.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Dina utdata bör innehålla hello namnen på alla noder i hello som du har överfört som indata för `-hosts`. Till exempel en **mpirun** kommando med två noder returnerar utdata som liknar hello följande:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Kör en MPI prestandamått
hello följande Intel MPI kommando kör en pingpong benchmark tooverify hello konfiguration och anslutningen toohello RDMA klusternätverk.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Du bör se utdata som liknar följande hello på ett fungerande kluster med två noder. Förvänta dig fördröjning vid eller under 3 mikrosekunder för meddelandestorleken in too512 byte i hello Azure RDMA nätverk.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks toorun:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Nästa steg
* Distribuera och köra din Linux MPI program på Linux-kluster.
* Se hello [Intel MPI biblioteket dokumentationen](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) vägledning om Intel MPI.
* Försök en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate en Intel Lyster kluster med hjälp av en CentOS-baserade HPC-avbildning. Mer information finns i [distribuera Intel molnet Edition för Lyster på Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
