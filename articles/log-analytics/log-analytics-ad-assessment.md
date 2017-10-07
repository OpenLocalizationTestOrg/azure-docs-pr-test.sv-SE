---
title: "aaaOptimize Active Directory-miljön med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda hello Active Directory Assessment lösning tooassess hello risk och hälsotillståndet för server-miljöer med regelbundna intervall."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 63290d95302a9e1d243cd993ac50556ed42b97bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-active-directory-environment-with-hello-active-directory-assessment-solution-in-log-analytics"></a>Optimera din Active Directory-miljö med hello Active Directory lösning i logganalys

![AD-bedömning symbol](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Du kan använda hello Active Directory Assessment lösning tooassess hello risk och hälsotillståndet för server-miljöer med regelbundna intervall. Den här artikeln hjälper dig att installera och använda hello lösning så att du kan vidta åtgärder för möjliga problem.

Den här lösningen ger en prioriterad lista med rekommendationer för specifika tooyour distribuerad serverinfrastruktur. hello rekommendationer kategoriseras i fyra fokus områden som hjälper dig att snabbt förstå hello risk och vidta lämpliga åtgärder.

hello rekommendationer baserat på hello kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök. Varje rekommendation vägledning om ett problem kan varför betydelse tooyou och hur tooimplement hello föreslagna ändringar.

Du kan välja fokusområden som är den viktigaste tooyour organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.

När du har lagt till hello lösning och är slutförd, Sammanfattning visas information för fokusområden på hello **AD-bedömning** instrumentpanelen för hello infrastrukturen i din miljö. hello följande avsnitt beskrivs hur toouse hello information om hello **AD-bedömning** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din serverinfrastruktur för Active Directory.

![Bild av SQL-bedömning sida vid sida](./media/log-analytics-ad-assessment/ad-tile.png)

![Bild av SQL-bedömning instrumentpanelen](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösningar.

* Agenterna måste installeras på domänkontrollanter som är medlemmar i hello domän toobe utvärderas.
* hello Active Directory Assessment lösning kräver en version som stöds av .NET Framework 4 (4.5.2 eller senare) installerat på varje dator som har en OMS-agent.
* Lägg till hello Active Directory Assessment lösning tooyour OMS-arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).  Det krävs ingen ytterligare konfiguration.

  > [!NOTE]
  > När du har lagt till hello lösning läggs hello AdvisorAssessment.exe filen tooservers med agenter. Konfigurationsdata Läs och skickas sedan toohello OMS-tjänsten i hello molnet för bearbetning. Logik är tillämpade toohello mottagna data och hello Molntjänsten innehåller hello-data.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory Assessment samling information

Active Directory-bedömning samlar in data från följande källor med hello agenter som du har aktiverat hello:

- Insamlare för registret
- LDAP-insamlare
- .NET framework
- Händelsen logginsamlare
- Active Directory Service interfaces (ADSI)
- Windows PowerShell
- Filen datainsamlare
- Windows Management Instrumentation (WMI)
- DCDIAG verktyget API
- File Replication Service (NTFRS) API
- Anpassade C#-kod


hello följande tabell visas data collection metoder för agenter, om Operations Manager (SCOM) krävs och hur ofta data samlas in av en agent.

| Plattform | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 dagar |

## <a name="understanding-how-recommendations-are-prioritized"></a>Förstå hur rekommendationer prioriteras
Varje rekommendation är ett givet värde som identifierar hello relativa betydelse hello rekommendation. Endast hello 10 viktigaste rekommendationer visas.

### <a name="how-weights-are-calculated"></a>Hur vikterna beräknas
Viktningar är sammanställda värden som baseras på tre viktiga faktorer:

* Hej *sannolikhet* att ett problem som identifieras orsakar problem. En högre sannolikhet är lika tooa större hela poängen för hello rekommendation.
* Hej *inverkan* av hello problemet på din organisation om det uppstår ett problem. En högre inverkan är lika tooa större hela poängen för hello rekommendation.
* Hej *arbete* krävs tooimplement hello rekommendation. En högre prestanda är lika tooa mindre hela poängen för hello rekommendation.

hello viktning för varje rekommendation uttrycks i procent av hello sammanlagda poängen för varje fokusområde. Till exempel ökar implementera denna rekommendation om en rekommendation i hello säkerhet och efterlevnad Fokusområde har ett resultat på 5%, din övergripande säkerhet och efterlevnad poäng av 5%.

### <a name="focus-areas"></a>Fokusområden
**Säkerhet och efterlevnad** -fokus visas här rekommendationer för potentiella säkerhetshot och överträdelser, företagets principer och krav på teknisk, juridisk och regelmässig efterlevnad.

**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.

**Prestanda och skalbarhet** -fokus visas här rekommendationer toohelp din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan toorespond toochanging infrastrukturen måste.

**Uppgradera migrering och distribution** – fokus visas här rekommendationer toohelp uppgradering, migrera och distribuera Active Directory tooyour befintlig infrastruktur.

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a>Bör du sträva tooscore 100% överallt fokus i?
Inte nödvändigtvis. hello rekommendationer baserat på hello kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök. Inga två server-infrastrukturen är dock hello samma och specifika rekommendationer kan vara mer eller mindre relevanta tooyou. Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte är synliga toohello Internet. Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering. Problem som är viktiga tooa mogen företag kanske mindre viktiga tooa uppstart. Du kanske vill tooidentify vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.

Varje rekommendation innehåller information om varför det är viktigt. Du bör använda den här vägledningen tooevaluate om implementera hello rekommendation är lämplig för dig, eftersom hello IT-tjänster och hello affärsbehoven i organisationen.

## <a name="use-assessment-focus-area-recommendations"></a>Använd assessment fokus området rekommendationer
Innan du kan använda en lösning i OMS, måste du ha hello redan är installerad. tooread mer om hur du installerar lösningar, se [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). När den har installerats kan du visa hello sammanfattning av rekommendationer med hjälp hello Assessment panelen på översiktssidan för hello i OMS.

Visa hello sammanfattas efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a>tooview rekommendationer för en fokus området och vidtar korrigeringsåtgärder
1. På hello **översikt** klickar du på hello **Assessment** panelen för din serverinfrastruktur.
2. På hello **Assessment** granskar hello sammanfattningsinformation i ett hello fokus området blad och klicka sedan på ett tooview rekommendationer om fokus.
3. På någon av hello fokus Områdessidor, kan du visa hello prioriteras rekommendationer för din miljö. Klicka på en rekommendation enligt **påverkade objekt** tooview information om varför hello rekommendation.  
    ![Bild av Assessment rekommendationer](./media/log-analytics-ad-assessment/ad-focus.png)
4. Du kan vidta åtgärder i **föreslagna åtgärder**. När hello objektet åtgärdats senare bedömningar poster som rekommenderade åtgärder som utförts och kompatibilitet poängen ökar. Korrigerade objekt visas som **skickades objekt**.

## <a name="ignore-recommendations"></a>Ignorera rekommendationer
Om du har rekommendationer som du vill tooignore kan skapa du en textfil som OMS använder tooprevent rekommendationer visas i sökresultatet assessment.

### <a name="tooidentify-recommendations-that-you-will-ignore"></a>tooidentify rekommendationer som kommer att ignorera
1. Logga in tooyour arbetsytan och öppna loggen sökning. Använd hello följande fråga toolist rekommendationer som har misslyckats för datorer i din miljö.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Här är en skärmbild som visar hello loggen sökfråga: ![misslyckades rekommendationer](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Välj rekommendationer som du vill tooignore. Du ska använda hello värden för RecommendationId i hello nästa procedur.

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a>toocreate och använda en IgnoreRecommendations.txt textfil
1. Skapa en fil med namnet IgnoreRecommendations.txt.
2. Klistra in eller ange varje RecommendationId för varje rekommendation att logganalys tooignore på en separat rad och spara och Stäng hello-filen.
3. Placera hello-filen i hello följande mapp på varje dator där du vill att OMS tooignore rekommendationer.
   * På datorer med hello Microsoft Monitoring Agent (ansluten direkt eller via Operations Manager) - *SystemDrive*: \Program\Microsoft övervakning Agent\Agent
   * På hello hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="tooverify-that-recommendations-are-ignored"></a>tooverify att rekommendationer ignoreras
När hello nästa schemalagda utvärdering körs som standard var sjunde dag hello angetts rekommendationer markeras *ignorerade* och visas inte på hello assessment instrumentpanel.

1. Du kan använda hello följande loggen Sök frågor toolist alla hello ignoreras rekommendationer.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Om du senare vill toosee ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.

## <a name="ad-assessment-solutions-faq"></a>Lösningar för AD-bedömning vanliga frågor och svar
*Hur ofta körs en utvärdering?*

* hello assessment körs var sjunde dag.

*Finns det ett sätt tooconfigure hur ofta hello assessment körs?*

* Inte just nu.

*Om en annan server för upptäcks när jag har lagt till en lösning, används den för att bedöma?*

* Ja, när det upptäcks att det bedöms från sedan, var sjunde dag.

*Om en server tas ur drift, när den tas bort från hello assessment?*

* Om en server inte lämnar data 3 veckor tas bort.

*Vad är hello process som hello datainsamling hello namn?*

* AdvisorAssessment.exe

*Hur lång tid tar det för toobe för data som samlas in?*

* hello faktiska datainsamling på hello servern tar ungefär en timme. Det kan ta längre tid på servrar som har ett stort antal Active Directory-servrar.

*Finns det en tooconfigure sätt när data samlas in?*

* Inte just nu.

*Varför visas endast hello topp 10 rekommendationer?*

* I stället för att ge dig en överväldigande uttömmande förteckning över aktiviteter, rekommenderar vi att du fokusera på adressering hello prioriteras rekommendationer först. När du åtgärda dem blir ytterligare rekommendationer tillgängliga. Om du föredrar toosee hello detaljerad lista kan du visa alla rekommendationer loggen sökning.

*Finns det ett sätt tooignore en rekommendation?*

* Ja, se [Ignorera rekommendationer](#ignore-recommendations) ovan.

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerad data för AD-bedömning och rekommendationer.
