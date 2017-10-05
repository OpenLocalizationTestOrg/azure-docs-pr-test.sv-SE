---
title: "Service Fabric och hur du distribuerar behållare i Linux | Microsoft Docs"
description: "Service Fabric och användning av Linux-behållare för att distribuera mikrotjänster program. Den här artikeln beskriver de funktioner som Service Fabric ger för behållare och hur du distribuerar en avbildning för Linux-behållaren i ett kluster"
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
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a><span data-ttu-id="4e982-104">Distribuera en Linux-behållare till Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4e982-104">Deploy a Linux container to Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e982-105">Distribuera Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="4e982-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="4e982-106">Distribuera behållare för Linux</span><span class="sxs-lookup"><span data-stu-id="4e982-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="4e982-107">Den här artikeln vägleder dig genom att bygga av tjänster i Docker behållare på Linux.</span><span class="sxs-lookup"><span data-stu-id="4e982-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="4e982-108">Service Fabric har flera behållare funktioner som hjälper dig med att skapa program som består av mikrotjänster som är behållare.</span><span class="sxs-lookup"><span data-stu-id="4e982-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="4e982-109">Dessa tjänster kallas av tjänster.</span><span class="sxs-lookup"><span data-stu-id="4e982-109">These services are called containerized services.</span></span>

<span data-ttu-id="4e982-110">Funktionerna innehåller;</span><span class="sxs-lookup"><span data-stu-id="4e982-110">The capabilities include;</span></span>

* <span data-ttu-id="4e982-111">Distribution av avbildningar för behållaren och aktivering</span><span class="sxs-lookup"><span data-stu-id="4e982-111">Container image deployment and activation</span></span>
* <span data-ttu-id="4e982-112">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="4e982-112">Resource governance</span></span>
* <span data-ttu-id="4e982-113">Autentisering för databasen</span><span class="sxs-lookup"><span data-stu-id="4e982-113">Repository authentication</span></span>
* <span data-ttu-id="4e982-114">Behållaren port till värden portmappning</span><span class="sxs-lookup"><span data-stu-id="4e982-114">Container port to host port mapping</span></span>
* <span data-ttu-id="4e982-115">Identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="4e982-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="4e982-116">Möjligheten att konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="4e982-116">Ability to configure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="4e982-117">Paketera en dockerbehållare med yeoman</span><span class="sxs-lookup"><span data-stu-id="4e982-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="4e982-118">När en behållare på Linux-paket, kan du välja antingen att använda en mall för yeoman eller [skapa programpaketet manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="4e982-118">When packaging a container on Linux, you can choose either to use a yeoman template or [create the application package manually](#manually).</span></span>

<span data-ttu-id="4e982-119">Ett Service Fabric-program kan innehålla en eller flera behållare med en viss roll i att leverera programmets funktioner.</span><span class="sxs-lookup"><span data-stu-id="4e982-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="4e982-120">I Service Fabric SDK för Linux finns en [Yeoman](http://yeoman.io/)-generator som gör det enkelt att skapa ditt program och lägga till en behållaravbildning.</span><span class="sxs-lookup"><span data-stu-id="4e982-120">The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image.</span></span> <span data-ttu-id="4e982-121">Nu ska vi använda Yeoman för att skapa ett program med en enda dockerbehållare som kallas *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="4e982-121">Let's use Yeoman to create an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="4e982-122">Du kan lägga till fler tjänster senare genom att redigera den genererade manifest filer.</span><span class="sxs-lookup"><span data-stu-id="4e982-122">You can add more services later by editing the generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="4e982-123">Installera Docker på utveckling-ruta</span><span class="sxs-lookup"><span data-stu-id="4e982-123">Install Docker on your development box</span></span>

<span data-ttu-id="4e982-124">Kör följande kommandon för att installera docker på rutan ditt Linux-utveckling (om du använder vagrant bilden i OSX docker redan har installerats):</span><span class="sxs-lookup"><span data-stu-id="4e982-124">Run the following commands to install docker on your Linux development box (if you are using the vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a><span data-ttu-id="4e982-125">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="4e982-125">Create the application</span></span>
1. <span data-ttu-id="4e982-126">I en terminal, skriver du in `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="4e982-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="4e982-127">Namnge programmet – till exempel mycontainerap</span><span class="sxs-lookup"><span data-stu-id="4e982-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="4e982-128">Ange en URL för bilden behållare från en DockerHub lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4e982-128">Provide the URL for the container image from a DockerHub repo.</span></span> <span data-ttu-id="4e982-129">Bild-parametern har formatet [lagringsplatsen] / [image name]</span><span class="sxs-lookup"><span data-stu-id="4e982-129">The image parameter takes the form [repo]/[image name]</span></span>
4. <span data-ttu-id="4e982-130">Om avbildningen inte har en arbetsbelastning startpunkten definierat, måste du uttryckligen ange inkommande kommandon med en kommaavgränsad uppsättning kommandon som ska köras i behållare som kommer att hålla den behållare som körs efter start.</span><span class="sxs-lookup"><span data-stu-id="4e982-130">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

![Service Fabric Yeoman-generator för behållare][sf-yeoman]

## <a name="deploy-the-application"></a><span data-ttu-id="4e982-132">Distribuera programmet</span><span class="sxs-lookup"><span data-stu-id="4e982-132">Deploy the application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="4e982-133">Med XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="4e982-133">Using XPlat CLI</span></span>
<span data-ttu-id="4e982-134">När du har skapat programmet kan du distribuera det till det lokala klustret med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="4e982-134">Once the application is built, you can deploy it to the local cluster using the Azure CLI.</span></span>

1. <span data-ttu-id="4e982-135">Anslut till det lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="4e982-135">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="4e982-136">Använd installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="4e982-136">Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="4e982-137">Öppna en webbläsare och gå till Service Fabric Explorer på http://localhost:19080/Explorer (ersätt localhost med den virtuella datorns privata IP om du använder Vagrant på Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="4e982-137">Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="4e982-138">Expandera programnoden och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="4e982-138">Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>
5. <span data-ttu-id="4e982-139">Använda avinstallera skriptet i mallen för att ta bort instansen av programmet och avregistrera programtypen.</span><span class="sxs-lookup"><span data-stu-id="4e982-139">Use the uninstall script provided in the template to delete the application instance and unregister the application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="4e982-140">Med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4e982-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="4e982-141">Se referens doc om hur du hanterar en [livscykel för program som använder Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="4e982-141">See the reference doc on managing an [application life cycle using the Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="4e982-142">Ett exempelprogram [utcheckningen Service Fabric behållarens kod exempel på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="4e982-142">For an example application, [checkout the Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="4e982-143">Lägga till fler tjänster till ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="4e982-143">Adding more services to an existing application</span></span>

<span data-ttu-id="4e982-144">Att lägga till en annan behållartjänst till ett program som redan har skapats med hjälp av `yo`, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e982-144">To add another container service to an application already created using `yo`, perform the following steps:</span></span>

1. <span data-ttu-id="4e982-145">Ändra katalogen till roten för det befintliga programmet.</span><span class="sxs-lookup"><span data-stu-id="4e982-145">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="4e982-146">Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="4e982-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="4e982-147">Kör `yo azuresfcontainer:AddService`</span><span class="sxs-lookup"><span data-stu-id="4e982-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="4e982-148">Paketera och distribuera en avbildning för behållaren manuellt</span><span class="sxs-lookup"><span data-stu-id="4e982-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="4e982-149">Manuellt paketera container service baseras på följande:</span><span class="sxs-lookup"><span data-stu-id="4e982-149">The process of manually packaging a containerized service is based on the following steps:</span></span>

1. <span data-ttu-id="4e982-150">Publicera behållarna i databasen.</span><span class="sxs-lookup"><span data-stu-id="4e982-150">Publish the containers to your repository.</span></span>
2. <span data-ttu-id="4e982-151">Skapa katalogstrukturen paketet.</span><span class="sxs-lookup"><span data-stu-id="4e982-151">Create the package directory structure.</span></span>
3. <span data-ttu-id="4e982-152">Redigera service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="4e982-152">Edit the service manifest file.</span></span>
4. <span data-ttu-id="4e982-153">Redigera programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="4e982-153">Edit the application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="4e982-154">Distribuera och aktivera en avbildning av behållare</span><span class="sxs-lookup"><span data-stu-id="4e982-154">Deploy and activate a container image</span></span>
<span data-ttu-id="4e982-155">I Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras.</span><span class="sxs-lookup"><span data-stu-id="4e982-155">In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="4e982-156">Om du vill distribuera och aktiverar en behållare, placerar du namnet på behållaren avbildningen till en `ContainerHost` element i service manifest.</span><span class="sxs-lookup"><span data-stu-id="4e982-156">To deploy and activate a container, put the name of the container image into a `ContainerHost` element in the service manifest.</span></span>

<span data-ttu-id="4e982-157">Tjänstmanifestet, lägga till en `ContainerHost` för startpunkten.</span><span class="sxs-lookup"><span data-stu-id="4e982-157">In the service manifest, add a `ContainerHost` for the entry point.</span></span> <span data-ttu-id="4e982-158">Ange den `ImageName` namnet på behållaren databasen och image.</span><span class="sxs-lookup"><span data-stu-id="4e982-158">Then set the `ImageName` to be the name of the container repository and image.</span></span> <span data-ttu-id="4e982-159">Följande partiella manifestet visar ett exempel på hur du distribuerar behållare som kallas `myimage:v1` från en databas som heter `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="4e982-159">The following partial manifest shows an example of how to deploy the container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="4e982-160">Du kan ange inkommande kommandon genom att ange den valfria `Commands` element med en kommaavgränsad uppsättning kommandon som ska köras i behållaren.</span><span class="sxs-lookup"><span data-stu-id="4e982-160">You can provide input commands by specifying the optional `Commands` element with a comma-delimited set of commands to run inside the container.</span></span>

> [!NOTE]
> <span data-ttu-id="4e982-161">Om bilden inte har en arbetsbelastning startpunkten definierats, så måste du uttryckligen ange inkommande kommandon i `Commands` element med en kommaavgränsad uppsättning kommandon som ska köras i behållare som kommer att hålla den behållare som körs efter start.</span><span class="sxs-lookup"><span data-stu-id="4e982-161">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands inside `Commands` element with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="4e982-162">Förstå resource-styrning</span><span class="sxs-lookup"><span data-stu-id="4e982-162">Understand resource governance</span></span>
<span data-ttu-id="4e982-163">Resurs-styrning är en funktion på behållaren som begränsar de resurser som behållaren kan använda på värden.</span><span class="sxs-lookup"><span data-stu-id="4e982-163">Resource governance is a capability of the container that restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="4e982-164">Den `ResourceGovernancePolicy`, som har angetts i applikationsmanifestet används för att deklarera gränserna för ett paket för service-kod.</span><span class="sxs-lookup"><span data-stu-id="4e982-164">The `ResourceGovernancePolicy`, which is specified in the application manifest is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="4e982-165">Gränserna kan anges för följande resurser:</span><span class="sxs-lookup"><span data-stu-id="4e982-165">Resource limits can be set for the following resources:</span></span>

* <span data-ttu-id="4e982-166">Minne</span><span class="sxs-lookup"><span data-stu-id="4e982-166">Memory</span></span>
* <span data-ttu-id="4e982-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="4e982-167">MemorySwap</span></span>
* <span data-ttu-id="4e982-168">CpuShares (CPU relativa viktade)</span><span class="sxs-lookup"><span data-stu-id="4e982-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="4e982-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="4e982-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="4e982-170">BlkioWeight (BlockIO relativa vikt).</span><span class="sxs-lookup"><span data-stu-id="4e982-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="4e982-171">I en framtida utgåva, stöd för att ange specifika block-i/o-gränser, till exempel IOPs, läsning och skrivning bit/s och andra ska inkluderas.</span><span class="sxs-lookup"><span data-stu-id="4e982-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="4e982-172">Autentisera en databas</span><span class="sxs-lookup"><span data-stu-id="4e982-172">Authenticate a repository</span></span>
<span data-ttu-id="4e982-173">Du kan behöva ange inloggningsuppgifter i behållaren databasen om du vill hämta en behållare.</span><span class="sxs-lookup"><span data-stu-id="4e982-173">To download a container, you might have to provide sign-in credentials to the container repository.</span></span> <span data-ttu-id="4e982-174">Logga in autentiseringsuppgifter anges i programmanifestet, används för att ange inloggningsinformation eller SSH-nyckeln för att ladda ned avbildningen behållare från avbildningslagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4e982-174">The sign-in credentials, specified in the application manifest, are used to specify the sign-in information, or SSH key, for downloading the container image from the image repository.</span></span> <span data-ttu-id="4e982-175">I följande exempel visas ett konto som heter *TestUser* tillsammans med lösenordet i klartext (*inte* rekommenderas):</span><span class="sxs-lookup"><span data-stu-id="4e982-175">The following example shows an account called *TestUser* along with the password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="4e982-176">Vi rekommenderar att du krypterar lösenordet genom att använda ett certifikat som har distribuerats till datorn.</span><span class="sxs-lookup"><span data-stu-id="4e982-176">We recommend that you encrypt the password by using a certificate that's deployed to the machine.</span></span>

<span data-ttu-id="4e982-177">I följande exempel visas ett konto som heter *TestUser*, där lösenordet som har krypterats med ett certifikat som kallas *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="4e982-177">The following example shows an account called *TestUser*, where the password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="4e982-178">Du kan använda den `Invoke-ServiceFabricEncryptText` PowerShell-kommando för att skapa den hemliga chiffertext för lösenordet.</span><span class="sxs-lookup"><span data-stu-id="4e982-178">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text for the password.</span></span> <span data-ttu-id="4e982-179">Mer information finns i artikeln [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="4e982-179">For more information, see the article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="4e982-180">Den privata nyckeln för certifikatet som används för att dekryptera lösenordet måste distribueras till den lokala datorn i en out-of-band-metod.</span><span class="sxs-lookup"><span data-stu-id="4e982-180">The private key of the certificate that's used to decrypt the password must be deployed to the local machine in an out-of-band method.</span></span> <span data-ttu-id="4e982-181">(Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar tjänstepaketet till datorn, kan det sedan dekryptera hemligheten.</span><span class="sxs-lookup"><span data-stu-id="4e982-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys the service package to the machine, it can decrypt the secret.</span></span> <span data-ttu-id="4e982-182">Med hjälp av hemlighet tillsammans med namnet på kontot kan den autentisera med behållaren lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4e982-182">By using the secret along with the account name, it can then authenticate with the container repository.</span></span>

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

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="4e982-183">Konfigurera portmappning behållaren port-till-värd</span><span class="sxs-lookup"><span data-stu-id="4e982-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="4e982-184">Du kan konfigurera en värd-port som används för att kommunicera med behållaren genom att ange en `PortBinding` i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="4e982-184">You can configure a host port used to communicate with the container by specifying a `PortBinding` in the application manifest.</span></span> <span data-ttu-id="4e982-185">Portbindningen mappar den port som tjänsten lyssnar i behållare till en port på värden.</span><span class="sxs-lookup"><span data-stu-id="4e982-185">The port binding maps the port to which the service is listening inside the container to a port on the host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="4e982-186">Konfigurera identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="4e982-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="4e982-187">Med hjälp av den `PortBinding` princip, kan du mappa en behållare port till en `Endpoint` i service manifest.</span><span class="sxs-lookup"><span data-stu-id="4e982-187">By using the `PortBinding` policy, you can map a container port to an `Endpoint` in the service manifest.</span></span> <span data-ttu-id="4e982-188">Slutpunkten `Endpoint1` kan ange en fast port (till exempel port 80).</span><span class="sxs-lookup"><span data-stu-id="4e982-188">The endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="4e982-189">Det kan också ange ingen port, och då en slumpmässigt vald port från klustrets programmet portintervallet är valt.</span><span class="sxs-lookup"><span data-stu-id="4e982-189">It can also specify no port at all, in which case a random port from the cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="4e982-190">Om du anger en slutpunkt med hjälp av den `Endpoint` tagg i tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten till Naming service.</span><span class="sxs-lookup"><span data-stu-id="4e982-190">If you specify an endpoint, using the `Endpoint` tag in the service manifest of a guest container, Service Fabric can automatically publish this endpoint to the Naming service.</span></span> <span data-ttu-id="4e982-191">Andra tjänster som körs i klustret kan därför att identifiera den här behållaren med hjälp av REST-frågor för att lösa.</span><span class="sxs-lookup"><span data-stu-id="4e982-191">Other services that are running in the cluster can thus discover this container using the REST queries for resolving.</span></span>

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

<span data-ttu-id="4e982-192">Genom att registrera med Naming service kan du enkelt göra behållare till en annan kommunikation i koden i din behållaren med hjälp av den [omvänd proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="4e982-192">By registering with the Naming service, you can easily do container-to-container communication in the code within your container by using the [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="4e982-193">Kommunikation utförs genom att tillhandahålla omvänd proxy http-lyssningsporten och namnet på de tjänster som du vill kommunicera med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="4e982-193">Communication is performed by providing the reverse proxy http listening port and the name of the services that you want to communicate with as environment variables.</span></span> <span data-ttu-id="4e982-194">Mer information finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4e982-194">For more information, see the next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="4e982-195">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="4e982-195">Configure and set environment variables</span></span>
<span data-ttu-id="4e982-196">Miljövariabler kan anges för varje kodpaketet i service manifest, både för tjänster som har distribuerats i behållare eller tjänster som distribueras som processer/gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="4e982-196">Environment variables can be specified for each code package in the service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="4e982-197">Dessa miljö variabelvärden kan åsidosättas i programmanifestet eller anges under distributionen som parametrar för programmet.</span><span class="sxs-lookup"><span data-stu-id="4e982-197">These environment variable values can be overridden specifically in the application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="4e982-198">Följande XML-kodfragment i tjänstmanifestet visar ett exempel på hur du anger miljövariabler för ett kodpaket:</span><span class="sxs-lookup"><span data-stu-id="4e982-198">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>

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

<span data-ttu-id="4e982-199">De här miljövariablerna kan åsidosättas på programnivå manifest:</span><span class="sxs-lookup"><span data-stu-id="4e982-199">These environment variables can be overridden at the application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="4e982-200">I föregående exempel vi angav ett explicit värde för den `HttpGateway` miljövariabeln (19000), medan vi anger du värdet för `BackendServiceName` parametern via den `[BackendSvc]` program-parametern.</span><span class="sxs-lookup"><span data-stu-id="4e982-200">In the previous example, we specified an explicit value for the `HttpGateway` environment variable (19000), while we set the value for `BackendServiceName` parameter via the `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="4e982-201">De här inställningarna kan du ange värdet för `BackendServiceName`värde när du distribuerar programmet och inte har ett fast värde i manifestet.</span><span class="sxs-lookup"><span data-stu-id="4e982-201">These settings enable you to specify the value for `BackendServiceName`value when you deploy the application and not have a fixed value in the manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="4e982-202">Slutföra exempel för programmet och tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="4e982-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="4e982-203">Ett exempel programmanifest följande:</span><span class="sxs-lookup"><span data-stu-id="4e982-203">An example application manifest follows:</span></span>

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

<span data-ttu-id="4e982-204">Ett exempel tjänstmanifestet (anges i föregående programmanifestet) visas nedan:</span><span class="sxs-lookup"><span data-stu-id="4e982-204">An example service manifest (specified in the preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4e982-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e982-205">Next steps</span></span>
<span data-ttu-id="4e982-206">Nu när du har använt ett container service lär du dig hur du hanterar livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="4e982-206">Now that you have deployed a containerized service, learn how to manage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="4e982-207">Översikt över Service Fabric och behållare</span><span class="sxs-lookup"><span data-stu-id="4e982-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="4e982-208">Interagera med Service Fabric-kluster med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4e982-208">Interacting with Service Fabric clusters using the Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="4e982-209">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="4e982-209">Related articles</span></span>

* [<span data-ttu-id="4e982-210">Kom igång med Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4e982-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="4e982-211">Kom igång med Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="4e982-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
