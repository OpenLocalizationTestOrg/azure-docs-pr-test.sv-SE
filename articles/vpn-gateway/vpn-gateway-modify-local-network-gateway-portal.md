---
title: "Ändra lokala nätverket gateway IP-adressprefix och VPN-Gateway IP-adress | Azure | Portal | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ändra IP-adressprefix för din lokala nätverksgateway som använder Azure portal."
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
ms.openlocfilehash: bdd6f90fe97408bd45a72adf58bfdc190207de46
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a><span data-ttu-id="43327-103">Ändra inställningar för lokalt nätverk gateway med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43327-103">Modify local network gateway settings using the Azure portal</span></span>

<span data-ttu-id="43327-104">Inställningar för din lokala nätverksgateway AddressPrefix eller GatewayIPAddress ändras.</span><span class="sxs-lookup"><span data-stu-id="43327-104">Sometimes the settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="43327-105">Den här artikeln visar hur du ändrar inställningarna för lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="43327-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="43327-106">Du kan också ändra dessa inställningar med hjälp av en annan metod genom att välja ett annat alternativ från listan nedan:</span><span class="sxs-lookup"><span data-stu-id="43327-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="43327-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="43327-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="43327-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="43327-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="43327-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="43327-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <span data-ttu-id="43327-110"><a name="ipaddprefix"></a>Ändra IP-adressprefix</span><span class="sxs-lookup"><span data-stu-id="43327-110"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

<span data-ttu-id="43327-111">När du ändrar IP-adressprefix beror de steg du följer på om din lokala nätverksgateway har en anslutning.</span><span class="sxs-lookup"><span data-stu-id="43327-111">When you modify IP address prefixes, the steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <span data-ttu-id="43327-112"><a name="gwip"></a>Ändra IP-adressen för gateway</span><span class="sxs-lookup"><span data-stu-id="43327-112"><a name="gwip"></a>Modify the gateway IP address</span></span>

<span data-ttu-id="43327-113">Om VPN-enheten som du vill ansluta till har bytt offentlig IP-adress måste du ändra den lokala nätverksgatewayen så att den återspeglar den ändringen.</span><span class="sxs-lookup"><span data-stu-id="43327-113">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="43327-114">När du ändrar den offentliga IP-adressen, beror de steg du följer på om din lokala nätverksgateway har en anslutning.</span><span class="sxs-lookup"><span data-stu-id="43327-114">When you change the public IP address, the steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a><span data-ttu-id="43327-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43327-115">Next steps</span></span>

<span data-ttu-id="43327-116">Du kan kontrollera din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="43327-116">You can verify your gateway connection.</span></span> <span data-ttu-id="43327-117">Se [verifiera en gatewayanslutning](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="43327-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>