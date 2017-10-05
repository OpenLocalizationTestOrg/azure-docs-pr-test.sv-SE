---
title: Azure Service Fabric med API Management Snabbstart | Microsoft Docs
description: "Den här guiden visar hur du snabbt kommer igång med Azure API Management och Service Fabric."
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
ms.openlocfilehash: e9f44d8a43d274768f43261fea68f0da9c681ae1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="83181-103">Service Fabric med Azure API Management-Snabbstart</span><span class="sxs-lookup"><span data-stu-id="83181-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="83181-104">Den här guiden visar hur du ställer in Azure API Management med Service Fabric och konfigurera din första API-åtgärden för att skicka trafik till backend-tjänster i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="83181-104">This guide shows you how to set up Azure API Management with Service Fabric and configure your first API operation to send traffic to back-end services in Service Fabric.</span></span> <span data-ttu-id="83181-105">Mer information om Azure API Management scenarier med Service Fabric finns i [översikt](service-fabric-api-management-overview.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="83181-105">To learn more about Azure API Management scenarios with Service Fabric, see the [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a><span data-ttu-id="83181-106">Distribuera API Management och Service Fabric till Azure</span><span class="sxs-lookup"><span data-stu-id="83181-106">Deploy API Management and Service Fabric to Azure</span></span>

<span data-ttu-id="83181-107">Det första steget är att distribuera API-hantering och ett Service Fabric-kluster till Azure i ett delat virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="83181-107">The first step is to deploy API Management and a Service Fabric cluster to Azure in a shared Virtual Network.</span></span> <span data-ttu-id="83181-108">Detta gör att API-hantering att kommunicera direkt med Service Fabric så att den kan utföra identifiering, service partition upplösning och vidarebefordra trafik direkt till alla backend-tjänster i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="83181-108">This allows API Management to communicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly to any backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="83181-109">topologi</span><span class="sxs-lookup"><span data-stu-id="83181-109">Topology</span></span>

<span data-ttu-id="83181-110">Den här guiden distribuerar följande topologin till Azure som API-hantering och Service Fabric finns i undernät i samma virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="83181-110">This guide deploys the following topology to Azure in which API Management and Service Fabric are in subnets of the same Virtual Network:</span></span>

 ![Bildrubrik][sf-apim-topology-overview]

<span data-ttu-id="83181-112">Resource Manager-mallar har angetts för varje steg i distributionen för att snabbt komma igång:</span><span class="sxs-lookup"><span data-stu-id="83181-112">To get started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="83181-113">Nätverkstopologi:</span><span class="sxs-lookup"><span data-stu-id="83181-113">Network topology:</span></span>
    - <span data-ttu-id="83181-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="83181-115">[Network.parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="83181-116">Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="83181-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="83181-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="83181-118">[cluster.parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="83181-119">API-hantering:</span><span class="sxs-lookup"><span data-stu-id="83181-119">API Management:</span></span>
    - <span data-ttu-id="83181-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="83181-121">[APIM.parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-to-azure-and-select-your-subscription"></a><span data-ttu-id="83181-122">Logga in på Azure och välja din prenumeration</span><span class="sxs-lookup"><span data-stu-id="83181-122">Sign in to Azure and select your subscription</span></span>

<span data-ttu-id="83181-123">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="83181-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="83181-124">När du startar en ny PowerShell-session, logga in på ditt Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="83181-124">When you start a new PowerShell session, sign in to your Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="83181-125">Logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="83181-125">Sign in to your Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="83181-126">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="83181-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="83181-127">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="83181-127">Create a resource group</span></span>

<span data-ttu-id="83181-128">Skapa en ny resursgrupp för din distribution.</span><span class="sxs-lookup"><span data-stu-id="83181-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="83181-129">Ange ett namn och en plats.</span><span class="sxs-lookup"><span data-stu-id="83181-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a><span data-ttu-id="83181-130">Distribuera nätverkets topologi</span><span class="sxs-lookup"><span data-stu-id="83181-130">Deploy the network topology</span></span>

<span data-ttu-id="83181-131">Det första steget är att ställa in nätverkets topologi som API Management och Service Fabric-klustret kommer att distribueras.</span><span class="sxs-lookup"><span data-stu-id="83181-131">The first step is to set up the network topology to which API Management and the Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="83181-132">Den [network.json] [ network-arm] Resource Manager-mall har konfigurerats för att skapa virtuella nätverk (VNET) med två undernät och två Nätverkssäkerhetsgrupp grupper (NSG).</span><span class="sxs-lookup"><span data-stu-id="83181-132">The [network.json][network-arm] Resource Manager template is configured to create a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="83181-133">Den [network.parameters.json] [ network-parameters-arm] parameterfilen innehåller namnen på undernät och NSG: er som API Management och Service Fabric ska distribueras till.</span><span class="sxs-lookup"><span data-stu-id="83181-133">The [network.parameters.json][network-parameters-arm] parameters file contains the names of the subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="83181-134">Den här guiden behöver inte parametervärden som ska ändras.</span><span class="sxs-lookup"><span data-stu-id="83181-134">For this guide, the parameter values do not need to be changed.</span></span> <span data-ttu-id="83181-135">API-hantering och Service Fabric Resource Manager mallar att dessa värden, så om de ändras här måste du ändra dem i andra Resource Manager-mallar i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="83181-135">The API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in the other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="83181-136">Hämta följande Resource Manager-mall och parametrar filen:</span><span class="sxs-lookup"><span data-stu-id="83181-136">Download the following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="83181-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="83181-138">[Network.parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="83181-139">Använd följande PowerShell-kommando för att distribuera Resource Manager-mall och parametern-filer för installation av nätverk:</span><span class="sxs-lookup"><span data-stu-id="83181-139">Use the following PowerShell command to deploy the Resource Manager template and parameter files for the network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a><span data-ttu-id="83181-140">Distribuera Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="83181-140">Deploy the Service Fabric cluster</span></span>

<span data-ttu-id="83181-141">När nätverksresurser är klar för distribution, är nästa steg att distribuera ett Service Fabric-kluster till VNET i undernät och NSG: N för Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-141">Once the network resources have finished deploying, the next step is to deploy a Service Fabric cluster to the VNET in the subnet and NSG designated for the Service Fabric cluster.</span></span> <span data-ttu-id="83181-142">För den här kursen är Service Fabric Resource Manager-mallen förkonfigurerad att använda namnen på virtuella nätverk, undernät och NSG: N som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="83181-142">For this tutorial, the Service Fabric Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

<span data-ttu-id="83181-143">Resource Manager-mall Service Fabric-klustret har konfigurerats för att skapa en säker kluster med Certifikatsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="83181-143">The Service Fabric cluster Resource Manager template is configured to create a secure cluster with certificate security.</span></span> <span data-ttu-id="83181-144">Certifikatet används för säker kommunikation nod till noden för klustret och för att hantera användarnas åtkomst till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-144">The certificate is used to secure node-to-node communication for your cluster and to manage user access to your Service Fabric cluster.</span></span> <span data-ttu-id="83181-145">API Management använder detta certifikat för att få åtkomst till Service Fabric Naming Service för identifiering av tjänst.</span><span class="sxs-lookup"><span data-stu-id="83181-145">API Management uses this certificate to access  the Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="83181-146">Det här steget kräver att ett certifikat i Key Vault för Klustersäkerhet.</span><span class="sxs-lookup"><span data-stu-id="83181-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="83181-147">Mer information om hur du skapar en säker kluster med Key Vault finns [den här guiden om hur du skapar ett kluster i Azure med hjälp av hanteraren för filserverresurser](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="83181-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="83181-148">Du kan lägga till Azure Active Directory-autentisering utöver det certifikat som används för åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-148">You may add Azure Active Directory authentication in addition to the certificate used for cluster access.</span></span> <span data-ttu-id="83181-149">Azure Active Directory är det rekommenderade sättet att hantera användarnas åtkomst till Service Fabric-kluster, men är inte nödvändigt att slutföra den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="83181-149">Azure Active Directory is the recommended way to manage user access to your Service Fabric cluster, but is not necessary to complete this tutorial.</span></span> <span data-ttu-id="83181-150">Ett certifikat krävs oavsett hur för klustret nod till nod säkerhet och Azure API Management-autentisering, vilket inte stöder för närvarande autentisera med Azure Active Directory för en Service Fabric-serverdel.</span><span class="sxs-lookup"><span data-stu-id="83181-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="83181-151">Hämta följande Resource Manager-mall och parametrar filen:</span><span class="sxs-lookup"><span data-stu-id="83181-151">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="83181-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="83181-153">[cluster.parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="83181-154">Fyll i de tomma parametrarna i den `cluster.parameters.json` filen för din distribution, inklusive den [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) för kluster-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="83181-154">Fill in the empty parameters in the `cluster.parameters.json` file for your deployment, including the [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="83181-155">Använd följande PowerShell-kommando för att distribuera Resource Manager-mall och parametern-filer för att skapa Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="83181-155">Use the following PowerShell command to deploy the Resource Manager template and parameter files to create the Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="83181-156">Distribuera API Management</span><span class="sxs-lookup"><span data-stu-id="83181-156">Deploy API Management</span></span>

<span data-ttu-id="83181-157">Slutligen kan du distribuera API-hantering till VNET i undernät och NSG för API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-157">Finally, deploy API Management to the VNET in the subnet and NSG designated for API Management.</span></span> <span data-ttu-id="83181-158">Du behöver inte vänta för Service Fabric-kluster-distributionen ska slutföras innan du distribuerar API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-158">You do not need to wait for the Service Fabric cluster deployment to finish before deploying API Management.</span></span> 

<span data-ttu-id="83181-159">För den här kursen är API Management Resource Manager-mallen förkonfigurerad att använda namnen på virtuella nätverk, undernät och NSG: N som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="83181-159">For this tutorial, the API Management Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

 1. <span data-ttu-id="83181-160">Hämta följande Resource Manager-mall och parametrar filen:</span><span class="sxs-lookup"><span data-stu-id="83181-160">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="83181-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="83181-162">[APIM.parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="83181-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="83181-163">Fyll i de tomma parametrarna i den `apim.parameters.json` för din distribution.</span><span class="sxs-lookup"><span data-stu-id="83181-163">Fill in the empty parameters in the `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="83181-164">Använd följande PowerShell-kommando för att distribuera mallen och parametern filer för Resource Manager för API-hantering:</span><span class="sxs-lookup"><span data-stu-id="83181-164">Use the following PowerShell command to deploy the Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="83181-165">Konfigurera API-hantering</span><span class="sxs-lookup"><span data-stu-id="83181-165">Configure API Management</span></span>

<span data-ttu-id="83181-166">När din API-hantering och Service Fabric-klustret har distribuerats, kan du konfigurera en Service Fabric-serverdel i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="83181-167">På så sätt kan du skapa en princip för backend-tjänsten som skickar trafik till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-167">This allows you to create a backend service policy that sends traffic to your Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="83181-168">API Management-säkerhet</span><span class="sxs-lookup"><span data-stu-id="83181-168">API Management Security</span></span>

<span data-ttu-id="83181-169">Om du vill konfigurera Service Fabric-serverdelen måste du först konfigurera säkerhetsinställningar för API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-169">To configure the Service Fabric backend, you first need to configure API Management security settings.</span></span> <span data-ttu-id="83181-170">Gå till din API Management-tjänst i Azure-portalen för att konfigurera säkerhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="83181-170">To configure security settings, go to your API Management service in the Azure portal.</span></span>

#### <a name="enable-the-api-management-rest-api"></a><span data-ttu-id="83181-171">Aktivera API Management REST API</span><span class="sxs-lookup"><span data-stu-id="83181-171">Enable the API Management REST API</span></span>

<span data-ttu-id="83181-172">API Management REST API är för närvarande det enda sättet att konfigurera en backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="83181-172">The API Management REST API is currently the only way to configure a backend service.</span></span> <span data-ttu-id="83181-173">Det första steget är att aktivera API Management REST API och skydda den.</span><span class="sxs-lookup"><span data-stu-id="83181-173">The first step is to enable the API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="83181-174">Välj i API Management-tjänsten **Management API - PREVIEW** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="83181-174">In the API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="83181-175">Kontrollera den **aktivera API Management REST API** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="83181-175">Check the **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="83181-176">Observera Management API-URL - detta är den URL som vi använder senare att ställa in serverdelen Service Fabric</span><span class="sxs-lookup"><span data-stu-id="83181-176">Note the Management API URL - this is the URL we'll use later to set up the Service Fabric backend</span></span>
 4. <span data-ttu-id="83181-177">Generera en **åtkomsttoken** genom att välja ett förfallodatum och en nyckel, klicka på den **generera** knappen längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="83181-177">Generate an **access Token** by selecting an expiry date and a key, then click the **Generate** button toward the bottom of the page.</span></span>
 5. <span data-ttu-id="83181-178">Kopiera den **åtkomsttoken** och spara den – vi använder informationen i följande steg.</span><span class="sxs-lookup"><span data-stu-id="83181-178">Copy the **access token** and save it - we'll use this in the following steps.</span></span> <span data-ttu-id="83181-179">Observera att detta skiljer sig från den primära och sekundära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="83181-179">Note this is different from the primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="83181-180">Överföra ett certifikat för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="83181-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="83181-181">API Management måste autentisera med Service Fabric-klustret för identifiering av tjänst använder ett klientcertifikat som har åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access to your cluster.</span></span> <span data-ttu-id="83181-182">För enkelhetens skull använder den här kursen samma certifikat som anges när du skapar Service Fabric-kluster, vilket som standard kan användas för åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-182">For simplicity, this tutorial uses the same certificate specified when creating the Service Fabric cluster, which by default can be used to access your cluster.</span></span>

 1. <span data-ttu-id="83181-183">Välj i API Management-tjänsten **klientcertifikat - PREVIEW** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="83181-183">In the API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="83181-184">Klicka på den **+ Lägg till** knappen</span><span class="sxs-lookup"><span data-stu-id="83181-184">Click the **+ Add** button</span></span>
 2. <span data-ttu-id="83181-185">Välj privat nyckel fil (.pfx) för kluster-certifikatet som du angav när du skapar Service Fabric-kluster, ger det ett namn och ange lösenordet för privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="83181-185">Select the private key file (.pfx) of the cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide the private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="83181-186">Den här kursen använder samma certifikat för klientautentisering och kluster nod till noden säkerhet.</span><span class="sxs-lookup"><span data-stu-id="83181-186">This tutorial uses the same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="83181-187">Du kan använda ett separat klientcertifikat om du har en konfigurerad för att komma åt Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-187">You may use a separate client certificate if you have one configured to access your Service Fabric cluster.</span></span>

### <a name="configure-the-backend"></a><span data-ttu-id="83181-188">Konfigurera backend</span><span class="sxs-lookup"><span data-stu-id="83181-188">Configure the backend</span></span>

<span data-ttu-id="83181-189">Nu när API Management-säkerheten är konfigurerad, kan du konfigurera Service Fabric-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="83181-189">Now that API Management security is configured, you can configure the Service Fabric backend.</span></span> <span data-ttu-id="83181-190">För Service Fabric serverdelar är Service Fabric-klustret serverdelen i stället för en specifik tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="83181-190">For Service Fabric backends, the Service Fabric cluster is the backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="83181-191">På så sätt kan en enda princip att dirigera till mer än en tjänst i klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-191">This allows a single policy to route to more than one service in the cluster.</span></span>

<span data-ttu-id="83181-192">Det här steget kräver åtkomst-token som du tidigare genererade och tumavtrycket för ditt kluster-certifikat som du har överfört till API-hantering i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="83181-192">This step requires the access token you generated earlier and the thumbprint for your cluster certificate you uploaded to API Management in the previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="83181-193">Om du använder ett separat klientcertifikat i föregående steg för API-hantering, behöver du tumavtrycket för klientcertifikatet förutom klustret certifikat-tumavtrycket i det här steget.</span><span class="sxs-lookup"><span data-stu-id="83181-193">If you used a separate client certificate in the previous step for API Management, you need the thumbprint for the client certificate in addition to the cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="83181-194">Skicka följande HTTP PUT begäran till URL: en för API Management API du antecknade tidigare när du aktiverar API Management REST API för att konfigurera tjänsten Service Fabric-serverdel.</span><span class="sxs-lookup"><span data-stu-id="83181-194">Send the following HTTP PUT request to the API Management API URL you noted earlier when enabling the API Management REST API to configure the Service Fabric backend service.</span></span> <span data-ttu-id="83181-195">Du bör se en `HTTP 201 Created` svar när kommandot slutförs.</span><span class="sxs-lookup"><span data-stu-id="83181-195">You should see an `HTTP 201 Created` response when the command succeeds.</span></span> <span data-ttu-id="83181-196">Mer information om varje fält finns i API Management [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="83181-196">For more information on each field, see the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="83181-197">HTTP-kommando och URL:</span><span class="sxs-lookup"><span data-stu-id="83181-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="83181-198">Huvuden för begäran:</span><span class="sxs-lookup"><span data-stu-id="83181-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="83181-199">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="83181-199">Request body:</span></span>
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

<span data-ttu-id="83181-200">Den **url** här parametern är en fullständigt kvalificerade namn för en tjänst i klustret som alla begäranden som dirigeras till som standard om inget namn anges i en backend-princip.</span><span class="sxs-lookup"><span data-stu-id="83181-200">The **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed to by default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="83181-201">Du kan använda falska tjänstnamn, till exempel ”fabric: / falska/service” om du inte vill ha en återställningsplats tjänst.</span><span class="sxs-lookup"><span data-stu-id="83181-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend to have a fallback service.</span></span>

<span data-ttu-id="83181-202">Referera till API-hantering [backend API-referensdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) för mer information om varje fält.</span><span class="sxs-lookup"><span data-stu-id="83181-202">Refer to the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="83181-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="83181-203">Example</span></span>

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

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="83181-204">Distribuera en backend-tjänst för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="83181-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="83181-205">Nu när du har den Service Fabric som konfigurerats som en serverdel för API Management skapar du principer för serverdelen för dina API: er som skickar trafik till Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="83181-205">Now that you have the Service Fabric configured as a backend to API Management, you can author backend policies for your APIs that send traffic to your Service Fabric services.</span></span> <span data-ttu-id="83181-206">Men du måste först en tjänst som körs i Service Fabric att acceptera begäranden.</span><span class="sxs-lookup"><span data-stu-id="83181-206">But first you need a service running in Service Fabric to accept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="83181-207">Skapa ett Service Fabric-tjänsten med en HTTP-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="83181-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="83181-208">Den här självstudiekursen skapar vi en grundläggande tillståndslös ASP.NET Core tillförlitlig tjänst med standardmallen för Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="83181-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using the default Web API project template.</span></span> <span data-ttu-id="83181-209">Detta skapar en HTTP-slutpunkt för din tjänst som du ska visa genom Azure API Management:</span><span class="sxs-lookup"><span data-stu-id="83181-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="83181-210">Börja med [ställa in din utvecklingsmiljö för utveckling av ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="83181-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="83181-211">När din utvecklingsmiljö har ställts in, starta Visual Studio som administratör och skapa en ASP.NET Core-tjänst:</span><span class="sxs-lookup"><span data-stu-id="83181-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="83181-212">Välj Arkiv -> Nytt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83181-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="83181-213">Välj mall för Service Fabric program i molnet och ger den namnet **”ApiApplication”**.</span><span class="sxs-lookup"><span data-stu-id="83181-213">Select the Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="83181-214">Välj mallen ASP.NET Core-tjänst och namnge projektet **”WebApiService”**.</span><span class="sxs-lookup"><span data-stu-id="83181-214">Select the ASP.NET Core service template and name the project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="83181-215">Välj mall för Web API ASP.NET Core 1.1-projekt.</span><span class="sxs-lookup"><span data-stu-id="83181-215">Select the Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="83181-216">När projektet har skapats kan du öppna `PackageRoot\ServiceManifest.xml` och ta bort den `Port` attribut från slutpunktskonfigurationen för resursen:</span><span class="sxs-lookup"><span data-stu-id="83181-216">Once the project is created, open `PackageRoot\ServiceManifest.xml` and remove the `Port` attribute from the endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="83181-217">Detta gör att Service Fabric anger vilken port dynamiskt från programmet portintervall, som vi öppnas via Nätverkssäkerhetsgruppen i klustret Resource Manager-mall så att trafik till den från API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-217">This allows Service Fabric to specify a port dynamically from the application port range, which we opened through the Network Security Group in the cluster Resource Manager template, allowing traffic to flow to it from API Management.</span></span>
 
 6. <span data-ttu-id="83181-218">Tryck på F5 i Visual Studio för att verifiera ditt webb-API finns lokalt.</span><span class="sxs-lookup"><span data-stu-id="83181-218">Press F5 in Visual Studio to verify the web API is available locally.</span></span> 

    <span data-ttu-id="83181-219">Öppna Service Fabric Explorer och detaljnivån ned till en specifik instans av ASP.NET Core-tjänsten för att se basadressen tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="83181-219">Open Service Fabric Explorer and drill down to a specific instance of the ASP.NET Core service to see the base address the service is listening on.</span></span> <span data-ttu-id="83181-220">Lägg till `/api/values` för basen som adressen och öppna den i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="83181-220">Add `/api/values` to the base address and open it in a browser.</span></span> <span data-ttu-id="83181-221">Detta anropar hämta-metoden på ValuesController i Web API-mallen.</span><span class="sxs-lookup"><span data-stu-id="83181-221">This invokes the Get method on the ValuesController in the Web API template.</span></span> <span data-ttu-id="83181-222">Standardsvar som tillhandahålls av mallen, en JSON-matris som innehåller två strängar returnerar:</span><span class="sxs-lookup"><span data-stu-id="83181-222">It returns the default response that is provided by the template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="83181-223">Det här är den slutpunkt som du ska visa genom API Management i Azure.</span><span class="sxs-lookup"><span data-stu-id="83181-223">This is the endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="83181-224">Slutligen kan du distribuera programmet till klustret i Azure.</span><span class="sxs-lookup"><span data-stu-id="83181-224">Finally, deploy the application to your cluster in Azure.</span></span> <span data-ttu-id="83181-225">[Med Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), högerklicka på projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="83181-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click the Application project and select **Publish**.</span></span> <span data-ttu-id="83181-226">Ange din klusterslutpunkten (till exempel `mycluster.westus.cloudapp.azure.com:19000`) att distribuera programmet till Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="83181-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) to deploy the application to your Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="83181-227">En ASP.NET Core tillståndslösa tjänsten med namnet `fabric:/ApiApplication/WebApiService` bör körs i Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="83181-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="83181-228">Skapa en API-åtgärd</span><span class="sxs-lookup"><span data-stu-id="83181-228">Create an API operation</span></span>

<span data-ttu-id="83181-229">Vi är nu redo att skapa en åtgärd i API-hantering med externa klienter som kommunicerar med ASP.NET Core tillståndslösa tjänsten som körs i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="83181-229">Now we're ready to create an operation in API Management that external clients use to communicate with the ASP.NET Core stateless service running in the Service Fabric cluster.</span></span>

 1. <span data-ttu-id="83181-230">Logga in på Azure-portalen och navigera till din API Management service-distributionen.</span><span class="sxs-lookup"><span data-stu-id="83181-230">Log in to the Azure portal and navigate to your API Management service deployment.</span></span>
 2. <span data-ttu-id="83181-231">I bladet API Management-tjänsten väljer **API: er – förhandsgranskning**</span><span class="sxs-lookup"><span data-stu-id="83181-231">In the API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="83181-232">Lägg till ett nytt API genom att klicka på den **tomt API** rutan och fylla i dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="83181-232">Add a new API by clicking the **Blank API** box and filling out the dialog box:</span></span>

     - <span data-ttu-id="83181-233">**Webbtjänstens URL**: för Service Fabric serverdelar används inte den här URL-värdet.</span><span class="sxs-lookup"><span data-stu-id="83181-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="83181-234">Du kan ange ett värde här.</span><span class="sxs-lookup"><span data-stu-id="83181-234">You can put any value here.</span></span> <span data-ttu-id="83181-235">Den här kursen använder: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="83181-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="83181-236">**Namnet**: Ange ett namn för din API.</span><span class="sxs-lookup"><span data-stu-id="83181-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="83181-237">Den här kursen använder `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="83181-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="83181-238">**URL-schema**: Välj HTTP, HTTPS eller båda.</span><span class="sxs-lookup"><span data-stu-id="83181-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="83181-239">Den här kursen använder `both`.</span><span class="sxs-lookup"><span data-stu-id="83181-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="83181-240">**API-URL-Suffix**: Ange ett suffix för vårt API.</span><span class="sxs-lookup"><span data-stu-id="83181-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="83181-241">Den här kursen använder `myapp`.</span><span class="sxs-lookup"><span data-stu-id="83181-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="83181-242">När API: N har skapats klickar du på **+ Lägg till åtgärden** att lägga till en frontend API-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="83181-242">Once the API is created, click **+ Add operation** to add a front-end API operation.</span></span> <span data-ttu-id="83181-243">Fyll i värden:</span><span class="sxs-lookup"><span data-stu-id="83181-243">Fill out the values:</span></span>
    
     - <span data-ttu-id="83181-244">**URL: en**: Välj `GET` och ange en URL-sökväg för API: et.</span><span class="sxs-lookup"><span data-stu-id="83181-244">**URL**: Select `GET` and provide a URL path for the API.</span></span> <span data-ttu-id="83181-245">Den här kursen använder `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="83181-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="83181-246">URL-sökväg anges här skickas URL-sökvägen till serverdelen Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83181-246">By default, the URL path specified here is the URL path sent to the backend Service Fabric service.</span></span> <span data-ttu-id="83181-247">Om du använder samma URL-sökvägen här som tjänsten använder i det här fallet `/api/values`, sedan åtgärden fungerar utan ytterligare ändringar.</span><span class="sxs-lookup"><span data-stu-id="83181-247">If you use the same URL path here that your service uses, in this case `/api/values`, then the operation works without further modification.</span></span> <span data-ttu-id="83181-248">Du kan också ange en URL-sökväg här som skiljer sig från URL-sökvägen som används av din serverdel Service Fabric-tjänsten, i så fall behöver du också ange en sökväg omarbetning i principen för åtgärden senare.</span><span class="sxs-lookup"><span data-stu-id="83181-248">You may also specify a URL path here that is different from the URL path used by your backend Service Fabric service, in which case you will also need to specify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="83181-249">**Visningsnamn**: Ange ett namn för API: et.</span><span class="sxs-lookup"><span data-stu-id="83181-249">**Display name**: Provide any name for the API.</span></span> <span data-ttu-id="83181-250">Den här kursen använder `Values`.</span><span class="sxs-lookup"><span data-stu-id="83181-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="83181-251">Konfigurera en backend-princip</span><span class="sxs-lookup"><span data-stu-id="83181-251">Configure a backend policy</span></span>

<span data-ttu-id="83181-252">Backend-principen kopplar samman allt tillsammans.</span><span class="sxs-lookup"><span data-stu-id="83181-252">The backend policy ties everything together.</span></span> <span data-ttu-id="83181-253">Det är där du konfigurerar Service Fabric serverdelstjänsten begäranden dirigeras.</span><span class="sxs-lookup"><span data-stu-id="83181-253">This is where you configure the backend Service Fabric service to which requests are routed.</span></span> <span data-ttu-id="83181-254">Du kan använda den här principen för alla API-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="83181-254">You can apply this policy to any API operation.</span></span> <span data-ttu-id="83181-255">Den [backend-konfiguration för Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) innehåller följande begär routning kontroller:</span><span class="sxs-lookup"><span data-stu-id="83181-255">The [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides the following request routing controls:</span></span> 
 - <span data-ttu-id="83181-256">Instansurval av-tjänsten genom att ange ett Service Fabric-tjänstnamn, antingen hårdkodad (till exempel `"fabric:/myapp/myservice"`) eller genereras från HTTP-begäran (till exempel `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="83181-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from the HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="83181-257">Partitionen lösning genom att generera en partitionsnyckel med alla Service Fabric-partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="83181-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="83181-258">Val av replik för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="83181-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="83181-259">Lösning försök villkor som gör att du kan ange villkor för återlösa på en plats och skicka en begäran.</span><span class="sxs-lookup"><span data-stu-id="83181-259">Resolution retry conditions that allow you to specify the conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="83181-260">För den här självstudiekursen skapar du en backend-princip som omdirigerar begäranden direkt till ASP.NET Core tillståndslösa tjänsten har distribuerats tidigare:</span><span class="sxs-lookup"><span data-stu-id="83181-260">For this tutorial, create a backend policy that routes requests directly to the ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="83181-261">Markera och redigera den **inkommande principer** för den `Values` igen genom att klicka på redigeringsikonen och sedan välja **kodvy**.</span><span class="sxs-lookup"><span data-stu-id="83181-261">Select and edit the **inbound policies** for the `Values` operation by clicking the edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="83181-262">I Redigeraren för grupprincipobjekt kod, lägger du till en `set-backend-service` princip under inkommande principer som visas här och klicka på den **spara** knappen:</span><span class="sxs-lookup"><span data-stu-id="83181-262">In the policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click the **Save** button:</span></span>
    
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

<span data-ttu-id="83181-263">För en fullständig uppsättning attribut för Service Fabric-backend-principen, referera till den [API Management backend-dokumentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="83181-263">For a full set of Service Fabric back-end policy attributes, refer to the [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-the-api-to-a-product"></a><span data-ttu-id="83181-264">Lägg till API: et för en produkt.</span><span class="sxs-lookup"><span data-stu-id="83181-264">Add the API to a product.</span></span> 

<span data-ttu-id="83181-265">Innan du kan anropa API: et, måste den läggas till en produkt där du kan ge åtkomst till användare.</span><span class="sxs-lookup"><span data-stu-id="83181-265">Before you can call the API, it must be added to a product where you can grant access to users.</span></span> 

 1. <span data-ttu-id="83181-266">Välj i API Management-tjänsten **produkter - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="83181-266">In the API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="83181-267">Som standard API Management providers två produkter: Start och obegränsad.</span><span class="sxs-lookup"><span data-stu-id="83181-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="83181-268">Välj obegränsade produkten.</span><span class="sxs-lookup"><span data-stu-id="83181-268">Select the Unlimited product.</span></span>
 3. <span data-ttu-id="83181-269">Välj API: er.</span><span class="sxs-lookup"><span data-stu-id="83181-269">Select APIs.</span></span>
 4. <span data-ttu-id="83181-270">Klicka på den **+ Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="83181-270">Click the **+ Add** button.</span></span>
 5. <span data-ttu-id="83181-271">Välj den `Service Fabric App` API som du skapade i föregående steg och klicka på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="83181-271">Select the `Service Fabric App` API you created in the previous steps and click the **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="83181-272">testa den</span><span class="sxs-lookup"><span data-stu-id="83181-272">Test it</span></span>

<span data-ttu-id="83181-273">Nu kan du försöka skicka en begäran till backend-tjänsten i Service Fabric via API Management direkt från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="83181-273">You can now try sending a request to your back-end service in Service Fabric through API Management directly from the Azure portal.</span></span>

 1. <span data-ttu-id="83181-274">Välj i API Management-tjänsten **API - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="83181-274">In the API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="83181-275">I den `Service Fabric App` API som du skapade i föregående steg, Välj den **Test** fliken.</span><span class="sxs-lookup"><span data-stu-id="83181-275">In the `Service Fabric App` API you created in the previous steps, select the **Test** tab.</span></span>
 3. <span data-ttu-id="83181-276">Klicka på den **skicka** knappen Skicka ett testbegäran till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83181-276">Click the **Send** button to send a test request to the backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83181-277">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83181-277">Next steps</span></span>

<span data-ttu-id="83181-278">Du bör nu ha en grundläggande installation med Service Fabric och API-hantering.</span><span class="sxs-lookup"><span data-stu-id="83181-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="83181-279">Den här kursen använder grundläggande certifikatbaserad användarautentisering för Service Fabric-kluster som du kan komma igång snabbt.</span><span class="sxs-lookup"><span data-stu-id="83181-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster to get you set up quickly.</span></span> <span data-ttu-id="83181-280">Mer avancerade användarautentisering för Service Fabric-kluster är bättre med [Azure Active Directory-autentisering](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="83181-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="83181-281">Nästa [skapa och konfigurera avancerad produktinställningarna i Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) att förbereda ditt program för verkliga världen trafik.</span><span class="sxs-lookup"><span data-stu-id="83181-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) to prepare your application for real world traffic.</span></span>

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
