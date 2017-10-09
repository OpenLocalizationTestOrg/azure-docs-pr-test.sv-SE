---
title: aaaRedeploy Windows-datorer i Azure | Microsoft Docs
description: "Hur utfärdar tooredeploy Windows-datorer i Azure toomitigate RDP-anslutning."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 903d9d5bf241075931ee4b746690c553d808a58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a>Distribuera Windows virtuella toonew Azure nod
Om du har med svårigheter felsökning av RDP (Remote Desktop)-anslutning eller program åtkomst till tooWindows-baserad Azure virtuell dator (VM), omdistribuera hello VM kan hjälpa. När du distribuerar en virtuell dator flyttas hello VM tooa ny nod i hello Azure-infrastrukturen och sedan aktiveras den tillbaka, behåller alla konfigurationsalternativ och associerade resurser. Den här artikeln beskrivs hur du tooredeploy en virtuell dator med hjälp av Azure PowerShell eller hello Azure-portalen.

> [!NOTE]
> När du distribuerar en virtuell dator hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats. 


## <a name="using-azure-powershell"></a>Använda Azure PowerShell
Kontrollera att du har hello senaste Azure PowerShell 1.x installerat på datorn. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

hello följande exempel distribuerar hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM du hittar detaljerad hjälp på [felsökning av RDP-anslutningar](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [detaljerad RDP felsökningssteg](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Om du inte kommer åt ett program som körs på den virtuella datorn, du kan också läsa [felsökning av problem med programmet](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

