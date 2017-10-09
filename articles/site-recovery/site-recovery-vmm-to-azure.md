---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln tooAzure | Microsoft Docs
description: "Samordna replikering, redundans och återställning av Hyper-V-datorer som hanteras i System Center VMM-moln tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 84182fe4b63862ac7643208a22f236a7515a25a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med Site Recovery i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klassiska Azure](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell – Klassisk](site-recovery-deploy-with-powershell.md)


Den här artikeln beskriver hur tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center VMM-moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Om du vill toomigrate datorer tooAzure (utan återställning), Läs mer i [i den här artikeln](site-recovery-migrate-to-azure.md).


## <a name="deployment-steps"></a>Distributionssteg

Hello artikel toocomplete gör så här distribution:


1. [Lär dig mer](site-recovery-components.md) om hello-arkitekturen för den här distributionen. Dessutom kan du [lära dig mer om](site-recovery-hyper-v-azure-architecture.md) hur Hyper-V-replikering fungerar i Site Recovery.
2. Kontrollera krav och begränsningar.
3. Ställa in Azure-nätverk och lagringskonton.
4. Förbered hello lokal VMM-servern och Hyper-V-värdar.
5. Skapa ett Recovery Services-valv. hello valvet innehåller konfigurationsinställningar och styr replikeringen.
6. Ange inställningar för datakällan. Registrera hello VMM-servern i valvet hello. Installera hello Azure Site Recovery Provider på hello VMM server installation hello Microsoft Recovery Services-agenten på Hyper-V-värdar.
7. Konfigurera inställningar för mål och replikering.
8. Aktivera replikering för hello virtuella datorer.
9. Kör ett test redundans toomake att allt fungerar som förväntat.



## <a name="prerequisites"></a>Krav


**Stöd för krav** | **Detaljer**
--- | ---
**Azure** | Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).
**Lokala servrar** | [Lär dig mer](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) om krav för hello lokal VMM-servern och Hyper-V-värdar.
**Lokala virtuella Hyper-V-datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL: er** | hello VMM-servern måste komma åt toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).


## <a name="prepare-for-deployment"></a>Förbereda för distribution
tooprepare för distribution måste du:

1. [Skapa ett Azure-nätverk](#set-up-an-azure-network) som de virtuella Azure-datorerna ska anslutas till efter en redundansväxling.
2. [Skapa ett Azure-lagringskonto](#set-up-an-azure-storage-account) för replikerade data.
3. [Förbereda VMM-servern för hello](#prepare-the-vmm-server) för distributionen av Site Recovery.
4. Förbereda för nätverksmappning. Konfigurera nätverk så att du kan konfigurera nätverksmappning under Site Recovery-distributionen.

### <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk
Du behöver ett Azure-nätverk toowhich virtuella Azure-datorer skapas efter redundans ansluter.

* hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet.
* Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du ställa in hello Azure-nätverk i [Resource Manager-läget](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) eller [klassiskt läge](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Vi rekommenderar att du konfigurerar ett nätverk innan du börjar. Om du inte behöver du toodo under distributionen av Site Recovery.
Azure nätverk som används av Site Recovery kan inte vara [flyttas](../azure-resource-manager/resource-group-move-resources.md) inom hello samma, eller mellan olika, prenumerationer.

### <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto
* Du behöver en standard/premium toohold data replikeras tooAzure Azure storage-konto. [Premiumlagring](../storage/common/storage-premium-storage.md) används för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg latens toohost-i/o-intensiv arbetsbelastning. Om du vill toouse en premium konto toostore replikerade data måste du även en standardlagring konto toostore replikeringsloggar som avbilda pågående ändringar tooon lokala data. hello-kontot måste vara i hello samma region som hello Recovery Services-valvet.
* Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/common/storage-create-storage-account.md) eller [klassiskt läge](../storage/common/storage-create-storage-account.md).
* Vi rekommenderar att du skapar ett konto innan du börjar. Om du inte behöver du toodo under distributionen av Site Recovery.
- Observera att storage-konton som används av Site Recovery inte kan vara [flyttas](../azure-resource-manager/resource-group-move-resources.md) inom hello samma, eller mellan olika, prenumerationer.

### <a name="prepare-hello-vmm-server"></a>Förbereda hello VMM-servern
* Kontrollera att den hello VMM-servern uppfyller hello [krav](#prerequisites).
* Under distributionen av Site Recovery kan du ange att alla moln på VMM-servern ska vara tillgängliga i hello Azure-portalen. Du kan aktivera hello molnet i VMM-Administratörskonsol hello inställningen om du bara vill att specifika moln tooappear hello-portalen.

### <a name="prepare-for-network-mapping"></a>Förbereda för nätverksmappning
Du måste konfigurera nätverksmappning under distributionen av Site Recovery. Nätverksmappningen mappar mellan VMM VM-nätverk och Azure-målnätverken, tooenable hello följande:

* Datorer som växlar över på hello samma nätverk kan ansluta tooeach andra, även om de inte redundansväxlas i hello samma sätt eller hello samma återställningsplan.
* Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan virtuella Azure-datorer ansluta tooon lokala virtuella datorer.
* tooset nätverksmappning, här är vad du behöver:

  * Se till att virtuella datorer på hello källan Hyper-V-värdservern är anslutna tooa VMM VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
  * Ett Azure-nätverk så som det beskrivs [ovan](#set-up-an-azure-network)

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **New** > **Monitoring + Management** > **Backup and Site Recovery (OMS)**.

    ![Nytt valv](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. I **namn**, ange ett eget namn tooidentify hello valv. Om du har mer än en prenumeration väljer du en av dem.
4. [Skapa en resursgrupp](../azure-resource-manager/resource-group-template-deploy-portal.md) eller välj en befintlig. Ange en Azure-region. Datorer blir replikerade toothis region. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Om du vill tooquickly åtkomst hello valvet från hello instrumentpanelen, klickar du på **PIN-kod toodashboard** > **skapa valv**.

    ![Nytt valv](./media/site-recovery-vmm-to-azure/new-vault.png)

hello nya valvet visas på hello **instrumentpanelen** > **alla resurser**, och på hello huvudsakliga **Recovery Services-valv** bladet.


## <a name="select-hello-protection-goal"></a>Välj hello skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. I **Recovery Services-valv**väljer hello-valvet.
2. Under **Komma igång** klickar du på **Webbplatsåterställning** > **Förbereda infrastrukturen** > **Skyddsmål**.

    ![Välja mål](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. I **skyddsmål** Välj **tooAzure**, och välj **Ja, med Hyper-V**. Välj **Ja** tooconfirm som du använder VMM toomanage Hyper-V-värdar och hello återställningsplatsen. Klicka sedan på **OK**.

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Installera hello Azure Site Recovery Provider på hello VMM-servern och registrera hello servern i hello valv. Installera hello Azure Recovery Services-agenten på Hyper-V-värdar.

1. Klicka på **Förbereda infrastrukturen** > **Källa**.

    ![Konfigurera källan](./media/site-recovery-vmm-to-azure/set-source1.png)

2. I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.

    ![Konfigurera källan](./media/site-recovery-vmm-to-azure/set-source2.png)

3. I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav och URL: en krav för](#prerequisites).
4. Hämta installationsfilen för hello Azure Site Recovery-providern.
5. Hämta hello registreringsnyckel. Du behöver den när du kör installationsprogrammet. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-hello-provider-on-hello-vmm-server"></a>Installera hello providern på hello VMM-servern

1. Kör installationsfilen för hello-providern på hello VMM-servern.
2. I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.
3. I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.

    ![Installationsplats](./media/site-recovery-vmm-to-azure/provider2.png)
4. När installationen är klar klickar du på **registrera** tooregister hello VMM-servern i hello-valvet.
5. I hello **Valvinställningar** klickar du på **Bläddra** tooselect hello valvnyckelfilen. Ange hello Azure Site Recovery-prenumerationen och valvnamnet hello.

    ![Serverregistrering](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. I **Internetanslutning**, ange hur providern som körs på hello VMM-servern ansluter tooSite Recovery via hello hello internet.

   * Om du vill hello providern tooconnect direkt markerar **ansluter direkt tooAzure Site Recovery utan en proxy**.
   * Om den befintliga proxyservern kräver autentisering eller om du vill toouse en anpassad proxyserver, Välj **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.
   * Om du använder en anpassad proxyserver, ange hello-adress, port och autentiseringsuppgifter.
   * Om du använder en proxyserver du bör redan ha tillåtit hello webbadresser som beskrivs i [krav](#on-premises-prerequisites).
   * Om du använder en anpassad proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentiseras. inställningarna för hello VMM RunAs-konto kan ändras i hello VMM-konsolen. I **inställningar**, expandera **säkerhet** > **kör som-konton**, och sedan ändra hello lösenordet för DRAProxyAccount. Du behöver toorestart hello VMM-tjänsten så att den här inställningen träder i kraft.

     ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Acceptera eller ändra hello platsen för ett SSL-certifikat som genereras automatiskt för datakryptering. Det här certifikatet används om du aktiverar datakryptering för ett moln som skyddas av Azure hello Azure Site Recovery-portalen. Skydda det här certifikatet. När du kör en växling vid fel tooAzure måste den toodecrypt, om datakryptering är aktiverat.

    > [!NOTE]
    > Det rekommenderas toouse hello krypteringsfunktioner som tillhandahålls av Azure för att kryptera data i vila, istället för att använda hello datakrypteringsalternativet som tillhandahålls av Azure Site Recovery. hello krypteringsfunktioner som tillhandahålls av Azure kan aktiveras för en lagring > kontot och hjälper till att få bättre prestanda eftersom hello kryptering/dekryptering hanteras av Azure storage.
    > [Lär dig mer om kryptering med Storage-tjänsten från Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
9. Aktivera **synkronisera molnmetadata**om du vill toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen. Klicka på **registrera** toocomplete hello processen.

    ![Serverregistrering](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Registreringen startar. När registreringen är klar hello server visas i **Site Recovery-infrastruktur** > **VMM-servrar**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installera hello Azure Recovery Services-agenten på Hyper-V-värdar

1. När du har konfigurerat hello providern behöver du toodownload hello installationsfilen för hello Azure Recovery Services-agenten. Kör installationsprogrammet på varje Hyper-V-server i hello VMM-moln.

    ![Hyper-V-platser](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. I **Kravkontroll**, klicka på **Nästa**. Alla nödvändiga komponenter som saknas installeras automatiskt.

    ![Krav för Recovery Services-agenten](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. I **installationsinställningar**Godkänn eller ändra hello installationsplatsen och hello cacheplatsen. Du kan konfigurera hello cachen på en enhet som har minst 5 GB tillgängligt utrymme, men vi rekommenderar en cacheenhet med 600 GB eller mer ledigt utrymme. Klicka på **Installera**.
4. När installationen är klar klickar du på **Stäng** toofinish.

    ![Registrera MARS-agenten](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Kommandoradsinstallation
Du kan installera hello Microsoft Azure Recovery Services-agenten från kommandoraden med hjälp av hello följande kommando:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Konfigurera internet proxy åtkomst tooSite återställning från Hyper-V-värdar

hello Recovery Services-agenten körs på Hyper-V-värdar måste tooAzure för internet-åtkomst för VM-replikering. Om du försöker komma åt hello internet via en proxyserver, ställa in den på följande sätt:

1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värd. Som standard finns en genväg till Microsoft Azure Backup på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.
3. På hello **proxykonfiguration** anger du information om proxyservern.

    ![Registrera MARS-agenten](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Kontrollera att hello-agenten kan nå hello webbadresser som beskrivs i hello [krav](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön
Ange hello Azure storage-konto toobe används för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.

1. Klicka på **Förbered infrastruktur** > **mål**hello prenumerationen och välj hello resursgruppen där du vill att toocreate hello misslyckades för virtuella datorer. Välj hello distributionsmodell som du vill toouse i Azure (klassiska eller resource management) för hello misslyckades för virtuella datorer.

    ![Lagring](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

    ![Lagring](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Om du inte har skapat ett lagringskonto och du vill toocreate en med Resource Manager klickar du på **+ lagringskonto** toodo det internt.  På hello **skapa lagringskonto** anger ett kontonamn, typ, prenumeration och plats. hello-konto måste finnas i hello samma plats som hello Recovery Services-valvet.

   ![Lagring](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen gör det hello Azure-portalen. [Läs mer](../storage/common/storage-create-storage-account.md)
   * Om du använder ett premium-lagringskonto för replikerade data, skapa en ytterligare standardlagringskonto toostore replikeringsloggar som samlar in löpande ändringar tooon lokala data.
5. Om du inte har skapat ett Azure-nätverk och du vill toocreate en med Resource Manager klickar du på **+ nätverk** toodo det internt. På hello **skapa virtuellt nätverk** anger ett nätverksnamn, adressintervall, information om undernät, prenumeration och plats. hello nätverket bör finnas i hello samma plats som hello Recovery Services-valvet.

   ![Nätverk](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Om du vill toocreate ett nätverk med hello klassiska modellen gör det hello Azure-portalen. [Läs mer](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurera nätverksmappning

* [Läs](#prepare-for-network-mapping) en snabb överblick över vad nätverksmappning gör.
* Kontrollera att virtuella datorer på hello VMM-servern är ansluten tooa Virtuellt datornätverk och att du har skapat minst ett virtuellt Azure-nätverk. Flera Virtuella datornätverk kan vara mappade tooa enda Azure-nätverk.

Konfigurera mappning på följande sätt:

1. I **Site Recovery-infrastruktur** > **nätverksmappningar** > **nätverksmappning**, klicka på hello **+ nätverksmappning**  ikon.

    ![Nätverksmappning](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. I **Lägg till nätverksmappning**väljer hello VMM-källservern och **Azure** som hello mål.
3. Kontrollera hello prenumeration och hello distributionsmodell efter växling vid fel.
4. I **Källnätverk**väljer hello lokala VM-källnätverket önskade toomap hello listan som är associerade med hello VMM-servern.
5. I **målnätverket**, Välj hello Azure-nätverk i vilka replik virtuella Azure-datorer kommer att finnas när de skapas. Klicka sedan på **OK**.

    ![Nätverksmappning](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Det här händer när nätverksmappningen börjar:

* Befintliga virtuella datorer i VM-källnätverket hello är anslutna toohello målnätverket när mappningen börjar. Nya virtuella datorer anslutna toohello källnätverket ansluts toohello mappade Azure-nätverket när replikeringen sker.
* Om du ändrar en befintlig nätverksmappning ansluts virtuella replikdatorer med hello nya inställningar.
* Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet som hello källa virtual machine finns så hello replikerade virtuella datorn ansluter toothat målundernätverket efter en redundansväxling.
* Om det inte finns något målundernät med ett matchande namn, ansluter hello virtuella toohello första undernätet i hello nätverk.

## <a name="configure-replication-settings"></a>Konfigurera replikeringsinställningar
1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn.
3. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).

    > [!NOTE]
    >  En 30 andra frekvens stöds inte vid replikering av toopremium lagring. hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring. [Läs mer](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. I **kvarhållningstid för återställningspunkten**, ange i timmar hur länge hello kvarhållningsperiod ska vara för varje återställningspunkt. Skyddade datorer kan återställas tooany punkt inom en period.
5. I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (1–12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas. Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn. Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Observera att om du aktiverar programkonsekventa ögonblicksbilder påverkas hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.
6. I **starttid för inledande replikering**, ange när toostart hello inledande replikering. hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid.
7. I **kryptera data lagrade på Azure**, ange om tooencrypt vilande data i Azure-lagring. Klicka sedan på **OK**.

    ![Replikeringsprincip](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. När du skapar en ny princip associeras den automatiskt med hello VMM-moln. Klicka på **OK**. Du kan associera ytterligare VMM-moln (och hello virtuella datorer i dem) med den här replikeringsprincipen i **replikering** > principnamn > **associera VMM-moln**.

    ![Replikeringsprincip](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Kapacitetsplanering

Nu när du har konfigurerat den grundläggande infrastrukturen är det dags att planera för kapaciteten och tänka igenom om du behöver ytterligare resurser.

Site Recovery tillhandahåller en kapacitet planner toohelp du allokera hello rätt resurser för din källmiljö, Site Recovery-komponenter, nätverk och lagring. Du kan köra hello planner i snabbläge för att få uppskattningar baserat på ett medelvärde för antalet virtuella datorer, diskar och lagring eller i detaljerat läge där du kan ange siffrorna på arbetsbelastningsnivå hello. Innan du börjar:

* Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.
* Beräkna hello dagliga (omsättningen) förändringstakten för replikerade data. Du kan använda hello [Capacity planner för Hyper-V-replikering](https://www.microsoft.com/download/details.aspx?id=39057) toohelp du göra detta.

1. Klicka på **hämta**, toodownload hello verktyget och sedan köra den. [Läs hello artikel](site-recovery-capacity-planner.md) som medföljer hello-verktyget.
2. När du är klar väljer du **Ja** i **har du kört hello Capacity Planner**?

   ![Kapacitetsplanering](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   Lär dig mer om att [kontrollera nätverkets bandbredd](#network-bandwidth-considerations)




## <a name="enable-replication"></a>Aktivera replikering

Innan du börjar bör du kontrollera att ditt Azure-konto har hello krävs [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.

Aktivera replikering på följande sätt:

1. Klicka på **Steg 2: Replikera program** > **Källa**. När du har aktiverat replikering för hello första gången, klickar du på **+ replikera** i hello valvet tooenable replikering för ytterligare datorer.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. I hello **källa** bladet välj hello VMM-servern och hello moln i vilka hello Hyper-V-värdarna finns. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. I **mål**hello prenumeration, postredundans distributionsmodell och välj hello storage-konto som du använder för replikerade data.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Välj hello storage-konto som du vill toouse. Om du vill toouse ett annat lagringskonto än de som du har kan du [skapar du en](#set-up-an-azure-storage-account). Om du använder ett premium-lagringskonto för replikerade data måste tooselect en ytterligare standardlagring konto toostore replikeringsloggar som samlar in löpande ändringar tooon lokala data.toocreate ett lagringskonto med hjälp av hello Resource Manager-modellen Klicka på **Skapa nytt**. Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen gör det [i hello Azure-portalen](../storage/common/storage-create-storage-account.md). Klicka sedan på **OK**.
5. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas efter växling vid fel. Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare**, tooselect hello Azure-nätverk per dator. Om du vill toouse ett annat nätverk än de som du har kan du [skapar du en](#set-up-an-azure-network). ett nätverk som använder toocreate hello Resource Manager modell klickar du på **Skapa nytt**. Om du vill toocreate ett nätverk med hello klassiska modellen gör det [i hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Välj ett undernät om det behövs. Klicka sedan på **OK**.
6. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. I **egenskaper** > **konfigurera egenskaper**hello operativsystemet för hello valda virtuella datorer och välj hello OS-disken.

    - Kontrollera att hello Azure VM namn (mål) uppfyller [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Som standard markeras alla hello diskar på hello VM för replikering, men du kan rensa diskar tooexclude dem.

        - Du kanske vill tooexclude diskar tooreduce replikering bandbredd. Exempelvis kan du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller appar startas om (till exempel pagefile.sys eller Microsoft SQL Server tempdb). Du kan utesluta hello disk från replikering av unselecting hello-disk.
        - Endast standarddiskar kan undantas. Du kan inte undanta OS-diskar.
        - Vi rekommenderar att du inte undantar dynamiska diskar. Site Recovery kan inte identifiera om en virtuell hårddisk är en standarddisk eller dynamisk disk på den virtuella gästdatorn. Om alla diskar för beroende dynamisk volym inte uteslutas visar hello skyddade dynamisk disk som en disk som misslyckats när hello Virtuella växlar och hello data på disken inte är tillgängliga.
        - När replikering har aktiverats kan du inte lägga till eller ta bort diskar för replikering. Om du vill tooadd eller utesluta en disk, du behöver toodisable skydd för hello VM och aktivera det igen.
        - Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du misslyckas under tre diskar och skapa två direkt i Azure VM misslyckades exempelvis endast tre diskar för hello som har redundansväxling tillbaka från Azure tooHyper-V. Du kan inte innehålla diskar som skapats manuellt i återställning efter fel, eller i omvända replikeringen från Hyper-V tooAzure.
        - Om du undantar en disk som behövs för ett program toooperate efter redundans tooAzure måste toocreate det manuellt i Azure, så att hello replikeras program kan köras. Alternativt kan du integrera Azure automation i en återställningsplan, toocreate hello disken under växling vid fel för hello-datorn.

    Klicka på **OK** toosave ändringar. Du kan ange ytterligare egenskaper senare.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, Välj hello replikeringsprincip som du vill tooapply för hello skyddade virtuella datorer. Klicka sedan på **OK**. Du kan ändra hello replikeringsprincip i **replikeringsprinciper** > principnamn > **redigera inställningar för**. De ändringar du gör används både för datorer som redan replikeras och för nya datorer.

   ![Aktivera replikering](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Du kan följa förloppet för hello **Aktivera skydd** jobb **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs, hello datorn är redo för redundans.

### <a name="view-and-manage-vm-properties"></a>Visa och hantera egenskaper för virtuella datorer

Vi rekommenderar att du kontrollerar hello egenskaper för hello källdatorn. Kom ihåg att hello Azure VM-namnet måste uppfylla [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. I **skyddade objekt**, klickar du på **replikerade objekt**, och välj hello datorn toosee information.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. I **egenskaper**, kan du visa replikering och information om växling vid fel för hello VM.

    ![Aktivera replikering](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, kan du ange hello Azure VM-namn och mål för storlek. Ändra hello namnet toocomply med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) om du behöver. Du kan också visa och ändra information om hello målnätverket, undernätet och hello IP-adress som är tilldelad toohello Azure VM.
Tänk på följande:

   * Du kan ange IP-adressen för hello mål. Om du inte anger någon adress använder hello redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under redundansväxlingen, misslyckas hello växling vid fel. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.
   * hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:

     * Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
     * Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello antalet tillåtna för hello Målstorlek sedan hello högsta används.
     * Till exempel om en källdator har två nätverkskort och hello måldatorn stöder fyra hello måldatorn att ha två kort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
     * Om hello VM finns flera nätverkskort ansluts alla toohello samma nätverk.

     ![Aktivera replikering](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. I **diskar** du kan se hello operativsystemet och datadiskarna på hello virtuell dator som kommer att replikeras.

#### <a name="managed-disks"></a>Hanterade diskar

I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, du kan ange ”Använd hanterade diskar” inställningen för ”Ja” för hello VM om du vill tooattach hanterade diskar tooyour datorn på tooAzure för migrering. Hanterade diskar förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera hello storage-konton som är associerade med hello Virtuella diskar. [Mer information om hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Hanterade diskar kan skapas och anslutna toohello virtuell dator endast på en tooAzure för växling vid fel. Om du aktiverar skydd fortsätter data från lokala datorer tooreplicate toostorage konton.
   Hanterade diskar kan endast skapas för virtuella datorer som distribueras med hello Resource manager-distributionsmodellen.  

  > [!NOTE]
  > Återställning från Azure tooon lokala Hyper-V-miljön stöds inte för datorer med hanterade diskar. Ange ”Använd hanterade diskar” för ”Ja” bara om du tänker toomigrate den här datorn till Azure.

   - När du anger ”Använd hanterade diskar” för ”Ja” kan skulle endast tillgänglighetsuppsättningar i resursgruppen hello med ”Använd hanterade diskar” för ”Ja” vara tillgänglig för urvalet. Det beror på att virtuella datorer med hanterade diskar kan bara ingå i tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning för ”Ja”. Se till att du skapar tillgänglighetsuppsättningar med ”Använd hanterade diskar” egenskapsuppsättning baserat på avsiktshantering toouse hanteras diskarna på redundanskluster.  På samma sätt när du anger ”Använd hanterade diskar” för ”Nej” kan skulle endast tillgänglighetsuppsättningar i resursgruppen för hello med ”Använd hanterade diskar” värdet för egenskapen för ”Nej” vara tillgänglig för urvalet. [Mer information om hanterade diskar och tillgänglighetsuppsättningar](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Om hello storage-konto som används för replikering har krypterats med Storage Service-kryptering när som helst i tid, misslyckas skapandet av hanterade diskar under växling vid fel. Du kan antingen ange ”Använd hanterade diskar” för ”Nej” och försök växling vid fel eller inaktivera skyddet för hello virtuella datorn och skydda den tooa storage-konto som inte var lagringstjänstens kryptering aktiverat när som helst i tid.
  > [Lär dig mer om Storage Service Encryption och hanterade diskar](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-hello-deployment"></a>Testa hello-distribution

Du kan köra ett redundanstest för en enskild virtuell dator eller en återställningsplan som innehåller en eller flera virtuella datorer tootest hello-distribution.

### <a name="before-you-start"></a>Innan du börjar

 - Om du vill tooconnect tooAzure virtuella datorer med RDP efter en växling vid fel, Lär dig mer om [förbereder tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - toofully test som du behöver toocopy Active Directory och DNS i din testmiljö. [Läs mer](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Köra ett redundanstest

1. toofail via en enda virtuell dator i **replikerade objekt**, klicka på hello VM > **+ testa redundans**.
2. toofail över en återställning planera i **Återställningsplaner**, högerklicka på hello plan > **Redundanstest**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).
3. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer ansluta efter redundansväxlingen.
4. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **Redundanstest** jobb **Site Recovery-jobb**.
5. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM är hello rätt storlek, att den har anslutit toohello lämpligt nätverk och körs.
6. Om du har förberett för anslutningar efter växling vid fel, bör du kunna tooconnect toohello Azure VM.
7. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar** spela in och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.

Mer information finns i hello [testa redundans tooAzure](site-recovery-test-failover-to-azure.md) artikel.

## <a name="monitor-hello-deployment"></a>Övervakaren hello distribution

Här visas hur du kan övervaka konfigurationsinställningarna, statusen och hälsan för hello distributionen av Site Recovery:

1. Klicka på hello valvet namn tooaccess hello **Essentials** instrumentpanelen. På den här instrumentpanelen kan du övervaka Site Recovery-jobb, replikeringsstatusen, återställningsplaner, servertillstånd och händelser.  Du kan anpassa **Essentials** tooshow hello paneler och layouter som är mest användbara tooyou, inklusive hello status för andra Site Recovery och Backup-valv.

    ![Essentials](./media/site-recovery-vmm-to-azure/essentials.png)
2. I **hälsa**, kan du övervaka problem på lokala servrar (VMM- eller servrar) och hello händelser som skapats av Site Recovery i hello senaste 24 timmarna.
3. I hello **replikerade objekt**, **Återställningsplaner**, och **Site Recovery-jobb** paneler som du kan hantera och övervaka replikering. Du kan visa mer detaljer om jobb under **Jobb** > **Site Recovery-jobb**.

## <a name="command-line-installation-for-hello-azure-site-recovery-provider"></a>Kommandoradsinstallation för hello Azure Site Recovery Provider

hello Azure Site Recovery-providern kan installeras från kommandoraden hello. Den här metoden kan vara används tooinstall hello-providern på Server Core för Windows Server 2012 R2.

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Kör installationsprogrammet för dessa kommandon tooextract hello-providern från en upphöjd kommandotolk:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Kör det här kommandot tooinstall hello komponenter:

            C:\ASR> setupdr.exe /i
4. Köra dessa kommandon, tooregister hello server i hello-valv:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

Där:

* **/ Credentials**: obligatorisk parameter som anger där hello registreringsnyckelfilen finns.  
* **/ FriendlyName**: obligatorisk parameter för hello namnet på hello Hyper-V-värdservern som visas i hello Azure Site Recovery-portalen.
* * **/ EncryptionEnabled**: valfri parameter när du replikerar virtuella Hyper-V-datorer i VMM-moln tooAzure. Om du vill tooencrypt virtuella datorer i Azure (kryptering av vilande data). Se till att hello namnet på hello-filen har en **.pfx** tillägg. Kryptering är inaktiverat som standard.

    > [!NOTE]
    > Det rekommenderas toouse hello krypteringsfunktioner som tillhandahålls av Azure för att kryptera data i vila, istället för att använda hello kryptering (alternativ EncryptionEnabled) tillhandahålls av Azure Site Recovery. hello krypteringsfunktioner som tillhandahålls av Azure kan aktiveras för ett lagringskonto och hjälper dig att få bättre prestanda som görs av Azure hello kryptering/dekryptering  
    > Storage.
    > [Lär dig mer om kryptering med Storage-tjänsten i Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/ proxyaddress**: valfri parameter som anger hello proxyserver hello-adress.
* **/ Proxyport**: valfri parameter som anger hello port hello proxyserver.
* **/ proxyusername**: valfri parameter som anger hello proxyserverns användarnamn (Om proxyservern kräver autentisering).
* **/ proxypassword**: valfri parameter som anger hello lösenord tooauthenticate med proxyservern (om hello proxyservern kräver autentisering).


### <a name="network-bandwidth-considerations"></a>Att tänka på när det gäller nätverksbandbredden
Du kan använda hello kapacitet planner verktyget toocalculate hello bandbredd som du behöver för replikering (inledande replikering och sedan delta). toocontrol hello mängden användningen av nätverksbandbredd för replikering, har du några alternativ:

* **Begränsa bandbredden**: Hyper-V-trafik som replikeras tooa sekundär plats går igenom en specifik Hyper-V-värd. Du kan begränsa bandbredden på värdservern för hello.
* **Justera bandbredden**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar.

#### <a name="throttle-bandwidth"></a>Begränsa bandbredden
1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värdservern. Som standard finns en genväg till Microsoft Azure Backup på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.
3. På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**, och ange hello gränserna för arbete och fritid timmar. Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.

    ![Begränsa bandbredden](./media/site-recovery-vmm-to-azure/throttle2.png)

Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning. Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

#### <a name="influence-network-bandwidth"></a>Påverka nätverkets bandbredd
Hej **UploadThreadsPerVM** registret värde kontrollerar hello antalet trådar som används för att överföra data (inledande replikering eller delta) för en disk. Ett högre värde ökar hello nätverksbandbredd som används för replikering. Hej **DownloadThreadsPerVM** registervärdet anger hello antalet trådar som används för att överföra data under återställning efter fel.

1. Hello registret, navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Ändra värdet för hello **UploadThreadsPerVM** (eller skapa hello nyckeln om det inte finns) toocontrol trådarna som används för diskreplikering.
   * Ändra värdet för hello **DownloadThreadsPerVM** (eller skapa hello nyckeln om det inte finns) toocontrol trådarna som används för redundanstrafik från Azure.
2. hello standardvärdet är 4. I ett ”överetablerat” nätverk bör registernycklarna ändras från hello standardvärden. hello maximala är 32. Övervaka trafik toooptimize hello värde.

## <a name="next-steps"></a>Nästa steg

När den inledande replikeringen är klar och du har testat hello distribution, kan du anropa redundans som hello behov. [Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur toorun dem.
