---
title: "Konfigurera belastningsutjämning distribution läge | Microsoft Docs"
description: "Så här konfigurerar du Azure belastningsutjämnare belastningsdistributionsläget stöd för mappning mellan käll-IP"
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
ms.openlocfilehash: 4cb000c8ee1bb2e267dc0813dab23a77a46080ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-distribution-mode-for-load-balancer"></a><span data-ttu-id="074d2-103">Konfigurera distributionsplatsen läge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-103">Configure the distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="074d2-104">Hash-baserad distribution läge</span><span class="sxs-lookup"><span data-stu-id="074d2-104">Hash-based distribution mode</span></span>

<span data-ttu-id="074d2-105">Standard-distribution-algoritmen är en 5-tuppel (käll-IP, källport, mål-IP, målport protokolltyp) hash för att mappa trafik till tillgängliga servrar.</span><span class="sxs-lookup"><span data-stu-id="074d2-105">The default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash to map traffic to available servers.</span></span> <span data-ttu-id="074d2-106">Det ger varaktighet endast inom en transportsession.</span><span class="sxs-lookup"><span data-stu-id="074d2-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="074d2-107">Paket i samma session dirigeras till samma datacenter IP-Adressen (DIP)-instans bakom Utjämning av nätverksbelastning slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="074d2-107">Packets in the same session will be directed to the same datacenter IP (DIP) instance behind the load balanced endpoint.</span></span> <span data-ttu-id="074d2-108">När klienten startar en ny session från samma käll-IP, källport ändras och gör att trafik att gå till en annan DIP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="074d2-108">When the client starts a new session from the same source IP, the source port changes and causes the traffic to go to a different DIP endpoint.</span></span>

![Hash-baserad belastningsutjämnare](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="074d2-110">Bild 1-5-tuppel-distribution</span><span class="sxs-lookup"><span data-stu-id="074d2-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="074d2-111">Tillhörighet för käll-IP</span><span class="sxs-lookup"><span data-stu-id="074d2-111">Source IP affinity mode</span></span>

<span data-ttu-id="074d2-112">Vi har en annan distribution läge kallas källa IP-tillhörighet (även kallat session tillhörighet eller klienttillhörighet IP).</span><span class="sxs-lookup"><span data-stu-id="074d2-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="074d2-113">Azure belastningsutjämnare kan konfigureras för att använda en 2-tuppel (käll-IP, mål-IP) eller 3-tuppel (käll-IP, mål-IP-protokollet) för att mappa trafik till de tillgängliga servrarna.</span><span class="sxs-lookup"><span data-stu-id="074d2-113">Azure Load Balancer can be configured to use a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) to map traffic to the available servers.</span></span> <span data-ttu-id="074d2-114">Anslutningar som initieras från samma klientdator går till samma DIP-slutpunkten med hjälp av mappning mellan käll-IP.</span><span class="sxs-lookup"><span data-stu-id="074d2-114">By using Source IP affinity, connections initiated from the same client computer goes to the same DIP endpoint.</span></span>

<span data-ttu-id="074d2-115">Följande diagram illustrerar en konfiguration med 2-tuppel.</span><span class="sxs-lookup"><span data-stu-id="074d2-115">The following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="074d2-116">Observera hur 2-tuppel körs via belastningsutjämnaren till den virtuella datorn 1 (VM1) som säkerhetskopieras av VM2 och VM3.</span><span class="sxs-lookup"><span data-stu-id="074d2-116">Notice how the 2-tuple runs through the load balancer to virtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![sessionen tillhörighet](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="074d2-118">Bild 2-2-tuppel-distribution</span><span class="sxs-lookup"><span data-stu-id="074d2-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="074d2-119">Mappning mellan käll-IP-löser inkompatibilitet mellan Azure belastningsutjämnare och Gateway för fjärrskrivbord (RD).</span><span class="sxs-lookup"><span data-stu-id="074d2-119">Source IP affinity solves an incompatibility between the Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="074d2-120">Du kan nu skapa en RD gateway-servergrupp i en enda molntjänst.</span><span class="sxs-lookup"><span data-stu-id="074d2-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="074d2-121">En annan Användarscenario är media överföringen där dataöverföringen sker via UDP, men kontrollplan uppnås via TCP:</span><span class="sxs-lookup"><span data-stu-id="074d2-121">Another use case scenario is media upload where the data upload happens through UDP but the control plane is achieved through TCP:</span></span>

* <span data-ttu-id="074d2-122">En klient upprättar en TCP-session till den offentliga adressen belastningsutjämnade först, hämtar dirigeras till en specifik DIP den här kanalen är fortfarande aktiv för att övervaka hälsotillståndet för anslutningen</span><span class="sxs-lookup"><span data-stu-id="074d2-122">A client first initiates a TCP session to the load balanced public address, gets directed to a specific DIP, this channel is left active to monitor the connection health</span></span>
* <span data-ttu-id="074d2-123">En ny UDP-session från samma klientdator initieras till samma offentliga belastningsutjämnade slutpunkten, förväntningen här är att den här anslutningen också dirigeras till samma DIP-slutpunkt som den tidigare TCP-anslutningen så att mediet överför kan köras på hög genomströmning samtidigt också en kontrollkanal via TCP.</span><span class="sxs-lookup"><span data-stu-id="074d2-123">A new UDP session from the same client computer is initiated to the same load balanced public endpoint, the expectation here is that this connection is also directed to the same DIP endpoint as the previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="074d2-124">Ändringar i en belastningsutjämnad uppsättning är (att ta bort eller lägga till en virtuell dator), distribution av klientbegäranden recomputed.</span><span class="sxs-lookup"><span data-stu-id="074d2-124">When a load-balanced set changes (removing or adding a virtual machine), the distribution of client requests is recomputed.</span></span> <span data-ttu-id="074d2-125">Du kan inte vara beroende nya anslutningar från befintliga klienter som slutar på samma server.</span><span class="sxs-lookup"><span data-stu-id="074d2-125">You cannot depend on new connections from existing clients ending up at the same server.</span></span> <span data-ttu-id="074d2-126">Dessutom kan använder käll-IP distribution tillhörighet orsaka en olika fördelning av trafik.</span><span class="sxs-lookup"><span data-stu-id="074d2-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="074d2-127">Klienter som kör bakom proxyservrar kan ses som en unik klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="074d2-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="074d2-128">Konfigurera mappning mellan käll-IP-inställningar för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="074d2-129">För virtuella datorer, kan du använda PowerShell för att ändra timeout-inställningar:</span><span class="sxs-lookup"><span data-stu-id="074d2-129">For virtual machines, you can use PowerShell to change timeout settings:</span></span>

<span data-ttu-id="074d2-130">Lägg till en Azure slutpunkt till en virtuell dator och ange belastningsdistributionsläget för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-130">Add an Azure endpoint to a Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="074d2-131">LoadBalancerDistribution kan anges till sourceIP för 2-tuppel (käll-IP, mål-IP) belastningsutjämning, sourceIPProtocol för belastningsutjämning för 3-tuppel (käll-IP, mål-IP-protokollet) eller inget om du vill att standardbeteendet för 5-tuppel belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="074d2-131">LoadBalancerDistribution can be set to sourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want the default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="074d2-132">Använd följande för att hämta en slutpunkt distribution läge belastningsutjämningskonfigurationen:</span><span class="sxs-lookup"><span data-stu-id="074d2-132">Use the following to retrieve an endpoint load balancer distribution mode configuration:</span></span>

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

<span data-ttu-id="074d2-133">Om det inte finns någon LoadBalancerDistribution-elementet använder Azure belastningsutjämnare standard 5-tuppel-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="074d2-133">If the LoadBalancerDistribution element is not present then the Azure Load balancer uses the default 5-tuple algorithm.</span></span>

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="074d2-134">Ange läget Distribution på en belastningsutjämnad slutpunktsuppsättning</span><span class="sxs-lookup"><span data-stu-id="074d2-134">Set the Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="074d2-135">Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste du ange läget distribution på belastningsutjämnad slutpunktsuppsättning:</span><span class="sxs-lookup"><span data-stu-id="074d2-135">If endpoints are part of a load balanced endpoint set, the distribution mode must be set on the load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a><span data-ttu-id="074d2-136">Konfiguration av cloud Service ändra läge för distribution</span><span class="sxs-lookup"><span data-stu-id="074d2-136">Cloud Service configuration to change distribution mode</span></span>

<span data-ttu-id="074d2-137">Du kan utnyttja Azure SDK för .NET 2.5 (som ska släppas i November) för att uppdatera Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="074d2-137">You can leverage the Azure SDK for .NET 2.5 (to be released in November) to update your Cloud Service.</span></span> <span data-ttu-id="074d2-138">Slutpunktsinställningar för molntjänster görs i .csdef.</span><span class="sxs-lookup"><span data-stu-id="074d2-138">Endpoint settings for Cloud Services are made in the .csdef.</span></span> <span data-ttu-id="074d2-139">En uppgradering av distribution krävs för att kunna uppdatera belastningsdistributionsläget för belastningsutjämning för en distribution för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="074d2-139">In order to update the load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="074d2-140">Här är ett exempel på .csdef ändringar av slutpunktsinställningar:</span><span class="sxs-lookup"><span data-stu-id="074d2-140">Here is an example of .csdef changes for endpoint settings:</span></span>

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

## <a name="api-example"></a><span data-ttu-id="074d2-141">API-exempel</span><span class="sxs-lookup"><span data-stu-id="074d2-141">API example</span></span>

<span data-ttu-id="074d2-142">Du kan konfigurera belastningsutjämning belastningsfördelningen med hjälp av service management API.</span><span class="sxs-lookup"><span data-stu-id="074d2-142">You can configure the load balancer distribution using the service management API.</span></span> <span data-ttu-id="074d2-143">Se till att lägga till den `x-ms-version` huvud har angetts till version `2014-09-01` eller högre.</span><span class="sxs-lookup"><span data-stu-id="074d2-143">Make sure to add the `x-ms-version` header is set to version `2014-09-01` or higher.</span></span>

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="074d2-144">Uppdatera konfigurationen av den angivna uppsättningen med Utjämning av nätverksbelastning i en distribution</span><span class="sxs-lookup"><span data-stu-id="074d2-144">Update the configuration of the specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="074d2-145">Exempel på begäran</span><span class="sxs-lookup"><span data-stu-id="074d2-145">Request example</span></span>

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

<span data-ttu-id="074d2-146">Värdet för LoadBalancerDistribution kan vara sourceIP för mappning mellan 2-tuppel, sourceIPProtocol för 3-tuppel tillhörighet eller ingen (ingen tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="074d2-146">The value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="074d2-147">dvs 5-tuppel)</span><span class="sxs-lookup"><span data-stu-id="074d2-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="074d2-148">Svar</span><span class="sxs-lookup"><span data-stu-id="074d2-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="074d2-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="074d2-149">Next Steps</span></span>

[<span data-ttu-id="074d2-150">Översikt över interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="074d2-151">Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="074d2-152">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="074d2-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
