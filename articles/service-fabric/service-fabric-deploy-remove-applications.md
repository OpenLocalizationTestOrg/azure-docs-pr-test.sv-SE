---
title: aaaAzure Service Fabric-programdistribution | Microsoft Docs
description: "Hur toodeploy och ta bort program i Service Fabric med hjälp av PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 3de9c6a937ee7b29bf9ec86d6e9e631487797507
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Distribuera och ta bort program med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient-API:er](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

När en [programtyp har paketerats][10], den är klar för distribution till Azure Service Fabric-kluster. Distributionen omfattar hello följande tre steg:

1. Överför hello programmet paketet toohello avbildningsarkivet
2. Registrera hello programtyp
3. Skapa hello programinstansen

När ett program distribueras och kör en instans i hello kluster kan du ta bort hello programinstansen och dess programtyp. toocompletely ta bort ett program från hello kluster omfattar hello följande steg:

1. Ta bort (eller ta bort) hello kör programinstansen
2. Avregistrera hello programtyp om du inte längre behöver
3. Ta bort hello programpaketet från hello avbildningsarkivet

Om du använder [Visual Studio för att distribuera och felsöka program](service-fabric-publish-app-remote-cluster.md) på klustret för lokal utveckling hello alla ovanstående steg hanteras automatiskt via ett PowerShell-skript.  Det här skriptet finns i hello *skript* för hello projektet. Den här artikeln innehåller bakgrund på vad skriptet gör så att du kan utföra samma åtgärder utanför Visual Studio hello. 
 
## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster
Innan du kör PowerShell-kommandon i den här artikeln måste alltid starta med hjälp av [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric-klustret. tooconnect toohello lokal utveckling klustret, kör hello följande:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Exempel på anslutande tooa fjärrkluster eller kluster som skyddas med Azure Active Directory, X509 certifikat eller Windows Active Directory finns [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-hello-application-package"></a>Överför hello programpaket
Ladda upp programpaket för hello placeras på en plats som kan nås av interna Service Fabric-komponenter.
Om du vill tooverify hello programpaket lokalt använder hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

Hej [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot överföringar hello avbildningsarkivet för programmet paketet toohello klustret.
Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.  tooimport hello SDK modul, kör:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Anta att du skapar och paketerar ett program med namnet *MyApplication* i Visual Studio 2015. Som standard är hello programmets typnamn som anges i hello ApplicationManifest.xml ”MyApplicationType”.  hello-programpaket som innehåller hello nödvändiga programmanifestet, service manifest och paket i kod-config-data, finns i *C:\Users\<användarnamn\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

hello visar följande kommando hello innehållet i hello programpaket:

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Om hello programpaket är stort och/eller har många filer, kan du [komprimera](service-fabric-package-apps.md#compress-a-package). hello komprimeringen minskar hello storlek och hello antal filer.
hello Bieffekten är att registrera och Avregistrerar hello programtyp är snabbare. Tid för gå långsammare för närvarande, särskilt om du inkluderar hello tid toocompress hello paketet. 

toocompress ett paket, Använd hello samma [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommando. Komprimering kan utföras separat från överföringen med hjälp av hello `SkipCopy` flagga eller tillsammans med hello överför igen. Tillämpa komprimering på komprimerade paketet är ingen op.
toouncompress komprimerade paketet, Använd hello samma [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) med hello `UncompressPackage` växla.

hello komprimerar följande cmdlet hello-paket utan att kopiera den toohello bildarkivet. hello paketet innehåller nu komprimerade filer för hello `Code` och `Config` paket. programmet hello och hello service manifest är inte zippade, eftersom de behövs för många interna åtgärder (till exempel dela program namn och version Extraheringen av anslutningstyp för vissa verifieringar-paket). Komprimerar hello manifesten skulle göra att dessa åtgärder ineffektiv.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

För stora programpaket tidskrävande hello komprimering. Använda en snabb SSD-enhet för bästa resultat. hello komprimering gånger och hello storleken på hello komprimerade paketet också variera beroende på hello paketinnehållet.
Här är till exempel komprimering statistik för vissa paket som visar hello inledande och hello komprimerade paketstorleken med hello komprimering tid.

|Ursprunglig storlek (MB)|Antal filer|Tid för komprimering|Komprimerade paketet storlek (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

När ett paket har komprimerats, det kan vara överförda tooone eller flera Service Fabric-kluster efter behov. mekanism för hello-distribution är samma för komprimerade och okomprimerade paket. Om hello paketet har komprimerats, lagras som sådana i hello avbildningsarkivet för klustret och den är okomprimerade på hello nod innan hello program körs.


hello överför följande exempel hello paketet toohello image store, till en mapp med namnet ”MyApplicationV1”:

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.  tooimport hello SDK modul, kör:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Om du inte anger hello *- ApplicationPackagePathInImageStore* parametern hello programpaket kopieras till hello ”Debug” mapp i hello avbildningsarkivet.

hello tid det tar tooupload ett paket skiljer sig beroende på flera faktorer. Vissa av dessa faktorer är hello antalet filer i hello paketet och hello paketstorleken hello filstorlekar. hello nätverkshastigheten hello källdatorn och hello Service Fabric-klustret påverkar också hello tid. Hej Standardtidsgräns för [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) är 30 minuter.
Beroende på hello beskrivs faktorer, du kan ha tooincrease hello timeout. Om du komprimerar hello paketet i hello kopiera anrop måste tooalso anses hello komprimering gång.

Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.

## <a name="register-hello-application-package"></a>Registrera hello programpaket
hello programtypen och versionen som deklarerats i hello programmanifestet blir tillgängliga för användning när hello programpaketet har registrerats. hello system läser hello paketet på hello föregående steg, verifierar hello paketet, bearbetar hello paketets innehåll och kopierar hello bearbetas paketplatsen tooan internt system.  

Kör hello [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet tooregister hello programtyp i hello klustret och gör den tillgänglig för distribution:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

”MyApplicationV1” är hello mapp i hello avbildningsarkivet där hello programpaket finns. hello programtyp med namnet ”MyApplicationType” och versionen ”1.0.0” (båda finns i programmanifestet hello) är nu registrerad i hello klustret.

Hej [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kommando returnerar bara när hello systemet har registrerades hello programpaket. Hur länge registrering tar beror på hello storlek och innehållet i hello programpaket. Om det behövs, hello **- TimeoutSec** parametern kan vara används toosupply en längre tidsgräns (hello standardvärdet för timeout är 60 sekunder).

Om du har en stor program paketet eller om du upplever tidsgränser använda hello **- asynkrona** parameter. hello kommando returnerar när hello klustret accepterar hello registreringskommandot och hello bearbetningen fortsätter efter behov.
Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus. Du kan använda det här kommandot toodetermine när hello registreringen är klar.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a>Skapa hello program
Du kan skapa en instans av ett program från en version av programmet som har registrerats med hjälp av hello [ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet. hello-namnet för varje program måste börja med hello *”fabric”:* schemat och måste vara unikt för varje förekomst av programmet. Alla standardtjänster som definierats i hello programmanifestet av hello Målprogramstyp skapas också.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Flera instanser av programmet kan skapas för en viss version av typen registrerade programmet. Varje instans av programmet körs i isolering med sin egen arbetskatalog och processen.

toosee som heter appar och tjänster som körs i hello klustret, kör hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) och [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Ta bort ett program
När en instans av programmet inte längre behövs, om du permanent ta bort den namn med hjälp av hello [ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet. [Ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) tar automatiskt bort alla tjänster som tillhör toohello program samt, permanent tar du bort alla tillstånd för tjänsten. 

> [!WARNING]
> Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Avregistrera en programtyp
När en viss version av en typ av program inte längre behövs, bör du avregistrera hello programtyp med hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet. Avregistrerar oanvända program typer versioner lagringsutrymme som används av hello image store. En typ av program kan avregistreras så länge inga program instansieras mot det och inte väntar programmet uppgraderingar hänvisar till den.

Kör [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello programtyper som är registrerade i hello klustret:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Kör [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister en specifik typ:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a>Ta bort ett programpaket från hello avbildningsarkivet
När ett programpaket inte längre behövs, du kan ta bort den från hello image store toofree upp systemresurser.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a>Felsökning
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiera ServiceFabricApplicationPackage begär en ImageStoreConnectionString
hello Service Fabric SDK miljö har redan hello rätt standardvärden ställa in. Men om det behövs, hello ImageStoreConnectionString för alla kommandon bör matcha hello-värde som hello Service Fabric-klustret. Du kan hitta hello ImageStoreConnectionString i hello klustermanifestet hämtades med hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) och Get-ImageStoreConnectionStringFromClusterManifest kommandon:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.  tooimport hello SDK modul, kör:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Hej ImageStoreConnectionString hittades i klustermanifestet hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.

### <a name="deploy-large-application-package"></a>Distribuera stora programpaket
Problem: [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) timeout för stora programpaket (ordning GB).
Försök:
- Ange en längre tidsgräns för [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot med `TimeoutSec` parameter. Som standard är hello timeout 30 minuter.
- Kontrollera hello nätverksanslutning mellan källdatorn och kluster. Om hello är långsam, Överväg att använda en dator med en bättre nätverksanslutning.
Om hello klientdatorn finns i en annan region än hello kluster, Överväg att använda en klientdator i en närmare eller samma region som hello kluster.
- Kontrollera om du träffa externa begränsning. Till exempel när hello avbildningsarkivet är konfigurerade toouse azure storage, kan överför att begränsas.

Problem: Överför paketet slutfördes, men [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) timeout. Försök:
- [Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.
hello komprimeringen minskar hello storlek och hello antal filer, vilket i sin tur minskar hello mängden trafik och fungerar som Service Fabric måste utföra. hello överföringen kan ta längre tid (särskilt om du inkluderar hello komprimering tid), men registrera och avregistrera hello programtyp är snabbare.
- Ange en längre tidsgräns för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) med `TimeoutSec` parameter.
- Ange `Async` växla för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). hello kommando returnerar när hello klustret accepterar hello kommandot och hello registrering av programtyp hello fortsätter asynkront. Därför finns det inget behov av toospecify en senare tidsgräns i det här fallet. Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus. Du kan använda det här kommandot toodetermine när hello registreringen är klar.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Distribuera programpaket med många filer
Problem: [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) tidsgränsen uppnås för ett programpaket med många filer (efter tusentalsavgränsare).
Försök:
- [Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet. hello komprimeringen minskar hello antal filer.
- Ange en längre tidsgräns för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) med `TimeoutSec` parameter.
- Ange `Async` växla för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). hello kommando returnerar när hello klustret accepterar hello kommandot och hello registrering av programtyp hello fortsätter asynkront.
Därför finns det inget behov av toospecify en senare tidsgräns i det här fallet. Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus. Du kan använda det här kommandot toodetermine när hello registreringen är klar.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Nästa steg
[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

[Service Fabric hälsa introduktion](service-fabric-health-introduction.md)

[Diagnostisera och felsöka en Service Fabric-tjänst](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellen är ett program i Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
