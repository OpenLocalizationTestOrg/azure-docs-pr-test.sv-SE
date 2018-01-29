---
title: "Konfigurera Azure belastningsutjämnare distribution läge | Microsoft Docs"
description: "Hur du konfigurerar distributionsplatsen läge för Azure belastningsutjämnare som stöd för mappning mellan käll-IP."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: d04a469c04553b7d6a14df7054ad5ef795baa500
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2017
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Konfigurera distributionsplatsen-läge för Azure belastningsutjämnare

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

## <a name="hash-based-distribution-mode"></a>Hash-baserad distribution läge

Standardläget för distribution för Azure belastningsutjämnare är en 5-tuppel-hash. Tuppeln består av käll-IP, källport, mål-IP, målport och protokolltyp. Hash-algoritmen som används för att avbilda trafik till de tillgängliga servrarna och algoritmen ger varaktighet endast inom en transportsession. Paket som är i samma session dirigeras till samma datacenter IP-Adressen (DIP) instans bakom Utjämning av nätverksbelastning-slutpunkten. När klienten startar en ny session från samma käll-IP, källport ändras och gör att trafik att gå till en annan DIP-slutpunkt.

![5-tuppel-hash-baserad distribution läge](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Tillhörighet för käll-IP

Belastningsutjämnare kan också konfigureras med hjälp av källa IP-tillhörighet distribution. Det här läget för distribution kallas även session tillhörighet eller IP-klienttilldelning. Läget som använder en 2-tuppel (käll-IP och mål-IP) eller 3-tuppel (käll-IP, mål-IP och protokoll typen) hash för att avbilda trafik till de tillgängliga servrarna. Med hjälp av mappning mellan käll-IP-går anslutningar som initieras från samma klientdator du till samma DIP-slutpunkten.

Följande bild illustrerar en konfiguration med 2-tuppel. Observera hur 2-tuppel körs via belastningsutjämnaren till den virtuella datorn 1 (VM1). VM1 säkerhetskopieras av VM2 och VM3.

![2-tuppel sessionsläge tillhörighet distribution](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Käll-IP-tillhörighet löser inkompatibilitet mellan Azure belastningsutjämnare och fjärrskrivbordsgateway (RD Gateway). Med det här läget kan skapa du en RD Gateway-servergrupp i en enda molntjänst.

En annan Användarscenario är media överföringen. Överföra data sker via UDP, men kontrollplan uppnås via TCP:

* En klient upprättar en TCP-session till den offentliga adressen för Utjämning av nätverksbelastning och omdirigeras till en specifik DIP. Kanalen är fortfarande aktiv för att övervaka hälsotillståndet för anslutningen.
* En ny UDP-session från samma klientdator initieras till samma belastningsutjämnade offentlig slutpunkt. Anslutningen omdirigeras till samma DIP-slutpunkt som den tidigare TCP-anslutningen. Uppladdningen av medier kan köras på hög genomströmning samtidigt som en kontrollkanal via TCP.

> [!NOTE]
> När en belastningsutjämnad uppsättning ändras genom att ta bort eller lägga till en virtuell dator, recomputed distribution av klientbegäranden. Du kan inte vara beroende nya anslutningar från befintliga klienter att hamna på samma server. Dessutom använder käll-IP distribution tillhörighet kan det orsaka en olika fördelning av trafik. Klienter som kör bakom proxyservrar kan ses som en unik klientprogrammet.

## <a name="configure-source-ip-affinity-settings"></a>Konfigurera inställningar för datakälla IP-tillhörighet

För virtuella datorer genom för att ändra timeout-inställningar med hjälp av Azure PowerShell. Lägga till en Azure slutpunkt till en virtuell dator och konfigurera belastningsdistributionsläget för belastningsfördelning:

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

Ange värdet för den `LoadBalancerDistribution` element för in önskad mängd belastningsutjämning. Ange sourceIP för belastningsutjämning för 2-tuppel (käll-IP och mål-IP). Ange sourceIPProtocol för 3-tuppel (käll-IP, mål-IP och protokoll typen) belastningsutjämning. Ange ingen för standardbeteendet för 5-tuppel belastningsutjämning.

Hämta en slutpunkt distribution läge konfiguration av belastningsutjämning med hjälp av dessa inställningar:

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

När den `LoadBalancerDistribution` elementet finns inte, Azure belastningsutjämnare använder standard-5-tuppel-algoritmen.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Konfigurera distributionsplatsen läge på belastningsutjämnad slutpunktsuppsättning

När slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste distribution-läge konfigureras på belastningsutjämnad slutpunktsuppsättning:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Konfigurera distributionsplatsen läge för slutpunkter för molntjänster

Använda Azure SDK för .NET 2.5 för att uppdatera Molntjänsten. Slutpunktsinställningar för molntjänster görs i .csdef-filen. Om du vill uppdatera belastningsdistributionsläget för belastningsutjämning för en distribution med molntjänster, krävs en uppgradering av distributionen.

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

I följande exempel visas hur du konfigurerar om belastningsdistributionsläget för belastningsutjämning för en angiven mängd med Utjämning av nätverksbelastning i en distribution. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Ändra Distributionsplatsens läge för distribuerade belastningsutjämnad uppsättning

Använd Azure klassiska distributionsmodellen om du vill ändra en befintlig distributionskonfiguration. Lägg till den `x-ms-version` sidhuvud och ange värdet till version 2014-09-01 eller senare.

#### <a name="request"></a>Förfrågan

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

Som tidigare beskrivits, ange den `LoadBalancerDistribution` elementet så att sourceIP för mappning mellan 2-tuppel, sourceIPProtocol för 3-tuppel tillhörighet eller ingen för ingen tillhörighet (5-tuppel tillhörighet).

#### <a name="response"></a>Svar

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Nästa steg

* [Översikt över Azure interna belastningsutjämnare](load-balancer-internal-overview.md)
* [Komma igång med att konfigurera en Internetriktade belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md)
* [Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
