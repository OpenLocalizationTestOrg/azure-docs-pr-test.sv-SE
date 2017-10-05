---
title: "Hämta en Linux-VHD från Azure | Microsoft Docs"
description: "Hämta en Linux VHD med Azure CLI och Azure portal."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 3eb88478b43f8e3a36ae04bf3703f238e8cb1f3e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="0e36b-103">Hämta en Linux-VHD från Azure</span><span class="sxs-lookup"><span data-stu-id="0e36b-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="0e36b-104">I den här artikeln får du lära dig hur du hämtar en [Linux virtuell hårddisk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) filen från Azure med hjälp av Azure CLI och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e36b-104">In this article, you learn how to download a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using the Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="0e36b-105">Virtuella datorer (VM) i Azure används [diskar](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som en plats att lagra ett operativsystem, program och data.</span><span class="sxs-lookup"><span data-stu-id="0e36b-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="0e36b-106">Alla virtuella Azure-datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="0e36b-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="0e36b-107">Operativsystemdisken har skapats ursprungligen från en avbildning och både operativsystemdisken och image är virtuella hårddiskar som lagras i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0e36b-107">The operating system disk is initially created from an image, and both the operating system disk and the image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="0e36b-108">Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="0e36b-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="0e36b-109">Om du inte redan har gjort det installerar [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="0e36b-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-the-vm"></a><span data-ttu-id="0e36b-110">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="0e36b-110">Stop the VM</span></span>

<span data-ttu-id="0e36b-111">En virtuell Hårddisk kan inte hämtas från Azure om den är kopplad till en aktiv virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0e36b-111">A VHD can’t be downloaded from Azure if it's attached to a running VM.</span></span> <span data-ttu-id="0e36b-112">Du måste stoppa den virtuella datorn för att ladda ned en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="0e36b-112">You need to stop the VM to download a VHD.</span></span> <span data-ttu-id="0e36b-113">Om du vill använda en virtuell Hårddisk som en [bild](tutorial-custom-images.md) för att skapa andra virtuella datorer med nya diskar, måste du ta bort etableringen och generalisera operativsystemet finns i filen och stoppa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0e36b-113">If you want to use a VHD as an [image](tutorial-custom-images.md) to create other VMs with new disks, you need to deprovision and generalize the operating system contained in the file and stop the VM.</span></span> <span data-ttu-id="0e36b-114">Om du vill använda den virtuella Hårddisken som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, behöver du bara stoppa och ta bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0e36b-114">To use the VHD as a disk for a new instance of an existing VM or data disk, you only need to stop and deallocate the VM.</span></span>

<span data-ttu-id="0e36b-115">Om du vill använda den virtuella Hårddisken som en bild för att skapa andra virtuella datorer, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0e36b-115">To use the VHD as an image to create other VMs, complete these steps:</span></span>

1. <span data-ttu-id="0e36b-116">Använda SSH, kontonamnet och offentliga IP-adressen för den virtuella datorn för att ansluta till den och ta bort etableringen av den.</span><span class="sxs-lookup"><span data-stu-id="0e36b-116">Use SSH, the account name, and the public IP address of the VM to connect to it and deprovision it.</span></span> <span data-ttu-id="0e36b-117">Den + user-parameter tar också bort det senaste kontot för etablerad användare.</span><span class="sxs-lookup"><span data-stu-id="0e36b-117">The +user parameter also removes the last provisioned user account.</span></span> <span data-ttu-id="0e36b-118">Om du gräddning autentiseringsuppgifter i för att den virtuella datorn, lämnar ut detta + user-parameter.</span><span class="sxs-lookup"><span data-stu-id="0e36b-118">If you are baking account credentials in to the VM, leave out this +user parameter.</span></span> <span data-ttu-id="0e36b-119">I följande exempel tar bort det senaste kontot för etablerad användare:</span><span class="sxs-lookup"><span data-stu-id="0e36b-119">The following example removes the last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="0e36b-120">Logga in på ditt Azure-konto med [az inloggningen](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="0e36b-120">Sign in to your Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="0e36b-121">Stoppa och ta bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0e36b-121">Stop and deallocate the VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="0e36b-122">Generalisera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0e36b-122">Generalize the VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="0e36b-123">Om du vill använda den virtuella Hårddisken som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0e36b-123">To use the VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="0e36b-124">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0e36b-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="0e36b-125">Klicka på **Virtual Machines** på navmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e36b-125">On the Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="0e36b-126">Välj den virtuella datorn från listan.</span><span class="sxs-lookup"><span data-stu-id="0e36b-126">Select the VM from the list.</span></span>
4.  <span data-ttu-id="0e36b-127">På bladet för den virtuella datorn klickar du på **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="0e36b-127">On the blade for the VM, click **Stop**.</span></span>

    ![Stoppa VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="0e36b-129">Generera en SAS-URL</span><span class="sxs-lookup"><span data-stu-id="0e36b-129">Generate SAS URL</span></span>

<span data-ttu-id="0e36b-130">Om du vill hämta VHD-filen måste du generera en [signatur för delad åtkomst (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span><span class="sxs-lookup"><span data-stu-id="0e36b-130">To download the VHD file, you need to generate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="0e36b-131">När URL: en genereras tilldelas en förfallotid till URL: en.</span><span class="sxs-lookup"><span data-stu-id="0e36b-131">When the URL is generated, an expiration time is assigned to the URL.</span></span>

1.  <span data-ttu-id="0e36b-132">Klicka på menyn i bladet för den virtuella datorn, **diskar**.</span><span class="sxs-lookup"><span data-stu-id="0e36b-132">On the menu of the blade for the VM, click **Disks**.</span></span>
2.  <span data-ttu-id="0e36b-133">Välj disken som operativsystemet för den virtuella datorn och klicka sedan på **exportera**.</span><span class="sxs-lookup"><span data-stu-id="0e36b-133">Select the operating system disk for the VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="0e36b-134">Klicka på **generera URL**.</span><span class="sxs-lookup"><span data-stu-id="0e36b-134">Click **Generate URL**.</span></span>

    ![Generera en URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="0e36b-136">Hämta VHD</span><span class="sxs-lookup"><span data-stu-id="0e36b-136">Download VHD</span></span>

1.  <span data-ttu-id="0e36b-137">Klicka på Hämta VHD-filen under den URL som har genererats.</span><span class="sxs-lookup"><span data-stu-id="0e36b-137">Under the URL that was generated, click Download the VHD file.</span></span>

    ![Hämta VHD](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="0e36b-139">Du kan behöva klicka på **spara** i webbläsaren om du vill starta nedladdningen.</span><span class="sxs-lookup"><span data-stu-id="0e36b-139">You may need to click **Save** in the browser to start the download.</span></span> <span data-ttu-id="0e36b-140">Standardnamnet för VHD-filen är *abcd*.</span><span class="sxs-lookup"><span data-stu-id="0e36b-140">The default name for the VHD file is *abcd*.</span></span>

    ![Klicka på Spara i webbläsaren](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="0e36b-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e36b-142">Next steps</span></span>

- <span data-ttu-id="0e36b-143">Lär dig hur du [överför och skapa en Linux VM från anpassade disken med Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e36b-143">Learn how to [upload and create a Linux VM from custom disk with the Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="0e36b-144">[Hantera Azure-diskarna Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e36b-144">[Manage Azure disks the Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

