---
title: "aaaDifferent sätt toocreate en Windows-dator i Azure | Microsoft Docs"
description: "Visar hello olika sätt toocreate Windows-dator med Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2928d4daa9b44c4d3a5083092a82c9a7f7c69fae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a><span data-ttu-id="91f7f-103">Olika sätt toocreate Windows-dator</span><span class="sxs-lookup"><span data-stu-id="91f7f-103">Different ways toocreate a Windows virtual machine</span></span>

<span data-ttu-id="91f7f-104">Azure erbjuder olika sätt toocreate en virtuell dator eftersom virtuella datorer som är lämpliga för olika användare och syften.</span><span class="sxs-lookup"><span data-stu-id="91f7f-104">Azure offers different ways toocreate a virtual machine because virtual machines are suited for different users and purposes.</span></span> <span data-ttu-id="91f7f-105">Det innebär att du behöver toomake vissa val om hello virtuella datorn och hur toocreate den.</span><span class="sxs-lookup"><span data-stu-id="91f7f-105">This means that you need toomake some choices about hello virtual machine and how toocreate it.</span></span> <span data-ttu-id="91f7f-106">Den här artikeln innehåller en sammanfattning av dessa alternativ och länkar tooinstructions.</span><span class="sxs-lookup"><span data-stu-id="91f7f-106">This article gives you a summary of these choices and links tooinstructions.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="91f7f-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="91f7f-107">Azure portal</span></span>
<span data-ttu-id="91f7f-108">Använda hello Azure-portalen är ett enkelt sätt tootry ut en virtuell dator, särskilt om du precis börjat med Azure.</span><span class="sxs-lookup"><span data-stu-id="91f7f-108">Using hello Azure portal is a simple way tootry out a virtual machine, especially if you're just starting out with Azure.</span></span> 

[<span data-ttu-id="91f7f-109">Skapa en virtuell dator som kör Windows med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="91f7f-109">Create a virtual machine running Windows using hello portal</span></span>](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a><span data-ttu-id="91f7f-110">Mall</span><span class="sxs-lookup"><span data-stu-id="91f7f-110">Template</span></span>
<span data-ttu-id="91f7f-111">Virtuella datorer kräver en kombination av resurser (till exempel en tillgänglighet uppsättningar och storage-konton).</span><span class="sxs-lookup"><span data-stu-id="91f7f-111">Virtual machines require a combination of resources (such as a availability sets and storage accounts).</span></span> <span data-ttu-id="91f7f-112">I stället för att distribuera och hantera varje resurs separat, kan du skapa en Azure Resource Manager-mall som distribuerar och etablerar alla hello resurser i en enda, samordnad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="91f7f-112">Rather than deploying and managing each resource separately, you can create an Azure Resource Manager template that deploys and provisions all of hello resources in a single, coordinated operation.</span></span>

* [<span data-ttu-id="91f7f-113">Skapa en virtuell Windows-dator med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="91f7f-113">Create a Windows virtual machine with a Resource Manager template</span></span>](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a><span data-ttu-id="91f7f-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="91f7f-114">Azure PowerShell</span></span>
<span data-ttu-id="91f7f-115">Om du föredrar att arbeta i en kommandotolk, kan du använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91f7f-115">If you prefer working in a command shell, you can use Azure PowerShell.</span></span>

* [<span data-ttu-id="91f7f-116">Skapa en virtuell Windows-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="91f7f-116">Create a Windows VM using PowerShell</span></span>](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a><span data-ttu-id="91f7f-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91f7f-117">Visual Studio</span></span>
<span data-ttu-id="91f7f-118">Använd Visual Studio toobuild, hantera, och distribuera virtuella datorer med hello Azure verktyg för Visual Studio och hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="91f7f-118">Use Visual Studio toobuild, manage, and deploy VMs with hello Azure Tools for Visual Studio and hello Azure SDK.</span></span>

[<span data-ttu-id="91f7f-119">Azure-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91f7f-119">Azure Tools for Visual Studio</span></span>](https://www.visualstudio.com/features/azure-tools-vs)

