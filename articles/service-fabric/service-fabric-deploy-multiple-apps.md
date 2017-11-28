---
title: "aaaDeploy ett Node.js-program som använder MongoDB | Microsoft Docs"
description: "Genomgång av hur toopackage flera körbara filer toodeploy tooan Azure Service Fabric gästkluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell;mikhegn
ms.openlocfilehash: 2775080f0d9d42d6ba15cca911e23067106be26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-guest-executables"></a><span data-ttu-id="c83b3-103">Distribuera flera körbara gäster</span><span class="sxs-lookup"><span data-stu-id="c83b3-103">Deploy multiple guest executables</span></span>
<span data-ttu-id="c83b3-104">Den här artikeln visar hur toopackage och distribuera flera gäst körbara filer tooAzure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c83b3-104">This article shows how toopackage and deploy multiple guest executables tooAzure Service Fabric.</span></span> <span data-ttu-id="c83b3-105">För att skapa och distribuera en enda Service Fabric-paket läsa hur för[distribuera en gäst körbara tooService Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="c83b3-105">For building and deploying a single Service Fabric package read how too[deploy a guest executable tooService Fabric](service-fabric-deploy-existing-app.md).</span></span>

<span data-ttu-id="c83b3-106">När den här genomgången visar hur toodeploy ett program med en Node.js-klientdel som använder MongoDB som datalager hello, du kan använda hello steg tooany program som är beroende av ett annat program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-106">While this walkthrough shows how toodeploy an application with a Node.js front end that uses MongoDB as hello data store, you can apply hello steps tooany application that has dependencies on another application.</span></span>   

<span data-ttu-id="c83b3-107">Du kan använda Visual Studio tooproduce hello med programpaket som innehåller flera gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="c83b3-107">You can use Visual Studio tooproduce hello application package that contains multiple guest executables.</span></span> <span data-ttu-id="c83b3-108">Se [med hjälp av Visual Studio toopackage ett befintligt program](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="c83b3-108">See [Using Visual Studio toopackage an existing application](service-fabric-deploy-existing-app.md).</span></span> <span data-ttu-id="c83b3-109">När du har lagt till hello första gäst körbar fil högerklickar du på hello projektet och välj hello **Lägg till -> Ny Service Fabric-tjänsten** tooadd hello andra gäst körbara projektet toohello lösningen.</span><span class="sxs-lookup"><span data-stu-id="c83b3-109">After you have added hello first guest executable, right click on hello application project and select hello **Add->New Service Fabric service** tooadd hello second guest executable project toohello solution.</span></span> <span data-ttu-id="c83b3-110">Obs: Om du väljer toolink hello källa i hello Visual Studio-projekt, skapa hello Visual Studio-lösning ska kontrollera att programpaketet är upp toodate med ändringar i hello källan.</span><span class="sxs-lookup"><span data-stu-id="c83b3-110">Note: If you choose toolink hello source in hello Visual Studio project, building hello Visual Studio solution, will make sure that your application package is up toodate with changes in hello source.</span></span> 

## <a name="samples"></a><span data-ttu-id="c83b3-111">Exempel</span><span class="sxs-lookup"><span data-stu-id="c83b3-111">Samples</span></span>
* [<span data-ttu-id="c83b3-112">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="c83b3-112">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="c83b3-113">Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="c83b3-113">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a><span data-ttu-id="c83b3-114">Manuellt flera gäst körbara programmet hello i paketet</span><span class="sxs-lookup"><span data-stu-id="c83b3-114">Manually package hello multiple guest executable application</span></span>
<span data-ttu-id="c83b3-115">Alternativt kan du paketera hello gäst körbara manuellt.</span><span class="sxs-lookup"><span data-stu-id="c83b3-115">Alternatively you can manually package hello guest executable.</span></span> <span data-ttu-id="c83b3-116">Hello manuell förpackningen kan den här artikeln använder hello Service Fabric paketering verktyg som finns på [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span><span class="sxs-lookup"><span data-stu-id="c83b3-116">For hello manual packaging, this article uses hello Service Fabric packaging tool, which is available at [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span></span>

### <a name="packaging-hello-nodejs-application"></a><span data-ttu-id="c83b3-117">Paketering hello Node.js-program</span><span class="sxs-lookup"><span data-stu-id="c83b3-117">Packaging hello Node.js application</span></span>
<span data-ttu-id="c83b3-118">Den här artikeln förutsätter att Node.js inte är installerat på hello noder i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="c83b3-118">This article assumes that Node.js is not installed on hello nodes in hello Service Fabric cluster.</span></span> <span data-ttu-id="c83b3-119">Följaktligen måste tooadd Node.exe toohello rotkatalogen för programmet nod innan paketering.</span><span class="sxs-lookup"><span data-stu-id="c83b3-119">As a consequence, you need tooadd Node.exe toohello root directory of your node application before packaging.</span></span> <span data-ttu-id="c83b3-120">hello katalogstrukturen på hello Node.js-program (med snabb webbramverk och Jade mall engine) bör se ut ungefär toohello en nedan:</span><span class="sxs-lookup"><span data-stu-id="c83b3-120">hello directory structure of hello Node.js application (using Express web framework and Jade template engine) should look similar toohello one below:</span></span>

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

<span data-ttu-id="c83b3-121">Som ett nästa steg kan du skapa ett programpaket för hello Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-121">As a next step, you create an application package for hello Node.js application.</span></span> <span data-ttu-id="c83b3-122">hello koden nedan skapar ett Service Fabric-programpaket som innehåller hello Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-122">hello code below creates a Service Fabric application package that contains hello Node.js application.</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

<span data-ttu-id="c83b3-123">Nedan visas en beskrivning av hello-parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="c83b3-123">Below is a description of hello parameters that are being used:</span></span>

* <span data-ttu-id="c83b3-124">**/ source** punkter toohello katalogen för hello-program som ska paketeras.</span><span class="sxs-lookup"><span data-stu-id="c83b3-124">**/source** points toohello directory of hello application that should be packaged.</span></span>
* <span data-ttu-id="c83b3-125">**/ target** definierar hello directory i vilka hello paketet ska skapas.</span><span class="sxs-lookup"><span data-stu-id="c83b3-125">**/target** defines hello directory in which hello package should be created.</span></span> <span data-ttu-id="c83b3-126">Den här katalogen har toobe skiljer sig från hello källkatalogen.</span><span class="sxs-lookup"><span data-stu-id="c83b3-126">This directory has toobe different from hello source directory.</span></span>
* <span data-ttu-id="c83b3-127">**/ AppName** definierar hello programnamn till hello befintliga program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-127">**/appname** defines hello application name of hello existing application.</span></span> <span data-ttu-id="c83b3-128">Det är viktigt toounderstand detta innebär toohello tjänstnamn i hello manifestet och inte toohello Service Fabric programnamn.</span><span class="sxs-lookup"><span data-stu-id="c83b3-128">It's important toounderstand that this translates toohello service name in hello manifest, and not toohello Service Fabric application name.</span></span>
* <span data-ttu-id="c83b3-129">**/exe** definierar hello körbara att Service Fabric ska toolaunch, i det här fallet `node.exe`.</span><span class="sxs-lookup"><span data-stu-id="c83b3-129">**/exe** defines hello executable that Service Fabric is supposed toolaunch, in this case `node.exe`.</span></span>
* <span data-ttu-id="c83b3-130">**/Ma** definierar hello-argument som håller på att använda toolaunch hello körbara.</span><span class="sxs-lookup"><span data-stu-id="c83b3-130">**/ma** defines hello argument that is being used toolaunch hello executable.</span></span> <span data-ttu-id="c83b3-131">Node.js är inte installerad, måste Service Fabric toolaunch hello Node.js webbservern genom att köra `node.exe bin/www`.</span><span class="sxs-lookup"><span data-stu-id="c83b3-131">As Node.js is not installed, Service Fabric needs toolaunch hello Node.js web server by executing `node.exe bin/www`.</span></span>  <span data-ttu-id="c83b3-132">`/ma:'bin/www'`Visar hello paketering verktyget toouse `bin/ma` som hello argument för node.exe.</span><span class="sxs-lookup"><span data-stu-id="c83b3-132">`/ma:'bin/www'` tells hello packaging tool toouse `bin/ma` as hello argument for node.exe.</span></span>
* <span data-ttu-id="c83b3-133">**/ AppType** definierar hello Service Fabric-programmets typnamn.</span><span class="sxs-lookup"><span data-stu-id="c83b3-133">**/AppType** defines hello Service Fabric application type name.</span></span>

<span data-ttu-id="c83b3-134">Om du bläddrar toohello katalog som angavs i hello/Target-parametern kan du se hello verktyget har skapat ett fullt fungerande Service Fabric-paket som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="c83b3-134">If you browse toohello directory that was specified in hello /target parameter, you can see that hello tool has created a fully functioning Service Fabric package as shown below:</span></span>

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
<span data-ttu-id="c83b3-135">hello genererade ServiceManifest.xml har nu ett avsnitt som beskriver hur hello Node.js webbserver bör startas, enligt hello kodfragmentet nedan:</span><span class="sxs-lookup"><span data-stu-id="c83b3-135">hello generated ServiceManifest.xml now has a section that describes how hello Node.js web server should be launched, as shown in hello code snippet below:</span></span>

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
<span data-ttu-id="c83b3-136">I det här exemplet lyssnar hello Node.js webbservern tooport 3000, så du måste tooupdate hello slutpunktsinformation i hello ServiceManifest.xml filen enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="c83b3-136">In this sample, hello Node.js web server listens tooport 3000, so you need tooupdate hello endpoint information in hello ServiceManifest.xml file as shown below.</span></span>   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a><span data-ttu-id="c83b3-137">Paketering hello MongoDB-program</span><span class="sxs-lookup"><span data-stu-id="c83b3-137">Packaging hello MongoDB application</span></span>
<span data-ttu-id="c83b3-138">Nu när du har paketerat hello Node.js-program, kan du gå vidare och paketet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c83b3-138">Now that you have packaged hello Node.js application, you can go ahead and package MongoDB.</span></span> <span data-ttu-id="c83b3-139">Som tidigare nämnts, hello steg som du går igenom nu är inte specifik tooNode.js och MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c83b3-139">As mentioned before, hello steps that you go through now are not specific tooNode.js and MongoDB.</span></span> <span data-ttu-id="c83b3-140">De gäller i själva verket tooall program som är avsedda toobe packade tillsammans som ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-140">In fact, they apply tooall applications that are meant toobe packaged together as one Service Fabric application.</span></span>  

<span data-ttu-id="c83b3-141">toopackage MongoDB, vill du att du paketerar Mongod.exe och Mongo.exe toomake.</span><span class="sxs-lookup"><span data-stu-id="c83b3-141">toopackage MongoDB, you want toomake sure you package Mongod.exe and Mongo.exe.</span></span> <span data-ttu-id="c83b3-142">Båda binärfiler finns i hello `bin` för din MongoDB-installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c83b3-142">Both binaries are located in hello `bin` directory of your MongoDB installation directory.</span></span> <span data-ttu-id="c83b3-143">hello katalogstruktur verkar liknande toohello en nedan.</span><span class="sxs-lookup"><span data-stu-id="c83b3-143">hello directory structure looks similar toohello one below.</span></span>

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
<span data-ttu-id="c83b3-144">Service Fabric behöver toostart MongoDB med ett kommando liknande toohello en nedan, så du behöver toouse hello `/ma` parameter när paketera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c83b3-144">Service Fabric needs toostart MongoDB with a command similar toohello one below, so you need toouse hello `/ma` parameter when packaging MongoDB.</span></span>

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> <span data-ttu-id="c83b3-145">hello bevaras inte data som ett nodfel hello gäller om du placerar hello MongoDB datakatalog på hello lokal katalog för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="c83b3-145">hello data is not being preserved in hello case of a node failure if you put hello MongoDB data directory on hello local directory of hello node.</span></span> <span data-ttu-id="c83b3-146">Du bör använda beständig lagring eller implementera MongoDB replikuppsättning ordning tooprevent förlust av data.</span><span class="sxs-lookup"><span data-stu-id="c83b3-146">You should either use durable storage or implement a MongoDB replica set in order tooprevent data loss.</span></span>  
>
>

<span data-ttu-id="c83b3-147">I PowerShell eller hello-kommandogränssnittet kör vi hello paketering verktyget med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="c83b3-147">In PowerShell or hello command shell, we run hello packaging tool with hello following parameters:</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

<span data-ttu-id="c83b3-148">I programpaket för Service Fabric ordning tooadd MongoDB tooyour, behöver du toomake att hello/Target parametern pekar toohello samma katalog som innehåller redan hello programmanifestet tillsammans med hello Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-148">In order tooadd MongoDB tooyour Service Fabric application package, you need toomake sure that hello /target parameter points toohello same directory that already contains hello application manifest along with hello Node.js application.</span></span> <span data-ttu-id="c83b3-149">Du måste också kontrollera att du använder toomake hello samma ApplicationType namn.</span><span class="sxs-lookup"><span data-stu-id="c83b3-149">You also need toomake sure that you are using hello same ApplicationType name.</span></span>

<span data-ttu-id="c83b3-150">Nu ska vi Bläddra i katalog toohello och undersöka vilka hello-verktyget har skapats.</span><span class="sxs-lookup"><span data-stu-id="c83b3-150">Let's browse toohello directory and examine what hello tool has created.</span></span>

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
<span data-ttu-id="c83b3-151">Som du ser hello verktyget lägga till en ny mapp, MongoDB, toohello katalog som innehåller hello MongoDB-binärfiler.</span><span class="sxs-lookup"><span data-stu-id="c83b3-151">As you can see, hello tool added a new folder, MongoDB, toohello directory that contains hello MongoDB binaries.</span></span> <span data-ttu-id="c83b3-152">Om du öppnar hello `ApplicationManifest.xml` fil, du kan se det hello-paketet innehåller nu både hello Node.js-program och MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c83b3-152">If you open hello `ApplicationManifest.xml` file, you can see that hello package now contains both hello Node.js application and MongoDB.</span></span> <span data-ttu-id="c83b3-153">hello koden nedan visar hello innehållet i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="c83b3-153">hello code below shows hello content of hello application manifest.</span></span>

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-hello-application"></a><span data-ttu-id="c83b3-154">Publishing hello-program</span><span class="sxs-lookup"><span data-stu-id="c83b3-154">Publishing hello application</span></span>
<span data-ttu-id="c83b3-155">hello sista steget är toopublish hello programmet toohello lokala Service Fabric-kluster med hjälp av hello PowerShell-skript nedan:</span><span class="sxs-lookup"><span data-stu-id="c83b3-155">hello last step is toopublish hello application toohello local Service Fabric cluster by using hello PowerShell scripts below:</span></span>

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

<span data-ttu-id="c83b3-156">När programmet hello har publicerade toohello lokala klustret kan du komma åt hello Node.js-program på hello-port som vi angav i hello tjänstmanifestet av hello Node.js-program, till exempel http://localhost: 3000.</span><span class="sxs-lookup"><span data-stu-id="c83b3-156">Once hello application is successfully published toohello local cluster, you can access hello Node.js application on hello port that we have entered in hello service manifest of hello Node.js application--for example http://localhost:3000.</span></span>

<span data-ttu-id="c83b3-157">I den här kursen har du sett hur tooeasily paketet två befintliga program som en Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-157">In this tutorial, you have seen how tooeasily package two existing applications as one Service Fabric application.</span></span> <span data-ttu-id="c83b3-158">Du har också fått lära dig hur toodeploy den tooService Fabric så att den kan dra nytta av några av hello Service Fabric-funktioner, till exempel integrering med hög tillgänglighet och hälsa.</span><span class="sxs-lookup"><span data-stu-id="c83b3-158">You have also learned how toodeploy it tooService Fabric so that it can benefit from some of hello Service Fabric features, such as high availability and health system integration.</span></span>


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a><span data-ttu-id="c83b3-159">Lägga till flera gäst körbara filer tooan befintliga program med hjälp av Yeoman på Linux</span><span class="sxs-lookup"><span data-stu-id="c83b3-159">Adding more guest executables tooan existing application using Yeoman on Linux</span></span>

<span data-ttu-id="c83b3-160">tooadd en annan tooan tjänstprogrammet redan skapats med hjälp av `yo`, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c83b3-160">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span> 
1. <span data-ttu-id="c83b3-161">Ändra toohello rotkatalog till hello befintliga program.</span><span class="sxs-lookup"><span data-stu-id="c83b3-161">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="c83b3-162">Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="c83b3-162">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="c83b3-163">Kör `yo azuresfguest:AddService` och ange hello nödvändiga informationen.</span><span class="sxs-lookup"><span data-stu-id="c83b3-163">Run `yo azuresfguest:AddService` and provide hello necessary details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c83b3-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c83b3-164">Next steps</span></span>
* <span data-ttu-id="c83b3-165">Lär dig mer om hur du distribuerar behållare med [översikt över Service Fabric och behållare](service-fabric-containers-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c83b3-165">Learn about deploying containers with [Service Fabric and containers overview](service-fabric-containers-overview.md)</span></span>
* [<span data-ttu-id="c83b3-166">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="c83b3-166">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="c83b3-167">Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="c83b3-167">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
