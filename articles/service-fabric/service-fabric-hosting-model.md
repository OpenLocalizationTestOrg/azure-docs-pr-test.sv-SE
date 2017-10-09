---
title: "aaaAzure Fabric värd tjänstmodell | Microsoft Docs"
description: "Beskriver relationen mellan repliker (eller instanser) av en distribuerad Servic Fabric-tjänst och värdprocessen för tjänsten."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: 30e0ba19cd672a722f76477a311ecef7c213b055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-hosting-model"></a>Service Fabric värdmodell
Den här artikeln innehåller en översikt över program som är värd för modeller som tillhandahålls av Service Fabric och beskriver hello skillnaderna mellan hello **delad Process** och **exklusiv processen** modeller. Beskriver hur ett distribuerat program visas på ett Service Fabric-noden och förhållandet mellan repliker (eller instanser) av hello-tjänsten och hello tjänstvärden processen.

Innan du fortsätter, kontrollera att du är bekant med [Service Fabric-programmodell] [ a1] och förstå olika begrepp och relationen mellan dem.. 

> [!NOTE]
> I den här artikeln för enkelhetens skull, såvida inte uttryckligen anges:
>
> - All användning av word *replik* refererar tooboth en replik av en tillståndskänslig tjänst eller en instans av en statless tjänst.
> - *CodePackage* är behandlade motsvarande för*ServiceHost* process som registrerar en *ServiceType* och värdar repliker tjänster av som *ServiceType*.
>

toounderstand Hej värdmodell, låt oss gå igenom ett exempel. Låt oss säga vi har en *ApplicationType* 'MyAppType' som har en *ServiceType* 'MyServiceType' som tillhandahålls av *ServicePackage* 'MyServicePackage' som har en *CodePackage* 'MyCodePackage' som registrerar *ServiceType* 'MyServiceType' när den körs.

Anta att vi har ett kluster med 3 noder och skapar vi ett *programmet* **fabric: / App1** av typen 'MyAppType'. I detta *programmet* **fabric: / App1** vi skapa en tjänst **fabric: / App1/ServiceA** av typen 'MyServiceType' som har 2 partitioner (säg **P1** & **P2**) och 3 repliker per partition. hello följande diagram visar hello visning av det här programmet eftersom det hamnar distribuerad på en nod.

<center>
![Noden vy av distribuerade program][node-view-one]
</center>

Service Fabric aktiverat 'MyServicePackage' som startade 'MyCodePackage' som är värd repliker från båda hello-partitioner, d.v.s. **P1** & **P2**. Observera att alla hello-noder i klustret hello kommer att ha samma vy eftersom vi valde antal repliker per partition lika toonumber av noderna i klustret hello. Nu ska vi skapa en annan tjänst **fabric: / App1/ServiceB** i programmet **fabric: / App1** som har 1 partition (säg **P3**) och 3 repliker per partition. Följande diagram visar hello ny vy på hello nod:

<center>
![Noden vy av distribuerade program][node-view-two]
</center>

Som vi ser Service Fabric placeras hello nya repliken för partition **P3** för tjänsten **fabric: / App1/ServiceB** i hello befintliga aktivering av 'MyServicePackage'. Nu kan skapa en ny *programmet* **fabric: / App2** av typen 'MyAppType' och inuti **fabric: / App2** skapa tjänst **fabric: / App2/ServiceA** som har 2 partitioner (säger **P4** & **P5**) och 3 repliker per partition. Följande diagram visar hello ny nod vy:

<center>
![Noden vy av distribuerade program][node-view-three]
</center>

Den här gången Service Fabric har aktiverat en ny kopia av 'MyServicePackage ”som startar en ny kopia av 'MyCodePackage' och repliker från både partitioner för tjänsten **fabric: / App2/ServiceA** (d.v.s. **P4** & **P5**) placeras i den här nya kopian 'MyCodePackage'.

## <a name="shared-process-model"></a>Delade processmodell
Vad vi såg ovan är hello standard värdmodell som tillhandahålls av Service Fabric och kallas tooas **delad Process** modell. I den här modellen för en given *programmet*, bara en kopia av en angivna *ServicePackage* aktiveras på en *nod* (som startar alla hello *CodePackages* finns i den) och alla hello repliker av alla tjänster för en viss *ServiceType* placeras i hello *CodePackage* som registrerar som *ServiceType*. Med andra ord alla hello repliker av alla tjänster för en viss *ServiceType* dela hello samma process.

## <a name="exclusive-process-model"></a>Exklusiv processmodell
hello andra värdmodell som tillhandahålls av Service Fabric är **exklusiv processen** modell. I den här modellen på en viss *nod*, placera varje replik Service Fabric aktiverar en ny kopia av *ServicePackage* (som startar alla hello *CodePackages* finns i den ) och repliken placeras i hello *CodePackage* den registrerade hello *ServiceType* av hello service toowhich repliken tillhör. Med andra ord bor varje replik i sin egen dedikerade process. 

Den här modellen stöds startversion 5.6 i Service Fabric. **Exklusiv processen** modell kan väljas när hello för att skapa hello-tjänst (med hjälp av [PowerShell][p1], [REST] [ r1]eller [FabricClient][c1]) genom att ange **ServicePackageActivationMode** som 'ExclusiveProcess'.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Om du har en standardtjänst i programmanifestet kan du välja **exklusiv processen** modellen genom att ange **ServicePackageActivationMode** attribut som visas nedan:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Fortsätter med vårt exempel ovan kan skapa en annan tjänst **fabric: / App1/ServiceC** i programmet **fabric: / App1** som har 2 partitioner (säg **P6**  &  **P7**) och 3 repliker per partition med **ServicePackageActivationMode** ange too'ExclusiveProcess'. Följande diagram visar ny vy på hello nod:

<center>
![Noden vy av distribuerade program][node-view-four]
</center>

Som du kan se Service Fabric aktiverat två nya kopior av 'MyServicePackage' (en för varje replik från partition **P6** & **P7**) och placeras varje replik i den dedikerade kopian av *CodePackage*. En annan sak toonote här är när **exklusiv processen** modellen används för en viss *programmet*, flera kopior av en angivna *ServicePackage* kan vara aktiva i en  *Noden*. I ovanstående exempel vi se att tre kopior av 'MyServicePackage' är aktiva för **fabric: / App1**. Var och en av dessa active kopior av 'MyServicePackage' har en **ServicePackageAtivationId** som är associerade med den som identifierar kopian i *programmet* **fabric: / App1**.

När bara **delad Process** mall används för ett *programmet*, till exempel **fabric: / App2** i ovanstående exempel det finns endast en aktiv kopia av *ServicePackage* på en *nod* och **ServicePackageAtivationId** för den här aktiveringen av *ServicePackage* ' tom sträng ”.

> [!NOTE]
>- **Delad Process** värdmodell motsvarar för**ServicePackageAtivationMode** lika **SharedProcess**. Detta är standardvärdet för hello värdmodell och **ServicePackageAtivationMode** behöver inte anges vid hello tiden för att skapa hello-tjänst.
>
>- **Exklusiv processen** värdmodell motsvarar för**ServicePackageAtivationMode** lika **ExclusiveProcess** och måste toobe explicit när hello skapar hello tjänsten. 
>
>- Värdmodell av en tjänst kan identifieras genom att fråga hello [tjänstbeskrivningen] [ p2] och titta på värdet för **ServicePackageAtivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Arbeta med distribuerat tjänstpaket
En aktiv kopia av en *ServicePackage* på en nod kallas [distribuerat tjänstpaket][p3]. Som tidigare nämnts ovan, när **exklusiv processen** modellen används för att skapa tjänster, för en given *programmet*, kan det finnas flera distribuerad tjänst paket för hello samma  *ServicePackage*. När du utför åtgärder specifika toodeployed tjänstpaket som [reporting hälsotillståndet för ett distribuerat tjänstpaket] [ p4] eller [att starta om kodpaketet för en distribuerad tjänstepaketet] [ p5] osv., **ServicePackageActivationId** behov toobe tillhandahålls tooidentify ett paket med specifika distribuerade tjänsten.

 **ServicePackageActivationId** för en distribuerad tjänst paketet kan hämtas genom att fråga hello lista över [distribuerat tjänstpaket] [ p3] på en nod. När du frågar [distribueras tjänsttyper][p6], [distribueras repliker] [ p7] och [distribuerade kod paket] [ p8] på en nod, innehåller hello frågeresultat även hello **ServicePackageActivationId** i det överordnade distribuerade abonnemang.

> [!NOTE]
>- Under **delad Process** värdmodell, på en angivna *nod*, för en given *programmet*, bara en kopia av en *ServicePackage* har aktiverats. Den har **ServicePackageActivationId** lika med för*tom sträng* och behöver inte anges när utför distribuerat tjänstpaket relaterade åtgärder. 
>
> - Under **exklusiv processen** värdmodell, på en angivna *nod*, för en given *programmet*, en eller flera kopior av en *ServicePackage* kan vara aktiv. Varje aktivering har en *icke-tom* **ServicePackageActivationId** och måste toobe anges när utför distribuerat tjänstpaket relaterade åtgärder. 
>
> - Om **ServicePackageActivationId** är ommited som standard too'empty strängen '. Om en distribuerad tjänst paketet som har aktiverats **delad Process** modellen finns och kommer att utföra åtgärden på den, annars hello misslyckas åtgärden.
>
> - Det rekommenderas inte tooquery en gång och cache **ServicePackageActivationId** eftersom det genereras dynamiskt och ändra av olika skäl. Innan du utför en åtgärd som måste **ServicePackageActivationId**, bör du först läsa hello lista över [distribuerat tjänstpaket] [ p3] på en nod och sedan använda  *ServicePackageActivationId** från resultatet tooperform hello ursprungliga frågeåtgärden.
>
>

## <a name="guest-executable-and-container-applications"></a>Gästen körbar fil och en behållare program
Service Fabric behandlar [gäst körbara] [ a2] och [behållare] [ a3] program som statless tjänster som är fristående dvs. det finns ingen Service Fabric-körningsmiljön i *ServiceHost* (en process eller behållaren). Eftersom dessa tjänster är fristående, antal repliker per *ServiceHost* gäller inte för dessa tjänster. hello vanligaste konfigurationen används med tjänsterna är enskild partition med [InstanceCount] [ c2] lika med för-1 (dvs. en kopia av hello Tjänstkod som körs på varje nod i klustret). 

Hej standard **ServicePackageActivationMode** tjänsterna är **SharedProcess** då Service Fabric endast aktiverar en kopia av *ServicePackage* på en *Nod* för en given *programmet* vilket innebär att endast en kopia av tjänstkoden körs en *nod*. Om du vill att flera kopior av dina service code toorun på en *nod* när du skapar flera tjänster (*Service1* för*ServiceN*) av *ServiceType* (anges i *ServiceManifest*) eller när tjänsten är multi-partitionerat, bör du ange **ServicePackageActivationMode** som **ExclusiveProcess**vid hello tiden för att skapa hello-tjänst.

## <a name="changing-hosting-model-of-an-existing-service"></a>Ändra värdmodell en befintlig tjänst
Ändra värdmodell en befintlig tjänst från **delad Process** för**exklusiv processen** och vice versa via uppgradera eller uppdatera mekanism (eller tjänst i specifikationen i program Manifestet) stöds inte för närvarande. Stöd för den här funktionen kommer i framtida versioner.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Att välja mellan delad process och exklusiv processmodell
Både dessa värd modeller har dess- och nackdelar och användaren måste tooevaluate vilket som bäst passar deras behov. **Delad Process** modellen ger bättre användning av OS-resurser eftersom färre processer har initierat, flera repliker i hello samma process kan dela portar, osv. Om en av replikerna hello träffar ett fel där toobring ned hello tjänstvärden måste påverkar dock alla repliker i samma process.

 **Exklusiv processen** modellen ger bättre isolering med varje replik i en egen process och en replik som påverkar inte andra repliker. Det är praktiskt i de fall där portdelning inte stöds av hello kommunikationsprotokoll. Det underlättar hello möjlighet tooapply resurs styrning på replik-nivå. Hej å andra sidan **exklusiv processen** förbrukar mer OS-resurser som den skapas en process för varje replik på hello nod.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Exklusiv processmodellen och programmet modellen överväganden
hello rekommenderat sätt toomodel ditt program i Service Fabric är tookeep en *ServiceType* per *ServicePackage* och den här modellen fungerar bra för de flesta av hello program. 

Dock tooenable särskilda scenarier där en *ServiceType* per *ServicePackage* får inte vara önskvärt funktionellt, Service Fabric kan toohave fler än ett *ServiceType* per *ServicePackage* (och en *CodePackage* kan registrera fler än en *ServiceType*). Här följer några hello scenarier där de här konfigurationerna kan vara användbara:

- Vill du toooptimize OS resursutnyttjande genom att skapa färre processer och med tätare replik processer.
- Repliker från olika ServiceTypes måste tooshare vissa vanliga data som har en hög initiering eller minne kostnad.
- Du har ett ledigt tjänsterbjudande och du vill tooput en gräns på resursutnyttjande genom att placera alla repliker av hello service i samma process.

**Exklusiv processen** värdmodell är inte konsekvent med programmodell att flera *ServiceTypes* per *ServicePackage*. Detta beror på att flera *ServiceTypes* per *ServicePackage* är utformad tooachieve högre resursdelning mellan replikerna och aktiverar tätare replik processer. Detta är motstridiga toowhat **exklusiv processen** modellen är utformad tooachieve.

Överväg att hello fallet med flera *ServiceTypes* per *ServicePackage* med olika *CodePackage* registrera varje *ServiceType*. Anta att vi har en *ServicePackage* 'MultiTypeServicePackge' som har två *CodePackages*:

- 'MyCodePackageA' som registrerar *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB' som registrerar *ServiceType* 'MyServiceTypeB'.

Nu kan exempelvis skapar vi ett *programmet* **fabric: / SpecialApp** och i **fabric: / SpecialApp** vi skapa följande två tjänster med **exklusiv processen** modellen:

- Tjänsten **fabric: / SpecialApp/ServiceA** av typen 'MyServiceTypeA' med två partitioner (säg **P1** och **P2**) och 3 repliker per partition.
- Tjänsten **fabric: / SpecialApp/ServiceB** av typen 'MyServiceTypeB' med två partitioner (säg **P3** och **P4**) och 3 repliker per partition.

På en viss nod har båda hello-tjänsterna två repliker. Eftersom vi använde **exklusiv processen** model toocreate hello-tjänster, Service Fabric aktiverar en ny kopia av 'MyServicePackge' för varje replik. Varje aktivering av 'MultiTypeServicePackge' startar en kopia av 'MyCodePackageA' och 'MyCodePackageB'. Endast en av 'MyCodePackageA' eller 'MyCodePackageB' ska dock vara värd för hello replik som 'MultiTypeServicePackge' har aktiverats. Följande diagram visar hello nod vy:

<center>
![Noden vy av distribuerade program][node-view-five]
</center>

 Som vi kan se i hello aktivering av 'MultiTypeServicePackge' för replik av partitionen **P1** för tjänsten **fabric: / SpecialApp/ServiceA**, hello replik är värd för 'MyCodePackageA' och ' MyCodePackageB' är bara aktiv och körs. På samma sätt i aktivering av 'MultiTypeServicePackge' för replik av partitionen **P3** för tjänsten **fabric: / SpecialApp/ServiceB**, hello replik är värd för 'MyCodePackageB' och 'MyCodePackageA' är just igång och så vidare. Därför mer hello antalet *CodePackages* (registrera olika *ServiceTypes*) per *ServicePackage*, högre är redundant Resursanvändning. 
 
 Hej i andra hand om vi skapa tjänster **fabric: / SpecialApp/ServiceA** och **fabric: / SpecialApp/ServiceB** med **delad Process** modellen, kommer Service Fabric Aktivera bara en kopia av 'MultiTypeServicePackge' för *programmet* **fabric: / SpecialApp** (som vi såg tidigare). 'MyCodePackageA' ska vara värd för alla repliker för tjänsten **fabric: / SpecialApp/ServiceA** (eller för en tjänst av typen 'MyServiceTypeA' toobe mer exakta) och 'MyCodePackageB' ska vara värd för alla repliker för tjänsten **fabric: / SpecialApp/ServiceB** (eller för en tjänst av typen 'MyServiceTypeB' toobe mer exakta). Följande diagram visar hello nod vyn i den här inställningen: 

<center>
![Noden vy av distribuerade program][node-view-six]
</center>

I ovanstående exempel hello, du tror om 'MyCodePackageA' registrerar både 'MyServiceTypeA' och 'MyServiceTypeB' och det finns inga MyCodePackageB sedan det blir inte redundant *CodePackage* körs. Det här är korrekt, men som tidigare nämnts är det här programmodell inte överensstämmer med **exklusiv processen** värdmodell. Om målet är tooput dedikerad varje replik i en egen process registrera både *ServiceTypes* från samma *CodePackage* inte behövs och placera varje *ServiceType* i en egen *ServicePacakge* är ett mer naturligt val.

## <a name="next-steps"></a>Nästa steg
[Paketerar ett program] [ a4] och få redo toodeploy.

[Distribuera och ta bort program] [ a5] beskriver hur toouse PowerShell toomanage programinstanser.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage