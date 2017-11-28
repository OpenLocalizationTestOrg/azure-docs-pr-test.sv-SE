---
title: "aaaAvailability anger för klassiska virtuella Linux-datorer | Microsoft Docs"
description: "Konfigurera en tillgänglighetsuppsättning för en ny eller befintlig virtuell Linux-dator i hello klassiska distributionsmodellen med hjälp av hello Azure-portalen och Azure PowerShell."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b8624315-beca-4ec7-8441-2e98b166b548
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: cynthn
ms.openlocfilehash: 8d8d041e3540e42a1921f5665469a2fdcaa30a29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-availability-set-for-linux-virtual-machines-in-hello-classic-deployment-model"></a><span data-ttu-id="f89bc-103">Hur tooconfigure en tillgänglighetsuppsättning för Linux virtuella datorer i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="f89bc-103">How tooconfigure an availability set for Linux virtual machines in hello classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="f89bc-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f89bc-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f89bc-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f89bc-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="f89bc-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="f89bc-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f89bc-107">Du kan också [konfigurera tillgänglighetsmängder](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) i Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="f89bc-107">You can also [configure availability sets](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]