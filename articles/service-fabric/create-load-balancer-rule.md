---
title: "aaaCreate en belastningsutjämnare för Azure-regel för ett kluster"
description: "Konfigurera en Azure-belastningsutjämnaren tooopen portar för Azure Service Fabric-klustret."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a><span data-ttu-id="abf77-103">Öppna portar för ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="abf77-103">Open ports for a Service Fabric cluster</span></span>

<span data-ttu-id="abf77-104">hello belastningsutjämnaren distribueras med Azure Service Fabric-klustret dirigerar trafik tooyour app som körs på en nod.</span><span class="sxs-lookup"><span data-stu-id="abf77-104">hello load balancer deployed with your Azure Service Fabric cluster directs traffic tooyour app running on a node.</span></span> <span data-ttu-id="abf77-105">Om du ändrar din app toouse en annan port måste du exponera porten (eller vidarebefordra en annan port) i hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="abf77-105">If you change your app toouse a different port, you must expose that port (or route a different port) in hello Azure Load Balancer.</span></span>

<span data-ttu-id="abf77-106">När du har distribuerat din service fabric-kluster tooAzure skapades automatiskt en belastningsutjämnare för dig.</span><span class="sxs-lookup"><span data-stu-id="abf77-106">When you deployed your service fabric cluster tooAzure, a load balancer was automatically created for you.</span></span> <span data-ttu-id="abf77-107">Om du inte har en belastningsutjämnare, se [konfigurera en Internetriktade belastningsutjämnare](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="abf77-107">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

## <a name="configure-service-fabric"></a><span data-ttu-id="abf77-108">Konfigurera service fabric</span><span class="sxs-lookup"><span data-stu-id="abf77-108">Configure service fabric</span></span>

<span data-ttu-id="abf77-109">Service Fabric-programmet **ServiceManifest.xml** config-fil som definierar ditt program förväntar sig toouse hello-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="abf77-109">Your Service Fabric application **ServiceManifest.xml** config file defines hello endpoints your application expects toouse.</span></span> <span data-ttu-id="abf77-110">När hello-konfigurationsfilen har uppdaterats toodefine en slutpunkt kan hello belastningsutjämnare måste vara uppdaterade tooexpose som (eller en annan) port.</span><span class="sxs-lookup"><span data-stu-id="abf77-110">After hello config file has been updated toodefine an endpoint, hello load balancer must be updated tooexpose that (or a different) port.</span></span> <span data-ttu-id="abf77-111">Mer information om hur toocreate hello fabric tjänstslutpunkten finns [konfigurera en slutpunkt](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="abf77-111">For more information on how toocreate hello service fabric endpoint, see [Setup an Endpoint](service-fabric-service-manifest-resources.md).</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="abf77-112">Skapa en regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="abf77-112">Create a load balancer rule</span></span>

<span data-ttu-id="abf77-113">En regel för belastningsutjämnare öppnar en port mot internet och vidarebefordrar trafik toohello interna nodens port som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="abf77-113">A load balancer rule opens up an internet-facing port and forwards traffic toohello internal node's port used by your application.</span></span> <span data-ttu-id="abf77-114">Om du inte har en belastningsutjämnare, se [konfigurera en Internetriktade belastningsutjämnare](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="abf77-114">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

<span data-ttu-id="abf77-115">toocreate belastningsutjämning regel måste toocollect hello följande information:</span><span class="sxs-lookup"><span data-stu-id="abf77-115">toocreate a load balancer rule, you need toocollect hello following information:</span></span>

- <span data-ttu-id="abf77-116">Belastningsutjämnarens namn.</span><span class="sxs-lookup"><span data-stu-id="abf77-116">Load balancer name.</span></span>
- <span data-ttu-id="abf77-117">Resursgruppen för hello läsa in belastningsutjämning och service fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="abf77-117">Resource group of hello load balancer and service fabric cluster.</span></span>
- <span data-ttu-id="abf77-118">Externa porten.</span><span class="sxs-lookup"><span data-stu-id="abf77-118">External port.</span></span>
- <span data-ttu-id="abf77-119">Intern port.</span><span class="sxs-lookup"><span data-stu-id="abf77-119">Internal port.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="abf77-120">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="abf77-120">Azure CLI</span></span>
>[!NOTE]
><span data-ttu-id="abf77-121">Om du måste toodetermine hello namnet på hello belastningsutjämnare använder du det här kommandot tooquickly hämta en lista över alla belastningsutjämnare och hello associerade resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="abf77-121">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and hello associated resource groups.</span></span>
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

<span data-ttu-id="abf77-122">Det tar bara en enda kommando toocreate en belastningsutjämningsregel med hello **Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="abf77-122">It only takes a single command toocreate a load balancer rule with hello **Azure CLI**.</span></span> <span data-ttu-id="abf77-123">Du behöver bara tooknow båda hello namnet på hello load balancer och resurs grupp toocreate en ny regel.</span><span class="sxs-lookup"><span data-stu-id="abf77-123">You just need tooknow both hello name of hello load balancer and resource group toocreate a new rule.</span></span>

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

<span data-ttu-id="abf77-124">hello Azure CLI-kommandot har några parametrar som beskrivs i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="abf77-124">hello Azure CLI command has a few parameters that are described in hello following table:</span></span>

| <span data-ttu-id="abf77-125">Parameter</span><span class="sxs-lookup"><span data-stu-id="abf77-125">Parameter</span></span> | <span data-ttu-id="abf77-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="abf77-126">Description</span></span> |
| --------- | ----------- |
| `--backend-port`  | <span data-ttu-id="abf77-127">hello port hello service fabric program lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="abf77-127">hello port hello service fabric application is listening to.</span></span> |
| `--frontend-port` | <span data-ttu-id="abf77-128">hello port hello att läsa in belastningsutjämning visar för externa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="abf77-128">hello port hello load balancer exposes for external connections.</span></span> |
| `-lb-name` | <span data-ttu-id="abf77-129">hello namnet på hello att läsa in belastningsutjämning toochange.</span><span class="sxs-lookup"><span data-stu-id="abf77-129">hello name of hello load balancer toochange.</span></span> |
| `-g`       | <span data-ttu-id="abf77-130">hello resursgrupp som har både hello belastningsutjämnare och service fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="abf77-130">hello resource group that has both hello load balancer and service fabric cluster.</span></span> |
| `-n`       | <span data-ttu-id="abf77-131">hello valt namn på hello regel.</span><span class="sxs-lookup"><span data-stu-id="abf77-131">hello chosen name of hello rule.</span></span> |


>[!NOTE]
><span data-ttu-id="abf77-132">Mer information om hur toocreate en belastningsutjämnare med hello Azure CLI, se [skapar en belastningsutjämnare med hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="abf77-132">For more information on how toocreate a load balancer with hello Azure CLI, see [Create a load balancer with hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="abf77-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="abf77-133">PowerShell</span></span>

>[!NOTE]
><span data-ttu-id="abf77-134">Om du behöver toodetermine hello namnet på hello belastningsutjämnare kan använda det här kommandot tooquickly get en lista över alla belastningsutjämnare och associerade resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="abf77-134">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and associated resource groups.</span></span>
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

<span data-ttu-id="abf77-135">PowerShell är lite mer komplicerat än hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="abf77-135">PowerShell is a little more complicated than hello Azure CLI.</span></span> <span data-ttu-id="abf77-136">Göra begreppsmässigt hello följande steg toocreate en regel.</span><span class="sxs-lookup"><span data-stu-id="abf77-136">Conceptually, do hello following steps toocreate a rule.</span></span>

1. <span data-ttu-id="abf77-137">Hämta hello belastningsutjämnaren från Azure.</span><span class="sxs-lookup"><span data-stu-id="abf77-137">Get hello load balancer from Azure.</span></span>
2. <span data-ttu-id="abf77-138">Skapa en regel.</span><span class="sxs-lookup"><span data-stu-id="abf77-138">Create a rule.</span></span>
3. <span data-ttu-id="abf77-139">Lägga till hello regeln toohello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="abf77-139">Add hello rule toohello load balancer.</span></span>
4. <span data-ttu-id="abf77-140">Uppdatera hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="abf77-140">Update hello load balancer.</span></span>

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

<span data-ttu-id="abf77-141">Om hello `New-AzureRmLoadBalancerRuleConfig` kommandot hello `-FrontendPort` representerar hello port hello belastningsutjämnaren visar för externa anslutningar och hello `-BackendPort` representerar hello port hello service fabric-appen lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="abf77-141">Regarding hello `New-AzureRmLoadBalancerRuleConfig` command, hello `-FrontendPort` represents hello port hello load balancer exposes for external connections, and hello `-BackendPort` represents hello port hello service fabric app is listening to.</span></span>

>[!NOTE]
><span data-ttu-id="abf77-142">Mer information om hur toocreate en belastningsutjämnare med PowerShell, se [skapar en belastningsutjämnare med PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="abf77-142">For more information on how toocreate a load balancer with PowerShell, see [Create a load balancer with PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span></span>

