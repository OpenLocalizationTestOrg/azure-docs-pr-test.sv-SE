---
title: "Tidsgränsen för inaktivitet aaaConfigure Load Balancer TCP | Microsoft Docs"
description: "Konfigurera timeout för inaktivitet Load Balancer TCP"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 4625c6a8-5725-47ce-81db-4fa3bd055891
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2bf0704b891f708e0a5bd7aa827441930f51cfaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a><span data-ttu-id="cd551-103">Konfigurera inställningar för TCP-tidsgränsen för inaktivitet för Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="cd551-103">Configure TCP idle timeout settings for Azure Load Balancer</span></span>

<span data-ttu-id="cd551-104">I standardkonfigurationen har Azure belastningsutjämnare en timeout för inaktivitet inställning för 4 minuter.</span><span class="sxs-lookup"><span data-stu-id="cd551-104">In its default configuration, Azure Load Balancer has an idle timeout setting of 4 minutes.</span></span> <span data-ttu-id="cd551-105">Om en period av inaktivitet är längre än hello timeout-värde, det är inte säkert som hello TCP eller HTTP-sessionen upprätthålls mellan hello-klienten och Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="cd551-105">If a period of inactivity is longer than hello timeout value, there's no guarantee that hello TCP or HTTP session is maintained between hello client and your cloud service.</span></span>

<span data-ttu-id="cd551-106">När hello anslutningen är stängd klientprogrammet får hello följande felmeddelande ”: hello underliggande anslutningen stängdes: en anslutning som förväntades toobe hållas aktiv stängdes av hello-servern”.</span><span class="sxs-lookup"><span data-stu-id="cd551-106">When hello connection is closed, your client application may receive hello following error message: "hello underlying connection was closed: A connection that was expected toobe kept alive was closed by hello server."</span></span>

<span data-ttu-id="cd551-107">Ett vanligt är toouse ett keep-alive TCP.</span><span class="sxs-lookup"><span data-stu-id="cd551-107">A common practice is toouse a TCP keep-alive.</span></span> <span data-ttu-id="cd551-108">Den här övningen behåller hello anslutning active för en längre period.</span><span class="sxs-lookup"><span data-stu-id="cd551-108">This practice keeps hello connection active for a longer period.</span></span> <span data-ttu-id="cd551-109">Mer information finns i dessa [.NET exempel](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd551-109">For more information, see these [.NET examples](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span></span> <span data-ttu-id="cd551-110">Med keep-alive aktiverat, paket skickas under perioder av inaktivitet på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="cd551-110">With keep-alive enabled, packets are sent during periods of inactivity on hello connection.</span></span> <span data-ttu-id="cd551-111">Dessa keep alive-paket Kontrollera att hello värde för timeout för inaktivitet nåddes och hello anslutning upprätthålls under lång tid.</span><span class="sxs-lookup"><span data-stu-id="cd551-111">These keep-alive packets ensure that hello idle timeout value is never reached and hello connection is maintained for a long period.</span></span>

<span data-ttu-id="cd551-112">Den här inställningen fungerar för inkommande anslutningar.</span><span class="sxs-lookup"><span data-stu-id="cd551-112">This setting works for inbound connections only.</span></span> <span data-ttu-id="cd551-113">tooavoid att förlora hello-anslutning, måste du konfigurera hello TCP keep-alive med ett intervall som är mindre än hello timeout vid inaktivitet inställningen eller Höj hello timeout vid inaktivitet värdet.</span><span class="sxs-lookup"><span data-stu-id="cd551-113">tooavoid losing hello connection, you must configure hello TCP keep-alive with an interval less than hello idle timeout setting or increase hello idle timeout value.</span></span> <span data-ttu-id="cd551-114">toosupport sådana scenarier vi har lagt till stöd för en konfigurerbar timeout för inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="cd551-114">toosupport such scenarios, we've added support for a configurable idle timeout.</span></span> <span data-ttu-id="cd551-115">Du kan nu ange en varaktighet på 4 too30 minuter.</span><span class="sxs-lookup"><span data-stu-id="cd551-115">You can now set it for a duration of 4 too30 minutes.</span></span>

<span data-ttu-id="cd551-116">TCP keep-alive fungerar bra för scenarier där batterilivslängd inte är en begränsning.</span><span class="sxs-lookup"><span data-stu-id="cd551-116">TCP keep-alive works well for scenarios where battery life is not a constraint.</span></span> <span data-ttu-id="cd551-117">Det rekommenderas inte för mobila program.</span><span class="sxs-lookup"><span data-stu-id="cd551-117">It is not recommended for mobile applications.</span></span> <span data-ttu-id="cd551-118">Med hjälp av ett keep-alive i ett mobilt program TCP tömma kan hello enheten batteri snabbare.</span><span class="sxs-lookup"><span data-stu-id="cd551-118">Using a TCP keep-alive in a mobile application can drain hello device battery faster.</span></span>

![Tidsgränsen för TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

<span data-ttu-id="cd551-120">hello följande avsnitt beskrivs hur toochange inaktiv timeout-inställningar på virtuella datorer och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="cd551-120">hello following sections describe how toochange idle timeout settings in virtual machines and cloud services.</span></span>

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a><span data-ttu-id="cd551-121">Konfigurera hello TCP-tidsgränsen för din instansnivå offentliga IP-too15 minuter</span><span class="sxs-lookup"><span data-stu-id="cd551-121">Configure hello TCP timeout for your instance-level public IP too15 minutes</span></span>

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

<span data-ttu-id="cd551-122">`IdleTimeoutInMinutes` är valfritt.</span><span class="sxs-lookup"><span data-stu-id="cd551-122">`IdleTimeoutInMinutes` is optional.</span></span> <span data-ttu-id="cd551-123">Om det inte har angetts är hello standardvärdet för timeout 4 minuter.</span><span class="sxs-lookup"><span data-stu-id="cd551-123">If it is not set, hello default timeout is 4 minutes.</span></span> <span data-ttu-id="cd551-124">hello godkända timeout-intervallet är 4 too30 minuter.</span><span class="sxs-lookup"><span data-stu-id="cd551-124">hello acceptable timeout range is 4 too30 minutes.</span></span>

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a><span data-ttu-id="cd551-125">Ange tidsgränsen för inaktivitet hello när du skapar en Azure slutpunkt på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="cd551-125">Set hello idle timeout when creating an Azure endpoint on a virtual machine</span></span>

<span data-ttu-id="cd551-126">toochange Hej tidsgränsen för en slutpunkt, använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="cd551-126">toochange hello timeout setting for an endpoint, use hello following:</span></span>

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

<span data-ttu-id="cd551-127">tooretrieve timeout vid inaktivitet konfigurationen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd551-127">tooretrieve your idle timeout configuration, use hello following command:</span></span>

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="cd551-128">Ange hello TCP-tidsgränsen på en belastningsutjämnad slutpunktsuppsättning</span><span class="sxs-lookup"><span data-stu-id="cd551-128">Set hello TCP timeout on a load-balanced endpoint set</span></span>

<span data-ttu-id="cd551-129">Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste hello TCP-timeout anges på hello belastningsutjämnad slutpunktsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="cd551-129">If endpoints are part of a load-balanced endpoint set, hello TCP timeout must be set on hello load-balanced endpoint set.</span></span> <span data-ttu-id="cd551-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cd551-130">For example:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a><span data-ttu-id="cd551-131">Ändra timeout-inställningar för molntjänster</span><span class="sxs-lookup"><span data-stu-id="cd551-131">Change timeout settings for cloud services</span></span>

<span data-ttu-id="cd551-132">Du kan använda hello Azure SDK tooupdate Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="cd551-132">You can use hello Azure SDK tooupdate your cloud service.</span></span> <span data-ttu-id="cd551-133">Du gör slutpunktsinställningar för för molntjänster i hello .csdef-filen.</span><span class="sxs-lookup"><span data-stu-id="cd551-133">You make endpoint settings for cloud services in hello .csdef file.</span></span> <span data-ttu-id="cd551-134">Uppdatering av hello TCP-tidsgränsen för distribution av en molnbaserad tjänst kräver en uppgradering av distributionen.</span><span class="sxs-lookup"><span data-stu-id="cd551-134">Updating hello TCP timeout for deployment of a cloud service requires a deployment upgrade.</span></span> <span data-ttu-id="cd551-135">Ett undantag är om hello TCP-timeout anges endast för en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="cd551-135">An exception is if hello TCP timeout is specified only for a public IP.</span></span> <span data-ttu-id="cd551-136">Offentliga IP-inställningar finns i hello .cscfg-filen och du kan uppdatera dem via uppdatering av programdistribution och uppgradering.</span><span class="sxs-lookup"><span data-stu-id="cd551-136">Public IP settings are in hello .cscfg file, and you can update them through deployment update and upgrade.</span></span>

<span data-ttu-id="cd551-137">Hej .csdef ändringar av slutpunktsinställningar är:</span><span class="sxs-lookup"><span data-stu-id="cd551-137">hello .csdef changes for endpoint settings are:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="cd551-138">Hej .cscfg ändringar för hello timeout-inställningen på offentliga IP-adresser är:</span><span class="sxs-lookup"><span data-stu-id="cd551-138">hello .cscfg changes for hello timeout setting on public IPs are:</span></span>

```xml
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
    </InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="rest-api-example"></a><span data-ttu-id="cd551-139">REST API-exempel</span><span class="sxs-lookup"><span data-stu-id="cd551-139">REST API example</span></span>

<span data-ttu-id="cd551-140">Du kan konfigurera timeout för inaktivitet hello TCP med hello service management API.</span><span class="sxs-lookup"><span data-stu-id="cd551-140">You can configure hello TCP idle timeout by using hello service management API.</span></span> <span data-ttu-id="cd551-141">Kontrollera att hello `x-ms-version` -huvud är angivet tooversion `2014-06-01` eller senare.</span><span class="sxs-lookup"><span data-stu-id="cd551-141">Make sure that hello `x-ms-version` header is set tooversion `2014-06-01` or later.</span></span> <span data-ttu-id="cd551-142">Uppdatera konfigurationen av angivna hello belastningsutjämnade hello inkommande slutpunkter på alla virtuella datorer i en distribution.</span><span class="sxs-lookup"><span data-stu-id="cd551-142">Update hello configuration of hello specified load-balanced input endpoints on all virtual machines in a deployment.</span></span>

### <a name="request"></a><span data-ttu-id="cd551-143">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="cd551-143">Request</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a><span data-ttu-id="cd551-144">Svar</span><span class="sxs-lookup"><span data-stu-id="cd551-144">Response</span></span>

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
        <Path>path-of-probe</Path>
        <Port>port-assigned-to-probe</Port>
        <Protocol>probe-protocol</Protocol>
        <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
        <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
        <Rules>
        <Rule>
            <Order>priority-of-the-rule</Order>
            <Action>permit-rule</Action>
            <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
            <Description>description-of-the-rule</Description>
        </Rule>
        </Rules>
    </EndpointACL>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

## <a name="next-steps"></a><span data-ttu-id="cd551-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd551-145">Next steps</span></span>

[<span data-ttu-id="cd551-146">Översikt över interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="cd551-146">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="cd551-147">Kom igång med att konfigurera en Internetriktade belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="cd551-147">Get started configuring an Internet-facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="cd551-148">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="cd551-148">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)
