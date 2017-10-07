---
title: "aaaVM startar om eller ändrar storlek på frågor | Microsoft Docs"
description: "Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/13/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 3d00ba17d9558941a37a29034604cb15e0803e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure
> [!div class="op_single_selector"]
> * [Klassisk](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

När du försöker toostart en stoppad Azure virtuell dator (VM) eller ändra storlek på en befintlig virtuell Azure-dator, är hello vanliga fel du stöter på ett Allokeringsfel. Det här felet uppstår när hello kluster eller den region antingen inte har resurser som är tillgängliga eller inte support hello begärda VM-storlek.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Samla in granskningsloggar
toostart felsökning, samla in hello audit loggar tooidentify hello fel som är associerade med hello problemet.

I hello Azure-portalen klickar du på **Bläddra** > **virtuella datorer** > *din Windows-dator*  >   **Inställningar för** > **granskningsloggar**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fel när du startar en stoppad virtuell dator
Du försöker toostart en stoppad virtuell dator men får ett Allokeringsfel.

### <a name="cause"></a>Orsak
hello begäran toostart hello stoppats VM har toobe med hello ursprungliga klustret som är värd för hello-Molntjänsten. Hello-klustret har dock inte ledigt utrymme tillgängligt toofulfill hello begäran.

### <a name="resolution"></a>Lösning
* Skapa en ny molntjänst och associera den med antingen en region eller en region-baserat virtuellt nätverk, men inte en tillhörighetsgrupp.
* Ta bort hello stoppad virtuell dator.
* Återskapa hello VM i hello ny molntjänst med hjälp av hello diskar.
* Starta hello återskapas VM.

Om du får ett felmeddelande när du försöker toocreate en ny molntjänst, försök igen senare eller ändra hello region för hello-Molntjänsten.

> [!IMPORTANT]
> hello ny molntjänst får ett nytt namn och VIP, så behöver du toochange informationen för alla hello beroenden som använder denna information för hello befintlig molntjänst.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator
Du försöker tooresize en befintlig virtuell dator men får ett Allokeringsfel.

### <a name="cause"></a>Orsak
hello begäran tooresize hello VM har toobe försökt på hello ursprungliga klustret värdar hello Molntjänsten. Dock hello kluster stöder inte hello begärda VM-storlek.

### <a name="resolution"></a>Lösning
Minska hello begärda VM-storlek och försök igen hello storlek på begäran.

* Klicka på **Bläddra igenom alla** > **virtuella datorer (klassisk)** > *din virtuella dator* > **inställningar** > **storlek**. Detaljerade anvisningar finns i [ändra storlek på virtuella hello](https://msdn.microsoft.com/library/dn168976.aspx).

Om det inte är möjligt tooreduce hello VM-storlek, gör du följande:

* Skapa en ny molntjänst säkerställer den inte är länkade tooan tillhörighetsgrupp och inte är kopplad till ett virtuellt nätverk som är länkade tooan tillhörighetsgrupp.
* Skapa en ny, större storlek virtuell dator i den.

Du kan konsolidera dina virtuella datorer i hello samma molntjänst. Om en befintlig molntjänst är associerad med en region-baserat virtuellt nätverk, kan du ansluta hello nya cloud service toohello befintligt virtuellt nätverk.

Om hello befintlig molntjänst inte är associerad med en region-baserat virtuellt nätverk, sedan du har toodelete hello virtuella datorer i hello befintlig molntjänst och återskapa dem i hello ny molntjänst från deras diskar. Det är dock viktigt tooremember att hello ny molntjänst får ett nytt namn och VIP, så behöver du tooupdate dessa för alla hello beroenden som för närvarande använder den här informationen för hello befintlig molntjänst.

## <a name="next-steps"></a>Nästa steg
Om du får problem när du skapar en virtuell Windows-dator i Azure, se [felsöka distributionsproblem med att skapa en virtuell Windows-dator i Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

