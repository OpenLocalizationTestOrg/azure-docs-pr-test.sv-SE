---
title: aaaInject data till virtuella Linux-datorer i Azure | Microsoft Docs
description: "Det här avsnittet beskrivs hur tooinject anpassade data till en virtuella Azure-datorn när hello instans skapas och hur toolocate hello anpassade data på Windows- eller Linux."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: d376093c-e01d-4ee3-a826-2b5a35caaa7e
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: a3197e06a8d367eab6336577e5cfb6d2d6858441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="injecting-custom-data-into-an-azure-virtual-machine"></a><span data-ttu-id="c3632-103">Placera anpassade data till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="c3632-103">Injecting custom data into an Azure virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="c3632-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c3632-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c3632-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c3632-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="c3632-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c3632-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="c3632-107">Information om hur du använder hello tillägget för anpassat skript med hello Resource Manager-modellen finns [här](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c3632-107">For information about using hello Custom Script Extension with hello Resource Manager model, see [here](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-inject-custom-data](../../../../includes/virtual-machines-common-classic-inject-custom-data.md)]

