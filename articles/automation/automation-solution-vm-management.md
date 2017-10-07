---
title: "aaaStart/stoppa virtuella datorer vid låg belastning på nätverket [förhandsgranskning] lösning | Microsoft Docs"
description: "hello VM hanteringslösningar startar och stoppar Azure Resource Manager virtuella datorer på ett schema och övervakning proaktiv från logganalys."
services: automation
documentationCenter: 
authors: mgoedtel
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: magoedte
ms.openlocfilehash: 6cbe16dfb40bf13f29d9e58ca0bc8c5c7979879d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="startstop-vms-during-off-hours-preview-solution-in-automation"></a>Starta/stoppa virtuella datorer vid låg belastning på nätverket [förhandsgranskning] Lösning i Automation

hello Starta/stoppa virtuella datorer vid låg belastning på nätverket [förhandsgranskning] lösning startar och stoppar Azure Resource Manager virtuella datorer på ett fördefinierat schema och ger inblick i hello lyckats hello Automation-jobb som starta och stoppa virtuella datorer med OMS Logganalys.  

## <a name="prerequisites"></a>Krav

- Hej runbooks fungerar med en [Azure kör som-konto](automation-offering-get-started.md#authentication-methods).  hello kör som-konto är hello önskad autentiseringsmetod eftersom den använder certifikatautentisering i stället för ett lösenord som kan gälla eller som ändras ofta.  

- Den här lösningen kan endast hantera virtuella datorer som är i hello samma prenumeration som där hello Automation-kontot finns.  

- Den här lösningen bara distribuerar toohello följande Azure-regioner - Australien sydost, östra USA, Sydostasien och västra Europa.  Hej runbooks som hanterar hello VM schema kan rikta virtuella datorer i en region.  

- toosend e-postmeddelanden när hello starta och stoppa VM-runbooks som är klar, affärsklass Office 365-prenumeration krävs.  

## <a name="solution-components"></a>Lösningskomponenter

Den här lösningen består av hello efter resurser som ska importeras och lägga till tooyour Automation-konto.

### <a name="runbooks"></a>Runbooks

Runbook | Beskrivning|
--------|------------|
CleanSolution-MS-Mgmt-VM | Denna runbook tar bort alla befintliga resurser, och scheman för när du går toodelete hello lösningen från prenumerationen.|  
SendMailO365-MS-Mgmt | Denna runbook skickar ett e-postmeddelande via Office 365 Exchange.|
StartByResourceGroup-MS-Mgmt-VM | Denna runbook är avsedda toostart virtuella datorer (både klassiska och ARM-baserade virtuella datorer) som finns i en angiven lista med grupper i Azure-resurs.
StopByResourceGroup-MS-Mgmt-VM | Denna runbook är avsedda toostop virtuella datorer (både klassiska och ARM-baserade virtuella datorer) som finns i en angiven lista med grupper i Azure-resurs.|
<br>

### <a name="variables"></a>Variabler

Variabel | Beskrivning|
---------|------------|
**SendMailO365-MS-Mgmt** Runbook ||
SendMailO365-IsSendEmail-MS-Mgmt | Anger om StartByResourceGroup-MS-Mgmt-VM och StopByResourceGroup-MS-Mgmt-VM-runbooks kan skicka e-postmeddelande när återställningen är klar.  Välj **SANT** tooenable och **FALSKT** toodisable e-aviseringar. Standardvärdet är **Falskt**.| 
**StartByResourceGroup-MS-Mgmt-VM** Runbook ||
StartByResourceGroup-ExcludeList-MS-Mgmt-VM | Ange VM namn toobe uteslutas från management-åtgärd; separera namn med hjälp av semi-colon(;) utan blanksteg. Värden är skiftlägeskänsliga och jokertecken (asterisk) stöds.|
StartByResourceGroup SendMailO365-EmailBodyPreFix-MS-Mgmt | Texten som kan vara efter toohello början av hello e-postmeddelandet.|
StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Anger hello för hello Automation-konto som innehåller hello e-runbook.  **Ändra inte den här variabeln.**|
StartByResourceGroup SendMailO365-EmailRunbookName-MS-Mgmt | Anger hello för hello e-runbook.  Detta används av hello StartByResourceGroup-MS-Mgmt-VM och StopByResourceGroup-MS-Mgmt-VM runbooks toosend e-post.  **Ändra inte den här variabeln.**|
StartByResourceGroup SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Anger hello hello resursgruppen som innehåller hello e-runbook.  **Ändra inte den här variabeln.**|
StartByResourceGroup SendMailO365-EmailSubject-MS-Mgmt | Anger hello texten hello ämnesraden av hello e-post.|  
StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Anger hello mottagare av hello e-post.  Ange separata namn med semi-colon(;) utan blanksteg.|
StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Ange VM namn toobe uteslutas från management-åtgärd; separera namn med hjälp av semi-colon(;) utan blanksteg. Värden är skiftlägeskänsliga och jokertecken (asterisk) stöds.  Standardvärdet (asterisk) tas alla resursgrupper i hello prenumerationen.|
StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Anger hello-prenumeration som innehåller virtuella datorer toobe hanteras av den här lösningen.  Det här måste vara hello samma prenumeration där hello Automation-konto på den här lösningen finns.|
**StopByResourceGroup-MS-Mgmt-VM** Runbook ||
StopByResourceGroup-ExcludeList-MS-Mgmt-VM | Ange VM namn toobe uteslutas från management-åtgärd; separera namn med hjälp av semi-colon(;) utan blanksteg. Värden är skiftlägeskänsliga och jokertecken (asterisk) stöds.|
StopByResourceGroup SendMailO365-EmailBodyPreFix-MS-Mgmt | Texten som kan vara efter toohello början av hello e-postmeddelandet.|
StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Anger hello för hello Automation-konto som innehåller hello e-runbook.  **Ändra inte den här variabeln.**|
StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Anger hello hello resursgruppen som innehåller hello e-runbook.  **Ändra inte den här variabeln.**|
StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Anger hello texten hello ämnesraden av hello e-post.|  
StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Anger hello mottagare av hello e-post.  Ange separata namn med semi-colon(;) utan blanksteg.|
StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Ange VM namn toobe uteslutas från management-åtgärd; separera namn med hjälp av semi-colon(;) utan blanksteg. Värden är skiftlägeskänsliga och jokertecken (asterisk) stöds.  Standardvärdet (asterisk) tas alla resursgrupper i hello prenumerationen.|
StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Anger hello-prenumeration som innehåller virtuella datorer toobe hanteras av den här lösningen.  Det här måste vara hello samma prenumeration där hello Automation-konto på den här lösningen finns.|  
<br>

### <a name="schedules"></a>Scheman

Schema | Beskrivning|
---------|------------|
StartByResourceGroup-schema-MS-Mgmt | Schema för StartByResourceGroup runbook som utför hello Start för virtuella datorer som hanteras av den här lösningen. När skapas standardvärdet tooUTC tidszon.|
StopByResourceGroup-schema-MS-Mgmt | Schema för StopByResourceGroup runbook som utför hello avstängning av virtuella datorer som hanteras av den här lösningen. När skapas standardvärdet tooUTC tidszon.|

### <a name="credentials"></a>Autentiseringsuppgifter

Autentiseringsuppgift | Beskrivning|
-----------|------------|
O365-Autentiseringsuppgift | Anger en giltig Office 365 användarens konto toosend e-postadress.  Krävs endast om variabeln SendMailO365-IsSendEmail-MS-Mgmt anges för**SANT**.

## <a name="configuration"></a>Konfiguration

Utföra hello följande steg tooadd hello Starta/stoppa virtuella datorer vid låg belastning på nätverket [förhandsgranskning] lösning tooyour Automation-konto och konfigurera sedan hello variabler toocustomize hello lösning.

1. Hello-startsidan i hello Azure-portalen, Välj hello **Marketplace** panelen.  Om hello panelen är inte längre Fäst tooyour-startsidan hello vänstra navigeringsfönstret väljer **ny**.  
2. Skriv i hello Marketplace bladet **starta VM** i hello sökrutan och välj sedan hello lösning **Starta/Stoppa VMs kontorstid [förhandsgranskning]** från hello sökresultaten.  
3. I hello **Starta/Stoppa VMs kontorstid [förhandsgranskning]** bladet för hello markerad lösning granska hello sammanfattningsinformation och klicka sedan på **skapa**.  
4. Hej **Lägg till lösning** bladet visas där du är begärd tooconfigure hello lösningen innan du kan importera till Automation-prenumeration.<br><br> ![Bladet VM-hantering, lägga till lösning](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5.  På hello **Lägg till lösning** bladet väljer **arbetsytan** och här du väljer en OMS-arbetsyta som är länkade toohello som tillhör samma Azure-prenumeration som hello Automation-konto eller skapa en ny OMS-arbetsyta.  Om du inte har en OMS-arbetsyta kan du välja **Skapa ny arbetsyta** och på hello **OMS-arbetsytan** bladet utföra hello följande: 
   - Ange ett namn för hello nya **OMS-arbetsytan**.
   - Välj en **prenumeration** toolink tooby att välja hello nedrullningsbara listan om hello standard valt inte är lämplig.
   - Du kan skapa en ny **resursgrupp** eller välja en befintlig resursgrupp.  
   - Välj en **Plats**.  Hello endast platser för markeringen är för närvarande **Australien sydost**, **östra USA**, **Sydostasien**, och **Västeuropa**.
   - Välj en **Prisnivå**.  hello lösningen erbjuds i två nivåer: Frigör och OMS betald nivå.  hello kostnadsfria nivån har en gräns på hello mängden data som samlas in varje dag, Bevarandeperiod och runbook-jobbet runtime minuter.  hello OMS betald nivån har inte en gräns på hello mängden data som samlas in varje dag.  

        > [!NOTE]
        > Medan hello fristående betald nivå visas som ett alternativ kan är den inte tillämplig.  Om du väljer det och fortsätta med hello skapandet av den här lösningen i din prenumeration, misslyckas.  Detta åtgärdas när den här lösningen släpps officiellt.<br>Om du använder den här lösningen, kommer den endast använda automation-jobbminuter och logga inmatning.  hello-lösning lägger inte till ytterligare OMS noder tooyour miljö.  

6. När du har angett hello krävs information på hello **OMS-arbetsytan** bladet, klickar du på **skapa**.  Hello information har verifierats och hello arbetsytan har skapats, du kan följa förloppet under **meddelanden** hello-menyn.  Du kommer att returneras toohello **Lägg till lösning** bladet.  
7. På hello **Lägg till lösning** bladet väljer **Automation-konto**.  Om du skapar en ny OMS-arbetsyta, kommer du att nödvändiga tooalso skapar ett nytt Automation-konto som ska associeras med hello ny OMS-arbetsytan angavs tidigare, inklusive Azure-prenumeration, resursgrupp och region.  Du kan välja **skapa ett Automation-konto** och på hello **lägga till Automation-konto** bladet ange hello följande: 
  - I hello **namn** anger hello namnet på hello Automation-konto.

    Alla andra alternativ fylls i automatiskt baserat på valda hello OMS-arbetsytan och dessa alternativ kan inte ändras.  Ett Azure kör som-konto är hello standardmetoden för autentisering för hello runbooks i den här lösningen.  När du klickar på **OK**hello konfigurationsalternativ verifieras och hello Automation-kontot har skapats.  Du kan följa förloppet under **meddelanden** hello-menyn. 

    Annars kan du välja ett befintligt Automation kör som-konto.  Observera att hello kontot som du väljer kan inte redan vara länkade tooanother OMS-arbetsyta, annars ett meddelande visas i hello bladet tooinform du.  Om den redan är länkad kommer du behöver tooselect ett annat Automation kör som-konto eller skapa en ny.<br><br> ![Automation-konto är redan länkad tooOMS arbetsytan](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Slutligen på hello **Lägg till lösning** bladet väljer **Configuration** och hello **parametrar** bladet visas.  På hello **parametrar** bladet uppmanas du att:  
   - Ange hello **ResourceGroup målnamn**, vilket är ett Resursgruppsnamn som innehåller virtuella datorer toobe hanteras av den här lösningen.  Du kan ange flera namn och skilja dem åt med ett semikolon (värden är skiftlägeskänsliga).  Om du vill tootarget virtuella datorer i alla resursgrupper i prenumerationen hello stöds med jokertecken.
   - Välj en **schema** som är ett återkommande datum och tid för att starta och stoppa hello Virtuella datorer i hello mål resurs grupperna.  Som standard hello schema är konfigurerade toohello UTC-tidszonen och välja en annan region finns inte.  Om du vill tooconfigure hello schema tooyour viss tidszon när du har konfigurerat hello lösning finns [ändra hello start och stopp schema](#modifying-the-startup-and-shutdown-schedule) nedan.    

10. När du har slutfört konfigurera hello startinställningar som krävs för hello-lösning, Välj **skapa**.  Alla inställningar kommer att valideras och sedan försöker den toodeploy hello lösning i din prenumeration.  Den här processen kan ta flera sekunder toocomplete och du kan följa förloppet under **meddelanden** hello-menyn. 

## <a name="collection-frequency"></a>Insamlingsfrekvens

Automation-loggen och jobbet dataströmmen jobbdata inhämtas in hello OMS databasen var femte minut.  

## <a name="using-hello-solution"></a>Med hello-lösning

När du lägger till hello VM hanteringslösningen, i din OMS-arbetsytan hello **StartStopVM visa** panelen läggs tooyour OMS-instrumentpanelen.  Den här panelen visar antalet och grafisk representation av hello runbooks jobb för hello lösning som har startat och har slutförts.<br><br> ![Panelen VM-hantering, StartStopVM-vy](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

I ditt Automation-konto som du kan komma åt och hantera hello lösning genom att välja hello **lösningar** panelen och sedan hello **lösningar** bladet väljer hello lösning **Start-Stop-VM [ Arbetsytan]** hello-listan.<br><br> ![Lista över Automation-lösningar](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

När du väljer hello lösning visas hello **Start-Stop-VM [Workspace]** lösning bladet där du kan granska viktig information, till exempel hello **StartStopVM** panelen, som i din OMS-arbetsyta som Visar antalet och grafisk representation av hello runbooks jobb för hello lösning som har startat och har slutförts.<br><br> ![Lösningsblad i Automation VM](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Du kan också öppna OMS-arbetsytan och utföra ytterligare analys av hello jobbposter härifrån.  Klicka bara på **alla inställningar**, och i hello **inställningar** bladet väljer **Snabbstart** och klicka sedan på hello **Snabbstart** bladet välj ** OMS-portalen**.   Det öppnar en ny flik eller en ny webbläsarsession och visar dig OMS-arbetsytan som är kopplad till ditt Automation-konto och prenumeration.  


### <a name="configuring-e-mail-notifications"></a>Konfigurera e-postaviseringar

tooenable e-postaviseringar när hello starta och stoppa VM-runbooks som är klar måste du toomodify hello **O365Credential** autentiseringsuppgifter och minst hello följande variabler:

 - SendMailO365-IsSendEmail-MS-Mgmt
 - StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
 - StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

tooconfigure hello **O365Credential** autentiseringsuppgifter, utföra hello följande steg:

1. Från ditt automation-konto klickar du på **alla inställningar** hello överst i fönstret hello. 
2. På hello **inställningar** bladet under hello avsnittet **Automation resurser**väljer **tillgångar**. 
3. På hello **tillgångar** bladet, Välj hello **autentiseringsuppgifter** panelen och från hello **autentiseringsuppgifter** bladet, Välj hello **O365Credential**.  
4. Ange ett giltigt Office 365-användarnamn och lösenord och klicka sedan på **spara** toosave ändringarna.  

tooconfigure hello variabler markerade tidigare, utför hello följande steg:

1. Från ditt automation-konto klickar du på **alla inställningar** hello överst i fönstret hello. 
2. På hello **inställningar** bladet under hello avsnittet **Automation resurser**väljer **tillgångar**. 
3. På hello **tillgångar** bladet, Välj hello **variabler** panelen och från hello **variabler** bladet välj hello variabel som anges ovan och sedan ändra dess värde hello Beskrivning för den angivna i hello [variabeln](##variables) ovan.  
4. Klicka på **spara** toosave hello ändringar toohello variabeln.   

### <a name="modifying-hello-startup-and-shutdown-schedule"></a>Ändra hello start och stopp schema

Hantera hello-start och stopp schema i den här lösningen följer samma steg som beskrivs i hello [schemaläggning av en runbook i Azure Automation](automation-schedules.md).  Kom ihåg att du inte kan ändra hello schemakonfigurationen.  Du behöver toodisable hello befintligt schema och sedan skapa en ny och sedan länka toohello **StartByResourceGroup-MS-Mgmt-VM** eller **StopByResourceGroup-MS-Mgmt-VM** runbook som du vill hello schemalägga tooapply till.   

## <a name="log-analytics-records"></a>Log Analytics-poster

Automation skapar två typer av poster i hello OMS-databasen.

### <a name="job-logs"></a>Jobbloggar

Egenskap | Beskrivning|
----------|----------|
Anropare |  Vem som initierat hello igen.  Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.|
Kategori | Klassificering av hello typ av data.  Hello-värdet är JobLogs för automatisering.|
CorrelationId | GUID som är hello Korrelations-Id för hello runbook-jobbet.|
JobId | GUID som är hello-Id för hello runbook-jobbet.|
operationName | Anger hello åtgärd utförs i Azure.  För automatisering hello värdet kommer att vara jobb.|
resourceId | Anger hello resurstypen i Azure.  Hello-värdet är hello Automation-konto som är associerade med hello runbook för automatisering.|
ResourceGroup | Anger hello resursgruppens namn av hello runbook-jobbet.|
ResourceProvider | Anger hello Azure-tjänst som tillhandahåller hello resurser kan du distribuera och hantera.  Hello-värdet är för automatisering och Azure Automation.|
ResourceType | Anger hello resurstypen i Azure.  Hello-värdet är hello Automation-konto som är associerade med hello runbook för automatisering.|
resultType | hello status för hello runbook-jobbet.  Möjliga värden:<br>- Startad<br>- Stoppad<br>-Pausad<br>- Misslyckades<br>- Slutförd|
resultDescription | Beskriver hello runbook jobbstatus resultat.  Möjliga värden:<br>-Jobbet har startats<br>-Jobbet misslyckades<br>-Jobbet slutfördes|
RunbookName | Anger hello för hello runbook.|
SourceSystem | Anger hello källsystemet för hello-data som skickats.  För automatisering hello värdet kommer att vara: OpsManager|
StreamType | Anger hello typen av händelse. Möjliga värden:<br>- Verbose<br>- Utdata<br>- Fel<br>- Varning|
SubscriptionId | Anger hello prenumerations-ID för hello jobb.
Tid | Datum och tid när hello runbook-jobbet körs.|


### <a name="job-streams"></a>Arbetsflöden

Egenskap | Beskrivning|
----------|----------|
Anropare |  Vem som initierat hello igen.  Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.|
Kategori | Klassificering av hello typ av data.  Hello-värdet är JobStreams för automatisering.|
JobId | GUID som är hello-Id för hello runbook-jobbet.|
operationName | Anger hello åtgärd utförs i Azure.  För automatisering hello värdet kommer att vara jobb.|
ResourceGroup | Anger hello resursgruppens namn av hello runbook-jobbet.|
resourceId | Anger hello resursens Id i Azure.  Hello-värdet är hello Automation-konto som är associerade med hello runbook för automatisering.|
ResourceProvider | Anger hello Azure-tjänst som tillhandahåller hello resurser kan du distribuera och hantera.  Hello-värdet är för automatisering och Azure Automation.|
ResourceType | Anger hello resurstypen i Azure.  Hello-värdet är hello Automation-konto som är associerade med hello runbook för automatisering.|
resultType | hello resultatet av hello runbook-jobb vid hello tid hello händelsen skapades.  Möjliga värden:<br>- Ärendeposter|
resultDescription | Innehåller hello utdataström från hello runbook.|
RunbookName | hello namnet på hello runbook.|
SourceSystem | Anger hello källsystemet för hello-data som skickats.  För automatisering hello värdet kommer att vara OpsManager|
StreamType | hello typ av jobbström. Möjliga värden:<br>- Status<br>- Utdata<br>- Varning<br>- Fel<br>- Felsökning<br>- Verbose|
Tid | Datum och tid när hello runbook-jobbet körs.|

När du utför alla loggen sökningar som returnerar poster för **JobLogs** eller **JobStreams**, kan du välja hello **JobLogs** eller **JobStreams** vy som visar en uppsättning paneler sammanfattning hello-uppdateringar returneras av hello sökningen.

## <a name="sample-log-searches"></a>Exempel på loggsökningar

hello innehåller följande tabell exempel loggen söker efter jobbposter som samlas in av den här lösningen. 

Fråga | Beskrivning|
----------|----------|
Hitta jobb för runbook StartVM som har slutförts | Kategori = JobLogs RunbookName_s = "StartByResourceGroup-MS-Mgmt-VM" ResultType = lyckades & #124; mätning av antal () efter JobId_g|
Hitta jobb för runbook StopVM som har slutförts | Kategori = JobLogs RunbookName_s = "StartByResourceGroup-MS-Mgmt-VM" ResultType = lyckades & #124; mätning av antal () efter JobId_g
Visa jobbstatus med tiden för runbookarna StartVM och StopVM | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ELLER "StopByResourceGroup-MS-Mgmt-VM" INTE(ResultType="started") | Mät Count() ResultType intervall 1 dag|

## <a name="removing-hello-solution"></a>Ta bort hello lösning

Om du inte längre behöver toouse hello lösningen några ytterligare kan du ta bort den från hello Automation-konto.  Hello lösning tas endast bort hello runbooks, tas inte bort hello scheman eller variabler som skapades när hello lösning har lagts till.  Dessa resurser måste toodelete manuellt om du inte använder dem med andra runbooks.  

toodelete Hej lösning, utföra hello följande steg:

1.  Automation-konto, Välj hello **lösningar** panelen.  
2.  På hello **lösningar** bladet, Välj hello lösning **Start-Stop-VM [Workspace]**.  På hello **VMManagementSolution [Workspace]** bladet hello-menyn klickar du på **ta bort**.<br><br> ![Ta bort VM Mgmt lösning](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  I hello **ta bort lösningen** fönstret bekräfta toodelete hello lösning.
4.  Hello information har verifierats och hello lösningen tas bort, du kan följa förloppet under **meddelanden** hello-menyn.  Du kommer att returneras toohello **VMManagementSolution [Workspace]** bladet när hello processen tooremove lösning startar.  

OMS-arbetsytan och hello Automation-konto raderas inte som en del av den här processen.  Om du inte vill att tooretain hello OMS-arbetsyta måste toomanually ta bort den.  Detta kan åstadkommas också från hello Azure-portalen.   Hello-startsidan i hello Azure-portalen, Välj **logganalys** och klicka sedan på hello **logganalys** bladet, Välj hello arbetsytan och klickar på **ta bort** hello-menyn hello inställningsbladet för arbetsytan.  
      
## <a name="next-steps"></a>Nästa steg

- toolearn mer information om hur tooconstruct olika sökfrågor och granska hello Automation jobb loggar med Log Analytics finns [logga sökningar i logganalys](../log-analytics/log-analytics-log-searches.md)
- Mer om runbook-körningen hur toomonitor runbook-jobb och annan teknisk information finns i toolearn [spåra ett runbook-jobb](automation-runbook-execution.md)
- toolearn mer om logganalys OMS och datakällor för samlingen, se [insamling av Azure storage-data i logganalys-översikt](../log-analytics/log-analytics-azure-storage.md)






   

