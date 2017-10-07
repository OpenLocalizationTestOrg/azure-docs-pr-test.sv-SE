---
title: "aaaOptimize din SQL Server-miljö med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda hello SQL-bedömning lösning tooassess hello risk och hälsotillståndet för din SQL server-miljöer med regelbundna intervall med Azure logganalys."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a>Optimera SQL Server-miljön med hello SQL lösning i logganalys

![SQL-bedömning symbol](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

Du kan använda hello SQL-bedömning lösning tooassess hello risk och hälsotillståndet för server-miljöer med regelbundna intervall. Den här artikeln hjälper dig att installera hello lösning så att du kan vidta åtgärder för möjliga problem.

Den här lösningen ger en prioriterad lista med rekommendationer för specifika tooyour distribuerad serverinfrastruktur. hello rekommendationer kategoriseras i sex fokus områden som hjälper dig att snabbt förstå hello risk och vidta åtgärder.

hello rekommendationer baserat på hello kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök. Varje rekommendation vägledning om ett problem kan varför betydelse tooyou och hur tooimplement hello föreslagna ändringar.

Du kan välja fokusområden som är den viktigaste tooyour organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.

När du har lagt till hello lösning och är slutförd, Sammanfattning visas information för fokusområden på hello **SQL-bedömning** instrumentpanelen för hello infrastrukturen i din miljö. hello följande avsnitt beskrivs hur toouse hello information om hello **SQL-bedömning** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din SQL server-infrastrukturen.

![Bild av SQL-bedömning sida vid sida](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Bild av SQL-bedömning instrumentpanelen](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
SQL-bedömning fungerar med alla versioner av SQL Server för hello Standard, Developer eller Enterprise Edition stöds för närvarande.

Använd följande information tooinstall hello och konfigurera hello lösning.

* Agenterna måste installeras på servrar som har installerat SQL Server.
* hello SQL lösning kräver en version av .NET Framework 4 installeras på varje dator som har en OMS-agent som stöds.
* I ordning tooinstall hello lösning måste hello användaren vara en administratör eller deltagare toohello Azure-prenumeration när med hello Azure-portalen. Hello användaren måste dessutom vara Rollmedlem hello OMS arbetsytan deltagare eller administratör i hello OMS-portalen.
* När du använder hello Operations Manager-agenten med SQL-bedömning behöver toouse ett Run-As för Operations Manager-konto. Se [Operations Manager kör som-konton för OMS](#operations-manager-run-as-accounts-for-oms) nedan för mer information.

  > [!NOTE]
  > hello MMA agenten stöder inte Run-As för Operations Manager-konton.
  >
  >
* Lägg till hello SQL-bedömning lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). Det krävs ingen ytterligare konfiguration.

> [!NOTE]
> När du har lagt till hello lösning läggs hello AdvisorAssessment.exe filen tooservers med agenter. Konfigurationsdata Läs och skickas sedan toohello OMS-tjänsten i hello molnet för bearbetning. Logik är tillämpade toohello mottagna data och hello Molntjänsten innehåller hello-data.

## <a name="sql-assessment-data-collection-details"></a>SQL-bedömning information för samlingen
SQL-bedömning samlar in WMI-data, registerdata, prestandadata och SQL Server hantering av dynamiska visa resultat med hjälp av hello agenter som du har aktiverat.

hello följande tabell visas data collection metoder för agenter, om Operations Manager (SCOM) krävs och hur ofta data samlas in av en agent.

| Plattform | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 dagar |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager kör som-konton för OMS
Logganalys i OMS använder hello Operations Manager-agenten och hantering av grupp toocollect och skicka data toohello OMS-tjänsten. OMS-versioner av hanteringspaket för arbetsbelastningar tooprovide mervärde tjänster. Varje arbetsbelastning kräver belastningsspecifikt privilegier toorun hanteringspaket i en annan säkerhetskontext, till exempel ett domänkonto. Du måste tooprovide autentiseringsuppgifter genom att konfigurera en Operations Manager kör som-konto.

Använd följande information tooset hello Operations Manager kör som-konto för SQL-bedömning hello.

### <a name="set-hello-run-as-account-for-sql-assessment"></a>Ange hello kör som-konto för SQL-bedömning
 Om du redan använder hello hanteringspaket för SQL Server, bör du använda Kör som-kontot.

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a>tooconfigure hello SQL kör som-konto i hello Operations-konsolen
> [!NOTE]
> Om du använder hello OMS direkt agent, i stället för hello SCOM-agent, hello hanteringspaket körs alltid i hello säkerhetskontexten för hello kontot Lokalt System. Hoppa över steg 1 – 5 nedan och kör hello antingen T-SQL eller Powershell exemplet anger att NT AUTHORITY\SYSTEM hello användarnamn.
>
>

1. Öppna hello Operations-konsolen i Operations Manager, och klicka sedan på **Administration**.
2. Under **kör som-konfiguration**, klickar du på **profiler**, och öppna **OMS SQL Assessment kör som-profilen**.
3. På hello **kör som-konton** klickar du på **Lägg till**.
4. Välj en Windows kör som-konto som innehåller hello autentiseringsuppgifter krävs för SQL Server, eller klicka på **ny** toocreate en.

   > [!NOTE]
   > hello kör som-kontotyp måste vara Windows. hello kör som-kontot måste också ingå i gruppen lokala administratörer på alla Windows-servrar som är värd för SQL Server-instanser.
   >
   >
5. Klicka på **Spara**.
6. Ändra och kör följande T-SQL-exempel på varje krävs tooRun för SQL Server-instans toogrant minsta möjliga behörigheter på som-konto tooperform SQL-bedömning hello. Dock behöver du inte toodo det om ett kör som-konto redan ingår i serverrollen för hello sysadmin på SQL Server-instanser.

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a>tooconfigure hello SQL kör som-konto med hjälp av Windows PowerShell
Öppna ett PowerShell-fönster och kör följande skript när du har uppdaterat den med din information hello:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Förstå hur rekommendationer prioriteras
Varje rekommendation är ett givet värde som identifierar hello relativa betydelse hello rekommendation. Endast hello tio viktigaste rekommendationer visas.

### <a name="how-weights-are-calculated"></a>Hur vikterna beräknas
Viktningar är sammanställda värden som baseras på tre viktiga faktorer:

* Hej *sannolikhet* att ett problem som identifieras ska orsaka problem. En högre sannolikhet är lika tooa större hela poängen för hello rekommendation.
* Hej *inverkan* av hello problemet på din organisation om det uppstår ett problem. En högre inverkan är lika tooa större hela poängen för hello rekommendation.
* Hej *arbete* krävs tooimplement hello rekommendation. En högre prestanda är lika tooa mindre hela poängen för hello rekommendation.

hello viktning för varje rekommendation uttrycks i procent av hello sammanlagda poängen för varje fokusområde. Till exempel om en rekommendation i hello säkerhet och efterlevnad Fokusområde har ett resultat på 5%, ökar implementera denna rekommendation din övergripande säkerhet och efterlevnad poäng av 5%.

### <a name="focus-areas"></a>Fokusområden
**Säkerhet och efterlevnad** -fokus visas här rekommendationer för potentiella säkerhetshot och överträdelser, företagets principer och krav på teknisk, juridisk och regelmässig efterlevnad.

**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.

**Prestanda och skalbarhet** -fokus visas här rekommendationer toohelp din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan toorespond toochanging infrastrukturen måste.

**Uppgradera migrering och distribution** – fokus visas här rekommendationer toohelp uppgradering, migrera och distribuera SQL Server tooyour befintlig infrastruktur.

**Åtgärder och övervakning** – fokus visas här rekommendationer toohelp förenklad IT-avdelningen implementera förebyggande underhåll och maximera prestanda.

**Ändrings- och konfigurationshantering** -fokus visas här rekommendationer toohelp skydda dagliga uppgifter, se till att ändringar inte negativt påverka din infrastruktur, upprätta ändringskontroll och tootrack och granska systemkonfigurationer.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Bör du sträva tooscore 100% överallt fokus i?
Inte nödvändigtvis. hello rekommendationer baserat på hello kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök. Inga två server-infrastrukturen är dock hello samma och specifika rekommendationer kan vara mer eller mindre relevanta tooyou. Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte är synliga toohello Internet. Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering. Problem som är viktiga tooa mogen företag kanske mindre viktiga tooa uppstart. Du kanske vill tooidentify vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.

Varje rekommendation innehåller information om varför det är viktigt. Du bör använda den här vägledningen tooevaluate om implementera hello rekommendation är lämplig för dig, eftersom hello IT-tjänster och hello affärsbehoven i organisationen.

## <a name="use-assessment-focus-area-recommendations"></a>Använd assessment fokus området rekommendationer
Innan du kan använda en lösning i OMS, måste du ha hello redan är installerad. tooread mer om hur du installerar lösningar, se [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). När den har installerats ser du hello sammanfattning av rekommendationer med hjälp av hello SQL-bedömning panelen på översiktssidan för hello i OMS.

Visa hello sammanfattas efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview rekommendationer för en fokus området och vidtar korrigeringsåtgärder
1. På hello **översikt** klickar du på hello **SQL-bedömning** panelen.
2. På hello **SQL-bedömning** granskar hello sammanfattningsinformation i ett hello fokus området blad och klicka sedan på ett tooview rekommendationer om fokus.
3. På någon av hello fokus Områdessidor, kan du visa hello prioriteras rekommendationer för din miljö. Klicka på en rekommendation enligt **påverkade objekt** tooview information om varför hello rekommendation.  
    ![Bild av SQL-bedömning rekommendationer](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Du kan vidta åtgärder i **föreslagna åtgärder**. När hello objektet åtgärdats registrerar senare bedömningar den rekommenderade åtgärder som utförts och kompatibilitet poängen ökar. Korrigerade objekt visas som **skickades objekt**.

## <a name="ignore-recommendations"></a>Ignorera rekommendationer
Om du har rekommendationer som du vill tooignore kan skapa du en textfil som OMS använder tooprevent rekommendationer visas i sökresultatet assessment.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>tooidentify rekommendationer som kommer att ignorera
1. Logga in tooyour arbetsytan och öppna loggen sökning. Använd hello följande fråga toolist rekommendationer som har misslyckats för datorer i din miljö.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Här är en skärmbild som visar hello loggen sökfråga: ![misslyckades rekommendationer](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Välj rekommendationer som du vill tooignore. Du ska använda hello värden för RecommendationId i hello nästa procedur.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate och använda en IgnoreRecommendations.txt textfil
1. Skapa en fil med namnet IgnoreRecommendations.txt.
2. Klistra in eller ange varje RecommendationId för varje rekommendation att OMS tooignore på en separat rad och spara och Stäng hello-filen.
3. Placera hello-filen i hello följande mapp på varje dator där du vill att OMS tooignore rekommendationer.
   * På datorer med hello Microsoft Monitoring Agent (ansluten direkt eller via Operations Manager) - *SystemDrive*: \Program\Microsoft övervakning Agent\Agent
   * På hello hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify att rekommendationer ignoreras
1. När hello nästa schemalagda utvärdering körs som standard var sjunde dag angetts hello rekommendationer markeras ignoreras och visas inte på hello assessment instrumentpanelen.
2. Du kan använda hello följande loggen Sök frågor toolist alla hello ignoreras rekommendationer.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Om du senare vill toosee ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.

## <a name="sql-assessment-solution-faq"></a>SQL lösning vanliga frågor och svar
*Hur ofta körs en utvärdering?*

* hello assessment körs var sjunde dag.

*Finns det ett sätt tooconfigure hur ofta hello assessment körs?*

* Inte just nu.

*Om en annan server identifieras när jag har lagt till hello SQL lösning, kommer utvärderas det?*

* Ja, när det upptäcks att det bedöms från sedan, var sjunde dag.

*Om en server tas ur drift, när den tas bort från hello assessment?*

* Om en server inte lämnar data 3 veckor tas bort.

*Vad är hello process som hello datainsamling hello namn?*

* AdvisorAssessment.exe

*Hur lång tid tar det för toobe för data som samlas in?*

* hello faktiska datainsamling på hello servern tar ungefär en timme. Det kan ta längre tid på servrar som har ett stort antal databaser eller SQL-instanser.

*Vilken typ av data som samlas in?*

* hello följande typer av data samlas in:
  * WMI
  * Registret
  * Prestandaräknare
  * SQL dynamiska hanteringsvyer (DMV).

*Finns det en tooconfigure sätt när data samlas in?*

* Inte just nu.

*Varför måste jag tooconfigure en Kör som-konto?*

* För SQL Server körs ett litet antal SQL-frågor. För att de toorun en Kör som-konto med VIEW SERVER STATE behörigheter tooSQL måste användas.  Dessutom i ordning tooquery WMI krävs lokal administratörsbehörighet.

*Varför visas endast hello topp 10 rekommendationer?*

* I stället för att ge dig en överväldigande uttömmande förteckning över aktiviteter, rekommenderar vi att du fokusera på adressering hello prioriteras rekommendationer först. När du åtgärda dem blir ytterligare rekommendationer tillgängliga. Om du föredrar toosee hello detaljerad lista kan du visa alla rekommendationer hello OMS loggen sökning.

*Finns det ett sätt tooignore en rekommendation?*

* Ja, se [Ignorera rekommendationer](#ignore-recommendations) ovan.

## <a name="next-steps"></a>Nästa steg
* [Söka i loggar](log-analytics-log-searches.md) tooview detaljerade SQL-bedömning data och rekommendationer.
