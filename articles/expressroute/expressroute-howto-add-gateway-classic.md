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
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Konfigurera en virtuell nätverksgateway för ExpressRoute med hjälp av PowerShell (klassisk)
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisk - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Den här artikeln får du via hello steg tooadd ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet. hello stegen för den här konfigurationen är specifikt för Vnet som har skapats med hello **klassiska distributionsmodellen** och som kommer att användas i en ExpressRoute-konfiguration. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Innan du börjar
Kontrollera att du har installerat hello Azure PowerShell-cmdlets som krävs för den här konfigurationen (1.0.2 eller senare). Om du inte har installerat hello cmdlets, behöver du toodo så innan du påbörjar hello konfigurationssteg. Mer information om hur du installerar Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Nästa steg
När du har skapat hello VNet gateway kan länka du ditt VNet tooan ExpressRoute-kretsen. Se [länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md).

