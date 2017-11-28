---
title: "Hantera Azure Service Fabric-program med hjälp av Azure Service Fabric CLI"
description: "Lär dig hur du distribuerar och ta bort program från ett Azure Service Fabric-kluster med hjälp av Azure Service Fabric CLI"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: c3a2eb3e6e54f952ef963bb2a0292d9ad7b53bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="d7924-103">Hantera ett Azure Service Fabric-program med hjälp av Azure Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="d7924-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="d7924-104">Lär dig mer om att skapa och ta bort program som körs i ett Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7924-104">Learn how to create and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7924-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d7924-105">Prerequisites</span></span>

* <span data-ttu-id="d7924-106">Installera Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="d7924-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="d7924-107">Välj sedan Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="d7924-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="d7924-108">Mer information finns i [komma igång med Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d7924-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="d7924-109">Har ett Service Fabric-programpaket klar att distribueras.</span><span class="sxs-lookup"><span data-stu-id="d7924-109">Have a Service Fabric application package ready to be deployed.</span></span> <span data-ttu-id="d7924-110">Mer information om hur du författare och paketet ett program Läs mer om den [Service Fabric programmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="d7924-110">For more information about how to author and package an application, read about the [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="d7924-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="d7924-111">Overview</span></span>

<span data-ttu-id="d7924-112">Följ stegen för att distribuera ett nytt program:</span><span class="sxs-lookup"><span data-stu-id="d7924-112">To deploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="d7924-113">Överför ett programpaket till Service Fabric image store.</span><span class="sxs-lookup"><span data-stu-id="d7924-113">Upload an application package to the Service Fabric image store.</span></span>
2. <span data-ttu-id="d7924-114">Etablera en typ av program.</span><span class="sxs-lookup"><span data-stu-id="d7924-114">Provision an application type.</span></span>
3. <span data-ttu-id="d7924-115">Ange och skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="d7924-115">Specify and create an application.</span></span>
4. <span data-ttu-id="d7924-116">Ange och skapa tjänster.</span><span class="sxs-lookup"><span data-stu-id="d7924-116">Specify and create services.</span></span>

<span data-ttu-id="d7924-117">Ta bort ett befintligt program genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7924-117">To remove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="d7924-118">Ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="d7924-118">Delete the application.</span></span>
2. <span data-ttu-id="d7924-119">Avetablera typen associerade program.</span><span class="sxs-lookup"><span data-stu-id="d7924-119">Unprovision the associated application type.</span></span>
3. <span data-ttu-id="d7924-120">Ta bort image store-innehåll.</span><span class="sxs-lookup"><span data-stu-id="d7924-120">Delete the image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="d7924-121">Distribuera ett nytt program</span><span class="sxs-lookup"><span data-stu-id="d7924-121">Deploy a new application</span></span>

<span data-ttu-id="d7924-122">För att distribuera ett nytt program måste du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d7924-122">To deploy a new application, complete the following tasks:</span></span>

### <a name="upload-a-new-application-package-to-the-image-store"></a><span data-ttu-id="d7924-123">Ladda upp ett nytt programpaket till image store</span><span class="sxs-lookup"><span data-stu-id="d7924-123">Upload a new application package to the image store</span></span>

<span data-ttu-id="d7924-124">Ladda upp programpaketet till Service Fabric image store innan du skapar ett program.</span><span class="sxs-lookup"><span data-stu-id="d7924-124">Before you create an application, upload the application package to the Service Fabric image store.</span></span>

<span data-ttu-id="d7924-125">Om ditt programpaket är i till exempel den `app_package_dir` directory, använda följande kommandon för att ladda upp katalogen:</span><span class="sxs-lookup"><span data-stu-id="d7924-125">For example, if your application package is in the `app_package_dir` directory, use the following commands to upload the directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="d7924-126">För stora programpaket kan du ange den `--show-progress` alternativet för att visa förloppet för överföringen.</span><span class="sxs-lookup"><span data-stu-id="d7924-126">For large application packages, you can specify the `--show-progress` option to display the progress of the upload.</span></span>

### <a name="provision-the-application-type"></a><span data-ttu-id="d7924-127">Etablera programtypen</span><span class="sxs-lookup"><span data-stu-id="d7924-127">Provision the application type</span></span>

<span data-ttu-id="d7924-128">När överföringen är klar kan du etablera programmet.</span><span class="sxs-lookup"><span data-stu-id="d7924-128">When the upload is finished, provision the application.</span></span> <span data-ttu-id="d7924-129">Om du vill distribuera programmet, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d7924-129">To provision the application, use the following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="d7924-130">Värdet för `application-type-build-path` är namnet på den katalog där du laddade upp ditt programpaket.</span><span class="sxs-lookup"><span data-stu-id="d7924-130">The value for `application-type-build-path` is the name of the directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="d7924-131">Skapa ett program från en typ av program</span><span class="sxs-lookup"><span data-stu-id="d7924-131">Create an application from an application type</span></span>

<span data-ttu-id="d7924-132">När du distribuera programmet, använder du följande kommando att namnge och skapa programmet:</span><span class="sxs-lookup"><span data-stu-id="d7924-132">After you provision the application, use the following command to name and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="d7924-133">`app-name`är det namn som du vill använda för programinstansen.</span><span class="sxs-lookup"><span data-stu-id="d7924-133">`app-name` is the name that you want to use for the application instance.</span></span> <span data-ttu-id="d7924-134">Du kan få ytterligare parametrar från tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="d7924-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="d7924-135">Programnamnet måste börja med prefixet `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="d7924-135">The application name must start with the prefix `fabric:/`.</span></span>

### <a name="create-services-for-the-new-application"></a><span data-ttu-id="d7924-136">Skapa tjänster för det nya programmet</span><span class="sxs-lookup"><span data-stu-id="d7924-136">Create services for the new application</span></span>

<span data-ttu-id="d7924-137">När du har skapat ett program kan du skapa tjänster från programmet.</span><span class="sxs-lookup"><span data-stu-id="d7924-137">After you have created an application, create services from the application.</span></span> <span data-ttu-id="d7924-138">I följande exempel skapar vi tillståndslösa tjänsten från våra program.</span><span class="sxs-lookup"><span data-stu-id="d7924-138">In the following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="d7924-139">De tjänster som du kan skapa från ett program som har definierats i en tjänstmanifestet i det tidigare etablerade programpaketet.</span><span class="sxs-lookup"><span data-stu-id="d7924-139">The services that you can create from an application are defined in a service manifest in the previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="d7924-140">Kontrollera programdistribution och hälsa</span><span class="sxs-lookup"><span data-stu-id="d7924-140">Verify application deployment and health</span></span>

<span data-ttu-id="d7924-141">Om du vill kontrollera att allt är felfri, använder du följande kommandon för health:</span><span class="sxs-lookup"><span data-stu-id="d7924-141">To verify everything is healthy, use the following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="d7924-142">Verifiera att tjänsten är felfritt, att använda liknande kommandon för att hämta hälsotillståndet för både tjänsten och programmet:</span><span class="sxs-lookup"><span data-stu-id="d7924-142">To verify that the service is healthy, use similar commands to retrieve the health of both the service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="d7924-143">Felfri tjänster och program har en `HealthState` värdet för `Ok`.</span><span class="sxs-lookup"><span data-stu-id="d7924-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="d7924-144">Ta bort ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="d7924-144">Remove an existing application</span></span>

<span data-ttu-id="d7924-145">Om du vill ta bort ett program måste du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d7924-145">To remove an application, complete the following tasks:</span></span>

### <a name="delete-the-application"></a><span data-ttu-id="d7924-146">Ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="d7924-146">Delete the application</span></span>

<span data-ttu-id="d7924-147">Om du vill ta bort programmet, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d7924-147">To delete the application, use the following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a><span data-ttu-id="d7924-148">Avetablera programtypen</span><span class="sxs-lookup"><span data-stu-id="d7924-148">Unprovision the application type</span></span>

<span data-ttu-id="d7924-149">När du har tagit bort programmet avetablera du programtypen om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="d7924-149">After you delete the application, you can unprovision the application type if you no longer need it.</span></span> <span data-ttu-id="d7924-150">Om du vill avetablera programtypen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d7924-150">To unprovision the application type, use the following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="d7924-151">Typnamn och Typversion måste matcha namnet och versionen i tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="d7924-151">The type name and type version must match the name and version in the previously provisioned application manifest.</span></span>

### <a name="delete-the-application-package"></a><span data-ttu-id="d7924-152">Ta bort programpaketet</span><span class="sxs-lookup"><span data-stu-id="d7924-152">Delete the application package</span></span>

<span data-ttu-id="d7924-153">När du har avetablerade programtypen kan du ta bort programpaketet från avbildningsarkivet om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="d7924-153">After you have unprovisioned the application type, you can delete the application package from the image store if you no longer need it.</span></span> <span data-ttu-id="d7924-154">Om du tar bort programpaket kan frigöra utrymme på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="d7924-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="d7924-155">Om du vill ta bort programmet paketet från avbildningsarkivet, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d7924-155">To delete the application package from the image store, use the following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="d7924-156">`content-path`måste vara namnet på den katalog som du överfört när du skapade programmet.</span><span class="sxs-lookup"><span data-stu-id="d7924-156">`content-path` must be the name of the directory that you uploaded when you created the application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="d7924-157">Uppgradera program</span><span class="sxs-lookup"><span data-stu-id="d7924-157">Upgrade application</span></span>

<span data-ttu-id="d7924-158">Du kan upprepa samma uppsättning steg för att etablera en andra versioner av programmet när du har skapat ditt program.</span><span class="sxs-lookup"><span data-stu-id="d7924-158">After creating your application, you can repeat the same set of steps to provision a second version of your application.</span></span> <span data-ttu-id="d7924-159">Sedan kan du övergå till den andra versionen av programmet som körs med en uppgradering av Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="d7924-159">Then, with a Service Fabric application upgrade you can transition to running the second version of the application.</span></span> <span data-ttu-id="d7924-160">Mer information finns i dokumentationen på [Service Fabric programuppgraderingar](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="d7924-160">For more information, see the documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="d7924-161">Om du vill utföra en uppgradering att etablera nästa version av programmet som använder samma kommandon som tidigare:</span><span class="sxs-lookup"><span data-stu-id="d7924-161">To perform an upgrade, first provision the next version of the application using the same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="d7924-162">Det rekommenderas sedan att utföra en övervakade automatisk uppgradering, starta uppgraderingen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d7924-162">It is recommended then to perform a monitored automatic upgrade, launch the upgrade by running the following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="d7924-163">Uppgraderingar åsidosätta befintliga parametrar med oavsett set anges.</span><span class="sxs-lookup"><span data-stu-id="d7924-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="d7924-164">Programmet parametrar ska skickas som argument för kommandot uppgradera om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d7924-164">Application parameters should be passed as arguments to the upgrade command, if necessary.</span></span> <span data-ttu-id="d7924-165">Parametrar för programmet ska vara kodad som ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="d7924-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="d7924-166">Om du vill hämta alla tidigare angivna parametrar du kan använda den `sfctl application info` kommando.</span><span class="sxs-lookup"><span data-stu-id="d7924-166">To retrieve any parameters previously specified, you can use the `sfctl application info` command.</span></span>

<span data-ttu-id="d7924-167">När en uppgradering av programmet pågår status kan hämtas med hjälp av den `sfctl application upgrade-status` kommando.</span><span class="sxs-lookup"><span data-stu-id="d7924-167">When an application upgrade is in progress, the status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="d7924-168">Slutligen, om en uppgradering är pågående och behöver avbrytas, kan du använda den `sfctl application upgrade-rollback` att återställa uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="d7924-168">Finally, if an upgrade is in progress and needs to be canceled, you can use the `sfctl application upgrade-rollback` to roll back the upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7924-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7924-169">Next steps</span></span>

* [<span data-ttu-id="d7924-170">Grunderna i Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="d7924-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="d7924-171">Komma igång med Service Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="d7924-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="d7924-172">Starta en uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="d7924-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)
