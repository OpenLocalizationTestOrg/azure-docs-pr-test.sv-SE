---
title: aaaHow toouse PerfInsights i Microsoft Azure | Microsoft Docs
description: "Lär sig hur toouse PerfInsights tootroubleshoot Windows VM prestandaproblem."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: genli
ms.openlocfilehash: f23ff7708c0c63bd02674b1bdc07753e8a89d9be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-perfinsights"></a>Hur toouse PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) är ett automatiserat skript som samlar in viktig diagnostisk information, körs i/o-belastning stress och ger en analys rapporten toohelp felsöka Windows VM prestandaproblem i Microsoft Azure. 

Vi rekommenderar att du kör det här skriptet innan du skapar ett supportärende hos Microsoft VM prestandaproblem.

## <a name="supported-troubleshooting-scenarios"></a>Felsökning scenarier som stöds

PerfInsights kan samla in och analysera flera typer av information som är grupperade i unika scenarier.

### <a name="collect-disk-configuration"></a>Samla in diskkonfigurationen 

Det här scenariot samlar in hello diskkonfigurationen och annan viktig information, inklusive hello följande objekt:

-   Händelseloggar

-   Nätverkets status för alla inkommande och utgående anslutningar

-   Inställningarna för nätverk och brandvägg

-   Uppgiftslista för alla program som körs på hello system

-   Informationsfilen som skapas av msinfo32 för hello virtuell dator (VM)

-   Microsoft SQL Server-databasen-konfigurationsinställningarna (om hello VM identifieras som en server som kör SQL Server)

-   Lagring tillförlitlighet räknare

-   Viktiga snabbkorrigeringar för Windows

-   Installerade filterdrivrutiner

Detta är en passiv insamling av information som inte påverkar hello system. 

>[!Note]
>Det här scenariot ingår automatiskt i varje hello följande scenarier.

### <a name="benchmarkstorage-performance-test"></a>Prestandamått/lagring prestandatester

Det här scenariot körs hello [diskspd](https://github.com/Microsoft/diskspd) mäta test (IOPS och Mbit/s) för alla enheter som är anslutna toohello VM. 

> [!Note]
> Det här scenariot kan påverka hello system och ska inte köras på ett levande produktionssystem. Om det behövs kör det här scenariot i ett dedicerat Underhåll fönstret tooavoid eventuella problem. En ökad belastning som orsakas av ett spårningen eller prestandamått test att sänka hello prestanda på den virtuella datorn.
>

### <a name="general-vm-slow-analysis"></a>Allmänna VM långsam analys 

Det här scenariot körs en [prestandaräknaren](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) spårningen med hjälp av hello räknare som anges i hello Generalcounters.txt filen. Om hello VM identifieras som en server som kör SQL Server, kör en räknare spårning av prestanda med hjälp av hello räknare som finns i hello Sqlcounters.txt filen. Den omfattar också prestanda diagnostikdata.

### <a name="vm-slow-analysis-and-benchmark"></a>Långsam VM analys- och prestandamått

Det här scenariot körs en [prestandaräknaren](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) spårning som följs av en [diskspd](https://github.com/Microsoft/diskspd) benchmark-test. 

> [!Note]
> Det här scenariot kan påverka hello system och ska inte köras på ett levande produktionssystem. Om det behövs kör det här scenariot i ett dedicerat Underhåll fönstret tooavoid eventuella problem. En ökad belastning som orsakas av ett spårningen eller prestandamått test att sänka hello prestanda på den virtuella datorn.
>

### <a name="azure-files-analysis"></a>Azure filer analys 

Det här scenariot körs en särskild prestandaräknaren avbildning tillsammans med en spårning i nätverket. Avbildningen innehåller alla räknare för hello ”SMB-Klientresurser”. hello nedan följer några viktiga SMB-resursen prestandaräknare i klient som ingår i hello avbildning:

| **Typ**     | **SMB-klienten resurser räknare** |
|--------------|-------------------------------|
| IOPS         | Data-begäranden per sekund             |
|              | Läs begäranden per sekund             |
|              | Skriva begäranden per sekund            |
| Svarstid      | Medel s/begäran         |
|              | Medel s/diskläsning                 |
|              | Medel s/diskskrivning                |
| I/o-storlek      | Genomsn. Begäran om byte-Data       |
|              | Genomsn. Lästa byte /               |
|              | Genomsn. Byte/skrivning              |
| Dataflöde   | Byte/sek                |
|              | Lästa byte/sek                |
|              | Skrivna byte/s               |
| Kölängd | Genomsn. Läs Kölängd        |
|              | Genomsn. Skriva Kölängd       |
|              | Genomsn. Kölängd för data        |

### <a name="custom-configuration"></a>Anpassad konfiguration 

När du kör en anpassad konfiguration kör du alla spår (Prestandadiagnostik, prestandaräknare, xperf, nätverk, storport) parallellt, beroende på hur många olika spår har valts. När spårning är klar körs hello verktyget hello diskspd-prestandamått, om den är markerad. 

> [!Note]
> Det här scenariot kan påverka hello system och ska inte köras på ett levande produktionssystem. Om det behövs kör det här scenariot i ett dedicerat Underhåll fönstret tooavoid eventuella problem. En ökad belastning som orsakas av ett spårningen eller prestandamått test att sänka hello prestanda på den virtuella datorn.
>

## <a name="what-kind-of-information-is-collected-by-hello-script"></a>Vilken typ av information som samlas in av hello skript?

Information om Windows VM, diskar eller lagring pooler konfiguration, prestandaräknare, loggar och olika spårningssessioner samlas in beroende på hello prestanda scenario för:

|Data som samlas in                              |  |  | Scenarier för prestanda |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Samla in disk konfiguration | Prestandamått/lagringsprestanda test | Allmänna VM långsam analys | Långsam VM analys- och prestandamått | Azure filer analys | Anpassad konfiguration |
| Information från händelseloggar      | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Systeminformation               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Volymen karta                       | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Disk-karta                         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Aktiviteter som körs                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Lagring tillförlitlighet räknare     | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Storage-informationen              | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Fsutil utdata                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Drivrutinen filterinformationen               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Netstat-utdata                   | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Nätverkskonfiguration            | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Brandväggskonfiguration           | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Konfiguration av SQL Server         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Prestanda diagnostik spårningar * |                            |                                    | Ja                      |                                |                      | Ja                  |
| Prestandaräknaren Trace **     |                            |                                    |                          |                                |                      | Ja                  |
| SMB-räknaren Trace **             |                            |                                    |                          |                                | Ja                  |                      |
| SQL Server-räknaren Trace **      |                            |                                    |                          |                                |                      | Ja                  |
| XPerf spårning                      |                            |                                    |                          |                                |                      | Ja                  |
| StorPort-spårning                   |                            |                                    |                          |                                |                      | Ja                  |
| Spårning i nätverket                    |                            |                                    |                          |                                | Ja                  | Ja                  |
| Diskspd Benchmark trace ***      |                            | Ja                                |                          | Ja                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Prestanda diagnostik trace (*)

Kör en regelbaserad motor i hello toocollect bakgrundsinformation och diagnostisera pågående prestandaproblem. Följande regler hello stöds:

- HighCpuUsage regel: identifierar perioder med hög CPU-användning och visar hello högsta CPU användning konsumenter under dessa punkter.
- HighDiskUsage regel: identifierar ett högt användning punkter på fysiska diskar och visar hello högsta disk användning konsumenter under dessa punkter.
- HighResolutionDiskMetric regel: Visar IOPS, genomflöde och mått för i/o-svarstid per 50 tid i millisekunder för varje fysisk disk. Det hjälper tooquickly identifiera disken begränsning punkter.
- HighMemoryUsage regel: identifierar höga minnesområdet användning punkter och visar hello övre minne användning konsumenter under dessa punkter.

> [!NOTE] 
> För närvarande stöds Windows-versioner som innehåller hello .NET Framework 3.5 eller senare versioner.

### <a name="performance-counter-trace-"></a>Prestandaräknaren trace (*)

Samlar in hello följande prestandaräknare:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty sidor, \Cache\Lazy tömningar/s, \Server\Pool växlingsbart systemminne, fel, \Server\Pool växlingsbart systemminne-fel
- Valda räknare under \Network gränssnitt, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network nätverkskort, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, \Per Network Interface Card processoraktivitet, \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>För SQL Server-instanser
- \SQL server: bufferthanteraren, \SQLServer:Resource Pool statistik, \SQLServer:SQL Statistics\
- \SQLServer:locks \SQLServer:General statistik
- \SQLServer:Access metoder

#### <a name="for-azure-files"></a>För Azure-filer
\SMB Klientresurser

### <a name="diskspd-benchmark-trace-"></a>Diskspd Benchmark-spårning (*)
Diskspd-i/o arbetsbelastning tester [OS-disken (write) och pool-enheter (läsa/skriva)]

## <a name="run-hello-perfinsights-on-your-vm"></a>Kör hello PerfInsights på den virtuella datorn

### <a name="what-do-i-have-tooknow-before-i-run-hello-script"></a>Vad har jag tooknow innan jag kör hello skript? 

**Skriptkrav för**

1.  Det här skriptet måste köras på hello VM som har hello prestandaproblem. 

2.  hello stöds följande operativsystem: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 och Windows 10.

**Möjliga problem när du kör skriptet hello på produktion virtuella datorer:**

1.  hello skript kan hello prestanda försämras av hello VM när den används tillsammans med hello ”Benchmark” eller ”anpassad” scenario som har konfigurerats med hjälp av XPerf eller DiskSpd. Var försiktig när du kör hello skript i en produktionsmiljö.

2.  När du använder skriptet hello tillsammans med hello ”Benchmark” eller ”anpassad” scenario som konfigureras med hjälp av DiskSpd, se till att inga andra bakgrundsaktivitet stör hello i/o-arbetsbelastning på hello testas diskar.

3.  Som standard använder hello skript hello tillfällig lagring enhet toocollect data. Om spårning förblir aktiverad under en längre tid, kanske hello mängden data som samlas in relevanta. Detta kan minska hello tillgänglighet utrymme på hello diskutrymme, därför påverkar alla program som förlitar sig på den här enheten.

### <a name="how-do-i-run-perfinsights"></a>Hur kör PerfInsights? 

toorun Hej skript, gör du följande:

1. Hämta [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Avblockera hello PerfInsights.zip fil. toodo, högerklicka på hello PerfInsights.zip filen genom att välja **egenskaper**. I hello **allmänna** väljer **avblockera** och välj sedan **OK**. Detta säkerställer att hello skriptet körs utan några ytterligare säkerhetsmeddelanden.  

    ![Låsa upp hello zip-filen](media/how-to-use-perfInsights/unlock-file.png)

3.  Expandera hello komprimerade PerfInsights.zip fil i tillfälliga enheten (som standard, vanligen hello D enhet). hello komprimerade filen ska innehålla hello följande filer och mappar:

    ![filer i hello komprimerad mapp](media/how-to-use-perfInsights/file-folder.png)

4.  Öppna Windows PowerShell som administratör och kör sedan hello PerfInsights.ps1 skript.

    ```
    cd <hello path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Du måste kanske tooenter ”y” tooif du tillfrågas om tooconfirm att toochange hello körningsprincipen.

    I dialogrutan för hello friskrivning ges hello alternativet tooshare diagnostikinformation med Microsoft-supporten. Du måste också godkänna toohello license avtal toocontinue. Gör dina val och klicka sedan på **kör skriptet**.

    ![FRISKRIVNING rutan](media/how-to-use-perfInsights/disclaimer.png)

5.  Skicka hello case nummer, om den är tillgänglig när du kör hello-skript (det är vår statistiska). Klicka på **OK**.
    
    ![Ange support-ID](media/how-to-use-perfInsights/enter-support-number.png)

6.  Välj enheten tillfällig lagring. hello skript kan automatisk identifiering hello enhetsbeteckningen för hello enhet. Om problem uppstår i det här skedet kan du kanske uppmanas tooselect hello enhet (hello Standardenheten är D). Genererade loggar lagras här i loggen för hello\_samlingsmapp. När du registrerar eller acceptera hello enhetsbeteckning, klickar du på **OK**.

    ![Ange enhet](media/how-to-use-perfInsights/enter-drive.png)

7.  Välj ett scenario med felsökning hello tillhandahålls lista.

       ![Välj supportscenarier](media/how-to-use-perfInsights/select-scenarios.png)

8.  Du kan också köra PerfInsights utan användargränssnitt.

    hello följande kommandot körs hello ”Allmänt VM långsam analys” felsökning scenariot utan en UI-kommandotolk eller samla in data i 30 sekunder. Ombeds du tooconsent toohello samma friskrivning och LICENSAVTALET som nämns i steg 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Om du vill PerfInsights toorun i tyst läge använder den **- AcceptDisclaimerAndShareDiagnostics** parameter. Till exempel använda hello följande kommando:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-hello-script"></a>Hur felsöker problem när du kör skriptet hello?

Om hello skriptet avslutas onormalt, kan du rensa ett inkonsekvent tillstånd genom att köra skript hello tillsammans med hello - rensning växel, enligt följande:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Om eventuella problem uppstår under hello automatisk identifiering av temporär hello-enhet kan du kanske uppmanas tooselect hello enhet (hello Standardenheten är D).

![Ange enhet](media/how-to-use-perfInsights/enter-drive.png)

hello skript avinstallerar hello verktyg och tar bort temporära mappar.

### <a name="troubleshooting-other-script-issues"></a>Felsökning av problem med andra skript 

Tryck på Ctrl + C toointerrupt hello skriptkörningen om eventuella problem uppstår när du kör skriptet hello. tooremove temporära objekt, i avsnittet hello ”Rensa efter onormal avslutning”.

Om du fortsätter tooexperience skript fel trots flera försök, rekommenderar vi att du kör hello skript i ”felsökningsläge” med hjälp av hello ”-Debug” parametern alternativet vid start.

När hello fel inträffar, kopiera hello full utgående hello PowerShell-konsolen och skicka den toohello Microsoft Support-agent som hjälper dig felsöka toohelp hello problem.

### <a name="how-do-i-run-hello-script-in-custom-configuration-mode"></a>Hur kör hello skript i läget för anpassad konfiguration?

Genom att välja hello **anpassad** konfiguration, kan du aktivera flera spårningar parallellt (Använd SKIFT toomulti – Välj):

![Välj scenarier](media/how-to-use-perfInsights/select-scenario.png)

När du väljer hello Prestandadiagnostik prestandaräknaren Trace XPerf spårning, spårning i nätverket eller Storport Trace scenarier, följer du anvisningarna hello i hello dialogrutor och försök tooreproduce hello problemet med långsamma prestanda när du har startat hello spårningar.

hello följande dialogrutan kan du starta en spårning:

![Starta spårning](media/how-to-use-perfInsights/start-trace-message.png)

toostop hello spårning, har du tooconfirm hello kommandot i en andra dialogruta.

![Stoppa spårning](media/how-to-use-perfInsights/stop-trace-message.png)
![Stoppa spårning](media/how-to-use-perfInsights/ok-trace-message.png)

När Hej spårningar eller åtgärder har slutförts, skapas en ny fil i D:\\loggen\_samling (eller hello temporärkatalog) som heter **CollectedData\_åååå-MM-dd\_hh\_mm \_ss.zip.** Du kan skicka den här filen toohello stöd agenten för analys.

## <a name="review-hello-diagnostics-report-created-by-perfinsights"></a>Granska hello diagnostik rapporten som skapas av PerfInsights

Inom hello **CollectedData\_åååå-MM-dd\_hh\_mm\_ss.zip filen** som genereras av PerfInsights, kan du hitta en HTML-rapport som beskriver hello resultaten av PerfInsights. tooreview hello rapporten, expandera hello **CollectedData\_åååå-MM-dd\_hh\_mm\_ss.zip** fil och sedan öppnar hello **PerfInsights Report.html**fil.

Välj hello **resultaten** fliken.

![fliken Sök](media/how-to-use-perfInsights/findingtab.png)

**Anteckningar**

-   Meddelanden i rött kända konfigurationsproblem som kan orsaka prestandaproblem.

-   Meddelanden i gult är varningar som representerar icke-optimal konfigurationer som inte nödvändigtvis orsakar prestandaproblem.

-   Meddelanden i blått är informativa instruktioner.

Granska hello http-länkar för alla felmeddelanden i rött tooget mer detaljerad information om hello resultat och hur de påverkar hello prestanda eller bästa praxis för prestanda-optimerad konfigurationer.

### <a name="disk-configuration-tab"></a>Fliken för konfiguration av disk

Hej **översikt** avsnitt visar olika vyer av hello lagringskonfiguration, inklusive information från Diskpart och lagringsutrymmen

Hej **DiskMap** och **VolumeMap** avsnitt beskrivs på ett dubbla perspektiv logiska volymer och fysiska diskar är andra relaterade tooeach.

I hello PhysicalDisk perspektiv (DiskMap) visas hello i tabellen alla logiska volymer som körs på hello disk. I följande exempel hello, körs PhysicalDrive2 2 logiska volymer som har skapats på flera partitioner (J och H):

![datafliken](media/how-to-use-perfInsights/disktab.png)

I hello volym perspektiv (*VolumeMap*), hello tabeller visar alla hello fysiska diskar under varje logisk volym. Observera att du kan köra en logisk volym på flera fysiska diskar för RAID/dynamiska diskar. I följande exempel hello *C:\\montera* är en monteringspunkt som konfigurerats som *SpannedDisk* på PhysicalDisks \#2 och \#3:

![fliken volym](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>SQL Server-fliken

Om hello mål VM är värd för SQL Server-instanser, visas ytterligare en flik i hello-rapport som heter **SQL Server**:

![SQL-fliken](media/how-to-use-perfInsights/sqltab.png)

Det här avsnittet innehåller en ”översikt” och ytterligare sub flikar för varje hello SQL Server-instanser som finns på hello VM.

Hej ”Overview” avsnittet innehåller en användbar tabell som sammanfattar alla hello fysiska diskar (system- och datadiskar) som körs och som innehåller en blandning av datafiler och transaktionsloggfiler.

I följande exempel hello *PhysicalDrive0* (som kör hello C-enheten) visas eftersom både hello *modeldev* och *modellog* filerna på hello C-enheten och de är av olika typer (till exempel datafilen och transaktionsloggen, respektive):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

hello SQL Server-instans-specifika flikar innehåller ett allmänt avsnitt som innehåller grundläggande information om valda hello-instansen och ytterligare avsnitt avancerad information, inklusive inställningar, konfigurationer och användaralternativ.

## <a name="references-toohello-external-tools-used"></a>Refererar till externa toohello verktyg som används

### <a name="diskspd"></a>Diskspd

DISKSPD är ett lagring belastningen generator och prestanda test från hello Windows och Windows Server och moln serverinfrastruktur engineering team. Mer information finns i [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf är ett kommandoradsverktyg toocapture spår från hello Windows Performance Tools Kit.

Mer information finns i [Windows Performance Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Nästa steg

### <a name="upload-diagnostics-logs-and-reports-toomicrosoft-support-for-further-review"></a>Överför diagnostik loggar och rapporter tooMicrosoft stöd för ytterligare granskning

När du arbetar med hello Microsoft Support-personal kanske begärda tootransmit hello utdata som genereras av PerfInsights tooassist hello felsökningsprocessen.

hello supportpersonalen du skapar en DTM arbetsyta och du får ett e-postmeddelande som innehåller en länk toohello [DTM portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) och ett unikt användarnamn och lösenord.

Det här meddelandet ska skickas från **CTS Automated diagnostik Services** (ctsadiag@microsoft.com).

![Exempel på hello-meddelande](media/how-to-use-perfInsights/supportemail.png)

För ytterligare säkerhet kan du kommer att nödvändiga toochange lösenord på först använda.

När du loggar in tooDTM hittar du en dialogrutan rutan tooupload hello **CollectedData\_åååå-MM-dd\_hh\_mm\_ss.zip** filen som samlats in av PerfInsights.
