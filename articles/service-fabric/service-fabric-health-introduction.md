---
title: "aaaHealth övervakning i Service Fabric | Microsoft Docs"
description: "En introduktion toohello Azure Service Fabric hälsoövervakning modell, som ger övervakning av hello klustret och dess program och tjänster."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 904f36374ca6ca7e4caa1d43c92584e7e4c50087
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-health-monitoring"></a>Introduktion tooService Fabric-hälsoövervakning
Azure Service Fabric introducerar en hälsomodell som ger omfattande, flexibla och utökningsbara hälsoutvärderingen och rapportering. hello modellen tillåter nära realtid övervakning av hello tillståndet för hello kluster och hello-tjänster som körs i den. Du kan lätt få information om hälsa och rätta potentiella problem innan de sprids och orsaka massiv avbrott. I vanliga modellen hello samman services skicka rapporter baserat på deras lokala vyer och att informationen är tooprovide en övergripande klusternivå vy.

Service Fabric-komponenter använder den här omfattande hälsa modellen tooreport det aktuella tillståndet. Du kan använda hello samma mekanism tooreport hälsa från dina program. Om du investerar i hög kvalitet hälsa reporting som samlar in din anpassade villkor kan du identifiera och åtgärda problem för tillämpningsprogrammet körs mycket enklare.

hello följande Microsoft Virtual Academy videon beskrivs också hello Service Fabric-hälsomodell och hur de används:<center><a target="_blank" href="https://mva.microsoft.com/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> Vi har startats hello hälsa undersystemet tooaddress behovet av övervakade uppgraderingar. Service Fabric ger övervakade program och kluster uppgraderingar som säkerställa fullständig tillgänglighet, utan driftstopp och minimala toono åtgärder från användaren. tooachieve dessa mål hello uppgradering kontrollerar hälsotillstånd baserat på konfigurerad uppgradera principer. En uppgradering kan fortsätta endast när hälsa respekterar önskade trösklar. Annars hello uppgraderingen automatiskt återställs antingen eller pausats toogive administratörer en chansen toofix hello problem. toolearn mer om programmet uppgraderingar finns [i den här artikeln](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Hälsoarkivet
Hej hälsoarkivet behåller hälsorelaterade information om entiteter i hello kluster för enkel hämtning och utvärdering. Det implementeras som en Service Fabric beständiga tillståndskänslig service tooensure hög tillgänglighet och skalbarhet. hello health store är en del av hello **fabric: / System** programmet och det är tillgängligt när hello klustret fungerar och körs.

## <a name="health-entities-and-hierarchy"></a>Hälsotillstånd entiteter och hierarki
hello hälsa entiteter är ordnade i logiska hierarkin som samlar in interaktioner och beroenden mellan olika enheter. Hej hälsoarkivet skapar automatiskt hälsa entiteter och hierarki baserat på rapporter som tagits emot från Service Fabric-komponenter.

hello hälsa entiteter spegla hello Service Fabric-enheter. (Till exempel **hälsa programmet entiteten** matchar en programinstans distribuerat i hello kluster när **hälsa nod entiteten** matchar ett Service Fabric.) hello hälsa hierarkin samlar in hello interaktioner hello systementiteter och det är hello grunden för avancerade hälsoutvärderingen. Du kan lära dig om nyckelbegrepp i Service Fabric [teknisk översikt för Service Fabric](service-fabric-technical-overview.md). Mer information om programmet finns [Service Fabric programmodell](service-fabric-application-model.md).

Tillåt hello kluster och program toobe effektivt rapporterade felsökas och övervakas hello hälsa entiteter och hierarkin. Hej hälsomodell ger en rättvisande *detaljerade* representation av hello hälsotillstånd hello många flytta delar i hello kluster.

![Enheter som hälsa.][1]
hello hälsa entiteter, ordnad i en hierarki baserat på överordnade och underordnade relationer.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

hello hälsa entiteter är:

* **Klustret**. Representerar hello hälsotillståndet för ett Service Fabric-kluster. Klustret hälsorapporter beskrivs villkor som påverkar hello hela klustret. Dessa villkor påverkar flera entiteter i hello kluster eller själva hello-klustret. Baserat på villkor hello kan inte hello rapport begränsa hello problemet ned tooone eller fler underordnade som ohälsosamt. Exempel: hello hjärna hello kluster dela på grund av problem med toonetwork partitionering eller kommunikation.
* **Noden**. Representerar hello hälsotillståndet för en Service Fabric-nod. Noden hälsorapporter beskrivs villkor som påverkar hello nod funktioner. De vanligtvis påverkar alla distribuerade hello entiteter som körs på den. Exempel är noden slut på diskutrymme (eller andra datoromfattande egenskaper, till exempel minne, anslutningar) och när en nod är nere. hello identifieras nod av hello-nodnamnet (sträng).
* **Programmet**. Representerar hello hälsotillståndet för en programinstans som körs i hello kluster. Programhälsan rapporter beskriver villkoren som påverkar hello övergripande hälsa för hello program. De kan inte begränsas av tooindividual underordnade (tjänster eller distribuerade program). Exempel inkluderar hello slutpunkt till slutpunkt interaktion mellan olika tjänster i hello program. hello identifieras programmet genom hello programnamn (URI).
* **Tjänsten**. Representerar hello hälsotillståndet för en tjänst som körs i hello kluster. Tjänstens hälsa rapporter beskriver villkoren som påverkar hello övergripande hälsa för hello-tjänsten. hello rapport kan inte begränsa hello problemet tooan ohälsosamt partition eller replik. Exempel är en tjänstkonfiguration (till exempel port eller externa filresurs) som orsakar problem för alla partitioner. hello identifieras-tjänsten av hello tjänstnamn (URI).
* **Partitionen**. Representerar hello hälsotillståndet för en partition med tjänsten. Partitionen hälsorapporter beskrivs villkor som påverkar hello hela replikuppsättningen. Exempel: när hello antalet repliker är under målantalet och när en partition är förlorar kvorum. hello identifieras partition av hello partitions-ID (GUID).
* **Replik**. Representerar hello hälsotillståndet för en tillståndskänslig service replik eller en tillståndslös tjänstinstans. hello replik är hello minsta enheten watchdogs och dess komponenter kan rapportera om för ett program. Exempel är en primär replik som det går inte att replikera operations toosecondaries och långsam replikering för tillståndskänsliga tjänster. Dessutom kan en tillståndslös instans rapportera när den har snart slut på resurser eller har problem med nätverksanslutningen. Hej replikeringsentitet identifieras av hello partitions-ID (GUID) och hello målrepliker eller instanser-ID (länge).
* **DeployedApplication**. Representerar hello hälsotillståndet för en *program som körs på en nod*. Distribuerade program hälsorapporter beskrivs villkor specifika toohello program på hello-nod som kan begränsas av tooservice paket som har distribuerats på hello samma nod. Exempel: fel när programpaket inte kan hämtas på noden och problem med att ställa in programmet säkerhetsobjekt på hello-nod. hello distribuerade program identifieras av namn (URI) och nodnamnet (sträng).
* **DeployedServicePackage**. Representerar hello hälsotillståndet för ett tjänstepaket som körs på en nod i klustret hello. Beskriver villkor specifika tooa service-paket som inte påverkar hello andra servicepaket på hello samma nod för hello samma program. Exempel är en kodpaketet i hello service-paket som inte kan startas och ett konfigurationspaket som inte kan läsas. hello distribuerat tjänstpaket identifieras av programnamn (URI), nodnamnet (sträng), manifestet tjänstnamn (sträng) och activation service paket-ID (sträng).

hello Granulariteten för hello hälsomodell gör det enkelt toodetect och åtgärda problem. Till exempel om en tjänst inte svarar, är det möjligt tooreport som hello programinstansen är felfri. Rapportering på att nivå inte är idealiskt, men eftersom hello problemet inte kanske påverkar alla hello tjänster i programmet. hello rapporten ska vara tillämpade toohello ohälsosamt service eller tooa specifika underordnad partition om mer information pekar toothat partition. hello data automatiskt hämtar via hello hierarki och en ohälsosamt partition görs synliga på tjänster och program. Den här aggregeringen hjälper toopinpoint och Lös hello grundorsaken till problemet hello snabbare.

hello hälsa hierarki består av överordnade och underordnade relationer. Ett kluster består av noder och program. Program tjänster och distribuerade program. Distribuerade program har distribuerat tjänstpaket. Tjänster som har partitioner och varje partition har en eller flera repliker. Det finns en särskild relation mellan noder och distribuerade entiteter. Ett feltillstånd nod påverkar som rapporteras av dess utfärdare systemkomponent hello Failover Manager service, hello distribuerade program, servicepaket och repliker som har distribuerats på den.

hello hälsa hierarkin representerar hello senaste status för hello baserat på hello senaste hälsorapporter som är nästan realtidsdata.
Interna och externa watchdogs kan rapportera om hello samma entiteter baserat på programspecifika logik eller anpassade övervakade villkor. Användarrapporter samexistera med hello systemrapporter.

Planera tooinvest i hur tooreport och svara toohealth under hello design av stora molntjänster. Den här direkta investement gör hello service enklare toodebug, övervaka och hantera.

## <a name="health-states"></a>Hälsotillstånd
Service Fabric använder tre hälsotillstånd tillstånd toodescribe om en entitet är felfri eller inte: OK, varning och fel. Alla rapporter skickas toohello hälsoarkivet måste ange ett av dessa lägen. utvärderingsresultat av hello hälsa är ett av dessa lägen.

Hej möjliga [hälsotillstånd](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) är:

* **OK**. hello entiteten är felfri. Det finns inga kända problem rapporteras på den eller dess underordnade objekt (i förekommande fall).
* **Varning**. hello entiteten har några problem, men det kan fortfarande fungera korrekt. Till exempel fördröjning, men de gör inte problemen funktionella ännu. I vissa fall åtgärda hello varningsvillkor själva utan externa åtgärder. I dessa fall hälsorapporter öka medvetenhet och ger inblick i vad som händer. I annat fall kan hello varningsvillkor försämras i ett allvarligt problem utan åtgärder från användaren.
* **Fel**. hello entiteten är felfri. Måste åtgärdas toofix hello tillstånd hello entiteten eftersom den inte fungera korrekt.
* **Okänd**. hello entiteten finns inte i hello health store. Det här resultatet kan hämtas från hello distribuerade frågor som sammanfoga resultat från flera komponenter. Till exempel hello get nod listfrågan går för**FailoverManager**, **ClusterManager**, och **HealthManager**; hämta programmet listfrågan går för **ClusterManager** och **HealthManager**. De här frågorna sammanfoga resultat från flera komponenter. Om en annan komponent returnerar en entitet som inte finns i health store hello resultatet är okänd hälsotillstånd. En entitet är inte i arkivet eftersom hälsorapporter som ännu inte har bearbetats eller hello enheten har rensats efter borttagningen.

## <a name="health-policies"></a>Hälsoprinciper
Hej hälsoarkivet gäller hälsa principer toodetermine om en entitet är felfri baserat på dess rapporter och dess underordnade.

> [!NOTE]
> Hälsoprinciper kan anges i hello klustermanifestet (för kluster- och nodnamn hälsoutvärderingen) eller hello programmanifestet (för utvärdering av programmet och alla dess underordnade). Hälsotillstånd utvärdering begäranden kan även skicka in anpassade utvärdering hälsoprinciper som endast används för att utvärdering.
> 
> 

Som standard gäller strikta regler (allt måste vara felfria) för hello överordnad-underordnad hierarkisk relation i Service Fabric. Om ett enda av hello underordnade har en ohälsosam händelse anses hello överordnade feltillstånd.

### <a name="cluster-health-policy"></a>Klustret hälsoprincip
Hej [kluster hälsoprincip](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) är används tooevaluate hello klustret hälsa tillstånd och noden hälsotillstånd. hello princip kan definieras i hello klustermanifestet. Om det inte finns, används standardprincipen för hello (noll tillåten fel).
hello klustret hälsoprincip innehåller:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Anger om tootreat varning hälsorapporter som fel under hälsoutvärderingen. Standard: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Anger hello maximalt tillåten procentandel av program som kan vara felaktiga innan hello klustret anses vara fel.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Anger hello maximalt tillåten procentandel noder som kan vara felaktiga innan hello klustret anses vara fel. I stora kluster är vissa noder alltid på eller ut för reparationer, så den här procentandelen ska vara konfigurerade tootolerate som.
* [ApplicationTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). hello typen hälsa princip programavbildningen kan användas under klustret utvärdering toodescribe specialprogram hälsotyper. Som standard är alla program sätta i en pool och med MaxPercentUnhealthyApplications. Om vissa programtyper ska behandlas annorlunda, kan de tas bort från hello globala poolen. I stället utvärderas de mot hello procenttal som är associerade med deras programmets typnamn i hello mappning. Till exempel i ett kluster finns tusentals program av olika typer och några programinstanser för kontroll av en särskild programtyp. hello styra program ska aldrig vara fel. Du kan ange globala MaxPercentUnhealthyApplications too20% tootolerate vissa fel, men för hello programtyp ”ControlApplicationType” ange hello MaxPercentUnhealthyApplications too0. Det här sättet om några av hello många program är felfria, men under hello globala ohälsosamt procent hello klustret skulle vara utvärderas tooWarning. Ett varningshälsotillstånd påverkar inte klustrets uppgraderingen eller andra övervakning utlöses av hälsotillståndet för fel. Men även en kontrollprogrammet i fel skulle göra kluster feltillstånd, vilket utlöser återställningsfasen eller pausar hello klusteruppgradering, beroende på hello uppgradera konfiguration.
  För hello programmet typer som definieras i hello mappning, tas alla instanser av programmet ut ur hello global pool av program. De utvärderas baserat på hello Totalt antal program av hello programtyp, hello specifika MaxPercentUnhealthyApplications från hello mappa. Alla hello resten av hello program finns kvar i hello globala poolen och utvärderas med MaxPercentUnhealthyApplications.

hello följande exempel är ett utdrag ur ett klustermanifest. toodefine poster i hello typen programavbildningen, prefixet hello parametern med ”ApplicationTypeMaxPercentUnhealthyApplications-”, följt av hello programmets typnamn.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Programmets hälsoprincip
Hej [programmets hälsoprincip](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) beskriver hur hello utvärdering av händelser och underordnade tillstånd aggregeringen är klar för program och deras underordnade. Det kan definieras i hello programmanifestet **ApplicationManifest.xml**, i hello programpaketet. Om inga principer anges, förutsätter Service Fabric att hello entitet feltillstånd om den har en hälsorapport eller en underordnad hello varnings- eller hälsotillstånd.
hello konfigurerbara principerna är:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Anger om tootreat varning hälsorapporter som fel under hälsoutvärderingen. Standard: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Anger hello maximala tolereras procentandelen av distribuerade program som kan vara felaktiga innan programmet hello anses vara fel. Den här procent beräknas genom att dividera hello antalet felaktiga distribuerade program över hello antalet noder som hello program för närvarande har distribuerats på i hello kluster. hello beräkning Avrundar uppåt tootolerate ett fel på litet antal noder. Standard procentandel: noll.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Anger hello standard service typen hälsoprinciper, som ersätter hello hälsa standardprincipen för alla tjänsttyper av i hello program.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Innehåller en karta över service hälsoprinciper per typ av tjänst. Dessa principer ersätter hello service typen hälsa standardprinciperna för varje typ av angivna tjänsten. Du kan till exempel konfigurera hello hälsoprinciper för utvärderingen annorlunda om ett program har en typ av tillståndslösa gateway och en tillståndskänslig engine service type. Du kan få mer detaljerad kontroll över hello hälsotillståndet för hello-tjänsten när du anger princip per typ av tjänst.

### <a name="service-type-health-policy"></a>Hälsoprincip för Service-typen
Hej [service type hälsoprincip](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) anger hur tooevaluate och aggregering hello tjänster och hello underordnade tjänster. hello principen innehåller:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Anger hello maximala tolereras procentandelen ohälsosamt partitioner innan en tjänst anses vara felaktig. Standard procentandel: noll.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Anger hello maximala tolereras procentandelen ohälsosamt repliker innan en partition anses vara felaktig. Standard procentandel: noll.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Anger hello maximala tolereras procentandelen ohälsosamt services innan programmet hello anses vara felaktig. Standard procentandel: noll.

hello följande exempel är ett utdrag ur ett programmanifest:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Utvärdering av hälsotillstånd
Användare och tjänster som automatiskt kan utvärdera hälsan för en enhet när som helst. tooevaluate en entitets hälsotillstånd, hello health store mängder hälsotillstånd rapporter om hello entiteten och utvärderar alla dess underordnade (i förekommande fall). hello hälsa aggregering algoritmen använder hälsoprinciper som anger hur tooevaluate hälsorapporter och hur tooaggregate underordnade hälsotillstånd (i förekommande fall).

### <a name="health-report-aggregation"></a>Hälsotillstånd rapporten aggregering
En entitet kan ha flera hälsorapporter som skickas av olika rapportörer (systemkomponenter eller watchdogs) på olika egenskaper. hello aggregering använder hello associerade hälsoprinciper, särskilt hello ConsiderWarningAsError medlem av programmet eller kluster hälsoprincip. ConsiderWarningAsError anger hur tooevaluate varningar.

hello aggregerade hälsotillstånd utlöses av hello *sämsta* hälsorapporter hello entiteten. Om det finns minst ett fel hälsorapport, hello aggregerade hälsotillståndet är ett fel.

![Hälsotillstånd rapporten aggregering med felrapport.][2]

En hälsa entitet som har en eller flera fel hälsorapporter utvärderas som fel. hello sak samma gäller för ett utgånget hälsorapport, oavsett dess hälsotillstånd.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Om det finns inga felrapporter och en eller flera varningar, är hello aggregerade hälsotillstånd varning eller fel, beroende på hello ConsiderWarningAsError princip flaggan.

![Hälsotillstånd rapporten aggregering med varning rapporten och ConsiderWarningAsError false.][3]

Hälsotillstånd rapporten aggregeringen med varning rapporten och ConsiderWarningAsError ange toofalse (standard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Underordnade hälsa aggregering
hello visar aggregerade hälsotillståndet för en entitet hello underordnade hälsotillstånd (i förekommande fall). hello-algoritmen för sammanställningen underordnade hälsotillstånd använder hello hälsoprinciper tillämpliga baserat på hello entitetstypen.

![Underordnade entiteter hälsa aggregering.][4]

Underordnade aggregering baserat på hälsoprinciper.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

När hello health store har utvärderas alla hello underordnade aggregerar deras hälsotillstånd baserat på hello konfigurerade maximala procentandelen ohälsosamt underordnade. Procenttalet hämtas från hello princip baserat på hello entiteten och underordnade.

* Om alla underordnade har OK tillstånd, är hälsotillstånd för hello underordnade aggregeras OK.
* Om underordnade har både OK och varning tillstånd, hälsotillstånd för hello underordnade aggregeras är en varning.
* Om det finns underordnade med fel tillstånd som inte tar hänsyn till hello tillåtna procentandelen ohälsosamt underordnade, är hello aggregerade hälsotillstånd ett fel.
* Om hello underordnade med fel tillstånd avseende hello maximalt tillåten procentandel av feltillstånd underordnade hello varning aggregerade hälsotillstånd.

## <a name="health-reporting"></a>Rapportering av hälsotillstånd
Systemkomponenter, System Fabric program och intern/extern watchdogs kan rapportera mot Service Fabric-enheter. hello-rapportörer Se *lokala* bestämning av hello hälsotillstånd hello övervakas entiteter, baserat på hello villkor som de övervakar. De behöver inte toolook vid varje globalt tillstånd eller samla in data. hello önskat beteende är toohave enkel rapportörer och inte komplexa organismer som behöver toolook på många saker tooinfer toosend vilken information.

toosend hälsotillstånd toohello hälsa datalager, en rapport måste tooidentify hello påverkas entiteten och skapa en hälsorapport. toosend hello rapport, Använd hello [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API, rapporten hälsa API: er som är exponerad på hello `Partition` eller `CodePackageActivationContext` objekt, PowerShell-cmdletar och REST.

### <a name="health-reports"></a>Hälsorapporter
Hej [hälsorapporter](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) för varje hello entiteter i hello kluster innehåller hello följande information:

* **SourceId**. En sträng som unikt identifierar hello rapport hello hälsa händelse.
* **Identifiering**. Identifierar hello entiteten där hello rapporten används. Den skiljer sig beroende på hello [entitetstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Kluster. Ingen.
  * Noden. Nodnamnet (sträng).
  * Programmet. Programnamn (URI). Representerar hello namnet på hello programinstansen distribueras i hello kluster.
  * Tjänsten. Tjänstens namn (URI). Representerar hello namnet på hello tjänstinstans distribueras i hello kluster.
  * Partition. Partitions-ID (GUID). Representerar hello partition Unik identifierare.
  * Repliken. hello tillståndskänslig service replik-ID eller hello tillståndslös tjänstinstans-ID (INT64).
  * DeployedApplication. Programnamnet (URI) och nodnamnet (sträng).
  * DeployedServicePackage. Programnamn (URI), nodnamnet (sträng) och service manifest namn (sträng).
* **Egenskapen**. En *sträng* (inte en fast uppräkning) som gör att hello rapport toocategorize hello hälsa händelse för en specifik egenskap för hello entitet. Till exempel rapport A kan rapportera hello hälsotillstånd hello Node01 ”” lagringsegenskap och rapport B kan rapportera hello hälsotillstånd hello Node01 ”anslutning”-egenskap. Rapporterna behandlas som separata hälsa händelser för hello Node01 entiteten i hello health store.
* **Beskrivning av**. En sträng som gör att en rapport tooprovide detaljerad information om hello hälsohändelse. **SourceId**, **egenskapen**, och **HealthState** fullständigt beskriva hello rapporten. hello beskrivning lägger till läsbart information om hello rapport. hello text gör det enklare för administratörer och användare toounderstand hello hälsorapport.
* **HealthState**. En [uppräkningen](service-fabric-health-introduction.md#health-states) som beskriver hello hälsotillståndet för hello rapporten. hello godkända värden är OK, varning och fel.
* **TimeToLive**. Timespan som anger hur länge hello hälsorapport är giltig. Tillsammans med **RemoveWhenExpired**, gör det hello health store vet hur tooevaluate upphört att gälla händelser. Som standard hello-värdet är oändlig och hello rapporten är giltig alltid.
* **RemoveWhenExpired**. Ett booleskt värde. Om ange tootrue, hello utgångna hälsa rapporten tas bort automatiskt från hello health store och hello rapporten påverkar inte entiteten hälsoutvärderingen. Används när hello rapporten är giltig för en viss tidsperiod endast tid och hello-rapport behöver inte tooexplicitly rensa den. Det har också används toodelete rapporter från hello health store (till exempel en watchdog ändras och slutar att skicka rapporter med föregående käll- och egenskapen). Den kan skicka en rapport med en kort TimeToLive tillsammans med RemoveWhenExpired tooclear eventuella tidigare tillstånd från hello health store. Om hello värdet toofalse, behandlas hello utgångna rapporten som ett fel på hello hälsoutvärderingen. hello FALSKT signalerar toohello health store hello källan bör rapportera regelbundet på den här egenskapen. Om det inte måste sedan det vara något fel med hello watchdog. hello watchdog hälsa fångas av överväger hello händelsen som ett fel.
* **SequenceNumber**. Ett positivt heltal som behöver toobe allt större representerar hello ordning hello rapporter. Den används av hello store toodetect inaktuella hälsorapporter som tas emot sent på grund av fördröjningar i nätverket eller andra problem. En rapport avvisas om hello sekvensnummer är mindre än eller lika med toohello nyligen använts nummer för hello samma entitet, källa och egenskapen. Om den inte är angiven ska hello sekvensnummer genereras automatiskt. Det är nödvändigt tooput i hello sekvensnummer endast när rapportering för övergångslägen. I den här situationen måste hello källa tooremember vilka rapporter som det skickas och hålla hello information för återställning vid redundans.

Dessa fyra delar av information – SourceId, identifiering, egenskap och HealthState--krävs för varje hälsorapport. hello SourceId sträng är inte tillåten toostart med hello prefixet ”**System.**”, som är reserverad för systemrapporter. För hello samma entitet det finns endast en rapport för hello samma käll- och egenskapen. Flera rapporter för hello samma källa och egenskapen åsidosätter varandra på hello hälsa på klientsidan (om de grupperas) eller på hello health store på klientsidan. hello ersättning baseras på sekvensnummer; nyare rapporter (med högre sekvensnummer) ersätter äldre rapporter.

### <a name="health-events"></a>Hälsotillstånd händelser
Internt, för att hålla hello hälsoarkivet [hälsa händelser](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), som innehåller alla hello information från hello rapporter och ytterligare metadata. hello metadata innehåller hello hello rapporten gavs toohello hälsa klienten och hello tid den ändrats på hello på serversidan. hello hälsa händelser som returneras av [hälsoförfrågningar](service-fabric-view-entities-aggregated-health.md#health-queries).

hello tillagda metadata innehåller:

* **SourceUtcTimestamp**. hello tid hello rapporten har fått toohello hälsa klienten (Coordinated Universal Time).
* **LastModifiedUtcTimestamp**. hello tid hello rapporten senast ändrades på serversidan för hello (Coordinated Universal Time).
* **IsExpired**. En flagga tooindicate om hello rapporten har upphört att gälla när hello frågan kördes av hello health store. En händelse kan ha gått endast om RemoveWhenExpired är false. Annars hello händelsen returneras inte av frågan och tas bort från hello store.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. hello senast för fel-OK/varning övergångar. De här fälten kan hello historik över hello hälsa tillståndsövergångar för hello-händelse.

hello tillstånd övergången fält kan användas för smartare aviseringar eller ”historiska” hälsoinformation för händelsen. De aktiverar som scenarier:

* Meddela när en egenskap har varit varning/fel för mer än X minuter. Kontrollera hello villkoret för en viss tidsperiod undviker aviseringar på tillfälliga förhållanden. Till exempel en avisering om hello hälsotillstånd har varning under mer än fem minuter kan översättas till (HealthState == varning och nu - LastWarningTransitionTime > 5 minuter).
* Meddela endast på de villkor som har ändrats i hello senast X minuter. Om en rapport var redan vid fel innan hello angiven tid, ignoreras det eftersom den redan har signalerat tidigare.
* Om en egenskap växla mellan varnings- och bestämma hur länge den har varit ohälsosam (det vill säga inte OK). Till exempel en avisering om hello-egenskapen inte har varit vid god hälsa under mer än fem minuter kan översättas till (HealthState! = Ok och nu - LastOkTransitionTime > 5 minuter).

## <a name="example-report-and-evaluate-application-health"></a>Exempel: Rapport och utvärdera programs hälsa
hello följande exempel skickas en hälsorapport via PowerShell på programmet hello **fabric: / WordCount** från hello källa **MyWatchdog**. hello hälsorapport innehåller information om hello hälsa egenskapen ”tillgänglighet” i ett felaktigt hälsotillstånd med oändlig TimeToLive. Sedan hello programhälsan frågor som returnerar samman hälsa tillstånd fel och hello rapporterade hälsa händelser i hello lista över händelser som hälsa.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Hälsotillstånd Modellanvändning
Hej hälsomodell kan molntjänster och hello underliggande Service Fabric-plattformen tooscale, eftersom övervakning och hälsa är fördelade på hello olika Övervakare i hello kluster.
Andra system har en centraliserad tjänst på hello klusternivå som Parsar alla hello *potentiellt* användbar information som sänds av tjänster. Den här metoden hämmar sina skalbarhet. Den också tillåter inte dem toocollect specifik information toohelp identifiera potentiella problem och som Stäng toohello grundorsaken som möjligt.

Hej hälsomodell används kraftigt för övervakning och diagnos av, för att bedöma hälsa för klustret och program och för övervakade uppgraderingar. Andra tjänster använder hälsa data tooperform automatiska reparationer, skapa klustret hälsohistorik och utfärda aviseringar på vissa villkor.

## <a name="next-steps"></a>Nästa steg
[Visa Service Fabric-hälsorapporter](service-fabric-view-entities-aggregated-health.md)

[Använda systemet hälsorapporter för felsökning](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hur tooreport och kontrollera-tjänsten för hälsotillstånd](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Lägg till anpassade hälsorapporter för Service Fabric](service-fabric-report-health.md)

[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

