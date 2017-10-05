---
title: "Felsökning av problem med Azure File storage i Linux | Microsoft Docs"
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
ms.openlocfilehash: 62cd62ec3a2900f06acacc0852a48b5e3ff1c8cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="11b78-103">Felsökning av problem med Azure File storage i Linux</span><span class="sxs-lookup"><span data-stu-id="11b78-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="11b78-104">Den här artikeln innehåller vanliga problem som är relaterade till Microsoft Azure File storage när du ansluter från Linux-klienter.</span><span class="sxs-lookup"><span data-stu-id="11b78-104">This article lists common problems that are related to Microsoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="11b78-105">Det ger också möjliga orsaker och lösningar på problemen.</span><span class="sxs-lookup"><span data-stu-id="11b78-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a><span data-ttu-id="11b78-106">”[åtkomst nekad] kvoter överskridits” när du försöker öppna en fil</span><span class="sxs-lookup"><span data-stu-id="11b78-106">"[permission denied] Disk quota exceeded" when you try to open a file</span></span>

<span data-ttu-id="11b78-107">I Linux visas ett felmeddelande som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="11b78-107">In Linux, you receive an error message that resembles the following:</span></span>

<span data-ttu-id="11b78-108">**<filename>[åtkomst nekad] Diskkvoten har överskridits**</span><span class="sxs-lookup"><span data-stu-id="11b78-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="11b78-109">Orsak</span><span class="sxs-lookup"><span data-stu-id="11b78-109">Cause</span></span>

<span data-ttu-id="11b78-110">Du har nått den övre gränsen för samtidiga öppna referenser som tillåts för en fil.</span><span class="sxs-lookup"><span data-stu-id="11b78-110">You have reached the upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="11b78-111">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-111">Solution</span></span>

<span data-ttu-id="11b78-112">Minska antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="11b78-112">Reduce the number of concurrent open handles by closing some handles, and then retry the operation.</span></span> <span data-ttu-id="11b78-113">Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="11b78-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a><span data-ttu-id="11b78-114">Långsamma filkopieringen till och från Azure File storage i Linux</span><span class="sxs-lookup"><span data-stu-id="11b78-114">Slow file copying to and from Azure File storage in Linux</span></span>

-   <span data-ttu-id="11b78-115">Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som i/o-storlek för optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="11b78-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as the I/O size for optimal performance.</span></span>
-   <span data-ttu-id="11b78-116">Om du känner till dess slutliga storlek på en fil som du utökar med hjälp av skrivningar och programvaran inte problem med kompatibilitet när en unwritten slutet på filen innehåller nollor, anger du filstorlek i förväg i stället för att varje skrivning en utöka skrivning.</span><span class="sxs-lookup"><span data-stu-id="11b78-116">If you know the final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on the file contains zeros, then set the file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="11b78-117">Använd rätt copy-metoden:</span><span class="sxs-lookup"><span data-stu-id="11b78-117">Use the right copy method:</span></span>
    -   <span data-ttu-id="11b78-118">Använd [AzCopy](storage-use-azcopy.md#file-copy) för alla överföring mellan två filresurser.</span><span class="sxs-lookup"><span data-stu-id="11b78-118">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="11b78-119">Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="11b78-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="11b78-120">”Montera error(112): värddatorn är inte tillgänglig” på grund av en återanslutning timeout</span><span class="sxs-lookup"><span data-stu-id="11b78-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="11b78-121">Det uppstår ett ”112” montera på Linux-klienten när klienten har varit inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="11b78-121">A "112" mount error occurs on the Linux client when the client has been idle for a long time.</span></span> <span data-ttu-id="11b78-122">När utökad inaktivitetstid klienten kopplas och anslutningen på grund av timeout.</span><span class="sxs-lookup"><span data-stu-id="11b78-122">After extended idle time, the client disconnects and the connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="11b78-123">Orsak</span><span class="sxs-lookup"><span data-stu-id="11b78-123">Cause</span></span>

<span data-ttu-id="11b78-124">Anslutningen kan vara inaktiv av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="11b78-124">The connection can be idle for the following reasons:</span></span>

-   <span data-ttu-id="11b78-125">Nätverksfel för kommunikation som gör att återupprätta en TCP-anslutning till servern när du använder alternativet ”soft” monteringspunkt</span><span class="sxs-lookup"><span data-stu-id="11b78-125">Network communication failures that prevent re-establishing a TCP connection to the server when the default “soft” mount option is used</span></span>
-   <span data-ttu-id="11b78-126">Senaste återanslutning korrigeringar som inte finns i äldre kärnor</span><span class="sxs-lookup"><span data-stu-id="11b78-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="11b78-127">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-127">Solution</span></span>

<span data-ttu-id="11b78-128">Den här återanslutning i Linux-kärnan nu problemet som en del av följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="11b78-128">This reconnection problem in the Linux kernel is now fixed as part of the following changes:</span></span>

- [<span data-ttu-id="11b78-129">Korrigera Återanslut om du vill skjuta upp smb3 session inte återansluta långt efter socket återansluta</span><span class="sxs-lookup"><span data-stu-id="11b78-129">Fix reconnect to not defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="11b78-130">Anropa tjänsten echo omedelbart efter socket återansluta</span><span class="sxs-lookup"><span data-stu-id="11b78-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="11b78-131">CIFS: Åtgärda ett möjligt minnet skadas under Återanslut</span><span class="sxs-lookup"><span data-stu-id="11b78-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="11b78-132">CIFS: Åtgärda ett möjligt låses dubbelt mutex under Återanslut (för kernel v4.9 och senare)</span><span class="sxs-lookup"><span data-stu-id="11b78-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="11b78-133">Men kan de här ändringarna inte flyttas ännu till Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="11b78-133">However, these changes might not be ported yet to all the Linux distributions.</span></span> <span data-ttu-id="11b78-134">Den här och andra återanslutning korrigeringar görs i de följande populära Linux kärnor: 4.4.40, 4.8.16 och 4.9.1.</span><span class="sxs-lookup"><span data-stu-id="11b78-134">This fix and other reconnection fixes are made in the following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="11b78-135">Du kan hämta den här korrigeringen genom att uppgradera till någon av dessa rekommenderade kernel-versioner.</span><span class="sxs-lookup"><span data-stu-id="11b78-135">You can get this fix by upgrading to one of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="11b78-136">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-136">Workaround</span></span>

<span data-ttu-id="11b78-137">Du kan undvika det här problemet genom att ange en hård monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="11b78-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="11b78-138">Detta gör att klienten ska vänta tills en anslutning har upprättats eller det explicit avbryts och kan användas för att förhindra fel på grund av timeout i nätverket.</span><span class="sxs-lookup"><span data-stu-id="11b78-138">This forces the client to wait until a connection is established or until it’s explicitly interrupted and can be used to prevent errors because of network time-outs.</span></span> <span data-ttu-id="11b78-139">Den här lösningen orsaka obestämd väntar.</span><span class="sxs-lookup"><span data-stu-id="11b78-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="11b78-140">Var beredd på att stoppa anslutningar vid behov.</span><span class="sxs-lookup"><span data-stu-id="11b78-140">Be prepared to stop connections as necessary.</span></span>

<span data-ttu-id="11b78-141">Om du inte uppgradera till de senaste kernel-versionerna, kan du undvika det här problemet genom att spara en fil i Azure-filresursen som du skriver till var 30: e sekund eller mindre.</span><span class="sxs-lookup"><span data-stu-id="11b78-141">If you cannot upgrade to the latest kernel versions, you can work around this problem by keeping a file in the Azure file share that you write to every 30 seconds or less.</span></span> <span data-ttu-id="11b78-142">Detta måste vara en skrivåtgärd, till exempel skriva om skapade eller ändrade datum för filen.</span><span class="sxs-lookup"><span data-stu-id="11b78-142">This must be a write operation, such as rewriting the created or modified date on the file.</span></span> <span data-ttu-id="11b78-143">Annars kan du få cachelagrade resultaten och åtgärden kan inte utlösa återanslutning.</span><span class="sxs-lookup"><span data-stu-id="11b78-143">Otherwise, you might get cached results, and your operation might not trigger the reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="11b78-144">”Montera error(115): pågår” när du monterar Azure File storage med hjälp av SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="11b78-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="11b78-145">Orsak</span><span class="sxs-lookup"><span data-stu-id="11b78-145">Cause</span></span>

<span data-ttu-id="11b78-146">Vissa Linux-distributioner som ännu stöder inte krypteringsfunktioner i SMB 3.0 och användare kan få felmeddelandet ”115” om de försöker montera Azure File storage med hjälp av SMB 3.0 på grund av en funktion som saknas.</span><span class="sxs-lookup"><span data-stu-id="11b78-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try to mount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="11b78-147">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-147">Solution</span></span>

<span data-ttu-id="11b78-148">Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel.</span><span class="sxs-lookup"><span data-stu-id="11b78-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="11b78-149">Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="11b78-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="11b78-150">Den här funktionen har varit anpassats för Ubuntu nr 17.04 från och Ubuntu 16,10 vid tidpunkten för publicering.</span><span class="sxs-lookup"><span data-stu-id="11b78-150">At the time of publishing, this functionality has been backported to Ubuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="11b78-151">Om din Linux SMB-klienten inte har stöd för kryptering, montera Azure File storage med hjälp av SMB 2.1 från ett Azure virtuell Linux-dator som är i samma datacenter som File storage-konto.</span><span class="sxs-lookup"><span data-stu-id="11b78-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in the same datacenter as the File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="11b78-152">Långsam prestanda på en Azure-filresursen monteras på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="11b78-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="11b78-153">Orsak</span><span class="sxs-lookup"><span data-stu-id="11b78-153">Cause</span></span>

<span data-ttu-id="11b78-154">En möjlig orsak till dåliga prestanda är inaktiverad cachelagring.</span><span class="sxs-lookup"><span data-stu-id="11b78-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="11b78-155">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-155">Solution</span></span>

<span data-ttu-id="11b78-156">Om du vill kontrollera om cachelagring är inaktiverad, leta efter den **cache =** post.</span><span class="sxs-lookup"><span data-stu-id="11b78-156">To check whether caching is disabled, look for the **cache=** entry.</span></span> 

<span data-ttu-id="11b78-157">**Cache = ingen** anger att cachelagring har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="11b78-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="11b78-158">Montera resursen med hjälp av monteringskommandot eller genom att uttryckligen lägga till den **cache = strikt** alternativet att monteringskommandot så att standard cachelagring eller ”strikt” cachelagring läge är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="11b78-158">Remount the share by using the default mount command or by explicitly adding the **cache=strict** option to the mount command to ensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="11b78-159">I vissa fall kan den **serverino** mount-alternativet kan orsaka den **ls** kommando för att köra stat mot varje katalogposten.</span><span class="sxs-lookup"><span data-stu-id="11b78-159">In some scenarios, the **serverino** mount option can cause the **ls** command to run stat against every directory entry.</span></span> <span data-ttu-id="11b78-160">Detta resulterar i försämrade prestanda när du med en stor katalog.</span><span class="sxs-lookup"><span data-stu-id="11b78-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="11b78-161">Du kan kontrollera mount-alternativ din **/etc/fstab** post:</span><span class="sxs-lookup"><span data-stu-id="11b78-161">You can check the mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="11b78-162">Du kan också kontrollera om rätt alternativ som används genom att köra den **sudo montera | grep cifs** kommandot och kontrollera dess utdata, till exempel i följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="11b78-162">You can also check whether the correct options are being used by running the  **sudo mount | grep cifs** command and checking its output, such as the following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="11b78-163">Om den **cache = strikt** eller **serverino** alternativet är inte finns, demontera och montera Azure File storage igen genom att köra monteringskommandot från den [dokumentationen](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="11b78-163">If the **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running the mount command from the [documentation](storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="11b78-164">Kontrollera sedan som den **/etc/fstab** posten har rätt alternativ.</span><span class="sxs-lookup"><span data-stu-id="11b78-164">Then, recheck that the **/etc/fstab** entry has the correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a><span data-ttu-id="11b78-165">Tidsstämplar förlorades i kopiering av filer från Windows till Linux</span><span class="sxs-lookup"><span data-stu-id="11b78-165">Time stamps were lost in copying files from Windows to Linux</span></span>

<span data-ttu-id="11b78-166">På Linux/Unix-plattformar, den **cp -p** kommandot misslyckas om filen 1 och 2-filen ägs av olika användare.</span><span class="sxs-lookup"><span data-stu-id="11b78-166">On Linux/Unix platforms, the **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="11b78-167">Orsak</span><span class="sxs-lookup"><span data-stu-id="11b78-167">Cause</span></span>

<span data-ttu-id="11b78-168">Flaggan force **f** i COPYFILE resulterar i att köra **cp -p -f** på Unix.</span><span class="sxs-lookup"><span data-stu-id="11b78-168">The force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="11b78-169">Det här kommandot inte heller går att bevara tidsstämpeln för den fil som du inte äger.</span><span class="sxs-lookup"><span data-stu-id="11b78-169">This command also fails to preserve the time stamp of the file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="11b78-170">Lösning</span><span class="sxs-lookup"><span data-stu-id="11b78-170">Workaround</span></span>

<span data-ttu-id="11b78-171">Använd det storage-kontot för att kopiera filerna:</span><span class="sxs-lookup"><span data-stu-id="11b78-171">Use the storage account user for copying the files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="11b78-172">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="11b78-172">Need help?</span></span> <span data-ttu-id="11b78-173">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="11b78-173">Contact support.</span></span>

<span data-ttu-id="11b78-174">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="11b78-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly.</span></span>
