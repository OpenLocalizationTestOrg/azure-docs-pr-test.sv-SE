---
title: Fel vid tilldelning av aaaTroubleshooting Windows VM | Microsoft Docs
description: "Felsök tilldelningsproblem när du skapar, startar om eller ändra storlek på en Windows-dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: cjiang
ms.openlocfilehash: d0cc75ac60d952d8e4310cebc37654dc4f80857f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a><span data-ttu-id="51546-103">Felsök tilldelningsproblem när du skapar, startar om eller ändra storlek på virtuella Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="51546-103">Troubleshoot allocation failures when you create, restart, or resize Windows VMs in Azure</span></span>
<span data-ttu-id="51546-104">När du skapar en virtuell dator, starta om stoppats (frigjorts) virtuella datorer eller ändra storlek på en virtuell dator, allokerar Microsoft Azure beräkningsresurser tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="51546-104">When you create a VM, restart stopped (deallocated) VMs, or resize a VM, Microsoft Azure allocates compute resources tooyour subscription.</span></span> <span data-ttu-id="51546-105">Ibland kan du få felmeddelanden när du utför dessa åtgärder--även innan du når hello Azure-prenumerationsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="51546-105">You may occasionally receive errors when performing these operations -- even before you reach hello Azure subscription limits.</span></span> <span data-ttu-id="51546-106">Den här artikeln förklarar hello orsakerna till vissa av hello vanliga Allokeringsfel och föreslår möjliga reparation.</span><span class="sxs-lookup"><span data-stu-id="51546-106">This article explains hello causes of some of hello common allocation failures and suggests possible remediation.</span></span> <span data-ttu-id="51546-107">hello informationen kan också vara användbar när du planerar distributionen av hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="51546-107">hello information may also be useful when you plan hello deployment of your services.</span></span> <span data-ttu-id="51546-108">Du kan också [Felsök tilldelningsproblem när du skapar, startar om eller ändra storlek på virtuella Linux-datorer i Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51546-108">You can also [troubleshoot allocation failures when you create, restart, or resize Linux VMs in Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

