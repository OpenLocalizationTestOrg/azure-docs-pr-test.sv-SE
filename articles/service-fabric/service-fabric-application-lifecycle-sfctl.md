---
title: "aaaManage Azure Service Fabric-program med hjälp av Azure Service Fabric CLI"
description: "Lär dig hur toodeploy och ta bort program från en Azure Service Fabric-kluster med hjälp av Azure Service Fabric CLI"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: d9f98cee1d70f71a2aab68ff556956619910e4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="4cffb-103">Hantera ett Azure Service Fabric-program med hjälp av Azure Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="4cffb-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="4cffb-104">Lär dig hur toocreate och ta bort program som körs i ett Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="4cffb-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4cffb-105">Krav</span><span class="sxs-lookup"><span data-stu-id="4cffb-105">Prerequisites</span></span>

* <span data-ttu-id="4cffb-106">Installera Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="4cffb-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="4cffb-107">Välj sedan Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="4cffb-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="4cffb-108">Mer information finns i [komma igång med Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="4cffb-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="4cffb-109">Har ett Service Fabric application package redo toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="4cffb-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="4cffb-110">Mer information om hur tooauthor och paketet program kan läsa om hello [Service Fabric programmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="4cffb-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="4cffb-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="4cffb-111">Overview</span></span>

<span data-ttu-id="4cffb-112">toodeploy ett nytt program slutföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="4cffb-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="4cffb-113">Ladda upp ett paket toohello Service Fabric avbildningen programarkiv.</span><span class="sxs-lookup"><span data-stu-id="4cffb-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="4cffb-114">Etablera en typ av program.</span><span class="sxs-lookup"><span data-stu-id="4cffb-114">Provision an application type.</span></span>
3. <span data-ttu-id="4cffb-115">Ange och skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="4cffb-115">Specify and create an application.</span></span>
4. <span data-ttu-id="4cffb-116">Ange och skapa tjänster.</span><span class="sxs-lookup"><span data-stu-id="4cffb-116">Specify and create services.</span></span>

<span data-ttu-id="4cffb-117">tooremove ett befintligt program slutföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="4cffb-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="4cffb-118">Ta bort hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-118">Delete hello application.</span></span>
2. <span data-ttu-id="4cffb-119">Avetablera hello associerade programtyp.</span><span class="sxs-lookup"><span data-stu-id="4cffb-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="4cffb-120">Ta bort hello image store innehåll.</span><span class="sxs-lookup"><span data-stu-id="4cffb-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="4cffb-121">Distribuera ett nytt program</span><span class="sxs-lookup"><span data-stu-id="4cffb-121">Deploy a new application</span></span>

<span data-ttu-id="4cffb-122">toodeploy ett nytt program fullständig hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="4cffb-122">toodeploy a new application, complete hello following tasks:</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="4cffb-123">Ladda upp en ny paketet toohello bilden appbutik</span><span class="sxs-lookup"><span data-stu-id="4cffb-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="4cffb-124">Ladda upp hello programmet paketet toohello Service Fabric avbildningsarkivet innan du skapar ett program.</span><span class="sxs-lookup"><span data-stu-id="4cffb-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span>

<span data-ttu-id="4cffb-125">Till exempel om ditt programpaket i hello `app_package_dir` directory, Använd hello följande kommandon tooupload hello directory:</span><span class="sxs-lookup"><span data-stu-id="4cffb-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="4cffb-126">För stora programpaket kan du ange hello `--show-progress` alternativet toodisplay hello fortskrider hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="4cffb-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="4cffb-127">Etablera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="4cffb-127">Provision hello application type</span></span>

<span data-ttu-id="4cffb-128">Etablera hello programmet när hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="4cffb-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="4cffb-129">tooprovision hello program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4cffb-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="4cffb-130">Hej värdet för `application-type-build-path` är hello namnet på hello katalog där du laddade upp ditt programpaket.</span><span class="sxs-lookup"><span data-stu-id="4cffb-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="4cffb-131">Skapa ett program från en typ av program</span><span class="sxs-lookup"><span data-stu-id="4cffb-131">Create an application from an application type</span></span>

<span data-ttu-id="4cffb-132">När du etablerar hello program kan använda hello följande kommando tooname och skapa programmet:</span><span class="sxs-lookup"><span data-stu-id="4cffb-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="4cffb-133">`app-name`är hello namn som du vill toouse för hello programinstansen.</span><span class="sxs-lookup"><span data-stu-id="4cffb-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="4cffb-134">Du kan få ytterligare parametrar från tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="4cffb-135">hello programmets namn måste börja med hello prefixet `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="4cffb-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="4cffb-136">Skapa tjänster för hello nytt program</span><span class="sxs-lookup"><span data-stu-id="4cffb-136">Create services for hello new application</span></span>

<span data-ttu-id="4cffb-137">När du har skapat ett program kan skapa tjänster från hello program.</span><span class="sxs-lookup"><span data-stu-id="4cffb-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="4cffb-138">I följande exempel hello, skapar vi en ny tjänst för tillståndslösa från våra program.</span><span class="sxs-lookup"><span data-stu-id="4cffb-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="4cffb-139">hello-tjänster som du kan skapa från ett program som har definierats i en tjänstmanifestet i hello tidigare etablerade programpaketet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="4cffb-140">Kontrollera programdistribution och hälsa</span><span class="sxs-lookup"><span data-stu-id="4cffb-140">Verify application deployment and health</span></span>

<span data-ttu-id="4cffb-141">tooverify allt är felfri, använder du följande kommandon för health hello:</span><span class="sxs-lookup"><span data-stu-id="4cffb-141">tooverify everything is healthy, use hello following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="4cffb-142">tooverify att hello-tjänsten är felfri, Använd liknande kommandon tooretrieve hello hälsotillståndet för både hello-tjänsten och programmet:</span><span class="sxs-lookup"><span data-stu-id="4cffb-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="4cffb-143">Felfri tjänster och program har en `HealthState` värdet för `Ok`.</span><span class="sxs-lookup"><span data-stu-id="4cffb-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="4cffb-144">Ta bort ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="4cffb-144">Remove an existing application</span></span>

<span data-ttu-id="4cffb-145">tooremove ett program, fullständig hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="4cffb-145">tooremove an application, complete hello following tasks:</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="4cffb-146">Ta bort programmet hello</span><span class="sxs-lookup"><span data-stu-id="4cffb-146">Delete hello application</span></span>

<span data-ttu-id="4cffb-147">toodelete hello program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4cffb-147">toodelete hello application, use hello following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="4cffb-148">Avetablera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="4cffb-148">Unprovision hello application type</span></span>

<span data-ttu-id="4cffb-149">När du har tagit bort programmet hello avetablera du hello programtyp om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="4cffb-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="4cffb-150">toounprovision hello programtyp, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4cffb-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="4cffb-151">hello typen namn och typ versionen måste matcha hello namn och version i hello tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="4cffb-152">Ta bort hello programpaket</span><span class="sxs-lookup"><span data-stu-id="4cffb-152">Delete hello application package</span></span>

<span data-ttu-id="4cffb-153">När du har avetablerade hello programtyp, kan du ta bort hello programpaket från avbildningsarkivet hello om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="4cffb-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="4cffb-154">Om du tar bort programpaket kan frigöra utrymme på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="4cffb-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="4cffb-155">toodelete hello programpaket från avbildningsarkivet hello, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4cffb-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="4cffb-156">`content-path`måste vara hello namnet på hello-katalog som du laddade upp när du skapade programmet hello.</span><span class="sxs-lookup"><span data-stu-id="4cffb-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="4cffb-157">Uppgradera program</span><span class="sxs-lookup"><span data-stu-id="4cffb-157">Upgrade application</span></span>

<span data-ttu-id="4cffb-158">När du har skapat programmet, du kan upprepa hello samma uppsättning steg tooprovision en andra versioner av programmet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-158">After creating your application, you can repeat hello same set of steps tooprovision a second version of your application.</span></span> <span data-ttu-id="4cffb-159">Du kan sedan övergång toorunning hello andra versionen av programmet hello med en uppgradering av Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="4cffb-159">Then, with a Service Fabric application upgrade you can transition toorunning hello second version of hello application.</span></span> <span data-ttu-id="4cffb-160">Mer information finns i dokumentationen för hello på [Service Fabric programuppgraderingar](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="4cffb-160">For more information, see hello documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="4cffb-161">tooperform en uppgradering första etablera hello nästa version av hello använder hello samma kommandon som tidigare:</span><span class="sxs-lookup"><span data-stu-id="4cffb-161">tooperform an upgrade, first provision hello next version of hello application using hello same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="4cffb-162">Det rekommenderas sedan tooperform en övervakade automatisk uppgradering starta hello uppgraderingen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4cffb-162">It is recommended then tooperform a monitored automatic upgrade, launch hello upgrade by running hello following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="4cffb-163">Uppgraderingar åsidosätta befintliga parametrar med oavsett set anges.</span><span class="sxs-lookup"><span data-stu-id="4cffb-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="4cffb-164">Programmet parametrar ska skickas som argument toohello uppgradera kommando, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4cffb-164">Application parameters should be passed as arguments toohello upgrade command, if necessary.</span></span> <span data-ttu-id="4cffb-165">Parametrar för programmet ska vara kodad som ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="4cffb-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="4cffb-166">tooretrieve parametrar anges tidigare kan du använda hello `sfctl application info` kommando.</span><span class="sxs-lookup"><span data-stu-id="4cffb-166">tooretrieve any parameters previously specified, you can use hello `sfctl application info` command.</span></span>

<span data-ttu-id="4cffb-167">När en uppgradering av programmet pågår hello status kan hämtas med hjälp av den `sfctl application upgrade-status` kommando.</span><span class="sxs-lookup"><span data-stu-id="4cffb-167">When an application upgrade is in progress, hello status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="4cffb-168">Slutligen, om en uppgradering pågår och behöver toobe avbrytas, kan du använda hello `sfctl application upgrade-rollback` tooroll tillbaka hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="4cffb-168">Finally, if an upgrade is in progress and needs toobe canceled, you can use hello `sfctl application upgrade-rollback` tooroll back hello upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cffb-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cffb-169">Next steps</span></span>

* [<span data-ttu-id="4cffb-170">Grunderna i Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="4cffb-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="4cffb-171">Komma igång med Service Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="4cffb-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="4cffb-172">Starta en uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="4cffb-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)
