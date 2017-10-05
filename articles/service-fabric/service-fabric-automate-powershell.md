---
title: Automatisera hantering av Azure Service Fabric | Microsoft Docs
description: "Distribuera, uppgradering, testa och ta bort Service Fabric-program med hjälp av PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: f03f4294-571d-4262-8a77-cc8b481b959d
ms.service: service-fabric
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/16/2017
ms.author: ryanwi
redirect_url: /azure/service-fabric/service-fabric-deploy-remove-applications
ms.openlocfilehash: fb3c2a77ea887289eebf343e223c190781d0e4c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatisera livscykeln program med hjälp av PowerShell
Många aspekter av den [Service Fabric application livscykel](service-fabric-application-lifecycle.md) kan automatiseras.  Den här artikeln visar hur du använder PowerShell för att automatisera vanliga uppgifter för att distribuera, uppgradera, ta bort och testa Azure Service Fabric-program.  Hanteras och HTTP-APIs för apphantering finns också tillgängliga. Se [applivscykeln](service-fabric-application-lifecycle.md) för mer information.  

## <a name="prerequisites"></a>Krav
Innan du gå vidare till aktiviteter i artikeln måste du:

* Bekanta dig med Service Fabric-begrepp som beskrivs i [teknisk översikt över Service Fabric](service-fabric-technical-overview.md).
* [Installera runtime, verktyg och SDK](service-fabric-get-started.md), som installeras även den **ServiceFabric** PowerShell-modulen.
* [Aktiverar körning av PowerShell-skript](service-fabric-get-started.md#enable-powershell-script-execution).
* Starta en lokala klustret.  Starta ett nytt PowerShell-fönster som administratör och kör sedan installationsskriptet kluster från SDK-mappen:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Innan du kör PowerShell-kommandon i den här artikeln först ansluta till det lokala Service Fabric-klustret med hjälp av [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* Följande uppgifter kräver ett v1-programpaket för att distribuera och ett v2 programpaket för uppgradering. Hämta den [ **WordCount** exempelprogram](http://aka.ms/servicefabricsamples) (finns i komma igång-exempel). Skapa och paketera program i Visual Studio (Högerklicka på **WordCount** i Solution Explorer och markera **paketet**). Kopiera v1-paketet i `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` till `C:\Temp\WordCount`. Kopiera `C:\Temp\WordCount` till `C:\Temp\WordCountV2`, skapar programpaket v2 för uppgradering. Öppna `C:\Temp\WordCountV2\ApplicationManifest.xml` i en textredigerare. I den **ApplicationManifest** element, ändring av **ApplicationTypeVersion** attribut från ”1.0.0” till ”2.0.0” för att uppdatera versionsnumret app. Spara filen ändrade ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Uppgift: Distribuera ett Service Fabric-program
När du har skapat och paketeras programmet (eller hämtat programpaketet), kan du distribuera programmet till en lokal Service Fabric-klustret. Distributionen omfattar ladda upp programpaketet, registrera programtypen och skapa programinstansen. Följ instruktionerna i det här avsnittet för att distribuera ett nytt program till ett kluster.

### <a name="step-1-upload-the-application-package"></a>Steg 1: Ladda upp programpaketet
Ladda upp programpaketet image store placeras på en plats tillgänglig för interna Service Fabric-komponenter.  Programpaketet innehåller nödvändiga programmanifestet och service manifest kod, konfiguration och data medan du skapar program och instanser av tjänsten. Om du vill verifiera app-paketet lokalt använder den [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) cmdlet.  Den [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot laddar upp paketet. Exempel:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Steg 2: Registrera programtypen
Registrera programpaketet gör programtypen och versionen som deklarerats i programmanifestet tillgängliga för användning. System-läsningar paketet har överförts i steg 1, kontrollera paketet (motsvarar körs [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) lokalt), bearbeta paketinnehållet och kopiera bearbetade paketet till en plats för internt system.  Kör den [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Om du vill se alla programtyper som registrerats i klustret, kör den [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Steg 3: Skapa programinstansen
Ett program kan initieras med hjälp av någon version av programmet som har registrerats med hjälp av den [ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) kommando. Namnet på varje program som är deklarerad i distribuera tid och måste börja med den **fabric:** schemat och vara unikt för varje förekomst av programmet. Programmets typnamn och typ programversion deklareras i den **ApplicationManifest.xml** -filen för programpaketet. Om alla standardtjänster definierades i programmanifestet av programmet måltypen är de som skapas just nu.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Den [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) kommando visar alla programinstanser som har skapats, tillsammans med deras allmänna status. Den [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) kommando visar alla tjänstinstanser som har skapats i en given programinstansen. Standardtjänster (eventuella) visas.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Uppgift: Uppgradera ett Service Fabric-program
Du kan uppgradera en tidigare distribuerad Service Fabric-program med en uppdaterad programpaket. Den här uppgiften uppgraderar WordCount-program som har distribuerats i ”uppgiften: distribuera ett Service Fabric-program”. Läs igenom [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md) för mer information.

Endast program versionsnummer har uppdaterats i WordCountV2 programpaket som skapats i förutsättningarna för att hålla saker enkel för det här exemplet. En mer realistisk scenariot skulle innebära uppdaterar din service-filer i koden, konfiguration och data och sedan återskapa och paketera program med uppdaterade versionsnummer.  

### <a name="step-1-upload-the-updated-application-package"></a>Steg 1: Ladda upp det uppdaterade programpaketet
WordCount v1 programmet är redo att uppgraderas. Om du öppnar ett PowerShell-fönster som administratör och skriv [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), du ser att version 1.0.0 av typen WordCount programmet distribueras.  

Kopiera nu det uppdaterade programpaketet till Service Fabric image store (där programpaketen lagras av Service Fabric). Parametern **ApplicationPackagePathInImageStore** informerar Service Fabric där den kan hitta programpaketet. I följande kopieras programpaket till **WordCountV2** i image store:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Steg 2: Registrera uppdaterade programtyp
Nästa steg är att registrera den nya versionen av programmet med Service Fabric som kan utföras med hjälp av den [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Steg 3: Starta uppgraderingen
Olika uppgradera parametrar, tidsgränser och hälsotillstånd villkor kan tillämpas på programuppgraderingar. Läs igenom den [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) och [uppgraderingsprocessen](service-fabric-application-upgrade.md) dokument om du vill veta mer. Alla tjänster och instanser måste vara *felfri* efter uppgraderingen.  Ange den **HealthCheckStableDuration** till 60 sekunder (så att tjänsterna som är felfri för minst 20 sekunder innan uppgraderingen fortsätter till nästa uppgraderingsdomän).  Du också ange den **UpgradeDomainTimeout** till 1200 sekunder och **UpgradeTimeout** till 3000 sekunder. Slutligen, ställ det **UpgradeFailureAction** till **återställning**, som begär att Service Fabric återställer programmet till föregående version om fel uppstår under uppgraderingen.

Nu kan du starta uppgraderingen av program med hjälp av den [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Observera att programmets namn är samma som namnet på det tidigare distribuerade v1.0.0 program (fabric: / WordCount). Service Fabric använder det här namnet för att identifiera vilket program komma uppgraderas. Om du anger timeout vara för kort, kan det uppstå ett felmeddelande för timeout om problemet. Referera till [felsöka programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md), eller öka timeout-alternativen.

### <a name="step-4-check-upgrade-progress"></a>Steg 4: Kontrollera Uppgraderingsförlopp
Du kan övervaka programmet Uppgraderingsförlopp med hjälp av [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), eller genom att använda den [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Om några minuter på [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet visar att alla uppgraderingsdomäner har uppgraderats (slutförd).

## <a name="task-test-a-service-fabric-application"></a>Uppgift: Testa ett Service Fabric-program
Om du vill skriva hög kvalitet services behöver utvecklare kan orsaka instabilt infrastruktur fel för att testa stabiliteten för sina tjänster. Service Fabric ger utvecklare möjligheten att medföra fel åtgärder och testa tjänster med fel med testscenarier chaos och växling vid fel.  Läs igenom [introduktion till fel Analysis Service](service-fabric-testability-overview.md) för ytterligare information.

### <a name="step-1-run-the-chaos-test-scenario"></a>Steg 1: Kör chaos Testscenario
Testscenario chaos genererar fel över hela Service Fabric-kluster. Scenariot komprimerar fel som normalt sett över månader eller år till några timmar. Kombinationen av överlagrad fel med hög feltolerans hittar specialfall som annars skulle missas. Följande exempel kör test-scenario chaos i 60 minuter.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Steg 2: Kör test-scenario växling vid fel
Redundans Testa scenariot riktad mot en specifik tjänst partition i stället för hela klustret och den lämnar andra tjänster påverkas inte. Scenariot upprepas i en sekvens av simulerade fel i valideringen av tjänsten när affärslogik körs. Ett fel i tjänsten validering indikerar ett problem som kräver ytterligare undersökning. Testet redundans startar bara ett fel i taget, till skillnad från chaos test-scenario som kan ge upphov till flera fel. Följande exempel kör redundanstest i 60 minuter mot i fabric: / WordCount/WordCountService-tjänsten.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Uppgift: Ta bort ett Service Fabric-program
Du kan ta bort en instans av ett distribuerat program, ta bort den etablerade programtypen från klustret och ta bort programpaketet från ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Steg 1: Ta bort en instans av programmet
När en instans av programmet inte längre behövs, du permanent bort den med hjälp av den [ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet. Detta tar automatiskt bort alla tjänster som hör till programmet tar permanent bort alla tillstånd för tjänsten. Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Steg 2: Avregistrera programtypen
När du inte längre behöver en viss version av en typ av program, avregistrera den med hjälp av den [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet. Avregistrera oanvända typer Frigör lagringsutrymme som används av programpaket i image store. En typ av program kan avregistreras så länge det finns inga program instansierad mot den eller väntande programuppgraderingar som refererar till den.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Steg 3: Ta bort programpaketet
Efter att programtypen är avregistrera programpaketet kan tas bort från avbildningsarkivet med hjälp av den [ta bort ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Nästa steg
[Livscykel för Service Fabric-program](service-fabric-application-lifecycle.md)

[Distribuera ett program](service-fabric-deploy-remove-applications.md)

[Uppgradering av programmet](service-fabric-application-upgrade.md)

[Azure Service Fabric-cmdlets](/powershell/azure/overview?view=azureservicefabricps)

