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
# <a name="deploy-and-remove-applications-using-powershell"></a><span data-ttu-id="29a2a-103">Distribuera och ta bort program med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="29a2a-103">Deploy and remove applications using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29a2a-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29a2a-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="29a2a-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29a2a-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="29a2a-106">FabricClient-API:er</span><span class="sxs-lookup"><span data-stu-id="29a2a-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="29a2a-107">När en [programtyp har paketerats][10], den är klar för distribution till Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="29a2a-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="29a2a-108">Distributionen omfattar hello följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="29a2a-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="29a2a-109">Överför hello programmet paketet toohello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="29a2a-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="29a2a-110">Registrera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="29a2a-110">Register hello application type</span></span>
3. <span data-ttu-id="29a2a-111">Skapa hello programinstansen</span><span class="sxs-lookup"><span data-stu-id="29a2a-111">Create hello application instance</span></span>

<span data-ttu-id="29a2a-112">När ett program distribueras och kör en instans i hello kluster kan du ta bort hello programinstansen och dess programtyp.</span><span class="sxs-lookup"><span data-stu-id="29a2a-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="29a2a-113">toocompletely ta bort ett program från hello kluster omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29a2a-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="29a2a-114">Ta bort (eller ta bort) hello kör programinstansen</span><span class="sxs-lookup"><span data-stu-id="29a2a-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="29a2a-115">Avregistrera hello programtyp om du inte längre behöver</span><span class="sxs-lookup"><span data-stu-id="29a2a-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="29a2a-116">Ta bort hello programpaketet från hello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="29a2a-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="29a2a-117">Om du använder [Visual Studio för att distribuera och felsöka program](service-fabric-publish-app-remote-cluster.md) på klustret för lokal utveckling hello alla ovanstående steg hanteras automatiskt via ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="29a2a-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="29a2a-118">Det här skriptet finns i hello *skript* för hello projektet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="29a2a-119">Den här artikeln innehåller bakgrund på vad skriptet gör så att du kan utföra samma åtgärder utanför Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="29a2a-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="29a2a-120">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="29a2a-120">Connect toohello cluster</span></span>
<span data-ttu-id="29a2a-121">Innan du kör PowerShell-kommandon i den här artikeln måste alltid starta med hjälp av [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="29a2a-121">Before you run any PowerShell commands in this article, always start by using [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric cluster.</span></span> <span data-ttu-id="29a2a-122">tooconnect toohello lokal utveckling klustret, kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="29a2a-122">tooconnect toohello local development cluster, run hello following:</span></span>

```powershell
PS C:\>Connect-ServiceFabricCluster
```

<span data-ttu-id="29a2a-123">Exempel på anslutande tooa fjärrkluster eller kluster som skyddas med Azure Active Directory, X509 certifikat eller Windows Active Directory finns [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="29a2a-123">For examples of connecting tooa remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="upload-hello-application-package"></a><span data-ttu-id="29a2a-124">Överför hello programpaket</span><span class="sxs-lookup"><span data-stu-id="29a2a-124">Upload hello application package</span></span>
<span data-ttu-id="29a2a-125">Ladda upp programpaket för hello placeras på en plats som kan nås av interna Service Fabric-komponenter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-125">Uploading hello application package puts it in a location that's accessible by internal Service Fabric components.</span></span>
<span data-ttu-id="29a2a-126">Om du vill tooverify hello programpaket lokalt använder hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-126">If you want tooverify hello application package locally, use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="29a2a-127">Hej [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot överföringar hello avbildningsarkivet för programmet paketet toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="29a2a-127">hello [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command uploads hello application package toohello cluster image store.</span></span>
<span data-ttu-id="29a2a-128">Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-128">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="29a2a-129">tooimport hello SDK modul, kör:</span><span class="sxs-lookup"><span data-stu-id="29a2a-129">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="29a2a-130">Anta att du skapar och paketerar ett program med namnet *MyApplication* i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="29a2a-130">Suppose you build and package an application named *MyApplication* in Visual Studio 2015.</span></span> <span data-ttu-id="29a2a-131">Som standard är hello programmets typnamn som anges i hello ApplicationManifest.xml ”MyApplicationType”.</span><span class="sxs-lookup"><span data-stu-id="29a2a-131">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="29a2a-132">hello-programpaket som innehåller hello nödvändiga programmanifestet, service manifest och paket i kod-config-data, finns i *C:\Users\<användarnamn\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="29a2a-132">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\<username\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span> 

<span data-ttu-id="29a2a-133">hello visar följande kommando hello innehållet i hello programpaket:</span><span class="sxs-lookup"><span data-stu-id="29a2a-133">hello following command lists hello contents of hello application package:</span></span>

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

<span data-ttu-id="29a2a-134">Om hello programpaket är stort och/eller har många filer, kan du [komprimera](service-fabric-package-apps.md#compress-a-package).</span><span class="sxs-lookup"><span data-stu-id="29a2a-134">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package).</span></span> <span data-ttu-id="29a2a-135">hello komprimeringen minskar hello storlek och hello antal filer.</span><span class="sxs-lookup"><span data-stu-id="29a2a-135">hello compression reduces hello size and hello number of files.</span></span>
<span data-ttu-id="29a2a-136">hello Bieffekten är att registrera och Avregistrerar hello programtyp är snabbare.</span><span class="sxs-lookup"><span data-stu-id="29a2a-136">hello side effect is that registering and un-registering hello application type are faster.</span></span> <span data-ttu-id="29a2a-137">Tid för gå långsammare för närvarande, särskilt om du inkluderar hello tid toocompress hello paketet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-137">Upload time may be slower currently, especially if you include hello time toocompress hello package.</span></span> 

<span data-ttu-id="29a2a-138">toocompress ett paket, Använd hello samma [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommando.</span><span class="sxs-lookup"><span data-stu-id="29a2a-138">toocompress a package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span> <span data-ttu-id="29a2a-139">Komprimering kan utföras separat från överföringen med hjälp av hello `SkipCopy` flagga eller tillsammans med hello överför igen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-139">Compression can be done separate from upload, by using hello `SkipCopy` flag, or together with hello upload operation.</span></span> <span data-ttu-id="29a2a-140">Tillämpa komprimering på komprimerade paketet är ingen op.</span><span class="sxs-lookup"><span data-stu-id="29a2a-140">Applying compression on a compressed package is no-op.</span></span>
<span data-ttu-id="29a2a-141">toouncompress komprimerade paketet, Använd hello samma [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) med hello `UncompressPackage` växla.</span><span class="sxs-lookup"><span data-stu-id="29a2a-141">toouncompress a compressed package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command with hello `UncompressPackage` switch.</span></span>

<span data-ttu-id="29a2a-142">hello komprimerar följande cmdlet hello-paket utan att kopiera den toohello bildarkivet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-142">hello following cmdlet compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="29a2a-143">hello paketet innehåller nu komprimerade filer för hello `Code` och `Config` paket.</span><span class="sxs-lookup"><span data-stu-id="29a2a-143">hello package now includes zipped files for hello `Code` and `Config` packages.</span></span> <span data-ttu-id="29a2a-144">programmet hello och hello service manifest är inte zippade, eftersom de behövs för många interna åtgärder (till exempel dela program namn och version Extraheringen av anslutningstyp för vissa verifieringar-paket).</span><span class="sxs-lookup"><span data-stu-id="29a2a-144">hello application and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span> <span data-ttu-id="29a2a-145">Komprimerar hello manifesten skulle göra att dessa åtgärder ineffektiv.</span><span class="sxs-lookup"><span data-stu-id="29a2a-145">Zipping hello manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="29a2a-146">För stora programpaket tidskrävande hello komprimering.</span><span class="sxs-lookup"><span data-stu-id="29a2a-146">For large application packages, hello compression takes time.</span></span> <span data-ttu-id="29a2a-147">Använda en snabb SSD-enhet för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="29a2a-147">For best results, use a fast SSD drive.</span></span> <span data-ttu-id="29a2a-148">hello komprimering gånger och hello storleken på hello komprimerade paketet också variera beroende på hello paketinnehållet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-148">hello compression times and hello size of hello compressed package also differ based on hello package content.</span></span>
<span data-ttu-id="29a2a-149">Här är till exempel komprimering statistik för vissa paket som visar hello inledande och hello komprimerade paketstorleken med hello komprimering tid.</span><span class="sxs-lookup"><span data-stu-id="29a2a-149">For example, here is compression statistics for some packages, which show hello initial and hello compressed package size, with hello compression time.</span></span>

|<span data-ttu-id="29a2a-150">Ursprunglig storlek (MB)</span><span class="sxs-lookup"><span data-stu-id="29a2a-150">Initial size (MB)</span></span>|<span data-ttu-id="29a2a-151">Antal filer</span><span class="sxs-lookup"><span data-stu-id="29a2a-151">File count</span></span>|<span data-ttu-id="29a2a-152">Tid för komprimering</span><span class="sxs-lookup"><span data-stu-id="29a2a-152">Compression Time</span></span>|<span data-ttu-id="29a2a-153">Komprimerade paketet storlek (MB)</span><span class="sxs-lookup"><span data-stu-id="29a2a-153">Compressed package size (MB)</span></span>|
|----------------:|---------:|---------------:|---------------------------:|
|<span data-ttu-id="29a2a-154">100</span><span class="sxs-lookup"><span data-stu-id="29a2a-154">100</span></span>|<span data-ttu-id="29a2a-155">100</span><span class="sxs-lookup"><span data-stu-id="29a2a-155">100</span></span>|<span data-ttu-id="29a2a-156">00:00:03.3547592</span><span class="sxs-lookup"><span data-stu-id="29a2a-156">00:00:03.3547592</span></span>|<span data-ttu-id="29a2a-157">60</span><span class="sxs-lookup"><span data-stu-id="29a2a-157">60</span></span>|
|<span data-ttu-id="29a2a-158">512</span><span class="sxs-lookup"><span data-stu-id="29a2a-158">512</span></span>|<span data-ttu-id="29a2a-159">100</span><span class="sxs-lookup"><span data-stu-id="29a2a-159">100</span></span>|<span data-ttu-id="29a2a-160">00:00:16.3850303</span><span class="sxs-lookup"><span data-stu-id="29a2a-160">00:00:16.3850303</span></span>|<span data-ttu-id="29a2a-161">307</span><span class="sxs-lookup"><span data-stu-id="29a2a-161">307</span></span>|
|<span data-ttu-id="29a2a-162">1024</span><span class="sxs-lookup"><span data-stu-id="29a2a-162">1024</span></span>|<span data-ttu-id="29a2a-163">500</span><span class="sxs-lookup"><span data-stu-id="29a2a-163">500</span></span>|<span data-ttu-id="29a2a-164">00:00:32.5907950</span><span class="sxs-lookup"><span data-stu-id="29a2a-164">00:00:32.5907950</span></span>|<span data-ttu-id="29a2a-165">615</span><span class="sxs-lookup"><span data-stu-id="29a2a-165">615</span></span>|
|<span data-ttu-id="29a2a-166">2048</span><span class="sxs-lookup"><span data-stu-id="29a2a-166">2048</span></span>|<span data-ttu-id="29a2a-167">1000</span><span class="sxs-lookup"><span data-stu-id="29a2a-167">1000</span></span>|<span data-ttu-id="29a2a-168">00:01:04.3775554</span><span class="sxs-lookup"><span data-stu-id="29a2a-168">00:01:04.3775554</span></span>|<span data-ttu-id="29a2a-169">1231</span><span class="sxs-lookup"><span data-stu-id="29a2a-169">1231</span></span>|
|<span data-ttu-id="29a2a-170">5012</span><span class="sxs-lookup"><span data-stu-id="29a2a-170">5012</span></span>|<span data-ttu-id="29a2a-171">100</span><span class="sxs-lookup"><span data-stu-id="29a2a-171">100</span></span>|<span data-ttu-id="29a2a-172">00:02:45.2951288</span><span class="sxs-lookup"><span data-stu-id="29a2a-172">00:02:45.2951288</span></span>|<span data-ttu-id="29a2a-173">3074</span><span class="sxs-lookup"><span data-stu-id="29a2a-173">3074</span></span>|

<span data-ttu-id="29a2a-174">När ett paket har komprimerats, det kan vara överförda tooone eller flera Service Fabric-kluster efter behov.</span><span class="sxs-lookup"><span data-stu-id="29a2a-174">Once a package is compressed, it can be uploaded tooone or multiple Service Fabric clusters as needed.</span></span> <span data-ttu-id="29a2a-175">mekanism för hello-distribution är samma för komprimerade och okomprimerade paket.</span><span class="sxs-lookup"><span data-stu-id="29a2a-175">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="29a2a-176">Om hello paketet har komprimerats, lagras som sådana i hello avbildningsarkivet för klustret och den är okomprimerade på hello nod innan hello program körs.</span><span class="sxs-lookup"><span data-stu-id="29a2a-176">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>


<span data-ttu-id="29a2a-177">hello överför följande exempel hello paketet toohello image store, till en mapp med namnet ”MyApplicationV1”:</span><span class="sxs-lookup"><span data-stu-id="29a2a-177">hello following example uploads hello package toohello image store, into a folder named "MyApplicationV1":</span></span>

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

<span data-ttu-id="29a2a-178">Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-178">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="29a2a-179">tooimport hello SDK modul, kör:</span><span class="sxs-lookup"><span data-stu-id="29a2a-179">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="29a2a-180">Om du inte anger hello *- ApplicationPackagePathInImageStore* parametern hello programpaket kopieras till hello ”Debug” mapp i hello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-180">If you do not specify hello *-ApplicationPackagePathInImageStore* parameter, hello application package is copied into hello "Debug" folder in hello image store.</span></span>

<span data-ttu-id="29a2a-181">hello tid det tar tooupload ett paket skiljer sig beroende på flera faktorer.</span><span class="sxs-lookup"><span data-stu-id="29a2a-181">hello time it takes tooupload a package differs depending on multiple factors.</span></span> <span data-ttu-id="29a2a-182">Vissa av dessa faktorer är hello antalet filer i hello paketet och hello paketstorleken hello filstorlekar.</span><span class="sxs-lookup"><span data-stu-id="29a2a-182">Some of these factors are hello number of files in hello package, hello package size, and hello file sizes.</span></span> <span data-ttu-id="29a2a-183">hello nätverkshastigheten hello källdatorn och hello Service Fabric-klustret påverkar också hello tid.</span><span class="sxs-lookup"><span data-stu-id="29a2a-183">hello network speed between hello source machine and hello Service Fabric cluster also impacts hello upload time.</span></span> <span data-ttu-id="29a2a-184">Hej Standardtidsgräns för [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) är 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-184">hello default timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minutes.</span></span>
<span data-ttu-id="29a2a-185">Beroende på hello beskrivs faktorer, du kan ha tooincrease hello timeout.</span><span class="sxs-lookup"><span data-stu-id="29a2a-185">Depending on hello described factors, you may have tooincrease hello timeout.</span></span> <span data-ttu-id="29a2a-186">Om du komprimerar hello paketet i hello kopiera anrop måste tooalso anses hello komprimering gång.</span><span class="sxs-lookup"><span data-stu-id="29a2a-186">If you are compressing hello package in hello copy call, you need tooalso consider hello compression time.</span></span>

<span data-ttu-id="29a2a-187">Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-187">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="29a2a-188">Registrera hello programpaket</span><span class="sxs-lookup"><span data-stu-id="29a2a-188">Register hello application package</span></span>
<span data-ttu-id="29a2a-189">hello programtypen och versionen som deklarerats i hello programmanifestet blir tillgängliga för användning när hello programpaketet har registrerats.</span><span class="sxs-lookup"><span data-stu-id="29a2a-189">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="29a2a-190">hello system läser hello paketet på hello föregående steg, verifierar hello paketet, bearbetar hello paketets innehåll och kopierar hello bearbetas paketplatsen tooan internt system.</span><span class="sxs-lookup"><span data-stu-id="29a2a-190">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="29a2a-191">Kör hello [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet tooregister hello programtyp i hello klustret och gör den tillgänglig för distribution:</span><span class="sxs-lookup"><span data-stu-id="29a2a-191">Run hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet tooregister hello application type in hello cluster and make it available for deployment:</span></span>

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

<span data-ttu-id="29a2a-192">”MyApplicationV1” är hello mapp i hello avbildningsarkivet där hello programpaket finns.</span><span class="sxs-lookup"><span data-stu-id="29a2a-192">"MyApplicationV1" is hello folder in hello image store where hello application package is located.</span></span> <span data-ttu-id="29a2a-193">hello programtyp med namnet ”MyApplicationType” och versionen ”1.0.0” (båda finns i programmanifestet hello) är nu registrerad i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="29a2a-193">hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) is now registered in hello cluster.</span></span>

<span data-ttu-id="29a2a-194">Hej [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kommando returnerar bara när hello systemet har registrerades hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="29a2a-194">hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) command returns only after hello system has successfully registered hello application package.</span></span> <span data-ttu-id="29a2a-195">Hur länge registrering tar beror på hello storlek och innehållet i hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="29a2a-195">How long registration takes depends on hello size and contents of hello application package.</span></span> <span data-ttu-id="29a2a-196">Om det behövs, hello **- TimeoutSec** parametern kan vara används toosupply en längre tidsgräns (hello standardvärdet för timeout är 60 sekunder).</span><span class="sxs-lookup"><span data-stu-id="29a2a-196">If needed, hello **-TimeoutSec** parameter can be used toosupply a longer timeout (hello default timeout is 60 seconds).</span></span>

<span data-ttu-id="29a2a-197">Om du har en stor program paketet eller om du upplever tidsgränser använda hello **- asynkrona** parameter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-197">If you have a large application package or if you are experiencing timeouts, use hello **-Async** parameter.</span></span> <span data-ttu-id="29a2a-198">hello kommando returnerar när hello klustret accepterar hello registreringskommandot och hello bearbetningen fortsätter efter behov.</span><span class="sxs-lookup"><span data-stu-id="29a2a-198">hello command returns when hello cluster accepts hello register command, and hello processing continues as needed.</span></span>
<span data-ttu-id="29a2a-199">Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus.</span><span class="sxs-lookup"><span data-stu-id="29a2a-199">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="29a2a-200">Du kan använda det här kommandot toodetermine när hello registreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="29a2a-200">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a><span data-ttu-id="29a2a-201">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="29a2a-201">Create hello application</span></span>
<span data-ttu-id="29a2a-202">Du kan skapa en instans av ett program från en version av programmet som har registrerats med hjälp av hello [ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-202">You can instantiate an application from any application type version that has been registered successfully by using hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="29a2a-203">hello-namnet för varje program måste börja med hello *”fabric”:* schemat och måste vara unikt för varje förekomst av programmet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-203">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance.</span></span> <span data-ttu-id="29a2a-204">Alla standardtjänster som definierats i hello programmanifestet av hello Målprogramstyp skapas också.</span><span class="sxs-lookup"><span data-stu-id="29a2a-204">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
<span data-ttu-id="29a2a-205">Flera instanser av programmet kan skapas för en viss version av typen registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-205">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="29a2a-206">Varje instans av programmet körs i isolering med sin egen arbetskatalog och processen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-206">Each application instance runs in isolation, with its own work directory and process.</span></span>

<span data-ttu-id="29a2a-207">toosee som heter appar och tjänster som körs i hello klustret, kör hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) och [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span><span class="sxs-lookup"><span data-stu-id="29a2a-207">toosee which named apps and services are running in hello cluster, run hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) and [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span></span>

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

## <a name="remove-an-application"></a><span data-ttu-id="29a2a-208">Ta bort ett program</span><span class="sxs-lookup"><span data-stu-id="29a2a-208">Remove an application</span></span>
<span data-ttu-id="29a2a-209">När en instans av programmet inte längre behövs, om du permanent ta bort den namn med hjälp av hello [ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-209">When an application instance is no longer needed, you can permanently remove it by name using hello [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="29a2a-210">[Ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) tar automatiskt bort alla tjänster som tillhör toohello program samt, permanent tar du bort alla tillstånd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="29a2a-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span> 

> [!WARNING]
> <span data-ttu-id="29a2a-211">Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.</span><span class="sxs-lookup"><span data-stu-id="29a2a-211">This operation cannot be reversed, and application state cannot be recovered.</span></span>

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a><span data-ttu-id="29a2a-212">Avregistrera en programtyp</span><span class="sxs-lookup"><span data-stu-id="29a2a-212">Unregister an application type</span></span>
<span data-ttu-id="29a2a-213">När en viss version av en typ av program inte längre behövs, bör du avregistrera hello programtyp med hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-213">When a particular version of an application type is no longer needed, you should unregister hello application type using hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="29a2a-214">Avregistrerar oanvända program typer versioner lagringsutrymme som används av hello image store.</span><span class="sxs-lookup"><span data-stu-id="29a2a-214">Unregistering unused application types releases storage space used by hello image store.</span></span> <span data-ttu-id="29a2a-215">En typ av program kan avregistreras så länge inga program instansieras mot det och inte väntar programmet uppgraderingar hänvisar till den.</span><span class="sxs-lookup"><span data-stu-id="29a2a-215">An application type can be unregistered as long as no applications are instantiated against it and no pending application upgrades are referencing it.</span></span>

<span data-ttu-id="29a2a-216">Kör [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello programtyper som är registrerade i hello klustret:</span><span class="sxs-lookup"><span data-stu-id="29a2a-216">Run [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello application types currently registered in hello cluster:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

<span data-ttu-id="29a2a-217">Kör [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister en specifik typ:</span><span class="sxs-lookup"><span data-stu-id="29a2a-217">Run [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister a specific application type:</span></span>

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="29a2a-218">Ta bort ett programpaket från hello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="29a2a-218">Remove an application package from hello image store</span></span>
<span data-ttu-id="29a2a-219">När ett programpaket inte längre behövs, du kan ta bort den från hello image store toofree upp systemresurser.</span><span class="sxs-lookup"><span data-stu-id="29a2a-219">When an application package is no longer needed, you can delete it from hello image store toofree up system resources.</span></span>

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a><span data-ttu-id="29a2a-220">Felsökning</span><span class="sxs-lookup"><span data-stu-id="29a2a-220">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="29a2a-221">Kopiera ServiceFabricApplicationPackage begär en ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="29a2a-221">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="29a2a-222">hello Service Fabric SDK miljö har redan hello rätt standardvärden ställa in.</span><span class="sxs-lookup"><span data-stu-id="29a2a-222">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="29a2a-223">Men om det behövs, hello ImageStoreConnectionString för alla kommandon bör matcha hello-värde som hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="29a2a-223">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="29a2a-224">Du kan hitta hello ImageStoreConnectionString i hello klustermanifestet hämtades med hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) och Get-ImageStoreConnectionStringFromClusterManifest kommandon:</span><span class="sxs-lookup"><span data-stu-id="29a2a-224">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="29a2a-225">Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-225">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="29a2a-226">tooimport hello SDK modul, kör:</span><span class="sxs-lookup"><span data-stu-id="29a2a-226">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="29a2a-227">Hej ImageStoreConnectionString hittades i klustermanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="29a2a-227">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="29a2a-228">Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29a2a-228">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="29a2a-229">Distribuera stora programpaket</span><span class="sxs-lookup"><span data-stu-id="29a2a-229">Deploy large application package</span></span>
<span data-ttu-id="29a2a-230">Problem: [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) timeout för stora programpaket (ordning GB).</span><span class="sxs-lookup"><span data-stu-id="29a2a-230">Issue: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) times out for a large application package (order of GB).</span></span>
<span data-ttu-id="29a2a-231">Försök:</span><span class="sxs-lookup"><span data-stu-id="29a2a-231">Try:</span></span>
- <span data-ttu-id="29a2a-232">Ange en längre tidsgräns för [kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kommandot med `TimeoutSec` parameter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-232">Specify a larger timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command, with `TimeoutSec` parameter.</span></span> <span data-ttu-id="29a2a-233">Som standard är hello timeout 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-233">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="29a2a-234">Kontrollera hello nätverksanslutning mellan källdatorn och kluster.</span><span class="sxs-lookup"><span data-stu-id="29a2a-234">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="29a2a-235">Om hello är långsam, Överväg att använda en dator med en bättre nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="29a2a-235">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="29a2a-236">Om hello klientdatorn finns i en annan region än hello kluster, Överväg att använda en klientdator i en närmare eller samma region som hello kluster.</span><span class="sxs-lookup"><span data-stu-id="29a2a-236">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="29a2a-237">Kontrollera om du träffa externa begränsning.</span><span class="sxs-lookup"><span data-stu-id="29a2a-237">Check if you are hitting external throttling.</span></span> <span data-ttu-id="29a2a-238">Till exempel när hello avbildningsarkivet är konfigurerade toouse azure storage, kan överför att begränsas.</span><span class="sxs-lookup"><span data-stu-id="29a2a-238">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="29a2a-239">Problem: Överför paketet slutfördes, men [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) timeout. Försök:</span><span class="sxs-lookup"><span data-stu-id="29a2a-239">Issue: Upload package completed successfully, but [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out. Try:</span></span>
- <span data-ttu-id="29a2a-240">[Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-240">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="29a2a-241">hello komprimeringen minskar hello storlek och hello antal filer, vilket i sin tur minskar hello mängden trafik och fungerar som Service Fabric måste utföra.</span><span class="sxs-lookup"><span data-stu-id="29a2a-241">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="29a2a-242">hello överföringen kan ta längre tid (särskilt om du inkluderar hello komprimering tid), men registrera och avregistrera hello programtyp är snabbare.</span><span class="sxs-lookup"><span data-stu-id="29a2a-242">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="29a2a-243">Ange en längre tidsgräns för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) med `TimeoutSec` parameter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-243">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="29a2a-244">Ange `Async` växla för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="29a2a-244">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="29a2a-245">hello kommando returnerar när hello klustret accepterar hello kommandot och hello registrering av programtyp hello fortsätter asynkront.</span><span class="sxs-lookup"><span data-stu-id="29a2a-245">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span> <span data-ttu-id="29a2a-246">Därför finns det inget behov av toospecify en senare tidsgräns i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-246">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="29a2a-247">Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus.</span><span class="sxs-lookup"><span data-stu-id="29a2a-247">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="29a2a-248">Du kan använda det här kommandot toodetermine när hello registreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="29a2a-248">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="29a2a-249">Distribuera programpaket med många filer</span><span class="sxs-lookup"><span data-stu-id="29a2a-249">Deploy application package with many files</span></span>
<span data-ttu-id="29a2a-250">Problem: [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) tidsgränsen uppnås för ett programpaket med många filer (efter tusentalsavgränsare).</span><span class="sxs-lookup"><span data-stu-id="29a2a-250">Issue: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="29a2a-251">Försök:</span><span class="sxs-lookup"><span data-stu-id="29a2a-251">Try:</span></span>
- <span data-ttu-id="29a2a-252">[Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-252">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="29a2a-253">hello komprimeringen minskar hello antal filer.</span><span class="sxs-lookup"><span data-stu-id="29a2a-253">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="29a2a-254">Ange en längre tidsgräns för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) med `TimeoutSec` parameter.</span><span class="sxs-lookup"><span data-stu-id="29a2a-254">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="29a2a-255">Ange `Async` växla för [registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="29a2a-255">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="29a2a-256">hello kommando returnerar när hello klustret accepterar hello kommandot och hello registrering av programtyp hello fortsätter asynkront.</span><span class="sxs-lookup"><span data-stu-id="29a2a-256">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span>
<span data-ttu-id="29a2a-257">Därför finns det inget behov av toospecify en senare tidsgräns i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="29a2a-257">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="29a2a-258">Hej [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) kommando visar alla registrerades typen programversioner och deras registreringsstatus.</span><span class="sxs-lookup"><span data-stu-id="29a2a-258">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="29a2a-259">Du kan använda det här kommandot toodetermine när hello registreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="29a2a-259">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a><span data-ttu-id="29a2a-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29a2a-260">Next steps</span></span>
[<span data-ttu-id="29a2a-261">Uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="29a2a-261">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="29a2a-262">Service Fabric hälsa introduktion</span><span class="sxs-lookup"><span data-stu-id="29a2a-262">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="29a2a-263">Diagnostisera och felsöka en Service Fabric-tjänst</span><span class="sxs-lookup"><span data-stu-id="29a2a-263">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="29a2a-264">Modellen är ett program i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="29a2a-264">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
