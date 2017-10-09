---
title: "Återställa en Azure VPN gateway tooreestablish IPsec-tunnlar | Microsoft Docs"
description: "Den här artikeln beskriver hur du återställer Azure VPN-Gateway tooreestablish IPsec-tunnlar. hello artikeln gäller tooVPN gateways i både hello klassiska och hello Resource Manager distributionsmodellerna."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 84dd741f0bebd6b18cb235216a68a88da5fe17b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reset-a-vpn-gateway"></a>Återställ en VPN Gateway

Du kan behöva återställa en Azure VPN-gateway om VPN-anslutningen mellan flera platser i en eller flera VPN-tunnlar för plats-till-plats bryts. I så fall kan din lokala VPN-enheter är alla fungerar, men är inte tooestablish IPsec-tunnlar med hello Azure VPN-gatewayer. Den här artikeln hjälper dig att återställa din VPN-gateway.

### <a name="what-happens-during-a-reset"></a>Vad som händer under en återställning?

En VPN-gateway består av två VM-instanser som körs i en konfiguration i aktivt vänteläge. När du återställer hello gateway hello gateway startas och sedan återställer hello mellan lokala konfigurationer tooit. hello gateway behåller hello offentliga IP-adressen har redan. Det innebär att du inte behöver tooupdate hello VPN routerkonfiguration med en ny offentlig IP-adress för Azure VPN-gateway.

När du skickar hello kommandot tooreset hello gateway startas hello aktuella aktiva instansen av hello Azure VPN-gateway omedelbart. Det blir ett kort mellanrum under hello växling vid fel från hello aktiva instansen (startas), toohello vänteläge instans. hello mellanrummet bör vara mindre än en minut.

Om inte hello anslutningen återställs efter hello första omstarten problemet hello samma kommando igen tooreboot hello andra VM-instans (hello nya aktiva gateway). Om hello två omstarter begärda tillbaka tooback kan finnas det en längre period där båda VM-instanser (aktiva och vänteläge) startas. Detta innebär att ett längre uppehåll hello VPN-anslutning, in too2 too4 minuter för virtuella datorer toocomplete hello omstarter.

Efter två omstarter, om du fortfarande har problem med nätverksanslutningen mellan platser, öppnar du en supportbegäran från hello Azure-portalen.

## <a name="before"></a>Innan du börjar

Innan du återställer din gateway, kontrollera hello huvudpunkter som anges nedan för varje IPsec plats-till-plats (S2S) VPN-tunnel. Alla matchning av datatyp i hello objekt leder hello att koppla från S2S VPN-tunnlar. Kontrollera och korrigera hello konfigurationer för lokalt och Azure VPN-gatewayer behöver du inte onödiga omstarter och avbrott för hello andra fungerande anslutningar på hello gateways.

Kontrollera hello följande objekt innan du återställer din gateway:

* hello Internet IP-adresser (VIP) för både hello Azure VPN-gateway och hello lokala VPN-gateway har konfigurerats korrekt i båda hello Azure och hello lokala VPN-principer.
* hello i förväg delad nyckel måste hello samma på både Azure och lokala VPN-gatewayer.
* Om du tillämpar specifika IPsec/IKE-konfiguration, som kryptering, hash-algoritmer och PFS (Perfect Forward Secrecy), se till att både hello Azure och lokala VPN-gatewayer har hello samma konfigurationer.

## <a name="portal"></a>Azure-portalen

Du kan återställa en Resource Manager VPN-gateway med hello Azure-portalen. Om du vill tooreset klassiska gateway, se hello [PowerShell](#resetclassic) steg.

### <a name="resource-manager-deployment-model"></a>Distributionsmodell med Resource Manager

1. Öppna hello [Azure-portalen](https://portal.azure.com) och navigera toohello Resource Manager virtuell nätverksgateway som du vill tooreset.
2. Klicka på ”Återställ' hello bladet för hello virtuell nätverksgateway.

  ![Återställ VPN-Gateway-bladet](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. I hello återställa bladet, klicka på hello **återställa** knappen.

## <a name="ps"></a>PowerShell

### <a name="resource-manager-deployment-model"></a>Distributionsmodell med Resource Manager

Hej cmdlet för att återställa en gateway är **Återställ AzureRmVirtualNetworkGateway**. Innan du utför en återställning, kontrollera att du har hello senaste versionen av hello [Resource Manager PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0). hello återställer följande exempel en virtuell nätverksgateway med namnet VNet1GW i hello TestRG1 resursgrupp:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Resultat:

När du får ett returnerade resultat, kan du anta hello gateway återställning har slutförts. Men finns det inget i hello returnerade resultat att uttryckligen att hello återställning har slutförts. Om du vill toolook nära på hello historik toosee exakt när hello gateway återställa uppstod, du kan visa informationen i hello [Azure-portalen](https://portal.azure.com). I hello portal, navigerar för**'GatewayName' -> Resource Health**.

### <a name="resetclassic"></a>Klassiska distributionsmodellen

Hej cmdlet för att återställa en gateway är **Återställ AzureVNetGateway**. Innan du utför en återställning, kontrollera att du har hello senaste versionen av hello [Service Management (SM) PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0). hello återställs följande exempel hello-gateway för virtuellt nätverk med namnet ”ContosoVNet”:

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Resultat:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Azure CLI

Använd hello-tooreset hello gateway [az vnet-nätverksgateway återställa](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) kommando. hello återställer följande exempel en virtuell nätverksgateway med namnet VNet5GW i hello TestRG5 resursgrupp:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Resultat:

När du får ett returnerade resultat, kan du anta hello gateway återställning har slutförts. Men finns det inget i hello returnerade resultat att uttryckligen att hello återställning har slutförts. Om du vill toolook nära på hello historik toosee exakt när hello gateway återställa uppstod, du kan visa informationen i hello [Azure-portalen](https://portal.azure.com). I hello portal, navigerar för**'GatewayName' -> Resource Health**.
