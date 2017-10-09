---
title: "aaaCreate en intern belastningsutjämnare för Azure-molntjänster | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med PowerShell i hello klassiska distributionsmodellen"
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
ms.openlocfilehash: fe7975bca7bec3248626b0ad0fad6823e278ade2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a><span data-ttu-id="ac949-103">Komma igång med att skapa en intern belastningsutjämnare (klassisk) för molntjänster</span><span class="sxs-lookup"><span data-stu-id="ac949-103">Get started creating an internal load balancer (classic) for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ac949-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac949-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="ac949-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ac949-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="ac949-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="ac949-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> <span data-ttu-id="ac949-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ac949-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="ac949-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ac949-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="ac949-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ac949-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="ac949-110">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ac949-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

## <a name="configure-internal-load-balancer-for-cloud-services"></a><span data-ttu-id="ac949-111">Konfigurera en intern belastningsutjämnare för molntjänster</span><span class="sxs-lookup"><span data-stu-id="ac949-111">Configure internal load balancer for cloud services</span></span>

<span data-ttu-id="ac949-112">Interna belastningsutjämnare stöds för både virtuella datorer och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="ac949-112">Internal load balancer is supported for both virtual machines and cloud services.</span></span> <span data-ttu-id="ac949-113">En intern belastningsutjämnare slutpunkt som skapats i en molnbaserad tjänst som ligger utanför ett regionalt virtuellt nätverk ska vara tillgänglig endast inom hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac949-113">An internal load balancer endpoint created in a cloud service that is outside a regional virtual network will be accessible only within hello cloud service.</span></span>

<span data-ttu-id="ac949-114">hello interna belastningsutjämningskonfigurationen har toobe ange under hello skapandet av hello första distributionen i hello Molntjänsten, som visas i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="ac949-114">hello internal load balancer configuration has toobe set during hello creation of hello first deployment in hello cloud service, as shown in hello sample below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ac949-115">En nödvändig toorun hello stegen nedan är toohave ett virtuellt nätverk som redan har skapats för distribution av hello moln.</span><span class="sxs-lookup"><span data-stu-id="ac949-115">A prerequisite toorun hello steps below is toohave a virtual network already created for hello cloud deployment.</span></span> <span data-ttu-id="ac949-116">Du behöver hello virtuellt nätverk namn och undernät namn toocreate hello intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ac949-116">You will need hello virtual network name and subnet name toocreate hello Internal Load Balancing.</span></span>

### <a name="step-1"></a><span data-ttu-id="ac949-117">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ac949-117">Step 1</span></span>

<span data-ttu-id="ac949-118">Öppna hello tjänstekonfigurationsfilen (.cscfg) för din molndistribution i Visual Studio och Lägg till följande avsnitt toocreate hello intern belastningsutjämning under hello senast hello ”`</Role>`” hello nätverkskonfigurationen-objekt.</span><span class="sxs-lookup"><span data-stu-id="ac949-118">Open hello service configuration file (.cscfg) for your cloud deployment in Visual Studio and add hello following section toocreate hello Internal Load Balancing under hello last "`</Role>`" item for hello network configuration.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="ac949-119">Lägg till hello värden för hello network configuration file tooshow hur den ser ut.</span><span class="sxs-lookup"><span data-stu-id="ac949-119">Let's add hello values for hello network configuration file tooshow how it will look.</span></span> <span data-ttu-id="ac949-120">I exemplet hello förutsätter att du har skapat ett virtuellt nätverk kallas ”test_vnet” med ett undernät 10.0.0.0/24 kallas test_subnet och en statisk IP-adress 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="ac949-120">In hello example, assume you created a VNet called "test_vnet" with a subnet 10.0.0.0/24 called test_subnet and a static IP 10.0.0.4.</span></span> <span data-ttu-id="ac949-121">hello belastningsutjämnaren namnges testLB.</span><span class="sxs-lookup"><span data-stu-id="ac949-121">hello load balancer will be named testLB.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="ac949-122">Mer information om hello belastningen belastningsutjämnaren schemat finns [Lägg till belastningsutjämnare](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac949-122">For more information about hello load balancer schema, see [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span></span>

### <a name="step-2"></a><span data-ttu-id="ac949-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ac949-123">Step 2</span></span>

<span data-ttu-id="ac949-124">Ändra hello service definition (.csdef) filen tooadd slutpunkter toohello intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ac949-124">Change hello service definition (.csdef) file tooadd endpoints toohello Internal Load Balancing.</span></span> <span data-ttu-id="ac949-125">hello nu skapa en rollinstans av, hello tjänstdefinitionsfilen läggs hello rollen instanser toohello intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ac949-125">hello moment a role instance is created, hello service definition file will add hello role instances toohello Internal Load Balancing.</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="ac949-126">Följande hello samma värden från hello-exemplet ovan, Lägg till hello värden toohello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="ac949-126">Following hello same values from hello example above, let's add hello values toohello service definition file.</span></span>

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="ac949-127">hello nätverkstrafik blir belastningsutjämnas med hjälp av hello testLB belastningsutjämnare använder port 80 för inkommande begäranden, skickar tooworker rollinstanser även på port 80.</span><span class="sxs-lookup"><span data-stu-id="ac949-127">hello network traffic will be load balanced using hello testLB load balancer using port 80 for incoming requests, sending tooworker role instances also on port 80.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac949-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac949-128">Next steps</span></span>

[<span data-ttu-id="ac949-129">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="ac949-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="ac949-130">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ac949-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

