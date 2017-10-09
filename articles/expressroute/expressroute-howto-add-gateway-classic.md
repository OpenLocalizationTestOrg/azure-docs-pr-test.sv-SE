---
title: "Konfigurera en gateway för virtuellt nätverk för ExpressRoute med hjälp av PowerShell: klassiska: Azure | Microsoft Docs"
description: "Konfigurera en gateway för virtuellt nätverk för en klassisk distribution modell VNet med PowerShell för en ExpressRoute-konfiguration."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 6f37d4d9cba546b5416ab99040f5ef6dae273380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a><span data-ttu-id="fd333-103">Konfigurera en virtuell nätverksgateway för ExpressRoute med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="fd333-103">Configure a virtual network gateway for ExpressRoute using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd333-104">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd333-104">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="fd333-105">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd333-105">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="fd333-106">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fd333-106">Video - Azure Portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="fd333-107">Den här artikeln får du via hello steg tooadd ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet.</span><span class="sxs-lookup"><span data-stu-id="fd333-107">This article will walk you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="fd333-108">hello stegen för den här konfigurationen är specifikt för Vnet som har skapats med hello **klassiska distributionsmodellen** och som kommer att användas i en ExpressRoute-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fd333-108">hello steps for this configuration are specifically for VNets that were created using hello **classic deployment model** and that will be be used in an ExpressRoute configuration.</span></span> 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="fd333-109">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="fd333-109">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a><span data-ttu-id="fd333-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fd333-110">Before beginning</span></span>
<span data-ttu-id="fd333-111">Kontrollera att du har installerat hello Azure PowerShell-cmdlets som krävs för den här konfigurationen (1.0.2 eller senare).</span><span class="sxs-lookup"><span data-stu-id="fd333-111">Verify that you have installed hello Azure PowerShell cmdlets needed for this configuration (1.0.2 or later).</span></span> <span data-ttu-id="fd333-112">Om du inte har installerat hello cmdlets, behöver du toodo så innan du påbörjar hello konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="fd333-112">If you haven't installed hello cmdlets, you'll need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="fd333-113">Mer information om hur du installerar Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fd333-113">For more information about installing Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="fd333-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd333-114">Next steps</span></span>
<span data-ttu-id="fd333-115">När du har skapat hello VNet gateway kan länka du ditt VNet tooan ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="fd333-115">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="fd333-116">Se [länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="fd333-116">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

