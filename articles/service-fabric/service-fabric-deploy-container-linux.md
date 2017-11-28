---
title: "aaaService infrastruktur och distribuerar behållare i Linux | Microsoft Docs"
description: "Service Fabric och hello används av Linux behållare toodeploy mikrotjänster program. Den här artikeln beskriver hello-funktioner med Service Fabric för behållare och hur toodeploy en Linux-behållare bild i ett kluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a><span data-ttu-id="db21e-104">Distribuera en Linux-behållaren tooService Fabric</span><span class="sxs-lookup"><span data-stu-id="db21e-104">Deploy a Linux container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db21e-105">Distribuera Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="db21e-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="db21e-106">Distribuera behållare för Linux</span><span class="sxs-lookup"><span data-stu-id="db21e-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="db21e-107">Den här artikeln vägleder dig genom att bygga av tjänster i Docker behållare på Linux.</span><span class="sxs-lookup"><span data-stu-id="db21e-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="db21e-108">Service Fabric har flera behållare funktioner som hjälper dig med att skapa program som består av mikrotjänster som är behållare.</span><span class="sxs-lookup"><span data-stu-id="db21e-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="db21e-109">Dessa tjänster kallas av tjänster.</span><span class="sxs-lookup"><span data-stu-id="db21e-109">These services are called containerized services.</span></span>

<span data-ttu-id="db21e-110">hello funktionerna innehåller;</span><span class="sxs-lookup"><span data-stu-id="db21e-110">hello capabilities include;</span></span>

* <span data-ttu-id="db21e-111">Distribution av avbildningar för behållaren och aktivering</span><span class="sxs-lookup"><span data-stu-id="db21e-111">Container image deployment and activation</span></span>
* <span data-ttu-id="db21e-112">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="db21e-112">Resource governance</span></span>
* <span data-ttu-id="db21e-113">Autentisering för databasen</span><span class="sxs-lookup"><span data-stu-id="db21e-113">Repository authentication</span></span>
* <span data-ttu-id="db21e-114">Behållaren toohost port portmappning</span><span class="sxs-lookup"><span data-stu-id="db21e-114">Container port toohost port mapping</span></span>
* <span data-ttu-id="db21e-115">Identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="db21e-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="db21e-116">Möjlighet tooconfigure och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="db21e-116">Ability tooconfigure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="db21e-117">Paketera en dockerbehållare med yeoman</span><span class="sxs-lookup"><span data-stu-id="db21e-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="db21e-118">När en behållare på Linux-paket, kan du välja antingen toouse yeoman mall eller [skapa hello programpaket manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="db21e-118">When packaging a container on Linux, you can choose either toouse a yeoman template or [create hello application package manually](#manually).</span></span>

<span data-ttu-id="db21e-119">Ett Service Fabric-program kan innehålla en eller flera behållare med en viss roll i att leverera hello-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="db21e-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="db21e-120">hello Service Fabric-SDK för Linux innehåller en [Yeoman](http://yeoman.io/) generator som gör det enkelt toocreate ditt program och lägga till en behållare avbildning.</span><span class="sxs-lookup"><span data-stu-id="db21e-120">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="db21e-121">Nu ska vi använda Yeoman toocreate kallas för ett program med en enda dockerbehållare *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="db21e-121">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="db21e-122">Du kan senare lägga till fler tjänster genom att redigera hello genereras manifest-filer.</span><span class="sxs-lookup"><span data-stu-id="db21e-122">You can add more services later by editing hello generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="db21e-123">Installera Docker på utveckling-ruta</span><span class="sxs-lookup"><span data-stu-id="db21e-123">Install Docker on your development box</span></span>

<span data-ttu-id="db21e-124">Kör hello följande kommandon tooinstall docker på rutan ditt Linux-utveckling (om du använder hello vagrant i OSX docker redan har installerats):</span><span class="sxs-lookup"><span data-stu-id="db21e-124">Run hello following commands tooinstall docker on your Linux development box (if you are using hello vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a><span data-ttu-id="db21e-125">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="db21e-125">Create hello application</span></span>
1. <span data-ttu-id="db21e-126">I en terminal, skriver du in `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="db21e-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="db21e-127">Namnge programmet – till exempel mycontainerap</span><span class="sxs-lookup"><span data-stu-id="db21e-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="db21e-128">Ange hello URL för hello behållaren avbildningen från en DockerHub lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="db21e-128">Provide hello URL for hello container image from a DockerHub repo.</span></span> <span data-ttu-id="db21e-129">Hej avbildningen parametern tar hello formuläret [lagringsplatsen] / [image name]</span><span class="sxs-lookup"><span data-stu-id="db21e-129">hello image parameter takes hello form [repo]/[image name]</span></span>
4. <span data-ttu-id="db21e-130">Om hello avbildningen inte har en arbetsbelastning startpunkten definierats, måste du tooexplicitly ange inkommande kommandon med en kommaavgränsad uppsättning kommandon toorun inuti hello behållare som kommer att hålla hello-behållare som körs efter start.</span><span class="sxs-lookup"><span data-stu-id="db21e-130">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="deploy-hello-application"></a><span data-ttu-id="db21e-132">Distribuera programmet hello</span><span class="sxs-lookup"><span data-stu-id="db21e-132">Deploy hello application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="db21e-133">Med XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="db21e-133">Using XPlat CLI</span></span>
<span data-ttu-id="db21e-134">När hello programmet är skapat kan distribuera du den toohello lokala kluster med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="db21e-134">Once hello application is built, you can deploy it toohello local cluster using hello Azure CLI.</span></span>

1. <span data-ttu-id="db21e-135">Ansluta toohello lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="db21e-135">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="db21e-136">Använd hello installera skript hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="db21e-136">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="db21e-137">Öppna en webbläsare och gå tooService Fabric-Utforskaren på http://localhost:19080/Explorer (Ersätt localhost med hello privat IP hello VM om använder Vagrant på Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="db21e-137">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="db21e-138">Expandera noden för hello-program och Lägg märke till att det finns en post för din typ av program och en annan för hello första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="db21e-138">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>
5. <span data-ttu-id="db21e-139">Använd hello avinstallera skript i hello toodelete hello programmet mallinstansen och avregistrering av programtyp hello.</span><span class="sxs-lookup"><span data-stu-id="db21e-139">Use hello uninstall script provided in hello template toodelete hello application instance and unregister hello application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="db21e-140">Med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="db21e-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="db21e-141">Se hello referens doc om hur du hanterar en [livscykel som använder hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="db21e-141">See hello reference doc on managing an [application life cycle using hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="db21e-142">Ett exempelprogram [utcheckningen hello Service Fabric-behållaren kod exempel på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="db21e-142">For an example application, [checkout hello Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="db21e-143">Lägga till fler tjänster tooan befintliga program</span><span class="sxs-lookup"><span data-stu-id="db21e-143">Adding more services tooan existing application</span></span>

<span data-ttu-id="db21e-144">tooadd en annan behållare tjänstprogram tooan redan har skapats med hjälp av `yo`, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db21e-144">tooadd another container service tooan application already created using `yo`, perform hello following steps:</span></span>

1. <span data-ttu-id="db21e-145">Ändra toohello rotkatalog till hello befintliga program.</span><span class="sxs-lookup"><span data-stu-id="db21e-145">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="db21e-146">Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="db21e-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="db21e-147">Kör `yo azuresfcontainer:AddService`</span><span class="sxs-lookup"><span data-stu-id="db21e-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="db21e-148">Paketera och distribuera en avbildning för behållaren manuellt</span><span class="sxs-lookup"><span data-stu-id="db21e-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="db21e-149">hello processen manuellt paketera container service baseras på hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db21e-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="db21e-150">Publicera hello behållare tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="db21e-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="db21e-151">Skapa hello paketet katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="db21e-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="db21e-152">Redigera hello service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="db21e-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="db21e-153">Redigera hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="db21e-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="db21e-154">Distribuera och aktivera en avbildning av behållare</span><span class="sxs-lookup"><span data-stu-id="db21e-154">Deploy and activate a container image</span></span>
<span data-ttu-id="db21e-155">I hello Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras.</span><span class="sxs-lookup"><span data-stu-id="db21e-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="db21e-156">toodeploy och aktivera en behållare, placera hello namnet på hello behållaren bilden i en `ContainerHost` element i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="db21e-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="db21e-157">Hello tjänstmanifestet, lägga till en `ContainerHost` för hello startpunkt.</span><span class="sxs-lookup"><span data-stu-id="db21e-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="db21e-158">Därefter uppsättning hello `ImageName` toobe hello namnet på hello behållaren databasen och image.</span><span class="sxs-lookup"><span data-stu-id="db21e-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="db21e-159">hello följande partiella manifestet visar ett exempel på hur toodeploy hello behållaren kallas `myimage:v1` från en databas som heter `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="db21e-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

<span data-ttu-id="db21e-160">Du kan ange inkommande kommandon genom att ange hello valfria `Commands` element med en kommaavgränsad uppsättning kommandon toorun i hello behållare.</span><span class="sxs-lookup"><span data-stu-id="db21e-160">You can provide input commands by specifying hello optional `Commands` element with a comma-delimited set of commands toorun inside hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="db21e-161">Om hello avbildningen inte har en arbetsbelastning startpunkten definierats, måste du tooexplicitly ange inkommande kommandon i `Commands` element med en kommaavgränsad uppsättning kommandon toorun inuti hello behållare som kommer att hålla hello-behållare som körs Start.</span><span class="sxs-lookup"><span data-stu-id="db21e-161">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands inside `Commands` element with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="db21e-162">Förstå resource-styrning</span><span class="sxs-lookup"><span data-stu-id="db21e-162">Understand resource governance</span></span>
<span data-ttu-id="db21e-163">Resurs-styrning är en funktion för hello-behållare som begränsar hello resurser som hello behållare kan använda på hello värden.</span><span class="sxs-lookup"><span data-stu-id="db21e-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="db21e-164">Hej `ResourceGovernancePolicy`, som har angetts i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod.</span><span class="sxs-lookup"><span data-stu-id="db21e-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="db21e-165">Resursen gränser kan anges för hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="db21e-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="db21e-166">Minne</span><span class="sxs-lookup"><span data-stu-id="db21e-166">Memory</span></span>
* <span data-ttu-id="db21e-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="db21e-167">MemorySwap</span></span>
* <span data-ttu-id="db21e-168">CpuShares (CPU relativa viktade)</span><span class="sxs-lookup"><span data-stu-id="db21e-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="db21e-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="db21e-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="db21e-170">BlkioWeight (BlockIO relativa vikt).</span><span class="sxs-lookup"><span data-stu-id="db21e-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="db21e-171">I en framtida utgåva, stöd för att ange specifika block-i/o-gränser, till exempel IOPs, läsning och skrivning bit/s och andra ska inkluderas.</span><span class="sxs-lookup"><span data-stu-id="db21e-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
>
>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a><span data-ttu-id="db21e-172">Autentisera en databas</span><span class="sxs-lookup"><span data-stu-id="db21e-172">Authenticate a repository</span></span>
<span data-ttu-id="db21e-173">Du kan ha tooprovide inloggningsuppgifter toohello behållare databasen toodownload en behållare.</span><span class="sxs-lookup"><span data-stu-id="db21e-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="db21e-174">hello inloggningsuppgifter, anges i hello programmanifestet är används toospecify hello inloggningsinformation eller SSH-nyckeln för att ladda ned hello behållaren avbildningen från hello avbildningslagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="db21e-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="db21e-175">hello följande exempel visas ett konto som heter *TestUser* tillsammans med hello lösenord i klartext (*inte* rekommenderas):</span><span class="sxs-lookup"><span data-stu-id="db21e-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

<span data-ttu-id="db21e-176">Vi rekommenderar att du krypterar hello lösenord med hjälp av ett certifikat som har distribuerats toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="db21e-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="db21e-177">hello följande exempel visas ett konto som heter *TestUser*, där hello lösenord har krypterats med ett certifikat som kallas *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="db21e-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="db21e-178">Du kan använda hello `Invoke-ServiceFabricEncryptText` PowerShell-kommandot toocreate hello hemliga chiffertext hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="db21e-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="db21e-179">Mer information finns i artikeln hello [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="db21e-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="db21e-180">hello privat nyckel för hello-certifikat som har använt toodecrypt hello lösenord måste vara distribuerad toohello lokal dator i en out-of-band-metod.</span><span class="sxs-lookup"><span data-stu-id="db21e-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="db21e-181">(Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar hello paketet toohello maskin, kan den sedan dekryptera hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="db21e-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="db21e-182">Med hjälp av hello hemlighet tillsammans med hello kontonamn kan den autentisera med hello behållaren databasen.</span><span class="sxs-lookup"><span data-stu-id="db21e-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="db21e-183">Konfigurera portmappning behållaren port-till-värd</span><span class="sxs-lookup"><span data-stu-id="db21e-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="db21e-184">Du kan konfigurera en toocommunicate för porten som används av värden med hello behållare genom att ange en `PortBinding` i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="db21e-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="db21e-185">hello port bindning maps hello port toowhich hello tjänsten lyssnar inuti hello behållaren tooa port på hello värden.</span><span class="sxs-lookup"><span data-stu-id="db21e-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="db21e-186">Konfigurera identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="db21e-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="db21e-187">Med hjälp av hello `PortBinding` princip, kan du mappa en behållare port tooan `Endpoint` i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="db21e-187">By using hello `PortBinding` policy, you can map a container port tooan `Endpoint` in hello service manifest.</span></span> <span data-ttu-id="db21e-188">Hej endpoint `Endpoint1` kan ange en fast port (till exempel port 80).</span><span class="sxs-lookup"><span data-stu-id="db21e-188">hello endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="db21e-189">Det kan också ange ingen port, och då en slumpmässigt vald port från portintervall för hello kluster programmet är valt.</span><span class="sxs-lookup"><span data-stu-id="db21e-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="db21e-190">Om du anger en slutpunkt med hello `Endpoint` tagg i hello tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="db21e-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="db21e-191">Andra tjänster som körs i klustret hello kan därför att identifiera den här behållaren med hjälp av hello REST-frågor för att lösa.</span><span class="sxs-lookup"><span data-stu-id="db21e-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

<span data-ttu-id="db21e-192">Genom att registrera med hello Naming service kan du enkelt göra behållare till en annan kommunikation i hello kod i din behållaren med hjälp av hello [omvänd proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="db21e-192">By registering with hello Naming service, you can easily do container-to-container communication in hello code within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="db21e-193">Kommunikation utförs genom att tillhandahålla omvänd proxy hello http lyssningsport och hello namn hello-tjänster som du vill använda toocommunicate med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="db21e-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="db21e-194">Mer information finns i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="db21e-194">For more information, see hello next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="db21e-195">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="db21e-195">Configure and set environment variables</span></span>
<span data-ttu-id="db21e-196">Miljövariabler kan anges för varje kodpaketet i hello tjänstmanifestet, både för tjänster som har distribuerats i behållare eller tjänster som distribueras som processer/gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="db21e-196">Environment variables can be specified for each code package in hello service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="db21e-197">Dessa miljö variabelvärden kan åsidosättas i programmanifestet hello eller anges under distributionen som parametrar för programmet.</span><span class="sxs-lookup"><span data-stu-id="db21e-197">These environment variable values can be overridden specifically in hello application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="db21e-198">hello följande service manifest XML-fragment visar ett exempel på hur toospecify miljövariabler för ett paket med koden:</span><span class="sxs-lookup"><span data-stu-id="db21e-198">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

<span data-ttu-id="db21e-199">De här miljövariablerna kan åsidosättas på hello manifestet programnivå:</span><span class="sxs-lookup"><span data-stu-id="db21e-199">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="db21e-200">I föregående exempel hello vi angav ett explicit värde för hello `HttpGateway` miljövariabeln (19000), medan vi ordnar hello värde för `BackendServiceName` parametern via hello `[BackendSvc]` program-parametern.</span><span class="sxs-lookup"><span data-stu-id="db21e-200">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="db21e-201">Dessa inställningar kan du toospecify hello värdet för `BackendServiceName`värde när du distribuerar hello program och inte har ett fast värde i hello manifest.</span><span class="sxs-lookup"><span data-stu-id="db21e-201">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="db21e-202">Slutföra exempel för programmet och tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="db21e-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="db21e-203">Ett exempel programmanifest följande:</span><span class="sxs-lookup"><span data-stu-id="db21e-203">An example application manifest follows:</span></span>

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

<span data-ttu-id="db21e-204">Ett exempel tjänstmanifestet (anges i föregående programmanifestet hello) visas nedan:</span><span class="sxs-lookup"><span data-stu-id="db21e-204">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a><span data-ttu-id="db21e-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db21e-205">Next steps</span></span>
<span data-ttu-id="db21e-206">Nu när du har använt ett container service lära dig hur toomanage livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="db21e-206">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="db21e-207">Översikt över Service Fabric och behållare</span><span class="sxs-lookup"><span data-stu-id="db21e-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="db21e-208">Interaktion med Service Fabric-kluster med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="db21e-208">Interacting with Service Fabric clusters using hello Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="db21e-209">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="db21e-209">Related articles</span></span>

* [<span data-ttu-id="db21e-210">Kom igång med Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="db21e-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="db21e-211">Kom igång med Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="db21e-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
