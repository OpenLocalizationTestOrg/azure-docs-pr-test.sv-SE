---
title: "aaaManage Azure mikrotjänster belastningen med hjälp av mätvärden | Microsoft Docs"
description: "Läs mer om hur tooconfigure och Använd mått i Service Fabric toomanage tjänsten resursförbrukning."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 592dc749ce30683a1e439a702b7d0dc0a638276f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Hantera resursförbrukning och belastningen i Service Fabric med
*Mått* hello resurser som din dig för tjänster och som tillhandahålls av hello noder i klustret hello. Ett mått är något som du vill toomanage i ordning tooimprove eller övervaka hello prestanda-tjänster. Exempelvis kan du titta på minne förbrukning tooknow om tjänsten är överbelastad. Ett annat syfte är toofigure ut om hello-tjänsten kan flytta någon annanstans där minne är mindre begränsad i ordning tooget bättre prestanda.

Till exempel minne, Disk och CPU-användning är exempel på mått. De här måtten är fysiska statistik, och resurser som motsvarar toophysical resurser på hello-nod som behöver toobe hanteras. Mått kan också vara (och ofta är) logiska mått. Logisk är till exempel ”MyWorkQueueDepth” eller ”MessagesToProcess” eller ”TotalRecords”. Logiska mått är programdefinierade och motsvarar indirekt toosome fysiska resursförbrukning. Logiska mått är vanliga eftersom det kan vara svårt toomeasure och rapportera förbrukning av fysiska resurser för per tjänst. hello komplexiteten med att mäta och rapportera egna fysiska mått är också varför Service Fabric innehåller vissa siffror som standard.

## <a name="default-metrics"></a>Standard-mått
Anta att du vill att tooget startade skrivning och distribuera din tjänst. Nu vet du inte vilka fysiska eller logiska resurser som den förbrukar. Det är bra! hello resurshanteraren för Service Fabric-kluster använder vissa standard mått när inga andra mått har angetts. De är:

  - PrimaryCount - antal primära repliker på hello nod 
  - ReplicaCount - antal totala tillståndskänslig repliker på hello nod
  - Antal - antalet för alla service-objekt (tillståndslösa och tillståndskänsliga) på hello nod

| Mått | Läs in tillståndslös instans | Tillståndskänsliga sekundära belastning | Tillståndskänsliga primära belastning |
| --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |
| ReplicaCount |0 |1 |1 |
| Antal |1 |1 |1 |

Ange en hyfsad fördelning av arbete i hello kluster för grundläggande arbetsbelastningar hello standard mått. I följande exempel hello, låt oss se vad som händer när vi skapar två tjänster och förlitar sig på hello standard mätvärden för belastningsutjämning. första hello-tjänsten är en tillståndskänslig service med tre partitioner och ett mål replikuppsättningen storleken på tre. andra hello-tjänsten är en tillståndslös tjänst med en partition och instansantal av tre.

Här är vad du får:

<center>
![Klustret Layout med standard-mått][Image1]
</center>

Vissa saker toonote:
  - Primära repliker för hello tillståndskänslig service är fördelade på flera noder
  - Repliker för hello samma partition som finns på olika noder
  - hello Totalt antal primärfärgerna och sekundärservrar distribueras i hello kluster
  - hello Totalt antal tjänstobjekt fördelas jämnt på varje nod

Bra!

hello standard mått fungera utmärkt som en start. Dock hanterar hello standard mått endast du gjort hittills är korrekt. Till exempel: Vad är hello sannolikheten att hello partitionering schemat du plockats resultat i perfekt även användning av alla partitioner? Vad är hello chans att hello belastning för en viss tjänst är konstant över tid, eller bara hello samma över flera partitioner just nu?

Du kan köra med bara hello standard. Men om du gör det vanligtvis innebär att din klusteranvändning lägre och mer ojämn än du vill. Detta beror på att hello standard mått inte anpassningsbar och förutsätter att allt är likvärdiga. En primär är upptagen och ett som inte är båda bidra till exempel ”1” toohello PrimaryCount mått. I hello värsta fall kan med hjälp av endast hello standard mätvärden också leda till overscheduled noder, vilket resulterar i prestandaproblem. Om du är intresserad komma hello de flesta utanför klustret och undvika prestandaproblem måste toouse anpassade mått och dynamisk rapportering.

## <a name="custom-metrics"></a>Anpassade mått
Mått är konfigurerade på grundval av per med namnet-service-instans när du skapar hello-tjänsten.

Alla mått har vissa egenskaper som beskriver den: ett namn, en vikt och en standard-belastning.

* Måttnamnet: hello namnet på hello mått. hello måttnamnet är en unik identifierare för hello mått i hello kluster ur hello Resource Manager.
* Vikt: Tjänstmåttets vikt definierar hur viktigt det här måttet är relativ toohello andra mått för den här tjänsten.
* Standard-belastning: hello standard belastningen representeras på olika sätt beroende på om hello-tjänsten är tillståndslösa och tillståndskänsliga.
  * För tillståndslösa tjänster har varje mått en egenskap med namnet DefaultLoad
  * För tillståndskänsliga tjänster ange du:
    * PrimaryDefaultLoad: hello standardmängden mätvärdet tjänsten förbrukar när det är en primär
    * SecondaryDefaultLoad: hello standardmängden mätvärdet tjänsten förbrukar när det är en sekundär

> [!NOTE]
> Om du definiera anpassade mått och du vill too_also_ Använd hello standard mått, too_explicitly_ måste lägga till hello standard mått tillbaka och definiera vikterna och värden för dessa. Det beror på att du måste definiera hello förhållandet mellan hello standard mått och din anpassade mått. Till exempel kanske du bryr dig om ConnectionCount eller WorkQueueDepth mer än primära distributionsplatser. Som standard hello vikten av hello PrimaryCount mått är hög, så du tooreduce den tooMedium när du lägger till dina andra mått tooensure de högre prioritet.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Definiera mått för din tjänst - exempel
Anta att du vill hello följande konfiguration:

  - Tjänsten rapporterar ett mått med namnet ”ConnectionCount”
  - Du bör också toouse hello standard mått 
  - Du har gjort vissa mått och vet att en primär replik av den tjänsten tar normalt 20 enheter ”ConnectionCount”
  - Sekundärservrar använda 5 enheter i ”ConnectionCount”
  - Du vet att ”ConnectionCount” hello viktigaste mått som hanterar hello prestandan för viss tjänsten
  - Du ändå vill primära repliker belastningsutjämnade. NLB primära repliker är vanligtvis bra oavsett vad. Detta förhindrar hello förlust av vissa nod eller fault domän påverkar en majoritet av primära repliker tillsammans med den. 
  - Annars är hello standard bra

Här är hello kod som du skulle kunna skriva toocreate en tjänst med mått konfigurationen:

Kod:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> Hej exemplen ovan och hello resten av det här dokumentet beskriver hantera mått på grundval av per-med namnet-tjänst. Det är också möjligt toodefine mätvärden för dina tjänster på hello service _typen_ nivå. Detta åstadkoms genom att ange dem i din service manifest. Definiera typ nivån mått rekommenderas inte av flera skäl. hello beror första på att mått namn är ofta miljöspecifikt. Om det inte finns ett fast kontrakt på plats, kan du vara säker som hello-måttet ”kärnor” i en miljö är inte ”MiliCores” eller ”kärnor” i andra. Om din mått har definierats i ditt manifest måste toocreate nya manifesten per miljö. Detta leder vanligtvis tooa spridning av olika manifest med bara smärre skillnader, vilket kan leda till problem med toomanagement.  
>
> Mått belastningar tilldelas ofta på grundval av per med namnet-service-instans. Till exempel anta att du skapar en instans av hello-tjänst för CustomerA som planer toouse den inte. Anta också att du skapar en annan för CustomerB som har en större arbetsbelastning. I så fall skulle vill du förmodligen tootweak hello standard laddar för dessa tjänster. Om du har mått och belastningar som anges via manifesten och du vill toosupport det här scenariot, kräver olika program och typer av tjänster för varje kund. hello värden som definierats vid tidpunkten för skapandet av tjänsten åsidosätter de som definieras i hello manifest så att du kan använda de tooset hello specifika standardvärdena. Dock gör gör hello-värden som deklarerats i hello manifesten toonot matchar de hello-tjänsten körs med. Detta kan leda till tooconfusion. 
>

Som en påminnelse: Om du bara vill toouse hello standard mått du inte behöver tootouch hello mått samling alls eller göra något speciellt när du skapar din tjänst. hello standard mått hämta används automatiskt när inga andra definieras. 

Nu går vi igenom de här inställningarna i detalj och diskutera hello beteende som den inverkar.

## <a name="load"></a>Läsa in
Hej hela platsen för att definiera mått är toorepresent vissa belastningen. *Läs in* är hur mycket av ett visst mått som används av vissa tjänstinstansen eller repliken på en viss nod. Läs in kan konfigureras på nästan alla punkt. Exempel:

  - Läs in kan definieras när en tjänst skapas. Detta kallas _standard belastningen_.
  - hello mått information, inklusive standard läses in för en tjänst kan uppdateras när hello-tjänsten har skapats. Detta kallas _uppdatera en tjänst_. 
  - hello belastningar för en given partition kan vara Återställ toohello standardvärden för tjänsten. Detta kallas _återställa partitionsbelastning_.
  - Läs in rapporteras på en per service objektet bas dynamiskt vid körning. Detta kallas _reporting belastningen_. 
  
Alla dessa strategier kan användas i hello samma tjänst under sin livslängd. 

## <a name="default-load"></a>Standard-inläsning
*Standard belastningen* är hur mycket av hello mått förbrukar varje serviceobjektet (tillståndslös instans eller tillståndskänslig replik) för den här tjänsten. hello klustret Resource Manager använder det här numret för hello belastningen på hello webbtjänstobjektet tills det mottar annan information, till exempel en dynamisk rapport. Hello standard belastningen är en statisk definition för enklare tjänster. hello standard belastningen uppdateras aldrig och används under hello livstid hello-tjänsten. Standard laddar fungerar bra för enkel kapacitetsplanering scenarier där vissa mängder resurser är dedikerade toodifferent arbetsbelastningar och ändras inte.

> [!NOTE]
> Mer information om kapacitetshantering av och definiera kapacitet för hello noder i klustret finns [i den här artikeln](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

hello klustret Resource Manager kan tillståndskänsliga tjänster toospecify en annan standard belastning för sina primärfärgerna och sekundärservrar. Tillståndslösa tjänster kan bara ange ett värde som gäller tooall instanser. Hello standard belastningen för primära och sekundära repliker är vanligtvis olika för tillståndskänsliga tjänster, eftersom repliker har olika typer av resurser i varje roll. Till exempel primärfärgerna vanligtvis fungerar både läsningar och skrivningar och hantera de flesta av hello beräkningar arbetsbördan, medan sekundärservrar inte. Vanligtvis är hello standard belastningen för en primär replik högre än hello standard belastning för sekundära repliker. hello reellt tal bör beror på dina egna mått.

## <a name="dynamic-load"></a>Dynamisk
Anta att du har kört din tjänst för en stund. Med viss övervakning upplever du som:

1. Några partitioner eller instanser av en viss tjänst förbruka mer resurser än andra
2. Vissa tjänster ha belastningen varierar över tiden.

Det finns många saker som kan leda till att dessa typer av belastningen variationer. Till exempel är olika tjänster eller partitioner associerade med olika kunder med olika krav. Läs in kan också ändra eftersom hello mängd arbete hello-tjänsten varierar under hello loppet av hello dag. Oavsett hello orsak finns vanligtvis inte tal som du kan använda för standardvärde. Detta gäller särskilt om du vill tooget hello de flesta användning utanför hello klustret. Ett värde som du väljer för standard är felaktigt vissa hello tid. Felaktig standard läser in resultatet i hello klustret Resource Manager antingen över eller under fördela resurser. Därför kan har du noder som över eller under används även om hello klustret Resource Manager tror hello klustret balanseras. Standard-belastning är fortfarande bra eftersom de ger viss information för inledande placering, men de har inte en fullständig artikel för verkliga arbetsbelastningar. tooaccurately avbilda ändra resursbehov, hello klustret Resource Manager kan varje tjänst objektet tooupdate sin egen belastning under körningen. Detta kallas dynamisk reporting.

Dynamisk rapporter kan repliker eller instanser tooadjust sina allokering/rapporterade belastningen på mått under sin livslängd. En replik för tjänsten eller instans som var kall och inte gör allt arbete rapporteras vanligtvis som använde små mängder av ett visst mått. En upptagen replik eller skulle rapporterar att de använder mer.

Rapportering belastningen per målrepliker eller instanser kan hello klustret Resource Manager tooreorganize hello enskild tjänstobjekt i hello kluster. Ordna hello services säkerställer att de får hello resurser som de kräver. Upptagen services hämta effektivt för ”frigöra” resurser från andra repliker eller instanser som är för närvarande kall eller göra mindre arbete.

Inom Reliable Services hello kod tooreport load dynamiskt ser ut så här:

Kod:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

En tjänst kan rapportera på någon av hello mått har definierats för den vid skapandet. Om en tjänst rapporter belastning för ett mått som inte är konfigurerad toouse, ignorerar Service Fabric rapporten. Om det finns andra mått som rapporteras vid hello samma tid som är giltiga rapporterna accepteras. Service-kod kan mäta och rapportera alla hello mått vet hur till, och operatörer kan ange hello mått configuration toouse utan toochange hello service-kod. 

### <a name="updating-a-services-metric-configuration"></a>Ett mått tjänstkonfigurationen uppdaterades
hello listan över mått som är associerade med hello-tjänsten och hello egenskaperna för dessa mått kan uppdateras dynamiskt när hello-tjänst är aktiv. Detta ger undersökningar och flexibilitet. Några exempel när det är praktiskt är:

  - inaktivera ett mått med en buggy rapport för en viss tjänst
  - Konfigurera om hello vikten av mått baserat på önskat beteende
  - Aktivera ett nytt mått när hello koden har redan distribuerats och godkänts via andra mekanismer
  - Ändra hello standard belastningen för en tjänst baserat på observerade beteende och förbrukning

hello huvudsakliga API: er för att ändra mått konfiguration är `FabricClient.ServiceManagementClient.UpdateServiceAsync` i C# och `Update-ServiceFabricService` i PowerShell. Den information som du anger med dessa API: er ersätter hello befintliga mått information för hello tjänsten omedelbart. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Blandning av belastningen standardvärden och dynamisk rapporter
Standard belastning och dynamisk last kan användas för hello samma tjänst. När en tjänst använder både standard belastning och dynamisk rapporter, standard belastning som fungerar som en uppskattning tills dynamiska rapporter visas. Standard belastningen är bra eftersom den ger hello klustret Resource Manager något toowork med. hello standard belastning kan hello klustret Resource Manager tooplace hello tjänstobjekt bra platser när de skapas. Om ingen standard belastningen information tillhandahålls är placering av tjänster effektiv slumpmässiga. När belastningen rapporter kommer senare hello inledande slumpmässiga placering är ofta fel och hello klustret Resource Manager har toomove tjänster.

Vi tar det tidigare exemplet och se vad som händer när vi lägger till vissa anpassade mått och dynamisk rapportering. I det här exemplet använder vi ”MemoryInMb” som ett exempel mått.

> [!NOTE]
> Minne är en av hello system mått som Service Fabric kan [resursen styr](service-fabric-resource-governance.md), och rapportering själv är vanligtvis svårt. Vi räknar inte faktiskt du tooreport på minnesförbrukning; Minne används här som ett stöd toolearning om hello funktioner i hello klustret Resource Manager.
>

Vi förutsätter att det ursprungligen skapades hello tillståndskänslig service med hello följande kommando:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Som en påminnelse är den här syntaxen (”MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad”).

Nu ska vi se vad en layout för möjliga klustret kan se ut så:

<center>
![Kluster balanserad med både standard- och anpassade mått][Image2]
</center>

Vissa saker som är noteras:

* Sekundära repliker inom en partition kan ha sina egna belastning
* Övergripande leta hello mått belastningsutjämnade. För minne, hello förhållandet mellan hello högsta och lägsta belastningen är 1,75 (hello nod med hello de flesta belastningen är N3 är hello minst N2 och 28/16 = 1,75).

Det finns vissa saker som vi behöver tooexplain:

* Vad fastställa om ett förhållande på 1,75 var rimliga? Hur hello klustret Resource Manager vet om som är tillräckligt bra eller om det finns mer arbete toodo?
* När sker belastningsutjämning?
* Vad innebär det att minnet var viktas ”hög”?

## <a name="metric-weights"></a>Tjänstmåttets vikt
Spårning hello samma mått över olika tjänster är viktigt. Globala vyn är vad tillåter hello klustret Resource Manager tootrack förbrukning i hello kluster, balanserar förbrukning mellan noder och se till att noderna inte gå igenom kapacitet. Men tjänster kan ha olika vyer som toohello vikten av hello samma mått. Även i ett kluster med många mått och många tjänster finns perfekt belastningsutjämnade lösningar inte för alla. Hur ska hello klustret Resource Manager hanterar sådana situationer?

Tjänstmåttets vikt Tillåt hello klustret Resource Manager toodecide hur toobalance hello kluster, när det finns inget perfekta svar. Tjänstmåttets vikt kan också hello klustret Resource Manager toobalance specifika tjänster på olika sätt. Mått kan ha fyra olika vikt nivåer: noll, lågt, Medium och hög. Ett mått med en vikt på noll bidrar ingenting när du överväger saker är balanserade eller inte. Belastningen bidrar dock fortfarande toocapacity management. Mått med noll är användbar och används ofta som en del av tjänst och prestandaövervakning. [Den här artikeln](service-fabric-diagnostics-event-generation-infra.md) finns mer information om hello användning av mätvärden för övervakning och diagnostik-tjänster. 

hello verkliga effekten av olika mått vikter i hello klustret är den hello klustret Resource Manager genererar olika lösningar. Tjänstmåttets vikt berätta hello klustret Resource Manager för att vissa mått är viktigare än andra. När det finns ingen perfekt lösning hello föredrar klustret Resource Manager lösningar som balanserar hello högre viktat mått bättre. Om en tjänst tror ett viss mått är oviktigt kan hittas deras användning av den måtten imbalanced. Detta gör att en annan tjänst tooget en jämn fördelning av vissa mått som är viktiga tooit.

Nu ska vi titta på ett exempel på några belastningen rapporter och hur olika mått viktas resulterar i olika fördelningar i hello klustret. I det här exemplet finns att växla hello relativa vikten av hello mätvärden orsakar hello klustret Resource Manager toocreate olika åtgärder av tjänster.

<center>
![Tjänstmåttets vikt exempel och dess påverkan på NLB lösningar][Image3]
</center>

I det här exemplet finns fyra olika tjänster, alla reporting olika värden för två olika mått, MetricA och MetricB. I ett fall definiera alla hello tjänster MetricA är hello viktigaste (vikt = hög) och MetricB som oviktigt (vikt = låg). Därmed visas den hello klustret Resource Manager placerar hello services så att MetricA är bättre belastningsutjämnade än MetricB. ”Bättre belastningsutjämnade” innebär att MetricA har en lägre har en lägre standardavvikelse än MetricB. I andra fall hello omvända vi hello mått vikter. Därför växlingar hello klustret Resource Manager tjänster A och B toocome in med en allokering där MetricB är bättre belastningsutjämnade än MetricA.

> [!NOTE]
> Tjänstmåttets vikt bestämmer hur hello klustret Resource Manager ska utjämna, men inte när NLB ska hända. Mer information om belastningsutjämning kolla [den här artikeln](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Globala mått vikterna
Anta att ServiceA definierar MetricA som vikt hög och ServiceB anger hello vikt för MetricA tooLow eller noll. Vad är hello faktiska vikten hamnar används?

Det finns flera vikter som spåras för varje mått. hello första vikt är hello som definieras för hello mått när hello-tjänsten har skapats. hello är andra vikt en global vikt, som beräknas automatiskt. hello hanteraren för filserverresurser använder båda dessa vikter vid bedömningen lösningar. Det är viktigt att hänsyn tas till båda vikter. Detta gör hello klustret Resource Manager toobalance varje tjänst bl.a tooits äger prioriteter och kontrollera även att hello-kluster som helhet allokeras korrekt.

Vad som skulle hända om hello klustret Resource Manager inte bryr dig om både globala och lokala saldo? Det är enkelt tooconstruct lösningar som balanseras globalt, men vilket kan resultera i sämre resursbalansen för enskilda tjänster. I följande exempel hello, ska vi titta på en tjänst som konfigurerats med bara hello standard mått och se vad som händer när endast globala saldo betraktas som:

<center>
![hello effekten av en Global endast-lösning][Image4]
</center>

I hello översta exempel endast baseras på globalt saldo balanseras verkligen hello klustret som helhet. Alla noder har hello samma antal primärfärgerna och hello samma antal totala repliker. Men om du tittar på hello faktiska effekten av den här allokeringen är det inte så bra: hello förlust av alla noder som påverkar en viss arbetsbelastning oproportionerligt, eftersom det tar bort all dess primärfärgerna. Till exempel om hello första noden misslyckas hello tre primärfärgerna för hello tre olika partitioner hello cirkel tjänsten skulle alla gå förlorade. Hello triangel och Sexhörning tjänster har däremot deras partitioner förlorar en replik. Detta medför att några avbrott än med toorecover hello ned replik.

I hello ned exempel har hello klustret Resource Manager distribuerade hello repliker baserat på båda hello global och per service saldo. Vid beräkning av hello poängen för hello lösningen ger de flesta hello vikt toohello globala lösningen och en (konfigureras) del tooindividual services. Globala balansera för ett mått beräknas baserat på hello medelvärdet av hello mått vikter från varje tjänst. Varje tjänst balanseras bl.a tooits egna definierade mått vikter. Detta säkerställer att balanseras hello tjänster inom själva enligt tootheir egna behov. Därför distribueras hello samma första noden misslyckades hello över alla partitioner i alla tjänster. hello påverkan tooeach är hello samma.

## <a name="next-steps"></a>Nästa steg
- Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Definiera defragmentering mått är ett sätt tooconsolidate belastningen på noder i stället för att sprida. toolearn hur tooconfigure defragmenteringen finns för[i den här artikeln](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)
- Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)
- Förflyttningskostnad är ett sätt att signalering toohello klustret Resource Manager att vissa tjänster är dyrare toomove än andra. toolearn mer om förflyttningskostnad, referera för[i den här artikeln](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
