---
title: aaaPackage en Azure Service Fabric-app | Microsoft Docs
description: Hur toopackage ett Service Fabric-program innan du distribuerar tooa klustret.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: b3918e1e25e532acdc9440855213e1fa364ea000
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="package-an-application"></a>Paketera ett program
Den här artikeln beskriver hur toopackage ett Service Fabric-program och förbereda för distribution.

## <a name="package-layout"></a>Paketet layout
hello programmanifestet, en eller flera service manifest och andra nödvändiga paketet filer måste ordnas i en viss layout för distribution i ett Service Fabric-kluster. hello exempel manifesten i den här artikeln måste toobe ordnade i hello följande katalogstruktur:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

hello mappar namnges toomatch hello **namn** attribut för varje motsvarande element. Till exempel om hello tjänstmanifestet finns två kod paket med hello namn **MyCodeA** och **MyCodeB**, och sedan två mappar med samma namn innehåller hello hello-binärfiler som krävs för varje kod paketet.

## <a name="use-setupentrypoint"></a>Använd SetupEntryPoint
Vanliga scenarier där **SetupEntryPoint** är när du behöver toorun en körbar fil innan hello-tjänsten startar eller måste tooperform en åtgärd med förhöjda privilegier. Exempel:

* Ställa in och initierar miljövariabler som hello körbara behöver. Det är inte begränsad tooonly körbara filer skrivs via hello Service Fabric programmeringsmodeller. Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.
* Konfigurera åtkomstkontroll genom att installera security-certifikat.

Mer information om hur tooconfigure hello **SetupEntryPoint**, se [konfigurera hello princip för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurera
### <a name="build-a-package-by-using-visual-studio"></a>Skapa ett paket med hjälp av Visual Studio
Om du använder Visual Studio 2015 toocreate ditt program kan använda du hello paketet kommandot tooautomatically skapa ett paket som matchar hello layout som beskrivs ovan.

toocreate ett paket, högerklicka på projektet i Solution Explorer hello och välj hello paketkommando, enligt nedan:

![Ett program med Visual Studio-paket][vs-package-command]

När paketering är klar hittar du hello platsen för hello i hello **utdata** fönster. hello paketering steg sker automatiskt när du distribuerar eller felsöker programmet i Visual Studio.

### <a name="build-a-package-by-command-line"></a>Skapa ett paket av kommandoraden
Det är också möjligt tooprogrammatically paketet upp appen med `msbuild.exe`. Hello huven körs Visual Studio under det så hello utdata är samma.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a>Hello testpaket
Du kan verifiera hello paketet struktur lokalt via PowerShell genom att använda hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) kommando.
Detta kommando kontrollerar manifestet parsning problem och kontrollera alla referenser. Detta kommando kontrollerar endast hello strukturella är korrekt hello kataloger och filer i hello-paketet.
Går det inte verifiera några hello kod eller data paketinnehållet utöver att kontrollera att alla nödvändiga filer finns.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Det här felet visas den hello *MySetup.bat* fil som refereras i hello tjänstmanifestet **SetupEntryPoint** saknas hello kodpaketet. När hello saknad fil har lagts till, skickar hello programmet verifiering:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Om programmet har [applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) definierats kan du skicka dem i [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) för rätt validering.

Om du vet hello kluster där hello programmet ska distribueras, bör du skicka in hello `ImageStoreConnectionString` parameter. I det här fallet verifieras hello paketet också mot tidigare versioner av programmet hello som redan körs i hello klustret. Till exempel hello verifiering kan känna av om ett paket med hello samma version men olika innehåll har redan distribuerats.  

När programmet hello paketeras på rätt sätt och har klarat valideringen kan utvärdera baserat på hello storlek och hello antalet filer som eventuellt komprimering.

## <a name="compress-a-package"></a>Komprimera ett paket
Om ett paket är stora eller många filer, kan du komprimera den för snabbare distribution. Komprimeringen minskar hello antal filer och hello paketstorleken.
För en komprimerad programpaket [Uploading hello programpaket](service-fabric-deploy-remove-applications.md#upload-the-application-package) kan ta längre jämfört med toouploading hello okomprimerade paketet (särskilt om komprimering tid är inberäknade), men [registrera](service-fabric-deploy-remove-applications.md#register-the-application-package) och [avregistrerades hello programtyp](service-fabric-deploy-remove-applications.md#unregister-an-application-type) är snabbare för ett komprimerade programpaket.

mekanism för hello-distribution är samma för komprimerade och okomprimerade paket. Om hello paketet har komprimerats, lagras som sådana i hello avbildningsarkivet för klustret och den är okomprimerade på hello nod innan hello program körs.
hello komprimering ersätter hello giltigt Service Fabric-paket med hello komprimerad version. hello mapp måste tillåta skrivbehörighet. Kör komprimering på en redan komprimerade paketet ger inga ändringar.

Du kan komprimera ett paket genom att köra Powershell-kommando för hello [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) med `CompressPackage` växla. Du kan expandera hello paketet med hello samma kommando med `UncompressPackage` växla.

hello komprimerar följande kommando hello-paket utan att kopiera den toohello bildarkivet. Du kan kopiera en komprimerade paketet tooone eller flera Service Fabric-kluster efter behov med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) utan hello `SkipCopy` flaggan.
hello paketet innehåller nu komprimerade filer för hello `code`, `config`, och `data` paket. hello programmanifestet och hello service manifest är inte zippade, eftersom de behövs för många interna åtgärder (till exempel dela program namn och version Extraheringen av anslutningstyp för vissa verifieringar-paket).
Komprimerar hello manifesten skulle göra att dessa åtgärder ineffektiv.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Du kan komprimera och kopiera hello paket med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) i ett steg.
Om hello paketet är stort, ange en tillräckligt hög tidsgränsen tooallow för både hello paketet komprimering och hello överför toohello klustret.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Internt, beräknar Service Fabric kontrollsummor för hello programpaket för verifiering. När du använder komprimering beräknas hello kontrollsummor på hello zippade versioner av varje paket.
Om du kopierade en okomprimerad version av ditt programpaket och du vill toouse komprimering för hello samma paket, måste du ändra hello versioner av hello `code`, `config`, och `data` paket tooavoid kontrollsummorna överensstämmer inte. Om hello-paket är oförändrat, istället för att ändra hello version, kan du använda [diff etablering](service-fabric-application-upgrade-advanced.md). Med det här alternativet inte innehåller hello oförändrat paketet i stället referera till den från hello service manifest.

Om du har överfört en komprimerad version av hello paketet och du vill toouse en okomprimerad paketet, på samma sätt måste du uppdatera hello versioner tooavoid hello felaktig matchning av kontrollsumma.

hello paketet är nu paketeras korrekt, verifiera och komprimerade (vid behov), så att den är klar för [distribution](service-fabric-deploy-remove-applications.md) tooone eller flera Service Fabric-kluster.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Komprimera paket när du distribuerar med Visual Studio
Du kan instruera Visual Studio toocompress paket för distribution, genom att lägga till hello `CopyPackageParameters` elementet tooyour Publicera profil och ange hello `CompressPackage` attribut för`true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Nästa steg
[Distribuera och ta bort program] [ 10] beskriver hur toouse PowerShell toomanage programinstanser

[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur tooconfigure parametrar och miljövariabler för olika programinstanser.

[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur toorun services under principer toorestrict åtkomst.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
