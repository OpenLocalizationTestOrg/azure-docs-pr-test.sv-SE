---
title: Paketet en Azure Service Fabric-app | Microsoft Docs
description: Hur du paketerar ett Service Fabric-program innan du distribuerar till ett kluster.
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
ms.openlocfilehash: 486a27d7ca576c8fe1552c02eb24ece6b8bb2ba8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="package-an-application"></a><span data-ttu-id="a1489-103">Paketera ett program</span><span class="sxs-lookup"><span data-stu-id="a1489-103">Package an application</span></span>
<span data-ttu-id="a1489-104">Den här artikeln beskriver hur du paketerar ett Service Fabric-program och förbereda för distribution.</span><span class="sxs-lookup"><span data-stu-id="a1489-104">This article describes how to package a Service Fabric application and make it ready for deployment.</span></span>

## <a name="package-layout"></a><span data-ttu-id="a1489-105">Paketet layout</span><span class="sxs-lookup"><span data-stu-id="a1489-105">Package layout</span></span>
<span data-ttu-id="a1489-106">Applikationsmanifestet, en eller flera service manifest och andra nödvändiga paketfilerna måste ordnas i en viss layout för distribution i ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="a1489-106">The application manifest, one or more service manifests, and other necessary package files must be organized in a specific layout for deployment into a Service Fabric cluster.</span></span> <span data-ttu-id="a1489-107">I exemplet manifesten i den här artikeln måste du ordnas i följande katalogstruktur:</span><span class="sxs-lookup"><span data-stu-id="a1489-107">The example manifests in this article would need to be organized in the following directory structure:</span></span>

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

<span data-ttu-id="a1489-108">Mapparna namn att matcha den **namn** attribut för varje motsvarande element.</span><span class="sxs-lookup"><span data-stu-id="a1489-108">The folders are named to match the **Name** attributes of each corresponding element.</span></span> <span data-ttu-id="a1489-109">Om exempelvis service manifest finns två kod paket med namnen **MyCodeA** och **MyCodeB**, två mappar med samma namn ska innehålla de nödvändiga binärfilerna för varje kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="a1489-109">For example, if the service manifest contained two code packages with the names **MyCodeA** and **MyCodeB**, then two folders with the same names would contain the necessary binaries for each code package.</span></span>

## <a name="use-setupentrypoint"></a><span data-ttu-id="a1489-110">Använd SetupEntryPoint</span><span class="sxs-lookup"><span data-stu-id="a1489-110">Use SetupEntryPoint</span></span>
<span data-ttu-id="a1489-111">Vanliga scenarier där **SetupEntryPoint** när du behöver köra en körbar fil innan du startar tjänsten eller som du behöver utföra en åtgärd med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="a1489-111">Typical scenarios for using **SetupEntryPoint** are when you need to run an executable before the service starts or you need to perform an operation with elevated privileges.</span></span> <span data-ttu-id="a1489-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a1489-112">For example:</span></span>

* <span data-ttu-id="a1489-113">Ställa in och initierar miljövariabler som behöver körbar fil.</span><span class="sxs-lookup"><span data-stu-id="a1489-113">Setting up and initializing environment variables that the service executable needs.</span></span> <span data-ttu-id="a1489-114">Det är inte begränsad till endast körbara filer skrivs via Service Fabric programmeringsmodeller.</span><span class="sxs-lookup"><span data-stu-id="a1489-114">It is not limited to only executables written via the Service Fabric programming models.</span></span> <span data-ttu-id="a1489-115">Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.</span><span class="sxs-lookup"><span data-stu-id="a1489-115">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="a1489-116">Konfigurera åtkomstkontroll genom att installera security-certifikat.</span><span class="sxs-lookup"><span data-stu-id="a1489-116">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="a1489-117">Mer information om hur du konfigurerar den **SetupEntryPoint**, se [konfigurera principen för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="a1489-117">For more information on how to configure the **SetupEntryPoint**, see [Configure the policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<a id="Package-App"></a>
## <a name="configure"></a><span data-ttu-id="a1489-118">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="a1489-118">Configure</span></span>
### <a name="build-a-package-by-using-visual-studio"></a><span data-ttu-id="a1489-119">Skapa ett paket med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1489-119">Build a package by using Visual Studio</span></span>
<span data-ttu-id="a1489-120">Om du använder Visual Studio 2015 för att skapa programmet kan använda du kommandot paketet för att automatiskt skapa ett paket som matchar layout som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="a1489-120">If you use Visual Studio 2015 to create your application, you can use the Package command to automatically create a package that matches the layout described above.</span></span>

<span data-ttu-id="a1489-121">Om du vill skapa ett paket, högerklicka på programmet projektet i Solution Explorer och välj kommandot paketera enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="a1489-121">To create a package, right-click the application project in Solution Explorer and choose the Package command, as shown below:</span></span>

![Ett program med Visual Studio-paket][vs-package-command]

<span data-ttu-id="a1489-123">När paketering är klar, kan du hitta platsen för paketets i den **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="a1489-123">When packaging is complete, you can find the location of the package in the **Output** window.</span></span> <span data-ttu-id="a1489-124">Steget paketering sker automatiskt när du distribuerar eller felsöker programmet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1489-124">The packaging step occurs automatically when you deploy or debug your application in Visual Studio.</span></span>

### <a name="build-a-package-by-command-line"></a><span data-ttu-id="a1489-125">Skapa ett paket av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="a1489-125">Build a package by command line</span></span>
<span data-ttu-id="a1489-126">Det är också möjligt att programmässigt packa upp appen med `msbuild.exe`.</span><span class="sxs-lookup"><span data-stu-id="a1489-126">It is also possible to programmatically package up your application using `msbuild.exe`.</span></span> <span data-ttu-id="a1489-127">Under huven körs Visual Studio den så att utdata är samma.</span><span class="sxs-lookup"><span data-stu-id="a1489-127">Under the hood, Visual Studio is running it so the output is same.</span></span>

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a><span data-ttu-id="a1489-128">Testa paketet</span><span class="sxs-lookup"><span data-stu-id="a1489-128">Test the package</span></span>
<span data-ttu-id="a1489-129">Du kan verifiera paketet strukturen lokalt via PowerShell med hjälp av den [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) kommando.</span><span class="sxs-lookup"><span data-stu-id="a1489-129">You can verify the package structure locally through PowerShell by using the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span>
<span data-ttu-id="a1489-130">Detta kommando kontrollerar manifestet parsning problem och kontrollera alla referenser.</span><span class="sxs-lookup"><span data-stu-id="a1489-130">This command checks for manifest parsing issues and verify all references.</span></span> <span data-ttu-id="a1489-131">Detta kommando kontrollerar endast strukturella riktighet kataloger och filer i paketet.</span><span class="sxs-lookup"><span data-stu-id="a1489-131">This command only verifies the structural correctness of the directories and files in the package.</span></span>
<span data-ttu-id="a1489-132">Går det inte verifiera några paketinnehållet kod eller data utöver att kontrollera att alla nödvändiga filer finns.</span><span class="sxs-lookup"><span data-stu-id="a1489-132">It doesn't verify any of the code or data package contents beyond checking that all necessary files are present.</span></span>

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

<span data-ttu-id="a1489-133">Det här felet visas som den *MySetup.bat* fil som refereras i service manifest **SetupEntryPoint** kodpaketet saknas.</span><span class="sxs-lookup"><span data-stu-id="a1489-133">This error shows that the *MySetup.bat* file referenced in the service manifest **SetupEntryPoint** is missing from the code package.</span></span> <span data-ttu-id="a1489-134">När filen saknas läggs skickar programmet-verifiering:</span><span class="sxs-lookup"><span data-stu-id="a1489-134">After the missing file is added, the application verification passes:</span></span>

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

<span data-ttu-id="a1489-135">Om programmet har [applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) definierats kan du skicka dem i [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) för rätt validering.</span><span class="sxs-lookup"><span data-stu-id="a1489-135">If your application has [application parameters](service-fabric-manage-multiple-environment-app-configuration.md) defined, you can pass them in [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) for proper validation.</span></span>

<span data-ttu-id="a1489-136">Om du vet klustret där programmet ska distribueras, bör du skicka in den `ImageStoreConnectionString` parameter.</span><span class="sxs-lookup"><span data-stu-id="a1489-136">If you know the cluster where the application will be deployed, it is recommended you pass in the `ImageStoreConnectionString` parameter.</span></span> <span data-ttu-id="a1489-137">I det här fallet kontrolleras också paketet gentemot tidigare versioner av programmet som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="a1489-137">In this case, the package is also validated against previous versions of the application that are already running in the cluster.</span></span> <span data-ttu-id="a1489-138">Till exempel valideringen kan känna av om ett paket med samma version men olika innehåll har redan distribuerats.</span><span class="sxs-lookup"><span data-stu-id="a1489-138">For example, the validation can detect whether a package with the same version but different content was already deployed.</span></span>  

<span data-ttu-id="a1489-139">När programmet levereras på rätt sätt och har klarat valideringen kan utvärdera baserat på storleken och antalet filer som eventuellt komprimering.</span><span class="sxs-lookup"><span data-stu-id="a1489-139">Once the application is packaged correctly and passes validation, evaluate based on the size and the number of files if compression is needed.</span></span>

## <a name="compress-a-package"></a><span data-ttu-id="a1489-140">Komprimera ett paket</span><span class="sxs-lookup"><span data-stu-id="a1489-140">Compress a package</span></span>
<span data-ttu-id="a1489-141">Om ett paket är stora eller många filer, kan du komprimera den för snabbare distribution.</span><span class="sxs-lookup"><span data-stu-id="a1489-141">When a package is large or has many files, you can compress it for faster deployment.</span></span> <span data-ttu-id="a1489-142">Komprimeringen minskar antalet filer och paketstorleken.</span><span class="sxs-lookup"><span data-stu-id="a1489-142">Compression reduces the number of files and the package size.</span></span>
<span data-ttu-id="a1489-143">För en komprimerad programpaket [ladda upp programpaketet](service-fabric-deploy-remove-applications.md#upload-the-application-package) kan ta längre tid jämfört med överför okomprimerade paketet (särskilt om komprimering tid är inberäknade), men [registrerar](service-fabric-deploy-remove-applications.md#register-the-application-package) och [avregistrerades programtypen](service-fabric-deploy-remove-applications.md#unregister-an-application-type) är snabbare för ett komprimerade programpaket.</span><span class="sxs-lookup"><span data-stu-id="a1489-143">For a compressed application package, [Uploading the application package](service-fabric-deploy-remove-applications.md#upload-the-application-package) may take longer compared to uploading the uncompressed package (specially if compression time is factored in), but [registering](service-fabric-deploy-remove-applications.md#register-the-application-package) and [un-registering the application type](service-fabric-deploy-remove-applications.md#unregister-an-application-type) are faster for a compressed application package.</span></span>

<span data-ttu-id="a1489-144">Mekanism för distribution är samma för komprimerade och okomprimerade paket.</span><span class="sxs-lookup"><span data-stu-id="a1489-144">The deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="a1489-145">Om paketet har komprimerats, lagras som sådana i klustret image store och den är okomprimerade på noden innan programmet körs.</span><span class="sxs-lookup"><span data-stu-id="a1489-145">If the package is compressed, it is stored as such in the cluster image store and it's uncompressed on the node before the application is run.</span></span>
<span data-ttu-id="a1489-146">Komprimeringen ersätter giltigt Service Fabric-paket med den komprimerade versionen.</span><span class="sxs-lookup"><span data-stu-id="a1489-146">The compression replaces the valid Service Fabric package with the compressed version.</span></span> <span data-ttu-id="a1489-147">Mappen måste tillåta skrivbehörighet.</span><span class="sxs-lookup"><span data-stu-id="a1489-147">The folder must allow write permissions.</span></span> <span data-ttu-id="a1489-148">Kör komprimering på en redan komprimerade paketet ger inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="a1489-148">Running compression on an already compressed package yields no changes.</span></span>

<span data-ttu-id="a1489-149">Du kan komprimera ett paket genom att köra Powershell-kommando [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) med `CompressPackage` växla.</span><span class="sxs-lookup"><span data-stu-id="a1489-149">You can compress a package by running the Powershell command [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) with `CompressPackage` switch.</span></span> <span data-ttu-id="a1489-150">Du kan expandera paketet med samma kommando med `UncompressPackage` växla.</span><span class="sxs-lookup"><span data-stu-id="a1489-150">You can uncompress the package with the same command, using `UncompressPackage` switch.</span></span>

<span data-ttu-id="a1489-151">Följande kommando komprimerar paketet utan att kopiera den till image store.</span><span class="sxs-lookup"><span data-stu-id="a1489-151">The following command compresses the package without copying it to the image store.</span></span> <span data-ttu-id="a1489-152">Du kan kopiera komprimerade paketet till en eller flera Service Fabric-kluster, efter behov, med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) utan den `SkipCopy` flaggan.</span><span class="sxs-lookup"><span data-stu-id="a1489-152">You can copy a compressed package to one or more Service Fabric clusters, as needed, using [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) without the `SkipCopy` flag.</span></span>
<span data-ttu-id="a1489-153">Paketet innehåller nu komprimerade filer för den `code`, `config`, och `data` paket.</span><span class="sxs-lookup"><span data-stu-id="a1489-153">The package now includes zipped files for the `code`, `config`, and `data` packages.</span></span> <span data-ttu-id="a1489-154">Applikationsmanifestet och service manifest är inte zippade, eftersom de behövs för många interna åtgärder (t.ex. paketet fildelning, program namn och version Extraheringen av anslutningstyp för vissa verifieringar).</span><span class="sxs-lookup"><span data-stu-id="a1489-154">The application manifest and the service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span>
<span data-ttu-id="a1489-155">Komprimerar manifesten gör dessa åtgärder ineffektiv.</span><span class="sxs-lookup"><span data-stu-id="a1489-155">Zipping the manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="a1489-156">Du kan komprimera och kopiera paketet med [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) i ett steg.</span><span class="sxs-lookup"><span data-stu-id="a1489-156">Alternatively, you can compress and copy the package with [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in one step.</span></span>
<span data-ttu-id="a1489-157">Om paketet är stort, ange en tillräckligt hög tidsgräns för både paketet komprimering och överför till klustret.</span><span class="sxs-lookup"><span data-stu-id="a1489-157">If the package is large, provide a high enough timeout to allow time for both the package compression and the upload to the cluster.</span></span>
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

<span data-ttu-id="a1489-158">Internt, beräknar Service Fabric kontrollsummor för programpaket för verifiering.</span><span class="sxs-lookup"><span data-stu-id="a1489-158">Internally, Service Fabric computes checksums for the application packages for validation.</span></span> <span data-ttu-id="a1489-159">När du använder komprimering beräknas av kontrollsummor i komprimerade versioner av varje paket.</span><span class="sxs-lookup"><span data-stu-id="a1489-159">When using compression, the checksums are computed on the zipped versions of each package.</span></span>
<span data-ttu-id="a1489-160">Om du kopierade en okomprimerad version av ditt programpaket och du vill använda komprimering för samma paket, måste du ändra versionerna av den `code`, `config`, och `data` paket för att undvika felaktig matchning av kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="a1489-160">If you copied an uncompressed version of your application package, and you want to use compression for the same package, you must change the versions of the `code`, `config`, and `data` packages to avoid checksum mismatch.</span></span> <span data-ttu-id="a1489-161">Om paketen är desamma, istället för att ändra versionen, kan du använda [diff etablering](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="a1489-161">If the packages are unchanged, instead of changing the version, you can use [diff provisioning](service-fabric-application-upgrade-advanced.md).</span></span> <span data-ttu-id="a1489-162">Med det här alternativet inte är oförändrad paketet i stället referera till den från service manifest.</span><span class="sxs-lookup"><span data-stu-id="a1489-162">With this option, do not include the unchanged package instead reference it from the service manifest.</span></span>

<span data-ttu-id="a1489-163">Om du har överfört en komprimerad version av paketet och du vill använda en okomprimerad paketet, på samma sätt måste du uppdatera versioner för att undvika den felaktig matchning av kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="a1489-163">Similarly, if you uploaded a compressed version of the package and you want to use an uncompressed package, you must update the versions to avoid the checksum mismatch.</span></span>

<span data-ttu-id="a1489-164">Paketet är nu paketeras korrekt verifieras och komprimerade (vid behov), så att den är klar för [distribution](service-fabric-deploy-remove-applications.md) till en eller flera Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="a1489-164">The package is now packaged correctly, validated, and compressed (if needed), so it is ready for [deployment](service-fabric-deploy-remove-applications.md) to one or more Service Fabric clusters.</span></span>

### <a name="compress-packages-when-deploying-using-visual-studio"></a><span data-ttu-id="a1489-165">Komprimera paket när du distribuerar med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1489-165">Compress packages when deploying using Visual Studio</span></span>
<span data-ttu-id="a1489-166">Du kan instruera Visual Studio för att komprimera paket för distribution, genom att lägga till den `CopyPackageParameters` elementet så att din profil och ange den `CompressPackage` attributet `true`.</span><span class="sxs-lookup"><span data-stu-id="a1489-166">You can instruct Visual Studio to compress packages on deployment, by adding the `CopyPackageParameters` element to your publish profile, and set the `CompressPackage` attribute to `true`.</span></span>

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a><span data-ttu-id="a1489-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1489-167">Next steps</span></span>
<span data-ttu-id="a1489-168">[Distribuera och ta bort program] [ 10] beskriver hur du använder PowerShell för att hantera programinstanser</span><span class="sxs-lookup"><span data-stu-id="a1489-168">[Deploy and remove applications][10] describes how to use PowerShell to manage application instances</span></span>

<span data-ttu-id="a1489-169">[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur du konfigurerar parametrar och miljövariabler för olika programinstanser.</span><span class="sxs-lookup"><span data-stu-id="a1489-169">[Managing application parameters for multiple environments][11] describes how to configure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="a1489-170">[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur du kör tjänster under säkerhetsprinciper för att begränsa åtkomsten.</span><span class="sxs-lookup"><span data-stu-id="a1489-170">[Configure security policies for your application][12] describes how to run services under security policies to restrict access.</span></span>

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
