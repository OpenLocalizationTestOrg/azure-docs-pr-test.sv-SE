---
title: aaaTroubleshoot distributionen av Windows VM i Azure | Microsoft Docs
description: "Felsökning av distributionsproblem med Resource Manager när du skapar en ny Windows virtuell dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Felsöka distributionsproblem när du skapar en ny Windows virtuell dator i Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>De främsta problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Andra Virtuella distributionsproblem och frågor finns i [felsöka distribution problem med Windows virtuell dator i Azure](troubleshoot-deploy-vm.md).

## <a name="collect-activity-logs"></a>Samla in aktiviteten loggar
toostart felsökning, samla in hello aktivitet loggar tooidentify hello fel som är associerade med hello problemet. hello innehåller följande länkar detaljerad information om hello processen toofollow.

[Visa distributionsåtgärder](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Visa aktiviteten loggar toomanage Azure resurser](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** om hello OS är Windows generaliserad, och det är upp och/eller avbildas med hello generaliserad inställningen kommer det inte finns några fel. På liknande sätt, om hello OS är specialanpassat för Windows och den är upp och/eller avbildas med särskilda hello-inställningen och det inte finns några fel.

**Överför fel:**

**N<sup>1</sup>:** om hello OS är Windows generaliserad och det överförs som specialiserade, du får ett etablering timeout-fel med hello VM fastnat på hello OOBE-skärmen.

**N<sup>2</sup>:** om hello OS är specialanpassat för Windows och det överförs som generaliserad, får du en allokering felmeddelande med hello VM som fastnat på hello OOBE-skärmen eftersom hello nya virtuella datorn körs med hello ursprungsdatorn namn, användarnamn och lösenord.

**Lösning**

tooresolve båda dessa fel, Använd [Lägg till AzureRmVhd tooupload hello ursprungliga VHD](https://msdn.microsoft.com/library/mt603554.aspx), tillgänglig lokalt, med hello samma inställning som för hello OS (generaliserad/specialiserade). tooupload som generaliserad, Kom ihåg toorun sysprep först.

**Avbilda fel:**

**N<sup>3</sup>:** om hello OS är Windows generaliserad och avbildas som specialiserade, du får en allokering tidsgränsfel eftersom hello ursprungliga VM inte kan användas eftersom den har markerats som generaliserad.

**N<sup>4</sup>:** om hello OS är specialanpassat för Windows och avbildningen som generaliserad, får du en allokering felmeddelande eftersom hello nya virtuella datorn körs med hello ursprungliga datornamn, användarnamn och lösenord. Dessutom hello ursprungliga VM kan inte användas eftersom den har markerats som särskilda.

**Lösning**

tooresolve båda dessa fel, ta bort hello nuvarande avbildningen från hello-portalen och [avbilda från hello aktuella virtuella hårddiskar](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello med samma inställning som för hello OS (generaliserad/specialiserade).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Problem: Marketplace-anpassad/galleriet bilden. Allokeringsfel
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
Om du får problem när du startar en stoppad virtuell Windows-dator eller ändra storlek på en befintlig Windows virtuell dator i Azure, se [felsöka Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

