---
title: "aaaFail tillbaka den virtuella VMware-datorer från Azure hello klassiska portal | Microsoft Docs"
description: "Läs mer om misslyckas tillbaka toohello den lokala platsen efter redundans för virtuella VMware-datorer och fysiska servrar tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 80bc3ab2708a5df953c6532b353da19a4c44ac34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site-classic-portal"></a>Växla tillbaka virtuella VMware-datorer och fysiska servrar toohello lokal plats (klassiska portal)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klassisk Azure-portal](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klassiska Azure-portalen (bakåtkompatibelt)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Den här artikeln beskriver hur toofail tillbaka virtuella Azure-datorer från Azure toohello lokal plats. Följ hello instruktionerna i den här artikeln när du är klar toofail tillbaka din virtuella VMware-datorer eller fysiska Windows-/ Linux-servrar när de har redundansväxlats från hello lokal plats tooAzure med den här [kursen](site-recovery-vmware-to-azure-classic.md).

## <a name="overview"></a>Översikt
Det här diagrammet visar hello återställning av arkitekturen för det här scenariot.

Använd den här arkitekturen när hello processervern är på plats och du använder en ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Använd den här arkitekturen om hello processen servern på Azure och du har en VPN-anslutning eller en ExpressRoute-anslutning.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

toosee hello fullständig lista över portar och hello återställning architechture diagram finns toohello bilden nedan

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Här är hur återställningen fungerar:

* När du har redundansväxlats tooAzure växlar du tillbaka tooyour lokal plats i några steg:
  * **Fas 1**: skydda igen hello virtuella Azure-datorer så att de startar replikeras tillbaka tooVMware virtuella datorer som körs på den lokala platsen. Aktivera återaktivera skydd flyttar hello VM till en skyddsgrupp för återställning efter fel som har skapats automatiskt när hello redundans skyddsgruppen skapades. Om du har lagt till redundans grupp tooa recovery skyddsplanen lades toohello plan också automatiskt i skyddsgruppen för hello återställning efter fel.  Under skyddar ange du hur tooplan toofail tillbaka.
  * **Fas 2**: när Azure-VMs replikerar tooyour lokal plats, kör en misslyckas över toofail tillbaka från Azure.
  * **Steg 3**: efter dina data har misslyckats tillbaka, skyddar hello lokala virtuella datorer som du inte tillbaka till, så att de startar replikerar tooAzure.


  [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failback/player]

### <a name="failback-toohello-original-or-alternate-location"></a>Toohello ursprungliga eller en annan plats för återställning efter fel
Om du redundansväxlade en VMware VM kan du växla tillbaka toohello samma datakälla VM om det fortfarande finns lokalt. I det här scenariot kommer endast deltaändringar hello flyttas tillbaka. Tänk på följande:

* Om du redundansväxlade fysiska servrar och sedan återställning är alltid tooa nya VMware VM.
  * Observera att innan tillbaka en fysisk dator
  * Fysisk dator som skyddas kommer tillbaka som en virtuell dator när redundansväxling tillbaka från Azure tooVMware
  * Se till att du upptäcker att minst en Huvudmålet Server tillsammans med hello nödvändiga ESX/ESXi-värdar toowhich måste toofailback.
* Om du växlar tillbaka krävs toohello ursprungliga VM hello följande:

  * Om hello VM hanteras av en vCenter-server ska hello Huvudmålets ESX-värden ha åtkomst toohello VMs datalagret.
  * Om hello VM finns på en ESX-värd men inte hanteras av vCenter sedan hello hårddisk hello VM måste vara i ett tillgängligt datalager hello Huvudmålserverns värden.
  * Om den virtuella datorn finns på en ESX-värd och inte använder vCenter bör du genomföra identifieringen av hello ESX-värd för hello Huvudmålservern innan du skyddar. Detta gäller att om du växlar tillbaka fysiska servrar för.
  * Ett annat alternativ (om hello lokalt VM finns) är toodelete den innan du gör en återställning efter fel. Sedan skapar återställning sedan en ny virtuell dator på hello samma värden som hello huvudmålservern ESX-värd.
* När du återställning tooan alternativ plats hello data kommer att återställda toohello samma datalager och hello samma ESX-värden som används av hello lokala huvudmålservern.

## <a name="prerequisites"></a>Krav
* Du behöver en VMware-miljön i ordning toofail tillbaka virtuella VMware-datorer och fysiska servrar. Misslyckas tillbaka tooa stöds inte fysisk server.
* I ordning toofail bör tillbaka har du skapat ett Azure-nätverk när du först ställa in skydd. Återställning efter fel behöver en VPN eller ExpressRoute-anslutning från hello Azure nätverk där hello Azure virtuella datorer är placerade toohello lokal plats.
* Om hello virtuella datorer du vill toofail tillbaka tooare hanteras av en vCenter-server behöver du toomake att du har behörighet för hello som krävs för identifiering av virtuella datorer på vCenter-servrar. [Läs mer](site-recovery-vmware-to-azure-classic.md).
* Om det finns ögonblicksbilder på en virtuell dator misslyckas återaktivera skydd. Du kan ta bort hello ögonblicksbilder eller hello diskar.
* Innan du växlar tillbaka behöver toocreate ett antal komponenter:
  * **Skapa en processerver i Azure**. Det här är en Azure VM som du behöver toocreate och fortsätta att köras under återställning efter fel. Du kan ta bort hello datorn när återställning har slutförts.
  * **Skapa en huvudmålserver**: hello huvudmålservern skickar och tar emot data för återställning efter fel. hello hanteringsservern som du skapade lokala har en huvudmålserver som installeras som standard. Beroende på hello mängden misslyckade tillbaka trafik kan du dock behöva toocreate en separat huvudmålserver för återställning efter fel.
  * Om du vill toocreate en ytterligare huvudmålservern körs på Linux, behöver tooset upp hello Linux VM innan du installerar hello huvudmålservern, som beskrivs nedan.
* Konfigurationsservern är lokala när du gör en återställning efter fel. Under återställning efter fel, måste hello virtuella datorn finns i hello Configuration server-databas, misslyckas vilka återställning inte genomföras. Se därför till att du vidtar regelbunden schemalagd säkerhetskopiering av servern. Om för katastrofåterställning, behöver du toorestore med hello samma IP-adress så att återställningen fungerar.

## <a name="set-up-hello-process-server-in-azure"></a>Ställ in hello processerver i Azure
Du behöver tooinstall en processerver i Azure så att hello Azure virtuella datorer kan skicka hello data tillbaka tooon lokala huvudmålservern.

1. I hello Site Recovery-portalen > **Konfigurationsservrar** Välj tooadd en ny processerver.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Ange namn på en server och ange ett namn och lösenord som du använder tooconnect toohello virtuella Azure-datorn som administratör. I **konfigurationsservern** Välj hello lokala management-servern och ange hello Azure nätverk i vilka hello processervern ska distribueras. Detta bör vara hello nätverk där hello Azure Virtual Machines finns. Ange en unik IP-adress från hello väljer undernät och påbörjar distributionen.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

   En processerver för jobbet toodeploy hello ska utlösas

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

   Efter hello distribueras processerver i Azure som du kan logga in på den med hjälp av hello autentiseringsuppgifter du angav. hello körs första gången du loggar in i dialogrutan för hello processen server. Ange hello IP-adress för hello lokala management-servern och dess lösenfras. Lämna hello standardinställningen port 443. Du kan också lämna hello 9443 standardporten för datareplikering såvida inte den här inställningen ändras särskilt när du ställer in hello lokala management-servern.

   > [!NOTE]
   > visas under hello server inte **VM egenskaper**. Det är bara synliga under hello **servrar** fliken i hello management server toowhich har registrerats. Det kan ta om 10 – 15 minuter för hello processen server tooappear.
   >
   >

## <a name="set-up-hello-master-target-server-on-premises"></a>Ställ in hello huvudmålservern server lokalt
Hej huvudmålservern tar emot hello återställning av data. En huvudmålserver installeras automatiskt på hello lokala management-servern, men om du växlar tillbaka stora mängder data måste du kanske tooset upp en ytterligare huvudmålservern. Gör på följande sätt:

> [!NOTE]
> Om du vill tooinstall en huvudmålserver på Linux, följer du anvisningarna för hello i hello nästa procedur.
>
>

1. Om du installerar hello huvudmålservern i Windows, öppna hello snabbstartsidan från hello VM som du installerar hello huvudmålservern och hämta hello installationsfilen för hello Unified installationsprogram för Azure Site Recovery-guiden.
2. Kör installationsprogrammet och i **innan du börjar** Välj **lägga till ytterligare processer servrar tooscale ut distribution**.
3. Fullständig hello guiden i hello samma sätt som du gjorde när du [konfigurera hello server](site-recovery-vmware-to-azure-classic.md). På hello **Server konfigurationsinformation** anger hello IP-adressen för den här huvudmålservern och en lösenfras tooaccess hello VM.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Konfigurera en Linux-VM som hello huvudmålservern
tooset in hello management-server som kör hello huvudmålservern som Linux VM som du behöver tooinstall hello Cent) S 6.6 minimalt operativsystem, hämta hello SCSI-ID: N för varje SCSI-hårddisk, installera vissa ytterligare paket och tillämpa vissa egna ändringar.

#### <a name="install-centos-66"></a>Installera CentOS 6.6
1. Installera hello CentOS 6.6 minimalt operativsystem på hanteringsservern för hello VM. Behåll hello ISO i ett DVD-enheten och starta hello system. Hoppa över hello media testning, Välj amerikansk engelska på hello språk, Välj **grundläggande lagringsenheter**, kontrollera att hello hårddisken inte är viktiga data och klicka på **Ja**, ta bort alla data. Ange hello värdnamnet för hello hanteringsserver och välj hello nätverkskort.  I hello **redigera System** dialogrutan Välj ** Anslut automatiskt ** och lägga till en statisk IP-adress, nätverk och DNS-inställningarna. Ange en tidszon och ett lösenord tooaccess hello rothanteringsservern.
2. När du uppmanas att hello typ av installation som väljer **Skapa anpassad Layout** som hello partition. När du klickar på **nästa** Välj **lediga** och klicka på Skapa. Skapa  **/** , **/var/krascher** och **/home partitioner** med **FS typ:** **ext4**. Skapa hello växlingen partion som **FS typ: växlingen**.
3. Om befintliga enheter är en varning visas. Klicka på **Format** tooformat hello enhet med hello partitionsinställningar. Klicka på **skrivåtgärder ändra toodisk** tooapply hello partition ändringar.
4. Välj **installera startprogram** > **nästa** tooinstall hello startprogram på hello rot-partitionen.
5. När installationen är klar klickar du på **omstart**.

#### <a name="retrieve-hello-scsi-ids"></a>Hämta hello SCSI-ID: N
1. Hämta hello SCSI-ID: N för varje SCSI-hårddisk i hello VM efter installationen. toodo detta Stäng hello hanteringsservern VM högerklickar du i hello VM egenskaper i VMware hello VM post > **redigera inställningar för** > **alternativ**.
2. Välj **Avancerat** > **allmän artikel** och på **konfigurationsparametrar**. Det här alternativet kommer att de-active när hello datorn körs. toomake it active hello datorn måste stängas av.
3. Om hello rad **disk. EnableUUID** finns kontrollera hello värdet för**SANT** (skiftlägeskänsligt). Om det redan finns kan du avbryta och testa hello SCSI-kommando i ett gästoperativsystem när den har startats.
4. Om hello raden inte befintliga Klicka **Lägg till rad** – och lägga till den med hello **SANT** värde. Använd inte dubbla citattecken.

#### <a name="install-additional-packages"></a>Installera ytterligare paket
Du behöver toodownload och vissa ytterligare paket installeras.

1. Kontrollera att hello huvudmålservern är anslutna toohello internet.
2. Kör det här kommandot toodownload och installera 15 paket från hello CentOS databasen: **# yum installera – y xfsprogs perl lsscsi rsync wget kexec-tools**.
3. Om hello källdatorer som du skyddar kör Linux med Reiser XFS filsystem för hello roten eller starta enheten, och sedan hämta och installera ytterligare paket på följande sätt:

   * # <a name="cd-usrlocal"></a>CD /usr/local
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>rpm – ivh kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm reiserfs-verktyg för webbplatsuppgradering-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Använd anpassade ändringar
Gör följande tooapply anpassade ändringar när du är klar hello efter installationen steg och installerade hello paket hello:

1. Kopiera hello RHEL 6-64 Unified Agent binära toohello VM. Kör det här kommandot toountar hello binära: **tar – zxvf<file name>**
2. Kör det här kommandot toogive behörigheter: **# chmod 755./ApplyCustomChanges.sh**
3. Kör skript hello: **#./ApplyCustomChanges.sh**. Du bör bara köra hello skriptet en gång. Starta om servern för hello när hello skriptet har körts.

## <a name="run-hello-failback"></a>Kör hello återställning efter fel
### <a name="reprotect-hello-azure-vms"></a>Skyddar virtuella hello Azure-datorer
1. I hello Site Recovery-portalen > **datorer** väljer du fliken hello virtuell dator som har växlas över och klicka på **skydda igen**.
2. I **Huvudmålserver** och **Processervern** Välj hello lokala huvudmålservern och hello Azure VM processervern.
3. Välj hello-konto som du konfigurerade för att ansluta toohello VM.
4. Välj hello återställning efter fel version av hello skyddsgruppen. Om exempelvis hello VM är skyddad i SG1 måste tooselect PG1_Failback.
5. Om du vill toorecover tooan alternativ plats, Välj hello kvarhållningsenhetens och datalagret som konfigurerats för hello huvudmålservern. När du växlar tillbaka toohello lokal plats hello virtuella VMware-datorer i hello återställning skyddsplan använder hello samma datalagret som hello huvudmålservern. Om du vill toorecover hello replik Azure VM toohello samma lokala VM sedan hello lokala VM bör redan vara på hello samma datalagret som hello master mål server. Om det inte finns någon VM lokal skapas en ny under återaktivera skydd.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. När du klickar på **OK** toobegin återaktivera skydd ett jobb börjar tooreplicate hello virtuell dator från Azure toohello lokal plats. Du kan följa förloppet för hello på hello **jobb** fliken.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-toohello-on-premises-site"></a>Kör en växling vid fel toohello lokal plats
Efter att återaktivera skydd hello VM är flyttade toohello återställning efter fel version av skyddsgruppen och läggs till automatiskt toohello återställningsplan som du använde för hello redundans tooAzure om det finns en.

1. I hello **Återställningsplaner** sidan Välj hello återställningsplan som innehåller hello återställning gruppen och klicka på **redundans** > **oplanerad växling**.
2. I **bekräfta redundans** Kontrollera hello redundansriktning (tooAzure) och välj hello återställningspunkt som du vill använda toouse för hello växling vid fel (senaste). Om du har aktiverat **Multi-VM** när du konfigurerade replikeringsegenskaper kan du återställa toohello senaste app eller kraschkonsekvent återställningspunkt. Klicka på hello markerat toostart hello växling vid fel.
3. Under växling vid fel stängs hello virtuella datorer i Azure Site Recovery. När du har kontrollerat att återställningen har slutförts som förväntat du kan du kan kontrollera att hello Azure virtuella datorer har stängts av som förväntat.

### <a name="reprotect-hello--on-premises-site"></a>Skapa nytt hello lokal plats
När återställningen är klar dina data kommer att vara tillbaka hello lokal plats, men skyddas inte. toostart replikering tooAzure igen hello följande:

1. I hello Site Recovery-portalen > **datorer** väljer hello virtuella datorer som har misslyckats tillbaka och klicka på fliken **skydda igen**.
2. När du har kontrollerat att replikering tooAzure fungerar som förväntat i Azure du kan ta bort hello Azure virtuella datorer (för närvarande inte körs) som har misslyckats igen.

### <a name="common-issues-in-failback"></a>Vanliga problem i återställning efter fel
1. Om du utför skrivskyddade användaridentifieringen vCenter och skydda virtuella datorer det lyckas och redundans fungerar. När hello skydda igen misslyckas det eftersom hello datastores inte kan identifieras. Som ett symtom visas inte hello datastores visas när skydda igen. tooresolve, kan du uppdatera autentiseringsuppgifterna för hello vCenter med rätt konto som har behörighet och försök hello jobb. [Läs mer](site-recovery-vmware-to-azure-classic.md)
2. När återställning en Linux VM och köra den lokalt, visas hello Nätverkshanteraren paketet avinstalleras från hello-datorn. Det beror på att när hello VM återställs i Azure hello Nätverkshanteraren paketet tas bort.
3. När en virtuell dator har konfigurerats med statiska IP-adress och växlas över tooAzure, förvärvas hello IP-adress via DHCP. När du redundansväxlar tillbaka tooOn lokal fortsätter hello VM toouse DHCP tooacquire hello IP-adress. Du behöver toomanually inloggningen till hello dator och ange hello IP-adress tillbaka tooStatic adressen om det behövs.
4. Om du använder ESXi 5.5 gratisversion eller vSphere 6 Hypervisor gratisversion redundans lyckades, men lyckas inte återställning efter fel. Du kommer ned tooupgrade tooeither Utvärderingslicens tooenable återställning efter fel.

## <a name="failing-back-with-expressroute"></a>Tillbaka med ExpressRoute
Du kan växla tillbaka via en VPN-anslutning eller Azure ExpressRoute. Om du vill toouse ExpressRoute Obs hello följande:

* ExpressRoute ska ställas in på hello virtuella Azure-nätverket toowhich källa datorer misslyckas över och i som virtuella Azure-datorer finns efter hello redundansväxlingen.
* Data som är replikerade tooan Azure storage-konto på en offentlig slutpunkt. Du bör konfigurera offentlig peering i ExpressRoute med hello mål-Datacenter för Site Recovery replikering toouse ExpressRoute.
