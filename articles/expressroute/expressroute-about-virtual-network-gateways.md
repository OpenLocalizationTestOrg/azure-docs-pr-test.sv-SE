---
title: "aaaAbout ExpressRoute virtuella nätverksgatewayerna | Microsoft Docs"
description: "Läs mer om virtuella nätverks-gateway för ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: 4daf4f96b4fadb00683d8e536e51734853008c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Om virtuella nätverksgatewayer för ExpressRoute
En virtuell nätverksgateway används toosend nätverkstrafik mellan virtuella Azure-nätverk och lokala platser. När du konfigurerar en ExpressRoute-anslutning, måste du skapa och konfigurera en virtuell nätverksgateway och gateway för virtuell nätverksanslutning.

När du skapar en virtuell nätverksgateway anger du flera inställningar. En av hello krävs inställningar anger om hello gateway ska användas för ExpressRoute eller plats-till-plats VPN-trafik. I hello Resource Manager-distributionsmodellen hello inställningen är '-GatewayType'.

När nätverkstrafiken som skickas på en privat anslutning, använder du hello gateway-typ 'ExpressRoute'. Detta är även hänvisade tooas en ExpressRoute-gateway. När skickas nätverkstrafik krypterade över hello offentliga Internet kan du använda hello gateway-typ 'Vpn'. Detta är refererad tooas en VPN-gateway. Plats-till-plats-, punkt-till-plats- och VNet-VNet-anslutningar använder allihop en VPN-gateway.

Varje virtuellt nätverk kan bara ha en VNet-gateway per gateway-typ. Du kan exempelvis ha en VNet-gateway som använder -GatewayType Vpn och en som använder -GatewayType ExpressRoute. Den här artikeln fokuserar på hello ExpressRoute-gateway för virtuellt nätverk.

## <a name="gwsku"></a>Gateway-SKU:er
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Om du vill tooupgrade gateway-tooa kraftigare hello gateway SKU, i de flesta fall kan du använda 'Storleksändring AzureRmVirtualNetworkGateway' PowerShell-cmdlet. Den här metoden fungerar för uppgraderingar tooStandard och HighPerformance SKU: er. Dock tooupgrade toohello UltraPerformance SKU måste toorecreate hello gateway.

### <a name="aggthroughput"></a>Beräknad sammanställda genomflöde av gateway-SKU
hello visar följande tabell hello gateway-typer och hello uppskattade sammanställda genomflöde. Den här tabellen gäller tooboth hello Resource Manager och klassiska distributionsmodeller.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Appens dataflöde beror på flera faktorer, till exempel hello slutpunkt till slutpunkt-svarstid och hello antalet trafiken flödar hello-programmet öppnas. hello talen i hello tabellen representerar hello övre gräns som programmet hello kan theorectically uppnå i en perfekt miljö. 
> 
>

## <a name="resources"></a>REST API: er och PowerShell-cmdlets
Ytterligare tekniska resurser och särskild syntax krav för användning av REST API: er och PowerShell-cmdletar för gateway för virtuella nätverkskonfigurationer finns hello följande sidor:

| **Klassisk** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST-API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST-API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Nästa steg
Se [ExpressRoute översikt](expressroute-introduction.md) för mer information om tillgänglig anslutning konfigurationer. 

