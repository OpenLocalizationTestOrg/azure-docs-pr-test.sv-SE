---
title: "Verifiera en anslutning för VPN-Gateway | Microsoft Docs"
description: "Den här artikeln visar hur du verifierar ett virtuellt nätverk VPN Gateway-anslutningen."
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
ms.openlocfilehash: b2d702ecdd5e1fca342e7c84c6e75339097f0bcd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="94e50-103">Verifiera en anslutning för VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="94e50-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="94e50-104">Den här artikeln visar hur du verifierar en VPN-anslutning för gateway för både klassiskt och Resource Manager distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="94e50-104">This article shows you how to verify a VPN gateway connection for both the classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="94e50-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94e50-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="94e50-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="94e50-106">PowerShell</span></span>

<span data-ttu-id="94e50-107">För att verifiera en VPN-anslutning för gateway för Resource Manager-distributionsmodellen med hjälp av PowerShell, installera den senaste versionen av den [Azure Resource Manager PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="94e50-107">To verify a VPN gateway connection for the Resource Manager deployment model using PowerShell, install the latest version of the [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="94e50-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="94e50-108">Azure CLI</span></span>

<span data-ttu-id="94e50-109">För att verifiera en VPN-anslutning för gateway för Resource Manager-distributionsmodellen med hjälp av Azure CLI, installera den senaste versionen av den [CLI-kommandona](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="94e50-109">To verify a VPN gateway connection for the Resource Manager deployment model using Azure CLI, install the latest version of the [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="94e50-110">Azure-portalen (klassisk)</span><span class="sxs-lookup"><span data-stu-id="94e50-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="94e50-111">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="94e50-111">PowerShell (classic)</span></span>

<span data-ttu-id="94e50-112">Installera de senaste versionerna av Azure PowerShell-cmdlets för att verifiera VPN-gateway-anslutningen för den klassiska distributionsmodellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="94e50-112">To verify your VPN gateway connection for the classic deployment model using PowerShell, install the latest versions of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="94e50-113">Se till att ladda ned och installera den [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modul.</span><span class="sxs-lookup"><span data-stu-id="94e50-113">Be sure to download and install the [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="94e50-114">Använd Add-AzureAccount för att logga in på den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="94e50-114">Use 'Add-AzureAccount' to log in to the classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="94e50-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94e50-115">Next steps</span></span>

* <span data-ttu-id="94e50-116">Du kan lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="94e50-116">You can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="94e50-117">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="94e50-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>