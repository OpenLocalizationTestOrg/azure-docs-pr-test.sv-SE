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
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Komma igång med att skapa en Internetuppkopplad belastningsutjämnare för molntjänster

> [!div class="op_single_selector"]
> * [Klassisk Azure-portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser. Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln. Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

Molntjänster konfigureras automatiskt med en belastningsutjämnare och kan anpassas via hello modell.

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a>Skapa en belastningsutjämnare med hello tjänstdefinitionsfilen

Du kan utnyttja hello Azure SDK för .NET 2,5 tooupdate Molntjänsten. Slutpunktsinställningar för molntjänster görs i hello [service definition](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef-filen.

hello följande exempel visas hur en servicedefinition.csdef-fil för en distribution konfigureras:

Kontrollerar hello fragment för hello .csdef-filen som genererats av en molndistribution visas hello extern slutpunkt som konfigurerats toouse portar HTTP på port 10000 10001 och 10002.

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

## <a name="check-load-balancer-health-status-for-cloud-services"></a>Kontrollera hälsostatusen för belastningsutjämnaren för molntjänster

hello följande är ett exempel på en hälsoavsökningen:

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

hello belastningsutjämnaren kombinerar hello information hello slutpunkt och hello av hello avsökningen toocreate en URL i formatet hello `http://{DIP of VM}:80/Probe.aspx` som kan vara används tooquery hello hälsotillståndet för hello-tjänsten.

hello-tjänsten identifierar regelbundna avsökningar från hello samma IP-adress. Detta är hello hälsa avsökningen begäran kommer från hello värden för hello nod där hello virtuella datorn körs. hello-tjänsten har toorespond med statuskod 200 HTTP för hello belastningen belastningsutjämnaren tooassume att hello-tjänsten är felfri. HTTP-status kod (till exempel 503) direkt tar hello virtuella datorn från rotation.

hello avsökningen definition kontrollerar även hello frekvensen av hello avsökning. I vårt fall hello belastningsutjämnaren avsökning hello endpoint var 5 sekunder. Om inget positivt svar har tagits emot för 10 sekunder (två avsökningen intervall), hello avsökningen antas ned och hello virtuell dator tas bort från rotationen. På liknande sätt, om hello tjänsten ligger utanför rotation och ett positivt svar tas emot, hello service återgår toorotation direkt. Om hello-tjänsten fluktuerar mellan felfritt och feltillstånd, bestämma hello belastningsutjämnaren toodelay hello ny införandet av hello service tillbaka toorotation tills det är felfri för ett antal avsökningar.

Kontrollera hello service definition schemat för hello [hälsoavsökningen](https://msdn.microsoft.com/library/azure/jj151530.aspx) för mer information.

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

