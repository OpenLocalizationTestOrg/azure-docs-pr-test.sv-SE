---
title: aaaSecurity Center planerings- och bruksanvisning | Microsoft Docs
description: "Det här dokumentet hjälper dig att tooplan inför införandet av Azure Security Center och överväganden för den dagliga verksamheten."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: b0a0a6f5fd56fbd46f7736928c99e3bcd0b1e140
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-planning-and-operations-guide"></a>Planerings- och användningsguide för Azure Security Center
Den här guiden är för (IT) experter, IT-arkitekter, informationssäkerhetsanalytiker och molnet administratörer vars organisationer planerar toouse Azure Security Center.

>[!NOTE] 
>Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>

## <a name="planning-guide"></a>Planeringsanvisningar
Den här handboken innehåller en uppsättning steg och aktiviteter som du kan följa toooptimize din användning av Security Center utifrån din organisations säkerhetskrav och molnhanteringsmodell. tootake nytta av Security Center, det är viktigt toounderstand hur olika medarbetare eller team i din organisation använder hello service toomeet säker utvecklingsarbetet, driften, övervakningen, styrningen och incidenthanteringen. viktiga områden hello tooconsider när du planerar toouse Security Center är:

* Säkerhetsroller och åtkomstkontroll
* Säkerhetsprinciper och säkerhetsrekommendationer
* Datainsamling och lagring
* Fortlöpande säkerhetsövervakning
* Incidenthantering

I nästa avsnitt om hello, får du lära dig hur tooplan för var och en av dessa områden och tillämpar rekommendationerna baserat på dina krav.

> [!NOTE]
> Läs [Azure Security Center vanliga frågor (FAQ)](security-center-faq.md) en lista över vanliga frågor som kan vara bra hello design och planering fasen.
> 

## <a name="security-roles-and-access-controls"></a>Säkerhetsroller och åtkomstkontroll
Beroende på hello storlek och strukturen för din organisation kan använda olika medarbetare och avdelningar Security Center tooperform olika säkerhetsrelaterade uppgifter. I följande diagram hello finns ett exempel på fiktiva personer och deras respektive roller och ansvarsområden:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Security Center kan dessa personer toomeet de här olika ansvarsområdena. Exempel:

**Jens (molnansvarig)**

* Hanterar en arbetsbelastning i molnet och relaterade resurser.
* Ansvarig för att implementera och underhålla säkerheten i enlighet med företagets säkerhetspolicy.

**Elisabeth (IT-chef)**

* Ansvarig för alla aspekter av säkerhet för hello företag
* Om du vill ha toounderstand hello företagets säkerhetstillståndet över molnarbetsbelastningar
* Måste toobe känner till större attacker och risker

**Daniel (IT-säkerhetsansvarig)**

* Anger företagets säkerhetsprinciper tooensure hello lämpliga finns appnivåskydd
* Övervakar principefterlevnaden.
* Genererar rapporter till chefer eller granskare.

**Selma (säkerhetsmedarbetare)**

* Övervakar och svarar toosecurity aviseringar 24/7
* Eskalerar tooCloud arbetsbelastning ägare eller analytiker för IT-säkerhet

**Sami (säkerhetsanalytiker)**

* Undersöker attacker.
* Arbeta med Molnansvarig tooapply reparation 

Security Center använder [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md), vilket ger [inbyggda roller](../active-directory/role-based-access-built-in-roles.md) som kan tilldelas toousers, grupper och tjänster i Azure. När en användare öppnar Security Center, ser de bara information relaterade tooresources som de har åtkomst till. Vilket innebär att hello användaren har tilldelats hello rollen som ägare, deltagare eller läsare toohello prenumerationen eller resursen gruppen som en resurs tillhör. Det finns två specifika roller i Security Center i tillägg toothese roller:

- **Säkerhet reader**: användare som tillhör toothis roll är att kan tooview rättigheter tooSecurity Center, som innehåller rekommendationer, aviseringar, princip och hälsa, men den fungerar inte kan toomake ändringar.
- **Säkerhet admin**: samma som säkerhet reader, men det kan också uppdatera hello säkerhetsprincip stänga rekommendationer och aviseringar.

hello Security Center roller som beskrivs ovan har inte åtkomst tooother service områden i Azure, till exempel lagring, webb & Mobile eller Sakernas Internet.  

> [!NOTE]
> En användare behöver toobe minst en prenumerationen eller resursen gruppägare deltagare toobe kan toosee Security Center i Azure. 
> 
> 

Med hjälp av hello personer beskrivs i hello föregående diagram, hello som behövs för följande ROLLER:

**Jens (molnansvarig)**

* ägare/deltagare i resursgrupp

**Daniel (IT-säkerhetsansvarig)**

* Agare/deltagare i prenumeration eller Security Admin

**Selma (säkerhetsmedarbetare)**

* Läsare i prenumeration eller säkerhet Reader tooview aviseringar
* Ägare/deltagare i prenumeration eller säkerhet administratör krävs toodismiss aviseringar

**Sami (säkerhetsanalytiker)**

* Prenumerationsaviseringarna Reader tooview
* Ägare/deltagare i prenumeration krävs toodismiss aviseringar
* Åtkomst toohello arbetsytan kan krävas

Vissa andra tooconsider för viktig information:

* Endast ägare och deltagare i prenumerationer samt Security-admins kan ändra säkerhetsprinciper
* Endast ägare och deltagare i prenumerationer och resursgrupper kan tillämpa säkerhetsrekommendationer på en resurs.

När du planerar åtkomstkontrollen med rollbaserad Åtkomstkontroll för Security Center vara säker på att toounderstand som i din organisation kommer att använda Security Center. samt vilka typer av uppgifter som de ska utföra, innan du konfigurerar den rollbaserade åtkomsten.

> [!NOTE]
> Vi rekommenderar att du ger hello minst Tillåtande rollen som behövs för användare toocomplete sina uppgifter. Till exempel användare som bara behöver tooview information om hello säkerhetsstatusen på resurser men inte vidta några åtgärder, till exempel tillämpa rekommendationer eller ändra principer, som ska tilldelas rollen för hello läsare.
> 
> 

## <a name="security-policies-and-recommendations"></a>Säkerhetsprinciper och säkerhetsrekommendationer
En säkerhetsprincip definierar hello kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen. I Security Center kan du definiera principer enligt tooyour företagets säkerhetskrav och hello typ av program eller känslighet hello data.

Principer som är aktiverade i hello prenumerationsnivån automatiskt sprida tooall resursgrupper i hello prenumeration som visas i följande diagram hello:

![Säkerhetsprinciper](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Om du behöver tooreview vilka principer som har ändrats, kan du använda [Azure-granskningsloggarna](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Alla principändringar loggas alltid i granskningsloggarna i Azure.
> 
> 

### <a name="security-recommendations"></a>Säkerhetsrekommendationer
Innan du konfigurerar säkerhetsprinciper, granskar du varje hello [säkerhetsrekommendationer](security-center-recommendations.md), och bestämma om dessa principer som är lämpliga för olika prenumerationer och resursgrupper. Det är också viktigt toounderstand vilken åtgärd som ska vidtas tooaddress [säkerhetsrekommendationer](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) och vem i din organisation är ansvarig för att övervaka för nya rekommendationer och ta hello behövs steg.

Security Center rekommenderar att du uppger kontaktuppgifter för säkerhetsrelaterade frågor relaterade till din Azure-prenumeration. Den här informationen kommer att användas av Microsoft toocontact om hello Microsoft Security Response Center (MSRC) upptäcker att din kundinformation används redan av en olaglig eller obehörig part. Läs [ange säkerhet kontaktinformation i Azure Security Center](security-center-provide-security-contact-details.md) för mer information om hur tooenable den här rekommendationen.

## <a name="data-collection-and-storage"></a>Datainsamling och datalagring
Azure Security Center använder hello Microsoft Monitoring Agent – detta är hello samma agent används av hello Operations Management Suite och Log Analytics-tjänsten – toocollect säkerhetsdata från virtuella datorer. Data som samlas in från den här agenten kommer att lagras i logganalysarbetsytor.

### <a name="agent"></a>Agent

När datainsamling har aktiverats i säkerhetsprincipen hello hello Microsoft Monitoring Agent (för [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) eller [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) har installerats på alla stöds virtuella Azure-datorer och nya filer som skapas.  Om den virtuella datorn har redan hello Microsoft Monitoring Agent installerad, hello Azure Security Center använder hello aktuella installerad agent. hello agent processen är utformad toobe icke-inkräktande och ha mycket minimal påverkan på VM-prestanda.

hello Microsoft Monitoring Agent för Windows kräver används TCP-port 443. Se hello [felsökning artikel](security-center-troubleshooting-guide.md) för ytterligare information.

Om du vill toodisable datainsamling vid något tillfälle kan inaktivera du det i hello säkerhetsprincipen. Men eftersom hello Microsoft Monitoring Agent kan användas av andra Azure-hantering och övervakning services hello-agenten avinstalleras inte automatiskt när du stänga av insamling av data i Security Center. Du kan avinstallera hello-agenten manuellt om det behövs.

> [!NOTE]
> toofind en lista över virtuella datorer som stöds, läsa hello [Azure Security Center vanliga frågor (FAQ)](security-center-faq.md).
> 

### <a name="workspace"></a>Arbetsyta

Data som samlas in från hello Microsoft Monitoring Agent (för Azure Security Center) kommer att lagras i en befintlig arbetsytor logganalys som är associerade med din Azure-prenumeration eller en ny arbetsytor med hänsyn till kontot hello Geo av hello VM. 

Du kan bläddra toosee en lista över dina logganalys arbetsytor, inklusive alla som skapats av Azure Security Center i hello Azure-portalen. En relaterad resursgrupp skapas för nya arbetsytor. Både följer namnkonventionen: 

* Arbetsyta: *DefaultWorkspace-[prenumerations-ID]-[geo]*
* Resursgrupp: *DefaultResourceGroup-[geo]*

För arbetsytor som skapats av Azure Security Center sparas data i 30 dagar. Avslutar arbetsytor baseras kvarhållning på hello arbetsytan prisnivån.

> [!NOTE]
> Microsoft att starka åtaganden tooprotect hello sekretess och säkerhet för dessa data. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst. Mer information om datahantering och sekretess finns i [Datasäkerhet i Azure Security Center](security-center-data-security.md).
> 

## <a name="ongoing-security-monitoring"></a>Fortlöpande säkerhetsövervakning
Efter första konfigurationen och tillämpningen av Security Center-rekommendationerna överväger hello nästa steg Security Center operativa processer.

tooaccess Security Center från hello Azure-portalen kan du klicka på **Bläddra** och skriv **Security Center** i hello **Filter** fältet. hello vyer att hello användare hämtar sker enligt toothese tillämpas filter, hello exemplet nedan visar en miljö med många problem toobe beskrivs:

![instrumentpanel](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Security Center påverkar inte era vanliga driftrutiner, den passivt övervaka dina distributioner och ger rekommendationer baserat på hello säkerhetsprinciper som du har aktiverat.

När du först välja toouse Security Center för aktuella Azure-miljön, se till att du läser igenom alla rekommendationer som kan göras i hello **rekommendationer** panelen eller per resurs (**Compute** **Nätverk**, **lagring & data**, **programmet**).

När du har genomfört alla rekommendationer hello **förebyggande** vara grön för alla resurser som löstes. Kontinuerlig övervakning blir då enklare eftersom du hädanefter bara behöver vidta åtgärder baserat på ändringar i hello resource security health och i rekommendationsrutorna.

Hej **identifiering** är mer reaktiv, här visas aviseringar om problem som antingen ägde rum nu, eller inträffade i hello senaste och upptäcktes av Security Center kontroller och 3 part. hello rutan med säkerhetsaviseringar visas diagram som representerar hello antalet hotidentifieringsaviseringar som uppkommit per dag och fördelning hello olika allvarlighetsgrader (låg, medelhög och hög). Mer information om säkerhetsaviseringar [hantering och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Du kan även utnyttja Microsoft Power BI toovisualize Security Center-data. Läs [Kunskap genom statistik från Azure Security Center med Power BI](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Övervakning av nya och ändrade resurser
De flesta miljöer i Azure är dynamiska, med nya resurser som skapas och försvinner, nya konfigurationer och ändringar och så vidare. Security Center säkerställer du att insyn i hello säkerhetsläget för de här nya resurserna.

När du lägger till nya resurser (virtuella datorer, SQL-databaser) tooyour Azure-miljön för att identifiera dessa resurser Säkerhetscenter automatiskt och börjar toomonitor säkerhet. Detta omfattar även arbetarroller och webbroller i PaaS. Om datainsamling har aktiverats i hello [säkerhetsprincip](security-center-policies.md)ytterligare övervakningsfunktionerna aktiveras automatiskt för dina virtuella datorer.

![Huvuddelar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. För virtuella datorer, klickar du på **Compute** under området **Förebyggande**. Eventuella problem med att aktivera data eller tillhörande rekommendationer baslinjekonfigurationer i hello **översikt** fliken och **Monitoring Recommendations** avsnitt.
2. Visa hello **rekommendationer** toosee vad, eventuella säkerhetsrisker identifierades för hello ny resurs.
3. Det är mycket vanligt att när nya virtuella datorer läggs tooyour miljö, endast hello operativsystemet först installeras. Hej resursägaren kan behöva vissa tid toodeploy andra appar som kommer att användas av dessa virtuella datorer.  Vi rekommenderar att du bör känna hello planerna för arbetsbelastningen. Kommer den toobe en programserver? Baserat på vilka den här nya arbetsbelastningen är pågående toobe, du kan aktivera lämpliga hello **säkerhetsprincip**, vilket är hello tredje steget i arbetsflödet.
4. När nya resurser läggs tooyour Azure-miljön, är det möjligt att nya aviseringar visas i hello **säkerhetsaviseringar** panelen. Alltid kontrollera om det finns nya aviseringar i den här rutan och vidta åtgärder enligt tooSecurity Center rekommendationer.

Du ska också tooregularly hello tillstånd för Övervakare av befintliga resurser tooidentify konfigurationsändringar som har skapat säkerhetsrisker, inte rekommenderade baslinjer och säkerhetsaviseringar. Starta i instrumentpanelen för hello Security Center. Du har tre huvudområden tooreview konsekvent därifrån.

![Åtgärder](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Hej **förebyggande** avsnittet panelen ger snabb åtkomst tooyour viktiga resurser. Använd det här alternativet toomonitor beräkning, nätverk, lagring och data och program.
2. Hej **rekommendationer** Kontrollpanelen möjliggör tooreview Security Center rekommendationer. Under den fortlöpande övervakningen hända att du inte har rekommendationer dagligen, vilket är normalt eftersom du utförde alla rekommendationer på hello Security Center-installationen. Därför kan du kanske inte har nya uppgifter i det här avsnittet varje dag och behöver bara tooaccess den efter behov.
3. Hej **identifiering** avsnitt kan ändra på en mycket ofta eller mycket ovanligt basis. Du bör alltid kontrollera säkerhetsaviseringarna och vidta åtgärder enligt rekommendationerna i Security Center.

## <a name="incident-response"></a>Incidenthantering
Security Center identifierar och varnar dig toothreats när de inträffar. Organisationer bör övervaka för nya säkerhetsaviseringar och vidta åtgärder som behövs tooinvestigate ytterligare eller reparera hello-attack. Mer information om hur hotidentifieringen i Security Center fungerar finns i [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md).

När den här artikeln saknar hello avsiktshantering tooassist dig att skapa en egen incidenthanteringsplan kan vi toouse Microsoft Azure Security Response i hello molnet livscykel hello grund för incidenter etapper. hello steg visas i följande diagram hello:

![Misstänkt aktivitet](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Du kan använda hello National Institute of Standards and Technology (NIST) [datorn Incident hantering säkerhetsguiden](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) som en referens tooassist du vill skapa egna.
> 

Du kan använda Security Center-aviseringar under hello följande steg:

* **Identifiera**: Identifiera misstänkt aktivitet i en eller flera resurser. 
* **Utvärdera**: utföra hello inledande assessment tooobtain mer information om hello misstänkt aktivitet.
* **Diagnostisera**: Använd hello reparation steg tooconduct hello tekniska proceduren tooaddress hello problemet.

Varje Säkerhetsvarning innehåller information som kan användas för toobetter förstå hello uppbyggnad hello angrepp och föreslår eventuella ändringar. Vissa aviseringar finns länkar tooeither mer information eller tooother informationskällor inom Azure. Du kan använda informationen för ytterligare forskning och toobegin minskning hello och du kan också söka säkerhetsrelaterade data som lagras på din arbetsyta.

hello visas följande exempel en misstänkt RDP-aktivitet som äger rum:

![Misstänkt aktivitet](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Som du ser det här bladet visar information om hello tid att hello attack ägde rum, hello källa värdnamn, hello virtuell dator och ger även rekommendation steg. I vissa fall hello vara källinformation hello attack tom. [Här](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) finns mer information om de fall då uppgift om källa saknas i aviseringar i Azure Security Center.

I hello [hur tooLeverage hello Azure Security Center & Microsoft Operations Management Suite för en Incident svar](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) video kan du se vissa demonstrationer som kan hjälpa dig toounderstand hur Security Center kan användas i varje en av dessa steg.

> [!NOTE]
> Läs [Leveraging Azure Security Center för Incident svar](security-center-incident-response.md) mer information om hur toouse Security Center funktioner tooassist du under svaret Incident bearbeta. 
> 
> 

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur tooplan för införandet av Security Center. toolearn mer om Security Center finns hello följande:

* [Hantera och åtgärda toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.

