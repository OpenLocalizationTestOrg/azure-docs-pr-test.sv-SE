---
title: "aaaCreating en lokal avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Förstå och köra hello steg toocreate en lokala VM-avbildning och distribuera toohello Azure Marketplace för andra toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="8c992-103">Utveckla en lokal avbildning av virtuell dator för hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="8c992-103">Develop an on-premises virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="8c992-104">Vi rekommenderar starkt att du utveckla Azure virtuella hårddiskar (VHD) direkt i hello molnet genom att använda Remote Desktop Protocol.</span><span class="sxs-lookup"><span data-stu-id="8c992-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in hello cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="8c992-105">Om du måste det är dock möjligt toodownload en virtuell Hårddisk och utveckla genom att använda lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="8c992-105">However, if you must, it is possible toodownload a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="8c992-106">För lokal utveckling, måste du hämta hello operativsystemet VHD från hello skapade VM.</span><span class="sxs-lookup"><span data-stu-id="8c992-106">For on-premises development, you must download hello operating system VHD of hello created VM.</span></span> <span data-ttu-id="8c992-107">De här stegen skulle ske steget 3.3, ovan.</span><span class="sxs-lookup"><span data-stu-id="8c992-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="8c992-108">Hämta en VHD-avbildningen</span><span class="sxs-lookup"><span data-stu-id="8c992-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="8c992-109">Leta upp en blob-URL</span><span class="sxs-lookup"><span data-stu-id="8c992-109">Locate a blob URL</span></span>
<span data-ttu-id="8c992-110">I ordning toodownload hello VHD först lokalisera hello blob-URL för hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="8c992-110">In order toodownload hello VHD, first locate hello blob URL for hello operating system disk.</span></span>

<span data-ttu-id="8c992-111">Leta upp hello blob-URL från hello nya [Microsoft Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="8c992-111">Locate hello blob URL from hello new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="8c992-112">Gå för**Bläddra** > **VMs**, och välj sedan hello distribueras VM.</span><span class="sxs-lookup"><span data-stu-id="8c992-112">Go too**Browse** > **VMs**, and then select hello deployed VM.</span></span>
2. <span data-ttu-id="8c992-113">Under **konfigurera**väljer hello **diskar** panelen, vilket öppnar bladet för hello-diskar.</span><span class="sxs-lookup"><span data-stu-id="8c992-113">Under **Configure**, select hello **Disks** tile, which opens hello Disks blade.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="8c992-115">Välj hello **OS-disken**, vilket öppnar ett nytt blad som visar egenskaper för disk, inklusive hello VHD-plats.</span><span class="sxs-lookup"><span data-stu-id="8c992-115">Select hello **OS Disk**, which opens another blade that displays disk properties, including hello VHD location.</span></span>
4. <span data-ttu-id="8c992-116">Kopiera URL: en blob.</span><span class="sxs-lookup"><span data-stu-id="8c992-116">Copy this blob URL.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="8c992-118">Nu kan ta bort hello distribuerade virtuella datorn utan att ta bort hello stödjande diskar.</span><span class="sxs-lookup"><span data-stu-id="8c992-118">Now, delete hello deployed VM without deleting hello backing disks.</span></span> <span data-ttu-id="8c992-119">Du kan också avbryta hello VM i stället för att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="8c992-119">You can also stop hello VM instead of deleting it.</span></span> <span data-ttu-id="8c992-120">Hämta inte hello operativsystemet VHD när hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="8c992-120">Do not download hello operating system VHD when hello VM is running.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="8c992-122">Ladda ned en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="8c992-122">Download a VHD</span></span>
<span data-ttu-id="8c992-123">När du vet hello blob URL, du kan hämta hello VHD med hjälp av hello [Azure-portalen](http://manage.windowsazure.com/) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c992-123">After you know hello blob URL, you can download hello VHD by using hello [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="8c992-124">För närvarande hello den här guiden skapas finns hello funktioner toodownload en virtuell Hårddisk inte ännu i hello nya Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c992-124">At hello time of this guide’s creation, hello functionality toodownload a VHD is not yet present in hello new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="8c992-125">**Hämta hello operativsystemet VHD via hello aktuella [Azure-portalen](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="8c992-125">**Download hello operating system VHD via hello current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="8c992-126">Logga in toohello Azure-portalen om du inte har gjort det redan.</span><span class="sxs-lookup"><span data-stu-id="8c992-126">Sign in toohello Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="8c992-127">Klicka på hello **lagring** fliken.</span><span class="sxs-lookup"><span data-stu-id="8c992-127">Click hello **Storage** tab.</span></span>
3. <span data-ttu-id="8c992-128">Välj hello storage-konto i vilka hello VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="8c992-128">Select hello storage account within which hello VHD is stored.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="8c992-130">Detta visar lagringskontoegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="8c992-130">This displays storage account properties.</span></span> <span data-ttu-id="8c992-131">Välj hello **behållare** fliken.</span><span class="sxs-lookup"><span data-stu-id="8c992-131">Select hello **Containers** tab.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="8c992-133">Välj hello-behållare i vilken hello VHD lagras.</span><span class="sxs-lookup"><span data-stu-id="8c992-133">Select hello container in which hello VHD is stored.</span></span> <span data-ttu-id="8c992-134">När du skapade från hello portal lagras hello VHD i en VHD-behållare.</span><span class="sxs-lookup"><span data-stu-id="8c992-134">By default, when created from hello portal, hello VHD is stored in a vhds container.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="8c992-136">Välj rätt operativsystem för hello VHD genom att jämföra hello URL toohello något du har sparat.</span><span class="sxs-lookup"><span data-stu-id="8c992-136">Select hello correct operating system VHD by comparing hello URL toohello one you saved.</span></span>
7. <span data-ttu-id="8c992-137">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="8c992-137">Click **Download**.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="8c992-139">Hämta en virtuell Hårddisk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c992-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="8c992-140">Dessutom toousing hello Azure-portalen kan du använda hello [spara AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet toodownload hello operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="8c992-140">In addition toousing hello Azure portal, you can use hello [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet toodownload hello operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="8c992-141">Exempelvis spara AzureVhd-källa ”https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” - LocalFilePath ”C:\Users\Administrator\Desktop\baseimagevm.vhd” - StorageKey<String></span><span class="sxs-lookup"><span data-stu-id="8c992-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="8c992-142">**Spara-AzureVhd** har också en **NumberOfThreads** alternativ som kan vara används tooincrease parallellitet toomake hello bästa utnyttjande av bandbredd för hello nedladdning.</span><span class="sxs-lookup"><span data-stu-id="8c992-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used tooincrease parallelism toomake hello best use of available bandwidth for hello download.</span></span>
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a><span data-ttu-id="8c992-143">Överför virtuella hårddiskar tooan Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="8c992-143">Upload VHDs tooan Azure storage account</span></span>
<span data-ttu-id="8c992-144">Om du har förberett din virtuella hårddiskar lokala måste tooupload dem till en storage-konto i Azure.</span><span class="sxs-lookup"><span data-stu-id="8c992-144">If you prepared your VHDs on-premises, you need tooupload them into a storage account in Azure.</span></span> <span data-ttu-id="8c992-145">Det här steget sker efter att skapa den virtuella Hårddisken lokalt men innan du hämtar certifikat för VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="8c992-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="8c992-146">Skapa ett lagringskonto och en behållare</span><span class="sxs-lookup"><span data-stu-id="8c992-146">Create a storage account and container</span></span>
<span data-ttu-id="8c992-147">Vi rekommenderar att virtuella hårddiskar ska överföras till ett lagringskonto i en region i hello USA.</span><span class="sxs-lookup"><span data-stu-id="8c992-147">We recommend that VHDs be uploaded into a storage account in a region in hello United States.</span></span> <span data-ttu-id="8c992-148">Alla virtuella hårddiskar för en enskild SKU ska placeras i en enskild behållare inom ett enda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8c992-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="8c992-149">toocreate ett lagringskonto som du kan använda hello [Microsoft Azure-portalen](https://portal.azure.com/), PowerShell eller kommandoradsverktyget för hello Linux.</span><span class="sxs-lookup"><span data-stu-id="8c992-149">toocreate a storage account, you can use hello [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or hello Linux command-line tool.</span></span>  

<span data-ttu-id="8c992-150">**Skapa ett lagringskonto från hello Microsoft Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="8c992-150">**Create a storage account from hello Microsoft Azure portal**</span></span>

1. <span data-ttu-id="8c992-151">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="8c992-151">Click **New**.</span></span>
2. <span data-ttu-id="8c992-152">Välj **lagring**.</span><span class="sxs-lookup"><span data-stu-id="8c992-152">Select **Storage**.</span></span>
3. <span data-ttu-id="8c992-153">Fyll i hello lagringskontonamn och välj sedan en plats.</span><span class="sxs-lookup"><span data-stu-id="8c992-153">Fill in hello storage account name, and then select a location.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="8c992-155">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8c992-155">Click **Create**.</span></span>
5. <span data-ttu-id="8c992-156">hello-bladet för hello skapade lagringskontot ska vara öppen.</span><span class="sxs-lookup"><span data-stu-id="8c992-156">hello blade for hello created storage account should be open.</span></span> <span data-ttu-id="8c992-157">Om inte, Välj **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="8c992-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="8c992-158">Konto bladet på hello lagring, Välj hello lagringskonto skapas.</span><span class="sxs-lookup"><span data-stu-id="8c992-158">On hello Storage account blade, select hello storage account created.</span></span>
6. <span data-ttu-id="8c992-159">Välj **behållare**.</span><span class="sxs-lookup"><span data-stu-id="8c992-159">Select **Containers**.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="8c992-161">Hello behållare bladet välj **Lägg till**, och ange behörigheter för behållaren av namn och hello en behållare.</span><span class="sxs-lookup"><span data-stu-id="8c992-161">On hello Containers blade, select **Add**, and then enter a container name and hello container permissions.</span></span> <span data-ttu-id="8c992-162">Välj **privata** behörigheter för behållaren.</span><span class="sxs-lookup"><span data-stu-id="8c992-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="8c992-163">Vi rekommenderar att du skapar en behållare per att du planerar toopublish SKU.</span><span class="sxs-lookup"><span data-stu-id="8c992-163">We recommend that you create one container per SKU that you are planning toopublish.</span></span>
> 
> 

  ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="8c992-165">Skapa ett lagringskonto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c992-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="8c992-166">Med hjälp av PowerShell, skapa ett lagringskonto med hjälp av hello [ny AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8c992-166">Using PowerShell, create a storage account by using hello [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="8c992-167">Därefter kan du skapa en behållare i detta lagringskonto med hjälp av hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8c992-167">Then you can create a container within that storage account by using hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="8c992-168">Dessa kommandon förutsätter att hello aktuella kontexten för lagringskontot har redan angetts i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c992-168">Those commands assume that hello current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="8c992-169">Se för[konfigurera Azure PowerShell](marketplace-publishing-powershell-setup.md) för mer information om PowerShell-installationen.</span><span class="sxs-lookup"><span data-stu-id="8c992-169">Refer too[Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="8c992-170">Skapa ett lagringskonto med hjälp av kommandoradsverktyget hello för Mac- och Linux</span><span class="sxs-lookup"><span data-stu-id="8c992-170">Create a storage account by using hello command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="8c992-171">Från [Linux kommandoradsverktyget](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skapa ett lagringskonto på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="8c992-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="8c992-172">Skapa en behållare på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="8c992-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="8c992-173">Ladda upp en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="8c992-173">Upload a VHD</span></span>
<span data-ttu-id="8c992-174">När hello storage-konto och behållaren har skapats kan överföra du din förberedda virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="8c992-174">After hello storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="8c992-175">Du kan använda PowerShell, hello Linux-kommandoradsverktyget eller andra hanteringsverktyg för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8c992-175">You can use PowerShell, hello Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="8c992-176">Överföra en virtuell Hårddisk med PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c992-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="8c992-177">Använd hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8c992-177">Use hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="8c992-178">Överföra en virtuell Hårddisk med hjälp av kommandoradsverktyget hello för Mac- och Linux</span><span class="sxs-lookup"><span data-stu-id="8c992-178">Upload a VHD by using hello command-line tool for Mac and Linux</span></span>
<span data-ttu-id="8c992-179">Med hello [Linux kommandoradsverktyget](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), använder hello följande: azure vm-avbildning skapa <image name> --plats <Location of hello data center> --Operativsystemet Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="8c992-179">With hello [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use hello following: azure vm image create <image name> --location <Location of hello data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="8c992-180">Se även</span><span class="sxs-lookup"><span data-stu-id="8c992-180">See also</span></span>
* [<span data-ttu-id="8c992-181">Skapa en avbildning av virtuell dator för hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="8c992-181">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="8c992-182">Konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c992-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

