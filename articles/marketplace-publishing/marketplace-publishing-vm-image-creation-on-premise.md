---
title: "Skapa en avbildning av virtuell dator lokalt för Azure Marketplace | Microsoft Docs"
description: "Förstå och utföra stegen för att skapa en lokal VM-avbildning och distribuera på Azure Marketplace för andra att köpa."
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
ms.openlocfilehash: 8f6b9a9293dc149586e6e5fd55028170ea825b07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="cdd8a-103">Utveckla en avbildning av virtuell dator lokalt för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="cdd8a-103">Develop an on-premises virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="cdd8a-104">Vi rekommenderar starkt att du utveckla Azure virtuella hårddiskar (VHD) direkt i molnet genom att använda Remote Desktop Protocol.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in the cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="cdd8a-105">Om du måste går det att hämta en virtuell Hårddisk och utveckla genom att använda lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-105">However, if you must, it is possible to download a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="cdd8a-106">Du måste hämta operativsystemet VHD av skapade VM för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-106">For on-premises development, you must download the operating system VHD of the created VM.</span></span> <span data-ttu-id="cdd8a-107">De här stegen skulle ske steget 3.3, ovan.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="cdd8a-108">Hämta en VHD-avbildningen</span><span class="sxs-lookup"><span data-stu-id="cdd8a-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="cdd8a-109">Leta upp en blob-URL</span><span class="sxs-lookup"><span data-stu-id="cdd8a-109">Locate a blob URL</span></span>
<span data-ttu-id="cdd8a-110">För att kunna hämta den virtuella Hårddisken, att hitta blob-URL: en för operativsystemets disk.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-110">In order to download the VHD, first locate the blob URL for the operating system disk.</span></span>

<span data-ttu-id="cdd8a-111">Hitta blob-URL från den nya [Microsoft Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="cdd8a-111">Locate the blob URL from the new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="cdd8a-112">Gå till **Bläddra** > **VMs**, och välj sedan den distribuerade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-112">Go to **Browse** > **VMs**, and then select the deployed VM.</span></span>
2. <span data-ttu-id="cdd8a-113">Under **konfigurera**, Välj den **diskar** panelen, vilket öppnar bladet diskar.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-113">Under **Configure**, select the **Disks** tile, which opens the Disks blade.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="cdd8a-115">Välj den **OS-disken**, vilket öppnar ett annat blad som visar egenskaper för disk, inklusive VHD-plats.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-115">Select the **OS Disk**, which opens another blade that displays disk properties, including the VHD location.</span></span>
4. <span data-ttu-id="cdd8a-116">Kopiera URL: en blob.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-116">Copy this blob URL.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="cdd8a-118">Ta bort den distribuerade virtuella datorn utan att ta bort diskar för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-118">Now, delete the deployed VM without deleting the backing disks.</span></span> <span data-ttu-id="cdd8a-119">Du kan även stoppa den virtuella datorn i stället för att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-119">You can also stop the VM instead of deleting it.</span></span> <span data-ttu-id="cdd8a-120">Hämta inte operativsystemet VHD när den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-120">Do not download the operating system VHD when the VM is running.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="cdd8a-122">Ladda ned en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="cdd8a-122">Download a VHD</span></span>
<span data-ttu-id="cdd8a-123">Du känner till blob-Adressen, kan du hämta den virtuella Hårddisken med hjälp av den [Azure-portalen](http://manage.windowsazure.com/) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-123">After you know the blob URL, you can download the VHD by using the [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="cdd8a-124">När den här guiden skapas finns funktionaliteten för att ladda ned en virtuell Hårddisk inte ännu i den nya Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-124">At the time of this guide’s creation, the functionality to download a VHD is not yet present in the new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="cdd8a-125">**Ladda ned operativsystemet VHD via aktuellt [Azure-portalen](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="cdd8a-125">**Download the operating system VHD via the current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="cdd8a-126">Logga in på Azure portal om du inte har gjort det redan.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-126">Sign in to the Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="cdd8a-127">Klicka på den **lagring** fliken.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-127">Click the **Storage** tab.</span></span>
3. <span data-ttu-id="cdd8a-128">Välj lagringskonto för den virtuella Hårddisken lagras.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-128">Select the storage account within which the VHD is stored.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="cdd8a-130">Detta visar lagringskontoegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-130">This displays storage account properties.</span></span> <span data-ttu-id="cdd8a-131">Välj den **behållare** fliken.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-131">Select the **Containers** tab.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="cdd8a-133">Markera den behållare där den virtuella Hårddisken lagras.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-133">Select the container in which the VHD is stored.</span></span> <span data-ttu-id="cdd8a-134">När de skapas från portalen, lagras den virtuella Hårddisken i en VHD-behållare.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-134">By default, when created from the portal, the VHD is stored in a vhds container.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="cdd8a-136">Välj rätt operativsystem VHD genom att jämföra Webbadressen till den som du sparade.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-136">Select the correct operating system VHD by comparing the URL to the one you saved.</span></span>
7. <span data-ttu-id="cdd8a-137">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-137">Click **Download**.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="cdd8a-139">Hämta en virtuell Hårddisk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdd8a-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="cdd8a-140">Förutom att använda Azure-portalen kan du använda den [spara AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) för att ladda ned operativsystemet VHD.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-140">In addition to using the Azure portal, you can use the [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet to download the operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="cdd8a-141">Exempelvis spara AzureVhd-källa ”https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” - LocalFilePath ”C:\Users\Administrator\Desktop\baseimagevm.vhd” - StorageKey<String></span><span class="sxs-lookup"><span data-stu-id="cdd8a-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="cdd8a-142">**Spara-AzureVhd** har också en **NumberOfThreads** alternativ som kan användas för att öka parallellitet för att göra bästa användningen av bandbredd för hämtning.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used to increase parallelism to make the best use of available bandwidth for the download.</span></span>
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a><span data-ttu-id="cdd8a-143">Överför virtuella hårddiskar till ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="cdd8a-143">Upload VHDs to an Azure storage account</span></span>
<span data-ttu-id="cdd8a-144">Om du har förberett din virtuella hårddiskar lokalt måste du överföra dem till ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-144">If you prepared your VHDs on-premises, you need to upload them into a storage account in Azure.</span></span> <span data-ttu-id="cdd8a-145">Det här steget sker efter att skapa den virtuella Hårddisken lokalt men innan du hämtar certifikat för VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="cdd8a-146">Skapa ett lagringskonto och en behållare</span><span class="sxs-lookup"><span data-stu-id="cdd8a-146">Create a storage account and container</span></span>
<span data-ttu-id="cdd8a-147">Vi rekommenderar att virtuella hårddiskar ska överföras till ett lagringskonto i en region i USA.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-147">We recommend that VHDs be uploaded into a storage account in a region in the United States.</span></span> <span data-ttu-id="cdd8a-148">Alla virtuella hårddiskar för en enskild SKU ska placeras i en enskild behållare inom ett enda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="cdd8a-149">Om du vill skapa ett lagringskonto som du kan använda den [Microsoft Azure-portalen](https://portal.azure.com/), PowerShell eller Linux kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-149">To create a storage account, you can use the [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or the Linux command-line tool.</span></span>  

<span data-ttu-id="cdd8a-150">**Skapa ett lagringskonto från Microsoft Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="cdd8a-150">**Create a storage account from the Microsoft Azure portal**</span></span>

1. <span data-ttu-id="cdd8a-151">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-151">Click **New**.</span></span>
2. <span data-ttu-id="cdd8a-152">Välj **lagring**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-152">Select **Storage**.</span></span>
3. <span data-ttu-id="cdd8a-153">Fyll i lagringskontonamn och välj sedan en plats.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-153">Fill in the storage account name, and then select a location.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="cdd8a-155">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-155">Click **Create**.</span></span>
5. <span data-ttu-id="cdd8a-156">Bladet för det skapade lagringskontot ska vara öppen.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-156">The blade for the created storage account should be open.</span></span> <span data-ttu-id="cdd8a-157">Om inte, Välj **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="cdd8a-158">Välj lagringskonto som skapats i bladet Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-158">On the Storage account blade, select the storage account created.</span></span>
6. <span data-ttu-id="cdd8a-159">Välj **behållare**.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-159">Select **Containers**.</span></span>
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="cdd8a-161">På bladet behållare väljer **Lägg till**, och ange sedan ett behållarnamn och behörigheter för behållaren.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-161">On the Containers blade, select **Add**, and then enter a container name and the container permissions.</span></span> <span data-ttu-id="cdd8a-162">Välj **privata** behörigheter för behållaren.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="cdd8a-163">Vi rekommenderar att du skapar en behållare per SKU som du planerar att publicera.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-163">We recommend that you create one container per SKU that you are planning to publish.</span></span>
> 
> 

  ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="cdd8a-165">Skapa ett lagringskonto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdd8a-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="cdd8a-166">Med hjälp av PowerShell, skapa ett lagringskonto med hjälp av den [ny AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-166">Using PowerShell, create a storage account by using the [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="cdd8a-167">Därefter kan du skapa en behållare i detta lagringskonto med hjälp av den [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-167">Then you can create a container within that storage account by using the [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="cdd8a-168">Dessa kommandon förutsätter att den aktuella kontexten för lagringskontot har redan ställts in i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-168">Those commands assume that the current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="cdd8a-169">Referera till [konfigurera Azure PowerShell](marketplace-publishing-powershell-setup.md) för mer information om PowerShell-installationen.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-169">Refer to [Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="cdd8a-170">Skapa ett lagringskonto med hjälp av kommandoradsverktyget för Mac- och Linux</span><span class="sxs-lookup"><span data-stu-id="cdd8a-170">Create a storage account by using the command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="cdd8a-171">Från [Linux kommandoradsverktyget](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skapa ett lagringskonto på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="cdd8a-172">Skapa en behållare på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="cdd8a-173">Ladda upp en virtuell hårddisk</span><span class="sxs-lookup"><span data-stu-id="cdd8a-173">Upload a VHD</span></span>
<span data-ttu-id="cdd8a-174">När lagringskontot och behållaren har skapats kan överföra du din förberedda virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-174">After the storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="cdd8a-175">Du kan använda PowerShell, Linux-kommandoradsverktyget eller andra hanteringsverktyg för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-175">You can use PowerShell, the Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="cdd8a-176">Överföra en virtuell Hårddisk med PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdd8a-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="cdd8a-177">Använd den [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cdd8a-177">Use the [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="cdd8a-178">Överföra en virtuell Hårddisk med hjälp av kommandoradsverktyget för Mac- och Linux</span><span class="sxs-lookup"><span data-stu-id="cdd8a-178">Upload a VHD by using the command-line tool for Mac and Linux</span></span>
<span data-ttu-id="cdd8a-179">Med den [Linux kommandoradsverktyget](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), använder du följande: azure vm-avbildning skapa <image name> --plats <Location of the data center> --Operativsystemet Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="cdd8a-179">With the [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use the following: azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="cdd8a-180">Se även</span><span class="sxs-lookup"><span data-stu-id="cdd8a-180">See also</span></span>
* [<span data-ttu-id="cdd8a-181">Skapa en avbildning av virtuell dator på Marketplace</span><span class="sxs-lookup"><span data-stu-id="cdd8a-181">Creating a virtual machine image for the Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="cdd8a-182">Konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdd8a-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

