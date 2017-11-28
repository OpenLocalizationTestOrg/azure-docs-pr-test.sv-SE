---
title: problem med aaaTroubleshoot Azure File storage i Windows | Microsoft Docs
description: "Felsökning av problem med Azure File storage i Windows"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: genli
ms.openlocfilehash: 19529d8af5d98790e2e381cd21ad4d0284acb124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a><span data-ttu-id="1806e-103">Felsökning av problem med Azure File storage i Windows</span><span class="sxs-lookup"><span data-stu-id="1806e-103">Troubleshoot Azure File storage problems in Windows</span></span>

<span data-ttu-id="1806e-104">Den här artikeln innehåller vanliga problem som är relaterade tooMicrosoft Azure File storage när du ansluter från Windows-klienter.</span><span class="sxs-lookup"><span data-stu-id="1806e-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Windows clients.</span></span> <span data-ttu-id="1806e-105">Det ger också möjliga orsaker och lösningar på problemen.</span><span class="sxs-lookup"><span data-stu-id="1806e-105">It also provides possible causes and resolutions for these problems.</span></span> <span data-ttu-id="1806e-106">Dessutom toohello felsökning steg i den här artikeln kan du också använda [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) så att hello Windows klientmiljö har rätt krav.</span><span class="sxs-lookup"><span data-stu-id="1806e-106">In addition toohello troubleshooting steps in this article, you can also use [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) to ensure that hello Windows client environment has correct prerequisites.</span></span> <span data-ttu-id="1806e-107">AzFileDiagnostics automatiserar identifiering av de flesta hello symtom som nämns i den här artikeln och hjälper dig att konfigurera din miljö tooget optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="1806e-107">AzFileDiagnostics automates detection of most of hello symptoms mentioned in this article and helps set up your environment tooget optimal performance.</span></span> <span data-ttu-id="1806e-108">Du kan också använda informationen i hello [Azure Files delar felsökaren](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) som ger steg tooassist du problem ansluter/mappning/montering Azure Files delar.</span><span class="sxs-lookup"><span data-stu-id="1806e-108">You can also find this information in hello [Azure Files shares Troubleshooter](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) that provides steps tooassist you with problems connecting/mapping/mounting Azure Files shares.</span></span>


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a><span data-ttu-id="1806e-109">Fel 53, fel 67 eller fel 87 när du montera eller demontera en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="1806e-109">Error 53, Error 67, or Error 87 when you mount or unmount an Azure file share</span></span>

<span data-ttu-id="1806e-110">När du försöker toomount en filresurs från lokalt eller från ett annat datacenter kan du få hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="1806e-110">When you try toomount a file share from on-premises or from a different datacenter, you might receive hello following errors:</span></span>

- <span data-ttu-id="1806e-111">Systemfel 53 har uppstått.</span><span class="sxs-lookup"><span data-stu-id="1806e-111">System error 53 has occurred.</span></span> <span data-ttu-id="1806e-112">hello nätverkssökvägen hittades inte.</span><span class="sxs-lookup"><span data-stu-id="1806e-112">hello network path was not found.</span></span>
- <span data-ttu-id="1806e-113">Systemfel 67 har uppstått.</span><span class="sxs-lookup"><span data-stu-id="1806e-113">System error 67 has occurred.</span></span> <span data-ttu-id="1806e-114">hello nätverkets namn kan inte hittas.</span><span class="sxs-lookup"><span data-stu-id="1806e-114">hello network name cannot be found.</span></span>
- <span data-ttu-id="1806e-115">Systemfel 87 har uppstått.</span><span class="sxs-lookup"><span data-stu-id="1806e-115">System error 87 has occurred.</span></span> <span data-ttu-id="1806e-116">hello-parametern är felaktig.</span><span class="sxs-lookup"><span data-stu-id="1806e-116">hello parameter is incorrect.</span></span>

### <a name="cause-1-unencrypted-communication-channel"></a><span data-ttu-id="1806e-117">Orsak 1: Okrypterad kommunikationskanalen</span><span class="sxs-lookup"><span data-stu-id="1806e-117">Cause 1: Unencrypted communication channel</span></span>

<span data-ttu-id="1806e-118">Av säkerhetsskäl hello anslutningar tooAzure filresurser blockeras om hello kommunikationskanalen inte är krypterad och hello anslutningsförsöket inte genomförs från samma datacenter där hello Azure-filresurser finns.</span><span class="sxs-lookup"><span data-stu-id="1806e-118">For security reasons, connections tooAzure file shares are blocked if hello communication channel isn’t encrypted and if hello connection attempt isn't made from hello same datacenter where hello Azure file shares reside.</span></span> <span data-ttu-id="1806e-119">Kryptering för kommunikation kanal tillhandahålls endast om hello användarens klientens operativsystem stöder SMB-kryptering.</span><span class="sxs-lookup"><span data-stu-id="1806e-119">Communication channel encryption is provided only if hello user’s client OS supports SMB encryption.</span></span>

<span data-ttu-id="1806e-120">Windows 8, Windows Server 2012 och senare versioner av varje system för att förhandla begäranden som innehåller SMB 3.0, som stöder kryptering.</span><span class="sxs-lookup"><span data-stu-id="1806e-120">Windows 8, Windows Server 2012, and later versions of each system negotiate requests that include SMB 3.0, which supports encryption.</span></span>

### <a name="solution-for-cause-1"></a><span data-ttu-id="1806e-121">Lösning för orsak 1</span><span class="sxs-lookup"><span data-stu-id="1806e-121">Solution for cause 1</span></span>

<span data-ttu-id="1806e-122">Ansluta från en klient som har en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="1806e-122">Connect from a client that does one of hello following:</span></span>

- <span data-ttu-id="1806e-123">Uppfyller hello av Windows 8 och Windows Server 2012 eller senare versioner</span><span class="sxs-lookup"><span data-stu-id="1806e-123">Meets hello requirements of Windows 8 and Windows Server 2012 or later versions</span></span>
- <span data-ttu-id="1806e-124">Ansluter från en virtuell dator i hello samma datacenter som hello Azure storage-konto som används för hello Azure-filresursen</span><span class="sxs-lookup"><span data-stu-id="1806e-124">Connects from a virtual machine in hello same datacenter as hello Azure storage account that is used for hello Azure file share</span></span>

### <a name="cause-2-port-445-is-blocked"></a><span data-ttu-id="1806e-125">Orsak 2: Port 445 är blockerad</span><span class="sxs-lookup"><span data-stu-id="1806e-125">Cause 2: Port 445 is blocked</span></span>

<span data-ttu-id="1806e-126">Systemfel 53 eller systemfel 67 kan inträffa om port 445 utgående kommunikation tooan Azure File storage datacenter är blockerad.</span><span class="sxs-lookup"><span data-stu-id="1806e-126">System error 53 or system error 67 can occur if port 445 outbound communication tooan Azure File storage datacenter is blocked.</span></span> <span data-ttu-id="1806e-127">toosee hello sammanfattning av Internet-leverantörer som Tillåt eller inte Tillåt åtkomst från port 445, gå för[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span><span class="sxs-lookup"><span data-stu-id="1806e-127">toosee hello summary of ISPs that allow or disallow access from port 445, go too[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span></span>

<span data-ttu-id="1806e-128">toounderstand om detta är hello orsak bakom hälsningsmeddelande ”Systemfel 53” du kan använda Portqry tooquery hello TCP:445 slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="1806e-128">toounderstand whether this is hello reason behind hello "System error 53" message, you can use Portqry tooquery hello TCP:445 endpoint.</span></span> <span data-ttu-id="1806e-129">Om hello TCP:445 endpoint visas som filtrerats, blockeras hello TCP-port.</span><span class="sxs-lookup"><span data-stu-id="1806e-129">If hello TCP:445 endpoint is displayed as filtered, hello TCP port is blocked.</span></span> <span data-ttu-id="1806e-130">Här är en exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="1806e-130">Here is an example query:</span></span>

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

<span data-ttu-id="1806e-131">Om TCP-port 445 blockeras av en regel längs sökvägen hello visas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1806e-131">If TCP port 445 is blocked by a rule along hello network path, you will see hello following output:</span></span>

  `TCP port 445 (microsoft-ds service): FILTERED`

<span data-ttu-id="1806e-132">Mer information om hur toouse Portqry, se [beskrivning av hello Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span><span class="sxs-lookup"><span data-stu-id="1806e-132">For more information about how toouse Portqry, see [Description of hello Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span></span>

### <a name="solution-for-cause-2"></a><span data-ttu-id="1806e-133">Lösning för orsak 2</span><span class="sxs-lookup"><span data-stu-id="1806e-133">Solution for cause 2</span></span>

<span data-ttu-id="1806e-134">Arbeta med din IT-avdelningen tooopen port 445 utgående för[Azure IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1806e-134">Work with your IT department tooopen port 445 outbound too[Azure IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

### <a name="cause-3-ntlmv1-is-enabled"></a><span data-ttu-id="1806e-135">Orsak 3: NTLMv1 är aktiverat</span><span class="sxs-lookup"><span data-stu-id="1806e-135">Cause 3: NTLMv1 is enabled</span></span>

<span data-ttu-id="1806e-136">Systemfel 53 eller systemfel 87 kan inträffa om NTLMv1 kommunikation är aktiverat på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="1806e-136">System error 53 or system error 87 can occur if NTLMv1 communication is enabled on hello client.</span></span> <span data-ttu-id="1806e-137">Azure File storage stöder NTLMv2-autentisering.</span><span class="sxs-lookup"><span data-stu-id="1806e-137">Azure File storage supports only NTLMv2 authentication.</span></span> <span data-ttu-id="1806e-138">Med NTLMv1 aktiverad skapar en mindre säkra klient.</span><span class="sxs-lookup"><span data-stu-id="1806e-138">Having NTLMv1 enabled creates a less-secure client.</span></span> <span data-ttu-id="1806e-139">Därför blockeras kommunikation för Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="1806e-139">Therefore, communication is blocked for Azure File storage.</span></span> 

<span data-ttu-id="1806e-140">toodetermine om detta är hello felorsak hello, kontrollera att följande registerundernyckel hello är tooa värdet 3:</span><span class="sxs-lookup"><span data-stu-id="1806e-140">toodetermine whether this is hello cause of hello error, verify that hello following registry subkey is set tooa value of 3:</span></span>

<span data-ttu-id="1806e-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span><span class="sxs-lookup"><span data-stu-id="1806e-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span></span>

<span data-ttu-id="1806e-142">Mer information finns i hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) avsnitt på TechNet.</span><span class="sxs-lookup"><span data-stu-id="1806e-142">For more information, see hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) topic on TechNet.</span></span>

### <a name="solution-for-cause-3"></a><span data-ttu-id="1806e-143">Lösning för orsak 3</span><span class="sxs-lookup"><span data-stu-id="1806e-143">Solution for cause 3</span></span>

<span data-ttu-id="1806e-144">Återställa hello **LmCompatibilityLevel** värdet toohello standardvärdet 3 i hello följande registerundernyckel:</span><span class="sxs-lookup"><span data-stu-id="1806e-144">Revert hello **LmCompatibilityLevel** value toohello default value of 3 in hello following registry subkey:</span></span>

  <span data-ttu-id="1806e-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span><span class="sxs-lookup"><span data-stu-id="1806e-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span></span>

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a><span data-ttu-id="1806e-146">Fel 1816 ”finns inte tillräckligt med kvoten är tillgängliga tooprocess kommandot” när du kopierar tooan Azure-filresursen</span><span class="sxs-lookup"><span data-stu-id="1806e-146">Error 1816 “Not enough quota is available tooprocess this command” when you copy tooan Azure file share</span></span>

### <a name="cause"></a><span data-ttu-id="1806e-147">Orsak</span><span class="sxs-lookup"><span data-stu-id="1806e-147">Cause</span></span>

<span data-ttu-id="1806e-148">Fel 1816 händer när du når hello övre gräns för samtidiga öppna referenser som tillåts för en fil på hello dator där hello filresurs monteras.</span><span class="sxs-lookup"><span data-stu-id="1806e-148">Error 1816 happens when you reach hello upper limit of concurrent open handles that are allowed for a file on hello computer where hello file share is being mounted.</span></span>

### <a name="solution"></a><span data-ttu-id="1806e-149">Lösning</span><span class="sxs-lookup"><span data-stu-id="1806e-149">Solution</span></span>

<span data-ttu-id="1806e-150">Minska hello antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="1806e-150">Reduce hello number of concurrent open handles by closing some handles, and then retry.</span></span> <span data-ttu-id="1806e-151">Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1806e-151">For more information, see [Microsoft Azure Storage performance and scalability checklist](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a><span data-ttu-id="1806e-152">Långsam fil kopierar tooand från Azure File storage i Windows</span><span class="sxs-lookup"><span data-stu-id="1806e-152">Slow file copying tooand from Azure File storage in Windows</span></span>

<span data-ttu-id="1806e-153">Du kan se långsamma prestanda när du försöker tootransfer filer toohello tjänsten Azure File.</span><span class="sxs-lookup"><span data-stu-id="1806e-153">You might see slow performance when you try tootransfer files toohello Azure File service.</span></span>

- <span data-ttu-id="1806e-154">Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som hello i/o-storlek för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="1806e-154">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="1806e-155">Om du vet hello slutliga storlek på en fil som du utökar med skriver och programvaran har inte kompatibilitetsproblem när hello unwritten slutet på hello-filen innehåller nollor, sedan ange hello filstorlek i förväg i stället för att varje skrivning en utöka skrivning.</span><span class="sxs-lookup"><span data-stu-id="1806e-155">If you know hello final size of a file that you are extending with writes, and your software doesn’t have compatibility problems when hello unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="1806e-156">Använd hello rätt copy-metoden:</span><span class="sxs-lookup"><span data-stu-id="1806e-156">Use hello right copy method:</span></span>
    -   <span data-ttu-id="1806e-157">Använd [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) för alla överföring mellan två filresurser.</span><span class="sxs-lookup"><span data-stu-id="1806e-157">Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="1806e-158">Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="1806e-158">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a><span data-ttu-id="1806e-159">Överväganden för Windows 8.1 eller Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="1806e-159">Considerations for Windows 8.1 or Windows Server 2012 R2</span></span>

<span data-ttu-id="1806e-160">För klienter som kör Windows 8.1 eller Windows Server 2012 R2, se till att hello [KB3114025](https://support.microsoft.com/help/3114025) snabbkorrigeringen är installerad.</span><span class="sxs-lookup"><span data-stu-id="1806e-160">For clients that are running Windows 8.1 or Windows Server 2012 R2, make sure that hello [KB3114025](https://support.microsoft.com/help/3114025) hotfix is installed.</span></span> <span data-ttu-id="1806e-161">Den här snabbkorrigeringen förbättrar hello prestanda för create och Stäng referenser.</span><span class="sxs-lookup"><span data-stu-id="1806e-161">This hotfix improves hello performance of create and close handles.</span></span>

<span data-ttu-id="1806e-162">Du kan köra hello följande skript toocheck om hello snabbkorrigeringen har installerats:</span><span class="sxs-lookup"><span data-stu-id="1806e-162">You can run hello following script toocheck whether hello hotfix has been installed:</span></span>

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

<span data-ttu-id="1806e-163">Om snabbkorrigeringen installeras visas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1806e-163">If hotfix is installed, hello following output is displayed:</span></span>

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> <span data-ttu-id="1806e-164">Windows Server 2012 R2-avbildningar i Azure Marketplace har snabbkorrigering KB3114025 installeras som standard, från och med December 2015.</span><span class="sxs-lookup"><span data-stu-id="1806e-164">Windows Server 2012 R2 images in Azure Marketplace have hotfix KB3114025 installed by default, starting in December 2015.</span></span>

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a><span data-ttu-id="1806e-165">Ingen mapp med en enhetsbeteckning i **datorn**</span><span class="sxs-lookup"><span data-stu-id="1806e-165">No folder with a drive letter in **My Computer**</span></span>

<span data-ttu-id="1806e-166">Om du mappar en Azure-filresurs som en administratör med hjälp av net Använd hello resurs visas toobe saknas.</span><span class="sxs-lookup"><span data-stu-id="1806e-166">If you map an Azure file share as an administrator by using net use, hello share appears toobe missing.</span></span>

### <a name="cause"></a><span data-ttu-id="1806e-167">Orsak</span><span class="sxs-lookup"><span data-stu-id="1806e-167">Cause</span></span>

<span data-ttu-id="1806e-168">Som standard körs inte Utforskaren i Windows som administratör.</span><span class="sxs-lookup"><span data-stu-id="1806e-168">By default, Windows File Explorer does not run as an administrator.</span></span> <span data-ttu-id="1806e-169">Om du kör net Använd från en administrativ kommandotolk mappa hello nätverksenhet som administratör.</span><span class="sxs-lookup"><span data-stu-id="1806e-169">If you run net use from an administrative command prompt, you map hello network drive as an administrator.</span></span> <span data-ttu-id="1806e-170">Eftersom mappade enheter är användarcentrerad, visas inte hello-användarkonto som loggas i hello enheter om de är monterade under ett annat användarkonto.</span><span class="sxs-lookup"><span data-stu-id="1806e-170">Because mapped drives are user-centric, hello user account that is logged in does not display hello drives if they are mounted under a different user account.</span></span>

### <a name="solution"></a><span data-ttu-id="1806e-171">Lösning</span><span class="sxs-lookup"><span data-stu-id="1806e-171">Solution</span></span>
<span data-ttu-id="1806e-172">Montera hello resurs från en kommandorad för icke-administratörer.</span><span class="sxs-lookup"><span data-stu-id="1806e-172">Mount hello share from a non-administrator command line.</span></span> <span data-ttu-id="1806e-173">Du kan också följa [den här TechNet-artikeln](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** registervärdet.</span><span class="sxs-lookup"><span data-stu-id="1806e-173">Alternatively, you can follow [this TechNet topic](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** registry value.</span></span>

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a><span data-ttu-id="1806e-174">Kommandot net use misslyckas om hello storage-konto innehåller ett snedstreck</span><span class="sxs-lookup"><span data-stu-id="1806e-174">Net use command fails if hello storage account contains a forward slash</span></span>

### <a name="cause"></a><span data-ttu-id="1806e-175">Orsak</span><span class="sxs-lookup"><span data-stu-id="1806e-175">Cause</span></span>

<span data-ttu-id="1806e-176">kommandot net use för hello tolkar ett snedstreck (/) som ett kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="1806e-176">hello net use command interprets a forward slash (/) as a command-line option.</span></span> <span data-ttu-id="1806e-177">Om ditt användarkonto börjar med ett snedstreck, misslyckas hello enhetsmappning.</span><span class="sxs-lookup"><span data-stu-id="1806e-177">If your user account name starts with a forward slash, hello drive mapping fails.</span></span>

### <a name="solution"></a><span data-ttu-id="1806e-178">Lösning</span><span class="sxs-lookup"><span data-stu-id="1806e-178">Solution</span></span>

<span data-ttu-id="1806e-179">Du kan använda något av följande steg toowork runt hello hello:</span><span class="sxs-lookup"><span data-stu-id="1806e-179">You can use either of hello following steps toowork around hello problem:</span></span>

- <span data-ttu-id="1806e-180">Kör följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1806e-180">Run hello following PowerShell command:</span></span>

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  <span data-ttu-id="1806e-181">Du kan köra det här sättet hello-kommando från en batchfil:</span><span class="sxs-lookup"><span data-stu-id="1806e-181">From a batch file, you can run hello command this way:</span></span>

  `Echo new-smbMapping ... | powershell -command –`

- <span data-ttu-id="1806e-182">Placera citattecken runt hello viktiga toowork problemet--om hello snedstreck är hello första felaktiga tecknet.</span><span class="sxs-lookup"><span data-stu-id="1806e-182">Put double quotation marks around hello key toowork around this problem--unless hello forward slash is hello first character.</span></span> <span data-ttu-id="1806e-183">Om det är Använd hello interaktivt läge och ange ditt lösenord separat eller återskapa dina nycklar tooget en nyckel som inte börjar med ett snedstreck.</span><span class="sxs-lookup"><span data-stu-id="1806e-183">If it is, either use hello interactive mode and enter your password separately or regenerate your keys tooget a key that doesn't start with a forward slash.</span></span>

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a><span data-ttu-id="1806e-184">Programmet eller tjänsten kan inte komma åt en monterad enhet i Azure File storage</span><span class="sxs-lookup"><span data-stu-id="1806e-184">Application or service cannot access a mounted Azure File storage drive</span></span>

### <a name="cause"></a><span data-ttu-id="1806e-185">Orsak</span><span class="sxs-lookup"><span data-stu-id="1806e-185">Cause</span></span>

<span data-ttu-id="1806e-186">Enheter som är monterade per användare.</span><span class="sxs-lookup"><span data-stu-id="1806e-186">Drives are mounted per user.</span></span> <span data-ttu-id="1806e-187">Om programmet eller tjänsten körs under ett annat användarkonto än hello som monterad hello enhet visas inte programmet hello hello enhet.</span><span class="sxs-lookup"><span data-stu-id="1806e-187">If your application or service is running under a different user account than hello one that mounted hello drive, hello application will not see hello drive.</span></span>

### <a name="solution"></a><span data-ttu-id="1806e-188">Lösning</span><span class="sxs-lookup"><span data-stu-id="1806e-188">Solution</span></span>

<span data-ttu-id="1806e-189">Använd någon av följande lösningar hello:</span><span class="sxs-lookup"><span data-stu-id="1806e-189">Use one of hello following solutions:</span></span>

-   <span data-ttu-id="1806e-190">Montera hello enhet från hello samma användarkonto som innehåller hello program.</span><span class="sxs-lookup"><span data-stu-id="1806e-190">Mount hello drive from hello same user account that contains hello application.</span></span> <span data-ttu-id="1806e-191">Du kan använda ett verktyg som PsExec.</span><span class="sxs-lookup"><span data-stu-id="1806e-191">You can use a tool such as PsExec.</span></span>
- <span data-ttu-id="1806e-192">Överför hello lagringskontonamn och nyckel i hello användarens namn och lösenord parametrarna för kommandot net use för hello.</span><span class="sxs-lookup"><span data-stu-id="1806e-192">Pass hello storage account name and key in hello user name and password parameters of hello net use command.</span></span>

<span data-ttu-id="1806e-193">När du har följt anvisningarna får du följande felmeddelande när du kör net användning för hello system/Nätverkstjänstkonto hello: ”systemfel 1312 har uppstått.</span><span class="sxs-lookup"><span data-stu-id="1806e-193">After you follow these instructions, you might receive hello following error message when you run net use for hello system/network service account: "System error 1312 has occurred.</span></span> <span data-ttu-id="1806e-194">En angiven inloggningssession finns inte.</span><span class="sxs-lookup"><span data-stu-id="1806e-194">A specified logon session does not exist.</span></span> <span data-ttu-id="1806e-195">Det kanske redan har avslutats ”.</span><span class="sxs-lookup"><span data-stu-id="1806e-195">It may already have been terminated."</span></span> <span data-ttu-id="1806e-196">Om detta inträffar, kontrollerar du att det hello användarnamnet som skickades toonet Använd innehåller domäninformation (till exempel ”: [lagringskontonamn]. file.core.windows .net”).</span><span class="sxs-lookup"><span data-stu-id="1806e-196">If this occurs, make sure that hello username that is passed toonet use includes domain information (for example: "[storage account name].file.core.windows.net").</span></span>

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a><span data-ttu-id="1806e-197">Fel som ”du kopierar en fil tooa mål som inte har stöd för kryptering”</span><span class="sxs-lookup"><span data-stu-id="1806e-197">Error "You are copying a file tooa destination that does not support encryption"</span></span>

<span data-ttu-id="1806e-198">När en fil har kopierats över hello nätverk, dekrypteras hello-filen på hello källdator skickas i klartext och omkrypteras på hello mål.</span><span class="sxs-lookup"><span data-stu-id="1806e-198">When a file is copied over hello network, hello file is decrypted on hello source computer, transmitted in plaintext, and re-encrypted at hello destination.</span></span> <span data-ttu-id="1806e-199">Men du kan se hello följande fel när du försöker toocopy en krypterad fil: ”kopierar du hello filen tooa mål som inte har stöd för kryptering”.</span><span class="sxs-lookup"><span data-stu-id="1806e-199">However, you might see hello following error when you're trying toocopy an encrypted file: "You are copying hello file tooa destination that does not support encryption."</span></span>

### <a name="cause"></a><span data-ttu-id="1806e-200">Orsak</span><span class="sxs-lookup"><span data-stu-id="1806e-200">Cause</span></span>
<span data-ttu-id="1806e-201">Det här problemet kan inträffa om du använder EFS (ENCRYPTING File System).</span><span class="sxs-lookup"><span data-stu-id="1806e-201">This problem can occur if you are using Encrypting File System (EFS).</span></span> <span data-ttu-id="1806e-202">BitLocker-krypterade filer kan vara kopierade tooAzure File storage.</span><span class="sxs-lookup"><span data-stu-id="1806e-202">BitLocker-encrypted files can be copied tooAzure File storage.</span></span> <span data-ttu-id="1806e-203">Azure File storage stöder dock inte NTFS EFS.</span><span class="sxs-lookup"><span data-stu-id="1806e-203">However, Azure File storage does not support NTFS EFS.</span></span>

### <a name="workaround"></a><span data-ttu-id="1806e-204">Lösning</span><span class="sxs-lookup"><span data-stu-id="1806e-204">Workaround</span></span>
<span data-ttu-id="1806e-205">toocopy en fil hello nätverket, du måste först dekryptera den.</span><span class="sxs-lookup"><span data-stu-id="1806e-205">toocopy a file over hello network, you must first decrypt it.</span></span> <span data-ttu-id="1806e-206">Använd någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="1806e-206">Use one of hello following methods:</span></span>

- <span data-ttu-id="1806e-207">Använd hello **Kopiera /d** kommando.</span><span class="sxs-lookup"><span data-stu-id="1806e-207">Use hello **copy /d** command.</span></span> <span data-ttu-id="1806e-208">Tillåter hello krypterade filer toobe sparas som dekrypterade filerna på hello mål.</span><span class="sxs-lookup"><span data-stu-id="1806e-208">It allows hello encrypted files toobe saved as decrypted files at hello destination.</span></span>
- <span data-ttu-id="1806e-209">Ange hello följande registernyckel:</span><span class="sxs-lookup"><span data-stu-id="1806e-209">Set hello following registry key:</span></span>
  - <span data-ttu-id="1806e-210">Sökväg = HKLM\Software\Policies\Microsoft\Windows\System</span><span class="sxs-lookup"><span data-stu-id="1806e-210">Path = HKLM\Software\Policies\Microsoft\Windows\System</span></span>
  - <span data-ttu-id="1806e-211">Värdetypen = DWORD</span><span class="sxs-lookup"><span data-stu-id="1806e-211">Value type = DWORD</span></span>
  - <span data-ttu-id="1806e-212">Name = CopyFileAllowDecryptedRemoteDestination</span><span class="sxs-lookup"><span data-stu-id="1806e-212">Name = CopyFileAllowDecryptedRemoteDestination</span></span>
  - <span data-ttu-id="1806e-213">Värde = 1</span><span class="sxs-lookup"><span data-stu-id="1806e-213">Value = 1</span></span>

<span data-ttu-id="1806e-214">Tänk på registernyckeln inställningen hello påverkar alla åtgärder på en kopia som görs toonetwork resurser.</span><span class="sxs-lookup"><span data-stu-id="1806e-214">Be aware that setting hello registry key affects all copy operations that are made toonetwork shares.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="1806e-215">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="1806e-215">Need help?</span></span> <span data-ttu-id="1806e-216">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="1806e-216">Contact support.</span></span>
<span data-ttu-id="1806e-217">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet löst snabbt.</span><span class="sxs-lookup"><span data-stu-id="1806e-217">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
