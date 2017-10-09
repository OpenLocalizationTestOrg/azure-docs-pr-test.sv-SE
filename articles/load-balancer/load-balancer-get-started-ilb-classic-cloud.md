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
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Komma igång med att skapa en intern belastningsutjämnare (klassisk) för molntjänster

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Molntjänster](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).

## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurera en intern belastningsutjämnare för molntjänster

Interna belastningsutjämnare stöds för både virtuella datorer och molntjänster. En intern belastningsutjämnare slutpunkt som skapats i en molnbaserad tjänst som ligger utanför ett regionalt virtuellt nätverk ska vara tillgänglig endast inom hello-Molntjänsten.

hello interna belastningsutjämningskonfigurationen har toobe ange under hello skapandet av hello första distributionen i hello Molntjänsten, som visas i hello exemplet nedan.

> [!IMPORTANT]
> En nödvändig toorun hello stegen nedan är toohave ett virtuellt nätverk som redan har skapats för distribution av hello moln. Du behöver hello virtuellt nätverk namn och undernät namn toocreate hello intern belastningsutjämning.

### <a name="step-1"></a>Steg 1

Öppna hello tjänstekonfigurationsfilen (.cscfg) för din molndistribution i Visual Studio och Lägg till följande avsnitt toocreate hello intern belastningsutjämning under hello senast hello ”`</Role>`” hello nätverkskonfigurationen-objekt.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Lägg till hello värden för hello network configuration file tooshow hur den ser ut. I exemplet hello förutsätter att du har skapat ett virtuellt nätverk kallas ”test_vnet” med ett undernät 10.0.0.0/24 kallas test_subnet och en statisk IP-adress 10.0.0.4. hello belastningsutjämnaren namnges testLB.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Mer information om hello belastningen belastningsutjämnaren schemat finns [Lägg till belastningsutjämnare](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Steg 2

Ändra hello service definition (.csdef) filen tooadd slutpunkter toohello intern belastningsutjämning. hello nu skapa en rollinstans av, hello tjänstdefinitionsfilen läggs hello rollen instanser toohello intern belastningsutjämning.

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

Följande hello samma värden från hello-exemplet ovan, Lägg till hello värden toohello tjänstdefinitionsfilen.

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

hello nätverkstrafik blir belastningsutjämnas med hjälp av hello testLB belastningsutjämnare använder port 80 för inkommande begäranden, skickar tooworker rollinstanser även på port 80.

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

