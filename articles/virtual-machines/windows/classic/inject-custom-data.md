---
title: Mata in data i virtuella Windows-datorer i Azure | Microsoft Docs
description: "Det här avsnittet beskriver hur du mata in anpassade data i en virtuell Azure-dator när instansen skapas och hur du hittar anpassade data på Windows- eller Linux."
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 48759f76-eaa0-4202-ada0-706d3f9a9467
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 7836577f16940b618a2912012ba8a8e7160980e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="injecting-custom-data-into-an-azure-virtual-machine"></a><span data-ttu-id="1dd51-103">Placera anpassade data till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="1dd51-103">Injecting custom data into an Azure virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="1dd51-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1dd51-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1dd51-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="1dd51-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="1dd51-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="1dd51-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="1dd51-107">Information om hur du använder tillägget för anpassat skript med Resource Manager-modellen finns [här](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1dd51-107">For information about using the Custom Script Extension with the Resource Manager model, see [here](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-inject-custom-data](../../../../includes/virtual-machines-common-classic-inject-custom-data.md)]

