---
title: "aaaCreate en Internetriktade belastningsutjämnare för Azure-molntjänster | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsutjämnare i klassiska distributionsmodellen för molntjänster"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d93cf76d417cbfc744cf07ba48c43a63cc14df69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a><span data-ttu-id="8fb48-103">Komma igång med att skapa en Internetuppkopplad belastningsutjämnare för molntjänster</span><span class="sxs-lookup"><span data-stu-id="8fb48-103">Get started creating an Internet facing load balancer for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8fb48-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="8fb48-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="8fb48-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fb48-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="8fb48-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8fb48-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="8fb48-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="8fb48-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="8fb48-108">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="8fb48-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="8fb48-109">Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="8fb48-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="8fb48-110">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8fb48-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="8fb48-111">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8fb48-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="8fb48-112">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8fb48-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

<span data-ttu-id="8fb48-113">Molntjänster konfigureras automatiskt med en belastningsutjämnare och kan anpassas via hello modell.</span><span class="sxs-lookup"><span data-stu-id="8fb48-113">Cloud services are automatically configured with a load balancer and can be customized via hello service model.</span></span>

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a><span data-ttu-id="8fb48-114">Skapa en belastningsutjämnare med hello tjänstdefinitionsfilen</span><span class="sxs-lookup"><span data-stu-id="8fb48-114">Create a load balancer using hello service definition file</span></span>

<span data-ttu-id="8fb48-115">Du kan utnyttja hello Azure SDK för .NET 2,5 tooupdate Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="8fb48-115">You can leverage hello Azure SDK for .NET 2.5 tooupdate your cloud service.</span></span> <span data-ttu-id="8fb48-116">Slutpunktsinställningar för molntjänster görs i hello [service definition](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef-filen.</span><span class="sxs-lookup"><span data-stu-id="8fb48-116">Endpoint settings for cloud services are made in hello [service definition](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef file.</span></span>

<span data-ttu-id="8fb48-117">hello följande exempel visas hur en servicedefinition.csdef-fil för en distribution konfigureras:</span><span class="sxs-lookup"><span data-stu-id="8fb48-117">hello following example shows how a servicedefinition.csdef file for a cloud deployment is configured:</span></span>

<span data-ttu-id="8fb48-118">Kontrollerar hello fragment för hello .csdef-filen som genererats av en molndistribution visas hello extern slutpunkt som konfigurerats toouse portar HTTP på port 10000 10001 och 10002.</span><span class="sxs-lookup"><span data-stu-id="8fb48-118">Checking hello snippet for hello .csdef file generated by a cloud deployment, you can see hello external endpoint configured toouse ports HTTP on port 10000, 10001, and 10002.</span></span>

```xml
<ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
<Endpoints>
    <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
    <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
    <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

    <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

    <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
        <AllocatePublicPortFrom>
            <FixedPortRange min=“10110” max=“10120“  />
        </AllocatePublicPortFrom>
    </InstanceInputEndpoint>
    <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
</Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a><span data-ttu-id="8fb48-119">Kontrollera hälsostatusen för belastningsutjämnaren för molntjänster</span><span class="sxs-lookup"><span data-stu-id="8fb48-119">Check load balancer health status for cloud services</span></span>

<span data-ttu-id="8fb48-120">hello följande är ett exempel på en hälsoavsökningen:</span><span class="sxs-lookup"><span data-stu-id="8fb48-120">hello following is an example of a health probe:</span></span>

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

<span data-ttu-id="8fb48-121">hello belastningsutjämnaren kombinerar hello information hello slutpunkt och hello av hello avsökningen toocreate en URL i formatet hello `http://{DIP of VM}:80/Probe.aspx` som kan vara används tooquery hello hälsotillståndet för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8fb48-121">hello load balancer combines hello information of hello endpoint and hello information of hello probe toocreate a URL in hello form of `http://{DIP of VM}:80/Probe.aspx` that can be used tooquery hello health of hello service.</span></span>

<span data-ttu-id="8fb48-122">hello-tjänsten identifierar regelbundna avsökningar från hello samma IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8fb48-122">hello service detects periodic probes from hello same IP address.</span></span> <span data-ttu-id="8fb48-123">Detta är hello hälsa avsökningen begäran kommer från hello värden för hello nod där hello virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="8fb48-123">This is hello health probe request coming from hello host of hello node where hello virtual machine is running.</span></span> <span data-ttu-id="8fb48-124">hello-tjänsten har toorespond med statuskod 200 HTTP för hello belastningen belastningsutjämnaren tooassume att hello-tjänsten är felfri.</span><span class="sxs-lookup"><span data-stu-id="8fb48-124">hello service has toorespond with a HTTP 200 status code for hello load balancer tooassume that hello service is healthy.</span></span> <span data-ttu-id="8fb48-125">HTTP-status kod (till exempel 503) direkt tar hello virtuella datorn från rotation.</span><span class="sxs-lookup"><span data-stu-id="8fb48-125">Any other HTTP status code (for example 503) directly takes hello virtual machine out of rotation.</span></span>

<span data-ttu-id="8fb48-126">hello avsökningen definition kontrollerar även hello frekvensen av hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="8fb48-126">hello probe definition also controls hello frequency of hello probe.</span></span> <span data-ttu-id="8fb48-127">I vårt fall hello belastningsutjämnaren avsökning hello endpoint var 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="8fb48-127">In our case above, hello load balancer is probing hello endpoint every 5 secs.</span></span> <span data-ttu-id="8fb48-128">Om inget positivt svar har tagits emot för 10 sekunder (två avsökningen intervall), hello avsökningen antas ned och hello virtuell dator tas bort från rotationen.</span><span class="sxs-lookup"><span data-stu-id="8fb48-128">If no positive answer is received for 10 secs (two probe intervals), hello probe is assumed down, and hello virtual machine is taken out of rotation.</span></span> <span data-ttu-id="8fb48-129">På liknande sätt, om hello tjänsten ligger utanför rotation och ett positivt svar tas emot, hello service återgår toorotation direkt.</span><span class="sxs-lookup"><span data-stu-id="8fb48-129">Similarly, if hello service is out of rotation and a positive answer is received, hello service is put back toorotation right away.</span></span> <span data-ttu-id="8fb48-130">Om hello-tjänsten fluktuerar mellan felfritt och feltillstånd, bestämma hello belastningsutjämnaren toodelay hello ny införandet av hello service tillbaka toorotation tills det är felfri för ett antal avsökningar.</span><span class="sxs-lookup"><span data-stu-id="8fb48-130">If hello service is fluctuating between healthy and unhealthy, hello load balancer can decide toodelay hello re-introduction of hello service back toorotation until it has been healthy for a number of probes.</span></span>

<span data-ttu-id="8fb48-131">Kontrollera hello service definition schemat för hello [hälsoavsökningen](https://msdn.microsoft.com/library/azure/jj151530.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8fb48-131">Check hello service definition schema for hello [health probe](https://msdn.microsoft.com/library/azure/jj151530.aspx) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8fb48-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fb48-132">Next steps</span></span>

[<span data-ttu-id="8fb48-133">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8fb48-133">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="8fb48-134">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8fb48-134">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="8fb48-135">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8fb48-135">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

