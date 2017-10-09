---
title: aaaBalance Azure Service Fabric-kluster | Microsoft Docs
description: "En introduktion toobalancing klustret med hello resurshanteraren för Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a>NLB service fabric-kluster
hello resurshanteraren för Service Fabric-klustret har stöd för dynamisk ändringar, korsreagerande tooadditions eller borttagningar av noder eller tjänster. Den korrigerar också automatiskt begränsningen överträdelser och balanserar proaktivt hello klustret. Hur ofta dessa åtgärder vidtas för men vad utlöser dem?

Det finns tre olika kategorier av arbete som hello klustret Resource Manager utför. De är:

1. Placering – det här steget behandlar placera alla tillståndskänslig repliker eller tillståndslösa instanser som saknas. Placering innehåller både nya tjänster och hanterar tillståndskänslig repliker eller tillståndslösa instanser som har misslyckats. Ta bort och släppa repliker eller instanser hanteras här.
2. Begränsningen kontrollerar – det här steget kontrollerar och korrigerar brott mot hello olika placeringen (regler) inom hello system. Exempel på regler är till exempel att säkerställa att noderna inte är överbelastade och att en tjänst placeringen är uppfyllda.
3. Belastningsutjämning – det här steget kontrollerar toosee om ombalansering behövs baserat på konfigurerad hello önskad nivå av balansera för olika mått. I så fall försöker toofind en ordning i hello kluster som är mer belastningsutjämnade.

## <a name="configuring-cluster-resource-manager-timers"></a>Konfigurera Timers för hanteraren för filserverresurser
hello första uppsättning kontroller runt belastningsutjämning är en uppsättning timers. Dessa timers styr hur ofta hello klustret Resource Manager undersöker hello klustret och tar korrigerande åtgärder.

Var och en av dessa olika typer av korrigeringar hello klustret Resource Manager kan göra styrs av en annan timer som styr frekvensen. När varje timer utlöses schemaläggs hello. Hej Resource Manager som standard:

* genomsöker dess tillstånd och tillämpar uppdateringar (till exempel inspelningen som en nod nedåt) var 1/10 sekunder
* Anger hello placering Kontrollera flagga 
* Anger att hello begränsningen kontrollera varje sekund
* Anger hello NLB flaggan var femte sekund.

Exempel på hello configuration styr dessa timers är nedan:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Idag utför hello klustret Resource Manager endast en av dessa åtgärder samtidigt, sekventiellt. Det är därför vi finns toothese timers som ”minsta intervall” och hello åtgärder som hämta vidtas när hello timers går som ”inställningen flaggor”. Hello klustret Resource Manager sköter väntande begäranden till exempel toocreate services innan NLB hello klustret. Som du ser hello standard tidsintervall som angetts söker hello klustret Resource Manager efter något den behov toodo ofta. Detta innebär normalt att hello uppsättning ändringar som gjorts under varje steg är liten. Att göra små ändringar ofta tillåter hello klustret Resource Manager toobe responsiv när saker i hello kluster. hello standard timers ange vissa batchbearbetning eftersom många hello samma typer av händelser tenderar toooccur samtidigt. 

Till exempel när noder misslyckas de kan göra så att hela feldomäner i taget. Alla dessa fel avbildas under hello nästa tillstånd uppdatera efter hello *PLBRefreshGap*. hello korrigeringar fastställs under hello efter placering, kontroll-begränsning, och NLB körs. Som standard hello klustret Resource Manager inte skanna via timmar för ändringar i hello kluster och försök tooaddress alla ändringar på en gång. Det skulle kunna leda toobursts av omsättning.

hello klustret Resource Manager måste också toodetermine vissa ytterligare information om hello kluster imbalanced. Som vi har två delar i konfigurationen: *BalancingThresholds* och *ActivityThresholds*.

## <a name="balancing-thresholds"></a>NLB tröskelvärden
Ett tröskelvärde för NLB är hello huvudsakliga kontroll för att utlösa ombalansering. hello NLB tröskelvärdet för ett mått är ett _förhållandet_. Om hello belastning för ett mått på hello mest inläst nod dividerat med hello mängden belastningen på hello minst inlästa nod överskrider den måtten *BalancingThreshold*, och sedan hello klustret är imbalanced. Därför är utlösta hello nästa gång hello klustret Resource Manager kontrollerar. Hej *MinLoadBalancingInterval* timer definierar hur ofta hello klustret Resource Manager ska kontrollera om ombalansering krävs. Kontrollera innebär inte att något händer. 

Tröskelvärden för belastningsutjämning definieras på grundval av per mått som en del av hello klustret definition. Mer information om mått kolla [i den här artikeln](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>
![Belastningsutjämning tröskelvärde-exempel][Image1]
</center>

I det här exemplet förbrukar en enhet av vissa mått för varje tjänst. I hello översta exempelvis hello belastningen på en nod är fem och hello minsta är två. Anta att hello NLB tröskelvärdet för det här måttet är tre. Eftersom hello förhållandet i hello kluster är 5/2 = 2,5 och som är mindre än hello angivet tröskelvärde i tre, hello-kluster är belastningsutjämnat. Ingen belastningsutjämning utlöses när hello klustret Resource Manager kontrollerar.

I hello ned exempel är hello belastningen på en nod 10, medan hello minst två, vilket resulterar i ett förhållande på fem. Fem är större än hello avsedda belastningsutjämning tröskelvärdet på tre för den måtten. Detta innebär att en rebalancing kör schemalagda nästa gång hello NLB timer utlöses. I den här situationen är vissa belastningen vanligtvis distribuerade tooNode3. Eftersom hello resurshanteraren för Service Fabric-klustret inte använder en girig metod, kan vissa belastningen också vara distribuerade tooNode2. 

<center>
![Belastningsutjämning tröskelvärdet exempel åtgärder][Image2]
</center>

> [!NOTE]
> ”NLB” hanterar två olika strategier för att hantera belastningen i klustret. hello standard strategi som hello hanteraren för filserverresurser använder är toodistribute belastningen över hello noder i klustret hello. hello andra strategin är [defragmentering](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Defragmentering utförs under hello kör samma belastningsutjämning. hello belastningsutjämning och defragmentering strategier kan användas för olika mått i hello samma kluster. En tjänst kan ha belastningsutjämning och defragmentering mått. För defragmentering mätvärden hello förhållandet mellan hello läses in i hello klustret utlösare ombalansering när det är _nedan_ hello NLB tröskelvärdet. 
>

Hämta nedan hello NLB tröskelvärdet är inte en explicit målet. Tröskelvärden för belastningsutjämning är bara en *utlösaren*. När NLB körs avgör hello klustret Resource Manager vilka förbättringar som det kan göra eventuella. Bara för att en belastningsutjämning sökning har inletts innebär inte något flyttas. Ibland är hello klustret imbalanced men för begränsad toocorrect. Alternativt hello förbättringar kräver förflyttningar för [kostsamma](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Tröskelvärden för aktiviteten
Ibland men noder är relativt imbalanced, hello *totala* belastningen i hello kluster är låg. hello bristande belastning kan vara tillfälliga dip eller eftersom hello kluster är ny och bara hämtning startade. I båda fallen kan du inte vill toospend tid NLB hello klustret eftersom det finns lite toobe erfarenheter. Om hello klustret genomgått belastningsutjämning, skulle du ägnar åt nätverket och beräkna resurser toomove runt saker och ting utan att göra några stora *absolut* skillnaden. tooavoid onödiga flyttar det finns en annan kontroll kallas tröskelvärden för aktiviteten. Aktiviteten tröskelvärden kan toospecify vissa absolut undre gränsvärde för aktiviteten. Om ingen nod är över tröskeln, utlösas belastningsutjämning inte även om hello NLB tröskelvärdet har uppnåtts.

Anta att vi behåller våra tröskelvärdet för belastningsutjämning av tre för det här måttet. Anta också att vi har ett tröskelvärde för aktiviteten på 1536. I hello första fallet medan hello klustret är imbalanced per hello NLB tröskelvärdet det är ingen nod uppfyller aktivitet gränsen, så ingenting händer. I hello nedre exempelvis är Nod1 över hello aktivitet tröskelvärdet. Eftersom både hello tröskelvärdet för belastningsutjämning och hello aktivitet tröskelvärdet för hello mått överskrids, har belastningsutjämning schemalagts. Exempelvis ska vi titta på hello följande diagram: 

<center>
![Aktiviteten tröskelvärdet exempel][Image3]
</center>

Precis som tröskelvärden för NLB är aktiviteten tröskelvärden definierade per-mått via hello klustret definition:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Tröskelvärden för belastningsutjämning och aktivitet är bundet tooa specifikt mått - belastningsutjämning utlöses endast om båda hello tröskelvärdet för belastningsutjämning och aktivitet tröskelvärde har överskridits för hello samma mått.

## <a name="balancing-services-together"></a>NLB tjänster tillsammans
Om hello klustret är imbalanced eller inte är ett hela beslut. Hello sätt vi gå om hur du löser det flyttas enskild tjänst repliker och instanser runt. Det låter väl logiskt, rätt? Om minnet är Staplad på en nod, kan flera repliker eller instanser bidra tooit. Åtgärdar hello obalans kan det krävas flytta hello tillståndskänslig repliker eller tillståndslösa instanser som använder hello imbalanced mått.

Ibland om en tjänst som inte är själva imbalanced flyttas (Kom ihåg hello diskussion av lokala och globala viktas tidigare). Varför skulle en tjänst flyttas när allt som tjänstens mått har balanserade? Låt oss se ett exempel:

- Anta att det finns fyra tjänster, Service1, plats2, tjänst3 och Service4. 
- Service1 rapporterar mått Metric1 och Metric2. 
- Plats2 rapporterar mått Metric2 och Metric3. 
- Tjänst3 rapporterar mått Metric3 och Metric4.
- Service4 rapporterar mått Metric99. 

Du kan visserligen se där vi här: det finns en kedja! Vi har inte fyra oberoende tjänster har vi tre tjänster som är relaterade och ett som är inaktiverat på sin egen.

<center>
![NLB tjänster tillsammans][Image4]
</center>

På grund av kedjan är det möjligt att obalans i mått 1-4 kan orsaka repliker eller instanser som tillhör tooservices 1-3 toomove runt. Vi vet att en obalans i mått 1, 2 eller 3 kan orsaka förflyttningar i Service4. Det skulle inte finns några sedan flytta hello repliker eller instanser som tillhör tooService4 runt kan gör absolut ingenting tooimpact hello balans mellan mått 1-3.

hello klustret Resource Manager siffror automatiskt reda på vilka tjänster som är relaterade. Lägga till, ta bort eller ändra hello mått för tjänster kan påverka deras relationer. Till exempel kan mellan två körningar av belastningsutjämning plats2 ha varit uppdaterade tooremove Metric2. Detta innebär att hello kedjan mellan Service1 och plats2. Nu i stället för två grupper av relaterade tjänster finns tre:

<center>
![NLB tjänster tillsammans][Image5]
</center>

## <a name="next-steps"></a>Nästa steg
* Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret. Mer om mått toolearn och hur tooconfigure dem, kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)
* Förflyttningskostnad är ett sätt att signalering toohello klustret Resource Manager att vissa tjänster är dyrare toomove än andra. Mer information om förflyttningskostnad finns för[i den här artikeln](service-fabric-cluster-resource-manager-movement-cost.md)
* hello klustret Resource Manager har flera begränsningar som du kan konfigurera tooslow ned omsättningen i hello kluster. De inte normalt krävs, men om du behöver dem kan du läsa om dem [här](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
