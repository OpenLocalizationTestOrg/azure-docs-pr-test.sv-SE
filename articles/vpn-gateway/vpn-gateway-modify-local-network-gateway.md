---
title: "Ändra lokala nätverket gateway IP-adressprefix och VPN-Gateway IP-adress | Azure | PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 2a095b96a8c352abeca72640d37c0d629b447763
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a><span data-ttu-id="a0cf8-103">Ändra inställningar för den lokala nätverksgatewayen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0cf8-103">Modify local network gateway settings using PowerShell</span></span>

<span data-ttu-id="a0cf8-104">Inställningar för din lokala nätverksgateway AddressPrefix eller GatewayIPAddress ändras.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-104">Sometimes the settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="a0cf8-105">Den här artikeln visar hur du ändrar inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="a0cf8-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ från listan nedan:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a0cf8-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a0cf8-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="a0cf8-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0cf8-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="a0cf8-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a0cf8-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="a0cf8-110"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a0cf8-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="a0cf8-111">Installera den senaste versionen av Azure Resource Managers PowerShell-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-111">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a0cf8-112">Mer information om hur man installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-112">See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information about installing the PowerShell cmdlets.</span></span>

## <span data-ttu-id="a0cf8-113"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="a0cf8-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="a0cf8-114"><a name="gwip"></a>Ändra IP-adressen för gateway</span><span class="sxs-lookup"><span data-stu-id="a0cf8-114"><a name="gwip"></a>Modify the gateway IP address</span></span>

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="a0cf8-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0cf8-115">Next steps</span></span>

<span data-ttu-id="a0cf8-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-116">You can verify your gateway connection.</span></span> <span data-ttu-id="a0cf8-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>