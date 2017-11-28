---
title: aaaAzure Service Fabric-programdistribution | Microsoft Docs
description: "Använd hello FabricClient APIs toodeploy och ta bort program i Service Fabric."
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
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a><span data-ttu-id="74146-103">Distribuera och ta bort program med hjälp av FabricClient</span><span class="sxs-lookup"><span data-stu-id="74146-103">Deploy and remove applications using FabricClient</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="74146-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="74146-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="74146-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74146-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="74146-106">FabricClient-API:er</span><span class="sxs-lookup"><span data-stu-id="74146-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="74146-107">När en [programtyp har paketerats][10], den är klar för distribution till Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="74146-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="74146-108">Distributionen omfattar hello följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="74146-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="74146-109">Överför hello programmet paketet toohello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="74146-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="74146-110">Registrera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="74146-110">Register hello application type</span></span>
3. <span data-ttu-id="74146-111">Skapa hello programinstansen</span><span class="sxs-lookup"><span data-stu-id="74146-111">Create hello application instance</span></span>

<span data-ttu-id="74146-112">När ett program distribueras och kör en instans i hello kluster kan du ta bort hello programinstansen och dess programtyp.</span><span class="sxs-lookup"><span data-stu-id="74146-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="74146-113">toocompletely ta bort ett program från hello kluster omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="74146-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="74146-114">Ta bort (eller ta bort) hello kör programinstansen</span><span class="sxs-lookup"><span data-stu-id="74146-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="74146-115">Avregistrera hello programtyp om du inte längre behöver</span><span class="sxs-lookup"><span data-stu-id="74146-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="74146-116">Ta bort hello programpaketet från hello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="74146-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="74146-117">Om du använder [Visual Studio för att distribuera och felsöka program](service-fabric-publish-app-remote-cluster.md) på klustret för lokal utveckling hello alla ovanstående steg hanteras automatiskt via ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="74146-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="74146-118">Det här skriptet finns i hello *skript* för hello projektet.</span><span class="sxs-lookup"><span data-stu-id="74146-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="74146-119">Den här artikeln innehåller bakgrund på vad skriptet gör så att du kan utföra samma åtgärder utanför Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="74146-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="74146-120">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="74146-120">Connect toohello cluster</span></span>
<span data-ttu-id="74146-121">Ansluta toohello klustret genom att skapa en [FabricClient](/dotnet/api/system.fabric.fabricclient) instansen innan du kör någon av hello kodexempel i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="74146-121">Connect toohello cluster by creating a [FabricClient](/dotnet/api/system.fabric.fabricclient) instance before you run any of hello code examples in this article.</span></span> <span data-ttu-id="74146-122">Exempel på anslutande tooa lokal utveckling kluster eller ett kluster eller kluster som skyddas med Azure Active Directory, X509 certifikat eller Windows Active Directory finns [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span><span class="sxs-lookup"><span data-stu-id="74146-122">For examples of connecting tooa local development cluster or a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span></span> <span data-ttu-id="74146-123">tooconnect toohello lokal utveckling klustret, kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="74146-123">tooconnect toohello local development cluster, run hello following:</span></span>

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a><span data-ttu-id="74146-124">Överför hello programpaket</span><span class="sxs-lookup"><span data-stu-id="74146-124">Upload hello application package</span></span>
<span data-ttu-id="74146-125">Anta att du skapar och paketerar ett program med namnet *MyApplication* i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74146-125">Suppose you build and package an application named *MyApplication* in Visual Studio.</span></span> <span data-ttu-id="74146-126">Som standard är hello programmets typnamn som anges i hello ApplicationManifest.xml ”MyApplicationType”.</span><span class="sxs-lookup"><span data-stu-id="74146-126">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="74146-127">hello-programpaket som innehåller hello nödvändiga programmanifestet, service manifest och paket i kod-config-data, finns i *C:\Users\&lt; användarnamnet&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="74146-127">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\&lt;username&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span>

<span data-ttu-id="74146-128">Ladda upp programpaket för hello placeras på en plats som kan nås av hello interna Service Fabric-komponenter.</span><span class="sxs-lookup"><span data-stu-id="74146-128">Uploading hello application package puts it in a location that's accessible by hello internal Service Fabric components.</span></span> <span data-ttu-id="74146-129">Service Fabric verifierar hello programpaketet under hello registrering av hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="74146-129">Service Fabric verifies hello application package during hello registration of hello application package.</span></span> <span data-ttu-id="74146-130">Om du vill tooverify hello programpaket lokalt (d.v.s. innan du laddar upp) använda hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="74146-130">However, if you want tooverify hello application package locally (i.e., before uploading), use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="74146-131">Hej [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API överför hello paketet toohello klustret avbildningen appbutik.</span><span class="sxs-lookup"><span data-stu-id="74146-131">hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uploads hello application package toohello cluster image store.</span></span> 

<span data-ttu-id="74146-132">Om hello programpaket är stort och/eller har många filer, kan du [komprimera](service-fabric-package-apps.md#compress-a-package) och kopiera den toohello image store med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="74146-132">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package) and copy it toohello image store using PowerShell.</span></span> <span data-ttu-id="74146-133">hello komprimeringen minskar hello storlek och hello antal filer.</span><span class="sxs-lookup"><span data-stu-id="74146-133">hello compression reduces hello size and hello number of files.</span></span>

<span data-ttu-id="74146-134">Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="74146-134">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="74146-135">Registrera hello programpaket</span><span class="sxs-lookup"><span data-stu-id="74146-135">Register hello application package</span></span>
<span data-ttu-id="74146-136">hello programtypen och versionen som deklarerats i hello programmanifestet blir tillgängliga för användning när hello programpaketet har registrerats.</span><span class="sxs-lookup"><span data-stu-id="74146-136">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="74146-137">hello system läser hello paketet på hello föregående steg, verifierar hello paketet, bearbetar hello paketets innehåll och kopierar hello bearbetas paketplatsen tooan internt system.</span><span class="sxs-lookup"><span data-stu-id="74146-137">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="74146-138">Hej [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registren hello programtyp i hello klustret och gör den tillgänglig för distribution.</span><span class="sxs-lookup"><span data-stu-id="74146-138">hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers hello application type in hello cluster and make it available for deployment.</span></span>

<span data-ttu-id="74146-139">Hej [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API ger information om alla programtyper av som har registrerats.</span><span class="sxs-lookup"><span data-stu-id="74146-139">hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API provides information about all successfully registered application types.</span></span> <span data-ttu-id="74146-140">Du kan använda den här API-toodetermine när hello registreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="74146-140">You can use this API toodetermine when hello registration is done.</span></span>

## <a name="create-an-application-instance"></a><span data-ttu-id="74146-141">Skapa en instans av programmet</span><span class="sxs-lookup"><span data-stu-id="74146-141">Create an application instance</span></span>
<span data-ttu-id="74146-142">Du kan skapa en instans av ett program från alla program som har registrerats med hjälp av hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="74146-142">You can instantiate an application from any application type that has been registered successfully by using hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span></span> <span data-ttu-id="74146-143">hello-namnet för varje program måste börja med hello *”fabric”:* schemat och måste vara unikt för varje förekomst av programmet (inom ett kluster).</span><span class="sxs-lookup"><span data-stu-id="74146-143">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance (within a cluster).</span></span> <span data-ttu-id="74146-144">Alla standardtjänster som definierats i hello programmanifestet av hello Målprogramstyp skapas också.</span><span class="sxs-lookup"><span data-stu-id="74146-144">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

<span data-ttu-id="74146-145">Flera instanser av programmet kan skapas för en viss version av typen registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="74146-145">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="74146-146">Varje instans av programmet körs i isolering med sin egen arbetskatalog och processer.</span><span class="sxs-lookup"><span data-stu-id="74146-146">Each application instance runs in isolation, with its own working directory and set of processes.</span></span>

<span data-ttu-id="74146-147">toosee som heter program och tjänster som körs i hello klustret, kör hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) och [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API: er.</span><span class="sxs-lookup"><span data-stu-id="74146-147">toosee which named applications and services are running in hello cluster, run hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) and [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) APIs.</span></span>

## <a name="create-a-service-instance"></a><span data-ttu-id="74146-148">Skapa en instans av tjänsten</span><span class="sxs-lookup"><span data-stu-id="74146-148">Create a service instance</span></span>
<span data-ttu-id="74146-149">Du kan skapa en instans av en tjänst från en typ med hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="74146-149">You can instantiate a service from a service type using hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span></span>  <span data-ttu-id="74146-150">Om hello-tjänsten har deklarerats som en standardtjänst i programmanifestet hello instansieras hello-tjänsten när programmet hello instansieras.</span><span class="sxs-lookup"><span data-stu-id="74146-150">If hello service is declared as a default service in hello application manifest, hello service is instantiated when hello application is instantiated.</span></span>  <span data-ttu-id="74146-151">Anropar hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API för en tjänst som redan har instansierats returneras ett undantag av typen FabricException som innehåller en felkod med värdet FabricErrorCode.ServiceAlreadyExists.</span><span class="sxs-lookup"><span data-stu-id="74146-151">Calling hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API for a service that is already instantiated will return an exception of type FabricException containing an error code with a value of FabricErrorCode.ServiceAlreadyExists.</span></span>

## <a name="remove-a-service-instance"></a><span data-ttu-id="74146-152">Ta bort en instans av tjänsten</span><span class="sxs-lookup"><span data-stu-id="74146-152">Remove a service instance</span></span>
<span data-ttu-id="74146-153">När en instans av tjänsten är inte längre behövs, kan du ta bort den hello kör programinstansen genom att anropa hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="74146-153">When a service instance is no longer needed, you can remove it from hello running application instance by calling hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span></span>  

> [!WARNING]
> <span data-ttu-id="74146-154">Den här åtgärden kan inte ångras och tillståndet för tjänsten kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="74146-154">This operation cannot be reversed, and service state cannot be recovered.</span></span>

## <a name="remove-an-application-instance"></a><span data-ttu-id="74146-155">Ta bort en instans av programmet</span><span class="sxs-lookup"><span data-stu-id="74146-155">Remove an application instance</span></span>
<span data-ttu-id="74146-156">När en instans av programmet inte längre behövs, om du permanent ta bort den namn med hjälp av hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="74146-156">When an application instance is no longer needed, you can permanently remove it by name using hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span></span> <span data-ttu-id="74146-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) tar automatiskt bort alla tjänster som tillhör toohello program samt, permanent tar du bort alla tillstånd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="74146-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span>

> [!WARNING]
> <span data-ttu-id="74146-158">Den här åtgärden kan inte ångras och går inte att återställa programtillstånd.</span><span class="sxs-lookup"><span data-stu-id="74146-158">This operation cannot be reversed, and application state cannot be recovered.</span></span>

## <a name="unregister-an-application-type"></a><span data-ttu-id="74146-159">Avregistrera en programtyp</span><span class="sxs-lookup"><span data-stu-id="74146-159">Unregister an application type</span></span>
<span data-ttu-id="74146-160">När en viss version av en typ av program inte längre behövs, bör du avregistrera den viss versionen av hello programtyp med hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="74146-160">When a particular version of an application type is no longer needed, you should unregister that particular version of hello application type using hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span></span> <span data-ttu-id="74146-161">Avregistrerar oanvända versioner av programmet typer versioner lagringsutrymme som används av hello image store.</span><span class="sxs-lookup"><span data-stu-id="74146-161">Unregistering unused versions of application types releases storage space used by hello image store.</span></span> <span data-ttu-id="74146-162">En version av en typ av program kan avregistreras så länge inga program instansieras mot den här versionen av hello programtyp och inga väntande programuppgraderingar refererar till den här versionen av hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="74146-162">A version of an application type can be unregistered as long as no applications are instantiated against that version of hello application type and no pending application upgrades are referencing that version of hello application type.</span></span>

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="74146-163">Ta bort ett programpaket från hello avbildningsarkivet</span><span class="sxs-lookup"><span data-stu-id="74146-163">Remove an application package from hello image store</span></span>
<span data-ttu-id="74146-164">När ett programpaket inte längre behövs, kan du ta bort den från hello image store toofree system-resurser med hjälp av hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span><span class="sxs-lookup"><span data-stu-id="74146-164">When an application package is no longer needed, you can delete it from hello image store toofree up system resources using hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="74146-165">Felsökning</span><span class="sxs-lookup"><span data-stu-id="74146-165">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="74146-166">Kopiera ServiceFabricApplicationPackage begär en ImageStoreConnectionString</span><span class="sxs-lookup"><span data-stu-id="74146-166">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="74146-167">hello Service Fabric SDK miljö har redan hello rätt standardvärden ställa in.</span><span class="sxs-lookup"><span data-stu-id="74146-167">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="74146-168">Men om det behövs, hello ImageStoreConnectionString för alla kommandon bör matcha hello-värde som hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="74146-168">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="74146-169">Du kan hitta hello ImageStoreConnectionString i hello klustermanifestet hämtades med hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) och Get-ImageStoreConnectionStringFromClusterManifest kommandon:</span><span class="sxs-lookup"><span data-stu-id="74146-169">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="74146-170">Hej **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, som är del av hello Service Fabric SDK PowerShell-modulen är används tooget hello bild lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="74146-170">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="74146-171">tooimport hello SDK modul, kör:</span><span class="sxs-lookup"><span data-stu-id="74146-171">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


<span data-ttu-id="74146-172">Hej ImageStoreConnectionString hittades i klustermanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="74146-172">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="74146-173">Se [förstå hello image store-anslutningssträng](service-fabric-image-store-connection-string.md) för ytterligare information om hello image store och avbildningen lagra anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="74146-173">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="74146-174">Distribuera stora programpaket</span><span class="sxs-lookup"><span data-stu-id="74146-174">Deploy large application package</span></span>
<span data-ttu-id="74146-175">Problem: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API tiden av stora programpaket (ordning GB).</span><span class="sxs-lookup"><span data-stu-id="74146-175">Issue: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API times out for a large application package (order of GB).</span></span>
<span data-ttu-id="74146-176">Försök:</span><span class="sxs-lookup"><span data-stu-id="74146-176">Try:</span></span>
- <span data-ttu-id="74146-177">Ange en längre tidsgräns för [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) -metoden med `timeout` parameter.</span><span class="sxs-lookup"><span data-stu-id="74146-177">Specify a larger timeout for [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) method, with `timeout` parameter.</span></span> <span data-ttu-id="74146-178">Som standard är hello timeout 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="74146-178">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="74146-179">Kontrollera hello nätverksanslutning mellan källdatorn och kluster.</span><span class="sxs-lookup"><span data-stu-id="74146-179">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="74146-180">Om hello är långsam, Överväg att använda en dator med en bättre nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="74146-180">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="74146-181">Om hello klientdatorn finns i en annan region än hello kluster, Överväg att använda en klientdator i en närmare eller samma region som hello kluster.</span><span class="sxs-lookup"><span data-stu-id="74146-181">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="74146-182">Kontrollera om du träffa externa begränsning.</span><span class="sxs-lookup"><span data-stu-id="74146-182">Check if you are hitting external throttling.</span></span> <span data-ttu-id="74146-183">Till exempel när hello avbildningsarkivet är konfigurerade toouse azure storage, kan överför att begränsas.</span><span class="sxs-lookup"><span data-stu-id="74146-183">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="74146-184">Problem: Överför paketet slutfördes, men [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API timeout. Försök:</span><span class="sxs-lookup"><span data-stu-id="74146-184">Issue: Upload package completed successfully, but [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API times out. Try:</span></span>
- <span data-ttu-id="74146-185">[Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="74146-185">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="74146-186">hello komprimeringen minskar hello storlek och hello antal filer, vilket i sin tur minskar hello mängden trafik och fungerar som Service Fabric måste utföra.</span><span class="sxs-lookup"><span data-stu-id="74146-186">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="74146-187">hello överföringen kan ta längre tid (särskilt om du inkluderar hello komprimering tid), men registrera och avregistrera hello programtyp är snabbare.</span><span class="sxs-lookup"><span data-stu-id="74146-187">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="74146-188">Ange en längre tidsgräns för [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API med `timeout` parameter.</span><span class="sxs-lookup"><span data-stu-id="74146-188">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API with `timeout` parameter.</span></span>

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="74146-189">Distribuera programpaket med många filer</span><span class="sxs-lookup"><span data-stu-id="74146-189">Deploy application package with many files</span></span>
<span data-ttu-id="74146-190">Problem: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) tidsgränsen uppnås för ett programpaket med många filer (efter tusentalsavgränsare).</span><span class="sxs-lookup"><span data-stu-id="74146-190">Issue: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="74146-191">Försök:</span><span class="sxs-lookup"><span data-stu-id="74146-191">Try:</span></span>
- <span data-ttu-id="74146-192">[Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="74146-192">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="74146-193">hello komprimeringen minskar hello antal filer.</span><span class="sxs-lookup"><span data-stu-id="74146-193">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="74146-194">Ange en längre tidsgräns för [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) med `timeout` parameter.</span><span class="sxs-lookup"><span data-stu-id="74146-194">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) with `timeout` parameter.</span></span>

## <a name="code-example"></a><span data-ttu-id="74146-195">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="74146-195">Code example</span></span>
<span data-ttu-id="74146-196">hello följande exempel kopierar ett paket toohello avbildningen programarkiv, etablerar hello programtyp, skapas en instans av programmet, skapar en tjänstinstans, tar bort hello programinstansen, avregistrera bestämmelser hello programtyp, och tar bort hello programpaketet från avbildningsarkivet hello.</span><span class="sxs-lookup"><span data-stu-id="74146-196">hello following example copies an application package toohello image store, provisions hello application type, creates an application instance, creates a service instance, removes hello application instance, un-provisions hello application type, and deletes hello application package from hello image store.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create hello application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from hello application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision hello application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete hello application package from a location in hello image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a><span data-ttu-id="74146-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74146-197">Next steps</span></span>
[<span data-ttu-id="74146-198">Uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="74146-198">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="74146-199">Service Fabric hälsa introduktion</span><span class="sxs-lookup"><span data-stu-id="74146-199">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="74146-200">Diagnostisera och felsöka en Service Fabric-tjänst</span><span class="sxs-lookup"><span data-stu-id="74146-200">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="74146-201">Modellen är ett program i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="74146-201">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
