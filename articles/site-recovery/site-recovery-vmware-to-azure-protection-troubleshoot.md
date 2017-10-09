---
title: aaaTroubleshoot skydd fel VMware/fysiska tooAzure | Microsoft Docs
description: "Den här artikeln beskriver hello vanliga VMware datorn replikeringsfel och hur tootroubleshoot dem."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/26/2017
ms.author: asgang
ms.openlocfilehash: b821e9aa2610482ba1900645fb75e75744dc442f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Felsöka replikeringsproblem i lokal VMware/fysisk server
Du kan få ett visst felmeddelande när du skyddar din virtuella VMware-datorer eller fysiska servrar med hjälp av Azure Site Recovery. Det här artikeln beskriver några av hello vanligaste felmeddelandena uppstod, tillsammans med felsökning steg tooresolve dem.


## <a name="initial-replication-is-stuck-at-0"></a>Den inledande replikeringen har fastnat på % 0
De flesta hello inledande replikeringsfel som vi stöta på support är på grund av problem med tooconnectivity mellan serverprocessen till källservern eller processen server till Azure.
I de flesta fall kan du själv felsökning av dessa problem genom att följa hello stegen nedan.

###<a name="check-hello-following-on-source-machine"></a>Kontrollera följande hello på KÄLLDATORN
* Använd Telnet tooping hello Processervern med https-porten (standard 9443) från källservern datorn kommandoraden, enligt nedan toosee om det finns några problem med nätverksanslutningen eller brandväggsport allvarliga problem.
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > Använda Telnet kan inte använda PING tootest anslutning.  Om Telnet inte är installerad, följer du hello steg listan [här](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Om tooconnect tillåter inkommande port 9443 på hello Processervern och kontrollera om hello avslutas fortfarande problem. Det har uppstått tillfällen där processervern har bakom DMZ som orsakade det här problemet.

* Kontrollera hello status för tjänsten `InMage Scout VX Agent – Sentinel/OutpostStart` om den inte körs och kontrollera om hello problemet fortfarande kvarstår.   
 
###<a name="check-hello-following-on-process-server"></a>Kontrollera följande hello på PROCESSERVERN

* **Kontrollera om processervern aktivt push-överföring av data tooAzure** 

Öppna hello Aktivitetshanteraren (tryck på Ctrl-Skift-Esc) från Processervern datorn. På fliken toohello prestanda och klickar på öppna Resursövervakaren länk. Från hanteraren för filserverresurser, gå tooNetwork fliken. Kontrollera om cbengine.exe i ”processer med nätverksaktivitet” aktivt skickar stora mängder (i MB) data.

![Aktivera replikering](./media/site-recovery-protection-common-errors/cbengine.png)

Om inte så hello nedan:

* **Kontrollera om processervern är kan tooconnect Azure Blob**: Välj och kontrollera cbengine.exe tooview hello TCP-anslutningar toosee om det finns en anslutning från processen tooAzure Storage blob-Serveradress.

![Aktivera replikering](./media/site-recovery-protection-common-errors/rmonitor.png)

Om det inte gå tooControl panelen > tjänster, kontrollerar du om hello följande tjänster är igång:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
(Re) Starta alla tjänster som inte körs och kontrollera om hello problemet fortfarande kvarstår.

* **Kontrollera om processervern är kan tooconnect tooAzure offentliga IP-adress via port 443**

Öppna hello senaste CBEngineCurr.errlog från `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` och Sök efter: 443 och anslutningen misslyckades.

![Aktivera replikering](./media/site-recovery-protection-common-errors/logdetails1.png)

Om det uppstår problem kan från Processervern kommandorad använder telnet tooping din Azure offentlig IP-adress (maskeras i ovanstående bild) hittades i hello CBEngineCurr.currLog via port 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Om du tooconnect, kontrollerar du om hello Åtkomstproblemet på grund av toofirewall eller proxyserver som beskrivs i nästa steg.


* **Kontrollera om IP-adressbaserade brandväggen på processervern inte blockerar åtkomst**: Om du använder en IP-adressbaserade brandväggsregler på hello-servern och hämta sedan hello fullständig lista över Microsoft Azure Datacenter IP-intervall från [här ](https://www.microsoft.com/download/details.aspx?id=41653) och Lägg till dem tooyour brandväggen configuration tooensure de tillåter kommunikation tooAzure (och hello HTTPS (443)-port).  Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).

* **Kontrollera om URL-baserade processervern-brandväggen inte blockerar åtkomsten**: Om du använder en URL-Adressen baserat brandväggsregler på hello-servern, kontrollera hello följande URL: er läggs toofirewall konfiguration. 
     
  `*.accesscontrol.windows.net:` Används för Access Control och identitetshantering

  `*.backup.windowsazure.com:` Används för överföring av replikeringsdata och dirigering

  `*.blob.core.windows.net:`Används för åtkomst toohello replikerade lagringskonto som lagrar data

  `*.hypervrecoverymanager.windowsazure.com:` Används för åtgärder för replikeringshantering och dirigering

  `time.nist.gov`och `time.windows.com`: används toocheck tidssynkronisering mellan system och global tid.

URL: er för **Azure Government molnet**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* **Kontrollera om proxyinställningarna på processervern inte blockerar åtkomst**.  Säkerställ att hello Proxyservernamn matchar av hello DNS-server om du använder en proxyserver.
toocheck angivna när hello Configuration Server-installationen. Gå tooregistry nyckel

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Nu ska du se till att hello samma inställningar som används av Azure Site Recovery agent toosend data.
Sök Microsoft Azure Backup 

![Aktivera replikering](./media/site-recovery-protection-common-errors/mab.png)

Öppna den och klicka på åtgärden > ändra egenskaper. Du bör se hello proxyadress som ska vara densamma som visas av hello registerinställningar under fliken proxykonfiguration. Om inte, ändra det toohello samma adress.

![Aktivera replikering](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Kontrollera om begränsning av bandbredd inte är begränsad på processervern**: öka hello bandbredd och kontrollera om hello problemet fortfarande kvarstår.

##<a name="next-steps"></a>Nästa steg
Om du behöver mer hjälp för att sedan skicka frågan för[ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Vi har en aktiv community och en av våra tekniker kommer att kunna tooassist du.
