---
title: aaaTroubleshoot Linux VM distribution-RM | Microsoft Docs
description: "Felsökning av distributionsproblem med Resource Manager när du skapar en ny virtuell Linux-dator i Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 2dd7f1855bba75d86eb90f88e6d573cd42fd8d87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Felsökning av Resource Manager distributionsproblem med att skapa en ny virtuell Linux-dator i Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>De främsta problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Andra Virtuella distributionsproblem och frågor finns i [felsöka distribuera virtuella Linux-problem i Azure](troubleshoot-deploy-vm.md).
## <a name="collect-activity-logs"></a>Samla in aktiviteten loggar
toostart felsökning, samla in hello aktivitet loggar tooidentify hello fel som är associerade med hello problemet. hello innehåller följande länkar detaljerad information om hello processen toofollow.

[Visa distributionsåtgärder](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Visa aktiviteten loggar toomanage Azure resurser](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** om hello OS är Linux generaliserad, och det är upp och/eller avbildas med hello generaliserad inställningen kommer det inte finns några fel. På liknande sätt, om hello OS är specialanpassat för Linux och den är upp och/eller avbildas med särskilda hello-inställningen och det inte finns några fel.

**Överför fel:**

**N<sup>1</sup>:** om hello OS är Linux generaliserad och det överförs som specialiserade, du får en allokering tidsgränsfel eftersom hello VM har hello etablering steget.

**N<sup>2</sup>:** om hello OS är specialanpassat för Linux och det överförs som generaliserad, får du en allokering felmeddelande eftersom hello nya virtuella datorn körs med hello ursprungliga datornamn, användarnamn och lösenord.

**Lösning:**

tooresolve båda dessa fel överför Hej ursprungliga virtuell Hårddisk, tillgänglig lokalt, hello med samma inställning som för hello OS (generaliserad/specialiserade). tooupload som generaliserad, Kom ihåg toorun-ta bort etableringen först.

**Avbilda fel:**

**N<sup>3</sup>:** om hello OS är Linux generaliserad och avbildas som specialiserade, du får en allokering tidsgränsfel eftersom hello ursprungliga VM inte kan användas eftersom den har markerats som generaliserad.

**N<sup>4</sup>:** om hello OS är specialanpassat för Linux och avbildningen som generaliserad, får du en allokering felmeddelande eftersom hello nya virtuella datorn körs med hello ursprungliga datornamn, användarnamn och lösenord. Dessutom hello ursprungliga VM kan inte användas eftersom den har markerats som särskilda.

**Lösning:**

tooresolve båda dessa fel, ta bort hello nuvarande avbildningen från hello-portalen och [avbilda från hello aktuella virtuella hårddiskar](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hello med samma inställning som för hello OS (generaliserad/specialiserade).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Anpassad / galleriet / marketplace-avbildning; Allokeringsfel
Det här felet uppstår i situationer när hello ny VM-begäran är fäst tooa kluster som antingen har inte stöd för hello VM-storlek som begärs eller saknar lediga utrymmet tooaccommodate hello begäran.

**Orsak 1:** hello kluster stöder inte hello begärda VM-storlek.

**Lösning 1:**

* Försök hello förfrågan med en mindre VM-storlek.
* Om hello efterfrågade storleken hello VM inte kan ändras:
  * Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.
    Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.
  * När alla hello stoppa virtuella datorer, skapa hello ny virtuell dator i hello önskad storlek.
  * Starta hello nya virtuella datorn först och sedan väljer av hello stoppas virtuella datorer och klicka på **starta**.

**Orsak 2:** hello klustret har inte frigöra resurser.

**Lösning 2:**

* Försök igen med begäran hello vid ett senare tillfälle.
* Om hello ny virtuell dator kan vara en del av en annan tillgänglighetsuppsättning in
  * Skapa en ny virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region).
  * Lägg till hello nya VM toohello samma virtuella nätverk.

## <a name="next-steps"></a>Nästa steg
Om du får problem när du startar en stoppad Linux VM eller ändra storlek på en befintlig Linux VM i Azure, se [felsöka Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

