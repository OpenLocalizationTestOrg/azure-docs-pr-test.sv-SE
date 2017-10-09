---
title: aaaService Fabric klustret Resource Manager - programgrupper | Microsoft Docs
description: "Översikt över hello programgruppen funktioner i hello resurshanteraren för Service Fabric-kluster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a>Introduktion tooApplication grupper
Resurshanteraren för Service Fabric-klustret hanterar vanligtvis klusterresurser genom att sprida belastningen hello (representeras [mått](service-fabric-cluster-resource-manager-metrics.md)) jämnt över hello klustret. Service Fabric hanterar hello kapacitet hello noder i hello och hello klustret som helhet via [kapacitet](service-fabric-cluster-resource-manager-cluster-description.md). Mått och kapacitet fungerar bra för många arbetsbelastningar, men mönster som använder olika Service Fabric-programinstanser ibland hämtas ytterligare krav. Till exempel kanske du vill:

- Reservera vissa kapacitet på hello noder i klustret hello för hello tjänster inom vissa namngivna programinstansen
- Begränsa hello Totalt antal noder som kör hello services i en namngiven programinstansen på (i stället för att sprida ut dem över hello hela klustret)
- Definiera kapacitet på hello namngivna programinstansen själva toolimit hello antal tjänster eller total förbrukning av nätverksresurser hello tjänster i den

toomeet kraven hello resurshanteraren för Service Fabric-klustret har stöd för en funktion som kallas programgrupper.

## <a name="limiting-hello-maximum-number-of-nodes"></a>Begränsa hello maximalt antal noder
hello enklaste användningsfall för programmet kapacitet är när en instans av programmet måste toobe begränsad tooa vissa maximalt antal noder. Detta konsoliderar alla tjänster inom den programinstansen till ett visst antal datorer. Konsolidering är användbart när du försöker toopredict eller cap fysiska Resursanvändning hello tjänster inom den namngivna programinstansen. 

hello visar följande bild en programinstans med och utan ett maximalt antal noder som definierats:

<center>
![Definiera maximalt antal noder programinstansen][Image1]
</center>

Hello program har inte ett maximalt antal noder som definierats i hello vänstra exempelvis och den har tre tjänster. hello klustret Resource Manager har sprids ut alla repliker sex tillgängliga noder tooachieve hello bästa balansen i hello kluster (hello standardbeteendet). I högra hello-exempel finns vi hello samma program begränsad toothree noder.

hello-parameter som anger det här beteendet kallas MaximumNodes. Denna parameter kan anges när programmet skulle skapas eller uppdateras för en programinstans som redan körs.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

Inom hello uppsättning noder garanterar inte hello klustret Resource Manager vilka tjänstobjekt placeras tillsammans eller vilka noder hämta används.

## <a name="application-metrics-load-and-capacity"></a>Programmet mått, belastning och kapacitet
Programgrupper kan du också toodefine mätvärden som är associerade med en viss namngiven programinstansen och den programinstansen kapacitet för dessa mått. Programmet mått kan du tootrack och reservera gränsen hello resursförbrukning hello tjänster i den programinstansen.

Det finns två värden som kan anges för varje mått:

- **Total kapacitet för programmet** – den här inställningen motsvarar hello total kapacitet för hello program för ett viss mått. hello klustret Resource Manager tillåts hello skapandet av nya tjänster i den här programinstansen som skulle orsaka totala tooexceed det här värdet. Anta att till exempel hello programinstansen hade en kapacitet på 10 och redan hade belastningen på fem. hello skapandet av en tjänst med belastning totala standard 10 skulle inte tillåts.
- **Maximal kapacitet för noden** – den här inställningen anger hello maximala totala belastningen för hello program på en enda nod. Om belastningen över denna kapacitet, flyttar hello klustret Resource Manager repliker tooother noder så att hello belastningen minskar.


PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C#:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Reserverar kapacitet
Ett annat vanligt användningsområde för programgrupper är tooensure att resurser inom hello klustret är reserverade för ett visst program-instans. hello utrymme reserveras alltid när hello programinstansen skapas.

Reservera diskutrymme i hello kluster för programmet hello sker omedelbart även om:
- hello programinstansen har skapats men har inte några tjänster inom det ännu
- hello antal tjänster inom hello programinstansen ändras varje gång 
- hello tjänster finns men förbrukar inte hello resurser 

Reservera resurser för en instans av programmet kräver att ange två ytterligare parametrar: *MinimumNodes* och *NodeReservationCapacity*

- **MinimumNodes** -definierar hello minsta antalet noder som programmet hello instans ska köras på.  
- **NodeReservationCapacity** -den här inställningen är per mått för hello program. hello-värdet är hello som mått som reserverats för programmet hello på varje nod där som hello tjänster i programmet körs.

Kombinera **MinimumNodes** och **NodeReservationCapacity** garanterar en minsta belastningen reservation för hello program inom hello kluster. Om det finns mindre återstående kapacitet i hello kluster än hello totala reservationen krävs, kan inte skapas av programmet hello. 

Nu ska vi titta på ett exempel på kapacitet reservation:

<center>
![Programinstanser definiera reserverad kapacitet][Image2]
</center>

I hello vänstra exempelvis har program inte några program kapacitet har definierats. hello klustret Resource Manager balanserar allt enligt toonormal regler.

I hello exempel på hello rätt anta att Application1 har skapats med hello följande inställningar:

- MinimumNodes ange tootwo
- Ett program mått som definieras med
  - NodeReservationCapacity 20

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

Service Fabric reserverar kapacitet på två noder för Application1 och tillåter inte tjänster från Application2 tooconsume den kapaciteten även om det finns ingen belastning som förbrukas av hello tjänster i Application1 hello samtidigt. Denna kapacitet för reserverade programmet betraktas som förbrukats och räknas av mot hello återstående kapacitet på noden och inom hello kluster.  hello reservation dras av från hello återstående klustret kapacitet omedelbart, men hello reserverade förbrukning dras av från hello kapacitet för en viss nod endast när minst en tjänstobjekt placeras i den. Denna senare reservation ger flexibilitet och bättre resursutnyttjande eftersom resurser som endast är reserverad på noder vid behov.

## <a name="obtaining-hello-application-load-information"></a>Hämta hello belastningen programinformation
Du kan hämta hello information om hello sammanställd belastning som rapporterats av repliker av dess tjänster för varje program som har ett program kapacitet har definierats för ett eller flera mått.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

Hej ApplicationLoad frågan returnerar hello grundläggande information om kapacitet för program som har angetts för programmet hello. Informationen omfattar information om lägsta noder och maximalt antal noder av hello och hello nummer som hello programmet för närvarande använder. Det innehåller även information om varje belastningen mått, inklusive:

* Måttnamnet: Namnet på hello mått.
* Reservation Capacity: Kapacitet för kluster som är reserverade i hello kluster för det här programmet.
* Programinläsning: Totalt antal belastningen på det här programmet underordnade repliker.
* Kapacitet för programmet: Högsta tillåtna värdet för programinläsning.

## <a name="removing-application-capacity"></a>Kapacitet för att ta bort program
Efter hello programmet kapacitet parametrar har angetts för ett program, kan de tas bort med hjälp av API: er för Update-programmet eller PowerShell-cmdlets. Exempel:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Det här kommandot tar bort alla parametrar för program-kapacitet från hello programinstansen. Detta inkluderar MinimumNodes, MaximumNodes och hello programmet statistik, och eventuella. hello effekten av hello kommandot sker omedelbart. När det här kommandot har slutförts använder hello klustret Resource Manager hello standardbeteendet för att hantera program. Programmet kapacitet parametrar kan anges igen `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Begränsningar för programmet kapacitet
Det finns flera begränsningar för programmet kapacitet parametrar som måste följas. Om det finns valideringsfel görs inga ändringar.

- Alla måste heltal vara icke-negativt tal.
- MinimumNodes måste aldrig vara större än MaximumNodes.
- Om kapaciteten för ett mått för belastningen definieras måste de följa reglerna:
  - Noden Reservation Capacity får inte vara större än den maximala nod kapaciteten. Du kan till exempel begränsa hello kapacitet för hello mått ”CPU” på hello nod tootwo enheter och försök tooreserve tre enheter på varje nod.
  - Om MaximumNodes anges får sedan hello produkten av MaximumNodes och maximumkapacitet för noden inte vara större än den Totalkapaciteten för programmet. Anta till exempel hello maxkapacitet nod för belastningen mått ”CPU” anges tooeight. Anta också att du ställer in hello maximalt antal noder too10. I det här fallet måste totalkapacitet för programmet vara större än 80 för mätvärdet belastningen.

både under skapa program och uppdateringar tillämpas hello begränsningar.

## <a name="how-not-toouse-application-capacity"></a>Hur inte toouse programmet kapacitet
- Försök inte toouse hello programgruppen funktioner tooconstrain hello programmet tooa _specifika_ del noder. Med andra ord kan du ange att programmet hello körs på högst fem noder, men inte vilka specifika fem noder i klustret hello. Begränsa ett program toospecific kan noder ske med hjälp av placeringsbegränsningar för tjänster.
- Försök inte toouse hello programmet kapacitet tooensure att två tjänster från samma program är placerad hello hello samma noder. I stället använda [tillhörighet](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) eller [placeringen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Nästa steg
- Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)
- toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)
- Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)
- Mer information om hur mått fungerar normalt läsa på [Service Fabric belastningen mått](service-fabric-cluster-resource-manager-metrics.md)
- hello klustret Resource Manager har många alternativ för att beskriva hello klustret. toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
