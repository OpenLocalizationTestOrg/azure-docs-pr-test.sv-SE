---
title: "aaaWire lösning i Log Analytics | Microsoft Docs"
description: "Wire-data är konsoliderade nätverks- och data från datorer med OMS-agenter, inklusive Operations Manager och Windows-anslutna agenter. Nätverksdata kombineras med din logg data toohelp du korrelera data."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Överföring Data 2.0 (förhandsgranskning) lösning i logganalys

![Överföring Data symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

Wire-data är konsoliderade nätverks- och data från datorer med OMS-agenter, inklusive Operations Manager, Windows-ansluten och Linux-agenter. Nätverksdata kombineras med dina andra logga data toohelp du korrelera data.

Dessutom tooOMS agenter hello Wire-Data lösningen använder Microsoft Dependency agenter som installeras på datorer i din IT-infrastruktur. Beroende agenter Övervakare nätverk data som skickas tooand från datorer för nätverket nivåer 2-3 i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), inklusive hello olika protokoll och portar som används. Informationen skickas sedan tooLog Analytics använder agenter.

> [!NOTE]
> Du kan inte lägga till hello tidigare version av hello Wire-Data lösning toonew arbetsytor. Om du har hello ursprungliga Wire-Data-lösning aktiverat kan du fortsätta toouse den. Dock toouse överföring Data 2.0, måste du först ta bort hello originalversionen.

Som standard logganalys samlar in loggdata för CPU, minne, disk och nätverk prestandadata från räknare som är inbyggd i Windows. Nätverk och andra datainsamling görs realtid för varje agent, inklusive undernät och programnivå protokoll som används av hello-dator. Du kan lägga till andra prestandaräknare på inställningssidan för hello på hello-loggar.

Om du har använt [sFlow](http://www.sflow.org/) eller annan programvara med [Ciscos NetFlow protokollet](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)hello statistik och data som du ser från wire-data kommer att bekant tooyou.

Hello typer av inbyggda loggen sökfrågor bland annat:

- Agenter som tillhandahåller wire-data
- IP-adressen för agenter som tillhandahåller wire-data
- Utgående kommunikation via IP-adresser
- Antal byte skickade efter programprotokoll
- Antalet byte som skickats av en programtjänsten
- Byte mottagna efter olika protokoll
- Totalt antal byte som skickas och tas emot av IP-version
- Genomsnittlig svarstid för anslutningar som har ett tillförlitligt sätt
- Datorn bearbetar som initierat eller mottagit nätverkstrafik
- Mängden nätverkstrafik för en process

När du söker med wire-data, kan du filtrera och data tooview gruppinformation hello översta agenter och övre protokoll. Eller så kan du visa när vissa datorer (IP-adresser/MAC-adresser) kommunicerat med varandra, hur lång tid och hur mycket data skickades – i princip du visa metadata om nätverkstrafik som är Sökbaserade.

Eftersom du visar metadata, är det dock inte nödvändigtvis användbart för avancerad felsökning. Wire-data i logganalys är inte en fullständig insamlingen av nätverksdata. Så är det inte avsedd för djupt paketnivå felsökning. Hej fördelen med att använda hello agent, jämfört med tooother samling metoder, är att du inte har tooinstall installationer, konfigurera om dina nätverksväxlar eller utför komplicerade konfigurationer. Wire-data är helt enkelt agent-baserad – hello agent installeras på en dator och den ska övervaka sin egen nätverkstrafik. En annan fördel är om du vill toomonitor arbetsbelastningar som körs i molntjänstleverantörer eller värdleverantör eller Microsoft Azure, där hello användaren inte äger hello fabric lager.

## <a name="connected-sources"></a>Anslutna källor

Wire-Data hämtar data från hello Microsoft Beroendeagent. Hej Beroendeagent är beroende av hello OMS-Agent för dess anslutningar tooLog Analytics. Detta innebär att en server måste ha hello OMS-Agent installeras och konfigureras först och sedan installerar du hello Beroendeagent. hello beskrivs följande tabell hello anslutna källor som hello Wire-Data-lösningen stöder.

| **Ansluten datakälla** | **Stöds** | **Beskrivning** |
| --- | --- | --- |
| Windows-agenter | Ja | Wire-Data analyserar och samlar in data från datorer med Windows-agenten. <br><br> I tillägg toohello [OMS-Agent](log-analytics-windows-agents.md), Windows-agenter kräver hello Microsoft Beroendeagent. Se hello [operativsystem](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) för en fullständig lista över versioner av operativsystemet. |
| Linux-agenter | Ja | Wire-Data analyserar och samlar in data från datorer för Linux-agenten.<br><br> I tillägg toohello [OMS-Agent](log-analytics-linux-agents.md), Linux-agenter kräver hello Microsoft Beroendeagent. Se hello [operativsystem](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) för en fullständig lista över versioner av operativsystemet. |
| System Center Operations Manager-hanteringsgrupp | Ja | Wire-Data analyserar och samlar in data från Windows- och Linux-agenter i en ansluten [System Center Operations Manager-hanteringsgruppen](log-analytics-om-agents.md). <br><br> En direkt anslutning från hello System Center Operations Manager-agenten datorn tooLog Analytics krävs. Data vidarebefordras från hello management group tooLog Analytics. |
| Azure Storage-konto | Nej | Wire-Data samlar in data från agentdatorer, så det finns inga data från den toocollect från Azure Storage. |

På Windows hello Microsoft Monitoring Agent (MMA) används av både System Center Operations Manager och logganalys toogather och skicka data. Beroende på hello kontexten kallas hello agent hello System Center Operations Manager-Agent, OMS-Agent, Log Analytics Agent, MMA eller direkt Agent. System Center Operations Manager och Log Analytics tillhandahåller något olika versioner av hello MMA. Dessa versioner kan varje rapport tooSystem Center Operations Manager, tooLog Analytics eller tooboth.

På Linux, hello OMS-Agent för Linux samlar in och skickar data tooLog Analytics. Du kan använda Wire-Data på servrar med direkta OMS-Agent eller på servrar som är anslutna tooLog Analytics via System Center Operations Manager-hanteringsgrupper.

I den här artikeln refererar tooall agenter om Linux- eller Windows, om anslutna tooa System Center Operations Manager-hanteringsgruppen eller direkt tooLog Analytics kallas hello _OMS-agent_. Vi använder hello distributionsnamnet hello Agent endast om det behövs för kontext.

Hej Beroendeagent överföra inte alla data och kräver inte några ändringar toofirewalls eller portar. hello data Wire-Data skickas alltid av hello OMS-agenten tooLog Analytics, antingen direkt eller använder hello OMS-Gateway.

![agenten diagram](./media/log-analytics-wire-data/agents.png)

Om du är en System Center Operations Manager-användare med en management group anslutna tooLog Analytics:

- Ingen ytterligare konfiguration krävs när System Center Operations Manager-agenter kan komma åt hello Internet tooconnect tooLog Analytics.
- Du behöver tooconfigure hello OMS Gateway toowork med System Center Operations Manager när System Center Operations Manager-agenter inte kan komma åt logganalys över hello Internet.

Om du använder hello direkt Agent, behöver du tooconfigure hello OMS-agent själva tooconnect tooLog Analytics eller tooyour OMS-Gateway. Du kan hämta hello OMS Gateway från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Krav

- Kräver hello [insikt och Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) lösningen-erbjudandet.
- Om du använder hello tidigare version av hello Wire-Data lösningen måste du först bort den. Alla data som hämtats via hello ursprungliga Wire-Data-lösning är dock fortfarande tillgängliga i överföring Data 2.0 och Sök i loggfilen.
- Administratörsbehörighet är nödvändiga tooinstall eller avinstallera hello Beroendeagent.
- hello beroende agenten måste installeras på en dator med ett 64-bitars operativsystem.

### <a name="operating-systems"></a>Operativsystem

hello listar följande avsnitt hello stöds operativsystem för hello Beroendeagent. Wire-Data stöder inte 32-bitars arkitekturer för alla operativsystem.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows-skrivbordet

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux och Oracle Linux (med RHEL Kernel)

- Endast standard och SMP Linux kernel-versioner stöds.
- Avvikande kernel Frigör som PAE och Xen, inte stöds för Linux-distribution. Till exempel ett system med hello versionen sträng med _2.6.16.21-0.8-xen_ stöds inte.
- Anpassade kärnor, inklusive omkompileringar standard kärnor stöds inte.
- CentOSPlus kernel stöds inte.
- Oracle Unbreakable Enterprise Kernel (UEK) ingår i ett senare avsnitt i den här artikeln.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **OS-version** | **Kernel-version** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **OS-version** | **Kernel-version** |
| --- | --- |
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

| **OS-version** | **Kernel-version** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux med Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **OS-version** | **Kernel-version** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **OS-version** | **Kernel-version** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **OS-version** | **Kernel-version** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **OS-version** | **Kernel-version** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Hämtar Beroendeagent

| **Fil** | **OS** | **Version** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Konfiguration

Utför följande steg tooconfigure hello Wire-Data lösningen för dina arbetsytor hello.

1. Aktivera hello aktivitet logganalys lösningar från hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. Installera hello beroende agenten på varje dator där du vill tooget data. Hej Beroendeagent kan övervaka anslutningar tooimmediate grannar, så du inte kan ha en agent på varje dator.

### <a name="install-hello-dependency-agent-on-windows"></a>Installera hello Beroendeagent i Windows

Administratörsbehörighet är nödvändiga tooinstall eller avinstallera hello-agenten.

Hej Beroendeagent är installerad på Windows-datorer via InstallDependencyAgent Windows.exe. Om du kör den här körbara filen utan alternativ, startas en guide som att du kan följa tooinstall interaktivt.

Använd följande steg tooinstall hello beroende agenten på varje dator som kör Windows hello:

1. Installera hello OMS-agenten med hjälp av hello instruktionerna på [toohello ansluta Windows-datorer i Azure Log Analytics-tjänsten](log-analytics-windows-agents.md).
2. Hämta hello Windows-agenten via hello länk i hello föregående avsnitt och kör sedan med hjälp av följande kommando hello: InstallDependencyAgent Windows.exe
3. Följ hello guiden tooinstall hello agent.
4. Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation. På Windows-agenter är hello loggkatalogen %Programfiles%\Microsoft beroende Agent\logs.

#### <a name="windows-command-line"></a>Windows-kommandoraden

Använd alternativen från hello följande tabell tooinstall från en kommandorad. toosee en lista över hello flaggor för installation, kör installationsprogrammet för hello med hjälp av hello /? Flagga på följande sätt.

InstallDependencyAgent Windows.exe /?

| **Flaggan** | **Beskrivning** |
| --- | --- |
| <code>/?</code> | Hämta en lista över hello kommandoradsalternativ. |
| <code>/S</code> | Utföra en tyst installation utan uppmaningar för användaren. |

Filer för hello Windows Beroendeagent placeras i C:\Program Files\Microsoft beroende agenten som standard.

### <a name="install-hello-dependency-agent-on-linux"></a>Installera hello Beroendeagent på Linux

Rotåtkomst är nödvändiga tooinstall eller konfigurera hello agent.

Hej Beroendeagent är installerad på Linux-datorer via InstallDependencyAgent-Linux64.bin, ett kommandoskript med en självextraherande binär. Du kan köra hello-filen med hjälp av _del_ eller Lägg till kör behörigheter toohello själva filen.

Använd hello följa steg tooinstall hello beroende agenten på varje Linux-dator:

1. Installera hello OMS-agenten med hjälp av hello instruktionerna på [samla in och hantera data från Linux-datorer](log-analytics-agent-linux.md).
2. Hämta hello Linux beroendeagent via hello länk i hello föregående avsnitt och installera det som rot med hjälp av följande kommando hello: del InstallDependencyAgent Linux64.bin
3. Om hello Beroendeagent misslyckas toostart loggarna hello för Detaljerad felinformation. På Linux-agenter hello loggkatalogen är: /var/opt/microsoft/dependency-agent/log.

toosee en lista över hello flaggor för installation, kör installationsprogrammet för hello med hello `-help` flaggan på följande sätt.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Flaggan** | **Beskrivning** |
| --- | --- |
| <code>-help</code> | Hämta en lista över hello kommandoradsalternativ. |
| <code>-s</code> | Utföra en tyst installation utan uppmaningar för användaren. |
| <code>--check</code> | Kontrollera behörigheter och hello operativsystemet men inte installera hello agent. |

Filer för hello Beroendeagent placeras i hello följande kataloger:

| **Filer** | **Plats** |
| --- | --- |
| -Filer | /OPT/Microsoft/Dependency-Agent |
| Loggfiler | /var/OPT/Microsoft/Dependency-Agent/log |
| Config-filer | /etc/OPT/Microsoft/Dependency-Agent/config |
| Tjänsten körbara filer | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br><br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| Binära filer | /var/OPT/Microsoft/Dependency-Agent/Storage |

### <a name="installation-script-examples"></a>Exempel på skript för installation

tooeasily distribuera hello Beroendeagent på flera servrar samtidigt, hjälper det toouse ett skript. Du kan använda följande skript exempel toodownload hello och installera hello Beroendeagent på Windows- eller Linux.

#### <a name="powershell-script-for-windows"></a>Windows PowerShell-skript

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Shell-skript för Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Önskad tillståndskonfiguration

toodeploy hello Beroendeagent via Desired State Configuration kan du använda hello xPSDesiredStateConfiguration modulen och lite kod som hello nedan:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-hello-dependency-agent"></a>Avinstallera hello Beroendeagent

Använd följande avsnitt toohelp du ta bort hello Beroendeagent hello.

#### <a name="uninstall-hello-dependency-agent-on-windows"></a>Avinstallera hello Beroendeagent på Windows

En administratör kan avinstallera hello beroende agenten för Windows via Kontrollpanelen.

En administratör kan också köra %Programfiles%\Microsoft beroende Agent\Uninstall.exe toouninstall hello Beroendeagent.

#### <a name="uninstall-hello-dependency-agent-on-linux"></a>Avinstallera hello Beroendeagent på Linux

toocompletely avinstallera Hej Beroendeagent från Linux, måste du ta bort själva hello-agenten och hello connector, som installeras automatiskt med hello agent. Du kan avinstallera både med hjälp av hello följande enskilt kommando:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Hanteringspaket

När Wire-Data är aktiverat i logganalys-arbetsytan, skickas ett 300 KB hanteringspaket tooall hello Windows-servrar på arbetsytan. Om du använder System Center Operations Manager-agenter i en [ansluten hanteringsgrupp](log-analytics-om-agents.md), hello Dependency Monitor-hanteringspaket distribueras från System Center Operations Manager. Om hello agenter är anslutna direkt levererar logganalys hello management pack.

hello management pack heter Microsoft.IntelligencePacks.ApplicationDependencyMonitor. De skrivs till: %Programfiles%\Microsoft övervakning Agent\Agent\Health State\Management servicepack. hello-datakälla som använder hello hanteringspaket är: % Program files%\Microsoft övervakning Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-hello-solution"></a>Med hello-lösning

**Installera och konfigurera hello lösning**

Använd följande information tooinstall hello och konfigurera hello lösning.

- hello Wire-Data lösningen hämtar data från datorer som kör Windows Server 2012 R2, Windows 8.1 och senare operativsystem.
- Microsoft .NET Framework 4.0 eller senare krävs för datorer som du vill tooacquire wire-data från.
- Lägg till hello Wire-Data lösning tooyour logganalys-arbetsytan med hjälp av hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). Det krävs ingen ytterligare konfiguration.
- Om du vill tooview wire-data för en viss lösning behöver du toohave hello lösning redan lagts till tooyour arbetsytan.

När du har agenter som installerats och du installerar hello lösning, visas hello överföring Data 2.0 panelen på arbetsytan.

> [!NOTE]
> För närvarande måste du använda hello OMS-portalen tooview wire-data. Du kan inte använda hello Azure portal tooview wire-data.

![Panelen Wire-Data](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a>Med hello överföring Data 2.0-lösning

I hello OMS-portalen klickar du på hello **överföring Data 2.0** panelen tooopen hello Wire-Data instrumentpanelen. hello instrumentpanelen innehåller hello blad i hello i den följande tabellen. Varje bladet visar upp too10 objekt som matchar att bladets kriterier för hello angetts scope och ett tidsintervall. Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på **se alla** längst ned hello hello bladet eller genom att klicka på hello bladet sidhuvud.

| **Bladet** | **Beskrivning** |
| --- | --- |
| Agenter som samlar in nätverkstrafik | Visar hello antalet agenter som hämtar nätverkstrafik och visar hello översta 10 datorer som hämtar trafik. Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>. Klicka på en dator i hello listan toorun en logg sökning returnerar hello sammanlagt antal byte som har hämtats. |
| Lokala undernät | Visar hello antalet lokala undernät som agenter har identifierats.  Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> som visar en lista över alla undernät med hello antal byte som skickats över var och en. Klicka på ett undernät i hello listan toorun en logg sökning returnerar hello Totalt antal byte som skickats över hello undernät. |
| Protokoll på programnivå | Visar hello antalet protokoll på programnivå används, som identifierats av agenter. Klicka på hello nummer toorun en logg sökning efter <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>. Klicka på ett protokoll toorun en logg sökning returnerar hello sammanlagt antal byte som skickats med hello-protokollet. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Överföring Data instrumentpanelen](./media/log-analytics-wire-data/wire-data-dash.png)

Du kan använda hello **agenter som samlar in nätverkstrafik** bladet toodetermine hur mycket nätverksbandbredd som används av datorer. Det här bladet kan hjälpa dig att enkelt hitta hello _chattiest_ dator i din miljö. Sådana datorer kan vara överbelastad fungerar onormalt eller använder mer nätverksresurser än normalt.

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example01.png)

På liknande sätt kan du använda hello **lokala undernät** bladet toodetermine hur mycket nätverkstrafik gå igenom dina undernät. Användarna definiera ofta undernät runt viktiga områden för sina program. Det här bladet ger en överblick över dessa områden.

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example02.png)

Hej **protokoll på programnivå** bladet är användbar eftersom den är användbara veta vilka protokoll som används. Du kan till exempel förvänta SSH toonot vara används i din nätverksmiljö. Visa information som är tillgängliga i hello bladet kan snabbt bekräfta eller disprove din förväntan.

![loggen Sök-exempel](./media/log-analytics-wire-data/log-search-example03.png)

I det här exemplet kan gå till SSH information toosee vilka datorer som använder SSH och många andra kommunikationsdetaljer.

![del sökresultat](./media/log-analytics-wire-data/ssh-details.png)

Det är också användbart tooknow om protokolltrafik ökar eller minskar med tiden. Till exempel om ökar hello mängden data som skickas av ett program som kan vara något du bör vara medveten om, eller att du kan hitta anmärkningsvärda.

## <a name="input-data"></a>Indata

Wire-data samlas in metadata om nätverkstrafik som använder hello agenter som du har aktiverat. Varje agent skickar data om var 15: e sekund.

## <a name="output-data"></a>utdata

En post med en typ av _WireData_ skapas för varje typ av indata. WireData poster har egenskaper som visas i följande tabell hello:

| Egenskap | Beskrivning |
|---|---|
| Dator | Datornamnet där data har samlats in |
| TimeGenerated | Tiden hello posten |
| LocalIP | Hello lokala datorns IP-adress |
| SessionState | Ansluten eller frånkopplad |
| receivedBytes | Mängden byte som tagits emot |
| ProtocolName | Namnet på hello nätverksprotokoll som används |
| IPVersion | IP-version |
| Riktning | Inkommande eller utgående |
| MaliciousIP | IP-adressen för en känd skadlig källa |
| Allvarsgrad | Allvarlighetsgrad för misstänkt skadlig programvara |
| RemoteIPCountry | Land hello fjärranslutna IP-adress |
| ManagementGroupName | Namnet på hanteringsgruppen för hello Operations Manager |
| SourceSystem | Källan där data har samlats in |
| SessionStartTime | Starttid för session |
| SessionEndTime | Sluttid för session |
| LocalSubnet | Undernät där data har samlats in |
| LocalPortNumber | Lokala portnumret |
| RemoteIP | Fjärranslutna IP-adress som används av hello fjärrdatorn |
| RemotePortNumber | Portnummer för hello fjärranslutna IP-adress |
| Sessions-ID | Ett unikt värde som identifierar kommunikationssessionen mellan två IP-adresser |
| sentBytes | Antal skickade byte |
| TotalBytes | Totalt antal byte som skickats under sessionen |
| ApplicationProtocol | Nätverksprotokoll som används   |
| Process-ID | Windows process-ID |
| Processnamn | Sökvägen och filnamnet för hello-process |
| RemoteIPLongitude | IP-Longitudvärde |
| RemoteIPLatitude | IP-latitud värde |


## <a name="next-steps"></a>Nästa steg

- [Söka i loggar](log-analytics-log-searches.md) tooview detaljerad överföring sökposter.
