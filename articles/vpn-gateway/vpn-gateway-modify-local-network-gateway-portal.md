---
title: "Ändra hello lokala nätverket gateway IP-adressprefix och hello VPN-Gateway IP-adress | Azure | Portal | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ändra IP-adressprefix för din lokala nätverksgateway med hjälp av hello Azure-portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 001df7b748ccc234d87aab3501a4f0e4ddfe60f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-portal"></a><span data-ttu-id="3c3e2-103">Ändra inställningar för lokalt nätverk gateway med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3c3e2-103">Modify local network gateway settings using hello Azure portal</span></span>

<span data-ttu-id="3c3e2-104">Hello inställningar för din lokala nätverksgateway AddressPrefix eller GatewayIPAddress ändras.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="3c3e2-105">Den här artikeln beskrivs hur du toomodify inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="3c3e2-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="3c3e2-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c3e2-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3c3e2-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="3c3e2-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c3e2-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="3c3e2-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3c3e2-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <span data-ttu-id="3c3e2-110"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="3c3e2-110"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

<span data-ttu-id="3c3e2-111">När du ändrar IP-adressprefix beror hello steg du följer på om din lokala nätverksgateway har en anslutning.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-111">When you modify IP address prefixes, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <span data-ttu-id="3c3e2-112"><a name="gwip"></a>Ändra IP-adressen för hello gateway</span><span class="sxs-lookup"><span data-stu-id="3c3e2-112"><a name="gwip"></a>Modify hello gateway IP address</span></span>

<span data-ttu-id="3c3e2-113">Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-113">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="3c3e2-114">När du ändrar hello offentlig IP-adress, beror hello steg du följer på om din lokala nätverksgateway har en anslutning.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-114">When you change hello public IP address, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a><span data-ttu-id="3c3e2-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c3e2-115">Next steps</span></span>

<span data-ttu-id="3c3e2-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3c3e2-116">You can verify your gateway connection.</span></span> <span data-ttu-id="3c3e2-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3c3e2-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>