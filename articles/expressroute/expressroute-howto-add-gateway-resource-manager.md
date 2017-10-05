---
title: "Lägga till en virtuell nätverksgateway till ett virtuellt nätverk för ExpressRoute: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln beskriver hur du lägger till en gateway för virtuellt nätverk till ett redan har skapat Resource Manager-VNet för ExpressRoute."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 3aeddd03e0be548933775164ae790ba208fc13ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="58a6c-103">Konfigurera en virtuell nätverksgateway för ExpressRoute med PowerShell</span><span class="sxs-lookup"><span data-stu-id="58a6c-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="58a6c-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="58a6c-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="58a6c-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="58a6c-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="58a6c-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="58a6c-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="58a6c-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="58a6c-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="58a6c-108">Den här artikeln vägleder dig genom stegen för att lägga till, ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet.</span><span class="sxs-lookup"><span data-stu-id="58a6c-108">This article walks you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="58a6c-109">Stegen för den här konfigurationen är specifikt för Vnet som har skapats med Resource Manager-distributionsmodellen som ska användas i en ExpressRoute-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="58a6c-109">The steps for this configuration are specifically for VNets that were created using the Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="58a6c-110">Mer information om gateways för virtuella datornätverk och gateway-konfigurationsinställningar för ExpressRoute finns [om virtuella nätverksgatewayerna expressroute](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="58a6c-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="58a6c-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="58a6c-111">Before beginning</span></span>
<span data-ttu-id="58a6c-112">Kontrollera att du har installerat det senaste Azure PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="58a6c-112">Verify that you have installed the latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="58a6c-113">Om du inte har installerat de senaste cmdletarna, måste du göra det innan du påbörjar konfigurationsstegen.</span><span class="sxs-lookup"><span data-stu-id="58a6c-113">If you haven't installed the latest cmdlets, you need to do so before beginning the configuration steps.</span></span> <span data-ttu-id="58a6c-114">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="58a6c-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="58a6c-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58a6c-115">Next steps</span></span>
<span data-ttu-id="58a6c-116">När du har skapat en gateway för virtuellt nätverk kan du länka ditt VNet till en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="58a6c-116">After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit.</span></span> <span data-ttu-id="58a6c-117">Se [länka ett virtuellt nätverk till en ExpressRoute-krets](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="58a6c-117">See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

