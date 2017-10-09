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
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>Felsökning av problem med Azure File storage i Linux

Den här artikeln innehåller vanliga problem som är relaterade tooMicrosoft Azure File storage när du ansluter från Linux-klienter. Det ger också möjliga orsaker och lösningar på problemen.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a>”[åtkomst nekad] kvoter överskridits” när du försöker tooopen en fil

I Linux visas ett felmeddelande som liknar följande hello:

**<filename>[åtkomst nekad] Diskkvoten har överskridits**

### <a name="cause"></a>Orsak

Du har nått hello övre gräns för samtidiga öppna referenser som tillåts för en fil.

### <a name="solution"></a>Lösning

Minska hello antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan hello igen. Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a>Långsam fil kopierar tooand från Azure File storage i Linux

-   Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som hello i/o-storlek för optimala prestanda.
-   Om du vet hello slutliga storlek på en fil som du utökar med hjälp av skrivningar och programvaran inte problem med kompatibilitet när en unwritten slutet på hello-filen innehåller nollor, och ange sedan hello filstorlek i förväg i stället för att varje skrivning en utöka skriva.
-   Använd hello rätt copy-metoden:
    -   Använd [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) för alla överföring mellan två filresurser.
    -   Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>”Montera error(112): värddatorn är inte tillgänglig” på grund av en återanslutning timeout

Ett ”112” mount-fel uppstår på hello Linux-klient när hello-klienten har varit inaktiv för länge. När utökad inaktivitetstid hello klienten kopplas och hello-anslutning.  

### <a name="cause"></a>Orsak

hello-anslutningen kan vara inaktiv för hello följande orsaker:

-   Nätverksfel för kommunikation som gör att återupprätta en TCP-anslutning toohello server när hello ”soft” monteringspunkt standardalternativet används
-   Senaste återanslutning korrigeringar som inte finns i äldre kärnor

### <a name="solution"></a>Lösning

Den här återanslutning i hello Linux kernel nu problemet som en del av hello följande ändringar:

- [Korrigera återansluta toonot skjuta upp smb3 session återansluta långt efter socket återansluta](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Anropa tjänsten echo omedelbart efter socket återansluta](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Åtgärda ett möjligt minnet skadas under Återanslut](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Åtgärda ett möjligt låses dubbelt mutex under Återanslut (för kernel v4.9 och senare)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

De här ändringarna kan dock inte att återföra ännu tooall hello Linux-distributioner. Den här och andra återanslutning korrigeringar görs i hello följande populära Linux kärnor: 4.4.40, 4.8.16 och 4.9.1. Du kan hämta den här korrigeringen genom att uppgradera tooone av dessa rekommenderade kernel-versioner.

### <a name="workaround"></a>Lösning

Du kan undvika det här problemet genom att ange en hård monteringspunkt. Detta tvingar hello klienten toowait förrän en anslutning har upprättats eller det explicit avbryts och kan vara används tooprevent fel på grund av timeout i nätverket. Den här lösningen orsaka obestämd väntar. Vara förberedda toostop anslutningar vid behov.

Om du inte uppgradera toohello senaste kernel-versioner, kan du undvika det här problemet genom att spara en fil i hello Azure-filresursen du skriver tooevery 30 sekunder eller mindre. Detta måste vara en skrivåtgärd som skriva om hello skapas eller ändras datumet på hello-fil. Annars kan du få cachelagrade resultaten och åtgärden kan inte utlösa hello återanslutning.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>”Montera error(115): pågår” när du monterar Azure File storage med hjälp av SMB 3.0

### <a name="cause"></a>Orsak

Vissa Linux-distributioner som ännu stöder inte krypteringsfunktioner i SMB 3.0 och användare kan få ett meddelande om ”115” om de försöker toomount Azure File storage med hjälp av SMB 3.0 på grund av en funktion som saknas.

### <a name="solution"></a>Lösning

Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel. Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region. När hello publicering har den här funktionen anpassats tooUbuntu nr 17.04 från och Ubuntu 16,10. Om din Linux SMB-klienten inte har stöd för kryptering, montera Azure File storage med hjälp av SMB 2.1 från en Azure Linux-dator som är i hello samma datacenter som hello File storage-konto.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Långsam prestanda på en Azure-filresursen monteras på en Linux-VM

### <a name="cause"></a>Orsak

En möjlig orsak till dåliga prestanda är inaktiverad cachelagring.

### <a name="solution"></a>Lösning

toocheck om cachelagring är inaktiverad, leta efter hello **cache =** post. 

**Cache = ingen** anger att cachelagring har inaktiverats.  Återmontering hello resursen med kommandot hello standard montera eller genom att uttryckligen lägga till hello **cache = strikt** alternativet toohello montera kommandot tooensure som standard cachelagring eller ”strikt” cachelagring läge är aktiverat.

I vissa scenarier, hello **serverino** mount-alternativet kan orsaka hello **ls** kommandot toorun stat mot varje katalogposten. Detta resulterar i försämrade prestanda när du med en stor katalog. Du kan kontrollera hello monteringsalternativ din **/etc/fstab** post:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Du kan också kontrollera om hello rätt alternativ som används genom att köra hello **sudo montera | grep cifs** kommandot och kontrollera dess utdata, till exempel hello följande exempel på utdata:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Om hello **cache = strikt** eller **serverino** alternativet är inte finns, demontera och montera Azure File storage igen genom att köra hello montera kommando från hello [dokumentationen](../storage-how-to-use-files-linux.md). Kontrollera sedan att hello **/etc/fstab** posten har hello rätt alternativ.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a>Tidsstämplar förlorades i kopiering av filer från Windows tooLinux

På Linux/Unix-plattformar hello **cp -p** kommandot misslyckas om filen 1 och 2-filen ägs av olika användare.

### <a name="cause"></a>Orsak

Hej flaggan force **f** i COPYFILE resulterar i att köra **cp -p -f** på Unix. Kommandot misslyckas även toopreserve hello tidsstämpeln för hello-fil som du inte äger.

### <a name="workaround"></a>Lösning

Använd hello storage-kontot för att kopiera hello filer:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet löst snabbt.
