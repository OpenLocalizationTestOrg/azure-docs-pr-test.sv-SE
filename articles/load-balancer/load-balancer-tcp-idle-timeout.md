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
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurera inställningar för TCP-tidsgränsen för inaktivitet för Azure belastningsutjämnare

I standardkonfigurationen har Azure belastningsutjämnare en timeout för inaktivitet inställning för 4 minuter. Om en period av inaktivitet är längre än hello timeout-värde, det är inte säkert som hello TCP eller HTTP-sessionen upprätthålls mellan hello-klienten och Molntjänsten.

När hello anslutningen är stängd klientprogrammet får hello följande felmeddelande ”: hello underliggande anslutningen stängdes: en anslutning som förväntades toobe hållas aktiv stängdes av hello-servern”.

Ett vanligt är toouse ett keep-alive TCP. Den här övningen behåller hello anslutning active för en längre period. Mer information finns i dessa [.NET exempel](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Med keep-alive aktiverat, paket skickas under perioder av inaktivitet på hello-anslutning. Dessa keep alive-paket Kontrollera att hello värde för timeout för inaktivitet nåddes och hello anslutning upprätthålls under lång tid.

Den här inställningen fungerar för inkommande anslutningar. tooavoid att förlora hello-anslutning, måste du konfigurera hello TCP keep-alive med ett intervall som är mindre än hello timeout vid inaktivitet inställningen eller Höj hello timeout vid inaktivitet värdet. toosupport sådana scenarier vi har lagt till stöd för en konfigurerbar timeout för inaktivitet. Du kan nu ange en varaktighet på 4 too30 minuter.

TCP keep-alive fungerar bra för scenarier där batterilivslängd inte är en begränsning. Det rekommenderas inte för mobila program. Med hjälp av ett keep-alive i ett mobilt program TCP tömma kan hello enheten batteri snabbare.

![Tidsgränsen för TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

hello följande avsnitt beskrivs hur toochange inaktiv timeout-inställningar på virtuella datorer och molntjänster.

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a>Konfigurera hello TCP-tidsgränsen för din instansnivå offentliga IP-too15 minuter

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

`IdleTimeoutInMinutes` är valfritt. Om det inte har angetts är hello standardvärdet för timeout 4 minuter. hello godkända timeout-intervallet är 4 too30 minuter.

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Ange tidsgränsen för inaktivitet hello när du skapar en Azure slutpunkt på en virtuell dator

toochange Hej tidsgränsen för en slutpunkt, använder hello följande:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

tooretrieve timeout vid inaktivitet konfigurationen, Använd hello följande kommando:

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

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Ange hello TCP-tidsgränsen på en belastningsutjämnad slutpunktsuppsättning

Om slutpunkter är en del av en belastningsutjämnad slutpunktsuppsättning, måste hello TCP-timeout anges på hello belastningsutjämnad slutpunktsuppsättning. Exempel:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Ändra timeout-inställningar för molntjänster

Du kan använda hello Azure SDK tooupdate Molntjänsten. Du gör slutpunktsinställningar för för molntjänster i hello .csdef-filen. Uppdatering av hello TCP-tidsgränsen för distribution av en molnbaserad tjänst kräver en uppgradering av distributionen. Ett undantag är om hello TCP-timeout anges endast för en offentlig IP-adress. Offentliga IP-inställningar finns i hello .cscfg-filen och du kan uppdatera dem via uppdatering av programdistribution och uppgradering.

Hej .csdef ändringar av slutpunktsinställningar är:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

Hej .cscfg ändringar för hello timeout-inställningen på offentliga IP-adresser är:

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

## <a name="rest-api-example"></a>REST API-exempel

Du kan konfigurera timeout för inaktivitet hello TCP med hello service management API. Kontrollera att hello `x-ms-version` -huvud är angivet tooversion `2014-06-01` eller senare. Uppdatera konfigurationen av angivna hello belastningsutjämnade hello inkommande slutpunkter på alla virtuella datorer i en distribution.

### <a name="request"></a>Förfrågan

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Svar

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

## <a name="next-steps"></a>Nästa steg

[Översikt över interna belastningsutjämnare](load-balancer-internal-overview.md)

[Kom igång med att konfigurera en Internetriktade belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)
