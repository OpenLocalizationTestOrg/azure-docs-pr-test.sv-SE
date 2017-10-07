---
title: "Lägga till ett virtuellt nätverk gateway tooa VNet för ExpressRoute: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att lägga till ett virtuellt nätverk gateway tooan redan skapat Resource Manager VNet expressroute."
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
ms.openlocfilehash: 8983430b426ad7c4af766294fa16427c5e9df5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="b51c5-103">Konfigurera en virtuell nätverksgateway för ExpressRoute med PowerShell</span><span class="sxs-lookup"><span data-stu-id="b51c5-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b51c5-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b51c5-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="b51c5-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="b51c5-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="b51c5-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="b51c5-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="b51c5-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b51c5-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="b51c5-108">Den här artikeln vägleder dig genom hello steg tooadd, ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet.</span><span class="sxs-lookup"><span data-stu-id="b51c5-108">This article walks you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="b51c5-109">hello stegen för den här konfigurationen är specifikt för Vnet som har skapats med hello Resource Manager-distributionsmodellen som ska användas i en ExpressRoute-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b51c5-109">hello steps for this configuration are specifically for VNets that were created using hello Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="b51c5-110">Mer information om gateways för virtuella datornätverk och gateway-konfigurationsinställningar för ExpressRoute finns [om virtuella nätverksgatewayerna expressroute](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="b51c5-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="b51c5-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b51c5-111">Before beginning</span></span>
<span data-ttu-id="b51c5-112">Kontrollera att du har installerat hello senaste Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="b51c5-112">Verify that you have installed hello latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b51c5-113">Om du inte har installerat hello senaste cmdlets måste toodo så innan du påbörjar hello konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="b51c5-113">If you haven't installed hello latest cmdlets, you need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="b51c5-114">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b51c5-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="b51c5-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b51c5-115">Next steps</span></span>
<span data-ttu-id="b51c5-116">När du har skapat hello VNet gateway kan länka du ditt VNet tooan ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b51c5-116">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="b51c5-117">Se [länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b51c5-117">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

