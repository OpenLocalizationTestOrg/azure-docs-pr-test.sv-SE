---
title: aaaSQL Server FCI - Azure Virtual Machines | Microsoft Docs
description: "Den här artikeln förklarar hur toocreate redundanskluster för SQL Server-instans på Azure Virtual Machines."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Konfigurera SQL Server-instans för redundanskluster på virtuella Azure-datorer

Den här artikeln förklarar hur toocreate en SQL Server redundanskluster instansen (FCI) på virtuella Azure-datorer i Resource Manager-modellen. Den här lösningen använder [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) som en programvarubaserad virtuellt SAN-nätverk som synkroniserar hello lagring (datadiskar) mellan hello noder (Azure virtuella datorer) i en Windows-kluster. S2D är ny i Windows Server 2016.

hello följande diagram visar hello komplett lösning på virtuella Azure-datorer:

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

föregående diagram visar hello:

- Två virtuella Azure-datorer i ett redundanskluster i Windows. När en virtuell dator är i ett redundanskluster som också kallas en *klusternod*, eller *noder*.
- Varje virtuell dator har två eller flera datadiskar.
- S2D synkroniserar hello data på hello data och visar hello synkroniseras lagring som en lagringspool.
- hello lagringspoolen visas ett kluster delad volym (CSV) toohello failover-kluster.
- hello SQL Server FCI-klusterrollen använder hello CSV för hello dataenheter.
- Ett Azure belastningsutjämnare toohold hello IP-adress för hello FCI för SQL Server.
- Tillgänglighetsuppsättningen Azure innehåller alla hello-resurser.

   >[!NOTE]
   >Alla Azure-resurser finns i hello diagram hello tillhör samma resursgrupp.

Mer information om S2D finns [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D stöder två typer av arkitekturer - konvergerade och hyperkonvergerad. hello-arkitekturen i det här dokumentet är hyperkonvergerad. Hyperkonvergerad infrastruktur platser hello lagring på hello samma servrar värden hello klustrade programmet. I den här arkitekturen är hello lagring på varje nod i SQL Server FCI.

### <a name="example-azure-template"></a>Exemplet Azure-mall

Du kan skapa hello hela lösningen från en mall i Azure. Ett exempel på en mall finns i hello GitHub [Azure Quickstart mallar](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). Det här exemplet är inte utformad eller testas för en viss arbetsbelastning. Du kan köra hello mallen toocreate en SQL Server-FCI med S2D lagring anslutna tooyour domän. Du kan utvärdera hello mall och ändra den efter dina egna behov.

## <a name="before-you-begin"></a>Innan du börjar

Det finns några saker du behöver tooknow och några saker som du behöver på plats innan du fortsätter.

### <a name="what-tooknow"></a>Vilka tooknow
Du bör ha en operativ tolkning av hello följande tekniker:

- [Tekniker för Windows-kluster](http://technet.microsoft.com/library/hh831579.aspx)
-  [Instanser av SQL Server-redundanskluster](http://msdn.microsoft.com/library/ms189134.aspx).

Du bör också ha en förståelse av hello följande tekniker:

- [Hyper-Konvergerad lösning med lagringsutrymmen i Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure-resursgrupper](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a>Vilka toohave

Innan du följer hello anvisningarna i den här artikeln bör bör du redan ha:

- En Microsoft Azure-prenumeration.
- En Windows-domän på virtuella Azure-datorer.
- Ett konto med behörighet toocreate objekt i hello virtuella Azure-datorn.
- Ett virtuellt Azure-nätverk och undernät med tillräckligt utrymme för IP-adresser för hello följande komponenter:
   - Både virtuella datorer.
   - hello redundans klustrets IP-adress.
   - En IP-adress för varje FCI.
- DNS har konfigurerats på hello Azure Network pekar toohello domänkontrollanter.

Med dessa krav är på plats, kan du fortsätta med att skapa klustret för växling vid fel. hello första steget är toocreate hello virtuella datorer.

## <a name="step-1-create-virtual-machines"></a>Steg 1: Skapa virtuella datorer

1. Logga in toohello [Azure-portalen](http://portal.azure.com) med din prenumeration.

1. [Skapa en Azure tillgänglighetsuppsättning](../tutorial-availability-sets.md).

   hello tillgänglighet in grupper virtuella datorer över feldomäner och uppdatera domäner. Hej tillgänglighetsuppsättning säkerställer att programmet inte påverkas av enskilda felpunkter som hello nätverks-switch eller hello power enhet av en samling servrar.

   Om du inte har skapat hello resursgruppen för dina virtuella datorer kan du göra det när du skapar en Azure tillgänglighetsuppsättning. Om du använder hello Azure portal toocreate hello tillgänglighetsuppsättning hello gör du följande steg:

   - I hello Azure-portalen klickar du på  **+**  tooopen hello Azure Marketplace. Sök efter **tillgänglighetsuppsättning**.
   - Klicka på **tillgänglighetsuppsättning**.
   - Klicka på **Skapa**.
   - På hello **skapa tillgänglighetsuppsättning** bladet set hello följande värden:
      - **Namnet**: ett namn för hello tillgänglighetsuppsättning.
      - **Prenumerationen**: din Azure-prenumeration.
      - **Resursgruppen**: Om du vill toouse en befintlig grupp **använda befintliga** och välj hello hello nedrullningsbara listan. Annars väljer **Skapa nytt** och Skriv ett namn för hello-gruppen.
      - **Plats**: Ange hello plats där du planerar att toocreate dina virtuella datorer.
      - **Fault domäner**: Använd hello standard (3).
      - **Uppdatera domäner**: Använd hello standard (5).
   - Klicka på **skapa** toocreate hello tillgänglighetsuppsättning.

1. Skapa hello virtuella datorer i hello tillgänglighetsuppsättning.

   Etablera två virtuella SQL Server-datorer i hello Azure tillgänglighetsuppsättning. Instruktioner finns i [etablera en virtuell dator med SQL Server i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md).

   Placera både virtuella datorer:

   - I hello är samma Azure-resursgrupp som ditt tillgänglighetsuppsättning i.
   - På hello samma nätverk som en domänkontrollant.
   - I ett undernät med tillräckligt utrymme för IP-adresser för både virtuella datorer och alla FCIs som du kan använda förr eller senare i det här klustret.
   - I hello Azure tillgänglighetsuppsättning.   

      >[!IMPORTANT]
      >Du kan inte ange eller ändra tillgänglighetsuppsättning när en virtuell dator har skapats.

   Välj en bild från hello Azure Marketplace. Du kan använda en Marketplace-avbildning med som omfattar Windows Server och SQL Server eller bara hello Windows Server. Mer information finns i [översikt över SQL Server på Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)

   hello officiella SQL Server-avbildningar i hello Azure-galleriet innehåller installerade SQL Server-instansen plus hello installationsprogrammet för SQL Server och hello nödvändig nyckel.

   Välj hello höger bild enligt toohow som du vill toopay för hello SQL Server-licens:

   - **Betala per användning licensiering**: hello per minut kostnaden för dessa avbildningar innehåller hello SQL Server-licensiering:
      - **SQL Server 2016 Enterprise på Windows Server Datacenter 2016**
      - **SQL Server 2016 Standard på Windows Server Datacenter 2016**
      - **SQL Server 2016 utvecklare i Windows Server Datacenter 2016**

   - **Bring-your-äger-licens (BYOL)**

      - **{BYOL} SQL Server 2016 Enterprise på Windows Server Datacenter 2016**
      - **{BYOL} SQL Server 2016 Standard på Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >När du skapar hello virtuell dator tar du bort hello förinstallerade fristående SQL Server-instansen. Du använder hello förinstallerade SQL Server media toocreate hello SQL Server FCI när du har konfigurerat hello failover-kluster och S2D.

   Alternativt kan du använda Azure Marketplace-bilder med bara hello-operativsystem. Välj en **Windows Server 2016 Datacenter** avbildningen och installera hello SQL Server FCI när du har konfigurerat hello failover-kluster och S2D. Den här avbildningen innehåller inte SQL Server-installationsmedia. Placera hello installationsmediet på en plats där du kan köra hello SQL Server-installation för varje server.

1. När Azure skapar virtuella datorer kan ansluta tooeach virtuell dator med RDP.

   När du först ansluta tooa virtuell dator med RDP frågan hello datorn om du vill tooallow toobe för den här datorn kan identifieras på hello nätverk. Klicka på **Ja**.

1. Om du använder en hello SQL Server-baserade virtuella avbildningar kan du ta bort hello SQL Server-instansen.

   - I **program och funktioner**, högerklicka på **Microsoft SQL Server 2016 (64-bitars)** och på **Avinstallera/ändra**.
   - Klicka på **ta bort**.
   - Välj hello standardinstansen.
   - Ta bort alla funktioner under **Databasmotortjänster**. Ta inte bort **Delade funktioner i**. Se hello följande bild:

      ![Ta bort funktioner](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Klicka på **nästa**, och klicka sedan på **ta bort**.

1. <a name="ports"></a>Öppna portar i brandväggen hello.

   Öppna hello följande portar på hello Windows-brandväggen på varje virtuell dator.

   | Syfte | TCP-Port | Anteckningar
   | ------ | ------ | ------
   | SQL Server | 1433 | Normal port för standardinstanser av SQL Server. Om du har använt en avbildning från galleriet hello öppnas automatiskt den här porten.
   | Hälsoavsökningen | 59999 | Alla öppna TCP-port. I ett senare steg, konfigurera hello belastningsutjämnare [hälsoavsökningen](#probe) och hello klustret toouse denna port.  

1. Lägg till lagring toohello virtuella datorn. Detaljerad information finns i [lägga till lagringsenheter](../../../storage/common/storage-premium-storage.md).

   Både virtuella datorer måste ha minst två hårddiskar.

   Koppla rådata diskar - inte NTFS-formaterade diskar.
      >[!NOTE]
      >Du kan bara aktivera S2D med ingen kontroll av behörighet om du kopplar NTFS-formaterade diskar.  

   Koppla minst två Premium-lagring (SSD-diskar) tooeach VM. Vi rekommenderar minst P30 diskar (1 TB).

   Ange värden för cachelagring**skrivskyddad**.

   hello lagringskapacitet som du använder i produktionsmiljöer beror på din arbetsbelastning. hello värdena beskrivs i den här artikeln för demonstration och testning.

1. [Lägg till hello virtuella datorer tooyour befintlig domän](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

När hello virtuella datorer skapas och konfigureras, kan du konfigurera hello failover-kluster.

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a>Steg 2: Konfigurera hello Windows-redundanskluster med S2D

hello nästa steg är tooconfigure hello redundanskluster med S2D. I det här steget ska du göra hello följande stegen:

1. Lägger till funktionen för redundanskluster i Windows
1. Verifiera hello-kluster
1. Skapa hello failover-kluster
1. Skapa hello molnet vittne
1. Lägga till lagringsenheter

### <a name="add-windows-failover-clustering-feature"></a>Lägger till funktionen för redundanskluster i Windows

1. toobegin, ansluta toohello första virtuella datorn med RDP med ett domänkonto som är medlem i lokala administratörer och har behörighet toocreate objekt i Active Directory. Använd det här kontot för hello resten av hello konfiguration.

1. [Lägg till redundanskluster funktionen tooeach virtuell dator](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   tooinstall redundansklusterfunktionen från hello-Gränssnittet hello följande steg på både virtuella datorer.
   - I **Serverhanteraren**, klickar du på **hantera**, och klicka sedan på **Lägg till roller och funktioner**.
   - I **guiden Lägg till roller och funktioner**, klickar du på **nästa** förrän du får för**Välj funktioner**.
   - I **Välj funktioner**, klickar du på **redundanskluster**. Inkludera alla nödvändiga funktioner och hello-hanteringsverktyg. Klicka på **lägga till funktioner**.
   - Klicka på **nästa** och klicka sedan på **Slutför** tooinstall hello funktioner.

   tooinstall hello funktionen för redundanskluster med PowerShell kan köra hello följande skript från en administratör PowerShell-session på en av hello virtuella datorer.

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

För referens, hello nästa steg gör hello anvisningarna under steg3 i [Hyper-Konvergerad lösning med lagringsutrymmen i Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-hello-cluster"></a>Verifiera hello-kluster

Den här guiden refererar tooinstructions under [verifiera kluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Verifiera hello-kluster i hello Användargränssnittet eller med PowerShell.

toovalidate hello kluster med hello-Gränssnittet hello följande från någon av hello virtuella datorer.

1. I **Serverhanteraren**, klickar du på **verktyg**, klicka på **Klusterhanteraren**.
1. I **Klusterhanteraren**, klickar du på **åtgärd**, klicka på **verifiera konfiguration...** .
1. Klicka på **Nästa**.
1. På **Välj servrar eller ett kluster**, hello-typnamn för både virtuella datorer.
1. På **test,**, Välj **kör bara valda test**. Klicka på **Nästa**.
1. På **testa markeringen**, inkluderar alla test med undantag **lagring**. Se hello följande bild:

   ![Verifiera test](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Klicka på **Nästa**.
1. På **bekräftelse**, klickar du på **nästa**.

Hej **guiden Verifiera en konfiguration** körs hello verifieringstester.

toovalidate hello kluster med PowerShell kan köra hello följande skript från en administratör PowerShell-session på en av hello virtuella datorer.

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

När du validerar hello kluster skapa hello failover-kluster.

### <a name="create-hello-failover-cluster"></a>Skapa hello failover-kluster

Den här guiden refererar för[skapa hello redundanskluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

toocreate hello failover-kluster, behöver du:
- hello namnen på hello virtuella datorer som blivit hello klusternoder.
- Ett namn för hello failover-kluster
- En IP-adress för hello failover-kluster. Du kan använda en IP-adress som inte används på hello samma Azure-nätverk och undernät som hello klusternoder.

hello följande PowerShell skapar ett redundanskluster. Uppdatera hello skriptet med hello namnen på hello noder (hello namn på virtuella datorer) och en tillgänglig IP-adress från hello Azure VNET:

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Skapa ett moln vittne

Molnet vittne är en ny typ av kluster kvorumvittne lagras i en Azure Storage Blob. Detta tar bort hello behovet av en separat virtuell dator som värd för en resurs som vittne.

1. [Skapa ett moln vittne för hello redundanskluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Skapa en blob-behållare.

1. Spara hello snabbtangenter och hello behållar-URL.

1. Konfigurera hello failover cluster klustret kvorumvittne. Se [konfigurera hello kvorumvittne i användargränssnittet för hello]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) i hello Användargränssnittet.

### <a name="add-storage"></a>Lägga till lagringsenheter

hello diskarna för S2D måste toobe tomt och utan partitioner eller andra data. Följ tooclean diskar [hello stegen i den här guiden](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Aktivera Store lagringsutrymmen Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   följande PowerShell hello kan lagringsutrymmen direkt.  

   ```PowerShell
   Enable-ClusterS2D
   ```

   I **Klusterhanteraren**, du kan nu se hello lagringspoolen.

1. [Skapa en volym](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   En av hello funktionerna i S2D är att det automatiskt skapas en lagringspool när du aktiverar den. Du är nu redo toocreate en volym. Hej PowerShell-kommandot `New-Volume` automatiserar hello volym skapas, inklusive formatering, lägga till toohello klustret och skapa en klusterdelad volym (CSV). hello följande exempel skapar en 800 GB (Gigabyte) CSV.

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   När det här kommandot har slutförts, är en 800 GB-volym monterad som en klusterresurs. hello volymen är på `C:\ClusterStorage\Volume1\`.

   hello följande diagram visar en delad klustervolym med S2D:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Steg 3: Testa redundans-kluster

I hanteraren för redundanskluster, kontrollera att du kan flytta hello storage resource toohello annan klusternod. Om du kan ansluta toohello redundanskluster med **Klusterhanteraren** och flytta hello lagringsutrymme från en nod toohello andra, är du redo tooconfigure hello FCI.

## <a name="step-4-create-sql-server-fci"></a>Steg 4: Skapa SQLServer FCI

När du har konfigurerat hello failover-kluster och alla komponenter i serverkluster inklusive lagring, kan du skapa hello SQL Server FCI.

1. Ansluta toohello första virtuella datorn med RDP.

1. I **Klusterhanteraren**, se till att alla klustrets kärnresurser är hello första virtuella datorn. Flytta alla resurser toothis virtuella datorn.

1. Leta upp hello-installationsmediet. Om hello virtuell dator använder en hello Azure Marketplace-bilder, hello media finns i `C:\SQLServer_<version number>_Full`. Klicka på **installationsprogrammet**.

1. I hello **SQL Server Installationscenter**, klickar du på **Installation**.

1. Klicka på **nya SQL-servern failover cluster installation**. Följ instruktionerna för hello i hello guiden tooinstall hello FCI för SQL Server.

   Hej FCI datakataloger måste toobe på klustrad lagring. Med S2D är det inte en delad disk utan en monteringspunkt punkt tooa volym på varje server. S2D synkroniserar hello volym mellan båda noderna. hello-volymen visas toohello klustret som en klusterdelad volym. Använd hello CSV monteringspunkt för hello datakataloger.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. När du har slutfört guiden hello installeras en SQL Server FCI på hello första noden.

1. När installationen har installerats hello FCI på hello första noden ansluta toohello andra noden med RDP.

1. Öppna hello **SQL Server Installationscenter**. Klicka på **Installation**.

1. Klicka på **Lägg till nod tooa SQL Server-redundanskluster**. Följ instruktionerna för hello i hello guiden tooinstall SQLServer och Lägg till den här servern toohello FCI.

   >[!NOTE]
   >Om du använde en galleriet Azure Marketplace-avbildning med SQL Server, ingick SQL Server-verktyg i hello avbildningen. Om du inte använde den här avbildningen, installera SQL Server-verktyg för hello separat. Se [Hämta SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>Steg 5: Skapa Azure belastningsutjämnare

På Azure virtual machines använder kluster en belastningen belastningsutjämnaren toohold en IP-adress som behöver toobe på en klusternod åt gången. I den här lösningen innehåller hello belastningsutjämnaren hello IP-adress för hello FCI för SQL Server.

[Skapa och konfigurera en Azure belastningsutjämnare](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a>Skapa hello belastningsutjämnare i hello Azure-portalen

toocreate hello belastningsutjämnare:

1. Gå toohello resursgrupp med hello virtuella datorer i hello Azure-portalen.

1. Klicka på **+ Lägg till**. Sök hello Marketplace för **belastningsutjämnaren**. Klicka på **belastningsutjämnare**.

1. Klicka på **Skapa**.

1. Konfigurera hello belastningsutjämnare med:

   - **Namnet**: ett namn som identifierar hello belastningsutjämnaren.
   - **Typen**: hello belastningsutjämnare kan vara offentligt eller privat. En privat belastningsutjämnare kan nås inifrån hello samma virtuella nätverk. De flesta Azure-program kan använda en privat belastningsutjämnare. Om ditt program måste åtkomst tooSQL Server direkt via hello Internet, kan du använda en offentlig belastningsutjämnare.
   - **Virtuellt nätverk**: hello samma nätverk som hello virtuella datorer.
   - **Undernät**: hello samma undernät som hello virtuella datorer.
   - **Privata IP-adressen**: hello samma IP-adress som du tilldelade toohello SQL Server FCI-klusterresursen nätverk.
   - **prenumerationen**: din Azure-prenumeration.
   - **Resursgruppen**: Använd hello samma resursgrupp som de virtuella datorerna.
   - **Plats**: Använd hello samma Azure-plats som de virtuella datorerna.
   Se hello följande bild:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a>Konfigurera hello belastningsutjämnarens serverdelspool

1. Returnera toohello Azure-resursgrupp med hello virtuella datorer och leta upp hello nya belastningsutjämnaren. Du kan ha toorefresh hello vyn på hello resursgruppen. Klicka på hello belastningsutjämnaren.

1. På hello belastningen belastningsutjämnaren bladet, klickar du på **serverdelspooler**.

1. Klicka på **+ Lägg till** tooadd en serverdelspool.

1. Ange ett namn för hello serverdelspool.

1. Klicka på **lägga till en virtuell dator**.

1. På hello **Välj virtuella datorer** bladet, klickar du på **Välj en tillgänglighetsuppsättning**.

1. Välj hello tillgänglighetsuppsättning du placerade hello SQL Server-datorer i.

1. På hello **Välj virtuella datorer** bladet, klickar du på **Välj hello virtuella datorerna**.

   Azure-portalen ska se ut så hello följande bild:

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. Klicka på **Välj** på hello **Välj virtuella datorer** bladet.

1. Klicka på **OK** två gånger.

### <a name="configure-a-load-balancer-health-probe"></a>Konfigurera en belastningsutjämnaren, hälsoavsökningen

1. På hello belastningen belastningsutjämnaren bladet, klickar du på **hälsoavsökning**.

1. Klicka på **+ Lägg till**.

1. På hello **Lägg till hälsoavsökningen** bladet <a name="probe"> </a>ange hello hälsa avsökningen parametrar:

   - **Namnet**: ett namn för hello hälsoavsökningen.
   - **Protokollet**: TCP.
   - **Port**: Ange tooan tillgängliga TCP-port. Den här porten kräver en öppen brandväggsport. Använd hello [samma port](#ports) du angiven för hello hälsoavsökningen hello brandväggen.
   - **Intervallet**: 5 sekunder.
   - **Tröskelvärde för ohälsosamt värde**: 2 upprepade fel.

1. Klicka på OK.

### <a name="set-load-balancing-rules"></a>Ange regler för belastningsutjämning

1. På hello belastningen belastningsutjämnaren bladet, klickar du på **belastningsutjämningsregler**.

1. Klicka på **+ Lägg till**.

1. Ange hello belastningsutjämning regler Startparametrar:

   - **Namnet**: ett namn för hello regler för belastningsutjämning.
   - **Frontend-IP-adressen**: använda hello IP-adress för hello nätverksresurs för SQL Server FCI-kluster.
   - **Port**: för hello SQL Server FCI TCP-port. hello instans standardporten är 1433.
   - **Serverdelsport**: det här värdet används hello samma port som hello **Port** värdet när du aktiverar **flytande IP (direkt serverreturnering)**.
   - **Serverdelspool**: Använd hello backend namn som du tidigare har konfigurerat.
   - **Hälsoavsökningen**: Använd hello hälsoavsökningen som du tidigare har konfigurerat.
   - **Sessionspersistence**: Ingen.
   - **Inaktivitetstid (minuter)**: 4.
   - **Flytande IP (direkt serverreturnering)**: aktiverat

1. Klicka på **OK**.

## <a name="step-6-configure-cluster-for-probe"></a>Steg 6: Konfigurera kluster för avsökningen

Ange hello klustret avsökningen Portparametern i PowerShell.

tooset Hej klustret avsökningen Portparametern, uppdatera variabler i hello följande skript från din miljö.

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a>Steg 7: Testa redundans för FCI

Testa redundans för hello FCI toovalidate klusterfunktionaliteten. Hej gör du följande steg:

1. Ansluta tooone hello SQL Server FCI klusternoder med RDP.

1. Öppna **Klusterhanteraren**. Klicka på **roller**. Observera vilken nod som äger hello FCI för SQL Server-rollen.

1. Högerklicka på hello FCI för SQL Server-rollen.

1. Klicka på **flytta** och på **bästa möjliga nod**.

**Hanteraren för redundanskluster** visar hello rollen och dess resurser i offlineläge. hello resurser sedan flytta och anslutas på hello andra noden.

### <a name="test-connectivity"></a>Testa anslutning

logga in tooanother virtuell dator i hello-tootest anslutning samma virtuella nätverk. Öppna **SQL Server Management Studio** och ansluta toohello SQL Server FCI namn.

>[!NOTE]
>Om nödvändigt, kan du [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Begränsningar
På Azure virtual machines stöds inte Microsoft Distributed Transaction Coordinator (DTC) på FCIs eftersom hello RPC-porten inte stöds av hello belastningsutjämnaren.

## <a name="see-also"></a>Se även

[Konfigurera S2D med fjärrskrivbord (Azure)](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Hyper-Konvergerad lösning med lagringsutrymmen direkt](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Direkt översikt över lagringsutrymmen i](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[SQL Server-stöd för S2D](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
