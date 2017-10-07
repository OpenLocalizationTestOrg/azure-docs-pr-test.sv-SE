---
title: aaaAzure Service Fabric med API Management Snabbstart | Microsoft Docs
description: "Den här guiden visar hur tooquickly Kom igång med Azure API Management och Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="0db83-103">Service Fabric med Azure API Management-Snabbstart</span><span class="sxs-lookup"><span data-stu-id="0db83-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="0db83-104">Den här guiden visar hur tooset upp Azure API Management med Service Fabric och konfigurera din första API åtgärden toosend trafik tooback tjänster i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0db83-104">This guide shows you how tooset up Azure API Management with Service Fabric and configure your first API operation toosend traffic tooback-end services in Service Fabric.</span></span> <span data-ttu-id="0db83-105">toolearn mer om Azure API Management scenarier med Service Fabric finns hello [översikt](service-fabric-api-management-overview.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0db83-105">toolearn more about Azure API Management scenarios with Service Fabric, see hello [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a><span data-ttu-id="0db83-106">Distribuera tooAzure API Management och Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0db83-106">Deploy API Management and Service Fabric tooAzure</span></span>

<span data-ttu-id="0db83-107">hello första steget är toodeploy API-hantering och ett Service Fabric-kluster tooAzure i ett delat virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0db83-107">hello first step is toodeploy API Management and a Service Fabric cluster tooAzure in a shared Virtual Network.</span></span> <span data-ttu-id="0db83-108">Detta tillåter API Management toocommunicate direkt med Service Fabric så att den kan utföra identifiering, service partition upplösning och vidarebefordra trafik direkt tooany backend-tjänsten i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0db83-108">This allows API Management toocommunicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly tooany backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="0db83-109">topologi</span><span class="sxs-lookup"><span data-stu-id="0db83-109">Topology</span></span>

<span data-ttu-id="0db83-110">Den här guiden distribuerar hello följande topologi tooAzure där API-hantering och Service Fabric finns i undernät för hello samma virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="0db83-110">This guide deploys hello following topology tooAzure in which API Management and Service Fabric are in subnets of hello same Virtual Network:</span></span>

 ![Bildrubrik][sf-apim-topology-overview]

<span data-ttu-id="0db83-112">tooget snabbt igång har Resource Manager-mallar angetts för varje steg i distributionen:</span><span class="sxs-lookup"><span data-stu-id="0db83-112">tooget started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="0db83-113">Nätverkstopologi:</span><span class="sxs-lookup"><span data-stu-id="0db83-113">Network topology:</span></span>
    - <span data-ttu-id="0db83-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="0db83-115">[Network.parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="0db83-116">Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="0db83-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="0db83-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="0db83-118">[cluster.parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="0db83-119">API-hantering:</span><span class="sxs-lookup"><span data-stu-id="0db83-119">API Management:</span></span>
    - <span data-ttu-id="0db83-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="0db83-121">[APIM.parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-tooazure-and-select-your-subscription"></a><span data-ttu-id="0db83-122">Logga in tooAzure och välja din prenumeration</span><span class="sxs-lookup"><span data-stu-id="0db83-122">Sign in tooAzure and select your subscription</span></span>

<span data-ttu-id="0db83-123">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="0db83-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="0db83-124">När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="0db83-124">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="0db83-125">Logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="0db83-125">Sign in tooyour Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="0db83-126">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="0db83-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="0db83-127">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="0db83-127">Create a resource group</span></span>

<span data-ttu-id="0db83-128">Skapa en ny resursgrupp för din distribution.</span><span class="sxs-lookup"><span data-stu-id="0db83-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="0db83-129">Ange ett namn och en plats.</span><span class="sxs-lookup"><span data-stu-id="0db83-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a><span data-ttu-id="0db83-130">Distribuera hello nätverkstopologi</span><span class="sxs-lookup"><span data-stu-id="0db83-130">Deploy hello network topology</span></span>

<span data-ttu-id="0db83-131">hello första steget är tooset in hello nätverks topologi toowhich API Management och hello Service Fabric-klustret kommer att distribueras.</span><span class="sxs-lookup"><span data-stu-id="0db83-131">hello first step is tooset up hello network topology toowhich API Management and hello Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="0db83-132">Hej [network.json] [ network-arm] Resource Manager-mall som är konfigurerade toocreate ett virtuellt nätverk (VNET) med två undernät och två Nätverkssäkerhetsgrupp grupper (NSG).</span><span class="sxs-lookup"><span data-stu-id="0db83-132">hello [network.json][network-arm] Resource Manager template is configured toocreate a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="0db83-133">Hej [network.parameters.json] [ network-parameters-arm] parameterfilen innehåller hello namnen på hello-undernät och NSG: er som API Management och Service Fabric ska distribueras till.</span><span class="sxs-lookup"><span data-stu-id="0db83-133">hello [network.parameters.json][network-parameters-arm] parameters file contains hello names of hello subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="0db83-134">Den här guiden behöver inte hello parametervärden toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="0db83-134">For this guide, hello parameter values do not need toobe changed.</span></span> <span data-ttu-id="0db83-135">hello API-hantering och Service Fabric Resource Manager-mallar används dessa värden, så om de ändras här måste du ändra dem i hello andra Resource Manager-mallar i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="0db83-135">hello API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in hello other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="0db83-136">Hämta hello följande Resource Manager-mall och parametrar:</span><span class="sxs-lookup"><span data-stu-id="0db83-136">Download hello following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="0db83-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="0db83-138">[Network.parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="0db83-139">Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer för nätverksinstallation hello hello:</span><span class="sxs-lookup"><span data-stu-id="0db83-139">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for hello network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a><span data-ttu-id="0db83-140">Distribuera hello Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="0db83-140">Deploy hello Service Fabric cluster</span></span>

<span data-ttu-id="0db83-141">När hello nätverksresurser är klar med att distribuera, hello nästa steg är toodeploy ett Service Fabric-kluster toohello VNET i undernät hello och NSG utsetts för hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-141">Once hello network resources have finished deploying, hello next step is toodeploy a Service Fabric cluster toohello VNET in hello subnet and NSG designated for hello Service Fabric cluster.</span></span> <span data-ttu-id="0db83-142">Den här självstudien är hello Service Fabric Resource Manager-mall förkonfigurerade toouse hello namnen på hello VNET och undernät NSG som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="0db83-142">For this tutorial, hello Service Fabric Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

<span data-ttu-id="0db83-143">Resource Manager-mall för hello Service Fabric-klustret är konfigurerade toocreate säker kluster med Certifikatsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="0db83-143">hello Service Fabric cluster Resource Manager template is configured toocreate a secure cluster with certificate security.</span></span> <span data-ttu-id="0db83-144">hello certifikatet är används toosecure nod till nod kommunikation för klustret och toomanage användaren åtkomst tooyour Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-144">hello certificate is used toosecure node-to-node communication for your cluster and toomanage user access tooyour Service Fabric cluster.</span></span> <span data-ttu-id="0db83-145">API Management använder det här certifikatet tooaccess hello Fabric namngivningstjänst för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="0db83-145">API Management uses this certificate tooaccess  hello Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="0db83-146">Det här steget kräver att ett certifikat i Key Vault för Klustersäkerhet.</span><span class="sxs-lookup"><span data-stu-id="0db83-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="0db83-147">Mer information om hur du skapar en säker kluster med Key Vault finns [den här guiden om hur du skapar ett kluster i Azure med hjälp av hanteraren för filserverresurser](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="0db83-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="0db83-148">Du kan lägga till Azure Active Directory-autentisering i tillägg toohello certifikatet som används för åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-148">You may add Azure Active Directory authentication in addition toohello certificate used for cluster access.</span></span> <span data-ttu-id="0db83-149">Azure Active Directory är hello rekommenderat sätt toomanage användaren åtkomst tooyour Service Fabric-kluster, men är inte nödvändigt toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0db83-149">Azure Active Directory is hello recommended way toomanage user access tooyour Service Fabric cluster, but is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="0db83-150">Ett certifikat krävs oavsett hur för klustret nod till nod säkerhet och Azure API Management-autentisering, vilket inte stöder för närvarande autentisera med Azure Active Directory för en Service Fabric-serverdel.</span><span class="sxs-lookup"><span data-stu-id="0db83-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="0db83-151">Hämta hello följande Resource Manager-mall och parametrar:</span><span class="sxs-lookup"><span data-stu-id="0db83-151">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="0db83-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="0db83-153">[cluster.parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="0db83-154">Fyll i hello tom parametrar i hello `cluster.parameters.json` -filen för din distribution, inklusive hello [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) för kluster-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="0db83-154">Fill in hello empty parameters in hello `cluster.parameters.json` file for your deployment, including hello [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="0db83-155">Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer toocreate hello Service Fabric-klustret hello:</span><span class="sxs-lookup"><span data-stu-id="0db83-155">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files toocreate hello Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="0db83-156">Distribuera API Management</span><span class="sxs-lookup"><span data-stu-id="0db83-156">Deploy API Management</span></span>

<span data-ttu-id="0db83-157">Slutligen kan distribuera API Management toohello VNET i undernät hello och NSG utsetts för API-hantering.</span><span class="sxs-lookup"><span data-stu-id="0db83-157">Finally, deploy API Management toohello VNET in hello subnet and NSG designated for API Management.</span></span> <span data-ttu-id="0db83-158">Du behöver inte toowait för hello Service Fabric-kluster distribution toofinish innan du distribuerar API-hantering.</span><span class="sxs-lookup"><span data-stu-id="0db83-158">You do not need toowait for hello Service Fabric cluster deployment toofinish before deploying API Management.</span></span> 

<span data-ttu-id="0db83-159">Den här självstudien är hello API Management Resource Manager-mall förkonfigurerade toouse hello namnen på hello VNET och undernät NSG som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="0db83-159">For this tutorial, hello API Management Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

 1. <span data-ttu-id="0db83-160">Hämta hello följande Resource Manager-mall och parametrar:</span><span class="sxs-lookup"><span data-stu-id="0db83-160">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="0db83-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="0db83-162">[APIM.parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0db83-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="0db83-163">Fyll i hello tom parametrar i hello `apim.parameters.json` för din distribution.</span><span class="sxs-lookup"><span data-stu-id="0db83-163">Fill in hello empty parameters in hello `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="0db83-164">Använd följande PowerShell-kommandot toodeploy hello Resource Manager mallen och parametern filer för API Management hello:</span><span class="sxs-lookup"><span data-stu-id="0db83-164">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="0db83-165">Konfigurera API-hantering</span><span class="sxs-lookup"><span data-stu-id="0db83-165">Configure API Management</span></span>

<span data-ttu-id="0db83-166">När din API-hantering och Service Fabric-klustret har distribuerats, kan du konfigurera en Service Fabric-serverdel i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="0db83-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="0db83-167">Detta gör att du toocreate för en backend-tjänsten som skickar trafik tooyour Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-167">This allows you toocreate a backend service policy that sends traffic tooyour Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="0db83-168">API Management-säkerhet</span><span class="sxs-lookup"><span data-stu-id="0db83-168">API Management Security</span></span>

<span data-ttu-id="0db83-169">tooconfigure hello Service Fabric serverdelen måste du först tooconfigure API Management säkerhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="0db83-169">tooconfigure hello Service Fabric backend, you first need tooconfigure API Management security settings.</span></span> <span data-ttu-id="0db83-170">tooconfigure säkerhetsinställningar, gå tooyour API Management-tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0db83-170">tooconfigure security settings, go tooyour API Management service in hello Azure portal.</span></span>

#### <a name="enable-hello-api-management-rest-api"></a><span data-ttu-id="0db83-171">Aktivera hello API Management REST API</span><span class="sxs-lookup"><span data-stu-id="0db83-171">Enable hello API Management REST API</span></span>

<span data-ttu-id="0db83-172">hello API Management REST API är för närvarande hello endast sätt tooconfigure en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="0db83-172">hello API Management REST API is currently hello only way tooconfigure a backend service.</span></span> <span data-ttu-id="0db83-173">hello första steget är tooenable hello API Management REST-API och skydda den.</span><span class="sxs-lookup"><span data-stu-id="0db83-173">hello first step is tooenable hello API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="0db83-174">Välj i hello API Management-tjänsten, **Management API - PREVIEW** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="0db83-174">In hello API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="0db83-175">Kontrollera hello **aktivera API Management REST API** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="0db83-175">Check hello **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="0db83-176">Observera hello URL för API Management – det här är hello-URL som vi använder senare tooset in hello Service Fabric-serverdelen</span><span class="sxs-lookup"><span data-stu-id="0db83-176">Note hello Management API URL - this is hello URL we'll use later tooset up hello Service Fabric backend</span></span>
 4. <span data-ttu-id="0db83-177">Generera en **åtkomsttoken** genom att välja ett förfallodatum och en nyckel, och klicka sedan på hello **generera** knappen hello nedre delen av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="0db83-177">Generate an **access Token** by selecting an expiry date and a key, then click hello **Generate** button toward hello bottom of hello page.</span></span>
 5. <span data-ttu-id="0db83-178">Kopiera hello **åtkomsttoken** och spara den – vi använder informationen i följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="0db83-178">Copy hello **access token** and save it - we'll use this in hello following steps.</span></span> <span data-ttu-id="0db83-179">Observera att detta skiljer sig från hello primärnyckel och sekundärnyckel.</span><span class="sxs-lookup"><span data-stu-id="0db83-179">Note this is different from hello primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="0db83-180">Överföra ett certifikat för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0db83-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="0db83-181">API Management måste autentisera med Service Fabric-klustret för identifiering av tjänst använder ett klientcertifikat som har åtkomst tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access tooyour cluster.</span></span> <span data-ttu-id="0db83-182">För enkelhetens skull hello den här självstudiekursen använder samma certifikat som anges när du skapar hello Service Fabric-kluster, vilket som standard kan vara används tooaccess klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-182">For simplicity, this tutorial uses hello same certificate specified when creating hello Service Fabric cluster, which by default can be used tooaccess your cluster.</span></span>

 1. <span data-ttu-id="0db83-183">Välj i hello API Management-tjänsten, **klientcertifikat - PREVIEW** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="0db83-183">In hello API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="0db83-184">Klicka på hello **+ Lägg till** knappen</span><span class="sxs-lookup"><span data-stu-id="0db83-184">Click hello **+ Add** button</span></span>
 2. <span data-ttu-id="0db83-185">Välj hello fil för privat nyckel (.pfx) för hello klustret certifikat som du angav när du skapar Service Fabric-kluster, ge det ett namn och ange lösenordet för hello privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="0db83-185">Select hello private key file (.pfx) of hello cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide hello private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="0db83-186">Den här kursen använder hello samma certifikat för autentisering och kluster nod till nod klientsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="0db83-186">This tutorial uses hello same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="0db83-187">Du kan använda ett separat klientcertifikat om du har en konfigurerad tooaccess Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-187">You may use a separate client certificate if you have one configured tooaccess your Service Fabric cluster.</span></span>

### <a name="configure-hello-backend"></a><span data-ttu-id="0db83-188">Konfigurera hello backend</span><span class="sxs-lookup"><span data-stu-id="0db83-188">Configure hello backend</span></span>

<span data-ttu-id="0db83-189">Nu när API Management-säkerheten är konfigurerad, kan du konfigurera hello Service Fabric-serverdel.</span><span class="sxs-lookup"><span data-stu-id="0db83-189">Now that API Management security is configured, you can configure hello Service Fabric backend.</span></span> <span data-ttu-id="0db83-190">För Service Fabric serverdelar är hello Service Fabric-kluster hello backend, i stället för en specifik tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0db83-190">For Service Fabric backends, hello Service Fabric cluster is hello backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="0db83-191">På så sätt kan en enda princip tooroute toomore än en tjänst i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="0db83-191">This allows a single policy tooroute toomore than one service in hello cluster.</span></span>

<span data-ttu-id="0db83-192">Det här steget kräver hello åtkomst-token som du tidigare genererade och hello tumavtrycket för ditt kluster certifikat du laddade upp tooAPI Management i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0db83-192">This step requires hello access token you generated earlier and hello thumbprint for your cluster certificate you uploaded tooAPI Management in hello previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="0db83-193">Om du använder ett separat klientcertifikat i hello föregående steg för API-hantering, behöver du hello tumavtrycket för hello klientcertifikat i tillägg toohello klustret tumavtryck för certifikatet i det här steget.</span><span class="sxs-lookup"><span data-stu-id="0db83-193">If you used a separate client certificate in hello previous step for API Management, you need hello thumbprint for hello client certificate in addition toohello cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="0db83-194">Skicka hello följande HTTP PUT-begäran toohello API Management API-URL som du antecknade tidigare när du aktiverar hello API Management REST API tooconfigure hello Service Fabric backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0db83-194">Send hello following HTTP PUT request toohello API Management API URL you noted earlier when enabling hello API Management REST API tooconfigure hello Service Fabric backend service.</span></span> <span data-ttu-id="0db83-195">Du bör se en `HTTP 201 Created` svar när hello kommandot slutförs.</span><span class="sxs-lookup"><span data-stu-id="0db83-195">You should see an `HTTP 201 Created` response when hello command succeeds.</span></span> <span data-ttu-id="0db83-196">Mer information om varje fält finns hello API Management [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="0db83-196">For more information on each field, see hello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="0db83-197">HTTP-kommando och URL:</span><span class="sxs-lookup"><span data-stu-id="0db83-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="0db83-198">Huvuden för begäran:</span><span class="sxs-lookup"><span data-stu-id="0db83-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="0db83-199">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="0db83-199">Request body:</span></span>
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

<span data-ttu-id="0db83-200">Hej **url** här parametern är ett fullständigt tjänstnamn för en tjänst i klustret som alla begäranden dirigeras tooby standard om inget namn anges i en backend-princip.</span><span class="sxs-lookup"><span data-stu-id="0db83-200">hello **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed tooby default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="0db83-201">Du kan använda falska tjänstnamn, till exempel ”fabric: / falska/service” om du inte avser toohave en återställningsplats tjänst.</span><span class="sxs-lookup"><span data-stu-id="0db83-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend toohave a fallback service.</span></span>

<span data-ttu-id="0db83-202">Se toohello API Management [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) för mer information om varje fält.</span><span class="sxs-lookup"><span data-stu-id="0db83-202">Refer toohello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="0db83-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="0db83-203">Example</span></span>

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="0db83-204">Distribuera en backend-tjänst för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0db83-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="0db83-205">Nu när du har hello Service Fabric konfigurerad som en backend-tooAPI hantering av skapar du backend-principer för dina API: er som skickar trafik tooyour Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="0db83-205">Now that you have hello Service Fabric configured as a backend tooAPI Management, you can author backend policies for your APIs that send traffic tooyour Service Fabric services.</span></span> <span data-ttu-id="0db83-206">Men du måste först en tjänst som körs i Service Fabric tooaccept begäranden.</span><span class="sxs-lookup"><span data-stu-id="0db83-206">But first you need a service running in Service Fabric tooaccept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="0db83-207">Skapa ett Service Fabric-tjänsten med en HTTP-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="0db83-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="0db83-208">Den här självstudiekursen skapar vi en grundläggande tillståndslös ASP.NET Core tillförlitlig tjänst med hello standardmallen Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="0db83-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using hello default Web API project template.</span></span> <span data-ttu-id="0db83-209">Detta skapar en HTTP-slutpunkt för din tjänst som du ska visa genom Azure API Management:</span><span class="sxs-lookup"><span data-stu-id="0db83-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="0db83-210">Börja med [ställa in din utvecklingsmiljö för utveckling av ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="0db83-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="0db83-211">När din utvecklingsmiljö har ställts in, starta Visual Studio som administratör och skapa en ASP.NET Core-tjänst:</span><span class="sxs-lookup"><span data-stu-id="0db83-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="0db83-212">Välj Arkiv -> Nytt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0db83-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="0db83-213">Välj hello Fabric tjänstprogrammet mall under molntjänster och ger den namnet **”ApiApplication”**.</span><span class="sxs-lookup"><span data-stu-id="0db83-213">Select hello Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="0db83-214">Välj namn hello projekt och hello ASP.NET Core-tjänstmall **”WebApiService”**.</span><span class="sxs-lookup"><span data-stu-id="0db83-214">Select hello ASP.NET Core service template and name hello project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="0db83-215">Välj hello Web API ASP.NET Core 1.1 projektmall.</span><span class="sxs-lookup"><span data-stu-id="0db83-215">Select hello Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="0db83-216">När du har skapat hello projektet öppnar du `PackageRoot\ServiceManifest.xml` och ta bort hello `Port` attribut från hello slutpunktskonfigurationen för resursen:</span><span class="sxs-lookup"><span data-stu-id="0db83-216">Once hello project is created, open `PackageRoot\ServiceManifest.xml` and remove hello `Port` attribute from hello endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="0db83-217">Detta gör att Service Fabric toospecify en port dynamiskt från hello programmet portintervall, som vi öppnas via hello Nätverkssäkerhetsgruppen i hello klustret Resource Manager-mall, att tillåta trafik tooflow tooit från API-hantering.</span><span class="sxs-lookup"><span data-stu-id="0db83-217">This allows Service Fabric toospecify a port dynamically from hello application port range, which we opened through hello Network Security Group in hello cluster Resource Manager template, allowing traffic tooflow tooit from API Management.</span></span>
 
 6. <span data-ttu-id="0db83-218">Tryck på F5 i Visual Studio tooverify hello webb-API finns lokalt.</span><span class="sxs-lookup"><span data-stu-id="0db83-218">Press F5 in Visual Studio tooverify hello web API is available locally.</span></span> 

    <span data-ttu-id="0db83-219">Öppna Service Fabric Explorer detaljnivån tooa specifika instansen av hello ASP.NET Core toosee hello basadress hello-tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="0db83-219">Open Service Fabric Explorer and drill down tooa specific instance of hello ASP.NET Core service toosee hello base address hello service is listening on.</span></span> <span data-ttu-id="0db83-220">Lägg till `/api/values` toohello basadressen och öppna den i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0db83-220">Add `/api/values` toohello base address and open it in a browser.</span></span> <span data-ttu-id="0db83-221">Detta startar hello Get-metoden på hello ValuesController i hello Web API-mallen.</span><span class="sxs-lookup"><span data-stu-id="0db83-221">This invokes hello Get method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="0db83-222">Den returnerar hello Standardsvar som tillhandahålls av hello mall, en JSON-matris som innehåller två strängar:</span><span class="sxs-lookup"><span data-stu-id="0db83-222">It returns hello default response that is provided by hello template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="0db83-223">Detta är hello-slutpunkt som du ska visa genom API Management i Azure.</span><span class="sxs-lookup"><span data-stu-id="0db83-223">This is hello endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="0db83-224">Slutligen distribuera hello programmet tooyour kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="0db83-224">Finally, deploy hello application tooyour cluster in Azure.</span></span> <span data-ttu-id="0db83-225">[Med Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), högerklicka på projektet hello och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="0db83-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click hello Application project and select **Publish**.</span></span> <span data-ttu-id="0db83-226">Ange din klusterslutpunkten (till exempel `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello programmet tooyour Service Fabric-klustret i Azure.</span><span class="sxs-lookup"><span data-stu-id="0db83-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello application tooyour Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="0db83-227">En ASP.NET Core tillståndslösa tjänsten med namnet `fabric:/ApiApplication/WebApiService` bör körs i Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="0db83-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="0db83-228">Skapa en API-åtgärd</span><span class="sxs-lookup"><span data-stu-id="0db83-228">Create an API operation</span></span>

<span data-ttu-id="0db83-229">Nu är klar toocreate en åtgärd i API Management hello det externa klienter Använd toocommunicate med ASP.NET Core tillståndslösa tjänsten som körs i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0db83-229">Now we're ready toocreate an operation in API Management that external clients use toocommunicate with hello ASP.NET Core stateless service running in hello Service Fabric cluster.</span></span>

 1. <span data-ttu-id="0db83-230">Logga in toohello Azure-portalen och navigera tooyour API Management service-distributionen.</span><span class="sxs-lookup"><span data-stu-id="0db83-230">Log in toohello Azure portal and navigate tooyour API Management service deployment.</span></span>
 2. <span data-ttu-id="0db83-231">I hello API Management service-bladet välj **API: er – förhandsgranskning**</span><span class="sxs-lookup"><span data-stu-id="0db83-231">In hello API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="0db83-232">Lägg till ett nytt API genom att klicka på hello **tomt API** rutan och fyller i dialogrutan för hello:</span><span class="sxs-lookup"><span data-stu-id="0db83-232">Add a new API by clicking hello **Blank API** box and filling out hello dialog box:</span></span>

     - <span data-ttu-id="0db83-233">**Webbtjänstens URL**: för Service Fabric serverdelar används inte den här URL-värdet.</span><span class="sxs-lookup"><span data-stu-id="0db83-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="0db83-234">Du kan ange ett värde här.</span><span class="sxs-lookup"><span data-stu-id="0db83-234">You can put any value here.</span></span> <span data-ttu-id="0db83-235">Den här kursen använder: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="0db83-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="0db83-236">**Namnet**: Ange ett namn för din API.</span><span class="sxs-lookup"><span data-stu-id="0db83-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="0db83-237">Den här kursen använder `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="0db83-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="0db83-238">**URL-schema**: Välj HTTP, HTTPS eller båda.</span><span class="sxs-lookup"><span data-stu-id="0db83-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="0db83-239">Den här kursen använder `both`.</span><span class="sxs-lookup"><span data-stu-id="0db83-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="0db83-240">**API-URL-Suffix**: Ange ett suffix för vårt API.</span><span class="sxs-lookup"><span data-stu-id="0db83-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="0db83-241">Den här kursen använder `myapp`.</span><span class="sxs-lookup"><span data-stu-id="0db83-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="0db83-242">När hello API har skapats klickar du på **+ Lägg till åtgärden** tooadd en frontend-API-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0db83-242">Once hello API is created, click **+ Add operation** tooadd a front-end API operation.</span></span> <span data-ttu-id="0db83-243">Fyll ut hello värden:</span><span class="sxs-lookup"><span data-stu-id="0db83-243">Fill out hello values:</span></span>
    
     - <span data-ttu-id="0db83-244">**URL: en**: Välj `GET` och ange en URL-sökväg för hello API.</span><span class="sxs-lookup"><span data-stu-id="0db83-244">**URL**: Select `GET` and provide a URL path for hello API.</span></span> <span data-ttu-id="0db83-245">Den här kursen använder `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="0db83-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="0db83-246">Hello URL-sökväg anges här skickas hello URL-sökvägen toohello serverdelstjänst Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0db83-246">By default, hello URL path specified here is hello URL path sent toohello backend Service Fabric service.</span></span> <span data-ttu-id="0db83-247">Om du använder hello samma URL-sökvägen här som tjänsten använder i det här fallet `/api/values`, sedan hello åtgärden fungerar utan ytterligare ändringar.</span><span class="sxs-lookup"><span data-stu-id="0db83-247">If you use hello same URL path here that your service uses, in this case `/api/values`, then hello operation works without further modification.</span></span> <span data-ttu-id="0db83-248">Du kan också ange en URL-sökväg här som skiljer sig från hello URL-sökvägen som används av din serverdel Service Fabric-tjänsten, då du kommer också att behovet av toospecify omarbetning av en sökväg i din princip för åtgärden senare.</span><span class="sxs-lookup"><span data-stu-id="0db83-248">You may also specify a URL path here that is different from hello URL path used by your backend Service Fabric service, in which case you will also need toospecify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="0db83-249">**Visningsnamn**: Ange ett namn för hello API.</span><span class="sxs-lookup"><span data-stu-id="0db83-249">**Display name**: Provide any name for hello API.</span></span> <span data-ttu-id="0db83-250">Den här kursen använder `Values`.</span><span class="sxs-lookup"><span data-stu-id="0db83-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="0db83-251">Konfigurera en backend-princip</span><span class="sxs-lookup"><span data-stu-id="0db83-251">Configure a backend policy</span></span>

<span data-ttu-id="0db83-252">hello backend principen kopplar samman allt tillsammans.</span><span class="sxs-lookup"><span data-stu-id="0db83-252">hello backend policy ties everything together.</span></span> <span data-ttu-id="0db83-253">Det är där du konfigurerar hello backend Service Fabric toowhich tjänstbegäranden dirigeras.</span><span class="sxs-lookup"><span data-stu-id="0db83-253">This is where you configure hello backend Service Fabric service toowhich requests are routed.</span></span> <span data-ttu-id="0db83-254">Du kan använda den här principen tooany API-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0db83-254">You can apply this policy tooany API operation.</span></span> <span data-ttu-id="0db83-255">Hej [backend-konfiguration för Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) innehåller hello följande begär routning kontroller:</span><span class="sxs-lookup"><span data-stu-id="0db83-255">hello [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides hello following request routing controls:</span></span> 
 - <span data-ttu-id="0db83-256">Instansurval av-tjänsten genom att ange ett Service Fabric-tjänstnamn, antingen hårdkodad (till exempel `"fabric:/myapp/myservice"`) eller genereras från hello HTTP-begäran (till exempel `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="0db83-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from hello HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="0db83-257">Partitionen lösning genom att generera en partitionsnyckel med alla Service Fabric-partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="0db83-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="0db83-258">Val av replik för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="0db83-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="0db83-259">Lösning försök villkor som gör att du toospecify hello villkor för återlösa på en plats och skicka en begäran.</span><span class="sxs-lookup"><span data-stu-id="0db83-259">Resolution retry conditions that allow you toospecify hello conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="0db83-260">För den här självstudiekursen skapar du en backend-princip att vägar direkt begär toohello ASP.NET Core tillståndslösa tjänsten har distribuerats tidigare:</span><span class="sxs-lookup"><span data-stu-id="0db83-260">For this tutorial, create a backend policy that routes requests directly toohello ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="0db83-261">Markera och redigera hello **inkommande principer** för hello `Values` igen genom att klicka på redigeringsikonen hello och sedan välja **kodvy**.</span><span class="sxs-lookup"><span data-stu-id="0db83-261">Select and edit hello **inbound policies** for hello `Values` operation by clicking hello edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="0db83-262">I Redigeraren för hello kod lägger du till en `set-backend-service` princip under inkommande principer som visas här och på hello **spara** knappen:</span><span class="sxs-lookup"><span data-stu-id="0db83-262">In hello policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click hello **Save** button:</span></span>
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

<span data-ttu-id="0db83-263">En fullständig uppsättning attribut för Service Fabric-backend-principen, finns i toohello [API Management backend-dokumentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="0db83-263">For a full set of Service Fabric back-end policy attributes, refer toohello [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-hello-api-tooa-product"></a><span data-ttu-id="0db83-264">Lägga till hello API tooa produkt.</span><span class="sxs-lookup"><span data-stu-id="0db83-264">Add hello API tooa product.</span></span> 

<span data-ttu-id="0db83-265">Innan du kan anropa hello API, måste den läggas tooa produkten där kan du bevilja åtkomst toousers.</span><span class="sxs-lookup"><span data-stu-id="0db83-265">Before you can call hello API, it must be added tooa product where you can grant access toousers.</span></span> 

 1. <span data-ttu-id="0db83-266">Välj i hello API Management-tjänsten, **produkter - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="0db83-266">In hello API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="0db83-267">Som standard API Management providers två produkter: Start och obegränsad.</span><span class="sxs-lookup"><span data-stu-id="0db83-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="0db83-268">Välj hello obegränsade produkten.</span><span class="sxs-lookup"><span data-stu-id="0db83-268">Select hello Unlimited product.</span></span>
 3. <span data-ttu-id="0db83-269">Välj API: er.</span><span class="sxs-lookup"><span data-stu-id="0db83-269">Select APIs.</span></span>
 4. <span data-ttu-id="0db83-270">Klicka på hello **+ Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0db83-270">Click hello **+ Add** button.</span></span>
 5. <span data-ttu-id="0db83-271">Välj hello `Service Fabric App` API du skapade i föregående steg i hello och klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="0db83-271">Select hello `Service Fabric App` API you created in hello previous steps and click hello **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="0db83-272">testa den</span><span class="sxs-lookup"><span data-stu-id="0db83-272">Test it</span></span>

<span data-ttu-id="0db83-273">Nu kan du försöka skicka en begäran tooyour backend-tjänst i Service Fabric via API Management direkt från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0db83-273">You can now try sending a request tooyour back-end service in Service Fabric through API Management directly from hello Azure portal.</span></span>

 1. <span data-ttu-id="0db83-274">Välj i hello API Management-tjänsten, **API - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="0db83-274">In hello API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="0db83-275">I hello `Service Fabric App` API som du skapade i föregående steg för hello, Välj hello **Test** fliken.</span><span class="sxs-lookup"><span data-stu-id="0db83-275">In hello `Service Fabric App` API you created in hello previous steps, select hello **Test** tab.</span></span>
 3. <span data-ttu-id="0db83-276">Klicka på hello **skicka** knappen toosend en test begäran toohello backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0db83-276">Click hello **Send** button toosend a test request toohello backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0db83-277">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0db83-277">Next steps</span></span>

<span data-ttu-id="0db83-278">Du bör nu ha en grundläggande installation med Service Fabric och API-hantering.</span><span class="sxs-lookup"><span data-stu-id="0db83-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="0db83-279">Den här kursen använder grundläggande certifikatbaserad användarautentisering för din tooget för Service Fabric-klustret som du komma igång snabbt.</span><span class="sxs-lookup"><span data-stu-id="0db83-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster tooget you set up quickly.</span></span> <span data-ttu-id="0db83-280">Mer avancerade användarautentisering för Service Fabric-kluster är bättre med [Azure Active Directory-autentisering](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="0db83-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="0db83-281">Nästa [skapa och konfigurera avancerad produktinställningarna i Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare ditt program för verkliga världen trafik.</span><span class="sxs-lookup"><span data-stu-id="0db83-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare your application for real world traffic.</span></span>

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
