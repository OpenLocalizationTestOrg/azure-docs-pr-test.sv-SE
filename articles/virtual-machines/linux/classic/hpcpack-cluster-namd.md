---
title: "aaaNAMD with Microsoft HPC Pack på virtuella Linux-datorer | Microsoft Docs"
description: "Distribuera ett Microsoft HPC Pack kluster på Azure och köra en NAMD simulering med charmrun på flera Linux compute-noder"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Kör NAMD med Microsoft HPC Pack på beräkningsnoder för Linux i Azure
Den här artikeln visar enkelriktade toorun en Linux högpresterande datorbearbetning (HPC) arbetsbelastningen på virtuella Azure-datorer. Här kan du ställa in en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) på Azure-kluster med beräkningsnoder för Linux och köra en [NAMD](http://www.ks.uiuc.edu/Research/namd/) simuleringen toocalculate och visualisera hello struktur för ett stort biomolecular system.  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (för Nanoscale molekylära Dynamics program) är en parallell molekylära dynamics paket designat för högpresterande simulering av stora biomolecular system som innehåller in toomillions av atomer. Exempel på dessa system är virus, cell strukturer och stora protein. NAMD skalas toohundreds kärnor för vanliga simulering och toomore än 500 000 kärnor för hello största simulering.
* **Microsoft HPC Pack** ger funktioner toorun storskaliga HPC och parallella program i kluster av lokala datorer eller virtuella Azure-datorer. Ursprungligen utvecklades som en lösning för Windows HPC-arbetsbelastningar, HPC Pack nu stöder som kör Linux HPC-program på Linux compute-nod virtuella datorer som distribueras i ett HPC Pack-kluster. Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) en introduktion.

För andra alternativ toorun Linux HPC arbetsbelastningar i Azure, se [tekniska resurser för batch- och högpresterande datorbearbetning](../../../batch/batch-hpc-solutions.md).

## <a name="prerequisites"></a>Krav
* **HPC Pack kluster med Linux datornoder** -distribuera ett kluster med HPC Pack med Linux compute-noder på Azure med hjälp av antingen en [Azure Resource Manager-mall](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) eller en [Azure PowerShell-skriptet](hpcpack-cluster-powershell-script.md). Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) hello krav och anvisningar för något av alternativen. Om du väljer hello distributionsalternativ för PowerShell-skript finns i hello exempelkonfigurationsfilen i hello exempelfiler hello slutet av den här artikeln. Den här filen konfigurerar ett Azure-baserade HPC Pack kluster som består av en Windows Server 2012 R2 huvudnod och fyra storlek stora CentOS 6.6 compute-noder. Anpassa den här filen behövs för din miljö.
* **NAMD programvara och kursen filer** -hämta NAMD programvara för Linux från hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) plats (registrering krävs). Den här artikeln är baserad på NAMD version 2.10 och använder hello [Linux-x86_64 (64-bitars Intel/AMD med Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) Arkiv. Dessutom hämta hello [NAMD självstudiekursen filer](http://www.ks.uiuc.edu/Training/Tutorials/#namd). hello nedladdningar är .tar-filer, och du behöver ett Windows-verktyget tooextract hello-filer på hello klustrets huvudnod. tooextract hello filer, följ instruktionerna för hello senare i den här artikeln. 
* **VMD** (valfritt) – toosee hello resultatet från jobbet NAMD, hämta och installera hello molekylära visualiseringen programmet [VMD](http://www.ks.uiuc.edu/Research/vmd/) på en dator som du väljer. hello aktuella versionen är 1.9.2. Se hello VMD hämta platsen tooget igång.  

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Ställ in ömsesidigt förtroende mellan beräkningsnoder
Kör ett jobb mellan noder på flera Linux-noder kräver hello noder tootrust varandra (av **rsh** eller **ssh**). När du skapar hello HPC Pack kluster med hello Microsoft HPC Pack IaaS distributionsskriptet konfigurerar hello skript automatiskt permanenta ömsesidigt förtroende för hello administratörskontot som du anger. Vanliga användare som du skapar i hello klustrets domän, har du tooset tillfälliga ömsesidigt förtroende mellan hello noder när ett jobb har allokerats toothem. Förstör hello relationen när hello jobbet har slutförts. toodo detta för varje användare måste ange ett RSA-nyckelpar toohello kluster vilka HPC Pack använder tooestablish hello förtroenderelation. Följ instruktionerna.

### <a name="generate-an-rsa-key-pair"></a>Generera en RSA-nyckelpar
Det är enkelt toogenerate ett RSA-nyckelpar, som innehåller en offentlig nyckel och en privat nyckel, genom att köra hello Linux **ssh-keygen** kommando.

1. Logga in tooa Linux-dator.
2. Kör följande kommando hello:
   
   ```bash
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
1. [Ansluta med Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello huvudnod VM med hello autentiseringsuppgifter för domänen som du angav när du har distribuerat hello kluster (till exempel hpc\clusteradmin). Du kan hantera hello kluster från hello huvudnod.
2. Använd standard Windows Server procedurer toocreate ett domänanvändarkonto i hello klustrets Active Directory-domän. Till exempel använda hello Active Directory-användare och datorer verktyget i hello huvudnod. hello exemplen i den här artikeln förutsätter att du skapar en domänanvändare med namnet hpcuser i hello hpclab domän (hpclab\hpcuser).
3. Lägga till hello domän användaren toohello HPC Pack klustret som en användare i klustret. Instruktioner finns i [Lägg till eller ta bort klustret användare](https://technet.microsoft.com/library/ff919330.aspx).
4. Skapa en fil med namnet C:\cred.xml och kopiera hello RSA viktiga data till den. Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. Öppna en kommandotolk och ange följande kommando tooset hello autentiseringsuppgifter data för hello hpclab\hpcuser konto hello. Du använder hello **extendeddata** parametern toopass hello namnet hello C:\cred.xml fil du skapade hello viktiga data.
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Det här kommandot har slutförts utan utdata. Lagra hello cred.xml filen på en säker plats när du har angett hello autentiseringsuppgifter för hello användarkonton som du behöver toorun jobb, eller ta bort den.
6. Om du har genererat hello RSA-nyckelpar på en Linux-noder kan du komma ihåg toodelete hello nycklar när du är klar med dem.. HPC Pack inte ställts in ömsesidigt förtroende om den hittar en befintlig id_rsa- eller id_rsa.pub-fil.

> [!IMPORTANT]
> Vi rekommenderar inte kör ett Linux-jobb som en Klusteradministratör i ett delat kluster, eftersom ett jobb som skickats av en administratör köras under rotkontot hello på hello Linux noder. Ett jobb som skickats av en icke-administratörer körs under ett lokalt användarkonto för Linux med hello samma namn som hello jobbet användare. I det här fallet konfigurerar HPC Pack ömsesidigt förtroende för den här Linux användaren alla hello noder allokeras toohello jobb. Du kan ställa in hello Linux användare manuellt på hello Linux noder innan du kör jobbet hello eller HPC Pack skapar hello användaren automatiskt när hello jobbet har skickats. Om HPC Pack skapar hello användare, HPC Pack tar bort den efter hello jobbet har slutförts. tooreduce säkerhetshot, hello nycklarna tas bort när hello jobbet har slutförts på hello noder.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Skapa en filresurs för Linux-noder
Nu konfigurera en SMB-filresurs och montera hello delad mapp på alla Linux noder tooallow hello Linux noder tooaccess NAMD filer med en sökväg. Nedan följer stegen toomount en delad mapp på hello huvudnod. En resurs som rekommenderas för distributioner, till exempel CentOS 6.6 som för närvarande inte stöder hello Azure File service. Om Linux-noder stöder en Azure-filresurs, se [hur toouse Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md). För ytterligare alternativ för delning med HPC Pack-fil, se [komma igång med Linux compute-noder i ett HPC Pack-kluster i Azure](hpcpack-cluster.md).

1. Skapa en mapp på hello huvudnod och dela den tooEveryone genom att ange behörighet för läsning och skrivning. I det här exemplet \\ \\CentOS66HN\Namd är hello namnet på hello mapp där CentOS66HN är hello värdnamnet för hello huvudnod.
2. Skapa en undermapp som heter namd2 i hello delad mapp. Skapa en annan undermapp som heter namdsample i namd2.
3. Extrahera hello NAMD filer i mappen hello med hjälp av en Windows-versionen av **tar** eller något annat verktyg i Windows som körs på .tar Arkiv. 
   
   * Extrahera hello NAMD tar Arkiv för\\\\CentOS66HN\Namd\namd2.
   * Extrahera hello självstudiekursen filer under \\ \\CentOS66HN\Namd\namd2\namdsample.
4. Öppna Windows PowerShell-fönstret och kör följande kommandon toomount hello delad mapp på hello Linux noder hello.
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

hello första kommandot skapar en mapp med namnet /namd2 på alla noder i hello LinuxNodes grupp. hello andra kommandot monterar hello delade mappen //CentOS66HN/Namd/namd2 till hello mapp med dir_mode och file_mode bits set too777. Hej *användarnamn* och *lösenord* hello kommandot ska vara hello autentiseringsuppgifterna för en användare i hello huvudnod.

> [!NOTE]
> Hej ”\`” symbol i andra hello-kommandot är en symbolen för PowerShell. ”\`,” innebär hello ””, (kommatecken tecken) är en del av hello-kommando.
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a>Skapa ett Bash-skript toorun ett NAMD-jobb
Jobbet NAMD måste en *nodelist* för **charmrun** toodetermine hello antalet noder toouse när du startar NAMD processer. Du använder ett Bash-skript som genererar hello nodlistfilen och kör **charmrun** med den här nodlistfilen. Du kan sedan skicka ett NAMD jobb i HPC Cluster Manager som anropar det här skriptet.

Med hjälp av en textredigerare önskat, skapa ett Bash-skript i hello /namd2 mapp hello NAMD programfiler och ger den namnet hpccharmrun.sh. Kopiera hello hpccharmrun.sh exempelskript tillhandahålls hello slutet av den här artikeln för en snabb konceptbevis och gå för[skickar ett jobb för NAMD](#submit-a-namd-job).

> [!TIP]
> Spara skriptet som en textfil med Linux radbrytningar (endast LF, inte CR LF). Detta säkerställer att den körs korrekt på hello Linux noder.
> 
> 

Nedan visas information om den här bash-skript har. 

1. Definiera några variabler.
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. Hämta information om noden från hello miljövariabler. $NODESCORES lagras en lista över delade ord från $CCP_NODES_CORES. $COUNT är hello storleken på $NODESCORES.
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   hello format för hello $CCP_NODES_CORES variabeln är följande:
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   Den här variabeln visar hello totala antalet noder, nodnamn och antalet kärnor på varje nod som är allokerade toohello jobb. Till exempel om hello jobbet måste 10 kärnor toorun, liknar hello värdet för $CCP_NODES_CORES:
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. Starta om hello $CCP_NODES_CORES variabeln inte anges **charmrun** direkt. (Detta bör endast inträffa när du kör det här skriptet direkt på Linux-noder.)
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. Eller skapa en nodlistfilen för **charmrun**.
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. Kör **charmrun** med hello nodlistfilen hämta dess returstatus och ta bort hello nodlistfilen hello slutet.
   
   ${CCP_NUMCPUS} är en annan miljövariabel som angetts av hello HPC Pack huvudnod. Lagrar den hello antal totala kärnor allokerade toothis jobb. Vi använder den toospecify hello antal processer för charmrun.
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. Avsluta med hello **charmrun** returstatus.
   
   ```
   exit ${RTNSTS}
   ```

Följande är hello nodlistfilen vilka hello skriptet genererar hello information:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Exempel:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Skicka ett NAMD-jobb
Nu är du redo toosubmit NAMD-jobb i HPC Cluster Manager.

1. Anslut tooyour klustrets huvudnod och starta HPC Cluster Manager.
2. I **resurshantering**, se till att hello Linux datornoderna hello **Online** tillstånd. Om du inte markerar du dem och klickar på **Anslut**.
3. I **jobbhantering**, klickar du på **nytt jobb**.
4. Ange ett namn för jobbet som *hpccharmrun*.
   
   ![Nytt HPC-jobb][namd_job]
5. På hello **jobbinformation** sidan under **jobbet resurser**, Välj hello typ av resurs som **nod** och ange hello **minsta** too3. , vi kör hello jobb på tre noder för Linux och varje nod har fyra kärnor.
   
   ![Jobbet resurser][job_resources]
6. Klicka på **redigera uppgifter** i hello vänstra navigeringsfönstret och klicka sedan på **Lägg till** tooadd en toohello aktiviteten.    
7. På hello **information om aktiviteter och i/o-omdirigering** sidan Ange hello följande värden:
   
   * **Kommandoraden**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`
     
     > [!TIP]
     > hello är föregående kommandoraden ett enda kommando utan radbrytningar. Den radbryts tooappear på flera rader under **kommandoraden**.
     > 
     > 
   * **Arbetskatalogen** -/namd2
   * **Minsta** – 3
     
     ![Uppgiftsinformation][task_details]
     
     > [!NOTE]
     > Du har angett hello arbetskatalogen här eftersom **charmrun** försöker toonavigate toohello samma arbetskatalogen på varje nod. Om hello arbetskatalogen har inte angetts, startas HPC Pack hello-kommando i en mapp som skapats på ett hello Linux noder slumpmässigt namn. Detta medför följande fel på hello hello andra noder: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid problemet, ange en mappsökväg som kan nås av alla noder som hello arbetskatalogen.
     > 
     > 
8. Klicka på **OK** och klicka sedan på **skicka** toorun jobbet.
   
   Som standard skickar HPC Pack hello jobb som din aktuella inloggade användarkontot. En dialogruta blir du ombedd tooenter hello användarnamn och lösenord när du klickar på **skicka**.
   
   ![Jobbet autentiseringsuppgifter][creds]
   
   Under vissa förhållanden HPC Pack kommer ihåg hello användarinformation du inkommande innan och inte visa den här dialogrutan. toomake HPC Pack visa igen, ange hello följande kommando i Kommandotolken och sedan skicka hello-jobbet.
   
   ```command
   hpccred delcreds
   ```
9. hello jobbet tar flera minuter toofinish.
10. Hitta hello jobblogg på \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log och hello utdatafiler i \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.
11. Du kan också starta VMD tooview jobbresultaten. hello steg för att visualisera hello NAMD utdatafilerna (i det här fallet en ubiquitin protein molekylen vattenstämplar gäller) ligger utanför hello omfånget för den här artikeln. Se [NAMD kursen](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) mer information.
    
    ![Jobbet resultat][vmd_view]

## <a name="sample-files"></a>Exempelfiler
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Exempel XML-konfigurationsfilen för kluster-distributionen med PowerShell-skript
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Exempelskript för hpccharmrun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
