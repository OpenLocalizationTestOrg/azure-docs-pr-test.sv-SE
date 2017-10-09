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
# <a name="modify-local-network-gateway-settings-using-powershell"></a><span data-ttu-id="c777a-103">Ändra inställningar för den lokala nätverksgatewayen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="c777a-103">Modify local network gateway settings using PowerShell</span></span>

<span data-ttu-id="c777a-104">Hello inställningar för din lokala nätverksgateway AddressPrefix eller GatewayIPAddress ändras.</span><span class="sxs-lookup"><span data-stu-id="c777a-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="c777a-105">Den här artikeln beskrivs hur du toomodify inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="c777a-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="c777a-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="c777a-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c777a-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c777a-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="c777a-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c777a-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="c777a-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c777a-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="c777a-110"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c777a-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="c777a-111">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c777a-111">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="c777a-112">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c777a-112">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information about installing hello PowerShell cmdlets.</span></span>

## <span data-ttu-id="c777a-113"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="c777a-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="c777a-114"><a name="gwip"></a>Ändra IP-adressen för hello gateway</span><span class="sxs-lookup"><span data-stu-id="c777a-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="c777a-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c777a-115">Next steps</span></span>

<span data-ttu-id="c777a-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c777a-116">You can verify your gateway connection.</span></span> <span data-ttu-id="c777a-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c777a-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>