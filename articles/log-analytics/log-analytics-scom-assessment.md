---
title: "aaaOptimize din miljö för System Center Operations Manager med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda hello System Center Operations Manager Assessment lösning tooassess hello risk och hälsotillståndet för server-miljöer med regelbundna intervall."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c024e53826e91524c120bdb98ae7d96d6dc37d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-environment-with-hello-system-center-operations-manager-assessment-preview-solution"></a>Optimera din miljö med hello lösning för System Center Operations Manager Assessment (förhandsgranskning)

![System Center Operations Manager Assessment symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

Du kan använda hello System Center Operations Manager Assessment lösning tooassess hello risk och hälsotillståndet för ditt System Center Operations Manager server-miljöer med regelbundna intervall. Den här artikeln hjälper dig att installera, konfigurera och använda hello lösning så att du kan vidta åtgärder för möjliga problem.

Den här lösningen ger en prioriterad lista med rekommendationer för specifika tooyour distribuerad serverinfrastruktur. hello rekommendationer kategoriseras i fyra fokus områden som hjälper dig att snabbt förstå hello risk och vidta åtgärder.

hello rekommendationer baserat på hello kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök. Varje rekommendation vägledning om ett problem kan varför betydelse tooyou och hur tooimplement hello föreslagna ändringar.

Du kan välja fokusområden som är den viktigaste tooyour organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.

När du har lagt till hello lösning och är slutförd, Sammanfattning visas information för fokusområden på hello **System Center Operations Manager Assessment** instrumentpanel för din infrastruktur. hello följande avsnitt beskrivs hur toouse hello information om hello **System Center Operations Manager Assessment** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din SCOM-infrastruktur.

![System Center Operations Manager-lösningen sida vid sida](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager Assessment instrumentpanelen](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning

hello lösningen fungerar med Microsoft System Operations Manager 2012 R2 och 2012 SP1.

Använd följande information tooinstall hello och konfigurera hello lösning.

 - Innan du kan använda en lösning i OMS, måste du ha hello redan är installerad. Installera hello-lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) eller genom att följa instruktionerna hello i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

 - När du lägger till hello lösning toohello arbetsytan visar hello bedömning av System Center Operations Manager-panelen på instrumentpanelen för hello hello-meddelande krävs ytterligare konfiguration. Klicka på panelen hello och följ hello konfigurationssteg som nämns i hello sida

 ![System Center Operations Manager-instrumentpanelen](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 Konfigurationen av hello System Center Operations Manager kan göras via hello skript genom att följa anvisningarna i hello konfigurationssidan för hello lösning i OMS hello.

 I stället tooconfigure hello assessment via SCOM-konsolen, följ hello nedan stegen i hello samma ordning
1. [Ange hello-kör som-konto för System Center Operations Manager](#operations-manager-run-as-accounts-for-oms)  
2. [Konfigurera hello System Center Operations Manager Assessment regel](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>System Center Operations Manager assessment data samling information

hello System Center Operations Manager assessment samlar in WMI-data, registerdata, EventLog data, Operations Manager data via Windows PowerShell, SQL-frågor, filen information insamlaren med hello-server som du har aktiverat.

hello följande tabell visar metoder för insamling av data för System Center Operations Manager, och hur ofta data samlas in av en agent.

| Plattform | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | sju dagar |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager kör som-konton för OMS

OMS bygger på hanteringspaket för arbetsbelastningar tooprovide mervärde tjänster. Varje arbetsbelastning kräver belastningsspecifikt privilegier toorun hanteringspaket i en annan säkerhetskontext, till exempel ett domänkonto. Konfigurera en Operations Manager kör som-konto tooprovide autentiseringsuppgifter.

Använd följande information tooset hello Operations Manager-kör som-konto för System Center Operations Manager hello.

### <a name="set-hello-run-as-account"></a>Ange hello kör som-konto

1. Hello Operations Manager-konsolen, gå toohello **Administration** fliken.
2. Under hello **kör som-konfiguration**, klickar du på **konton**.
3. Skapa hello kör som-konto, följa via hello guiden skapar ett windowskonto. hello konto toouse är hello en identifieras och som har alla hello krav nedan:

    >[!NOTE]
    hello kör som-konto måste uppfylla följande krav:
    - En domänmedlem konto hello gruppen Administratörer på alla servrar i hello-miljö (alla Operations Manager-roller - hanteringsservern, OpsMgr-databasen, datalagret, rapportering, webbkonsolen, Gateway)
    - Åtgärden Managers administratörsroll för hello-hanteringsgrupp som ska utvärderas
    - Köra hello [skriptet](#sql-script-to-grant-granular-permissions-to-the-run-as-account) toogrant detaljerade behörigheter toohello konto för SQL-instans som används av Operations Manager.
      Obs: Om det här kontot har redan sysadmin-behörighet, går vidare hello skriptkörningen.

4. Under **Distributionssäkerhet**väljer **säkrare**.
5. Ange hello hanteringsserver där hello konto distribueras.
3. Gå tillbaka toohello kör som-konfiguration och klicka på **profiler**.
4. Sök efter hello *SCOM Assessment profil*.
5. hello namn bör vara: *Microsoft System Center Advisor SCOM Assessment kör som-profilen*.
6. Högerklicka på och uppdatera dess egenskaper och lägga till hello nyligen skapade kör som-konto du skapade i steg 3.

### <a name="sql-script-toogrant-granular-permissions-toohello-run-as-account"></a>SQL-skript toogrant detaljerade behörigheter toohello kör som-konto

Köra hello följande SQL-skript toogrant krävs behörigheter toohello kör som-konto på hello SQL-instans som används av Operations Manager.

```
-- Replace <UserName> with hello actual user name being used as Run As Account.
USE master

-- Create login for hello user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions toouser.
GRANT VIEW SERVER STATE too[UserName]
GRANT VIEW ANY DEFINITION too[UserName]
GRANT VIEW ANY DATABASE too[UserName]

-- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
-- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT too[UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace hello Operations Manager database name with hello one in your environment
Use [OperationsManager];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager DatawareHouse database name with hello one in your environment
Use [OperationsManagerDW];
GRANT SELECT too[UserName]
GO

--Replace hello Operations Manager Audit Collection database name with hello one in your environment
Use [OperationsManagerAC];
GRANT SELECT too[UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace hello Operations Manager database name with hello one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-hello-assessment-rule"></a>Konfigurera hello assessment regel

hello System Center Operations Manager Assessment lösningens management pack innehåller en regel med namnet *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln*. Den här regeln är ansvarig för att köra hello assessment. tooenable hello regeln och konfigurera hello frekvens, Använd hello procedurerna nedan.

Hello Microsoft System Center Advisor SCOM Assessment kör Assessment regeln är inaktiverad som standard. Du måste aktivera hello regeln på en hanteringsserver toorun hello assessment. Använd hello följande steg.

#### <a name="enable-hello-rule-for-a-specific-management-server"></a>Aktivera hello-regel för en viss hanteringsserver

1. I hello **redigering** arbetsytan hello Operations Manager-konsolen, Sök efter hello regeln *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln* i hello **regler** fönstret.
2. I sökresultaten hello hello väljer du en som innehåller hello text *typ: hanteringsservern*.
3. Högerklicka på hello regeln och klickar sedan på **åsidosätter** > **för ett specifikt objekt i klassen: hanteringsservern**.
4.  I hello tillgängliga listan med hanteringsservrar, väljer du hello hanteringsservern där hello regeln ska köras.
5.  Se till att du ändrar åsidosättningsvärde för**SANT** för hello **aktiverad** parametervärdet.  
    ![åsidosätt parametern](./media/log-analytics-scom-assessment/rule.png)

Konfigurera hello frekvensen av hello körs med hello nästa procedur fortfarande i det här fönstret.

#### <a name="configure-hello-run-frequency"></a>Konfigurera hello kör frekvens

hello assessment är standardintervallet för konfigurerade toorun var 10 080 minuter (eller sju dagar) hello. Du kan åsidosätta hello värdet tooa minimivärdet 1440 minuter (eller en dag). hello värdet representerar hello minsta lucka krävs mellan på varandra följande assessment körs. toooverride hello intervall, Använd hello stegen nedan.

1. I hello **redigering** arbetsytan hello Operations Manager-konsolen, Sök efter hello regeln *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln* i hello **regler** fönstret.
2. I sökresultaten hello hello väljer du en som innehåller hello text *typ: hanteringsservern*.
3. Högerklicka på hello regeln och klickar sedan på **åsidosättning hello regeln** > **för alla objekt i klassen: hanteringsservern**.
4. Ändra hello **intervall** parametern värdet tooyour önskad intervall. I hello exemplet nedan värdet hello too1440 minuter (en dag).  
    ![intervall för parametern](./media/log-analytics-scom-assessment/interval.png)  
    Om hello värdet tooless än 1 440 minuter, sedan hello regeln körs vid ett intervall med en dag. I det här exemplet hello regeln ignorerar hello intervallvärdet och körs med en frekvens som en dag.


## <a name="understanding-how-recommendations-are-prioritized"></a>Förstå hur rekommendationer prioriteras

Varje rekommendation är ett givet värde som identifierar hello relativa betydelse hello rekommendation. Endast hello 10 viktigaste rekommendationer visas.

### <a name="how-weights-are-calculated"></a>Hur vikterna beräknas

Viktningar är sammanställda värden som baseras på tre viktiga faktorer:

- Hej *sannolikhet* att ett problem som identifieras ska orsaka problem. En högre sannolikhet är lika tooa större hela poängen för hello rekommendation.
- Hej *inverkan* av hello problemet på din organisation om det uppstår ett problem. En högre inverkan är lika tooa större hela poängen för hello rekommendation.
- Hej *arbete* krävs tooimplement hello rekommendation. En högre prestanda är lika tooa mindre hela poängen för hello rekommendation.

hello viktning för varje rekommendation uttrycks i procent av hello sammanlagda poängen för varje fokusområde. Till exempel ökar implementera denna rekommendation om en rekommendation i hello tillgänglighet och affärskontinuitet Fokusområde har ett resultat på 5%, din övergripande tillgänglighet och affärskontinuitet poäng av 5%.

### <a name="focus-areas"></a>Fokusområden

**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.

**Prestanda och skalbarhet** -fokus visas här rekommendationer toohelp din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan toorespond toochanging infrastrukturen måste.

**Uppgradering och migrering distribution** – fokus visas här rekommendationer toohelp uppgradering, migrera och distribuera SQL Server tooyour befintlig infrastruktur.

**Åtgärder och övervakning** – fokus visas här rekommendationer toohelp förenklad IT-avdelningen implementera förebyggande underhåll och maximera prestanda.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Bör du sträva tooscore 100% överallt fokus i?

Inte nödvändigtvis. hello rekommendationer baserat på hello kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök. Inga två server-infrastrukturen är dock hello samma och specifika rekommendationer kan vara mer eller mindre relevanta tooyou. Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte är synliga toohello Internet. Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering. Problem som är viktiga tooa mogen företag kanske mindre viktiga tooa uppstart. Du kanske vill tooidentify vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.

Varje rekommendation innehåller information om varför det är viktigt. Använd den här vägledningen tooevaluate om implementera hello rekommendation är lämplig för dig, eftersom hello IT-tjänster och hello affärsbehoven i organisationen.

## <a name="use-assessment-focus-area-recommendations"></a>Använd assessment fokus området rekommendationer

Innan du kan använda en lösning i OMS, måste du ha hello redan är installerad. tooread mer om hur du installerar lösningar, se [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). När den har installerats ser du hello sammanfattning av rekommendationer med hjälp av hello bedömning av System Center Operations Manager-panelen på översiktssidan för hello i OMS.

Visa hello sammanfattas efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview rekommendationer för en fokus området och vidtar korrigeringsåtgärder

1. På hello **översikt** klickar du på hello **System Center Operations Manager Assessment** panelen.
2. På hello **System Center Operations Manager Assessment** granskar hello sammanfattningsinformation i ett hello fokus området blad och klicka sedan på ett tooview rekommendationer om fokus.
3. På någon av hello fokus Områdessidor, kan du visa hello prioriteras rekommendationer för din miljö. Klicka på en rekommendation enligt **påverkade objekt** tooview information om varför hello rekommendation.  
    ![Fokusområde](./media/log-analytics-scom-assessment/focus-area.png)
4. Du kan vidta åtgärder i **föreslagna åtgärder**. När hello objektet åtgärdats registrerar senare bedömningar den rekommenderade åtgärder som utförts och kompatibilitet poängen ökar. Korrigerade objekt visas som **skickades objekt**.

## <a name="ignore-recommendations"></a>Ignorera rekommendationer

Om du har rekommendationer som du vill tooignore, kan du skapa en textfil med OMS tooprevent rekommendationer visas i sökresultatet assessment.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-want-tooignore"></a>tooidentify rekommendationer som du vill tooignore

1. Logga in tooyour arbetsytan och öppna loggen sökning. Använd hello följande fråga toolist rekommendationer som har misslyckats för datorer i din miljö.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Här är en skärmbild som visar hello loggen sökfråga:  
    ![loggsökning](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Välj rekommendationer som du vill tooignore. Du ska använda hello värden för RecommendationId i hello nästa procedur.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate och använda en IgnoreRecommendations.txt textfil

1. Skapa en fil med namnet IgnoreRecommendations.txt.
2. Klistra in eller ange varje RecommendationId för varje rekommendation att OMS tooignore på en separat rad och spara och Stäng hello-filen.
3. Placera hello-filen i hello följande mapp på varje dator där du vill att OMS tooignore rekommendationer.
4. På hello hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify att rekommendationer ignoreras

1. När hello nästa schemalagda utvärdering körs som standard var sjunde dag angetts hello rekommendationer markeras ignoreras och visas inte på hello assessment instrumentpanelen.
2. Du kan använda hello följande loggen Sök frågor toolist alla hello ignoreras rekommendationer.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Om du senare vill toosee ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>System Center Operations Manager lösning vanliga frågor och svar

*Jag har lagt till hello assessment lösning toomy OMS-arbetsyta. Men du ser inte hello rekommendationer. Varför inte?* När du lägger till hello lösningen använder du hello följa steg visa hello rekommendationer på hello OMS-instrumentpanelen.  

- [Ange hello-kör som-konto för System Center Operations Manager](#operations-manager-run-as-accounts-for-oms)  
- [Konfigurera hello System Center Operations Manager Assessment regel](#configure-the-assessment-rule)


*Finns det ett sätt tooconfigure hur ofta hello assessment körs?* Ja. Se [konfigurera hello kör frekvens](#configure-the-run-frequency).

*Om en annan server identifieras när jag har lagt till hello System Center Operations Manager Assessment lösning, kommer utvärderas det?* Ja, efter identifiering, det bedöms fr.o.m--som standard var sjunde dag.

*Vad är hello process som hello datainsamling hello namn?* AdvisorAssessment.exe

*Där körs hello AdvisorAssessment.exe processen?* AdvisorAssessment.exe körs under hello HealthService för hello hanteringsservern där hello assessment regeln är aktiverad. Med den här processen uppnås identifiering av hela miljön genom insamling av fjärråtkomst.

*Hur lång tid tar det för insamling av data?* Insamling av data på hello servern tar ungefär en timme. Det kan ta längre tid i miljöer som har många instanser för Operations Manager eller databaser.

*Vad händer om jag in hello tidsintervall hello assessment tooless än 1 440 minuter?*  hello bedömning är förkonfigurerade toorun högst en gång per dag. Om du åsidosätter hello intervallvärdet värdet tooa mindre än 1 440 minuter använder hello assessment 1440 minuter som hello intervall.

*Hur tooknow om det finns fel som krävs?* Om hello assessment kördes och du inte ser resultaten, är det troligt att vissa av hello förutsättningar för hello misslyckades. Du kan köra frågor: `Type=Operation Solution=SCOMAssessment` och `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` misslyckades i loggen Sök toosee hello förutsättningar.

*Det finns en `Failed tooconnect toohello SQL Instance (….).` meddelandet i förutvärdering fel. Vad är hello problemet?* AdvisorAssessment.exe hello process som samlar in data, körs under hello HealthService av hello management server. Som en del av hello assessment försöker hello processen tooconnect toohello SQL Server där hello Operations Manager-databasen finns. Det här felet kan inträffa när brandväggsregler blockera hello anslutning toohello SQL Server-instansen.

*Vilken typ av data som samlas in?*  hello följande typer av data har samlats in: - WMI-data – data - EventLog-data – Operations Manager registerdata via Windows PowerShell, SQL-frågor och filen information insamlaren.

*Varför måste jag tooconfigure en Kör som-konto?* Olika SQL-frågor körs för en Operations Manager-server. För att de toorun, måste du använda ett kör som-konto med tillräcklig behörighet. Dessutom har lokal administratörsbehörighet krävs tooquery WMI.

*Varför visas endast hello topp 10 rekommendationer?* I stället för att ge dig en fullständig, överväldigande lista över aktiviteter, rekommenderar vi att du fokusera på adressering hello prioriteras rekommendationer först. När du åtgärda dem blir ytterligare rekommendationer tillgängliga. Om du föredrar toosee hello detaljerad lista kan du visa alla rekommendationer loggen sökning.

*Finns det ett sätt tooignore en rekommendation?* Ja, se hello [Ignorera rekommendationer](#Ignore-recommendations).


## <a name="next-steps"></a>Nästa steg

- [Söka i loggar](log-analytics-log-searches.md) tooview detaljerad data för System Center Operations Manager bedömning och rekommendationer.
