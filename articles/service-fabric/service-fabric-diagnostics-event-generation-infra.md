---
title: "aaaAzure Service Fabric-plattformen nivå övervakning | Microsoft Docs"
description: "Lär dig mer om plattformen händelser och loggar används toomonitor och diagnostisera Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: f8fb1c8b546e05c517ae12c91906acc04cd6eaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="platform-level-event-and-log-generation"></a>Generering av plattform nivån händelse och loggfiler

## <a name="monitoring-hello-cluster"></a>Övervaka hello kluster

Det är viktigt toomonitor på hello plattform nivå toodetermine om maskin- och klustret fungerar som förväntat. Även om Service Fabric kan behålla program som körs under ett maskinvarufel, men du behöver fortfarande toodiagnose om fel uppstår i ett program eller hello underliggande infrastruktur. Du också övervaka ditt kluster toobetter planera för kapacitet att hjälpa till att beslut om att lägga till eller ta bort maskinvara.

Service Fabric innehåller fem olika loggen kanaler out-of-the-box som genererar hello följande händelser:

* Operativa kanalen: övergripande åtgärder som utförs av Service Fabric och hello kluster, inklusive händelser för en nod, kommer ett nytt program som distribueras, eller en SA uppgradera återställning osv.
* Operativa kanalen - detaljerad: hälsorapporter och beslut för belastningsutjämning
* Data & Messaging kanal: kritiska loggar och händelser som genererats i våra messaging (för närvarande endast hello ReverseProxy) och datasökväg (reliable services modeller)
* Data & Messaging kanaler – detaljerad: utförlig kanal som innehåller alla icke-kritiska hello-loggar från data och meddelanden i hello kluster (den här kanalen har en mycket stor volym med händelser)   
* [Händelselogg och tillförlitlig](service-fabric-reliable-services-diagnostics.md): programmering modellen specifika händelser
* [Tillförlitliga aktörer händelser](service-fabric-reliable-actors-diagnostics.md): programmering modellen specifika händelser och prestandaräknare
* Stöd för loggarna: systemloggar som genereras av Service Fabric endast toobe för oss när ger support

Dessa olika kanaler täcker de flesta av hello plattform nivån loggning som rekommenderas. tooimprove plattformsnivå loggning investera i bättre förstå hello hälsomodell och lägga till anpassade hälsorapporter och lägga till anpassade **prestandaräknare** toobuild en realtid förståelse av hello påverkan på dina tjänster och program på hello klustret.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric-hälsa och Läs in rapportering

Service Fabric har sin egen hälsomodell som beskrivs i detalj i följande artiklar:
- [Introduktion tooService Fabric-hälsoövervakning](service-fabric-health-introduction.md)
- [Rapportera och kontrollera hälsan hos tjänster](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Lägg till anpassade hälsorapporter för Service Fabric](service-fabric-report-health.md)
- [Visa Service Fabric-hälsorapporter](service-fabric-view-entities-aggregated-health.md)

Övervakning av hälsotillstånd är kritiska toomultiple aspekter för driften av en tjänst. Övervakning av hälsotillstånd är särskilt viktigt när Service Fabric utför en uppgradering av namngivna programmet. När varje uppgraderingsdomän för tjänsten hello uppgraderas och är tillgängliga tooyour kunder, måste hello uppgraderingsdomänen klara hälsokontroller innan hello distribution flyttar toohello nästa uppgraderingsdomän. Om bra hälsostatus inte uppnås återställs hello distribution, så att programmet hello är i ett fungerande tillstånd. Även om vissa kunder kan påverka innan hello services återställs kan merparten av kunderna drabbas av problemet. En lösning inträffar också relativt snabbt och utan att behöva toowait för åtgärden från en mänsklig operatör. Hej mer hälsokontroller som ingår i din kod hello mer robust tjänsten är toodeployment problem.

En annan aspekt av tjänstens hälsa rapporterar mått från hello-tjänsten. Mått är viktiga i Service Fabric eftersom de används toobalance Resursanvändning. Mått kan också vara en indikator för systemhälsa. Till exempel du kanske har ett program som har många tjänster och varje instans i rapporterna en begäranden per andra (RPS) mått. Om en tjänst använder mer resurser än en annan tjänst, Service Fabric flyttar tjänstinstanser runt hello klustret tootry toomaintain även resursutnyttjande. En mer detaljerad förklaring av hur resursutnyttjande fungerar, se [hantera resursförbrukning och hämta i Service Fabric med](service-fabric-cluster-resource-manager-metrics.md).

Mått också hjälper ger dig insyn i hur tjänsten utförs. Du kan använda mått toocheck som hello-tjänst körs inom förväntade parametrar med tiden. Till exempel om trender visar att på 9: 00 på måndag morgon hello genomsnittlig RPS är 1 000 och du kan ställa in en hälsorapport som varnar dig om hello RPS är under 500 eller större än 1 500. Allt kanske perfekt bra, men kan det vara värt en titt toobe till att dina kunder har en bra upplevelse. Tjänsten kan definiera ett mått som rapporteras för health check ändamål, men som inte påverkar hello resurs fördelningen av hello klustret. toodo detta, ange hello tjänstmåttets vikt toozero. Vi rekommenderar att du börjar alla med en vikt på noll och öka hello vikt inte förrän du är säker på att du förstår hur viktning hello mått påverkar resurser för klustret.

> [!TIP]
> Använd inte för många viktat mått. Det kan vara svårt toounderstand varför tjänstinstanser flyttas för belastningsutjämning. Några mått kan vara en stor!

All information som kan indikera hello hälsotillstånd och prestanda för ditt program är en kandidat för mått och hälsorapporter. En CPU-prestandaräknare ser du hur noden används, men den tala inte du en viss tjänst är felfri, eftersom flera tjänsterna kan köras på en enda nod. Men, mätvärden som RPS bearbetade, objekt och alla svarstid för begäran kan ange hello hälsotillståndet för en specifik tjänst.

tooreport hälsotillstånd, Använd koden liknande toothis:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

tooreport ett mått med kod liknande toothis:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>Service Fabric support-loggar

Om du behöver toocontact Microsoft-supporten för hjälp med Azure Service Fabric-kluster krävs nästan alltid support-loggar. Om klustret finns i Azure, support loggar konfigureras automatiskt och samlas in som en del av att skapa ett kluster. hello loggfilerna lagras i ett dedikerat storage-konto i din klusterresursgrupp. hello storage-konto har inte ett fast namn men hello-konto visas i blob-behållare och tabeller med namn som börjar med *fabric*. Information om hur du konfigurerar loggsamlingar för ett fristående kluster finns i [skapa och hantera ett fristående Azure Service Fabric-kluster](service-fabric-cluster-creation-for-windows-server.md) och [konfigurationsinställningarna för ett fristående Windows-kluster](service-fabric-cluster-manifest.md). För fristående Service Fabric instanser skickas hello loggar tooa lokala filresurs. Du är **krävs** toohave dessa loggar för support, men de är inte avsedda toobe som kan användas av personer utanför hello Microsoft customer support-teamet.

## <a name="enabling-diagnostics-for-a-cluster"></a>Aktivera diagnostik för ett kluster

I ordning tootake nytta av dessa loggar rekommenderas att ”-diagnostik är aktiverad när klustret skapas. Genom att aktivera diagnostik, när hello klustret distribueras Windows Azure-diagnostik är kan tooacknowledge hello drift, tillförlitlig tjänster och Reliable actors kanaler och lagra hello data som beskrivs mer i [aggregera händelser med Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).

Som visas ovan, finns det också ett valfritt fält tooadd en Application Insights (AI) instrumentation nyckel. Om du väljer toouse AI för alla händelseanalys (Läs mer om detta i [Händelseanalys med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)), inkluderar hello AppInsights resurs instrumentationKey (GUID) här.


Om du ska toodeploy behållare tooyour klustret, aktivera BOMULLSTUSS toopick in docker stats genom att lägga till den här tooyour ”WadCfg > DiagnosticMonitorConfiguration”:

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Mäta prestanda

Måttet prestanda på klustret hjälper dig att förstå hur den ska kunna toohandle belastning och enheten beslut runt skalning av klustret (Läs mer om att skala ett kluster [på Azure](service-fabric-cluster-scale-up-down.md), eller [lokalt](service-fabric-cluster-windows-server-add-remove-nodes.md)). Prestandadata är också användbart när jämfört med tooactions du eller ditt program och tjänster har tagit, när du analyserar loggar i hello framtida. 

En lista över prestandaräknare toocollect när du använder Service Fabric finns [prestandaräknare i Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Här följer två vanliga sätt som du kan konfigurera att samla in prestandadata för klustret:

* Med hjälp av en agent: det här är hello önskad sätt för att samla in prestanda från en dator, eftersom agenter har vanligtvis en lista över möjliga prestandamått som kan samlas in och det är en relativt enkel process toochoose hello mått du vill toocollect eller ändra dem. . Läs mer om [hur tooconfigure hello OMS för Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) och [konfigurerar hello OMS Windows Agent](../log-analytics/log-analytics-windows-agents.md) artiklar toolearn mer om hello OMS-agent som är en sådan övervakningsagenten kan toopick fungerar prestandadata för virtuella datorer i klustret och distribuerade behållare.

* Konfigurera diagnostik toowrite prestanda prestandaräknare tooa tabellen: för kluster i Azure, innebär detta att ändra hello Azure Diagnostics configuration toopick in hello lämpliga prestandaräknare från hello virtuella datorer i klustret, och aktivera den toopick in docker statistik om du planerar att distribuera en behållare. Läs om hur du konfigurerar [prestandaräknare i BOMULLSTUSS](service-fabric-diagnostics-event-aggregation-wad.md) i Service Fabric tooset in prestandaräknarsamlingen.

## <a name="next-steps"></a>Nästa steg

Måste toobe samman innan de kan skickas tooany analysplattform dina loggar och händelser. Läs mer om [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) och [BOMULLSTUSS](service-fabric-diagnostics-event-aggregation-wad.md) toobetter känner till vissa av hello rekommenderat alternativ.
