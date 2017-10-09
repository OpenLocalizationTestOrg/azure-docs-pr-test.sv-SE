---
title: aaaTest redundans tooAzure i Site Recovery | Microsoft Docs
description: "Lär dig mer om att köra ett redundanstest från lokala tooAzure"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: fa0a93f409cc9f2c2c06c2d91c65971dc90c507f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test--failover-tooazure-in-site-recovery"></a>Testa redundans tooAzure i Site Recovery



Den här artikeln innehåller information och instruktioner för att göra ett redundanstest eller en DR-test av virtuella datorer och fysiska servrar som skyddas med Site Recovery med Azure som hello återställningsplatsen.

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Testa redundans är kör toovalidate din replikeringsstrategi för eller utför en katastrofåterställning återställningsgranskning utan avbrott eller dataförlust. Gör ett redundanstest har inte någon effekt på hello pågående replikering eller i din produktionsmiljö. Testa redundans kan utföras på en virtuell dator eller en [återställningsplanen](site-recovery-create-recovery-plans.md). När du utlöser ett redundanstest måste toospecify hello toowhich nätverkstest virtuella datorer ska ansluta till. När ett redundanstest utlöses, du kan följa förloppet i hello **jobb** sidan.  


## <a name="supported-scenarios"></a>Scenarier som stöds
Testa redundans stöds i alla distributionsscenarier än [äldre VMware plats tooAzure](site-recovery-vmware-to-azure-classic-legacy.md). Testa redundans stöds inte även när den virtuella datorn misslyckades under tooAzure.  


## <a name="run-a-test-failover"></a>Köra ett redundanstest
Den här proceduren beskriver hur toorun ett redundanstest för en återställning planerar. Alternativt kan du också köra testa redundans för en enda dator med hjälp av hello lämpligt alternativ på den.

![Testa redundans](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Välj **Återställningsplaner** > *recoveryplan_name*. Klicka på **Redundanstestningen**.
1. Välj en **återställningspunkt** toofailover till. Du kan använda något av följande alternativ för hello:
    1.  **Senaste bearbetas**: det här alternativet flyttas över alla virtuella datorer i hello recovery plan toohello senaste återställningspunkten som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för hello senaste bearbetade återställningspunkten. Om du gör redundans för en återställningsplan går tooindividual virtuell dator och titta på **senaste återställningspunkter** panelen tooget informationen. Ingen tid läggs tooprocess hello obearbetade data, det här alternativet ger ett alternativ för låga RTO (mål) växling vid fel.
    1.  **Senaste programkonsekventa**: det här alternativet flyttas över alla virtuella datorer i hello recovery plan toohello senaste programkonsekvent återställningspunkt som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för hello senaste programkonsekventa återställningspunkten. Om du gör redundans för en återställningsplan går tooindividual virtuell dator och titta på **senaste återställningspunkter** panelen tooget informationen.
    1.  **Senaste**: det här alternativet först bearbetar alla hello-data som har skickats tooSite Recovery service toocreate en återställningspunkt för varje virtuell dator innan redundansväxlingen tooit. Det här alternativet får hello lägsta RPO (mål för återställningspunkt) som hello virtuell dator skapas efter växling vid fel måste alla hello data som har replikerats tooSite Recovery-tjänsten när hello redundans utlöstes.
    1.  **Senaste multi-VM bearbetas**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med konsekvens för flera på. Virtuella datorer som ingår i en grupp redundans toohello senaste vanliga multi-VM konsekvent replikeringsåterställning punkt. Andra virtuella datorer redundans tootheir senaste bearbetade återställningspunkten.  
    1.  **Senaste multi-VM programkonsekventa**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med multi-VM konsekvenskontroll på. Virtuella datorer som är en del av en replikering gruppera redundans toohello senaste vanliga multi-VM programkonsekvent återställningspunkt. Andra virtuella datorer redundans tootheir senaste programkonsekventa återställningspunkter. 
    1.  **Anpassad**: Om du gör testa redundans för en virtuell dator, så du kan använda alternativet toofailover tooa viss återställningspunkten.
1. Välj en **virtuella Azure-nätverket**: Ange ett virtuellt Azure-nätverk där de virtuella testdatorer som hello skulle skapas. Site Recovery försöker toocreate test virtuella datorer i ett undernät med samma namn och använder hello samma IP-adress som som angetts i **beräknings- och nätverksinställningar** inställningarna för hello virtuell dator. Om undernät med samma namn inte finns i hello Azure-nätverket för testning av redundans, och sedan testa virtuell dator skapas i hello första undernätet i alfabetisk ordning. Om samma IP inte är tillgänglig på undernätet hello hämtar virtuell dator en annan IP-adress som är tillgängliga i hello undernät. Läs det här avsnittet för [mer information](#creating-a-network-for-test-failover)
1. Om du växlar över tooAzure och datakryptering är aktiverat i **krypteringsnyckeln** väljer hello-certifikat som utfärdades när du har aktiverat datakryptering under installationen av providern. Du kan ignorera det här steget om du inte har aktiverat kryptering på hello virtuell dator.
1. Följa redundansförloppet på hello **jobb** fliken. Du ska kunna toosee hello testdatorn replik i hello Azure-portalen.
1. tooinitiate en RDP-anslutning på den virtuella datorn hello måste för[lägga till en offentlig IP-adress](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) hello nätverksgränssnittet för hello misslyckades över virtuella datorn. Om du misslyckas över tooa klassiska virtuella datorn, måste du för[lägga till en slutpunkt](../virtual-machines/windows/classic/setup-endpoints.md) port 3389
1. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar** spela in och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.


> [!TIP]
> Site Recovery försöker toocreate test virtuella datorer i ett undernät med samma namn och använder hello samma IP-adress som som angetts i **beräknings- och nätverksinställningar** inställningarna för hello virtuell dator. Om undernät med samma namn inte finns i hello Azure-nätverket för testning av redundans, och sedan testa virtuell dator skapas i hello första undernätet i alfabetisk ordning. Om IP-Adressen hello mål är en del av hello valt undernät, försöker platsåterställning toocreate hello testa redundans virtuell dator använder hello mål-IP-adress. Om IP-Adressen hello mål inte är del av hello valt undernät hämtar testa redundans virtuell dator skapas använder alla tillgängliga IP-adresser i hello valt undernät.
>
>

## <a name="test-failover-job"></a>Testjobb för växling vid fel

![Testa redundans](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

När ett redundanstest utlöses omfattar följande steg:

1. Kravkontroll: det här steget säkerställer att alla krav som gäller för växling vid fel är uppfyllda.
1. Redundans: Det här steget bearbetar hello data och gör den redo så att en virtuell Azure-dator kan skapas ur den. Om du har valt **senaste** återställningspunkten det här steget skapar en återställningspunkt från hello data som har skickats toohello service.
1. Starta: Det här steget skapar en virtuell Azure-dator med hjälp av hello data bearbetas i hello föregående steg.

## <a name="time-taken-for-failover"></a>Tidsåtgång för redundans

I vissa fall kräver redundans för virtuella datorer ett extra steg som tar vanligtvis cirka 8 too10 minuter toocomplete. Dessa fall är följande:

* Med en version av hello mobilitetstjänsten som är äldre än 9.8 virtuella VMware-datorer
* Fysiska servrar
* VMware Linux virtuella datorer
* Hyper-V virtuella datorer som skyddas som fysiska servrar
* VMware-datorer där följande drivrutiner inte finns som startdrivrutiner
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* Virtuella VMware-datorer som inte har DHCP-tjänsten aktiveras oavsett om de använder DHCP eller statiska IP-adresser

I alla hello andra fall detta mellanliggande steg krävs inte och hello tidsåtgång för hello redundans är betydligt lägre.


## <a name="creating-a-network-for-test-failover"></a>Skapa ett nätverk för att testa redundans
Vi rekommenderar att du väljer ett nätverk som är isolerad från nätverket produktion recovery plats som du angav i när du gör ett redundanstest **beräknings- och nätverksinställningar** inställningar för hello virtuell dator. Som standard när du skapar en Azure-nätverk, är isolerad från andra nätverk. Det här nätverket ska efterlikna produktionsnätverket:

1. Testa nätverk ska ha samma antal undernät som i produktionsnätverket och med samma namn som de hello undernät i produktion hello.
1. Testnätverket ska använda hello samma IP-adressintervall som produktionsnätverket.
1. Uppdatera hello DNS hello Testnätverket som hello IP-adress som du gav som mål-IP för hello DNS virtuella datorn under **beräknings- och nätverksinställningar** inställningar. Gå igenom [redundanstestning för active directory](site-recovery-active-directory.md#test-failover-considerations) mer information.


## <a name="test-failover-tooa-production-network-on-recovery-site"></a>Testa redundans tooa produktionsnätverk på återställningsplatsen
Vi rekommenderar att du väljer ett nätverk som skiljer sig från nätverket produktion recovery plats som du angav i när du gör ett redundanstest **beräknings- och nätverksinställningar** inställningar för hello virtuell dator. Men om du verkligen toovalidate end tooend nätverksanslutning i en misslyckad Observera följande punkter hello över virtuella datorn:

1. Kontrollera att hello den primära virtuella datorn är avstängd när du gör hello testa redundans. Om du inte gör det finns två virtuella datorer med hello samma identitet som körs i hello samma nätverk på hello samtidigt och som kan leda till tooundesired konsekvenser.
1. Alla ändringar du gör i hello testa redundans virtuella datorer skulle gå förlorade när du rensa hello redundans virtuella testdatorer. De här ändringarna inte replikeras tillbaka toohello primära virtuella datorn.
1. Det här kan du göra testning leder tooa driftstopp för dina produktionsprogram. Användare av programmet hello ska uppmanas toonot Använd hello programmet när hello DR detaljgranska pågår.  



## <a name="prepare-active-directory-and-dns"></a>Förbered Active Directory och DNS
toorun ett redundanstest för program, testa, behöver du en kopia av Active Directory-produktionsmiljö hello i din testmiljö. Gå igenom [redundanstestning för active directory](site-recovery-active-directory.md#test-failover-considerations) mer information.

## <a name="prepare-tooconnect-tooazure-vms-after-failover"></a>Förbereda tooconnect tooAzure virtuella datorer efter redundans

Om du vill tooconnect tooAzure virtuella datorer med RDP efter en växling vid fel, kontrollera att du hello åtgärder som sammanfattas i hello tabell.

**Växling vid fel** | **Plats** | **Åtgärder**
--- | --- | ---
**Azure virtuell dator som kör Windows** | På den lokala datorn före redundans | tooaccess hello Azure VM över hello internet, aktiverar du RDP, se till att TCP och UDP-regler har lagts till för hello **offentliga**, och att RDP tillåts i **Windows-brandväggen** > **tillåten Appar**, för alla profiler.<br/><br/> tooaccess via en plats-till-plats-anslutning aktiverar du RDP på hello datorn och se till att RDP tillåts i hello **Windows-brandväggen** -> **tillåtna appar och funktioner** för  **Domänen och privat** nätverk.<br/><br/>  Se till att hello operativsystemets SAN-principen anges för**OnlineAll**. [Läs mer](https://support.microsoft.com/kb/3031135).<br/><br/> Kontrollera att det finns ingen Windows-uppdateringar som väntar på hello virtuella datorn när du utlösa redundansväxling. Windows update kan starta när du växling vid fel och du inte kommer att kunna toologin toohello virtuella förrän hello uppdateringen är klar. <br/><br/>
**Azure virtuell dator som kör Windows** | På Azure VM efter växling vid fel | För en klassisk virtuell dator [lägga till en offentlig slutpunkt](../virtual-machines/windows/classic/setup-endpoints.md) för hello RDP-protokollet (port 3389)<br/><br/>  För en virtuell dator Resource Manager [lägga till en offentlig IP-adress](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) på den.<br/><br/> hello regler för nätverkssäkerhetsgrupper på hello redundansväxlats VM och hello Azure-undernät toowhich den ansluts måste tooallow inkommande anslutningar toohello RDP-porten.<br/><br/> För en virtuell dator Resource Manager kan du **starta diagnostik** toolook på en skärmbild av hello virtuell dator<br/><br/> Om du inte kan ansluta Kontrollera virtuella datorn körs hello och titta på dessa [felsökningstips](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).<br/><br/>
**Azure VM som kör Linux** | På den lokala datorn före redundans | Se till att hello Secure Shell-tjänsten på hello Azure VM anges toostart automatiskt på datorn startar.<br/><br/> Kontrollera att brandväggsreglerna tillåter en SSH-anslutning tooit.
**Azure VM som kör Linux** | Azure VM efter växling vid fel | hello regler för nätverkssäkerhetsgrupper på hello redundansväxlats VM och hello Azure-undernät toowhich den ansluts måste tooallow inkommande anslutningar toohello SSH-port.<br/><br/> För en klassisk virtuell dator [lägga till en offentlig slutpunkt](../virtual-machines/windows/classic/setup-endpoints.md) ska skapas, tooallow inkommande anslutningar på hello SSH-porten (TCP-port 22 som standard).<br/><br/> För en virtuell dator Resource Manager [lägga till en offentlig IP-adress](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) på den.<br/><br/> För en virtuell dator Resource Manager kan du **starta diagnostik** toolook på en skärmbild av hello virtuell dator<br/><br/>



## <a name="next-steps"></a>Nästa steg
När du har har försökt testa redundans kan du göra en [redundans](site-recovery-failover.md).
