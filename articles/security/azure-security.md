---
title: "aaaIntroduction tooAzure säkerhet | Microsoft Docs"
description: "Lär dig mer om Azure-säkerhet, att tjänsterna och hur det fungerar."
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
ms.date: 05/03/2017
ms.author: TomSh
ms.openlocfilehash: 2d42057e9586a0b6ce16a1582db3b3af842297af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security"></a>Introduktion tooAzure säkerhet
## <a name="overview"></a>Översikt
Vi vet att säkerheten är jobbet en i hello molnet och det är viktigt att du hitta korrekt och rimlig information om säkerheten i Azure. En av hello bästa orsaker toouse Azure för dina program och tjänster är tootake nytta av dess mängd säkerhetsverktyg och funktioner. Dessa verktyg och funktioner gör det möjligt toocreate säkra lösningar på hello säker Azure-plattformen. Microsoft Azure tillhandahåller sekretess, integritet och tillgänglighet av kundinformation, samtidigt som transparent accountability.

toohelp bättre förstå hello samling säkerhetsåtgärder som implementerats i Microsoft Azure från både hello kunden och Microsoft operations perspektiv, den här vitboken ”introduktion tooAzure säkerhet” skrivs tooprovide en omfattande titt på hello säkerhet med Microsoft Azure.

### <a name="azure-platform"></a>Azure-plattformen
Azure är en offentlig molntjänstplattform som har stöd för ett brett urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter. Det kan köras Linux behållare med Docker-integration. skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js; build-servrar för iOS, Android och Windows enheter.

Offentliga Azure-molnet och support hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på. När du bygger på, eller migrera IT tillgångar, en offentlig molntjänstleverantören måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från anläggningen tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag kan uppfylla sina säkerhetskrav.

Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för distributioner i din organisation. Det här dokumentet hjälper dig att förstå hur Azure-säkerhet funktioner kan hjälpa dig att uppfylla kraven.

> [!Note]
> hello fokuserar i det här dokumentet på kund-riktade kontroller som du kan använda toocustomize och öka säkerheten för dina program och tjänster.
>
> Vi lämnar vissa översiktlig information, men mer detaljerad information om hur Microsoft skyddar hello Azure-plattformen, se informationen i hello [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Abstrakt
Offentligt moln migreringar har från början, styrs av kostnaden besparingar och smidighet tooinnovate. Säkerhet ansågs stor betydelse för en stund och även en visa proppen för offentliga molnmigrering. Dock har offentligt moln säkerhet övergått från ett större problem tooone hello drivrutiner för molnmigrering. hello anledningen detta är hello överlägsen möjligheten för stora offentliga service providers tooprotect molnprogram och hello data för molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag uppfyller deras behov. Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav distributioner-toomeet IT-ADMINISTRATÖREN kontrollera principer och följa tooexternal förordningar.

Det här dokumentet beskrivs Microsofts metoden toosecurity i hello Microsoft Azure cloud platform:
* Säkerhetsfunktioner som implementeras av Microsoft toosecure hello Azure-infrastrukturen, kunddata och program.
* Azure-tjänster och säkerhet funktioner tillgängliga tooyou toomanage hello hello tjänster och dina data i Azure-prenumerationer.

## <a name="summary-azure-security-capabilities"></a>Översikt över Azure säkerhetsfunktioner
hello tabellen nedan ger en kort beskrivning av hello säkerhetsfunktioner som implementeras av Microsoft toosecure hello Azure-infrastrukturen, kunddata och säkra program.
### <a name="security-features-implemented-toosecure-hello-azure-platform"></a>Säkerhet funktioner implementerats tooSecure hello Azure-plattformen:
hello funktioner visas följande är funktioner som du kan granska tooprovide hello försäkran som hello Azure-plattformen hanteras på ett säkert sätt. Länkar har angetts för ytterligare nedåt på hur Microsoft adresser förtroende kundfrågor inom fyra områden: säker plattform, sekretess och kontroller, efterlevnad och insyn.


| [Säker plattform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Sekretess och kontroller](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Efterlevnad](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Genomskinlighet](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Säkerhet utvecklingscykeln](https://www.microsoft.com/en-us/sdl/)interna granskningar | [Hantera dina data hela tiden för hello](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Säkerhetscenter](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Hur Microsoft skyddar kunddata i Azure-tjänster](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Obligatoriska säkerhetsutbildning tillbaka grunden kontroller](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc) |  [Kontrollera på plats](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Gemensamt nav för kontroller](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Hur Microsoft hanterar Dataplats i Azure-tjänster](http://azuredatacentermap.azurewebsites.net/)|
| [Testa intrång](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc), [intrångsidentifiering, DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagemen), [granskningar & loggning](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Tillhandahålla dataåtkomst på dina villkor](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [hello Cloud Services på grund av fordringar Checklista](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Som i Microsoft kan komma åt dina data på vilka villkoren](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Läget för bilder datacentret](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fysisk säkerhet [säkra nätverk](https://docs.microsoft.com/en-us/azure/security/security-network-overview) | [Svara toolaw tvingande](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Efterlevnad av tjänsten, plats och bransch](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Hur Microsoft skyddar kunddata i Azure-tjänster](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Säkerhet Incident svar](http://aka.ms/SecurityResponsepaper), [delade ansvar](http://aka.ms/sharedresponsibility) |[Strikta sekretess standarder](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Granska certifikatutfärdare för Azure-tjänster, genomskinlighet hub](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-toosecure-data-and-application"></a>Säkerhet funktioner som erbjuds av Azure tooSecure Data och program
Beroende på hello modell moln finns variabeln ansvar för som ansvarar för att hantera hello säkerheten för hello programmet eller tjänsten. Det finns funktioner i hello Azure-plattformen tooassist du uppfylla dessa ansvarsområden via inbyggda funktioner och via partnerlösningar som kan distribueras till en Azure-prenumeration.

hello inbyggda funktioner som är ordnade i sex (6) huvudområden: åtgärder, program, lagring, nätverk, beräkning och identitet. Ytterligare information om hello funktioner som finns i hello Azure-plattformen i dessa områden med sex (6) tillhandahålls via sammanfattningsinformation.

## <a name="operations"></a>Åtgärder
Det här avsnittet innehåller ytterligare information om viktiga funktioner i säkerhetsåtgärder och översiktlig information om dessa funktioner.

### <a name="operations-management-suite-security-and-audit-dashboard"></a>Operations Management Suite säkerhet och granska instrumentpanel
Hej [OMS säkerhet och granska lösningen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) tillhandahåller en heltäckande översikt över din organisations IT säkerhetstillståndet med [inbyggda sökfrågor](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) för anmärkningsvärda problem som kräver din uppmärksamhet. Hej [säkerhet och granska](https://technet.microsoft.com/library/mt484091.aspx) instrumentpanelen är hello startsidan för allt relaterade toosecurity i OMS. Det ger en övergripande inblick i hello säkerhetsstatus för dina datorer. Den omfattar också hello möjlighet tooview alla händelser från hello senaste 24 timmarna, 7 dagar eller andra anpassade tidsintervall.

Dessutom kan du konfigurera OMS säkerhet och efterlevnad för[automatiskt utföra specifika åtgärder](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) när en viss händelse identifieras.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager ](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) kan du toowork med hello resurser i din lösning som en grupp. Du kan distribuera, uppdatera eller ta bort alla hello resurser i lösningen i en enda, samordnad åtgärd. Du använder en [Azure Resource Manager-mall](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) för distribution och mallen kan användas i olika miljöer, till exempel testning, mellanlagring och produktion. Resource Manager tillhandahåller säkerhets-, granskning och märkning funktioner toohelp hantera dina resurser efter distributionen.

Azure Resource Manager mallbaserade distributioner förbättra hello säkerheten för lösningar som distribueras i Azure eftersom Standardsäkerhet styr inställningar och kan integreras i standardiserad mallbaserade distributioner. Detta minskar risken för hello configuration säkerhetsrisker som kan ske vid manuell distribution.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) är en utökningsbar Management-program (APM)-tjänst för webbutvecklare. Du kan övervaka webbprogrammen levande och automatiskt identifiera prestandaavvikelser med Application Insights. Den innehåller kraftfulla analytics verktyg toohelp du diagnostisera problem och toounderstand vilka användare som faktiskt göra med dina appar. Det övervakar programmet alla hello gång den körs, både under testningen och när du har publicerat eller distribuera den.

Application Insights skapar diagram och tabeller som visar, till exempel vilka tidpunkter på dagen som de flesta användare hur responsiv hello appen är och hur den hanteras av en extern källa för tjänster som den är beroende.

Om det finns krascher, fel eller prestandaproblem kan söka du igenom hello telemetridata i detalj toodiagnose hello orsak. Och hello tjänsten skickar e-postmeddelanden om det finns ändringar i hello tillgänglighet och prestanda för din app. Application Insights blir därmed värdefulla säkerhetsverktyget eftersom det hjälper med hello tillgänglighet i hello sekretess, integritet och tillgänglighet säkerhet triad.

### <a name="azure-monitor"></a>Azure Monitor
[Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) erbjuder visualisering, fråga, routning, varningar, automatisk skalning och automation på data både från hello Azure-infrastrukturen ([aktivitetsloggen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) och varje enskild Azure resurs ([ Diagnostikloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). Du kan använda Azure-Monitor tooalert du på säkerhetsrelaterade händelser som genereras i Azure-loggarna.

### <a name="log-analytics"></a>Log Analytics
[Logga Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) tillhör [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – ger en IT-hanteringslösning för både lokalt och från tredje part molnbaserad infrastruktur (till exempel AWS) utöver tooAzure resurser. Data från Azure-Monitor kan vidarebefordras direkt tooLog Analytics så att du kan se mått och loggfiler för hela miljön på ett ställe.

Logganalys kan vara användbart i kriminaltekniska och andra säkerhetsanalys hello-verktyget kan du tooquickly sökning i stora mängder säkerhetsrelaterade poster med en flexibel frågeställning. Dessutom lokalt [brandväggar och proxyservrar loggar kan exporteras till Azure och tillgängliga för analys med logganalys.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](https://docs.microsoft.com/azure/advisor/) är en anpassad cloud-konsult som hjälper dig att toooptimize dina Azure-distributioner. Den analyserar din resurskonfiguration och användningstelemetri. Rekommenderar lösningar toohelp förbättra hello [prestanda](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [säkerhet](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), och [hög tillgänglighet](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) av dina resurser vid sökning efter möjligheter för[minska din övergripande Azure tillbringar](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Advisor ger säkerhetsrekommendationer, vilket betydande kan förbättra din övergripande säkerhetstillståndet för lösningar som du distribuerar i Azure. De här rekommendationerna hämtas från säkerhetsanalys som utförs av [Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kan du förebygga, upptäcka och svara toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Dessutom hjälper Azure Security Center med säkerhetsåtgärder genom att ge dig en enda instrumentpanel som hämtar aviseringar och rekommendationer som kan åtgärdas omedelbart. Ofta kan du åtgärda problem med en enda klickning i hello Azure Security Center-konsolen.
## <a name="applications"></a>Program
hello avsnitt innehåller ytterligare information om viktiga funktioner i programmet säkerhets- och sammanfattning information om dessa funktioner.

### <a name="web-application-vulnerability-scanning"></a>Web Application säkerhetsproblem genomsökning
En av hello enklaste sätt tooget igång med att testa säkerhetsrisker på din [Apptjänst-app](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is) är toouse hello [integrering med Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tooperform enkelklickning säkerhetsproblem genomsökning av din app. Du kan visa hello testresultaten i ett enkelt att förstå rapport och lära dig hur toofix varje säkerhetsproblem med stegvisa instruktioner.

### <a name="penetration-testing"></a>Penetrationstester
Om du föredrar tooperform egna intrång testar eller vill toouse en annan skanner suite eller providern, måste du följa hello [Azure intrång testa godkännandeprocessen](https://security-forms.azure.com/penetration-testing/terms) och få godkännande i förväg tooperform hello önskad intrång tester.

### <a name="web-application-firewall"></a>Brandvägg för webbaserade program
Hej Brandvägg för webbaserade program (Brandvägg) i [Azure Programgateway](https://azure.microsoft.com/services/application-gateway/) hjälper dig att skydda webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionskapning. Kommer det förinställda med skydd från hot som identifieras av hello [öppna Web Application säkerhet projekt (OWASP) som hello topp 10 vanliga säkerhetsproblem](https://msdn.microsoft.com/library/).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Autentisering och auktorisering i Azure App Service
[App Service autentisering / auktorisering](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) är en funktion som gör det möjligt för dina program toosign användare så att du inte har toochange kod på hello-appserverdelen. Det ger ett enkelt sätt tooprotect programmet och arbete data per användare.

### <a name="layered-security-architecture"></a>Överlappande säkerhetsarkitekturen
Eftersom [Apptjänstmiljöer](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-intro) ger en isolerad körningsmiljö har distribuerats till en [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), utvecklare kan skapa en skiktad säkerhetsarkitekturen att tillhandahålla olika nivåer av nätverksåtkomst för varje program-nivå. Viljan vanliga är toohide API-servrar från allmän tillgång till Internet och endast tillåta API: er toobe anropas av överordnad webbprogram. [Nätverkssäkerhetsgrupper (NSG: er)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) kan användas på Azure Virtual Network-undernät som innehåller Apptjänstmiljöer toorestrict offentlig åtkomst tooAPI program.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web serverdiagnostik- och programdiagnostik
App Service web apps ange diagnostikfunktion för att logga information från både hello webbservern och hello webbprogram. Dessa logiskt är indelade i [web serverdiagnostik](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) och [programdiagnostik](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webbservern innehåller två huvudsakliga utvecklingen i diagnostisera och felsöka webbplatser och program.

hello första ny funktion är realtid statusinformation om programpooler, arbetsprocesser, webbplatser, programdomäner och begäranden som körs. hello andra nya fördelar är hello detaljerade spårningshändelser som spårar en begäran i hela hello Slutför begäran och svar process.

tooenable hello insamlingen av dessa spårningshändelser kan IIS 7 kan vara konfigurerade tooautomatically avbilda fullständig spårningsloggar i XML-format för en viss begäran baserat på tiden eller fel svarskoder.

#### <a name="web-server-diagnostics"></a>Web serverdiagnostik
Du kan aktivera eller inaktivera hello följande typer av loggar:

-   Detaljerad felloggning - information om felet för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre). Detta kan innehålla information som kan hjälpa dig att avgöra varför hello servern returnerade felkoden hello.

-   Spårning av misslyckade begäranden - detaljerad information om misslyckade förfrågningar, inklusive en spårning av hello IIS-komponenter som används tooprocess hello begäran och hello tid i varje komponent. Detta kan vara användbart om du försöker tooincrease platsprestanda eller isolera vad som orsakar en specifika HTTP-fel toobe returneras.

-   Web Server-loggning - Information om HTTP-transaktioner med hello W3C utökat loggfilsformat. Detta är användbart när du fastställer övergripande plats mätvärden, till exempel hello antal begäranden som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.

#### <a name="application-diagnostics"></a>Programdiagnostik
[Programdiagnostik](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) kan du toocapture information som produceras av ett webbprogram. ASP.NET-program kan använda hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) programdiagnostik för klassen toolog information toohello loggar. Det finns två typer av händelser i Application Diagnostics, de relaterade tooapplication prestanda och åtgärder som rör tooapplication fel och problem. hello programfel kan delas upp ytterligare i anslutning, säkerhet och fel. Fel problem är vanligtvis relaterade tooa problem med hello programkod.

I Programdiagnostik kan visa du gruppera händelser på följande sätt:

-   Alla (visar alla händelser)
-   Programfel (visar undantagshändelser)
-   Prestanda (visar prestandahändelser)

## <a name="storage"></a>Lagring
hello avsnitt innehåller ytterligare information om viktiga funktioner i Azure storage säkerhets- och sammanfattning information om dessa funktioner.

### <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Du kan skydda ditt lagringskonto med rollbaserad åtkomstkontroll (RBAC). Begränsa åtkomst baserat på hello [måste tooknow](https://en.wikipedia.org/wiki/Need_to_know) och [minsta privilegium](https://en.wikipedia.org/wiki/Principle_of_least_privilege) säkerhetsprinciper är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst. Dessa behörigheter beviljas genom att tilldela hello lämpliga RBAC rollen toogroups och program för ett visst område. Du kan använda [inbyggda RBAC-roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles), till exempel lagring konto deltagare tooassign behörighet toousers. Åtkomstnycklar toohello lagring för ett lagringskonto med hjälp av hello [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) modell kan kontrolleras via rollbaserad åtkomstkontroll (RBAC).

### <a name="shared-access-signature"></a>Signatur för delad åtkomst
En [signatur för delad åtkomst (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) ger delegerad åtkomst tooresources i ditt lagringskonto. hello SAS innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven period och på en viss uppsättning behörigheter. Du kan bevilja dessa begränsade behörigheter utan tooshare åtkomstnycklarna för ditt konto.

### <a name="encryption-in-transit"></a>Kryptering under överföring
Kryptering under överföring är en mekanism för att skydda data när de skickas över nätverk. Du kan skydda data med hjälp av med Azure Storage:
-   [Kryptering på transportnivå](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), till exempel HTTPS när du överför data till eller från Azure Storage.

-   [Tråd kryptering](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), som [kryptering i SMB 3.0](https://docs.microsoft.com/azure/storage/storage-security-guide) för [Azure-filresurser](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   Kryptering på klientsidan, tooencrypt hello data innan den överförs till lagringskontot och toodecrypt hello data när det överförs utanför lagring.

### <a name="encryption-at-rest"></a>Vilande kryptering
I många organisationer är ett obligatoriskt steg mot datasekretess, efterlevnad och data suveränitet i datakryptering i viloläge. Det finns tre Azure storage-säkerhetsfunktioner som tillhandahåller kryptering av data som är ”i vila”:

-   [Lagringstjänstens kryptering](https://docs.microsoft.com/azure/storage/storage-service-encryption) kan du toorequest att hello lagringstjänsten automatiskt kryptera data när du skriver den tooAzure lagring.

-   [Kryptering på klientsidan](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) ger också hello-funktionen för kryptering i vila.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kan du tooencrypt hello OS diskar och datadiskar som används av en virtuell IaaS-dator.

### <a name="storage-analytics"></a>Lagringsanalys
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) utför loggning och tillhandahåller data för mått för ett lagringskonto. Du kan använda den här tootrace begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto. Storage Analytics loggar detaljerad information om lyckade och misslyckade begäranden tooa storage-tjänst. Den här informationen kan vara används toomonitor enskilda förfrågningar och toodiagnose problem med en storage-tjänst. Begäranden loggas för bästa prestanda. följande typer av autentiserade begäranden hello loggas:
-   Lyckade begäranden.

-   Misslyckade begäranden, inklusive timeout, begränsning, nätverk, auktorisering och andra fel.

-   Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden.

-   Begäranden tooanalytics data.

### <a name="enabling-browser-based-clients-using-cors"></a>Aktivera webbläsarbaserade klienter med hjälp av CORS
[Cross-Origin Resource Sharing (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) är en mekanism som gör att domäner toogive varandra behörighet för åtkomst till varandras resurser. hello användaragent skickar extra rubriker tooensure som lästs in från en viss domän hello JavaScript-kod tillåts tooaccess resurser på en annan domän. hello senare domänen svarar den sedan med extra rubriker för att tillåta eller neka hello ursprungliga domänåtkomst tooits resurser.

Azure storage services stöder nu CORS så att när du har angett hello CORS-regler för hello-tjänsten är en korrekt autentiserad begäran mot hello tjänsten från en annan domän utvärderade toodetermine om det är tillåtet enligt toohello regler som du har Ange.
## <a name="networking"></a>Nätverk
hello avsnitt innehåller ytterligare information om viktiga funktioner i Azure-nätverk säkerhets- och sammanfattning information om dessa funktioner.

### <a name="network-layer-controls"></a>Nätverkskontroller lager
Åtkomstkontrollen för nätverk är hello act för att begränsa anslutningen tooand från specifika enheter och undernät och representerar hello kärnor för nätverkssäkerhet. hello målet av åtkomstkontrollen för nätverk är toomake till att dina virtuella datorer och tjänster finns tillgänglig tooonly användare och enheter toowhich du vill ha dem tillgängliga.

#### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
En [Nätverkssäkerhetsgrupp (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) är en grundläggande tillståndskänslig paket filtrering brandvägg och det gör du toocontrol åtkomst baserat på en [5-tuppel](https://www.techopedia.com/definition/28190/5-tuple). NSG: er ger inte programmet layer inspektion eller autentiserad åtkomstkontroller. De kan användas toocontrol trafik flytta mellan undernät i ett virtuellt Azure-nätverk och trafik mellan en Azure-nätverk och hello Internet.

#### <a name="route-control-and-forced-tunneling"></a>Dirigera kontroll och Tvingad tunneltrafik
hello är möjlighet toocontrol routning på dina virtuella Azure-nätverk en kritisk säkerhets- och kontroll nätverkskapacitet. Till exempel om du vill att all trafik tooand från det virtuella Azure-nätverket passerar den virtuella postsäkerhet toomake, du behöver toobe kan toocontrol och anpassa dirigeringsbeteendet. Du kan göra detta genom att konfigurera användardefinierade vägar i Azure.

[Användardefinierade vägar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) Tillåt toocustomize inkommande och utgående sökvägar för trafik som flyttar till och från enskilda virtuella datorer eller undernät tooinsure hello säkraste väg möjligt. [Tvingad tunneltrafik](https://www.petri.com/azure-forced-tunneling) är en mekanism som du kan använda tooensure som dina tjänster inte är tillåtna tooinitiate en anslutning toodevices på hello Internet.

Detta skiljer sig från att kunna tooaccept inkommande anslutningar och sedan svarar toothem. Frontend-webbservrar måste toorespond toorequests från Internet-värdar och så källkod för Internet-trafik tillåts inkommande toothese webbservrar och hello webbservrar kan svara.

Tvingad tunneling är vanliga tooforce utgående trafik toohello Internet toogo via lokala säkerhet proxyservrar och brandväggar.

#### <a name="virtual-network-security-appliances"></a>Säkerhetsenheter för virtuellt nätverk
Medan Nätverkssäkerhetsgrupper, användardefinierade vägar och Tvingad tunneling ger en hög säkerhetsnivå på hello nätverks- och lager i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), det kan finnas tillfällen när du vill tooenable säkerhet på högre nivåer av hello-stacken. Du kan komma åt dessa förbättrade funktioner för nätverkssäkerhet med hjälp av en Azure network security installation partnerlösning. Du kan hitta hello senaste Azure network security partnerlösningar genom att besöka hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) och söka efter ”säkerhet” och ”nätverkssäkerhet”.

### <a name="azure-virtual-network"></a>Azure Virtual Network

Azure-nätverk (VNet) är en representation av ditt eget nätverk i hello molnet. Det är en logisk isolering av hello Azure network fabric dedikerade tooyour prenumeration. Du kan helt styra hello IP-Adressblock, DNS-inställningar, säkerhetsprinciper och Routingtabellerna inom det här nätverket. Du kan också segmentera ditt VNet i undernät och placera Azure IaaS-virtuella maskiner (VMs) och/eller [molntjänster (PaaS rollinstanser)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) på virtuella Azure-nätverk.

Du kan dessutom ansluta hello virtuellt nätverk tooyour lokalt nätverk med ett av hello [anslutningsalternativ](https://docs.microsoft.com/azure/vpn-gateway/) tillgängliga i Azure. I princip kan du expandera ditt nätverk tooAzure, med fullständig kontroll över IP-Adressblock med hello förmån på företagsnivå som Azure tillhandahåller.

Azure-nätverk har stöd för olika scenarier för säker fjärråtkomst. Några av dessa är:

-   [Ansluta enskilda arbetsstationer tooan Azure Virtual Network](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Ansluta lokala nätverk tooan Azure-nätverk med en VPN-anslutning](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [Ansluta lokala nätverk tooan Azure-nätverk med en fast WAN-länk](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Anslut Azure Virtual Networks tooeach andra](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN-gateway
toosend nätverkstrafik mellan dina virtuella Azure-nätverket och den lokala platsen, måste du skapa en VPN-gateway för det virtuella Azure-nätverket. En [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) är en typ av virtuell nätverksgateway som skickar krypterad trafik mellan en offentlig anslutning. Du kan också använda VPN-gatewayer toosend trafik mellan virtuella Azure-nätverk via hello Azure nätverksinfrastruktur.

### <a name="express-route"></a>Express Route
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) är en dedikerad WAN-länk där du kan utöka ditt lokala nätverk till hello Microsoft cloud via en dedikerad privata anslutning underlättas av en provider för anslutningen.

![Express Route](./media/azure-security/azure-security-fig1.png)

Du kan upprätta anslutningar molntjänster tooMicrosoft, till exempel Microsoft Azure, Office 365 och CRM Online med ExpressRoute. Anslutningen kan vara från ett ”any-to-any”-nätverk (IP VPN), ett ”point-to-point”-nätverk med Ethernet eller en virtuell korsanslutning via en anslutningsleverantör på en samlokaliseringsanläggning.

ExpressRoute-anslutningar inte överskrider hello offentliga Internet och därför kan anses vara säkrare än VPN-baserade lösningar. Detta tillåter ExpressRoute anslutningar toooffer mer tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar via hello Internet.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) ger en [programmet leverans domänkontrollant (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) som en tjänst som erbjuder olika layer 7 belastningsutjämning för ditt program.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Det gör toooptimize web servergruppen produktivitet genom att avlasta CPU beräkningsintensiva SSL-avslutning toohello Programgateway (även kallat ”SSL-avlastning” eller ”SSL-bryggning”). Här finns även andra lager 7 funktioner inklusive resursallokering distribution av inkommande trafik, cookie-baserad session tillhörighet, URL-sökväg-baserad Routning och hello möjlighet toohost flera webbplatser bakom en enda Application Gateway. Azure Application Gateway är en Layer 7-belastningsutjämnare.

Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.

Programmet innehåller många program leverans domänkontrollant (ADC) funktioner inklusive HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet [Secure Sockets Layer (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) -avlastning, anpassade hälsoavsökningar, stöd för flera platser och många andra.

### <a name="web-application-firewall"></a>Brandvägg för webbaserade program
Brandvägg för webbaserade program är en funktion i [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) som ger skydd tooweb program som använder Programgateway för standardfunktioner programmet leverans kontrollen (ADC). Brandvägg för webbaserade program gör detta genom att skydda dem mot de flesta av hello OWASP översta 10 vanliga web säkerhetsproblem.

![Brandvägg för webbaserade program](./media/azure-security/azure-security-fig1.png)

-   Skydd mot SQL-inmatning

-   Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil

-   Skydd mot åtgärder som inte följer HTTP-protokollet

-   Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas

-   Skydd mot robotar, crawlers och skannrar

-   Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)


Centraliserad web application brandväggen tooprotect mot webbattacker gör säkerhetshantering mycket enklare och ger bättre säkerhet toohello programmet mot hello hot om intrång. En Brandvägg-lösning kan också reagera tooa säkerhetshot snabbare med uppdatering av ett känt problem på en central plats jämfört med att skydda alla enskilda webbprogram. Befintliga program gateways kan konverteras enkelt tooan Programgateway med Brandvägg för webbaserade program.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) kan du toocontrol hello distributionen av användartrafik för slutpunkter i olika datacenter. Service-slutpunkter som stöds av Traffic Manager innehåller virtuella Azure-datorer, webbprogram och molntjänster. Du kan även använda Traffic Manager med externa slutpunkter som inte tillhör Azure. Traffic Manager använder hello System DNS (Domain Name) toodirect klient begär toohello lämpligaste slutpunkten baserat på en [routning av nätverkstrafik metoden](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) och hello hälsa hello slutpunkter.

Traffic Manager erbjuder en uppsättning routning av nätverkstrafik metoder toosuit olika programbehov, endpoint hälsa [övervakning](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), och automatisk redundans. Traffic Manager är flexibel toofailure, inklusive hello fel på en hel Azure-region.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure belastningsutjämnare](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) ger hög tillgänglighet och nätverksprestanda tooyour program. Det är en belastningsutjämnare för lager 4 (TCP, UDP) som distribuerar inkommande trafik mellan felfri instanser av tjänster som definierats i en belastningsutjämnad uppsättning. Azure belastningsutjämnare kan konfigureras för att:

-   Läsa belastningsutjämna inkommande Internet-trafik toovirtual datorer. Den här konfigurationen kallas [mot Internet för belastningsutjämning](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Läs in Utjämna trafiken mellan virtuella datorer i ett virtuellt nätverk, mellan virtuella datorer i molntjänster eller mellan lokala datorer och virtuella datorer i ett virtuellt nätverk mellan platser. Den här konfigurationen kallas [intern belastningsutjämning](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Vidarebefordra externa trafiken tooa specifik virtuell dator

### <a name="internal-dns"></a>Internt DNS
Du kan hantera hello lista över DNS-servrar som används i ett VNet i hello Management Portal eller i konfigurationsfilen för hello nätverk. Kunden lägga too12 DNS-servrar för varje virtuellt nätverk. När du anger DNS-servrar, är det viktigt tooverify du lista kundens DNS-servrar i hello rätt ordning för kundens miljö. DNS-servern visar fungerar inte resursallokering. De används i hello ordning som de anges. Om hello första DNS-servern i hello listan kan toobe uppnåtts använder hello klienten DNS-servern oavsett om hello DNS-servern fungerar korrekt eller inte. toochange hello DNS-server för kundens virtuella nätverk, ta bort DNS-servrar för hello hello listan och lägga till dem i ordning hello kunden vill. DNS stöder hello tillgänglighet aspekt av hello ”CIA” säkerhet triad.

### <a name="azure-dns"></a>Azure DNS
Hej [Domain Name System](https://technet.microsoft.com/library/bb629410.aspx), DNS, eller översätter (eller lösa) en webbplats eller tjänst name tooits IP-adress. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) är en tjänst som är värd för DNS-domäner är att tillhandahålla namnmatchning med hjälp av Microsoft Azure-infrastrukturen. Värd för dina domäner i Azure, kan du hantera din DNS hello poster med samma autentiseringsuppgifter, API: er, verktyg och fakturering som andra Azure-tjänster. DNS stöder hello tillgänglighet aspekt av hello ”CIA” säkerhet triad.
### <a name="log-analytics-nsgs"></a>Log Analytics NSG: er
Du kan aktivera hello följande diagnostiska loggen kategorier för NSG: er:
-   : Innehåller poster för vilka NSG regler är tillämpade tooVMs och instans roller baserat på MAC-adress. hello status för de här reglerna samlas var 60: e sekund.

-   Regler för räknaren: innehåller poster för hur många gånger varje NSG-regel är tillämpade toodeny eller Tillåt trafiken.

### <a name="azure-security-center"></a>Azure Security Center
Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger dig utökade överblick och kontroll över, hello säkerheten för dina Azure-resurser. Det ger integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer, och upptäcka hot som annars kanske skulle förbli oupptäckta, fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar. Rekommendationer Nätverkscenter runt brandväggar, Nätverkssäkerhetsgrupper, konfigurera regler för inkommande trafik och mycket mer.

Tillgängliga nätverksrekommendationer är följande:

-   [Lägg till nästa generations brandvägg](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) rekommenderar att du lägger till en nästa generations Brandvägg från en Microsoft-partner tooincrease din säkerhetsskydd

-   [Vidarebefordra trafik via nästa generations Brandvägg endast](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) rekommenderar att du konfigurerar nätverk regler för nätverkssäkerhetsgrupper (NSG) som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg.

-   [Aktivera Nätverkssäkerhetsgrupper för undernät eller virtuella datorer](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) rekommenderar att du aktiverar NSG: er på undernät eller virtuella datorer.

-   [Begränsa åtkomst via Internetuppkopplad slutpunkt](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) rekommenderar att du konfigurerar regler för inkommande trafik för NSG: er.


## <a name="compute"></a>Compute

hello avsnitt innehåller ytterligare information om viktiga funktioner i det här området och sammanfattning information om dessa funktioner.

### <a name="antimalware--antivirus"></a>Program mot skadlig kod & Antivirus
Du kan använda dina virtuella datorer från skadliga filer, annonsprogram och andra hot program mot skadlig kod från säkerhet leverantörer, till exempel Microsoft, Symantec, Trend Micro, McAfee och Kaspersky tooprotect med Azure IaaS. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) för Azure-molntjänster och virtuella datorer är en funktion av skydd som hjälper dig att identifiera och ta bort virus, spionprogram och annan skadlig programvara. Microsoft Antimalware innehåller konfigurerbara aviseringar när känd skadlig eller oönskad programvara försöker tooinstall själva eller köras på din Azure-system. Microsoft Antimalware kan också distribueras med hjälp av Azure Security Center

### <a name="hardware-security-module"></a>Maskinvarusäkerhetsmodul
Kryptering och du inte öka säkerheten hello nycklar sig själva som är skyddade. Du kan förenkla hello hantering och säkerhet för dina kritiska hemligheter och nycklar genom att lagra dem i [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault ger hello alternativet toostore dina nycklar i maskinvara moduler (HSM) 140-2-certifierade tooFIPS nivå 2 säkerhetskrav. Din SQL Server krypteringsnycklarna för säkerhetskopiering eller [transparent datakryptering](https://msdn.microsoft.com/library/bb934049.aspx) kan alla lagras i Nyckelvalvet med alla nycklar och hemligheter från ditt program. Behörigheter och komma åt toothese skyddade objekt hanteras via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Säkerhetskopiering av virtuell dator
[Azure-säkerhetskopiering](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) är en lösning som skyddar dina programdata med noll kapitalinvesteringar och minimala driftskostnader. Programfel kan förstöra dina data och mänskliga fel kan introducera buggar i dina program som kan leda till problem med toosecurity. Dina virtuella datorer som kör Windows och Linux är skyddade med Azure Backup.

### <a name="azure-site-recovery"></a>Azure Site Recovery
En viktig del av din organisations [business continuity/haveriberedskap (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) strategi är att räkna ut hur tookeep företagets arbetsbelastningar och appar upp och körs när du planerade och oplanerade avbrott inträffar. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) hjälper till att samordna replikering, redundans och återställning av arbetsbelastningar och appar så att de är tillgängliga från en sekundär plats om den primära platsen kraschar.

### <a name="sql-vm-tde"></a>DATAKRYPTERINGSPRINCIP FÖR VM
[Transparent datakryptering (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) och kryptering på kolumnen (radera) är SQL server-funktioner för kryptering. Den här typen av kryptering kräver kunder toomanage och lagra hello Kryptografiska nycklar som du använder för kryptering.

hello Azure Key Vault (AKV)-tjänsten är utformad tooimprove hello säkerhet och hantering av dessa nycklar i en säker och hög tillgänglighet på. hello SQL Server-anslutningen kan SQL Server toouse nycklarna från Azure Key Vault.

Om du kör SQL Server med lokala datorer, finns steg som du kan följa tooaccess Azure Key Vault från din lokala SQL Server-datorn. Men för SQL Server i Azure virtuella datorer sparar du tid genom att använda funktionen för hello Azure Key Vault-integrering. Med några Azure PowerShell-cmdlets tooenable funktionen kan du automatisera hello konfiguration krävs för en SQL-VM tooaccess nyckelvalvet.

### <a name="vm-disk-encryption"></a>VM-diskkryptering
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) är en ny funktion som hjälper dig att kryptera din Windows- och Linux IaaS virtuella diskar. Det gäller hello branschen standard BitLocker för Windows och hello DM-Crypt i Linux tooprovide volymkryptering för hello OS och hello datadiskar. hello-lösning är integrerad med Azure Key Vault toohelp du styra och hantera hello-diskkryptering nycklar och hemligheter i Nyckelvalvet-prenumeration. hello lösning betyder också att krypteras alla data på hello virtuella diskar i vila i ditt Azure storage.

### <a name="virtual-networking"></a>Virtuella nätverk
Virtuella datorer måste nätverksanslutningen. toosupport kräver detta krav, Azure virtuella datorer toobe anslutna tooan Azure Virtual Network. Ett virtuellt Azure-nätverk är en logisk konstruktion som bygger på hello fysisk Azure nätverksinfrastruktur. Varje logiska [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) isoleras från alla andra virtuella Azure-nätverk. Denna isolering kan försäkra dig om att nätverkstrafik i din distribution inte är tillgänglig tooother Microsoft Azure-kunder.

### <a name="patch-updates"></a>Patch-uppdateringar
Patch-uppdateringar ange hello grunden för att hitta och åtgärda eventuella problem och förenkla hello management programuppdateringsprocessen både genom att minska hello antalet programuppdateringar måste du distribuera i ditt företag och genom att öka din möjlighet toomonitor efterlevnad.

### <a name="security-policy-management-and-reporting"></a>Hantering av säkerhetsprinciper och rapportering
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger du ökad insyn i, och kontroll över, hello säkerheten för dina Azure-resurser. Det ger integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer, och upptäcka hot som annars kanske skulle förbli oupptäckta, fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

### <a name="azure-security-center"></a>Azure Security Center
Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

## <a name="identify-and-access-management"></a>Identifiera och åtkomsthantering

Skydda system, program och data börjar med identitetsbaserade åtkomstkontroller. hello identitets- och hanteringsfunktioner som är inbyggda i Microsoft-produkter och tjänster att skydda din organisations- och personlig information från obehörig åtkomst när du gör den tillgänglig toolegitimate användare var och när Du behöver den.

### <a name="secure-identity"></a>Skydda identitet
Microsoft använder flera säkerhetspraxis och tekniker på Microsofts produkter och tjänster toomanage identitet och åtkomst.
-   [Multifaktorautentisering](https://azure.microsoft.com/services/multi-factor-authentication/) kräver användare toouse flera metoder för åtkomst, lokalt och i hello molnet. Det ger stark autentisering med ett antal alternativ för enkel verifiering när källändringshastigheten användare med en process för enkel inloggning.

-   [Microsoft Authenticator](https://aka.ms/authenticator) tillhandahåller en användarvänlig Multifaktorautentisering som fungerar med både Microsoft Azure Active Directory och Microsoft-konton och har stöd för wearables och fingeravtryck-baserade godkännanden.

-   [Tvingande principer för lösenord](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) ökar hello säkerheten för traditionella lösenord genom att införa längd och komplexitet krav, tvingas periodiska rotation och kontoutelåsning efter misslyckade försök.

-   [Token-baserad autentisering](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) möjliggör autentisering via Active Directory Federation Services (AD FS) eller säkra token från tredje part-system.

-   [Rollbaserad åtkomstkontroll (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) aktiverar du toogrant åtkomst baserat på hello användaren tilldelas rollen, vilket gör det enkelt toogive användare endast hello åtkomstnivå de behöver tooperform sina arbetsuppgifter. Du kan anpassa RBAC per organisationens affärsmodell och risktolerans.

-   [Integrerad Identitetshantering (hybrididentitet)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) kan du toomaintain kontroll över användarnas åtkomst på interna datacenter och moln plattformar och skapa en enda användaridentitet för autentisering och auktorisering tooall resurser.

### <a name="secure-apps-and-data"></a>Skydda data och appar
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), en omfattande identitets- och molnlösning, hjälper till att skydda åtkomsten toodata i program på platsen och hello molnet och förenklar hello hantering av användare och grupper. Det kombinerar core katalogtjänster, avancerad identitet styrning, säkerhet och hantering av åtkomst, och gör det lätt för utvecklare toobuild principbaserad Identitetshantering i sina appar. tooenhance Azure Active Directory, kan du lägga till betalda funktioner med hjälp av hello Azure Active Directory Basic, Premium P1 och Premium P2-versioner.

| Ledigt / vanliga funktioner     | Grundläggande funktioner    |Premium P1-funktioner |Premium P2-funktioner | Azure Active Directory Join – endast relaterade funktioner för Windows 10|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Katalogobjekt](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#directory-objects), [(Lägg till/Uppdatera/ta bort) för hantering av användare/grupp / användarbaserade etablering, enhetsregistrering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#usergroup-management-addupdatedelete-user-based-provisioning-device-registration), [enkel inloggning (SSO)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#single-sign-on-sso), [självbetjäning Ändra lösenordet för dina molnanvändare](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-change-for-cloud-users), [Connect (Synkroniseringsmotorn som utökar lokala kataloger tooAzure Active Directory)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory), [säkerhet / rapporter](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#securityusage-reports)       |     [Gruppbaserad åtkomsthantering / etablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#group-based-access-managementprovisioning), [Självbetjäning för återställning av lösenord för dina molnanvändare](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-reset-for-cloud-users), [företagsanpassning (inloggning sidor/åtkomstpanelen anpassning)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#company-branding-logon-pagesaccess-panel-customization), [Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#application-proxy), [SLA 99,9%](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#sla-999) |  [Self-Service Group och app Management/egen-Service programmet tillägg/dynamiska grupper](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-group), [Self-Service lösenord återställning/ändra/Unlock med lokala återskrivning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-resetchangeunlock-with-on-premises-write-back), [Multi-Factor Authentication (molnet och lokalt (MFA-Server))](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#multi-factor-authentication-cloud-and-on-premises-mfa-server), [MIM CAL + MIM Server](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mim-cal-mim-server), [Cloud App Discovery](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#cloud-app-discovery), [Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-health), [lösenord för automatisk förnyelse för gruppkonton](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#automatic-password-rollover-for-group-accounts)|     [Identitetsskydd](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-configure)|    [Ansluta till en enhet tooAzure AD, skrivbordet SSO Microsoft Passport for Azure AD, administratör Bitlocker-återställning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery), [MDM automatisk registrering, Self-Service Bitlocker-återställning, ytterligare lokala administratörer tooWindows 10-enheter via Azure AD-anslutning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mdm-auto-enrollment)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) är en premium-funktion i Azure Active Directory som gör att du tooidentify molnprogram som används av hello anställda i din organisation.

- [Azure Active Directory-identitetsskydd](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) är en security-tjänst som använder Azure Active Directory avvikelseidentifiering identifiering funktioner tooprovide en samlad vy riskhändelser och potentiella sårbarheter som kan påverka din organisations identiteter.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kan du toojoin virtuella Azure-datorer tooa domän utan hello måste toodeploy domänkontrollanter. Användare loggar in toothese virtuella datorer med sina företagsuppgifter för Active Directory och sömlöst kan komma åt resurser.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) är en hög tillgänglighet, globala identity management-tjänsten för konsumentinriktade program som kan skalas toohundreds miljoner identiteter och integrera olika mobila och plattformar. Kunderna kan logga in tooall dina appar via anpassningsbara funktioner som använder den befintliga sociala medier konton eller skapa nya fristående autentiseringsuppgifter.

- [Azure Active Directory B2B-samarbete](https://aka.ms/aad-b2b-collaboration) är en lösning för integrering av säker partner som har stöd för dina företagsomfattande relationer genom att aktivera samarbetar tooaccess företagets program och data selektivt med hjälp av sina självhanterad identiteter.

- [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) kan du tooextend moln funktioner tooWindows 10 enheter för central hantering. Det gör det möjligt för användare tooconnect toohello företagets eller organisationens moln via Azure Active Directory och förenklar åtkomst tooapps och resurser.

- [Azure Active Directory Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) ger enkel inloggning och säker fjärråtkomst för webbprogram finns lokalt.

## <a name="next-steps"></a>Nästa steg
- [Komma igång med Microsoft Azure-säkerhet](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Azure-tjänster och funktioner som du kan använda toohelp skydda dina data i Azure och tjänster

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

Förebygga, upptäcka och åtgärda toothreats med ökad synlighet och kontroll över hello säkerheten för dina Azure-resurser

- [Övervakning av säkerhetshälsa i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

hello övervakningsfunktionerna i Azure Security Center toomonitor följer principerna.
