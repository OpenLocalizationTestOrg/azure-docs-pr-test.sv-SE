---
title: "Äldre Azure virtuell nätverksgateway SKU: er | Microsoft Docs"
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
ms.openlocfilehash: 3b2126b1ecd1613950bbf311ae08fafd4af0d51f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="c7927-103">Arbeta med virtuell nätverksgateway SKU: er (äldre SKU: er)</span><span class="sxs-lookup"><span data-stu-id="c7927-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="c7927-104">Den här artikeln innehåller information om den äldre (gamla) virtuell nätverksgatewayen SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c7927-104">This article contains information about the legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="c7927-105">Äldre SKU: er fortfarande fungerar i båda distributionsmodellerna för VPN-gatewayer som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="c7927-105">The legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="c7927-106">Klassiska VPN-gatewayer fortsätta att använda äldre SKU: er, både för befintliga gateways och för nya gatewayer.</span><span class="sxs-lookup"><span data-stu-id="c7927-106">Classic VPN gateways continue to use the legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="c7927-107">När du skapar nya Resource Manager VPN gateway ska använda den nya gatewayen SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c7927-107">When creating new Resource Manager VPN gateways, use the new gateway SKUs.</span></span> <span data-ttu-id="c7927-108">Information om nya SKU: er finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="c7927-108">For information about the new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="c7927-109"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="c7927-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="c7927-110"><a name="agg"></a>Beräknad sammanställda genomflöde av SKU</span><span class="sxs-lookup"><span data-stu-id="c7927-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="c7927-111"><a name="config"></a>Konfigurationer som stöds av SKU- och VPN-typ</span><span class="sxs-lookup"><span data-stu-id="c7927-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="c7927-112"><a name="resize"></a>Ändra storlek på en gateway (ändra en gateway-SKU)</span><span class="sxs-lookup"><span data-stu-id="c7927-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="c7927-113">Du kan ändra storlek på en gateway-SKU inom samma SKU-familjen.</span><span class="sxs-lookup"><span data-stu-id="c7927-113">You can resize a gateway SKU within the same SKU family.</span></span> <span data-ttu-id="c7927-114">Till exempel om du har en Standard-SKU, du kan ändra storlek till en HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="c7927-114">For example, if you have a Standard SKU, you can resize to a HighPerformance SKU.</span></span> <span data-ttu-id="c7927-115">Du kan inte ändra storlek på VPN-gatewayer mellan de gamla SKU: er och nya SKU-familjer.</span><span class="sxs-lookup"><span data-stu-id="c7927-115">You can't resize your VPN gateways between the old SKUs and the new SKU families.</span></span> <span data-ttu-id="c7927-116">Du kan exempelvis gå från en Standard-SKU till en VpnGw2 SKU.</span><span class="sxs-lookup"><span data-stu-id="c7927-116">For example, you can't go from a Standard SKU to a VpnGw2 SKU.</span></span> 

<span data-ttu-id="c7927-117">Om du vill ändra storlek på en gateway-SKU för den klassiska distributionsmodellen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c7927-117">To resize a gateway SKU for the classic deployment model, use the following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="c7927-118">Om du vill ändra storlek på en gateway-SKU för Resource Manager-distributionsmodellen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c7927-118">To resize a gateway SKU for the Resource Manager deployment model, use the following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="c7927-119"><a name="migrate"></a>Migrera till ny gateway SKU: er</span><span class="sxs-lookup"><span data-stu-id="c7927-119"><a name="migrate"></a>Migrate to the new gateway SKUs</span></span>

<span data-ttu-id="c7927-120">Om du arbetar med Resource Manager-distributionsmodellen kan du migrera till ny gateway SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c7927-120">If you are working with the Resource Manager deployment model, you can migrate to the new gateway SKUS.</span></span> <span data-ttu-id="c7927-121">Om du arbetar med den klassiska distributionsmodellen kan inte migreras till de nya SKU: er och i stället måste fortsätta att använda de äldre SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c7927-121">If you are working with the classic deployment model, you can't migrate to the new SKUs and must instead continue to use the legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="c7927-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7927-122">Next steps</span></span>

<span data-ttu-id="c7927-123">Mer information om den nya Gateway-SKU: er finns [Gateway-SKU: er](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="c7927-123">For more information about the new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="c7927-124">Mer information om konfigurationsinställningar finns [konfigurationsinställningar för VPN-Gateway](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="c7927-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>