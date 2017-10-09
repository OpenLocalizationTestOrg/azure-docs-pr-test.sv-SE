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
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="0be68-103">Arbeta med virtuell nätverksgateway SKU: er (äldre SKU: er)</span><span class="sxs-lookup"><span data-stu-id="0be68-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="0be68-104">Den här artikeln innehåller information om hello äldre (gamla) virtuell nätverksgateway SKU: er.</span><span class="sxs-lookup"><span data-stu-id="0be68-104">This article contains information about hello legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="0be68-105">hello äldre SKU: er fortfarande fungerar i båda distributionsmodellerna för VPN-gatewayer som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="0be68-105">hello legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="0be68-106">Klassiska VPN-gatewayer fortsätta toouse hello äldre SKU: er, både för befintliga gateways och för nya gatewayer.</span><span class="sxs-lookup"><span data-stu-id="0be68-106">Classic VPN gateways continue toouse hello legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="0be68-107">När du skapar nya Resource Manager VPN gateway Använd hello ny gateway SKU: er.</span><span class="sxs-lookup"><span data-stu-id="0be68-107">When creating new Resource Manager VPN gateways, use hello new gateway SKUs.</span></span> <span data-ttu-id="0be68-108">Information om hello nya SKU: er finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0be68-108">For information about hello new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="0be68-109"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="0be68-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="0be68-110"><a name="agg"></a>Beräknad sammanställda genomflöde av SKU</span><span class="sxs-lookup"><span data-stu-id="0be68-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="0be68-111"><a name="config"></a>Konfigurationer som stöds av SKU- och VPN-typ</span><span class="sxs-lookup"><span data-stu-id="0be68-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="0be68-112"><a name="resize"></a>Ändra storlek på en gateway (ändra en gateway-SKU)</span><span class="sxs-lookup"><span data-stu-id="0be68-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="0be68-113">Du kan ändra storlek på en gateway-SKU inom hello samma familj av SKU.</span><span class="sxs-lookup"><span data-stu-id="0be68-113">You can resize a gateway SKU within hello same SKU family.</span></span> <span data-ttu-id="0be68-114">Om du har en Standard-SKU, kan du ändra tooa HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="0be68-114">For example, if you have a Standard SKU, you can resize tooa HighPerformance SKU.</span></span> <span data-ttu-id="0be68-115">Du kan inte ändra storlek på din VPN-gatewayer mellan hello gamla SKU: er och hello nya SKU-familjer.</span><span class="sxs-lookup"><span data-stu-id="0be68-115">You can't resize your VPN gateways between hello old SKUs and hello new SKU families.</span></span> <span data-ttu-id="0be68-116">Du kan inte gå från en Standard-SKU tooa VpnGw2 SKU.</span><span class="sxs-lookup"><span data-stu-id="0be68-116">For example, you can't go from a Standard SKU tooa VpnGw2 SKU.</span></span> 

<span data-ttu-id="0be68-117">tooresize gateway SKU för hello klassiska distributionsmodellen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0be68-117">tooresize a gateway SKU for hello classic deployment model, use hello following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="0be68-118">tooresize gateway SKU för hello Resource Manager-distributionsmodellen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0be68-118">tooresize a gateway SKU for hello Resource Manager deployment model, use hello following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="0be68-119"><a name="migrate"></a>Migrera toohello ny gateway SKU: er</span><span class="sxs-lookup"><span data-stu-id="0be68-119"><a name="migrate"></a>Migrate toohello new gateway SKUs</span></span>

<span data-ttu-id="0be68-120">Om du arbetar med hello Resource Manager-distributionsmodellen kan du migrera toohello ny gateway SKU: er.</span><span class="sxs-lookup"><span data-stu-id="0be68-120">If you are working with hello Resource Manager deployment model, you can migrate toohello new gateway SKUS.</span></span> <span data-ttu-id="0be68-121">Om du arbetar med hello klassiska distributionsmodellen kan du inte migrera toohello nya SKU: er och måste i stället fortsätta toouse hello äldre SKU: er.</span><span class="sxs-lookup"><span data-stu-id="0be68-121">If you are working with hello classic deployment model, you can't migrate toohello new SKUs and must instead continue toouse hello legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="0be68-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0be68-122">Next steps</span></span>

<span data-ttu-id="0be68-123">Mer information om Hej ny Gateway-SKU: er, se [Gateway-SKU: er](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="0be68-123">For more information about hello new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="0be68-124">Mer information om konfigurationsinställningar finns [konfigurationsinställningar för VPN-Gateway](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="0be68-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>