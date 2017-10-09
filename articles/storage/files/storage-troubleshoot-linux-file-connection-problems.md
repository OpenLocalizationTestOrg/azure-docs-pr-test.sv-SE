---
title: aaaTroubleshoot Azure File storage problem i Linux | Microsoft Docs
description: "Felsökning av problem med Azure File storage i Linux"
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
ms.date: 07/11/2017
ms.author: genli
ms.openlocfilehash: 4bdc3c6ed2e48f245060a03632fca9bd14d33545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="6934e-103">Felsökning av problem med Azure File storage i Linux</span><span class="sxs-lookup"><span data-stu-id="6934e-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="6934e-104">Den här artikeln innehåller vanliga problem som är relaterade tooMicrosoft Azure File storage när du ansluter från Linux-klienter.</span><span class="sxs-lookup"><span data-stu-id="6934e-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="6934e-105">Det ger också möjliga orsaker och lösningar på problemen.</span><span class="sxs-lookup"><span data-stu-id="6934e-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a><span data-ttu-id="6934e-106">”[åtkomst nekad] kvoter överskridits” när du försöker tooopen en fil</span><span class="sxs-lookup"><span data-stu-id="6934e-106">"[permission denied] Disk quota exceeded" when you try tooopen a file</span></span>

<span data-ttu-id="6934e-107">I Linux visas ett felmeddelande som liknar följande hello:</span><span class="sxs-lookup"><span data-stu-id="6934e-107">In Linux, you receive an error message that resembles hello following:</span></span>

<span data-ttu-id="6934e-108">**<filename>[åtkomst nekad] Diskkvoten har överskridits**</span><span class="sxs-lookup"><span data-stu-id="6934e-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="6934e-109">Orsak</span><span class="sxs-lookup"><span data-stu-id="6934e-109">Cause</span></span>

<span data-ttu-id="6934e-110">Du har nått hello övre gräns för samtidiga öppna referenser som tillåts för en fil.</span><span class="sxs-lookup"><span data-stu-id="6934e-110">You have reached hello upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="6934e-111">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-111">Solution</span></span>

<span data-ttu-id="6934e-112">Minska hello antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan hello igen.</span><span class="sxs-lookup"><span data-stu-id="6934e-112">Reduce hello number of concurrent open handles by closing some handles, and then retry hello operation.</span></span> <span data-ttu-id="6934e-113">Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6934e-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a><span data-ttu-id="6934e-114">Långsam fil kopierar tooand från Azure File storage i Linux</span><span class="sxs-lookup"><span data-stu-id="6934e-114">Slow file copying tooand from Azure File storage in Linux</span></span>

-   <span data-ttu-id="6934e-115">Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som hello i/o-storlek för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="6934e-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="6934e-116">Om du vet hello slutliga storlek på en fil som du utökar med hjälp av skrivningar och programvaran inte problem med kompatibilitet när en unwritten slutet på hello-filen innehåller nollor, och ange sedan hello filstorlek i förväg i stället för att varje skrivning en utöka skriva.</span><span class="sxs-lookup"><span data-stu-id="6934e-116">If you know hello final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="6934e-117">Använd hello rätt copy-metoden:</span><span class="sxs-lookup"><span data-stu-id="6934e-117">Use hello right copy method:</span></span>
    -   <span data-ttu-id="6934e-118">Använd [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) för alla överföring mellan två filresurser.</span><span class="sxs-lookup"><span data-stu-id="6934e-118">Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="6934e-119">Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="6934e-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="6934e-120">”Montera error(112): värddatorn är inte tillgänglig” på grund av en återanslutning timeout</span><span class="sxs-lookup"><span data-stu-id="6934e-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="6934e-121">Ett ”112” mount-fel uppstår på hello Linux-klient när hello-klienten har varit inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="6934e-121">A "112" mount error occurs on hello Linux client when hello client has been idle for a long time.</span></span> <span data-ttu-id="6934e-122">När utökad inaktivitetstid hello klienten kopplas och hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6934e-122">After extended idle time, hello client disconnects and hello connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="6934e-123">Orsak</span><span class="sxs-lookup"><span data-stu-id="6934e-123">Cause</span></span>

<span data-ttu-id="6934e-124">hello-anslutningen kan vara inaktiv för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="6934e-124">hello connection can be idle for hello following reasons:</span></span>

-   <span data-ttu-id="6934e-125">Nätverksfel för kommunikation som gör att återupprätta en TCP-anslutning toohello server när hello ”soft” monteringspunkt standardalternativet används</span><span class="sxs-lookup"><span data-stu-id="6934e-125">Network communication failures that prevent re-establishing a TCP connection toohello server when hello default “soft” mount option is used</span></span>
-   <span data-ttu-id="6934e-126">Senaste återanslutning korrigeringar som inte finns i äldre kärnor</span><span class="sxs-lookup"><span data-stu-id="6934e-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="6934e-127">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-127">Solution</span></span>

<span data-ttu-id="6934e-128">Den här återanslutning i hello Linux kernel nu problemet som en del av hello följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="6934e-128">This reconnection problem in hello Linux kernel is now fixed as part of hello following changes:</span></span>

- [<span data-ttu-id="6934e-129">Korrigera återansluta toonot skjuta upp smb3 session återansluta långt efter socket återansluta</span><span class="sxs-lookup"><span data-stu-id="6934e-129">Fix reconnect toonot defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="6934e-130">Anropa tjänsten echo omedelbart efter socket återansluta</span><span class="sxs-lookup"><span data-stu-id="6934e-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="6934e-131">CIFS: Åtgärda ett möjligt minnet skadas under Återanslut</span><span class="sxs-lookup"><span data-stu-id="6934e-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="6934e-132">CIFS: Åtgärda ett möjligt låses dubbelt mutex under Återanslut (för kernel v4.9 och senare)</span><span class="sxs-lookup"><span data-stu-id="6934e-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="6934e-133">De här ändringarna kan dock inte att återföra ännu tooall hello Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="6934e-133">However, these changes might not be ported yet tooall hello Linux distributions.</span></span> <span data-ttu-id="6934e-134">Den här och andra återanslutning korrigeringar görs i hello följande populära Linux kärnor: 4.4.40, 4.8.16 och 4.9.1.</span><span class="sxs-lookup"><span data-stu-id="6934e-134">This fix and other reconnection fixes are made in hello following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="6934e-135">Du kan hämta den här korrigeringen genom att uppgradera tooone av dessa rekommenderade kernel-versioner.</span><span class="sxs-lookup"><span data-stu-id="6934e-135">You can get this fix by upgrading tooone of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="6934e-136">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-136">Workaround</span></span>

<span data-ttu-id="6934e-137">Du kan undvika det här problemet genom att ange en hård monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="6934e-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="6934e-138">Detta tvingar hello klienten toowait förrän en anslutning har upprättats eller det explicit avbryts och kan vara används tooprevent fel på grund av timeout i nätverket.</span><span class="sxs-lookup"><span data-stu-id="6934e-138">This forces hello client toowait until a connection is established or until it’s explicitly interrupted and can be used tooprevent errors because of network time-outs.</span></span> <span data-ttu-id="6934e-139">Den här lösningen orsaka obestämd väntar.</span><span class="sxs-lookup"><span data-stu-id="6934e-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="6934e-140">Vara förberedda toostop anslutningar vid behov.</span><span class="sxs-lookup"><span data-stu-id="6934e-140">Be prepared toostop connections as necessary.</span></span>

<span data-ttu-id="6934e-141">Om du inte uppgradera toohello senaste kernel-versioner, kan du undvika det här problemet genom att spara en fil i hello Azure-filresursen du skriver tooevery 30 sekunder eller mindre.</span><span class="sxs-lookup"><span data-stu-id="6934e-141">If you cannot upgrade toohello latest kernel versions, you can work around this problem by keeping a file in hello Azure file share that you write tooevery 30 seconds or less.</span></span> <span data-ttu-id="6934e-142">Detta måste vara en skrivåtgärd som skriva om hello skapas eller ändras datumet på hello-fil.</span><span class="sxs-lookup"><span data-stu-id="6934e-142">This must be a write operation, such as rewriting hello created or modified date on hello file.</span></span> <span data-ttu-id="6934e-143">Annars kan du få cachelagrade resultaten och åtgärden kan inte utlösa hello återanslutning.</span><span class="sxs-lookup"><span data-stu-id="6934e-143">Otherwise, you might get cached results, and your operation might not trigger hello reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="6934e-144">”Montera error(115): pågår” när du monterar Azure File storage med hjälp av SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="6934e-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="6934e-145">Orsak</span><span class="sxs-lookup"><span data-stu-id="6934e-145">Cause</span></span>

<span data-ttu-id="6934e-146">Vissa Linux-distributioner som ännu stöder inte krypteringsfunktioner i SMB 3.0 och användare kan få ett meddelande om ”115” om de försöker toomount Azure File storage med hjälp av SMB 3.0 på grund av en funktion som saknas.</span><span class="sxs-lookup"><span data-stu-id="6934e-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try toomount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="6934e-147">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-147">Solution</span></span>

<span data-ttu-id="6934e-148">Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel.</span><span class="sxs-lookup"><span data-stu-id="6934e-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="6934e-149">Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="6934e-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="6934e-150">När hello publicering har den här funktionen anpassats tooUbuntu nr 17.04 från och Ubuntu 16,10.</span><span class="sxs-lookup"><span data-stu-id="6934e-150">At hello time of publishing, this functionality has been backported tooUbuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="6934e-151">Om din Linux SMB-klienten inte har stöd för kryptering, montera Azure File storage med hjälp av SMB 2.1 från en Azure Linux-dator som är i hello samma datacenter som hello File storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6934e-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in hello same datacenter as hello File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="6934e-152">Långsam prestanda på en Azure-filresursen monteras på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="6934e-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="6934e-153">Orsak</span><span class="sxs-lookup"><span data-stu-id="6934e-153">Cause</span></span>

<span data-ttu-id="6934e-154">En möjlig orsak till dåliga prestanda är inaktiverad cachelagring.</span><span class="sxs-lookup"><span data-stu-id="6934e-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="6934e-155">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-155">Solution</span></span>

<span data-ttu-id="6934e-156">toocheck om cachelagring är inaktiverad, leta efter hello **cache =** post.</span><span class="sxs-lookup"><span data-stu-id="6934e-156">toocheck whether caching is disabled, look for hello **cache=** entry.</span></span> 

<span data-ttu-id="6934e-157">**Cache = ingen** anger att cachelagring har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="6934e-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="6934e-158">Återmontering hello resursen med kommandot hello standard montera eller genom att uttryckligen lägga till hello **cache = strikt** alternativet toohello montera kommandot tooensure som standard cachelagring eller ”strikt” cachelagring läge är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="6934e-158">Remount hello share by using hello default mount command or by explicitly adding hello **cache=strict** option toohello mount command tooensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="6934e-159">I vissa scenarier, hello **serverino** mount-alternativet kan orsaka hello **ls** kommandot toorun stat mot varje katalogposten.</span><span class="sxs-lookup"><span data-stu-id="6934e-159">In some scenarios, hello **serverino** mount option can cause hello **ls** command toorun stat against every directory entry.</span></span> <span data-ttu-id="6934e-160">Detta resulterar i försämrade prestanda när du med en stor katalog.</span><span class="sxs-lookup"><span data-stu-id="6934e-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="6934e-161">Du kan kontrollera hello monteringsalternativ din **/etc/fstab** post:</span><span class="sxs-lookup"><span data-stu-id="6934e-161">You can check hello mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="6934e-162">Du kan också kontrollera om hello rätt alternativ som används genom att köra hello **sudo montera | grep cifs** kommandot och kontrollera dess utdata, till exempel hello följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="6934e-162">You can also check whether hello correct options are being used by running hello  **sudo mount | grep cifs** command and checking its output, such as hello following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="6934e-163">Om hello **cache = strikt** eller **serverino** alternativet är inte finns, demontera och montera Azure File storage igen genom att köra hello montera kommando från hello [dokumentationen](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6934e-163">If hello **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running hello mount command from hello [documentation](../storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="6934e-164">Kontrollera sedan att hello **/etc/fstab** posten har hello rätt alternativ.</span><span class="sxs-lookup"><span data-stu-id="6934e-164">Then, recheck that hello **/etc/fstab** entry has hello correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a><span data-ttu-id="6934e-165">Tidsstämplar förlorades i kopiering av filer från Windows tooLinux</span><span class="sxs-lookup"><span data-stu-id="6934e-165">Time stamps were lost in copying files from Windows tooLinux</span></span>

<span data-ttu-id="6934e-166">På Linux/Unix-plattformar hello **cp -p** kommandot misslyckas om filen 1 och 2-filen ägs av olika användare.</span><span class="sxs-lookup"><span data-stu-id="6934e-166">On Linux/Unix platforms, hello **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="6934e-167">Orsak</span><span class="sxs-lookup"><span data-stu-id="6934e-167">Cause</span></span>

<span data-ttu-id="6934e-168">Hej flaggan force **f** i COPYFILE resulterar i att köra **cp -p -f** på Unix.</span><span class="sxs-lookup"><span data-stu-id="6934e-168">hello force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="6934e-169">Kommandot misslyckas även toopreserve hello tidsstämpeln för hello-fil som du inte äger.</span><span class="sxs-lookup"><span data-stu-id="6934e-169">This command also fails toopreserve hello time stamp of hello file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="6934e-170">Lösning</span><span class="sxs-lookup"><span data-stu-id="6934e-170">Workaround</span></span>

<span data-ttu-id="6934e-171">Använd hello storage-kontot för att kopiera hello filer:</span><span class="sxs-lookup"><span data-stu-id="6934e-171">Use hello storage account user for copying hello files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="6934e-172">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="6934e-172">Need help?</span></span> <span data-ttu-id="6934e-173">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="6934e-173">Contact support.</span></span>

<span data-ttu-id="6934e-174">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet löst snabbt.</span><span class="sxs-lookup"><span data-stu-id="6934e-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
