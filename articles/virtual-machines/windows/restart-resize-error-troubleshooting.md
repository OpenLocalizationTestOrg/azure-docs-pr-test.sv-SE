---
title: "aaaVM startar om eller ändrar storlek på problem i Azure | Microsoft Docs"
description: "Felsökning av Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2cf7c2d19bf5f79fab4ffc0eff9ccc1182d601c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Felsöka distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure
När du försöker toostart en stoppad Azure virtuell dator (VM) eller ändra storlek på en befintlig virtuell Azure-dator, är hello vanliga fel du stöter på ett Allokeringsfel. Det här felet uppstår när hello kluster eller den region antingen inte har resurser som är tillgängliga eller inte support hello begärda VM-storlek.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Samla in aktiviteten loggar
toostart felsökning, samla in hello aktivitet loggar tooidentify hello fel som är associerade med hello problemet. hello följande länkar innehåller detaljerad information om hello processen:

[Visa distributionsåtgärder](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Visa aktiviteten loggar toomanage Azure resurser](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fel när du startar en stoppad virtuell dator
Du försöker toostart en stoppad virtuell dator men får ett Allokeringsfel.

### <a name="cause"></a>Orsak
hello begäran toostart hello stoppats VM har toobe med hello ursprungliga klustret som är värd för hello-Molntjänsten. Hello-klustret har dock inte ledigt utrymme tillgängligt toofulfill hello begäran.

### <a name="resolution"></a>Lösning
* Stoppa alla hello virtuella datorer i hello tillgänglighet ange och starta sedan om varje virtuell dator.
  
  1. Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.
  2. När alla Hej stoppa virtuella datorer, Välj av hello stoppade virtuella datorer och klicka på Start.
* Försök igen med begäran för hello omstart vid ett senare tillfälle.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator
Du försöker tooresize en befintlig virtuell dator men får ett Allokeringsfel.

### <a name="cause"></a>Orsak
hello begäran tooresize hello VM har toobe försökt på hello ursprungliga klustret värdar hello Molntjänsten. Dock hello kluster stöder inte hello begärda VM-storlek.

### <a name="resolution"></a>Lösning
* Försök hello förfrågan med en mindre VM-storlek.
* Om hello efterfrågade storleken hello VM inte kan ändras:
  
  1. Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.
     
     * Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.
  2. När alla Hej stoppa virtuella datorer, ändra storlek på hello önskad VM tooa större storlek.
  3. Välj hello storlek VM och klicka på **starta**, och sedan början av hello stoppas virtuella datorer.

## <a name="next-steps"></a>Nästa steg
Om du får problem när du skapar en ny Windows virtuell dator i Azure, se [felsöka distributionsproblem med att skapa en ny Windows virtuell dator i Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

