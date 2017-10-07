---
title: aaaTroubleshoot Azure Application Gateway felaktiga Gateway (502) fel | Microsoft Docs
description: "Lär dig hur tootroubleshoot programmet Gateway 502 fel"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Felsöka Felaktig gateway-fel i Programgateway

Lär dig hur tootroubleshoot Felaktig gateway (502) fel togs emot när du använder application gateway.

## <a name="overview"></a>Översikt

När du har konfigurerat en Programgateway en hello-fel som kan användarna få är ”Serverfel: 502 - webbservern tog emot ett ogiltigt svar när den fungerade som en gateway eller proxy server”. Det här felet kan inträffa på grund av följande toohello huvudsakliga skälen:

* Azure Application Gateway [backend-adresspool har inte konfigurerats eller tom](#empty-backendaddresspool).
* Inga virtuella datorer hello eller instanser i [Skaluppsättning är felfria](#unhealthy-instances-in-backendaddresspool).
* Backend-virtuella datorer eller instanser av Skaluppsättning [svarar inte toohello standard hälsoavsökningen](#problems-with-default-health-probe.md).
* Ogiltig eller felaktig [konfigurationen av anpassade hälsoavsökningar](#problems-with-custom-health-probe.md).
* [Begär timeout eller anslutningsproblem](#request-time-out) med användarförfrågningar.

## <a name="empty-backendaddresspool"></a>Tom BackendAddressPool

### <a name="cause"></a>Orsak

Om hello Application Gateway har inga virtuella datorer eller VM Scale Set i hello backend-adresspool, det går inte att vidarebefordra alla kundbegäran och genererar en felaktig gateway-fel.

### <a name="solution"></a>Lösning

Se till att hello backend-adresspool inte är tom. Detta kan göras antingen via PowerShell, CLI eller portal.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

hello utdata från föregående cmdlet hello ska innehålla icke-tom backend-adresspool. Följande är ett exempel där två pooler returneras som har konfigurerats med FQDN eller IP-adresser för serverdel virtuella datorer. hello Etableringsstatus för hello BackendAddressPool måste vara ”lyckades”.

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>Feltillstånd instanser i BackendAddressPool

### <a name="cause"></a>Orsak

Om alla hello instanser av BackendAddressPool är ohälsosamt kan sedan Programgateway inte någon backend-tooroute användarbegäran om att. Detta kan också vara fallet hello när backend-instanser är felfria men har inte hello krävs programmet distribuerades.

### <a name="solution"></a>Lösning

Kontrollera att hello instanser är felfria och hello programmet har konfigurerats korrekt. Kontrollera om hello backend-instanserna är kan toorespond tooa ping från en annan virtuell dator i hello samma virtuella nätverk. Se till att ett webbprogram för webbläsaren begäran toohello är användbar om konfigurerats med en offentlig slutpunkt.

## <a name="problems-with-default-health-probe"></a>Problem med standard hälsoavsökningen

### <a name="cause"></a>Orsak

502 fel kan också vara ofta indikatorer som hello standard hälsoavsökningen och är inte tooreach backend-virtuella datorer. När en Programgateway instans etableras, konfigurerar den automatiskt en standard hälsa avsökningen tooeach BackendAddressPool med hjälp av egenskaper för hello BackendHttpSetting. Inga användarindata är obligatoriska tooset den här avsökningen. När en regel för belastningsutjämning konfigureras görs mer specifikt en association mellan en BackendHttpSetting och BackendAddressPool. En standard-avsökning har konfigurerats för var och en av dessa kopplingar och Programgateway initierar en regelbundna hälsokontroller Kontrollera anslutningen tooeach instans i hello BackendAddressPool på hello-porten som anges i hello BackendHttpSetting element. Följande tabell visar hello värden som är associerade med hello standard hälsoavsökningen.

| Avsökningen egenskapen | Värde | Beskrivning |
| --- | --- | --- |
| URL för webbavsökning |http://127.0.0.1/ |URL-sökväg |
| intervall |30 |Avsökningsintervall i sekunder |
| Timeout |30 |Avsökningen tidsgräns i sekunder |
| Tröskelvärde för ohälsosamt värde |3 |Avsökning för antal nya försök. hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde. |

### <a name="solution"></a>Lösning

* Se till att en standardwebbplats har konfigurerats och lyssnar på 127.0.0.1.
* Om BackendHttpSetting anger en annan port än 80 vara hello standardwebbplats konfigurerade toolisten på denna port.
* hello ska anropet toohttp://127.0.0.1:port returnera ett HTTP-Resultatkod 200. Detta ska returneras inom tidsgränsen för hello 30 sekunder.
* Se till att konfigurerade porten är öppen och att det inte finns några brandväggsregler Azure Nätverkssäkerhetsgrupperna, som blockerar inkommande eller utgående trafik på hello konfigurerade porten.
* Om klassiska virtuella Azure-datorer eller tjänst i molnet används med FQDN eller offentlig IP-adress, kontrollerar du att hello motsvarande [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) öppnas.
* Om hello VM konfigureras via Azure Resource Manager och ligger utanför hello VNet där Application Gateway har distribuerats, [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) måste konfigureras tooallow åtkomst på hello önskad port.

## <a name="problems-with-custom-health-probe"></a>Problem med anpassade hälsoavsökningen

### <a name="cause"></a>Orsak

Anpassade hälsoavsökningar kan ytterligare flexibilitet toohello standard sökning beteende. När du använder anpassade avsökningar kan användare konfigurera hello avsökningsintervall, hello URL, och sökvägen tootest och hur många misslyckade svar tooaccept innan du markerar hello backend-pool-instans som ohälsosamt. hello följande ytterligare egenskaper läggs till.

| Avsökningen egenskapen | Beskrivning |
| --- | --- |
| Namn |Namnet på hello avsökning. Det här namnet är används toorefer toohello avsökning i backend-HTTP-inställningar. |
| Protokoll |Protokoll som används toosend hello avsökning. hello avsökningen använder hello-protokollet som definieras i hello backend-HTTP-inställningar |
| Värd |Värden namnet toosend hello avsökning. Gäller endast när flera platser har konfigurerats på Application Gateway. Detta skiljer sig från den virtuella datorns värdnamn. |
| Sökväg |Relativ sökväg hello avsökning. hello giltig sökväg börjar från '/'. hello avsökningen skickas för\<protokollet\>://\<värden\>:\<port\>\<sökväg\> |
| intervall |Avsökning intervall i sekunder. Detta är hello tidsintervallet mellan två på varandra följande avsökningar. |
| Timeout |Avsökning tidsgräns i sekunder. Om ett giltigt svar inte emot inom denna tidsgräns, markeras hello avsökningen som misslyckad. |
| Tröskelvärde för ohälsosamt värde |Avsökning för antal nya försök. hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde. |

### <a name="solution"></a>Lösning

Verifiera att hello avsökning för anpassad hälsa är korrekt konfigurerad som hello föregående tabell. Dessutom toohello föregående felsökning, se också till hello följande:

* Kontrollera att hello-avsökning har angetts korrekt enligt hello [guiden](application-gateway-create-probe-ps.md).
* Om Application Gateway har konfigurerats för en viss plats, som standard hello värden ska namn anges som 127.0.0.1, såvida inte annat konfigurerade i anpassade avsökning.
* Se till att ett anrop toohttp: / /\<värden\>:\<port\>\<sökväg\> returnerar ett HTTP-Resultatkod 200.
* Kontrollera att intervallet, timeout och UnhealtyThreshold är inom acceptabel hello-intervall.
* Om du använder en HTTPS-avsökning, se till att hello backend-servern kräver SNI genom att konfigurera ett fallback-certifikatet på hello backend-servern. 
* Kontrollera att intervallet, timeout och UnhealtyThreshold är inom acceptabel hello-intervall.

## <a name="request-time-out"></a>Tidsgräns för begäran

### <a name="cause"></a>Orsak

När en begäran tas emot Programgateway gäller hello konfigurerade regler toohello begäran och dirigerar den tooa backend-adresspool instans. Det väntar på en konfigurerbar tidsperiod på svar från hello backend-instans. Det här intervallet är som standard **30 sekunder**. Om Application Gateway inte får ett svar från serverprogrammet i det här intervallet visas användarbegäran ett 502 fel.

### <a name="solution"></a>Lösning

Programgateway kan användare tooconfigure inställningen via BackendHttpSetting som kan vara och sedan använda toodifferent pooler. Olika backend-pooler kan ha olika BackendHttpSetting och därför olika begäran-timeout konfigurerats.

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a>Nästa steg

Om hello föregående stegen inte löser problemet hello kan öppna en [stöder biljett](https://azure.microsoft.com/support/options/).

