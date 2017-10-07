---
title: "aaaSet av slutpunkter på en klassiska Windows VM | Microsoft Docs"
description: "Lär dig tooset av slutpunkter för en virtuell Windows-dator i hello Azure klassiska portal tooallow kommunikation med en Windows-dator i Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a><span data-ttu-id="eefd5-103">Hur tooset av slutpunkter för klassiska Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="eefd5-103">How tooset up endpoints on a classic Windows virtual machine in Azure</span></span>
<span data-ttu-id="eefd5-104">Alla Windows virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen automatiskt kan kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma molntjänst eller ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="eefd5-104">All Windows virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="eefd5-105">Datorer på hello Internet eller andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="eefd5-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="eefd5-106">Den här artikeln är också tillgängligt för [virtuella Linux-datorer](../../linux/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="eefd5-106">This article is also available for [Linux virtual machines](../../linux/classic/setup-endpoints.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eefd5-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="eefd5-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="eefd5-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="eefd5-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="eefd5-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="eefd5-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="eefd5-110">I hello **Resource Manager** distributionsmodell, slutpunkter konfigureras med hjälp av **Nätverkssäkerhetsgrupper (NSG: er)**.</span><span class="sxs-lookup"><span data-stu-id="eefd5-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="eefd5-111">Mer information finns i [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="eefd5-111">For more information, see [Allow external access tooyour VM using hello Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="eefd5-112">När du skapar en virtuell Windows-dator i hello Azure-portalen skapas vanliga slutpunkter som de för fjärrskrivbord och Windows PowerShell-fjärrkommunikation vanligtvis automatiskt.</span><span class="sxs-lookup"><span data-stu-id="eefd5-112">When you create a Windows virtual machine in hello Azure portal, common endpoints like those for Remote Desktop and Windows PowerShell Remoting are typically created for you automatically.</span></span> <span data-ttu-id="eefd5-113">Du kan konfigurera ytterligare slutpunkter när du skapar virtuella hello eller senare efter behov.</span><span class="sxs-lookup"><span data-stu-id="eefd5-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="eefd5-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eefd5-114">Next steps</span></span>
* <span data-ttu-id="eefd5-115">toouse en Azure PowerShell-cmdlet tooset in en VM-slutpunkt finns [Lägg till AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="eefd5-115">toouse an Azure PowerShell cmdlet tooset up a VM endpoint, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span></span>
* <span data-ttu-id="eefd5-116">toouse toomanage en Azure PowerShell-cmdlet: en ACL för en slutpunkt finns [hantera åtkomstkontrollistor (ACL) för slutpunkter med hjälp av PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="eefd5-116">toouse an Azure PowerShell cmdlet toomanage an ACL on an endpoint, see [Managing access control lists (ACLs) for endpoints by using PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).</span></span>
* <span data-ttu-id="eefd5-117">Om du har skapat en virtuell dator i hello Resource Manager-distributionsmodellen kan du använda Azure PowerShell för[skapa nätverk säkerhetsgrupper](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol trafik toohello VM.</span><span class="sxs-lookup"><span data-stu-id="eefd5-117">If you created a virtual machine in hello Resource Manager deployment model, you can use Azure PowerShell too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol traffic toohello VM.</span></span>
