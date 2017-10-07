---
title: "aaaUpdate hanteringslösning i OMS | Microsoft Docs"
description: "Den här artikeln är avsedd toohelp du förstår hur toouse den här lösningen toomanage uppdateringar för Windows- och Linux-datorer."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 2dd321913bf049ab1996fd60a2f74b8417084dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-management-solution-in-oms"></a>Uppdateringshanteringslösning i OMS

![Symbolen Hantering av uppdateringar](./media/oms-solution-update-management/update-management-symbol.png)

hello Update hanteringslösning i OMS kan du toomanage operativsystemet säkerhetsuppdateringar för din Windows- och Linux-datorer som distribueras i Azure, lokala miljöer eller andra molntjänstleverantörer av.  Du kan snabbt bedöma hello statusen för uppdateringar på alla agentdatorer som och hantera hello processen för att installera nödvändiga uppdateringar för servrar.


## <a name="solution-overview"></a>Lösningsöversikt
Datorer som hanteras av OMS använder hello följande för att utföra bedömning och uppdatera distributioner:

* OMS-agent för Windows eller Linux
* Önskad PowerShell-tillståndskonfiguration (DSC) för Linux
* Automation Hybrid Runbook Worker
* Microsoft Update eller Windows Server Update Services för Windows-datorer

hello följande diagram visar en översikt över hello beteende och dataflöde med hur hello lösning utvärderar och tillämpar säkerhet uppdateringar tooall anslutna Windows Server- och Linux-datorer i en arbetsyta.    

#### <a name="windows-server"></a>Windows Server
![Processflöde för hantering av Windows Server-uppdateringar](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Processflöde för hantering av Linux-uppdateringar](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

När hello datorn utför en sökning för att kontrollera uppdateringskompatibilitet, vidarebefordrar hello informationen i bulk tooOMS hello OMS-agent. På en Windows-dator utförs hello kompatibilitetsgenomsökningen varje 12 timmar som standard.  Dessutom schema för genomsökning av toohello, hello-sökning för att kontrollera uppdateringskompatibilitet initieras inom 15 minuter om hello Microsoft Monitoring Agent (MMA) är startas om, tidigare tooupdate installation och efter installationen av uppdateringen.  Hej kompatibilitetsgenomsökningen utförs var 3: e timme som standard med en Linux-dator och en kompatibilitetsgenomsökningen startas inom 15 minuter om hello MMA agenten startas.  

hello kompatibilitetsinformation sedan bearbetas och sammanfattas i hello instrumentpaneler som ingår i hello lösning eller sökbara med användardefinierade eller pre-användardefinierade frågor.  hello lösning rapporterar hur uppdaterade hello datorer baserat på vilken datakälla du är konfigurerade toosynchronize med.  Om hello Windows-dator är konfigurerad tooreport tooWSUS, beroende på när WSUS senast synkroniserades med Microsoft Update, hello resultatet kan skilja sig från Microsoft Updates visar.  Hej samma för Linux-datorer som är konfigurerade tooreport tooa lokala lagringsplatsen jämfört med en offentlig lagringsplatsen.   

Du kan distribuera och installera programuppdateringar på datorer som kräver hello uppdateringar genom att skapa en schemalagd distribution.  Uppdateringar som klassificeras som *valfritt* ingår inte i hello distribution omfång för Windows-datorer, endast obligatoriska uppdateringar.  hello schemalagd distribution definierar vilka måldatorerna får hello tillämpliga uppdateringar, antingen genom att explicit ange datorer eller välja en [datorgrupp](../log-analytics/log-analytics-computer-groups.md) som baseras på loggen söker i en viss uppsättning datorer.  Du anger en schema-tooapprove och ange en tidsperiod när uppdateringar är tillåtna toobe som är installerade på.  Uppdateringar installeras av runbooks i Azure Automation.  Du kan inte kan se dessa runbook-flöden och de kräver inte någon konfigurering.  När en Update-distribution skapas, skapas ett schema som startar en master uppdatera runbook på hello angav tid för hello ingår datorer.  Denna master-runbook startar en underordnad runbook på varje agent som utför installationen av de nödvändiga uppdateringarna.       

Vid hello datum och tid som anges i distribution av hello, kör hello måldatorerna hello distribution parallellt.  En sökning är första utförs tooverify hello-uppdateringar fortfarande krävs och installerar dem.  Det är viktigt toonote för WSUS-klientdatorer, om hello-uppdateringarna inte har godkänts i WSUS, distribution av hello misslyckas.  hello resultaten av hello tillämpas uppdateringar vidarebefordras tooOMS toobe bearbetas och sammanfattas i hello instrumentpaneler eller genom att söka efter händelser hello hello.     

## <a name="prerequisites"></a>Krav
* hello-lösningen stöder utför uppdateringen bedömningar mot Windows Server 2008 och senare och programuppdateringsdistributioner mot Windows Server 2008 R2 SP1 och senare.  Installationsalternativ för Server Core och Nano Server stöds inte.

    > [!NOTE]
    > Stöd för att distribuera uppdateringar tooWindows Server 2008 R2 SP1 kräver .NET Framework 4.5 och WMF 5.0 eller senare.
    >  
* Windows-klientoperativsystem stöds inte.  
* Windows-agenter måste antingen vara konfigurerade toocommunicate med en Windows Server Update Services (WSUS)-server eller har åtkomst tooMicrosoft uppdatering.  

    > [!NOTE]
    > Windows hello-agenten kan inte hanteras samtidigt av System Center Configuration Manager.  
    >
* CentOS 6 (x86/x64) och 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) och 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) och 12 (x64)  
* Ubuntu 12.04 LTS och senare x86/x64   
    > [!NOTE]  
    > tooavoid uppdateringar tillämpas utanför en underhållsperiod på Ubuntu, konfigurera obevakad uppgradering paketet toodisable automatiska uppdateringar. Mer information om hur tooconfigure detta, se [automatiska uppdateringar-avsnittet i hello Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* Linux-agenter måste ha åtkomst tooan uppdatera databasen.  

    > [!NOTE]
    > En OMS-Agent stöds för Linux konfigurerade tooreport toomultiple OMS arbetsytor inte med den här lösningen.  
    >

Ytterligare information om hur tooinstall hello OMS-Agent för Linux och hämta senaste versionen av hello finns för[Operations Management Suite-Agent för Linux](https://github.com/microsoft/oms-agent-for-linux).  Information om hur tooinstall hello OMS-Agent för Windows, granska [Operations Management Suite-agenten för Windows](../log-analytics/log-analytics-windows-agents.md).  

### <a name="permissions"></a>Behörigheter
Uppdatera distributioner i ordning toocreate måste du toobe beviljas hello deltagarrollen i både Automation-konto och logganalys-arbetsytan.  

## <a name="solution-components"></a>Lösningskomponenter
Den här lösningen består av hello efter resurser som har lagts till tooyour Automation-konto och direktanslutna agenter eller Operations Manager ansluten hanteringsgrupp.

### <a name="management-packs"></a>Hanteringspaket
Om din hanteringsgrupp för System Center Operations Manager är anslutna tooan OMS-arbetsyta, är hello följande hanteringspaket installerade i Operations Manager.  Dessa hanteringspaket också har installerats på direktanslutna Windows-datorer när du lägger till den här lösningen. Det finns inget tooconfigure eller hantera med de här hanteringspaketen.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Uppdatera distributions-MP

Mer information om hur lösningen hanteringspaketen är uppdaterade finns [ansluta Operations Manager tooLog Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Hybrid Worker-grupper
När du aktiverar den här lösningen, anslutna en Windows-dator direkt tooyour OMS-arbetsytan konfigureras automatiskt som en Hybrid Runbook Worker toosupport hello-runbooks som ingår i den här lösningen.  För varje Windows-dator som hanteras av hello-lösning, visas det under hello Hybrid Runbook Worker grupper bladet för hello Automation-kontot efter hello namngivningskonvention *Hostname FQDN_GUID*.  Du kan inte ha de här grupperna som mål med runbook-flöden i ditt konto. Då misslyckas de. Dessa grupper är endast avsedda toosupport hello hanteringslösning.   

Du kan dock lägga till hello Windows tooa Hybrid Runbook Worker-datorgruppen i Automation-runbooks för toosupport ditt Automation-konto så länge som du använder samma konto för både hello lösningen och Hybrid Runbook Worker gruppmedlemskap hello.  Den här funktionen har lagts till tooversion 7.2.12024.0 av hello Hybrid Runbook Worker.  

## <a name="configuration"></a>Konfiguration
Utföra hello följande steg tooadd hello uppdateringshantering lösning tooyour OMS-arbetsytan och bekräfta agenterna rapporterar. Windows-agenter redan anslutna tooyour arbetsytan läggs till automatiskt utan ytterligare konfiguration.

Du kan distribuera hello-lösning med hjälp av hello följande metoder:

* Från Azure Marketplace i hello Azure-portalen genom att välja hello Automation- och kontrollservern erbjudande eller lösning för hantering av uppdateringar
* Från hello OMS lösningar galleri i OMS-arbetsyta

Om du redan har en Automation-konto och OMS-arbetsyta som är länkad tillsammans i hello samma resursgrupp och region, välja Automation & kontroll ska kontrollera konfigurationen och bara installera hello lösning och konfigurera den i båda tjänsterna.  Att välja hello uppdateringshantering lösningen från Azure Marketplace hello ger samma beteende.  Om du inte har antingen services som distribuerats i din prenumeration gör hello i hello **Skapa ny lösning** bladet och bekräfta tooinstall hello andra markerat rekommenderade lösningar.  Alternativt kan du kan lägga till hello uppdateringshantering lösning tooyour OMS-arbetsytan med hello steg som beskrivs i [lägga till OMS-lösningar](../log-analytics/log-analytics-add-solutions.md) från hello lösningar Gallery.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-toooms"></a>Bekräfta OMS-agenter och Operations Manager management group anslutna tooOMS

tooconfirm direktanslutna OMS-Agent för Linux och Windows kommunicerar med OMS, efter några minuter kan du köra hello efter logg sökning:

* Linux – `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows – `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Du kan granska hello följande tooverify agenten anslutning till OMS på en Windows-dator:

1.  Öppna Microsoft Monitoring Agent på Kontrollpanelen och på hello **Azure logganalys (OMS)** fliken, hello agent visas ett meddelande om: **hello Microsoft Monitoring Agent har lyckats ansluta toohello Microsoft Operations Management Suite-tjänsten**.   
2.  Öppna hello Windows-händelseloggen, navigera för**program och tjänster för Logs\Operations** och Sök efter händelse-ID 3000 och 5002 från käll-tjänst-anslutningen.  Dessa händelser anger hello dator har registrerats med hello OMS-arbetsytan och tar emot konfigurationen.  

Om hello agent inte kan toocommunicate med hello OMS-tjänsten och den är konfigurerad toocommunicate med hello internet genom en brandvägg eller proxyserver, bekräfta hello brandvägg eller proxyserver har konfigurerats korrekt genom att granska [nätverk konfigurationen för Windows-agenten](../log-analytics/log-analytics-windows-agents.md#network) eller [nätverkskonfigurationen för Linux-agenten](../log-analytics/log-analytics-agent-linux.md#network).

> [!NOTE]
> Om Linux-system är konfigurerade toocommunicate med en proxy- eller OMS-Gateway och onboarding är den här lösningen uppdatera hello *proxy.conf* behörigheter toogrant hello omiuser gruppen läsbehörighet på hello-filen genom att Utför hello följande kommandon:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`


Nya Linux-agenter visar statusen **Uppdaterad** när en utvärdering har utförts.  Den här processen kan ta upp too6 timmar.

tooconfirm en Operations Manager management-grupp kommunicerar med OMS, se [Validera Operations Manager Integration with OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Datainsamling
### <a name="supported-agents"></a>Agenter som stöds
hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| Windows-agenter |Ja |hello lösningen samlar in information om uppdateringar från Windows-agenter och initierar installationen av obligatoriska uppdateringar. |
| Linux-agenter |Ja |hello lösningen samlar in information om uppdateringar från Linux-agenter och initierar installationen av obligatoriska uppdateringar för distributioner som stöds. |
| Operations Manager-hanteringsgrupp |Ja |hello lösningen samlar in information om uppdateringar från agenter i en ansluten hanteringsgrupp.<br>En direkt anslutning från hello Operations Manager-agenten tooLog Analytics krävs inte. Data vidarebefordras från hello management group toohello OMS-databasen. |
| Azure Storage-konto |Nej |Azure Storage inkluderar inte information om systemuppdateringar. |

### <a name="collection-frequency"></a>Insamlingsfrekvens
För varje hanterad Windows-dator utförs en genomsökning två gånger per dag. Varje kvart hello Windows API anropas tooquery för hello senaste uppdatering tid toodetermine om status har ändrats, och i så fall en kompatibilitetsgenomsökningen startas.  För varje hanterad Linux-dator utförs en sökning var tredje timme.

Det kan ta allt från 30 minuter upp too6 timmar för hello instrumentpanelen toodisplay uppdaterade data från hanterade datorer.   

## <a name="using-hello-solution"></a>Med hello-lösning
När du lägger till hello uppdateringshantering lösning tooyour OMS-arbetsytan hello **uppdateringshantering** panelen läggs tooyour OMS-instrumentpanelen. Den här panelen visar antal och grafisk representation av hello antalet datorer i din miljö och deras kompatibilitet.<br><br>
![Sammanfattningspanel uppdateringshantering](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Visning av kontroll av uppdateringar
Klicka på hello **uppdateringshantering** panelen tooopen hello **uppdateringshantering** instrumentpanelen.<br><br> ![Instrumentpanel för sammanfattning av uppdateringshantering](./media/oms-solution-update-management/update-management-dashboard.png)<br>

Den här instrumentpanelen innehåller en detaljerad analys av uppdateringsstatus efter typ av operativsystem och uppdateringsklassificering – kritiskt, säkerhet med flera (till exempel definitionsuppdatering). hello resultaten i varje sida vid sida på den här instrumentpanelen avspeglar endast uppdateringar som godkänts för distribution, som baseras utifrån hello datorer synkroniseringskälla.   Hej **uppdateringsdistributioner** när valda, omdirigerar toohello distributioner sida där du kan visa scheman, distributioner som för närvarande körs slutförda distributioner eller schemalägga en ny distribution.  

Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på hello specifika panelen eller toorun en fråga för en viss kategori och fördefinierade villkor, välja en hello lista tillgängliga under hello **vanliga uppdateringsfrågor** kolumn.    

## <a name="installing-updates"></a>Installera uppdateringar
När uppdateringar har bedömts för alla hello Linux och Windows-datorer i din arbetsyta kan du ha begärda uppdateringar har installerats genom att skapa en *Uppdateringsdistribution*.  En uppdateringsdistribution är en schemalagd installation av nödvändiga uppdateringar för en eller flera datorer.  Du anger hello datum och tid för distribution av hello dessutom tooa dator eller grupp av datorer som ska tas med i hello omfånget för en distribution.  toolearn mer om datorgrupper, se [datorgrupper i logganalys](../log-analytics/log-analytics-computer-groups.md).  När du inkluderar datorgrupper i din distribution, utvärderas grupp memnbership bara en gång på hello tid då schemat skapades.  Efterföljande ändringar tooa grupp visas inte.  toowork runt problemet, ta bort hello schemalagd uppdateringsdistributionen och återskapa den.

> [!NOTE]
> Windows-datorer som distribueras från hello Azure Marketplace som standard konfigureras tooreceive automatiska uppdateringar från Windows Update-tjänsten.  Det här beteendet ändras inte när du lägger till den här lösningen eller virtuella Windows-datorer tooyour arbetsytan.  Om du gör inte aktivt hanterade uppdateringar med den här lösningen hello standardbeteendet (automatiskt tillämpa uppdateringar) gäller.  

För virtuella datorer skapas från hello på begäran Red Hat Enterprise Linux (RHEL) bilder som finns i Azure Marketplace, de är registrerade tooaccess hello [Red Hat Update infrastruktur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) distribuerade i Azure.  Alla Linux-distribution måste du uppdatera från hello distributioner online filens centrallager efter deras metoder som stöds.  

### <a name="viewing-update-deployments"></a>Visa uppdateringsdistributioner
Klicka på hello **Uppdateringsdistribution** panelen tooview hello lista över befintliga distributioner.  De grupperas efter status – **Schemalagda**, **Kör** och **Slutförda**.<br><br> ![Uppdatera sidan med scheman över uppdateringsdistributioner](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

hello egenskaperna som visas för varje distribution av uppdatering beskrivs i hello i den följande tabellen.

| Egenskap | Beskrivning |
| --- | --- |
| Namn |Namnet på hello distribution. |
| Schema |Typ av schema.  Det tillgängliga alternativen är *En gång*, *Återkommande veckovis* eller *Återkommer månadsvis*. |
| Starttid |Datum och tid som hello distributionen är schemalagd toostart. |
| Varaktighet |Antal minuter hello distribution tillåts toorun.  Om alla uppdateringar inte installeras inom den här tiden och sedan hello återstående uppdateringar måste vänta tills hello nästa distribution. |
| Servrar |Antal datorer som påverkas av hello distribution.  |
| Status |Aktuell status för hello distribution.<br><br>Möjliga värden:<br>– Ej startad<br>- Kör<br>- Färdig |

Markera en slutförd detalj skärm för distribution av tooview hello som innehåller hello kolumner i hello i den följande tabellen.  Dessa kolumner fylls i om hello distribution inte har ännu startas inte.<br><br> ![Översikt över uppdateringsdistributionens resultat](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Kolumn | Beskrivning |
| --- | --- |
| **Vy över datorer** | |
| Windows-datorer |Visar hello antalet Windows-datorer i hello Uppdateringsdistribution efter status.  Klicka på en status toorun en logg sökning returnerar alla uppdaterar poster med statusen för hello uppdatera distributionen. |
| Linux-datorer |Visar hello antalet Linux-datorer i hello Uppdateringsdistribution efter status.  Klicka på en status toorun en logg sökning returnerar alla uppdaterar poster med statusen för hello uppdatera distributionen. |
| Installationsstatus för datorer |Visar hello-datorer som ingår i hello distribution och hello procentandelen uppdateringar som installerats. Klicka på någon av hello poster toorun en logg sökning returnerar alla saknade och viktiga uppdateringar. |
| **Vy över uppdateringar** | |
| Windows-uppdateringar |Visar en lista över Windows-uppdateringar som ingår i hello uppdatera distributionen och deras installationsstatus per varje uppdatering.  Välj en uppdatering toorun en logg sökning returnerar alla uppdatera posterna för den specifika uppdateringen eller klicka på hello status toorun en logg sökning returnerar alla uppdatera poster för hello-distribution. |
| Linux-uppdateringar |Visar en lista över Linux-uppdateringar som ingår i hello uppdatera distributionen och deras installationsstatus per varje uppdatering.  Välj en uppdatering toorun en logg sökning returnerar alla uppdatera posterna för den specifika uppdateringen eller klicka på hello status toorun en logg sökning returnerar alla uppdatera poster för hello-distribution. |

### <a name="creating-an-update-deployment"></a>Skapa en uppdateringsdistribution
Skapa en ny Uppdateringsdistribution genom att klicka på hello **Lägg till** knappen hello överst i hello skärmen tooopen hello **distribution av nya** sidan.  Du måste ange värden för hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
| --- | --- |
| Namn |Unikt namn tooidentify hello distribution. |
| Tidszon |Tidszon toouse för hello starttid. |
| Schematyp | Typ av schema.  Det tillgängliga alternativen är *En gång*, *Återkommande veckovis* eller *Återkommer månadsvis*.  
| Starttid |Datum och tid toostart hello distribution. **Obs:** hello enligt tidigaste utgång en distribution kan köra är 30 minuter från aktuell tid om du behöver toodeploy omedelbart. |
| Varaktighet |Antal minuter hello distribution tillåts toorun.  Om alla uppdateringar inte installeras inom den här tiden och sedan hello återstående uppdateringar måste vänta tills hello nästa distribution. |
| Datorer |Namn på datorer eller datorn grupper tooinclude och mål i hello distribution.  Välj en eller flera poster i hello nedrullningsbara listan. |

<br><br> ![Ny sida för uppdateringsdistribution](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Tidsintervall
Som standard är hello omfattning hello data som analyseras i hello lösning för hantering av uppdateringar från alla anslutna hanteringsgrupper som genereras inom hello senaste 1 dagen.

toochange hello tidsintervall hello data, Välj **Data baserat på** hello överst i hello instrumentpanelen. Du kan välja poster skapas eller uppdateras i hello senaste 7 dagarna, 1 dag eller 6 timmar. Du kan även välja **Anpassat** och ange ett eget datumintervall.

## <a name="log-analytics-records"></a>Log Analytics-poster
hello uppdateringshantering lösning skapar två typer av poster i hello OMS-databasen.

### <a name="update-records"></a>Uppdateringsposter
En post med typen av **uppdatering** skapas för varje uppdatering som antingen installerats eller behövs på varje dator. Uppdateringsposter har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
| --- | --- |
| Typ |*Uppdatering* |
| SourceSystem |hello-källa som godkänts för installation av hello uppdateringen.<br>Möjliga värden:<br>- Microsoft Update<br>– Windows Update<br>– SCCM<br>- Linux-servrar (hämtade från pakethanterare) |
| Godkända |Anger om hello uppdatering har godkänts för installation.<br> För Linux-servrar är detta för närvarande valfritt eftersom korrigeringar inte hanteras av OMS. |
| Klassificering för Windows |Klassificering av hello uppdateringen.<br>Möjliga värden:<br>-    Program<br>- Kritisk uppdatering<br>- Definitionsuppdatering<br>- Funktionspaket<br>- Säkerhetsuppdatering<br>- Service pack<br>- Samlad uppdatering<br>- Uppdatering |
| Klassificering för Linux |Cassification hello uppdatering.<br>Möjliga värden:<br>- Kritisk uppdatering<br>- Säkerhetsuppdatering<br>- Övriga uppdateringar |
| Dator |Namnet på hello-dator. |
| InstallTimeAvailable |Anger om hello installationstiden är tillgänglig från andra agenter som installerats hello samma uppdatering. |
| InstallTimePredictionSeconds |Beräknad installationstid i sekunder som baseras på andra agenter som installerats hello samma uppdatering. |
| KBID |ID för hello KB-artikel som beskriver hello uppdatering. |
| ManagementGroupName |Namnet på hanteringsgruppen för hello för SCOM-agenter.  För övriga agenter är detta AOI-<workspace ID>. |
| MSRCBulletinID |ID för hello Microsofts säkerhetsbulletin som beskriver hello uppdateringen. |
| MSRCSeverity |Allvarlighetsgraden hello Microsofts säkerhetsbulletin.<br>Möjliga värden:<br>- Kritiskt<br>- Viktig<br>- Mellan |
| Valfri |Anger om hello uppdatering är valfria. |
| Produkt |Namnet på hello produktuppdatering hello används för.  Klicka på **visa** tooopen hello artikel i en webbläsare. |
| PackageSeverity |hello allvarlighetsgraden hello säkerhetsrisker som åtgärdas i den här uppdateringen som rapporteras av hello Linux distro leverantörer. |
| PublishDate |Datum och tid då hello uppdateringen har installerats. |
| RebootBehavior |Anger om hello update tvingar en omstart.<br>Möjliga värden:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Revisionsnummer hello uppdatering. |
| SourceComputerId |GUID toouniquely identifiera hello-dator. |
| TimeGenerated |Datum och tid då hello post uppdaterades senast. |
| Rubrik |Rubrik på hello uppdateringen. |
| UpdateID |GUID toouniquely identifiera hello uppdateringen. |
| UpdateState |Anger om hello uppdateringen är installerad på den här datorn.<br>Möjliga värden:<br>-Installeras - är hello uppdateringen installerad på den här datorn.<br>-Behov – hello uppdateringen installeras inte och krävs på den här datorn. |

När du utför alla loggen sökningar som returnerar poster med en typ av **uppdatering** kan du välja hello **uppdateringar** vy som visar en uppsättning paneler sammanfattning hello-uppdateringar returneras av hello sökningen. Du kan klicka på hello poster i hello **saknas och tillämpa uppdateringar** och **obligatoriska och valfria uppdateringar** rutor tooscope hello visa toothat uppsättning uppdateringar. Välj hello **lista** eller **tabell** visa tooreturn hello enskilda poster.<br>

![En loggsöknings uppdateringsvy med poster över uppdateringstyp](./media/oms-solution-update-management/update-la-view-updates.png)  

I hello **tabell** vy, kan du klicka på hello **KBID** för alla poster tooopen en webbläsare med hello KB-artikel. Detta gör att du tooquickly Läs mer om hello information om hello viss uppdatering.<br>

![En loggsöknings tabellvy med paneler med poster över uppdateringstyp](./media/oms-solution-update-management/update-la-view-table.png)

I hello **lista** vyn du klickar på hello **visa** länken nästa toohello KBID tooopen hello KB-artikel.<br>

![En loggsöknings listvy med paneler med poster över uppdateringstyp](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary-poster
En post med en typ av **UpdateSummary** skapas för varje Windows-agentdator. Den här posten uppdateras varje gång hello datorn genomsöks efter uppdateringar. **UpdateSummary** poster har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
| --- | --- |
| Typ |UpdateSummary |
| SourceSystem |OpsManager |
| Dator |Namnet på hello-dator. |
| CriticalUpdatesMissing |Antal viktiga uppdateringar som saknas på hello-datorn. |
| ManagementGroupName |Namnet på hanteringsgruppen för hello för SCOM-agenter. För övriga agenter är detta AOI-<workspace ID>. |
| NETRuntimeVersion |Version av hello .NET som är installerad på datorn hello. |
| OldestMissingSecurityUpdateBucket |Bucket toocategorize hello tid sedan publicerades hello äldsta saknas säkerhetsuppdatering på den här datorn.<br>Möjliga värden:<br>- Äldre<br>-    180 dagar sedan<br>- 150 dagar sedan<br>-    120 dagar sedan<br>- 90 dagar sedan<br>- 60 dagar sedan<br>-    30 dagar sedan<br>-    Nyligen |
| OldestMissingSecurityUpdateInDays |Antal dagar sedan publicerades hello äldsta saknas säkerhetsuppdatering på den här datorn. |
| OsVersion |Version av hello operativsystemet installeras på hello-dator. |
| OtherUpdatesMissing |Antal andra uppdateringar som saknas på hello-datorn. |
| SecurityUpdatesMissing |Antal säkerhetsuppdateringar som saknas på hello-datorn. |
| SourceComputerId |GUID toouniquely identifiera hello-dator. |
| TimeGenerated |Datum och tid då hello post uppdaterades senast. |
| TotalUpdatesMissing |Totalt antal uppdateringar som saknas på hello-datorn. |
| WindowsUpdateAgentVersion |Versionsnummer för hello Windows Update-agenten på hello-dator. |
| WindowsUpdateSetting |Inställningen för hur hello datorn installerar viktiga uppdateringar.<br>Möjliga värden:<br>- Inaktiverad<br>- Meddela innan installation<br>- Schemalagd installation |
| WSUSServer |URL för WSUS-servern om hello dator är konfigurerad toouse en. |

## <a name="sample-log-searches"></a>Exempel på loggsökningar
hello följande tabell innehåller exempel loggen söker efter uppdateringen innehåller information som samlas in av den här lösningen.

| Fråga | Beskrivning |
| --- | --- |
| Skriv:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false &#124; measure count() by Computer |Datorer Windows-baserade servrar som behöver uppdateras |
| Skriv:Update OSType=Linux UpdateState!="Not needed" &#124; measure count() by Computer |Linux-servrar som behöver uppdateringar | 
| Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Alla datorer med saknade uppdateringar |
| Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |Saknade uppdateringar fören specifik dator (ersätt värdet med namnet på din egen dator)|
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |Alla datorer som saknar kritiska uppdateringar eller säkerhetsuppdateringar | 
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |Kritiska uppdateringar eller säkerhetsuppdateringar som krävs för datorer där uppdateringarna görs manuellt |
| Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |Felhändelser för datorer som saknar kritiska uppdateringar eller säkerhetsuppdateringar |
| Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Alla datorer med saknade samlade uppdateringar | 
| Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |Separata, saknade uppdateringar bland samtliga datorer | 
| Skriv:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Title, UpdateRunName |Dator med Windows-baserad server med uppdateringar som misslyckades i en uppdateringskörning | 
| Skriv:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Product, UpdateRunName |Linux-server med misslyckade uppdateringar vid en uppdateringskörning | 
| Type=UpdateSummary &#124; measure count() by WSUSServer |WSUS datormedlemskap | 
| Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |Konfigurering av automatiska uppdateringar | 
| Type=UpdateSummary WindowsUpdateSetting=Manual |Datorer med automatiska uppdateringar inaktiverat | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |Lista över alla hello Linux-datorer som har en tillgänglig Paketuppdatering | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |Lista över alla hello Linux-datorer som har en tillgänglig Paketuppdatering som åtgärdar problem kritiskt eller säkerhet | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" |Lista över alla paket som har en tillgänglig uppdatering | 
| Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |Lista över alla paket som har en tillgänglig uppdatering vilken åtgärdar kritiska problem eller säkerhetsproblem | 
| Skriv:UpdateRunProgress &#124; measure Count() by UpdateRunName |Lista vilka uppdateringsdistributioner som har ändrade datorer | 
| Skriv:UpdateRunProgress UpdateRunName="DeploymentName" &#124; measure Count() by Computer |Datorer som har uppdaterats i den här uppdateringskörningen (ersätt värdet med uppdateringsdistributionens namn | 
| Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |Lista över alla hello ”Ubuntu” maskiner med alla tillgängliga uppdateringar | 

## <a name="troubleshooting"></a>Felsökning

Det här avsnittet innehåller information som toohelp felsökning av problem med hello lösning för uppdatering.  

### <a name="how-do-i-troubleshoot-onboarding-issues"></a>Hur felsöker jag problem med integrationsprocessen?
Om du får problem när tooonboard hello lösning eller en virtuell dator, kontrollera hello **program och tjänster för Logs\Operations** händelseloggen för händelser med händelse-ID 4502 och händelse meddelande som innehåller **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.  hello följande tabell visar felmeddelanden och en möjlig lösning för varje.  

| Meddelande | Orsak | Lösning |   
|----------|----------|----------|  
| Det går inte tooRegister dator för uppdateringshantering,<br>Registreringen misslyckades med undantaget<br>System.InvalidOperationException: {"Meddelande":"Datorn har redan<br>registrerad tooa annat konto. "} | Datorn är redan publicerats så tooanother arbetsytan för uppdateringshantering | Utföra rensning av gamla artefakter av [hello hybrid runbook gruppen tas bort](../automation/automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Det går inte att registrera för datorn för hantering av korrigering<br>Registreringen misslyckades med undantaget<br>System.Net.Http.HttpRequestException: Ett fel uppstod när hello begäran skickades. ---><br>System.Net.WebException: hello underliggande anslutningen<br>stängdes: Ett oväntat fel<br>uppstod vid mottagning. ---> System.ComponentModel.Win32Exception:<br>hello-klienten och servern inte kan kommunicera<br>eftersom de inte har en gemensam algoritm. | Proxy/Gateway/Firewall blockerar kommunikationen. | [Granska nätverkskraven](../automation/automation-offering-get-started.md#network-planning)|  
| Det går inte tooRegister dator för uppdateringshantering,<br>Registreringen misslyckades med undantaget<br>Newtonsoft.Json.JsonReaderException: Det gick inte att parsa positivt oändligt värde. | Proxy/Gateway/Firewall blockerar kommunikationen. | [Granska nätverkskraven](../automation/automation-offering-get-started.md#network-planning)| 
| hello-certifikatet som presenterades av tjänsten hello <wsid>. oms.opinsights.azure.com<br>utfärdades inte av en certifikatutfärdare<br>som används för Microsoft-tjänster. Kontakta<br>ditt nätverk administratören toosee om de kör en proxy som hindrar<br>TLS/SSL-kommunikation. |Proxy/Gateway/Firewall blockerar kommunikationen. | [Granska nätverkskraven](../automation/automation-offering-get-started.md#network-planning)|  
| Det går inte tooRegister dator för uppdateringshantering,<br>Registreringen misslyckades med undantaget<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Det gick inte toocreate ett självsignerat certifikat. ---><br>System.UnauthorizedAccessException: Åtkomst nekad. | Fel vid genereringen av ett självsignerat certifikat. | Kontrollera att systemkontot har<br>läsbehörighet toofolder:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Hur felsöker jag distributioner av uppdateringar?
Du kan visa hello resultaten av hello runbook ansvariga för att distribuera hello uppdateringar som ingår i hello schemalagd distribution från hello jobb-bladet för ditt Automation-konto som är kopplad till hello OMS-arbetsyta som stöder den här lösningen.  Hej runbook **korrigering MicrosoftOMSComputer** är en underordnad runbook som riktar sig till en viss hanterad dator och granska hello utförliga strömmen visas detaljerad information om distributionen.  hello utdata ska visas som krävs för uppdateringar som gäller kan hämta status, installationsstatus och ytterligare information.<br><br> ![Jobbstatus för uppdateringsdistribution](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Mer information finns i [Automation runbook output and messages](../automation/automation-runbook-output-and-messages.md) (Utdata och meddelanden för Automation-runbook).   

## <a name="next-steps"></a>Nästa steg
* Använd loggen söker i [logganalys](../log-analytics/log-analytics-log-searches.md) tooview detaljerad data för uppdateringen.
* [Skapa egna instrumentpaneler](../log-analytics/log-analytics-dashboards.md) som visar uppdateringskompatibilitet för dina hanterade datorer.
* [Skapa aviseringar](../log-analytics/log-analytics-alerts.md) som visas när viktiga uppdateringar saknas på datorer eller när automatiska uppdateringar är inaktiverade för en dator.  
