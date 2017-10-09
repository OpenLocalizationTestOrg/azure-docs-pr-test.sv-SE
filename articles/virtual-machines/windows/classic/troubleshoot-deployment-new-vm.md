---
title: aaaTroubleshoot Windows VM distribution-klassisk | Microsoft Docs
description: "Felsökning av problem med klassisk distribution när du skapar en ny Windows virtuell dator i Azure"
services: virtual-machines-windows
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f01d237-ba39-4c32-b72d-18f5f505d43a
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cjiang
ms.openlocfilehash: aa12cb013a18e0572fbef8b7ea69106dd47c1fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Felsöka klassisk distributionsproblem med att skapa en ny Windows virtuell dator i Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Hello Resource Manager version av den här artikeln finns [här](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Samla in granskningsloggar
toostart felsökning, samla in hello audit loggar tooidentify hello fel som är associerade med hello problemet.

I hello Azure-portalen klickar du på **Bläddra** > **virtuella datorer** > *din Windows-dator*  >   **Inställningar för** > **granskningsloggar**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** om hello OS är Windows generaliserad, och det är upp och/eller avbildas med hello generaliserad inställningen kommer det inte finns några fel. På liknande sätt, om hello OS är specialanpassat för Windows och den är upp och/eller avbildas med särskilda hello-inställningen och det inte finns några fel.

**Överför fel:**

**N<sup>1</sup>:** om hello OS är Windows generaliserad och det överförs som specialiserade, du får ett etablering timeout-fel med hello VM fastnat på hello OOBE-skärmen.

**N<sup>2</sup>:** om hello OS är specialanpassat för Windows och det överförs som generaliserad, får du en allokering felmeddelande med hello VM som fastnat på hello OOBE-skärmen eftersom hello nya virtuella datorn körs med hello ursprungsdatorn namn, användarnamn och lösenord.

**Lösning:**

tooresolve båda dessa fel överför Hej ursprungliga virtuell Hårddisk, tillgänglig lokalt, hello med samma inställning som för hello OS (generaliserad/specialiserade). tooupload som generaliserad, Kom ihåg toorun sysprep först. Se [skapa och ladda upp en Windows Server VHD-tooAzure](createupload-vhd.md) för mer information.

**Avbilda fel:**

**N<sup>3</sup>:** om hello OS är Windows generaliserad och avbildas som specialiserade, du får en allokering tidsgränsfel eftersom hello ursprungliga VM inte kan användas eftersom den har markerats som generaliserad.

**N<sup>4</sup>:** om hello OS är specialanpassat för Windows och avbildningen som generaliserad, får du en allokering felmeddelande eftersom hello nya virtuella datorn körs med hello ursprungliga datornamn, användarnamn och lösenord. Dessutom hello ursprungliga VM kan inte användas eftersom den har markerats som särskilda.

**Lösning:**

tooresolve båda dessa fel, ta bort hello nuvarande avbildningen från hello-portalen och [avbilda från hello aktuella virtuella hårddiskar](capture-image.md) hello med samma inställning som för hello OS (generaliserad/specialiserade).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Anpassad / galleriet / marketplace-avbildning; Allokeringsfel
Det här felet uppstår i situationer när hello nya VM-begäran skickas tooa kluster som antingen är inte tillgängligt utrymme tooaccommodate hello begäran eller inte kan stödja hello VM-storlek som begärs. Det är inte möjligt toomix annan serie för virtuella datorer i hello samma molntjänst. Om du vill toocreate en ny virtuell dator i en annan storlek än vad din molntjänst kan stödja misslyckas så hello beräknings-begäran.

Beroende på hello begränsningarna för hello molnbaserad tjänst som du använder toocreate Hej ny virtuell dator, som kan uppstå ett fel som orsakats av något av två situationer.

**Orsak 1:** hello Molntjänsten är fäst tooa specifikt kluster eller den länkade tooan tillhörighetsgrupp och därför Fäst tooa specifikt kluster avsiktligt. Nya beräkningsresurser begär i den tillhörighetsgruppen testas i samma kluster där hello befintliga resurser finns hello. Hello samma kluster får dock inte stöd för hello begärt VM-storlek eller har inte tillräckligt ledigt utrymme, vilket resulterar i ett Allokeringsfel. Detta är SANT om hello nya resurser skapas via en ny molntjänst eller en befintlig molntjänst.

**Lösning 1:**

* Skapa en ny molntjänst och koppla den till en region eller en region-baserat virtuellt nätverk.
* Skapa en ny virtuell dator i hello ny molntjänst.
  Om du får ett felmeddelande när du försöker toocreate en ny molntjänst försök vid ett senare tillfälle eller ändra hello region för hello-Molntjänsten.

> [!IMPORTANT]
> Om du skulle toocreate en ny virtuell dator i en befintlig molntjänst men det gick inte att och hade toocreate en ny molntjänst för den nya virtuella datorn, kan du välja tooconsolidate dina virtuella datorer i hello samma molntjänst. toodo så ta bort hello virtuella datorer i hello befintlig molntjänst och avbilda dem från sina diskar i hello ny molntjänst. Det är dock viktigt tooremember att hello ny molntjänst får ett nytt namn och VIP, så behöver du tooupdate dessa för alla hello beroenden som för närvarande använder den här informationen för hello befintlig molntjänst.
> 
> 

**Orsak 2:** hello molntjänst som är associerad med ett virtuellt nätverk som är länkade tooan tillhörighetsgrupp, så att den är fäst tooa specifikt kluster avsiktligt. Alla nya beräkning resurs begäranden i den tillhörighetsgruppen testas därför i samma kluster där hello befintliga resurser finns hello. Hello samma kluster får dock inte stöd för hello begärt VM-storlek eller har inte tillräckligt ledigt utrymme, vilket resulterar i ett Allokeringsfel. Detta är SANT om hello nya resurser skapas via en ny molntjänst eller en befintlig molntjänst.

**Lösning 2:**

* Skapa en ny regionalt virtuellt nätverk.
* Skapa hello ny virtuell dator i hello nytt virtuellt nätverk.
* [Ansluta befintliga virtuella nätverket](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) toohello nytt virtuellt nätverk. Läs mer [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Du kan också [migrera din tillhörighet-grupp-baserade virtuella nätverk tooa regionalt virtuellt nätverk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), och sedan skapa hello ny virtuell dator.

## <a name="next-steps"></a>Nästa steg
Om du får problem när du startar en stoppad virtuell Windows-dator eller ändra storlek på en befintlig Windows virtuell dator i Azure, se [felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).

