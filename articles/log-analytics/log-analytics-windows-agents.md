---
title: aaaConnect Windows-datorer tooAzure Log Analytics | Microsoft Docs
description: "Den här artikeln visar hello steg tooconnect hello Windows-datorer i din lokala infrastruktur toohello logganalys-tjänsten med hjälp av en anpassad version av hello Microsoft Monitoring Agent (MMA)."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a><span data-ttu-id="9014b-103">Ansluta Windows-datorer toohello Log Analytics-tjänsten i Azure</span><span class="sxs-lookup"><span data-stu-id="9014b-103">Connect Windows computers toohello Log Analytics service in Azure</span></span>

<span data-ttu-id="9014b-104">Den här artikeln visar hello steg tooconnect Windows-datorer i din lokala infrastruktur tooOMS arbetsytor med hjälp av en anpassad version av hello Microsoft Monitoring Agent (MMA).</span><span class="sxs-lookup"><span data-stu-id="9014b-104">This article shows hello steps tooconnect Windows computers in your on-premises infrastructure tooOMS workspaces by using a customized version of hello Microsoft Monitoring Agent (MMA).</span></span> <span data-ttu-id="9014b-105">Du behöver tooinstall och ansluta agenter för alla hello datorer som du vill tooonboard för toosend data toohello Log Analytics-tjänsten och tooview och agera på dessa data.</span><span class="sxs-lookup"><span data-stu-id="9014b-105">You need tooinstall and connect agents for all of hello computers that you want tooonboard in order for them toosend data toohello Log Analytics service and tooview and act on that data.</span></span> <span data-ttu-id="9014b-106">Varje agent kan rapportera toomultiple arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="9014b-106">Each agent can report toomultiple workspaces.</span></span>

<span data-ttu-id="9014b-107">Du kan installera agenter med installationsprogrammet, kommandorad, eller med önskad tillstånd Configuration (DSC) i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9014b-107">You can install agents using Setup, command line, or with Desired State Configuration (DSC) in Azure Automation.</span></span>  

>[!NOTE]
<span data-ttu-id="9014b-108">För virtuella datorer som körs i Azure, kan du förenkla installationen med hjälp av hello [tillägg för virtuell dator](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9014b-108">For virtual machines running in Azure, you can simplify installation by using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="9014b-109">På datorer med Internetanslutning använder hello agenten hello anslutning toohello Internet toosend data tooOMS.</span><span class="sxs-lookup"><span data-stu-id="9014b-109">On computers with Internet connectivity, hello agent uses hello connection toohello Internet toosend data tooOMS.</span></span> <span data-ttu-id="9014b-110">För datorer som inte har Internetanslutning kan du använda en proxy- eller hello [OMS Gateway](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="9014b-110">For computers that do not have Internet connectivity, you can use a proxy or hello [OMS Gateway](log-analytics-oms-gateway.md).</span></span>

<span data-ttu-id="9014b-111">Ansluta din Windows-datorer tooOMS är enkelt med tre enkla steg:</span><span class="sxs-lookup"><span data-stu-id="9014b-111">Connecting your Windows computers tooOMS is straightforward using three simple steps:</span></span>

1. <span data-ttu-id="9014b-112">Hämta installationsfilen för hello agent från hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="9014b-112">Download hello agent setup file from hello OMS portal</span></span>
2. <span data-ttu-id="9014b-113">Installera hello agent med hello-metod som du väljer</span><span class="sxs-lookup"><span data-stu-id="9014b-113">Install hello agent using hello method you choose</span></span>
3. <span data-ttu-id="9014b-114">Konfigurera hello agent eller Lägg till ytterligare arbetsytor, om det behövs</span><span class="sxs-lookup"><span data-stu-id="9014b-114">Configure hello agent or add additional workspaces, if necessary</span></span>

<span data-ttu-id="9014b-115">hello följande diagram visar hello förhållandet mellan din Windows-datorer och OMS när du har installerat och konfigurerat agenter.</span><span class="sxs-lookup"><span data-stu-id="9014b-115">hello following diagram shows hello relationship between your Windows computers and OMS after you’ve installed and configured agents.</span></span>

![OMS-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

<span data-ttu-id="9014b-117">Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, kan du konfigurera dina datorer tooconnect toohello OMS-Gateway.</span><span class="sxs-lookup"><span data-stu-id="9014b-117">If your IT security policies do not allow computers on your network tooconnect toohello Internet, you can configure your computers tooconnect toohello OMS Gateway.</span></span> <span data-ttu-id="9014b-118">Mer information och anvisningar om hur tooconfigure dina servrar toocommunicate via en OMS-Gateway toohello OMS-tjänst, se [ansluta datorer tooOMS med hello OMS Gateway](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="9014b-118">For more information and steps on how tooconfigure your servers toocommunicate through an OMS Gateway toohello OMS service, see [Connect computers tooOMS using hello OMS Gateway](log-analytics-oms-gateway.md).</span></span>

## <a name="system-requirements-and-required-configuration"></a><span data-ttu-id="9014b-119">Systemkrav och nödvändiga konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="9014b-119">System requirements and required configuration</span></span>
<span data-ttu-id="9014b-120">Granska följande information tooensure hello kraven hello innan du installerar eller distribuera agenter.</span><span class="sxs-lookup"><span data-stu-id="9014b-120">Before you install or deploy agents, review hello following details tooensure you meet hello requirements.</span></span>

- <span data-ttu-id="9014b-121">Du kan bara installera hello OMS MMA på datorer som kör Windows Server 2008 SP 1 eller senare eller Windows 7 SP1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9014b-121">You can only install hello OMS MMA on computers running Windows Server 2008 SP 1 or later or Windows 7 SP1 or later.</span></span>
- <span data-ttu-id="9014b-122">Du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9014b-122">You need an Azure subscription.</span></span>  <span data-ttu-id="9014b-123">Mer information finns i [Kom igång med logganalys](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9014b-123">For more information, see [Get started with Log Analytics](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="9014b-124">Alla Windows-datorer måste vara kan tooconnect toohello Internet med hjälp av HTTPS eller toohello OMS-Gateway.</span><span class="sxs-lookup"><span data-stu-id="9014b-124">Each Windows computer must be able tooconnect toohello Internet using HTTPS or toohello OMS Gateway.</span></span> <span data-ttu-id="9014b-125">Den här anslutningen kan vara direkt via en proxy eller via hello OMS-Gateway.</span><span class="sxs-lookup"><span data-stu-id="9014b-125">This connection can be direct, via a proxy, or through hello OMS Gateway.</span></span>
- <span data-ttu-id="9014b-126">Du kan installera hello OMS MMA på fristående datorer, servrar och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9014b-126">You can install hello OMS MMA on stand-alone computers, servers, and virtual machines.</span></span> <span data-ttu-id="9014b-127">Om du vill tooconnect Azure som värd för virtuella datorer tooOMS [ansluta Azure virtuella datorer tooLog Analytics](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9014b-127">If you want tooconnect Azure-hosted virtual machines tooOMS, see [Connect Azure virtual machines tooLog Analytics](log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="9014b-128">hello-agenten måste toouse TCP-port 443 för olika resurser.</span><span class="sxs-lookup"><span data-stu-id="9014b-128">hello agent needs toouse TCP port 443 for various resources.</span></span>

### <a name="network"></a><span data-ttu-id="9014b-129">Nätverk</span><span class="sxs-lookup"><span data-stu-id="9014b-129">Network</span></span>

<span data-ttu-id="9014b-130">För Windows-agenter tooconnect tooand registrera med hello OMS-tjänsten, måste de ha åtkomst toonetwork resurser, inklusive hello portnummer och URL: er för domänen.</span><span class="sxs-lookup"><span data-stu-id="9014b-130">For Windows agents tooconnect tooand register with hello OMS service, they must have access toonetwork resources, including hello port numbers and domain URLs.</span></span>

- <span data-ttu-id="9014b-131">För proxy-servrar behöver du tooensure som hello lämpliga proxyserver resurser konfigureras i agentinställningarna för.</span><span class="sxs-lookup"><span data-stu-id="9014b-131">For proxy servers, you need tooensure that hello appropriate proxy server resources are configured in agent settings.</span></span>
- <span data-ttu-id="9014b-132">För brandväggar som begränsar åtkomst toohello Internet du eller ditt nätverk tekniker behöver tooconfigure din brandvägg toopermit åtkomst tooOMS.</span><span class="sxs-lookup"><span data-stu-id="9014b-132">For firewalls that restrict access toohello Internet, you or your networking engineers need tooconfigure your firewall toopermit access tooOMS.</span></span> <span data-ttu-id="9014b-133">Ingen åtgärd krävs i agentinställningarna.</span><span class="sxs-lookup"><span data-stu-id="9014b-133">No action is needed in agent settings.</span></span>

<span data-ttu-id="9014b-134">hello följande tabell visar resurser som krävs för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="9014b-134">hello following table shows resources needed for communication.</span></span>

>[!NOTE]
><span data-ttu-id="9014b-135">Några av följande resurser hello nämnt åtgärdsinformation, som hette tidigare för Log Analytics.</span><span class="sxs-lookup"><span data-stu-id="9014b-135">Some of hello following resources mention Operational Insights, which was a previous name for Log Analytics.</span></span>

| <span data-ttu-id="9014b-136">Agentresurs</span><span class="sxs-lookup"><span data-stu-id="9014b-136">Agent Resource</span></span> | <span data-ttu-id="9014b-137">Portar</span><span class="sxs-lookup"><span data-stu-id="9014b-137">Ports</span></span> | <span data-ttu-id="9014b-138">Kringgå HTTPS-kontroll</span><span class="sxs-lookup"><span data-stu-id="9014b-138">Bypass HTTPS inspection</span></span> |
|---|---|---|
| <span data-ttu-id="9014b-139">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="9014b-139">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="9014b-140">443</span><span class="sxs-lookup"><span data-stu-id="9014b-140">443</span></span> | <span data-ttu-id="9014b-141">Ja</span><span class="sxs-lookup"><span data-stu-id="9014b-141">Yes</span></span> |
| <span data-ttu-id="9014b-142">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="9014b-142">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="9014b-143">443</span><span class="sxs-lookup"><span data-stu-id="9014b-143">443</span></span> | <span data-ttu-id="9014b-144">Ja</span><span class="sxs-lookup"><span data-stu-id="9014b-144">Yes</span></span> |
| <span data-ttu-id="9014b-145">*.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="9014b-145">*.blob.core.windows.net</span></span> | <span data-ttu-id="9014b-146">443</span><span class="sxs-lookup"><span data-stu-id="9014b-146">443</span></span> | <span data-ttu-id="9014b-147">Ja</span><span class="sxs-lookup"><span data-stu-id="9014b-147">Yes</span></span> |
| <span data-ttu-id="9014b-148">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="9014b-148">*.azure-automation.net</span></span> | <span data-ttu-id="9014b-149">443</span><span class="sxs-lookup"><span data-stu-id="9014b-149">443</span></span> | <span data-ttu-id="9014b-150">Ja</span><span class="sxs-lookup"><span data-stu-id="9014b-150">Yes</span></span> |



## <a name="download-hello-agent-setup-file-from-oms"></a><span data-ttu-id="9014b-151">Hämta installationsfilen för hello agent från OMS</span><span class="sxs-lookup"><span data-stu-id="9014b-151">Download hello agent setup file from OMS</span></span>
1. <span data-ttu-id="9014b-152">I hello OMS-portalen på hello **översikt** klickar du på hello **inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="9014b-152">In hello OMS portal, on hello **Overview** page, click hello **Settings** tile.</span></span>  <span data-ttu-id="9014b-153">Klicka på hello **anslutna källor** fliken hello överst.</span><span class="sxs-lookup"><span data-stu-id="9014b-153">Click hello **Connected Sources** tab at hello top.</span></span>  
    <span data-ttu-id="9014b-154">![Fliken för anslutna datakällor](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-154">![Connected Sources tab](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span></span>
2. <span data-ttu-id="9014b-155">Klicka på **Windows-servrar** och klicka sedan på **ladda ned Windows Agent** tillämpliga tooyour dator processor typen toodownload hello installationsfilen.</span><span class="sxs-lookup"><span data-stu-id="9014b-155">Click **Windows Servers** and then click **Download Windows Agent** applicable tooyour computer processor type toodownload hello setup file.</span></span>
3. <span data-ttu-id="9014b-156">På hello höger i **arbetsyte-ID**och klicka på Kopiera-ikonen hello klistra in hello-ID i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="9014b-156">On hello right of **Workspace ID**, click hello copy icon and paste hello ID into Notepad.</span></span>
4. <span data-ttu-id="9014b-157">På hello höger i **primärnyckel**och klicka på Kopiera-ikonen hello klistra in hello nyckel i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="9014b-157">On hello right of **Primary Key**, click hello copy icon and paste hello key into Notepad.</span></span>     

## <a name="install-hello-agent-using-setup"></a><span data-ttu-id="9014b-158">Installera hello-agenten med hjälp av installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="9014b-158">Install hello agent using setup</span></span>
1. <span data-ttu-id="9014b-159">Kör installationsprogrammet tooinstall hello agent på en dator som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="9014b-159">Run Setup tooinstall hello agent on a computer that you want toomanage.</span></span>
2. <span data-ttu-id="9014b-160">Klicka på välkomstsidan hello **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-160">On hello Welcome page, click **Next**.</span></span>
3. <span data-ttu-id="9014b-161">På sidan Licensvillkor för hello Läs hello licens och klicka på **jag accepterar**.</span><span class="sxs-lookup"><span data-stu-id="9014b-161">On hello License Terms page, read hello license and then click **I Agree**.</span></span>
4. <span data-ttu-id="9014b-162">Hello målmappen sida, ändra eller behålla hello standardinstallationsmappen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-162">On hello Destination Folder page, change or keep hello default installation folder and then click **Next**.</span></span>
5. <span data-ttu-id="9014b-163">Du kan välja tooconnect hello agent tooAzure logganalys (OMS), Operations Manager, eller lämna hello val tomt om du senare vill tooconfigure hello agent hello installationsalternativ för Agent på sidan.</span><span class="sxs-lookup"><span data-stu-id="9014b-163">On hello Agent Setup Options page, you can choose tooconnect hello agent tooAzure Log Analytics (OMS), Operations Manager, or you can leave hello choices blank if you want tooconfigure hello agent later.</span></span> <span data-ttu-id="9014b-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-164">Click **Next**.</span></span>   
    - <span data-ttu-id="9014b-165">Om du väljer tooconnect tooAzure logganalys (OMS) kan du klistra in hello **arbetsyte-ID** och **Arbetsytenyckel (primärnyckel)** som du kopierade i anteckningar i hello föregående proceduren och klicka sedan på  **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-165">If you chose tooconnect tooAzure Log Analytics (OMS), paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in hello previous procedure and then click **Next**.</span></span>  
        <span data-ttu-id="9014b-166">![Klistra in arbetsyte-ID och primärnyckel](./media/log-analytics-windows-agents/connect-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-166">![paste Workspace ID and Primary Key](./media/log-analytics-windows-agents/connect-workspace.png)</span></span>
    - <span data-ttu-id="9014b-167">Om du väljer tooconnect tooOperations Manager skriver hello **Hanteringsgruppnamn**, **hanteringsservern** namn, och **Hanteringsserverporten**, och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-167">If you chose tooconnect tooOperations Manager, type hello **Management Group Name**, **Management Server** name, and **Management Server Port**, and then click **Next**.</span></span> <span data-ttu-id="9014b-168">Hej Agentåtgärdskontot på sidan Välj hello lokalt systemkonto eller ett domänkonto som är lokala och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9014b-168">On hello Agent Action Account page, choose either hello Local System account or a local domain account and then click **Next**.</span></span>  
        <span data-ttu-id="9014b-169">![konfiguration av hanteringsgrupp](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agentåtgärdskontot](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-169">![management group configuration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent action account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span></span>

6. <span data-ttu-id="9014b-170">Hello redo tooInstall på sidan Granska dina val och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="9014b-170">On hello Ready tooInstall page, review your choices and then click **Install**.</span></span>
7. <span data-ttu-id="9014b-171">På hello konfigurationen slutförts, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="9014b-171">On hello Configuration completed successfully page, click **Finish**.</span></span>
8. <span data-ttu-id="9014b-172">När du är färdig hello **Microsoft Monitoring Agent** visas i **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="9014b-172">When complete, hello **Microsoft Monitoring Agent** appears in **Control Panel**.</span></span> <span data-ttu-id="9014b-173">Du kan granska konfigurationen av det och verifiera att hello-agenten är ansluten tooOperational insikter (OMS).</span><span class="sxs-lookup"><span data-stu-id="9014b-173">You can review your configuration there and verify that hello agent is connected tooOperational Insights (OMS).</span></span> <span data-ttu-id="9014b-174">När du är ansluten tooOMS hello agent visas ett meddelande om: **hello Microsoft Monitoring Agent har lyckats ansluta toohello Microsoft Operations Management Suite-tjänsten.**</span><span class="sxs-lookup"><span data-stu-id="9014b-174">When connected tooOMS, hello agent displays a message stating: **hello Microsoft Monitoring Agent has successfully connected toohello Microsoft Operations Management Suite service.**</span></span>

## <a name="configure-proxy-settings"></a><span data-ttu-id="9014b-175">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="9014b-175">Configure proxy settings</span></span>

<span data-ttu-id="9014b-176">Du kan använda hello följa proceduren tooconfigure proxyinställningar för hello Microsoft Monitoring Agent på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="9014b-176">You can use hello following procedure tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel.</span></span> <span data-ttu-id="9014b-177">Behöver du toouse den här proceduren för varje server.</span><span class="sxs-lookup"><span data-stu-id="9014b-177">You need toouse this procedure for each server.</span></span> <span data-ttu-id="9014b-178">Om du har många servrar du behöver tooconfigure kan det vara enklare toouse tooautomate ett skript för den här processen.</span><span class="sxs-lookup"><span data-stu-id="9014b-178">If you have many servers that you need tooconfigure, you might find it easier toouse a script tooautomate this process.</span></span> <span data-ttu-id="9014b-179">I så fall, finns hello nästa procedur [tooconfigure proxyinställningar för hello Microsoft Monitoring Agent använder ett skript för](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span><span class="sxs-lookup"><span data-stu-id="9014b-179">If so, see hello next procedure [tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span></span>

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a><span data-ttu-id="9014b-180">tooconfigure proxyinställningar för hello Microsoft Monitoring Agent med hjälp av Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="9014b-180">tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel</span></span>
1. <span data-ttu-id="9014b-181">Öppna **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="9014b-181">Open **Control Panel**.</span></span>
2. <span data-ttu-id="9014b-182">Öppna **Microsoft Monitoring Agent**.</span><span class="sxs-lookup"><span data-stu-id="9014b-182">Open **Microsoft Monitoring Agent**.</span></span>
3. <span data-ttu-id="9014b-183">Klicka på hello **proxyinställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="9014b-183">Click hello **Proxy Settings** tab.</span></span>  
    <span data-ttu-id="9014b-184">![fliken proxyinställningar](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-184">![proxy settings tab](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span></span>
4. <span data-ttu-id="9014b-185">Välj **använder en proxyserver** och ange hello URL och portnummer, om något toohello behövs, liknande exemplet som visas.</span><span class="sxs-lookup"><span data-stu-id="9014b-185">Select **Use a proxy server** and type hello URL and port number, if one is needed, similar toohello example shown.</span></span> <span data-ttu-id="9014b-186">Om proxyservern kräver autentisering, skriver du hello användarnamn och lösenord tooaccess hello proxyserver.</span><span class="sxs-lookup"><span data-stu-id="9014b-186">If your proxy server requires authentication, type hello username and password tooaccess hello proxy server.</span></span>


### <a name="verify-agent-connectivity-toooms"></a><span data-ttu-id="9014b-187">Kontrollera agent connectivity tooOMS</span><span class="sxs-lookup"><span data-stu-id="9014b-187">Verify agent connectivity tooOMS</span></span>

<span data-ttu-id="9014b-188">Du kan enkelt kontrollera om dina agenter kommunicerar med OMS med hello nedan:</span><span class="sxs-lookup"><span data-stu-id="9014b-188">You can easily verify whether your agents are communicating with OMS using hello following procedure:</span></span>

1.  <span data-ttu-id="9014b-189">Öppna Kontrollpanelen på hello dator med Windows hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="9014b-189">On hello computer with hello Windows agent, open Control Panel.</span></span>
2.  <span data-ttu-id="9014b-190">Öppna Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="9014b-190">Open Microsoft Monitoring Agent.</span></span>
3.  <span data-ttu-id="9014b-191">Fliken hello Azure logganalys (OMS).</span><span class="sxs-lookup"><span data-stu-id="9014b-191">Click hello Azure Log Analytics (OMS) tab.</span></span>
4.  <span data-ttu-id="9014b-192">Du bör se hello agentens ansluten toohello Operations Management Suite-tjänsten i hello Status-kolumnen.</span><span class="sxs-lookup"><span data-stu-id="9014b-192">In hello Status column, you should see that hello agent connected successfully toohello Operations Management Suite service.</span></span>

![Agent som är ansluten](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a><span data-ttu-id="9014b-194">tooconfigure proxyinställningar för hello Microsoft Monitoring Agent med hjälp av ett skript</span><span class="sxs-lookup"><span data-stu-id="9014b-194">tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script</span></span>
<span data-ttu-id="9014b-195">Kopiera hello följande exempel, uppdatera det med information specifik tooyour miljö, spara om filen med filnamnstillägget PS1 och sedan köra hello skript på varje dator som ansluter direkt toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9014b-195">Copy hello following sample, update it with information specific tooyour environment, save it with a PS1 file name extension, and then run hello script on each computer that connects directly toohello OMS service.</span></span>

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a><span data-ttu-id="9014b-196">Installera hello agent med kommandoraden hello</span><span class="sxs-lookup"><span data-stu-id="9014b-196">Install hello agent using hello command line</span></span>
- <span data-ttu-id="9014b-197">Ändra och sedan använda hello följande exempel tooinstall hello agent hello kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="9014b-197">Modify and then use hello following example tooinstall hello agent using hello command line.</span></span> <span data-ttu-id="9014b-198">hello exempel utförs en fullständigt tyst installation.</span><span class="sxs-lookup"><span data-stu-id="9014b-198">hello example performs a fully silent installation.</span></span>

    >[!NOTE]
    <span data-ttu-id="9014b-199">Om du vill tooupgrade en agent måste toouse hello logganalys scripting-API.</span><span class="sxs-lookup"><span data-stu-id="9014b-199">If you want tooupgrade an agent, you need toouse hello Log Analytics scripting API.</span></span> <span data-ttu-id="9014b-200">Se hello nästa avsnitt tooupgrade en agent.</span><span class="sxs-lookup"><span data-stu-id="9014b-200">See hello next section tooupgrade an agent.</span></span>

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

<span data-ttu-id="9014b-201">hello-agenten använder IExpress som dess Self-Extractor med hello `/c` kommando.</span><span class="sxs-lookup"><span data-stu-id="9014b-201">hello agent uses IExpress as its self-extractor using hello `/c` command.</span></span> <span data-ttu-id="9014b-202">Du kan se hello kommandoradsväxlar vid [kommandoradsväxlar för IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) och sedan hello update exempel toosuit dina behov.</span><span class="sxs-lookup"><span data-stu-id="9014b-202">You can see hello command-line switches at [Command-line switches for IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) and then update hello example toosuit your needs.</span></span>

|<span data-ttu-id="9014b-203">MMA-specifika alternativ</span><span class="sxs-lookup"><span data-stu-id="9014b-203">MMA-specific options</span></span>                   |<span data-ttu-id="9014b-204">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9014b-204">Notes</span></span>         |
|---------------------------------------|--------------|
|<span data-ttu-id="9014b-205">ADD_OPINSIGHTS_WORKSPACE</span><span class="sxs-lookup"><span data-stu-id="9014b-205">ADD_OPINSIGHTS_WORKSPACE</span></span>               | <span data-ttu-id="9014b-206">1 = konfigurera hello agent tooreport tooa arbetsytan</span><span class="sxs-lookup"><span data-stu-id="9014b-206">1 = Configure hello agent tooreport tooa workspace</span></span>                |
|<span data-ttu-id="9014b-207">OPINSIGHTS_WORKSPACE_ID</span><span class="sxs-lookup"><span data-stu-id="9014b-207">OPINSIGHTS_WORKSPACE_ID</span></span>                | <span data-ttu-id="9014b-208">Arbetsyte-Id (guid) för hello arbetsytan tooadd</span><span class="sxs-lookup"><span data-stu-id="9014b-208">Workspace Id (guid) for hello workspace tooadd</span></span>                    |
|<span data-ttu-id="9014b-209">OPINSIGHTS_WORKSPACE_KEY</span><span class="sxs-lookup"><span data-stu-id="9014b-209">OPINSIGHTS_WORKSPACE_KEY</span></span>               | <span data-ttu-id="9014b-210">Arbetsytan nyckel används tooinitially autentisera med hello-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="9014b-210">Workspace key used tooinitially authenticate with hello workspace</span></span> |
|<span data-ttu-id="9014b-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span><span class="sxs-lookup"><span data-stu-id="9014b-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span></span>  | <span data-ttu-id="9014b-212">Ange hello molnmiljö där hello arbetsytan finns</span><span class="sxs-lookup"><span data-stu-id="9014b-212">Specify hello cloud environment where hello workspace is located</span></span> <br> <span data-ttu-id="9014b-213">0 = kommersiella azuremolnet (standard)</span><span class="sxs-lookup"><span data-stu-id="9014b-213">0 = Azure commercial cloud (default)</span></span> <br> <span data-ttu-id="9014b-214">1 = azure Government</span><span class="sxs-lookup"><span data-stu-id="9014b-214">1 = Azure Government</span></span> |
|<span data-ttu-id="9014b-215">OPINSIGHTS_PROXY_URL</span><span class="sxs-lookup"><span data-stu-id="9014b-215">OPINSIGHTS_PROXY_URL</span></span>               | <span data-ttu-id="9014b-216">URI för hello proxy toouse</span><span class="sxs-lookup"><span data-stu-id="9014b-216">URI for hello proxy toouse</span></span> |
|<span data-ttu-id="9014b-217">OPINSIGHTS_PROXY_USERNAME</span><span class="sxs-lookup"><span data-stu-id="9014b-217">OPINSIGHTS_PROXY_USERNAME</span></span>               | <span data-ttu-id="9014b-218">Användarnamnet tooaccess en autentiserad proxyserver</span><span class="sxs-lookup"><span data-stu-id="9014b-218">Username tooaccess an authenticated proxy</span></span> |
|<span data-ttu-id="9014b-219">OPINSIGHTS_PROXY_PASSWORD</span><span class="sxs-lookup"><span data-stu-id="9014b-219">OPINSIGHTS_PROXY_PASSWORD</span></span>               | <span data-ttu-id="9014b-220">Lösenordet tooaccess en autentiserad proxyserver</span><span class="sxs-lookup"><span data-stu-id="9014b-220">Password tooaccess an authenticated proxy</span></span> |

>[!NOTE]
<span data-ttu-id="9014b-221">tooavoid hitting hello kommandorad maxlängden för IExpress, installera hello agent med Ingen arbetsyta som konfigurerats och sedan använda ett skript tooset konfiguration för hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="9014b-221">tooavoid hitting hello command-line length limit of IExpress, install hello agent with no workspace configured and then use a script tooset configuration for hello workspace.</span></span>

>[!NOTE]
<span data-ttu-id="9014b-222">Om du får en `Command line option syntax error.` när du använder hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, kan du använda följande lösning hello:</span><span class="sxs-lookup"><span data-stu-id="9014b-222">If you get a `Command line option syntax error.` when using hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, you can use hello following workaround:</span></span>
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a><span data-ttu-id="9014b-223">Lägg till en arbetsyta med hjälp av ett skript</span><span class="sxs-lookup"><span data-stu-id="9014b-223">Add a workspace using a script</span></span>
<span data-ttu-id="9014b-224">Lägg till en arbetsyta med följande exempel hello hello logganalys agent scripting-API:</span><span class="sxs-lookup"><span data-stu-id="9014b-224">Add a workspace using hello Log Analytics agent scripting API with hello following example:</span></span>

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

<span data-ttu-id="9014b-225">tooadd en arbetsyta i Azure och som tillhör amerikanska myndigheter, Använd hello följande skriptexempel:</span><span class="sxs-lookup"><span data-stu-id="9014b-225">tooadd a workspace in Azure for US Government, use hello following script sample:</span></span>
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
<span data-ttu-id="9014b-226">Om du har använt hello kommandoraden eller skript tidigare tooinstall eller konfigurera hello agent `EnableAzureOperationalInsights` ersattes med `AddCloudWorkspace`.</span><span class="sxs-lookup"><span data-stu-id="9014b-226">If you've used hello command line or script previously tooinstall or configure hello agent, `EnableAzureOperationalInsights` was replaced by `AddCloudWorkspace`.</span></span>

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a><span data-ttu-id="9014b-227">Installera hello-agenten i Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="9014b-227">Install hello agent using DSC in Azure Automation</span></span>

<span data-ttu-id="9014b-228">Du kan använda hello följande skript exempel tooinstall hello agent i Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="9014b-228">You can use hello following script example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="9014b-229">hello exempel installerar hello 64-bitars agent identifieras av hello `URI` värde.</span><span class="sxs-lookup"><span data-stu-id="9014b-229">hello example installs hello 64-bit agent, identified by hello `URI` value.</span></span> <span data-ttu-id="9014b-230">Du kan också använda hello 32-bitars version genom att ersätta hello URI-värdet.</span><span class="sxs-lookup"><span data-stu-id="9014b-230">You can also use hello 32-bit version by replacing hello URI value.</span></span> <span data-ttu-id="9014b-231">hello URI: er för båda versionerna är:</span><span class="sxs-lookup"><span data-stu-id="9014b-231">hello URIs for both versions are:</span></span>

- <span data-ttu-id="9014b-232">Windows 64-bitars agent - https://go.microsoft.com/fwlink/?LinkId=828603</span><span class="sxs-lookup"><span data-stu-id="9014b-232">Windows 64-bit agent - https://go.microsoft.com/fwlink/?LinkId=828603</span></span>
- <span data-ttu-id="9014b-233">Windows 32-bitars agent - https://go.microsoft.com/fwlink/?LinkId=828604</span><span class="sxs-lookup"><span data-stu-id="9014b-233">Windows 32-bit agent - https://go.microsoft.com/fwlink/?LinkId=828604</span></span>


>[!NOTE]
<span data-ttu-id="9014b-234">Den här proceduren och skript exempel uppgraderar inte en befintlig agent.</span><span class="sxs-lookup"><span data-stu-id="9014b-234">This procedure and script example will not upgrade an existing agent.</span></span>

1. <span data-ttu-id="9014b-235">Importera hello xPSDesiredStateConfiguration DSC-modul från [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9014b-235">Import hello xPSDesiredStateConfiguration DSC Module from [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) into Azure Automation.</span></span>  
2.  <span data-ttu-id="9014b-236">Skapa en variabel Azure Automation-tillgångar för *OPSINSIGHTS_WS_ID* och *OPSINSIGHTS_WS_KEY*.</span><span class="sxs-lookup"><span data-stu-id="9014b-236">Create Azure Automation variable assets for *OPSINSIGHTS_WS_ID* and *OPSINSIGHTS_WS_KEY*.</span></span> <span data-ttu-id="9014b-237">Ange *OPSINSIGHTS_WS_ID* tooyour OMS logganalys arbetsyte-ID och ange *OPSINSIGHTS_WS_KEY* toohello primärnyckel för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="9014b-237">Set *OPSINSIGHTS_WS_ID* tooyour OMS Log Analytics workspace ID and set *OPSINSIGHTS_WS_KEY* toohello primary key of your workspace.</span></span>
3.  <span data-ttu-id="9014b-238">Använd hello följande skript och spara den som MMAgent.ps1</span><span class="sxs-lookup"><span data-stu-id="9014b-238">Use hello following script and save it as MMAgent.ps1</span></span>
4.  <span data-ttu-id="9014b-239">Ändra och sedan använda hello följande exempel tooinstall hello agent i Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="9014b-239">Modify and then use hello following example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="9014b-240">Importera MMAgent.ps1 till Azure Automation via hello Azure Automation-gränssnittet eller cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9014b-240">Import MMAgent.ps1 into Azure Automation by using hello Azure Automation interface or cmdlet.</span></span>
5.  <span data-ttu-id="9014b-241">Tilldela en nod toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9014b-241">Assign a node toohello configuration.</span></span> <span data-ttu-id="9014b-242">Hello noden kontrollerar konfigurationen och hello MMA flyttas toohello nod inom 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="9014b-242">Within 15 minutes, hello node checks its configuration and hello MMA is pushed toohello node.</span></span>

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-hello-latest-productid-value"></a><span data-ttu-id="9014b-243">Hämta hello senaste ProductId värde</span><span class="sxs-lookup"><span data-stu-id="9014b-243">Get hello latest ProductId value</span></span>

<span data-ttu-id="9014b-244">Hej `ProductId value` i hello MMAgent.ps1 skript är unika tooeach agent-version.</span><span class="sxs-lookup"><span data-stu-id="9014b-244">hello `ProductId value` in hello MMAgent.ps1 script is unique tooeach agent version.</span></span> <span data-ttu-id="9014b-245">När en uppdaterad version av varje agent publiceras ändras hello ProductId värde.</span><span class="sxs-lookup"><span data-stu-id="9014b-245">When an updated version of each agent is published, hello ProductId value changes.</span></span> <span data-ttu-id="9014b-246">Så, när hello ProductId ändras i framtida hello, hittar hello agent-version med ett enkelt skript.</span><span class="sxs-lookup"><span data-stu-id="9014b-246">So, when hello ProductId changes in hello future, you can find hello agent version using a simple script.</span></span> <span data-ttu-id="9014b-247">Du kan använda hello följande skript tooget hello installerat ProductId värdet när du har hello senaste agent installerade versionen på en testserver.</span><span class="sxs-lookup"><span data-stu-id="9014b-247">After you have hello latest agent version installed on a test server, you can use hello following script tooget hello installed ProductId value.</span></span> <span data-ttu-id="9014b-248">Du kan uppdatera hello värdet i hello MMAgent.ps1 skript med hello senaste ProductId värdet.</span><span class="sxs-lookup"><span data-stu-id="9014b-248">Using hello latest ProductId value, you can update hello value in hello MMAgent.ps1 script.</span></span>

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a><span data-ttu-id="9014b-249">Konfigurera en agent manuellt eller lägga till ytterligare arbetsytor</span><span class="sxs-lookup"><span data-stu-id="9014b-249">Configure an agent manually or add additional workspaces</span></span>
<span data-ttu-id="9014b-250">Om du har installerat agenter utan att konfigurera dem eller om du vill hello agent tooreport toomultiple arbetsytor, kan du använda följande information tooenable en agent hello eller konfigurera om den.</span><span class="sxs-lookup"><span data-stu-id="9014b-250">If you've installed agents but did not configure them or if you want hello agent tooreport toomultiple workspaces, you can use hello following information tooenable an agent or reconfigure it.</span></span> <span data-ttu-id="9014b-251">När du har konfigurerat hello agent kan registreras med hello agent-tjänsten och får nödvändiga konfigurationsinformation och hanteringspaket som innehåller information om lösning.</span><span class="sxs-lookup"><span data-stu-id="9014b-251">After you've configured hello agent, it will register with hello agent service and will get necessary configuration information and management packs that contain solution information.</span></span>

1. <span data-ttu-id="9014b-252">När du har installerat hello Microsoft Monitoring Agent kan öppna **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="9014b-252">After you've installed hello Microsoft Monitoring Agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="9014b-253">Öppna **Microsoft Monitoring Agent** och klicka sedan på hello **Azure logganalys (OMS)** fliken.</span><span class="sxs-lookup"><span data-stu-id="9014b-253">Open **Microsoft Monitoring Agent** and then click hello **Azure Log Analytics (OMS)** tab.</span></span>   
3. <span data-ttu-id="9014b-254">Klicka på **Lägg till** tooopen hello **lägga till en Log Analytics-arbetsyta** rutan.</span><span class="sxs-lookup"><span data-stu-id="9014b-254">Click **Add** tooopen hello **Add a Log Analytics Workspace** box.</span></span>
4. <span data-ttu-id="9014b-255">Klistra in hello **arbetsyte-ID** och **Arbetsytenyckel (primärnyckel)** som du kopierade i anteckningar i föregående procedur för hello arbetsytan som du vill tooadd och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9014b-255">Paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in a previous procedure for hello workspace that you want tooadd and then click **OK**.</span></span>  
    <span data-ttu-id="9014b-256">![Konfigurera åtgärdsinformation](./media/log-analytics-windows-agents/add-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-256">![configure Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span></span>

<span data-ttu-id="9014b-257">När data har samlats in från datorer som övervakas av hello agent, hello antalet datorer som övervakas av OMS visas i hello OMS-portalen på hello **anslutna källor** fliken i **inställningar** som  **Servrar som är anslutna**.</span><span class="sxs-lookup"><span data-stu-id="9014b-257">After data is collected from computers monitored by hello agent, hello number of computers monitored by OMS will appear in hello OMS portal on hello **Connected Sources** tab in **Settings** as **Servers Connected**.</span></span>


## <a name="toodisable-an-agent"></a><span data-ttu-id="9014b-258">toodisable en agent</span><span class="sxs-lookup"><span data-stu-id="9014b-258">toodisable an agent</span></span>
1. <span data-ttu-id="9014b-259">När du har installerat agenten hello öppna **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="9014b-259">After installing hello agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="9014b-260">Öppna Microsoft Monitoring Agent och klicka sedan på hello **Azure logganalys (OMS)** fliken.</span><span class="sxs-lookup"><span data-stu-id="9014b-260">Open Microsoft Monitoring Agent and then click hello **Azure Log Analytics (OMS)** tab.</span></span>
3. <span data-ttu-id="9014b-261">Välj en arbetsyta och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="9014b-261">Select a workspace and then click **Remove**.</span></span> <span data-ttu-id="9014b-262">Upprepa det här steget för alla andra arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="9014b-262">Repeat this step for all other workspaces.</span></span>


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="9014b-263">Du kan också konfigurera agenter tooreport tooan Operations Manager-hanteringsgruppen</span><span class="sxs-lookup"><span data-stu-id="9014b-263">Optionally, configure agents tooreport tooan Operations Manager management group</span></span>

<span data-ttu-id="9014b-264">Du kan också använda hello MMA agenten som en Operations Manager-agent om du använder Operations Manager i din IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9014b-264">If you use Operations Manager in your IT infrastructure, you can also use hello MMA agent as an Operations Manager agent.</span></span>

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="9014b-265">tooconfigure MMA agenter tooreport tooan Operations Manager-hanteringsgrupp</span><span class="sxs-lookup"><span data-stu-id="9014b-265">tooconfigure MMA agents tooreport tooan Operations Manager management group</span></span>
1.  <span data-ttu-id="9014b-266">Öppna på hello dator där hello-agenten är installerad **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="9014b-266">On hello computer where hello agent is installed, open **Control Panel**.</span></span>  
2.  <span data-ttu-id="9014b-267">Öppna **Microsoft Monitoring Agent** och klicka sedan på hello **Operations Manager** fliken.</span><span class="sxs-lookup"><span data-stu-id="9014b-267">Open **Microsoft Monitoring Agent** and then click hello **Operations Manager** tab.</span></span>  
    <span data-ttu-id="9014b-268">![Microsoft Monitoring Agent Operations Manager-fliken](./media/log-analytics-windows-agents/om-mg01.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-268">![Microsoft Monitoring Agent Operations Manager tab](./media/log-analytics-windows-agents/om-mg01.png)</span></span>
3.  <span data-ttu-id="9014b-269">Om Operations Manager-servrar har integrering med Active Directory, klickar du på **automatiskt uppdatera hanteringsgrupptilldelningar från AD DS**.</span><span class="sxs-lookup"><span data-stu-id="9014b-269">If your Operations Manager servers have integration with Active Directory, click **Automatically update management group assignments from AD DS**.</span></span>
4.  <span data-ttu-id="9014b-270">Klicka på **Lägg till** tooopen hello **lägga till en Hanteringsgrupp** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9014b-270">Click **Add** tooopen hello **Add a Management Group** dialog box.</span></span>  
    <span data-ttu-id="9014b-271">![Microsoft Monitoring Agent lägga till en Hanteringsgrupp](./media/log-analytics-windows-agents/oms-mma-om02.png)</span><span class="sxs-lookup"><span data-stu-id="9014b-271">![Microsoft Monitoring Agent Add a Management Group](./media/log-analytics-windows-agents/oms-mma-om02.png)</span></span>
5.  <span data-ttu-id="9014b-272">I **hanteringsgruppnamn** rutan, hello-typnamn för hanteringsgruppen.</span><span class="sxs-lookup"><span data-stu-id="9014b-272">In **Management group name** box, type hello name of your management group.</span></span>
6.  <span data-ttu-id="9014b-273">I hello **primära hanteringsserver** rutan, typen hello datornamnet på hello primära hanteringsserver.</span><span class="sxs-lookup"><span data-stu-id="9014b-273">In hello **Primary management server** box, type hello computer name of hello primary management server.</span></span>
7.  <span data-ttu-id="9014b-274">I hello **hanteringsserverporten** rutan typen hello TCP-portnummer.</span><span class="sxs-lookup"><span data-stu-id="9014b-274">In hello **Management server port** box, type hello TCP port number.</span></span>
8.  <span data-ttu-id="9014b-275">Under **Agentåtgärdskontot**, Välj hello lokalt systemkonto eller ett lokala domänkonto.</span><span class="sxs-lookup"><span data-stu-id="9014b-275">Under **Agent Action Account**, choose either hello Local System account or a local domain account.</span></span>
9.  <span data-ttu-id="9014b-276">Klicka på **OK** tooclose hello **lägga till en Hanteringsgrupp** dialogrutan och klicka sedan på **OK** tooclose hello **Microsoft Monitoring agentegenskaper**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9014b-276">Click **OK** tooclose hello **Add a Management Group** dialog box and then click **OK** tooclose hello **Microsoft Monitoring Agent Properties** dialog box.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9014b-277">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9014b-277">Next steps</span></span>

- <span data-ttu-id="9014b-278">[Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.</span><span class="sxs-lookup"><span data-stu-id="9014b-278">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
