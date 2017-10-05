---
title: "Konfigurera slutpunkter på en klassisk Linux VM | Microsoft Docs"
description: "Lär dig att konfigurera slutpunkter för en Linux-VM i den klassiska Azure-portalen för att tillåta kommunikation med en virtuell Linux-dator i Azure"
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
ms.openlocfilehash: 4fd8b847b0f60648d1661ce5a8667c641e616ed4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a><span data-ttu-id="d0155-103">Hur du ställer in slutpunkter på en Linux klassiska virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="d0155-103">How to set up endpoints on a Linux classic virtual machine in Azure</span></span>
<span data-ttu-id="d0155-104">Alla Linux virtuella datorer som du skapar i Azure med hjälp av den klassiska distributionsmodellen kan automatiskt kommunicera via en kanal för privat nätverk med andra virtuella datorer i samma molntjänst eller virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0155-104">All Linux virtual machines that you create in Azure using the classic deployment model can automatically communicate over a private network channel with other virtual machines in the same cloud service or virtual network.</span></span> <span data-ttu-id="d0155-105">Datorer på Internet eller andra virtuella nätverk måste dock slutpunkter för att dirigera inkommande nätverkstrafik till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d0155-105">However, computers on the Internet or other virtual networks require endpoints to direct the inbound network traffic to a virtual machine.</span></span> <span data-ttu-id="d0155-106">Den här artikeln är också tillgängligt för [virtuella Windows-datorer](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d0155-106">This article is also available for [Windows virtual machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0155-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d0155-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d0155-108">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d0155-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="d0155-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d0155-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="d0155-110">I den **Resource Manager** distributionsmodell, slutpunkter konfigureras med hjälp av **Nätverkssäkerhetsgrupper (NSG: er)**.</span><span class="sxs-lookup"><span data-stu-id="d0155-110">In the **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="d0155-111">Mer information finns i [öppna portar och slutpunkter](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d0155-111">For more information, see [Opening ports and endpoints](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d0155-112">När du skapar en virtuell Linux-dator i Azure-portalen kan skapas en slutpunkt för SSH (Secure Shell) vanligtvis automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d0155-112">When you create a Linux virtual machine in the Azure portal, an endpoint for Secure Shell (SSH) is typically created for you automatically.</span></span> <span data-ttu-id="d0155-113">Du kan konfigurera ytterligare slutpunkter när du skapar den virtuella datorn eller senare efter behov.</span><span class="sxs-lookup"><span data-stu-id="d0155-113">You can configure additional endpoints while creating the virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="d0155-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0155-114">Next steps</span></span>
* <span data-ttu-id="d0155-115">Du kan också skapa en VM-slutpunkt med hjälp av den [Azure-kommandoradsgränssnittet](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d0155-115">You can also create a VM endpoint by using the [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="d0155-116">Kör den **azure vm endpoint skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="d0155-116">Run the **azure vm endpoint create** command.</span></span>
* <span data-ttu-id="d0155-117">Om du har skapat en virtuell dator i Resource Manager-distributionsmodellen, du kan använda Azure CLI i Resource Manager-läge till [skapa nätverk säkerhetsgrupper](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) för trafiken till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d0155-117">If you created a virtual machine in the Resource Manager deployment model, you can use the Azure CLI in Resource Manager mode to [create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) to control traffic to the VM.</span></span>
