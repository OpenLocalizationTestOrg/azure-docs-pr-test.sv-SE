---
title: Skapa en virtuell dator i Azure portal | Microsoft Docs
description: Skapa en virtuell Windows-dator i Azure-portalen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a><span data-ttu-id="433c4-103">Skapa en virtuell dator som kör Windows i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="433c4-103">Create a virtual machine running Windows in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="433c4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="433c4-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="433c4-105">PowerShell: Klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="433c4-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="433c4-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="433c4-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="433c4-107">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="433c4-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="433c4-108">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="433c4-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="433c4-109">Lär dig hur du [utföra dessa steg med Resource Manager-distributionsmodellen](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) med hjälp av den **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="433c4-109">Learn how to [perform these steps using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using the **Azure portal**.</span></span>

<span data-ttu-id="433c4-110">Den här kursen visar hur du skapar en Azure virtuell dator (VM) med Windows Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="433c4-110">This tutorial shows you how to create an Azure virtual machine (VM) running Windows in the Azure portal.</span></span> <span data-ttu-id="433c4-111">Vi använder en Windows Server-avbildning som exempel, men det är bara en av de många avbildningar som Azure erbjuder.</span><span class="sxs-lookup"><span data-stu-id="433c4-111">We'll use a Windows Server image as an example, but that's just one of the many images Azure offers.</span></span> <span data-ttu-id="433c4-112">Observera att avbildningsalternativ beror på din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="433c4-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="433c4-113">Till exempel vara Windows desktop-avbildningar tillgängliga för MSDN-prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="433c4-113">For example, Windows desktop images may be available to MSDN subscribers.</span></span>

<span data-ttu-id="433c4-114">Det här avsnittet visar hur du använder den **instrumentpanelen** i Azure portal för att välja och skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="433c4-114">This section shows you how to use the **Dashboard** in the Azure portal to select and then create the virtual machine.</span></span>

<span data-ttu-id="433c4-115">Du kan också skapa virtuella datorer med [egna avbildningar](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="433c4-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="433c4-116">Läs om denna och andra metoder i [olika sätt att skapa en virtuell dator för Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="433c4-116">To learn about this and other methods, see [Different ways to create a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <span data-ttu-id="433c4-117"><a id="createvirtualmachine"></a>Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="433c4-117"><a id="createvirtualmachine"> </a>Create the virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="433c4-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="433c4-118">Next steps</span></span>
* <span data-ttu-id="433c4-119">Lär dig hur du [skapa en virtuell dator med hjälp av Resource Manager-distributionsmodellen](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="433c4-119">Learn how to [create a VM using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in the Azure portal.</span></span>
* <span data-ttu-id="433c4-120">Logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="433c4-120">Log on to the virtual machine.</span></span> <span data-ttu-id="433c4-121">Instruktioner finns i [logga in på en virtuell dator som kör Windows Server](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="433c4-121">For instructions, see [Log on to a virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="433c4-122">Ansluta en disk för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="433c4-122">Attach a disk to store data.</span></span> <span data-ttu-id="433c4-123">Du kan koppla både tom och diskar som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="433c4-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="433c4-124">Mer information finns i [ansluta en datadisk till en Windows-dator som skapats med den klassiska distributionsmodellen](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="433c4-124">For instructions, see the [Attach a data disk to a Windows virtual machine created with the classic deployment model](attach-disk.md).</span></span>
