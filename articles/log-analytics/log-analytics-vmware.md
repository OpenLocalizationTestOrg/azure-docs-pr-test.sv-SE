---
title: "VMware övervakning lösning i Log Analytics | Microsoft Docs"
description: "Lär dig mer om hur lösningen VMware övervakning kan hantera loggar och övervaka ESXi-värdar."
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
ms.openlocfilehash: 17072c4b6e4fdf6e4dc2b7a6a4ded7fa9f9f6fde
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>VMware övervakning (förhandsgranskning) lösning i logganalys

![VMware symbol](./media/log-analytics-vmware/vmware-symbol.png)

Övervakning av VMware-lösning i Log Analytics är en lösning som hjälper dig att skapa en centraliserad loggning och övervakning tillvägagångssätt för stora VMware-loggar. Den här artikeln beskriver hur du kan felsöka, samla in och hantera ESXi-värdar i en enda plats med lösningen. Med lösningen kan se du detaljerad information för alla dina ESXi-värdar i en enda plats. Du kan se översta händelsen antal, status och trender för VM- och ESXi-värdar som tillhandahålls via loggarna ESXi-värd. Du kan felsöka genom att visa och söker efter loggar med centraliserad ESXi-värd. Och du kan skapa varningar baserat på loggen sökfrågor.

Lösningen använder inbyggd syslog-funktionerna i ESXi-värd för pusha data till ett mål VM, som har OMS-Agent. Lösningen skriva inte filer till syslog inom den virtuella datorn. OMS-agenten lyssnar till detta öppnar porten 1514. När den tar emot data skickar OMS-agenten data till OMS.

## <a name="installing-and-configuring-the-solution"></a>Installera och konfigurera lösningen
Använd följande information för att installera och konfigurera lösningen.

* Lägg till övervakning av VMware-lösning i OMS-arbetsytan med processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>VMware ESXi-värdar som stöds
vSphere ESXi 5.5 för värden och 6.0

#### <a name="prepare-a-linux-server"></a>Förbered en Linux-server
Skapa ett Linux-operativsystem VM ta emot alla syslog-data från ESXi-värdar. Den [OMS Linux-agenten](log-analytics-linux-agents.md) är den samling för alla ESXi-värd syslog-data. Du kan använda flera ESXi-värdar för att vidarebefordra loggar till en Linux-server, som i följande exempel.  

   ![Syslog-flöde](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Ange syslog-samling
1. Ange syslog-vidarebefordring för VSphere. Detaljerad information för att ställa in syslog-vidarebefordran finns [konfigurerar syslog på ESXi 5.x och 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Gå till **ESXi Värdkonfiguration** > **programvara** > **avancerade inställningar** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. I den *Syslog.global.logHost* fältet, lägga till Linux-server och portnumret *1514*. Till exempel `tcp://hostname:1514` eller`tcp://123.456.789.101:1514`
3. Öppna ESXi-värd-brandväggen för syslog. **ESXi Värdkonfiguration** > **programvara** > **säkerhetsprofil** > **brandväggen** och öppna **egenskaper**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Kontrollera vSphere-konsolen och verifiera att den syslog är korrekt konfigurerad. Bekräfta på ESXI-värd porten **1514** har konfigurerats.
5. Hämta och installera OMS-Agent för Linux i Linux-servern. Mer information finns i [dokumentationen till OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. När OMS-Agent för Linux installeras, gå till katalogen /etc/opt/microsoft/omsagent/sysconf/omsagent.d och kopiera filen vmware_esxi.conf till katalogen /etc/opt/microsoft/omsagent/conf/omsagent.d och ändringen ägare/grupp och behörigheterna för filen. Exempel:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Starta om OMS-Agent för Linux genom att köra `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Testa anslutningen mellan Linux- och ESXi-värd med hjälp av den `nc` på ESXi-värd. Exempel:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. I OMS-portalen, söka loggen efter `Type=VMware_CL`. När OMS samlar in syslog-data, behåller den syslog-format. I portalen vissa specifika fält avbildas, t.ex *värdnamn* och *ProcessName*.  

    ![typ](./media/log-analytics-vmware/type.png)  

    Om sökresultaten Visa loggen liknar bilden ovan, klart att använda instrumentpanelen för övervakning av OMS-VMware-lösning.  

## <a name="vmware-data-collection-details"></a>VMware information för samlingen
Övervakning av VMware-lösningen samlar in olika mått och logga prestandadata från ESXi-värdar som använder OMS-agenter för Linux som du har aktiverat.

I följande tabell visar metoder för insamling av data och annan information om hur data samlas in.

| Plattform | OMS-Agent för Linux | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |var 3: e minut |

I följande tabell visar exempel på datafält som samlas in av övervakning av VMware-lösningen:

| Fältnamn | Beskrivning |
| --- | --- |
| Device_s |VMware-lagringsenheter |
| ESXIFailure_s |fel-typer |
| EventTime_t |tidpunkten då händelsen inträffade |
| HostName_s |ESXi värdnamn |
| Operation_s |Skapa virtuell dator eller ta bort virtuell dator |
| ProcessName_s |händelsenamnet |
| ResourceId_s |namnet på VMware-värden |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI-status |
| SyslogMessage_s |Syslog-data |
| UserName_s |användare som skapas eller tas bort VM |
| VMName_s |VM-namn |
| Dator |värddatorn |
| TimeGenerated |tid som data har genererats |
| DataCenter_s |VMware datacenter |
| StorageLatency_s |lagring svarstid (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Övervakning av VMware lösning: översikt
VMware-ikonen visas i OMS-portalen. Det ger en övergripande bild av eventuella fel. När du klickar på ikonen försättas i en instrumentpanelsvy.

![sida vid sida](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Navigera instrumentpanelsvyn
I den **VMware** instrumentpanelsvyn blad ordnas efter:

* Felberäkning Status
* Högsta värden av händelse räknar
* Antal översta händelse
* Virtual Machine-aktiviteter
* ESXi-värd Disk händelser

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klicka på ett blad öppna Log Analytics sökfönstret som visar detaljerad information som är specifik för bladet.

Härifrån kan redigera du frågan om du vill ändra för ett specifikt. En självstudiekurs om grunderna i OMS-sökning, ta en titt på [OMS loggen Sök kursen.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Sök efter händelser för ESXi-värd
En enda ESXi-värd genererar flera loggar, baserat på deras processer. Övervakning av VMware-lösningen centraliserar dem och sammanfattar händelse antal. Centraliserad vyn hjälper dig att förstå vilka ESXi-värd har en stor volym med händelser och vilka händelser inträffar oftast i din miljö.

![Händelse](./media/log-analytics-vmware/events.png)

Du kan gå vidare genom att klicka på ESXi-värd eller en händelsetyp.

När du klickar på ett ESXi värdnamn, kan du visa information från den ESXi-värden. Om du vill begränsa sökresultaten med händelsetyp, lägger du till `“ProcessName_s=EVENT TYPE”` i sökningen. Du kan välja **ProcessName** i sökrutan filtrera. Som begränsar informationen du.

![öka detaljnivån](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Hitta hög VM-aktiviteter
En virtuell dator kan skapas och tas bort på ESXi-värd. Det är användbart för en administratör att identifiera hur många virtuella datorer skapas en ESXi-värd. Som i sin tur, hjälper till att förstå prestanda och kapacitetsplanering. Det är viktigt att hålla reda på VM-aktivitetshändelser när du hanterar din miljö.

![öka detaljnivån](./media/log-analytics-vmware/vmactivities1.png)

Om du vill se ytterligare ESXi-värd VM skapa data klickar du på ett ESXi värdnamn.

![öka detaljnivån](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Vanliga sökfrågor
Lösningen innehåller andra användbara frågor som kan hjälpa dig att hantera din ESXi-värdar, till exempel hög lagringsutrymme, svarstid för lagring och sökvägsfel.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![frågor](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Spara frågor
Spara sökningar är en funktion som standard i OMS och kan hjälpa dig att hålla alla frågor som du har hittat användbart. När du skapar en fråga som vara användbara sparar du genom att klicka på den **Favoriter**. En sparad fråga kan du enkelt återanvända det senare från den [min instrumentpanel](log-analytics-dashboards.md) sida där du kan skapa egna anpassade instrumentpaneler.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Skapa aviseringar från frågor
När du har skapat dina frågor, kanske du vill använda frågor för att meddela dig när specifika händelser äger rum. Se [aviseringar i logganalys](log-analytics-alerts.md) information om hur du skapar aviseringar. Exempel på varningar frågor och andra frågan-exempel finns i [övervakaren VMware med OMS logganalys](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogginlägg.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Vad behöver jag göra på ESXi-värd inställningen? Vilken effekt det har på min aktuella miljön?
Lösningen använder inbyggd ESXi-värd Syslog-vidarebefordran mekanism. Du behöver inte några ytterligare Microsoft-programvara på ESXi-värd för att samla in loggarna. Det bör ha en låg påverkan på din befintliga miljö. Dock behöver du ange syslog-vidarebefordran, vilket är ESXI funktioner.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Behöver jag startar om ESXi-värd?
Nej. Den här processen kräver inte omstart. Ibland uppdateras vSphere korrekt inte syslog. I så fall, logga in på ESXi-värd och Läs in syslog. Igen och behöver du starta om värddatorn, så den här processen är inte störande för din miljö.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kan jag öka eller minska mängden loggdata som skickas till OMS?
Ja, du kan. Du kan använda ESXi-värd loggningsnivån inställningarna i vSphere. Loggsamlingen är baserad på den *info* nivå. Så om du vill granska virtuell dator skapas eller tas bort, måste du behålla den *info* nivån på Hostd. Mer information finns i [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Varför Hostd inte tillhandahåller data till OMS? Mina inställningar har angetts till informationen.
Det uppstod ett ESXi-värd programfel för syslog-tidsstämpel. Mer information finns i [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). När du har installerat lösningen ska Hostd fungera normalt.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kan jag har flera ESXi-värdar som vidarebefordrar syslog-data till en enda virtuell dator med omsagent?
Ja. Du kan ha flera ESXi-värdar som vidarebefordrar till en enda virtuell dator med omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Varför visas inte data som flödar till OMS?
Det kan finnas flera anledningar:

* ESXi-värd inte korrekt skicka data till den virtuella datorn kör omsagent. Om du vill testa, utför du följande steg:

  1. Bekräfta genom att logga in på ESXi-värd med hjälp av ssh och kör följande kommando:`nc -z ipaddressofVM 1514`

      Om detta inte lyckas, vSphere-inställningarna i avancerade sannolikt inte korrigera. Se [konfigurera syslog samling](#configure-syslog-collection) information om hur du ställer in ESXi-värd för syslog-vidarebefordran.
  2. Om syslog-port anslutningen lyckas, men du fortfarande inte ser några data, ladda du syslog på ESXi-värd med hjälp av ssh att köra följande kommando:` esxcli system syslog reload`
* Den virtuella datorn med OMS-Agent har inte angetts korrekt. Utför följande steg om du vill testa detta:

  1. OMS lyssnar på port 1514 och skickar data till OMS. Kontrollera att den är öppen genom att köra följande kommando:`netstat -a | grep 1514`
  2. Du bör se port `1514/tcp` öppna. Om du inte vill kontrollera att omsagent är korrekt installerad. Syslog-porten är inte öppen på den virtuella datorn om du inte ser portinformationen.

     1. Kontrollera att OMS-Agent körs med hjälp av `ps -ef | grep oms`. Om den inte körs starta processen genom att köra kommandot` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Öppna filen `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf`.

         Kontrollera att rätt användare och grupp-inställning är giltigt, liknar:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Om filen inte finns eller användare och grupp-inställning är fel, vidta åtgärder av [förbereder en Linux-server](#prepare-a-linux-server).

## <a name="next-steps"></a>Nästa steg
* Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys att visa detaljerad VMware värd för data.
* [Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) visar VMware värd för data.
* [Skapa aviseringar](log-analytics-alerts.md) när specifika VMware värden händelser inträffar.
