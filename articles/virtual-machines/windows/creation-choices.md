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
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a>Olika sätt toocreate Windows-dator

Azure erbjuder olika sätt toocreate en virtuell dator eftersom virtuella datorer som är lämpliga för olika användare och syften. Det innebär att du behöver toomake vissa val om hello virtuella datorn och hur toocreate den. Den här artikeln innehåller en sammanfattning av dessa alternativ och länkar tooinstructions.

## <a name="azure-portal"></a>Azure Portal
Använda hello Azure-portalen är ett enkelt sätt tootry ut en virtuell dator, särskilt om du precis börjat med Azure. 

[Skapa en virtuell dator som kör Windows med hjälp av hello portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>Mall
Virtuella datorer kräver en kombination av resurser (till exempel en tillgänglighet uppsättningar och storage-konton). I stället för att distribuera och hantera varje resurs separat, kan du skapa en Azure Resource Manager-mall som distribuerar och etablerar alla hello resurser i en enda, samordnad åtgärd.

* [Skapa en virtuell Windows-dator med en Resource Manager-mall](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
Om du föredrar att arbeta i en kommandotolk, kan du använda Azure PowerShell.

* [Skapa en virtuell Windows-dator med PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
Använd Visual Studio toobuild, hantera, och distribuera virtuella datorer med hello Azure verktyg för Visual Studio och hello Azure SDK.

[Azure-verktyg för Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

