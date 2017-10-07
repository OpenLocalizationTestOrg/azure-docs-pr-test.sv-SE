---
title: "aaaFailback virtuella VMware-datorer från Azure tooon lokala | Microsoft Docs"
description: "Läs mer om misslyckas tillbaka toohello den lokala platsen efter redundans för virtuella VMware-datorer och fysiska servrar tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: 258f5a55252083135b2040e5a235fa1ffbf3b9d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site"></a>Växla tillbaka virtuella VMware-datorer och fysiska servrar toohello lokal plats


Den här artikeln beskriver hur toofailback Azure virtuella datorer från Azure toohello lokal plats. Följ instruktionerna för hello här när du är klar toofail tillbaka din virtuella VMware-datorer eller Windows- eller Linux fysiska servrar när du har skyddat dina datorer med detta igen [referens](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Om du använder hello klassiska Azure-portalen, se tooinstructions nämns [här](site-recovery-failback-azure-to-vmware-classic.md) för förbättrad VMware tooAzure arkitektur och [här](site-recovery-failback-azure-to-vmware-classic-legacy.md) för äldre hello-arkitektur

## <a name="overview"></a>Översikt
hello diagram i det här avsnittet visar hello återställning av arkitekturen för det här scenariot.

Använd den här arkitekturen när hello Processervern är på plats och du använder en Azure ExpressRoute-anslutning:

![Arkitekturdiagram för ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

När hello Processervern finns på Azure och du har en VPN-anslutning eller en ExpressRoute-anslutning som använder den här arkitekturen:

![Arkitekturdiagram för VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

En fullständig lista över portar och hello Arkitekturdiagram för återställning efter fel finns i toohello följande bild:

![Återställning av failover lista över alla portar](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

När du har redundansväxlats tooAzure växlar du tillbaka tooyour lokal plats i tre steg:

* **Fas 1**: skydda igen hello virtuella Azure-datorer så att de startar replikeras tillbaka toohello virtuella VMware-datorer som körs på den lokala platsen.
* **Fas 2**: när virtuella datorerna i Azure är replikerade tooyour lokal plats kan du köra en växling vid fel toofail tillbaka från Azure.
* **Steg 3**: efter dina data har misslyckats tillbaka, skyddar hello lokala virtuella datorer som du inte tillbaka till, så att de startar replikerar tooAzure.

### <a name="fail-back-toohello-original-location-or-an-alternate-location"></a>Växla tillbaka toohello ursprungliga platsen eller en alternativ plats
När du redundansväxlar en VMware VM, kan du växla tillbaka toohello samma datakälla VM om det fortfarande finns lokalt. I det här scenariot misslyckades endast hello går tillbaka.

Om du redundansväxlade fysiska servrar återställning är alltid tooa nya VMware VM. Observera att innan tillbaka en fysisk dator:
* En skyddad fysisk dator kommer tillbaka som en virtuell dator när den har redundansväxlats tillbaka från Azure tooVMware. En fysisk dator för Windows Server 2008 R2 SP1, kan inte om den är skyddad och redundansväxlas tooAzure, flyttas tillbaka. En dator med Windows Server 2008 R2 SP1 som startats som en virtuell dator på lokala kommer att kunna toofail igen.
* Se till att du upptäcker att minst en huvudmålservern tillsammans med hello nödvändiga ESX/ESXi-värdar du behöver toofail tillbaka till.

Om du växlar tillbaka toohello ursprungliga VM hello följande krävs:

* Om hello VM hanteras av en vCenter-server, ska hello master målets ESX-värden ha åtkomst toohello VMs datalagret.
* Om hello VM finns på en ESX-värd men inte hanteras av vCenter, hello hårddisk hello VM måste vara i ett datalager som är tillgänglig via hello Huvudmålserverns värden.
* Om den virtuella datorn finns på en ESX-värd och inte använder vCenter, bör du genomföra identifieringen av hello ESX-värd för hello Huvudmålservern innan du skyddar. Detta gäller att om du växlar tillbaka fysiska servrar för.
* Ett annat alternativ (om hello lokalt VM finns) är toodelete den innan du gör en återställning efter fel. Återställning efter fel skapar sedan en ny virtuell dator på hello samma värden som hello huvudmålservern ESX-värd.

När du växlar tillbaka tooan alternativ plats hello data är återställda toohello samma datalager och hello samma ESX-värden som används av hello lokala huvudmålservern.

## <a name="prerequisites"></a>Krav
* toofail tillbaka virtuella VMware-datorer och fysiska servrar, måste en VMware-miljön. Misslyckas tillbaka tooa stöds inte fysisk server.
* toofail tillbaka, du måste ha skapat ett Azure-nätverk när du först ställa in skydd. Återställning efter fel behöver en VPN eller ExpressRoute-anslutning från hello Azure nätverk där hello Azure virtuella datorer är placerade toohello lokal plats.
* Hello virtuella datorer som du vill toofail igen tooare hanteras av en vCenter-server, se till att du har hello behörigheter som krävs för identifiering av virtuella datorer på vCenter-servrar. Mer information finns i [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
* Om det finns ögonblicksbilder på en virtuell dator, misslyckas återaktivera skydd. Du kan ta bort hello ögonblicksbilder eller hello diskar.
* Innan du växlar tillbaka skapa dessa komponenter:
  * **Skapa en Processerver i Azure**. Den här komponenten är en Azure VM som du skapar och fortsätta att köras under återställning efter fel. Du kan ta bort hello VM när återställning har slutförts.
  * **Skapa en huvudmålserver**: hello huvudmålservern skickar och tar emot data för återställning efter fel. hello hanteringsservern som du skapade lokala har en huvudmålserver som installeras som standard. Beroende på hello mängden trafik som misslyckades igen kan du dock behöva toocreate en separat huvudmålserver för återställning efter fel.
  * toocreate en ytterligare huvudmålservern som körs på Linux, ställa in hello Linux VM innan du installerar hello huvudmålservern, såsom beskrivs senare.
* En konfigurationsservern är lokala när du gör en återställning efter fel. Under återställning efter fel, måste hello virtuella datorn finns i hello configuration server-databas. Om hello configuration server-databasen innehåller inga VM, kan inte återställning lyckas. Därför ska du kontrollera att du gör regelbundna schemalagda säkerhetskopieringar av servern. I en katastrof, behöver du toorestore med hello samma IP-adress så att återställningen fungerar.
* Ange hello disk.enableUUID=true inställningen i **konfigurationsparametrar** av hello master mål VM i VMware. Om den här raden inte finns, kan du lägga till den. Den här inställningen är obligatorisk tooprovide en konsekvent universellt Unik identifierare (UUID) toohello virtuell disk (VMDK)-filen så att den är monterad på rätt sätt.
* Tänk på att en ”Master målservern får inte vara lagring vMotioned” tillstånd, vilket kan orsaka hello återställning toofail. hello VM kan inte startas eftersom hello diskar inte blir tillgängliga tooit.
* Lägg till en enhet som kallas en kvarhållningsenhetens till hello huvudmålservern. Lägg till en disk och formatera hello enhet.

## <a name="failback-policy"></a>Princip för återställning efter fel
tooreplicate tillbaka tooon (lokalt) behöver du en princip för återställning efter fel. hello princip skapas automatiskt när du skapar en princip för riktning framåt och det associeras automatiskt med hello konfigurationsservern. Den kan inte redigeras. hello principen har följande replikeringsinställningarna hello:

* Återställningspunktmålet tröskelvärde = 15 minuter
* Kvarhållningstid för återställningspunkten = 24 timmar
* Frekvens för appkonsekvent ögonblicksbild = 60 minuter

 ![Replikeringsinställningarna för hello principer](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Konfigurera en Processerver i Azure
Installera en Processerver i Azure så att hello Azure virtuella datorer kan skicka hello data tillbaka toohello lokala huvudmålservern.

Om du har skyddat de virtuella datorerna som klassiska resurser (hello VM återställas i Azure är en virtuell dator som har skapats med hjälp av hello klassiska distributionsmodellen), behöver du en Processerver i Azure. Om du har återställt hello virtuella datorer med Azure Resource Manager som hello distributionstypen måste hello Processervern ha Resource Manager som hello distributionstypen. hello distributionstypen väljs av hello Azure virtuella nätverk som du distribuerar hello Processervern till.

1. I **valvet** > **inställningar** > **Site Recovery-infrastruktur** (under **hantera**) > **Konfigurationsservrar** (under **för VMware och fysiska datorer**), Välj hello konfigurationsservern.
2. Klicka på **bearbeta Server**.

  ![Bearbeta knappen Server](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Välj toodeploy hello Processervern som **distribuera en återställning efter fel Processerver i Azure**.
4. Välj hello-prenumeration som du har återställt hello virtuella datorerna till.
5. Välj hello Azure-nätverk som du har återställt hello virtuella datorerna till. Hej måste Processervern toobe i samma nätverk så att hello återställts virtuella datorer och hello processen servern kan kommunicera hello.
6. Om du har valt en *klassiska distributionsmodellen* nätverk, skapa en virtuell dator via hello Azure Marketplace och sedan installera hello Processervern i den.

 ![Hej ”Lägg till Processerver” fönster](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 När du skapar hello Processervern, betalar du uppmärksamhet toohello följande:
 * hello hello avbildningen heter *Microsoft Azure Site Recovery Process Server V2*. Välj **klassiska** som hello distributionsmodell.

       ![Select "Classic" as hello Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Installera hello Processervern bl.a toohello instruktionerna i [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
7. Om du väljer hello *Resource Manager* Azure nätverk, distribuera hello Processervern genom att tillhandahålla hello följande information:

  * hello namnet på hello resursgrupp som du vill toodeploy hello servern till
  * hello namnet på hello-server
  * Ett användarnamn och lösenord för att logga in toohello server
  * hello storage-konto som du vill toodeploy hello servern ska
  * hello-undernät och hello nätverksgränssnittet som du vill tooconnect tooit
   >[!NOTE]
   >Du måste skapa en egen [nätverksgränssnittet](../virtual-network/virtual-networks-multiple-nics.md) (NIC) och markera den när du distribuerar hello Processervern.

    ![Ange information i dialogrutan för hello ”Lägg till Processerver”](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Klicka på **OK**. Den här åtgärden startar ett jobb som skapar en Resource Manager distribution typen virtuell dator under installationen av hello Processervern. tooregister hello server toohello konfigurationsservern och kör hello installationsprogrammet i hello VM genom att följa instruktionerna hello i [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md). Ett jobb toodeploy hello Processervern utlöses också.

  Hej Processervern visas på hello **konfigurationsservrar** > **associerade servrar** > **servrar** fliken.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > Hej Processervern inte visas under **VM egenskaper**. Visas endast på hello **servrar** fliken i hello hanteringsserver som den är registrerad. Det kan ta 10 minuter för too15 för hello Processervern tooappear.


## <a name="set-up-hello-master-target-server-on-premises"></a>Ställ in hello huvudmålservern server lokalt
Hej huvudmålservern tar emot hello återställning av data. hello-server installeras automatiskt på hello lokala management-servern, men om du växlar tillbaka för mycket data, måste du kanske tooset upp en ytterligare huvudmålservern. tooset upp en rikta server lokalt, hello följande:

> [!NOTE]
> tooset upp en huvudmålserver på Linux, hoppa över toohello nästa procedur. Använd endast CentOS 6.6 minimalt operativsystem som hello huvudmålservern OS.

1. Om du ställer in hello huvudmålservern i Windows, öppna hello Snabbkurs sida från hello virtuell dator som du installerar hello huvudmålservern på.
2. Hämta hello installationsfilen för hello Unified installationsprogram för Azure Site Recovery-guiden.
3. Kör installationsprogrammet för hello och i **innan du börjar**väljer **lägga till ytterligare servrar tooscale ut distribution**.
4. Fullständig hello guiden i hello samma sätt som du gjorde när du [konfigurera hello server](site-recovery-vmware-to-azure-classic.md). På hello **Server konfigurationsinformation** anger hello hello huvudmålservern serverns IP-adress, och ange en lösenfras tooaccess hello VM.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Konfigurera en Linux-VM som hello huvudmålservern
tooset in hello management-server som kör hello huvudmålservern som en Linux VM, installera hello CentOS 6.6 minimalt operativsystem. Hämta hello SCSI-ID: N för varje SCSI-hårddisk, installerar vissa ytterligare paket och tillämpa vissa egna ändringar.

#### <a name="install-centos-66"></a>Installera CentOS 6.6

1. Installera hello CentOS 6.6 minimalt operativsystem på hanteringsservern för hello VM. Behåll hello ISO på en DVD-enhet och starta hello system. Hoppa över hello media testning. Välj **amerikansk engelska** som hello språk, Välj **grundläggande lagringsenheter**, kontrollera att hello hårddisken inte innehåller några viktiga data, klickar du på **Ja**, och alla data. Ange hello värdnamnet för hello hanteringsserver och välj hello nätverkskort.  I hello **redigera System** dialogrutan **Anslut automatiskt**, och sedan lägga till en statisk IP-adress, nätverk och DNS-inställningarna. Ange en tidszon. tooaccess hello hanteringsservern, ange hello rotlösenordet.
2. När du tillfrågas om vilken typ av installation som du vill använda, Välj **Skapa anpassad Layout** som hello partition. Klicka på **Nästa**. Välj **lediga**, och klicka sedan på **skapa**. Skapa  **/** , **/var/krascher**, och **/home partitioner** med **FS typ:** **ext4**. Skapa hello växlingen partition som **FS typ: växlingen**.
3. Om befintliga enheter hittas, visas ett varningsmeddelande. Klicka på **Format** tooformat hello enhet med hello partitionsinställningar. Klicka på **skrivåtgärder ändra toodisk** tooapply hello partition ändringar.
4. Välj **installera startprogram** > **nästa** tooinstall hello startprogram på hello rot-partitionen.
5. När hello installationen är klar klickar du på **omstart**.

#### <a name="retrieve-hello-scsi-ids"></a>Hämta hello SCSI-ID: N

1. Hämta hello SCSI-ID: N för varje SCSI-hårddisk i hello VM efter hello installationen. toodo Stäng så hello hanteringsservern VM. Högerklicka hello VM posten i hello egenskaper för Virtuella datorer i VMware, > **redigera inställningar för** > **alternativ**.
2. Välj **Avancerat** > **allmän artikel**, och klicka sedan på **konfigurationsparametrar**. Det här alternativet är inte tillgänglig när hello datorn körs. För hello alternativet toobe tillgängliga, måste hello datorn stängas av.
3. Gör något av följande hello:
 * Om hello rad **disk. EnableUUID** finns, se till att hello värdet för**SANT** (skiftlägeskänsligt). Om hello värdet är redan tooTrue, kan du avbryta och testa hello SCSI-kommando i ett gästoperativsystem när den har startats.
 * Om hello rad **disk. EnableUUID** inte finns, klickar du på **Lägg till rad**, och Lägg sedan till den med hello **SANT** värde. Använd inte dubbla citattecken.

#### <a name="install-additional-packages"></a>Installera ytterligare paket
Hämta och installera ytterligare paket.

1. Kontrollera att hello huvudmålservern är anslutna toohello Internet.
2. toodownload och 15 Installationspaketen från lagringsplatsen för hello CentOS, kör du kommandot: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Om hello källdatorer du skyddar kör Linux med en Reiser eller XFS filsystem för hello rot- eller enhet, hämta och installera ytterligare paket på följande sätt:

   * \#CD /usr/local
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#rpm – ivh kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm reiserfs-verktyg för webbplatsuppgradering-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \#YUM installera enheten mapper multipath (obligatorisk tooenable multipath paket på hello huvudmålservern)

#### <a name="apply-custom-changes"></a>Använd anpassade ändringar
När du har slutfört hello efter installationen steg och installerade hello-paket, tillämpa egna ändringar hello följande:

1. Kopiera hello RHEL 6-64 Unified Agent binära toohello VM. toountar hello binära, köra det här kommandot: `tar –zxvf <file name>`.
2. toogive behörighet kan köra det här kommandot: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Kör hello följande skript: `# ./ApplyCustomChanges.sh`. Kör bara en gång. Starta om hello servern när den har slutförts.

## <a name="run-hello-failback"></a>Kör hello återställning efter fel
### <a name="reprotect-hello-azure-vms"></a>Skyddar virtuella hello Azure-datorer
1. I **valvet**i **replikerade objekt**, högerklicka på hello VM misslyckades över och välj sedan **skydda igen**.
2. På bladet hello visas hello riktningen skydd **Azure tooOn lokala** har redan valts.
3. I **Huvudmålserver** och **Processervern**hello lokala huvudmålservern och välj hello Azure VM Processervern.
4. Välj hello datalagret som du vill toorecover hello diskar lokalt till. Använd det här alternativet när hello lokala virtuella datorn bort och du behöver toocreate nya diskar. Ignorera hello alternativet om hello diskar redan finns, men du behöver fortfarande toospecify ett värde.
5. Använda en enhet toostop hello bevarandepunkter i tid när hello VM replikerade tillbaka tooon lokala. Vissa kriterier i en kvarhållningsenhetens visas här. Hello-enhet visas inte för hello huvudmålservern utan dessa villkor.

  * Volymen får inte vara används för andra ändamål (mål för replikering och så vidare).
  * Volymen bör inte vara i låsläge.
  * Volymen bör inte vara en cachevolym. (hello huvudmålservern installationen får inte finnas på den volymen. Hej Processervern plus master anpassad installation målvolymen är inte berättigad till kvarhållningsvolymen. Här hello installerade Processervern plus master målvolymen är hello cache hello huvudmålservern.)
  * hello volymtyp av filsystem får inte vara FAT och FAT32.
  * hello volymens kapacitet måste vara noll.
  * hello standard kvarhållningsvolymen för Windows är R-volym.
  * hello standard kvarhållningsvolymen för Linux är /mnt/retention.

6. principen för återställning av hello väljs automatiskt.
7. När du klickar på **OK** toobegin återaktivera skydd, ett jobb börjar tooreplicate hello virtuell dator från Azure toohello lokal plats. Du kan följa förloppet för hello på hello **jobb** fliken.

Om du vill toorecover tooan alternativ plats, Välj hello kvarhållningsenhetens och datalager som är konfigurerade för hello huvudmålservern. När du växlar tillbaka toohello lokal plats hello virtuella VMware-datorer i hello återställning skyddsplan använda hello samma datalagret som hello huvudmålservern. Om du vill toorecover hello replik Azure VM toohello samma lokala VM, hello lokala VM bör redan vara i hello samma datalagret som hello huvudmålservern. Om det inte finns någon VM lokal, skapas en ny under återaktivera skydd.

![Välj ”Azure tooon lokala” hello nedrullningsbara menyn](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Du kan även skydda igen på en återställning plan nivå. Om du har en replikeringsgrupp kan du skydda den igen med en återställningsplan. Använd hello tidigare värdena för varje skyddad dator när du skyddar med hjälp av en återställningsplan.

> [!NOTE]
> Replikeringsgrupper borde vara skyddad tillbaka med hello samma huvudmålservern. En gemensam tidpunkt kan inte fastställas för dem om de är skyddade igen med en annan huvudmålserver servrar.

### <a name="run-a-failover-toohello-on-premises-site"></a>Kör en växling vid fel toohello lokal plats
När du skyddar hello VM, kan du påbörja en växling från Azure tooon lokala.

1. Högerklicka på hello virtuella hello replikerade objekt på sidan och välj sedan **oplanerad växling**.
2. I **bekräfta redundans**kontrollerar hello redundansriktning (från Azure) och välj sedan hello återställningspunkt som du vill toouse hello växling vid fel (senaste hello eller hello senaste programkonsekventa återställningspunkten). En programkonsekventa återställningspunkt inträffar före hello senaste punkt i tiden och den kommer att orsaka dataförluster.
3. Under växling vid fel stängs Site Recovery hello Azure virtuella datorer. Du kan kontrollera tooensure hello Azure virtuella datorer har stängts av korrekt när du har kontrollerat att återställningen har slutförts som förväntat.

### <a name="reprotect-hello-on-premises-site"></a>Skapa nytt hello lokal plats
När återställning har slutförts, bort commit hello virtuella tooensure som hello VMs återställas i Azure. toodo så, högerklicka på hello skyddade objekt och klicka sedan på **genomför**. Den här åtgärden startar ett jobb som tar bort hello tidigare återställda virtuella datorer i Azure.

När hello genomförande har slutförts, dina data ska vara tillbaka på hello lokal plats, men det kommer inte att skydda. toostart replikering tooAzure igen, hello följande:

1. I **valvet**i **inställningen** > **replikerade objekt**, Välj hello virtuella datorer som har misslyckats tillbaka och klicka sedan på **att skydda**.
2. Ge hello värdet hello som måste toobe används toosend data tillbaka tooAzure.
3. Klicka på **OK**.

När hello återaktivera skyddet har slutförts, hello VM replikeras tillbaka tooAzure och du kan göra en växling vid fel.

### <a name="resolve-common-failback-issues"></a>Lösa vanliga problem med återställning efter fel
* Om du utför en skrivskyddade användaren vCenter identifiering och skydda virtuella datorer det lyckas och redundans fungerar. Under återaktivera skydd misslyckas redundans eftersom hello datastores inte kan identifieras. Som ett symtom visas inte hello datastores visas under återaktivera skydd. tooresolve problemet kan du uppdatera hello vCenter autentiseringsuppgifter med ett lämpligt konto som har behörighet och försök hello jobb. Mer information finns i [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md)
* Du kan se hello Nätverkshanteraren paketet har avinstallerats från hello datorn när du återställas på en Linux VM och köra den lokalt. Den här avinstallationen beror hello Nätverkshanteraren paketet tas bort när hello VM återställs i Azure.
* När en virtuell dator konfigureras med en statisk IP-adress och växlas över tooAzure, förvärvas hello IP-adress via DHCP. När du redundansväxlar tillbaka tooon lokala fortsätter hello VM toouse DHCP tooacquire hello IP-adress. Logga in toohello datorn och ange hello IP-adress tillbaka tooa statisk adress manuellt.
* Om du använder ESXi 5.5 gratisversion eller vSphere 6 Hypervisor gratisversion redundans lyckades, men återställning efter fel skulle misslyckas. tooenable återställning, uppgradera tooeither programmet utvärderingslicens.
* Om du inte kan nå hello konfigurationsservern från hello Processervern, kontrollera anslutningen toohello konfigurationsservern genom Telnet toohello configuration server-dator på port 443. Du kan också försöka tooping hello konfigurationsservern från hello Processervern datorn. En Process-Server bör också ha ett pulsslag när det är anslutet toohello konfigurationsservern.
* Om du försöker toofail tillbaka tooan alternativa vCenter, se till att din nya vCenter identifieras och att hello huvudmålservern identifieras också. Ett typiskt symtom är att hello datastores inte är tillgänglig eller visas i hello **skyddar** dialogrutan.
* En WS2008R2SP1 dator som skyddas som en fysisk lokala datorn kan inte flyttas tillbaka från Azure tooon lokala.

## <a name="fail-back-with-expressroute"></a>Växla tillbaka med ExpressRoute
Du kan växla tillbaka via en VPN-anslutning eller en ExpressRoute-anslutning. Om du vill toouse en ExpressRoute-anslutning, Tänk hello följande:

* Hej ExpressRoute-anslutning ska ställas in på hello Azure-nätverket som hello källdatorer växla över tooand där hello Azure Virtual Machines finns efter hello redundansväxlingen.
* Data som är replikerade tooan Azure storage-konto på en offentlig slutpunkt. toouse en ExpressRoute-anslutning, ställa in offentlig peering i ExpressRoute med hello Måldatacenter för Site Recovery replikering.
