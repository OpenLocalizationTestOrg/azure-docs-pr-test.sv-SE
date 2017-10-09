---
title: "aaaService infrastrukturen och distribuera behållare | Microsoft Docs"
description: "Service Fabric och hello användning av behållare toodeploy mikrotjänster program. Den här artikeln beskriver hello-funktioner med Service Fabric för behållare och hur toodeploy en behållare för Windows image i ett kluster."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a><span data-ttu-id="9f0c5-104">Distribuera en Windows-behållaren tooService Fabric</span><span class="sxs-lookup"><span data-stu-id="9f0c5-104">Deploy a Windows container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9f0c5-105">Distribuera Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="9f0c5-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="9f0c5-106">Distribuera Dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="9f0c5-106">Deploy Docker Container</span></span>](service-fabric-deploy-container-linux.md)
> 
> 

<span data-ttu-id="9f0c5-107">Den här artikeln vägleder dig genom hello processen att bygga av tjänster i Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-107">This article walks you through hello process of building containerized services in Windows containers.</span></span>

<span data-ttu-id="9f0c5-108">Service Fabric har flera funktioner som hjälper dig med att skapa program som består av mikrotjänster som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-108">Service Fabric has several capabilities that help you with building applications that are composed of microservices running inside containers.</span></span> 

<span data-ttu-id="9f0c5-109">hello funktioner omfattar:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-109">hello capabilities include:</span></span>

* <span data-ttu-id="9f0c5-110">Distribution av avbildningar för behållaren och aktivering</span><span class="sxs-lookup"><span data-stu-id="9f0c5-110">Container image deployment and activation</span></span>
* <span data-ttu-id="9f0c5-111">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="9f0c5-111">Resource governance</span></span>
* <span data-ttu-id="9f0c5-112">Autentisering för databasen</span><span class="sxs-lookup"><span data-stu-id="9f0c5-112">Repository authentication</span></span>
* <span data-ttu-id="9f0c5-113">Portmappning för behållaren port-till-värd</span><span class="sxs-lookup"><span data-stu-id="9f0c5-113">Container port-to-host port mapping</span></span>
* <span data-ttu-id="9f0c5-114">Identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="9f0c5-114">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="9f0c5-115">Möjlighet tooconfigure och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="9f0c5-115">Ability tooconfigure and set environment variables</span></span>

<span data-ttu-id="9f0c5-116">Nu ska vi titta på hur var och en av funktionerna fungerar när du paketerar ett container service-toobe som ingår i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-116">Let's look at how each of capabilities works when you're packaging a containerized service toobe included in your application.</span></span>

## <a name="package-a-windows-container"></a><span data-ttu-id="9f0c5-117">Paketet en Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="9f0c5-117">Package a Windows container</span></span>
<span data-ttu-id="9f0c5-118">När du paketerar en behållare kan du välja toouse antingen en projektmall för Visual Studio eller [skapa hello programpaket manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-118">When you package a container, you can choose toouse either a Visual Studio project template or [create hello application package manually](#manually).</span></span>  <span data-ttu-id="9f0c5-119">När du använder Visual Studio hello programmet paketet struktur och manifest-filer skapas av hello nytt projekt mallen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-119">When you use Visual Studio, hello application package structure and manifest files are created by hello New Project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="9f0c5-120">hello enklaste sättet toopackage en befintlig behållare avbildning till en tjänst är toouse Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-120">hello easiest way toopackage an existing container image into a service is toouse Visual Studio.</span></span>

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a><span data-ttu-id="9f0c5-121">Använd Visual Studio toopackage en befintlig avbildning i behållaren</span><span class="sxs-lookup"><span data-stu-id="9f0c5-121">Use Visual Studio toopackage an existing container image</span></span>
<span data-ttu-id="9f0c5-122">Visual Studio har ett Service Fabric service mallen toohelp du distribuerar en behållare tooa Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-122">Visual Studio provides a Service Fabric service template toohelp you deploy a container tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="9f0c5-123">Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-123">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="9f0c5-124">Välj **gäst behållaren** som mall för hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-124">Choose **Guest Container** as hello service template.</span></span>
3. <span data-ttu-id="9f0c5-125">Välj **avbildningsnamn** och ange hello sökvägen toohello bilden i lagringsplatsen för behållaren.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-125">Choose **Image Name** and provide hello path toohello image in your container repository.</span></span> <span data-ttu-id="9f0c5-126">Till exempel `myrepo/myimage:v1` i https://hub.docker.com</span><span class="sxs-lookup"><span data-stu-id="9f0c5-126">For example, `myrepo/myimage:v1` in https://hub.docker.com</span></span>
4. <span data-ttu-id="9f0c5-127">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-127">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="9f0c5-128">Om av tjänsten måste en slutpunkt för kommunikation, kan du nu lägga till hello-protokollet, porten och typen toohello ServiceManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-128">If your containerized service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="9f0c5-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-129">For example:</span></span> 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    <span data-ttu-id="9f0c5-130">Genom att tillhandahålla hello `UriScheme`, Service Fabric automatiskt registrerar hello behållaren slutpunkt med hello Naming service för synlighet.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-130">By providing hello `UriScheme`, Service Fabric automatically registers hello container endpoint with hello Naming service for discoverability.</span></span> <span data-ttu-id="9f0c5-131">hello porten kan vara fast (som visas i föregående exempel hello) eller dynamiskt allokerade.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-131">hello port can either be fixed (as shown in hello preceding example) or dynamically allocated.</span></span> <span data-ttu-id="9f0c5-132">Om du inte anger en port är den dynamiskt allokerade från hello programmet portintervall (som skulle hända med alla service).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-132">If you don't specify a port, it is dynamically allocated from hello application port range (as would happen with any service).</span></span>
    <span data-ttu-id="9f0c5-133">Du måste också tooconfigure hello behållaren toohost portmappning genom att ange en `PortBinding` princip i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-133">You also need tooconfigure hello container toohost port mapping by specifying a `PortBinding` policy in hello application manifest.</span></span> <span data-ttu-id="9f0c5-134">Mer information finns i [konfigurera behållaren toohost portmappning](#Portsection).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-134">For more information, see [Configure container toohost port mapping](#Portsection).</span></span>
6. <span data-ttu-id="9f0c5-135">Om din behållaren måste resursen styrning och lägger till en `ResourceGovernancePolicy`.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-135">If your container needs resource governance then add a `ResourceGovernancePolicy`.</span></span>
8. <span data-ttu-id="9f0c5-136">Om din behållaren måste tooauthenticate med en privat databas, Lägg sedan till `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-136">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>
7. <span data-ttu-id="9f0c5-137">Om du kör på en Windows Server 2016-dator med aktiverat stöd för behållare, kan du använda hello paketet och publicera åtgärd toodeploy tooyour lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-137">If you are running on a Windows Server 2016 machine with container support enabled, you can use hello package and publish action toodeploy tooyour local cluster.</span></span> 
8. <span data-ttu-id="9f0c5-138">När du är klar kan du publicera programmet hello tooa fjärrkluster eller kontrollera hello lösning toosource kontrollen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-138">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span> 

<span data-ttu-id="9f0c5-139">Ett exempel utcheckningen hello [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="9f0c5-139">For an example, checkout hello [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="creating-a-windows-server-2016-cluster"></a><span data-ttu-id="9f0c5-140">Skapa ett kluster för Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="9f0c5-140">Creating a Windows Server 2016 cluster</span></span>
<span data-ttu-id="9f0c5-141">toodeploy av programmet, du behöver toocreate aktiverad i ett kluster som kör Windows Server 2016 med stöd för behållaren.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-141">toodeploy your containerized application, you need toocreate a cluster running Windows Server 2016 with container support enabled.</span></span> <span data-ttu-id="9f0c5-142">Klustret kan köras lokalt eller distribueras via Azure Resource Manager i Azure.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-142">Your cluster may be running locally, or deployed via Azure Resource Manager in Azure.</span></span> 

<span data-ttu-id="9f0c5-143">toodeploy ett kluster med Azure Resource Manager, Välj hello **Windows Server 2016 med behållare** bild-alternativet i Azure.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-143">toodeploy a cluster using Azure Resource Manager, choose hello **Windows Server 2016 with Containers** image option in Azure.</span></span> <span data-ttu-id="9f0c5-144">Se artikeln hello [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-144">See hello article [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="9f0c5-145">Kontrollera att du använder hello följande Azure Resource Manager-inställningar:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-145">Ensure that you use hello following Azure Resource Manager settings:</span></span>

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
<span data-ttu-id="9f0c5-146">Du kan också använda hello [fem nod Azure Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate ett kluster.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-146">You can also use hello [Five Node Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate a cluster.</span></span> <span data-ttu-id="9f0c5-147">Du kan också läsa en grupp [blogginlägget](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) med hjälp av Service Fabric- och Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-147">Alternatively read a community [blog post](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) on using Service Fabric and Windows containers.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="9f0c5-148">Paketera och distribuera en avbildning för behållaren manuellt</span><span class="sxs-lookup"><span data-stu-id="9f0c5-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="9f0c5-149">hello processen manuellt paketera container service baseras på hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="9f0c5-150">Publicera hello behållare tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="9f0c5-151">Skapa hello paketet katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="9f0c5-152">Redigera hello service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="9f0c5-153">Redigera hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="9f0c5-154">Distribuera och aktivera en avbildning av behållare</span><span class="sxs-lookup"><span data-stu-id="9f0c5-154">Deploy and activate a container image</span></span>
<span data-ttu-id="9f0c5-155">I hello Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="9f0c5-156">toodeploy och aktivera en behållare, placera hello namnet på hello behållaren bilden i en `ContainerHost` element i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="9f0c5-157">Hello tjänstmanifestet, lägga till en `ContainerHost` för hello startpunkt.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="9f0c5-158">Därefter uppsättning hello `ImageName` toobe hello namnet på hello behållaren databasen och image.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="9f0c5-159">hello följande partiella manifestet visar ett exempel på hur toodeploy hello behållaren kallas `myimage:v1` från en databas som heter `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="9f0c5-160">Du kan ange ytterligare kommandon toorun vid start av hello behållare under hello `Commands` element.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-160">You can specify optional commands toorun upon starting hello container under hello `Commands` element.</span></span> <span data-ttu-id="9f0c5-161">För flera kommandon, komma-avgränsa dem.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-161">For multiple commands, comma-delimit them.</span></span> 

## <a name="understand-resource-governance"></a><span data-ttu-id="9f0c5-162">Förstå resource-styrning</span><span class="sxs-lookup"><span data-stu-id="9f0c5-162">Understand resource governance</span></span>
<span data-ttu-id="9f0c5-163">Resurs-styrning är en funktion för hello-behållare som begränsar hello resurser som hello behållare kan använda på hello värden.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="9f0c5-164">Hej `ResourceGovernancePolicy`, som har angetts i hello programmanifestet är används toodeclare gränserna för ett paket för service-kod.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="9f0c5-165">Resursen gränser kan anges för hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="9f0c5-166">Minne</span><span class="sxs-lookup"><span data-stu-id="9f0c5-166">Memory</span></span>
* <span data-ttu-id="9f0c5-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="9f0c5-167">MemorySwap</span></span>
* <span data-ttu-id="9f0c5-168">CpuShares (CPU relativa viktade)</span><span class="sxs-lookup"><span data-stu-id="9f0c5-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="9f0c5-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="9f0c5-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="9f0c5-170">BlkioWeight (BlockIO relativa vikt).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="9f0c5-171">Stöd för att ange specifika blockera i/o-gränser som IOPs och läsning/skrivning BPS planeras för framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-171">Support for specifying specific block IO limits such as IOPs, read/write BPS, and others are planned for a future release.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="9f0c5-172">Autentisera en databas</span><span class="sxs-lookup"><span data-stu-id="9f0c5-172">Authenticate a repository</span></span>
<span data-ttu-id="9f0c5-173">Du kan ha tooprovide inloggningsuppgifter toohello behållare databasen toodownload en behållare.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="9f0c5-174">hello inloggningsuppgifter, anges i hello programmanifestet är används toospecify hello inloggningsinformation eller SSH-nyckeln för att ladda ned hello behållaren avbildningen från hello avbildningslagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="9f0c5-175">hello följande exempel visas ett konto som heter *TestUser* tillsammans med hello lösenord i klartext (*inte* rekommenderas):</span><span class="sxs-lookup"><span data-stu-id="9f0c5-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="9f0c5-176">Vi rekommenderar att du krypterar hello lösenord med hjälp av ett certifikat som har distribuerats toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="9f0c5-177">hello följande exempel visas ett konto som heter *TestUser*, där hello lösenord har krypterats med ett certifikat som kallas *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="9f0c5-178">Du kan använda hello `Invoke-ServiceFabricEncryptText` PowerShell-kommandot toocreate hello hemliga chiffertext hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="9f0c5-179">Mer information finns i artikeln hello [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="9f0c5-180">hello privat nyckel för hello-certifikat som har använt toodecrypt hello lösenord måste vara distribuerad toohello lokal dator i en out-of-band-metod.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="9f0c5-181">(Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar hello paketet toohello maskin, kan den sedan dekryptera hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="9f0c5-182">Med hjälp av hello hemlighet tillsammans med hello kontonamn kan den autentisera med hello behållaren databasen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

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

## <span data-ttu-id="9f0c5-183"><a name ="Portsection"></a>Konfigurera mappning för behållaren toohost port</span><span class="sxs-lookup"><span data-stu-id="9f0c5-183"><a name ="Portsection"></a> Configure container toohost port mapping</span></span>
<span data-ttu-id="9f0c5-184">Du kan konfigurera en toocommunicate för porten som används av värden med hello behållare genom att ange en `PortBinding` i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="9f0c5-185">hello port bindning maps hello port toowhich hello tjänsten lyssnar inuti hello behållaren tooa port på hello värden.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="9f0c5-186">Konfigurera identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="9f0c5-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="9f0c5-187">Du kan använda hello `PortBinding` elementet toomap en behållare port tooan slutpunkt i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-187">You can use hello `PortBinding` element toomap a container port tooan endpoint in hello service manifest.</span></span> <span data-ttu-id="9f0c5-188">I följande exempel hello, hello endpoint `Endpoint1` och anger en fast port 8905.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-188">In hello following example, hello endpoint `Endpoint1` specifies a fixed port, 8905.</span></span> <span data-ttu-id="9f0c5-189">Det kan också ange ingen port, och då en slumpmässigt vald port från portintervall för hello kluster programmet är valt.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>


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
<span data-ttu-id="9f0c5-190">Om du anger en slutpunkt med hello `Endpoint` tagg i hello tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="9f0c5-191">Andra tjänster som körs i klustret hello kan därför att identifiera den här behållaren med hjälp av hello REST-frågor för att lösa.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

<span data-ttu-id="9f0c5-192">Genom att registrera med hello Naming service kan du utföra behållare till en annan kommunikation i en behållare med hello [omvänd proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-192">By registering with hello Naming service, you can perform container-to-container communication within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="9f0c5-193">Kommunikation utförs genom att tillhandahålla omvänd proxy hello http lyssningsport och hello namn hello-tjänster som du vill använda toocommunicate med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="9f0c5-194">Mer information finns i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-194">For more information, see hello next section.</span></span> 

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="9f0c5-195">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="9f0c5-195">Configure and set environment variables</span></span>
<span data-ttu-id="9f0c5-196">Miljövariabler kan anges för varje kodpaketet i hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-196">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="9f0c5-197">Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-197">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="9f0c5-198">Du kan åsidosätta miljövariabeln värden i hello application manifest eller ange dem under distributionen som parametrar för programmet.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-198">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="9f0c5-199">hello följande service manifest XML-fragment visar ett exempel på hur toospecify miljövariabler för ett paket med koden:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-199">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

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

<span data-ttu-id="9f0c5-200">De här miljövariablerna kan åsidosättas på hello manifestet programnivå:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-200">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="9f0c5-201">I föregående exempel hello vi angav ett explicit värde för hello `HttpGateway` miljövariabeln (19000), medan vi ordnar hello värde för `BackendServiceName` parametern via hello `[BackendSvc]` program-parametern.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-201">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="9f0c5-202">Dessa inställningar kan du toospecify hello värdet för `BackendServiceName`värde när du distribuerar hello program och inte har ett fast värde i hello manifest.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-202">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="configure-isolation-mode"></a><span data-ttu-id="9f0c5-203">Konfigurera isoleringsläge</span><span class="sxs-lookup"><span data-stu-id="9f0c5-203">Configure isolation mode</span></span>

<span data-ttu-id="9f0c5-204">Windows stöder två arbetslägen för behållare - processen och Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-204">Windows supports two isolation modes for containers - process and Hyper-V.</span></span>  <span data-ttu-id="9f0c5-205">Med hello arbetsprocesser hello alla hello-behållare som körs på samma värd datorn resursen hello kernel med hello-värden.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-205">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="9f0c5-206">Med hello Hyper-V-isoleringsläge isoleras hello kärnor mellan varje Hyper-V-behållaren och hello behållaren värden.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-206">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="9f0c5-207">hello isoleringsläge har angetts i hello `ContainerHostPolicies` tagg i hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-207">hello isolation mode is specified in hello `ContainerHostPolicies` tag in hello application manifest file.</span></span>  <span data-ttu-id="9f0c5-208">hello arbetslägen som kan anges är `process`, `hyperv`, och `default`.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-208">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="9f0c5-209">Hej `default` isoleringsläge standardvärden för`process` på Windows Server är värd för, och är som standard för`hyperv` på Windows 10-värdar.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-209">hello `default` isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span>  <span data-ttu-id="9f0c5-210">hello följande utdrag visar hur hello isoleringsläge har angetts i hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="9f0c5-210">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="9f0c5-211">Slutföra exempel för programmet och tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="9f0c5-211">Complete examples for application and service manifest</span></span>

<span data-ttu-id="9f0c5-212">Ett exempel programmanifest följande:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-212">An example application manifest follows:</span></span>

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

<span data-ttu-id="9f0c5-213">Ett exempel tjänstmanifestet (anges i föregående programmanifestet hello) visas nedan:</span><span class="sxs-lookup"><span data-stu-id="9f0c5-213">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="9f0c5-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f0c5-214">Next steps</span></span>
<span data-ttu-id="9f0c5-215">Nu när du har använt ett container service lära dig hur toomanage livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="9f0c5-215">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="9f0c5-216">Översikt över Service Fabric och behållare</span><span class="sxs-lookup"><span data-stu-id="9f0c5-216">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* <span data-ttu-id="9f0c5-217">Ett exempel utcheckningen [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="9f0c5-217">For an example, checkout [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>
