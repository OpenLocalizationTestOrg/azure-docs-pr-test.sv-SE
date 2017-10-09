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
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a><span data-ttu-id="3b36c-103">Felsöka replikeringsproblem i lokal VMware/fysisk server</span><span class="sxs-lookup"><span data-stu-id="3b36c-103">Troubleshoot on-premises VMware/Physical server replication issues</span></span>
<span data-ttu-id="3b36c-104">Du kan få ett visst felmeddelande när du skyddar din virtuella VMware-datorer eller fysiska servrar med hjälp av Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3b36c-104">You may receive a specific error message when protecting your VMware virtual machines or physical servers using Azure Site Recovery.</span></span> <span data-ttu-id="3b36c-105">Det här artikeln beskriver några av hello vanligaste felmeddelandena uppstod, tillsammans med felsökning steg tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="3b36c-105">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span>


## <a name="initial-replication-is-stuck-at-0"></a><span data-ttu-id="3b36c-106">Den inledande replikeringen har fastnat på % 0</span><span class="sxs-lookup"><span data-stu-id="3b36c-106">Initial replication is stuck at 0%</span></span>
<span data-ttu-id="3b36c-107">De flesta hello inledande replikeringsfel som vi stöta på support är på grund av problem med tooconnectivity mellan serverprocessen till källservern eller processen server till Azure.</span><span class="sxs-lookup"><span data-stu-id="3b36c-107">Most of hello initial replication failures that we encounter at support are due tooconnectivity issues between source server-to-process server or process server-to-Azure.</span></span>
<span data-ttu-id="3b36c-108">I de flesta fall kan du själv felsökning av dessa problem genom att följa hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3b36c-108">For most cases, you can self troubleshoot these issues by following hello steps listed below.</span></span>

###<a name="check-hello-following-on-source-machine"></a><span data-ttu-id="3b36c-109">Kontrollera följande hello på KÄLLDATORN</span><span class="sxs-lookup"><span data-stu-id="3b36c-109">Check hello following on SOURCE MACHINE</span></span>
* <span data-ttu-id="3b36c-110">Använd Telnet tooping hello Processervern med https-porten (standard 9443) från källservern datorn kommandoraden, enligt nedan toosee om det finns några problem med nätverksanslutningen eller brandväggsport allvarliga problem.</span><span class="sxs-lookup"><span data-stu-id="3b36c-110">From Source Server machine command line, use Telnet tooping hello Process Server with https port (default 9443) as shown below toosee if there are any network connectivity issues or firewall port blocking issues.</span></span>
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > <span data-ttu-id="3b36c-111">Använda Telnet kan inte använda PING tootest anslutning.</span><span class="sxs-lookup"><span data-stu-id="3b36c-111">Use Telnet, don’t use PING tootest connectivity.</span></span>  <span data-ttu-id="3b36c-112">Om Telnet inte är installerad, följer du hello steg listan [här](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="3b36c-112">If Telnet is not installed, follow hello steps list [here](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span></span>

<span data-ttu-id="3b36c-113">Om tooconnect tillåter inkommande port 9443 på hello Processervern och kontrollera om hello avslutas fortfarande problem.</span><span class="sxs-lookup"><span data-stu-id="3b36c-113">If unable tooconnect, allow inbound port 9443 on hello Process Server and check if hello problem still exits.</span></span> <span data-ttu-id="3b36c-114">Det har uppstått tillfällen där processervern har bakom DMZ som orsakade det här problemet.</span><span class="sxs-lookup"><span data-stu-id="3b36c-114">There has been some cases where process server was behind DMZ, which was causing this problem.</span></span>

* <span data-ttu-id="3b36c-115">Kontrollera hello status för tjänsten `InMage Scout VX Agent – Sentinel/OutpostStart` om den inte körs och kontrollera om hello problemet fortfarande kvarstår.</span><span class="sxs-lookup"><span data-stu-id="3b36c-115">Check hello status of service `InMage Scout VX Agent – Sentinel/OutpostStart` if it is not running and check if hello problem still exists.</span></span>   
 
###<a name="check-hello-following-on-process-server"></a><span data-ttu-id="3b36c-116">Kontrollera följande hello på PROCESSERVERN</span><span class="sxs-lookup"><span data-stu-id="3b36c-116">Check hello following on PROCESS SERVER</span></span>

* <span data-ttu-id="3b36c-117">**Kontrollera om processervern aktivt push-överföring av data tooAzure**</span><span class="sxs-lookup"><span data-stu-id="3b36c-117">**Check if process server is actively pushing data tooAzure**</span></span> 

<span data-ttu-id="3b36c-118">Öppna hello Aktivitetshanteraren (tryck på Ctrl-Skift-Esc) från Processervern datorn.</span><span class="sxs-lookup"><span data-stu-id="3b36c-118">From Process Server machine, open hello Task Manager (press Ctrl-Shift-Esc ).</span></span> <span data-ttu-id="3b36c-119">På fliken toohello prestanda och klickar på öppna Resursövervakaren länk.</span><span class="sxs-lookup"><span data-stu-id="3b36c-119">Go toohello Performance tab and click ‘Open Resource Monitor’ link.</span></span> <span data-ttu-id="3b36c-120">Från hanteraren för filserverresurser, gå tooNetwork fliken. Kontrollera om cbengine.exe i ”processer med nätverksaktivitet” aktivt skickar stora mängder (i MB) data.</span><span class="sxs-lookup"><span data-stu-id="3b36c-120">From Resource Manager, go tooNetwork tab. Check if cbengine.exe in ‘Processes with Network Activity’ is actively sending large volume (in Mbs) of data.</span></span>

![Aktivera replikering](./media/site-recovery-protection-common-errors/cbengine.png)

<span data-ttu-id="3b36c-122">Om inte så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="3b36c-122">If not follow hello steps listed below:</span></span>

* <span data-ttu-id="3b36c-123">**Kontrollera om processervern är kan tooconnect Azure Blob**: Välj och kontrollera cbengine.exe tooview hello TCP-anslutningar toosee om det finns en anslutning från processen tooAzure Storage blob-Serveradress.</span><span class="sxs-lookup"><span data-stu-id="3b36c-123">**Check if Process server is able tooconnect Azure Blob**: Select and check cbengine.exe tooview hello ‘TCP Connections’ toosee if there is connectivity from Process server tooAzure Storage blob URL.</span></span>

![Aktivera replikering](./media/site-recovery-protection-common-errors/rmonitor.png)

<span data-ttu-id="3b36c-125">Om det inte gå tooControl panelen > tjänster, kontrollerar du om hello följande tjänster är igång:</span><span class="sxs-lookup"><span data-stu-id="3b36c-125">If not then go tooControl Panel > Services, check if hello following services are up and running:</span></span>

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
<span data-ttu-id="3b36c-126">(Re) Starta alla tjänster som inte körs och kontrollera om hello problemet fortfarande kvarstår.</span><span class="sxs-lookup"><span data-stu-id="3b36c-126">(Re)Start any service which is not running and check if hello problem still exists.</span></span>

* <span data-ttu-id="3b36c-127">**Kontrollera om processervern är kan tooconnect tooAzure offentliga IP-adress via port 443**</span><span class="sxs-lookup"><span data-stu-id="3b36c-127">**Check if Process server is able tooconnect tooAzure Public IP address using port 443**</span></span>

<span data-ttu-id="3b36c-128">Öppna hello senaste CBEngineCurr.errlog från `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` och Sök efter: 443 och anslutningen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="3b36c-128">Open hello latest CBEngineCurr.errlog from `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` and search for :443  and connection attempt failed.</span></span>

![Aktivera replikering](./media/site-recovery-protection-common-errors/logdetails1.png)

<span data-ttu-id="3b36c-130">Om det uppstår problem kan från Processervern kommandorad använder telnet tooping din Azure offentlig IP-adress (maskeras i ovanstående bild) hittades i hello CBEngineCurr.currLog via port 443.</span><span class="sxs-lookup"><span data-stu-id="3b36c-130">If there are issues, then from Process Server command line, use telnet tooping your Azure Public IP address (masked in above image) found in hello CBEngineCurr.currLog using port 443.</span></span>

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
<span data-ttu-id="3b36c-131">Om du tooconnect, kontrollerar du om hello Åtkomstproblemet på grund av toofirewall eller proxyserver som beskrivs i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3b36c-131">If you are unable tooconnect, then check if hello access issue is due toofirewall or Proxy as described in next step.</span></span>


* <span data-ttu-id="3b36c-132">**Kontrollera om IP-adressbaserade brandväggen på processervern inte blockerar åtkomst**: Om du använder en IP-adressbaserade brandväggsregler på hello-servern och hämta sedan hello fullständig lista över Microsoft Azure Datacenter IP-intervall från [här ](https://www.microsoft.com/download/details.aspx?id=41653) och Lägg till dem tooyour brandväggen configuration tooensure de tillåter kommunikation tooAzure (och hello HTTPS (443)-port).</span><span class="sxs-lookup"><span data-stu-id="3b36c-132">**Check if IP address-based firewall on Process server are not blocking access**: If you are using an IP address-based firewall rules on hello server, then download hello complete list of Microsoft Azure Datacenter IP Ranges from [here](https://www.microsoft.com/download/details.aspx?id=41653) and add them tooyour firewall configuration tooensure they allow communication tooAzure (and hello HTTPS (443) port).</span></span>  <span data-ttu-id="3b36c-133">Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).</span><span class="sxs-lookup"><span data-stu-id="3b36c-133">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

* <span data-ttu-id="3b36c-134">**Kontrollera om URL-baserade processervern-brandväggen inte blockerar åtkomsten**: Om du använder en URL-Adressen baserat brandväggsregler på hello-servern, kontrollera hello följande URL: er läggs toofirewall konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3b36c-134">**Check if URL-based firewall on Process server is not blocking access**:  If you are using a URL based firewall rules on hello server, ensure hello following URLs are added toofirewall configuration.</span></span> 
     
  <span data-ttu-id="3b36c-135">`*.accesscontrol.windows.net:` Används för Access Control och identitetshantering</span><span class="sxs-lookup"><span data-stu-id="3b36c-135">`*.accesscontrol.windows.net:` Used for access control and identity management</span></span>

  <span data-ttu-id="3b36c-136">`*.backup.windowsazure.com:` Används för överföring av replikeringsdata och dirigering</span><span class="sxs-lookup"><span data-stu-id="3b36c-136">`*.backup.windowsazure.com:` Used for replication data transfer and orchestration</span></span>

  <span data-ttu-id="3b36c-137">`*.blob.core.windows.net:`Används för åtkomst toohello replikerade lagringskonto som lagrar data</span><span class="sxs-lookup"><span data-stu-id="3b36c-137">`*.blob.core.windows.net:` Used for access toohello storage account that stores replicated data</span></span>

  <span data-ttu-id="3b36c-138">`*.hypervrecoverymanager.windowsazure.com:` Används för åtgärder för replikeringshantering och dirigering</span><span class="sxs-lookup"><span data-stu-id="3b36c-138">`*.hypervrecoverymanager.windowsazure.com:` Used for replication management operations and orchestration</span></span>

  <span data-ttu-id="3b36c-139">`time.nist.gov`och `time.windows.com`: används toocheck tidssynkronisering mellan system och global tid.</span><span class="sxs-lookup"><span data-stu-id="3b36c-139">`time.nist.gov` and `time.windows.com`: Used toocheck time synchronization between system and global time.</span></span>

<span data-ttu-id="3b36c-140">URL: er för **Azure Government molnet**:</span><span class="sxs-lookup"><span data-stu-id="3b36c-140">URLs for **Azure Government Cloud**:</span></span>

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* <span data-ttu-id="3b36c-141">**Kontrollera om proxyinställningarna på processervern inte blockerar åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="3b36c-141">**Check if Proxy Settings on Process server are not blocking access**.</span></span>  <span data-ttu-id="3b36c-142">Säkerställ att hello Proxyservernamn matchar av hello DNS-server om du använder en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="3b36c-142">If you are using a Proxy Server, ensure hello proxy server name is resolving by hello DNS server.</span></span>
<span data-ttu-id="3b36c-143">toocheck angivna när hello Configuration Server-installationen.</span><span class="sxs-lookup"><span data-stu-id="3b36c-143">toocheck what you have provided at hello time of Configuration Server setup.</span></span> <span data-ttu-id="3b36c-144">Gå tooregistry nyckel</span><span class="sxs-lookup"><span data-stu-id="3b36c-144">Go tooregistry key</span></span>

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

<span data-ttu-id="3b36c-145">Nu ska du se till att hello samma inställningar som används av Azure Site Recovery agent toosend data.</span><span class="sxs-lookup"><span data-stu-id="3b36c-145">Now ensure that hello same settings are being used by Azure Site Recovery agent toosend data.</span></span>
<span data-ttu-id="3b36c-146">Sök Microsoft Azure Backup</span><span class="sxs-lookup"><span data-stu-id="3b36c-146">Search Microsoft Azure  Backup</span></span> 

![Aktivera replikering](./media/site-recovery-protection-common-errors/mab.png)

<span data-ttu-id="3b36c-148">Öppna den och klicka på åtgärden > ändra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b36c-148">Open it and click on Action > Change Properties.</span></span> <span data-ttu-id="3b36c-149">Du bör se hello proxyadress som ska vara densamma som visas av hello registerinställningar under fliken proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="3b36c-149">Under Proxy Configuration tab, you should see hello proxy address, which should be same as shown by hello registry settings.</span></span> <span data-ttu-id="3b36c-150">Om inte, ändra det toohello samma adress.</span><span class="sxs-lookup"><span data-stu-id="3b36c-150">If not, please change it toohello same address.</span></span>

![Aktivera replikering](./media/site-recovery-protection-common-errors/mabproxy.png)

* <span data-ttu-id="3b36c-152">**Kontrollera om begränsning av bandbredd inte är begränsad på processervern**: öka hello bandbredd och kontrollera om hello problemet fortfarande kvarstår.</span><span class="sxs-lookup"><span data-stu-id="3b36c-152">**Check if Throttle bandwidth is not constrained on Process server**:  Increase hello bandwidth  and check if hello problem still exists.</span></span>

##<a name="next-steps"></a><span data-ttu-id="3b36c-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b36c-153">Next steps</span></span>
<span data-ttu-id="3b36c-154">Om du behöver mer hjälp för att sedan skicka frågan för[ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3b36c-154">If you need more help, then post your query too[ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span></span> <span data-ttu-id="3b36c-155">Vi har en aktiv community och en av våra tekniker kommer att kunna tooassist du.</span><span class="sxs-lookup"><span data-stu-id="3b36c-155">We have an active community and one of our engineers will be able tooassist you.</span></span>
