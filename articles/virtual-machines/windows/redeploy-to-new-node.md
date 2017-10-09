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
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a><span data-ttu-id="984db-103">Distribuera Windows virtuella toonew Azure nod</span><span class="sxs-lookup"><span data-stu-id="984db-103">Redeploy Windows virtual machine toonew Azure node</span></span>
<span data-ttu-id="984db-104">Om du har med svårigheter felsökning av RDP (Remote Desktop)-anslutning eller program åtkomst till tooWindows-baserad Azure virtuell dator (VM), omdistribuera hello VM kan hjälpa.</span><span class="sxs-lookup"><span data-stu-id="984db-104">If you have been facing difficulties troubleshooting Remote Desktop (RDP) connection or application access tooWindows-based Azure virtual machine (VM), redeploying hello VM may help.</span></span> <span data-ttu-id="984db-105">När du distribuerar en virtuell dator flyttas hello VM tooa ny nod i hello Azure-infrastrukturen och sedan aktiveras den tillbaka, behåller alla konfigurationsalternativ och associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="984db-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on, retaining all your configuration options and associated resources.</span></span> <span data-ttu-id="984db-106">Den här artikeln beskrivs hur du tooredeploy en virtuell dator med hjälp av Azure PowerShell eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="984db-106">This article shows you how tooredeploy a VM using Azure PowerShell or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="984db-107">När du distribuerar en virtuell dator hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="984db-107">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 


## <a name="using-azure-powershell"></a><span data-ttu-id="984db-108">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="984db-108">Using Azure PowerShell</span></span>
<span data-ttu-id="984db-109">Kontrollera att du har hello senaste Azure PowerShell 1.x installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="984db-109">Make sure you have hello latest Azure PowerShell 1.x installed on your machine.</span></span> <span data-ttu-id="984db-110">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="984db-110">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="984db-111">hello följande exempel distribuerar hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="984db-111">hello following example deploys hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="984db-112">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="984db-112">Next steps</span></span>
<span data-ttu-id="984db-113">Om du har problem med anslutning tooyour VM du hittar detaljerad hjälp på [felsökning av RDP-anslutningar](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [detaljerad RDP felsökningssteg](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="984db-113">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting RDP connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [detailed RDP troubleshooting steps](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="984db-114">Om du inte kommer åt ett program som körs på den virtuella datorn, du kan också läsa [felsökning av problem med programmet](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="984db-114">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

