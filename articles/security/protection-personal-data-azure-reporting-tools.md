---
title: aaaDocument skydd av personuppgifter med Azure reporting verktyg | Microsoft Docs
description: "hur toouse Azure reporting services och tekniker toohelp skydda sekretessen för personliga data."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 3230d26ed308a8a0e72421c001793be06334a7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Skydd av personuppgifter-dokument med Azure reporting verktyg

Den här artikeln handlar om hur toouse Azure reporting services och tekniker toohelp skydda sekretessen för personliga data.

## <a name="scenario"></a>Scenario

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna. toohelp dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien

hello företag använder Microsoft Azure för bearbetning och lagring av företagets data. Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas. Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser. hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.

Företagets anställda åtkomst hello nätverket från hello företagets fjärranslutna kontor och resa agenter finns runt hello world har åtkomst till företagsresurser för toosome.

## <a name="problem-statement"></a>Problembeskrivning

hello företag måste skydda hello säkerheten för de anställda och kunder personliga data via en flera lager säkerhetsstrategi som använder Azure-hantering och funktioner tooimpose strikt säkerhetskontroller på åtkomst tooand behandling av personliga uppgifter, och måste vara kan toodemonstrate dess skyddsåtgärder toointernal och externa granskare.

## <a name="company-goal"></a>Företagets mål

Som en del av dess skydd på djupet säkerhetsstrategi, den är en företagets mål tootrack all åtkomst tooand bearbetning av personliga data och se till att dokumentationen för tillräcklig integritetsskydd för personliga data finns på plats och fungerar.

## <a name="solutions"></a>Lösningar

Microsoft Azure tillhandahåller omfattande övervakning, loggning och diagnostik verktyg toohelp spåra och registrera aktiviteter och händelser associerade med åtkomst till och bearbeta personliga data, geografiska flödet av data och tredje parts åtkomst toopersonal data. Eftersom säkerheten för personliga data i hello molnet är ett delat ansvar, tillhandahåller Microsoft kunder med också:

- Detaljerad information om dess egna bearbetning av kunders data

- Säkerhetsåtgärder hanteras av Microsoft

- Var och hur den skickar kunders data

- Information om Microsofts egna sekretess går igenom processen

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) är Microsofts molnbaserade, flera innehavare katalog och identity management-tjänsten. hello tjänstens inloggning och granska rapporteringsfunktioner tillhandahålla detaljerad inloggnings- och programmet användning aktivitet information toohelp du övervaka och kontrollera åtkomst toocustomers och anställdas personliga data.

Det finns två typer av aktivitetsrapporter:

- Hej [granskningsloggar aktivitetrapporter/](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) ger detaljerad information om aktiviteter eller uppgifter i systemet

- Hej [inloggningar rapporten/aktivitetsloggen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) visar som har utfört varje aktivitet som anges i hello kontrollrapport

Du kan använda hello två tillsammans för att spåra hello historik för varje aktivitet och vem som utförde varje. Båda typerna av rapporter kan anpassas och kan filtreras.

#### <a name="how-do-i-access-hello-audit-and-security-logs"></a>Hur kommer jag åt hello granskning och säkerhet loggar?

hello gransknings-och kan nås från hello Active Directory-portalen på tre olika sätt: genom hello **aktiviteten** avsnitt (väljer du antingen **granskningsloggar** eller **inloggningar**), eller från **användare och grupper** eller **företagsprogram** under **hantera** i Active Directory. Rapporter kan även nås via hello Azure Active Directory reporting API. 

1. Välj i hello Azure-portalen, **Azure Active Directory.**

2. I hello **aktiviteten** väljer **granskningsloggar.**

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. Anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.

4.  Markera ett objekt i hello listan Visa toosee alla information om den.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Azure Active Directory reporting även innehåller två typer av säkerhetsrapporter, **användare som har flaggats för risk** och **riskfyllda inloggningar**, som kan hjälpa dig att övervaka potentiella risker i Azure-miljön.

Läs mer om hello rapporteringstjänsten [Azure Active Directory reporting](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)

Besök [Azure Active Directory aktivitetsrapporter](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) för mer information om hello rapporterna i Azure Active Directory. Den här platsen innehåller mer information om hur tooaccess och använda [granskningsloggar aktivitetsrapporter](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) och [inloggningsaktivitet rapporter](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) hello-portalen. Det innehåller även information om [användare som har flaggats för risk](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) och [riskfyllda inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) säkerhetsrapporter.

Besök hello [Azure Active Directory audit API-referens](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) plats för mer information om hur tooconnect tooAzure Directory reporting programmässigt.

### <a name="log-analytics"></a>Log Analytics

[Logga Analytics](https://azure.microsoft.com/services/log-analytics/) kan [samla in data från Azure-Monitor](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage) toocorrelate den med andra data och ange ytterligare analys. Azure övervakaren samlar in och analyserar övervakningsdata för Azure-miljön. 

Analysverktyg i logganalys som loggen sökningar, vyer och lösningar arbeta mot alla insamlade data, vilket ger dig centraliserad analys av hela miljön. Logganalys kan sammanställa och analysera händelseloggarna för Windows, IIS-loggar och systemloggar som kan hjälpa dig att upptäcka potentiella överträdelser av personliga data som kan uttsätta personuppgifter toounauthorized användare.

#### <a name="how-do-i-use-log-analytics"></a>Hur använder Log Analytics?

Du kan använda logganalys via hello OMS-portalen eller hello Azure-portalen från en webbläsare. Logganalys innehåller en fråga språk tooquickly hämta och konsolidera data i hello-databas. Du kan skapa och spara loggen sökningar toodirectly analysera data i hello-portalen.

toocreate logganalys-arbetsytan i hello Azure-portalen hello följande:

1. Välj **logganalys** hello listan med tjänster i hello Marketplace.

2. Välj **skapa,** sedan ange hello namnet på din OMS-arbetsyta din prenumeration, resursgrupp, plats och välj prisnivån.

3. Klicka på **OK** toodisplay en lista över dina arbetsytor.

4. Välj en arbetsyta toosee information.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

Besök hello [logganalys dokumentationen](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) toolearn mer om hello-tjänsten.

Besök hello [komma igång med en logganalys-arbetsytan](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) självstudiekursen toocreate en arbetsyta för utvärdering och hello grundläggande information om hur hur toouse hello service.

Besök hello efter webbsidor mer detaljerad information om hur tooconnect toouse logganalys med hello loggar som beskrivs ovan:

[Windows-händelseloggar datakällor i logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[IIS-loggar i logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[Syslog-datakällor i logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Azure-Monitor/Azure-aktivitetsloggen 

[Azure-Monitor](https://azure.microsoft.com/services/monitor/) innehåller basnivån infrastruktur mått och loggar för de flesta tjänster i Microsoft Azure.
Övervakning hjälper dig toogain djupa insikter om din Azure-program. Azure övervakaren är beroende av hello Azure diagnostics tillägg (Windows eller Linux) att samla in de flesta program nivån mått och loggar. [hello Azure-aktivitetsloggen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) är en av hello-resurser som du kan visa med Azure-Monitor. Den spårar varje API-anrop och innehåller en mängd information om aktiviteter som sker i [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
Du kan söka hello aktivitetsloggen (tidigare kallade drift- eller granskningsloggar) för information om resurs enligt hello Azure-infrastrukturen. 

Även om hello information registreras i hello aktivitet loggen gäller tooperformance och tjänsten för hälsotillstånd, det finns även information som är relaterade tooprotection av data. Använder hello aktivitetsloggen, kan du bestämma hello ”vad som, och när” för alla skrivåtgärder (PUT, POST, ta bort) vidtagits hello resurser i din Azure-prenumeration.

Till exempel innehåller en post när en administratör tar bort en nätverkssäkerhetsgrupp som kan påverka hello skydd av personuppgifter. Aktiviteten loggposter som lagras i Azure-Monitor i 90 dagar.

#### <a name="how-do-i-use-hello-data-collected-by-azure-monitor"></a>Hur använder hello data som samlas in av Azure-Monitor?

Det finns ett antal sätt toouse hello data i hello aktivitetsloggen och andra Azure-Monitor-resurser.

- Du kan strömma hello data tooother platser i verkliga rad.

- Du kan lagra hello data under längre tid än hello standardinställningar med hjälp av en [Azure storage-konto](https://docs.microsoft.com/azure/storage/common/storage-introduction) och ange en bevarandeprincip.

- Kan visual hello data i grafik och diagram, med hjälp av hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/), [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft PowerBI](https://powerbi.microsoft.com/), eller tredje parts visualiseringsverktyg.

- Du kan fråga hello data med hjälp av hello Azure-Monitor REST-API, CLI-kommandona [PowerShell](https://docs.microsoft.com/powershell/) cmdlets eller hello .NET SDK.

Välj tooget igång med Azure-Monitor **fler tjänster** i hello Azure-portalen.

1. Rulla nedåt för**övervakaren** i hello **övervaka och hantera** avsnitt.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  Övervakaren öppnas i hello **aktivitetsloggen** vyn.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

Du kan skapa och spara frågor för vanliga filter och PIN-kod hello viktigaste frågor tooa portalens instrumentpanel så att du vet alltid om händelser som uppfyller dina kriterier har inträffat.

1. Du kan filtrera hello vy av resursgrupp, timespan och händelsekategori.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Du kan sedan fästa frågor tooa portalens instrumentpanel genom att klicka på hello **PIN-kod** knappen. På så sätt kan du skapa en enda källa för användningsdata om dina tjänster. hello frågans namn och antalet resultat visas på instrumentpanelen för hello.

Du kan också använda hello övervakaren tooview mått för alla Azure-resurser, Konfigurera diagnostikinställningar för- och aviseringar och söka hello-loggen. Mer information om hur toouse hello Azure-Monitor och aktivitetsloggen, se [Kom igång med Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Azure Diagnostics 

hello diagnostik kapacitet i Azure möjliggör insamling av data från flera källor. hello Windows händelseloggar, bland annat hello säkerhetsloggen, kan vara särskilt användbart i spårning och dokumentera skydd av personuppgifter. hello säkerhetsloggen spårar misslyckade och lyckade inloggningshändelser, samt behörighetsändringar, identifiera mönster som anger vissa typer av attacker, ändringar av säkerhetsrelaterade principer, medlemskap i säkerhetsgrupper och mycket mer.

Till exempel aviseringar händelse-ID 4695 du toohello försökte unprotection av granskningsbara skyddade data. Detta gäller toohello Data Protection API (DPAPI), som hjälper tooprotect data, till exempel privata nycklar, lagrade autentiseringsuppgifter och annan konfidentiell information.

#### <a name="how-do-i-enable-hello-diagnostics-extension-for-windows-vms"></a>Hur aktiverar hello diagnostik tillägg för virtuella Windows-datorer?

Du kan använda PowerShell tooenable hello diagnostik tillägget för en virtuell dator i Windows, så som toocollect loggdata. hello steg för att göra det beror på vilken distributionsmodell du använder (Resource Manager eller klassisk). tooenable hello diagnostik tillägg på en befintlig virtuell dator som har skapats via hello Resource Manager-distributionsmodellen, som du kan använda hello [Set-AzureRMVMDiagnosticsExtension PowerShell-cmdleten](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1).

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$diagnosticsconfig_path* är hello sökvägen toohello fil som innehåller hello diagnostik konfigurationen i XML. Detaljerade instruktioner om hur du aktiverar Azure-diagnostik på en virtuell dator, se [Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows.](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics)

hello Azure diagnostics tillägget kan överföra hello insamlade data tooan Azure storage-konto eller skicka det tooservices som Application Insights. Du kan sedan använda hello data för granskning.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Hur jag för att lagra och visa diagnostikdata?

Det är viktigt tooremember att diagnostikdata inte lagras permanent om du överför toohello Microsoft Azure storage-emulatorn eller tooAzure lagringsutrymme. toostore och visa diagnostiska data i Azure Storage, Följ dessa steg:

1. Ange ett lagringskonto i hello ServiceConfiguration.cscfg-filen. Azure Diagnostics kan använda hello Blob-tjänsten eller hello tabelltjänsten, beroende på hello typ av data. Windows-händelseloggar lagras i tabellformat.

2. Överföra hello data. Du kan begära tootransfer hello diagnostikdata via hello konfigurationsfilen. För SDK-2.4 och tidigare kan du också göra hello begäran programmässigt.

3. Visa hello data med [Azure Lagringsutforskaren](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), [Server Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) i Visual Studio eller [Azure Diagnostics Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) i Azure Management Studio.

Mer information om hur tooperform av de här stegen finns [Store och visa diagnostiska data i Azure Storage.](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)

### <a name="azure-storage-analytics"></a>Azure Storage-analys 

Storage Analytics loggar detaljerad information om lyckade och misslyckade begäranden tooa storage-tjänst. Den här informationen kan vara används toomonitor enskilda begäranden som kan bidra till att dokumentera åtkomst toopersonal data som lagras i hello-tjänsten. Storage Analytics loggning är inte aktiverad som standard för ditt lagringskonto. Du kan aktivera den i hello Azure-portalen.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>Konfigurera övervakning för ett lagringskonto?

tooconfigure övervakning för ett lagringskonto hello följande:

1. Välj **lagringskonton** i hello Azure-portalen, markera hello namnet på hello-konto som du vill toomonitor.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. I hello **övervakning** väljer **diagnostik.**

3.  Välj hello **typen** mått data som du vill toomonitor för varje tjänst (Blob, tabell-fil). tooinstruct Azure Storage toosave diagnostik loggar för läsa, skriva och ta bort förfrågningar för hello blob-, tabell- och queue-tjänster, Välj **Blob-loggar, tabellen loggar** och **kö loggar.**

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. På hello skjutreglaget längst ned hello anger hello **kvarhållning** princip i dagar (värdet 1 – 365). Hello standardinställningen är sju dagar.

5. Välj **spara** tooapply hello-konfigurationsinställningar.

Lagring loggning loggposter innehålla följande information om enskilda förfrågningar hello:

- Tidsinformation som starttid, svarstid för slutpunkt till slutpunkt och svarstid för servern.

- Information om hello Lagringsåtgärden, till exempel hello åtgärdstypen hello nyckeln för hello lagring objektet hello klienten är åtkomst till lyckades eller misslyckades och returnerade toohello klientens hello-HTTP-statuskod.

- Information om autentisering, till exempel hello typ av autentisering hello klienten använde.

- Concurrency information, till exempel hello ETag-värde och tidsstämpel för senaste ändring.

- hello storlekar på hälsningsmeddelande för förfrågan och svar.

Mer detaljerad information om hur tooenable Storage Analytics loggning finns [övervaka ett lagringskonto i hello Azure-portalen.](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

### <a name="azure-security-center"></a>Azure Security Center 

[Azure Security Center](https://azure.microsoft.com/services/security-center/) Övervakare hello säkerhetstillståndet hos dina Azure-resurser i ordning tooprevent identifiera hot och ger rekommendationer för att svara. Den innehåller flera olika sätt toohelp dokumentet din säkerhetsåtgärder som skyddar hello sekretess personliga data.

Övervakning av säkerhetshälsa hjälper dig att säkerställa att dina säkerhetsprinciper. Säkerhetsövervakning är en proaktiv strategi där granskningar dina resurser tooidentify system som inte uppfyller organisationens normer och bästa praxis. Du kan övervaka hello säkerhetstillståndet hos hello följande resurser:

- Beräkningstimmar (virtuella datorer och molntjänster)

- Networking (virtuella nätverk)

- Lagrings- och (server och databas granskning och hotidentifiering identifiering, TDE, lagringskryptering)

- Program (potentiella säkerhetsproblem)

Säkerhetsproblem i någon av dessa kategorier kan utgöra ett hot toohello sekretess personliga data.

#### <a name="how-do-i-view-hello-security-state-of-my-azure-resources"></a>Hur visar hello säkerhetsläget för min Azure-resurser?

Security Center analyserar regelbundet hello säkerhetstillståndet hos dina Azure-resurser. Du kan visa alla eventuella säkerhetsproblem identifieras i hello **förebyggande** avsnittet hello instrumentpanelen.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. I hello **förebyggande** avsnitt, Välj hello **Compute** panelen. Här visas en **översikt,** tillsammans med hello **virtuella datorer** lista över alla virtuella datorer och deras säkerhetstillstånd och hello **molntjänster** lista över webb-och arbetsroller övervakas via Security Center.

2. På hello **översikt** fliken, andra en rekommendation tooview mer information.

3. På hello **virtuella datorer** väljer du ett VM-tooview ytterligare information.

När datainsamling har aktiverats i Azure Security Center hello Microsoft Monitoring Agent etableras automatiskt på alla befintliga och nya kompatibla virtuella datorer som distribueras. Data som samlas in från den här agenten lagras i antingen en befintlig [logganalys](https://azure.microsoft.com/services/log-analytics/) arbetsytan som är associerade med din prenumeration eller en ny arbetsyta.

[Hot Intelligence rapporter](https://docs.microsoft.com/azure/security-center/security-center-threat-report) tillhandahålls av Security Center. Dessa ger användbar information toohelp fram hello angripare identitet, mål, aktuell och historisk attack kampanjer och medvetande, verktyg och procedurer som används. Information om minskning och reparation ingår också.

hello Huvudsyftet med rapporterna hot är toohelp du toorespond effektivt toohello omedelbar hot och hjälper dig att vidta åtgärder efteråt toomitigate hello problemet. hello information i hello rapporter kan även vara användbart när du dokumentera dina incidenter för rapporterings- och granskningsändamål.

hello hot Intelligence rapporter visas i. PDF-format, som nås via en länk i hello **rapporter** i hello **misstänkta processen körs** bladet för varje säkerhetsvarning i Azure Security Center.

Mer information om hur tooview och använda hello hot Intelligence-rapporten finns [Azure Security Center hot Intelligence-rapporten.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

## <a name="next-steps"></a>Nästa steg:

[Komma igång med hello Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Vad är Log Analytics?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[Översikt över övervakning i Microsoft Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Introduktion toohello Azure-aktivitetsloggen (video)](https://azure.microsoft.com/resources/videos/intro-activity-log/)
