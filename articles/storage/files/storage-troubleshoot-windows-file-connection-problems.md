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
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a>Felsökning av problem med Azure File storage i Windows

Den här artikeln innehåller vanliga problem som är relaterade tooMicrosoft Azure File storage när du ansluter från Windows-klienter. Det ger också möjliga orsaker och lösningar på problemen. Dessutom toohello felsökning steg i den här artikeln kan du också använda [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) så att hello Windows klientmiljö har rätt krav. AzFileDiagnostics automatiserar identifiering av de flesta hello symtom som nämns i den här artikeln och hjälper dig att konfigurera din miljö tooget optimala prestanda. Du kan också använda informationen i hello [Azure Files delar felsökaren](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) som ger steg tooassist du problem ansluter/mappning/montering Azure Files delar.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Fel 53, fel 67 eller fel 87 när du montera eller demontera en Azure-filresurs

När du försöker toomount en filresurs från lokalt eller från ett annat datacenter kan du få hello följande fel:

- Systemfel 53 har uppstått. hello nätverkssökvägen hittades inte.
- Systemfel 67 har uppstått. hello nätverkets namn kan inte hittas.
- Systemfel 87 har uppstått. hello-parametern är felaktig.

### <a name="cause-1-unencrypted-communication-channel"></a>Orsak 1: Okrypterad kommunikationskanalen

Av säkerhetsskäl hello anslutningar tooAzure filresurser blockeras om hello kommunikationskanalen inte är krypterad och hello anslutningsförsöket inte genomförs från samma datacenter där hello Azure-filresurser finns. Kryptering för kommunikation kanal tillhandahålls endast om hello användarens klientens operativsystem stöder SMB-kryptering.

Windows 8, Windows Server 2012 och senare versioner av varje system för att förhandla begäranden som innehåller SMB 3.0, som stöder kryptering.

### <a name="solution-for-cause-1"></a>Lösning för orsak 1

Ansluta från en klient som har en av följande hello:

- Uppfyller hello av Windows 8 och Windows Server 2012 eller senare versioner
- Ansluter från en virtuell dator i hello samma datacenter som hello Azure storage-konto som används för hello Azure-filresursen

### <a name="cause-2-port-445-is-blocked"></a>Orsak 2: Port 445 är blockerad

Systemfel 53 eller systemfel 67 kan inträffa om port 445 utgående kommunikation tooan Azure File storage datacenter är blockerad. toosee hello sammanfattning av Internet-leverantörer som Tillåt eller inte Tillåt åtkomst från port 445, gå för[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

toounderstand om detta är hello orsak bakom hälsningsmeddelande ”Systemfel 53” du kan använda Portqry tooquery hello TCP:445 slutpunkt. Om hello TCP:445 endpoint visas som filtrerats, blockeras hello TCP-port. Här är en exempelfråga:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Om TCP-port 445 blockeras av en regel längs sökvägen hello visas hello följande utdata:

  `TCP port 445 (microsoft-ds service): FILTERED`

Mer information om hur toouse Portqry, se [beskrivning av hello Portqry.exe command-line utility](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Lösning för orsak 2

Arbeta med din IT-avdelningen tooopen port 445 utgående för[Azure IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>Orsak 3: NTLMv1 är aktiverat

Systemfel 53 eller systemfel 87 kan inträffa om NTLMv1 kommunikation är aktiverat på hello-klienten. Azure File storage stöder NTLMv2-autentisering. Med NTLMv1 aktiverad skapar en mindre säkra klient. Därför blockeras kommunikation för Azure File storage. 

toodetermine om detta är hello felorsak hello, kontrollera att följande registerundernyckel hello är tooa värdet 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Mer information finns i hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) avsnitt på TechNet.

### <a name="solution-for-cause-3"></a>Lösning för orsak 3

Återställa hello **LmCompatibilityLevel** värdet toohello standardvärdet 3 i hello följande registerundernyckel:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a>Fel 1816 ”finns inte tillräckligt med kvoten är tillgängliga tooprocess kommandot” när du kopierar tooan Azure-filresursen

### <a name="cause"></a>Orsak

Fel 1816 händer när du når hello övre gräns för samtidiga öppna referenser som tillåts för en fil på hello dator där hello filresurs monteras.

### <a name="solution"></a>Lösning

Minska hello antalet samtidiga öppna referenser genom att stänga några referenser och försök sedan igen. Mer information finns i [Microsoft Azure Storage checklistan för prestanda och skalbarhet](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a>Långsam fil kopierar tooand från Azure File storage i Windows

Du kan se långsamma prestanda när du försöker tootransfer filer toohello tjänsten Azure File.

- Om du inte har ett visst minsta i/o-storlek krav, rekommenderar vi att du använder 1 MB som hello i/o-storlek för optimala prestanda.
-   Om du vet hello slutliga storlek på en fil som du utökar med skriver och programvaran har inte kompatibilitetsproblem när hello unwritten slutet på hello-filen innehåller nollor, sedan ange hello filstorlek i förväg i stället för att varje skrivning en utöka skrivning.
-   Använd hello rätt copy-metoden:
    -   Använd [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) för alla överföring mellan två filresurser.
    -   Använd [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mellan filresurser på en lokal dator.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Överväganden för Windows 8.1 eller Windows Server 2012 R2

För klienter som kör Windows 8.1 eller Windows Server 2012 R2, se till att hello [KB3114025](https://support.microsoft.com/help/3114025) snabbkorrigeringen är installerad. Den här snabbkorrigeringen förbättrar hello prestanda för create och Stäng referenser.

Du kan köra hello följande skript toocheck om hello snabbkorrigeringen har installerats:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Om snabbkorrigeringen installeras visas hello följande utdata:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2-avbildningar i Azure Marketplace har snabbkorrigering KB3114025 installeras som standard, från och med December 2015.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Ingen mapp med en enhetsbeteckning i **datorn**

Om du mappar en Azure-filresurs som en administratör med hjälp av net Använd hello resurs visas toobe saknas.

### <a name="cause"></a>Orsak

Som standard körs inte Utforskaren i Windows som administratör. Om du kör net Använd från en administrativ kommandotolk mappa hello nätverksenhet som administratör. Eftersom mappade enheter är användarcentrerad, visas inte hello-användarkonto som loggas i hello enheter om de är monterade under ett annat användarkonto.

### <a name="solution"></a>Lösning
Montera hello resurs från en kommandorad för icke-administratörer. Du kan också följa [den här TechNet-artikeln](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** registervärdet.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a>Kommandot net use misslyckas om hello storage-konto innehåller ett snedstreck

### <a name="cause"></a>Orsak

kommandot net use för hello tolkar ett snedstreck (/) som ett kommandoradsalternativ. Om ditt användarkonto börjar med ett snedstreck, misslyckas hello enhetsmappning.

### <a name="solution"></a>Lösning

Du kan använda något av följande steg toowork runt hello hello:

- Kör följande PowerShell-kommando hello:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Du kan köra det här sättet hello-kommando från en batchfil:

  `Echo new-smbMapping ... | powershell -command –`

- Placera citattecken runt hello viktiga toowork problemet--om hello snedstreck är hello första felaktiga tecknet. Om det är Använd hello interaktivt läge och ange ditt lösenord separat eller återskapa dina nycklar tooget en nyckel som inte börjar med ett snedstreck.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a>Programmet eller tjänsten kan inte komma åt en monterad enhet i Azure File storage

### <a name="cause"></a>Orsak

Enheter som är monterade per användare. Om programmet eller tjänsten körs under ett annat användarkonto än hello som monterad hello enhet visas inte programmet hello hello enhet.

### <a name="solution"></a>Lösning

Använd någon av följande lösningar hello:

-   Montera hello enhet från hello samma användarkonto som innehåller hello program. Du kan använda ett verktyg som PsExec.
- Överför hello lagringskontonamn och nyckel i hello användarens namn och lösenord parametrarna för kommandot net use för hello.

När du har följt anvisningarna får du följande felmeddelande när du kör net användning för hello system/Nätverkstjänstkonto hello: ”systemfel 1312 har uppstått. En angiven inloggningssession finns inte. Det kanske redan har avslutats ”. Om detta inträffar, kontrollerar du att det hello användarnamnet som skickades toonet Använd innehåller domäninformation (till exempel ”: [lagringskontonamn]. file.core.windows .net”).

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a>Fel som ”du kopierar en fil tooa mål som inte har stöd för kryptering”

När en fil har kopierats över hello nätverk, dekrypteras hello-filen på hello källdator skickas i klartext och omkrypteras på hello mål. Men du kan se hello följande fel när du försöker toocopy en krypterad fil: ”kopierar du hello filen tooa mål som inte har stöd för kryptering”.

### <a name="cause"></a>Orsak
Det här problemet kan inträffa om du använder EFS (ENCRYPTING File System). BitLocker-krypterade filer kan vara kopierade tooAzure File storage. Azure File storage stöder dock inte NTFS EFS.

### <a name="workaround"></a>Lösning
toocopy en fil hello nätverket, du måste först dekryptera den. Använd någon av följande metoder hello:

- Använd hello **Kopiera /d** kommando. Tillåter hello krypterade filer toobe sparas som dekrypterade filerna på hello mål.
- Ange hello följande registernyckel:
  - Sökväg = HKLM\Software\Policies\Microsoft\Windows\System
  - Värdetypen = DWORD
  - Name = CopyFileAllowDecryptedRemoteDestination
  - Värde = 1

Tänk på registernyckeln inställningen hello påverkar alla åtgärder på en kopia som görs toonetwork resurser.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet löst snabbt.
