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
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>Felsökning av problem med Azure File storage i Linux

Den här artikeln innehåller vanliga problem som är relaterade till Microsoft Azure File storage när du ansluter från Linux-klienter. Det ger också möjliga orsaker och lösningar på problemen.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>”[åtkomst nekad] kvoter överskridits” när du försöker öppna en fil

I Linux visas ett felmeddelande som liknar följande:

**<filename>[åtkomst nekad] Diskkvoten har överskridits**

### <a name="cause"></a>Orsak

Du har nått den övre gränsen för samtidiga öppna referenser som tillåts för en fil.

### <a name="solution"></a>Lösning

Minska antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan igen. Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](storage-performance-checklist.md).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a>Långsamma filkopieringen till och från Azure File storage i Linux

-   Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som i/o-storlek för optimala prestanda.
-   Om du känner till dess slutliga storlek på en fil som du utökar med hjälp av skrivningar och programvaran inte problem med kompatibilitet när en unwritten slutet på filen innehåller nollor, anger du filstorlek i förväg i stället för att varje skrivning en utöka skrivning.
-   Använd rätt copy-metoden:
    -   Använd [AzCopy](storage-use-azcopy.md#file-copy) för alla överföring mellan två filresurser.
    -   Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>”Montera error(112): värddatorn är inte tillgänglig” på grund av en återanslutning timeout

Det uppstår ett ”112” montera på Linux-klienten när klienten har varit inaktiv för länge. När utökad inaktivitetstid klienten kopplas och anslutningen på grund av timeout.  

### <a name="cause"></a>Orsak

Anslutningen kan vara inaktiv av följande skäl:

-   Nätverksfel för kommunikation som gör att återupprätta en TCP-anslutning till servern när du använder alternativet ”soft” monteringspunkt
-   Senaste återanslutning korrigeringar som inte finns i äldre kärnor

### <a name="solution"></a>Lösning

Den här återanslutning i Linux-kärnan nu problemet som en del av följande ändringar:

- [Korrigera Återanslut om du vill skjuta upp smb3 session inte återansluta långt efter socket återansluta](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Anropa tjänsten echo omedelbart efter socket återansluta](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Åtgärda ett möjligt minnet skadas under Återanslut](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Åtgärda ett möjligt låses dubbelt mutex under Återanslut (för kernel v4.9 och senare)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Men kan de här ändringarna inte flyttas ännu till Linux-distributioner. Den här och andra återanslutning korrigeringar görs i de följande populära Linux kärnor: 4.4.40, 4.8.16 och 4.9.1. Du kan hämta den här korrigeringen genom att uppgradera till någon av dessa rekommenderade kernel-versioner.

### <a name="workaround"></a>Lösning

Du kan undvika det här problemet genom att ange en hård monteringspunkt. Detta gör att klienten ska vänta tills en anslutning har upprättats eller det explicit avbryts och kan användas för att förhindra fel på grund av timeout i nätverket. Den här lösningen orsaka obestämd väntar. Var beredd på att stoppa anslutningar vid behov.

Om du inte uppgradera till de senaste kernel-versionerna, kan du undvika det här problemet genom att spara en fil i Azure-filresursen som du skriver till var 30: e sekund eller mindre. Detta måste vara en skrivåtgärd, till exempel skriva om skapade eller ändrade datum för filen. Annars kan du få cachelagrade resultaten och åtgärden kan inte utlösa återanslutning.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>”Montera error(115): pågår” när du monterar Azure File storage med hjälp av SMB 3.0

### <a name="cause"></a>Orsak

Vissa Linux-distributioner som ännu stöder inte krypteringsfunktioner i SMB 3.0 och användare kan få felmeddelandet ”115” om de försöker montera Azure File storage med hjälp av SMB 3.0 på grund av en funktion som saknas.

### <a name="solution"></a>Lösning

Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel. Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region. Den här funktionen har varit anpassats för Ubuntu nr 17.04 från och Ubuntu 16,10 vid tidpunkten för publicering. Om din Linux SMB-klienten inte har stöd för kryptering, montera Azure File storage med hjälp av SMB 2.1 från ett Azure virtuell Linux-dator som är i samma datacenter som File storage-konto.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Långsam prestanda på en Azure-filresursen monteras på en Linux-VM

### <a name="cause"></a>Orsak

En möjlig orsak till dåliga prestanda är inaktiverad cachelagring.

### <a name="solution"></a>Lösning

Om du vill kontrollera om cachelagring är inaktiverad, leta efter den **cache =** post. 

**Cache = ingen** anger att cachelagring har inaktiverats.  Montera resursen med hjälp av monteringskommandot eller genom att uttryckligen lägga till den **cache = strikt** alternativet att monteringskommandot så att standard cachelagring eller ”strikt” cachelagring läge är aktiverat.

I vissa fall kan den **serverino** mount-alternativet kan orsaka den **ls** kommando för att köra stat mot varje katalogposten. Detta resulterar i försämrade prestanda när du med en stor katalog. Du kan kontrollera mount-alternativ din **/etc/fstab** post:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Du kan också kontrollera om rätt alternativ som används genom att köra den **sudo montera | grep cifs** kommandot och kontrollera dess utdata, till exempel i följande exempel på utdata:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Om den **cache = strikt** eller **serverino** alternativet är inte finns, demontera och montera Azure File storage igen genom att köra monteringskommandot från den [dokumentationen](storage-how-to-use-files-linux.md). Kontrollera sedan som den **/etc/fstab** posten har rätt alternativ.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Tidsstämplar förlorades i kopiering av filer från Windows till Linux

På Linux/Unix-plattformar, den **cp -p** kommandot misslyckas om filen 1 och 2-filen ägs av olika användare.

### <a name="cause"></a>Orsak

Flaggan force **f** i COPYFILE resulterar i att köra **cp -p -f** på Unix. Det här kommandot inte heller går att bevara tidsstämpeln för den fil som du inte äger.

### <a name="workaround"></a>Lösning

Använd det storage-kontot för att kopiera filerna:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.
