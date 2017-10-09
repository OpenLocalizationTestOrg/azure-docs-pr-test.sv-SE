---
title: "aaaLegacy Azure virtuell nätverksgateway SKU: er | Microsoft Docs"
description: "Gamla virtuella nätverks-gateway-SKU: er."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 710417581423d2fbc62827cab7949f2e137c5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Arbeta med virtuell nätverksgateway SKU: er (äldre SKU: er)

Den här artikeln innehåller information om hello äldre (gamla) virtuell nätverksgateway SKU: er. hello äldre SKU: er fortfarande fungerar i båda distributionsmodellerna för VPN-gatewayer som redan har skapats. Klassiska VPN-gatewayer fortsätta toouse hello äldre SKU: er, både för befintliga gateways och för nya gatewayer. När du skapar nya Resource Manager VPN gateway Använd hello ny gateway SKU: er. Information om hello nya SKU: er finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKU:er

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Beräknad sammanställda genomflöde av SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Konfigurationer som stöds av SKU- och VPN-typ

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Ändra storlek på en gateway (ändra en gateway-SKU)

Du kan ändra storlek på en gateway-SKU inom hello samma familj av SKU. Om du har en Standard-SKU, kan du ändra tooa HighPerformance SKU. Du kan inte ändra storlek på din VPN-gatewayer mellan hello gamla SKU: er och hello nya SKU-familjer. Du kan inte gå från en Standard-SKU tooa VpnGw2 SKU. 

tooresize gateway SKU för hello klassiska distributionsmodellen, Använd hello följande kommando:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

tooresize gateway SKU för hello Resource Manager-distributionsmodellen, Använd hello följande kommando:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migrera toohello ny gateway SKU: er

Om du arbetar med hello Resource Manager-distributionsmodellen kan du migrera toohello ny gateway SKU: er. Om du arbetar med hello klassiska distributionsmodellen kan du inte migrera toohello nya SKU: er och måste i stället fortsätta toouse hello äldre SKU: er.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Nästa steg

Mer information om Hej ny Gateway-SKU: er, se [Gateway-SKU: er](vpn-gateway-about-vpngateways.md#gwsku).

Mer information om konfigurationsinställningar finns [konfigurationsinställningar för VPN-Gateway](vpn-gateway-about-vpn-gateway-settings.md).