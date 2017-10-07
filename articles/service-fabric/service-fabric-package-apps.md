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
# <a name="package-an-application"></a><span data-ttu-id="2caf9-103">Paketera ett program</span><span class="sxs-lookup"><span data-stu-id="2caf9-103">Package an application</span></span>
<span data-ttu-id="2caf9-104">Den här artikeln beskriver hur toopackage ett Service Fabric-program och förbereda för distribution.</span><span class="sxs-lookup"><span data-stu-id="2caf9-104">This article describes how toopackage a Service Fabric application and make it ready for deployment.</span></span>

## <a name="package-layout"></a><span data-ttu-id="2caf9-105">Paketet layout</span><span class="sxs-lookup"><span data-stu-id="2caf9-105">Package layout</span></span>
<span data-ttu-id="2caf9-106">hello programmanifestet, en eller flera service manifest och andra nödvändiga paketet filer måste ordnas i en viss layout för distribution i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="2caf9-106">hello application manifest, one or more service manifests, and other necessary package files must be organized in a specific layout for deployment into a Service Fabric cluster.</span></span> <span data-ttu-id="2caf9-107">hello exempel manifesten i den här artikeln måste toobe ordnade i hello följande katalogstruktur:</span><span class="sxs-lookup"><span data-stu-id="2caf9-107">hello example manifests in this article would need toobe organized in hello following directory structure:</span></span>

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

<span data-ttu-id="2caf9-108">hello mappar namnges toomatch hello **namn** attribut för varje motsvarande element.</span><span class="sxs-lookup"><span data-stu-id="2caf9-108">hello folders are named toomatch hello **Name** attributes of each corresponding element.</span></span> <span data-ttu-id="2caf9-109">Till exempel om hello tjänstmanifestet finns två kod paket med hello namn **MyCodeA** och **MyCodeB**, och sedan två mappar med samma namn innehåller hello hello-binärfiler som krävs för varje kod paketet.</span><span class="sxs-lookup"><span data-stu-id="2caf9-109">For example, if hello service manifest contained two code packages with hello names **MyCodeA** and **MyCodeB**, then two folders with hello same names would contain hello necessary binaries for each code package.</span></span>

## <a name="use-setupentrypoint"></a><span data-ttu-id="2caf9-110">Använd SetupEntryPoint</span><span class="sxs-lookup"><span data-stu-id="2caf9-110">Use SetupEntryPoint</span></span>
<span data-ttu-id="2caf9-111">Vanliga scenarier där **SetupEntryPoint** är när du behöver toorun en körbar fil innan hello-tjänsten startar eller måste tooperform en åtgärd med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="2caf9-111">Typical scenarios for using **SetupEntryPoint** are when you need toorun an executable before hello service starts or you need tooperform an operation with elevated privileges.</span></span> <span data-ttu-id="2caf9-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2caf9-112">For example:</span></span>

* <span data-ttu-id="2caf9-113">Ställa in och initierar miljövariabler som hello körbara behöver.</span><span class="sxs-lookup"><span data-stu-id="2caf9-113">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="2caf9-114">Det är inte begränsad tooonly körbara filer skrivs via hello Service Fabric programmeringsmodeller.</span><span class="sxs-lookup"><span data-stu-id="2caf9-114">It is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="2caf9-115">Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.</span><span class="sxs-lookup"><span data-stu-id="2caf9-115">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="2caf9-116">Konfigurera åtkomstkontroll genom att installera security-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2caf9-116">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="2caf9-117">Mer information om hur tooconfigure hello **SetupEntryPoint**, se [konfigurera hello princip för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="2caf9-117">For more information on how tooconfigure hello **SetupEntryPoint**, see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<a id="Package-App"></a>
## <a name="configure"></a><span data-ttu-id="2caf9-118">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="2caf9-118">Configure</span></span>
### <a name="build-a-package-by-using-visual-studio"></a><span data-ttu-id="2caf9-119">Skapa ett paket med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2caf9-119">Build a package by using Visual Studio</span></span>
<span data-ttu-id="2caf9-120">Om du använder Visual Studio 2015 toocreate ditt program kan använda du hello paketet kommandot tooautomatically skapa ett paket som matchar hello layout som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="2caf9-120">If you use Visual Studio 2015 toocreate your application, you can use hello Package command tooautomatically create a package that matches hello layout described above.</span></span>

<span data-ttu-id="2caf9-121">toocreate ett paket, högerklicka på projektet i Solution Explorer hello och välj hello paketkommando, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="2caf9-121">toocreate a package, right-click hello application project in Solution Explorer and choose hello Package command, as shown below:</span></span>

![Ett program med Visual Studio-paket][vs-package-command]

<span data-ttu-id="2caf9-123">När paketering är klar hittar du hello platsen för hello i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="2caf9-123">When packaging is complete, you can find hello location of hello package in hello **Output** window.</span></span> <span data-ttu-id="2caf9-124">hello paketering steg sker automatiskt när du distribuerar eller felsöker programmet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2caf9-124">hello packaging step occurs automatically when you deploy or debug your application in Visual Studio.</span></span>

### <a name="build-a-package-by-command-line"></a><span data-ttu-id="2caf9-125">Skapa ett paket av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="2caf9-125">Build a package by command line</span></span>
<span data-ttu-id="2caf9-126">Det är också möjligt tooprogrammatically paketet upp appen med `msbuild.exe`.</span><span class="sxs-lookup"><span data-stu-id="2caf9-126">It is also possible tooprogrammatically package up your application using `msbuild.exe`.</span></span> <span data-ttu-id="2caf9-127">Hello huven körs Visual Studio under det så hello utdata är samma.</span><span class="sxs-lookup"><span data-stu-id="2caf9-127">Under hello hood, Visual Studio is running it so hello output is same.</span></span>

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a><span data-ttu-id="2caf9-128">Hello testpaket</span><span class="sxs-lookup"><span data-stu-id="2caf9-128">Test hello package</span></span>
<span data-ttu-id="2caf9-129">Du kan verifiera hello paketet struktur lokalt via PowerShell genom att använda hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) kommando.</span><span class="sxs-lookup"><span data-stu-id="2caf9-129">You can verify hello package structure locally through PowerShell by using hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span>
<span data-ttu-id="2caf9-130">Detta kommando kontrollerar manifestet parsning problem och kontrollera alla referenser.</span><span class="sxs-lookup"><span data-stu-id="2caf9-130">This command checks for manifest parsing issues and verify all references.</span></span> <span data-ttu-id="2caf9-131">Detta kommando kontrollerar endast hello strukturella är korrekt hello kataloger och filer i hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="2caf9-131">This command only verifies hello structural correctness of hello directories and files in hello package.</span></span>
<span data-ttu-id="2caf9-132">Går det inte verifiera några hello kod eller data paketinnehållet utöver att kontrollera att alla nödvändiga filer finns.</span><span class="sxs-lookup"><span data-stu-id="2caf9-132">It doesn't verify any of hello code or data package contents beyond checking that all necessary files are present.</span></span>

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

<span data-ttu-id="2caf9-133">Det här felet visas den hello *MySetup.bat* fil som refereras i hello tjänstmanifestet **SetupEntryPoint** saknas hello kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="2caf9-133">This error shows that hello *MySetup.bat* file referenced in hello service manifest **SetupEntryPoint** is missing from hello code package.</span></span> <span data-ttu-id="2caf9-134">När hello saknad fil har lagts till, skickar hello programmet verifiering:</span><span class="sxs-lookup"><span data-stu-id="2caf9-134">After hello missing file is added, hello application verification passes:</span></span>

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

<span data-ttu-id="2caf9-135">Om programmet har [applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) definierats kan du skicka dem i [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) för rätt validering.</span><span class="sxs-lookup"><span data-stu-id="2caf9-135">If your application has [application parameters](service-fabric-manage-multiple-environment-app-configuration.md) defined, you can pass them in [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) for proper validation.</span></span>

<span data-ttu-id="2caf9-136">Om du vet hello kluster där hello programmet ska distribueras, bör du skicka in hello `ImageStoreConnectionString` parameter.</span><span class="sxs-lookup"><span data-stu-id="2caf9-136">If you know hello cluster where hello application will be deployed, it is recommended you pass in hello `ImageStoreConnectionString` parameter.</span></span> <span data-ttu-id="2caf9-137">I det här fallet verifieras hello paketet också mot tidigare versioner av programmet hello som redan körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2caf9-137">In this case, hello package is also validated against previous versions of hello application that are already running in hello cluster.</span></span> <span data-ttu-id="2caf9-138">Till exempel hello verifiering kan känna av om ett paket med hello samma version men olika innehåll har redan distribuerats.</span><span class="sxs-lookup"><span data-stu-id="2caf9-138">For example, hello validation can detect whether a package with hello same version but different content was already deployed.</span></span>  

<span data-ttu-id="2caf9-139">När programmet hello paketeras på rätt sätt och har klarat valideringen kan utvärdera baserat på hello storlek och hello antalet filer som eventuellt komprimering.</span><span class="sxs-lookup"><span data-stu-id="2caf9-139">Once hello application is packaged correctly and passes validation, evaluate based on hello size and hello number of files if compression is needed.</span></span>

## <a name="compress-a-package"></a><span data-ttu-id="2caf9-140">Komprimera ett paket</span><span class="sxs-lookup"><span data-stu-id="2caf9-140">Compress a package</span></span>
<span data-ttu-id="2caf9-141">Om ett paket är stora eller många filer, kan du komprimera den för snabbare distribution.</span><span class="sxs-lookup"><span data-stu-id="2caf9-141">When a package is large or has many files, you can compress it for faster deployment.</span></span> <span data-ttu-id="2caf9-142">Komprimeringen minskar hello antal filer och hello paketstorleken.</span><span class="sxs-lookup"><span data-stu-id="2caf9-142">Compression reduces hello number of files and hello package size.</span></span>
<span data-ttu-id="2caf9-143">För en komprimerad programpaket [Uploading hello programpaket](service-fabric-deploy-remove-applications.md#upload-the-application-package) kan ta längre jämfört med toouploading hello okomprimerade paketet (särskilt om komprimering tid är inberäknade), men [registrera](service-fabric-deploy-remove-applications.md#register-the-application-package) och [avregistrerades hello programtyp](service-fabric-deploy-remove-applications.md#unregister-an-application-type) är snabbare för ett komprimerade programpaket.</span><span class="sxs-lookup"><span data-stu-id="2caf9-143">For a compressed application package, [Uploading hello application package](service-fabric-deploy-remove-applications.md#upload-the-application-package) may take longer compared toouploading hello uncompressed package (specially if compression time is factored in), but [registering](service-fabric-deploy-remove-applications.md#register-the-application-package) and [un-registering hello application type](service-fabric-deploy-remove-applications.md#unregister-an-application-type) are faster for a compressed application package.</span></span>

<span data-ttu-id="2caf9-144">mekanism för hello-distribution är samma för komprimerade och okomprimerade paket.</span><span class="sxs-lookup"><span data-stu-id="2caf9-144">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="2caf9-145">Om hello paketet har komprimerats, lagras som sådana i hello avbildningsarkivet för klustret och den är okomprimerade på hello nod innan hello program körs.</span><span class="sxs-lookup"><span data-stu-id="2caf9-145">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>
<span data-ttu-id="2caf9-146">hello komprimering ersätter hello giltigt Service Fabric-paket med hello komprimerad version.</span><span class="sxs-lookup"><span data-stu-id="2caf9-146">hello compression replaces hello valid Service Fabric package with hello compressed version.</span></span> <span data-ttu-id="2caf9-147">hello mapp måste tillåta skrivbehörighet.</span><span class="sxs-lookup"><span data-stu-id="2caf9-147">hello folder must allow write permissions.</span></span> <span data-ttu-id="2caf9-148">Kör komprimering på en redan komprimerade paketet ger inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="2caf9-148">Running compression on an already compressed package yields no changes.</span></span>

<span data-ttu-id="2caf9-149">Du kan komprimera ett paket genom att köra Powershell-kommando för hello [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) med `CompressPackage` växla.</span><span class="sxs-lookup"><span data-stu-id="2caf9-149">You can compress a package by running hello Powershell command [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) with `CompressPackage` switch.</span></span> <span data-ttu-id="2caf9-150">Du kan expandera hello paketet med hello samma kommando med `UncompressPackage` växla.</span><span class="sxs-lookup"><span data-stu-id="2caf9-150">You can uncompress hello package with hello same command, using `UncompressPackage` switch.</span></span>

<span data-ttu-id="2caf9-151">hello komprimerar följande kommando hello-paket utan att kopiera den toohello bildarkivet.</span><span class="sxs-lookup"><span data-stu-id="2caf9-151">hello following command compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="2caf9-152">Du kan kopiera en komprimerade paketet tooone eller flera Service Fabric-kluster efter behov med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) utan hello `SkipCopy` flaggan.</span><span class="sxs-lookup"><span data-stu-id="2caf9-152">You can copy a compressed package tooone or more Service Fabric clusters, as needed, using [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) without hello `SkipCopy` flag.</span></span>
<span data-ttu-id="2caf9-153">hello paketet innehåller nu komprimerade filer för hello `code`, `config`, och `data` paket.</span><span class="sxs-lookup"><span data-stu-id="2caf9-153">hello package now includes zipped files for hello `code`, `config`, and `data` packages.</span></span> <span data-ttu-id="2caf9-154">hello programmanifestet och hello service manifest är inte zippade, eftersom de behövs för många interna åtgärder (till exempel dela program namn och version Extraheringen av anslutningstyp för vissa verifieringar-paket).</span><span class="sxs-lookup"><span data-stu-id="2caf9-154">hello application manifest and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span>
<span data-ttu-id="2caf9-155">Komprimerar hello manifesten skulle göra att dessa åtgärder ineffektiv.</span><span class="sxs-lookup"><span data-stu-id="2caf9-155">Zipping hello manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="2caf9-156">Du kan komprimera och kopiera hello paket med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) i ett steg.</span><span class="sxs-lookup"><span data-stu-id="2caf9-156">Alternatively, you can compress and copy hello package with [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in one step.</span></span>
<span data-ttu-id="2caf9-157">Om hello paketet är stort, ange en tillräckligt hög tidsgränsen tooallow för både hello paketet komprimering och hello överför toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="2caf9-157">If hello package is large, provide a high enough timeout tooallow time for both hello package compression and hello upload toohello cluster.</span></span>
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

<span data-ttu-id="2caf9-158">Internt, beräknar Service Fabric kontrollsummor för hello programpaket för verifiering.</span><span class="sxs-lookup"><span data-stu-id="2caf9-158">Internally, Service Fabric computes checksums for hello application packages for validation.</span></span> <span data-ttu-id="2caf9-159">När du använder komprimering beräknas hello kontrollsummor på hello zippade versioner av varje paket.</span><span class="sxs-lookup"><span data-stu-id="2caf9-159">When using compression, hello checksums are computed on hello zipped versions of each package.</span></span>
<span data-ttu-id="2caf9-160">Om du kopierade en okomprimerad version av ditt programpaket och du vill toouse komprimering för hello samma paket, måste du ändra hello versioner av hello `code`, `config`, och `data` paket tooavoid kontrollsummorna överensstämmer inte.</span><span class="sxs-lookup"><span data-stu-id="2caf9-160">If you copied an uncompressed version of your application package, and you want toouse compression for hello same package, you must change hello versions of hello `code`, `config`, and `data` packages tooavoid checksum mismatch.</span></span> <span data-ttu-id="2caf9-161">Om hello-paket är oförändrat, istället för att ändra hello version, kan du använda [diff etablering](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="2caf9-161">If hello packages are unchanged, instead of changing hello version, you can use [diff provisioning](service-fabric-application-upgrade-advanced.md).</span></span> <span data-ttu-id="2caf9-162">Med det här alternativet inte innehåller hello oförändrat paketet i stället referera till den från hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="2caf9-162">With this option, do not include hello unchanged package instead reference it from hello service manifest.</span></span>

<span data-ttu-id="2caf9-163">Om du har överfört en komprimerad version av hello paketet och du vill toouse en okomprimerad paketet, på samma sätt måste du uppdatera hello versioner tooavoid hello felaktig matchning av kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="2caf9-163">Similarly, if you uploaded a compressed version of hello package and you want toouse an uncompressed package, you must update hello versions tooavoid hello checksum mismatch.</span></span>

<span data-ttu-id="2caf9-164">hello paketet är nu paketeras korrekt, verifiera och komprimerade (vid behov), så att den är klar för [distribution](service-fabric-deploy-remove-applications.md) tooone eller flera Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="2caf9-164">hello package is now packaged correctly, validated, and compressed (if needed), so it is ready for [deployment](service-fabric-deploy-remove-applications.md) tooone or more Service Fabric clusters.</span></span>

### <a name="compress-packages-when-deploying-using-visual-studio"></a><span data-ttu-id="2caf9-165">Komprimera paket när du distribuerar med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2caf9-165">Compress packages when deploying using Visual Studio</span></span>
<span data-ttu-id="2caf9-166">Du kan instruera Visual Studio toocompress paket för distribution, genom att lägga till hello `CopyPackageParameters` elementet tooyour Publicera profil och ange hello `CompressPackage` attribut för`true`.</span><span class="sxs-lookup"><span data-stu-id="2caf9-166">You can instruct Visual Studio toocompress packages on deployment, by adding hello `CopyPackageParameters` element tooyour publish profile, and set hello `CompressPackage` attribute too`true`.</span></span>

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a><span data-ttu-id="2caf9-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2caf9-167">Next steps</span></span>
<span data-ttu-id="2caf9-168">[Distribuera och ta bort program] [ 10] beskriver hur toouse PowerShell toomanage programinstanser</span><span class="sxs-lookup"><span data-stu-id="2caf9-168">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances</span></span>

<span data-ttu-id="2caf9-169">[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur tooconfigure parametrar och miljövariabler för olika programinstanser.</span><span class="sxs-lookup"><span data-stu-id="2caf9-169">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="2caf9-170">[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur toorun services under principer toorestrict åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2caf9-170">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
