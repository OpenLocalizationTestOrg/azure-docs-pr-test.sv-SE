---
title: aaaAutomate Azure Service Fabric programhantering | Microsoft Docs
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
ms.openlocfilehash: a64a39dbee26be8ac15fee767a90fd06bfe4b896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-hello-application-lifecycle-using-powershell"></a>Automatisera hello programmet livscykel med hjälp av PowerShell
Många aspekter av hello [Service Fabric application livscykel](service-fabric-application-lifecycle.md) kan automatiseras.  Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter för att distribuera, uppgradera, ta bort och testa Azure Service Fabric-program.  Hanteras och HTTP-APIs för apphantering finns också tillgängliga. Se [applivscykeln](service-fabric-application-lifecycle.md) för mer information.  

## <a name="prerequisites"></a>Krav
Innan du flyttar toohello aktiviteter i hello artikel, måste du:

* Bekanta dig med hello Service Fabric-begrepp som beskrivs i [teknisk översikt över Service Fabric](service-fabric-technical-overview.md).
* [Installera hello runtime, verktyg och SDK](service-fabric-get-started.md), som installeras också hello **ServiceFabric** PowerShell-modulen.
* [Aktiverar körning av PowerShell-skript](service-fabric-get-started.md#enable-powershell-script-execution).
* Starta en lokala klustret.  Starta ett nytt PowerShell-fönster som administratör och kör sedan hello klustret installationsskriptet från hello SDK-mappen:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Innan du kör PowerShell-kommandon i den här artikeln först ansluta toohello lokala Service Fabric-kluster med hjälp av [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* hello följande uppgifter kräver en v1 programmet paketet toodeploy och ett v2 programpaket för uppgradering. Hämta hello [ **WordCount** exempelprogram](http://aka.ms/servicefabricsamples) (finns i hello komma igång exempel). Skapa och paketera hello program i Visual Studio (Högerklicka på **WordCount** i Solution Explorer och markera **paketet**). Kopiera hello v1 paketet i `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` för`C:\Temp\WordCount`. Kopiera `C:\Temp\WordCount` för`C:\Temp\WordCountV2`, skapa hello v2 programpaket för uppgradering. Öppna `C:\Temp\WordCountV2\ApplicationManifest.xml` i en textredigerare. I hello **ApplicationManifest** element, ändra hello **ApplicationTypeVersion** attribut från ”1.0.0” för ”2.0.0” tooupdate hello app versionsnumret. Spara hello ändras ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Uppgift: Distribuera ett Service Fabric-program
När du har skapat och paketeras hello program (eller hämtas hello programpaket), kan du distribuera hello programmet till en lokal Service Fabric-klustret. Distributionen omfattar överföringen hello programpaket, registrera hello programtyp och skapa hello programinstansen. Använd hello instruktioner i det här avsnittet toodeploy ett nytt program tooa kluster.

### <a name="step-1-upload-hello-application-package"></a>Steg 1: Överför hello programpaket
Överför hello programmet paketet toohello placeras avbildningsarkivet på en plats tillgänglig toointernal Service Fabric-komponenter.  hello programpaketet innehåller hello nödvändiga programmanifestet och service manifest kod, konfiguration och data medan toocreate hello program och instanser av tjänsten. Om du vill tooverify hello appaket lokalt, Använd hello [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) cmdlet.  Hej [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot överföringar hello paketet. Exempel:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-hello-application-type"></a>Steg 2: Registrera hello programtyp
Registrera hello programpaket gör hello programtypen och versionen som deklarerats i hello programmanifestet som är tillgängliga för användning. hello systemet läser hello-paket som överförts i hello steg 1, verifiera hello paketet (motsvarande toorunning [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) lokalt), bearbeta hello paketets innehåll och kopierar hello bearbetas paketet tooan plats för internt system.  Kör hello [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCount
```
alla typer av programmet hello registrerad i hello-klustret, kör hello toosee [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-hello-application-instance"></a>Steg 3: Skapa hello programinstansen
Ett program kan initieras med hjälp av någon version av programmet som har registrerats med hjälp av hello [ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) kommando. hello-namnet för varje program som är deklarerad i distribuera tid och måste börja med hello **fabric:** schemat och vara unikt för varje förekomst av programmet. hello programmets typnamn och typ programversion deklareras i hello **ApplicationManifest.xml** -filen för hello programpaket. Om alla standardtjänster definierades i hello programmanifestet av hello Målprogramstyp, skapas de just nu.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Hej [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) kommando visar alla programinstanser som har skapats, tillsammans med deras allmänna status. Hej [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) kommando visar alla hello-tjänstinstanser som har skapats i en given programinstansen. Standardtjänster (eventuella) visas.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Uppgift: Uppgradera ett Service Fabric-program
Du kan uppgradera en tidigare distribuerad Service Fabric-program med en uppdaterad programpaket. Den här uppgiften uppgraderar hello WordCount program som har distribuerats i ”uppgiften: distribuera ett Service Fabric-program”. Läs igenom [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md) för mer information.

tookeep saker enkel i det här exemplet endast hello program versionsnummer har uppdaterats i hello WordCountV2 programpaket som skapats i hello krav. En mer realistisk scenariot skulle innebära uppdaterar din service-filer i koden, konfiguration och data och sedan återskapa och paketera programmet hello med uppdaterade versionsnummer.  

### <a name="step-1-upload-hello-updated-application-package"></a>Steg 1: Överför hello uppdaterade programpaket
Hej WordCount v1 programmet är redo toobe uppgraderas. Om du öppnar ett PowerShell-fönster som administratör och skriv [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), visas den versionen 1.0.0 av hello WordCount programtyp har distribuerats.  

Kopiera hello uppdaterats programmet paketet toohello Service Fabric avbildningsarkivet (där hello programpaket lagras av Service Fabric). Hej parametern **ApplicationPackagePathInImageStore** informerar Service Fabric där den kan hitta hello programpaket. Hej följande kommando kopior hello programpaket för**WordCountV2** i hello avbildningsarkivet:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-hello-updated-application-type"></a>Steg 2: Registrera hello uppdateras programtyp
hello nästa steg är tooregister hello ny version av programmet hello med Service Fabric som kan utföras med hjälp av hello [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-hello-upgrade"></a>Steg 3: Starta hello uppgradering
Olika uppgradera parametrar, tidsgränser och hälsotillstånd villkor kan vara tillämpade tooapplication uppgraderingar. Läs igenom hello [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) och [uppgraderingsprocessen](service-fabric-application-upgrade.md) dokument toolearn mer. Alla tjänster och instanser måste vara *felfri* efter hello uppgradering.  Ange hello **HealthCheckStableDuration** too60 sekunder (så att hello tjänster är felfri för minst 20 sekunder innan hello uppgraderingen fortsätter toohello nästa uppgraderingsdomän).  Även ange hello **UpgradeDomainTimeout** too1200 sekunder och hello **UpgradeTimeout** too3000 sekunder. Slutligen, Ställ hello **UpgradeFailureAction** för**återställning**, vilka begäranden som Service Fabric återställer hello programmet toohello tidigare version om fel uppstår under uppgraderingen.

Nu kan du starta uppgradering av programmet hello med hjälp av hello [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Observera att hello programnamnet är samma hello som hello tidigare distribuerat v1.0.0 programnamn (fabric: / WordCount). Service Fabric använder det här namnet tooidentify vilket program komma uppgraderas. Ett felmeddelande för timeout att tillstånd hello problem kan uppstå om du ställer in hello timeout toobe för kort. Se för[felsöka programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md), eller öka hello timeout.

### <a name="step-4-check-upgrade-progress"></a>Steg 4: Kontrollera Uppgraderingsförlopp
Du kan övervaka programmet Uppgraderingsförlopp med hjälp av [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), eller genom att använda hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Om några minuter hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) cmdlet visar att alla uppgraderingsdomäner har uppgraderats (slutförd).

## <a name="task-test-a-service-fabric-application"></a>Uppgift: Testa ett Service Fabric-program
toowrite hög kvalitet services utvecklare behöver toobe kan tooinduce instabilt infrastruktur fel tootest hello stabiliteten för sina tjänster. Service Fabric ger utvecklare hello möjlighet tooinduce fel åtgärder och testa tjänster i hello förekomst av fel med hjälp av hello chaos och redundans testscenarier.  Läs igenom [introduktion toohello fel Analysis Service](service-fabric-testability-overview.md) för ytterligare information.

### <a name="step-1-run-hello-chaos-test-scenario"></a>Steg 1: Kör hello chaos Testscenario
hello chaos Testscenario genererar fel över hello hela Service Fabric-kluster. hello scenariot komprimerar fel som normalt sett över månader eller år tooa några timmar. hello kombination av överlagrad fel med hög feltolerans hittar specialfall som annars skulle missas. hello följande exempel kör hello chaos Testscenario i 60 minuter.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-hello-failover-test-scenario"></a>Steg 2: Kör hello redundans Testscenario
hello redundans Testa scenariot riktad mot en specifik tjänst partition i stället för hello hela klustret och den lämnar andra tjänster påverkas inte. hello scenariot går igenom en sekvens av simulerade fel i valideringen av tjänsten när affärslogik körs. Ett fel i tjänsten validering indikerar ett problem som kräver ytterligare undersökning. Hej redundanstest startar bara ett fel vid ett tillfälle som skillnad från toohello chaos Testscenario, som kan ge upphov till flera fel. hello följande exempel kör hello redundanstest i 60 minuter mot hello fabric: / WordCount/WordCountService-tjänsten.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Uppgift: Ta bort ett Service Fabric-program
Du kan ta bort en instans av ett distribuerat program, ta bort hello etablerats programtyp från hello-kluster och ta bort hello programpaketet från hello Avbildningsarkiv.

### <a name="step-1-remove-an-application-instance"></a>Steg 1: Ta bort en instans av programmet
När en instans av programmet inte längre behövs, du permanent bort den med hjälp av hello [ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet. Detta tar automatiskt bort alla tjänster tillhörande toohello program, tar permanent bort alla Tjänststatus. Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-hello-application-type"></a>Steg 2: Avregistrera hello programtyp
När du inte längre behöver en viss version av en typ av program, avregistrera den med hjälp av hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet. Avregistrerar oanvända typer versioner lagringsutrymme som används av hello programpaket i hello avbildningsarkivet. En typ av program kan avregistreras så länge det finns inga program instansierad mot den eller väntande programuppgraderingar som refererar till den.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-hello-application-package"></a>Steg 3: Ta bort hello programpaket
När hello programtyp har avregistrerats hello programpaket kan tas bort från avbildningsarkivet hello med hjälp av hello [ta bort ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Nästa steg
[Livscykel för Service Fabric-program](service-fabric-application-lifecycle.md)

[Distribuera ett program](service-fabric-deploy-remove-applications.md)

[Uppgradering av programmet](service-fabric-application-upgrade.md)

[Azure Service Fabric-cmdlets](/powershell/azure/overview?view=azureservicefabricps)

