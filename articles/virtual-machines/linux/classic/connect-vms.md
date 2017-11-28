---
title: "aaaConnect virtuella Linux-datorer i en molntjänst | Microsoft Docs"
description: "Ansluta Linux virtuella datorer som skapats med hello klassisk distribution modellen tooan Azure-molntjänst eller ett virtuellt nätverk."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 323baf04390d53ffb2810e24a24d175c08e60cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-linux-virtual-machines-created-with-hello-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a><span data-ttu-id="d7c54-103">Ansluta Linux virtuella datorer som skapats med hello klassiska distributionsmodellen med virtuella nätverk eller moln-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d7c54-103">Connect Linux virtual machines created with hello classic deployment model with a virtual network or cloud service</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d7c54-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d7c54-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d7c54-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d7c54-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d7c54-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d7c54-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="d7c54-107">Linux virtuella datorer som skapats med hello klassiska distributionsmodellen placeras alltid i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="d7c54-107">Linux virtual machines created with hello classic deployment model are always placed in a cloud service.</span></span> <span data-ttu-id="d7c54-108">hello Molntjänsten fungerar som en behållare och ger ett unikt offentliga DNS-namn, en offentlig IP-adress och en uppsättning slutpunkter tooaccess hello virtuella datorn över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d7c54-108">hello cloud service acts as a container and provides a unique public DNS name, a public IP address, and a set of endpoints tooaccess hello virtual machine over hello Internet.</span></span> <span data-ttu-id="d7c54-109">Hej Molntjänsten kan vara i ett virtuellt nätverk, men som är inte ett krav.</span><span class="sxs-lookup"><span data-stu-id="d7c54-109">hello cloud service can be in a virtual network, but that's not a requirement.</span></span> <span data-ttu-id="d7c54-110">Du kan också [ansluta virtuella Windows-datorer med virtuella nätverk eller molnet](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7c54-110">You can also [connect Windows virtual machines with a virtual network or cloud service](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="d7c54-111">Om en molnbaserad tjänst som inte är i ett virtuellt nätverk, kallas en *fristående* Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d7c54-111">If a cloud service isn't in a virtual network, it's called a *standalone* cloud service.</span></span> <span data-ttu-id="d7c54-112">hello virtuella datorer i en fristående molntjänst kommunicerar med andra virtuella datorer med hjälp av hello offentliga DNS-namn för andra virtuella datorer och hello trafik färdas över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d7c54-112">hello virtual machines in a standalone cloud service communicate with other virtual machines by using hello other virtual machines’ public DNS names, and hello traffic travels over hello Internet.</span></span> <span data-ttu-id="d7c54-113">Om en tjänst i molnet är i ett virtuellt nätverk hello virtuella datorer i den molntjänst som kan kommunicera med alla andra virtuella datorer i hello virtuellt nätverk utan att skicka all trafik via hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d7c54-113">If a cloud service is in a virtual network, hello virtual machines in that cloud service can communicate with all other virtual machines in hello virtual network without sending any traffic over hello Internet.</span></span>

<span data-ttu-id="d7c54-114">Om du placerar dina virtuella datorer i hello samma fristående molnbaserad tjänst, du kan fortfarande använda belastningsutjämning och tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="d7c54-114">If you place your virtual machines in hello same standalone cloud service, you can still use load balancing and availability sets.</span></span> <span data-ttu-id="d7c54-115">Mer information finns i [virtuella datorer för belastningsutjämning](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [hantera hello tillgängligheten för virtuella datorer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7c54-115">For details, see [Load balancing virtual machines](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Manage hello availability of virtual machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="d7c54-116">Du kan inte ordna hello virtuella datorer i undernät eller ansluta en fristående cloud service tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d7c54-116">However, you can't organize hello virtual machines on subnets or connect a standalone cloud service tooyour on-premises network.</span></span> <span data-ttu-id="d7c54-117">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="d7c54-117">Here's an example:</span></span>

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a><span data-ttu-id="d7c54-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7c54-118">Next steps</span></span>
<span data-ttu-id="d7c54-119">När du skapar en virtuell dator är det en bra idé för[lägger till en datadisk](attach-disk.md) så att dina tjänster och arbetsbelastningar har en plats toostore data.</span><span class="sxs-lookup"><span data-stu-id="d7c54-119">After you create a virtual machine, it's a good idea too[add a data disk](attach-disk.md) so your services and workloads have a location toostore data.</span></span>
