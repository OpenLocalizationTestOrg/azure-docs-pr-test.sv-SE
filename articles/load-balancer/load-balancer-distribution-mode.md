---
title: "aaaConfigure belastningsutjämnaren distribution läge | Microsoft Docs"
description: "Hur tooconfigure Azure läsa in belastningsutjämning distribution läge toosupport källa IP-tillhörighet"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: e745240b733ffc07928d8ed0ae097785ad4f412e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-distribution-mode-for-load-balancer"></a><span data-ttu-id="ce7f2-103">Konfigurera hello distribution läge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-103">Configure hello distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="ce7f2-104">Hash-baserad distribution läge</span><span class="sxs-lookup"><span data-stu-id="ce7f2-104">Hash-based distribution mode</span></span>

<span data-ttu-id="ce7f2-105">hello standard distribution algoritmen är en 5-tuppel (käll-IP, källport, mål-IP, målport protokolltyp) hash-toomap trafik tooavailable servrar.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-105">hello default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash toomap traffic tooavailable servers.</span></span> <span data-ttu-id="ce7f2-106">Det ger varaktighet endast inom en transportsession.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="ce7f2-107">Paket i hello samma session kommer att dirigeras toohello-instans samma datacenter IP (DIP) bakom hello belastningsutjämnade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-107">Packets in hello same session will be directed toohello same datacenter IP (DIP) instance behind hello load balanced endpoint.</span></span> <span data-ttu-id="ce7f2-108">När hello klienten startar en ny session från Hej samma käll-IP, hello källport ändras och gör hello trafik toogo tooa annan DIP slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-108">When hello client starts a new session from hello same source IP, hello source port changes and causes hello traffic toogo tooa different DIP endpoint.</span></span>

![Hash-baserad belastningsutjämnare](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="ce7f2-110">Bild 1-5-tuppel-distribution</span><span class="sxs-lookup"><span data-stu-id="ce7f2-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="ce7f2-111">Tillhörighet för käll-IP</span><span class="sxs-lookup"><span data-stu-id="ce7f2-111">Source IP affinity mode</span></span>

<span data-ttu-id="ce7f2-112">Vi har en annan distribution läge kallas källa IP-tillhörighet (även kallat session tillhörighet eller klienttillhörighet IP).</span><span class="sxs-lookup"><span data-stu-id="ce7f2-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="ce7f2-113">Azure belastningsutjämnare kan vara konfigurerade toouse en 2-tuppel (käll-IP, mål-IP) eller 3-tuppel (käll-IP, mål-IP-protokollet) toomap trafik toohello tillgängliga servrar.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-113">Azure Load Balancer can be configured toouse a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) toomap traffic toohello available servers.</span></span> <span data-ttu-id="ce7f2-114">Med hjälp av mappning mellan käll-IP, anslutningar initieras från hello samma klientdator går toohello samma DIP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-114">By using Source IP affinity, connections initiated from hello same client computer goes toohello same DIP endpoint.</span></span>

<span data-ttu-id="ce7f2-115">hello följande diagram illustrerar en konfiguration med 2-tuppel.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-115">hello following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="ce7f2-116">Observera hur hello 2-tuppel körs via hello belastningen belastningsutjämnaren toovirtual datorn 1 (VM1) som säkerhetskopieras av VM2 och VM3.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-116">Notice how hello 2-tuple runs through hello load balancer toovirtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![sessionen tillhörighet](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="ce7f2-118">Bild 2-2-tuppel-distribution</span><span class="sxs-lookup"><span data-stu-id="ce7f2-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="ce7f2-119">Mappning mellan käll-IP-löser inkompatibilitet mellan hello Azure belastningsutjämnare och Gateway för fjärrskrivbord (RD).</span><span class="sxs-lookup"><span data-stu-id="ce7f2-119">Source IP affinity solves an incompatibility between hello Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="ce7f2-120">Du kan nu skapa en RD gateway-servergrupp i en enda molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="ce7f2-121">En annan Användarscenario är media överföringen där hello dataöverföringen sker via UDP men hello kontrollplan uppnås via TCP:</span><span class="sxs-lookup"><span data-stu-id="ce7f2-121">Another use case scenario is media upload where hello data upload happens through UDP but hello control plane is achieved through TCP:</span></span>

* <span data-ttu-id="ce7f2-122">En klient upprättar en TCP-session toohello belastningsutjämnade offentlig adress först, hämtar dirigerad tooa specifika DIP, den här kanalen är vänstra active toomonitor hello anslutningshälsa</span><span class="sxs-lookup"><span data-stu-id="ce7f2-122">A client first initiates a TCP session toohello load balanced public address, gets directed tooa specific DIP, this channel is left active toomonitor hello connection health</span></span>
* <span data-ttu-id="ce7f2-123">En ny UDP-session från hello samma klientdatorn är initierad toohello samma belastningsutjämnade offentlig slutpunkt, hello förväntan här är att den här anslutningen är också dirigerad toohello samma DIP som hello tidigare TCP-anslutningen så att överföra media kan vara köra vid hög genomströmning samtidigt också en kontrollkanal via TCP.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-123">A new UDP session from hello same client computer is initiated toohello same load balanced public endpoint, hello expectation here is that this connection is also directed toohello same DIP endpoint as hello previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="ce7f2-124">När en belastningsutjämnad uppsättning ändras (att ta bort eller lägga till en virtuell dator), recomputed hello distribution av klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-124">When a load-balanced set changes (removing or adding a virtual machine), hello distribution of client requests is recomputed.</span></span> <span data-ttu-id="ce7f2-125">Du kan inte vara beroende nya anslutningar från befintliga klienter som slutar på hello samma server.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-125">You cannot depend on new connections from existing clients ending up at hello same server.</span></span> <span data-ttu-id="ce7f2-126">Dessutom kan använder käll-IP distribution tillhörighet orsaka en olika fördelning av trafik.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="ce7f2-127">Klienter som kör bakom proxyservrar kan ses som en unik klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="ce7f2-128">Konfigurera mappning mellan käll-IP-inställningar för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="ce7f2-129">Du kan använda PowerShell toochange timeout-inställningar för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="ce7f2-129">For virtual machines, you can use PowerShell toochange timeout settings:</span></span>

<span data-ttu-id="ce7f2-130">Lägg till en Azure endpoint tooa virtuella datorn och ange belastningsdistributionsläget för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-130">Add an Azure endpoint tooa Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="ce7f2-131">LoadBalancerDistribution kan ställas in toosourceIP för 2-tuppel (käll-IP, mål-IP) belastningsutjämning, sourceIPProtocol för belastningsutjämning för 3-tuppel (käll-IP, mål-IP-protokollet) eller inget om du vill att hello standardbeteendet för 5-tuppel belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-131">LoadBalancerDistribution can be set toosourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want hello default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="ce7f2-132">Använd hello följande tooretrieve en slutpunkt distribution läge belastningsutjämningskonfigurationen:</span><span class="sxs-lookup"><span data-stu-id="ce7f2-132">Use hello following tooretrieve an endpoint load balancer distribution mode configuration:</span></span>

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

<span data-ttu-id="ce7f2-133">Om det inte finns någon hello LoadBalancerDistribution element använder hello Azure belastningsutjämnare hello standard 5-tuppel-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-133">If hello LoadBalancerDistribution element is not present then hello Azure Load balancer uses hello default 5-tuple algorithm.</span></span>

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="ce7f2-134">Ange hello Distribution läge på en belastningsutjämnad slutpunktsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ce7f2-134">Set hello Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="ce7f2-135">Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste du ange hello distribution läge på hello belastningsutjämnad slutpunktsuppsättning:</span><span class="sxs-lookup"><span data-stu-id="ce7f2-135">If endpoints are part of a load balanced endpoint set, hello distribution mode must be set on hello load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a><span data-ttu-id="ce7f2-136">Cloud Service konfigurationsläge toochange distribution</span><span class="sxs-lookup"><span data-stu-id="ce7f2-136">Cloud Service configuration toochange distribution mode</span></span>

<span data-ttu-id="ce7f2-137">Du kan utnyttja hello Azure SDK för .NET 2.5 (toobe ut i November) tooupdate Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-137">You can leverage hello Azure SDK for .NET 2.5 (toobe released in November) tooupdate your Cloud Service.</span></span> <span data-ttu-id="ce7f2-138">Slutpunktsinställningar för molntjänster görs i hello .csdef.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-138">Endpoint settings for Cloud Services are made in hello .csdef.</span></span> <span data-ttu-id="ce7f2-139">I ordning tooupdate hello belastningsutjämnaren belastningsdistributionsläget för distribution av molntjänster krävs en uppgradering av distributionen.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-139">In order tooupdate hello load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="ce7f2-140">Här är ett exempel på .csdef ändringar av slutpunktsinställningar:</span><span class="sxs-lookup"><span data-stu-id="ce7f2-140">Here is an example of .csdef changes for endpoint settings:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
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

## <a name="api-example"></a><span data-ttu-id="ce7f2-141">API-exempel</span><span class="sxs-lookup"><span data-stu-id="ce7f2-141">API example</span></span>

<span data-ttu-id="ce7f2-142">Du kan konfigurera hello belastningsutjämnaren belastningsdistribution använder hello service management API.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-142">You can configure hello load balancer distribution using hello service management API.</span></span> <span data-ttu-id="ce7f2-143">Se till att tooadd hello `x-ms-version` -huvud är angivet tooversion `2014-09-01` eller högre.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-143">Make sure tooadd hello `x-ms-version` header is set tooversion `2014-09-01` or higher.</span></span>

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="ce7f2-144">Uppdatera hello konfigurationen av angivna hello belastningsutjämnad uppsättning i en distribution</span><span class="sxs-lookup"><span data-stu-id="ce7f2-144">Update hello configuration of hello specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="ce7f2-145">Exempel på begäran</span><span class="sxs-lookup"><span data-stu-id="ce7f2-145">Request example</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

<span data-ttu-id="ce7f2-146">hello-värdet för LoadBalancerDistribution kan vara sourceIP för mappning mellan 2-tuppel, sourceIPProtocol för 3-tuppel tillhörighet eller ingen (ingen tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="ce7f2-146">hello value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="ce7f2-147">dvs 5-tuppel)</span><span class="sxs-lookup"><span data-stu-id="ce7f2-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="ce7f2-148">Svar</span><span class="sxs-lookup"><span data-stu-id="ce7f2-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="ce7f2-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce7f2-149">Next Steps</span></span>

[<span data-ttu-id="ce7f2-150">Översikt över interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="ce7f2-151">Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="ce7f2-152">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="ce7f2-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
