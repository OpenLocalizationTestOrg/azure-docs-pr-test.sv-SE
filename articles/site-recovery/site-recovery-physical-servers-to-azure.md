---
title: aaaReplicate fysiska servrar tooAzure | Microsoft Docs
description: "Beskriver hur toodeploy Azure Site Recovery tooorchestrate replikering, redundans och återställning av lokala Windows-/ Linux fysiska servrar tooAzure med hjälp av hello Azure-portalen"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: physical-walkthrough-overview
ms.openlocfilehash: cf5928fb631f6858d57b27f6f21babc312714e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
---
# <a name="replicate-physical-machines-tooazure-by-using-site-recovery"></a>Replikera fysiska datorer tooAzure genom att använda Site Recovery


Den här artikeln beskriver hur tooreplicate lokala fysiska datorer tooAzure med hello Azure Site Recovery-tjänsten i hello Azure-portalen.

Om du vill toomigrate fysiska datorer tooAzure (endast redundans), skrivskyddade [migrera tooAzure med Site Recovery](site-recovery-migrate-to-azure.md) toolearn mer.

Publicera kommentarer och frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Krav

**Stöd för krav** | **Detaljer**
--- | ---
**Azure** | Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).
**Lokal konfigurationsserver** | Lokal dator (fysisk eller VMware VM) kör Windows Server 2012 R2 eller senare. Du ställa in hello konfigurationsservern under distributionen av Site Recovery.<br/><br/> Som standard hello processervern och huvudmålservern är installerade på den här datorn. När du skalar upp, du kan behöva en separat processerver och hello har samma krav som hello konfigurationsservern.<br/><br/> Lär dig mer om komponenterna i [konfigurera hello källmiljön](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Lokala virtuella datorer** | Datorer som du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**URL: er** | hello konfigurationsservern behöver åtkomst toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för hantering av kontrollen och identitet).<br/><br/> Tillåt URL: en för hello MySQL hämtning: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitetstjänsten** | Den här tjänsten är installerad på varje dator som du vill tooreplicate.

## <a name="limitations"></a>Begränsningar

**Begränsning** | **Detaljer**
--- | ---
**Azure** | Lagring och nätverk konton måste finnas i hello samma region som hello-valvet.<br/><br/> Om du använder ett premiumlagringskonto måste du också en standard lagra replikeringsloggar toostore för kontot.<br/><br/> Du kan replikera toopremium konton i Central och södra Indien.
**Lokal konfigurationsserver** | Om du installerar hello konfigurationsservern på en VMware-VM vara hello VM korttypen VMXNET3. Om det inte är [installera denna uppdatering](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1).<br/><br/> Om du använder en VM VMware ska vSphere PowerCLI 6.0 installeras på den.<br/><br> hello datorn får inte vara en domänkontrollant.<br/><br/> hello datorn bör ha en statisk IP-adress.<br/><br/> hello värdnamn ska vara högst 15 tecken eller mindre, och hello operativsystemet ska vara på engelska.
**Replikerade datorer** | Kontrollera [Azure VM begränsningar](site-recovery-prereq.md#azure-requirements).<br/><br/> Om du vill tooenable konsekvens för flera, som gör att datorer som kör samma arbetsbelastning toobe återställas hello peka tillsammans tooa konsekventa data, öppna port 20004 på hello-datorn.<br/><br/> Vissa typer av [Linux lagring](site-recovery-support-matrix-to-azure.md#support-for-storage) stöds.
**Återställning efter fel** | Du kan inte växla tillbaka från Azure tooa fysisk dator. Om du vill toobe kan toofail tillbaka tooon lokala efter växling vid fel, måste en VMware-miljön så att du kan växla tillbaka tooa VMware VM.


## <a name="set-up-azure"></a>Konfigurera Azure

1. [Skapa ett Azure nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

      a. Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.

      b. Du kan ställa in ett nätverk i Azure [Resource Manager](../resource-manager-deployment-model.md) eller i klassiskt läge.

2. Konfigurera en [Azure storage-konto](../storage/storage-create-storage-account.md#create-a-storage-account) för replikerade data.

    a. hello-kontot kan vara standard eller [premium](../storage/storage-premium-storage.md).

    b. Du kan konfigurera ett konto i Resource Manager eller i klassiskt läge.

## <a name="prepare-hello-configuration-server"></a>Förbereda hello konfigurationsservern

1. Installera Windows Server 2012 R2 eller senare på en lokal fysisk server eller en VMware-VM.

2. Kontrollera att hello datorn har åtkomst toohello webbadresserna i [krav](#prerequisites).

## <a name="prepare-for-mobility-service-installation"></a>Förbereda för installationen av Mobility

Om du vill toopush hello Mobility service tooa fysiska datorn behöver ett konto som kan användas av hello processen server tooaccess hello datorer. hello kontot används endast för hello push-installation. Du kan använda en domän eller lokalt konto:

  - För Windows, om du inte använder ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo detta i hello registret under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten för hello **LocalAccountTokenFilterPolicy**, med värdet 1. Om du vill tooadd hello registerposten för Windows från ett kommandoradsgränssnitt, skriver du:

        ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
  - För Linux ska hello kontot vara en rotanvändare på Linux-hello källservern.


## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-hello-protection-goal"></a>Välj hello skyddsmål

Välj vad du vill att tooreplicate och där du vill att tooreplicate till.

1. Klicka på **Recovery Services-valv** > **valvet**.
2. På hello **resurs** -menyn klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.

    ![Välja mål](./media/site-recovery-vmware-to-azure/choose-goal-physical.PNG)

3. I **skyddsmål**väljer **tooAzure** > **inte virtualiserade / andra**.


## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera virtuella datorer.

1. Klicka på **Site Recovery** > **förbereda infrastrukturen** > **källa**.
2. Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.

    ![Konfigurera källan](./media/site-recovery-vmware-to-azure/set-source1.png)

3. I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta hello **Unified installationsprogram för Site Recovery** installationsfilen.
5. Hämta hello **valvregistreringsnyckel**. Du behöver den här nyckeln när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

   ![Konfigurera källan](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Kör Site Recovery enhetlig installation

Innan du börjar hello följande:

- Få en snabb överblick av video. (hello videon beskriver hur tooreplicate virtuella VMware-datorer, men hello bearbeta är liknande för replikeringen av den fysiska datorn.)

    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

- Kontrollera att hello systemklockan är synkroniserad med i hello configuration server-dator, en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Installationen misslyckas om det är 15 minuter framför eller bakom.
- Kör installationsprogrammet som lokal administratör på hello configuration server-datorn.
- Kontrollera att TLS 1.0 har aktiverats på hello-datorn.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Innan du konfigurerar hello målmiljön Kontrollera toomake till att du har en [Azure storage-konto och nätverk](#set-up-azure).

1. Klicka på **Förbered infrastrukturen** > **mål**, och välj hello Azure-prenumeration du vill toouse.
2. Ange om ditt mål distributionsmodell är Resource Manager eller klassisk.
3. Site Recovery kontrollerar toomake till att du har en eller flera kompatibla Azure storage-konton och nätverk.

   ![mål](./media/site-recovery-vmware-to-azure/gs-target.png)

4. Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk** toocreate en Resource Manager-konto eller nätverket infogad.

## <a name="set-up-replication-settings"></a>Konfigurera replikeringsinställningar

Innan du börjar få en snabb överblick av video. (hello videon beskriver hur tooreplicate virtuella VMware-datorer, men hello bearbeta är liknande för replikeringen av den fysiska datorn.)

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klicka på toocreate en ny replikeringsprincip **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.
2. I **skapa replikeringsprincip**, ange ett principnamn.
3. I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen. Det här värdet anger hur ofta data återställningspunkter skapas. En avisering genereras om kontinuerlig replikering överskrider den gränsen.
4. I **kvarhållningstid för återställningspunkten**, ange hur länge (i timmar) hello kvarhållningsperiod är för varje återställningspunkt. Replikerade virtuella datorer kan återställas tooany punkt i ett fönster. In too24 stöds timmars kvarhållning för datorer replikerade toopremium lagring. In too72 stöds timmars kvarhållning för datorer replikerade toostandard lagring.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) skapas återställningspunkter som innehåller programkonsekventa ögonblicksbilder. Klicka på **OK** toocreate hello princip.

    ![Replikeringsprincip](./media/site-recovery-vmware-to-azure/gs-replication2.png)

6. När du skapar en ny princip associeras den automatiskt med hello konfigurationsservern. Som standard skapas automatiskt en matchande princip för återställning efter fel. Till exempel om hello replikeringsprincip är **rep princip**, hello principer är **rep-princip-återställning**. Den här principen används inte förrän du har initierat en återställning från Azure.  


## <a name="plan-capacity"></a>Planera kapacitet

1. Nu när du har grundläggande infrastrukturen konfigurerar du tycker om kapacitetsplanering och ta reda på om du behöver ytterligare resurser. [Läs mer](site-recovery-plan-capacity-vmware.md).
2. När du är klar med kapacitetsplanering, Välj **Ja** i **har du planerat kapaciteten?**

   ![Kapacitetsplanering](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Förbereda virtuella datorer för replikering

Alla datorer som du vill tooreplicate måste ha hello mobilitetstjänsten installeras. Du kan installera hello mobilitetstjänsten på ett antal olika sätt:

- Installera hello tjänst med en push-installation från hello processervern. Du behöver tooprepare datorer i förväg toouse den här metoden.
- Installera hello tjänst med hjälp av distributionsverktyg, till exempel System Center Configuration Manager eller Azure Automation Desired State Configuration.
- Installera hello-tjänsten manuellt.

[Läs mer](site-recovery-vmware-to-azure-install-mob-svc.md).


## <a name="enable-replication"></a>Aktivera replikering

Innan du börjar:

- Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.
- När du lägger till eller ändra virtuella datorer, kan det ta upp too15 minuter eller längre för ändringar tootake effekt och för dem tooappear hello-portalen.
- Du kan kontrollera hello senast identifierade tid för virtuella datorer i **Konfigurationsservrar** > **senaste kontakt på**.
- tooadd virtuella datorer utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den), och klicka på **uppdatera**.
- Om en virtuell dator är förberedd för push-installation installerar hello mobilitetstjänsten automatiskt hello processervern när du aktiverar replikering.
- Få en snabb överblick av video. (hello videon beskriver hur tooreplicate virtuella VMware-datorer, men hello bearbeta är liknande för replikeringen av den fysiska datorn.)

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]


### <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Som standard replikeras alla diskar på en dator. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en maskin- eller startas om (till exempel pagefile.sys eller SQL Server tempdb).

### <a name="replicate-vms"></a>Replikera virtuella datorer

1. Klicka på **replikera program** > **källa**.
2. I **källa**väljer **lokalt**.
3. I **datakällplats**väljer hello configuration servernamn.
4. I **datorn typen**väljer **fysiska datorer**.
5. I **processervern**, Välj hello processervern. Om du inte har skapat några ytterligare servrar är den här servern hello konfigurationsservern. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-physical-to-azure/chooseVM.png)

6. I **mål**väljer hello **prenumeration** och hello **resursgruppen** som du vill toocreate hello virtuella Azure-datorer efter redundans. Välj hello distribution modell som du vill toouse i Azure (klassiska eller Resource Manager) för hello misslyckades över virtuella datorer.

7. Välj hello Azure storage-konto som du vill använda toouse för replikering av data. Om du inte vill toouse ett konto som du redan har konfigurerat, kan du skapa en ny.

8. Välj hello **Azure-nätverk** och **undernät** toowhich virtuella Azure-datorer ansluta efter växling vid fel. Välj **konfigurera nu för valda datorer** tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare** tooselect hello Azure-nätverk per dator. Om du inte vill toouse ett befintligt nätverk kan skapa du en.

    ![Aktivera replikering](./media/site-recovery-physical-to-azure/targetsettings.png)

9. I **fysiska datorer**, klickar du på **+ fysisk dator** och ange hello **namn** och **IP-adress**. Välj operativsystem för hello hello datorn som du vill tooreplicate. Det tar några minuter tills datorer identifieras och visas i hello lista.

    ![Aktivera replikering](./media/site-recovery-physical-to-azure/machineselect.png)

10. I **egenskaper** > **konfigurera egenskaper**väljer hello konto toobe som används av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.
11. Alla diskar replikeras som standard. Klicka på **alla diskar**, och avmarkera alla diskar som du inte vill tooreplicate. Klicka sedan på **OK**. Du kan ange ytterligare egenskaper för Virtuella datorer senare.

    ![Aktivera replikering](./media/site-recovery-physical-to-azure/configprop.png)

12. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello. Om du ändrar en princip är ändringar tillämpade toohello replikerar datorn och toonew datorer.
13. Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen. Klicka sedan på **OK**. Tänk på följande:

    a. Datorer i replikeringsgrupper replikeras tillsammans och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.

    b. Vi rekommenderar att du samla in virtuella datorer och fysiska servrar så att de speglar dina arbetsbelastningar. Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda. Det bör bara användas om datorer som körs hello samma arbetsbelastning och du behöver enhetlighet.

    ![Aktivera replikering](./media/site-recovery-physical-to-azure/policy.png)

14. Klicka på **Aktivera replikering**. Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs, hello datorn är redo för redundans.

När du har aktiverat replikering installeras hello mobilitetstjänsten om du konfigurerar push-installation. När hello mobilitetstjänsten är push-installeras på en dator kan ett skyddsjobb startar och misslyckas. När hello fel toomanually måste starta om varje dator. Hello-skyddsjobb startar sedan igen och den inledande replikeringen sker.


### <a name="view-and-manage-azure-vm-properties"></a>Visa och hantera Virtuella Azure-egenskaper

Vi rekommenderar att du kontrollera hello VM egenskaper och gör eventuella ändringar som du behöver.

1. Klicka på **replikerade objekt**, och välj hello-datorn. Hej **Essentials** bladet visar information om inställningar för datorer och status.
2. I **egenskaper**, kan du visa replikering och information om växling vid fel för hello VM.
3. I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, kan du ange hello Azure VM-namn och mål för storlek. Ändra hello namnet toocomply med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) om du behöver.
4. Ändra inställningar för hello målnätverket, undernätet och IP-adress som är tilldelade toohello virtuella Azure-datorn:

    a. Du kan ange IP-adressen för hello mål.

    b.  Om du inte anger någon adress använder hello misslyckades över datorn DHCP.

    c. Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte växling vid fel.

    d. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.

    e. hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn:

     - Om hello antalet nätverkskort på källdatorn hello är hello samma som eller mindre än hello antalet nätverkskort som tillåts för hello måldatorns storlek så hello målet har hello samma antal kort som hello källa.
     - Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello antalet tillåtna för hello Målstorlek sedan hello högsta används.
     - Om en källdator har två nätverkskort och hello måldatorn stöder fyra har hello måldatorn två nätverkskort. Om hello källdatorn har två nätverkskort men målstorleken hello stöds stöder endast en har hello måldatorn bara en kort.     
   - Om hello virtuella datorn har flera nätverkskort, alla ansluta toohello samma nätverk.
   - Om hello virtuella datorn har flera nätverkskort och sedan hello först visas i listan hello blir hello *standard* nätverkskort i hello Azure VM.
5. I **diskar**, kan du se hello VM, operativsystemet och hello datadiskar som replikeras.

## <a name="run-a-test-failover"></a>Köra ett redundanstest

När du har konfigurerat allt, kör du en testa redundans toomake att allt fungerar som förväntat. Titta på en video Snabböversikt innan du börjar.

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. toofail över en enskild dator i **inställningar** > **replikerade objekt**, klickar du på **redundanstest**.

    ![Testa redundans](./media/site-recovery-vmware-to-azure/TestFailover.png)

2. toofail via en återställning planera i **inställningar** > **Återställningsplaner**, högerklicka på hello plan > **Redundanstestningen**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).  
3. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer är anslutna efter redundansväxlingen.
4. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller genom att klicka på hello **Redundanstest** jobb i valvnamnet > **inställningar** > **jobb**  >  **Site Recovery-jobb**.
5. När hello växling vid fel är klar kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM hello rätt storlek, att den är ansluten toohello lämpligt nätverk och att den körs.
6. Om du [förberedde för anslutning efter redundansväxlingen](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), bör du kunna tooconnect toohello Azure VM.
7. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. Det här steget tar bort hello virtuella datorer som har skapats under redundanstestningen.

Mer information finns i hello [testa redundans tooAzure](site-recovery-test-failover-to-azure.md) dokumentet.

## <a name="next-steps"></a>Nästa steg

När du få replikering och körs, när ett avbrott inträffar växlar du över tooAzure och virtuella Azure-datorer skapas från hello replikerade data. Du kan sedan komma åt arbetsbelastningar och appar i Azure, tills du växlar tillbaka tooyour primära platsen när den returnerar toonormal åtgärder.

- [Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur toorun dem.
- Om du migrerar datorer i stället för att replikera och misslyckas igen, [mer](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- När du replikerar fysiska datorer, kan du endast återställning tooa VMware-miljön. [Läs mer om återställning efter fel](site-recovery-failback-azure-to-vmware.md).

## <a name="third-party-software-notices-and-information"></a>Meddelanden om programvara från tredje part och information
Inte översätta eller lokalisera

hello programvara och inbyggd programvara som körs i hello Microsoft-produkt eller tjänst som baseras på eller innehåller material från hello projekt nedan (gemensamt kallade ”från tredje part kod”). Microsoft är inte hello ursprungliga författaren av hello kod från tredje part. hello ursprungliga upphovsrättsmeddelandet och licensen, enligt vilka Microsoft fått sådan kod från tredje part, är anges nedan.

hello informationen i avsnittet A gäller från tredje part kod komponenter från hello projekt som anges nedan. Sådana licenser och information tillhandahålls endast i informationssyfte. Den här koden från tredje part är att relicensed tooyou av Microsoft under Microsofts licensvillkoren för hello Microsoft-produkt eller tjänst.  

hello informationen i avsnittet B gäller kod från tredje part-komponenter som blir tillgängliga tooyou av Microsoft under hello ursprungliga licensvillkoren.

hello hela filen finns på hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft förbehåller sig alla rättigheter som inte uttryckligen beviljas, vare sig indirekt, via estoppel, eller på annat sätt.
