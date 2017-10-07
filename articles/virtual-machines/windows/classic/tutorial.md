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
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a>Skapa en virtuell dator som kör Windows i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Klassisk distribution](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-distributionsmodellen](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) med hello **Azure-portalen**.

De här självstudierna visar hur toocreate en Azure virtuell dator (VM) som kör Windows i hello Azure-portalen. Vi använder en Windows Server-avbildning som exempel, men det är bara en av hello många bilder Azure erbjuder. Observera att avbildningsalternativ beror på din prenumeration. Windows desktop-avbildningar kan exempelvis vara tillgängliga tooMSDN prenumeranter.

Det här avsnittet beskrivs hur du toouse hello **instrumentpanelen** i hello Azure portal tooselect och sedan skapa hello virtuell dator.

Du kan också skapa virtuella datorer med [egna avbildningar](createupload-vhd.md). toolearn om denna och andra metoder, se [olika sätt toocreate Windows-dator](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <a id="createvirtualmachine"></a>Skapa hello virtuell dator
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[skapa en virtuell dator med hjälp av Resource Manager-modellen hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i hello Azure-portalen.
* Logga in toohello virtuella datorn. Instruktioner finns i [inloggning tooa virtuell dator som kör Windows Server](connect-logon.md).
* Koppla en toostore diskdata. Du kan koppla både tom och diskar som innehåller data. Instruktioner finns i hello [bifoga en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen](attach-disk.md).
