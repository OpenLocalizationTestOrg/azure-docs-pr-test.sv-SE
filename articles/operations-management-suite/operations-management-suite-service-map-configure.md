---
title: "aaaConfigure Tjänstkarta i Operations Management Suite | Microsoft Docs"
description: "Tjänstkarta är en Operations Management Suite som automatiskt identifierar programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster. Den här artikeln innehåller information för att distribuera Tjänstkarta i din miljö och använda den i en mängd olika scenarier."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 3127f4440f2886370f8ff617c405c6d70a926eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-service-map-in-operations-management-suite"></a>Konfigurera Tjänstkarta i Operations Management Suite
Tjänstkarta identifierar automatiskt programkomponenter för Windows och Linux-datorer och maps hello kommunikation mellan tjänster. Du kan använda den tooview dina servrar som du betrakta dem--som sammanlänkade system som levererar kritiska tjänster. Tjänstkarta visar anslutningar mellan servrar, processer och portar i alla TCP-anslutna arkitektur med än installation av en agent krävs ingen konfiguration.

Den här artikeln beskriver hello information om att konfigurera agenter för Tjänstkarta och onboarding. Information om hur du använder Tjänstkarta finns [använda hello Tjänstkarta lösning i Operations Management Suite](operations-management-suite-service-map.md).

## <a name="dependency-agent-downloads"></a>Hämtar Beroendeagent
| Fil | Operativsystem | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |


## <a name="connected-sources"></a>Anslutna källor
Tjänstkarta hämtar data från hello Microsoft Beroendeagent. Hej Beroendeagent är beroende av hello OMS-Agent för dess anslutningar tooOperations Management Suite. Det innebär att en server måste ha hello OMS-Agent installeras och konfigureras först och sedan hello beroende agenten kan installeras. hello beskrivs följande tabell hello anslutna källor som har stöd för hello Tjänstkarta lösning.

| Ansluten datakälla | Stöds | Beskrivning |
|:--|:--|:--|
| Windows-agenter | Ja | Tjänstkarta analyserar och samlar in data från datorer med Windows-agenten. <br><br>I tillägg toohello [OMS-Agent](../log-analytics/log-analytics-windows-agents.md), Windows-agenter kräver hello Microsoft Beroendeagent. Se hello [operativsystem](#supported-operating-systems) för en fullständig lista över versioner av operativsystemet. |
| Linux-agenter | Ja | Tjänstkarta analyserar och samlar in data från datorer för Linux-agenten. <br><br>I tillägg toohello [OMS-Agent](../log-analytics/log-analytics-linux-agents.md), Linux-agenter kräver hello Microsoft Beroendeagent. Se hello [operativsystem](#supported-operating-systems) för en fullständig lista över versioner av operativsystemet. |
| System Center Operations Manager-hanteringsgrupp | Ja | Tjänstkarta analyserar och samlar in data från Windows- och Linux-agenter i en ansluten [System Center Operations Manager-hanteringsgruppen](../log-analytics/log-analytics-om-agents.md). <br><br>En direkt anslutning från hello System Center Operations Manager-agenten datorn tooOperations Management Suite krävs. Data vidarebefordras från hello management group toohello Operations Management Suite-databasen.|
| Azure Storage-konto | Nej | Tjänstkarta samlar in data från agentdatorer, så det finns inga data från den toocollect från Azure Storage. |

Tjänstkarta stöder endast 64-bitars plattformar.

För Windows hello Microsoft Monitoring Agent (MMA) används av både System Center Operations Manager och Operations Management Suite toogather och skicka övervakningsdata. (Den här agenten kallas hello System Center Operations Manager-Agent, OMS-Agent, Log Analytics Agent, MMA eller direkt Agent, beroende på hello-kontexten.) System Center Operations Manager och Operations Management Suite ger olika ut för hello box-versioner av hello MMA. Dessa versioner kan varje rapport tooSystem Center Operations Manager, tooOperations Management Suite eller tooboth.  

Hej OMS-Agent på Linux, för Linux samlar in och skickar övervakning data tooOperations Management Suite. Du kan använda Tjänstkarta på servrar med direkta OMS-Agent eller på servrar som är anslutna tooOperations Management Suite via System Center Operations Manager-hanteringsgrupper.  

I den här artikeln ska vi se tooall agenter--om Linux- eller Windows, om anslutna tooa System Center Operations Manager-hanteringsgrupp eller direkt tooOperations Management Suite--som hello ”OMS-Agent”. Vi använder hello distributionsnamnet hello Agent endast om det behövs för kontext.

Hej Tjänstkarta agent överföra inte alla data och kräver inte några ändringar toofirewalls eller portar. hello data i Tjänstkartan överförs alltid hello OMS-Agent tooOperations Management Suite, antingen direkt eller via hello OMS-Gateway.

![Tjänstkarta agenter](media/oms-service-map/agents.png)

Om du är en System Center Operations Manager-kund med en management group anslutna tooOperations Management Suite:

- Om System Center Operations Manager-agenter kan komma åt hello Internet tooconnect tooOperations Management Suite, krävs ingen ytterligare konfiguration.  
- Om System Center Operations Manager-agenter inte kan komma åt Operations Management Suite över hello Internet, måste tooconfigure hello OMS Gateway toowork med System Center Operations Manager.
  
Om du använder hello direkt OMS-Agent, behöver du tooconfigure hello själva OMS-agenten tooconnect tooOperations Management Suite eller tooyour OMS-Gateway. hello OMS-Gateway kan hämtas från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Hanteringspaket
När Tjänstkarta är aktiverat i en Operations Management Suite-arbetsyta, skickas ett 300 KB management pack tooall hello Windows-servrar på arbetsytan. Om du använder System Center Operations Manager-agenter i en [ansluten hanteringsgrupp](../log-analytics/log-analytics-om-agents.md), hello Tjänstkarta hanteringspaket distribueras från System Center Operations Manager. Om hello agenter är anslutna direkt levererar Operations Management Suite hello management pack.

hello management pack heter Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Det är skrivna too%Programfiles%\Microsoft övervakning Agent\Agent\Health Service State\Management Packs\. hello datakälla som använder hello hanteringspaket är % Program files%\Microsoft övervakning Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Installation
### <a name="install-hello-dependency-agent-on-microsoft-windows"></a>Installera hello Beroendeagent på Microsoft Windows
Administratörsbehörighet är nödvändiga tooinstall eller avinstallera hello-agenten.

Hej Beroendeagent är installerad på Windows-datorer i InstallDependencyAgent Windows.exe. Om du kör den här körbara filen utan alternativ, startas en guide som att du kan följa tooinstall interaktivt.  

Använd följande steg tooinstall hello beroende agenten på varje dator i Windows hello:

1.  Installera hello OMS-agenten med hjälp av hello instruktionerna på [toohello ansluta Windows-datorer i Azure Log Analytics-tjänsten](../log-analytics/log-analytics-windows-agents.md).
2.  Ladda ned Windows agent hello och kör den med hjälp av hello följande kommando: <br>`InstallDependencyAgent-Windows.exe`
3.  Följ hello guiden tooinstall hello agent.
4.  Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation. På Windows-agenter är hello loggkatalogen %Programfiles%\Microsoft beroende Agent\logs. 

#### <a name="windows-command-line"></a>Windows-kommandoraden
Använd alternativen från hello följande tabell tooinstall från en kommandorad. toosee en lista över hello flaggor för installation, kör installationsprogrammet för hello med hjälp av hello /? Flagga på följande sätt.

    InstallDependencyAgent-Windows.exe /?

| Flaggan | Beskrivning |
|:--|:--|
| /? | Hämta en lista över hello kommandoradsalternativ. |
| / S | Utföra en tyst installation utan uppmaningar för användaren. |

Filer för hello Windows Beroendeagent placeras i C:\Program Files\Microsoft beroende agenten som standard.

### <a name="install-hello-dependency-agent-on-linux"></a>Installera hello Beroendeagent på Linux
Rotåtkomst är nödvändiga tooinstall eller konfigurera hello agent.

Hej Beroendeagent är installerad på Linux-datorer via InstallDependencyAgent-Linux64.bin, ett kommandoskript med en självextraherande binär. Du kan köra hello-filen med hjälp av del eller Lägg till kör behörigheter toohello själva filen.
 
Använd hello följa steg tooinstall hello beroende agenten på varje Linux-dator:

1.  Installera hello OMS-agenten med hjälp av hello instruktionerna på [samla in och hantera data från Linux-datorer](https://technet.microsoft.com/library/mt622052.aspx).
2.  Installera hello Linux beroendeagent som rot med hello följande kommando:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation. Hello loggkatalogen är /var/opt/microsoft/dependency-agent/log på Linux-agenter.

toosee en lista över hello installationen flaggor, kör installationsprogrammet för hello med hello - hjälp flaggan på följande sätt.

    InstallDependencyAgent-Linux64.bin -help

| Flaggan | Beskrivning |
|:--|:--|
| -hjälp | Hämta en lista över hello kommandoradsalternativ. |
| -s | Utföra en tyst installation utan uppmaningar för användaren. |
| – Kontrollera | Kontrollera behörigheter och hello operativsystemet men inte installera hello agent. |

Filer för hello Beroendeagent placeras i hello följande kataloger:

| Filer | Plats |
|:--|:--|
| -Filer | /OPT/Microsoft/Dependency-Agent |
| Loggfiler | /var/OPT/Microsoft/Dependency-Agent/log |
| Config-filer | /etc/OPT/Microsoft/Dependency-Agent/config |
| Tjänsten körbara filer | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| Binära filer | /var/OPT/Microsoft/Dependency-Agent/Storage |

## <a name="installation-script-examples"></a>Exempel på skript för installation
tooeasily distribuera hello Beroendeagent på flera servrar samtidigt, hjälper det toouse ett skript. Du kan använda följande skript exempel toodownload hello och installera hello Beroendeagent på Windows- eller Linux.

### <a name="powershell-script-for-windows"></a>Windows PowerShell-skript
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Shell-skript för Linux
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Önskad tillståndskonfiguration
toodeploy hello Beroendeagent via Desired State Configuration kan du använda hello xPSDesiredStateConfiguration modulen och lite kod som hello nedan:
```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install hello Dependency Agent
    xRemoteFile DAPackage 
    {
        Uri = "https://aka.ms/dependencyagentwindows"
        DestinationPath = $DAPackageLocalPath
    }

    xPackage DA
    {
        Ensure="Present"
        Name = "Dependency Agent"
        Path = $DAPackageLocalPath
        Arguments = '/S'
        ProductId = ""
        InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
        InstalledCheckRegValueName = "DisplayName"
        InstalledCheckRegValueData = "Dependency Agent"
        DependsOn = "[xRemoteFile]DAPackage"
    }
  }
}
```

## <a name="uninstallation"></a>Avinstallation
### <a name="uninstall-hello-dependency-agent-on-windows"></a>Avinstallera hello Beroendeagent på Windows
En administratör kan avinstallera hello beroende agenten för Windows via Kontrollpanelen.

En administratör kan också köra %Programfiles%\Microsoft beroende Agent\Uninstall.exe toouninstall hello Beroendeagent.

### <a name="uninstall-hello-dependency-agent-on-linux"></a>Avinstallera hello Beroendeagent på Linux
toocompletely avinstallera Hej Beroendeagent från Linux, måste du ta bort själva hello-agenten och hello connector, som installeras automatiskt med hello agent. Du kan avinstallera både med hjälp av hello följande enskilt kommando:

    rpm -e dependency-agent dependency-agent-connector

## <a name="troubleshooting"></a>Felsökning
Om du har några problem med installeras eller köras Tjänstkarta kan i det här avsnittet hjälpa dig. Kontakta Microsoft-supporten om du fortfarande inte kan lösa problemet.

### <a name="dependency-agent-installation-problems"></a>Beroende Agent installationsproblem
#### <a name="installer-asks-for-a-reboot"></a>Installationsprogrammet frågar en omstart
Hej Beroendeagent *vanligtvis* inte kräver en omstart vid installation eller avinstallation. Men kräver vissa sällsynta fall, en omstart toocontinue med en installation på Windows Server. Detta händer när en omstart krävs för ett beroende, vanligtvis hello Microsoft Visual C++ Redistributable, på grund av en låst fil.

#### <a name="message-unable-tooinstall-dependency-agent-visual-studio-runtime-libraries-failed-tooinstall-code--codenumber-appears"></a>Meddelandet ”tooinstall Beroendeagent: bibliotek Visual Studio-körning misslyckades tooinstall (felkod = [code_number])” visas

hello Microsoft Beroendeagent bygger på hello bibliotek för Microsoft Visual Studio-körning. Du får ett meddelande om ett problem har uppstått under installationen av hello-bibliotek. 

hello runtime library installationsprogram skapa loggar i hello %LOCALAPPDATA%\temp. hello-filen är dd_vcredist_arch_yyyymmddhhmmss.log, där *arkitektur* är ”x86” eller ”amd64” och *ååååmmddhhmmss* är hello datum och tid (24-timmarsformat) när hello loggen har skapats. hello-loggen innehåller information om hello problem som blockerar installationen.

Det kan vara användbart tooinstall hello [bibliotek för senaste körning](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) själv först.

hello följande tabell visas nummer och förslag på lösningar.

| Kod | Beskrivning | Lösning |
|:--|:--|:--|
| 0x17 | hello biblioteket installer kräver en Windows-uppdatering inte har installerats. | Titta i hello senaste installer-loggen i biblioteket.<br><br>Om en referens för ”Windows8.1-KB2999226-x64.msu” följt av en rad ”fel 0x80240017: misslyckade tooexecute MSU-paket”, du har inte hello krav tooinstall KB2999226. Följ instruktionerna för hello i hello avsnittet med förutsättningar i [Universal C körning i Windows](https://support.microsoft.com/kb/2999226). Du kanske behöver toorun Windows Update och starta om flera gånger i ordning tooinstall hello krav.<br><br>Kör installationsprogrammet för hello Microsoft beroende agenten igen. |

### <a name="post-installation-issues"></a>Efter installationen problem
#### <a name="server-doesnt-appear-in-service-map"></a>Servern visas inte i Tjänstkartan
Om Beroendeagent-installationen har slutförts, men du inte ser servern i hello Tjänstkarta lösningen:
* Hej Beroendeagent installeras? Du kan kontrollera detta genom att markera toosee om hello-tjänsten är installerad och körs.<br><br>
**Windows**: Leta efter hello tjänst med namnet ”Microsoft Dependency Agent”.<br>
**Linux**: Leta efter hello kör processen ”microsoft-beroende-agent”.

* Är du på hello [kostnadsfria prisnivån för Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? hello fria abonnemang tillåter upp toofive unika Tjänstkarta servrar. Övriga servrar visas inte i Tjänstkartan, även om hello tidigare fem längre skickar data.

* Är din skicka serverloggen och perf data tooOperations Management Suite? Gå tooLog Sök och kör följande fråga för datorn hello: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Fick du ett antal händelser i hello resultat? Är hello data senaste? I så fall, fungerar OMS-Agent och kommunicerar med hello Operations Management Suite-tjänsten. Om inte, kontrollera hello OMS-agenten på servern: [OMS-Agent för Windows felsökning](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) eller [OMS-Agent för Linux felsökning](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Servern visas i Tjänstkartan men har inga processer
Om du ser servern i Tjänstkartan, men det finns ingen process eller anslutning till data, som anger att hello Beroendeagent är installerad och körs, men hello kernel-drivrutinen lästes inte in. 

Kontrollera hello C:\Program Files\Microsoft beroende Agent\logs\wrapper.log fil (Windows) eller /var/opt/microsoft/dependency-agent/log/service.log (Linux). hello sista raderna i hello filen bör ange varför hello kernel lästes inte in. Till exempel hello kernel kanske inte finns stöd för Linux om du har uppdaterat din kernel.

## <a name="data-collection"></a>Datainsamling
Du kan förvänta dig varje agent tootransmit ungefär 25 MB per dag, beroende på hur komplex din system beroenden är. Varje agent skickar Tjänstkarta beroendedata var 15: e sekund.  

Hej Beroendeagent förbrukar vanligtvis 0.1 systemminne och 0,1 procent för processor.

## <a name="supported-azure-regions"></a>Azure-regioner som stöds
Tjänstkarta är tillgängliga i hello följande Azure-regioner:
- Östra USA
- Västra Europa
- Västra centrala USA


## <a name="supported-operating-systems"></a>Operativsystem som stöds
hello listar följande avsnitt hello stöds operativsystem för hello Beroendeagent. Tjänstkarta stöder inte 32-bitars arkitekturer för alla operativsystem.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-skrivbordet
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux och Oracle Linux (med RHEL Kernel)
- Endast standard och SMP Linux kernel-versioner stöds.
- Avvikande kernel Frigör som PAE och Xen, inte stöds för Linux-distribution. Till exempel stöds ett system med hello versionen sträng med ”2.6.16.21-0.8-xen” inte.
- Anpassade kärnor, inklusive omkompileringar standard kärnor stöds inte.
- CentOSPlus kernel stöds inte.
- Oracle Unbreakable Enterprise Kernel (UEK) ingår i ett senare avsnitt i den här artikeln.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| OS-version | Kernel-version |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| OS-version | Kernel-version |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| OS-version | Kernel-version |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux med Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| OS-version | Kernel-version
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| OS-version | Kernel-version
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| OS-version | Kernel-version
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| OS-version | Kernel-version
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>diagnostik och användningsdata
Microsoft samlar automatiskt in användnings- och prestandadata via din användning av hello Tjänstkarta service. Microsoft använder denna data tooprovide och förbättra hello kvalitet, säkerhet och integritet hello Tjänstkarta service. Data innehåller information om hello konfiguration av programvaran som operativsystem och version. Den omfattar även IP-adress, DNS-namn och arbetsstationen namn i ordning tooprovide korrekta och effektiva funktioner för felsökning. Vi samlar inte in namn, adresser eller annan kontaktinformation.

Mer information om insamling och användning finns i hello [sekretesspolicy för Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nästa steg
- Lär dig hur för[använder Tjänstkarta](operations-management-suite-service-map.md) när den har distribuerats och konfigurerats.
