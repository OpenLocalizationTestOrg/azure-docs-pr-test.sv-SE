---
title: aaaReplicate Hyper-V VMs tooAzure | Microsoft Docs
description: "Beskriver hur tooorchestrate replikering, redundans och återställning av lokala Hyper-V VMs tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 04/05/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: hyper-v-site-walkthrough-overview
ms.openlocfilehash: 6fba41e43823fc57511d51ea2e09691159693982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure-using-azure-site-recovery-with-hello-azure-portal"></a>Replikera Hyper-V virtuella datorer (utan VMM) tooAzure med hjälp av Azure Site Recovery med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klassiska Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Den här artikeln beskriver hur tooreplicate lokala Hyper-V virtuella datorer tooAzure med [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen. I den här distributionen Hyper-V virtuella datorer hanteras inte av System Center Virtual Machine Manager (VMM).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Om du vill toomigrate datorer tooAzure (utan återställning), Läs mer i [i den här artikeln](site-recovery-migrate-to-azure.md).



## <a name="deployment-steps"></a>Distributionssteg

Hello artikel toocomplete gör så här distribution:

1. [Lär dig mer](site-recovery-components.md#hyper-v-to-azure) om hello-arkitekturen för den här distributionen. Dessutom kan du [lära dig mer om](site-recovery-hyper-v-azure-architecture.md) hur Hyper-V-replikering fungerar i Site Recovery.
2. Kontrollera krav och begränsningar.
3. Ställa in Azure-nätverk och lagringskonton.
4. Förbered Hyper-V-värdar.
5. Skapa ett Recovery Services-valv. hello valvet innehåller konfigurationsinställningar och styr replikeringen.
6. Ange inställningar för datakällan. Skapa en Hyper-V-plats som innehåller hello Hyper-V-värdar och registrera hello plats i hello-valvet. Installera hello Azure Site Recovery-providern och hello Microsoft Recovery Services-agenten på hello Hyper-V-värdar.
7. Konfigurera inställningar för mål och replikering.
8. Aktivera replikering för hello virtuella datorer.
9. Kör ett test redundans toomake att allt fungerar som förväntat.



## <a name="prerequisites"></a>Krav


**Krav** | **Detaljer** |
--- | --- |
**Azure** | Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).
**Lokala servrar** | [Lär dig mer](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) om kraven för hello lokala Hyper-V-värdar.
**Lokala virtuella Hyper-V-datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL: er** | Hyper-V-värdar behöver komma åt toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).



## <a name="prepare-for-deployment"></a>Förbereda för distribution

tooprepare för distribution måste du:

1. [Skapa ett Azure nätverk](#set-up-an-azure-network) i som virtuella Azure-datorer kommer att finnas när de skapas efter växling vid fel.
2. [Skapa ett Azure-lagringskonto](#set-up-an-azure-storage-account) för replikerade data.
3. [Förbereda hello Hyper-V-värdar](#prepare-the-hyper-v-hosts) tooensure som de kan komma åt hello krävs URL: er.

### <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

Skapa ett Azure-nätverk. Du behöver detta så att hello Azure virtuella datorer skapas efter växling vid fel är anslutna tooa nätverk.

* hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet.
* Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du ställa in hello Azure-nätverk i [Resource Manager-läget](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), eller [klassiskt läge](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Vi rekommenderar att du konfigurerar ett nätverk innan du börjar. Om du inte behöver du toodo under distributionen av Site Recovery.
- Storage-konton som används av Site Recovery kan inte vara [flyttas](../azure-resource-manager/resource-group-move-resources.md) inom hello samma, eller mellan olika, prenumerationer.


### <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

- Du behöver en standard/premium toohold data replikeras tooAzure Azure storage-konto. [Premiumlagring](../storage/storage-premium-storage.md) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg latens toohost-i/o-intensiv arbetsbelastning.
- Om du vill toouse en premium konto toostore replikerade data måste du även en standardlagring konto toostore replikeringsloggar som avbilda pågående ändringar tooon lokala data.
- Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/storage-create-storage-account.md), eller [klassiskt läge](../storage/storage-create-storage-account-classic-portal.md).
- Vi rekommenderar att du ställer in ett storage-konto innan du börjar. Om du inte behöver toodo under distributionen av Site Recovery. hello konton måste finnas i hello samma region som hello Recovery Services-valvet.
- Du kan inte flytta storage-konton används av Site Recovery över resursgrupper i hello samma prenumeration, eller mellan olika prenumerationer.


### <a name="prepare-hello-hyper-v-hosts"></a>Förbereda hello Hyper-V-värdar

* Kontrollera att hello Hyper-V-värdar uppfyller hello [krav](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager).
- Kontrollera att hello värdarna kan komma åt hello krävs URL: er.


## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **New** > **Monitoring + Management** > **Backup and Site Recovery (OMS)**.  

    ![Nytt valv](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. I **namn**, ange ett eget namn tooidentify hello valv. Om du har mer än en prenumeration väljer du en av dem.

4. [Skapa en ny resursgrupp](../azure-resource-manager/resource-group-template-deploy-portal.md) eller välj en befintlig och ange en Azure-region. Datorer blir replikerade toothis region. toocheck stöds regioner, se geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).

5. Om du vill tooquickly åtkomst hello valvet från hello instrumentpanelen, klickar du på **PIN-kod toodashboard**, och klicka sedan på **skapa**.

    ![Nytt valv](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

hello nya valvet visas i hello **instrumentpanelen** > **alla resurser** listan, och på hello huvudsakliga **Recovery Services-valv** bladet.



## <a name="select-hello-protection-goal"></a>Välj hello skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. I hello **Recovery Services-valv**väljer hello-valvet.
2. Under **Komma igång** klickar du på **Webbplatsåterställning** > **Förbereda infrastrukturen** > **Skyddsmål**.

    ![Välja mål](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. I **skyddsmål**väljer **tooAzure**, och välj **Ja, med Hyper-V**. Välj **nr** tooconfirm som du inte använder VMM. Klicka sedan på **OK**.

    ![Välja mål](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello Hyper-V-platsen, installera hello Azure Site Recovery-providern och hello Azure Recovery Services-agenten på Hyper-V-värdar och registrera hello-webbplats i hello-valvet.

1. I **Förbered infrastrukturen**, klickar du på **källa**. tooadd en ny Hyper-V-plats som en behållare för dina Hyper-V-värdar eller kluster, klicka på **+ Hyper-V-platsen**.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. I **skapa Hyper-V-platsen**, ange ett namn för hello-platsen. Klicka sedan på **OK**. Nu väljer hello plats du har skapat och klicka på **+ Hyper-V Server** tooadd en server toohello plats.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. I **Lägg till Server** > **servertyp**, kontrollera att **Hyper-V server** visas.

    - Kontrollera att hello Hyper-V-server som du vill tooadd uppfyller hello [krav](#on-premises-prerequisites), och är kan tooaccess hello angivna URL: er.
    - Hämta installationsfilen för hello Azure Site Recovery-providern. Du kör den här filen tooinstall hello providern och hello Recovery Services-agenten på varje Hyper-V-värd.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Installera hello Provider och agent

1. Kör installationsfilen för hello-providern på varje värd du lagt till toohello Hyper-V-platsen. Om du installerar på en Hyper-V-kluster, kan du köra installationsprogrammet på varje nod i klustret. Installera och registrera varje klusternod för Hyper-V säkerställer att virtuella datorer är skyddade, även om de migrera mellan noder.
2. I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.
3. I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.
4. I **Valvinställningar**, klickar du på **Bläddra** tooselect hello valvnyckelfilen som du hämtat. Ange hello Azure Site Recovery-prenumerationen, hello valvnamnet och hello Hyper-V platsservern toowhich hello Hyper-V tillhör.

    ![Serverregistrering](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. I **proxyinställningar**, ange hur providern som körs på Hyper-V-värdar som ansluter tooAzure Site Recovery via hello hello internet.

    * Om du vill att hello providern tooconnect direkt väljer **ansluter direkt tooAzure Site Recovery utan en proxy**.
    * Om den befintliga proxyservern kräver autentisering eller om du vill använda toouse en anpassad proxyserver för hello leverantörsanslutning väljer **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.
    * Om du använder en proxyserver:
        - Ange hello-adress, port och autentiseringsuppgifter
        - Kontrollera att hello webbadresser som beskrivs i hello [krav](#prerequisites) tillåts via hello proxy.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.

    ![Installationsplats](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. När registreringen är klar, hämtas metadata från hello Hyper-V-servern av Azure Site Recovery och hello server visas i **Site Recovery-infrastruktur** > **Hyper-V-värdar**.


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Ange hello Azure storage-konto för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.

1. Klicka på **Förbered infrastruktur** > **mål**.
2. Välj hello prenumeration och hello resursgrupp som du vill toocreate hello virtuella Azure-datorer efter redundans. Välj hello distribution modell som du vill toouse i Azure (klassiska eller resource management) för hello virtuella datorer.

3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

    - Om du inte har ett lagringskonto, klickar du på **+ lagring** toocreate en Resource Manager-baserade konto infogad. Läs mer om [lagringskraven](site-recovery-prereq.md#azure-requirements).
    - Om du inte har ett Azure-nätverk, klickar du på **+ nätverk** toocreate en infogad Resource Manager-baserat nätverk.

    ![Lagring](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Konfigurera replikeringsinställningar

1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn.
3. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).

    > [!NOTE]
    > En 30 andra frekvens stöds inte vid replikering av toopremium lagring. hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring. [Läs mer](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. I **kvarhållningstid för återställningspunkten**, ange i hur länge timmar hello kvarhållningsperiod är för varje återställningspunkt. Virtuella datorer kan återställas tooany punkt inom en period.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas.

    - Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn.
    - Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas.
    - Om du aktiverar programkonsekventa ögonblicksbilder så påverkar hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.

6. I **starttid för inledande replikering**, ange när toostart hello inledande replikering. hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid. Klicka sedan på **OK**.

    ![Replikeringsprincip](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

När du skapar en ny princip associeras den automatiskt med hello Hyper-V-platsen. Du kan associera en Hyper-V-platsen (och hello virtuella datorer i den) med flera replikeringsprinciper i **replikering** > principnamn > **associera Hyper-V-platsen**.

## <a name="capacity-planning"></a>Kapacitetsplanering

Nu när du har grundläggande infrastrukturen konfigurerar du tycker om kapacitetsplanering och ta reda på om du behöver ytterligare resurser.

Site Recovery tillhandahåller en kapacitet planner toohelp du allokera hello rätt resurser för beräkning, nätverk och lagring. Du kan köra hello planner i snabbläge för att få uppskattningar baserat på ett medelvärde för antalet virtuella datorer, diskar och lagring eller i detaljerat läge med anpassade nummer på hello arbetsbelastningsnivå. Innan du börjar måste du:

* Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.
* Beräkna hello dagliga (omsättningen) förändringstakten för dina replikerade data. Du kan använda hello [Capacity Planner för Hyper-V-replikering](https://www.microsoft.com/download/details.aspx?id=39057) toohelp du göra detta.

1. Klicka på **hämta** toodownload hello verktyget och sedan köra den. [Läs hello artikel](site-recovery-capacity-planner.md) som medföljer hello-verktyget.
2. När du är klar väljer du **Ja** i **har du kört hello Capacity Planner**?

   ![Kapacitetsplanering](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

Lär dig mer om att [kontrollera nätverkets bandbredd](#network-bandwidth-considerations)



## <a name="enable-replication"></a>Aktivera replikering

Innan du börjar bör du kontrollera att ditt Azure-konto har hello krävs [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.

Aktivera replikering för virtuella datorer på följande sätt:          

1. Klicka på **replikera program** > **källa**. När du har konfigurerat replikering för hello första gången du klickar på **+ replikera** tooenable replikering för ytterligare datorer.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. I **källa**väljer hello Hyper-V-platsen. Klicka sedan på **OK**.
3. I **mål**hello valvet prenumerationen och välj hello failover-modell som du vill ha toouse i Azure (klassiska eller resource management) efter växling vid fel.
4. Välj hello storage-konto som du vill toouse. Om du inte har ett konto som du vill toouse, kan du [skapar du en](#set-up-an-azure-storage-account). Klicka sedan på **OK**.
5. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas växling vid fel.

    - tooapply hello inställningar tooall datorer aktiveras för replikering, Välj **konfigurera nu för valda datorer**.
    - Välj **konfigurera senare** tooselect hello Azure-nätverk per dator.
    - Om du inte har ett nätverk som du vill toouse, kan du [skapar du en](#set-up-an-azure-network). Välj ett undernät om det behövs. Klicka sedan på **OK**.

   ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. I **egenskaper** > **konfigurera egenskaper**hello operativsystemet för hello valda virtuella datorer och välj hello OS-disken.
8. Kontrollera att hello Azure VM namn (mål) uppfyller [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Som standard markeras alla hello diskar på hello VM för replikering, men du kan rensa diskar tooexclude dem.
    - Du kanske vill tooexclude diskar tooreduce replikering bandbredd. Exempelvis kan du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller appar startas om (till exempel pagefile.sys eller Microsoft SQL Server tempdb). Du kan utesluta hello disk från replikering av unselecting hello-disk.
    - Endast standarddiskar kan undantas. Du kan inte undanta OS-diskar.
    - Vi rekommenderar att du inte undantar dynamiska diskar. Site Recovery kan inte identifiera om en virtuell hårddisk är en standarddisk eller dynamisk disk på den virtuella gästdatorn. Om alla diskar för beroende dynamisk volym inte uteslutas visar hello skyddade dynamisk disk som en disk som misslyckats när hello Virtuella växlar och hello data på disken inte är tillgängliga.
        - När replikering har aktiverats kan du inte lägga till eller ta bort diskar för replikering. Om du vill tooadd eller utesluta en disk, du behöver toodisable skydd för hello VM och aktivera det igen.
        - Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du misslyckas under tre diskar och skapa två direkt i Azure VM misslyckades exempelvis endast tre diskar för hello som har redundansväxling tillbaka från Azure tooHyper-V. Du kan inte innehålla diskar som skapats manuellt i återställning efter fel, eller i omvända replikeringen från Hyper-V tooAzure.
        - Om du undantar en disk som behövs för ett program toooperate efter redundans tooAzure måste toocreate det manuellt i Azure, så att hello replikeras program kan köras. Alternativt kan du integrera Azure automation i en återställningsplan, toocreate hello disken under växling vid fel för hello-datorn.

10. Klicka på **OK** toosave ändringar. Du kan ange ytterligare egenskaper senare.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, Välj hello replikeringsprincip som du vill tooapply för hello skyddade virtuella datorer. Klicka sedan på **OK**. Du kan ändra hello replikeringsprincip i **replikeringsprinciper** > principnamn > **redigera inställningar för**. De ändringar du gör används både för datorer som redan replikeras och för nya datorer.


   ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Du kan följa förloppet för hello **Aktivera skydd** jobb **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.

### <a name="view-and-manage-vm-properties"></a>Visa och hantera egenskaper för virtuella datorer

Vi rekommenderar att du kontrollerar hello egenskaper för hello källdatorn.

1. I **skyddade objekt**, klickar du på **replikerade objekt**, och välj hello-datorn.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. I **egenskaper**, kan du visa replikering och information om växling vid fel för hello VM.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, kan du ange hello Azure VM-namn och mål för storlek. Ändra hello namnet toocomply med krav för Azure om du behöver. Du kan också visa och ändra information om hello målnätverket, undernätet och IP-adress som ska tilldelas toohello Azure VM. Observera följande hello:

   * Du kan ange IP-adressen för hello mål. Om du inte anger någon adress använder hello redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under redundansväxlingen, misslyckas hello växling vid fel. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.
   * hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:

     * Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
     * Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
     * Till exempel om en källdator har två nätverkskort och hello måldatorn stöder fyra hello måldatorn att ha två kort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
     * Om hello VM har flera nätverkskort ansluts alla toohello samma nätverk.
     * Om hello virtuella datorn har flera nätverkskort hello först visas i listan hello blir hello *standard* hello Azure virtuella nätverkskort.

     ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. I **diskar**kan du visa hello operativsystem och datadiskar på hello virtuell dator som kommer att replikeras.

#### <a name="managed-disks"></a>Hanterade diskar

I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, du kan ange ”Använd hanterade diskar” inställningen för ”Ja” för hello VM om du vill tooattach hanterade diskar tooyour datorn på tooAzure för migrering. Hanterade diskar förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera hello storage-konton som är associerade med hello Virtuella diskar. [Mer information om hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Hanterade diskar kan skapas och anslutna toohello virtuell dator endast på en tooAzure för växling vid fel. Om du aktiverar skydd fortsätter data från lokala datorer tooreplicate toostorage konton.
   Hanterade diskar kan endast skapas för virtuella datorer som distribueras med hello Resource manager-distributionsmodellen.

  > [!NOTE]
  > Återställning från Azure tooon lokala Hyper-V-miljön stöds inte för datorer med hanterade diskar. Ange ”Använd hanterade diskar” för ”Ja” bara om du tänker toomigrate tooAzure för den här datorn.

   - När du anger ”Använd hanterade diskar” för ”Ja” kan skulle endast tillgänglighetsuppsättningar i resursgruppen hello med ”Använd hanterade diskar” för ”Ja” vara tillgänglig för urvalet. Det beror på att virtuella datorer med hanterade diskar kan bara ingå i tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning för ”Ja”. Se till att du skapar tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning baserat på avsiktshantering toouse hanteras diskarna på redundanskluster. På samma sätt när du anger ”Använd hanterade diskar” för ”Nej” kan skulle endast tillgänglighetsuppsättningar i resursgruppen för hello med ”Använd hanterade diskar” värdet för egenskapen för ”Nej” vara tillgänglig för urvalet. [Mer information om hanterade diskar och tillgänglighetsuppsättningar](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Om hello storage-konto som används för replikering har krypterats med Storage Service-kryptering när som helst i tid, misslyckas skapandet av hanterade diskar under växling vid fel. Du kan antingen ange ”Använd hanterade diskar” för ”Nej” och försök växling vid fel eller inaktivera skyddet för hello virtuella datorn och skydda den tooa storage-konto som inte var lagringstjänstens kryptering aktiverat när som helst i tid.
  > [Lär dig mer om Storage Service Encryption och hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-hello-deployment"></a>Testa hello-distribution

Du kan köra ett redundanstest för en enskild virtuell dator eller en återställningsplan som innehåller en eller flera virtuella datorer tootest hello-distribution.

### <a name="before-you-start"></a>Innan du börjar

 - Om du vill tooconnect tooAzure virtuella datorer med RDP efter en växling vid fel, Lär dig mer om [förbereder tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - toofully test som du behöver toocopy Active Directory och DNS i din testmiljö. [Läs mer](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Köra ett redundanstest

1. toofail över en enskild dator i **replikerade objekt**, klicka på hello VM > **+ testa redundans** ikon.
2. toofail över en återställning planera i **Återställningsplaner**, högerklicka på hello plan > **Redundanstest**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).
3. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer ska ansluta till efter redundansväxlingen.
4. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **Redundanstest** jobb i valvnamnet > **jobb** > **Site Recovery-jobb**.
5. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM är hello lämplig storlek som den är ansluten toohello lämpligt nätverk och att den körs.
6. Om du har förberett för anslutningar efter växling vid fel, bör du kunna tooconnect toohello Azure VM.
7. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar** spela in och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.

Mer information finns i hello [testa redundans tooAzure](site-recovery-test-failover-to-azure.md) artikel.



## <a name="monitor-hello-deployment"></a>Övervakaren hello distribution

Övervaka hello konfigurationsinställningarna, statusen och hälsan för distributionen av Site Recovery:

1. Klicka på hello valvet namn tooaccess hello **Essentials** instrumentpanelen. I den här instrumentpanelen kan du spåra Site Recovery jobb, replikeringsstatusen, återställningsplaner, Servertillstånd och händelser.  

    ![Essentials](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. I hello **hälsa** kan du övervaka Platsservrar som har problem och hello händelser som skapats av Site Recovery i hello senaste 24 timmarna. Du kan anpassa Essentials tooshow hello paneler och layouter som är mest användbara tooyou, inklusive hello status för andra Site Recovery och säkerhetskopieringsvalv.
3. Du kan hantera och övervaka replikeringen på hello **replikerade objekt**, **Återställningsplaner**, och **Site Recovery-jobb** paneler. Detaljer om jobb för mer information finns i **jobb** > **Site Recovery-jobb**.

## <a name="command-line-provider-and-agent-installation"></a>Providern och agenten kommandoradsinstallation

hello Azure Site Recovery-providern och agenten kan också installeras med följande kommandorad hello. Den här metoden kan vara används tooinstall hello-providern på en Server Core för Windows Server 2012 R2.

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Kör installationsprogrammet för dessa kommandon tooextract hello-providern från en upphöjd kommandotolk:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Kör det här kommandot tooinstall hello komponenter:

            C:\ASR> setupdr.exe /i
4. Kör dessa kommandon tooregister hello servern i valvet hello:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file>

<br/>
Där:

* **/ Credentials**: obligatorisk parameter som anger hello var i vilka hello registreringsnyckelfilen finns  
* **/ FriendlyName**: obligatorisk parameter för hello namnet på hello Hyper-V-värdservern som visas i hello Azure Site Recovery-portalen.
* **/ proxyaddress**: valfri parameter som anger hello proxyserver hello-adress.
* **/ Proxyport** : valfri parameter som anger hello port hello proxyserver.
* **/ proxyusername**: valfri parameter som anger hello proxyserverns användarnamn (Om proxyservern kräver autentisering).
* **/ proxypassword**: valfri parameter som anger hello lösenord för att autentisera med proxyservern hello (Om proxyservern kräver autentisering).


## <a name="network-bandwidth-considerations"></a>Att tänka på när det gäller nätverksbandbredden
Du kan använda hello [kapacitetsplaneringsverktyget för Hyper-V](site-recovery-capacity-planner.md) toocalculate hello bandbredd som du behöver för replikering (inledande replikering och sedan delta). toocontrol hello belopp av användningen av nätverksbandbredd för replikering har du några alternativ:

* **Begränsa bandbredden**: Hyper-V-trafik som replikeras tooAzure går igenom en specifik Hyper-V-värd. Du kan begränsa bandbredden på värdservern för hello.
* **Justera bandbredden**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar.

### <a name="throttle-bandwidth"></a>Begränsa bandbredden
1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värdservern. Som standard är en genväg till Microsoft Azure Backup finns på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello klickar du på **ändra egenskaper för**.
3. På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**, och ange hello gränserna för arbete och fritid timmar. Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.

    ![Begränsa bandbredden](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning. Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

### <a name="influence-network-bandwidth"></a>Påverka nätverkets bandbredd
1. I registret hello navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello bandbredd trafik på en replikering disk, ändra hello värdet hello **UploadThreadsPerVM**, eller skapa hello nyckeln om den inte finns.
   * tooinfluence hello bandbredd för redundanstrafik från Azure, ändra värdet för hello **DownloadThreadsPerVM**.
2. hello standardvärdet är 4. I ett ”överetablerat” nätverk bör registernycklarna ändras från hello standardvärden. hello maximala är 32. Övervaka trafik toooptimize hello värde.

## <a name="next-steps"></a>Nästa steg

När den inledande replikeringen är klar och du har testat hello distribution, kan du anropa redundans som hello behov. [Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur toorun dem.
