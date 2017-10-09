---
title: "aaaMigrating tooAzure Premium-lagring med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Migrera dina befintliga virtuella datorer tooAzure Premium-lagring med Site Recovery. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: luywang
ms.openlocfilehash: cb71c06e4a1a73d484e226a573d1ade48c87664d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-toopremium-storage-using-azure-site-recovery"></a>Migrera tooPremium lagring med hjälp av Azure Site Recovery

[Azure Premium-lagring](storage-premium-storage.md) ger stöd för virtuella datorer (VM) som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. hello syftet med den här guiden är toohelp användare migrera de Virtuella diskarna från en standardlagring konto tooa premiumlagringskonto med [Azure Site Recovery](../site-recovery/site-recovery-overview.md).

Site Recovery är en Azure-tjänst som stödjer tooyour affärskontinuitet och haveriberedskap genom att samordna hello replikeringen av lokala fysiska servrar och virtuella datorer toohello molnet (Azure) eller tooa sekundärt datacenter. Vid driftstopp på den primära platsen växlar du över toohello sekundär plats tookeep program och arbetsbelastningar som är tillgängliga. Du växlar tillbaka tooyour primära platsen när den returnerar toonormal igen. Site Recovery tillhandahåller redundanstestning toosupport haveriberedskap utan att påverka produktionsmiljöer. Du kan köra redundansväxlingar med minimal dataförlust (beroende på replikeringsfrekvens) för oväntade haverier. I scenariot hello av migrerar tooPremium lagring, kan du använda hello [redundans i Site Recovery](../site-recovery/site-recovery-failover.md) i Azure Site Recovery toomigrate mål diskar tooa premiumlagringskonto.

Vi rekommenderar migrering tooPremium lagring med hjälp av Site Recovery eftersom det här alternativet ger minimal avbrottstid och undviker hello manuell körning av kopierar diskar och skapa nya virtuella datorer. Site Recovery kommer systematiskt kopiera diskarna och skapa nya virtuella datorer under växling vid fel. Site Recovery stöder ett antal typer av redundansväxlingar med minimal eller ingen avbrottstid. tooplan din driftstopp och uppskattning dataförlust, se hello [typer av redundans](../site-recovery/site-recovery-failover.md) tabellen i Site Recovery. Om du [förbereda tooconnect tooAzure virtuella datorer efter redundans](../site-recovery/site-recovery-vmware-to-azure.md), bör du kunna tooconnect toohello virtuella Azure-datorn med RDP efter en växling vid fel.

![][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery-komponenter

Dessa är hello Site Recovery-komponenter som är relevanta toothis Migreringsscenario.

* **Konfigurationsservern** är en Azure VM som samordnar kommunikationen och hanterar processer för replikering och återställning av data. På den här virtuella datorn körs en enda installationsprogrammet tooinstall hello configuration filserver och en ytterligare komponent som kallas en processerver som en gateway för replikering. Läs mer om [server konfigurationskrav](../site-recovery/site-recovery-vmware-to-azure.md). Konfigurationsservern endast måste toobe som konfigurerats på en gång och kan användas för alla migreringar toohello samma region.

* **Processervern** är en replikerings-gateway som tar emot replikeringsdata från virtuella källdatorer optimerar hello data med cachelagring, komprimering och kryptering och skickar den tooa storage-konto. Den hanterar push-installation av hello mobility service toosource virtuella datorer och utför automatisk identifiering av virtuella källdatorer. hello standard processen server är installerad på hello konfigurationsservern. Du kan distribuera ytterligare fristående process servrar tooscale din distribution. Läs mer om [Metodtips för distribution av processerver](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) och [distribution av ytterligare servrar](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Processervern endast måste toobe som konfigurerats på en gång och kan användas för alla migreringar toohello samma region.

* **Mobilitetstjänsten** är en komponent som har distribuerats på varje standard virtuell dator som du vill tooreplicate. Den samlar in dataskrivningar på hello standard VM och vidarebefordrar dem toohello processervern. Läs mer om [replikeras förutsättningar dator](../site-recovery/site-recovery-vmware-to-azure.md).

Den här bilden illustrerar hur dessa komponenter.

![][15]

> [!NOTE]
> Site Recovery stöder inte hello migrering av lagringsutrymmen-diskarna.

Ytterligare komponenter för andra scenarier finns för[scenariots arkitektur](../site-recovery/site-recovery-vmware-to-azure.md).

## <a name="azure-essentials"></a>Azure essentials

Dessa hello Azure kraven för den här Migreringsscenario.

* En Azure-prenumeration
* Ett Azure Premium storage-konto toostore replikerade data
* Ett virtuellt Azure-nätverk (VNet) toowhich virtuella datorer ansluter när de skapas under växling vid fel. hello Azure VNet måste vara i hello samma region som hello något i vilka hello Site Recovery körs
* Ett Standard till Azure storage-konto i vilka toostore replikeringsloggar. Detta kan vara hello samma lagringskonto som hello Virtuella diskar migreras

## <a name="prerequisites"></a>Krav

* Förstå hello relevanta migrering scenariot komponenterna i föregående avsnitt hello
* Planera din driftstopp genom att lära dig om hello [redundans i Site Recovery](../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Installation och migrering av steg

Du kan använda Site Recovery toomigrate Azure IaaS-VM mellan regioner eller i samma region. hello följande instruktioner har anpassats för det här scenariot för migrering från hello artikel [replikera virtuella VMware-datorer eller fysiska servrar tooAzure](../site-recovery/site-recovery-vmware-to-azure.md). Följ länkarna hello detaljerade anvisningar i ytterligare toohello anvisningarna i den här artikeln.

1. **Skapa ett Recovery Services-valv**. Skapa och hantera hello Site Recovery-valvet via hello [Azure-portalen](https://portal.azure.com). Klicka på **nya** > **Management** > **säkerhetskopiering** och **Site Recovery (OMS)**. Du kan också klicka på **Bläddra** > **Återställningstjänstvalvet** > **Lägg till**. Virtuella datorer kommer att replikeras toohello region som du anger i det här steget. För migrering i hello hello samma region, Välj hello region där din virtuella källdatorer och källa storage-konton är. Observera att migreringen tooPremium storage-konton stöds endast i hello [Azure-portalen](https://portal.azure.com), inte hello [klassiska portalen](https://manage.windowsazure.com).

2. hello följande steg när du **välja skyddsmål**.

    2a. Öppna hello på hello VM där du vill att tooinstall hello konfigurationsservern [Azure-portalen](https://portal.azure.com). Gå för**Recovery Services-valv** > **inställningar**. Under **inställningar**väljer **Site Recovery**. Under **Site Recovery**väljer **steg 1: Förbered infrastrukturen**. Under **Förbered infrastruktur**väljer **skyddsmål**.

    ![][2]

    2b. Under **skyddsmål**, i hello första nedrullningsbara listan, Välj **tooAzure**. Hello andra nedrullningsbara listan, Välj **inte virtualiserade / andra**, och klicka sedan på **OK**.

    ![][3]

3. hello följande steg när du **konfigurera hello källmiljön (konfigurationsserver)**.

    3a. Hämta hello **Unified installationsprogram för Azure Site Recovery** och hello **valvregistreringsnyckel** genom att gå toohello **Förbered infrastruktur**  >  **Förbered källa** > **Lägg till Server** bladet. Du behöver hello valvet Registreringsinställningar viktiga toorun hello enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

    ![][4]

    3B. Lägg till konfigurationsservern i hello **Lägg till Server** bladet.

    ![][5]

    3c. Kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern och processervern hello på hello VM som du använder som hello konfigurationsservern. Du kan gå igenom hello skärmbilder [här](../site-recovery/site-recovery-vmware-to-azure.md) toocomplete hello installation. Du kan se toohello följande skärmbilder för steg som angetts för den här Migreringsscenario.

    I **innan du börjar**väljer **installera hello konfigurationsservern och processervern**.

    ![][6]

    3D. I **registrering**, bläddra och välj hello registreringsnyckel som du hämtade från hello-valvet.

    ![][7]

    3e. I **miljö information**väljer du om du ska tooreplicate virtuella VMware-datorer. Den här Migreringsscenario väljer **nr**.

    ![][8]

    3F. När hello installationen är klar visas hello **Microsoft Azure Site Recovery Configuration Server** fönster. Använd hello **hantera konton** fliken toocreate hello konto som Site Recovery kan användas för automatisk identifiering. (I hello scenario om hur du skyddar fysiska datorer, konfigurerar hello konto är irrelevant, men du behöver minst ett konto tooenable något av följande hello. I det här fallet, kan du kalla hello konto och lösenord som helst.) Använd hello **valvet registrering** fliken tooupload hello valvautentiseringsfilen.

    ![][9]

4. **Ställ in hello målmiljön**. Klicka på **Förbered infrastruktur** > **mål**, och ange hello distributionsmodell som du vill ha toouse för virtuella datorer efter redundans. Du kan välja **klassiska** eller **Resource Manager**, beroende på ditt scenario.

    ![][10]

    Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk. Observera att om du använder ett Premium-lagringskonto för replikerade data kan du tooset upp en ytterligare standardlagring konto toostore replikering loggar.

5. **Konfigurera replikeringsinställningar**. Följ [konfigurera replikeringsinställningar](../site-recovery/site-recovery-vmware-to-azure.md) tooverify som din konfigurationsservern är har associerat med hello replikeringsprincip som du skapar.

6. **Kapacitetsplanering**. Använd hello [kapacitetsplaneringsverktyg](../site-recovery/site-recovery-capacity-planner.md) tooaccurately uppskattning nätverkets bandbredd, lagring och andra krav toomeet måste din replikering. När du är klar väljer du **Ja** i **har du planerat kapaciteten?**.

    ![][11]

7. hello följande steg när du **installera mobilitetstjänsten och aktivera replikering**.

    7 a. Du kan välja för[push-installation](../site-recovery/site-recovery-vmware-to-azure.md) tooyour källa virtuella datorer eller för[manuellt installera mobilitetstjänsten](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) på dina virtuella källdatorer. Du hittar hello kravet på Skicka installationen och hello sökväg hello manuell installer i hello-länken. Om du göra en manuell installation kan behöva du toouse en intern IP-adress toofind hello configuration-server.

    ![][12]

    hello misslyckades över VM har två tillfälliga diskar: en från hello primära virtuella datorn och hello andra skapades under hello etableringen för den virtuella datorn i hello recovery region. tooexclude hello diskutrymme före replikering, installera hello mobilitetstjänsten innan du aktiverar replikering. toolearn mer information om hur tooexclude hello temporär disk finns för[Exkludera diskar från replikeringen](../site-recovery/site-recovery-vmware-to-azure.md).

    7b. Aktivera replikering på följande sätt:
      * Klicka på **replikera program** > **källa**. När du har aktiverat replikering för hello första gången, klicka på + replikera hello valvet tooenable replikering för ytterligare datorer.
      * I steg 1, som källa processervern.
      * Ange hello postredundans distributionsmodell, Premium storage-konto toomigrate till, en standardlagring konto toosave loggar och ett virtuellt nätverk toofail till i steg 2.
      * I steg 3, lägga till skyddade virtuella datorerna med IP-adress (du kanske behöver en intern IP-adress toofind dem).
      * Konfigurera hello egenskaper genom att välja hello-konton som du ställer in tidigare på hello processervern i steg 4.
      * Välj hello replikeringsprincip som du skapade tidigare, konfigurera replikeringsinställningar i steg 5.
      Klicka på **OK** och aktivera replikering.

    > [!NOTE]
    > När en Azure VM är frigjord och startas igen, det är inte säkert som ska hämtas hello samma IP-adress. Om hello eller IP-adress hello server-processen konfigurationsservern hello skyddad Azure virtuella datorer ändra kan hello replikering i det här scenariot inte fungerar korrekt.

    ![][13]

    När du utformar din Azure Storage-miljö, rekommenderar vi att du använder separata storage-konton för varje virtuell dator i en tillgänglighetsuppsättning. Vi rekommenderar att du följer hello bästa praxis i hello lagringsskikt för[använder flera lagringskonton för varje tillgänglighetsuppsättning](../virtual-machines/windows/manage-availability.md). Distribuera Virtuella diskar toomultiple storage-konton hjälper tooimprove lagerutrymme och distribuerar hello-i/o mellan hello Azure lagringsinfrastruktur. Om din virtuella dator finns i en tillgänglighetsuppsättning, istället för att replikera diskar för alla virtuella datorer i ett lagringskonto, rekommenderar vi Migrera flera virtuella datorer på flera gånger, så att hello virtuella datorer i hello samma tillgänglighetsuppsättning inte delar ett enda storage-konto. Använd hello **Aktivera replikering** bladet tooset upp en destinationslagringskontot för varje virtuell dator i taget. Du kan välja en postredundans distributionsmodell enligt tooyour behov. Om du väljer Resource Manager (RM) som din postredundans distributionsmodell, kan du växla över RM VM-tooan RM VM eller kan du växla över en klassiska Virtuella tooan RM VM.

8. **Köra ett redundanstest**. toocheck om din replikeringen är klar, Site Recovery och klicka sedan på **inställningar** > **replikerade objekt**. Hello statusen och procentandelen din replikeringen visas. Efter den första replikeringen är slutförd, kör Redundanstestet toovalidate din replikeringsstrategi för. Detaljerade anvisningar för att testa redundans finns för[köra ett redundanstest i Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md). Du kan se status för hello testningen i **inställningar** > **jobb** > **YOUR_FAILOVER_PLAN_NAME**. På bladet hello visas en sammanställning av hello steg och lyckade/misslyckade resultat. Klicka på hello steg toocheck hello felmeddelande om hello testa redundans misslyckas när som helst. Kontrollera att dina virtuella datorer och replikeringsstrategi uppfyller hello krav innan du kör en redundansväxling. Läs [Redundanstestning tooAzure i Site Recovery](../site-recovery/site-recovery-test-failover-to-azure.md) för mer information och anvisningar testningen.

9. **Kör en redundansväxling**. När hello redundanstestet har slutförts och kör en växling vid fel toomigrate din diskar tooPremium lagring och replikera hello VM-instanser. Följ hello beskrivs stegen i [kör en redundansväxling](../site-recovery/site-recovery-failover.md#run-a-failover). Kontrollera att du väljer **Stäng virtuella datorer och synkronisera senaste data för hello** toospecify att Site Recovery ska försöka tooshut ned hello skyddade virtuella datorer och synkronisera hello data så att hello senaste versionen av hello data kommer att redundansväxlas. Om du inte väljer det här alternativet eller hello försök inte lyckas blir hello växling vid fel från hello senaste tillgängliga återställningspunkten för hello VM. Site Recovery skapar en VM-instans vars typ är hello samma som eller liknande tooa Premium Storage-kompatibla VM. Du kan kontrollera hello prestanda och pris för olika VM-instanser genom att gå för[prissättning för Windows Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) eller [priser för Linux virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Eftermigreringen

1. **Konfigurera replikerade virtuella datorer toohello tillgänglighetsuppsättning om tillämpligt**. Site Recovery stöder inte migrera virtuella datorer tillsammans med hello tillgänglighetsuppsättning. Beroende på hello distribution av den replikerade virtuella datorn, gör du något av följande hello:
  * För en virtuell dator som skapats med hjälp av hello klassiska distributionsmodellen: Lägg till hello VM toohello tillgänglighetsuppsättning i hello Azure-portalen. Detaljerade anvisningar finns för[lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning](../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * För hello Resource Manager-distributionsmodellen: spara din konfiguration av hello VM och ta sedan bort och återskapa hello virtuella datorer i tillgänglighetsuppsättningen hello. toodo så Använd hello skript på [ange Azure Resource Manager VM Tillgänglighetsuppsättning](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Kontrollera hello begränsning av det här skriptet och planera driftstopp innan du kör hello skript.

2. **Ta bort gamla virtuella datorer och diskar**. Kontrollera att hello premiumdiskar är konsekventa med källan diskar och hello nya virtuella datorer utföra hello samma fungerar som källa för hello virtuella datorer innan du tar bort dessa. Ta bort hello VM i distributionsmodell för hello Resource Manager (RM) och ta bort hello diskar från källan storage-konton i hello Azure-portalen. Du kan ta bort hello VM och diskarna i hello klassiska portalen eller Azure-portalen i hello klassiska distributionsmodellen. Om det finns ett problem i vilka hello disk inte tas bort även om du har tagit bort hello VM kan du läsa avsnittet [Felsöka när du tar bort virtuella hårddiskar](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Rensa hello Azure Site Recovery-infrastruktur**. Om Site Recovery inte längre behövs tar Rensa du sin infrastruktur genom att ta bort replikerade objekt, hello konfigurationsservern och hello återställningsprincipen och sedan ta bort hello Azure Site Recovery-valvet.

## <a name="troubleshooting"></a>Felsökning

* [Övervaka och felsöka skydd för virtuella datorer och fysiska servrar](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Nästa steg

Se hello följande resurser för specifika scenarier för att migrera virtuella datorer:

* [Migrera Azure virtuella datorer mellan Storage-konton](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Skapa och ladda upp en Windows Server VHD-tooAzure.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrering av virtuella datorer från Amazon AWS tooMicrosoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Se även hello följande resurser toolearn mer om Azure Storage- och virtuella datorer i Azure:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](storage-premium-storage.md)

[1]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
