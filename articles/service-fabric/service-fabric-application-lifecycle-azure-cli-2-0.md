---
title: "aaaManage Azure Service Fabric-program med hjälp av Azure CLI 2.0"
description: "Lär dig hur toodeploy och ta bort program från en Azure Service Fabric-kluster med hjälp av Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ae1ba19513978b0f95ffb65d5f1f7a21ed5f2894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a><span data-ttu-id="bf773-103">Hantera ett Azure Service Fabric-program med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bf773-103">Manage an Azure Service Fabric application by using Azure CLI 2.0</span></span>

<span data-ttu-id="bf773-104">Lär dig hur toocreate och ta bort program som körs i ett Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="bf773-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf773-105">Krav</span><span class="sxs-lookup"><span data-stu-id="bf773-105">Prerequisites</span></span>

* <span data-ttu-id="bf773-106">Installera Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="bf773-106">Install Azure CLI 2.0.</span></span> <span data-ttu-id="bf773-107">Välj sedan Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bf773-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="bf773-108">Mer information finns i [Kom igång med Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="bf773-108">For more information, see [Get started with Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span></span>

* <span data-ttu-id="bf773-109">Har ett Service Fabric application package redo toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="bf773-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="bf773-110">Mer information om hur tooauthor och paketet program kan läsa om hello [Service Fabric programmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="bf773-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="bf773-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="bf773-111">Overview</span></span>

<span data-ttu-id="bf773-112">toodeploy ett nytt program slutföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="bf773-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="bf773-113">Ladda upp ett paket toohello Service Fabric avbildningen programarkiv.</span><span class="sxs-lookup"><span data-stu-id="bf773-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="bf773-114">Etablera en typ av program.</span><span class="sxs-lookup"><span data-stu-id="bf773-114">Provision an application type.</span></span>
3. <span data-ttu-id="bf773-115">Ange och skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="bf773-115">Specify and create an application.</span></span>
4. <span data-ttu-id="bf773-116">Ange och skapa tjänster.</span><span class="sxs-lookup"><span data-stu-id="bf773-116">Specify and create services.</span></span>

<span data-ttu-id="bf773-117">tooremove ett befintligt program slutföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="bf773-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="bf773-118">Ta bort hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="bf773-118">Delete hello application.</span></span>
2. <span data-ttu-id="bf773-119">Avetablera hello associerade programtyp.</span><span class="sxs-lookup"><span data-stu-id="bf773-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="bf773-120">Ta bort hello image store innehåll.</span><span class="sxs-lookup"><span data-stu-id="bf773-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="bf773-121">Distribuera ett nytt program</span><span class="sxs-lookup"><span data-stu-id="bf773-121">Deploy a new application</span></span>

<span data-ttu-id="bf773-122">toodeploy ett nytt program fullständig hello följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bf773-122">toodeploy a new application, complete hello following tasks.</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="bf773-123">Ladda upp en ny paketet toohello bilden appbutik</span><span class="sxs-lookup"><span data-stu-id="bf773-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="bf773-124">Ladda upp hello programmet paketet toohello Service Fabric avbildningsarkivet innan du skapar ett program.</span><span class="sxs-lookup"><span data-stu-id="bf773-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span> 

<span data-ttu-id="bf773-125">Till exempel om ditt programpaket i hello `app_package_dir` directory, Använd hello följande kommandon tooupload hello directory:</span><span class="sxs-lookup"><span data-stu-id="bf773-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
az sf application upload --path ~/app_package_dir
```

<span data-ttu-id="bf773-126">För stora programpaket kan du ange hello `--show-progress` alternativet toodisplay hello fortskrider hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="bf773-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="bf773-127">Etablera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="bf773-127">Provision hello application type</span></span>

<span data-ttu-id="bf773-128">Etablera hello programmet när hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="bf773-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="bf773-129">tooprovision hello program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bf773-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="bf773-130">Hej värdet för `application-type-build-path` är hello namnet på hello katalog där du laddade upp ditt programpaket.</span><span class="sxs-lookup"><span data-stu-id="bf773-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="bf773-131">Skapa ett program från en typ av program</span><span class="sxs-lookup"><span data-stu-id="bf773-131">Create an application from an application type</span></span>

<span data-ttu-id="bf773-132">När du etablerar hello program kan använda hello följande kommando tooname och skapa programmet:</span><span class="sxs-lookup"><span data-stu-id="bf773-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="bf773-133">`app-name`är hello namn som du vill toouse för hello programinstansen.</span><span class="sxs-lookup"><span data-stu-id="bf773-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="bf773-134">Du kan få ytterligare parametrar från hello tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="bf773-134">You can get additional parameters from hello previously provisioned application manifest.</span></span>

<span data-ttu-id="bf773-135">hello programmets namn måste börja med hello prefixet `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="bf773-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="bf773-136">Skapa tjänster för hello nytt program</span><span class="sxs-lookup"><span data-stu-id="bf773-136">Create services for hello new application</span></span>

<span data-ttu-id="bf773-137">När du har skapat ett program kan skapa tjänster från hello program.</span><span class="sxs-lookup"><span data-stu-id="bf773-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="bf773-138">I följande exempel hello, skapar vi en ny tjänst för tillståndslösa från våra program.</span><span class="sxs-lookup"><span data-stu-id="bf773-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="bf773-139">hello-tjänster som du kan skapa från ett program som har definierats i en tjänstmanifestet i hello tidigare etablerade programpaketet.</span><span class="sxs-lookup"><span data-stu-id="bf773-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="bf773-140">Kontrollera programdistribution och hälsa</span><span class="sxs-lookup"><span data-stu-id="bf773-140">Verify application deployment and health</span></span>

<span data-ttu-id="bf773-141">tooverify att ett program och tjänsten har distribuerats, kontrollera att programmet hello och tjänsten visas:</span><span class="sxs-lookup"><span data-stu-id="bf773-141">tooverify that an application and service were successfully deployed, check that hello application and service are listed:</span></span>

```azurecli
az sf application list
az sf service list --application-list TestApp
```

<span data-ttu-id="bf773-142">tooverify att hello-tjänsten är felfri, Använd liknande kommandon tooretrieve hello hälsotillståndet för både hello-tjänst och hello program:</span><span class="sxs-lookup"><span data-stu-id="bf773-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and hello application:</span></span>

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

<span data-ttu-id="bf773-143">Felfri tjänster och program har en `HealthState` värdet för `Ok`.</span><span class="sxs-lookup"><span data-stu-id="bf773-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="bf773-144">Ta bort ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="bf773-144">Remove an existing application</span></span>

<span data-ttu-id="bf773-145">tooremove ett program, fullständig hello följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bf773-145">tooremove an application, complete hello following tasks.</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="bf773-146">Ta bort programmet hello</span><span class="sxs-lookup"><span data-stu-id="bf773-146">Delete hello application</span></span>

<span data-ttu-id="bf773-147">toodelete hello program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bf773-147">toodelete hello application, use hello following command:</span></span>

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="bf773-148">Avetablera hello programtyp</span><span class="sxs-lookup"><span data-stu-id="bf773-148">Unprovision hello application type</span></span>

<span data-ttu-id="bf773-149">När du har tagit bort programmet hello avetablera du hello programtyp om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="bf773-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="bf773-150">toounprovision hello programtyp, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bf773-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="bf773-151">hello typen namn och typ versionen måste matcha hello namn och version i hello tidigare etablerade programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="bf773-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="bf773-152">Ta bort hello programpaket</span><span class="sxs-lookup"><span data-stu-id="bf773-152">Delete hello application package</span></span>

<span data-ttu-id="bf773-153">När du har avetablerade hello programtyp, kan du ta bort hello programpaket från avbildningsarkivet hello om du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="bf773-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="bf773-154">Om du tar bort programpaket kan frigöra utrymme på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="bf773-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="bf773-155">toodelete hello programpaket från avbildningsarkivet hello, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bf773-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
az sf application package-delete --content-path app_package_dir
```

<span data-ttu-id="bf773-156">`content-path`måste vara hello namnet på hello-katalog som du laddade upp när du skapade programmet hello.</span><span class="sxs-lookup"><span data-stu-id="bf773-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="related-articles"></a><span data-ttu-id="bf773-157">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="bf773-157">Related articles</span></span>

* [<span data-ttu-id="bf773-158">Kom igång med Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bf773-158">Get started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="bf773-159">Kom igång med Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="bf773-159">Get started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
