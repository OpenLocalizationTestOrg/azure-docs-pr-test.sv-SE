---
title: "aaaAzure operativa säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning av bästa praxis för Azure operativ säkerhet."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: b3b17ef20fb3545b1c268ac0d7ce692e07c8da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-best-practices"></a>Azure Operational säkerhetsmetoder
Azure operativ säkerhet refererar toohello tjänster, kontroller och funktioner tillgängliga toousers för att skydda sina data, program och andra resurser i Microsoft Azure. Azure operativ säkerhet bygger på ett ramverk som inkluderar hello kunskap via olika funktioner som är unika tooMicrosoft, inklusive hello Microsoft Security Development Lifecycle (SDL), hello Microsoft Security Response Center programmet och djup medvetenhet om hello cybersecurity hotbild.

I den här artikeln tar vi upp en samling Azure-databas säkerhetsmetoder. Följande rekommendationer härleds från våra upplevelse med säkerhet på Azure-databas och hello erfarenheter från kunder som dig själv.

För varje bästa praxis förklarar vi:
-   Vilka hello bra
-   Varför du vill tooenable som bästa praxis
-   Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
- Hur du kan lära dig tooenable hello bästa praxis

Den här artikeln Azure Operational säkerhetsmetoder baseras på en konsensus åsikt och Azure plattformsfunktioner och funktioner, som de finns på hello tid som den här artikeln skrevs. Åsikter och tekniker ändras med tiden och den här artikeln kommer att uppdateras på ett regelbundet tooreflect ändringarna.

Azure Operational Metodtips om säkerhet i den här artikeln omfattar:

-   Övervaka, hantera och skydda moln-infrastruktur
-   Hantera identitet och implementera enkel inloggning (SSO)
-   Spåra begäranden, analysera användningstrender och diagnostisera problem
-   Övervaka tjänster med en centraliserad övervakningslösning
-   Förebygga, upptäcka och åtgärda toothreats
-   Slutpunkt till slutpunkt scenariobaserade nätverksövervakning
-   Säker distribution med beprövade DevOps-verktyg

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Övervaka, hantera och skydda moln-infrastruktur
IT-avdelningen ansvarar för att hantera datacenter infrastruktur, program och data, inklusive hello stabilitet och säkerhet för dessa system. Få säkerhetsinsikter över öka komplexa IT-miljöer ofta kräver dock organisationer toocobble ihop data från flera system för säkerhet och hantering.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.

[OMS säkerhet och granska lösningen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) möjliggör IT tooactively övervaka alla resurser som hjälper dig att minimera hello konsekvenserna av säkerhet. OMS säkerhet och granska har säkerhetsdomäner som kan användas för att övervaka resurser.

Mer information om OMS artikeln hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

toohelp du förebygga, upptäcka och åtgärda toothreats, [Operations Management Suite (OMS) säkerhets- och granska lösningen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) samlar in och bearbetar data om dina resurser, som omfattar:

-   Säkerhetshändelselogg
-   ETW-händelser (Event Tracing for Windows)
-   AppLocker-granskningshändelser
-   Windows-brandväggslogg
-   Advanced Threat Analytics-händelser
-   Resultat från utvärdering av säkerhetsbaslinje
-   Resultat från utvärdering av program mot skadlig kod
-   Resultat från utvärdering av uppdateringar/korrigeringar
-   Syslog-dataströmmar som explicit har aktiverats på hello-agent


## <a name="manage-identity-and-implement-single-sign-on"></a>Hantera identitet och implementera enkel inloggning
[Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten.

[Azure AD](https://azure.microsoft.com/services/active-directory/) innehåller också en fullständig uppsättning [Identitetshantering](https://docs.microsoft.com/azure/security/security-identity-management-overview) funktioner inklusive [multifaktorautentisering](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), enhetsregistrering, självbetjäning lösenordshantering grupphantering via självbetjäning, privilegierat kontohantering, rollbaserad åtkomstkontroll, övervakning, omfattande granskning och säkerhet övervakning och avisering för programanvändning.

hello följande funktioner kan hjälpa säkra molnbaserade program, effektivisera IT-processer, minska kostnaderna och se till att företagsefterlevnad mål är uppfyllda:

-   Identitets- och åtkomsthantering för hello moln
-   Förenkla användaren åtkomst tooany molnappen
-   Skydda känsliga data och program
-   Ge personalen möjlighet att arbeta självständigt
-   Integrera med Azure Active Directory

### <a name="identity-and-access-management-for-hello-cloud"></a>Identitets- och åtkomsthantering för hello moln
Azure Active Directory (AD Azure) är en omfattande [identitets- och molnet hanteringslösning](https://www.microsoft.com/cloud-platform/identity-management), som ger dig en stabil uppsättning funktioner toomanage användare och grupper. Det hjälper att skydda åtkomst tooon lokala program och molnprogram, inklusive Microsoft web-tjänster som Office 365 och mycket icke-Microsoft-programvara som en tjänst (SaaS)-program.
toolearn mer hur tooenable identitetsskydd i Azure AD, se [aktiverar Azure Active Directory identitetsskydd](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-tooany-cloud-app"></a>Förenkla användaren åtkomst tooany molnappen
[Aktivera enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) toosimplify användaren åtkomst toothousands av molnprogram från Windows, Mac, Android och iOS-enheter. Användare kan starta program från en anpassad webbaserade åtkomstpanelen eller mobila appar med företagets autentiseringsuppgifter. Använd hello Azure AD Application Proxy modulen toogo utöver SaaS-program och publicera lokala webbprogram program tooprovide mycket säker fjärråtkomst och enkel inloggning.

### <a name="protect-sensitive-data-and-applications"></a>Skydda känsliga data och program
Aktivera [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) tooprevent obehörig åtkomst till tooon lokala program och molnprogram genom att tillhandahålla en extra nivå av autentisering. Skydda ditt företag och identifiera potentiella hot med säkerhetsövervakning, varningar och maskininlärningsbaserade rapporter som visar avvikande åtkomstmönster.

### <a name="enable-self-service-for-your-employees"></a>Ge personalen möjlighet att arbeta självständigt
Delegera viktiga uppgifter tooyour anställda, till exempel återställa lösenord och skapa och hantera grupper. [Aktivera självbetjäning lösenordsändring](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), Återställ och självbetjäning hantering med Azure AD.

### <a name="integrate-with-azure-active-directory"></a>Integrera med Azure Active Directory
Utöka [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) och andra lokala kataloger tooAzure AD tooenable enkel inloggning för alla molnbaserade program. Användarattribut kan vara automatiskt synkroniserade tooyour molnkatalog från alla typer av lokala kataloger.

toolearn mer om Azure Active Directory-integrering och hur tooenable, Läs hello artikeln [integrera dina lokala kataloger med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Spåra begäranden, analysera användningstrender och diagnostisera problem
[Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) utför loggning och tillhandahåller data för mått för ett lagringskonto. Du kan använda den här tootrace begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto.

Storage Analytics mätvärden är aktiverade som standard för nya storage-konton. Du kan aktivera loggning och konfigurera mått och loggar in hello Azure-portalen; Mer information finns i [övervaka ett lagringskonto i hello Azure-portalen](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Du kan också aktivera Storage Analytics programmässigt via hello REST API eller klientbibliotek för hello. Använd hello ange tjänstegenskaper åtgärden tooenable Storage Analytics individuellt för varje tjänst.

Utförliga instruktioner om hur du använder Storage Analytics och andra verktyg tooidentify, diagnostisera i, och felsöka problem med Azure Storage, se [övervaka, diagnostisera och felsöka Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

toolearn mer om Azure Active Directory-integrering och hur tooenable, läsa hello artikel [aktivera och konfigurera Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Övervaka tjänster
Molnprogram är komplicerade med många rörliga delar. Övervakning innehåller data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd. Toostave av eventuella problem eller felsöka efter de som hjälper också till. Du kan dessutom använda övervakning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.

### <a name="monitor-azure-resources"></a>Övervaka Azure-resurser
[Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) är hello platform-tjänst som tillhandahåller en enda källa för övervakning av Azure-resurser. Med Azure-Monitor kan du visualisera, fråga, vidarebefordra, arkivera och vidta åtgärder för hello mått och loggar som kommer från resurser i Azure. Du kan arbeta med dessa data med hello övervakaren portalbladet och [övervakaren PowerShell-Cmdlets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [plattformsoberoende CLI](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples), eller [Azure övervakaren REST API: er](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Aktivera Autoskala med Azure Övervakare
Aktivera [Azure övervakaren Autoskala](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) gäller endast toovirtual machine-skalningsuppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer.

### <a name="manage-roles-permissions-and-security"></a>Hantera roller behörigheter och säkerhet
Många grupper behöver toostrictly [reglera åtkomst toomonitoring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) data och inställningar. Till exempel om du har gruppmedlemmar som arbetar enbart om hur du övervakar (supporttekniker, devops engineers) eller om du använder en leverantör av hanterade tjänster, kanske du vill toogrant dem åtkomst till övervakningsdata samtidigt begränsa deras möjlighet toocreate tooonly, ändra, eller ta bort resurser.

Här visas hur tooquickly tillämpa en inbyggd övervakning RBAC rollen tooa användare i Azure eller skapa egna anpassade roll för en användare behöver begränsade behörigheter för övervakning. Den sedan behandlar säkerhetsaspekter för dina Azure-Monitor-relaterade resurser och hur du kan begränsa åtkomst till toohello data de innehåller.

## <a name="prevent-detect-and-respond-toothreats"></a>Förebygga, upptäcka och åtgärda toothreats
Hotidentifiering Security Center fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverks- och anslutna partnerlösningar. Det analyserar informationen ofta korrelerar information från flera källor, tooidentify hot. Säkerhetsaviseringar prioriteras i Security Center tillsammans med rekommendationer om hur tooremediate hello hot.

-   [Konfigurera en säkerhetsprincip för](https://docs.microsoft.com/azure/security-center/security-center-policies) för din Azure-prenumeration.
-   Använd hello [rekommendationer i Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) toohelp som du skyddar dina Azure-resurser.
-   Granska och hantera din aktuella [säkerhetsaviseringar](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kan du förebygga, upptäcka och svara toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Security Center har lätt att använda och effektivt hot förhindra, upptäcka och svar-funktioner som är inbyggda i tooAzure. Här är de viktigaste funktionerna:

-   Förstå hälsotillstånd för moln-säkerhet
-   Ta kontroll över molnsäkerhet
-   Distribuera enkelt integrerade molnsäkerhetslösningar
-   Upptäck hot och svara snabbt

### <a name="understand-cloud-security-state"></a>Förstå hälsotillstånd för moln-säkerhet
Använda Azure Security Center tooget en central vy över hello säkerhetsläget för alla dina Azure-resurser. Kontrollera att hello lämpliga säkerhetsåtgärder är uppfyllda och korrekt konfigurerad och snabbt identifiera alla resurser som kräver uppmärksamhet på ett ögonblick.

### <a name="take-control-of-cloud-security"></a>Ta kontroll över molnsäkerhet
Definiera [säkerhetsprinciper](https://docs.microsoft.com/azure/security-center/security-center-policies) för Azure-prenumerationer enligt tooyour företag har cloud security behov, skräddarsydda toohello typ av program eller känslighet hello data i varje prenumeration. Använda principer rekommendationer tooguide resurs-ägare genom hello av implementera nödvändiga kontroller – ta hello gissa molnet säkerhet.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Distribuera enkelt integrerade molnsäkerhetslösningar
[Aktivera säkerhetslösningar](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) från Microsoft och dess partners, inklusive branschledande brandväggar och program mot skadlig kod. Använd effektiv allokering toodeploy säkerhetslösningar – även nätverk ändringar är konfigurerade för dig. Dina säkerhetshändelser från partnerlösningar samlas automatiskt in för analys och varning.

### <a name="detect-threats-and-respond-fast"></a>Upptäck hot och svara snabbt
Ligg före aktuella och kommande molnhot med en integrerad, analysdriven metod. Genom att kombinera Microsoft global [hot intelligence](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) och kunskaper med insikter om molnet säkerhetshändelser över Azure-distributioner, Security Center hjälper dig att identifiera tidigt verkliga hot och falsklarm. Molnet säkerhetsaviseringar ger dig insikter om hello attack kampanj, inklusive relaterade händelser och vilka resurser och föreslår sätt tooremediate utfärdar och snabbt återställa.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Slutpunkt till slutpunkt scenariobaserade nätverksövervakning
Kunder kan du skapa ett nätverk i Azure genom att samordna och skapa olika enskilda nätverksresurser, till exempel virtuella nätverk, ExpressRoute, Programgateway, belastningsutjämnare och flera. Övervakning är tillgängligt på varje hello nätverksresurser.

[Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på en nivå på scenariot för nätverk i till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Automatisera övervakning av fjärrnätverk med infångade paket
Övervaka och diagnostisera problem utan att logga i tooyour virtuella datorer (VM) med hjälp av Nätverksbevakaren. Utlösaren [paketinsamling](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) genom att konfigurera aviseringar och få åtkomst tooreal tid prestandainformation på paketnivå hello. När du ser ett problem kan du undersöka i detalj för att få bättre diagnoser.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Få insikt i din nätverkstrafik med flödesloggar
Skapa en bättre förståelse av din trafik mönster med [Nätverkssäkerhetsgruppen flöde loggar](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Information som tillhandahålls av flödet loggar hjälper dig att samla in data för godkännande, granskning och övervakning av din säkerhet nätverksprofil.

### <a name="diagnose-vpn-connectivity-issues"></a>Diagnostisera VPN-anslutningsproblem
Nätverksbevakaren ger du hello möjligheten för[diagnostisera dina mest vanliga problem med VPN-Gateway och anslutningar](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Hello detaljerade så att du inte bara tooidentify hello problem utan också toouse loggar skapade toohelp ytterligare undersöka.

Mer om hur toolearn tooconfigure nätverksbevakaren och hur tooenable, Läs hello artikeln [konfigurera nätverksbevakaren](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Säker distribution med beprövade DevOps-verktyg
Här är några av hello lista av Azure DevOps metoder i det här Microsoft Cloud-utrymme, vilket gör att företag och team produktiv och effektiv.

-   **Infrastruktur som kod (IaC):** infrastruktur som koden är en uppsättning tekniker och metoder som hjälper IT-proffs ta bort hello belastningen som är associerade med hello dag tooday bygg- och hantering av modulära infrastruktur. Det gör att IT-proffs toobuild och underhålla sina moderna servermiljö på ett sätt som liknar hur för utvecklare att bygga och underhålla programkod. För Azure, har vi [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) kan du tooprovision dina program med en deklarativ mall. I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden. Du använder hello samma mall toorepeatedly distribuera programmet under varje steg i livscykeln för hello-programmet.
-   **Kontinuerlig integrering och distribution:** kan du konfigurera din Visual Studio Online grupprojekt för[automatiskt skapa och distribuera](https://www.visualstudio.com/docs/build/overview) tooAzure webbappar eller molntjänster. VSO distribuerar automatiskt hello binärfiler när du har gjort en build tooAzure efter varje kod incheckning. hello paketet build process som beskrivs här är likvärdiga toohello kommandot i Visual Studio och hello publishing stegen är likvärdiga toohello med kommandot Publicera i Visual Studio.
-   **Versionshantering:** Visual Studio [släpper Management](https://msdn.microsoft.com/library/vs/alm/release/overview) är en bra lösning för att automatisera distributionen av flera steg och hantera hello utgivningen. Skapa hanterade kontinuerlig distribution pipelines toorelease snabbt, enkelt och ofta. Vi kan mycket automatisera våra versionen med släpper Management och vi kan fördefinierade godkännandearbetsflöden. Distribuera lokalt och toohello molnet, utöka och anpassa efter behov.
-   **Appen prestanda övervakning:** identifiera problem, lösa problem och kontinuerligt förbättra dina program. Diagnostisera snabbt problem i ditt liveprogram. Förstå vad användarna gör med det. Konfigurationen är enkelt frågan för att lägga till JS kod och en webconfig-post och du endast ser resultat inom minuter i hello portal med alla hello information. [App insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) hjälper företag för snabbare identifiering av problem & reparation.
-   **Läsa in testar & Autoskala:** vi hittar prestandaproblem i vår app tooimprove distribution kvalitet och toomake till vår app alltid är igång eller tillgängliga toocater toohello affärsbehov. Kontrollera att din app kan hantera trafik för nästa start eller marknadsföring kampanjen. Börja köra molnbaserade [tester för att läsa in](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) nästan ingen tid med Visual Studio Online.

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [Azure operativ säkerhet](https://docs.microsoft.com/azure/security/azure-operational-security).
- tooLearn mer [Operations Management Suite | Säkerhet och efterlevnad](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Komma igång med Operations Management Suite säkerhet och granska lösningen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).
