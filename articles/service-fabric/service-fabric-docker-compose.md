---
title: "aaaAzure Service Fabric Docker Compose förhandsgranskning"
description: "Azure Service Fabric accepterar Docker Compose format toomake den enklare tooorchestrate-befintlig behållare med hjälp av Service Fabric. Detta stöd är för närvarande under förhandsgranskning."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a><span data-ttu-id="6c361-104">Docker Compose programstöd i Azure Service Fabric (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="6c361-104">Docker Compose application support in Azure Service Fabric (Preview)</span></span>

<span data-ttu-id="6c361-105">Docker använder hello [docker-compose.yml](https://docs.docker.com/compose) filen för att definiera flera behållare program.</span><span class="sxs-lookup"><span data-stu-id="6c361-105">Docker uses hello [docker-compose.yml](https://docs.docker.com/compose) file for defining multi-container applications.</span></span> <span data-ttu-id="6c361-106">toomake det enkelt för kunder som är bekant med Docker tooorchestrate befintliga behållarprogram på Azure Service Fabric, vi har tagit preview stöd för Docker Compose internt i hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="6c361-106">toomake it easy for customers familiar with Docker tooorchestrate existing container applications on Azure Service Fabric, we have included preview support for Docker Compose natively in hello platform.</span></span> <span data-ttu-id="6c361-107">Service Fabric kan acceptera version 3 och senare av `docker-compose.yml` filer.</span><span class="sxs-lookup"><span data-stu-id="6c361-107">Service Fabric can accept version 3 and later of `docker-compose.yml` files.</span></span> 

<span data-ttu-id="6c361-108">Eftersom detta stöd i förhandsgranskningen stöds bara en del av Skriv direktiven.</span><span class="sxs-lookup"><span data-stu-id="6c361-108">Because this support is in preview, only a subset of Compose directives is supported.</span></span> <span data-ttu-id="6c361-109">Till exempel stöds programuppgraderingar inte.</span><span class="sxs-lookup"><span data-stu-id="6c361-109">For example, application upgrades are not supported.</span></span> <span data-ttu-id="6c361-110">Du kan alltid ta bort och distribuera program i stället för att uppgradera dem.</span><span class="sxs-lookup"><span data-stu-id="6c361-110">However, you can always remove and deploy applications instead of upgrading them.</span></span>

<span data-ttu-id="6c361-111">toouse detta Förhandsgranska, skapa klustret med version 5.7 eller en större hello Service Fabric Runtime via hello Azure-portalen tillsammans med hello motsvarande SDK.</span><span class="sxs-lookup"><span data-stu-id="6c361-111">toouse this preview, create your cluster with version 5.7 or greater of hello Service Fabric runtime through hello Azure portal along with hello corresponding SDK.</span></span> 

> [!NOTE]
> <span data-ttu-id="6c361-112">Den här funktionen är i förhandsvisning och stöds inte i produktion.</span><span class="sxs-lookup"><span data-stu-id="6c361-112">This feature is in preview and is not supported in production.</span></span>

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a><span data-ttu-id="6c361-113">Distribuera en Docker Compose fil på Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6c361-113">Deploy a Docker Compose file on Service Fabric</span></span>

<span data-ttu-id="6c361-114">hello följande kommandon skapar ett Service Fabric-program (med namnet `fabric:/TestContainerApp` i hello föregående exempel), som du kan övervaka och hantera som alla andra Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="6c361-114">hello following commands create a Service Fabric application (named `fabric:/TestContainerApp` in hello preceding example), which you can monitor and manage like any other Service Fabric application.</span></span> <span data-ttu-id="6c361-115">Du kan använda hello angivna programnamnet för hälsoförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="6c361-115">You can use hello specified application name for health queries.</span></span>

### <a name="use-powershell"></a><span data-ttu-id="6c361-116">Använd PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c361-116">Use PowerShell</span></span>

<span data-ttu-id="6c361-117">Skapa ett Service Fabric skriva program från en docker-compose.yml-fil genom att köra följande kommando i PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="6c361-117">Create a Service Fabric Compose application from a docker-compose.yml file by running hello following command in PowerShell:</span></span>

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

<span data-ttu-id="6c361-118">`RegistryUserName`och `RegistryPassword` finns toohello behållare registret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6c361-118">`RegistryUserName` and `RegistryPassword` refer toohello container registry username and password.</span></span> <span data-ttu-id="6c361-119">När du har slutfört hello programmet, kan du kontrollera dess status med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6c361-119">After you've completed hello application, you can check its status by using hello following command:</span></span>

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

<span data-ttu-id="6c361-120">toodelete hello skriva program via PowerShell, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6c361-120">toodelete hello Compose application through PowerShell, use hello following command:</span></span>

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="6c361-121">Använd Azure Service Fabric CLI (sfctl)</span><span class="sxs-lookup"><span data-stu-id="6c361-121">Use Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="6c361-122">Alternativt kan du använda följande kommando för Service Fabric CLI hello:</span><span class="sxs-lookup"><span data-stu-id="6c361-122">Alternatively, you can use hello following Service Fabric CLI command:</span></span>

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

<span data-ttu-id="6c361-123">När du har skapat programmet hello, kan du kontrollera statusen genom att använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6c361-123">After you've created hello application, you can check its status by using hello following command:</span></span>

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

<span data-ttu-id="6c361-124">toodelete hello skriva program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6c361-124">toodelete hello Compose application, use hello following command:</span></span>

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a><span data-ttu-id="6c361-125">Skriv-direktiv som stöds</span><span class="sxs-lookup"><span data-stu-id="6c361-125">Supported Compose directives</span></span>

<span data-ttu-id="6c361-126">Den här förhandsversionen stöder en delmängd av hello konfigurationsalternativ från hello Skriv version 3-format, inklusive hello följande primitiver:</span><span class="sxs-lookup"><span data-stu-id="6c361-126">This preview supports a subset of hello configuration options from hello Compose version 3 format, including hello following primitives:</span></span>

* <span data-ttu-id="6c361-127">Tjänster > Distribuera > repliker</span><span class="sxs-lookup"><span data-stu-id="6c361-127">Services > Deploy > Replicas</span></span>
* <span data-ttu-id="6c361-128">Tjänster > Distribuera > placering > begränsningar</span><span class="sxs-lookup"><span data-stu-id="6c361-128">Services > Deploy > Placement > Constraints</span></span>
* <span data-ttu-id="6c361-129">Tjänster > Distribuera > resurser > gränser</span><span class="sxs-lookup"><span data-stu-id="6c361-129">Services > Deploy > Resources > Limits</span></span>
    * <span data-ttu-id="6c361-130">cpu - resurser</span><span class="sxs-lookup"><span data-stu-id="6c361-130">-cpu-shares</span></span>
    * <span data-ttu-id="6c361-131">-minne</span><span class="sxs-lookup"><span data-stu-id="6c361-131">-memory</span></span>
    * <span data-ttu-id="6c361-132">-minne-byte</span><span class="sxs-lookup"><span data-stu-id="6c361-132">-memory-swap</span></span>
* <span data-ttu-id="6c361-133">Tjänster > kommandon</span><span class="sxs-lookup"><span data-stu-id="6c361-133">Services > Commands</span></span>
* <span data-ttu-id="6c361-134">Tjänster > miljö</span><span class="sxs-lookup"><span data-stu-id="6c361-134">Services > Environment</span></span>
* <span data-ttu-id="6c361-135">Tjänster > portar</span><span class="sxs-lookup"><span data-stu-id="6c361-135">Services > Ports</span></span>
* <span data-ttu-id="6c361-136">Tjänster > bild</span><span class="sxs-lookup"><span data-stu-id="6c361-136">Services > Image</span></span>
* <span data-ttu-id="6c361-137">Tjänster > isolering (endast för Windows)</span><span class="sxs-lookup"><span data-stu-id="6c361-137">Services > Isolation (only for Windows)</span></span>
* <span data-ttu-id="6c361-138">Tjänster > loggning > drivrutin</span><span class="sxs-lookup"><span data-stu-id="6c361-138">Services > Logging > Driver</span></span>
* <span data-ttu-id="6c361-139">Tjänster > loggning > drivrutinen > Alternativ</span><span class="sxs-lookup"><span data-stu-id="6c361-139">Services > Logging > Driver > Options</span></span>
* <span data-ttu-id="6c361-140">Volymen & Distribuera > volym</span><span class="sxs-lookup"><span data-stu-id="6c361-140">Volume & Deploy > Volume</span></span>

<span data-ttu-id="6c361-141">Konfigurera hello kluster för att genomdriva gränserna, enligt beskrivningen i [Service Fabric resurs styrning](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6c361-141">Set up hello cluster for enforcing resource limits, as described in [Service Fabric resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="6c361-142">Alla andra Docker Compose direktiv stöds inte för den här förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="6c361-142">All other Docker Compose directives are unsupported for this preview.</span></span>

## <a name="servicednsname-computation"></a><span data-ttu-id="6c361-143">ServiceDnsName beräkning</span><span class="sxs-lookup"><span data-stu-id="6c361-143">ServiceDnsName computation</span></span>

<span data-ttu-id="6c361-144">Om hello tjänstnamnet som du anger i en Skriv-fil är ett fullständigt kvalificerat domännamn (dvs, den innehåller en punkt [.]), hello DNS-namn som registrerats av Service Fabric är `<ServiceName>` (inklusive hello punkt).</span><span class="sxs-lookup"><span data-stu-id="6c361-144">If hello service name that you specify in a Compose file is a fully qualified domain name (that is, it contains a dot [.]), hello DNS name registered by Service Fabric is `<ServiceName>` (including hello dot).</span></span> <span data-ttu-id="6c361-145">Om inte, varje sökvägssegmentet i hello programnamn blir en domänetikett i hello tjänsten DNS-namn med hello första sökvägssegment blir hello toppnivådomänen etikett.</span><span class="sxs-lookup"><span data-stu-id="6c361-145">If not, each path segment in hello application name becomes a domain label in hello service DNS name, with hello first path segment becoming hello top-level domain label.</span></span>

<span data-ttu-id="6c361-146">Till exempel om hello anges programnamnet är `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` skulle vara hello registrerade DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="6c361-146">For example, if hello specified application name is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` would be hello registered DNS name.</span></span>

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a><span data-ttu-id="6c361-147">Skillnader mellan Skriv (definition av instans) och Service Fabric-programmodell (typdefinition)</span><span class="sxs-lookup"><span data-stu-id="6c361-147">Differences between Compose (instance definition) and Service Fabric application model (type definition)</span></span>

<span data-ttu-id="6c361-148">En filen docker-compose.yml beskrivs distribuerbara behållare, inklusive deras egenskaper och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="6c361-148">A docker-compose.yml file describes a deployable set of containers, including their properties and configurations.</span></span>
<span data-ttu-id="6c361-149">Till exempel kan hello-filen innehålla miljövariabler och portar.</span><span class="sxs-lookup"><span data-stu-id="6c361-149">For example, hello file can contain environment variables and ports.</span></span> <span data-ttu-id="6c361-150">Du kan också ange parametrar för distribution, till exempel placeringen och gränserna för DNS-namn, i filen för hello docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="6c361-150">You can also specify deployment parameters, such as placement constraints, resource limits, and DNS names, in hello docker-compose.yml file.</span></span>

<span data-ttu-id="6c361-151">Hej [Service Fabric programmodell](service-fabric-application-model.md) använder tjänsttyper och programtyper, där du kan ha många programinstanser av hello samma typ.</span><span class="sxs-lookup"><span data-stu-id="6c361-151">hello [Service Fabric application model](service-fabric-application-model.md) uses service types and application types, where you can have many application instances of hello same type.</span></span> <span data-ttu-id="6c361-152">Du kan till exempel ha en programinstansen per kund.</span><span class="sxs-lookup"><span data-stu-id="6c361-152">For example, you can have one application instance per customer.</span></span> <span data-ttu-id="6c361-153">Den här typen baserat modellen har stöd för flera versioner av hello samma typ av program som har registrerats med hello körning.</span><span class="sxs-lookup"><span data-stu-id="6c361-153">This type-based model supports multiple versions of hello same application type that's registered with hello runtime.</span></span>

<span data-ttu-id="6c361-154">Till exempel kund A kan ha ett program som instansieras med AppTypeA 1.0 och kund B kan ha ett annat program instansierades med hello samma typ och version.</span><span class="sxs-lookup"><span data-stu-id="6c361-154">For example, customer A can have an application instantiated with type 1.0 of AppTypeA, and customer B can have another application instantiated with hello same type and version.</span></span> <span data-ttu-id="6c361-155">Du definierar hello programtyper i hello applikationsmanifest, och du kan ange hello namn och distribution programparametrar när du skapar hello program.</span><span class="sxs-lookup"><span data-stu-id="6c361-155">You define hello application types in hello application manifests, and you specify hello application name and deployment parameters when you create hello application.</span></span>

<span data-ttu-id="6c361-156">Även om den här modellen ger flexibilitet, planerar vi också toosupport en enklare, instans-baserade distributionsmodell där typer är implicit från hello manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="6c361-156">Although this model offers flexibility, we are also planning toosupport a simpler, instance-based deployment model where types are implicit from hello manifest file.</span></span> <span data-ttu-id="6c361-157">I den här modellen får varje program egna oberoende manifest.</span><span class="sxs-lookup"><span data-stu-id="6c361-157">In this model, each application gets its own independent manifest.</span></span> <span data-ttu-id="6c361-158">Vi förhandsgranskar detta arbete genom att lägga till stöd för docker-compose.yml, vilket är ett instans-baserad distribution-format.</span><span class="sxs-lookup"><span data-stu-id="6c361-158">We are previewing this effort by adding support for docker-compose.yml, which is an instance-based deployment format.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c361-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c361-159">Next steps</span></span>

* <span data-ttu-id="6c361-160">Håll dig uppdaterad om hello [programmodell för Service Fabric](service-fabric-application-model.md)</span><span class="sxs-lookup"><span data-stu-id="6c361-160">Read up on hello [Service Fabric application model](service-fabric-application-model.md)</span></span>
* [<span data-ttu-id="6c361-161">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="6c361-161">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)
