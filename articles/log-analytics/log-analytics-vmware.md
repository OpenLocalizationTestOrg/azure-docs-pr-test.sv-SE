---
title: "aaaVMware lösning för övervakning i Log Analytics | Microsoft Docs"
description: "Lär dig mer om hur hello VMware övervakning kan hantera loggar och övervaka ESXi-värdar."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: 959d5c2201fc5c7947f8b8870d055cdf9eea8e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>VMware övervakning (förhandsgranskning) lösning i logganalys

![VMware symbol](./media/log-analytics-vmware/vmware-symbol.png)

hello VMware övervakning lösning i Log Analytics är en lösning som hjälper dig att skapa en centraliserad loggning och övervakning tillvägagångssätt för stora VMware-loggar. Den här artikeln beskriver hur du kan felsöka, samla in och hantera hello ESXi-värdar i en enda plats med hello-lösning. Du kan se detaljerad information för alla dina ESXi-värdar i en enda plats med hello-lösning. Du kan se översta händelsen antal, status och trender för VM- och ESXi-värdar som tillhandahålls via hello ESXi-värd loggar. Du kan felsöka genom att visa och söker efter loggar med centraliserad ESXi-värd. Och du kan skapa varningar baserat på loggen sökfrågor.

hello lösningen använder inbyggd syslog-funktionerna i hello ESXi-värd toopush tooa datamålet VM, som har OMS-Agent. Hello lösningen skriva inte filer till syslog i hello målet VM. hello OMS-agenten öppnar port 1514 och lyssnar toothis. När den tar emot hello data skickas hello OMS-agent hello data i OMS.

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning.

* Lägg till hello VMware övervakning lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>VMware ESXi-värdar som stöds
vSphere ESXi 5.5 för värden och 6.0

#### <a name="prepare-a-linux-server"></a>Förbered en Linux-server
Skapa en Linux-operativsystem VM tooreceive alla syslog-data från hello ESXi-värdar. Hej [OMS Linux-agenten](log-analytics-linux-agents.md) är hello samlingen för alla ESXi-värd syslog-data. Du kan använda flera ESXi-värdar tooforward loggar tooa enda Linux-server, som i följande exempel hello.  

   ![Syslog-flöde](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Ange syslog-samling
1. Ange syslog-vidarebefordring för VSphere. Detaljerad information toohelp konfigurera syslog vidarebefordran, se [konfigurerar syslog på ESXi 5.x och 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Gå för**ESXi Värdkonfiguration** > **programvara** > **avancerade inställningar** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. I hello *Syslog.global.logHost* fältet, lägga till Linux-servern och hello portnummer *1514*. Till exempel `tcp://hostname:1514` eller`tcp://123.456.789.101:1514`
3. Öppna hello ESXi värdbrandväggen för syslog. **ESXi Värdkonfiguration** > **programvara** > **säkerhetsprofil** > **brandväggen** och öppna **egenskaper**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Kontrollera hello vSphere konsolen tooverify som syslog har konfigurerats korrekt. Bekräfta på hello ESXI värd porten **1514** har konfigurerats.
5. Hämta och installera hello OMS-Agent för Linux i hello Linux-servern. Mer information finns i hello [dokumentationen till OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. När du har installerat hello OMS-Agent för Linux gå toohello /etc/opt/microsoft/omsagent/sysconf/omsagent.d directory och kopiera hello vmware_esxi.conf toohello /etc/opt/microsoft/omsagent/conf/omsagent.d katalog och hello ändra hello ägare/grupp och behörigheter i hello-fil. Exempel:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Starta om hello OMS-Agent för Linux genom att köra `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Testa hello anslutningen mellan hello Linux-servern och hello ESXi-värd med hjälp av hello `nc` på hello ESXi-värd. Exempel:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection too123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. I hello OMS-portalen, söka loggen efter `Type=VMware_CL`. När OMS samlar in hello syslog-data, behåller hello syslog-format. I hello portal vissa specifika fält avbildas, t.ex *värdnamn* och *ProcessName*.  

    ![typ](./media/log-analytics-vmware/type.png)  

    Om sökresultaten Visa loggen är liknande toohello bilden ovan, klart toouse hello OMS VMware lösning instrumentpanelen för övervakning.  

## <a name="vmware-data-collection-details"></a>VMware information för samlingen
hello VMware övervakning samlar in olika mått och logga prestandadata från ESXi-värdar som använder hello OMS agenter för Linux som du har aktiverat.

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in.

| Plattform | OMS-Agent för Linux | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |var 3: e minut |

hello följande tabell visar exempel på datafält som samlas in av hello VMware övervakning:

| Fältnamn | description |
| --- | --- |
| Device_s |VMware-lagringsenheter |
| ESXIFailure_s |fel-typer |
| EventTime_t |tidpunkten då händelsen inträffade |
| HostName_s |ESXi värdnamn |
| Operation_s |Skapa virtuell dator eller ta bort virtuell dator |
| ProcessName_s |händelsenamnet |
| ResourceId_s |namnet på hello VMware-värden |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI-status |
| SyslogMessage_s |Syslog-data |
| UserName_s |användare som skapas eller tas bort VM |
| VMName_s |VM-namn |
| Dator |värddatorn |
| TimeGenerated |hello tidsdata har genererats |
| DataCenter_s |VMware datacenter |
| StorageLatency_s |lagring svarstid (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Övervakning av VMware lösning: översikt
hello VMware panelen visas i hello OMS-portalen. Det ger en övergripande bild av eventuella fel. När du klickar på panelen hello försättas i en instrumentpanelsvy.

![sida vid sida](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-hello-dashboard-view"></a>Navigera hello instrumentpanelsvy
I hello **VMware** instrumentpanelsvyn blad ordnas efter:

* Felberäkning Status
* Högsta värden av händelse räknar
* Antal översta händelse
* Virtual Machine-aktiviteter
* ESXi-värd Disk händelser

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klicka på någon bladet tooopen logganalys sökfönstret som visar detaljerad information som är specifik för hello-bladet.

Härifrån kan du redigera hello Sök frågan toomodify för ett specifikt. En självstudiekurs om hello grunderna i OMS-sökning, kolla hello [OMS loggen Sök kursen.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Sök efter händelser för ESXi-värd
En enda ESXi-värd genererar flera loggar, baserat på deras processer. hello VMware övervakning centraliserar dem och sammanfattar hello händelse antal. Centraliserad vyn hjälper dig att förstå vilka ESXi-värd har en stor volym med händelser och vilka händelser inträffar oftast i din miljö.

![Händelse](./media/log-analytics-vmware/events.png)

Du kan gå vidare genom att klicka på ESXi-värd eller en händelsetyp.

När du klickar på ett ESXi värdnamn, kan du visa information från den ESXi-värden. Om du vill toonarrow resultat med hello händelsetyp lägga till `“ProcessName_s=EVENT TYPE”` i sökningen. Du kan välja **ProcessName** i hello sökfilter. Som begränsar hello-information.

![öka detaljnivån](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Hitta hög VM-aktiviteter
En virtuell dator kan skapas och tas bort på ESXi-värd. Det är användbart för en administratör tooidentify hur många virtuella datorer skapas en ESXi-värd. Som i sin tur, hjälper toounderstand prestanda och kapacitetsplanering. Det är viktigt att hålla reda på VM-aktivitetshändelser när du hanterar din miljö.

![öka detaljnivån](./media/log-analytics-vmware/vmactivities1.png)

Om du vill toosee ytterligare ESXi-värd skapa VM-data, klickar du på ett ESXi värdnamn.

![öka detaljnivån](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Vanliga sökfrågor
hello lösningen innehåller andra användbara frågor som kan hjälpa dig att hantera din ESXi-värdar, till exempel hög lagringsutrymme, svarstid för lagring och sökvägsfel.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![frågor](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Spara frågor
Spara sökningar är en funktion som standard i OMS och kan hjälpa dig att hålla alla frågor som du har hittat användbart. När du skapar en fråga som vara användbara sparar du genom att klicka på hello **Favoriter**. En sparad fråga kan du enkelt återanvända det senare från hello [min instrumentpanel](log-analytics-dashboards.md) sida där du kan skapa egna anpassade instrumentpaneler.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Skapa aviseringar från frågor
När du har skapat dina frågor kan du kanske vill toouse hello frågor tooalert dig när specifika händelser äger rum. Se [aviseringar i logganalys](log-analytics-alerts.md) information om hur toocreate aviseringar. Exempel på varningar frågor och andra frågan exempel finns hello [övervakaren VMware med OMS logganalys](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogginlägg.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
### <a name="what-do-i-need-toodo-on-hello-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Vad behöver jag toodo på hello ESXi-värden inställningen? Vilken effekt det har på min aktuella miljön?
hello lösningen använder hello interna ESXi-värd Syslog vidarebefordran. Du behöver inte några ytterligare Microsoft-programvara på hello ESXi-värd toocapture hello loggar. Det bör ha en låg påverkan tooyour befintliga miljö. Du behöver dock tooset syslog vidarebefordran, vilket är ESXI funktioner.

### <a name="do-i-need-toorestart-my-esxi-host"></a>Behöver jag toorestart ESXi-värd?
Nej. Den här processen kräver inte omstart. Ibland uppdateras vSphere korrekt inte hello syslog. I så fall, logga in toohello ESXi-värd och Läs in hello syslog. Igen och har du inte toorestart hello värden, så den här processen är inte störande tooyour miljö.

### <a name="can-i-increase-or-decrease-hello-volume-of-log-data-sent-toooms"></a>Kan jag öka eller minska hello volymen av loggdata skickas tooOMS?
Ja, du kan. Du kan använda hello ESXi-värd loggningsnivån inställningar i vSphere. Loggsamlingen är baserad på hello *info* nivå. Så om du vill tooaudit VM skapas eller tas bort, måste tookeep hello *info* nivån på Hostd. Mer information finns i hello [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-toooms-my-log-setting-is-set-tooinfo"></a>Varför Hostd inte tillhandahåller data tooOMS? Min loggen är inställningen tooinfo.
Det uppstod ett ESXi-värd programfel för hello syslog tidsstämpel. Mer information finns i hello [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). När du har tillämpat hello lösning ska Hostd fungera normalt.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-tooa-single-vm-with-omsagent"></a>Kan jag har flera ESXi värdar vidarebefordran syslog data tooa enkel VM med omsagent?
Ja. Du kan ha flera ESXi värdar vidarebefordran tooa enkel VM med omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Varför visas inte data som flödar till OMS?
Det kan finnas flera anledningar:

* hello ESXi-värd inte överföra data toohello VM körs omsagent. tootest, utföra hello följande steg:

  1. tooconfirm, logga in med hjälp av ssh toohello ESXi-värd och köra hello följande kommando:`nc -z ipaddressofVM 1514`

      Om detta inte lyckas, korrekta vSphere i hello avancerad konfiguration är förmodligen inte. Se [konfigurera syslog samling](#configure-syslog-collection) information om hur tooset in hello ESXi värd för syslog-vidarebefordran.
  2. Om syslog-port anslutningen lyckas, men du fortfarande inte ser några data, läs sedan hello syslog på hello ESXi-värd med hjälp av ssh toorun hello följande kommando:` esxcli system syslog reload`
* hello virtuell dator med OMS-Agent har inte angetts korrekt. tootest, utföra hello följande steg:

  1. OMS lyssnar toohello port 1514 och skickar data till OMS. tooverify att den är öppen, kör hello följande kommando:`netstat -a | grep 1514`
  2. Du bör se port `1514/tcp` öppna. Om du inte vill kontrollera att hello omsagent är korrekt installerad. Om du inte ser hello portinformation sedan är hello syslog-port inte öppen på hello VM.

     1. Kontrollera att hello OMS-Agent körs med hjälp av `ps -ef | grep oms`. Om den inte körs starta hello processen genom att köra hello kommando` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Öppna hello `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` fil.

         Kontrollera att rätt hello-användare och grupp-inställning är giltig, liknande:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Om hello-filen inte finns eller hello användare och grupp-inställning är fel, vidta åtgärder av [förbereder en Linux-server](#prepare-a-linux-server).

## <a name="next-steps"></a>Nästa steg
* Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys tooview detaljerad VMware värd för data.
* [Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) visar VMware värd för data.
* [Skapa aviseringar](log-analytics-alerts.md) när specifika VMware värden händelser inträffar.
