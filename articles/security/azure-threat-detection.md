---
title: aaaAzure avancerade Hotidentifiering | Microsoft Docs
description: "Läs mer om identitetsskydd och dess funktioner."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 63e2c541fd4ce2c571af9d5845c9a9bd4b03b2a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-advanced-threat-detection"></a>Azure avancerade Hotidentifiering
## <a name="introduction"></a>Introduktion

### <a name="overview"></a>Översikt

Microsoft har utvecklat en serie faktablad, säkerhet översikter, bästa praxis och checklistor tooassist Azure kunder om hello olika säkerhetsrelaterade funktioner som är tillgängliga i och omgivande hello Azure-plattformen. Hej avsnitt angivet bredd och djup och uppdateras regelbundet. Det här dokumentet är en del av den serien som sammanfattas i hello efter abstrakt avsnittet.

### <a name="azure-platform"></a>Azure-plattformen

Azure är en offentlig molntjänstplattform som stöder hello stort urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter.
Den stöder hello följande programmeringsspråk:
-   Kör Linux behållare med Docker-integration.
-   Skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js
-   Build-servrar för iOS, Android och Windows enheter.

Offentliga Azure-molnet och support hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du migrerar tooa offentligt moln med en organisation som organisationen är ansvarig tooprotect dina data och ger säkerhet och styrning runt hello system.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag uppfyller deras behov. Azure tillhandahåller en mängd olika alternativ tooconfigure och anpassa toomeet hello säkerhetskrav din appdistribution. Det här dokumentet hjälper dig att möta dessa krav.

### <a name="abstract"></a>Abstrakt

Microsoft Azure erbjudanden som har skapats i avancerade threat detection funktioner med hjälp av tjänster som Azure Active Directory, Azure Operations Management Suite (OMS) och Azure Security Center. Den här samlingen av tjänster och funktioner ger ett enkelt och snabbt sätt toounderstand vad som händer i din Azure-distributioner.

I det här dokumentet hjälper dig hello ”Microsoft Azure metoder” mot hot säkerhetsproblem diagnostik och analysera hello risker som är associerade med hello skadliga aktiviteter som riktar sig mot servrar och andra Azure-resurser. Det hjälper dig att tooidentify hello metoder för identifiering och hantering av säkerhetsproblem med optimerad lösningar genom hello Azure-plattformen och kunden säkerhet riktade och tekniker.

I det här dokumentet fokuserar på hello teknik för Azure-plattformen och kunden vända kontroller och försöker inte tooaddress SLA: er priser modeller och DevOps överväganden.

## <a name="azure-active-directory-identity-protection"></a>Identitetsskydd för Azure Active Directory

![Identitetsskydd för Azure Active Directory](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Azure Active Directory-identitetsskydd](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) är en funktion i hello [Azure AD Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edition som ger dig en översikt över hello riskhändelser och potentiella sårbarheter som påverkar din organisation identiteter. Microsoft har skydda molnbaserade identiteter för över tio år och med Azure AD Identity Protection Microsoft göra skydd datorerna tillgängliga tooenterprise kunder. Identity Protection använder befintliga Azure ADS avvikelseidentifiering identifiering funktioner tillgängliga via [Azure AD avvikande aktivitetsrapporter](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports), och ger nya risk händelsetyper som kan identifiera avvikelser i realtid.

Identity Protection använder anpassningsbar maskininlärningsalgoritmer och heuristik toodetect avvikelser och riskhändelser som kan indikera att en identitet har komprometterats. Med dessa data kan identitetsskydd genererar rapporter och aviseringar som gör att du tooinvestigate händelserna risk och vidta lämpliga åtgärder för reparation eller lösning.

Men Azure Active Directory Identity Protection är mer än ett verktyg för övervakning och rapportering. Baserat på riskhändelser identitetsskydd beräknar en användare risknivå för varje användare, vilket gör att du tooconfigure risk-baserade policys tooautomatically skydda hello identiteter i din organisation.

Dessa risk-baserade principer i tillägg tooother [villkorlig åtkomstkontroll](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) tillhandahålls av Azure Active Directory och [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), kan automatiskt blockera eller erbjuda anpassade åtgärder som omfattar lösenordsåterställning och multifaktorautentisering tvingande.

### <a name="identity-protections-capabilities"></a>Identity Protection funktioner

Azure Active Directory Identity Protection är mer än en övervakning och rapportering av verktyget. tooprotect din organisations identiteter som du kan konfigurera risk-baserade principer som svarar toodetected problem automatiskt när en angiven risknivå har uppnåtts. Dessa principer dessutom tooother villkorlig åtkomst till kontroller som tillhandahålls av Azure Active Directory och EMS, kan blockera automatiskt eller initiera anpassningsbar reparationsåtgärder inklusive lösenordsåterställning och multifaktorautentisering tvingande.

Exempel på hello sätt som Azure Identity Protection hjälper dig skydda dina konton och identiteter inkluderar:

[Identifiering av riskhändelser och riskfyllda konton:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Identifiera händelsetyper sex risken med machine learning och heuristisk regler
-   Beräkning av risknivåer för användaren
-   Att tillhandahålla anpassade rekommendationer tooimprove övergripande säkerhetstillståndet genom att markera säkerhetsrisker

[Undersöka riskhändelser:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Skicka meddelanden för riskhändelser
-   Undersöka riskhändelser med hjälp av relevanta och sammanhangsberoende information
-   Tillhandahåller grundläggande arbetsflöden tootrack undersökningar
-   Att ge dig tillgång till tooremediation åtgärder, till exempel återställning av lösenord

[Principer för risk-baserad villkorlig åtkomst:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Principen toomitigate riskfyllda inloggningar genom att blockera inloggningar eller att kräva multifaktorautentisering utmaningar.
-   Principen tooblock eller säker riskfyllda användarkonton
-   Principen toorequire användare tooregister för multifaktorautentisering

### <a name="azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM)

Med [Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

Du kan hantera, styr och övervaka åtkomst inom din organisation. Detta inkluderar åtkomst tooresources i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.

Azure AD Privileged Identity Management kan du:

-   Få en avisering och rapportera om Azure AD-administratörer och just-in-time ”administrativ åtkomst tooMicrosoft onlinetjänster som Office 365 och Intune

-   Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar

-   Få meddelanden om åtkomst tooa Privilegierade roller

## <a name="microsoft-operations-management-suite-oms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur. Eftersom OMS implementeras som en molnbaserad tjänst kan du snabbt komma igång med minimal investering i infrastrukturtjänster. Nya säkerhetsfunktioner levereras automatiskt, sparar din löpande underhållet och uppgradera kostnader.

Dessutom tooproviding värdefulla tjänster på egen hand, OMS kan integreras med System Center-komponenter som [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) tooextend din befintliga security management investeringar i hello molnet. System Center och OMS kan fungera tillsammans tooprovide fullständig hybridhantering upplevelse.

### <a name="holistic-security-and-compliance-posture"></a>Heltäckande säkerhet och efterlevnad Hållningsdata

Hej [OMS säkerhet och granska instrumentpanelen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) tillhandahåller en heltäckande översikt över din organisations IT säkerhetstillståndet med inbyggda sökfrågor för anmärkningsvärda problem som kräver din uppmärksamhet. hello säkerhet och granska instrumentpanelen är hello startsidan för allt relaterade toosecurity i OMS. Det ger en övergripande inblick i hello säkerhetsstatus för dina datorer. Den omfattar också hello möjlighet tooview alla händelser från hello senaste 24 timmarna, 7 dagar eller andra anpassade tidsintervall.

OMS instrumentpaneler hjälper dig snabbt och enkelt att förstå hello övergripande säkerhetsläget i en miljö inuti hello kontexten för IT-avdelningen, inklusive: utvärdering av uppdateringar för programvara, program mot skadlig kod bedömning och konfigurationsbaslinjer. Loggdata för säkerhet är dessutom lätt tillgängliga toostreamline hello säkerhet och efterlevnad gransknings-och processer.

hello OMS säkerhet och granska instrumentpanelen är organiserat i fyra huvudsakliga kategorier:

![Instrumentpanelen för Säkerhet och granskning i OMS](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Säkerhetsdomäner:** i detta område, kommer du att kunna toofurther utforska säkerhetsposter över tid, gå till kontroll av skadlig kod, uppdatera assessment, nätverkssäkerhet, identitet och åtkomst, datorer med säkerhetshändelser och snabbt har instrumentpanelen för tooAzure Security Center.

-   **Viktiga problem:** det här alternativet kan tooquickly identifiera hello antal aktiva problem och hello allvarlighetsgrad för de här problemen.

-   **Identifieringar (förhandsversion):** aktiverar du tooidentify angreppsmönster genom att visualisera säkerhetsaviseringar som de ske mot dina resurser.

-   **Hotinformation:** aktiverar du tooidentify angreppsmönster genom att visualisera hello Totalt antal servrar med utgående skadlig IP-trafik, hello skadliga hot typ och en karta som visar var dessa IP-adresser kommer från.

-   **Vanliga säkerhetsfrågor:** det här alternativet får du en lista över de vanligaste hello-säkerhet frågor som du kan använda toomonitor din miljö. När du klickar på någon av dessa frågor öppnas hello Sök bladet med hello resultat för frågan.

### <a name="insight-and-analytics"></a>Insikter och analys
Hello i mitten av [logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) är hello OMS-lagringsplatsen, som är värd för hello Azure-molnet.

![Insikter och analys](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Data samlas in hello databasen från anslutna källor genom att konfigurera datakällor och lägga till lösningar tooyour prenumeration.

![prenumeration](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Datakällor och lösningar ska skapa olika posttyper som har en egen uppsättning egenskaper, men kan fortfarande analyseras tillsammans i frågor toohello databas. Detta ger dig toouse hello samma verktyg och metoder toowork med olika typer av data som samlas in av olika källor.


De flesta interaktionen med logganalys är hello OMS-portalen, som körs i en webbläsare och ger dig åtkomst tooconfiguration inställningar och flera verktyg tooanalyze och agera på insamlade data. Från hello-portalen kan du använda [logga sökningar](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) där du skapar frågor tooanalyze insamlade data [instrumentpaneler](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards), som du kan anpassa med grafiska vyer av dina mest värdefulla sökningar och [lösningar](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), som ger ytterligare funktioner och analys verktyg.

![Verktyg för analys](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Lösningar för att lägga till funktioner tooLog Analytics. De främst köras i hello molntjänster och ger analys av data som samlas in i hello OMS-databas. De kan också definiera nya posttyper toobe samlas in som kan analyseras med loggen sökningar eller genom ytterligare användargränssnittet i hello lösning i hello OMS-instrumentpanelen.
hello säkerhet och granska är ett exempel på dessa typer av lösningar.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Automation- och kontrollservern: avisering på säkerhetskonfiguration drifts

Azure Automation automatiserar administrativa processer med runbooks som är baserade på PowerShell och kör i hello Azure-molnet. Runbooks kan också köras på en server i dina lokala data center toomanage lokala resurser. Azure Automation ger konfigurationshantering med PowerShell DSC (Desired State Configuration).

![Azure Automation](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Du kan skapa och hantera DSC-resurser finns i Azure och tillämpa dem toocloud och lokalt system toodefine och automatiskt tillämpa konfigurationen eller hämta rapporter på drift toohelp försäkra dig om att säkerhetskonfigurationer finns kvar i principen.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center skyddar resurserna i Azure. Det ger integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer. Inom hello-tjänsten, kan du inte toodefine principerna inte bara mot dina Azure-prenumerationer, utan även mot [resursgrupper](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), så du kan vara mer detaljerade.

![Azure Security Center](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Microsofts ständigt på hello Håll utkik efter hot. De har åtkomst tooan omfattande uppsättning telemetri erfarenheterna från Microsofts global närvaro i hello molnet och lokalt. Wide når och skilda samlingen av datauppsättningar kan Microsoft toodiscover nya angreppsmönster och trender i dess lokala konsument- och företagsinriktade produkter samt online-tjänster.

Därför kan Security Center snabbt uppdateras som angripare släpper nya och allt mer avancerade kryphål. Den här metoden kan du hålla jämna steg med en fast flytta hot-miljö.

![Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Hotidentifiering Security Center fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar.  Den här informationen korrelerar information från flera källor, tooidentify hot analyseras.
Säkerhetsaviseringar prioriteras i Security Center tillsammans med rekommendationer om hur tooremediate hello hot.

Security Center använder avancerade säkerhetsanalyser, som går mycket längre än signaturbaserade lösningar. Genombrott i stordata och [maskininlärning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tekniker är används tooevaluate händelser över hello hela molninfrastrukturresurs – upptäcka hot som skulle vara omöjligt tooidentify med manuella metoder och förutsäga hello utvecklingen av attacker. Dessa säkerhet analytics omfattar hello följande.

### <a name="threat-intelligence"></a>Hotinformation

Microsoft har tillgång till en enorm mängd global hotinformation.
Telemetri flödar i från flera källor, till exempel Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, hello Microsoft Digital Crimes Unit (DCU) och Microsoft Security Response Center (MSRC).

![Hotinformation](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Forskare får även information om hot från som delas med större molntjänst-providers och prenumererar toothreat intelligence feeds från tredje part. Azure Security Center kan använda denna information tooalert du toothreats från kända obehöriga. Några exempel är:

-   **Utnyttjar hello Power av Machine Learning -** Azure Security Center har åtkomst tooa stora mängder data om molnet nätverksaktivitet, vilket kan vara används toodetect hot målobjekt för din Azure-distributioner. Exempel:

-   **Brute Force identifieringar -** Machine learning finns används toocreate ett historiska mönster för fjärråtkomst försök, vilket gör att de toodetect brute force-attacker mot SSH RDP och SQL-portar.

-   **Utgående DDoS och identifiering av Botnät** -ett gemensamt mål av attacker som mål molnresurser är toouse hello datorkraft av dessa resurser tooexecute andra attacker.

-   **Nya Beteendebaserade Analytics-servrar och virtuella datorer -** när en server eller virtuell dator äventyras, angripare använda en mängd olika tekniker tooexecute skadlig kod på systemet och undvika identifiering, säkerställer beständiga och undanröja säkerhetsåtgärder.

-   **Azure SQL Database-Hotidentifiering -** Hotidentifiering för Azure SQL-databas som identifierar avvikande databasaktiviteter som indikerar ovanliga och potentiellt skadliga försöker tooaccess eller utnyttja databaser.

### <a name="behavioral-analytics"></a>Beteendeanalys

Beteendeanalys är en teknik som analyserar och jämför tooa insamling av kända mönster. Dessa mönster är dock inte enkla signaturer. De är definieras via komplexa maskininlärningsalgoritmer som är kopplade toomassive datauppsättningar.

![Beteendeanalys](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


och av expertanalytiker genom noggrann analys av skadliga beteenden. Azure Security Center kan använda beteendeanalys tooidentify komprometteras resurser baserat på analys av loggar för virtuell dator, enhetsloggar för virtuellt nätverk, fabric loggar, krascher Dumpar och andra källor.

Det finns dessutom korrelation med andra signaler toocheck för underlag för en omfattande kampanj. Den här korrelation hjälper tooidentify händelser som är konsekventa med etablerade indikatorer för angrepp.

Några exempel är:
-   **Körningen av misstänkt processen:** angripare använda flera tekniker tooexecute skadlig programvara utan identifiering. Till exempel att en angripare kan ge skadlig kod hello samma namn som legitima systemfiler men placera dessa filer på en annan plats, Använd ett namn som är mycket som en ofarlig fil eller mask hello true filtillägget. Bearbeta körningar toodetect avvikare som dessa Security Center modeller processer beteenden och Övervakare.

-   **Dold skadlig kod och utnyttjande försök:** avancerad skadlig kod kan undandra sig traditionella antivirusprodukter genom att antingen skriva toodisk aldrig eller kryptera programvarukomponenter som lagras på disk. Dock kan sådana skadlig kod identifieras med hjälp av minnesanalys som hello skadlig kod måste lämna spårningar i minnet toofunction. När programmet kraschar avbildar en krasch-dump en del av minnet när hello hello krascher. Genom att analysera hello minne i hello kraschdump kan Azure Security Center identifiera metoder som används för tooexploit säkerhetsrisker i programvaran, komma åt känsliga data och smyg finns kvar i en komprometterad dator utan att påverka hello prestanda din dator.

-   **Lateral förflyttning och interna rekognosering:** toopersist i en komprometterad nätverks- och leta upp/skörd värdefulla data angripare försöka ofta toomove sidled från hello komprometteras datorn tooothers inom hello samma nätverk. Security Center övervakar processen och logga in aktiviteter toodiscover försöker tooexpand en angripare fot inom hello nätverk, till exempel fjärrkörning nätverksåtkomst och kontouppräkning.

-   **PowerShell-skript:** PowerShell kan användas av angripare tooexecute skadlig kod på virtuella datorer som mål för olika ändamål. Security Center inspekterar PowerShell-aktivitet för att hitta tecken på misstänkt aktivitet.

-   **Utgående attacker:** angripare ofta mål molnresurser med hello målet med hjälp av dessa resurser toomount ytterligare attacker. Angripna virtuella datorer, till exempel kan vara används toolaunch brute force-attacker mot andra virtuella datorer, skicka skräppost eller skanna öppna portar och andra enheter i hello Internet. Genom att använda machine learning toonetwork trafik Security Center kan identifiera när utgående nätverkskommunikation överskrider hello normen. När skräppost, Security Center korrelerar också ovanliga e-trafik med information från Office 365 toodetermine om hello e-post är sannolikt nefarious eller hello resultatet av en giltig e-kampanj.

### <a name="anomaly-detection"></a>Avvikelseidentifiering

Azure Security Center använder också avvikelseidentifiering identifiering tooidentify hot. I kontrast toobehavioral analytics (som beror på kända mönster som härletts från stora datamängder), avvikelseidentifiering är mer ”anpassad” och fokuserar på baslinjer som är specifika tooyour distributioner. Machine learning är tillämpade toodetermine normal aktivitet för dina distributioner och sedan regler är genererade toodefine avvikare förhållanden som kan representera en säkerhetshändelse. Här är ett exempel:

-   **Inkommande RDP/SSH brute force-attacker:** dina distributioner kan ha upptagen virtuella datorer med många inloggningar varje dag och andra virtuella datorer har några eller alla inloggningar. Azure Security Center kan fastställa baslinjen inloggningsaktivitet för dessa virtuella datorer och använda machine learning toodefine runt hello normal inloggningen aktiviteter. Om det inte finns någon avvikelse med hello baslinje som definierats för relaterade inloggning egenskaper, och sedan en avisering kan genereras. Maskininlärningen avgör vad som är viktigt.

### <a name="continuous-threat-intelligence-monitoring"></a>Kontinuerlig Hotinformation övervakning

Azure Security Center fungerar med säkerhet forskning vetenskap team i hela hello world som kontinuerligt övervakar ändringar i hello hotbild. Detta omfattar följande initiativ hello:

-   **Övervakning av hot intelligence:** hot intelligence inkluderar mekanismer, indikatorer, effekter och tillämplig råd om befintliga eller nya hot. Den här informationen delas i hello säkerhet community och Microsoft övervakar kontinuerligt hot intelligence feeds från interna och externa källor.

-   **Signal delning:** insikter från säkerhet arbetsgrupper i Microsofts stor portfölj av molnet och lokala tjänster, servrar och klienten endpoint enheter delade och analyseras.

-   **Microsoft security specialister:** pågående interaktion med team på Microsoft som fungerar i särskild säkerhet fält, exempelvis dataforensik och web angreppsidentifiering.

-   **Identifiering av inställning:** algoritmer körs mot verkliga kunden datauppsättningar och säkerhet forskare arbeta med kunder toovalidate hello resultat. SANT och FALSKT positiva identifieringar kan använda toorefine maskininlärningsalgoritmer.

Dessa kombinerade ansträngningar avslutas nya och förbättrade identifieringar som du kan dra nytta av omedelbart – åtgärd ingen för du tootake.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Funktioner för identifiering av Advanced Threat - andra Azure-tjänster

### <a name="virtual-machine-microsoft-antimalware"></a>Virtuell dator: Microsoft Antimalware

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) för Azure är en enskild agent lösning för program och klient-miljöer, utformats toorun i hello bakgrunden utan mänsklig inblandning. Du kan distribuera skydd baserat på hello behoven för din programbelastningar, med antingen grundläggande secure-som-standard eller avancerad anpassad konfiguration, inklusive övervakning av program mot skadlig kod. Azure program mot skadlig kod är ett säkerhetsalternativ för virtuella datorer i Azure och installeras automatiskt på alla Azure PaaS virtuella datorer.

**Funktioner i Azure toodeploy och aktivera Microsoft Antimalware för dina program**

#### <a name="microsoft-antimalware-core-features"></a>Microsoft-program mot skadlig kod kärnfunktioner

-   **Realtidsskydd -** övervakar aktiviteten i Cloud Services och på virtuella datorer toodetect och blockera skadlig kod körning.

-   **Schemalagd genomsökning -** regelbundet utför riktade skanning toodetect skadlig kod, inklusive program som körs aktivt.

-   **Korrigering av skadlig kod -** automatiskt vidtar åtgärder på upptäckt skadlig kod, till exempel ta bort eller sätta i karantän skadliga filer och rensa skadliga registerposter.

-   **Signaturuppdateringar -** automatiskt installerar hello senaste skydd signaturer (virusdefinitioner av) tooensure skydd är uppdaterad på en förbestämd frekvens.

-   **Motorn för program mot skadlig kod uppdateringar -** automatiskt uppdateringar hello Microsoft Antimalware-motorn.

-   **Uppdateringar för program mot skadlig kod plattform –** automatiskt uppdateringar hello Microsoft Antimalware plattform.

-   **Aktivt skydd -** rapporterar telemetri metadata om identifierade hot och misstänkta resurser tooMicrosoft Azure tooensure snabba svar toohello utvecklas hotbild och aktivera realtidsskydd synkron signatur leverans via hello Microsoft Active Protection System (MAPS).

-   **Exempel för reporting -** innehåller och rapporter prover toohello Microsoft Antimalware service toohelp förfina hello service och aktivera felsökning.

-   **Undantag –** låter programmet och service administratörer tooconfigure vissa filer, processer och driver tooexclude dem från skyddet och skanning för prestanda och/eller andra orsaker.

-   **Program mot skadlig kod händelseinsamling -** registrerar hello tjänstens hälsa för program mot skadlig kod, misstänkta aktiviteter och åtgärder som vidtagits i händelseloggen för hello operativsystem och samlar in dem i hello kundens Azure Storage-konto.

### <a name="azure-sql-database-threat-detection"></a>Hotidentifiering för Azure SQL-databas

[Azure SQL Database-Hotidentifiering](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) är en ny intelligence säkerhetsfunktion inbyggd hello Azure SQL Database-tjänsten. Undvika hello klockfrekvensen toolearn profil och identifiera avvikande databasaktiviteter, Azure SQL Database Hotidentifiering identifierar potentiella hot toohello-databasen.

Säkerhet polis eller andra avsedda administratörer kan få en omedelbar avisering om misstänkt databasaktiviteter när de inträffar. Varje meddelande innehåller information om hello misstänkt aktivitet och rekommenderar hur toofurther undersöka och minimera hotet hello.

För närvarande identifierar Azure SQL Database Hotidentifiering potentiella säkerhetsproblem och SQL injection attacker och avvikande databasen åtkomstmönster.

Vid mottagning av threat detection e-postmeddelande kan är användarna kan toonavigate och visa hello relevanta granskningsposter med hello djuplänk i hello e-post som öppnar en audit viewer och/eller förkonfigurerade granskning Excel-mallen som visar hello relevanta granskning poster runt hello tidpunkt hello misstänkta händelsen enligt toohello följande:
-   Granska lagring för hello/databasserver med hello avvikande databasaktiviteter

-   Relevanta audit lagringstabellen som användes när hello hello händelse toowrite granskningsloggen

-   Granska posterna för hello efter timme eftersom hello händelse inträffar.

-   Granskningsposter med liknande händelse-ID när hello hello händelse (valfritt för vissa detektorerna)

SQL-databas hot detektorerna använder en hello följande metoder för identifiering:

-   **Deterministisk identifiering –** identifierar misstänkta mönster (regler baserade) i hello SQL-klientfrågor som matchar kända attacker. Den här metoden har identifiering av hög och låg falsklarm dock begränsat täckning eftersom den ligger inom ”atomiska identifieringar” hello kategori.

-   **Beteendemässiga identifiering –** fel avvikande aktivitet, onormalt beteende för hello-databas som inte påträffades under hello senaste 30 dagarna.  Ett exempel på SQL avvikande aktivitet kan vara en insamling av misslyckade inloggningar-frågor, stor mängd data som extraheras, onormal kanoniska frågor och okänd IP-adresser som används tooaccess hello databas

### <a name="application-gateway-web-application-firewall"></a>Programmet Gateway Brandvägg för webbaserade program

[Web Application Firewall](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall) är en funktion i [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) som ger skydd tooweb program som använder Programgateway för standard [programkontrollen leverans](https://kemptechnologies.com/in/application-delivery-controllers) funktioner. Brandvägg för webbaserade program gör detta genom att skydda dem mot de flesta av hello [OWASP topp 10 vanliga säkerhetsproblem för webbprogram](https://www.owasp.org/index.php/Top_10_2010-Main)

![Application Gateway Web Application Firewally](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   Skydd mot SQL-inmatning

-   Skydd mot skriptkörning över flera webbplatser

-   Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil

-   Skydd mot åtgärder som inte följer HTTP-protokollet

-   Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas

-   Skydd mot robotar, crawlers och skannrar

-   Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)

Konfigurera Brandvägg på Programgateway innehåller följande förmåner tooyou hello:

-   Skydda ditt webbprogram från webben säkerhetsproblem och attacker utan modifiering toobackend kod.

-   Skydda flera web applications vid hello samma tid bakom en Programgateway. Programgateway stöder värd upp too20 webbplatser bakom en enda gateway som kan alla vara skyddad mot angrepp på webben.

-   Övervaka ditt webbprogram för att förhindra attacker, med hjälp av realtidsrapporter som genereras av gatewayens WAF-loggar.

-   Vissa kontroller för efterlevnad kräver alla internet facing slutpunkterna toobe skyddas av en Brandvägg. Genom att använda programgatewayen med WAF aktiverat kan du uppfylla dessa kompatibilitetskrav.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Avvikelseidentifiering – en API som byggts med Azure Machine Learning

Avvikelseidentifiering är en API som byggts med Azure Machine Learning som är användbar för att identifiera olika typer av avvikande mönster i tid series-data. hello API tilldelar en avvikelseidentifiering poäng tooeach datapunkt i hello tidsserier, som kan användas för att generera aviseringar, övervakning genom instrumentpaneler eller ansluta med loggnings-system.

Hej [Avvikelseidentifiering identifiering API](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) kan identifiera hello följande typer av avvikelser på tid series-data:

-   **Ger spikar i diagrammet och sjunker:** exempelvis när du övervakar hello antalet inloggning fel tooa service eller antal utcheckningar i en e-handelsplats ovanliga toppar korta kan indikera säkerhetsattacker eller avbrott i tjänsten.

-   **Positiva och negativa trender:** när du övervakar minnesanvändning i computing exempelvis krympa ledigt minne är indikation på en minnesläcka; när du övervakar service Kölängd beständiga tendens kan tyda på en underliggande problem med programvara.

-   **Nivå ändringar och ändringar i dynamiskt värdeintervallet:** exempelvis nivå ändringar i svarstiderna för en tjänst när en tjänst har uppgraderat eller lägre nivåer av undantag efter uppgraderingen kan vara intressanta toomonitor.

Hej maskininlärning baserad API som ger:

-   **Flexibel och kraftfull identifiering:** hello avvikelseidentifiering identifiering modeller användarna tooconfigure känslighet inställningar och identifiera avvikelser mellan säsongsbaserade och icke-säsongsbaserade datauppsättningar. Användare kan justera hello identifiering av modellen toomake hello avvikelseidentifiering API mindre eller känsligare bl.a tootheir behöver. Detta betyder att upptäcka hello mindre eller mer synliga avvikelser i data med och utan säsongsbaserade mönster.

-   **Skalbar och rimlig identifiering:** hello traditionella sätt att övervaka med finns tröskelvärden som angetts av experter kunskap är kostsamma och inte skalbara toomillions med dynamisk ändring av datauppsättningar. Hej avvikelseidentifiering identifiering modeller i detta API lärs och modeller justerade automatiskt från både historiska och realtid data.

-   **Proaktiv och tillämplig:** långsam trend och förändring av identifiering kan användas för tidig avvikelseidentifiering. hello tidig onormal signaler identifieras kan använda toodirect människor tooinvestigate och fungerar på hello problemområden.  Dessutom rotorsak analys modeller och aviseringar verktyg kan utvecklas ovanpå detta avvikelseidentifiering API-tjänsten.

Hej avvikelseidentifiering API är en effektiv lösning för en mängd olika scenarier som tjänstens hälsa & KPI övervakning, IoT, övervakning av programprestanda och övervakning av nätverkstrafik. Här följer några vanliga scenarier där detta API kan vara användbart:
- IT-avdelningar måste verktyg tootrack händelser, felkoden, användning loggen och prestanda (CPU, minne och så vidare) inom rimlig tid.

-   Online commerce platser vill tootrack kundaktiviteter, sidvisningar, klick och så vidare.

-   Verktyget företag vill tootrack förbrukning av vattenstämplar, gas, elektricitet och andra resurser.

-   Lokal/byggnad hanteringstjänster vill toomonitor temperatur, fukt, trafik och så vidare.

-   IoT/tillverkare ha toouse sensordata i tid serien toomonitor arbetsflöde, kvaliteten och så vidare.

-   Leverantörer, vänta såsom call Center behöver toomonitor service begäran trend volym incidenter, kölängd och så vidare.

-   Analytics affärsgrupper vill toomonitor business KPI: er (till exempel försäljning volym, våra kunder, priser) onormal flytt i realtid.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) är en kritisk komponent i hello Microsoft Cloud Security-stacken. Det är en heltäckande lösning som kan hjälpa din organisation när du flyttar tootake full nytta av molnapparnas potential hello men tänk kontrollen med ökad insyn i aktiviteten. Det hjälper även öka hello skydda viktiga data i molnappar.

Med verktyg som hjälper Upptäck skugg-IT, utvärdera risker, genomdriva principer, undersöka aktiviteter och stoppa hot kan din organisation säkrare flytta toohello molnet samtidigt som kontrollen av kritiska data.

<table style="width:100%">
 <tr>
   <td>Utforska</td>
   <td>Upptäck skugg-IT med Cloud App Security. Få insyn genom att identifiera appar, aktiviteter, användare, data och filer i din molnmiljö. Identifiera tredjepartsprogram som är anslutna tooyour moln.</td>
 </tr>
 <tr>
   <td>Undersök</td>
   <td>Undersök dina molnappar med hjälp av molnet dataforensik verktyg toodeep tittar i riskfyllda appar, specifika användare och filer i nätverket. Hitta mönster i hello data som samlas in från molnet. Generera rapporter toomonitor ditt moln.</td>

 </tr>
 <tr>
   <td>Kontrollen</td>
   <td>Minska riskerna genom att skapa principer och varningar tooachieve full kontroll över molntrafiken i nätverket. Använda Cloud App Security toomigrate dina användare toosafe sanktionerade molnappar.</td>

 </tr>
 <tr>
   <td>Skydda</td>
   <td>Använda Cloud App Security toosanction eller förbjuda program, tillämpa skydd mot dataförlust, behörighet och dela och skapa anpassade rapporter och aviseringar.</td>

 </tr>
 <tr>
   <td>Kontrollen</td>
   <td>Minska riskerna genom att skapa principer och varningar tooachieve full kontroll över molntrafiken i nätverket. Använda Cloud App Security toomigrate dina användare toosafe sanktionerade molnappar.</td>

 </tr>
</table>


![Cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security integrerar synlighet i molnet genom att

-   Med hjälp av Cloud Discovery toomap och identifiera molnmiljön och hello molnappar med hjälp av din organisation.


-   Sanktionering och förbjuder appar i molnet.



-   Med hjälp av enkelt att distribuera anslutningsverktyg som utnyttjar providerns API: er, för synlighet och styrning av appar som du ansluter till.

-   Hjälper att du har kontinuerlig kontroll av inställningen och kontinuerligt finjustera principer.

Samla in data från dessa källor körs Cloud App Security avancerad analys på hello data. Den omedelbart aviserar tooanomalous aktiviteter och du får djup insyn i din molnmiljö. Du kan konfigurera en princip i Cloud App Security och använder det tooprotect allt i din molnmiljö.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>Tredjeparts-ATD funktioner via Azure Marketplace

### <a name="web-application-firewall"></a>Brandvägg för webbaserade program

Brandvägg för webbaserade program kontrollerar om inkommande web trafik och block SQL injektionerna globala webbplatsskript, skadlig kod överföringar & DDoS-program och andra attacker riktade mot webbaserade program. Den kontrollerar också hello svar från hello backend-webbservrar för Data går förlorade förebyggande (DLP). hello integrerad access control motorn kan administratörer toocreate detaljerade principer för åtkomstkontroll för autentisering, auktorisering och redovisning, som ger organisationer stark autentisering och användarkontroll.

**Visar:**
-   Identifierar och blockerar SQL injektioner globala webbplatsskript, skadlig kod överföringar, DDoS-program eller andra attacker mot ditt program.

-   Autentisering och åtkomstkontroll.

-   Genomsöker utgående trafik toodetect känsliga data och maskera eller blockera hello information läcks.

-   Påskyndar hello leverans av web application innehåll, med hjälp av funktioner, till exempel cachelagring, komprimering och andra trafik optimeringar.

Följande är exempel på webbprogrammet brandväggar som är tillgängliga i Azure Marketplace:

[Barracuda Brandvägg för webbaserade program, Brocade virtuella Brandvägg för webbaserade program (Brocade vWAF), Imperva SecureSphere och hello ThreatSTOP IP-brandväggen.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Nästa steg

- [Identifieringsfunktioner i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Funktioner för avancerad identifiering av Azure Security Center hjälper tooidentify active hot målobjekt för Microsoft Azure-resurser och ger dig med hello insikter behövs toorespond snabbt.

- [Hotidentifiering för Azure SQL-databas](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Azure SQL Database-Hotidentifiering hjälpt adressen synpunkter om potentiella hot tootheir databasen.
