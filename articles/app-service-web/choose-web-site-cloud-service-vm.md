---
title: "aaaAzure Apptjänst, virtuella datorer, Service Fabric och Cloud Services jämförelse | Microsoft Docs"
description: "Lär dig hur toochoose mellan Azure App Service, virtuella datorer, Service Fabric och Cloud Services som värd för webbprogram."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7577332ed049df66178c7b2cd5c440a7f93a7865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Jämförelse mellan Azure App Service, virtuella datorer, Service Fabric och Cloud Services
## <a name="overview"></a>Översikt
Azure erbjuder flera olika sätt toohost webbplatser: [Azure App Service][Azure App Service], [virtuella datorer][Virtual Machines], [Service Fabric ] [ Service Fabric], och [molntjänster][Cloud Services]. Den här artikeln hjälper dig att förstå hello alternativ och göra hello rätta valet för ditt webbprogram.

Azure Apptjänst är hello bästa valet för de flesta web apps. Distribution och hantering är integrerade i hello-plattformen, platser kan snabbt skala toohandle hög belastningar och hello inbyggda load balancing och trafik manager ger hög tillgänglighet. Du kan flytta befintliga platser tooAzure Apptjänst med en [online Migreringsverktyg](https://www.migratetoazure.net/), använda en app för öppen källkod från hello Web Application Gallery eller skapa en ny plats med hello framework och -verktyg. Hej [WebJobs] [ WebJobs] funktionen gör det enkelt tooadd bakgrund jobbet bearbetning tooyour App Service webbapp.

Service Fabric är bra om du skapar en ny app eller skriva om en befintlig app toouse arkitektur för mikrotjänster. Appar som körs på en delad pool av datorer, kan du börja litet och växa toomassive skala och med hundratals eller tusentals datorer efter behov. Tillståndskänsliga tjänster gör det enkelt tooconsistently och på ett tillförlitligt sätt lagra Programtillstånd och Service Fabric hanterar automatiskt tjänsten partitionering, skalning och tillgänglighet för dig.  Service Fabric stöder också WebAPI med Open Web Interface för .NET (OWIN) och ASP.NET Core.  Jämfört med tooApp tjänsten, Service Fabric innehåller också mer kontroll över eller direkt åtkomst till hello underliggande infrastruktur. Du kan fjärråtkomst till dina servrar eller konfigurera servern startade uppgifter. Molntjänster är liknande tooService Fabric i kontroll jämfört med användbarhet, men det är nu en äldre service och Service Fabric rekommenderas för nyutveckling.

Om du har ett befintligt program som kräver betydande ändringar av toorun i Apptjänst eller Service Fabric kan du välja virtuella datorer i ordning toosimplify migrera toohello moln. Dock korrekt konfigurera kräver mycket mer tid för att skydda och hantera virtuella datorer och IT-området jämfört med tooAzure Apptjänst och Service Fabric. Om du funderar på Azure Virtual Machines, kontrollerar du att du beakta konto hello löpande underhållet enklare toopatch, uppdatera och hantera Virtuella miljön. Virtuella Azure-datorer är Infrastructure-as-a-Service (IaaS), medan App Service och Service Fabric är plattform som en-tjänst (Paas). 

## <a name="features"></a>Jämförelse av funktioner
hello följande tabell jämförs hello funktioner i Apptjänst, Cloud Services, virtuella datorer och Service Fabric toohelp du se hello bästa valet. Aktuell information om hello SLA för varje alternativ Se [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

| Funktion | Apptjänst (webbprogram) | Molntjänster (webbroller) | Virtuella datorer | Service Fabric | Anteckningar |
| --- | --- | --- | --- | --- | --- |
| Nästan omedelbar distribution |X | | |X |Distribuera ett program eller ett program uppdatering tooa molntjänst eller skapa en virtuell dator tar flera minuter minst; distribuera en webbapp för programmet tooa tar några sekunder. |
| Skala upp toolarger datorer utan Omdistributionen |X | | |X | |
| Web server-instanser Dela innehåll och konfiguration, vilket innebär att du inte har tooredeploy eller konfigurera om när du skalar. |X | | |X | |
| Distributionsmiljöer med flera (produktion och mellanlagring) |X |X | |X |Service Fabric kan du toohave flera miljöer för dina appar eller toodeploy olika versioner av din app sida-vid-sida. |
| Automatisk hantering av OS |X |X | | |Automatiska uppdateringar av OS planeras för en framtida Service Fabric-versionen. |
| Växla sömlös plattform (enkelt flytta mellan 32-bitars och 64-bitars) |X |X | | | |
| Distribuera kod med GIT, FTP |X | |X | | |
| Distribuera kod med webbdistribution |X | |X | |Molntjänster stöder hello användning av webbdistribution toodeploy uppdateringar tooindividual rollinstanser. Men du kan inte använda den för distribuering av en roll och om du använder webbdistribution för en uppdatering har toodeploy separat tooeach instans av en roll. Flera instanser som krävs i ordning tooqualify för hello Cloud Service SLA för produktionsmiljöer. |
| WebMatrix stöd |X | |X | | |
| Åtkomst tooservices som Service Bus, lagring, SQL-databas |X |X |X |X | |
| Värden webb- eller web services nivå i en arkitektur med flera nivåer |X |X |X |X | |
| Värden mellannivån en arkitektur med flera nivåer |X |X |X |X |App Service web apps kan enkelt värd för en REST API-mellannivå och hello [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) funktion kan vara värd för bakgrundsjobb för bearbetning. Du kan köra WebJobs i en dedikerad webbplats tooachieve oberoende skalbarhet för hello nivå. hello preview [API apps](../app-service-api/app-service-api-apps-why-best-platform.md) funktion tillhandahåller ytterligare funktioner för värdtjänster REST. |
| Integrerat stöd för MySQL as a service |X |X |X | |Molntjänster kan integrera MySQL as a service via Cleardbs erbjudanden, men inte som en del av arbetsflödet för hello Azure-portalen. |
| Stöd för ASP.NET, klassisk ASP, Node.js, PHP, Python |X |X |X |X |Service Fabric stöder hello skapandet av en web front-end med [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) eller så kan du distribuera program (Node.js, Java och så vidare) som en [gäst körbara](../service-fabric/service-fabric-deploy-existing-app.md). |
| Skala ut toomultiple instanser utan att distribuera om |X |X |X |X |Virtuella datorer kan skala ut toomultiple instanser, men hello-tjänster som körs på dem. måste skrivas toohandle detta skalbar. Du har tooconfigure en belastningsutjämnare tooroute begäranden om belastning över hello datorer och skapa en Tillhörighetsgrupp tooprevent samtidiga omstarter av alla instanser på grund av toomaintenance eller maskinvarufel. |
| Stöd för SSL |X |X |X |X |SSL för anpassade domännamn stöds bara för Basic och Standard läge för App Service web apps. Information om hur du använder SSL med webbappar finns [konfigurera ett SSL-certifikat för en Azure-webbplats](app-service-web-tutorial-custom-ssl.md). |
| Visual Studio-integration |X |X |X |X | |
| Fjärrfelsökning |X |X |X | | |
| Distribuera kod med TFS |X |X |X |X | |
| Nätverksisolering med [Azure Virtual Network](/azure/virtual-network/) |X |X |X |X |Se även [Azure Websites virtuell nätverksintegration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Stöd för [Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Integrerad Slutpunktsövervakning |X |X |X | | |
| Fjärråtkomst till skrivbordet tooservers | |X |X |X | |
| Installera eventuella anpassade MSI | |X |X |X |Service Fabric kan du toohost alla körbara filen som en [gäst körbara](../service-fabric/service-fabric-deploy-existing-app.md) eller du kan installera en app på hello virtuella datorer. |
| Möjlighet toodefine/kör uppstart uppgifter | |X |X |X | |
| Kan lyssna tooETW händelser | |X |X |X | |

## <a name="scenarios"></a>Scenarier och rekommendationer
Här följer några vanliga Programscenarier med rekommendationer som toowhich Azure web värd kan vara mest lämpliga för varje.

* [Jag behöver en frontwebb med behandling i bakgrunden och databasen backend toorun affärsprogram integrerad med lokala tillgångar.](#onprem)
* [Jag behöver ett tillförlitligt sätt toohost min företagets webbplats som kan skalas korrekt och erbjudanden globala nå.](#corp)
* [Jag har en IIS6-program som körs på Windows Server 2003.](#iis6)
* [Jag är ett litet företagsägare och jag behöver ett billigt sätt toohost min plats men med framtida tillväxt i åtanke.](#smallbusiness)
* [Jag är en webbplats eller en grafisk designer, och jag vill toodesign och skapa webbplatser för Mina kunders.](#designer)
* [Jag migrera Mina flernivåapp med en web front-end toohello moln.](#multitier)
* [Min programmet är beroende av anpassade Windows eller Linux-miljöer och jag vill toomove den toohello moln.](#custom)
* [Min plats använder programvara med öppen källkod och jag vill toohost den i Azure.](#oss)
* [Jag har ett line-of-business-program som behöver tooconnect toohello företagets nätverk.](#lob)
* [Jag vill toohost en REST API eller web-tjänst för mobila klienter.](#mobile)

### <a id="onprem"></a>Jag behöver en frontwebb med behandling i bakgrunden och databasen backend toorun affärsprogram integrerad med lokala tillgångar.
Azure Apptjänst är en bra lösning för komplexa program. Gör det möjligt att utveckla appar som skala automatiskt på en belastningen belastningsutjämnade plattform, skyddas med Active Directory och ansluta tooyour lokala resurser. Det gör Hantera apparna enkelt via en världsklass portal och API: er och ger dig toogain kunskaper om hur kunder använder dem med app insight verktyg. Hej [Webjobs] [ Webjobs] funktionen kan du köra bakgrundsprocesser och aktiviteter som en del av din webbnivå när hybridanslutning och funktioner för virtuella nätverk gör det enkelt tooconnect tillbaka tooon lokala resurser. Azure App Service tillhandahåller tre 9 SLA för webbprogram och kan du:

* Kör dina program på ett tillförlitligt sätt på en molnplattform självläkande, automatisk uppdatering.
* Skala automatiskt över ett globalt nätverk av datacenter.
* Säkerhetskopiera och återställa för katastrofåterställning.
* Vara ISO, SOC2 och PCI.
* Integrering med Active Directory

### <a id="corp"></a>Jag behöver ett tillförlitligt sätt toohost min företagets webbplats som kan skalas korrekt och erbjudanden globala nå.
Azure Apptjänst är en bra lösning för att hantera företagets webbplatser. Det ger web apps tooscale snabbt och enkelt toomeet kräver över ett globalt nätverk av datacenter. Den erbjuder lokala reach feltolerans och intelligent trafikhantering. Alla på en plattform som tillhandahåller världsklass hanteringsverktyg, så att du toogain insyn i hälsa för platsen och trafiken snabbt och enkelt. Azure App Service tillhandahåller tre 9 SLA för webbprogram och kan du:

* Vill du köra dina webbplatser på ett tillförlitligt sätt på en molnplattform självläkande, automatisk uppdatering.
* Skala automatiskt över ett globalt nätverk av datacenter.
* Säkerhetskopiera och återställa för katastrofåterställning.
* Hantera loggar och trafik med integrerade verktyg.
* Vara ISO, SOC2 och PCI.
* Integrering med Active Directory

### <a id="iis6"></a>Jag har en IIS6-program som körs på Windows Server 2003.
Azure App Service gör det enkelt tooavoid hello kostnader med infrastruktur som är associerade med migrera äldre IIS6 program. Microsoft har utvecklat [enkelt toouse Migreringsverktyg och detaljerade riktlinjer](https://www.movemetowebsites.net/) att aktivera toocheck kompatibilitet och identifiera eventuella ändringar som behöver toobe som gjorts. Integrering med Visual Studio, TFS och gemensamma CMS-verktyg gör det enkelt toodeploy IIS6 program direkt toohello molnet. När distribuerats ger hello Azure-portalen robust hanteringsverktyg som gör att du tooscale ned toomanage kostnader och toomeet ska beaktas vid behov. Med hello Migreringsverktyget kan du:

* Migrera äldre Windows Server 2003 web application toohello molnet snabbt och enkelt.
* Välja tooleave dina anslutna SQL-databas lokalt toocreate ett hybridprogram.
* Flytta SQL-databasen tillsammans med äldre programmet automatiskt.

### <a id="smallbusiness"></a>Jag är ett litet företagsägare och jag behöver ett billigt sätt toohost min plats men med framtida tillväxt i åtanke.
Azure Apptjänst är en bra lösning för det här scenariot eftersom du kan börja använda det kostnadsfritt och sedan lägga till flera funktioner när du behöver dem.. Varje ledigt webbprogram som levereras med en domän som tillhandahålls av Azure (*your_company*. azurewebsites.net), och hello-plattformen innehåller integrerade distribution och hantering av verktyg, samt en programgalleriet som gör det enkelt tooget igång. Det finns många andra tjänster och skalningsalternativ som tillåter hello plats tooevolve med ökad användarens behov. Med Azure App Service kan du:

* Börja med hello kostnadsfria nivån och skala upp efter behov.
* Använd hello Programgalleriet tooquickly konfigurera populära webbprogram, till exempel WordPress.
* Lägga till ytterligare Azure-tjänster och funktioner tooyour program efter behov.
* Skydda ditt webbprogram med HTTPS.

### <a id="designer"></a>Jag är en webbplats eller en grafisk designer, och jag vill toodesign och skapa webbplatser för Mina kunders
För webbutvecklare och designers Azure App Service kan enkelt integreras med en mängd olika verktyg och ramverk, inkluderar stöd för distribution för Git och FTP och integreras med verktyg och tjänster som Visual Studio och SQL-databas. Med App Service kan du:

* Använd kommandoradsverktygen för [automatiserade aktiviteter][scripting].
* Arbeta med populära språk som [.Net][dotnet], [PHP][PHP], [Node.js] [ nodejs], och [Python][Python].
* Välj tre skalning nivåer för att skala upp toovery hög kapacitet.
* Integrera med andra Azure-tjänster som [SQL-databas][sqldatabase], [Service Bus] [ servicebus] och [lagring] [ Storage], eller partnernätverk erbjudanden från hello [Azure Store][azurestore], till exempel MySQL och MongoDB.
* Integrera med verktyg som Visual Studio, Git, WebMatrix, WebDeploy, TFS och FTP.

### <a id="multitier"></a>Jag migrera Mina flernivåapp med en web front-end toohello moln
Om du kör en flernivåapp, till exempel en webbserver som ansluter tooa databas är ett bra alternativ som integreras med Azure SQL Database i Azure App Service. Och du kan använda hello WebJobs funktionen för backend-processer som körs.

Välj Service Fabric för en eller flera av dina nivåerna om du behöver mer kontroll över hello servermiljö, till exempel hello möjlighet tooremote till servern eller konfigurera servern startade uppgifter.

Välj virtuella datorer för en eller flera av dina nivåerna om du vill toouse datoravbildningen eller köra server-programvara eller tjänster som du inte kan konfigurera på Service Fabric.

### <a id="custom"></a>Min programmet är beroende av anpassade Windows eller Linux-miljöer och jag vill toomove den toohello moln.
Om ditt program kräver komplexa installation eller konfiguration av programvara och hello operativsystem, virtuella datorer troligen hello bästa lösningen. Med virtuella datorer kan du:

* Använd hello virtuella galleriet toostart med ett operativsystem, till exempel Windows eller Linux och anpassa den efter dina krav för programmet.
* Skapa och ladda upp en anpassad avbildning av en befintlig lokal server toorun på en virtuell dator i Azure.

### <a id="oss"></a>Min plats använder programvara med öppen källkod och jag vill toohost den i Azure
Om ditt öppen källkod framework stöds i App Service hello språk och ramverk som behövs i programmet konfigureras automatiskt för dig. Apptjänst kan du:

* Använda många populära öppen källa språk, till exempel [.NET][dotnet], [PHP][PHP], [Node.js] [ nodejs], och [Python][Python].
* Konfigurera WordPress, Drupal, Umbraco, DNN och många andra tredjeparts-webbprogram.
* Migrera ett befintligt program eller skapa en ny från hello Application Gallery.

Om ditt öppen källkod framework inte stöds på Apptjänst, kan du köra det på en av hello andra Azure webbvärd alternativ. Med virtuella datorer du installerar och konfigurerar hello programvara på hello avbildningen av datorn, vilket kan vara Windows eller Linux-baserade.

### <a id="lob"></a>Jag har ett line-of-business-program som behöver tooconnect toohello företagsnätverket
Om du vill toocreate ett line-of-business-program kan behöva din webbplats direktåtkomst tooservices eller data i hello företagsnätverk. Detta är möjligt på App Service, Service Fabric och virtuella datorer med hello [Azure Virtual Network service](/azure/virtual-network/). På Apptjänst kan du använda hello [VNET integrationsfunktionen](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), vilket gör att din Azure-program toorun som om de befann sig i företagsnätverket.

### <a id="mobile"></a>Jag vill toohost en REST API eller web-tjänst för mobila klienter
HTTP-baserat webbtjänsterna aktivera toosupport ett stort antal klienter, inklusive mobila klienter. Ramverk som ASP.NET Web API integrera med Visual Studio toomake den enklare toocreate och använda REST-tjänster.  Tjänsterna är tillgängliga från en webbserver-slutpunkt, så det är möjligt toouse någon web värd tekniken Azure toosupport det här scenariot. Men är Apptjänst ett bra alternativ för värd för REST API: er. Med App Service kan du:

* Snabbt skapa en [mobilappen](../app-service-mobile/app-service-mobile-value-prop.md) eller [API-app](../app-service-api/app-service-api-apps-why-best-platform.md) toohost hello HTTP-webbtjänsten i en global Azure-datacenter.
* Migrera befintliga tjänster eller skapa nya.
* Uppnå SLA för tillgänglighet med en enda instans, eller skala ut toomultiple särskilda datorer.
* Använd hello publicerade plats tooprovide REST API: er tooany HTTP-klienter, inklusive mobila klienter.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett konto, gå för<a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, där kan du direkt skapa en tillfällig startapp i Azure App Service kostnadsfritt. Inget kreditkort behövs, inga åtaganden.
> 
> 

## <a id="nextsteps"></a>Nästa steg
Mer information om hello tre alternativ för webbvärd finns [introduktion till Azure](../fundamentals-introduction-to-azure.md).

tooget igång med hello valt alternativ för ditt program finns hello följande resurser:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Virtuella Azure-datorer](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
