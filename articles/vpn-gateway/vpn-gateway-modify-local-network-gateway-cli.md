---
title: "Ändra lokala nätverket gateway IP-adressprefix och VPN-Gateway IP-adress | Azure | CLI | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ändra IP-adressprefix för din lokala nätverksgateway med hjälp av Azure CLI."
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
ms.openlocfilehash: 7db1ad970ebb93d46d5a861f9a9b27bf121531a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a><span data-ttu-id="37df0-103">Ändra inställningar för lokalt nätverk gateway med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="37df0-103">Modify local network gateway settings using the Azure CLI</span></span>

<span data-ttu-id="37df0-104">Inställningar för din lokala nätverksgateway-adressprefixet eller Gateway IP-adress ändras.</span><span class="sxs-lookup"><span data-stu-id="37df0-104">Sometimes the settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="37df0-105">Den här artikeln visar hur du ändrar inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="37df0-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="37df0-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ från listan nedan:</span><span class="sxs-lookup"><span data-stu-id="37df0-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="37df0-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="37df0-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="37df0-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="37df0-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="37df0-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="37df0-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="37df0-110"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="37df0-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="37df0-111">Installera den senaste versionen av CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="37df0-111">Install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="37df0-112">Information om att installera CLI-kommandona finns i [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="37df0-112">For information about installing the CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="37df0-113"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="37df0-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="37df0-114"><a name="gwip"></a>Ändra IP-adressen för gateway</span><span class="sxs-lookup"><span data-stu-id="37df0-114"><a name="gwip"></a>Modify the gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="37df0-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37df0-115">Next steps</span></span>

<span data-ttu-id="37df0-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="37df0-116">You can verify your gateway connection.</span></span> <span data-ttu-id="37df0-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="37df0-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

