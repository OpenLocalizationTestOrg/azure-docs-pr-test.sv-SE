---
title: "aaaUse beräkningsintensiva virtuella Azure-datorer med Batch | Microsoft Docs"
description: Hur tootake nytta av RDMA-kompatibla eller GPU-aktiverade VM storlekar i Azure Batch-pooler
services: batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: danlep
ms.openlocfilehash: 6a462a5f2a44ddcec8bf4e5c200d444cac8fafe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Använda RDMA-kompatibla eller GPU-aktiverade instanser i Batch-pooler

toorun vissa batchjobb, kanske du vill tootake utnyttja Azure VM-storlekar som utformats för storskaliga beräkning. Till exempel toorun flerinstans [arbetsbelastningar MPI](batch-mpi.md), kan du välja A8 A9, eller H-serien storlekar som har ett nätverk gränssnitt för Remote Direct Memory Access (RDMA). Dessa storlekar ansluta tooan InfiniBand-nätverk för kommunikationen mellan noder som kan påskynda MPI-program. Eller CUDA program du N-serien storlekar som innehåller NVIDIA Tesla grafikprocessorer (GPU) unit-kort.

Den här artikeln innehåller anvisningar och exempel toouse vissa av särskilda Azures i Batch-pooler. Specifikationer och bakgrunden finns:

* Högpresterande compute VM-storlekar ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* GPU-aktiverade VM-storlekar ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Prenumerationen och gränser

* **Kvoter** -en eller flera Azure-kvoter kan begränsa antalet hello eller typen av noder kan du lägga till tooa Batch-pool. Du är mer troligt toobe begränsad när du väljer RDMA-kompatibel GPU-aktiverad, eller andra flera kärnor VM-storlekar. Beroende på hello typ av Batch-kontot som du har skapat, kan du använda toohello konto sig själv eller tooyour prenumeration hello kvoter.

    * Om du har skapat Batch-kontot i hello **Batch-tjänsten** konfiguration, är du begränsad hello [dedikerade kärnor kvot per Batch-kontot](batch-quota-limit.md#resource-quotas). Som standard är den här kvoten 20 kärnor. En separat kvot gäller för[VM med låg prioritet](batch-low-pri-vms.md), om du använder dem. 

    * Om du har skapat hello konto i hello **användarens prenumeration** konfiguration, din prenumeration begränsar hello antal VM kärnor per region. Se [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md). Prenumerationen gäller även en regionala kvoten toocertain VM-storlekar, inklusive HPC och GPU-instanser. Inga ytterligare kvoter tillämpas i konfiguration för hello prenumeration toohello Batch-kontot. 

  Du kan behöva tooincrease kvoter för en eller flera när du använder en särskild VM-storlek i batchen. toorequest ökad kvot, öppna ett [online kundsupport](../azure-supportability/how-to-create-azure-support-request.md) utan kostnad.

* **Regional tillgänglighet** - beräkningsintensiva virtuella datorer kanske inte är tillgänglig i hello regioner där du skapar Batch-konton. toocheck att en storlek är tillgänglig finns [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Beroenden

Hej RDMA och GPU-funktionerna i beräkningsintensiva storlekar stöds endast i vissa operativsystem. Beroende på operativsystemet, kanske du behöver tooinstall eller konfigurera ytterligare drivrutin eller annan programvara. hello följande tabellerna sammanfattas dessa beroenden. Se länkade artiklar för ytterligare information. Alternativ tooconfigure Batch pooler finns senare i den här artikeln.


### <a name="linux-pools---virtual-machine-configuration"></a>Pooler för Linux - konfiguration av virtuell dator

| Storlek | Funktion | Operativsystem | Programvara som krävs | Inställningar för programpool |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r H16mr A8 A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | SUSE Linux Enterprise Server 12 HPC eller<br/>CentOS-baserade HPC<br/>(Azure Marketplace) | Intel MPI 5 | Aktivera kommunikationen mellan noder, inaktivera samtidiga uppgiftskörningen |
| [NC-serien *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms) | NVIDIA Tesla K80 GPU | Ubuntu 16.04 LTS.<br/>Red Hat Enterprise Linux 7.3, eller<br/>CentOS-baserad 7.3<br/>(Azure Marketplace) | NVIDIA CUDA Toolkit 8.0 drivrutiner | Saknas | 
| [NV serien](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS<br/>Red Hat Enterprise Linux 7.3<br/>CentOS-baserad 7.3<br/>(Azure Marketplace) | NVIDIA RUTNÄTET 4.3 drivrutiner | Saknas |

* RDMA-anslutningar på NC24r virtuella datorer stöds på CentOS-baserade 7.3 HPC med Intel MPI.



### <a name="windows-pools---virtual-machine-configuration"></a>Pooler för Windows - konfiguration av virtuell dator

| Storlek | Funktion | Operativsystem | Programvara som krävs | Inställningar för programpool |
| -------- | ------ | -------- | -------- | ----- |
| [H16r H16mr A8 A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 eller<br/>Windows Server 2012 (Azure Marketplace) | Microsoft MPI 2012 R2 eller senare, eller<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-tillägget | Aktivera kommunikationen mellan noder, inaktivera samtidiga uppgiftskörningen |
| [NC-serien *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla K80 GPU | Windows Server 2016 eller <br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA Tesla drivrutiner eller CUDA Toolkit 8.0 drivrutiner| Saknas | 
| [NV serien](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 eller<br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA RUTNÄTET 4.3 drivrutiner | Saknas |

* RDMA-anslutningar på NC24r virtuella datorer stöds i Windows Server 2012 R2 med HpcVMDrivers tillägg och Microsoft MPI eller Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Windows - pooler i Cloud services-konfiguration

> [!NOTE]
> N-serien storlekar stöds inte i Batch-pooler med hello cloud services-konfiguration.
>

| Storlek | Funktion | Operativsystem | Programvara som krävs | Inställningar för programpool |
| -------- | ------- | -------- | -------- | ----- |
| [H16r H16mr A8 A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2<br/>Windows Server 2012, eller<br/>Windows Server 2008 R2 (gäst-OS-familjen) | Microsoft MPI 2012 R2 eller senare, eller<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure VM-tillägget | Aktivera kommunikationen mellan noder,<br/> inaktivera samtidiga uppgiftskörningen |





## <a name="pool-configuration-options"></a>Konfigurationsalternativ för poolen

tooconfigure en specialiserad VM-storlek för Batch-pool, hello Batch-API: er och verktyg tillhandahåller flera alternativ tooinstall krävs programvara eller drivrutiner, inklusive:

* [Aktiviteten starta](batch-api-basics.md#start-task) -ladda upp ett installationspaket som en resurs filen tooan Azure storage-konto i hello samma region som hello Batch-kontot. Skapa resursfilen starta uppgiften kommandoraden tooinstall hello tyst när hello pool startas. Mer information finns i hello [REST API-dokumentation](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > hello startuppgift måste köras med behörighet för utökade (admin) och den måste vänta på att lyckas.
  >

* [Programpaketet](batch-application-packages.md) – Lägg till en installation av komprimerade paketet tooyour Batch-kontot och konfigurera en paket-referens i hello pool. Den här inställningen laddar upp och därefter hello paketet på alla noder i hello pool. Om hello-paket är ett installationsprogram, skapar du en starta uppgiften kommandoraden toosilently installera hello app på alla noder i poolen. Du kan också installera hello-paket när en aktivitet är schemalagd toorun på en nod.

* [Anpassade poolen avbildningen](batch-api-basics.md#pool) – skapa en anpassad Windows eller Linux VM-avbildning som innehåller drivrutiner, programvara eller andra inställningar som krävs för hello VM-storlek. Om du har skapat Batch-kontot i hello Användarkonfiguration prenumerationen ange hello anpassad avbildning för Batch-pool. (Anpassade avbildningar stöds inte i konton i hello Batch-tjänstkonfigurationen.) Anpassade avbildningar kan endast användas med pooler i hello konfiguration av virtuell dator.

  > [!IMPORTANT]
  > Du kan inte använder en anpassad avbildning som skapats med hanterade diskar eller med Premium-lagring i Batch-pooler.
  >



* [Batch-skeppsvarv](https://github.com/Azure/batch-shipyard) konfigurerar automatiskt hello GPU och RDMA toowork transparent med av arbetsbelastningar i Azure Batch. Batch skeppsvarv drivs helt med konfigurationsfiler. Det finns många exempel recept tillgängliga konfigurationer som möjliggör GPU och RDMA arbetsbelastningar som till exempel hello [CNTK GPU recept](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) som förkonfigurerar GPU drivrutiner på N-serien virtuella datorer och läser in kognitiva Toolkit för Microsoft-programvara som en Docker-bild.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Exempel: Microsoft MPI i en A8 VM-adresspool

toorun Windows MPI program på en pool med Azure A8 noder, behöver du tooinstall en stöds MPI-implementering. Här följer exempel steg tooinstall [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) på en Windows-poolen med hjälp av ett Batch-programpaket.

1. Hämta hello [installationspaketet](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) för hello senaste versionen av Microsoft MPI.
2. Skapa en zip-fil för hello-paketet.
3. Överför hello paketet tooyour Batch-kontot. Anvisningar finns i hello [programpaket](batch-application-packages.md) vägledning. Ange ett program-id som *MSMPI*, och en version som *8.1*. 
4. Skapa en pool i hello cloud services-konfiguration med hello önskat antal noder och skala med hello Batch-API: er eller Azure-portalen. hello följande tabell visas exempel inställningarna tooset in MPI i obevakat läge med Startuppgiften:

| Inställning | Värde |
| ---- | ----- | 
| **Bildtyp** | Molntjänster |
| **OS-familjen** | Windows Server 2012 R2 (OS-familjen 4) |
| **Nodstorlek** | A8 Standard |
| **Dessa kommunikation aktiverad** | True |
| **Maximalt antal uppgifter per nod** | 1 |
| **Programmet paketet refererar till** | MSMPI |
| **Starta aktivitet aktiverad** | True<br>**Kommandorad** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Användaridentitet** -poolen autouser, admin<br/>**Vänta tills lyckade** – SANT

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Exempel: NVIDIA Tesla drivrutiner på NC-VM-pool

toorun CUDA program på en pool med Linux NC-noder, behöver du tooinstall CUDA Toolkit 8.0 på hello noder. hello Toolkit installerar drivrutiner som behövs hello NVIDIA Tesla GPU. Här följer exempel steg toodeploy en anpassad avbildning Ubuntu 16.04 LTS med hello GPU drivrutiner:

1. Distribuera ett Azure NC6 virtuell dator som kör Ubuntu 16.04 LTS. Till exempel skapa hello VM i hello oss södra centrala region. Se till att du skapar hello VM med standardlagring, och *utan* hanterade diskar.
2. Följ hello steg tooconnect toohello VM och [CUDA drivrutinsinstallation](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms).
3. Ta bort etableringen hello Linux-agenten och sedan avbilda Linux VM-avbildning med hjälp av hello Azure CLI 1.0-kommandon. Anvisningar finns [avbilda en Linux-dator som körs på Azure](../virtual-machines/linux/capture-image-nodejs.md). Anteckna hello avbildningen URI.
  > [!IMPORTANT]
  > Använd inte Azure CLI 2.0 kommandon toocapture hello avbildning för Azure Batch. Hello CLI 2.0 kommandon avbilda för närvarande endast virtuella datorer som skapades med hjälp av hanterade diskar.
  >
4. Skapa ett Batch-konto med hello prenumeration Användarkonfiguration i en region som stöder NC virtuella datorer.
5. Använder hello Batch-API: er eller Azure portal, skapa en pool med hello anpassad avbildning och med hello önskat antal noder och skala. hello visas följande tabell exempel processpool-inställningar för hello avbildningen:

| Inställning | Värde |
| ---- | ---- |
| **Bildtyp** | Anpassad avbildning |
| **Anpassad avbildning** | Bild-URI för hello formuläret`https://yourstorageaccountdisks.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd` |
| **Noden agent SKU** | batch.node.Ubuntu 16.04 |
| **Nodstorlek** | NC6 Standard |



## <a name="next-steps"></a>Nästa steg

* toorun MPI-jobb på Azure Batch-pool finns hello [Windows](batch-mpi.md) eller [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) exempel.

* Exempel på GPU arbetsbelastningar på Batch finns hello [Batch skeppsvarv](https://github.com/Azure/batch-shipyard/) recept.
