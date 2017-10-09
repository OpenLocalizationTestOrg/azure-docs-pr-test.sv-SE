---
title: "aaaAvailability anger för klassiska virtuella Windows-datorer | Microsoft Docs"
description: "Konfigurera en tillgänglighetsuppsättning för en ny eller befintlig Windows virtuell dator i hello klassiska distributionsmodellen med hjälp av hello Azure-portalen och Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c3b7fdec-fb59-4412-a4f4-f3a0b9c62e93
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
ms.openlocfilehash: bd652a0401a34b57447551204b8f50d106715e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-availability-set-for-windows-virtual-machines-in-hello-classic-deployment-model"></a><span data-ttu-id="895af-103">Hur tooconfigure en tillgänglighetsuppsättning för Windows-datorer i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="895af-103">How tooconfigure an availability set for Windows virtual machines in hello classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="895af-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="895af-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="895af-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="895af-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="895af-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="895af-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="895af-107">Du kan också [konfigurera tillgänglighetsmängder](../tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="895af-107">You can also [configure availability sets](../tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]

