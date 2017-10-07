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
# <a name="configure-hello-distribution-mode-for-load-balancer"></a>Konfigurera hello distribution läge för belastningsutjämnare

## <a name="hash-based-distribution-mode"></a>Hash-baserad distribution läge

hello standard distribution algoritmen är en 5-tuppel (käll-IP, källport, mål-IP, målport protokolltyp) hash-toomap trafik tooavailable servrar. Det ger varaktighet endast inom en transportsession. Paket i hello samma session kommer att dirigeras toohello-instans samma datacenter IP (DIP) bakom hello belastningsutjämnade slutpunkt. När hello klienten startar en ny session från Hej samma käll-IP, hello källport ändras och gör hello trafik toogo tooa annan DIP slutpunkt.

![Hash-baserad belastningsutjämnare](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Bild 1-5-tuppel-distribution

## <a name="source-ip-affinity-mode"></a>Tillhörighet för käll-IP

Vi har en annan distribution läge kallas källa IP-tillhörighet (även kallat session tillhörighet eller klienttillhörighet IP). Azure belastningsutjämnare kan vara konfigurerade toouse en 2-tuppel (käll-IP, mål-IP) eller 3-tuppel (käll-IP, mål-IP-protokollet) toomap trafik toohello tillgängliga servrar. Med hjälp av mappning mellan käll-IP, anslutningar initieras från hello samma klientdator går toohello samma DIP-slutpunkten.

hello följande diagram illustrerar en konfiguration med 2-tuppel. Observera hur hello 2-tuppel körs via hello belastningen belastningsutjämnaren toovirtual datorn 1 (VM1) som säkerhetskopieras av VM2 och VM3.

![sessionen tillhörighet](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Bild 2-2-tuppel-distribution

Mappning mellan käll-IP-löser inkompatibilitet mellan hello Azure belastningsutjämnare och Gateway för fjärrskrivbord (RD). Du kan nu skapa en RD gateway-servergrupp i en enda molntjänst.

En annan Användarscenario är media överföringen där hello dataöverföringen sker via UDP men hello kontrollplan uppnås via TCP:

* En klient upprättar en TCP-session toohello belastningsutjämnade offentlig adress först, hämtar dirigerad tooa specifika DIP, den här kanalen är vänstra active toomonitor hello anslutningshälsa
* En ny UDP-session från hello samma klientdatorn är initierad toohello samma belastningsutjämnade offentlig slutpunkt, hello förväntan här är att den här anslutningen är också dirigerad toohello samma DIP som hello tidigare TCP-anslutningen så att överföra media kan vara köra vid hög genomströmning samtidigt också en kontrollkanal via TCP.

> [!NOTE]
> När en belastningsutjämnad uppsättning ändras (att ta bort eller lägga till en virtuell dator), recomputed hello distribution av klientbegäranden. Du kan inte vara beroende nya anslutningar från befintliga klienter som slutar på hello samma server. Dessutom kan använder käll-IP distribution tillhörighet orsaka en olika fördelning av trafik. Klienter som kör bakom proxyservrar kan ses som en unik klientprogrammet.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurera mappning mellan käll-IP-inställningar för belastningsutjämnare

Du kan använda PowerShell toochange timeout-inställningar för virtuella datorer:

Lägg till en Azure endpoint tooa virtuella datorn och ange belastningsdistributionsläget för belastningsutjämnare

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

LoadBalancerDistribution kan ställas in toosourceIP för 2-tuppel (käll-IP, mål-IP) belastningsutjämning, sourceIPProtocol för belastningsutjämning för 3-tuppel (käll-IP, mål-IP-protokollet) eller inget om du vill att hello standardbeteendet för 5-tuppel belastningsutjämning.

Använd hello följande tooretrieve en slutpunkt distribution läge belastningsutjämningskonfigurationen:

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

Om det inte finns någon hello LoadBalancerDistribution element använder hello Azure belastningsutjämnare hello standard 5-tuppel-algoritmen.

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a>Ange hello Distribution läge på en belastningsutjämnad slutpunktsuppsättning

Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste du ange hello distribution läge på hello belastningsutjämnad slutpunktsuppsättning:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a>Cloud Service konfigurationsläge toochange distribution

Du kan utnyttja hello Azure SDK för .NET 2.5 (toobe ut i November) tooupdate Molntjänsten. Slutpunktsinställningar för molntjänster görs i hello .csdef. I ordning tooupdate hello belastningsutjämnaren belastningsdistributionsläget för distribution av molntjänster krävs en uppgradering av distributionen.
Här är ett exempel på .csdef ändringar av slutpunktsinställningar:

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

## <a name="api-example"></a>API-exempel

Du kan konfigurera hello belastningsutjämnaren belastningsdistribution använder hello service management API. Se till att tooadd hello `x-ms-version` -huvud är angivet tooversion `2014-09-01` eller högre.

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a>Uppdatera hello konfigurationen av angivna hello belastningsutjämnad uppsättning i en distribution

#### <a name="request-example"></a>Exempel på begäran

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

hello-värdet för LoadBalancerDistribution kan vara sourceIP för mappning mellan 2-tuppel, sourceIPProtocol för 3-tuppel tillhörighet eller ingen (ingen tillhörighet. dvs 5-tuppel)

#### <a name="response"></a>Svar

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Nästa steg

[Översikt över interna belastningsutjämnare](load-balancer-internal-overview.md)

[Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
