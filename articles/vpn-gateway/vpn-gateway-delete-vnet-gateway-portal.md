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
ms.openlocfilehash: b014d43ab25124d3e08e19ca190b320fc8456593
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/04/2018
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Ta bort en virtuell nätverksgateway med hjälp av portalen

> [!div class="op_single_selector"]
> * [Azure-portalen](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klassisk)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

Den här artikeln innehåller instruktioner för att ta bort en Azure VPN-gatewayer distribueras med Resource Manager-distributionsmodellen. Det finns ett par olika metoder som du kan använda när du vill ta bort en virtuell nätverksgateway för en konfiguration för VPN-gateway.

- Om du vill ta bort allt och börja om från början, som i fallet med en testmiljö kan du ta bort resursgruppen. När du tar bort en resursgrupp tar bort alla resurser i gruppen. Den här metoden bör endast om du inte vill att alla resurser i resursgruppen. Du kan inte selektivt Radera endast några resurser med hjälp av den här metoden.

- Om du vill behålla vissa av resurserna i resursgruppen blir tar bort en virtuell nätverksgateway något mer komplicerad. Innan du kan ta bort den virtuella nätverksgatewayen, måste du först ta bort alla resurser som är beroende av denna gateway. De steg du följer beror på vilken typ av anslutningar som du skapade och beroende resurser för varje anslutning.

> [!IMPORTANT]
> Anvisningarna nedan beskriver hur du tar bort Azure VPN-gatewayer distribueras med Resource Manager-distributionsmodellen. Om du vill ta bort en VPN-gateway distribueras med den klassiska distributionsmodellen, Använd Azure PowerShell enligt [här](vpn-gateway-delete-vnet-gateway-classic-powershell.md).


## <a name="delete-a-vpn-gateway"></a>Ta bort en VPN-gateway

Du måste ta bort varje resurs som gäller för den virtuella nätverksgatewayen för att ta bort en virtuell nätverksgateway. Resurser måste tas bort i en viss ordning på grund av beroenden.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Nu är den virtuella nätverksgatewayen har tagits bort. Nästa steg hjälper dig ta bort alla resurser som används inte längre.

### <a name="to-delete-the-local-network-gateway"></a>Ta bort lokal nätverksgateway

1. I **alla resurser**, leta upp de lokala nätverksgatewayer som var associerade med varje anslutning.
2. På den **översikt** klickar du på bladet för den lokala nätverksgatewayen **ta bort**.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>Ta bort offentlig IP-adressresurs för gateway

1. I **alla resurser**, leta upp den offentliga IP-adressresurs som är kopplad till gateway. Om den virtuella nätverksgatewayen var aktiv-aktiv, visas två offentliga IP-adresser. 
2. På den **översikt** för offentlig IP-adress, klickar du på **ta bort**, sedan **Ja** att bekräfta.

### <a name="to-delete-the-gateway-subnet"></a>Ta bort gateway-undernätet.

1. I **alla resurser**, leta upp det virtuella nätverket. 
2. På den **undernät** bladet, klickar du på den **GatewaySubnet**, klicka på **ta bort**. 
3. Klicka på **Ja** att bekräfta att du vill ta bort gateway-undernätet.

## <a name="deleterg"></a>Ta bort en VPN-gateway genom att ta bort resursgruppen.

Om du inte vill inte behålla någon av dina resurser i resursgruppen och du vill börja om från början, kan du ta bort en hela resursgruppen. Detta är ett snabbt sätt att ta bort allt. Följande steg gäller endast för Resource Manager-distributionsmodellen.

1. I **alla resurser**, leta upp resursgruppen och klicka om du vill öppna bladet.
2. Klicka på **Ta bort**. Visa berörda resurser på Ta bort-bladet. Kontrollera att du vill ta bort alla resurser. Om inte, Följ stegen i [ta bort en VPN-gateway](#deletegw) överst i den här artikeln.
3. Om du vill fortsätta, skriver du namnet på resursgruppen som du vill ta bort och klicka sedan på **ta bort**.
