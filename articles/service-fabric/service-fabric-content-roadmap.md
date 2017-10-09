---
title: Mer om Azure Service Fabric aaaLearn | Microsoft Docs
description: "Läs mer om grundläggande begrepp som hello och huvudområden i Azure Service Fabric. Översikt över Service Fabric utökad och hur toocreate mikrotjänster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/14/2017
ms.author: ryanwi
ms.openlocfilehash: 7fe8de777755be11635912613bb5b970e3fe3ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="so-you-want-toolearn-about-service-fabric"></a>Så du toolearn om Service Fabric?
Azure Service Fabric är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga mikrotjänster.  Service Fabric har en stor yta men och det är mycket toolearn.  Den här artikeln innehåller en sammanfattning av Service Fabric och beskriver hello grundbegrepp programmeringsmodeller, program livscykel, testa, kluster och övervakning av hälsotillstånd. Läs hello [översikt](service-fabric-overview.md) och [vad är mikrotjänster?](service-fabric-overview-microservices.md) för en introduktion och hur Service Fabric kan vara används toocreate mikrotjänster. Den här artikeln innehåller inte en omfattande lista för innehåll, men länka toooverview och komma igång artiklar för varje del av Service Fabric. 

## <a name="core-concepts"></a>Huvudkoncept
[Service Fabric-terminologi](service-fabric-technical-overview.md), [programmodell](service-fabric-application-model.md), och [stöds programmeringsmodeller](service-fabric-choose-framework.md) får du större begrepp och beskrivningar, men här finns hello grunderna.

<table><tr><th>Huvudkoncept</th><th>Designtillfället</th><th>Körtid</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Designtid: programtyp, tjänsttyp, programpaket och manifestet, servicepaket och manifestet
En typ av program är hello namn/version tilldelad tooa samling tjänsttyper. Detta är definierad i en *ApplicationManifest.xml* fil som är inbäddad i ett paket för programkatalogen. hello programpaket sedan kopieras toohello Service Fabric klustret avbildningsarkivet. Du kan sedan skapa en namngiven programmet från den här programtypen som körs inom hello kluster. 

En tjänst är hello namn/version tilldelade tooa service code paket, data-paket och konfigurationspaket. Detta har definierats i en ServiceManifest.xml-fil som är inbäddad i ett paket katalog. hello paketet katalog sedan refereras till av ett programpaket *ApplicationManifest.xml* fil. Inom hello kluster kan när du har skapat ett namngivet program, du skapa en namngiven tjänst från en hello programmet typen tjänsttyper. En typ av beskrivs av dess *ServiceManifest.xml* fil. hello tjänsttypen består av körbar kod service konfigurationsinställningar, som läses in vid körning, och statiska data som används av hello-tjänsten.

![Service Fabric programtyper och typer av tjänster][cluster-imagestore-apptypes]

hello programpaket är en diskkatalog som innehåller typen hello-programmet *ApplicationManifest.xml* filen som refererar till hello servicepaket för varje typ av tjänst som utgör hello programtyp. Ett programpaket för en typ av e-program kan exempelvis innehålla referenser tooa kön servicepaket, en klientdel servicepaket och inget tjänstepaket för databasen. hello-filer i hello programkatalogen paketet är kopierade toohello Service Fabric klustret avbildningsarkivet. 

Inget tjänstepaket är en diskkatalog som innehåller hello service typen *ServiceManifest.xml* filen som refererar till hello kod, statiska data och konfigurationspaket för hello service-typen. hello filer i paketet katalog hello refereras av typen hello-programmet *ApplicationManifest.xml* fil. Inget tjänstepaket kan till exempel referera toohello kod, statiska data och konfigurationspaket som utgör en databastjänst.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Körtid: kluster och program, tjänster, partitioner och repliker noderna
Ett [Service Fabric-kluster](service-fabric-deploy-anywhere.md) är en nätverksansluten uppsättning virtuella eller fysiska datorer som dina mikrotjänster distribueras till och hanteras från. Kluster kan skala toothousands datorer.

En dator eller virtuell dator som ingår i ett kluster kallas för en nod. Varje nod har tilldelats ett nodnamn (en sträng). Noder har egenskaper som till exempel placeringsegenskaper. Varje dator eller virtuell dator har en Windows-tjänsten för automatisk start, `FabricHost.exe`, som startar vid start och startar sedan två körbara filer: `Fabric.exe` och `FabricGateway.exe`. Dessa två körbara filer som utgör hello-nod. För utveckling eller tester scenarier, du kan vara värd för flera noder i en enskild dator eller virtuell dator genom att köra flera instanser av `Fabric.exe` och `FabricGateway.exe`.

Ett namngivet program är en samling namngivna tjänster som utför en viss funktion eller funktioner. En tjänst som utför en fullständig och fristående funktion (det kan starta och köra oberoende av andra tjänster) och består av koden, konfiguration och data. När ett programpaket är kopierade toohello avbildningsarkivet kan skapa du en instans av programmet hello inom hello klustret genom att ange hello programmet paketet programtyp (med dess namn/version). Varje typ av programinstans tilldelas ett URI-namn som ser ut som *fabric: / MyNamedApp*. Du kan skapa flera namngivna program från en enda programtyp inom ett kluster. Du kan också skapa namngivna program från annan programtyper. Varje namngivet program är oberoende av varandra hanterade och en ny version.

När du har skapat ett namngivet program skapar du en instans av en av dess tjänsttyper (tjänsten) i hello kluster genom att ange hello tjänsttypen (med dess namn/version). Varje typ tjänstinstans tilldelas ett URI-namn omfång under dess namngivna program-URI. Till exempel om du skapar en ”mindatabas” med namnet tjänsten inom en ”MyNamedApp” med namnet programmet hello URI ser ut så: *fabric: / MyNamedApp/mindatabas*. Du kan skapa en eller flera namngivna tjänster i en namngiven programmet. Varje tjänsten kan ha sin egen partitionsschema och instansreplik/räknar. 

Det finns två typer av tjänster: tillståndslösa och tillståndskänsliga. Tillståndslösa tjänster kan lagra beständiga tillstånd i en extern storage-tjänst som Azure Storage, Azure SQL Database eller Azure Cosmos DB. Använd tillståndslösa tjänsten när hello-tjänsten har ingen beständig lagring alls. En tillståndskänslig använder tjänsten Service Fabric toomanage Tjänststatus via dess tillförlitliga samlingar eller Reliable Actors programmeringsmodeller. 

När du skapar en namngiven tjänst kan ange du ett partitionsschema. Tjänster med stora mängder tillstånd dela hello data över partitioner. Varje partition är ansvarig för en del av hello fullständig tillståndet för hello-tjänsten, som är fördelade på hello klusternoder. Inom en partition har tillståndslösa tjänster för namngivna instanser medan tillståndskänslig namngivna tjänster ha repliker. Vanligtvis har tillståndslösa namngivna tjänster bara en partition eftersom de har inget internt tillstånd. Stateful som heter tjänster upprätthålla deras tillstånd i repliker och varje partition har sin egen replikuppsättningen. Läsa och skriva åtgärder utförs i en replik (kallas hello primära). Toostate ändras från write åtgärder är replikerade toomultiple repliker (kallas Active sekundärservrar). 

hello visar följande diagram hello förhållandet mellan program och instanser av tjänsten, partitioner och repliker.

![Partitioner och repliker i en tjänst][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Partitionering, skalning och tillgänglighet
[Partitionering](service-fabric-concepts-partitioning.md) är inte unikt tooService Fabric. Ett välkänt formulär partitionering är Datapartitionering eller horisontell partitionering. Tillståndskänsliga tjänster med stora mängder tillstånd dela hello data över partitioner. Varje partition är ansvarig för en del av hello hello tjänstens fullständiga tillstånd. 

hello repliker för varje partition är fördelade på hello stycken klusternoder, vilket gör att din namngivna Tjänststatus för[skala](service-fabric-concepts-scalability.md). När hello data behöver växa, partitioner växer och Service Fabric balanserar partitioner mellan noder toomake effektiv användning av maskinvaruresurser. Om du lägger till nya noder toohello klustret kommer Service Fabric balansera hello partition repliker över hello ökat antal noder. Övergripande prestanda förbättras och konkurrens om åtkomst toomemory minskar. Om hello noder i klustret hello inte används effektivt, kan du minska hello antalet noder i klustret hello. Service Fabric balanserar igen hello partition repliker för hello minskade antalet noder toomake bättre användning av hello maskinvara på varje nod.

Inom en partition har tillståndslösa tjänster för namngivna instanser medan tillståndskänslig namngivna tjänster ha repliker. Vanligtvis har tillståndslösa namngivna tjänster bara en partition eftersom de har inget internt tillstånd. hello partition instanser ange för [tillgänglighet](service-fabric-availability-services.md). Om en instans misslyckas, andra instanser fortsätta toooperate normalt och Service Fabric skapar sedan en ny instans. Stateful som heter tjänster upprätthålla deras tillstånd i repliker och varje partition har sin egen replikuppsättningen. Läsa och skriva åtgärder utförs i en replik (kallas hello primära). Toostate ändras från write åtgärder är replikerade toomultiple repliker (kallas Active sekundärservrar). Service Fabric skapar en replik misslyckas, en ny replik från hello befintliga replikeringar.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Tillståndslösa och tillståndskänsliga mikrotjänster för Service Fabric
Service Fabric kan toobuild-program som består av mikrotjänster eller behållare. Tillståndslösa mikrotjänster (till exempel protokollet gateways och webbproxyservrar) har inte tillståndet föränderliga utanför en begäran och dess svar från hello-tjänsten. Arbetsroller i Azure Cloud Services är ett exempel på en tillståndslös tjänst. Tillståndskänsliga mikrotjänster (till exempel användarkonton, databaser, enheter, shoppingvagnar och köer) Underhåll tillståndet föränderliga, auktoritär utöver hello begäran och sitt svar. Dagens Internet-skala program består av en kombination av tillståndslösa och tillståndskänsliga mikrotjänster. 

En nyckel differentation med Service Fabric är starkt fokus på bygga tillståndskänsliga tjänster, antingen med hello [inbyggda programmeringsmodeller ](service-fabric-choose-framework.md) eller med container tillståndskänsliga tjänster. Hej [Programscenarier](service-fabric-application-scenarios.md) beskrivs hello scenarier där tillståndskänsliga tjänster används.

Varför har tillståndskänsliga mikrotjänster tillsammans med tillståndslös dem? hello två huvudsakliga skälen är:

* Du kan skapa hög genomströmning, låg latens, feltoleranta online transaktionsbearbetning (OLTP) tjänster genom att hålla koden och data Stäng på hello samma dator. Några exempel är interaktiva skyltfönster, sökning, Sakernas Internet (IoT)-system, handelssystem, kreditkort bearbetning och bedrägeri identifiering system och personliga Posthantering.
* Du kan förenkla programdesign. Tillståndskänsliga mikrotjänster ta bort hello behovet av ytterligare köer och cacheminnen som traditionellt krävs tooaddress hello tillgänglighet och svarstid kraven i ett rent tillståndslösa program. Tillståndskänsliga tjänster är naturligt hög tillgänglighet och låg latens, vilket minskar hello antalet flytta delar toomanage i ditt program som helhet.

## <a name="supported-programming-models"></a>Programmeringsmodeller som stöds
Service Fabric erbjuder flera sätt toowrite och hantera dina tjänster. Tjänster kan använda hello Service Fabric API: er tootake nytta av funktioner och ramverk för programmet hello-plattformen. Tjänster kan också vara alla kompilerade körbara program på alla språk och finns på ett Service Fabric-kluster. Mer information finns i [stöds programmeringsmodeller](service-fabric-choose-framework.md).

### <a name="containers"></a>Behållare
Som standard Service Fabric distribuerar och aktiverar tjänster som processer. Service Fabric kan också distribuera tjänster i [behållare](service-fabric-containers-overview.md). Allt du kan blanda tjänster i processer och tjänster i behållare i hello samma program. Service Fabric stöder distribution av Linux-behållare Windows behållare på Windows Server 2016. Du kan distribuera befintliga program, tillståndslösa tjänster eller tillståndskänsliga tjänster i behållare. 

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) är ett enkelt ramverk för att skriva tjänster som integreras med hello Service Fabric-plattformen och dra nytta av hello fullständig uppsättning funktioner. Reliable Services kan vara tillståndslös (liknande toomost service plattformar, till exempel webbservrar eller arbetsroller i Azure Cloud Services), där tillståndet sparas i en extern lösning, till exempel Azure DB eller Azure Table Storage. Reliable Services kan också vara stateful, där tillstånd sparat direkt i själva med hjälp av tillförlitliga samlingar hello-tjänsten. Tillståndet gjort [högtillgänglig](service-fabric-availability-services.md) via replikering och distribueras via [partitionering](service-fabric-concepts-partitioning.md), allt hanteras automatiskt av Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Bygger på Reliable Services hello [tillförlitliga aktören](service-fabric-reliable-actors-introduction.md) framework är ett programramverk som implementerar hello virtuella aktören mönster, baserat på hello aktören designmönstret. hello tillförlitliga aktören framework används oberoende enheter beräkning och tillstånd med Enkeltrådig körning kallas aktörer. hello tillförlitliga aktören ramverket innehåller inbyggda kommunikation för aktörer och förinställda beständighet och skalbar konfigurationer.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric kan integreras med [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) som en förstklassigt programmeringsmodell för att skapa webb- och API-program

### <a name="guest-executables"></a>Gästen körbara filer
En [gäst körbara](service-fabric-deploy-existing-app.md) är en befintlig, valfri körbar fil (skrivs på olika språk) finns på ett Service Fabric-kluster tillsammans med andra tjänster. Gästen körbara filer integrerar inte direkt med Service Fabric API: er. Men de fortfarande dra nytta av funktioner hello plattform erbjudanden, till exempel anpassade hälsa och läsa in reporting och tjänsten synlighet genom att anropa REST API: er. De kan också ha fullständig programmets livscykeln för support. 

## <a name="application-lifecycle"></a>Programlivscykel
Som med andra plattformar, ett program på Service Fabric vanligtvis passerar hello följande faser: design, utveckling, testning, distribution, uppgradering, underhåll och borttagning. Service Fabric har förstklassigt stöd för hello fullständiga programmet livscykeln för molnprogram från utveckling till distribution, dagliga hantering och underhåll tooeventual inaktivering. hello tjänstmodell kan flera olika roller tooparticipate oberoende i hello programmet livscykel. [Service Fabric application livscykel](service-fabric-application-lifecycle.md) ger en översikt över hello API: er och hur de används av hello olika roller i hello faserna i livscykeln för hello Service Fabric-programmet. 

hello hela applivscykeln kan hanteras med [PowerShell-cmdlets](/powershell/module/ServiceFabric/), [C#-API: er](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java API: er](/java/api/system.fabric._application_management_client), och [REST API: er](/rest/api/servicefabric/). Du kan också ställa in kontinuerlig integration/kontinuerlig distribution pipelines med hjälp av verktyg som [Visual Studio Team Services](service-fabric-set-up-continuous-integration.md) eller [Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md).

hello följande Microsoft Virtual Academy videon beskriver hur toomanage livscykeln för programmet:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Testa program och tjänster
toocreate verkligen molnskala tjänster, är det kritiska tooverify att dina program och tjänster kan klara verkliga fel. hello fel Analysis Service är utformad för att testa tjänster som bygger på Service Fabric. Med hello [fel Analysis Service](service-fabric-testability-overview.md), du kan medföra meningsfulla fel och köra scenarier för testning mot dina program. Dessa fel och scenarier utöva och validera hello många tillstånd och övergångar som en tjänst får under dess livslängd i en kontrollerad, säker och konsekvent sätt.

[Åtgärder](service-fabric-testability-actions.md) riktade till en tjänst för att testa med hjälp av enskilda fel. En service-utvecklare kan använda dessa som byggblock toowrite komplicerade scenarier. Exempel på simulerade fel är:

* Starta om en nod toosimulate valfritt antal situationer där en dator eller virtuell dator startas.
* Flytta en replik av din tillståndskänslig service toosimulate belastningsutjämning, redundans eller uppgradering av programmet.
* Anropa förlorar kvorum på tillståndskänslig service-toocreate en situation där skrivåtgärder inte kan fortsätta eftersom det inte finns tillräckligt med ”säkerhetskopiering” eller ”sekundär” repliker tooaccept nya data.
* Anropa förlust av data på en tillståndskänslig service toocreate en situation där alla InMemory-tillstånd rensas helt ut.

[Scenarier](service-fabric-testability-scenarios.md) komplex består av en eller flera åtgärder. hello fel Analysis Services innehåller två inbyggda kompletta scenarier:

* [Chaos scenariot](service-fabric-controlled-chaos.md)-simulerar kontinuerlig, överlagrad fel (korrekt och städat) under hela hello kluster under en längre tid.
* [Redundans scenariot](service-fabric-testability-scenarios.md#failover-test)-en version av hello chaos Testscenario som riktar sig till en specifik tjänst partition medan andra tjänster påverkas inte.

## <a name="clusters"></a>Kluster
Ett [Service Fabric-kluster](service-fabric-deploy-anywhere.md) är en nätverksansluten uppsättning virtuella eller fysiska datorer som dina mikrotjänster distribueras till och hanteras från. Kluster kan skala toothousands datorer. En dator eller virtuell dator som ingår i ett kluster kallas för en nod i klustret. Varje nod har tilldelats ett nodnamn (en sträng). Noder har egenskaper som till exempel placeringsegenskaper. Varje dator eller virtuell dator har en autostarttjänst `FabricHost.exe`, som startar vid start och startar sedan två körbara filer: Fabric.exe och FabricGateway.exe. Dessa två körbara filer som utgör hello-nod. För att testa scenarier, du kan vara värd för flera noder i en enskild dator eller virtuell dator genom att köra flera instanser av `Fabric.exe` och `FabricGateway.exe`.

Service Fabric-kluster kan skapas på virtuella eller fysiska datorer som kör Windows Server- eller Linux. Du kan toodeploy och köra Service Fabric-program i en miljö där du har en uppsättning Windows Server- eller Linux-datorer som är sammankopplade: lokalt, i Microsoft Azure eller på alla moln-providern.

hello beskrivs följande Microsoft Virtual Academy videon Service Fabric-kluster:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Kluster i Azure
Service Fabric-kluster som körs på Azure möjliggör integrering med andra Azure-funktioner och tjänster, vilket gör åtgärder och hantering av hello klustret enklare och mer tillförlitlig. Ett kluster är en Azure Resource Manager-resurs så att du kan ange modellen kluster som andra resurser i Azure. Resource Manager innehåller också enklare att hantera alla resurser som används av hello klustret som en enda enhet. Kluster på Azure är integrerade med Azure-diagnostik och logganalys. Klustret nodtyper är [skalningsuppsättningar i virtuella](/azure/virtual-machine-scale-sets/index), så autoskalning funktionen är inbyggd i.

Du kan skapa ett kluster i Azure via hello [Azure-portalen](service-fabric-cluster-creation-via-portal.md), från en [mallen](service-fabric-cluster-creation-via-arm.md), eller från [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

möjligt toobuild hello förhandsgranskning av Service Fabric på Linux, distribuera och hantera hög tillgänglighet och skalbara program på Linux precis som i Windows. hello Service Fabric ramverk (Reliable Services och Reliable Actors) är tillgängliga i Java på Linux, i tillägg tooC # (.NET Core). Du kan också skapa [körbara gästtjänster](service-fabric-deploy-existing-app.md) med valfritt språk eller ramverk. Hello preview stöder dessutom samordna Docker-behållare. Docker-behållare kan köra gäst körbara filer eller interna Service Fabric-tjänster som använder hello Service Fabric-ramverk. Mer information finns [Service Fabric på Linux](service-fabric-linux-overview.md).

Eftersom Service Fabric i Linux är en förhandsversion är det vissa funktioner som bara finns i Windows, inte i Linux. toolearn läsa fler [skillnader mellan Service Fabric på Linux och Windows](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Fristående kluster
Service Fabric ger ett installationspaket för du toocreate fristående Service Fabric-kluster lokalt eller på alla moln-providern. Fristående kluster ger du hello frihet toohost ett kluster, var du vill. Om dina data är ämne toocompliance eller andra begränsningar, eller du tookeep din lokala data, du kan vara värd för ditt eget kluster och program. Service Fabric-program kan köras i flera värdbaserade miljöer utan ändringar, så att dina kunskaper för att skapa program som följer med från en värd tooanother i miljön. 

[Skapa din första fristående Service Fabric-kluster](service-fabric-get-started-standalone-cluster.md)

Linux fristående kluster stöds inte ännu.

### <a name="cluster-security"></a>Klustersäkerhet
Kluster måste vara skyddad tooprevent obehöriga användare från att ansluta tooyour kluster, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt toocreate ett oskyddat kluster på så sätt kan anonyma användare tooconnect tooit om hanteringsslutpunkter exponeras toohello offentliga internet. Det är inte möjligt toolater aktivera säkerheten på ett oskyddat kluster: Klustersäkerhet är aktiverat när klustret skapas.

hello klustret scenarier är:
* Säkerhet för nod till nod
* Klient-till-nod-säkerhet
* Rollbaserad åtkomstkontroll (RBAC)

Mer information finns [skydda ett kluster](service-fabric-cluster-security.md).

### <a name="scaling"></a>Skalning
Om du lägger till nya noder toohello klustret balanserar Service Fabric hello partition repliker och instanser för hello ökat antalet noder. Övergripande prestanda förbättras och konkurrens om åtkomst toomemory minskar. Om hello noder i klustret hello inte används effektivt, kan du minska hello antalet noder i klustret hello. Service Fabric igen balanserar hello partition repliker och instanser för hello minskade antalet noder toomake bättre användning av hello maskinvara på varje nod. Du kan skala kluster på Azure antingen [manuellt](service-fabric-cluster-scale-up-down.md) eller [programmässigt](service-fabric-cluster-programmatic-scaling.md). Fristående kluster kan skalas [manuellt](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Klusteruppgradering
Med jämna mellanrum släpps nya versioner av hello Service Fabric runtime. Utför runtime eller fabric, uppgraderingar på klustret så att du alltid använder en [version som stöds](service-fabric-support.md). Dessutom toofabric uppgraderingar, du kan också uppdatera klusterkonfigurationen, till exempel certifikat eller portar för programmet.

Ett Service Fabric-kluster är en resurs som du äger, men delvis hanteras av Microsoft. Microsoft ansvarar för uppdatering hello underliggande OS och utföra fabric uppgraderingar på ditt kluster. Du kan ange ditt kluster tooreceive automatisk fabric uppgraderingar, när Microsoft publicerar en ny version eller välja tooselect en stöds fabric-version som du vill. Uppgradering av infrastrukturen och konfigurationen kan anges via hello Azure-portalen eller via Resource Manager. Mer information finns [uppgradera ett Service Fabric-kluster](service-fabric-cluster-upgrade.md). 

En fristående klustret är en resurs som du äger helt. Du är ansvarig för uppdatering hello underliggande OS och initierar fabric uppgraderingar. Om klustret kan ansluta för[https://www.microsoft.com/download](https://www.microsoft.com/download), du kan ställa in kluster tooautomatically hämtningen och etablera hello nya Service Fabric runtime-paketet. Du kan sedan initiera hello uppgraderingen. Om klustret inte kan komma åt [https://www.microsoft.com/download](https://www.microsoft.com/download), du kan hämta hello nya runtime-paketet manuellt från en internet-anslutna datorn och startar sedan hello uppgraderingen. Mer information finns [uppgradera en fristående Service Fabric-kluster](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Hälsoövervakning
Service Fabric introducerar en [hälsomodell](service-fabric-health-introduction.md) utformad tooflag ohälsosamt kluster och programmet villkoren på specifika enheter (till exempel klusternoder och tjänsten repliker). Hej hälsomodell använder hälsa rapportörer (systemkomponenter och watchdogs). hello målet är enkelt och snabbt diagnos och reparation. Service-skrivare måste toothink förskott om hälsotillstånd och hur för[utforma hälsa reporting](service-fabric-report-health.md#design-health-reporting). Eventuella villkor som kan påverka hälsa redovisas, särskilt om kan hjälpa flaggan problem Stäng toohello rot. hello hälsoinformation kan spara tid och pengar på felsökning och undersökning när hello-tjänsten är igång och körs i skala i produktion.

hello Service Fabric-rapportörer Övervakare identifiera villkor av intresse. De rapport om dessa villkor baserat på deras lokala vyn. Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) aggregerar hälsa data som skickas av alla rapportörer toodetermine om enheter är globalt felfria. hello modellen är avsedda toobe omfattande, flexibel och enkel toouse. hello kvaliteten på hello hälsorapporter anger hello riktighet hello hälsotillståndsvy hello-klustret. Falska positiva identifieringar som visar felaktigt ohälsosamt problem kan påverka uppgraderingar och andra tjänster som använder hälsa data. Exempel på sådana tjänster är reparation och mekanismer för aviseringar. Därför tänka är nödvändiga tooprovide rapporter som samlar in villkor för intresse för hello bästa möjliga sättet.

Rapportering kan göras från:
* hello övervakade Service Fabric-tjänsten målrepliker eller instanser.
* Internt watchdogs distribueras som en Service Fabric-tjänsten till exempel ett Service Fabric tillståndslös som övervakar villkor och skickar rapporter. Hej watchdogs kan distribueras på alla noder eller kan vara tillhör toohello övervakas tjänst.
* Internt watchdogs som körs på hello Service Fabric-noder men har inte implementerats som Service Fabric-tjänster.
* Externa watchdogs avsökningen hello resursen från utanför hello Service Fabric-klustret (till exempel övervakningstjänsten som Gomez).

Out of box hello rapport Service Fabric-komponenter hälsotillstånd för alla entiteter i hello klustret. [System hälsorapporter](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) ger inblick i klustret och programmet funktioner och flaggan problem med hjälp av hälsotillstånd. System hälsorapporter Kontrollera att entiteter implementeras och fungerar korrekt ur hello hello Service Fabric Runtime för program och tjänster. hello rapporter inte ange några hälsoövervakning av hello affärslogik hello service eller identifiera låsta processer. tooadd information specifik tooyour tjänstens logik [implementera anpassade hälsa reporting](service-fabric-report-health.md) i dina tjänster.

Service Fabric innehåller flera olika sätt för[visa hälsorapporter](service-fabric-view-entities-aggregated-health.md) samman i hello health store:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) eller andra visualiseringsverktyg för.
* Hälsoförfrågningar (via [PowerShell](/powershell/module/ServiceFabric/), hello [C# FabricClient APIs](/api/system.fabric.fabricclient.healthclient) och [Java FabricClient APIs](/java/api/system.fabric._health_client), eller [REST API: er](/rest/api/servicefabric)).
* Allmänna frågor som returnerar en lista över enheter som har hälsa som en av hello-egenskaper (via PowerShell, hello API eller REST).

hello följande Microsoft Virtual Academy videon beskriver hello Service Fabric-hälsomodell och hur de används:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="next-steps"></a>Nästa steg
* Lär dig hur toocreate en [kluster i Azure](service-fabric-cluster-creation-via-portal.md) eller en [fristående kluster på Windows](service-fabric-cluster-creation-for-windows-server.md).
* Försök att skapa en tjänst med hello [Reliable Services](service-fabric-reliable-services-quick-start.md) eller [Reliable Actors](service-fabric-reliable-actors-get-started.md) programmeringsmodeller.
* Lär dig hur för[migrera från molntjänster](service-fabric-cloud-services-migration-differences.md).
* Läs för[övervaka och diagnostisera services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Läs för[testa dina appar och tjänster](service-fabric-testability-overview.md).
* Läs för[hantera och samordna klusterresurser](service-fabric-cluster-resource-manager-introduction.md).
* Titta igenom hello [Service Fabric-exempel](http://aka.ms/servicefabricsamples).
* Lär dig mer om [Service Fabric supportalternativ](service-fabric-support.md).
* Läs hello [teambloggen](https://blogs.msdn.microsoft.com/azureservicefabric/) för artiklar och meddelanden.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
