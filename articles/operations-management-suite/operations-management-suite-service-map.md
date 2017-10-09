---
title: "aaaUse hello Tjänstkarta lösning i Operations Management Suite | Microsoft Docs"
description: "Tjänstkarta är en Operations Management Suite som automatiskt identifierar programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster. Den här artikeln innehåller information för att distribuera Tjänstkarta i din miljö och använda den i en mängd olika scenarier."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: f7c209182c9171cc520192ac13ca4d85174081b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-service-map-solution-in-operations-management-suite"></a>Använd hello Tjänstkarta lösning i Operations Management Suite
Tjänstkarta identifierar automatiskt programkomponenter för Windows och Linux-datorer och maps hello kommunikation mellan tjänster. Med Tjänstkartan, du kan visa dina servrar i hello sätt som du betrakta dem: som sammanlänkade system som levererar kritiska tjänster. Tjänstkarta visar anslutningar mellan servrar, processer och portar i alla TCP-anslutna arkitektur med annat än krävs ingen konfiguration hello installation av en agent.

Den här artikeln beskriver hello information om med hjälp av Tjänstkarta. Information om hur du konfigurerar Tjänstkarta och onboarding agenter finns [konfigurera Tjänstkarta lösning i Operations Management Suite](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Användningsfall: Se IT bearbetar beroende medveten

### <a name="discovery"></a>Identifiering
Tjänstkarta skapar automatiskt en gemensam referens karta över beroenden mellan dina servrar, processer och tjänster från tredje part. Den identifieras och mappas alla TCP-beroenden, identifiera oväntat anslutningar, fjärrsystem du beroende från tredje part och beroenden tootraditional mörka områden i nätverket, till exempel Active Directory. Tjänstkarta identifierar misslyckade Nätverksanslutningar de hanterade systemen försöker toomake, vilket hjälper dig att identifiera potentiella server felkonfiguration och avbrott nätverksproblem.

### <a name="incident-management"></a>Hantering av incidenter
Tjänstkarta hjälper till att eliminera hello gissningar av problemet isolering genom att visa hur datorerna är anslutna och påverkar varandra. Dessutom tooidentifying misslyckades anslutningar, det hjälper till att identifiera felkonfigurerad belastningsutjämnare, konstigt eller för stora belastningen på kritiska tjänster och obehöriga klienter som utvecklare datorer kommunicerar tooproduction system. Genom att använda integrerad arbetsflöden med Operations Management Suite ändra spårning kan se du också om en Ändringshändelse för en backend-dator eller tjänst förklarar hello grundorsaken till en incident.

### <a name="migration-assurance"></a>Säkerhet för migrering
Med hjälp av en Tjänstkarta kan du effektivt planera, påskynda och validera Azure migreringar som ser till att inget kvar och oväntat avbrott, sker inte. Du kan identifiera alla beroende av varandra system som behöver toomigrate tillsammans, utvärdera systemkonfiguration och kapacitet och identifiera om ett system som körs fortfarande används av användare eller är en kandidat för avställning av i stället för migrering. När hello flyttningen är klar kan du kan kontrollera på klienten belastning och identitet tooverify som testsystem och kunder försöker ansluta. Om dina subnätsdefinitioner för planering och brandväggen har problem, peka misslyckade anslutningar i Tjänstkartan maps toohello system som behöver ansluta.

### <a name="business-continuity"></a>Verksamhetskontinuitet
Om du använder Azure Site Recovery och behöver hjälp definierar hello recovery sekvensen för din miljö för programmet, Tjänstkarta automatiskt kan visa hur system är beroende av varandra tooensure att din återställningsplan är tillförlitlig. Genom att välja en kritisk server eller en grupp och visa dess klienter, kan du identifiera vilka frontend-system toorecover när hello server har återställts och är tillgängliga. Å andra sidan genom att titta på kritiska servrar backend-beroenden, kan du identifiera vilka system toorecover innan dina fokus system har återställts.

### <a name="patch-management"></a>Uppdateringshantering
Tjänstkarta förbättrar din användning av hello utvärdering av Operations Management Suite System uppdateringar genom att visa andra grupper och servrar beroende på din tjänst så att meddela dem i förväg innan du anteckna dina system för korrigering. Tjänstkarta förbättrar också uppdateringshantering i Operations Management Suite genom att visa om dina tjänster är tillgänglig och korrekt ansluten efter de korrigeras och startas om.


## <a name="mapping-overview"></a>Översikt över mappning
Tjänstkarta agenter samla in information om alla TCP-anslutna processer på hello servern där de är installerade och information om hello inkommande och utgående anslutningar för varje process. I vänster hello hello-listan, kan du välja datorer eller grupper som har beroenden i Tjänstkartan agenter toovisualize under ett angivet tidsintervall. Datorn beroende mappar fokuserar på en specifik dator, och de visar alla hello-datorer som är direkt TCP-klienter eller servrar för denna dator.  Datorgruppen maps Visa uppsättningar av servrar och deras beroenden.

![Tjänsten: översikt](media/oms-service-map/service-map-overview.png)

Datorer kan expanderas i hello kartan tooshow hello processer som körs med aktiva nätverksanslutningar under hello valt tidsintervall. När en fjärrdator med en Tjänstkarta agent är expanderat tooshow information, visas de processer som kommunicerar med hello fokus datorn. hello antal utan Agent frontend datorer som ansluter till hello fokus dator anges hello vänster på hello processer som de ansluter till. Om hello fokus datorn gör en anslutning tooa backend-dator som har ingen agent kan hello backend-server ingår i en servergrupp Port, tillsammans med andra anslutningar toohello samma portnummer.

Som standard mappar Tjänstkarta visa hello senaste 30 minuterna av beroendeinformation. Du kan fråga för historiska tidsintervall för in tooone timme tooshow hur beroenden slås i hello med hjälp av hello kontroller på hello övre vänstra tidigare (till exempel vid en incident eller innan en ändring inträffat). Tjänstkarta data lagras i 30 dagar i betald arbetsytor och 7 dagar i kostnadsfria arbetsytor.

## <a name="status-badges-and-border-coloring"></a>Status märken och kantlinje färgläggning
Vid hello vara längst ned på varje server i hello kartan en lista över status Aktivitetsikoner förmedla statusinformation om hello-server. hello Aktivitetsikoner betyda att det finns vissa relevant information om hello server från en hello Operations Management Suite-lösningen integreringar. Klicka på en Aktivitetsikon går du direkt toohello information om hello status i hello till höger. Hej inkluderar för närvarande tillgängliga status Aktivitetsikoner aviseringar, helpdesk, ändringar, säkerhet och uppdateringar.

Beroende på hello allvarlighetsgraden hello status Aktivitetsikoner datorn nod kantlinjer vara färgade röd (kritisk), gult (varning) eller blå (information). hello färg representerar hello de svåraste status för hello status Aktivitetsikoner. En grå kantlinje anger en nod som har inga indikatorer.

![Status Aktivitetsikoner](media/oms-service-map/status-badges.png)

## <a name="machine-groups"></a>Datorgrupper
Datorgrupper kan du toosee maps uppbyggd kring en uppsättning servrar, inte bara en så att du kan se alla hello medlemmar i ett flera nivåer program eller server-kluster i en karta.

Användare välja vilka servrar som tillhör en grupp och välj ett namn för hello-gruppen.  Du kan sedan välja tooview hello grupp med alla processer och anslutningar eller visa med bara hello processer och anslutningar som är direkt relaterade toohello andra medlemmar i hello-gruppen.

![Datorgruppen](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Skapa en grupp datorer
toocreate en grupp, Välj hello dator eller datorer du vill hello datorer i listan och klicka på **lägga till toogroup**.

![Skapa grupp](media/oms-service-map/machine-groups-create.png)

Där kan du välja **Skapa nytt** och namnge hello grupp.

![Gruppnamnet](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Datorgrupper som är för närvarande begränsad too10 servrar, men vi planerar tooincrease begränsningen snart.

### <a name="viewing-a-group"></a>Visa en grupp
När du har skapat några grupper kan visa du dem genom att välja fliken för hello-grupper.

![Grupper](media/oms-service-map/machine-groups-tab.png)

Välj sedan hello Group name tooview hello-mappning för datorn gruppen.
![Datorgruppen](media/oms-service-map/machine-group.png) hello-datorer som tillhör gruppen toohello beskrivs i vitt i hello kartan.

Expanderande hello gruppen visar en lista över hello-datorer som utgör hello datorgruppen.

![Datorer i datorgruppen](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Filtrera efter processer
Du kan växla hello kartvyn mellan visar alla processer och anslutningar i hello grupp och endast hello sådana som är direkt relaterade toohello datorgruppen.  Hej standardvyn är tooshow alla processer.  Du kan ändra hello vyn genom att klicka på hello filterikonen ovan hello kartan.

![Filtret grupp](media/oms-service-map/machine-groups-filter.png)

När **alla processer** är markerad hello kartan innehåller alla processer och anslutningar på varje dator hello i hello grupp.

![Bearbetar alla datorgruppen](media/oms-service-map/machine-groups-all.png)

Om du ändrar hello visa tooshow **grupp-anslutna processer**, hello kartan ska begränsas ned tooonly dessa processer och anslutningar som är direkt anslutna tooother datorer i hello-grupp, skapar en förenklad vy.

![Datorgruppen filtrerade processer](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-tooa-group"></a>Att lägga till datorer tooa grupp
tooadd datorer tooan befintlig grupp markerar hello rutorna nästa toohello datorer och klickar sedan på **lägga till toogroup**.  Välj hello-grupp som du vill tooadd hello datorer.
 
### <a name="removing-machines-from-a-group"></a>Om du tar bort datorer från en grupp
Expandera hello grupp namnet toolist hello datorer i hello datorgruppen i hello grupplistan.  Klicka på hello ellips menyn nästa toohello datorn tooremove och välj **ta bort**.

![Ta bort datorn från grupp](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Ta bort eller byta namn på en grupp
Klicka på hello ellips menyn nästa toohello gruppnamn i hello grupplistan.

![Menyn för dator-grupp](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rollikoner
Vissa processer hantera specifika roller på datorer: servrar, programservrar, databas och så vidare. Tjänstkarta annoterar processen och datorn rutor med rollen ikoner toohelp identifiera vid en snabb överblick över hello rollen en process eller spelas upp.

| Rollikon | Beskrivning |
|:--|:--|
| ![Webbserver](media/oms-service-map/role-web-server.png) | Webbserver |
| ![App-servern](media/oms-service-map/role-application-server.png) | Programserver |
| ![Databasserver](media/oms-service-map/role-database.png) | Databasserver |
| ![LDAP-servern](media/oms-service-map/role-ldap.png) | LDAP-servern |
| ![SMB-servern](media/oms-service-map/role-smb.png) | SMB-servern |

![Rollikoner](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Misslyckade anslutningar
Misslyckade anslutningar som visas i Tjänstkartan mappar för processer och datorer, med en röd streckad linje som anger att en klientdator inte fungerar tooreach en process eller port. Misslyckade anslutningar rapporteras från alla system med en distribuerad Tjänstkarta agent om systemet är hello en försök hello misslyckades-anslutning. Tjänstkarta mäter den här processen genom att följa TCP sockets som inte tooestablish en anslutning. Det här felet kan bero på att en brandvägg, en felaktig konfiguration i hello klient eller server, eller en fjärransluten tjänst som inte var tillgänglig.

![Misslyckade anslutningar](media/oms-service-map/failed-connections.png)

Förstå misslyckade anslutningar kan hjälpa dig med felsökning, verifiering av migreringen, säkerhetsanalys och övergripande arkitektur förstå. Misslyckade anslutningar är det ibland ofarligt, men de pekar ofta direkt tooa problem, till exempel en växling vid fel miljö plötsligt blir kan inte nås eller två program nivåer som tootalk efter en molnmigrering.

## <a name="client-groups"></a>Klientgrupper
Klientgrupper är rutorna på hello karta som representerar klientdatorer som inte har beroende agenter. En enda klientgrupp representerar hello klienter för en enskild process eller datorn.

![Klientgrupper](media/oms-service-map/client-groups.png)

toosee hello IP-adresser hello servrar i en klientgrupp, Välj hello grupp. hello innehållet i hello grupp visas i hello **klienten gruppegenskaper** fönstret.

![Egenskaper för klient](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Serverport grupper
Serverport grupper är rutor som representerar server-portarna på servrar som inte har beroende agenter. hello innehåller hello serverport och antalet hello antal servrar med anslutningar toothat port. Expandera hello rutan toosee hello enskilda servrar och anslutningar. Om det finns endast en server i hello ruta, anges hello namn eller IP-adress.

![Serverport grupper](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Snabbmeny
Klicka på hello ellips (...) överst hello höger om alla servrar som visar hello snabbmenyn för servern.

![Misslyckade anslutningar](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Läs in server-karta
Klicka på **belastningen Server kartan** tar tooa nya karta med hello valda servern som hello nya fokus dator.

### <a name="show-self-links"></a>Visa självlänkar
Klicka på **visa Self-Links** omritningar hello servernoden, inklusive själva länkar, som är TCP-anslutningar som börjar och slutar på rutiner inom hello-server. Om självlänkar är visas hello menykommandona för**dölja Self-Links**, så att du kan inaktivera dem.

## <a name="computer-summary"></a>Datorn sammanfattning
Hej **datorn sammanfattning** rutan innehåller en översikt över en server-operativsystem, beroende antal och data från andra Operations Management Suite-lösningar. Dessa data inkluderar prestandamått, supportavdelningen tjänstbiljetter, för spårning av ändringar, säkerhet och uppdateringar.

![Datorn sammanfattningsfönstret](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Dator- och egenskaper
Du kan välja datorer och processer ytterligare toogain-kontexten om deras egenskaper när du navigerar en Tjänstkarta karta. Datorer som innehåller information om DNS-namn, IPv4-adresser, CPU och minne kapacitet, VM-typ, operativsystem och version, senast omstart tid och hello-ID för sina Operations Management Suite och Tjänstkarta agenter.

![Egenskapsrutan för datorn](media/oms-service-map/machine-properties.png)

Du kan samla in information från operativsystemet metadata om processer, inklusive processnamn, beskrivningen, användarnamn och domän (på Windows), företagets namn, produktnamn, produktversionen, arbetskatalogen, kommandoraden och process som körs Starttid.

![Egenskapsrutan för processen](media/oms-service-map/process-properties.png)

Hej **Processammanfattning** tillhandahåller ytterligare information om hello processen anslutningen, inklusive dess bundna portar, inkommande och utgående anslutningar och kunde inte anslutningar.

![Processen sammanfattningsfönstret](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Operations Management Suite aviseringar integrering
Tjänstkarta integreras med Operations Management Suite varningar tooshow utlöses aviseringar för valda hello-servern i hello valt tidsintervall. en ikon visas i hello-servern om det finns aktuella aviseringar och hello **datorn aviseringar** fönstret visas hello aviseringar.

![Datorn aviseringspanelen](media/oms-service-map/machine-alerts.png)

tooenable Tjänstkarta toodisplay relevanta aviseringarna, skapa en aviseringsregel som utlöses för en specifik dator. toocreate rätt aviseringar:
- Inkludera en sats toogroup per dator (till exempel **datorn intervall 1 minut**).
- Välj tooalert baserat på mått mått.

![Konfiguration av varning](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Operations Management Suite logga händelser integrering
Tjänstkarta kan integreras med loggen Sök tooshow en uppräkning av alla tillgängliga logghändelser för valda hello-servern under hello valt tidsintervall. Du kan klicka på varje rad i hello lista av händelse antal toojump tooLog Sök och se hello enskilda logghändelser.

![Datorn logghändelser fönstret](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Operations Management Suite-supporten integrering
Tjänstkarta integrering med hello IT Service Management-anslutningstjänsten sker automatiskt när båda lösningarna är aktiverad och konfigurerad i Operations Management Suite-arbetsyta. hello integrering i Tjänstkartan är märkt ”helpdesk”. Mer information finns i [centralt hantera ITSM arbetsobjekt med hjälp av IT Service Management-anslutningstjänsten](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

Hej **datorn helpdesk** fönstret innehåller alla händelser som IT-tjänsthantering för hello valda servern i hello valt tidsintervall. hello-server visas en ikon om det aktuella objekt och hello datorn helpdesk rutan visas de.

![Datorn helpdesk fönstret](media/oms-service-map/service-desk.png)

tooopen hello objekt i din anslutna ITSM lösning Klicka **visa arbetsobjekt**.

tooview hello information om hello objekt i loggen sökning, klickar du på **visas i loggen Sök**.


## <a name="operations-management-suite-change-tracking-integration"></a>Operations Management Suite ändringsspårning integrering
Tjänstkarta integrering med ändringsspårning sker automatiskt när båda lösningarna är aktiverad och konfigurerad i Operations Management Suite-arbetsyta.

Hej **datorn ändringsspårning** fönstret visar alla ändringar med hello senaste först, tillsammans med en länk toodrill ned tooLog Sök efter ytterligare information.

![Datorn ändringsspårning fönstret](media/oms-service-map/change-tracking.png)

hello följande bild är en detaljerad vy av en ConfigurationChange händelse som kan uppstå när du har valt **visas i logganalys**.

![ConfigurationChange händelse](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Operations Management Suite prestanda integrering
Hej **prestanda på en dator** fönstret standard prestandastatistik för valda hello-servern. hello mått inkludera CPU-användning, minnesanvändning, nätverks-byte skickas och tas emot och en lista över hello Topprocesser med nätverks-byte skickas och tas emot. prestandadata för tooget hello nätverk, måste du också ha aktiverat hello överföring Data 2.0 lösning i Operations Management Suite.

![Datorn prestanda fönstret](media/oms-service-map/machine-performance.png)


## <a name="operations-management-suite-security-integration"></a>Integrering av Operations Management Suite-säkerhet
Tjänstkarta integrering med säkerhet och granska sker automatiskt när båda lösningarna är aktiverad och konfigurerad i Operations Management Suite-arbetsyta.

Hej **säkerhet dator** visar data från hello Operations Management Suite säkerhet och granska lösningen för hello valda servern. hello fönstret visas en sammanfattning av alla utestående säkerhetsproblem för hello server under hello valt tidsintervall. Klicka på någon av hello säkerhet problem flyttar ned till en logg sökning efter information om dem.

![Datorn säkerhet fönstret](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Integrering av Operations Management Suite-uppdateringar
Tjänstkarta integrering med uppdateringshantering sker automatiskt när båda lösningarna är aktiverad och konfigurerad i Operations Management Suite-arbetsyta.

Hej **datorn uppdateringar** rutan visar data från hello Operations Management Suite uppdateringshantering lösning för hello valda servern. hello-fönstret visas en sammanfattning av uppdateringar som saknas för hello servern under hello valt tidsintervall.

![Datorn ändringsspårning fönstret](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics-poster
Tjänstkarta dator- och lagerdata är tillgänglig för [Sök](../log-analytics/log-analytics-log-searches.md) i logganalys. Du kan använda den här informationen tooscenarios som innehåller planering för migrering, kapacitetsanalys, identifiering och felsökning av prestanda på begäran.

Genereras en post per timme för varje unikt datornamn och processen kan dessutom toohello poster som genereras när en process eller datorn startas inte eller är publicerat tooService mappa. Dessa poster har hello egenskaper i hello följande tabeller. mappa toofields av hello datorresurs i hello ServiceMap Azure Resource Manager API hello fält och värden i hello ServiceMapComputer_CL händelser. hello mappning fält och värden i hello ServiceMapProcess_CL händelser toohello av hello processen resurs i hello ServiceMap Azure Resource Manager API. Hej ResourceName_s fält matchar hello namnfältet i hello motsvarande Resource Manager-resurs. 

>[!NOTE]
>Eftersom Tjänstkarta funktioner växer, är dessa fält ämne toochange.

Det finns internt genererade egenskaper som du kan använda tooidentify unika processer och datorer:

- Dator: Använd ResourceId eller ResourceName_s toouniquely identifierar en dator i en Operations Management Suite-arbetsyta.
- Process: Använd ResourceId toouniquely identifiera en process i en Operations Management Suite-arbetsyta. ResourceName_s är unikt inom hello kontext för hello dator på vilken hello processen körs (MachineResourceName_s) 

Eftersom flera poster kan finnas för en angiven process och datorer i ett angivet tidsintervall frågor kan returnera fler än en post för hello samma dator eller process. tooinclude hello endast den senaste post, Lägg till ”| dedupliceringen ResourceId ”toohello frågan.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL poster
Poster med en typ av *ServiceMapComputer_CL* har inventeringsdata för servrar med Tjänstkarta agenter. Dessa poster har hello egenskaper i den följande tabellen hello:

| Egenskap | Beskrivning |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| Resurs-ID | hello Unik identifierare för en dator inom hello arbetsytan |
| ResourceName_s | hello Unik identifierare för en dator inom hello arbetsytan |
| ComputerName_s | hello datorn FQDN |
| Ipv4Addresses_s | En lista över hello-serverns IPv4-adresser |
| Ipv6Addresses_s | En lista med IPv6-adresser hello servrar |
| DnsNames_s | En matris med DNS-namn |
| OperatingSystemFamily_s | Windows- eller Linux |
| OperatingSystemFullName_s | hello fullständigt namn av hello operativsystem  |
| Bitness_s | hello bitness hello datorn (32-bitars eller 64-bitars)  |
| PhysicalMemory_d | hello fysiskt minne i MB |
| Cpus_d | hello antal processorer |
| CpuSpeed_d | hello CPU-hastighet i MHz|
| VirtualizationState_s | *Okänd*, *fysiska*, *virtuella*, *hypervisor-programmet* |
| VirtualMachineType_s | *Hyper-v*, *vmware*och så vidare |
| VirtualMachineNativeMachineId_g | hello VM-ID som tilldelats av dess hypervisor-programmet |
| VirtualMachineName_s | hello namnet på hello VM |
| BootTime_t | hello starttiden |



### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL poster
Poster med en typ av *ServiceMapProcess_CL* har inventeringsdata för TCP-anslutna processer på servrar med Tjänstkarta agenter. Dessa poster har hello egenskaper i den följande tabellen hello:

| Egenskap | Beskrivning |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| Resurs-ID | hello Unik identifierare för en process hello arbetsyta |
| ResourceName_s | hello Unik identifierare för en process i hello-dator som kör|
| MachineResourceName_s | hello-resursnamnet för hello datorn |
| ExecutableName_s | hello namnet på hello processen körbara |
| StartTime_t | Starttid för hello processen pool |
| FirstPid_d | hello första PID i hello processpool |
| Description_s | hello beskrivningen |
| CompanyName_s | hello namnet på hello företag |
| InternalName_s | hello interna namn |
| ProductName_s | hello namnet på hello produkt |
| ProductVersion_s | hello produktversion |
| FileVersion_s | hello filversion |
| CommandLine_s | hello kommandoraden |
| ExecutablePath _s | hello sökvägen toohello körbar fil |
| WorkingDirectory_s | hello arbetskatalogen |
| Användarnamn | hello konto under vilket hello processen körs |
| UserDomain | hello domänen under vilka hello processen körs |


## <a name="sample-log-searches"></a>Exempel på loggsökningar

### <a name="list-all-known-machines"></a>Visa en lista över alla kända datorer
Typ = ServiceMapComputer_CL | dedupliceringen ResourceId

### <a name="list-hello-physical-memory-capacity-of-all-managed-computers"></a>Lista över hello fysiskt minneskapacitet för alla hanterade datorer.
Typ = ServiceMapComputer_CL | Välj PhysicalMemory_d ComputerName_s | Dedupliceringen ResourceId

### <a name="list-computer-name-dns-ip-and-os"></a>Lista över datornamn, DNS, IP- och OS.
Typ = ServiceMapComputer_CL | Välj ComputerName_s, OperatingSystemFullName_s, DnsNames_s, IPv4Addresses_s | dedupliceringen ResourceId

### <a name="find-all-processes-with-sql-in-hello-command-line"></a>Hitta alla processer med ”sql” hello kommandoraden
Typ = ServiceMapProcess_CL CommandLine_s = \*sql\* | dedupliceringen ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Hitta en dator (senaste post) efter resursnamn
Typ = ServiceMapComputer_CL ”m-4b9c93f9-bc37-46df-b43c-899ba829e07b” | dedupliceringen ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Hitta en dator (senaste post) med IP-adress
Typ = ServiceMapComputer_CL ”10.229.243.232” | dedupliceringen ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>Visa en lista över alla kända processer på en angiven dator
Typ = ServiceMapProcess_CL MachineResourceName_s="m-4b9c93f9-bc37-46df-b43c-899ba829e07b” | dedupliceringen ResourceId

### <a name="list-all-computers-running-sql"></a>Lista med alla datorer som kör SQL
Typ = ServiceMapComputer_CL ResourceName_s IN {typ = ServiceMapProcess_CL \*sql\* | Distinkta MachineResourceName_s} | dedupliceringen ResourceId | Distinkta ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lista över alla unika versioner av curl i min datacenter
Typ = ServiceMapProcess_CL ExecutableName_s = curl | Distinkta ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Skapa en datorgrupp för alla datorer som kör CentOS
Typ = ServiceMapComputer_CL OperatingSystemFullName_s = \*CentOS\* | Distinkta ComputerName_s


## <a name="rest-api"></a>REST API
Alla hello-server, process och beroende data i Tjänstkartan är tillgänglig via hello [Service Map REST API](https://docs.microsoft.com/rest/api/servicemap/).


## <a name="diagnostic-and-usage-data"></a>diagnostik och användningsdata
Microsoft samlar automatiskt in användnings- och prestandadata via din användning av hello Tjänstkarta service. Microsoft använder denna data tooprovide och förbättra hello kvalitet, säkerhet och integritet hello Tjänstkarta service. tooprovide korrekta och effektiva funktioner för felsökning, hello data innehåller information om hello konfiguration av din programvara, till exempel operativsystem och version, IP-adress, DNS-namn och arbetsstationens namn. Microsoft samlar inte in namn, adresser eller annan kontaktinformation.

Mer information om insamling och användning finns i hello [sekretesspolicy för Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Nästa steg
Lär dig mer om [logga sökningar](../log-analytics/log-analytics-log-searches.md) logganalys tooretrieve data som samlas in av Tjänstkartan.


## <a name="troubleshooting"></a>Felsökning
Se hello [felsökning i hello konfigurera Tjänstkarta dokumentet](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Feedback
Du har feedback för oss om Tjänstkarta eller den här dokumentationen?  Besök vår [User Voice sidan](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), där du kan föreslår funktioner eller rösta in befintliga förslag.
