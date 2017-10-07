---
title: aaaVerify en VPN-gatewayanslutning | Microsoft Docs
description: "Den här artikeln visar hur tooverify en virtuell nätverksanslutning VPN-Gateway."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 0d3da94a76b36251d629f82b1575328c7ac10b26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="verify-a-vpn-gateway-connection"></a>Verifiera en anslutning för VPN-Gateway

Den här artikeln beskrivs hur du tooverify en VPN-anslutning för gateway för både klassiska hello och Resource Manager distributionsmodellerna.

## <a name="azure-portal"></a>Azure Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

tooverify en VPN-anslutning för gateway för hello Resource Manager distribution modellen med hjälp av PowerShell, installera hello senaste versionen av hello [Azure Resource Manager PowerShell-cmdlets](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

tooverify en VPN-anslutning för gateway för hello Resource Manager distribution modellen med hjälp av Azure CLI, installera hello senaste versionen av hello [CLI-kommandona](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 eller senare).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure-portalen (klassisk)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (klassisk)

tooverify VPN gateway-anslutningen för hello klassisk distribution modellen med hjälp av PowerShell, installera hello senaste versionerna av hello Azure PowerShell-cmdlets. Vara säker på att toodownload och installera hello [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul. Använd Add-AzureAccount toolog i toohello klassiska distributionsmodellen.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Nästa steg

* Du kan lägga till virtuella datorer tooyour virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.
