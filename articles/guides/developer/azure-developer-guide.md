---
title: "aaaGet igång-guide för utvecklare i Azure | Microsoft Docs"
description: "Det här avsnittet innehåller viktig information för utvecklare som söker tooget igång med hello Microsoft Azure-plattformen för deras utvecklingsbehov."
services: 
cloud: 
documentationcenter: 
author: ggailey777
manager: erikre
ms.assetid: 
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: glenga
ms.openlocfilehash: 72dc2678db7738923d4bc7783e297fea6fcded83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-guide-for-azure-developers"></a>Introduktionsguide för Azure-utvecklare

## <a name="what-is-azure"></a>Vad är Azure?

Azure är en komplett molnplattform som kan vara värd för dina befintliga program, förenkla hello utvecklingen av nya program och även förbättra lokala program. Azure integrerar hello molntjänster som du behöver toodevelop, testa, distribuera och hantera dina program – samtidigt dra nytta av hello effektivitet för cloud computing.

Som värd för dina program i Azure, kan du börja litet och enkelt skala ditt program när kundernas behov växer. Azure erbjuder också hello tillförlitlighet som behövs för program med hög tillgänglighet, även inklusive växling mellan olika regioner. Hej [Azure-portalen](https://portal.azure.com) kan du enkelt hantera alla dina Azure-tjänster. Du kan också hantera dina tjänster via programmering med hjälp av service-API: er och mallar.

**Vem ska läsa det här**: den här handboken är en introduktion toohello Azure-plattformen för programutvecklare. Det ger vägledning och att du måste skapa nya program i Azure eller migrera den befintliga program tooAzure toostart riktning.

## <a name="where-do-i-start"></a>Vad ska jag börja med?

Det kan vara en avskräckande uppgiften toofigure information om vilka tjänster du behöver toosupport din lösningsarkitektur med alla hello-tjänster som Azure erbjuder. Det här avsnittet visar hello Azure services att utvecklare ofta använder. En lista över alla Azure-tjänster finns hello [dokumentation för Azure](../../index.md).

Först måste du bestämma hur toohost ditt program i Azure. Du behöver toomanage hela din infrastruktur som en virtuell dator (VM). Kan du använda hanteringsfunktioner för hello plattform som Azure tillhandahåller? Du kanske behöver en serverlösa framework toohost kodkörning endast?

Programmet måste molnlagring som Azure erbjuder flera alternativ för. Du kan dra nytta av Azures enterprise-autentisering. Det finns också verktyg för molnbaserade utveckling och övervakning och de flesta värdtjänster erbjuder DevOps-integration.

Nu ska vi titta på några hello specifika tjänster som vi rekommenderar att undersöka för dina program.

### <a name="application-hosting"></a>Programvärd

Azure tillhandahåller flera molnbaserade beräkning erbjudanden toorun ditt program så att du inte har tooworry om hello infrastruktur information. Du kan enkelt skala upp eller ut dina resurser som din programanvändning växer.

Azure erbjuder tjänster som stöder utveckling av program och vara värd för behov. Azure tillhandahåller infrastruktur-som-en-tjänst (IaaS) toogive du fullständig kontroll över din värd för programmet. Azures plattform som en tjänst (PaaS)-erbjudanden ange hello fullständigt hanterade tjänster som behövs för toopower dina appar. Det är även true serverlösa värd i Azure där allt du behöver toodo Skriv koden.

![Azure-program webbhotell](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Om du vill hello snabbaste sökvägen toopublish webbaserade projekt kan du överväga att Azure App Service. Apptjänst gör det enkelt tooextend din web apps toosupport dina mobila klienter och publicera enkelt förbrukade REST API: er. Den här plattformen ger autentisering via sociala providers, baserat på trafik autoskalning testa i produktion och kontinuerlig och behållaren-baserade distributioner.

När du skapar en app i App Service kan välja du något av hello följande typer:

- [Webbappar](../../app-service-web/app-service-web-overview.md): kan du vara värd för webbplatser och webbprogram program som har skrivits i .NET, Java, PHP, Node.js och Python.

- [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md): utökar Web Apps toosupport åtkomst från mobila enheter. Den aktiverar autentisering med sociala providers och Azure Active Directory (Azure AD), innehåller en backend-lagring och integreras med [Azure Notification Hubs](../../notification-hubs/notification-hubs-push-notification-overview.md) för push-meddelanden.

- [API Apps](../../app-service-api/app-service-api-apps-why-best-platform.md): låter dig på ett säkert sätt exponera dina API: er i hello moln med Swagger-metadata så att klienter kan enkelt använda dem.

Eftersom alla tre apptyperna delar Hej Apptjänst runtime, värd för en webbplats, stöd för mobila klienter och använda dina API: er i Azure, allt från hello samma projekt eller. toolearn mer information om App Service finns [så App Service fungerar](../../app-service/app-service-how-works-readme.md).

Apptjänst har utformats med DevOps i åtanke. Det stöder olika verktyg för publicering och kontinuerlig integration distributioner, däribland GitHub webhooks, Jenkins, Visual Studio Team Services, TeamCity och andra.

Du kan migrera dina befintliga program tooApp tjänsten med hjälp av hello [online Migreringsverktyg](https://www.migratetoazure.net/).

>**När toouse**: Använd Apptjänst när du migrerar befintliga web applications tooAzure och när du behöver en helt hanterad plattform som värd för dina webbprogram. Du kan också använda Apptjänst när du behöver toosupport mobila klienter eller använda REST API: er med din app.

>**Kom igång**: App Service som gör det enkelt toocreate och distribuera din första [webbapp](../../app-service-web/web-sites-dotnet-get-started.md), [mobilappen](../../app-service-mobile/app-service-mobile-ios-get-started.md), eller [API-app](../../app-service-api/app-service-api-dotnet-get-started.md).

>**Prova nu**: App Service kan du etablera en tillfällig app tootry hello plattform utan toosign för ett Azure-konto. Försök hello plattform och [skapa en app i Azure App Service](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Som en infrastruktur-som-en-tjänst (IaaS)-provider Azure kan du distribuera tooor migrera dina program tooeither Windows eller Linux-datorer. Tillsammans med Azure Virtual Network stöder Azure Virtual Machines hello distribution av Windows eller Linux-datorer tooAzure. Med virtuella datorer har fullständig kontroll över hello konfigurationen av hello-dator. När du använder virtuella datorer som är du ansvarig för alla server installation, konfiguration, underhåll och operativsystemet programuppdateringar.

På grund av hello kontrollnivåer som du har med virtuella datorer, kan du köra en mängd olika serverarbetsbelastningar på Azure som inte får plats till en PaaS-modell. De här arbetsbelastningarna inkluderar databasservrar, Windows Server Active Directory och Microsoft SharePoint. Mer information finns i hello virtuella datorer dokumentationen för antingen [Linux](/azure/virtual-machines/linux/) eller [Windows](/azure/virtual-machines/windows/).

>**När toouse**: Använd virtuella datorer om du vill ha fullständig kontroll över dina program infrastruktur eller toomigrate lokala program arbetsbelastningar tooAzure utan toomake ändringar.

>**Kom igång**: skapa en [Linux VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) eller [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) från hello Azure-portalen.

#### <a name="azure-functions-serverless"></a>Azure Functions (serverlösa)

I stället för att bekymra dig om att bygga ut och hantera en hela programmet eller hello infrastruktur toorun din kod. Vad händer om du bara Skriv koden och ha den köra i svaret tooevents eller enligt ett schema?  [Azure Functions](../../azure-functions/functions-overview.md) är en ”serverlösa”-format som erbjuder att hello kan du bara skriva kod som du behöver. Med funktioner utlöses kodkörning av HTTP-begäranden, webhooks, cloud service händelser eller enligt ett schema. Skriva kod i programmeringsspråk du föredrar, till exempel C\#, F\#, Node.js, Python eller PHP. Med förbrukningsbaserad fakturering betalar bara för hello tid som koden körs och Azure skalas efter behov.

>**När toouse**: Använd Azure Functions när du har kod som utlöses av andra Azure-tjänster genom webbaserade händelser, eller enligt ett schema. Du kan också använda funktioner när du inte behöver hello arbetet med ett fullständigt värdbaserade projekt eller om du bara vill toopay för hello som koden körs. Det finns fler toolearn [översikt över Azure Functions](../../azure-functions/functions-overview.md).

>**Kom igång**: Följ hello funktioner Snabbstartsguide för[skapa din första funktion](../../azure-functions/functions-create-first-azure-function.md) från hello-portalen.

>**Prova nu**: Azure Functions kan du köra din kod utan att behöva toosign för ett Azure-konto. Prova nu till och [skapa din första Azure-funktion](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric är en plattform för distribuerade system som gör det enkelt toobuild paketera, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Det ger också funktioner för hantering av omfattande program för etablering, distribution, övervakning, uppgradering/uppdatering och borttagning distribuerade program. Appar som körs på en delad pool av datorer, kan du börja litet och skala toohundreds eller tusentals datorer efter behov.

Service Fabric stöder WebAPI med Open Web Interface för .NET (OWIN) och ASP.NET Core. Det ger SDK: er för att skapa tjänster på Linux i både .NET Core och Java. toolearn mer om Service Fabric finns hello [Service Fabric-Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**När toouse:** Service Fabric är bra när du skapar ett program eller skriva om ett befintligt program toouse arkitektur för mikrotjänster. Använda Service Fabric när du behöver mer kontroll över eller direkt åtkomst till hello underliggande infrastruktur.

>**Komma igång:** [skapa ditt första Azure Service Fabric-program](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Förbättra dina program med Azure-tjänster

Dessutom ger tooapplication värd, Azure Tjänsterbjudanden som kan förbättra hello funktioner, utveckling och underhåll av ditt program, både i hello molnet och lokalt.

#### <a name="hosted-storage-and-data-access"></a>Värdbaserade lagrings- och åtkomst

De flesta program måste lagra data så oavsett för hur du bestämmer dig för toohost ditt program i Azure bör du överväga att en eller flera av hello följande lagrings- och tjänster.

-   **Azure SQL Database**: Azure-baserad version av hello Microsoft SQL Server-databasmotorn för att lagra relationella tabelldata i hello molnet. SQL Database tillhandahåller förutsägbar prestanda, skalbarhet utan avbrott, verksamhetskontinuitet och dataskydd.

    >**När toouse**: när programmet kräver datalagring med referensintegritet, transaktionella stöd och stöd för TSQL-frågor.

    >**Kom igång**: [skapa en SQL-databas i minuter med hjälp av hello Azure-portalen](../../sql-database/sql-database-get-started.md).

-   **Azure Storage**: erbjuder beständig, högtillgänglig lagring för blobbar, köer, filer och andra typer av icke-relationella data. Storage tillhandahåller hello lagringsgrunden för virtuella datorer.

    >**När toouse**: när en app lagrar icke-relationella data, till exempel nyckel-värdepar (tabeller), blobbar, filer resurser eller meddelanden (köer).

    >**Kom igång**: Välj något av dessa typer av lagring: [blobbar](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabeller](../../cosmos-db/table-storage-how-to-use-dotnet.md), [köer](../../storage/queues/storage-dotnet-how-to-use-queues.md), eller [filer](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Azure DocumentDB**: en helt hanterad och skalbar NoSQL-databas, som innehåller SQL-frågor via objektdata. Du kan komma åt DocumentDB med hjälp av befintliga drivrutiner för MongoDB.
    >**När toouse:** när programmet måste toobe kan tooexecute SQL-frågor via JSON-dokument, eller om du använder MongoDB.

    >**Kom igång**: [skapa en DocumentDB C#-konsolprogram](../../documentdb/documentdb-get-started.md). Om du utvecklar en MongoDB [DocumentDB-Protokollstöd för MongoDB](../../documentdb/documentdb-protocol-mongodb.md).

Du kan använda [Azure Data Factory](../../data-factory/data-factory-introduction.md) toomove befintliga lokala data tooAzure. Om du inte är redo toomove data toohello moln [Hybridanslutningar](../../biztalk-services/integration-hybrid-connection-overview.md) i BizTalk-tjänst kan du ansluta din App Service finns app tooon lokala resurser. Du kan också ansluta tooAzure data- och storage-tjänster från din lokala program.

#### <a name="docker-support"></a>Docker-support

Docker-behållare, en typ av OS-virtualisering, kan du distribuera program på ett mer effektivt och förutsägbart sätt. Ett container program fungerar i produktion hello samma sätt som på dina system för utveckling och testning. Du kan hantera behållare med Docker standardverktyg. Du kan använda dina befintliga kunskaper och verktyg för populära öppen källkod toodeploy och hantera behållare-baserade program på Azure.

Azure tillhandahåller flera olika sätt toouse behållare i dina program.

-   **Azure Docker VM-tillägget**: kan du konfigurera den virtuella datorn med Docker verktyg tooact som en Docker-värd.

    >**När toouse**: när du vill toogenerate konsekvent behållardistributionerna för dina program på en virtuell dator eller när du vill toouse [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Kom igång**: [skapar en Docker-miljö i Azure med hjälp av hello Docker VM-tillägget](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Azure Container Service**: kan du skapa, konfigurera och hantera kluster för virtuella datorer som är förkonfigurerad toorun av program. toolearn mer om Container Service finns [Azure Container Service introduktion](../../container-service/container-service-intro.md).

    >**När toouse**: när du behöver toobuild produktionsklara, skalbara miljöer som ger ytterligare schemaläggning och hanteringsverktygen eller när du distribuerar ett Docker Swarm-kluster.

    >**Kom igång**: [distribuera ett Container Service-kluster](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Docker datorn**: kan du installera och hantera en Docker-motorn på virtuella värdar med kommandona för docker-datorn.

    >**När toouse**: när du behöver tooquickly prototyp en app genom att skapa en enda Docker-värd.

-   **Anpassad Docker-avbildning för Apptjänst**: kan du använda Docker-behållare från en behållare registret eller en kund-behållare när du distribuerar ett webbprogram på Linux.

    >**När toouse**: när du distribuerar ett webbprogram på Linux tooa Docker avbildningen.

    >**Kom igång**: [använder en anpassad Docker-avbildning för användning på Linux](../../app-service-web/app-service-linux-using-custom-docker-image.md).

### <a name="authentication"></a>Autentisering

Viktiga toonot endast vet vem som använder ditt program, men även tooprevent obehörig åtkomst tooyour resurser. Azure tillhandahåller flera olika sätt tooauthenticate din app.

-   **Azure Active Directory (AD Azure)**: hello Microsoft flera innehavare, molnbaserade identitets- och management-tjänsten. Du kan lägga till enkel inloggning på (SSO) tooyour program genom att integrera med Azure AD. Du kan komma åt egenskaper för katalog med hjälp av hello Azure AD Graph API direkt eller hello Microsoft Graph API. Du kan integrera med Azure AD-stöd för hello OAuth2.0 auktorisering framework och öppna ID Connect med hjälp av inbyggda HTTP-REST-slutpunkter och hello multiplatform Azure AD-autentiseringsbibliotek.

    >**När toouse**: Om du vill tooprovide enkel inloggning kan arbeta med Graph-baserade data eller autentisera domänbaserade användare.

    >**Kom igång**: toolearn finns fler hello [Utvecklarhandbok för Azure Active Directory](../../active-directory/active-directory-developers-guide.md).

-   **Autentiseringen av tjänsten App**: när du väljer Apptjänst toohost din app kan du också få stöd för inbyggda autentisering för Azure AD, tillsammans med sociala identitetsleverantörer, inklusive Facebook, Google, Microsoft och Twitter.

    >**När toouse**: Om du vill tooenable autentisering i en Apptjänst-app med hjälp av Azure AD, sociala identitetsleverantörer eller båda.

    >**Kom igång**: toolearn mer information om autentisering i App Service finns [autentisering och auktorisering i Azure App Service](../../app-service/app-service-authentication-overview.md).

toolearn mer om metodtips om säkerhet i Azure, se [Azure säkerhetsmetoder och mönster](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>Övervakning

Med programmet igång i Azure, behöver du toobe kan toomonitor prestanda, titta på problem och se hur kunder använder din app. Azure erbjuder flera alternativ för övervakning.

-   **Visual Studio Application Insights**: ett Azure-baserad extensible analytics-tjänsten som kan integreras med Visual Studio toomonitor live webbaserade program. Den ger dig hello data som du behöver toocontinuously förbättra hello prestanda och användbarhet dina appar, om de är finns på Azure eller inte.

    >**Kom igång**: Följ hello [Application Insights kursen](../../application-insights/app-insights-overview.md).

-   **Azure-Monitor**: en tjänst som hjälper dig att toovisualize, fråga, väg, arkivera och agera på hello mått och loggar som genereras av dina Azure-infrastrukturen och resurser. Övervakaren ger hello datavyer som du ser i hello Azure-portalen är en enda källa för övervakning av Azure-resurser.
 
    >**Kom igång**: [Kom igång med Azure-Monitor](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>DevOps-integration

Om det är etablering av virtuella datorer eller publicera dina webbprogram med kontinuerlig integration, integreras Azure med de flesta hello populära DevOps-verktyg. Du kan arbeta med hello verktyg som du redan har och maximera din befintliga upplevelse med stöd för verktyg som Jenkins GitHub, Puppet, Chef, TeamCity, Ansible, VSTS och andra.

>**Prova nu:** [testa flera hello DevOps integreringar](https://azure.microsoft.com/try/devops/).

>**Kom igång**: toosee DevOps-alternativ för en App Service-appen finns [kontinuerlig distribution tooAzure Apptjänst](../../app-service-web/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Azure-regioner

Azure är en global molnplattform som är allmänt tillgänglig i många regioner hello världen. När du etablerar en tjänst, program eller virtuell dator i Azure och tooselect en region som representerar ett visst datacenter där programmet körs eller där dina data lagras. Dessa områden motsvarar toospecific platser, vilket är publicerade på hello [Azure-regioner](https://azure.microsoft.com/regions/) sidan.

### <a name="choose-hello-best-region-for-your-application-and-data"></a>Välj hello bästa region för dina program och data

En av hello fördelarna med att använda Azure är att du kan distribuera ditt program toovarious Datacenter runt hello världen. hello-region som du väljer kan påverka hello prestanda för programmet. Exempelvis är det bättre toochoose en region som är närmare toomost av dina kunder tooreduce fördröjning i nätverksbegäranden. Du kan också tooselect din region toomeet hello juridiska krav för att distribuera din app i vissa länder. Är det alltid en bästa praxis toostore programdata hello samma datacenter eller i ett datacenter så nära som möjligt toohello datacenter som är värd för ditt program.

### <a name="multi-region-apps"></a>Flera regioner appar

Osannolik, är det inte möjligt för en hela datacentret toogo offline på grund av en händelse, till exempel en naturkatastrof eller ett Internet-fel. Det är en bästa praxis toohost viktiga affärsprogram i flera datacenter tooprovide högsta tillgänglighet. Med hjälp av flera regioner kan också minska svarstiden för globala användare och möjligheter ytterligare flexibilitet vid uppdatering av program.

Vissa tjänster, till exempel virtuella datorn och App-tjänster, använda [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) tooenable flera regioner support med växling mellan regioner toosupport hög tillgänglighet företagsprogram. Ett exempel finns [Azure Referensarkitektur: webbprogrammet med hög tillgänglighet](../../guidance/guidance-web-apps-multi-region.md).

>**När toouse**: när du har enterprise och hög tillgänglighet program som har nytta av redundans och replikering.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Hur hanterar jag mina program och projekt?

Azure tillhandahåller en omfattande uppsättning upplevelser för du toocreate och hantera Azure-resurser, program och projekt – såväl programmässigt som hello [Azure-portalen](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Kommandoradsverktyget gränssnitt och PowerShell

Azure tillhandahåller två sätt toomanage dina program och tjänster från hello kommandoraden med hjälp av Bash Terminal, hello kommandotolk eller din kommandoradsverktyget väljer. Vanligtvis kan du utföra samma uppgifter hello från hello kommandoraden som hello Azure-portalen, till exempel skapa och konfigurera virtuella datorer, virtuella nätverk, webbprogram och andra tjänster.

-   [Azure-kommandoradsgränssnittet (CLI)](../../xplat-cli-install.md): låter dig ansluta tooan Azure-prenumeration och program för olika aktiviteter mot Azure-resurser från hello-kommandoraden.

-   [Azure PowerShell](../../powershell-install-configure.md): innehåller en uppsättning av moduler med cmdletar som gör att du toomanage Azure resurser med hjälp av Windows PowerShell.

### <a name="azure-portal"></a>Azure Portal

hello Azure-portalen är ett webbaserat program som du kan använda toocreate, hantera och ta bort Azure-resurser och tjänster. hello Azure-portalen finns i <https://portal.azure.com>. Den innehåller en anpassningsbar instrumentpanel, verktyg för att hantera Azure-resurser och inställningar för toosubscription och faktureringsinformation. Mer information finns i hello [översikt över Azure portal](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST API:er

Azure bygger på en uppsättning REST API: er som stöder hello Azure-portalen Användargränssnittet. De flesta av dessa REST-API: er är också stöds toolet programmässigt etablera och hantera dina Azure-resurser och program från valfri Internet-aktiverad enhet. Hello fullständig uppsättning REST API-dokumentation, finns hello [Azure SDK för REST-referens](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API:er

Dessutom tooREST API: er många Azure-tjänster kan du hantera programmässigt resurser från ditt program med hjälp av plattformsspecifika Azure SDK, inklusive SDK: er för hello följande utvecklingsplattformar:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](http://azure.github.io/azure-sdk-for-node/)
-   [Java](https://docs.microsoft.com/java/api/)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)

Tjänster som [Mobilappar](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) och [Azure Media Services](../../media-services/media-services-dotnet-how-to-use.md) Ange klientens SDK toolet åtkomst till tjänster från webb- och mobile-klientprogram.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Kör appen på Azure sannolikt innebär att arbeta med flera Azure-tjänster, alla av vilka Följ hello samma livslängd och kan betraktas som en logisk enhet. Ett webbprogram kan till exempel använda Web Apps, SQL-databas, lagring, Azure Redis-Cache och Azure Content Delivery Network services. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) kan du arbeta med hello resurser i ditt program som en grupp. Du kan distribuera, uppdatera eller ta bort alla hello resurser i en enda, samordnad åtgärd.

Dessutom toologically gruppera och hantera relaterade resurser, Azure Resource Manager innehåller funktioner för distribution som du kan anpassa hello distributionen och konfigurationen av relaterade resurser. Till exempel genom att använda Resource Manager kan du distribuera och konfigurera ett program som består av flera virtuella datorer, en belastningsutjämnare och Azure SQL-databas som en enda enhet.

Du kan utveckla dessa distributioner med en Azure Resource Manager-mall som är en JSON-formaterade dokument. Mallar kan du definiera en distribution och hantera dina program genom att använda deklarativa mallar, i stället för skript. Mallar kan användas i olika miljöer som testning, mellanlagring och produktion. Till exempel med hjälp av mallar du kan lägga till en knapp tooa GitHub-repo som distribuerar hello koden i hello lagringsplatsen tooa uppsättning Azure-tjänster med en enda klickning.

>**När toouse**: Använd Resource Manager-mallar när du vill använda en mall-baserad distribution för din app som du kan hantera programmässigt med hjälp av REST API: er hello Azure CLI och Azure PowerShell.

>**Kom igång**: tooget startats med hjälp av mallar, se [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Förstå konton, prenumerationer och fakturering

Som utvecklare, vi som toodive i hello koden och försök tooget så snabbt som möjligt med att vår programmen kan köras igång. Vi vill verkligen tooencourage toostart arbetar i Azure så enkelt som möjligt. toohelp gör det enkelt, Azure erbjuder en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/free/). Vissa tjänster även har en ”testa det gratis” funktionalitet som [Azure App Service](https://tryappservice.azure.com/), som inte kräver att du skapar ett konto för även. Roliga eftersom den är toodive till kodning och distribuera ditt program tooAzure är också viktigt tootake vissa tid toounderstand hur Azure fungerar från en synvinkel av användarkonton, prenumerationer och fakturering.

### <a name="what-is-an-azure-account"></a>Vad är ett Azure-konto?

toobe kan toocreate eller arbeta med en Azure-prenumeration, måste du ha ett Azure-konto. Ett Azure-konto är helt enkelt en identitet i Azure AD eller i en katalog, t.ex en arbets- eller skolkonto organisation, som är betrodd av Azure AD. Om du inte tillhör toosuch en organisation, kan du alltid skapa en prenumeration med ditt Microsoft-Account som är betrott av Azure AD. toolearn mer information om hur du integrerar lokala Windows Server Active Directory med Azure AD finns [integrera dina lokala identiteter med Azure Active Directory](../../active-directory/active-directory-aadconnect.md).

Alla Azure-prenumerationer har en förtroenderelation med en Azure AD-instans. Detta innebär att den litar den directory tooauthenticate användare, tjänster och enheter. Flera prenumerationer kan lita hello samma katalog, men en prenumeration litar bara på en katalog. Det finns fler toolearn [hur Azure-prenumerationer är associerade med Azure Active Directory](../../active-directory/active-directory-how-subscriptions-associated-directory.md).

Dessutom toodefining enskilda Azure-konto identiteter, kallas även *användare*, du kan också definiera *grupper* i Azure AD. Att skapa användargrupper är ett bra sätt toomanage åtkomst tooresources i en prenumeration med hjälp av rollbaserad åtkomstkontroll (RBAC). hur toocreate grupper, se toolearn [skapar en grupp i Azure Active Directory preview](../../active-directory/active-directory-groups-create-azure-portal.md). Du kan också skapa och hantera grupper av [med hjälp av PowerShell](../../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Hantera prenumerationer

En prenumeration är en logisk enhet i Azure-tjänster som är länkade tooan Azure-konto. Varje associerade konto har en roll i en prenumeration. Fakturering för Azure-tjänster görs på grundval av per prenumeration. En lista över hello tillgänglig prenumerationserbjudanden efter typ, se [Erbjudandeinformationen för Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Administratörsroller

En Azure-prenumeration har flera konto administratörsroller, som du kan tilldela när som helst.

-   **Kontoadministratör**: den här rollen har full kontroll över hello prenumeration och är hello-konto som är ansvarig för fakturering.

-   **Tjänstadministratör**: den här rollen har kontroll över alla hello-tjänster i hello prenumerationen. Som standard är detta hello samma konto som hello kontoadministratör.

-   **Medadministratör**: den här rollen har hello samma åtkomst som hello tjänstadministratör, förutom att det går inte att ändra hello associering av hello prenumeration tooan Azure-katalogen.

toolearn mer om administratörsroller, se [hur tooadd eller ändra Azure-administratörsroller](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Resursgrupper

När du etablerar en ny Azure-tjänster måste göra du det i en viss prenumeration. Enskilda Azure-tjänster, som också kallas resurser skapas i hello kontexten för en resursgrupp. Resursgrupper gör det enklare toodeploy och hantera resurser i ditt program. En resursgrupp ska innehålla alla hello resurser för programmet som du vill toowork med som en enhet. Du kan flytta resurser mellan resursgrupper och även toodifferent prenumerationer. toolearn om hur du flyttar resurser, se [flytta resurser toonew resursgrupp eller prenumeration](../../resource-group-move-resources.md).

Hej resursutforskaren Azure är ett bra verktyg för visualisering av hello-resurser som du redan har skapat i din prenumeration. Det finns fler toolearn [tooview Använd resursutforskaren för Azure och ändra resurser](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-tooresources"></a>Bevilja åtkomst tooresources

När du tillåter åtkomst tooAzure resurser, men det är alltid en bra idé att ge användare med hello minst privilegium som krävs tooperform en viss uppgift.

-   **Rollbaserad åtkomstkontroll (RBAC)**: I Azure, kan du bevilja åtkomst toouser konton (säkerhetsobjekt) på ett angivet omfång: prenumeration, resursgrupp eller enskilda resurser. RBAC kan du distribuera en uppsättning resurser i en resursgrupp och bevilja behörigheter tooa specifik användare eller grupp. Du kan också begränsa åtkomst tooonly hello resurser som tillhör toohello målresursgruppen. Du kan också ge åtkomst tooa enda resurs, till exempel en virtuell dator eller virtuella nätverk. toogrant access kan du tilldela en roll toohello användare, grupp eller tjänstens huvudnamn. Det finns många fördefinierade roller och du kan också definiera dina egna anpassade roller.

    >**När toouse**: när du behöver detaljerad åtkomsthantering för användare och grupper.

    >**Kom igång**: det finns fler toolearn [Kom igång med åtkomsthantering i hello Azure-portalen](../../active-directory/role-based-access-control-what-is.md).

-   **Service principal objekt**: dessutom tooproviding komma åt toouser säkerhetsobjekt och grupper, kan du bevilja hello samma åtkomst tooa tjänstens huvudnamn.

    > **När toouse**: när du programmässigt hantera Azure-resurser eller bevilja tillgång till program. Mer information finns i [supplication Skapa Active Directory och tjänstens huvudnamn](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Taggar

Azure Resource Manager kan du tilldela anpassade taggar tooindividual resurser. Taggar som nyckel / värde-par, kan vara användbart när du behöver tooorganize resurser för fakturering och övervakning. Taggar ger dig ett sätt tootrack resurser över flera resursgrupper. Du kan tilldela taggar i hello-portalen i hello Azure Resource Manager-mall eller programmässigt med hjälp av hello REST-API, hello Azure CLI eller PowerShell. Du kan tilldela flera taggar tooeach resurs. Det finns fler toolearn [med hjälp av taggar tooorganize resurserna i Azure](../../resource-group-using-tags.md).

### <a name="billing"></a>Fakturering

Hello flytt från lokala datorer toocloud värdbaserade tjänster, är spårning och utvärdering av användningen av tjänsten och relaterade kostnader betydande problem. Det är viktigt toobe kan tooestimate vilka nya resurser kostnad toorun månadsvis. Du måste också toobe kan tooproject hur hello fakturering söker efter en viss månad baserat på hello aktuella utgifter.

#### <a name="get-resource-usage-data"></a>Hämta resursanvändningsdata

Azure tillhandahåller en uppsättning fakturering REST API: er som ger åtkomst tooresource användnings- och metadatainformation för Azure-prenumerationer. Dessa fakturering API: er ger du hello möjlighet toobetter förutsäga och hantera Azure kostnader. Du kan spåra och analyserar indelad i timmar, skapa utgiftsgränsen aviseringar och förutsäga framtida fakturering baserat på aktuella användningstrender.

>**Kom igång**: toolearn mer information om hur du använder hello fakturering API: er Se [översikt över Azure Billing-användning och RateCard APIs](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Förutsäga framtida kostnader

Även om det är svårt tooestimate kostnader i förväg Azure har en [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/) som du kan använda när du uppskattar hello kostnaden för distribuerade resurser. Du kan också använda hello fakturering bladet i hello portal och hello fakturering REST API: er tooestimate framtida kostnader, baserat på aktuella förbrukningen.

>**Kom igång**: se [översikt över Azure Billing-användning och RateCard APIs](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Ställa in faktureringsvarningar

När du har distribuerat programmet eller lösningen på Azure, kan du skapa aviseringar som skickar du e-post när du hanterar hello utgiftsgränser som definieras i hello avisering.

>**Kom igång**: toolearn finns fler [konfigurera fakturering aviseringar för Microsoft Azure-prenumerationer](../../billing-set-up-alerts.md).
