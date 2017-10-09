---
title: "Ändra hello lokala nätverket gateway IP-adressprefix och hello VPN-Gateway IP-adress | Azure | CLI | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ändra IP-adressprefix för din lokala nätverksgateway med hjälp av hello Azure CLI."
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
ms.openlocfilehash: 4b8bbf3e9d3d42ac2d12f87360fa0a4134c57a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-cli"></a><span data-ttu-id="9ae26-103">Ändra inställningar för lokalt nätverk gateway med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9ae26-103">Modify local network gateway settings using hello Azure CLI</span></span>

<span data-ttu-id="9ae26-104">Hello inställningar för din lokala nätverksgateway-adressprefixet eller Gateway IP-adress ändras.</span><span class="sxs-lookup"><span data-stu-id="9ae26-104">Sometimes hello settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="9ae26-105">Den här artikeln beskrivs hur du toomodify inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="9ae26-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="9ae26-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="9ae26-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9ae26-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9ae26-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="9ae26-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ae26-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="9ae26-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9ae26-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="9ae26-110"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9ae26-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="9ae26-111">Installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="9ae26-111">Install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="9ae26-112">Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9ae26-112">For information about installing hello CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="9ae26-113"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="9ae26-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="9ae26-114"><a name="gwip"></a>Ändra IP-adressen för hello gateway</span><span class="sxs-lookup"><span data-stu-id="9ae26-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="9ae26-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ae26-115">Next steps</span></span>

<span data-ttu-id="9ae26-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9ae26-116">You can verify your gateway connection.</span></span> <span data-ttu-id="9ae26-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9ae26-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

