---
title: "aaaService Fabric App uppgradera med hjälp av PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur hello upplevelse av att distribuera ett Service Fabric-program, ändra hello kod och lansera uppgradering med hjälp av PowerShell."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f31212264de45c3b257a0efafb75c10c279b989f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>Uppgradering av Service Fabric-programmet med PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

hello används oftast och rekommenderade uppgradera metoden är hello övervakas löpande uppgradering.  Azure Service Fabric övervakar hello hälsotillstånd hello programmet uppgraderas baserat på en uppsättning hälsoprinciper. När en uppdateringsdomän (UD) har uppgraderats, Service Fabric utvärderar hello programmets hälsotillstånd och fortsätter toohello nästa uppdateringsdomän eller misslyckas hello uppgraderingen beroende på hello hälsoprinciper.

En Programuppgradering av övervakade kan utföras med hjälp av hello hanteras eller interna API: er, PowerShell eller REST. Anvisningar för att uppgradera med hjälp av Visual Studio finns [uppgradera ditt program med Visual Studio](service-fabric-application-upgrade-tutorial.md).

Med Service Fabric övervakas samlade uppgraderingar konfigurera hello programadministratör hello utvärdering hälsoprincip att Service Fabric använder toodetermine om hello programmet är felfritt. Hej administratör kan dessutom konfigurera hello åtgärd toobe utförs när hello hälsoutvärderingen misslyckas (till exempel göra en automatisk återställning.) Det här avsnittet går igenom en övervakade uppgradering för en hello SDK-exempel som använder PowerShell. hello följande Microsoft Virtual Academy videoklippet även innehåller en app-uppgradering:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-hello-visual-objects-sample"></a>Steg 1: Skapa och distribuera hello visuella objekt exempel
Skapa och publicera programmet hello genom att högerklicka på projektet hello, **VisualObjectsApplication,** och välja hello **publicera** kommando.  Mer information finns i [Service Fabric uppgradera självstudien](service-fabric-application-upgrade-tutorial.md).  Du kan också använda PowerShell toodeploy ditt program.

> [!NOTE]
> Innan någon av hello Service Fabric-kommandon kan användas i PowerShell, måste du först tooconnect toohello kluster med hjälp av hello `Connect-ServiceFabricCluster` cmdlet. På liknande sätt, förutsätts att hello klustret har redan ställts in på den lokala datorn. Se hello artikel på [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started.md).
> 
> 

När du har skapat hello-projekt i Visual Studio kan du använda hello PowerShell-kommando [kopiera ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) toocopy hello programmet paketet toohello Avbildningsarkiv. Om du vill tooverify hello appaket lokalt, Använd hello [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) cmdlet. hello nästa steg är tooregister hello toohello Service Fabric programkörning med hello [registrera ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) cmdlet. hello sista steget är toostart en instans av programmet hello med hjälp av hello [ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.  De här tre stegen är liknande toousing hello **distribuera** menyalternativet i Visual Studio.

Nu kan du använda [Service Fabric Explorer tooview hello klustret och hello programmet](service-fabric-visualizing-your-cluster.md). hello programmet har en webbtjänst som kan vara navigerat tooin Internet Explorer genom att skriva [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) i hello adressfältet.  Du bör se vissa flytande visuella objekt flytta runt i hello-skärmen.  Du kan dessutom använda [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) toocheck hello programstatus.

## <a name="step-2-update-hello-visual-objects-sample"></a>Steg 2: Uppdatera hello visuella objekt exempel
Du kan se att med hello-version som har distribuerats i steg 1 hello visuella objekt inte rotera. Vi uppgradera det här programmet tooone där hello visuella objekt också rotera.

Välj hello VisualObjects.ActorService projekt inom hello VisualObjects lösningen och öppna hello StatefulVisualObjectActor.cs filen. Navigera i filen, toohello metoden `MoveObject`, kommentera ut `this.State.Move()`, och Avkommentera `this.State.Move(true)`. Den här ändringen roterar hello objekt när hello-tjänsten har uppgraderats.

Vi behöver också tooupdate hello *ServiceManifest.xml* fil (under PackageRoot) av hello projektet **VisualObjects.ActorService**. Uppdatera hello *CodePackage* hello service version too2.0 och hello motsvarande rader i hello *ServiceManifest.xml* fil.
Du kan använda hello Visual Studio *redigera Manifest filer* alternativ när du högerklickar på hello lösning toomake hello manifestfilen ändringar.

När hello ändringar har gjorts hello manifestet bör se ut som följande hello (markerade delar visa hello ändringar):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nu hello *ApplicationManifest.xml* fil (finns under hello **VisualObjects** projekt under hello **VisualObjects** lösning) är uppdaterade tooversion 2.0 för hello  **VisualObjects.ActorService** projekt. Dessutom är hello programversion uppdaterade too2.0.0.0 från 1.0.0.0. Hej *ApplicationManifest.xml* ser ut som hello följande utdrag:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Nu kan skapa hello projektet genom att välja bara hello **ActorService** -projektet och högerklicka och välja hello **bygga** alternativet i Visual Studio. Om du väljer **återskapa alla**, bör du uppdatera hello versioner för alla projekt, eftersom hello koden skulle ha ändrats. Nästa vi paketet hello uppdaterade tillämpningsprogrammet genom att högerklicka på ***VisualObjectsApplication***, välja hello Service Fabric-menyn och välja **paketet**. Den här åtgärden skapar ett programpaket som kan distribueras.  Uppdaterade programmet är redo toobe distribueras.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Steg 3: Bestäm hälsoprinciper och uppgradering av parametrar
Bekanta dig med hello [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) och hello [uppgraderingsprocessen](service-fabric-application-upgrade.md) tooget en god förståelse av hello olika uppgradera parametrar, timeout och hälsotillstånd kriterium tillämpas . Den här genomgången hello service hälsa utvärdering kriterium angetts toohello standard (och rekommenderas) värden, vilket innebär att alla tjänster och instanser ska vara *felfri* efter hello uppgradering.  

Vi dock öka hello *HealthCheckStableDuration* too60 sekunder (så att hello tjänster är felfri för minst 20 sekunder innan hello uppgraderingen fortsätter toohello nästa uppdatering domän).  Vi ska också ange hello *UpgradeDomainTimeout* toobe 1200 sekunder och hello *UpgradeTimeout* toobe 3000 sekunder.

Slutligen ska vi också ange hello *UpgradeFailureAction* toorollback. Det här alternativet kräver Service Fabric tooroll tillbaka hello toohello tidigare programversion om det uppstår problem under hello uppgraderingen. Det innebär när du startar uppgraderingen hello (i steg 4) har hello följande parametrar angetts:

FailureAction = återställning

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>Steg 4: Förbereda program för uppgradering
Nu hello programmet har skapats och redo toobe uppgraderas. Om du öppnar ett PowerShell-fönster som administratör och skriv [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), bör den gör att du vet att den är programtyp 1.0.0.0 av **VisualObjects** som har distribuerats.  

hello programpaket lagras under hello följande relativ sökväg där du okomprimerade hello Service Fabric-SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Katalogen där hello programpaket lagras bör du hitta en ”paketmapp”. Kontrollera hello tidsstämplar tooensure att det är hello senaste build-versionen (du kanske måste toomodify hello korrekt samt sökvägar).

Nu ska vi uppdatera kopia hello programmet paketet toohello Service Fabric Avbildningsarkiv (där hello programpaket lagras av Service Fabric). Hej parametern *ApplicationPackagePathInImageStore* informerar Service Fabric där den kan hitta hello programpaket. Vi har gjort programmet hello uppdateras ”VisualObjects\_V2” med följande kommando (du kanske måste toomodify sökvägar igen på lämpligt sätt) hello.

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

hello nästa steg är tooregister programmet med Service Fabric som kan utföras med hjälp av hello [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kommando:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Om hello föregående kommando inte lyckas är det troligt att du behöver en återskapning av alla tjänster. Som tidigare nämnts i steg 2 kan kanske du tooupdate din WebService-version.

## <a name="step-5-start-hello-application-upgrade"></a>Steg 5: Starta uppgradering av programmet hello
Nu ska vi alla set toostart hello programmet uppgradera med hjälp av hello [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) kommando:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


hello programnamnet är hello samma som det beskrivs i hello *ApplicationManifest.xml* fil. Service Fabric använder det här namnet tooidentify vilket program komma uppgraderas. Om du ställer in hello timeout toobe för kort, kan det uppstå ett felmeddelande om att tillstånd hello problem. Se avsnittet om felsökning toohello eller öka hello timeout.

Nu som hello programmet uppgraderingen fortsätter du kan övervaka den med hjälp av Service Fabric Explorer eller genom att använda hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell-kommando: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Om några minuter hello status som du fick med hjälp av hello föregående PowerShell-kommando bör ange att alla domäner som uppdateringen har uppgraderats (slutförd). Och du finner att hello visuella objekt i webbläsarfönstret har startat rotera!

Du kan uppgradera från version 2 tooversion 3 eller från version 2 tooversion 1 som en övning. Flytta från version 2 tooversion 1 anses också vara en uppgradering. Spela upp med tidsgränser och hälsotillstånd principer toomake själv bekant med dem. När du distribuerar tooan Azure klustret hello parametrar måste toobe angetts på rätt sätt. Det är bra tooset hello timeout hänsyn.

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

Styra hur programmet ska uppgraderas med hjälp av [Uppgraderingsparametrar](service-fabric-application-upgrade-parameters.md).

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade alternativ](service-fabric-application-upgrade-advanced.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).

