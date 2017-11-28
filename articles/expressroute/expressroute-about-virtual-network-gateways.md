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
# <a name="about-virtual-network-gateways-for-expressroute"></a><span data-ttu-id="9652c-103">Om virtuella nätverksgatewayer för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9652c-103">About virtual network gateways for ExpressRoute</span></span>
<span data-ttu-id="9652c-104">En virtuell nätverksgateway används toosend nätverkstrafik mellan virtuella Azure-nätverk och lokala platser.</span><span class="sxs-lookup"><span data-stu-id="9652c-104">A virtual network gateway is used toosend network traffic between Azure virtual networks and on-premises locations.</span></span> <span data-ttu-id="9652c-105">När du konfigurerar en ExpressRoute-anslutning, måste du skapa och konfigurera en virtuell nätverksgateway och gateway för virtuell nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="9652c-105">When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.</span></span>

<span data-ttu-id="9652c-106">När du skapar en virtuell nätverksgateway anger du flera inställningar.</span><span class="sxs-lookup"><span data-stu-id="9652c-106">When you create a virtual network gateway, you specify several settings.</span></span> <span data-ttu-id="9652c-107">En av hello krävs inställningar anger om hello gateway ska användas för ExpressRoute eller plats-till-plats VPN-trafik.</span><span class="sxs-lookup"><span data-stu-id="9652c-107">One of hello required settings specifies whether hello gateway will be used for ExpressRoute or Site-to-Site VPN traffic.</span></span> <span data-ttu-id="9652c-108">I hello Resource Manager-distributionsmodellen hello inställningen är '-GatewayType'.</span><span class="sxs-lookup"><span data-stu-id="9652c-108">In hello Resource Manager deployment model, hello setting is '-GatewayType'.</span></span>

<span data-ttu-id="9652c-109">När nätverkstrafiken som skickas på en privat anslutning, använder du hello gateway-typ 'ExpressRoute'.</span><span class="sxs-lookup"><span data-stu-id="9652c-109">When network traffic is sent on a private connection, you use hello gateway type 'ExpressRoute'.</span></span> <span data-ttu-id="9652c-110">Detta är även hänvisade tooas en ExpressRoute-gateway.</span><span class="sxs-lookup"><span data-stu-id="9652c-110">This is also referred tooas an ExpressRoute gateway.</span></span> <span data-ttu-id="9652c-111">När skickas nätverkstrafik krypterade över hello offentliga Internet kan du använda hello gateway-typ 'Vpn'.</span><span class="sxs-lookup"><span data-stu-id="9652c-111">When network traffic is sent encrypted across hello public Internet, you use hello gateway type 'Vpn'.</span></span> <span data-ttu-id="9652c-112">Detta är refererad tooas en VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="9652c-112">This is referred tooas a VPN gateway.</span></span> <span data-ttu-id="9652c-113">Plats-till-plats-, punkt-till-plats- och VNet-VNet-anslutningar använder allihop en VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="9652c-113">Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.</span></span>

<span data-ttu-id="9652c-114">Varje virtuellt nätverk kan bara ha en VNet-gateway per gateway-typ.</span><span class="sxs-lookup"><span data-stu-id="9652c-114">Each virtual network can have only one virtual network gateway per gateway type.</span></span> <span data-ttu-id="9652c-115">Du kan exempelvis ha en VNet-gateway som använder -GatewayType Vpn och en som använder -GatewayType ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9652c-115">For example, you can have one virtual network gateway that uses -GatewayType Vpn, and one that uses -GatewayType ExpressRoute.</span></span> <span data-ttu-id="9652c-116">Den här artikeln fokuserar på hello ExpressRoute-gateway för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9652c-116">This article focuses on hello ExpressRoute virtual network gateway.</span></span>

## <span data-ttu-id="9652c-117"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="9652c-117"><a name="gwsku"></a>Gateway SKUs</span></span>
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

<span data-ttu-id="9652c-118">Om du vill tooupgrade gateway-tooa kraftigare hello gateway SKU, i de flesta fall kan du använda 'Storleksändring AzureRmVirtualNetworkGateway' PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9652c-118">If you want tooupgrade your gateway tooa more powerful gateway SKU, in most cases you can use hello 'Resize-AzureRmVirtualNetworkGateway' PowerShell cmdlet.</span></span> <span data-ttu-id="9652c-119">Den här metoden fungerar för uppgraderingar tooStandard och HighPerformance SKU: er.</span><span class="sxs-lookup"><span data-stu-id="9652c-119">This will work for upgrades tooStandard and HighPerformance SKUs.</span></span> <span data-ttu-id="9652c-120">Dock tooupgrade toohello UltraPerformance SKU måste toorecreate hello gateway.</span><span class="sxs-lookup"><span data-stu-id="9652c-120">However, tooupgrade toohello UltraPerformance SKU, you will need toorecreate hello gateway.</span></span>

### <span data-ttu-id="9652c-121"><a name="aggthroughput"></a>Beräknad sammanställda genomflöde av gateway-SKU</span><span class="sxs-lookup"><span data-stu-id="9652c-121"><a name="aggthroughput"></a>Estimated aggregate throughput by gateway SKU</span></span>
<span data-ttu-id="9652c-122">hello visar följande tabell hello gateway-typer och hello uppskattade sammanställda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="9652c-122">hello following table shows hello gateway types and hello estimated aggregate throughput.</span></span> <span data-ttu-id="9652c-123">Den här tabellen gäller tooboth hello Resource Manager och klassiska distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="9652c-123">This table applies tooboth hello Resource Manager and classic deployment models.</span></span>

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="9652c-124">Appens dataflöde beror på flera faktorer, till exempel hello slutpunkt till slutpunkt-svarstid och hello antalet trafiken flödar hello-programmet öppnas.</span><span class="sxs-lookup"><span data-stu-id="9652c-124">Application throughput depends on multiple factors, such as hello end-to-end latency, and hello number of traffic flows hello application opens.</span></span> <span data-ttu-id="9652c-125">hello talen i hello tabellen representerar hello övre gräns som programmet hello kan theorectically uppnå i en perfekt miljö.</span><span class="sxs-lookup"><span data-stu-id="9652c-125">hello numbers in hello table represent hello upper limit that hello application can theorectically achieve in an ideal environment.</span></span> 
> 
>

## <span data-ttu-id="9652c-126"><a name="resources"></a>REST API: er och PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="9652c-126"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>
<span data-ttu-id="9652c-127">Ytterligare tekniska resurser och särskild syntax krav för användning av REST API: er och PowerShell-cmdletar för gateway för virtuella nätverkskonfigurationer finns hello följande sidor:</span><span class="sxs-lookup"><span data-stu-id="9652c-127">For additional technical resources and specific syntax requirements when using REST APIs and PowerShell cmdlets for virtual network gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="9652c-128">**Klassisk**</span><span class="sxs-lookup"><span data-stu-id="9652c-128">**Classic**</span></span> | <span data-ttu-id="9652c-129">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="9652c-129">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="9652c-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9652c-130">PowerShell</span></span>](https://msdn.microsoft.com/library/mt270335.aspx) |[<span data-ttu-id="9652c-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9652c-131">PowerShell</span></span>](https://msdn.microsoft.com/library/mt163510.aspx) |
| [<span data-ttu-id="9652c-132">REST-API</span><span class="sxs-lookup"><span data-stu-id="9652c-132">REST API</span></span>](https://msdn.microsoft.com/library/jj154113.aspx) |[<span data-ttu-id="9652c-133">REST-API</span><span class="sxs-lookup"><span data-stu-id="9652c-133">REST API</span></span>](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a><span data-ttu-id="9652c-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9652c-134">Next steps</span></span>
<span data-ttu-id="9652c-135">Se [ExpressRoute översikt](expressroute-introduction.md) för mer information om tillgänglig anslutning konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="9652c-135">See [ExpressRoute Overview](expressroute-introduction.md) for more information about available connection configurations.</span></span> 

