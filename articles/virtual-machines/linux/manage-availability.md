---
title: "Hantera tillgängligheten för virtuella Linux-datorer i Azure | Microsoft Docs"
description: "Lär dig hur du använder flera virtuella datorer för att säkerställa hög tillgänglighet för Linux-program i Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 891c852a-84c0-4940-a61e-ada6e185bf37
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f153a740e4814e2573e53b9c051d24c30ff9088f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-the-availability-of-linux-virtual-machines"></a><span data-ttu-id="2ab2e-103">Hantera tillgängligheten för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="2ab2e-103">Manage the availability of Linux virtual machines</span></span>

<span data-ttu-id="2ab2e-104">Lära dig mer om att konfigurera och hantera flera virtuella datorer för att säkerställa hög tillgänglighet för Linux-program i Azure.</span><span class="sxs-lookup"><span data-stu-id="2ab2e-104">Learn ways to set up and manage multiple virtual machines to ensure high availability for your Linux application in Azure.</span></span> <span data-ttu-id="2ab2e-105">Du kan också [hantera tillgängligheten för virtuella Windows-datorer](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ab2e-105">You can also [manage the availability of Windows virtual machines](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="2ab2e-106">Anvisningar om hur du skapar en tillgänglighetsuppsättning med hjälp av CLI i Resource Manager-distributionsmodellen finns [azure availset: kommandon för att hantera din tillgänglighetsuppsättningar](../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets).</span><span class="sxs-lookup"><span data-stu-id="2ab2e-106">For instructions on creating an availability set using CLI in the Resource Manager deployment model, see [azure availset: commands to manage your availability sets](../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets).</span></span>

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a><span data-ttu-id="2ab2e-107">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ab2e-107">Next steps</span></span>
<span data-ttu-id="2ab2e-108">Läs mer om virtuella datorer för belastningsutjämning i [nätverksbelastning virtuella datorer](../virtual-machines-linux-load-balance.md).</span><span class="sxs-lookup"><span data-stu-id="2ab2e-108">To learn more about load balancing your virtual machines, see [Load Balancing virtual machines](../virtual-machines-linux-load-balance.md).</span></span>

