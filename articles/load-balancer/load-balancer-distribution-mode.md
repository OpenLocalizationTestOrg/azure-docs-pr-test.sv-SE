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
# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfigurera distributionsplatsen läge för belastningsutjämnare

## <a name="hash-based-distribution-mode"></a>Hash-baserad distribution läge

Standard-distribution-algoritmen är en 5-tuppel (käll-IP, källport, mål-IP, målport protokolltyp) hash för att mappa trafik till tillgängliga servrar. Det ger varaktighet endast inom en transportsession. Paket i samma session dirigeras till samma datacenter IP-Adressen (DIP)-instans bakom Utjämning av nätverksbelastning slutpunkten. När klienten startar en ny session från samma käll-IP, källport ändras och gör att trafik att gå till en annan DIP-slutpunkt.

![Hash-baserad belastningsutjämnare](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Bild 1-5-tuppel-distribution

## <a name="source-ip-affinity-mode"></a>Tillhörighet för käll-IP

Vi har en annan distribution läge kallas källa IP-tillhörighet (även kallat session tillhörighet eller klienttillhörighet IP). Azure belastningsutjämnare kan konfigureras för att använda en 2-tuppel (käll-IP, mål-IP) eller 3-tuppel (käll-IP, mål-IP-protokollet) för att mappa trafik till de tillgängliga servrarna. Anslutningar som initieras från samma klientdator går till samma DIP-slutpunkten med hjälp av mappning mellan käll-IP.

Följande diagram illustrerar en konfiguration med 2-tuppel. Observera hur 2-tuppel körs via belastningsutjämnaren till den virtuella datorn 1 (VM1) som säkerhetskopieras av VM2 och VM3.

![sessionen tillhörighet](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Bild 2-2-tuppel-distribution

Mappning mellan käll-IP-löser inkompatibilitet mellan Azure belastningsutjämnare och Gateway för fjärrskrivbord (RD). Du kan nu skapa en RD gateway-servergrupp i en enda molntjänst.

En annan Användarscenario är media överföringen där dataöverföringen sker via UDP, men kontrollplan uppnås via TCP:

* En klient upprättar en TCP-session till den offentliga adressen belastningsutjämnade först, hämtar dirigeras till en specifik DIP den här kanalen är fortfarande aktiv för att övervaka hälsotillståndet för anslutningen
* En ny UDP-session från samma klientdator initieras till samma offentliga belastningsutjämnade slutpunkten, förväntningen här är att den här anslutningen också dirigeras till samma DIP-slutpunkt som den tidigare TCP-anslutningen så att mediet överför kan köras på hög genomströmning samtidigt också en kontrollkanal via TCP.

> [!NOTE]
> Ändringar i en belastningsutjämnad uppsättning är (att ta bort eller lägga till en virtuell dator), distribution av klientbegäranden recomputed. Du kan inte vara beroende nya anslutningar från befintliga klienter som slutar på samma server. Dessutom kan använder käll-IP distribution tillhörighet orsaka en olika fördelning av trafik. Klienter som kör bakom proxyservrar kan ses som en unik klientprogrammet.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurera mappning mellan käll-IP-inställningar för belastningsutjämnare

För virtuella datorer, kan du använda PowerShell för att ändra timeout-inställningar:

Lägg till en Azure slutpunkt till en virtuell dator och ange belastningsdistributionsläget för belastningsutjämnare

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

LoadBalancerDistribution kan anges till sourceIP för 2-tuppel (käll-IP, mål-IP) belastningsutjämning, sourceIPProtocol för belastningsutjämning för 3-tuppel (käll-IP, mål-IP-protokollet) eller inget om du vill att standardbeteendet för 5-tuppel belastningsutjämning.

Använd följande för att hämta en slutpunkt distribution läge belastningsutjämningskonfigurationen:

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

Om det inte finns någon LoadBalancerDistribution-elementet använder Azure belastningsutjämnare standard 5-tuppel-algoritmen.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Ange läget Distribution på en belastningsutjämnad slutpunktsuppsättning

Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste du ange läget distribution på belastningsutjämnad slutpunktsuppsättning:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Konfiguration av cloud Service ändra läge för distribution

Du kan utnyttja Azure SDK för .NET 2.5 (som ska släppas i November) för att uppdatera Molntjänsten. Slutpunktsinställningar för molntjänster görs i .csdef. En uppgradering av distribution krävs för att kunna uppdatera belastningsdistributionsläget för belastningsutjämning för en distribution för molntjänster.
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

Du kan konfigurera belastningsutjämning belastningsfördelningen med hjälp av service management API. Se till att lägga till den `x-ms-version` huvud har angetts till version `2014-09-01` eller högre.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Uppdatera konfigurationen av den angivna uppsättningen med Utjämning av nätverksbelastning i en distribution

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

Värdet för LoadBalancerDistribution kan vara sourceIP för mappning mellan 2-tuppel, sourceIPProtocol för 3-tuppel tillhörighet eller ingen (ingen tillhörighet. dvs 5-tuppel)

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
