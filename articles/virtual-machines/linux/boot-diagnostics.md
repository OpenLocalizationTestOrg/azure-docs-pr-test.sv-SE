---
title: "aaaBoot diagnostik för Linux virtuella datorer i Azure | Microsoft-dokument"
description: "Översikt över hello två felsökningsfunktioner för Linux virtuella datorer i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: d355d512de09d2f1d7a2718e3db3fb99c9dd9e24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a>Hur toouse starta diagnostik tootroubleshoot Linux virtuella datorer i Azure

Nu finns stöd för två felsökningsfunktioner i Azure: konsolutdata och skärmbild för Azure Virtual Machines Resource Manager-distributionsmodellen. 

När en egen avbildning tooAzure eller även starta en av hello plattform bilder, kan det finnas många orsaker till varför en virtuell dator hämtar till tillståndet ej startbar. Dessa funktioner aktivera du tooeasily diagnostisera och återställa dina virtuella datorer från startfel.

För Linux virtuella datorer kan du lätt visa hello utdata från konsolen loggen från hello Portal:

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
Men för både Windows och Linux virtuella datorer kan Azure också toosee en skärmbild av hello VM från hello hypervisor-programmet:

![Fel](./media/boot-diagnostics/screenshot2.png)

De här båda funktionerna finns för Azure Virtual Machines i alla regioner. Obs skärmbilder och utdata kan ta upp too10 minuter tooappear i ditt lagringskonto.

## <a name="common-boot-errors"></a>Vanliga startfel

- [Problem med filens system](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Kernel-problem](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB fel](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivera diagnostik på en ny virtuell dator
1. När du skapar en ny virtuell dator från hello Preview Portal väljer hello **Azure Resource Manager** hello distribution modellen listrutan:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Konfigurera hello övervakning alternativet tooselect hello storage-konto där du vill ha tooplace filerna diagnostik.
 
    ![Skapa en virtuell dator](./media/boot-diagnostics/screenshot4.jpg)

3. Om du distribuerar en Azure Resource Manager-mall, navigera tooyour virtuella datorresurser och Lägg till hello diagnostik profil avsnitt. Kom ihåg toouse hello ”2015-06-15” API version-huvud.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Hej diagnostikprofilen kan tooselect hello storage-konto där du vill att tooput dessa loggar.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Uppdatera en befintlig virtuell dator

tooenable startdiagnostikinställningar hello-portalen, du kan också uppdatera en befintlig virtuell dator via hello-portalen. Välj hello Startdiagnostikinställningar alternativet och spara. Starta om hello VM tootake effekt.

![Uppdatera befintlig virtuell dator](./media/boot-diagnostics/screenshot5.png)