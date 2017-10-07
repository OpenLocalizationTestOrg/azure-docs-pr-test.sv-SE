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
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Konfigurera en virtuell nätverksgateway för ExpressRoute med PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisk - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Den här artikeln vägleder dig genom hello steg tooadd, ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet. hello stegen för den här konfigurationen är specifikt för Vnet som har skapats med hello Resource Manager-distributionsmodellen som ska användas i en ExpressRoute-konfiguration. Mer information om gateways för virtuella datornätverk och gateway-konfigurationsinställningar för ExpressRoute finns [om virtuella nätverksgatewayerna expressroute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Innan du börjar
Kontrollera att du har installerat hello senaste Azure PowerShell-cmdlets. Om du inte har installerat hello senaste cmdlets måste toodo så innan du påbörjar hello konfigurationssteg. Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Nästa steg
När du har skapat hello VNet gateway kan länka du ditt VNet tooan ExpressRoute-kretsen. Se [länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md).

