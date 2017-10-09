---
title: "aaaAzure skriptexempel PowerShell - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration | Microsoft Docs"
description: "Azure PowerShell skriptexempel - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 7a3565356f13cb93759dec7ef9d0357e04c3410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Kopiera ögonblicksbild av hanterade diskar i samma prenumeration eller annan prenumeration med PowerShell

Det här skriptet skapar en kopia av en ögonblicksbild i hello samma samma prenumeration eller en annan prenumeration. Använd det här skriptet toomove en ögonblicksbild toodifferent prenumeration för datalagring. Lagra ögonblicksbilder i annan prenumeration för att skydda mot oavsiktlig borttagning av ögonblicksbilder i prenumerationen huvudsakliga. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en ögonblicksbild i hello mål en prenumeration med hjälp av hello-Id för hello källa ögonblicksbild. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Skapar ögonblicksbild konfigurationen som används för att skapa en ögonblicksbild. Den omfattar hello resurs-Id för hello överordnade ögonblicksbild och plats som är samma som hello överordnade ögonblicksbild.  |
| [Ny AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Skapar en ögonblicksbild med hjälp av ögonblicksbilder konfiguration, namnet på ögonblicksbilder och resursgruppens namn som parametrar skickades. |


## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en ögonblicksbild](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
