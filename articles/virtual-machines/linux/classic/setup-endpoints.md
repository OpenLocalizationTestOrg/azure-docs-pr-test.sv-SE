---
title: "aaaSet av slutpunkter på en klassisk Linux VM | Microsoft Docs"
description: "Läs tooset av slutpunkter för en Linux-VM i hello Azure klassiska portal tooallow kommunikation med en virtuell Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a><span data-ttu-id="df4e2-103">Hur tooset av slutpunkter på en Linux klassiska virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="df4e2-103">How tooset up endpoints on a Linux classic virtual machine in Azure</span></span>
<span data-ttu-id="df4e2-104">Alla Linux virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen kan automatiskt kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma cloud service eller virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="df4e2-104">All Linux virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="df4e2-105">Datorer på hello Internet eller andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="df4e2-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="df4e2-106">Den här artikeln är också tillgängligt för [virtuella Windows-datorer](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="df4e2-106">This article is also available for [Windows virtual machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df4e2-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="df4e2-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="df4e2-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="df4e2-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="df4e2-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="df4e2-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="df4e2-110">I hello **Resource Manager** distributionsmodell, slutpunkter konfigureras med hjälp av **Nätverkssäkerhetsgrupper (NSG: er)**.</span><span class="sxs-lookup"><span data-stu-id="df4e2-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="df4e2-111">Mer information finns i [öppna portar och slutpunkter](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="df4e2-111">For more information, see [Opening ports and endpoints](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="df4e2-112">När du skapar en virtuell Linux-dator i hello Azure-portalen skapas en slutpunkt för SSH (Secure Shell) vanligtvis automatiskt.</span><span class="sxs-lookup"><span data-stu-id="df4e2-112">When you create a Linux virtual machine in hello Azure portal, an endpoint for Secure Shell (SSH) is typically created for you automatically.</span></span> <span data-ttu-id="df4e2-113">Du kan konfigurera ytterligare slutpunkter när du skapar virtuella hello eller senare efter behov.</span><span class="sxs-lookup"><span data-stu-id="df4e2-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="df4e2-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df4e2-114">Next steps</span></span>
* <span data-ttu-id="df4e2-115">Du kan också skapa en VM-slutpunkt med hjälp av hello [Azure-kommandoradsgränssnittet](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="df4e2-115">You can also create a VM endpoint by using hello [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="df4e2-116">Kör hello **azure vm endpoint skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="df4e2-116">Run hello **azure vm endpoint create** command.</span></span>
* <span data-ttu-id="df4e2-117">Om du har skapat en virtuell dator i hello Resource Manager-distributionsmodellen kan du använda hello Azure CLI i Resource Manager-läget för[skapa nätverk säkerhetsgrupper](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol trafik toohello VM.</span><span class="sxs-lookup"><span data-stu-id="df4e2-117">If you created a virtual machine in hello Resource Manager deployment model, you can use hello Azure CLI in Resource Manager mode too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol traffic toohello VM.</span></span>
