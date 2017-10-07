---
title: aaaReplicate virtuella VMware-datorer tooAzure | Microsoft Docs
description: "Sammanfattar hello stegen för att replikera arbetsbelastningar som körs på virtuella VMware-datorer tooAzure lagring"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-overview
ms.openlocfilehash: f800e7d8a3b59a86809a995748eacf87630a1713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-tooazure-with-site-recovery"></a>Replikera VMware virtuella datorer tooAzure med Site Recovery

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Klassiska Azure](site-recovery-vmware-to-azure-classic.md)


Den här artikeln beskriver hur tooreplicate lokalt tooAzure för VMware-virtuella datorer, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Om du vill toomigrate virtuella VMware-datorer tooAzure (endast redundans), skrivskyddade [i den här artikeln](site-recovery-migrate-to-azure.md) toolearn mer.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="deployment-steps"></a>Distributionssteg

Här är vad du behöver toodo:

1. Kontrollera krav och begränsningar.
2. Ställa in Azure-nätverk och lagringskonton.
3. Förbereda hello lokal dator som du vill toodeploy som hello konfigurationsservern.
4. Förbered VMware-konton som toobe används för automatisk identifiering av virtuella datorer och eventuellt för push-installation av hello mobilitetstjänsten.
4. Skapa ett Recovery Services-valv. hello valvet innehåller konfigurationsinställningar och styr replikeringen.
5. Ange källa och mål replikering inställningar.
6. Distribuera hello mobilitetstjänsten på virtuella datorer du vill tooreplicate.
7. Aktivera replikering för hello virtuella datorer.
7. Kör ett test redundans toomake att allt fungerar som förväntat.

## <a name="prerequisites"></a>Krav

**Stöd för krav** | **Detaljer**
--- | ---
**Azure** | Lär dig mer om [krav för Azure](site-recovery-prereq.md#azure-requirements)
**Lokal konfigurationsserver** | Du behöver en VMware VM kör Windows Server 2012 R2 eller senare. Du konfigurerar den här servern under distributionen av Site Recovery.<br/><br/> Server- och huvudmålservern installeras som standard hello processen också på den här virtuella datorn. När du skalar upp, du kan behöva en separat processerver och hello har samma krav som hello konfigurationsservern.<br/><br/> Lär dig mer om komponenterna [här](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Lokal VMware-servrar** | En eller flera VMware vSphere-servrar, kör 6.0, 5.5, 5.1 med senaste uppdateringarna. Servrarna måste finnas i samma nätverk som konfigurationsservern hello (eller separat processerver) hello.<br/><br/> Vi rekommenderar en vCenter server toomanage-värdar som kör 6.0 eller 5.5 med hello senaste uppdateringarna. Funktioner som är tillgängliga i 5.5 stöds när du distribuerar version 6.0.
**Lokala virtuella datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). VM ska ha VMware-verktygen körs.
**URL: er** | hello konfigurationsservern behöver åtkomst toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).<br/><br/> Tillåt URL: en för hello MySQL hämtning: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitetstjänsten** | Installeras på varje replikerade virtuella datorn.




## <a name="limitations"></a>Begränsningar

**Begränsning** | **Detaljer**
--- | ---
**Azure** | Lagring och nätverk konton måste finnas i hello samma region som hello valvet<br/><br/> Om du använder ett premiumlagringskonto måste du också en standard lagra replikeringsloggar toostore för kontot<br/><br/> Du kan replikera toopremium konton i Central och södra Indien.
**Lokal konfigurationsserver** | VMware VM korttypen ska VMXNET3. Om det inte är [installera denna uppdatering](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 ska installeras.<br/><br> hello datorn får inte vara en domänkontrollant. hello datorn bör ha en statisk IP-adress.<br/><br/> hello värdnamn ska vara högst 15 tecken eller mindre, och operativsystemet ska vara på engelska.
**VMware** | Endast 5.5 funktioner som stöds i vCenter 6.0. Site Recovery har inte stöd för nya vCenter och vSphere 6.0 funktioner såsom mellan vCenter vMotion, virtuella volymerna och lagring DRS.
**Virtuella datorer** | Kontrollera [Virtuella Azure-begränsningar](site-recovery-prereq.md#azure-requirements)<br/><br/> Du kan replikera virtuella datorer med krypterade diskar eller virtuella datorer med UEFI/EFI-start.<br/><br> Delad disk inte stöds. Om Virtuella hello källdatorn har NIC-teamindelning kan konverteras tooa enkel NIC efter växling vid fel.<br/><br/> Om virtuella datorer har en iSCSI-disk, konverterar Site Recovery den tooa VHD-filen efter växling vid fel. Om hello iSCSI-målet kan nås av hello Azure VM, ansluter tooit och ser både den och hello VHD. Om det händer kan du koppla från hello iSCSI-mål.<br/><br/> Om du vill tooenable konsekvens för flera, vilket gör att virtuella datorer som kör samma arbetsbelastning toobe återställas hello peka tillsammans tooa konsekventa data, öppna porten 20004 på hello VM.<br/><br/> Windows måste installeras på hello C-enheten. hello OS-disk ska vara enkla och inte dynamiska. Hej datadisk kan vara dynamisk.<br/><br/> Linux/etc/hosts-filer på virtuella datorer bör innehålla poster som mappar hello lokal värd name tooIP adresser som är associerade med alla nätverkskort. Hej värdnamn, monteringspunkter, enhetsnamn, system-sökvägar och filnamn (/ etc; / usr) ska endast på engelska.<br/><br/> Vissa typer av [Linux lagring](site-recovery-support-matrix-to-azure.md#support-for-storage) stöds.<br/><br/>Skapa eller ange **disk.enableUUID=true** i hello VM-inställningar. Detta ger en konsekvent UUID toohello VMDK, så att den monterar korrekt och säkerställer att endast deltaändringar är överförda tillbaka tooon lokala under återställning efter fel utan fullständig replikering.

## <a name="set-up-azure"></a>Konfigurera Azure

1. [Skapa ett Azure nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.
    - Du kan ställa in ett nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.

2. Konfigurera en [Azure storage-konto](../storage/storage-create-storage-account.md#create-a-storage-account) för replikerade data.
    - hello-kontot kan vara standard eller [premium](../storage/storage-premium-storage.md).
    - Du kan konfigurera ett konto i Resource Manager eller i klassiskt läge.

3. [Förbereda ett konto](#prepare-for-automatic-discovery-and-push-installation) på hello vCenter-servern eller vSphere-värdar, så att Site Recovery kan automatiskt identifiera virtuella VMware-datorer.

## <a name="prepare-hello-configuration-server"></a>Förbereda hello konfigurationsservern

1. Installera Windows Server 2012 R2 eller senare på en VMware-VM.
2. Kontrollera hello VM har åtkomst toohello webbadresserna i [krav](#prerequisites).
3. Installera [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Förbereda för automatisk identifiering och push-installation

- **Förbereda ett konto för automatisk identifiering**: hello Site Recovery processervern upptäcker automatiskt virtuella datorer. toodo detta, Site Recovery behov autentiseringsuppgifter som har åtkomst till vCenter-servrar och vSphere ESXi-värdar.

    1. toouse ett särskilt konto skapar du en roll (på hello vCenter nivå med dessa [behörigheter](#vmware-account-permissions). Ge det ett namn som **Azure_Site_Recovery**.
    2. Sedan skapa en användare på hello vSphere-värd/vCenter-servern och tilldela hello rollen toohello användare. Du kan ange det här användarkontot under distributionen av Site Recovery.

- **Förbereda en konto toopush hello mobilitetstjänsten**: Om du vill toopush hello Mobility service tooVMs, du behöver ett konto som kan användas av hello processen server tooaccess hello VM. hello kontot används bara för hello push-installation. Du kan använda en domän eller lokalt konto:

    - För Windows, om du inte använder ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo, hello registrera under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten för hello **LocalAccountTokenFilterPolicy**, med värdet 1.
    - Om du vill tooadd hello registerposten för Windows från en CLI, skriver du:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - För Linux ska hello kontot vara rot på hello Linux källservern.

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-hello-protection-goal"></a>Välj hello skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. Klicka på **Recovery Services-valv** > valvet.
2. I hello resurs-menyn, klickar du på **Site Recovery** > **steg 1: Förbered infrastrukturen** > **skyddsmål**.

    ![Välja mål](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. I **skyddsmål**väljer **tooAzure** > **Ja, med VMware vSphere-Hypervisor**.

    ![Välja mål](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera virtuella datorer.

1. Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.
2. Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.

    ![Konfigurera källan](./media/site-recovery-vmware-to-azure/set-source1.png)
3. I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.
5. Hämta hello valvregistreringsnyckel. Du behöver detta när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

   ![Konfigurera källan](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Kör Site Recovery enhetlig installation

Hello följande innan du startar och kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern hello processervern och hello huvudmålservern.
    - Få en snabb överblick av video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Kontrollera att hello systemklockan är synkroniserad med i hello konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Den måste matcha. Installationen misslyckas om det är 15 minuter framför eller bakom.
    - Kör installationsprogrammet som en lokal administratör på hello konfigurationsservern VM.
    - Kontrollera att TLS 1.0 är aktiverat på hello VM.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).



### <a name="add-hello-account-for-automatic-discovery"></a>Lägg till hello konto för automatisk identifiering

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-toovmware-servers"></a>Anslut tooVMware servrar

Anslut toovSphere ESXi-värdar eller vCenter-servrar, toodiscover virtuella VMware-datorer.

- Om du lägger till hello vCenter-servern eller vSphere-värdar med ett konto utan administratörsbehörighet på servern hello måste hello kontot dessa behörigheter aktiveras:
    - Datacenter, datalagret, mapp, värd, nätverk, resurs, virtuella datorn vSphere distribuerade växel.
    - Hej vCenter-servern måste lagring vyer behörigheter.
- När du lägger till VMware-servrar kan det ta 15 minuter eller längre för dessa tooappear hello-portalen.
tooallow Azure Site Recovery toodiscover virtuella datorer som körs i din lokala miljö, behöver du tooconnect din VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery.

Välj **+ vCenter** toostart ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

Site Recovery ansluter tooVMware servrar med hjälp av hello angett inställningar och identifierar virtuella datorer.

## <a name="set-up-hello-target"></a>Ställ in hello mål

Innan du konfigurerar hello målmiljön kan du kontrollera att du har en [Azure storage-konto och nätverk](#set-up-azure)

1. Klicka på **Förbered infrastruktur** > **mål**, och välj hello Azure-prenumeration du vill toouse.
2. Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.
3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

   ![mål](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, toocreate en Resource Manager-konto eller nätverket infogad.

## <a name="set-up-replication-settings"></a>Konfigurera replikeringsinställningar

Få en snabb överblick av video innan du börjar:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klicka på toocreate en ny replikeringsprincip **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.
2. I **skapa replikeringsprincip**, ange ett principnamn.
3. I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen. Det här värdet anger hur ofta data återställningspunkter skapas. En avisering genereras om kontinuerlig replikering överskrider den gränsen.
4. I **kvarhållningstid för återställningspunkten**, ange hur länge (i timmar) hello kvarhållningsperiod är för varje återställningspunkt. Replikerade virtuella datorer kan återställas tooany punkt i ett fönster. Replikerade toopremium lagring och 72 timmar för standardlagring in too24 timmar kvarhållning har stöd för datorer.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas. Klicka på **OK** toocreate hello princip.

    ![Replikeringsprincip](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. När du skapar en ny princip associeras den automatiskt med hello konfigurationsservern. Som standard skapas automatiskt en matchande princip för återställning efter fel. Till exempel om hello replikeringsprincip är **rep princip** hello principer ska vara **rep-princip-återställning**. Den här principen används inte förrän du har initierat en återställning från Azure.  



## <a name="plan-capacity"></a>Planera kapacitet

1. Nu när du har grundläggande infrastrukturen konfigurerar du kan tänka kapacitetsplanering och ta reda på om du behöver ytterligare resurser. [Läs mer](site-recovery-plan-capacity-vmware.md).
2. När du är klar med kapacitetsplanering, Välj **Ja** i **har du planerat kapaciteten?**

   ![Kapacitetsplanering](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Förbereda virtuella datorer för replikering

Hej mobilitetstjänsten måste installeras på alla virtuella VMware-datorer som du vill tooreplicate. Du kan installera hello mobilitetstjänsten på ett antal olika sätt:

1. Installera med en push-installation från hello processervern. Tooprepare VMs toouse måste den här metoden.
2. Installera med distributionsverktyg, till exempel System Center Configuration Manager eller Azure automation DSC.
3.  Installera manuellt.

[Läs mer](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>Aktivera replikering

Innan du börjar:

- Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.
- När du lägger till eller ändra virtuella datorer, kan det ta upp too15 minuter eller längre för ändringar tootake effekt och för dem tooappear hello-portalen.
- Du kan kontrollera hello senast identifierade tid för virtuella datorer i **Konfigurationsservrar** > **senaste kontakt på**.
- tooadd virtuella datorer utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den), och klicka på **uppdatera**.
- Om en virtuell dator är förberedd för push-installation installerar hello mobilitetstjänsten automatiskt hello processervern när du aktiverar replikering.


### <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Alla diskar på en dator replikeras som standard. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb).

### <a name="replicate-vms"></a>Replikera virtuella datorer

Innan du börjar titta på en video Snabböversikt

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klicka på **Steg 2: Replikera program** > **Källa**.
2. I **källa**väljer hello konfigurationsservern.
3. I **datorn typen**väljer **virtuella datorer**.
4. I **vCenter/vSphere-Hypervisor**väljer hello vCenter-servern som hanterar hello vSphere-värd, eller hello värden.
5. Välj hello processervern. Om du inte skapat några ytterligare servrar blir hello konfigurationsservern. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. I **mål**hello prenumerationen och välj hello resursgrupp som du vill toocreate hello redundansväxlade virtuella datorer. Välj hello distributionsmodell som du vill använda toouse i Azure (klassiska eller resource management), för hello redundansväxlade virtuella datorer.


7. Välj hello Azure storage-konto som du vill använda toouse för replikering av data. Om du inte vill toouse ett konto som du redan har konfigurerat, kan du skapa en ny.

8. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas efter växling vid fel. Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare** tooselect hello Azure-nätverk per dator. Om du inte vill toouse ett befintligt nätverk kan skapa du en.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. I **egenskaper** > **konfigurera egenskaper**väljer hello-konto som ska användas av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.
11. Alla diskar replikeras som standard. Klicka på **alla diskar** och avmarkera alla diskar som du inte vill tooreplicate. Klicka sedan på **OK**. Du kan ange ytterligare egenskaper för Virtuella datorer senare.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello. Om du ändrar en princip kommer ändringarna att tillämpade tooreplicating datorn och toonew datorer.
12. Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen. Klicka sedan på **OK**. Tänk på följande:

    * Datorer i replikeringsgrupper replikeras tillsammans, och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.
    * Vi rekommenderar att du samla in virtuella datorer och fysiska servrar så att de speglar dina arbetsbelastningar. Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda och bör endast användas om datorer kör hello samma arbetsbelastning och du behöver enhetlighet.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Klicka på **Aktivera replikering**. Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.

### <a name="view-and-manage-vm-properties"></a>Visa och hantera egenskaper för virtuella datorer

Vi rekommenderar att du kontrollera hello VM egenskaper och gör eventuella ändringar som du behöver.

1. Klicka på **replikerade objekt** >, och välj hello-datorn. Hej **Essentials** bladet visar information om inställningar för datorer och status.
2. I **egenskaper**, kan du visa replikering och information om växling vid fel för hello VM.
3. I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, kan du ange hello Azure VM-namn och mål för storlek. Ändra hello namnet toocomply med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) om du behöver.
4. Ändra inställningar för hello målnätverket, undernätet och IP-adress som ska tilldelas toohello virtuella Azure-datorn:

   - Du kan ange IP-adressen för hello mål.

    - Om du inte anger någon adress använder hello redundansväxlade datorn DHCP.
    - Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte växling vid fel.
    - hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.

   - hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn:

     - Om hello antalet nätverkskort på källdatorn hello är hello samma som eller mindre än hello antalet nätverkskort som tillåts för hello måldatorn sedan hello målet att ha hello samma antal kort som hello källa.
     - Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello antal för hello Målstorlek hello högsta ska användas.
     - Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
   - Om hello virtuella datorn har flera nätverkskort ansluts alla toohello samma nätverk.
   - Om hello virtuella datorn har flera nätverkskort hello först visas i listan hello blir hello *standard* hello Azure virtuella nätverkskort.
4. I **diskar**, du kan se hello VM, operativsystemet och hello data diskar som ska replikeras.

#### <a name="managed-disks"></a>Hanterade diskar

I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, du kan ange ”Använd hanterade diskar” inställningen för ”Ja” för hello VM om du vill tooattach hanterade diskar tooyour datorn på tooAzure för växling vid fel. Hanterade diskar förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera hello storage-konton som är associerade med hello Virtuella diskar. [Mer information om hanterade diskar.](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview)

   - Hanterade diskar kan skapas och anslutna toohello virtuell dator endast på en tooAzure för växling vid fel. Om du aktiverar skydd fortsätter data från lokala datorer tooreplicate toostorage konton.  Hanterade diskar kan endast skapas för virtuella datorer som distribueras med hello Resource manager-distributionsmodellen.  

   - När du anger ”Använd hanterade diskar” för ”Ja” kan skulle endast tillgänglighetsuppsättningar i resursgruppen hello med ”Använd hanterade diskar” för ”Ja” vara tillgänglig för urvalet. Det beror på att virtuella datorer med hanterade diskar kan bara ingå i tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning för ”Ja”. Se till att du skapar tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning baserat på avsiktshantering toouse hanteras diskarna på redundanskluster.  På samma sätt när du anger ”Använd hanterade diskar” för ”Nej” kan skulle endast tillgänglighetsuppsättningar i resursgruppen för hello med ”Använd hanterade diskar” värdet för egenskapen för ”Nej” vara tillgänglig för urvalet. [Mer information om hanterade diskar och tillgänglighetsuppsättningar](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Om hello storage-konto som används för replikering har krypterats med Storage Service-kryptering när som helst i tid, misslyckas skapandet av hanterade diskar under växling vid fel. Du kan antingen ange ”Använd hanterade diskar” för ”Nej” och försök växling vid fel eller inaktivera skyddet för hello virtuella datorn och skydda den tooa storage-konto som inte var lagringstjänstens kryptering aktiverat när som helst i tid.
  > [Lär dig mer om Storage Service Encryption och hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="run-a-test-failover"></a>Köra ett redundanstest


När du har konfigurerat allt, kör du en testa redundans toomake att allt fungerar som förväntat. Få en snabb översikt för video innan du börjar
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. toofail över en enskild dator i **inställningar** > **replikerade objekt**, klicka på hello VM > **+ testa redundans** ikon.

    ![Testa redundans](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. toofail via en återställning planera i **inställningar** > **Återställningsplaner**, högerklicka på hello plan > **Redundanstestningen**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).  

1. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer ska ansluta till efter redundansväxlingen.

1. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **Redundanstest** jobb i valvnamnet > **inställningar** > **jobb**  >  **Site Recovery-jobb**.

1. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM är hello lämplig storlek som den är ansluten toohello lämpligt nätverk och att den körs.

1. Om du [förberedde för anslutning efter redundansväxlingen](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), bör du kunna tooconnect toohello Azure VM.

1. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.

[Lär dig mer](site-recovery-test-failover-to-azure.md) om redundanstestning.


## <a name="vmware-account-permissions"></a>Behörighet för VMware

Site Recovery behöver åtkomst tooVMware för hello processen server tooautomatically identifiera virtuella datorer och redundans och återställning av virtuella datorer.

- **Migrera**: Om du bara vill toomigrate virtuella VMware-datorer tooAzure utan att någonsin återställas dem. Du kan använda en VMware-konto med en skrivskyddad roll. Sådana rollen kan köra växling vid fel, men det går inte att stänga av skyddade källdatorer. Detta är inte nödvändigt för migrering.
- **Replikera eller återställa**: Om du vill toodeploy fullständig replikering (replikering, redundans, återställning) hello kontot måste vara kan toorun åtgärder, till exempel skapa och ta bort diskar, startar VMs osv.
- **Automatisk identifiering**: krävs minst ett konto för skrivskyddad.


**Aktivitet** | **Nödvändiga konto-rollen** | **Behörigheter** | **Detaljer**
--- | --- | --- | ---
**Processervern identifierar automatiskt virtuella VMware-datorer** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).
**Växling vid fel** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).<br/><br/> Användbart för migrering, men inte fullständig replikering, redundans och återställning efter fel.
**Redundans och återställning efter fel** | Vi rekommenderar att du skapar en roll (Azure_Site_Recovery) med behörigheter för hello som krävs och tilldela sedan hello rollen tooa VMware användare eller grupp | Data Center objektet –> sprida tooChild objekt, roll = Azure_Site_Recovery<br/><br/> DataStore -> allokera utrymme, bläddra datalagret, låg nivå filåtgärder, ta bort filen och uppdatera filer för virtuella datorer<br/><br/> Nätverk -> nätverk tilldela<br/><br/> Resurs -> Tilldela VM tooresource pool, migrera är avstängt VM, migrera driven på den virtuella datorn<br/><br/> Aktiviteter -> Skapa uppgift, uppdatera uppgift<br/><br/> Konfiguration av virtuell dator -><br/><br/> Virtual machine -> interagera -> fråga enhetsanslutning, konfigurera CD-skivor, konfigurera diskettenheter media, stänga av, slå på strömmen, installera för VMware-verktyg<br/><br/> Virtual machine -> Lager -> Skapa, registrera, avregistrera<br/><br/> Etablering av virtuell dator -> -> Tillåt virtuella hämtning, tillåter Överför filer för virtuella datorer<br/><br/> Virtual machine -> ögonblicksbilder -> Ta bort ögonblicksbilder | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).


## <a name="next-steps"></a>Nästa steg

När du få replikering och körs, när ett avbrott inträffar växlar du över tooAzure och virtuella Azure-datorer skapas från hello replikerade data. Du kan sedan komma åt arbetsbelastningar och appar i Azure, tills du växlar tillbaka tooyour primära platsen när den returnerar toonormal åtgärder.

- [Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.
- Om du migrerar datorer i stället för att replikera och misslyckas igen, [mer](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [Läs mer om återställning efter fel](site-recovery-failback-azure-to-vmware.md), toofail tillbaka och replikera virtuella datorer i Azure tillbaka toohello primära lokal plats.

## <a name="third-party-software-notices-and-information"></a>Meddelanden om programvara från tredje part och information
Inte översätta eller lokalisera

hello programvara och inbyggd programvara som körs i hello Microsoft-produkt eller tjänst som baseras på eller innehåller material från hello projekt nedan (gemensamt kallade ”kod från tredje part”).  Microsoft är hello inte ursprungliga författaren av hello kod från tredje part.  hello ursprungliga upphovsrättsmeddelandet och licensen, enligt vilka Microsoft fått sådan kod från tredje part, är anges nedan.

hello informationen i avsnittet A gäller kod från tredje part-komponenter från hello projekt som anges nedan. Sådana licenser och information tillhandahålls endast i informationssyfte.  Den här koden från tredje part är att relicensed tooyou av Microsoft under Microsofts licensvillkoren för hello Microsoft-produkt eller tjänst.  

hello informationen i avsnittet B gäller kod från tredje part-komponenter som blir tillgängliga tooyou av Microsoft under hello ursprungliga licensvillkoren.

hello hela filen finns på hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft förbehåller sig alla rättigheter som inte uttryckligen beviljas, vare sig indirekt, via estoppel eller på annat sätt.
