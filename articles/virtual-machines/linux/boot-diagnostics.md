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
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a><span data-ttu-id="29408-103">Hur toouse starta diagnostik tootroubleshoot Linux virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="29408-103">How toouse boot diagnostics tootroubleshoot Linux virtual machines in Azure</span></span>

<span data-ttu-id="29408-104">Nu finns stöd för två felsökningsfunktioner i Azure: konsolutdata och skärmbild för Azure Virtual Machines Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="29408-104">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="29408-105">När en egen avbildning tooAzure eller även starta en av hello plattform bilder, kan det finnas många orsaker till varför en virtuell dator hämtar till tillståndet ej startbar.</span><span class="sxs-lookup"><span data-stu-id="29408-105">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="29408-106">Dessa funktioner aktivera du tooeasily diagnostisera och återställa dina virtuella datorer från startfel.</span><span class="sxs-lookup"><span data-stu-id="29408-106">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="29408-107">För Linux virtuella datorer kan du lätt visa hello utdata från konsolen loggen från hello Portal:</span><span class="sxs-lookup"><span data-stu-id="29408-107">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="29408-109">Men för både Windows och Linux virtuella datorer kan Azure också toosee en skärmbild av hello VM från hello hypervisor-programmet:</span><span class="sxs-lookup"><span data-stu-id="29408-109">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Fel](./media/boot-diagnostics/screenshot2.png)

<span data-ttu-id="29408-111">De här båda funktionerna finns för Azure Virtual Machines i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="29408-111">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="29408-112">Obs skärmbilder och utdata kan ta upp too10 minuter tooappear i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="29408-112">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="29408-113">Vanliga startfel</span><span class="sxs-lookup"><span data-stu-id="29408-113">Common boot errors</span></span>

- [<span data-ttu-id="29408-114">Problem med filens system</span><span class="sxs-lookup"><span data-stu-id="29408-114">File system issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [<span data-ttu-id="29408-115">Kernel-problem</span><span class="sxs-lookup"><span data-stu-id="29408-115">Kernel Issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [<span data-ttu-id="29408-116">FSTAB fel</span><span class="sxs-lookup"><span data-stu-id="29408-116">FSTAB errors</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="29408-117">Aktivera diagnostik på en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="29408-117">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="29408-118">När du skapar en ny virtuell dator från hello Preview Portal väljer hello **Azure Resource Manager** hello distribution modellen listrutan:</span><span class="sxs-lookup"><span data-stu-id="29408-118">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="29408-120">Konfigurera hello övervakning alternativet tooselect hello storage-konto där du vill ha tooplace filerna diagnostik.</span><span class="sxs-lookup"><span data-stu-id="29408-120">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Skapa en virtuell dator](./media/boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="29408-122">Om du distribuerar en Azure Resource Manager-mall, navigera tooyour virtuella datorresurser och Lägg till hello diagnostik profil avsnitt.</span><span class="sxs-lookup"><span data-stu-id="29408-122">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="29408-123">Kom ihåg toouse hello ”2015-06-15” API version-huvud.</span><span class="sxs-lookup"><span data-stu-id="29408-123">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="29408-124">Hej diagnostikprofilen kan tooselect hello storage-konto där du vill att tooput dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="29408-124">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

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

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="29408-125">Uppdatera en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="29408-125">Update an existing virtual machine</span></span>

<span data-ttu-id="29408-126">tooenable startdiagnostikinställningar hello-portalen, du kan också uppdatera en befintlig virtuell dator via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="29408-126">tooenable boot diagnostics through hello portal, you can also update an existing virtual machine through hello portal.</span></span> <span data-ttu-id="29408-127">Välj hello Startdiagnostikinställningar alternativet och spara.</span><span class="sxs-lookup"><span data-stu-id="29408-127">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="29408-128">Starta om hello VM tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="29408-128">Restart hello VM tootake effect.</span></span>

![Uppdatera befintlig virtuell dator](./media/boot-diagnostics/screenshot5.png)