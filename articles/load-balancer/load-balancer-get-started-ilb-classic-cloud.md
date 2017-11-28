---
title: "Skapa en intern belastningsutjämnare för Azure Cloud Services | Microsoft Docs"
description: "Lär dig hur du skapar en intern belastningsutjämnare med hjälp av PowerShell i den klassiska distributionsmodellen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 8dbc951416d577fa7f534c2eab1605c6bee61fce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a><span data-ttu-id="64b15-103">Komma igång med att skapa en intern belastningsutjämnare (klassisk) för molntjänster</span><span class="sxs-lookup"><span data-stu-id="64b15-103">Get started creating an internal load balancer (classic) for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="64b15-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="64b15-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="64b15-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="64b15-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="64b15-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="64b15-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> <span data-ttu-id="64b15-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="64b15-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="64b15-108">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="64b15-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="64b15-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="64b15-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="64b15-110">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="64b15-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

## <a name="configure-internal-load-balancer-for-cloud-services"></a><span data-ttu-id="64b15-111">Konfigurera en intern belastningsutjämnare för molntjänster</span><span class="sxs-lookup"><span data-stu-id="64b15-111">Configure internal load balancer for cloud services</span></span>

<span data-ttu-id="64b15-112">Interna belastningsutjämnare stöds för både virtuella datorer och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="64b15-112">Internal load balancer is supported for both virtual machines and cloud services.</span></span> <span data-ttu-id="64b15-113">Slutpunkten för en intern belastningsutjämnare som skapas i en molntjänst som finns utanför ett regionalt virtuellt nätverk kan endast nås i molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="64b15-113">An internal load balancer endpoint created in a cloud service that is outside a regional virtual network will be accessible only within the cloud service.</span></span>

<span data-ttu-id="64b15-114">Du måste konfigurera interna belastningsutjämnare när du skapar den första distributionen i molntjänsten, som du ser i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="64b15-114">The internal load balancer configuration has to be set during the creation of the first deployment in the cloud service, as shown in the sample below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64b15-115">Innan du kör stegen nedan måste du ha ett virtuellt nätverk för molndistributionen.</span><span class="sxs-lookup"><span data-stu-id="64b15-115">A prerequisite to run the steps below is to have a virtual network already created for the cloud deployment.</span></span> <span data-ttu-id="64b15-116">Du behöver namnet på det virtuella nätverket och undernätet för att kunna skapa den interna belastningsutjämningen.</span><span class="sxs-lookup"><span data-stu-id="64b15-116">You will need the virtual network name and subnet name to create the Internal Load Balancing.</span></span>

### <a name="step-1"></a><span data-ttu-id="64b15-117">Steg 1</span><span class="sxs-lookup"><span data-stu-id="64b15-117">Step 1</span></span>

<span data-ttu-id="64b15-118">Öppna tjänstkonfigurationsfilen (.cscfg) för din distribution i Visual Studio och lägg till följande avsnitt för att skapa den interna belastningsutjämningen under det sista ”`</Role>`”-objektet för nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="64b15-118">Open the service configuration file (.cscfg) for your cloud deployment in Visual Studio and add the following section to create the Internal Load Balancing under the last "`</Role>`" item for the network configuration.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of the load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="64b15-119">Nu ska vi lägga till värdena för nätverkskonfigurationsfilen så att du ser hur det ser ut.</span><span class="sxs-lookup"><span data-stu-id="64b15-119">Let's add the values for the network configuration file to show how it will look.</span></span> <span data-ttu-id="64b15-120">I det här exemplet antar vi att du har skapat ett VNet med namnet ”test_vnet” och undernätet 10.0.0.0/24 med namnet test_subnet och den statiska IP-adressen 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="64b15-120">In the example, assume you created a VNet called "test_vnet" with a subnet 10.0.0.0/24 called test_subnet and a static IP 10.0.0.4.</span></span> <span data-ttu-id="64b15-121">Belastningsutjämnaren ges namnet testLB.</span><span class="sxs-lookup"><span data-stu-id="64b15-121">The load balancer will be named testLB.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="64b15-122">Mer information om belastningsutjämningsschemat finns i [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx) (Lägga till en belastningsutjämnare).</span><span class="sxs-lookup"><span data-stu-id="64b15-122">For more information about the load balancer schema, see [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span></span>

### <a name="step-2"></a><span data-ttu-id="64b15-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="64b15-123">Step 2</span></span>

<span data-ttu-id="64b15-124">Ändra tjänstdefinitionsfilen (.csdef) för att lägga till slutpunkter för den interna belastningsutjämningen.</span><span class="sxs-lookup"><span data-stu-id="64b15-124">Change the service definition (.csdef) file to add endpoints to the Internal Load Balancing.</span></span> <span data-ttu-id="64b15-125">Så fort en rollinstans skapas lägger tjänstdefinitionsfilen till rollinstansen i den interna belastningsutjämningen.</span><span class="sxs-lookup"><span data-stu-id="64b15-125">The moment a role instance is created, the service definition file will add the role instances to the Internal Load Balancing.</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="64b15-126">Vi använder värdena i exemplet ovan och lägger till dem i tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="64b15-126">Following the same values from the example above, let's add the values to the service definition file.</span></span>

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="64b15-127">Nätverkstrafiken kommer att belastningsutjämnas med belastningsutjämnaren testLB. Port 80 används för inkommande förfrågningar och samma port används för att skicka trafik till arbetsrollinstanser.</span><span class="sxs-lookup"><span data-stu-id="64b15-127">The network traffic will be load balanced using the testLB load balancer using port 80 for incoming requests, sending to worker role instances also on port 80.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64b15-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64b15-128">Next steps</span></span>

[<span data-ttu-id="64b15-129">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="64b15-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="64b15-130">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="64b15-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

