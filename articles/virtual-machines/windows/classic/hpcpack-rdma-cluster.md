---
title: aaaSet in en Windows RDMA klustret toorun MPI program | Microsoft Docs
description: "Lär dig hur toocreate Windows HPC Pack kluster med storlek H16r H16mr, A8 eller A9 VMs toouse hello Azure RDMA nätverk toorun MPI appar."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 23bc8740dbd05a7c7ab3f998489a41d0df4520a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-toorun-mpi-applications"></a>Konfigurera ett RDMA för Windows-kluster med HPC Pack toorun MPI program
Konfigurera ett RDMA för Windows-kluster i Azure med [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) och [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun parallella Message Passing Interface (MPI) program. När du ställer in RDMA-kompatibla, Windows Server-baserade noder i ett kluster med HPC Pack kommunikation MPI program effektiv via en låg latens, hög genomströmning nätverk i Azure som är baserad på remote direct memory access (RDMA)-teknik.

Om du vill toorun MPI arbetsbelastningar på virtuella Linux-datorer åtkomst hello Azure RDMA nätverket finns [ställa in en Linux RDMA klustret toorun MPI program](../../linux/classic/rdma-cluster.md).

## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack distributionsalternativ för kluster
Microsoft HPC Pack är ett verktyg på utan extra kostnad toocreate HPC-kluster lokalt eller i Azure toorun Windows eller Linux-HPC-program. HPC Pack innehåller en körningsmiljö för hello Microsofts implementering av hello-meddelande skickas gränssnitt för Windows (MS-MPI). När det används med RDMA-kompatibla instanser med en Windows Server operativsystem, tillhandahåller HPC Pack en effektiv alternativet toorun Windows MPI åtkomst hello Azure RDMA nätverket. 

Den här artikeln introducerar två scenarier och länkar toodetailed vägledning tooset in ett RDMA-Windows-kluster med Microsoft HPC Pack. 

* Scenario 1. Distribuera beräkningsintensiva worker rollinstanser (PaaS)
* Scenario 2. Distribuera beräkningsnoder i beräkningsintensiva virtuella datorer (IaaS)

Allmänna krav toouse beräkningsintensiva instanser med Windows, se [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenario 1: Distribuera beräkningsintensiva worker rollinstanser (PaaS)
Lägga till extra beräkningsresurser i Azure worker rollinstanser (Azure noder) körs i en molntjänst (PaaS) från ett befintligt paket för HPC-kluster. Den här funktionen kallas även ”burst tooAzure” HPC Pack stöder flera olika storlekar för hello arbetarinstanser roll. När du lägger till hello Azure-noder, ange ett av RDMA-kompatibla hello storlekar.

Nedan visas information och anvisningar för tooburst tooRDMA-kompatibla Azure-instanser från en befintlig (vanligtvis lokalt) klustret. Använd liknande procedurer tooadd worker-rollen instanser tooan HPC Pack huvudnod som distribueras i en Azure VM.

> [!NOTE]
> En självstudiekurs tooburst tooAzure med HPC Pack finns [ställer in en hybrid-kluster med HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Observera hello överväganden i hello följande som gäller specifikt tooRDMA-kompatibla Azure-noder.
> 
> 

![Burst tooAzure][burst]

### <a name="steps"></a>Steg
1. **Distribuera och konfigurera en huvudnod i HPC Pack 2012 R2**
   
    Hämta installationspaketet för hello senaste HPC Pack från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Krav och instruktioner tooprepare ett Azure burst-distribution, se [Burst tooAzure Worker-instanser med Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).
2. **Konfigurera ett certifikat i hello Azure-prenumeration**
   
    Konfigurera certifikat toosecure hello anslutning mellan hello huvudnod och Azure. Alternativ och procedurer finns [scenarier tooConfigure hello Azure-Hanteringscertifikat för HPC Pack](http://technet.microsoft.com/library/gg481759.aspx). HPC Pack installeras testdistributioner kan en standard Microsoft HPC Azure-Hanteringscertifikatet kan du snabbt överföra tooyour Azure-prenumeration.
3. **Skapa en ny molntjänst och ett lagringskonto**
   
    Använd hello Azure portal toocreate en tjänst i molnet och ett lagringskonto för hello distribution i en region där hello RDMA-kompatibla instanserna är tillgängliga.
4. **Skapa en mall för Azure nod**
   
    Använd hello skapa nod Mallguiden i HPC Cluster Manager. Anvisningar finns [skapar en mall för Azure noden](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) i ”steg tooDeploy Azure noder med Microsoft HPC Pack”.
   
    Första testerna föreslår vi konfigurerar en princip för manuell tillgänglighet i hello mallen.
5. **Lägga till noder toohello kluster**
   
    Använd hello guiden för Lägg till nod i HPC Cluster Manager. Mer information finns i [Lägg till Azure-noder toohello Windows HPC-kluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    När du anger hello storleken på hello noder, Välj en av hello RDMA-kompatibla instans storlekar.
   
   > [!NOTE]
   > HPC Pack distribuerar automatiskt minst två RDMA-kompatibla instanser (till exempel A8) som proxy-noder i varje burst tooAzure distribution med hello beräkningsintensiva instanser, dessutom toohello Azure rollen arbetarinstanser som du anger. hello proxy noder använder kärnor som tilldelas toohello prenumeration och avgifter tillsammans med rollinstanser för hello Azure worker.
   > 
   > 
6. **Starta (tillhandahålla) hello noder och anpassa dem online toorun jobb**
   
    Välj hello noder och använda hello **starta** åtgärd i HPC Cluster Manager. När etableringen är klar, Välj hello noder och använda hello **Anslut** åtgärd i HPC Cluster Manager. hello-noderna är klar toorun jobb.
7. **Skicka jobb toohello kluster**
   
   Använd HPC Pack jobbet skicka verktyg toorun kluster-jobb. Se [Microsoft HPC Pack: jobbhantering](http://technet.microsoft.com/library/jj899585.aspx).
8. **Stoppa (avetablering) hello noder**
   
   När du är klar jobb som körs, hello noder offline och använda hello **stoppa** åtgärd i HPC Cluster Manager.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenario 2: Distribuera datornoder i beräkningsintensiva virtuella datorer (IaaS)
I det här scenariot distribuera hello HPC Pack huvudnod och beräkning klusternoder på virtuella datorer i Azure-nätverk. HPC Pack innehåller flera [distributionsalternativ i Azure Virtual Machines](../../linux/hpcpack-cluster-options.md), inklusive skript för automatisk distribution och Azure quickstart mallar. Exempelvis hello följande överväganden och steg vägleder dig toouse hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) att automatisera hello distributionen av ett HPC Pack 2012 R2-kluster i Azure.

![Klustret i virtuella Azure-datorer][iaas]

### <a name="steps"></a>Steg
1. **Skapa en klustrets huvudnod och compute-nod virtuella datorer genom att köra hello HPC Pack IaaS-skriptet på en klientdator**
   
    Hämta hello HPC Pack IaaS-distributionsskriptet paketet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922).
   
    tooprepare hello klientdatorn, skapa hello-konfigurationsfilen och kör hello skript, se [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md). 
   
    toodeploy RDMA-kompatibla compute-noder, Observera hello följande ytterligare överväganden:
   
   * **Virtuellt nätverk**: Ange ett nytt virtuellt nätverk i en region i vilken hello RDMA-kompatibla instansstorleken du toouse är tillgänglig.
   * **Operativsystemet Windows Server**: toosupport RDMA-anslutning, ange ett Windows Server 2012 R2 eller Windows Server 2012-operativsystem för hello beräkningsnod virtuella datorer.
   * **Molntjänster**: Vi rekommenderar att distribuera din huvudnod i en molntjänst och dina compute-noder i en annan molntjänst.
   * **Gå nodstorlek**: det här scenariot bör du en storlek på minst A4 (Extra stor) för hello huvudnod.
   * **HpcVmDrivers tillägget**: hello distributionsskriptet installerar hello Azure VM-agenten och hello HpcVmDrivers tillägg automatiskt när du distribuerar storlek A8 eller A9 compute-noder med en Windows Server-operativsystem. HpcVmDrivers installerar drivrutiner på hello beräkningsnod virtuella datorer så att de kan ansluta toohello RDMA nätverk. På RDMA-kompatibla H-serien virtuella datorer, måste du manuellt installera hello HpcVmDrivers tillägg. Se [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Klusternätverkskonfigurationen**: hello distributionsskriptet konfigurerar automatiskt hello HPC Pack klustret i topologin 5 (alla noder på hello företagsnätverk). Den här topologin krävs för alla HPC Pack klustret distributioner i virtuella datorer. Ändra inte hello klustret nätverkets topologi senare.
2. **Ta hello datornoderna online toorun jobb**
   
    Välj hello noder och använda hello **Anslut** åtgärd i HPC Cluster Manager. hello-noderna är klar toorun jobb.
3. **Skicka jobb toohello kluster**
   
    Anslut toohello huvudnod toosubmit jobb eller konfigurera en lokal dator toodo detta. Mer information finns i [skicka jobb tooan HPC-kluster i Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Vidta hello noder offline och stoppa frigöra () dem.**
   
    När du är klar jobb som körs, och ta offline hello-noder i HPC Cluster Manager. Använd sedan Azure management tools tooshut dem ned.

## <a name="run-mpi-applications-on-hello-cluster"></a>Kör MPI-program på hello kluster
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Exempel: Kör mpipingpong i ett HPC Pack-kluster
tooverify ett HPC Pack distribution av RDMA-kompatibla hello instanser, kör hello HPC Pack **mpipingpong** på hello klustret. **mpipingpong** skickar datapaket mellan parad noder upprepade gånger toocalculate svarstid och genomströmning mått och statistik för hello RDMA-aktiverade program nätverket. Det här exemplet illustrerar ett typiskt mönster för att köra ett MPI-jobb (i det här fallet **mpipingpong**) genom att använda hello kluster **mpiexec** kommando.

Det här exemplet förutsätter att du har lagt till Azure-noder i en konfiguration för ”burst tooAzure” ([Scenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Om du har distribuerat HPC Pack på ett kluster med virtuella Azure-datorer, du behöver toomodify hello kommandot syntax toospecify en annan nodgrupp och ange ytterligare variabler toodirect nätverk trafik toohello RDMA nätverk.

toorun mpipingpong på hello klustret:

1. Öppna en kommandotolk på hello huvudnod eller på en korrekt konfigurerad klientdator.
2. tooestimate fördröjning mellan två noder i en Azure burst-distribution med fyra noder typen hello efter kommandot toosubmit ett jobb toorun mpipingpong med en liten paketstorlek och många iterationer:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    hello kommandot returnerar hello-ID för hello jobb som har skickats.
   
    Om du har distribuerat hello HPC Pack klustret distribueras på Azure Virtual Machines, ange en nodgrupp som innehåller compute-nod virtuella datorer som distribueras i en enda molntjänst och ändra hello **mpiexec** kommandot på följande sätt:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. När hello jobbet är slutfört, utdata tooview hello (i det här fallet hello utdata för uppgift 1 för hello jobb), typen hello efter
   
    ```Command
    task view <JobID>.1
    ```
   
    där &lt; *JobID* &gt; är hello-ID för hello jobb som har skickats.
   
    hello utdata innehåller latens resultat liknande toohello följande.
   
    ![Svarstid för ping-pong][pingpong1]
4. tooestimate dataflödet mellan två Azure burst noder, typen hello följande kommando toosubmit ett jobb toorun **mpipingpong** med ett stort paket, storlek och några iterationer:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    hello kommandot returnerar hello-ID för hello jobb som har skickats.
   
    Ändra hello kommandot som du antecknade i steg 2 i ett HPC Pack-kluster som distribueras på virtuella Azure-datorer.
5. När hello jobbet är slutfört, utdata tooview hello (i det här fallet hello utdata för uppgift 1 för hello jobb), typen hello följande:
   
    ```Command
    task view <JobID>.1
    ```
   
   hello utdata innehåller genomströmning resultat liknande toohello följande.
   
   ![Ping pong genomflöde][pingpong2]

### <a name="mpi-application-considerations"></a>Överväganden för MPI-program
Följande är överväganden för MPI program som körs med HPC Pack i Azure. Vissa gäller endast toodeployments av Azure-noder (rollen arbetarinstanser som ska läggas till i en konfiguration för ”burst tooAzure”).

* Worker-rollinstanser i en molntjänst är regelbundet etableras utan föregående meddelande av Azure (till exempel för Systemunderhåll eller om en instans misslyckas). Om en instans är etableras medan den körs ett MPI-jobb, hello instans förlorar data och returnerar toohello tillstånd när det först distribuerades, vilket kan orsaka hello MPI jobbet toofail. hello fler noder som du använder för en enstaka MPI-jobb och hello längre hello körs jobbet, hello mer troligt att en hello instanser etableras medan ett jobb körs. Överväg också att detta om du anger en enskild nod i hello distribution som en filserver.
* toorun MPI-jobb i Azure, du har inte toouse hello RDMA-kompatibla instanser. Du kan använda valfri instans storlek som stöds av HPC Pack. Hej RDMA-kompatibla instanser bör dock för att köra relativt stora MPI-jobb som är känsliga toohello svarstid och hello bandbredden för hello-nätverk som ansluter hello noder. Om du använder andra storlekar toorun och bandbredd-känslig för fördröjningar MPI-jobb, rekommenderar vi kör små jobb, där en enskild aktivitet körs på bara några noder.
* Program som har distribuerats tooAzure instanserna är ämne toohello kopplad till programmet hello licensvillkoren. Kontrollera med hello leverantören av alla kommersiella program för licensiering eller andra begränsningar för att köra i hello molnet. Alla leverantörer erbjuder inte licensiering enligt modellen Betala per användning.
* Azure-instanser behöver ytterligare konfigurera tooaccess lokala noder, resurser och servrar för fjärrskrivbordslicenser. Du kan konfigurera en Azure-nätverk för plats-till-plats till exempel tooenable hello Azure-noder tooaccess ett lokalt licensservern.
* toorun MPI program på Azure-instanser, registrera varje MPI-program med Windows-brandväggen på hello instanser genom att köra hello **hpcfwutil** kommando. Detta gör att MPI kommunikation tootake plats på en port som tilldelas dynamiskt av hello brandvägg.
  
  > [!NOTE]
  > För burst tooAzure distributioner, kan du också konfigurera en brandvägg undantag kommandot toorun automatiskt på alla nya Azure noder som har lagts till tooyour klustret. När du har kört hello **hpcfwutil** och kontrollera att dina program fungerar lägger till kommandot hello tooa startskript för din Azure-noder. Mer information finns i [använder ett startskript för Azure-noder](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack använder hello CCP_MPI_NETMASK klustret miljö variabeln toospecify godkända ett adressintervall för MPI-kommunikation. Från och med HPC Pack 2012 R2, hello CCP_MPI_NETMASK klustret miljövariabeln påverkar endast MPI kommunikationen mellan noder domänanslutna beräkning (antingen lokalt eller i virtuella Azure-datorer). hello variabeln ignoreras av noderna läggas till i en burst tooAzure konfiguration.
* MPI-jobb kan inte köras i Azure-instanser som har distribuerats i annat moln services (till exempel burst tooAzure distributioner med olika noden mallar eller Azure VM datornoderna distribueras i flera molntjänster). Om du har flera Azure noden distributioner som startas med olika noden mallar måste hello MPI-jobb köras på en enda uppsättning Azure-noder.
* När du lägger till Azure-noder tooyour klustret och tar dem online, hello HPC tjänsten för Rapportjobbschemaläggning omedelbart försöker toostart jobb på hello noder. Om bara en del av din arbetsbelastning kan köra på Azure, kontrollera att du uppdaterar eller skapar jobbet mallar toodefine vilka typer som kan köras på Azure. Till exempel krävs tooensure att jobb som skickats med en jobbmall bara köras på Azure-noder, Lägg till hello noden grupper egenskapen toohello jobbmall och välj AzureNodes som hello värde. toocreate anpassade grupper för din Azure-noder använder hello Lägg till HpcGroup HPC PowerShell-cmdlet.

## <a name="next-steps"></a>Nästa steg
* Utveckla med hello Azure Batch-tjänsten toorun MPI program på hanterade pooler med beräkningsnoder i Azure som en alternativ toousing HPC Pack. Se [Använd flera instanser av åtgärder toorun Message Passing Interface (MPI) program i Azure Batch](../../../batch/batch-mpi.md).
* Om du vill toorun Linux MPI-program som har åtkomst till hello Azure RDMA nätverket finns [ställa in en Linux RDMA klustret toorun MPI program](../../linux/classic/rdma-cluster.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
