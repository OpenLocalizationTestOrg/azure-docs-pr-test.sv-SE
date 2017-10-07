---
title: aaaAzure virtuella PowerShell-exempel | Microsoft Docs
description: Virtuell dator i Azure PowerShell-exempel
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 89fd26aa979687cdcdf9ae4e60be7d0d2eaeae91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure Virtual Machine PowerShell-exempel

hello innehåller följande tabell länkar tooPowerShell skript exempel som att skapa och hantera Windows-datorer.

| | |
|---|---|
|**Skapa virtuella datorer**||
| [Skapa en helt konfigurerade virtuell dator](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en resursgrupp, virtuell dator och alla relaterade resurser.|
| [Skapa virtuella datorer med hög tillgänglighet](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning.|
| [Skapa en virtuell dator och kör konfigurationsskript](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en virtuell dator och använder hello Azure anpassat skript tillägget tooinstall IIS. |
| [Skapa en virtuell dator och kör DSC-konfiguration](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en virtuell dator och använder hello Azure önskad tillstånd Configuration DSC ()-tillägget tooinstall IIS. |
| [Överför en virtuell Hårddisk och skapa virtuella datorer](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Uplaods en lokal VHD filen tooAzure, skapar och bild av hello VHD och skapar sedan en virtuell dator från den avbildningen. |
| [Skapa en virtuell dator från en hanterad OS-disk](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en virtuell dator genom att koppla en befintlig hanteras Disk som OS-disken. |
| [Skapa en virtuell dator från en ögonblicksbild](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en virtuell dator från en ögonblicksbild genom att först skapa en hanterad disk från en ögonblicksbild och bifoga hello nya hanterade disken som OS-disken. |
|**Hantera lagring**||
| [Skapa hanterade diskar från en virtuell Hårddisk i samma eller olika prenumeration](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en hanterad disk från en särskild virtuell Hårddisk som en OS-disk eller från en virtuell Hårddisk som datadisk i samma eller en annan prenumeration.  |
| [Skapa en hanterad disk från en ögonblicksbild](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en hanterad disk från en ögonblicksbild. |
| [Kopiera toosame för hanterade diskar eller annan prenumeration](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopior hanteras toosame eller annan prenumeration men hello i samma region som hello överordnade hanterade disken. 
| [Exportera en ögonblicksbild som VHD tooa storage-konto](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Exporterar en hanterad ögonblicksbild som VHD tooa storage-konto i en annan region. |
| [Skapa en ögonblicksbild från en virtuell Hårddisk](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar från en ögonblicksbild på kort tid.  |
| [Kopiera ögonblicksbilden toosame eller annan prenumeration](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kopior ögonblicksbild toosame eller en annan prenumeration men i hello samma region som hello överordnade ögonblicksbild. |
|**Skydda virtuella datorer**||
| [Kryptera en virtuell dator- och datadiskar](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Skapar en Azure Key Vault, krypteringsnyckeln och tjänstens huvudnamn och krypterar en virtuell dator. |
|**Övervaka virtuella datorer**||
| [Övervaka en virtuell dator med Operations Management Suite](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Skapar en virtuell dator, installerar hello Operations Management Suite-agenten och registrerar hello VM i en OMS-arbetsyta.  |
| | |

