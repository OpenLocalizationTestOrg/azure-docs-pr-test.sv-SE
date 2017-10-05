---
title: "Distribuera ett Node.js-program som använder MongoDB | Microsoft Docs"
description: "Genomgång av hur du paketerar flera gäst körbara filer ska distribueras till ett Azure Service Fabric-kluster"
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
ms.openlocfilehash: b71723034e5f663986c49481072bfd6779d3d57b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-guest-executables"></a><span data-ttu-id="18731-103">Distribuera flera körbara gäster</span><span class="sxs-lookup"><span data-stu-id="18731-103">Deploy multiple guest executables</span></span>
<span data-ttu-id="18731-104">Den här artikeln visar hur du paketera och distribuera flera gäst körbara filer till Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="18731-104">This article shows how to package and deploy multiple guest executables to Azure Service Fabric.</span></span> <span data-ttu-id="18731-105">Skapa och distribuera en enda Service Fabric-paket finns att [Distribuera gäst körbara till Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="18731-105">For building and deploying a single Service Fabric package read how to [deploy a guest executable to Service Fabric](service-fabric-deploy-existing-app.md).</span></span>

<span data-ttu-id="18731-106">Den här genomgången visar hur du distribuerar ett program med en Node.js-klientdel som använder MongoDB som datalager, men du kan använda stegen för att alla program som är beroende av ett annat program.</span><span class="sxs-lookup"><span data-stu-id="18731-106">While this walkthrough shows how to deploy an application with a Node.js front end that uses MongoDB as the data store, you can apply the steps to any application that has dependencies on another application.</span></span>   

<span data-ttu-id="18731-107">Du kan använda Visual Studio för att skapa programpaket som innehåller flera gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="18731-107">You can use Visual Studio to produce the application package that contains multiple guest executables.</span></span> <span data-ttu-id="18731-108">Se [med hjälp av Visual Studio för att paketera ett befintligt program](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="18731-108">See [Using Visual Studio to package an existing application](service-fabric-deploy-existing-app.md).</span></span> <span data-ttu-id="18731-109">När du har lagt till den första körbara fil som gäst, högerklicka på projektet för konsolprogrammet och välj den **Lägg till -> Ny Service Fabric-tjänsten** att lägga till det andra körbara gäst-projektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="18731-109">After you have added the first guest executable, right click on the application project and select the **Add->New Service Fabric service** to add the second guest executable project to the solution.</span></span> <span data-ttu-id="18731-110">Obs: Om du väljer att länka källa i Visual Studio-projekt skapar Visual Studio-lösning ska se till att ditt programpaket är uppdaterade med ändringar i källan.</span><span class="sxs-lookup"><span data-stu-id="18731-110">Note: If you choose to link the source in the Visual Studio project, building the Visual Studio solution, will make sure that your application package is up to date with changes in the source.</span></span> 

## <a name="samples"></a><span data-ttu-id="18731-111">Exempel</span><span class="sxs-lookup"><span data-stu-id="18731-111">Samples</span></span>
* [<span data-ttu-id="18731-112">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="18731-112">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="18731-113">Exempel på två gäst körbara filer (C# och nodejs) kommunicerar via Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="18731-113">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a><span data-ttu-id="18731-114">Paketet flera gäst körbara programmet manuellt</span><span class="sxs-lookup"><span data-stu-id="18731-114">Manually package the multiple guest executable application</span></span>
<span data-ttu-id="18731-115">Alternativt kan du paketera gästen körbara manuellt.</span><span class="sxs-lookup"><span data-stu-id="18731-115">Alternatively you can manually package the guest executable.</span></span> <span data-ttu-id="18731-116">Manuell förpackningen kan den här artikeln använder verktyget Service Fabric-paket, som finns på [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span><span class="sxs-lookup"><span data-stu-id="18731-116">For the manual packaging, this article uses the Service Fabric packaging tool, which is available at [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span></span>

### <a name="packaging-the-nodejs-application"></a><span data-ttu-id="18731-117">Paketera Node.js-program</span><span class="sxs-lookup"><span data-stu-id="18731-117">Packaging the Node.js application</span></span>
<span data-ttu-id="18731-118">Den här artikeln förutsätter att Node.js inte är installerat på noderna i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="18731-118">This article assumes that Node.js is not installed on the nodes in the Service Fabric cluster.</span></span> <span data-ttu-id="18731-119">Följaktligen behöver du lägga till Node.exe i rotkatalogen för programmet nod innan paketering.</span><span class="sxs-lookup"><span data-stu-id="18731-119">As a consequence, you need to add Node.exe to the root directory of your node application before packaging.</span></span> <span data-ttu-id="18731-120">Katalogstrukturen för Node.js-program (med snabb webbramverk och Jade mall engine) bör likna exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="18731-120">The directory structure of the Node.js application (using Express web framework and Jade template engine) should look similar to the one below:</span></span>

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

<span data-ttu-id="18731-121">Som ett nästa steg kan skapa du ett programpaket för Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="18731-121">As a next step, you create an application package for the Node.js application.</span></span> <span data-ttu-id="18731-122">Koden nedan skapar ett Service Fabric-programpaket som innehåller Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="18731-122">The code below creates a Service Fabric application package that contains the Node.js application.</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

<span data-ttu-id="18731-123">Nedan följer en beskrivning av de parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="18731-123">Below is a description of the parameters that are being used:</span></span>

* <span data-ttu-id="18731-124">**/ source** pekar på katalogen för det program som ska paketeras.</span><span class="sxs-lookup"><span data-stu-id="18731-124">**/source** points to the directory of the application that should be packaged.</span></span>
* <span data-ttu-id="18731-125">**/ target** definierar den katalog där paketet ska skapas.</span><span class="sxs-lookup"><span data-stu-id="18731-125">**/target** defines the directory in which the package should be created.</span></span> <span data-ttu-id="18731-126">Den här katalogen måste vara olika från källkatalogen.</span><span class="sxs-lookup"><span data-stu-id="18731-126">This directory has to be different from the source directory.</span></span>
* <span data-ttu-id="18731-127">**/ AppName** definierar namnet på programmet i det befintliga programmet.</span><span class="sxs-lookup"><span data-stu-id="18731-127">**/appname** defines the application name of the existing application.</span></span> <span data-ttu-id="18731-128">Det är viktigt att du förstår att detta innebär att tjänstnamnet i manifestet och inte namnet på Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="18731-128">It's important to understand that this translates to the service name in the manifest, and not to the Service Fabric application name.</span></span>
* <span data-ttu-id="18731-129">**/exe** definierar den körbara filen som Service Fabric ska starta i det här fallet `node.exe`.</span><span class="sxs-lookup"><span data-stu-id="18731-129">**/exe** defines the executable that Service Fabric is supposed to launch, in this case `node.exe`.</span></span>
* <span data-ttu-id="18731-130">**/Ma** definierar argumentet som används för att starta den körbara filen.</span><span class="sxs-lookup"><span data-stu-id="18731-130">**/ma** defines the argument that is being used to launch the executable.</span></span> <span data-ttu-id="18731-131">Node.js är inte installerad, Service Fabric behöver starta webbservern Node.js genom att köra `node.exe bin/www`.</span><span class="sxs-lookup"><span data-stu-id="18731-131">As Node.js is not installed, Service Fabric needs to launch the Node.js web server by executing `node.exe bin/www`.</span></span>  <span data-ttu-id="18731-132">`/ma:'bin/www'`Anger verktyget paketering för att använda `bin/ma` som argument för node.exe.</span><span class="sxs-lookup"><span data-stu-id="18731-132">`/ma:'bin/www'` tells the packaging tool to use `bin/ma` as the argument for node.exe.</span></span>
* <span data-ttu-id="18731-133">**/ AppType** definierar Service Fabric-programmets typnamn.</span><span class="sxs-lookup"><span data-stu-id="18731-133">**/AppType** defines the Service Fabric application type name.</span></span>

<span data-ttu-id="18731-134">Om du bläddrar till den katalog som angavs i parametern/Target kan du se att verktyget har skapat ett fullt fungerande Service Fabric-paket som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="18731-134">If you browse to the directory that was specified in the /target parameter, you can see that the tool has created a fully functioning Service Fabric package as shown below:</span></span>

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
<span data-ttu-id="18731-135">Den genererade ServiceManifest.xml har nu ett avsnitt som beskriver hur Node.js-webbserver bör startas, som visas i kodfragmentet nedan:</span><span class="sxs-lookup"><span data-stu-id="18731-135">The generated ServiceManifest.xml now has a section that describes how the Node.js web server should be launched, as shown in the code snippet below:</span></span>

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
<span data-ttu-id="18731-136">I det här exemplet lyssnar webbservern Node.js på port 3000, så du behöver uppdatera slutpunktsinformationen i filen ServiceManifest.xml enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="18731-136">In this sample, the Node.js web server listens to port 3000, so you need to update the endpoint information in the ServiceManifest.xml file as shown below.</span></span>   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a><span data-ttu-id="18731-137">Paketera programmet MongoDB</span><span class="sxs-lookup"><span data-stu-id="18731-137">Packaging the MongoDB application</span></span>
<span data-ttu-id="18731-138">Nu när du har paketerat Node.js-program, kan du gå vidare och paketet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18731-138">Now that you have packaged the Node.js application, you can go ahead and package MongoDB.</span></span> <span data-ttu-id="18731-139">Som tidigare nämnts, är de steg som du går igenom nu inte specifika för Node.js och MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18731-139">As mentioned before, the steps that you go through now are not specific to Node.js and MongoDB.</span></span> <span data-ttu-id="18731-140">Faktum är gäller de för alla program som är avsedda att vara packade tillsammans som ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="18731-140">In fact, they apply to all applications that are meant to be packaged together as one Service Fabric application.</span></span>  

<span data-ttu-id="18731-141">Om du vill paketera MongoDB, som du vill kontrollera att du paketerar Mongod.exe och Mongo.exe.</span><span class="sxs-lookup"><span data-stu-id="18731-141">To package MongoDB, you want to make sure you package Mongod.exe and Mongo.exe.</span></span> <span data-ttu-id="18731-142">Båda binärfiler finns i den `bin` för din MongoDB-installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="18731-142">Both binaries are located in the `bin` directory of your MongoDB installation directory.</span></span> <span data-ttu-id="18731-143">Katalogstrukturen ser ut som nedan.</span><span class="sxs-lookup"><span data-stu-id="18731-143">The directory structure looks similar to the one below.</span></span>

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
<span data-ttu-id="18731-144">Service Fabric måste börja MongoDB med ett kommando som liknar den nedan, så du behöver använda den `/ma` parameter när paketera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18731-144">Service Fabric needs to start MongoDB with a command similar to the one below, so you need to use the `/ma` parameter when packaging MongoDB.</span></span>

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> <span data-ttu-id="18731-145">Data som sparas inte när det gäller ett nodfel om du placerar katalogen MongoDB data på den lokala katalogen för noden.</span><span class="sxs-lookup"><span data-stu-id="18731-145">The data is not being preserved in the case of a node failure if you put the MongoDB data directory on the local directory of the node.</span></span> <span data-ttu-id="18731-146">Du bör använda beständig lagring eller implementera replikuppsättning MongoDB för att förhindra dataförlust.</span><span class="sxs-lookup"><span data-stu-id="18731-146">You should either use durable storage or implement a MongoDB replica set in order to prevent data loss.</span></span>  
>
>

<span data-ttu-id="18731-147">I PowerShell eller Kommandotolken kör vi verktyget paketering med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="18731-147">In PowerShell or the command shell, we run the packaging tool with the following parameters:</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

<span data-ttu-id="18731-148">För att lägga till MongoDB till ditt Service Fabric-programpaket måste du se till att/Target-parametern pekar på den katalog som innehåller programmet redan manifest tillsammans med Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="18731-148">In order to add MongoDB to your Service Fabric application package, you need to make sure that the /target parameter points to the same directory that already contains the application manifest along with the Node.js application.</span></span> <span data-ttu-id="18731-149">Du måste också se till att du använder samma ApplicationType namn.</span><span class="sxs-lookup"><span data-stu-id="18731-149">You also need to make sure that you are using the same ApplicationType name.</span></span>

<span data-ttu-id="18731-150">Nu ska vi Bläddra till mappen och undersöka vad verktyget har skapats.</span><span class="sxs-lookup"><span data-stu-id="18731-150">Let's browse to the directory and examine what the tool has created.</span></span>

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
<span data-ttu-id="18731-151">Verktyget till en ny mapp, MongoDB, i den katalog som innehåller MongoDB-binärfiler som du kan se.</span><span class="sxs-lookup"><span data-stu-id="18731-151">As you can see, the tool added a new folder, MongoDB, to the directory that contains the MongoDB binaries.</span></span> <span data-ttu-id="18731-152">Om du öppnar den `ApplicationManifest.xml` filen ser du att paketet innehåller nu både Node.js-program och MongoDB.</span><span class="sxs-lookup"><span data-stu-id="18731-152">If you open the `ApplicationManifest.xml` file, you can see that the package now contains both the Node.js application and MongoDB.</span></span> <span data-ttu-id="18731-153">Koden nedan visar innehållet i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="18731-153">The code below shows the content of the application manifest.</span></span>

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

### <a name="publishing-the-application"></a><span data-ttu-id="18731-154">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="18731-154">Publishing the application</span></span>
<span data-ttu-id="18731-155">Det sista steget är att publicera program till det lokala Service Fabric-klustret med hjälp av PowerShell-skript nedan:</span><span class="sxs-lookup"><span data-stu-id="18731-155">The last step is to publish the application to the local Service Fabric cluster by using the PowerShell scripts below:</span></span>

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

<span data-ttu-id="18731-156">När programmet har publicerats till det lokala klustret kan du komma åt Node.js-program på den port som vi angav i service manifest för Node.js-program, till exempel http://localhost: 3000.</span><span class="sxs-lookup"><span data-stu-id="18731-156">Once the application is successfully published to the local cluster, you can access the Node.js application on the port that we have entered in the service manifest of the Node.js application--for example http://localhost:3000.</span></span>

<span data-ttu-id="18731-157">I den här kursen har du fått veta hur du enkelt paketera två befintliga program som en Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="18731-157">In this tutorial, you have seen how to easily package two existing applications as one Service Fabric application.</span></span> <span data-ttu-id="18731-158">Du har lärt dig hur du distribuerar den till Service Fabric så att den kan dra nytta av några av Service Fabric-funktioner, till exempel hög tillgänglighet och integrering med hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="18731-158">You have also learned how to deploy it to Service Fabric so that it can benefit from some of the Service Fabric features, such as high availability and health system integration.</span></span>


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a><span data-ttu-id="18731-159">Att lägga till flera gäst körbara filer i ett befintligt program med hjälp av Yeoman på Linux</span><span class="sxs-lookup"><span data-stu-id="18731-159">Adding more guest executables to an existing application using Yeoman on Linux</span></span>

<span data-ttu-id="18731-160">Om du vill lägga till en till tjänst till ett program som redan har skapats med hjälp av `yo` utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="18731-160">To add another service to an application already created using `yo`, perform the following steps:</span></span> 
1. <span data-ttu-id="18731-161">Ändra katalogen till roten för det befintliga programmet.</span><span class="sxs-lookup"><span data-stu-id="18731-161">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="18731-162">Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="18731-162">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="18731-163">Kör `yo azuresfguest:AddService` och ange den nödvändiga informationen.</span><span class="sxs-lookup"><span data-stu-id="18731-163">Run `yo azuresfguest:AddService` and provide the necessary details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18731-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18731-164">Next steps</span></span>
* <span data-ttu-id="18731-165">Lär dig mer om hur du distribuerar behållare med [översikt över Service Fabric och behållare](service-fabric-containers-overview.md)</span><span class="sxs-lookup"><span data-stu-id="18731-165">Learn about deploying containers with [Service Fabric and containers overview](service-fabric-containers-overview.md)</span></span>
* [<span data-ttu-id="18731-166">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="18731-166">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="18731-167">Exempel på två gäst körbara filer (C# och nodejs) kommunicerar via Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="18731-167">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
