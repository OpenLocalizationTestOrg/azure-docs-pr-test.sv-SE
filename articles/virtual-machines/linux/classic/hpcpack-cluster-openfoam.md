---
title: "aaaRun OpenFOAM med HPC Pack på virtuella Linux-datorer | Microsoft Docs"
description: "Distribuera ett kluster för Microsoft HPC Pack på Azure och kör en OpenFOAM jobb på flera Linux compute-noder i ett nätverk med RDMA."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Kör OpenFoam med Microsoft HPC Pack på ett Linux RDMA-kluster i Azure
Den här artikeln visar enkelriktade toorun OpenFoam i virtuella Azure-datorer. Här kan du distribuera ett Microsoft HPC Pack kluster med Linux compute-noder på Azure och kör en [OpenFoam](http://openfoam.com/) jobbet med Intel MPI. Du kan använda RDMA-kompatibla virtuella Azure-datorer för hello compute-noder så att hello datornoderna kommunicera över hello Azure RDMA nätverk. Andra alternativ toorun OpenFoam i Azure är helt konfigurerade kommersiellt tillgängliga avbildningarna för hello Marketplace, till exempel Ubercloud's [OpenFoam 2.3 på CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), och genom att köra på [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (för åtgärden Öppna fältet och manipulering) är en öppen källkod beräkningsprogram (CFD)-programpaket som används ofta i tekniker och vetenskap i både kommersiella och academic organisationer. Den innehåller verktyg för meshing, särskilt snappyHexMesh en parallelized mesher för komplexa CAD-geometrier och för före och efter bearbetning. Nästan alla processer som körs parallellt, aktivera användare tootake full nytta av datormaskinvara tillgång.  

Microsoft HPC Pack ger funktioner toorun storskaliga HPC och parallella program, inklusive MPI program på kluster av Microsoft Azure-datorer. HPC Pack stöder också körs Linux HPC-program på Linux-beräkningsnod virtuella datorer distribueras i ett kluster med HPC Pack. Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) för en introduktion toousing Linux compute-noder med HPC Pack.

> [!NOTE]
> Den här artikeln beskrivs hur toorun en Linux MPI arbetsbelastning med HPC Pack. Det förutsätts att du vara bekant med Linux systemadministration och MPI arbetsbelastningar som körs på Linux-kluster. Om du använder versioner av MPI och OpenFOAM skiljer sig från hello som visas i den här artikeln kan kanske du toomodify vissa installations- och konfigurationssteg. 
> 
> 

## <a name="prerequisites"></a>Krav
* **HPC Pack kluster med RDMA-kompatibla Linux datornoder** – distribuera ett HPC Pack kluster med storlek A8 A9, H16r, eller H16rm Linux compute-noder med antingen en [Azure Resource Manager-mall](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) eller en [Azure PowerShell-skriptet](hpcpack-cluster-powershell-script.md). Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) hello krav och anvisningar för något av alternativen. Om du väljer hello distributionsalternativ för PowerShell-skript finns i hello exempelkonfigurationsfilen i hello exempelfiler hello slutet av den här artikeln. Använd den här konfigurationen toodeploy en Azure-baserade HPC Pack kluster som består av en storlek A8 Windows Server 2012 R2 huvudnod och 2 storlek A8 SUSE Linux Enterprise Server 12 compute-noder. Ersätt lämpliga värden för namn på din prenumeration och tjänsten. 
  
  **Ytterligare saker tooknow**
  
  * Linux RDMA nätverk krav i Azure, se [högpresterande compute VM-storlekar](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Om du använder hello distributionsalternativ för Powershell-skript kan du distribuera alla hello Linux compute-noder i ett moln service toouse hello RDMA-nätverksanslutning.
  * Anslut med SSH tooperform ytterligare administrativa uppgifter när du har distribuerat hello Linux noder. Hitta hello SSH-anslutningsinformationen för varje Linux-VM i hello Azure-portalen.  
* **Intel MPI** -toorun OpenFOAM på SLES 12 HPC compute-noder i Azure måste du tooinstall hello Intel MPI biblioteket 5 runtime från hello [Intel.com plats](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 är förinstallerat på CentOS-baserade HPC bilder.)  Installera Intel MPI på dina Linux compute-noder i ett senare steg om det behövs. tooprepare för det här steget när du har registrerat med Intel, länken hello i hello bekräftelse e-toohello relaterade webbsida. Kopiera hello hämta hello .tgz filen för hello lämplig version av Intel MPI-länk. Den här artikeln är baserad på Intel MPI version 5.0.3.048.
* **OpenFOAM källa Pack** -ladda ned hello OpenFOAM källa Pack programvara för Linux från hello [OpenFOAM Foundation-webbplatsen](http://openfoam.org/download/2-3-1-source/). Den här artikeln är baserad på källan Pack-version 2.3.1-baserad, hämtas som OpenFOAM 2.3.1.tgz. Följ instruktionerna för hello senare i den här artikeln toounpack och kompilera OpenFOAM på hello Linux compute-noder.
* **EnSight** (valfritt) – toosee hello resultaten av OpenFOAM simuleringen, hämta och installera hello [EnSight](https://www.ceisoftware.com/download/) visualisering och analys av programmet. Licensierings-och hämta är hello EnSight plats.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Ställ in ömsesidigt förtroende mellan beräkningsnoder
Kör ett jobb mellan noder på flera Linux-noder kräver hello noder tootrust varandra (av **rsh** eller **ssh**). När du skapar hello HPC Pack kluster med hello Microsoft HPC Pack IaaS distributionsskriptet konfigurerar hello skript automatiskt permanenta ömsesidigt förtroende för hello administratörskontot som du anger. Vanliga användare som du skapar i hello klustrets domän, har du tooset tillfälliga ömsesidigt förtroende mellan hello noder när ett jobb har allokerats toothem och förstöra hello relationen när hello jobbet har slutförts. tooestablish förtroende för varje användare måste ange ett RSA nyckelpar toohello kluster med HPC Pack för hello förtroende.

### <a name="generate-an-rsa-key-pair"></a>Generera en RSA-nyckelpar
Det är enkelt toogenerate ett RSA-nyckelpar, som innehåller en offentlig nyckel och en privat nyckel, genom att köra hello Linux **ssh-keygen** kommando.

1. Logga in tooa Linux-dator.
2. Kör följande kommando hello:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Tryck på **RETUR** toouse hello-standardinställningarna tills hello-kommandot har slutförts. Ange inte en lösenfras här. När du uppmanas att ange ett lösenord trycker du bara på **RETUR**.
   > 
   > 
   
   ![Generera en RSA-nyckelpar][keygen]
3. Ändra katalogen toohello ~/.ssh katalog. hello privata nyckeln lagras i id_rsa och hello offentliga nyckeln i id_rsa.pub.
   
   ![Privata och offentliga nycklar][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a>Lägga till hello nyckelpar toohello HPC Pack kluster
1. Gör en anslutning till Fjärrskrivbord anslutning tooyour huvudnod med ditt administratörskonto HPC Pack (hello-administratörskontot som du ställer in när du körde hello distributionsskriptet).
2. Använd standard Windows Server procedurer toocreate ett domänanvändarkonto i hello klustrets Active Directory-domän. Till exempel använda hello Active Directory-användare och datorer verktyget i hello huvudnod. hello exemplen i den här artikeln förutsätter att du skapar en domänanvändare med namnet hpclab\hpcuser.
3. Skapa en fil med namnet C:\cred.xml och kopiera hello RSA viktiga data till den. En exempelfil cred.xml är hello slutet av den här artikeln.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Öppna en kommandotolk och ange följande kommando tooset hello autentiseringsuppgifter data för hello hpclab\hpcuser konto hello. Du använder hello **extendeddata** parametern toopass hello namnet C:\cred.xml fil du skapade hello viktiga data.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Det här kommandot har slutförts utan utdata. Lagra hello cred.xml filen på en säker plats när du har angett hello autentiseringsuppgifter för hello användarkonton som du behöver toorun jobb, eller ta bort den.
5. Om du har genererat hello RSA-nyckelpar på en Linux-noder kan du komma ihåg toodelete hello nycklar när du är klar med dem.. Om HPC Pack hittar en befintlig id_rsa- eller id_rsa.pub fil, anger den inte ömsesidigt förtroende.

> [!IMPORTANT]
> Vi rekommenderar inte kör ett Linux-jobb som en Klusteradministratör i ett delat kluster, eftersom ett jobb som skickats av en administratör köras under rotkontot hello på hello Linux noder. Dock körs ett jobb som skickats av en icke-administratörer under ett lokalt användarkonto för Linux med hello samma namn som hello jobbet användare. I det här fallet konfigurerar HPC Pack ömsesidigt förtroende för den här användaren Linux över hello noder allokeras toohello jobb. Du kan ställa in hello Linux användare manuellt på hello Linux noder innan du kör jobbet hello eller HPC Pack skapar hello användaren automatiskt när hello jobbet har skickats. Om HPC Pack skapar hello användare, HPC Pack tar bort den efter hello jobbet har slutförts. tooreduce säkerhetshot, HPC Pack tar bort hello nycklar när jobbet har slutförts.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Skapa en filresurs för Linux-noder
Nu ställa in en vanlig SMB-resurs på en mapp på hello huvudnod. tooallow hello Linux noder tooaccess programfiler med en sökväg, montera hello delad mapp på hello Linux noder. Om du vill kan använda du ett annat alternativ, till exempel filer för Azure-resurs - rekommenderas för många scenarier- eller en NFS-resurs för fildelning. Se hello fildelning information och detaljerade anvisningar i [komma igång med Linux compute-noder i ett HPC Pack-kluster i Azure](hpcpack-cluster.md).

1. Skapa en mapp på hello huvudnod och dela den tooEveryone genom att ange behörighet för läsning och skrivning. Till exempel dela C:\OpenFOAM i hello huvudnod som \\ \\SUSE12RDMA HN\OpenFOAM. Här, *SUSE12RDMA HN* är hello värdnamnet på hello huvudnod.
2. Öppna Windows PowerShell-fönstret och kör följande kommandon hello:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

hello första kommandot skapar en mapp med namnet /openfoam på alla noder i hello LinuxNodes grupp. hello andra kommandot monterar hello delade mappen //SUSE12RDMA-HN/OpenFOAM på hello Linux noder med dir_mode och file_mode bits set too777. Hej *användarnamn* och *lösenord* hello kommandot ska vara hello autentiseringsuppgifterna för en användare i hello huvudnod.

> [!NOTE]
> Hej ”\`” symbol i andra hello-kommandot är en symbolen för PowerShell. ”\`,” innebär hello ””, (kommatecken tecken) är en del av hello-kommando.
> 
> 

## <a name="install-mpi-and-openfoam"></a>Installera MPI och OpenFOAM
toorun OpenFOAM som ett MPI-jobb i hello RDMA nätverk måste toocompile OpenFOAM med hello Intel MPI-bibliotek. 

Kör först flera **clusrun** kommandon tooinstall Intel MPI bibliotek (om det inte redan är installerat) och OpenFOAM på Linux-noder. Använd hello huvudnod resursen konfigurerade tidigare tooshare hello installationsfiler hello Linux noderna.

> [!IMPORTANT]
> Dessa installation och kompilerar stegen är exempel. Du måste viss erfarenhet av Linux system administration tooensure att beroende kompilerare och bibliotek har installerats korrekt. Du kanske måste toomodify vissa miljövariabler eller andra inställningar för din version av Intel MPI och OpenFOAM. Mer information finns i [Intel MPI-biblioteket för Linux-installationsguiden](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) och [OpenFOAM källa installationen](http://openfoam.org/download/2-3-1-source/) för din miljö.
> 
> 

### <a name="install-intel-mpi"></a>Installera Intel MPI
Spara hello hämtade installationspaketet för Intel MPI (l_mpi_p_5.0.3.048.tgz i det här exemplet) i C:\OpenFoam i hello huvudnod så att hello Linux noder kan komma åt den här filen från /openfoam. Kör sedan **clusrun** tooinstall Intel MPI-biblioteket på alla noder av hello Linux.

1. hello följande kommandon kopiera hello installationspaketet och extraherar du det för/opt/intel på varje nod.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. tooinstall Intel MPI biblioteket använda tyst, en silent.cfg-fil. Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln. Placera den här filen i hello delade mappen /openfoam. Mer information om hello silent.cfg fil finns [Intel MPI-biblioteket för installationsguiden för Linux - tyst Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Kontrollera att du sparar filen som en textfil med Linux silent.cfg radbrytningar (endast LF, inte CR LF). Det här steget säkerställer att den körs korrekt på hello Linux noder.
   > 
   > 
3. Installera Intel MPI biblioteket i tyst läge.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>Konfigurera MPI
För att testa, bör du lägga till följande rader toohello /etc/security/limits.conf på varje nod för hello Linux hello:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Starta om hello Linux noder när du uppdaterar hello limits.conf fil. Till exempel använder följande hello **clusrun** kommando:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Se till att den delade mappen hello monterade som /openfoam efter omstart.

### <a name="compile-and-install-openfoam"></a>Kompilera och installera OpenFOAM
Spara hello hämtade installationspaketet för hello OpenFOAM källa Pack (OpenFOAM-2.3.1.tgz i det här exemplet) tooC:\OpenFoam i hello huvudnod så att hello Linux noder kan komma åt den här filen från /openfoam. Kör sedan **clusrun** kommandon toocompile OpenFOAM på alla hello Linux-noder.

1. Skapa en mapp /opt/OpenFOAM på varje Linux-nod, kopiera hello paketet toothis källmappen, och extrahera det det.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. toocompile OpenFOAM med hello Intel MPI biblioteket först ställa in miljövariabler för både Intel MPI och OpenFOAM. Använd ett bash-skript som heter settings.sh tooset hello variabler. Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln. Placera den här filen (som sparas med radbrytningar för Linux) i hello delade mappen /openfoam. Den här filen innehåller inställningar för hello MPI och OpenFOAM körningar som du använder senare toorun ett OpenFOAM-jobb.
3. Installera beroende paket som behövs toocompile OpenFOAM. Beroende på Linux-distribution måste du kanske först tooadd en databas. Kör **clusrun** liknande toohello följande kommandon:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Om det behövs kommandon SSH tooeach Linux nod toorun hello tooconfirm som de fungerar korrekt.
4. Kör följande kommando toocompile OpenFOAM hello. hello kompileringsprocessen tar några tid toocomplete och genererar en stor mängd information toostandard utdata, så Använd hello **/ överlagrat** alternativet toodisplay hello utdata överlagrat.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > Hej ”\`” symbol i hello-kommandot är en symbolen för PowerShell. ”\`&” innebär hello ”&” är en del av hello-kommando.
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a>Förbereda toorun ett OpenFOAM-jobb
Nu get redo toorun ett MPI-jobb kallas sloshingTank3D som är en av hello OpenFoam prov på två Linux-noder. 

### <a name="set-up-hello-runtime-environment"></a>Ställ in hello körningsmiljön
tooset in hello runtime miljöer för MPI och OpenFOAM hello Linux-noder som kör följande kommando i Windows PowerShell-fönstret på hello huvudnod hello. (Det här kommandot är giltig för SUSE Linux endast.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Förbereda exempeldata
Använd hello huvudnod resursen du tidigare konfigurerat tooshare filer mellan hello Linux noder (monterade som /openfoam).

1. SSH tooone av din Linux compute-noder.
2. Kör hello efter kommandot tooset in hello OpenFOAM körningsmiljö, om du inte redan har gjort detta.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Kopiera hello sloshingTank3D toohello delade mapp och navigera tooit.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. När du använder hello-standardparametrar för det här exemplet, kan det ta flera minuter toorun så du kanske vill toomodify vissa parametrar toomake det snabbare. Ett enkelt alternativ är toomodify hello tid step variabler deltaT och writeInterval i hello system/controlDict-filen. Den här filen lagras alla indata som rör toohello kontroll över tid och läsning och skrivning av data för lösningen. Du kan till exempel ändra hello värde av deltaT från 0,05 too0.5 och writeInterval från 0,05 too0.5 hello värde.
   
   ![Ändra steg variabler][step_variables]
5. Ange önskade värden för hello variabler i hello system/decomposeParDict-filen. Det här exemplet använder två Linux noder varje med 8 kärnor, så ange numberOfSubdomains too16 och n för hierarchicalCoeffs too(1 1 16), vilket betyder att köra OpenFOAM parallellt med 16 processer. Mer information finns i [OpenFOAM användarhandboken: 3,4 program som körs parallellt](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Dela upp processer][decompose]
6. Kör följande kommandon från hello sloshingTank3D directory tooprepare hello exempeldata hello.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Du bör se hello exempeldatafiler kopieras till C:\OpenFoam\sloshingTank3D i hello huvudnod. (C:\OpenFoam är hello delad mapp på hello huvudnod.)
   
   ![Datafiler på hello huvudnod][data_files]

### <a name="host-file-for-mpirun"></a>Värdfilen för mpirun
I det här steget skapar du en värd fil (en lista över compute-noder) vilka hello **mpirun** kommando använder.

1. På en av hello Linux noder, skapa en fil med namnet hostfile under /openfoam, så den här filen kan nås på /openfoam/hostfile på alla noder i Linux.
2. Skriv din Linux nod-namn i den här filen. I det här exemplet innehåller hello filen hello följande namn:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Du kan också skapa den här filen på C:\OpenFoam\hostfile i hello huvudnod. Om du väljer det här alternativet, spara den som en textfil med Linux radbrytningar (endast LF, inte CR LF). Detta säkerställer att den körs korrekt på hello Linux noder.
   > 
   > 
   
   **Omslutning för Bash-skript**
   
   Om du har många Linux noder och du vill att dina jobb toorun på bara några av dem, är det inte en bra idé toouse en fast värd-fil, eftersom du inte vet vilka noder allokeras tooyour jobb. I det här fallet skriva ett bash-skript Omslutning för **mpirun** toocreate hello värdfilen automatiskt. Du kan hitta ett exempel bash-skript wrapper kallas hpcimpirun.sh hello slutet av den här artikeln och spara den som /openfoam/hpcimpirun.sh. Det här exempelskriptet hello följande:
   
   1. Ställer in hello miljövariabler för **mpirun**, och vissa tillägg kommandot parametrar toorun hello MPI jobb via hello RDMA nätverk. I det här fallet anger hello följande variabler:
      
      * I_MPI_FABRICS = shm:dapl
      * I_MPI_DAPL_PROVIDER = en-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Skapar en värd-fil enligt toohello miljö variabeln $CCP_NODES_CORES som anges av hello HPC-huvudnoden när hello jobbet är aktiverad.
      
      hello format för $CCP_NODES_CORES följer detta mönster:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      där
      
      * `<Number of nodes>`-hello antalet noder allokeras toothis jobb.  
      * `<Name of node_n_...>`-hello namnet på varje nod allokerade toothis jobb.
      * `<Cores of node_n_...>`-hello antalet kärnor på hello nod allokerade toothis jobb.
      
      Till exempel om hello jobbet måste två noder toorun, liknar $CCP_NODES_CORES
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Anrop hello **mpirun** kommando och lägger till två parametrar toohello-kommandoraden.
      
      * `--hostfile <hostfilepath>: <hostfilepath>`-hello sökvägen hello värden hello skript skapar
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-en miljövariabel som angetts av hello HPC Pack huvudnod, som lagrar hello antal totala kärnor allokerade toothis jobb. I det här fallet den anger hello antal processer för **mpirun**.

## <a name="submit-an-openfoam-job"></a>Skicka ett OpenFOAM-jobb
Nu kan du skicka ett jobb i HPC Cluster Manager. Du måste toopass hello skriptet hpcimpirun.sh i hello kommandorader för några av hello jobbuppgifter.

1. Anslut tooyour klustrets huvudnod och starta HPC Cluster Manager.
2. **I resurshantering**, se till att hello Linux datornoderna hello **Online** tillstånd. Om du inte markerar du dem och klickar på **Anslut**.
3. I **jobbhantering**, klickar du på **nytt jobb**.
4. Ange ett namn för jobbet som *sloshingTank3D*.
   
   ![Jobbinformation][job_details]
5. I **jobbet resurser**, Välj hello typ av resurs som ”nod” och ange hello minsta too2. Den här konfigurationen kör hello jobb på två noder i Linux, var och en har åtta kärnor i det här exemplet.
   
   ![Jobbet resurser][job_resources]
6. Klicka på **redigera uppgifter** i hello vänstra navigeringsfönstret och klicka sedan på **Lägg till** tooadd en toohello aktiviteten. Lägg till fyra uppgifter toohello jobbet med hello följande kommandorader och inställningar.
   
   > [!NOTE]
   > Kör `source /openfoam/settings.sh` ställer in hello OpenFOAM och MPI runtime miljöer, så var och en av följande uppgifter hello anropar den innan hello OpenFOAM kommando.
   > 
   > 
   
   * **Uppgift 1**. Kör **decomposePar** toogenerate filer för att köra **interDyMFoam** parallellt.
     
     * Tilldela en nod toohello aktivitet
     * **Kommandorad** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Arbetskatalogen** -/ openfoam/sloshingTank3D
     
     Se följande figur hello. Du konfigurerar hello återstående uppgifterna på samma sätt.
     
     ![Uppgift 1 information][task_details1]
   * **Uppgift 2**. Kör **interDyMFoam** i parallella toocompute hello exemplet.
     
     * Tilldela två noder toohello uppgift
     * **Kommandorad** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Arbetskatalogen** -/ openfoam/sloshingTank3D
   * **Uppgift 3**. Kör **reconstructPar** toomerge hello anger tid kataloger från varje processor_N_ katalog till en enda uppsättning.
     
     * Tilldela en nod toohello aktivitet
     * **Kommandorad** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Arbetskatalogen** -/ openfoam/sloshingTank3D
   * **Uppgift 4**. Kör **foamToEnsight** i parallella tooconvert hello OpenFOAM resultatet filerna till EnSight formatera och placera hello EnSight filer i en katalog med namnet Ensight i hello case directory.
     
     * Tilldela två noder toohello uppgift
     * **Kommandorad** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Arbetskatalogen** -/ openfoam/sloshingTank3D
7. Lägga till beroenden toothese aktiviteter i stigande ordning.
   
   ![Aktivitetsberoenden][task_dependencies]
8. Klicka på **skicka** toorun jobbet.
   
   Som standard skickar HPC Pack hello jobb som din aktuella inloggade användarkontot. När du klickar på **skicka**, du kan se en dialogruta visas där du tooenter hello användarnamn och lösenord.
   
   ![Jobbet autentiseringsuppgifter][creds]
   
   Under vissa förhållanden HPC Pack kommer ihåg hello användarinformation du inkommande innan och inte visa den här dialogrutan. toomake HPC Pack visa igen, ange hello följande kommando i Kommandotolken och sedan skicka hello-jobbet.
   
   ```
   hpccred delcreds
   ```
9. hello jobbet tar från flera minuter tooseveral timmar enligt toohello parametrar som du har angett för hello exemplet. Hello termisk karta visas i hello jobb som körs på hello Linux noder. 
   
   ![Termisk karta][heat_map]
   
   På varje nod startas åtta processer.
   
   ![Linux-processer][linux_processes]
10. Hitta hello jobbet resultat i mappar under C:\OpenFoam\sloshingTank3D och hello-loggfilerna på C:\OpenFoam när hello jobbet har slutförts.

## <a name="view-results-in-ensight"></a>Visa resultat i EnSight
Du kan också använda [EnSight](https://www.ceisoftware.com/) toovisualize och analysera hello resultatet av hello OpenFOAM jobbet. Finns det mer information om visualisering och animeringen i EnSight [video guiden](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1. När du har installerat EnSight på hello huvudnod startar du den.
2. Öppna C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Du kan se en behållare i hello viewer.
   
   ![Tanken i EnSight][tank]
3. Skapa en **Isosurface** från **internalMesh**, och välj sedan hello variabeln **alpha_water**.
   
   ![Skapa en isosurface][isosurface]
4. Ange hello färg för **Isosurface_part** skapade i föregående steg i hello. Till exempel den toowater blå.
   
   ![Redigera isosurface färg][isosurface_color]
5. Skapa en **Iso-volym** från **väggar** genom att välja **väggar** i hello **delar** panelen och klicka på hello **Isosurfaces**  knapp i toolbar hello.
6. Hello i dialogrutan Välj **typen** som **Isovolume** och ange hello Min för **Isovolume intervallet** too0.5. toocreate Hej isovolume, klickar du på **skapa med valda delar**.
7. Ange hello färg för **Iso_volume_part** skapade i föregående steg i hello. Till exempel den toodeep vattenstämplar blå.
8. Ange hello färg för **väggar**. Till exempel den tootransparent vit.
9. Klicka på **spela upp** toosee hello resultaten av hello simuleringen.
   
    ![Tanken resultat][tank_result]

## <a name="sample-files"></a>Exempelfiler
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Exempel XML-konfigurationsfilen för kluster-distributionen med PowerShell-skript
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Exempelfilen cred.xml
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-tooinstall-mpi"></a>Exempel på silent.cfg filen tooinstall MPI
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Exempelskript för settings.sh
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Exempelskript för hpcimpirun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
