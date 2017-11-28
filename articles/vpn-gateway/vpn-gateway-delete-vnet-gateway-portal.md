---
title: "Ta bort en virtuell nätverksgateway: Azure-portalen: Resource Manager | Microsoft Docs"
description: "Ta bort en virtuell nätverksgateway med Azure-portalen i Resource Manager-distributionsmodellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 1d289c09465cb8d5e4bfa569441dffcbf562b3bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a><span data-ttu-id="dee76-103">Ta bort en virtuell nätverksgateway med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="dee76-103">Delete a virtual network gateway using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="dee76-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dee76-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="dee76-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dee76-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="dee76-106">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="dee76-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

<span data-ttu-id="dee76-107">Det finns ett par olika metoder som du kan använda när du vill ta bort en virtuell nätverksgateway för en konfiguration för VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="dee76-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="dee76-108">Om du vill ta bort allt och börja om från början, som i fallet med en testmiljö kan du ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="dee76-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="dee76-109">När du tar bort en resursgrupp tar bort alla resurser i gruppen.</span><span class="sxs-lookup"><span data-stu-id="dee76-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="dee76-110">Det här är metod rekommenderas endast om du inte vill behålla några resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="dee76-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="dee76-111">Du kan inte selektivt Radera endast några resurser med hjälp av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="dee76-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="dee76-112">Om du vill behålla vissa av resurserna i resursgruppen blir tar bort en virtuell nätverksgateway något mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="dee76-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="dee76-113">Innan du kan ta bort den virtuella nätverksgatewayen, måste du först ta bort alla resurser som är beroende av denna gateway.</span><span class="sxs-lookup"><span data-stu-id="dee76-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="dee76-114">De steg du följer beror på vilken typ av anslutningar som du skapade och beroende resurser för varje anslutning.</span><span class="sxs-lookup"><span data-stu-id="dee76-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="delete-a-vpn-gateway"></a><span data-ttu-id="dee76-115">Ta bort en VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="dee76-115">Delete a VPN gateway</span></span>

<span data-ttu-id="dee76-116">Du måste ta bort varje resurs som gäller för den virtuella nätverksgatewayen för att ta bort en virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="dee76-116">To delete a virtual network gateway, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="dee76-117">Resurser måste tas bort i en viss ordning på grund av beroenden.</span><span class="sxs-lookup"><span data-stu-id="dee76-117">Resources must be deleted in a certain order due to dependencies.</span></span>

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

<span data-ttu-id="dee76-118">Nu är den virtuella nätverksgatewayen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="dee76-118">At this point, the virtual network gateway is deleted.</span></span> <span data-ttu-id="dee76-119">Nästa steg hjälper dig ta bort alla resurser som används inte längre.</span><span class="sxs-lookup"><span data-stu-id="dee76-119">The next steps help you delete any resources that are no longer being used.</span></span>

### <a name="to-delete-the-local-network-gateway"></a><span data-ttu-id="dee76-120">Ta bort lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="dee76-120">To delete the local network gateway</span></span>

1. <span data-ttu-id="dee76-121">I **alla resurser**, leta upp de lokala nätverksgatewayer som var associerade med varje anslutning.</span><span class="sxs-lookup"><span data-stu-id="dee76-121">In **All resources**, locate the local network gateways that were associated with each connection.</span></span>
2. <span data-ttu-id="dee76-122">På den **översikt** klickar du på bladet för den lokala nätverksgatewayen **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dee76-122">On the **Overview** blade for the local network gateway, click **Delete**.</span></span>

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a><span data-ttu-id="dee76-123">Ta bort offentlig IP-adressresurs för gateway</span><span class="sxs-lookup"><span data-stu-id="dee76-123">To delete the Public IP address resource for the gateway</span></span>

1. <span data-ttu-id="dee76-124">I **alla resurser**, leta upp den offentliga IP-adressresurs som är kopplad till gateway.</span><span class="sxs-lookup"><span data-stu-id="dee76-124">In **All resources**, locate the Public IP address resource that was associated to the gateway.</span></span> <span data-ttu-id="dee76-125">Om den virtuella nätverksgatewayen var aktiv-aktiv, visas två offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="dee76-125">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span> 
2. <span data-ttu-id="dee76-126">På den **översikt** för offentlig IP-adress, klickar du på **ta bort**, sedan **Ja** att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="dee76-126">On the **Overview** page for the Public IP address, click **Delete**, then **Yes** to confirm.</span></span>

### <a name="to-delete-the-gateway-subnet"></a><span data-ttu-id="dee76-127">Ta bort gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="dee76-127">To delete the gateway subnet</span></span>

1. <span data-ttu-id="dee76-128">I **alla resurser**, leta upp det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="dee76-128">In **All resources**, locate the virtual network.</span></span> 
2. <span data-ttu-id="dee76-129">På den **undernät** bladet, klickar du på den **GatewaySubnet**, klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dee76-129">On the **Subnets** blade, click the **GatewaySubnet**, then click **Delete**.</span></span> 
3. <span data-ttu-id="dee76-130">Klicka på **Ja** att bekräfta att du vill ta bort gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="dee76-130">Click **Yes** to confirm that you want to delete the gateway subnet.</span></span>

## <span data-ttu-id="dee76-131"><a name="deleterg"></a>Ta bort en VPN-gateway genom att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="dee76-131"><a name="deleterg"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="dee76-132">Om du inte vill inte behålla någon av dina resurser i resursgruppen och du vill börja om från början, kan du ta bort en hela resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="dee76-132">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="dee76-133">Detta är ett snabbt sätt att ta bort allt.</span><span class="sxs-lookup"><span data-stu-id="dee76-133">This is a quick way to remove everything.</span></span> <span data-ttu-id="dee76-134">Följande steg gäller endast för Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dee76-134">The following steps apply only to the Resource Manager deployment model.</span></span>

1. <span data-ttu-id="dee76-135">I **alla resurser**, leta upp resursgruppen och klicka om du vill öppna bladet.</span><span class="sxs-lookup"><span data-stu-id="dee76-135">In **All resources**, locate the resource group and click to open the blade.</span></span>
2. <span data-ttu-id="dee76-136">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dee76-136">Click **Delete**.</span></span> <span data-ttu-id="dee76-137">Visa berörda resurser på Ta bort-bladet.</span><span class="sxs-lookup"><span data-stu-id="dee76-137">On the Delete blade, view the affected resources.</span></span> <span data-ttu-id="dee76-138">Kontrollera att du vill ta bort alla resurser.</span><span class="sxs-lookup"><span data-stu-id="dee76-138">Make sure that you want to delete all of these resources.</span></span> <span data-ttu-id="dee76-139">Om inte, Följ stegen i [ta bort en VPN-gateway](#deletegw) överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dee76-139">If not, use the steps in [Delete a VPN gateway](#deletegw) at the top of this article.</span></span>
3. <span data-ttu-id="dee76-140">Om du vill fortsätta, skriver du namnet på resursgruppen som du vill ta bort och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dee76-140">To proceed, type the name of the resource group that you want to delete, then click **Delete**.</span></span>