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
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="03caa-103">Verifiera en anslutning för VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="03caa-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="03caa-104">Den här artikeln beskrivs hur du tooverify en VPN-anslutning för gateway för både klassiska hello och Resource Manager distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="03caa-104">This article shows you how tooverify a VPN gateway connection for both hello classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="03caa-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="03caa-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="03caa-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="03caa-106">PowerShell</span></span>

<span data-ttu-id="03caa-107">tooverify en VPN-anslutning för gateway för hello Resource Manager distribution modellen med hjälp av PowerShell, installera hello senaste versionen av hello [Azure Resource Manager PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="03caa-107">tooverify a VPN gateway connection for hello Resource Manager deployment model using PowerShell, install hello latest version of hello [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="03caa-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="03caa-108">Azure CLI</span></span>

<span data-ttu-id="03caa-109">tooverify en VPN-anslutning för gateway för hello Resource Manager distribution modellen med hjälp av Azure CLI, installera hello senaste versionen av hello [CLI-kommandona](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="03caa-109">tooverify a VPN gateway connection for hello Resource Manager deployment model using Azure CLI, install hello latest version of hello [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="03caa-110">Azure-portalen (klassisk)</span><span class="sxs-lookup"><span data-stu-id="03caa-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="03caa-111">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="03caa-111">PowerShell (classic)</span></span>

<span data-ttu-id="03caa-112">tooverify VPN gateway-anslutningen för hello klassisk distribution modellen med hjälp av PowerShell, installera hello senaste versionerna av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="03caa-112">tooverify your VPN gateway connection for hello classic deployment model using PowerShell, install hello latest versions of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="03caa-113">Vara säker på att toodownload och installera hello [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul.</span><span class="sxs-lookup"><span data-stu-id="03caa-113">Be sure toodownload and install hello [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="03caa-114">Använd Add-AzureAccount toolog i toohello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="03caa-114">Use 'Add-AzureAccount' toolog in toohello classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="03caa-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03caa-115">Next steps</span></span>

* <span data-ttu-id="03caa-116">Du kan lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="03caa-116">You can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="03caa-117">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="03caa-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
