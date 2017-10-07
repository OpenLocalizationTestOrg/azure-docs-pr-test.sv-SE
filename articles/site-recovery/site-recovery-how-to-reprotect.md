---
title: "aaaReprotect från Azure tooan lokal plats | Microsoft Docs"
description: "Du kan starta en återställning efter fel toobring VMs tillbaka tooon lokala efter en redundansväxling av virtuella datorer tooAzure. Lär dig hur tooreprotect innan en återställning efter fel."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 94c86e79cba4cd3f0c5821fdd5509cca1d0e7820
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-azure-tooan-on-premises-site"></a>Skydda igen från Azure tooan lokal plats



## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooreprotect Azure virtuella datorer från Azure tooan lokal plats. Följ hello instruktionerna i den här artikeln när du är klar toofail tillbaka din virtuella VMware-datorer eller fysiska Windows-/ Linux-servrar när de har redundansväxlats från hello lokal plats tooAzure (enligt beskrivningen i [replikera VMware virtual datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-failover.md)).

> [!WARNING]
> Du kan inte återställas efter du [slutföra migreringen](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), flytta en virtuell dator tooanother resursgrupp eller ta bort en virtuell Azure-dator.


Efter att återaktivera skyddet har slutförts och hello skyddade virtuella datorer replikerar, du kan starta en återställning efter fel på hello virtuella datorer toobring dem toohello lokal plats.

Skicka kommentarer eller frågor hello slutet av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Titta på hello följande video om hur toofail över från Azure tooan lokal plats för en snabb överblick.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Krav
När du förbereder tooreprotect virtuella datorer kan ta eller Överväg hello följande nödvändiga åtgärder:

* Om en vCenter-servern hanterar hello virtuella datorer som du vill toofail tillbaka till, kontrollerar du att hello [nödvändiga behörigheter](site-recovery-vmware-to-azure-classic.md) för identifiering av virtuella datorer på vCenter-servrar.

  > [!WARNING]
  > Om det finns ögonblicksbilder på hello lokalt master mål eller hello virtuell dator, återaktivera skydd misslyckas. Du kan ta bort hello ögonblicksbilder på hello Huvudmålet innan du går vidare tooreprotect. hello ögonblicksbilder på den virtuella datorn hello slås automatiskt samman under ett Skapa nytt jobb.

* Innan du växlar tillbaka skapa två ytterligare komponenter:

  * **Processervern**: hello [processervern](site-recovery-vmware-setup-azure-ps-resource-manager.md) tar emot data från hello skyddad virtuell dator i Azure och skickar data toohello lokal plats. Ett nätverk med låg latens krävs mellan hello processervern och hello skyddade virtuella datorn. Du kan därför har en lokal processerver om du använder en Azure ExpressRoute-anslutning eller en Azure-baserade processerver och en VPN-anslutning.
  
  * **Huvudmålservern**: hello huvudmålservern tar emot data för återställning efter fel. hello lokala management-server som du har skapat har en huvudmålserver som installeras som standard. Beroende på hello mängden trafik som misslyckades igen kan du dock behöva toocreate en separat huvudmålserver för återställning efter fel.
    * [En virtuell Linux-dator måste en Linux-huvudmålserver](site-recovery-how-to-install-linux-master-target.md).
    * En virtuell Windows-dator måste en Windows-huvudmålserver. Du kan använda hello lokalt processen server och master måldatorer igen.

    Hej huvudmålservern har andra krav som anges i [vanliga saker toocheck på Huvudmålet innan du skyddar](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

* En konfigurationsservern är lokala när du växlar tillbaka. Under återställning efter fel, måste hello virtuella datorn finns i hello configuration server-databas. Återställning är annars misslyckas. 

> [!IMPORTANT]
> Kontrollera att du vidtar regelbundna schemalagda säkerhetskopieringar serverns konfiguration. Om det finns en katastrof, återställa hello-server med samma IP-adress så att återställningen fungerar hello.

* Ange hello `disk.EnableUUID=true` i hello konfigurationsparametrar för hello huvudmålservern virtuell dator i VMware. Om den här raden inte finns, kan du lägga till den. Den här inställningen är obligatorisk tooprovide en konsekvent UUID toohello disk för virtuell dator (VMDK) så att den monterar korrekt.

* *Du kan inte använda Storage vMotion på hello huvudmålservern*. Detta kan orsaka hello toofail för återställning efter fel. hello virtuella datorn kan inte starta eftersom hello diskar inte är tillgängliga tooit. tooprevent problemet utelämna hello master målservrar vMotion-listan.

* Lägg till en ny enhet toohello huvudmålserver: en kvarhållningsenhetens. Lägg till en ny disk och format hello-enhet.


### <a name="frequently-asked-questions"></a>Vanliga frågor och svar

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-tooreplicate-data-back-toohello-on-premises-site"></a>Varför behöver jag en S2S VPN-anslutning eller en ExpressRoute-anslutning tooreplicate data tillbaka toohello lokal plats?
Replikeringen från lokala tooAzure kan inträffa under hello Internet eller en ExpressRoute-anslutning med offentlig peering, återaktivera skydd och återställning kräver en plats-till-plats (S2S) VPN-tooreplicate data. Ange hello nätverk så att hello misslyckades över virtuella datorer i Azure kan nå (ping) hello lokalt konfigurationsservern. Du kan också distribuera en processerver i hello Azure-nätverk för hello misslyckades över virtuella datorn. Den här processen server bör också vara kan toocommunicate med hello lokalt konfigurationsservern.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>När ska installera en processerver i Azure?


hello virtuella datorer i Azure som du vill tooreprotect skickar replikering data tooa processervern. Konfigurera nätverket så att hello virtuella datorer i Azure kan nå hello processervern.

Du kan distribuera en processerver i Azure eller använder hello befintlig processen server som du använde under växling vid fel. hello viktiga punkt tooconsider är hello latens toosend hello data från hello virtuella datorer på Azure toohello processervern.

Om du har en ExpressRoute-anslutning som konfigurerar kan du använda en lokal process toosend serverdata eftersom hello fördröjning mellan hello virtuell dator och hello processervern är låg.

 ![Arkitekturdiagram för ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Om du har bara en S2S-VPN, rekommenderar vi dock distribuera hello processerver i Azure.

 ![Arkitekturdiagram för VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Kom ihåg att replikering sker endast över hello S2S VPN eller hello privat peering i ExpressRoute-nätverk. Se till att tillräckligt med bandbredd är tillgänglig över nätverkskanalen.

Information om hur du installerar en Azure-baserade process finns [hantera en processerver som körs i Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Vi rekommenderar att du använder en för Azure-baserade processerver vid återställning. hello Replikeringsprestanda är högre om hello processervern är närmare toohello replikering av virtuell dator (hello misslyckades-over-dator i Azure). Under din bevis på koncept (POC) eller demonstration använda du dock hello lokalt processervern tillsammans med ExpressRoute med privat peering toocomplete hello POC snabbare.



#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Vilka portar bör jag öppna på olika komponenter så att återaktivera skyddet fungerar?

![Portar för redundans och återställning efter fel](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Vilka huvudmålservern ska använda för att återaktivera skydd?
En lokal huvudmålservern är obligatoriska tooreceive data från hello processervern och sedan skriva toohello lokala virtuella datorns VMDK. Om du skyddar virtuella Windows-datorer, måste en Windows-huvudmålserver. Du kan återanvända hello lokalt processervern och huvudmålservern<!-- !todo component -->. Du måste tooset huvudmålservern en ytterligare lokala Linux för virtuella Linux-datorer.


Information om hur du installerar en huvudmålserver finns:

* [Hur tooinstall Windows huvudservern mål](site-recovery-vmware-to-azure.md)
* [Hur tooinstall Linux huvudservern mål](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-hello-on-premises-esxi-host-during-failback"></a>Vilka typer av datalagret stöds på hello lokalt ESXi-värd under återställning efter fel?

Azure Site Recovery stöder misslyckas tillbaka för närvarande endast tooa virtuella file system (VMFS) eller virtuellt SAN-datalagret. Ett NFS-datalager stöds inte. På grund av toothis begränsning ange hello datastore markeringen på hello skyddar skärmen är tom hello gäller NFS datastores eller den visar hello virtuellt SAN-databasen men misslyckas under hello jobb. Om du avser toofail tillbaka du växla tillbaka tooit och skapa ett datalager VMFS lokalt. Den här återställning kommer en fullständig hämtning av hello VMDK.

### <a name="common-things-toocheck-after-completing-installation-of-hello-master-target-server"></a>Vanliga saker toocheck när du har slutfört installationen av hello huvudmålservern

* Om hello virtuella finns lokalt på Hej vCenter-servern, hello huvudmålservern måste komma åt toohello lokala virtuella datorns VMDK. Åtkomst är nödvändiga toowrite hello replikerade data toohello virtuella datorns diskar. Se till att hello lokala virtuella datorns datalagret har monterats på hello huvudmålservern värd med läs-/ skrivbehörighet.

* Om hello virtuella datorn inte finns lokalt på hello vCenter-servern måste hello Site Recovery-tjänsten toocreate en ny virtuell dator under återaktivera skydd. Den här virtuella datorn skapas på hello ESX-värd som du skapar hello huvudmålservern. Välj hello ESX-värd noggrant, så att hello återställning av virtuell dator skapas på hello-värden som du vill.

* *Du kan inte använda Storage vMotion för hello huvudmålservern*. Detta kan orsaka hello toofail för återställning efter fel. hello virtuella datorn kan inte starta eftersom hello diskar inte är tillgängliga tooit.

  > [!WARNING]
  > Om ett huvudmål genomgår en Storage vMotion aktivitet efter återaktivera skydd, migrera hello skyddade virtuella datorer diskar som är anslutna toohello huvudmålservern toohello mål för hello vMotion aktivitet. Om du försöker toofail efter denna misslyckas vill koppla bort av hello disk eftersom hello diskar inte finns. Det blir svårt toofind hello diskar i storage-konton. Du behöver toofind dem manuellt och kopplar dem toohello virtuella datorn. Efter det kan hello lokala virtuella datorn startas.

* Lägg till en kvarhållning enhet tooyour befintliga Windows huvudmålserver. Lägg till en ny disk och format hello-enhet. Hej kvarhållningsenhetens är används toostop hello punkter i tiden när hello virtuella datorn replikeras tillbaka toohello lokal plats. Följande är några villkor för en enhet för kvarhållning. Utan dessa villkor visas hello enheten inte för hello huvudmålservern.

   * hello volym används inte i något annat syfte, till exempel ett mål för replikering.

   * hello volymen är inte i låsläge.

   * hello volymen är inte en cachevolym. hello huvudmålservern installationen får inte finnas på den volymen. hello anpassad installationsvolymen för hello processen server och master mål är inte berättigad till en kvarhållningsvolymen. När hello processervern och huvudmålservern installeras på en volym, är hello volym en cache hello huvudmålservern.

   * hello filsystem hello volymen är inte FAT eller FAT32.

   * hello volymens kapacitet är noll.

   * hello standard kvarhållningsvolymen för Windows är hello R-volym.

   * hello standard kvarhållningsvolymen för Linux är /mnt/retention.

  > [!IMPORTANT]
  > Du behöver tooadd en ny enhet om du använder en befintlig process serverkonfiguration/server-dator eller en skala eller en process server eller huvudtjänsten server måldatorn. hello ny enhet ska uppfylla hello föregående krav. Om det inte finns någon hello kvarhållningsenhetens, visas den inte i hello markeringen listrutan på hello-portalen. När du lägger till en enhet toohello lokal huvudmålserver upptar too15 minuter för hello enhet tooappear hello markering på hello-portalen. Du kan också uppdatera hello konfigurationsservern om hello enheten inte visas efter 15 minuter.

* En Linux misslyckades över virtuell dator kräver en Linux-huvudmålserver. En Windows kunde inte över virtuell dator kräver en Windows-huvudmålserver.

* Installera VMware-verktyg på hello huvudmålservern. Hej datastores på hello huvudmålservern ESXi-värd kan inte identifieras utan hello VMware-verktyg.

* Aktivera hello `disk.EnableUUID=true` parameter på hello huvudmålservern virtuell dator med hjälp av hello vCenter egenskaper. <!-- !todo Needs link. -->

* Hej huvudmålservern bör ha minst en VMFS datastore ansluten. Om det ingen finns hello **Datastore** indata i hello skyddar sidan är tomt, och du kan inte fortsätta.

* Hej huvudmålservern kan inte ha ögonblicksbilder på hello diskar. Om det finns ögonblicksbilder, misslyckas återaktivera skydd och återställning efter fel.

* Hej huvudmålservern kan inte ha en Paravirtual SCSI-styrenhet. hello-styrenhet kan bara finnas en domänkontrollant för en LSI Logic. Utan en domänkontrollant för en LSI Logic återaktivera skyddet fungerar inte.

<!--
### Failback policy
tooreplicate back tooon-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with hello configuration server during creation.
2. This policy is not editable.
3. hello set values of hello policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-tooreprotect"></a>Steg tooreprotect

> [!NOTE]
> Efter att en virtuell dator startar i Azure, tar det lite tid för hello agent tooregister tillbaka toohello konfigurationsservern (upp too15 minuter). Återaktivera skydd misslyckas under den här tiden och ett felmeddelande som anger att hello-agenten inte är installerad. Vänta några minuter och försök sedan att återaktivera skyddet igen.



1. I **valvet** > **replikerade objekt**, högerklicka på hello virtuell dator som har växlas över och välj sedan **skydda igen**. Du kan också klicka på hello maskin- och välj **skydda igen** från hello kommandoknappar.
2. Observera att hello riktningen för skydd, i hello bladet **Azure tooOn lokala**, har redan valts.
3. I **Huvudmålserver** och **Processervern**hello lokala huvudmålservern och välj hello processervern.
4. För **Datastore**, Välj hello datastore toowhich som du vill toorecover hello diskar lokalt. Det här alternativet används när hello lokal virtuell dator tas bort, och du behöver toocreate nya diskar. Det här alternativet ignoreras om hello diskar redan finns, men du måste fortfarande toospecify ett värde.
5. Välj hello kvarhållningsenhetens.
6. principen för återställning av hello väljs automatiskt.
7. Klicka på **OK** toobegin återaktivera skydd. Ett jobb börjar tooreplicate hello virtuell dator från Azure toohello lokal plats. Du kan följa förloppet för hello på hello **jobb** fliken.

Om du vill toorecover tooan alternativ plats (när hello lokala virtuella datorn tas bort), Välj hello kvarhållningsenhetens och datalager som är konfigurerade för hello huvudmålservern. När du växlar tillbaka toohello lokal plats hello VMware virtuella datorer hello återställning skydd plan används hello samma datalagret som hello master riktade mot servern. En ny virtuell dator skapas sedan i vCenter.

Om du vill toorecover hello virtuell dator på Azure tooan befintliga lokala virtuella datorn, montera hello lokala virtuella datorn datastores med läs-/ skrivbehörighet på hello huvudmålservern server ESXi-värd.
    ![Skydda dialogrutan igen](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Du kan även skydda igen på hello nivå av en återställningsplan. En replikeringsgrupp kan att återaktivera skyddet endast via en återställningsplan. När du skyddar med hjälp av en återställningsplan måste tooprovide hello värden för varje skyddad dator.

> [!NOTE]
> Använd hello samma huvudmålservern server tooreprotect en replikeringsgrupp. Om du använder en annan huvudmålserver server tooreprotect en replikeringsgrupp kan inte hello-server tillhandahålla en gemensam plats i tid.

> [!NOTE]
> hello lokala virtuella datorn stängs av under återaktivera skydd. Detta hjälper att säkerställa datakonsekvens vid replikering. Aktivera inte hello virtuell dator efter att återaktivera skyddet har slutförts.

När hello återaktivera skydd lyckas, kommer hello virtuella ange ett skyddat läge.

## <a name="next-steps"></a>Nästa steg

Efter hello virtuell dator har angett ett skyddat läge, kan du [starta en återställning efter fel](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

hello återställning efter fel stängs hello virtuell dator i Azure och Start hello lokala virtuella datorn. Förvänta dig vissa avbrott för hello program. Välj en tid för återställning efter fel när hello program kan tolerera driftstopp.

## <a name="common-problems"></a>Vanliga problem

* Om du använder en mall toocreate dina virtuella datorer, måste du kontrollera att varje virtuell dator har sin egen UUID för hello diskar. Om hello lokala virtuella datorns UUID konflikt med som hello huvudmålservern eftersom både skapades från hello samma mall och återaktivera skydd misslyckas. Distribuera en annan huvudmålserver som inte har skapats från hello samma mall.

* Om du utför en skrivskyddade användaren vCenter identifiering och skydda virtuella datorer, skydd lyckas och redundans fungerar. Under återaktivera skydd misslyckas redundans eftersom hello datastores inte kan identifieras. Ett symtom är att hello datastores inte listats under återaktivera skydd. tooresolve det här problemet kan du uppdatera hello vCenter autentiseringsuppgifter med ett lämpligt konto som har behörighet, och försök hello jobb. Mer information finns i [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Du kan se hello Nätverkshanteraren paketet har avinstallerats från hello datorn när du återställas på en virtuell Linux-dator och köra det lokalt. Den här avinstallationen beror hello Nätverkshanteraren paketet tas bort när hello virtuella datorn återställs i Azure.

* När en virtuell Linux-dator är konfigurerad med en statisk IP-adress och växlas över tooAzure, förvärvas hello IP-adress från DHCP. När du redundansväxlar tooon lokala fortsätter hello virtuella dator toouse DHCP tooacquire hello IP-adress. Logga in toohello datorn manuellt och ange hello IP-adress tillbaka tooa statisk adress. En virtuell Windows-dator kan hämta den statiska IP-Adressen igen.

* Om du använder ESXi 5.5 gratisversion eller vSphere 6 Hypervisor gratisversion redundans lyckas, men lyckas inte återställning efter fel. tooenable återställning, uppgradera tooeither programmet utvärderingslicens.

* Om du inte kan nå hello konfigurationsservern från hello processervern, Använd Telnet toocheck anslutning toohello konfigurationsservern på port 443. Du kan också försöka tooping hello konfigurationsservern från hello processervern. En processerver bör också ha ett pulsslag när det är anslutet toohello konfigurationsservern.

* Om du försöker toofail tillbaka tooan alternativa vCenter kontrollerar du att din nya vCenter och hello huvudmålservern identifieras. Ett typiskt symtom är att hello datastores inte är tillgänglig eller visas i hello **skyddar** dialogrutan.

* En Windows Server 2008 R2 SP1-server som skyddas som en fysisk lokala servern kan inte flyttas tillbaka från Azure tooan lokal plats.
