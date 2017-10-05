---
title: "Service Fabric och distribuera behållare | Microsoft Docs"
description: "Service Fabric och användning av behållare för att distribuera mikrotjänster program. Den här artikeln beskriver de funktioner som Service Fabric ger för behållare och hur du distribuerar en Windows-avbildning för behållare i ett kluster."
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
ms.openlocfilehash: 25d6b056421e71fa70ed20a39589f77dbbc25c69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-windows-container-to-service-fabric"></a><span data-ttu-id="50548-104">Distribuera en Windows-behållare till Service Fabric</span><span class="sxs-lookup"><span data-stu-id="50548-104">Deploy a Windows container to Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="50548-105">Distribuera Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="50548-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="50548-106">Distribuera Dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="50548-106">Deploy Docker Container</span></span>](service-fabric-deploy-container-linux.md)
> 
> 

<span data-ttu-id="50548-107">Den här artikeln vägleder dig genom processen att bygga av tjänster i Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="50548-107">This article walks you through the process of building containerized services in Windows containers.</span></span>

<span data-ttu-id="50548-108">Service Fabric har flera funktioner som hjälper dig med att skapa program som består av mikrotjänster som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="50548-108">Service Fabric has several capabilities that help you with building applications that are composed of microservices running inside containers.</span></span> 

<span data-ttu-id="50548-109">Funktionerna inkluderar:</span><span class="sxs-lookup"><span data-stu-id="50548-109">The capabilities include:</span></span>

* <span data-ttu-id="50548-110">Distribution av avbildningar för behållaren och aktivering</span><span class="sxs-lookup"><span data-stu-id="50548-110">Container image deployment and activation</span></span>
* <span data-ttu-id="50548-111">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="50548-111">Resource governance</span></span>
* <span data-ttu-id="50548-112">Autentisering för databasen</span><span class="sxs-lookup"><span data-stu-id="50548-112">Repository authentication</span></span>
* <span data-ttu-id="50548-113">Portmappning för behållaren port-till-värd</span><span class="sxs-lookup"><span data-stu-id="50548-113">Container port-to-host port mapping</span></span>
* <span data-ttu-id="50548-114">Identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="50548-114">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="50548-115">Möjligheten att konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="50548-115">Ability to configure and set environment variables</span></span>

<span data-ttu-id="50548-116">Nu ska vi titta på hur var och en av funktionerna fungerar när du paketerar ett container service ska tas med i ditt program.</span><span class="sxs-lookup"><span data-stu-id="50548-116">Let's look at how each of capabilities works when you're packaging a containerized service to be included in your application.</span></span>

## <a name="package-a-windows-container"></a><span data-ttu-id="50548-117">Paketet en Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="50548-117">Package a Windows container</span></span>
<span data-ttu-id="50548-118">När du paketerar en behållare kan du använda antingen en projektmall för Visual Studio eller [skapa programpaketet manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="50548-118">When you package a container, you can choose to use either a Visual Studio project template or [create the application package manually](#manually).</span></span>  <span data-ttu-id="50548-119">När du använder Visual Studio skapas application paketet struktur och manifest-filer av mallen nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="50548-119">When you use Visual Studio, the application package structure and manifest files are created by the New Project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="50548-120">Det enklaste sättet att paketera en befintlig behållare-avbildning till en tjänst är att använda Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50548-120">The easiest way to package an existing container image into a service is to use Visual Studio.</span></span>

## <a name="use-visual-studio-to-package-an-existing-container-image"></a><span data-ttu-id="50548-121">Använda Visual Studio för att paketera en befintlig avbildning i behållaren</span><span class="sxs-lookup"><span data-stu-id="50548-121">Use Visual Studio to package an existing container image</span></span>
<span data-ttu-id="50548-122">Visual Studio har ett Service Fabric-tjänstmall som hjälper dig att distribuera en behållare till ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="50548-122">Visual Studio provides a Service Fabric service template to help you deploy a container to a Service Fabric cluster.</span></span>

1. <span data-ttu-id="50548-123">Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="50548-123">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="50548-124">Välj **gäst behållaren** som tjänstmallen.</span><span class="sxs-lookup"><span data-stu-id="50548-124">Choose **Guest Container** as the service template.</span></span>
3. <span data-ttu-id="50548-125">Välj **avbildningsnamn** och ange sökvägen till bilden i behållaren databasen.</span><span class="sxs-lookup"><span data-stu-id="50548-125">Choose **Image Name** and provide the path to the image in your container repository.</span></span> <span data-ttu-id="50548-126">Till exempel `myrepo/myimage:v1` i https://hub.docker.com</span><span class="sxs-lookup"><span data-stu-id="50548-126">For example, `myrepo/myimage:v1` in https://hub.docker.com</span></span>
4. <span data-ttu-id="50548-127">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="50548-127">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="50548-128">Om av tjänsten måste en slutpunkt för kommunikation, du kan nu lägga till protokollet, porten och typ ServiceManifest.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="50548-128">If your containerized service needs an endpoint for communication, you can now add the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="50548-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="50548-129">For example:</span></span> 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    <span data-ttu-id="50548-130">Genom att tillhandahålla den `UriScheme`, Service Fabric automatiskt registrerar behållaren slutpunkten med tjänsten Naming för synlighet.</span><span class="sxs-lookup"><span data-stu-id="50548-130">By providing the `UriScheme`, Service Fabric automatically registers the container endpoint with the Naming service for discoverability.</span></span> <span data-ttu-id="50548-131">Porten kan vara antingen fasta (som visas i föregående exempel) eller dynamiskt allokerade.</span><span class="sxs-lookup"><span data-stu-id="50548-131">The port can either be fixed (as shown in the preceding example) or dynamically allocated.</span></span> <span data-ttu-id="50548-132">Om du inte anger en port är den dynamiskt allokerade från portintervall program (som skulle hända med alla service).</span><span class="sxs-lookup"><span data-stu-id="50548-132">If you don't specify a port, it is dynamically allocated from the application port range (as would happen with any service).</span></span>
    <span data-ttu-id="50548-133">Du måste också konfigurera värden portmappning behållaren genom att ange en `PortBinding` princip i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="50548-133">You also need to configure the container to host port mapping by specifying a `PortBinding` policy in the application manifest.</span></span> <span data-ttu-id="50548-134">Mer information finns i [konfigurera behållare till värden portmappning](#Portsection).</span><span class="sxs-lookup"><span data-stu-id="50548-134">For more information, see [Configure container to host port mapping](#Portsection).</span></span>
6. <span data-ttu-id="50548-135">Om din behållaren måste resursen styrning och lägger till en `ResourceGovernancePolicy`.</span><span class="sxs-lookup"><span data-stu-id="50548-135">If your container needs resource governance then add a `ResourceGovernancePolicy`.</span></span>
8. <span data-ttu-id="50548-136">Om behållaren behöver autentiseras med en privat lagringsplats lägger du till `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="50548-136">If your container needs to authenticate with a private repository, then add `RepositoryCredentials`.</span></span>
7. <span data-ttu-id="50548-137">Om du kör på en Windows Server 2016-dator med aktiverat stöd för behållare, kan du använda paketet och publicera åtgärder för att distribuera till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="50548-137">If you are running on a Windows Server 2016 machine with container support enabled, you can use the package and publish action to deploy to your local cluster.</span></span> 
8. <span data-ttu-id="50548-138">När du är klar kan du publicera program till ett kluster eller kontrollera i lösningen till källkontroll.</span><span class="sxs-lookup"><span data-stu-id="50548-138">When ready, you can publish the application to a remote cluster or check in the solution to source control.</span></span> 

<span data-ttu-id="50548-139">Ett exempel checka ut den [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="50548-139">For an example, checkout the [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="creating-a-windows-server-2016-cluster"></a><span data-ttu-id="50548-140">Skapa ett kluster för Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="50548-140">Creating a Windows Server 2016 cluster</span></span>
<span data-ttu-id="50548-141">Om du vill distribuera av programmet, måste du skapa ett kluster som kör Windows Server 2016 med aktiverat stöd för behållaren.</span><span class="sxs-lookup"><span data-stu-id="50548-141">To deploy your containerized application, you need to create a cluster running Windows Server 2016 with container support enabled.</span></span> <span data-ttu-id="50548-142">Klustret kan köras lokalt eller distribueras via Azure Resource Manager i Azure.</span><span class="sxs-lookup"><span data-stu-id="50548-142">Your cluster may be running locally, or deployed via Azure Resource Manager in Azure.</span></span> 

<span data-ttu-id="50548-143">Om du vill distribuera ett kluster med Azure Resource Manager, Välj den **Windows Server 2016 med behållare** bild-alternativet i Azure.</span><span class="sxs-lookup"><span data-stu-id="50548-143">To deploy a cluster using Azure Resource Manager, choose the **Windows Server 2016 with Containers** image option in Azure.</span></span> <span data-ttu-id="50548-144">Se artikeln [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="50548-144">See the article [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="50548-145">Kontrollera att du använder följande Azure Resource Manager-inställningar:</span><span class="sxs-lookup"><span data-stu-id="50548-145">Ensure that you use the following Azure Resource Manager settings:</span></span>

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
<span data-ttu-id="50548-146">Du kan också använda den [fem nod Azure Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) att skapa ett kluster.</span><span class="sxs-lookup"><span data-stu-id="50548-146">You can also use the [Five Node Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) to create a cluster.</span></span> <span data-ttu-id="50548-147">Du kan också läsa en grupp [blogginlägget](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) med hjälp av Service Fabric- och Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="50548-147">Alternatively read a community [blog post](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) on using Service Fabric and Windows containers.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="50548-148">Paketera och distribuera en avbildning för behållaren manuellt</span><span class="sxs-lookup"><span data-stu-id="50548-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="50548-149">Manuellt paketera container service baseras på följande:</span><span class="sxs-lookup"><span data-stu-id="50548-149">The process of manually packaging a containerized service is based on the following steps:</span></span>

1. <span data-ttu-id="50548-150">Publicera behållarna i databasen.</span><span class="sxs-lookup"><span data-stu-id="50548-150">Publish the containers to your repository.</span></span>
2. <span data-ttu-id="50548-151">Skapa katalogstrukturen paketet.</span><span class="sxs-lookup"><span data-stu-id="50548-151">Create the package directory structure.</span></span>
3. <span data-ttu-id="50548-152">Redigera service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="50548-152">Edit the service manifest file.</span></span>
4. <span data-ttu-id="50548-153">Redigera programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="50548-153">Edit the application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="50548-154">Distribuera och aktivera en avbildning av behållare</span><span class="sxs-lookup"><span data-stu-id="50548-154">Deploy and activate a container image</span></span>
<span data-ttu-id="50548-155">I Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras.</span><span class="sxs-lookup"><span data-stu-id="50548-155">In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="50548-156">Om du vill distribuera och aktiverar en behållare, placerar du namnet på behållaren avbildningen till en `ContainerHost` element i service manifest.</span><span class="sxs-lookup"><span data-stu-id="50548-156">To deploy and activate a container, put the name of the container image into a `ContainerHost` element in the service manifest.</span></span>

<span data-ttu-id="50548-157">Tjänstmanifestet, lägga till en `ContainerHost` för startpunkten.</span><span class="sxs-lookup"><span data-stu-id="50548-157">In the service manifest, add a `ContainerHost` for the entry point.</span></span> <span data-ttu-id="50548-158">Ange den `ImageName` namnet på behållaren databasen och image.</span><span class="sxs-lookup"><span data-stu-id="50548-158">Then set the `ImageName` to be the name of the container repository and image.</span></span> <span data-ttu-id="50548-159">Följande partiella manifestet visar ett exempel på hur du distribuerar behållare som kallas `myimage:v1` från en databas som heter `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="50548-159">The following partial manifest shows an example of how to deploy the container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="50548-160">Du kan ange valfri kommandon som ska köras vid start av behållare under den `Commands` element.</span><span class="sxs-lookup"><span data-stu-id="50548-160">You can specify optional commands to run upon starting the container under the `Commands` element.</span></span> <span data-ttu-id="50548-161">För flera kommandon, komma-avgränsa dem.</span><span class="sxs-lookup"><span data-stu-id="50548-161">For multiple commands, comma-delimit them.</span></span> 

## <a name="understand-resource-governance"></a><span data-ttu-id="50548-162">Förstå resource-styrning</span><span class="sxs-lookup"><span data-stu-id="50548-162">Understand resource governance</span></span>
<span data-ttu-id="50548-163">Resurs-styrning är en funktion på behållaren som begränsar de resurser som behållaren kan använda på värden.</span><span class="sxs-lookup"><span data-stu-id="50548-163">Resource governance is a capability of the container that restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="50548-164">Den `ResourceGovernancePolicy`, som har angetts i applikationsmanifestet används för att deklarera gränserna för ett paket för service-kod.</span><span class="sxs-lookup"><span data-stu-id="50548-164">The `ResourceGovernancePolicy`, which is specified in the application manifest is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="50548-165">Gränserna kan anges för följande resurser:</span><span class="sxs-lookup"><span data-stu-id="50548-165">Resource limits can be set for the following resources:</span></span>

* <span data-ttu-id="50548-166">Minne</span><span class="sxs-lookup"><span data-stu-id="50548-166">Memory</span></span>
* <span data-ttu-id="50548-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="50548-167">MemorySwap</span></span>
* <span data-ttu-id="50548-168">CpuShares (CPU relativa viktade)</span><span class="sxs-lookup"><span data-stu-id="50548-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="50548-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="50548-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="50548-170">BlkioWeight (BlockIO relativa vikt).</span><span class="sxs-lookup"><span data-stu-id="50548-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="50548-171">Stöd för att ange specifika blockera i/o-gränser som IOPs och läsning/skrivning BPS planeras för framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="50548-171">Support for specifying specific block IO limits such as IOPs, read/write BPS, and others are planned for a future release.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="50548-172">Autentisera en databas</span><span class="sxs-lookup"><span data-stu-id="50548-172">Authenticate a repository</span></span>
<span data-ttu-id="50548-173">Du kan behöva ange inloggningsuppgifter i behållaren databasen om du vill hämta en behållare.</span><span class="sxs-lookup"><span data-stu-id="50548-173">To download a container, you might have to provide sign-in credentials to the container repository.</span></span> <span data-ttu-id="50548-174">Logga in autentiseringsuppgifter anges i programmanifestet, används för att ange inloggningsinformation eller SSH-nyckeln för att ladda ned avbildningen behållare från avbildningslagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="50548-174">The sign-in credentials, specified in the application manifest, are used to specify the sign-in information, or SSH key, for downloading the container image from the image repository.</span></span> <span data-ttu-id="50548-175">I följande exempel visas ett konto som heter *TestUser* tillsammans med lösenordet i klartext (*inte* rekommenderas):</span><span class="sxs-lookup"><span data-stu-id="50548-175">The following example shows an account called *TestUser* along with the password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="50548-176">Vi rekommenderar att du krypterar lösenordet genom att använda ett certifikat som har distribuerats till datorn.</span><span class="sxs-lookup"><span data-stu-id="50548-176">We recommend that you encrypt the password by using a certificate that's deployed to the machine.</span></span>

<span data-ttu-id="50548-177">I följande exempel visas ett konto som heter *TestUser*, där lösenordet som har krypterats med ett certifikat som kallas *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="50548-177">The following example shows an account called *TestUser*, where the password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="50548-178">Du kan använda den `Invoke-ServiceFabricEncryptText` PowerShell-kommando för att skapa den hemliga chiffertext för lösenordet.</span><span class="sxs-lookup"><span data-stu-id="50548-178">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text for the password.</span></span> <span data-ttu-id="50548-179">Mer information finns i artikeln [hantera hemligheter i Service Fabric program](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="50548-179">For more information, see the article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="50548-180">Den privata nyckeln för certifikatet som används för att dekryptera lösenordet måste distribueras till den lokala datorn i en out-of-band-metod.</span><span class="sxs-lookup"><span data-stu-id="50548-180">The private key of the certificate that's used to decrypt the password must be deployed to the local machine in an out-of-band method.</span></span> <span data-ttu-id="50548-181">(Den här metoden är Azure Resource Manager i Azure.) När Service Fabric distribuerar tjänstepaketet till datorn, kan det sedan dekryptera hemligheten.</span><span class="sxs-lookup"><span data-stu-id="50548-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys the service package to the machine, it can decrypt the secret.</span></span> <span data-ttu-id="50548-182">Med hjälp av hemlighet tillsammans med namnet på kontot kan den autentisera med behållaren lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="50548-182">By using the secret along with the account name, it can then authenticate with the container repository.</span></span>

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

## <span data-ttu-id="50548-183"><a name ="Portsection"></a>Konfigurera behållare för att värden portmappning</span><span class="sxs-lookup"><span data-stu-id="50548-183"><a name ="Portsection"></a> Configure container to host port mapping</span></span>
<span data-ttu-id="50548-184">Du kan konfigurera en värd-port som används för att kommunicera med behållaren genom att ange en `PortBinding` i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="50548-184">You can configure a host port used to communicate with the container by specifying a `PortBinding` in the application manifest.</span></span> <span data-ttu-id="50548-185">Portbindningen mappar den port som tjänsten lyssnar i behållare till en port på värden.</span><span class="sxs-lookup"><span data-stu-id="50548-185">The port binding maps the port to which the service is listening inside the container to a port on the host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="50548-186">Konfigurera identifiering av behållare till en annan och kommunikation</span><span class="sxs-lookup"><span data-stu-id="50548-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="50548-187">Du kan använda den `PortBinding` element att mappa en behållare-port till en slutpunkt i service manifest.</span><span class="sxs-lookup"><span data-stu-id="50548-187">You can use the `PortBinding` element to map a container port to an endpoint in the service manifest.</span></span> <span data-ttu-id="50548-188">I följande exempel slutpunkten `Endpoint1` och anger en fast port 8905.</span><span class="sxs-lookup"><span data-stu-id="50548-188">In the following example, the endpoint `Endpoint1` specifies a fixed port, 8905.</span></span> <span data-ttu-id="50548-189">Det kan också ange ingen port, och då en slumpmässigt vald port från klustrets programmet portintervallet är valt.</span><span class="sxs-lookup"><span data-stu-id="50548-189">It can also specify no port at all, in which case a random port from the cluster's application port range is chosen for you.</span></span>


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
<span data-ttu-id="50548-190">Om du anger en slutpunkt med hjälp av den `Endpoint` tagg i tjänstmanifestet av en gäst-behållare, Service Fabric kan automatiskt publicera den här slutpunkten till Naming service.</span><span class="sxs-lookup"><span data-stu-id="50548-190">If you specify an endpoint, using the `Endpoint` tag in the service manifest of a guest container, Service Fabric can automatically publish this endpoint to the Naming service.</span></span> <span data-ttu-id="50548-191">Andra tjänster som körs i klustret kan därför att identifiera den här behållaren med hjälp av REST-frågor för att lösa.</span><span class="sxs-lookup"><span data-stu-id="50548-191">Other services that are running in the cluster can thus discover this container using the REST queries for resolving.</span></span>

<span data-ttu-id="50548-192">Genom att registrera med Naming service kan du utföra behållare till en annan kommunikation i en behållare med den [omvänd proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="50548-192">By registering with the Naming service, you can perform container-to-container communication within your container by using the [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="50548-193">Kommunikation utförs genom att tillhandahålla omvänd proxy http-lyssningsporten och namnet på de tjänster som du vill kommunicera med som miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="50548-193">Communication is performed by providing the reverse proxy http listening port and the name of the services that you want to communicate with as environment variables.</span></span> <span data-ttu-id="50548-194">Mer information finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="50548-194">For more information, see the next section.</span></span> 

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="50548-195">Konfigurera och ange miljövariabler</span><span class="sxs-lookup"><span data-stu-id="50548-195">Configure and set environment variables</span></span>
<span data-ttu-id="50548-196">Miljövariabler kan anges för varje kodpaketet i tjänstmanifestet.</span><span class="sxs-lookup"><span data-stu-id="50548-196">Environment variables can be specified for each code package in the service manifest.</span></span> <span data-ttu-id="50548-197">Den här funktionen är tillgänglig för alla tjänster oavsett om de har distribueras som behållare eller processer, eller körbara gäster.</span><span class="sxs-lookup"><span data-stu-id="50548-197">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="50548-198">Du kan åsidosätta värden för miljövariabler i applikationsmanifestet eller ange dem under distributionen som programparametrar.</span><span class="sxs-lookup"><span data-stu-id="50548-198">You can override environment variable values in the application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="50548-199">Följande XML-kodfragment i tjänstmanifestet visar ett exempel på hur du anger miljövariabler för ett kodpaket:</span><span class="sxs-lookup"><span data-stu-id="50548-199">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>

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

<span data-ttu-id="50548-200">De här miljövariablerna kan åsidosättas på programnivå manifest:</span><span class="sxs-lookup"><span data-stu-id="50548-200">These environment variables can be overridden at the application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="50548-201">I föregående exempel vi angav ett explicit värde för den `HttpGateway` miljövariabeln (19000), medan vi anger du värdet för `BackendServiceName` parametern via den `[BackendSvc]` program-parametern.</span><span class="sxs-lookup"><span data-stu-id="50548-201">In the previous example, we specified an explicit value for the `HttpGateway` environment variable (19000), while we set the value for `BackendServiceName` parameter via the `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="50548-202">De här inställningarna kan du ange värdet för `BackendServiceName`värde när du distribuerar programmet och inte har ett fast värde i manifestet.</span><span class="sxs-lookup"><span data-stu-id="50548-202">These settings enable you to specify the value for `BackendServiceName`value when you deploy the application and not have a fixed value in the manifest.</span></span>

## <a name="configure-isolation-mode"></a><span data-ttu-id="50548-203">Konfigurera isoleringsläge</span><span class="sxs-lookup"><span data-stu-id="50548-203">Configure isolation mode</span></span>

<span data-ttu-id="50548-204">Windows stöder två arbetslägen för behållare - processen och Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="50548-204">Windows supports two isolation modes for containers - process and Hyper-V.</span></span>  <span data-ttu-id="50548-205">Om processisoleringsläget används delar alla behållare som körs på samma värddator kärna med värden.</span><span class="sxs-lookup"><span data-stu-id="50548-205">With the process isolation mode, all the containers running on the same host machine share the kernel with the host.</span></span> <span data-ttu-id="50548-206">Om Hyper-V-isoleringsläget används isoleras kärnorna mellan varje Hyper-V-behållare och behållarvärden.</span><span class="sxs-lookup"><span data-stu-id="50548-206">With the Hyper-V isolation mode, the kernels are isolated between each Hyper-V container and the container host.</span></span> <span data-ttu-id="50548-207">Isoleringsläge har angetts i den `ContainerHostPolicies` tagg i programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="50548-207">The isolation mode is specified in the `ContainerHostPolicies` tag in the application manifest file.</span></span>  <span data-ttu-id="50548-208">Isoleringslägena som kan anges är `process`, `hyperv` och `default`.</span><span class="sxs-lookup"><span data-stu-id="50548-208">The isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="50548-209">Den `default` isoleringsläge som standard `process` på Windows Server-värdar och standardvärdet är `hyperv` på Windows 10-värdar.</span><span class="sxs-lookup"><span data-stu-id="50548-209">The `default` isolation mode defaults to `process` on Windows Server hosts, and defaults to `hyperv` on Windows 10 hosts.</span></span>  <span data-ttu-id="50548-210">Följande kodfragment visar hur isoleringsläget har angetts i applikationsmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="50548-210">The following snippet shows how the isolation mode is specified in the application manifest file.</span></span>

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="50548-211">Slutföra exempel för programmet och tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="50548-211">Complete examples for application and service manifest</span></span>

<span data-ttu-id="50548-212">Ett exempel programmanifest följande:</span><span class="sxs-lookup"><span data-stu-id="50548-212">An example application manifest follows:</span></span>

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

<span data-ttu-id="50548-213">Ett exempel tjänstmanifestet (anges i föregående programmanifestet) visas nedan:</span><span class="sxs-lookup"><span data-stu-id="50548-213">An example service manifest (specified in the preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="50548-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50548-214">Next steps</span></span>
<span data-ttu-id="50548-215">Nu när du har använt ett container service lär du dig hur du hanterar livscykeln genom att läsa [Service Fabric application livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="50548-215">Now that you have deployed a containerized service, learn how to manage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="50548-216">Översikt över Service Fabric och behållare</span><span class="sxs-lookup"><span data-stu-id="50548-216">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* <span data-ttu-id="50548-217">Ett exempel utcheckningen [kodexempel för Service Fabric-behållare på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="50548-217">For an example, checkout [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>
