---
title: "Migrera till Azure Premium-lagring med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Migrera dina befintliga virtuella datorer till Azure Premium-lagring med Site Recovery. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
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
ms.openlocfilehash: cc364bdae49068a50ec86c537c3b878670b8b8b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="migrating-to-premium-storage-using-azure-site-recovery"></a>Migrera till Premium Storage med Azure Site Recovery

[Azure Premium-lagring](storage-premium-storage.md) ger stöd för virtuella datorer (VM) som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. Syftet med den här guiden är att hjälpa användare att migrera de Virtuella diskarna från ett standardlagringskonto till ett premiumlagringskonto med hjälp av [Azure Site Recovery](../site-recovery/site-recovery-overview.md).

Site Recovery är en Azure-tjänst som bidrar till din disaster recovery strategi för affärskontinuitet och genom att samordna replikeringen av lokala fysiska servrar och virtuella datorer till molnet (Azure) eller till ett sekundärt datacenter. Vid driftstopp på den primära platsen växlar du över till den sekundära platsen så att program och arbetsbelastningar som är tillgängliga. Du växlar tillbaka till den primära platsen när den återgår till normal drift. Site Recovery tillhandahåller redundanstestning så att den stöder haveriberedskap utan att påverka produktionsmiljöer. Du kan köra redundansväxlingar med minimal dataförlust (beroende på replikeringsfrekvens) för oväntade haverier. I scenariot för att migrera till Premium-lagring kan du använda den [redundans i Site Recovery](../site-recovery/site-recovery-failover.md) i Azure Site Recovery för att migrera måldiskarna till ett premiumlagringskonto.

Vi rekommenderar att du migrerar till Premium-lagring med hjälp av Site Recovery eftersom det här alternativet ger minimal avbrottstid och undviker manuell körningen av kopierar diskar och skapa nya virtuella datorer. Site Recovery kommer systematiskt kopiera diskarna och skapa nya virtuella datorer under växling vid fel. Site Recovery stöder ett antal typer av redundansväxlingar med minimal eller ingen avbrottstid. För att planera din driftstopp och beräkna dataförlust, finns det [typer av redundans](../site-recovery/site-recovery-failover.md) tabellen i Site Recovery. Om du [förbereda för att ansluta till virtuella Azure-datorer efter redundans](../site-recovery/site-recovery-vmware-to-azure.md), du ska kunna ansluta till den virtuella Azure-datorn med RDP efter en växling vid fel.

![][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery-komponenter

Dessa är Site Recovery-komponenter som är relevanta för den här Migreringsscenario.

* **Konfigurationsservern** är en Azure VM som samordnar kommunikationen och hanterar processer för replikering och återställning av data. På den här virtuella datorn körs en enda installationsfil för att installera konfigurationsservern och en ytterligare komponent som kallas en processerver som en gateway för replikering. Läs mer om [server konfigurationskrav](../site-recovery/site-recovery-vmware-to-azure.md). Konfigurationsservern endast måste konfigureras en gång och kan användas för alla migreringar till samma region.

* **Processervern** är en replikerings-gateway som tar emot replikeringsdata från virtuella källdatorer optimerar data med cachelagring, komprimering och kryptering och skickar det till ett lagringskonto. Den hanterar push-installation av mobilitetstjänsten till källdatorn virtuella datorer och utför automatisk identifiering av virtuella källdatorer. Standard-process-servern är installerad på konfigurationsservern. Du kan distribuera ytterligare fristående servrar för att skala din distribution. Läs mer om [Metodtips för distribution av processerver](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) och [distribution av ytterligare servrar](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Processervern endast måste konfigureras en gång och kan användas för alla migreringar till samma region.

* **Mobilitetstjänsten** är en komponent som distribueras på varje standard VM som du vill replikera. Den samlar in dataskrivningar på standard virtuella datorn och vidarebefordrar dem till processervern. Läs mer om [replikeras förutsättningar dator](../site-recovery/site-recovery-vmware-to-azure.md).

Den här bilden illustrerar hur dessa komponenter.

![][15]

> [!NOTE]
> Site Recovery har inte stöd för migrering av lagringsutrymmen-diskarna.

Ytterligare komponenter för andra scenarier, referera till [scenariots arkitektur](../site-recovery/site-recovery-vmware-to-azure.md).

## <a name="azure-essentials"></a>Azure essentials

Dessa är Azure kraven för den här Migreringsscenario.

* En Azure-prenumeration
* Ett Azure Premium storage-konto för att lagra replikerade data
* Virtuellt Azure-nätverk (VNet) virtuella datorer ansluter till när de skapas under växling vid fel. Azure-VNet måste vara i samma region som Site Recovery körs
* Ett Standard till Azure storage-konto att lagra replikeringsloggar. Detta kan vara samma lagringskonto som de Virtuella diskar migreras

## <a name="prerequisites"></a>Krav

* Förstå komponenterna i relevanta migrering scenariot i föregående avsnitt
* Planera din driftstopp genom att lära dig mer om den [redundans i Site Recovery](../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Installation och migrering av steg

Du kan använda Site Recovery för att migrera Azure IaaS-VM mellan regioner eller i samma region. Följande instruktioner har anpassats för det här scenariot för migrering från artikeln [replikera virtuella VMware-datorer eller fysiska servrar till Azure](../site-recovery/site-recovery-vmware-to-azure.md). Följ länkarna detaljerade steg i ytterligare instruktioner i den här artikeln.

1. **Skapa ett Recovery Services-valv**. Skapa och hantera Site Recovery-valvet via den [Azure-portalen](https://portal.azure.com). Klicka på **nya** > **Management** > **säkerhetskopiering** och **Site Recovery (OMS)**. Du kan också klicka på **Bläddra** > **Återställningstjänstvalvet** > **Lägg till**. Virtuella datorer kommer att replikeras till den region som du anger i det här steget. Välj den region där din virtuella källdatorer och källa storage-konton är för migrering i samma region. Observera att migrering till Premium-lagringskonton stöds endast i den [Azure-portalen](https://portal.azure.com), inte den [klassiska portalen](https://manage.windowsazure.com).

2. Följande steg hjälper dig **välja skyddsmål**.

    2a. På den virtuella datorn där du vill installera konfigurationsservern, öppna den [Azure-portalen](https://portal.azure.com). Gå till **Recovery Services-valv** > **inställningar**. Under **inställningar**väljer **Site Recovery**. Under **Site Recovery**väljer **steg 1: Förbered infrastrukturen**. Under **Förbered infrastruktur**väljer **skyddsmål**.

    ![][2]

    2b. Under **skyddsmål**, Välj i listan listruta **till Azure**. Välj i den andra listrutan **inte virtualiserade / andra**, och klicka sedan på **OK**.

    ![][3]

3. Följande steg hjälper dig **konfigurera källmiljön (konfigurationsserver)**.

    3a. Hämta den **Unified installationsprogram för Azure Site Recovery** och **valvregistreringsnyckel** genom att gå till den **Förbered infrastruktur**  >   **Förbered källa** > **Lägg till Server** bladet. Du behöver valvregistreringsnyckeln för att köra enhetlig installationen. Nyckeln är giltig i fem dagar efter genereringen.

    ![][4]

    3B. Lägg till konfigurationsservern i den **Lägg till Server** bladet.

    ![][5]

    3c. På den virtuella datorn använder du som konfigurationsservern och installera enhetlig för att installera konfigurationsservern och processervern. Du kan gå igenom skärmbilderna [här](../site-recovery/site-recovery-vmware-to-azure.md) att slutföra installationen. Du kan referera till följande skärmbilder för steg som angetts för den här Migreringsscenario.

    I **Before you begin** (Innan du börjar) väljer du **Install the configuration server and process server** (Installera konfigurationsservern och processervern).

    ![][6]

    3D. I **registrering**, bläddra och välj den certifikatnyckel som du hämtade från valvet.

    ![][7]

    3e. I **Miljöinformation** väljer du om du ska replikera virtuella VMwares-datorer. Den här Migreringsscenario väljer **nr**.

    ![][8]

    3F. När installationen är klar visas den **Microsoft Azure Site Recovery Configuration Server** fönster. Använd den **hantera konton** att skapa kontot som Site Recovery kan använda för automatisk identifiering. (Konfigurera kontot är inte relevant i scenario om hur du skyddar fysiska datorer men du behöver minst ett konto för att aktivera någon av följande steg. I det här fallet, kan du kalla kontot och lösenordet som helst.) Använd den **valvet registrering** att ladda upp valvautentiseringsfilen.

    ![][9]

4. **Konfigurera målmiljön**. Klicka på **Förbered infrastruktur** > **mål**, och ange den distributionsmodell som du vill använda för virtuella datorer efter redundans. Du kan välja **klassiska** eller **Resource Manager**, beroende på ditt scenario.

    ![][10]

    Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk. Observera att du måste konfigurera en ytterligare ett standardlagringskonto för att lagra replikeringsloggar om du använder ett Premium-lagringskonto för replikerade data.

5. **Konfigurera replikeringsinställningar**. Följ [konfigurera replikeringsinställningar](../site-recovery/site-recovery-vmware-to-azure.md) att kontrollera att serverns konfiguration har kopplats till replikeringsprincipen som du skapar.

6. **Kapacitetsplanering**. Använd den [kapacitetsplaneringsverktyg](../site-recovery/site-recovery-capacity-planner.md) för att beräkna bandbredd, lagring och andra krav för att uppfylla dina replikering behov. När du är klar väljer du **Ja** i **har du planerat kapaciteten?**.

    ![][11]

7. Följande steg hjälper dig **installera mobilitetstjänsten och aktivera replikering**.

    7 a. Du kan välja att [push-installation](../site-recovery/site-recovery-vmware-to-azure.md) till dina virtuella källdatorer eller till [manuellt installera mobilitetstjänsten](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) på dina virtuella källdatorer. Du kan hitta krav för push-överföring installation och sökvägen till manuell installationsprogrammet i länken. Om du gör en manuell installation kan du behöva använda en intern IP-adress för att hitta konfigurationsservern.

    ![][12]

    Kunde inte över VM har två tillfälliga diskar: en från den primära virtuella datorn och den andra skapades under etableringen för den virtuella datorn i området för återställning. Uteslut tillfälliga disken före replikering genom att installera mobilitetstjänsten innan du aktiverar replikering. Mer information om hur du utesluter den temporära disken avser [Exkludera diskar från replikeringen](../site-recovery/site-recovery-vmware-to-azure.md).

    7b. Aktivera replikering på följande sätt:
      * Klicka på **replikera program** > **källa**. När du har aktiverat replikering för första gången, klicka på + replikera i valvet för att aktivera replikering för ytterligare datorer.
      * I steg 1, som källa processervern.
      * Ange distributionsmodell efter en redundansväxling, ett premiumlagringskonto att migrera till ett standardlagringskonto för att spara loggar och ett virtuellt nätverk att växla till i steg 2.
      * Lägga till skyddade virtuella datorer efter IP-adress (du kanske behöver en intern IP-adress du hittar dem) i steg 3.
      * Konfigurera egenskaper för i steg 4, genom att välja de konton som du ställer in tidigare på processervern.
      * Välj replikeringsprincip som du skapade tidigare, konfigurera replikeringsinställningar i steg 5.
      Klicka på **OK** och aktivera replikering.

    > [!NOTE]
    > När en Azure VM är frigjord och startas igen, finns det ingen garanti för att den får samma IP-adress. Ändrar IP-adressen för server-processen konfigurationsservern eller skyddade virtuella Azure-datorer, kanske replikering i det här scenariot inte fungerar korrekt.

    ![][13]

    När du utformar din Azure Storage-miljö, rekommenderar vi att du använder separata storage-konton för varje virtuell dator i en tillgänglighetsuppsättning. Vi rekommenderar att du följer bästa praxis i lagringsskikt till [använder flera lagringskonton för varje tillgänglighetsuppsättning](../virtual-machines/windows/manage-availability.md). Distribuera Virtuella diskar med flera lagringskonton hjälper oss för att förbättra lagerutrymme och distribuerar läsning/skrivning i Azure storage-infrastruktur. Om din virtuella dator finns i en tillgänglighetsuppsättning, istället för att replikera diskar för alla virtuella datorer i ett lagringskonto, rekommenderar vi Migrera flera virtuella datorer på flera gånger, så att de virtuella datorerna i samma tillgänglighetsuppsättning inte delar ett enda storage-konto. Använd den **Aktivera replikering** bladet för att ställa in en destinationslagringskontot för varje virtuell dator i taget. Du kan välja en postredundans distributionsmodell enligt dina behov. Om du väljer Resource Manager (RM) som din postredundans distributionsmodell, kan du växla över en RM virtuell dator till en virtuell dator i RM eller kan du växla över en klassisk virtuell dator till en virtuell dator i RM.

8. **Köra ett redundanstest**. Om du vill kontrollera om din replikeringen är klar klickar du på Site Recovery och klicka sedan på **inställningar** > **replikerade objekt**. Du ser statusen och procentandelen din replikeringen. Efter den första replikeringen har slutförts kör du testa redundans för att verifiera din replikeringsstrategi för. Detaljerade anvisningar för att testa redundans finns i [köra ett redundanstest i Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md). Du kan se status för testning av redundans i **inställningar** > **jobb** > **YOUR_FAILOVER_PLAN_NAME**. På bladet visas en sammanställning av steg och lyckade/misslyckade resultat. Om du testar redundansen misslyckas när som helst, klickar du på det här steget för att kontrollera felmeddelande. Kontrollera att dina virtuella datorer och replikeringsstrategi uppfyller kraven innan du kör en redundansväxling. Läs [testa redundans till Azure i Site Recovery](../site-recovery/site-recovery-test-failover-to-azure.md) för mer information och anvisningar testningen.

9. **Kör en redundansväxling**. När testet har växling vid fel slutförts, kör en redundansväxling för att migrera diskarna till Premium-lagring och replikera VM-instanser. Följ de detaljerade anvisningarna i [kör en redundansväxling](../site-recovery/site-recovery-failover.md#run-a-failover). Kontrollera att du väljer **Stäng virtuella datorer och synkronisera senaste data** att ange att Site Recovery ska försöka stänga av de skyddade virtuella datorerna och synkronisera data så att den senaste versionen av data kommer att redundansväxlas. Om du inte väljer det här alternativet eller försöket inte lyckas blir växling vid fel från den senaste tillgängliga återställningspunkten för den virtuella datorn. Site Recovery skapar en VM-instans vars typ är samma som eller liknande till en Premium-lagring – kompatibla virtuell dator. Du kan kontrollera prestanda och priset för olika VM-instanser genom att gå till [prissättning för Windows Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) eller [priser för Linux virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Eftermigreringen

1. **Konfigurera replikerade virtuella datorerna till tillgänglighetsuppsättning om tillämpligt**. Site Recovery stöder inte migrera virtuella datorer tillsammans med tillgänglighetsuppsättningen. Beroende på distribution av den replikerade virtuella datorn, gör du något av följande:
  * För en virtuell dator som skapats med hjälp av den klassiska distributionsmodellen: lägga till den virtuella datorn till tillgänglighetsuppsättning i Azure-portalen. Detaljerade anvisningar finns i [lägga till en befintlig virtuell dator i en tillgänglighetsuppsättning](../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * För Resource Manager-distributionsmodellen: spara konfigurationen av den virtuella datorn och ta sedan bort och återskapa de virtuella datorerna i tillgänglighetsuppsättningen. Gör du genom att använda skript på [ange Azure Resource Manager VM Tillgänglighetsuppsättning](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Kontrollera begränsning av det här skriptet och planera driftstopp innan du kör skriptet.

2. **Ta bort gamla virtuella datorer och diskar**. Kontrollera att diskarna Premium är konsekventa med källdiskarna och nya virtuella datorer som utför samma funktion som källa för virtuella datorer innan du tar bort dessa. Ta bort den virtuella datorn och ta bort diskar från källan storage-konton i Azure-portalen i distributionsmodell Resource Manager (RM). I den klassiska distributionsmodellen kan du ta bort den virtuella datorn och diskarna i den klassiska portalen eller Azure-portalen. Om det finns ett problem där disken inte tas bort även om du har tagit bort den virtuella datorn kan du läsa avsnittet [Felsöka när du tar bort virtuella hårddiskar](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Rensa Azure Site Recovery-infrastruktur**. Om Site Recovery inte längre behövs tar Rensa du sin infrastruktur genom att ta bort replikerade objekt, konfigurationsservern och återställningsprincipen och sedan ta bort Azure Site Recovery-valvet.

## <a name="troubleshooting"></a>Felsökning

* [Övervaka och felsöka skydd för virtuella datorer och fysiska servrar](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Nästa steg

Se följande resurser för specifika scenarier för att migrera virtuella datorer:

* [Migrera Azure virtuella datorer mellan Storage-konton](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Skapa och ladda upp en Windows Server VHD till Azure.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrering av virtuella datorer från Amazon AWS till Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Se även följande resurser för att du lär dig mer om Azure Storage- och virtuella datorer i Azure:

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
