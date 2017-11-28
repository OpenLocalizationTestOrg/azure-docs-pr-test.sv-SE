---
title: "Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar | Microsoft Docs"
description: "Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 318d7146ea53a806baf813ff7de2fe91f18becc8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="94f30-103">Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="94f30-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="94f30-104">Du kan få fel vid försök att ta bort ett Azure storage-konto, en behållare eller en virtuell hårddisk (VHD) med den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94f30-104">You might receive errors when you try to delete an Azure storage account, container, or virtual hard disk (VHD) in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="94f30-105">Den här artikeln innehåller felsökningsinformation för att lösa problemet i en Azure Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="94f30-105">This article provides troubleshooting guidance to help resolve the problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="94f30-106">Om den här artikeln inte åtgärda problemet Azure, gå Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="94f30-106">If this article doesn't address your Azure problem, visit the Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="94f30-107">Du kan publicera ditt problem på dessa forum eller @AzureSupport på Twitter.</span><span class="sxs-lookup"><span data-stu-id="94f30-107">You can post your problem on these forums or to @AzureSupport on Twitter.</span></span> <span data-ttu-id="94f30-108">Du kan också filen en Azure-supportbegäran genom att välja **få support** på den [Azure-supporten](https://azure.microsoft.com/support/options/) plats.</span><span class="sxs-lookup"><span data-stu-id="94f30-108">Also, you can file an Azure support request by selecting **Get support** on the [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="94f30-109">Symtom</span><span class="sxs-lookup"><span data-stu-id="94f30-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="94f30-110">Scenario 1</span><span class="sxs-lookup"><span data-stu-id="94f30-110">Scenario 1</span></span>
<span data-ttu-id="94f30-111">När du försöker ta bort en virtuell Hårddisk i ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="94f30-111">When you try to delete a VHD in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="94f30-112">**Det gick inte att ta bort blobben 'vhds/BlobName.vhd'. Fel: Det finns för närvarande ett lån på blob och inga lease-ID har angetts i begäran.**</span><span class="sxs-lookup"><span data-stu-id="94f30-112">**Failed to delete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on the blob and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="94f30-113">Det här problemet kan uppstå eftersom en virtuell dator (VM) har ett lån på den virtuella Hårddisken som du försöker ta bort.</span><span class="sxs-lookup"><span data-stu-id="94f30-113">This problem can occur because a virtual machine (VM) has a lease on the VHD that you are trying to delete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="94f30-114">Scenario 2</span><span class="sxs-lookup"><span data-stu-id="94f30-114">Scenario 2</span></span>
<span data-ttu-id="94f30-115">När du försöker ta bort en behållare i ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="94f30-115">When you try to delete a container in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="94f30-116">**Det gick inte att ta bort lagring behållaren ”VHD”. Fel: Det finns för närvarande ett lån på behållaren och inga lease-ID har angetts i begäran.**</span><span class="sxs-lookup"><span data-stu-id="94f30-116">**Failed to delete storage container 'vhds'. Error: There is currently a lease on the container and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="94f30-117">Det här problemet kan uppstå eftersom behållaren har en virtuell Hårddisk som är låsta i lease-tillstånd.</span><span class="sxs-lookup"><span data-stu-id="94f30-117">This problem can occur because the container has a VHD that is locked in the lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="94f30-118">Scenario 3</span><span class="sxs-lookup"><span data-stu-id="94f30-118">Scenario 3</span></span>
<span data-ttu-id="94f30-119">När du försöker ta bort ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="94f30-119">When you try to delete a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="94f30-120">**Det gick inte att ta bort lagringskontot 'StorageAccountName'. Fel: Lagringskontot kan inte tas bort eftersom dess artefakter används.**</span><span class="sxs-lookup"><span data-stu-id="94f30-120">**Failed to delete storage account 'StorageAccountName'. Error: The storage account cannot be deleted due to its artifacts being in use.**</span></span>

<span data-ttu-id="94f30-121">Det här problemet kan inträffa eftersom lagringskontot innehåller en virtuell Hårddisk som är i tillståndet lån.</span><span class="sxs-lookup"><span data-stu-id="94f30-121">This problem can occur because the storage account contains a VHD that is in the lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="94f30-122">Lösning</span><span class="sxs-lookup"><span data-stu-id="94f30-122">Solution</span></span> 
<span data-ttu-id="94f30-123">Du måste identifiera den virtuella Hårddisken som orsakar felet och den associera virtuella datorn för att lösa dessa problem.</span><span class="sxs-lookup"><span data-stu-id="94f30-123">To resolve these problems, you must identify the VHD that is causing the error and the associated VM.</span></span> <span data-ttu-id="94f30-124">Sedan koppla från den virtuella Hårddisken från den virtuella datorn (för datadiskar) eller ta bort den virtuella datorn som använder den virtuella Hårddisken (för OS-diskar).</span><span class="sxs-lookup"><span data-stu-id="94f30-124">Then, detach the VHD from the VM (for data disks) or delete the VM that is using the VHD (for OS disks).</span></span> <span data-ttu-id="94f30-125">Detta tar bort lånet från den virtuella Hårddisken så att den kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="94f30-125">This removes the lease from the VHD and allows it to be deleted.</span></span> 

<span data-ttu-id="94f30-126">Om du vill göra detta använder du någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="94f30-126">To do this, use one of the following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="94f30-127">Metod 1 - Använd Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="94f30-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="94f30-128">Steg 1 identifiera den virtuella Hårddisken som förhindra borttagning av storage-konto</span><span class="sxs-lookup"><span data-stu-id="94f30-128">Step 1 Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="94f30-129">När du tar bort lagringskontot visas en dialogruta för meddelande, till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="94f30-129">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![meddelande när du tar bort lagringskontot](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="94f30-131">Kontrollera den **Disk URL** identifiera lagring konto och den virtuella Hårddisken som gör att du ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94f30-131">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="94f30-132">I följande exempel strängen innan ”. blob.core.windows.net” är lagringskontonamn och ”SCCM2012-2015-08-28.vhd” är virtuella Hårddiskens namn.</span><span class="sxs-lookup"><span data-stu-id="94f30-132">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="94f30-133">Steg 2 ta bort den virtuella Hårddisken med hjälp av Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="94f30-133">Step 2 Delete the VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="94f30-134">Hämta och installera den senaste versionen av [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="94f30-134">Download and Install the latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="94f30-135">Det här verktyget är en fristående app från Microsoft som hjälper dig att arbeta med Azure Storage-data med Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="94f30-135">This tool is a standalone app from Microsoft that allows you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="94f30-136">Öppna Azure Lagringsutforskaren, Välj</span><span class="sxs-lookup"><span data-stu-id="94f30-136">Open Azure Storage Explorer, select</span></span> ![ikon](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="94f30-138">Välj Azure-miljön fältet till vänster och sedan logga in.</span><span class="sxs-lookup"><span data-stu-id="94f30-138">on the left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="94f30-139">Välj alla prenumerationer eller den prenumeration som innehåller lagringskontot som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="94f30-139">Select all subscriptions or the subscription that contains the storage account you want to delete.</span></span>

    ![Lägg till prenumeration](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="94f30-141">Gå till lagringskontot som vi fick från disk URL: en tidigare, Välj den **Blobbbehållare** > **virtuella hårddiskar** och Sök efter den virtuella Hårddisken som gör att du ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94f30-141">Go to the storage account that we obtained from the disk URL earlier, select the **Blob Containers** > **vhds** and search for the VHD that prevents you delete the storage account.</span></span>
5. <span data-ttu-id="94f30-142">Kontrollera om den virtuella Hårddisken finns den **namn på virtuell** kolumnen att hitta den virtuella datorn som använder den här virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-142">If the VHD is found,  check the **VM Name** column to find the VM that is using this VHD.</span></span>

    ![Kontrollera vm](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="94f30-144">Ta bort lånet från den virtuella Hårddisken med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="94f30-144">Remove the lease from the VHD by using Azure portal.</span></span> <span data-ttu-id="94f30-145">Mer information finns i [ta bort lånet från den virtuella Hårddisken](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="94f30-145">For more information, see [Remove the lease from the VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="94f30-146">Gå till Azure Lagringsutforskaren, högerklicka på den virtuella Hårddisken och väljer Ta bort.</span><span class="sxs-lookup"><span data-stu-id="94f30-146">Go to the Azure Storage Explorer, right-click the VHD and then select delete.</span></span>

8. <span data-ttu-id="94f30-147">Ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94f30-147">Delete the storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="94f30-148">Metod 2 – använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="94f30-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="94f30-149">Steg 1: Identifiera den virtuella Hårddisken som förhindra borttagning av storage-konto</span><span class="sxs-lookup"><span data-stu-id="94f30-149">Step 1: Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="94f30-150">När du tar bort lagringskontot visas en dialogruta för meddelande, till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="94f30-150">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![meddelande när du tar bort lagringskontot](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="94f30-152">Kontrollera den **Disk URL** identifiera lagring konto och den virtuella Hårddisken som gör att du ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94f30-152">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="94f30-153">I följande exempel strängen innan ”. blob.core.windows.net” är lagringskontonamn och ”SCCM2012-2015-08-28.vhd” är virtuella Hårddiskens namn.</span><span class="sxs-lookup"><span data-stu-id="94f30-153">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="94f30-154">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94f30-154">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="94f30-155">På navmenyn väljer **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="94f30-155">On the Hub menu, select **All resources**.</span></span> <span data-ttu-id="94f30-156">Gå till lagringskontot och välj sedan **Blobbar** > **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="94f30-156">Go to the storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Skärmbild av portalen med storage-konto och ”VHD”-behållaren markerat](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="94f30-158">Leta upp den virtuella Hårddisken som vi tidigare hämtade från disk-URL.</span><span class="sxs-lookup"><span data-stu-id="94f30-158">Locate the VHD that we obtained from the disk URL earlier.</span></span> <span data-ttu-id="94f30-159">Sedan fastställa vilken virtuell dator med hjälp av den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-159">Then, determine which VM is using the VHD.</span></span> <span data-ttu-id="94f30-160">Vanligtvis kan du bestämma vilken VM innehåller den virtuella Hårddisken genom att kontrollera att namnet på den virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="94f30-160">Usually, you can determine which VM holds the VHD by checking name of the VHD:</span></span>

<span data-ttu-id="94f30-161">VM i modellen för Resource Manager-utveckling</span><span class="sxs-lookup"><span data-stu-id="94f30-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="94f30-162">OS-diskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="94f30-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="94f30-163">Datadiskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="94f30-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="94f30-164">VM i development modell</span><span class="sxs-lookup"><span data-stu-id="94f30-164">VM in Classic development model</span></span>

   * <span data-ttu-id="94f30-165">OS-diskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="94f30-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="94f30-166">Datadiskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="94f30-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-the-lease-from-the-vhd"></a><span data-ttu-id="94f30-167">Steg 2: Ta bort lånet från den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="94f30-167">Step 2: Remove the lease from the VHD</span></span>

<span data-ttu-id="94f30-168">[Ta bort lånet från den virtuella Hårddisken](#remove-the-lease-from-the-vhd), och ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94f30-168">[Remove the lease from the VHD](#remove-the-lease-from-the-vhd), and then delete the storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="94f30-169">Vad är ett lån?</span><span class="sxs-lookup"><span data-stu-id="94f30-169">What is a lease?</span></span>
<span data-ttu-id="94f30-170">Ett lån är ett lås som kan användas för att styra åtkomsten till en blob (till exempel en virtuell Hårddisk).</span><span class="sxs-lookup"><span data-stu-id="94f30-170">A lease is a lock that can be used to control access to a blob (for example, a VHD).</span></span> <span data-ttu-id="94f30-171">När en blob hyrs endast ägare av lånet åtkomst till blob.</span><span class="sxs-lookup"><span data-stu-id="94f30-171">When a blob is leased, only the owners of the lease can access the blob.</span></span> <span data-ttu-id="94f30-172">Ett lån är viktigt av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="94f30-172">A lease is important for the following reasons:</span></span>

* <span data-ttu-id="94f30-173">Det förhindrar att data skadas om flera ägare försök att skriva till samma del av blob samtidigt.</span><span class="sxs-lookup"><span data-stu-id="94f30-173">It prevents data corruption if multiple owners try to write to the same portion of the blob at the same time.</span></span>
* <span data-ttu-id="94f30-174">Det förhindrar att blob tas bort om något aktivt använder den (till exempel en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="94f30-174">It prevents the blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="94f30-175">Det förhindrar att lagringskontot tas bort om något aktivt använder den (till exempel en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="94f30-175">It prevents the storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-the-lease-from-the-vhd"></a><span data-ttu-id="94f30-176">Ta bort lånet från den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="94f30-176">Remove the lease from the VHD</span></span>
<span data-ttu-id="94f30-177">Om den virtuella Hårddisken är en OS-disk, måste du ta bort den virtuella datorn för att ta bort lånet:</span><span class="sxs-lookup"><span data-stu-id="94f30-177">If the VHD is an OS disk, you must delete the VM to remove the lease:</span></span>

1. <span data-ttu-id="94f30-178">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94f30-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="94f30-179">På den **hubb** väljer du **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="94f30-179">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="94f30-180">Välj den virtuella datorn som innehåller ett lån på den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-180">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="94f30-181">Kontrollera att inget aktivt använder den virtuella datorn, och du inte längre behöver den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="94f30-181">Make sure that nothing is actively using the virtual machine, and that you no longer need the virtual machine.</span></span>
5. <span data-ttu-id="94f30-182">Längst upp i den **VM information** bladet väljer **ta bort**, och klicka sedan på **Ja** att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="94f30-182">At the top of the **VM details** blade, select **Delete**, and then click **Yes** to confirm.</span></span>
6. <span data-ttu-id="94f30-183">Den virtuella datorn ska tas bort, men den virtuella Hårddisken kan behållas.</span><span class="sxs-lookup"><span data-stu-id="94f30-183">The VM should be deleted, but the VHD can be retained.</span></span> <span data-ttu-id="94f30-184">Den virtuella Hårddisken ska dock inte längre ha ett lån på den.</span><span class="sxs-lookup"><span data-stu-id="94f30-184">However, the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="94f30-185">Det kan ta några minuter för lånet frigörs.</span><span class="sxs-lookup"><span data-stu-id="94f30-185">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="94f30-186">Kontrollera att lånet släpps, gå till **alla resurser** > **Lagringskontonamnet** > **Blobbar**  >   **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="94f30-186">To verify that the lease is released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="94f30-187">I den **Blob-egenskaper** rutan i **lån Status** värdet bör vara **olåst**.</span><span class="sxs-lookup"><span data-stu-id="94f30-187">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="94f30-188">Om den virtuella Hårddisken är en datadisk, kan du koppla från den virtuella Hårddisken från den virtuella datorn att ta bort lånet:</span><span class="sxs-lookup"><span data-stu-id="94f30-188">If the VHD is a data disk, detach the VHD from the VM to remove the lease:</span></span>

1. <span data-ttu-id="94f30-189">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94f30-189">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="94f30-190">På den **hubb** väljer du **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="94f30-190">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="94f30-191">Välj den virtuella datorn som innehåller ett lån på den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-191">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="94f30-192">Välj **diskar** på den **VM information** bladet.</span><span class="sxs-lookup"><span data-stu-id="94f30-192">Select **Disks** on the **VM details** blade.</span></span>
5. <span data-ttu-id="94f30-193">Välj den datadisk som som innehåller ett lån på den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-193">Select the data disk that holds a lease on the VHD.</span></span> <span data-ttu-id="94f30-194">Du kan bestämma vilka virtuella Hårddisken är ansluten i disken genom att kontrollera URL: en för den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-194">You can determine which VHD is attached in the disk by checking the URL of the VHD.</span></span>
6. <span data-ttu-id="94f30-195">Kontrollera med säkerhet att inget aktivt använder datadisken.</span><span class="sxs-lookup"><span data-stu-id="94f30-195">Determine with certainty that nothing is actively using the data disk.</span></span>
7. <span data-ttu-id="94f30-196">Klicka på **Detach** på den **Disk information** bladet.</span><span class="sxs-lookup"><span data-stu-id="94f30-196">Click **Detach** on the **Disk details** blade.</span></span>
8. <span data-ttu-id="94f30-197">Disken nu att koppla från den virtuella datorn och den virtuella Hårddisken inte längre ska ha ett lån på den.</span><span class="sxs-lookup"><span data-stu-id="94f30-197">The disk should now be detached from the VM, and the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="94f30-198">Det kan ta några minuter för lånet frigörs.</span><span class="sxs-lookup"><span data-stu-id="94f30-198">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="94f30-199">Kontrollera att lånet har släppts, gå till **alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="94f30-199">To verify that the lease has been released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="94f30-200">I den **Blob-egenskaper** rutan i **lån Status** värdet bör vara **olåst**.</span><span class="sxs-lookup"><span data-stu-id="94f30-200">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94f30-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94f30-201">Next steps</span></span>
* [<span data-ttu-id="94f30-202">Ta bort ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="94f30-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="94f30-203">Hur du delar upp låst lånet för blob-lagring i Microsoft Azure (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="94f30-203">How to break the locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
