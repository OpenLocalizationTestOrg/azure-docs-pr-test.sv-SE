---
title: Replikera virtuella Hyper-V-datorer till Azure | Microsoft Docs
description: "Beskriver hur du samordnar replikering, redundans och återställning av lokala Hyper-V virtuella datorer till Azure"
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
ms.openlocfilehash: 8a2ea92759a777b1178fbfe8084a97eec931f709
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replikera virtuella Hyper-V-datorer (utan VMM) till Azure med Azure Site Recovery i Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klassiska Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Den här artikeln beskriver hur du replikera lokala Hyper-V virtuella datorer till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md) i Azure-portalen. I den här distributionen Hyper-V virtuella datorer hanteras inte av System Center Virtual Machine Manager (VMM).

När du har läst den här artikeln efter eventuella kommentarer längst ned eller tekniska frågor om den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Läs mer i [den här artikeln](site-recovery-migrate-to-azure.md) om att migrera datorer till Azure (utan återställning).



## <a name="deployment-steps"></a>Distributionssteg

Följ den här artikeln för att slutföra de här distributionsstegen:

1. [Lär dig mer](site-recovery-components.md#hyper-v-to-azure) om arkitekturen för den här distributionen. Dessutom kan du [lära dig mer om](site-recovery-hyper-v-azure-architecture.md) hur Hyper-V-replikering fungerar i Site Recovery.
2. Kontrollera krav och begränsningar.
3. Ställa in Azure-nätverk och lagringskonton.
4. Förbered Hyper-V-värdar.
5. Skapa ett Recovery Services-valv. Valvet innehåller konfigurationsinställningar och styr replikeringen.
6. Ange inställningar för datakällan. Skapa en Hyper-V-plats som innehåller Hyper-V-värdar och registrera platsen i valvet. Installera Azure Site Recovery-providern och Microsoft Recovery Services-agenten på Hyper-V-värdar.
7. Konfigurera inställningar för mål och replikering.
8. Aktivera replikering för virtuella Hyper-V-datorer.
9. Kör ett redundanstest för att kontrollera att allt fungerar som förväntat.



## <a name="prerequisites"></a>Krav


**Krav** | **Detaljer** |
--- | --- |
**Azure** | Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).
**Lokala servrar** | [Lär dig mer](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) om krav för lokal Hyper-V-värdar.
**Lokala virtuella Hyper-V-datorer** | Virtuella datorer du vill replikera bör köra ett[operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL: er** | Hyper-V-värdar behöver åtkomst till dessa webbadresser:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler ontrollerar du att de tillåter kommunikation till Azure.<br/></br> Tillåt [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-intervall för Azures datacenter) och HTTPS-port 443.<br/></br> Tillåt IP-adressintervall för Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och identitetshantering).



## <a name="prepare-for-deployment"></a>Förbereda för distribution

När du förbereder distributionen måste du:

1. [Skapa ett Azure nätverk](#set-up-an-azure-network) i som virtuella Azure-datorer kommer att finnas när de skapas efter växling vid fel.
2. [Skapa ett Azure-lagringskonto](#set-up-an-azure-storage-account) för replikerade data.
3. [Förbereda Hyper-V-värdar](#prepare-the-hyper-v-hosts) så de kan komma åt de nödvändiga URL: er.

### <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

Skapa ett Azure-nätverk. Du behöver detta så att den virtuella Azure-datorer skapas efter växling vid fel är ansluten till ett nätverk.

* Nätverket måste finnas på samma region som Recovery Services-valvet.
* Beroende på vilken resursmodell som du vill använda för redundansväxlade virtuella Azure-datorer kan du ställa in Azure-nätverket i [Resource Manager-läget](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), eller [klassiskt läge](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Vi rekommenderar att du konfigurerar ett nätverk innan du börjar. Om du inte gör det måste du göra det under distributionen av Site Recovery.
- Storage-konton som används av Site Recovery kan inte vara [flyttas](../azure-resource-manager/resource-group-move-resources.md) inom samma eller mellan olika, prenumerationer.


### <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

- Du behöver en standard/premium Azure storage-konto för att lagra data som replikeras till Azure. [Premiumlagring](../storage/storage-premium-storage.md) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg fördröjning till värd-i/o-intensiv arbetsbelastning.
- Om du vill använda ett Premium Storage-konto för replikerade data konfigurerar du ytterligare ett standardlagringskonto för att lagra replikeringsloggar som samlar in löpande ändringar i lokala data.
- Beroende på vilken resursmodell som du vill använda för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/storage-create-storage-account.md), eller [klassiskt läge](../storage/storage-create-storage-account-classic-portal.md).
- Vi rekommenderar att du ställer in ett storage-konto innan du börjar. Om du inte behöver göra det under distributionen av Site Recovery. Konton måste vara i samma region som Recovery Services-valvet.
- Du kan inte flytta storage-konton som används av Site Recovery i resursgrupper inom samma prenumeration, eller olika prenumerationer.


### <a name="prepare-the-hyper-v-hosts"></a>Förbereda Hyper-V-värdar

* Kontrollera att Hyper-V-värdar uppfyller de [krav](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager).
- Kontrollera att värdarna kan komma åt de nödvändiga URL: er.


## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på **New** > **Monitoring + Management** > **Backup and Site Recovery (OMS)**.  

    ![Nytt valv](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. I **Namn** anger du ett eget namn som identifierar valvet. Om du har mer än en prenumeration väljer du en av dem.

4. [Skapa en ny resursgrupp](../azure-resource-manager/resource-group-template-deploy-portal.md) eller välj en befintlig och ange en Azure-region. Datorer replikeras till den här regionen. Regioner som stöds finns i geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).

5. Om du vill att snabbt komma åt valvet från instrumentpanelen, klickar du på **fäst på instrumentpanelen**, och klicka sedan på **skapa**.

    ![Nytt valv](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Det nya valvet visas i den **instrumentpanelen** > **alla resurser** lista, och på primära **Recovery Services-valv** bladet.



## <a name="select-the-protection-goal"></a>Välj skyddsmål

Välj vad och vart du vill replikera.

1. I den **Recovery Services-valv**, Välj valvet.
2. Under **Komma igång** klickar du på **Webbplatsåterställning** > **Förbereda infrastrukturen** > **Skyddsmål**.

    ![Välja mål](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. I **skyddsmål**väljer **till Azure**, och välj **Ja, med Hyper-V**. Välj **nr** att bekräfta att du inte använder VMM. Klicka sedan på **OK**.

    ![Välja mål](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Konfigurera källmiljön

Konfigurera Hyper-V-platsen, installera Azure Site Recovery-providern och Azure Recovery Services-agenten på Hyper-V-värdar och registrera platsen i valvet.

1. I **Förbered infrastrukturen**, klickar du på **källa**. Om du vill lägga till en ny Hyper-V-plats som en behållare för Hyper-V-värdar eller kluster, klickar du på **+ Hyper-V-platsen**.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. I **skapa Hyper-V-platsen**, ange ett namn för platsen. Klicka sedan på **OK**. Nu väljer du den plats du har skapat och klicka på **+ Hyper-V Server** lägga till en server till platsen.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. I **Lägg till Server** > **servertyp**, kontrollera att **Hyper-V server** visas.

    - Kontrollera att Hyper-V-server som du vill lägga till som uppfyller den [krav](#on-premises-prerequisites), och komma åt de angivna URL: er.
    - Ladda ned installationsfilen för Azure Site Recovery-providern. Du kan köra den här filen för att installera providern och Recovery Services-agenten på varje Hyper-V-värd.

    ![Konfigurera källan](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Installera providern och agenten

1. Kör installationsfilen på varje värd som du lagt till Hyper-V-platsen för providern. Om du installerar på en Hyper-V-kluster, kan du köra installationsprogrammet på varje nod i klustret. Installera och registrera varje klusternod för Hyper-V säkerställer att virtuella datorer är skyddade, även om de migrera mellan noder.
2. I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.
3. I **Installation** accepterar du eller ändrar standardinstallationsplatsen för providern och klickar på **Installera**.
4. I **Valvinställningar**, klickar du på **Bläddra** att välja valvnyckelfilen som du hämtat. Ange Azure Site Recovery-prenumerationen och valvnamnet Hyper-V-platsen som Hyper-V-servern tillhör.

    ![Serverregistrering](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. I **proxyinställningar**, ange hur providern som körs på Hyper-V-värdar som ansluter till Azure Site Recovery via internet.

    * Om du vill att providern ska ansluta direkt väljer du **Anslut direkt till Azure Site Recovery utan proxyserver**.
    * Om den befintliga proxyservern kräver autentisering eller om du vill använda en anpassad proxyserver för Provider-anslutning väljer **Anslut till Azure Site Recovery via en proxyserver**.
    * Om du använder en proxyserver:
        - Ange adress, port och autentiseringsuppgifter
        - Kontrollera att URL: er som beskrivs i den [krav](#prerequisites) tillåts via proxy.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. När installationen är klar klickar du på **registrera** att registrera servern i valvet.

    ![Installationsplats](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. När registreringen är klar, hämtas metadata från Hyper-V-servern av Azure Site Recovery och servern visas i **Site Recovery-infrastruktur** > **Hyper-V-värdar**.


## <a name="set-up-the-target-environment"></a>Konfigurera målmiljön

Ange Azure storage-konto för replikering och Azure-nätverk som virtuella Azure-datorer ska ansluta till efter redundansväxlingen.

1. Klicka på **Förbered infrastruktur** > **mål**.
2. Välj prenumerationen och resursgrupp som du vill skapa den virtuella Azure-datorer efter redundans. Välj distributionsmodell som du vill använda i Azure (klassiska eller resource management) för de virtuella datorerna.

3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

    - Om du inte har ett lagringskonto, klickar du på **+ lagring** att skapa en infogad Resource Manager-baserade konto. Läs mer om [lagringskraven](site-recovery-prereq.md#azure-requirements).
    - Om du inte har ett Azure-nätverk, klickar du på **+ nätverk** att skapa en infogad Resource Manager-baserat nätverk.

    ![Lagring](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Konfigurera replikeringsinställningar

1. Skapa en ny replikeringsprincip genom att klicka på **Förbered infrastruktur** > **Replikeringsinställningar** > **+Skapa och koppla**.

    ![Nätverk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn.
3. I **Kopieringsfrekvens** anger du hur ofta du vill replikera förändringsdata (delta) efter den första replikeringen (med 30 sekunders mellanrum, var femte minut eller varje kvart).

    > [!NOTE]
    > En frekvens på 30 sekunder stöds inte när du replikerar till premiumlagring. Begränsningen bestäms av antalet ögonblicksbilder per blob (100) som stöds av premium-lagring. [Läs mer](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. I **kvarhållningstid för återställningspunkten**, ange i timmar hur lång kvarhållningsperiod är för varje återställningspunkt. Virtuella datorer kan återställas till valfri punkt inom ett fönster.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas.

    - Hyper-V använder två typer av ögonblicksbilder: en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en ögonblicksbild vid en viss tidpunkt av programdata på den virtuella datorn.
    - Programkonsekventa ögonblicksbilder använda VSS (Volume Shadow Copy Service) för att säkerställa att programmen är i ett konsekvent tillstånd när ögonblicksbilden tas.
    - Om du aktiverar programkonsekventa ögonblicksbilder så påverkar prestanda för program som körs på virtuella källdatorer. Kontrollera att värdet som du anger är mindre än antalet ytterligare återställningspunkter som du konfigurerar.

6. I **starttid för inledande replikering**, ange när börjar den inledande replikeringen. Replikeringen sker via Internetbandbredden så du kanske vill schemalägga den utanför kontorstid. Klicka sedan på **OK**.

    ![Replikeringsprincip](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

När du skapar en ny princip associeras den automatiskt med Hyper-V-platsen. Du kan associera en Hyper-V-plats (och de virtuella datorerna i den) med flera replikeringsprinciper i **replikering** > principnamn > **associera Hyper-V-platsen**.

## <a name="capacity-planning"></a>Kapacitetsplanering

Nu när du har grundläggande infrastrukturen konfigurerar du tycker om kapacitetsplanering och ta reda på om du behöver ytterligare resurser.

Site Recovery tillhandahåller ett kapacitetsplaneringsverktyg som hjälper dig att allokera rätt resurser för beräkning, nätverk och lagring. Du kan köra planeringsverktyget i snabbläge för att få uppskattningar baserat på ett medelvärde för antalet virtuella datorer, diskar och lagring eller i detaljerat läge med anpassade siffrorna på arbetsbelastningsnivå. Innan du börjar måste du:

* Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.
* Beräkna den dagliga (omsättningen) förändringstakten för dina replikerade data. Du kan använda den [Capacity Planner för Hyper-V-replikering](https://www.microsoft.com/download/details.aspx?id=39057) som hjälper dig att göra detta.

1. Klicka på **hämta** att ladda ned verktyget och sedan köra den. [Läs artikeln](site-recovery-capacity-planner.md) som medföljer verktyget.
2. När du är klar väljer du **Ja** i **Har du kört Capacity Planner**?

   ![Kapacitetsplanering](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

Lär dig mer om att [kontrollera nätverkets bandbredd](#network-bandwidth-considerations)



## <a name="enable-replication"></a>Aktivera replikering

Innan du börjar bör du kontrollera att ditt Azure-konto har de nödvändiga [behörigheterna](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) för att aktivera replikering för en ny virtuell dator till Azure.

Aktivera replikering för virtuella datorer på följande sätt:          

1. Klicka på **replikera program** > **källa**. När du har konfigurerat replikering för första gången, kan du klicka på **+ replikera** att aktivera replikering för ytterligare datorer.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. I **källa**, välj platsen för Hyper-V. Klicka sedan på **OK**.
3. I **mål**, Välj prenumerationen som valvet och växling vid fel modell du vill använda i Azure (klassiska eller resource management) efter växling vid fel.
4. Välj lagringskontot som du vill använda. Om du inte har ett konto som du vill använda, kan du [skapar du en](#set-up-an-azure-storage-account). Klicka sedan på **OK**.
5. Välj Azure-nätverk och undernät som virtuella Azure-datorer ska ansluta till när de skapas växling vid fel.

    - Markera för att tillämpa nätverksinställningarna på alla datorer som du aktiverar för replikering **konfigurera nu för valda datorer**.
    - Välj **Konfigurera senare** om du vill välja Azure-nätverket för varje dator.
    - Om du inte har ett nätverk som du vill använda, kan du [skapar du en](#set-up-an-azure-network). Välj ett undernät om det behövs. Klicka sedan på **OK**.

   ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. I **Virtual Machines** > **Välj virtuella datorer** klickar du på och väljer de datorer som du vill replikera. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. I **Egenskaper** > **Konfigurera egenskaper** väljer du operativsystemet för de valda virtuella datorerna och operativsystemdisken.
8. Kontrollera att namnet på den virtuella datorn i Azure (målnamnet) uppfyller [kraven för virtuella datorer i Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Som standard markeras alla diskar på den virtuella datorn för replikering men du kan rensa diskar för att exkludera dem.
    - Du kanske vill utesluta diskar för att minska bandbredden för replikering. Till exempel kanske du inte vill replikera diskar med tillfälliga data eller data som uppdateras varje gång en dator eller ett program startar om (till exempel pagefile.sys eller tempdb för Microsoft SQL Server). Du kan undanta en disk från replikering genom att avmarkera den.
    - Endast standarddiskar kan undantas. Du kan inte undanta OS-diskar.
    - Vi rekommenderar att du inte undantar dynamiska diskar. Site Recovery kan inte identifiera om en virtuell hårddisk är en standarddisk eller dynamisk disk på den virtuella gästdatorn. Om alla beroende dynamiska volymdiskar inte undantas identifieras skyddade dynamiska diskar som felaktiga på den virtuella datorn vid redundansväxling, och data på disken kan inte nås.
        - När replikering har aktiverats kan du inte lägga till eller ta bort diskar för replikering. Om du vill lägga till eller undanta en disk måste du inaktivera skyddet för den virtuella datorn och sedan aktivera det igen.
        - Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du till exempel växlar över tre diskar och skapar två direkt på den virtuella datorn i Azure så kommer endast de tre diskarna som växlades över att växlas tillbaka från Azure till Hyper-V. Du kan inte ta med diskar som har skapats manuellt i redundansväxlingar eller omvända replikeringar från Hyper-V till Azure.
        - Om du undantar en disk som behövs för att ett program ska fungera efter redundansväxlingen till Azure måste du skapa den manuellt i Azure så att det replikerade programmet kan köras. Du kan också integrera Azure Automation i en återställningsplan för att skapa disken under en redundansväxling av datorn.

10. Spara ändringarna genom att klicka på **OK**. Du kan ange ytterligare egenskaper senare.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. I **Replikeringsinställningar** > **Konfigurera replikeringsinställningar** väljer du den replikeringsprincip som du vill använda för de skyddade virtuella datorerna. Klicka sedan på **OK**. Du kan ändra replikeringsprincipen i **replikeringsprinciper** > principnamn > **redigera inställningar för**. De ändringar du gör används både för datorer som redan replikeras och för nya datorer.


   ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Du kan följa förloppet för jobbet **Aktivera skydd** under **Jobb** > **Site Recovery-jobb**. När jobbet **Slutför skydd** har körts är datorn redo för redundans.

### <a name="view-and-manage-vm-properties"></a>Visa och hantera egenskaper för virtuella datorer

Vi rekommenderar att du kontrollerar egenskaperna för källdatorn.

1. I **skyddade objekt**, klickar du på **replikerade objekt**, och välj datorn.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. I **Egenskaper** kan du visa information om replikering och redundans för den virtuella datorn.

    ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. I **Beräkning och nätverk** > **Beräkna egenskaper** kan du ange namnet och storleken på den virtuella Azure-datorn. Ändra namnet som ska uppfylla kraven för Azure om du behöver. Du kan också visa och ändra information om målnätverket, undernätet och IP-adressen som ska tilldelas den virtuella Azure-datorn. Tänk på följande:

   * Du kan ange IP-måladressen. Om du inte anger någon adress använder den redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under redundansväxlingen, misslyckas växlingen. Samma mål-IP-adress kan användas för att testa redundans om adressen är tillgänglig i nätverket för redundanstestet.
   * Antalet nätverkskort beror på storleken som du anger för den virtuella måldatorn enligt följande:

     * Om antalet nätverkskort på källdatorn är mindre än eller lika med antalet nätverkskort som tillåts för måldatorns storlek så kommer målet att ha samma antal kort som källan.
     * Om antalet nätverkskort för den virtuella källdatorn överskrider det tillåtna antalet för målstorleken så används den högsta målstorleken.
     * Om en källdator exempelvis har två nätverkskort och måldatorn stöder fyra så kommer måldatorn att ha två kort. Om källdatorn har två nätverkskort men målstorleken endast stöder ett så kommer måldatorn bara att ha ett kort.     
     * Om den virtuella datorn har flera nätverkskort ansluts alla till samma nätverk.
     * Om den virtuella datorn har flera nätverkskort så att den första som visas i listan blir den *standard* nätverkskort i den virtuella Azure-datorn.

     ![Aktivera replikering](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. I **diskar**, du kan se operativsystemet och datadiskar på den virtuella datorn som kommer att replikeras.

#### <a name="managed-disks"></a>Hanterade diskar

I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, du kan ange inställningen ”Använd hanterade diskar” till ”Ja” för den virtuella datorn om du vill bifoga hanterade diskar till din dator i samband med migrering till Azure. Hanterade diskar förenklar diskhantering för virtuella Azure IaaS-datorer genom att hantera de lagringskonton som är associerade med de virtuella diskarna. [Mer information om hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Hanterade diskar skapas och kopplas endast till den virtuella datorn vid en redundansväxling till Azure. När du aktiverar skydd fortsätter data från lokala datorer att replikeras till lagringskonton.
   Hanterade diskar kan endast skapas för virtuella datorer som distribueras med Resource manager-distributionsmodellen.

  > [!NOTE]
  > Återställning från Azure till lokala Hyper-V-miljön stöds inte för datorer med hanterade diskar. Ange endast ”Använd hanterade diskar” som ”Ja” om du planerar att migrera den här datorn till Azure.

   - När du anger ”Använd hanterade diskar” till ”Ja” kommer endast tillgänglighetsuppsättningar i resursgruppen med ”Använd hanterade diskar” inställd på ”Ja” vara tillgänglig för urvalet. Det beror på att virtuella datorer med hanterade diskar endast kan vara en del av tillgänglighetsuppsättningar med värdet ”Ja” som egenskap för ”Använd hanterade diskar”. Se till att du skapar tillgänglighetsuppsättningar med egenskapen ”Använd hanterade diskar” inställd i enlighet med hur du avser använda hanterade diskar vid redundansväxling. När du anger ”Använd hanterade diskar” som ”Nej” kommer endast tillgänglighetsuppsättningar i resursgruppen med ”Använd hanterade diskar” inställd på ”Nej” vara tillgänglig för urvalet. [Mer information om hanterade diskar och tillgänglighetsuppsättningar](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Om lagringskontot som används för replikering har krypterats med Storage Service-kryptering vid någon tidpunkt misslyckas skapandet av hanterade diskar vid redundansväxling. Du kan antingen ange ”Använd hanterade diskar” som ”Nej” och försöka upprepa redundansväxlingen eller inaktivera skyddet för den virtuella datorn och skydda det till ett lagringskonto som inte har krypterats av lagringstjänstens vid någon tidpunkt.
  > [Lär dig mer om Storage Service Encryption och hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-the-deployment"></a>Testa distributionen

Du kan testa distributionen genom att köra ett redundanstest för en enskild virtuell dator eller genom att köra en återställningsplan som innehåller en eller flera virtuella datorer.

### <a name="before-you-start"></a>Innan du börjar

 - Om du vill ansluta till virtuella Azure-datorer med RDP efter en redundansväxling kan du hitta mer information om att [förbereda en anslutning](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Om du vill testa fullständigt behöver du kopiera Active Directory och DNS i din testmiljö. [Läs mer](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Köra ett redundanstest

1. Att växla över en enskild dator i **replikerade objekt**, klicka på den virtuella datorn > **+ testa redundans** ikon.
2. Om du vill redundansväxla en återställningsplan går du till **Återställningsplaner**, högerklickar på planen > **Testa redundans**. Om du vill skapa en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).
3. I **Redundanstest**, Välj Azure-nätverk till vilka virtuella Azure-datorer ska ansluta till efter redundansväxlingen.
4. Starta redundansväxlingen genom att klicka på **OK**. Du kan följa förloppet genom att klicka på den virtuella datorn för att öppna dess egenskaper eller på den **Redundanstest** jobb i valvnamnet > **jobb** > **Site Recovery-jobb**.
5. När redundansväxlingen är klar bör du även kunna se Azure-replikdatorn på Azure-portalen > **Virtual Machines**. Kontrollera att den virtuella datorn har rätt storlek, att den är ansluten till rätt nätverk och att den körs.
6. Om du förberedde för anslutning efter redundansväxlingen bör du kunna ansluta till den virtuella Azure-datorn.
7. När du är klar klickar du på **Rensa redundanstest** i återställningsplanen. I **Kommentarer** skriver du och sparar eventuella observationer från redundanstestningen. När du gör det tas de virtuella datorerna som skapades under redundanstestningen bort.

Mer information finns i artikeln [Test failover to Azure](site-recovery-test-failover-to-azure.md) (Testa redundans till Azure).



## <a name="monitor-the-deployment"></a>Övervaka distributionen

Övervaka konfigurationsinställningarna, statusen och hälsan för distributionen av Site Recovery:

1. Klicka på valvnamnet för att få åtkomst till **Essentials**-instrumentpanelen. I den här instrumentpanelen kan du spåra Site Recovery jobb, replikeringsstatusen, återställningsplaner, Servertillstånd och händelser.  

    ![Essentials](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. I den **hälsa** kan du övervaka Platsservrar som upplever problem och händelser som skapats av Site Recovery under de senaste 24 timmarna. Du kan anpassa Essentials och visa de paneler och layouter som är mest användbara för dig, inklusive status för andra Site Recovery- och Backup-valv.
3. Du kan hantera och övervaka replikeringen på panelerna **Replikerade objekt**, **Återställningsplaner** och **Site Recovery-jobb**. Detaljer om jobb för mer information finns i **jobb** > **Site Recovery-jobb**.

## <a name="command-line-provider-and-agent-installation"></a>Providern och agenten kommandoradsinstallation

Azure Site Recovery-providern och agenten kan också installeras med följande kommandorad. Den här metoden kan användas för att installera providern på Server Core för Windows Server 2012 R2.

1. Ladda ned installationsfilen och registreringsnyckeln för providern till en mapp. Till exempel C:\ASR.
2. Extrahera installationsprogrammet för providern genom att köra dessa kommandon från en upphöjd kommandotolk:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Installera komponenterna genom att köra detta kommando:

            C:\ASR> setupdr.exe /i
4. Kör sedan följande kommandon för att registrera servern i valvet:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file>

<br/>
Där:

* **/ Credentials**: obligatorisk parameter som anger den plats där registreringsnyckelfilen finns  
* **/FriendlyName**: Obligatorisk parameter för namnet på Hyper-V-värdservern som visas på Azure Site Recovery-portalen.
* **/proxyAddress**: Valfri parameter som anger adressen till proxyservern.
* **/proxyport**: Valfri parameter som anger porten för proxyservern.
* **/ proxyusername**: valfri parameter som anger användarnamnet för proxyservern (Om proxyservern kräver autentisering).
* **/ proxypassword**: valfri parameter som anger lösenordet för att autentisera med proxyservern (Om proxyservern kräver autentisering).


## <a name="network-bandwidth-considerations"></a>Att tänka på när det gäller nätverksbandbredden
Du kan använda den [kapacitetsplaneringsverktyget för Hyper-V](site-recovery-capacity-planner.md) för att beräkna bandbredden som du behöver för replikering (inledande replikering och sedan delta). Om du vill styra bandbreddsanvändningen för en replikering kan du välja mellan några olika alternativ:

* **Begränsa bandbredden**: Hyper-V-trafik som replikeras till Azure går igenom en specifik Hyper-V-värd. Du kan begränsa bandbredden på värdservern.
* **Justera bandbredden**: Du kan påverka den bandbredd som används för replikering med hjälp av några registernycklar.

### <a name="throttle-bandwidth"></a>Begränsa bandbredden
1. Öppna snapin-modulen Microsoft Azure Backup MMC på Hyper-V-värdservern. Som standard finns det en genväg till Microsoft Azure Backup på skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klicka på **Ändra egenskaper** i snapin-modulen.
3. På fliken **Begränsning** väljer du **Aktivera användningsbegränsning för Internetbandbredd för säkerhetskopieringsåtgärder** och ange begränsningarna för arbetstid och övrig tid. Giltiga intervall är från 512 kbit/s till 102 Mbit/s.

    ![Begränsa bandbredden](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Du kan också ange begränsningar med hjälp av cmdleten [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx). Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

### <a name="influence-network-bandwidth"></a>Påverka nätverkets bandbredd
1. Gå till **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication** i registret.
   * Om du vill påverka bandbredd trafiken på en replikering disk ändrar du värdet i **UploadThreadsPerVM**, eller skapa nyckeln om den inte finns.
   * Ändra värdet för att påverka bandbredden för redundanstrafik från Azure **DownloadThreadsPerVM**.
2. Standardvärdet är 4. I ett ”överetablerat” nätverk bör du ändra registernycklarnas standardvärden. Det högsta antalet är 32. Övervaka trafiken för att optimera värdet.

## <a name="next-steps"></a>Nästa steg

När den inledande replikeringen är klar och du har testat distributionen kan du starta redundansväxlingar vid behov. [Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur du kör dem.
