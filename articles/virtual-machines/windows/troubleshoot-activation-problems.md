---
title: "aktiveringsproblem för aaaTroubleshoot Windows virtuell dator i Azure | Microsoft Docs"
description: "Ger hello felsökning för att åtgärda Windows aktiveringsproblem för virtuell dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 4ebdac66166e80dcd68cd9e2931b30a29ac01eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a><span data-ttu-id="f60d0-103">Felsöka problem med Windows Azure virtuella aktivering</span><span class="sxs-lookup"><span data-stu-id="f60d0-103">Troubleshoot Azure Windows virtual machine activation problems</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="f60d0-104">Om du har problem när du aktiverar Azure Windows virtuell dator (VM som skapas från en anpassad avbildning) kan använda du hello informationen i det här dokumentet tootroubleshoot hello problemet.</span><span class="sxs-lookup"><span data-stu-id="f60d0-104">If you have trouble when activating Azure Windows virtual machine (VM) that is created from a custom image, you can use hello information provided in this document tootroubleshoot hello issue.</span></span> 

## <a name="symptom"></a><span data-ttu-id="f60d0-105">Symtom</span><span class="sxs-lookup"><span data-stu-id="f60d0-105">Symptom</span></span>

<span data-ttu-id="f60d0-106">När du tooactivate en Windows Azure-dator visas ett felmeddelande meddelande som liknar följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="f60d0-106">When you try tooactivate an Azure Windows VM, you receive an error message resembles hello following sample:</span></span>

<span data-ttu-id="f60d0-107">**Fel: 0xC004F074 hello programvara LicensingService rapporterade hello datorn inte kunde aktiveras. Ingen nyckel ManagementService (KMS) kunde nås. Se hello programmets händelselogg för ytterligare information.**</span><span class="sxs-lookup"><span data-stu-id="f60d0-107">**Error: 0xC004F074 hello Software LicensingService reported that hello computer could not be activated. No Key ManagementService (KMS) could be contacted. Please see hello Application Event Log for additional information.**</span></span>

## <a name="cause"></a><span data-ttu-id="f60d0-108">Orsak</span><span class="sxs-lookup"><span data-stu-id="f60d0-108">Cause</span></span>
<span data-ttu-id="f60d0-109">I allmänhet uppstå Azure VM aktiveringsproblem om hello Windows VM inte är konfigurerad som använder hello lämplig KMS-klientinstallationsnyckel hello Windows VM har en anslutning problemet toohello Azure KMS-tjänsten (kms.core.windows.net, port 1668).</span><span class="sxs-lookup"><span data-stu-id="f60d0-109">Generally, Azure VM activation issues occur if hello Windows VM is not configured by using hello appropriate KMS client setup key, or hello Windows VM has a connectivity problem toohello Azure KMS service (kms.core.windows.net, port 1668).</span></span> 

## <a name="solution"></a><span data-ttu-id="f60d0-110">Lösning</span><span class="sxs-lookup"><span data-stu-id="f60d0-110">Solution</span></span>

>[!NOTE]
><span data-ttu-id="f60d0-111">Om du använder en plats-till-plats-VPN och Tvingad tunneltrafik finns [Använd Azure anpassade dirigerar tooenable KMS-aktivering med Tvingad tunneltrafik](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span><span class="sxs-lookup"><span data-stu-id="f60d0-111">If you are using a site-to-site VPN and forced tunneling, see [Use Azure custom routes tooenable KMS activation with forced tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span></span> 
>
><span data-ttu-id="f60d0-112">Om du använder ExpressRoute och du har publicerat en standardväg, se [Azure VM misslyckas tooactivate via ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span><span class="sxs-lookup"><span data-stu-id="f60d0-112">If you are using ExpressRoute and you have a default route published, see [Azure VM may fail tooactivate over ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span></span>

### <a name="step-1-configure-hello-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a><span data-ttu-id="f60d0-113">Steg 1 konfigurera hello lämplig KMS-klientinstallationsnyckel (för Windows Server 2016 och Windows Server 2012 R2)</span><span class="sxs-lookup"><span data-stu-id="f60d0-113">Step 1 Configure hello appropriate KMS client setup key (for Windows Server 2016 and Windows Server 2012 R2)</span></span>

<span data-ttu-id="f60d0-114">Hello VM som skapas från en anpassad avbildning för Windows Server 2016 eller Windows Server 2012 R2, måste du konfigurera hello lämplig KMS-klientinstallationsnyckel för hello VM.</span><span class="sxs-lookup"><span data-stu-id="f60d0-114">For hello VM that is created from a custom image of Windows Server 2016 or Windows Server 2012 R2, you must configure hello appropriate KMS client setup key for hello VM.</span></span>

<span data-ttu-id="f60d0-115">Det här steget gäller inte tooWindows 2012 eller Windows 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="f60d0-115">This step does not apply tooWindows 2012 or Windows 2008 R2.</span></span> <span data-ttu-id="f60d0-116">Den använder hello aktivering för Automation-virtuella datorer (AVMA)-funktioner som stöds endast av Windows Server 2016 och Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="f60d0-116">It uses hello Automation Virtual Machine Activation (AVMA) feature, which is supported only by Windows Server 2016 and Windows Server 2012 R2.</span></span>

1. <span data-ttu-id="f60d0-117">Kör **slmgr.vbs/dlv** i en upphöjd kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f60d0-117">Run **slmgr.vbs /dlv** at an elevated command prompt.</span></span> <span data-ttu-id="f60d0-118">Kontrollera hello beskrivningsvärde i hello utdata och sedan avgöra om den har skapats från retail (RETAIL-kanal) eller volym (VOLUME_KMSCLIENT) licens media:</span><span class="sxs-lookup"><span data-stu-id="f60d0-118">Check hello Description value in hello output, and then determine whether it was created from retail (RETAIL channel) or volume (VOLUME_KMSCLIENT) license media:</span></span>
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. <span data-ttu-id="f60d0-119">Om **slmgr.vbs/dlv** visar RETAIL-kanal, kör följande kommandon tooset hello hello [KMS-klientinstallationsnyckel](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) för hello version av Windows Server som används och tvinga den tooretry aktivering:</span><span class="sxs-lookup"><span data-stu-id="f60d0-119">If **slmgr.vbs /dlv** shows RETAIL channel, run hello following commands tooset hello [KMS client setup key](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) for hello version of Windows Server being used, and force it tooretry activation:</span></span> 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    <span data-ttu-id="f60d0-120">Till exempel för Windows Server 2016 Datacenter kör du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f60d0-120">For example, for Windows Server 2016 Datacenter, you would run hello following command:</span></span>

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-hello-connectivity-between-hello-vm-and-azure-kms-service"></a><span data-ttu-id="f60d0-121">Steg 2 Kontrollera hello anslutningen mellan hello VM och Azure KMS-tjänsten</span><span class="sxs-lookup"><span data-stu-id="f60d0-121">Step 2 Verify hello connectivity between hello VM and Azure KMS service</span></span>

1. <span data-ttu-id="f60d0-122">Ladda ned och extrahera hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) verktyget tooa lokal mapp i hello VM inte aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f60d0-122">Download and extract hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) tool tooa local folder in hello VM that does not activate.</span></span> 

2. <span data-ttu-id="f60d0-123">Gå tooStart, Sök på Windows PowerShell, högerklickar på Windows PowerShell och välj Kör som administratör.</span><span class="sxs-lookup"><span data-stu-id="f60d0-123">Go tooStart, search on Windows PowerShell, right-click Windows PowerShell, and then select Run as administrator.</span></span>

3. <span data-ttu-id="f60d0-124">Kontrollera att den virtuella datorn är hello konfigurerad toouse hello rätt Azure KMS-servern.</span><span class="sxs-lookup"><span data-stu-id="f60d0-124">Make sure that hello VM is configured toouse hello correct Azure KMS server.</span></span> <span data-ttu-id="f60d0-125">toodo detta genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f60d0-125">toodo this, run the following command:</span></span>
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    <span data-ttu-id="f60d0-126">hello kommandot ska returnera: nyckelhanteringstjänst datornamn har angetts tookms.core.windows.net:1688.</span><span class="sxs-lookup"><span data-stu-id="f60d0-126">hello command should return: Key Management Service machine name set tookms.core.windows.net:1688 successfully.</span></span>

4. <span data-ttu-id="f60d0-127">Kontrollera med hjälp av du att KMS-server som anslutningen toohello Psping.</span><span class="sxs-lookup"><span data-stu-id="f60d0-127">Verify by using Psping that you have connectivity toohello KMS server.</span></span> <span data-ttu-id="f60d0-128">Växla toohello mapp där du extraherade hello Pstools.zip ladda ned och kör sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="f60d0-128">Switch toohello folder where you extracted hello Pstools.zip download, and then run hello following:</span></span>
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  <span data-ttu-id="f60d0-129">Se till att du ser i hello sekund sista raden i hello utdata,: skickas = 4, mottagna = 4, förlorad = 0 (0% förlust).</span><span class="sxs-lookup"><span data-stu-id="f60d0-129">In hello second-to-last line of hello output, make sure that you see: Sent = 4, Received = 4, Lost = 0 (0% loss).</span></span>

  <span data-ttu-id="f60d0-130">Om förlorad är större än 0 (noll), har hello VM inte anslutning toohello KMS-servern.</span><span class="sxs-lookup"><span data-stu-id="f60d0-130">If Lost is greater than 0 (zero), hello VM does not have connectivity toohello KMS server.</span></span> <span data-ttu-id="f60d0-131">I det här fallet om hello virtuella datorn är i ett virtuellt nätverk och har en anpassad DNS-server anges, måste du se till att DNS-servern är kan tooresolve kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="f60d0-131">In this situation, if hello VM is in a virtual network and has a custom DNS server specified, you must make sure that DNS server is able tooresolve kms.core.windows.net.</span></span> <span data-ttu-id="f60d0-132">Eller ändra hello DNS server tooone som matchar kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="f60d0-132">Or, change hello DNS server tooone that does resolve kms.core.windows.net.</span></span>

  <span data-ttu-id="f60d0-133">Observera att om du tar bort alla DNS-servrar från ett virtuellt nätverk, virtuella datorer använder Azures interna DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f60d0-133">Notice that if you remove all DNS servers from a virtual network, VMs use Azure’s internal DNS service.</span></span> <span data-ttu-id="f60d0-134">Den här tjänsten kan matcha kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="f60d0-134">This service can resolve kms.core.windows.net.</span></span>
  
<span data-ttu-id="f60d0-135">Kontrollera också att hello gäst brandväggen inte har konfigurerats på ett sätt som skulle blockera aktiveringsförsök.</span><span class="sxs-lookup"><span data-stu-id="f60d0-135">Also verify that hello guest firewall has not been configured in a manner that would block activation attempts.</span></span>

5. <span data-ttu-id="f60d0-136">När du har kontrollerat lyckad anslutning tookms.core.windows.net kör du följande kommando i Kommandotolken för att upphöjd Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="f60d0-136">After you verify successful connectivity tookms.core.windows.net, run hello following command at that elevated Windows PowerShell prompt.</span></span> <span data-ttu-id="f60d0-137">Detta kommando försöker aktivera flera gånger.</span><span class="sxs-lookup"><span data-stu-id="f60d0-137">This command tries activation multiple times.</span></span>

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

<span data-ttu-id="f60d0-138">Aktiveringen returnerar information som liknar följande hello:</span><span class="sxs-lookup"><span data-stu-id="f60d0-138">A successful activation returns information that resembles hello following:</span></span>

<span data-ttu-id="f60d0-139">**Aktiverar Windows (r), ServerDatacenter edition (12345678-1234-1234-1234-12345678)... Produkten har aktiverats.**</span><span class="sxs-lookup"><span data-stu-id="f60d0-139">**Activating Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) … Product activated successfully.**</span></span>

## <a name="faq"></a><span data-ttu-id="f60d0-140">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="f60d0-140">FAQ</span></span> 

### <a name="i-created-hello-windows-server-2016-from-azure-marketplace-do-i-need-tooconfigure-kms-key-for-activating-hello-windows-server-2016"></a><span data-ttu-id="f60d0-141">Jag har skapat hello Windows Server 2016 från Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f60d0-141">I created hello Windows Server 2016 from Azure Marketplace.</span></span> <span data-ttu-id="f60d0-142">Behöver jag tooconfigure KMS-nyckel för att aktivera hello Windows Server 2016?</span><span class="sxs-lookup"><span data-stu-id="f60d0-142">Do I need tooconfigure KMS key for activating hello Windows Server 2016?</span></span> 
 
<span data-ttu-id="f60d0-143">Nej.</span><span class="sxs-lookup"><span data-stu-id="f60d0-143">No.</span></span> <span data-ttu-id="f60d0-144">hello-avbildning i Azure Marketplace har hello lämplig KMS-klientinstallationsnyckel redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="f60d0-144">hello image in Azure Marketplace has hello appropriate KMS client setup key already configured.</span></span> 

### <a name="does-windows-activation-work-hello-same-way-regardless-if-hello-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a><span data-ttu-id="f60d0-145">Windows activation arbete hello samma sätt oavsett om hello VM använder Azure Hybrid Använd förmånen (NAV) eller inte?</span><span class="sxs-lookup"><span data-stu-id="f60d0-145">Does Windows activation work hello same way regardless if hello VM is using Azure Hybrid Use Benefit (HUB) or not?</span></span> 
 
<span data-ttu-id="f60d0-146">Ja.</span><span class="sxs-lookup"><span data-stu-id="f60d0-146">Yes.</span></span> 
 
### <a name="what-happens-if-windows-activation-period-expires"></a><span data-ttu-id="f60d0-147">Vad händer om Windows-aktivering period har löpt ut?</span><span class="sxs-lookup"><span data-stu-id="f60d0-147">What happens if Windows activation period expires?</span></span> 
 
<span data-ttu-id="f60d0-148">När hello respitperioden upphört och Windows fortfarande inte är aktiverad, visas ytterligare meddelanden om att aktivera i Windows Server 2008 R2 och senare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="f60d0-148">When hello grace period has expired and Windows is still not activated, Windows Server 2008 R2 and later versions of Windows will show additional notifications about activating.</span></span> <span data-ttu-id="f60d0-149">hello skrivbordsunderlägg förblir svart och Windows Update installerar säkerhet och viktiga uppdateringar, men inte valfria uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="f60d0-149">hello desktop wallpaper remains black, and Windows Update will install security and critical updates only, but not optional updates.</span></span> <span data-ttu-id="f60d0-150">Hello meddelanden avsnittet längst ned hello hello [licensiering villkor](http://technet.microsoft.com/en-us/library/ff793403.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="f60d0-150">See  hello Notifications section at hello bottom of hello [Licensing Conditions](http://technet.microsoft.com/en-us/library/ff793403.aspx) page.</span></span>   

## <a name="need-help-contact-support"></a><span data-ttu-id="f60d0-151">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="f60d0-151">Need help?</span></span> <span data-ttu-id="f60d0-152">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="f60d0-152">Contact support.</span></span>
<span data-ttu-id="f60d0-153">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="f60d0-153">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
