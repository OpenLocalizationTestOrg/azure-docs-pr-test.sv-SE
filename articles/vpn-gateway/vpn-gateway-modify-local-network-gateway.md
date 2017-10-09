---
title: "Ändra hello lokala nätverket gateway IP-adressprefix och hello VPN-Gateway IP-adress | Azure | PowerShell | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ändra IP-adressprefix för din lokala nätverksgateway med hjälp av PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 1353598b39a97fae9bdb424505a5ae2560482654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Ändra inställningar för den lokala nätverksgatewayen med PowerShell

Hello inställningar för din lokala nätverksgateway AddressPrefix eller GatewayIPAddress ändras. Den här artikeln beskrivs hur du toomodify inställningarna för lokala gateway. Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Innan du börjar

Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för mer information om hur du installerar hello PowerShell-cmdlets.

## <a name="ipaddprefix"></a>Ändra IP-adressprefix

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="gwip"></a>Ändra IP-adressen för hello gateway

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nästa steg

Du kan kontrollera din gateway-anslutningen. Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).