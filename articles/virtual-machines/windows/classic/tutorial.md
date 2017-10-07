---
title: aaaCreate en virtuell dator i hello Azure-portalen | Microsoft Docs
description: Skapa en virtuell Windows-dator i hello Azure-portalen.
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
ms.openlocfilehash: 3848a3d06a7ce377633db78ea385826ed40f94fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a><span data-ttu-id="0c5ff-103">Skapa en virtuell dator som kör Windows i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0c5ff-103">Create a virtual machine running Windows in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c5ff-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0c5ff-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="0c5ff-105">PowerShell: Klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="0c5ff-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="0c5ff-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0c5ff-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0c5ff-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="0c5ff-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="0c5ff-109">Lär dig hur för[utföra dessa steg med hello Resource Manager-distributionsmodellen](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) med hello **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-109">Learn how too[perform these steps using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using hello **Azure portal**.</span></span>

<span data-ttu-id="0c5ff-110">De här självstudierna visar hur toocreate en Azure virtuell dator (VM) som kör Windows i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-110">This tutorial shows you how toocreate an Azure virtual machine (VM) running Windows in hello Azure portal.</span></span> <span data-ttu-id="0c5ff-111">Vi använder en Windows Server-avbildning som exempel, men det är bara en av hello många bilder Azure erbjuder.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-111">We'll use a Windows Server image as an example, but that's just one of hello many images Azure offers.</span></span> <span data-ttu-id="0c5ff-112">Observera att avbildningsalternativ beror på din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="0c5ff-113">Windows desktop-avbildningar kan exempelvis vara tillgängliga tooMSDN prenumeranter.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-113">For example, Windows desktop images may be available tooMSDN subscribers.</span></span>

<span data-ttu-id="0c5ff-114">Det här avsnittet beskrivs hur du toouse hello **instrumentpanelen** i hello Azure portal tooselect och sedan skapa hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-114">This section shows you how toouse hello **Dashboard** in hello Azure portal tooselect and then create hello virtual machine.</span></span>

<span data-ttu-id="0c5ff-115">Du kan också skapa virtuella datorer med [egna avbildningar](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="0c5ff-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="0c5ff-116">toolearn om denna och andra metoder, se [olika sätt toocreate Windows-dator](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c5ff-116">toolearn about this and other methods, see [Different ways toocreate a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <span data-ttu-id="0c5ff-117"><a id="createvirtualmachine"></a>Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0c5ff-117"><a id="createvirtualmachine"> </a>Create hello virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="0c5ff-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c5ff-118">Next steps</span></span>
* <span data-ttu-id="0c5ff-119">Lär dig hur för[skapa en virtuell dator med hjälp av Resource Manager-modellen hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-119">Learn how too[create a VM using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in hello Azure portal.</span></span>
* <span data-ttu-id="0c5ff-120">Logga in toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-120">Log on toohello virtual machine.</span></span> <span data-ttu-id="0c5ff-121">Instruktioner finns i [inloggning tooa virtuell dator som kör Windows Server](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="0c5ff-121">For instructions, see [Log on tooa virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="0c5ff-122">Koppla en toostore diskdata.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-122">Attach a disk toostore data.</span></span> <span data-ttu-id="0c5ff-123">Du kan koppla både tom och diskar som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="0c5ff-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="0c5ff-124">Instruktioner finns i hello [bifoga en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="0c5ff-124">For instructions, see hello [Attach a data disk tooa Windows virtual machine created with hello classic deployment model](attach-disk.md).</span></span>
