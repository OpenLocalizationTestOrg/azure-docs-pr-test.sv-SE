---
title: "aaaTroubleshoot fel när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar | Microsoft Docs"
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
ms.openlocfilehash: 77361593e2c924d39aba853e0807dc3188f50e60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="e17ff-103">Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="e17ff-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="e17ff-104">Du kan få fel när du försöker toodelete ett Azure storage-konto, en behållare eller en virtuell hårddisk (VHD) med hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e17ff-104">You might receive errors when you try toodelete an Azure storage account, container, or virtual hard disk (VHD) in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e17ff-105">Den här artikeln innehåller felsökning vägledning toohelp Lös hello problem i en Azure Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="e17ff-105">This article provides troubleshooting guidance toohelp resolve hello problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="e17ff-106">Om den här artikeln inte åtgärda problemet Azure, gå hello Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="e17ff-106">If this article doesn't address your Azure problem, visit hello Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="e17ff-107">Du kan publicera ditt problem på dessa forum eller too@AzureSupport på Twitter.</span><span class="sxs-lookup"><span data-stu-id="e17ff-107">You can post your problem on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="e17ff-108">Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.</span><span class="sxs-lookup"><span data-stu-id="e17ff-108">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="e17ff-109">Symtom</span><span class="sxs-lookup"><span data-stu-id="e17ff-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="e17ff-110">Scenario 1</span><span class="sxs-lookup"><span data-stu-id="e17ff-110">Scenario 1</span></span>
<span data-ttu-id="e17ff-111">När du försöker toodelete en virtuell Hårddisk i ett lagringskonto i en Resource Manager distribution visas hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="e17ff-111">When you try toodelete a VHD in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="e17ff-112">**Det gick inte toodelete blob 'vhds/BlobName.vhd'. Fel: Det finns för närvarande ett lån på hello blob och inget lease-ID har angetts i hello-begäran.**</span><span class="sxs-lookup"><span data-stu-id="e17ff-112">**Failed toodelete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on hello blob and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="e17ff-113">Det här problemet kan uppstå eftersom en virtuell dator (VM) har ett lån på hello VHD som du försöker toodelete.</span><span class="sxs-lookup"><span data-stu-id="e17ff-113">This problem can occur because a virtual machine (VM) has a lease on hello VHD that you are trying toodelete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="e17ff-114">Scenario 2</span><span class="sxs-lookup"><span data-stu-id="e17ff-114">Scenario 2</span></span>
<span data-ttu-id="e17ff-115">När du försöker toodelete en behållare i ett lagringskonto i en Resource Manager distribution visas hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="e17ff-115">When you try toodelete a container in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="e17ff-116">**Det gick inte virtuella hårddiskar' toodelete lagring behållare'. Fel: Det finns för närvarande ett lån på hello behållare och inget lease-ID har angetts i hello-begäran.**</span><span class="sxs-lookup"><span data-stu-id="e17ff-116">**Failed toodelete storage container 'vhds'. Error: There is currently a lease on hello container and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="e17ff-117">Det här problemet kan inträffa eftersom hello behållaren har en virtuell Hårddisk som är låsta i hello lease-tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e17ff-117">This problem can occur because hello container has a VHD that is locked in hello lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="e17ff-118">Scenario 3</span><span class="sxs-lookup"><span data-stu-id="e17ff-118">Scenario 3</span></span>
<span data-ttu-id="e17ff-119">Hello följande felmeddelande visas när toodelete ett lagringskonto i en Resource Manager distribution:</span><span class="sxs-lookup"><span data-stu-id="e17ff-119">When you try toodelete a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="e17ff-120">**Det gick inte toodelete storage-konto 'StorageAccountName'. Fel: hello storage-konto kan inte tas bort på grund av tooits artefakter används.**</span><span class="sxs-lookup"><span data-stu-id="e17ff-120">**Failed toodelete storage account 'StorageAccountName'. Error: hello storage account cannot be deleted due tooits artifacts being in use.**</span></span>

<span data-ttu-id="e17ff-121">Det här problemet kan inträffa eftersom hello storage-konto innehåller en virtuell Hårddisk som är i hello lease-tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e17ff-121">This problem can occur because hello storage account contains a VHD that is in hello lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="e17ff-122">Lösning</span><span class="sxs-lookup"><span data-stu-id="e17ff-122">Solution</span></span> 
<span data-ttu-id="e17ff-123">tooresolve dessa problem måste du identifiera hello VHD som orsakar fel hello och hello associerade VM.</span><span class="sxs-lookup"><span data-stu-id="e17ff-123">tooresolve these problems, you must identify hello VHD that is causing hello error and hello associated VM.</span></span> <span data-ttu-id="e17ff-124">Sedan koppla hello VHD från hello VM (för datadiskar) eller ta bort hello VM som använder hello VHD (för OS-diskar).</span><span class="sxs-lookup"><span data-stu-id="e17ff-124">Then, detach hello VHD from hello VM (for data disks) or delete hello VM that is using hello VHD (for OS disks).</span></span> <span data-ttu-id="e17ff-125">Det tar bort hello lån från hello VHD och gör den toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="e17ff-125">This removes hello lease from hello VHD and allows it toobe deleted.</span></span> 

<span data-ttu-id="e17ff-126">toodo denna, Använd en av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="e17ff-126">toodo this, use one of hello following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="e17ff-127">Metod 1 - Använd Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="e17ff-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="e17ff-128">Steg 1 identifiera hello VHD som förhindra borttagning av hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="e17ff-128">Step 1 Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="e17ff-129">När du tar bort hello storage-konto visas en dialogruta för meddelande, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="e17ff-129">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![meddelande när du tar bort hello storage-konto](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="e17ff-131">Kontrollera hello **Disk URL** tooidentify hello storage-konto och hello VHD som gör att du tar bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e17ff-131">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="e17ff-132">I följande exempel hello, hello sträng innan ”. blob.core.windows.net” Hej lagringskontonamn och ”SCCM2012-2015-08-28.vhd” hello VHD namn.</span><span class="sxs-lookup"><span data-stu-id="e17ff-132">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="e17ff-133">Steg 2 ta bort hello VHD med hjälp av Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="e17ff-133">Step 2 Delete hello VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="e17ff-134">Hämta och installera hello senaste versionen av [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="e17ff-134">Download and Install hello latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="e17ff-135">Det här verktyget är en fristående app från Microsoft som möjliggör tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="e17ff-135">This tool is a standalone app from Microsoft that allows you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="e17ff-136">Öppna Azure Lagringsutforskaren, Välj</span><span class="sxs-lookup"><span data-stu-id="e17ff-136">Open Azure Storage Explorer, select</span></span> ![ikon](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="e17ff-138">Välj Azure-miljön hello vänstra fältet och logga sedan in.</span><span class="sxs-lookup"><span data-stu-id="e17ff-138">on hello left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="e17ff-139">Markera alla prenumerationer eller hello-prenumeration som innehåller hello storage-konto som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="e17ff-139">Select all subscriptions or hello subscription that contains hello storage account you want toodelete.</span></span>

    ![Lägg till prenumeration](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="e17ff-141">Gå toohello storage-konto som vi fick från hello disk URL tidigare, Välj hello **Blobbbehållare** > **virtuella hårddiskar** och Sök efter hello VHD som gör att du ta bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e17ff-141">Go toohello storage account that we obtained from hello disk URL earlier, select hello **Blob Containers** > **vhds** and search for hello VHD that prevents you delete hello storage account.</span></span>
5. <span data-ttu-id="e17ff-142">Om hello VHD hittas, kontrollera hello **namn på virtuell** kolumnen toofind hello VM som använder den här virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="e17ff-142">If hello VHD is found,  check hello **VM Name** column toofind hello VM that is using this VHD.</span></span>

    ![Kontrollera vm](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="e17ff-144">Ta bort hello lån från hello VHD med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e17ff-144">Remove hello lease from hello VHD by using Azure portal.</span></span> <span data-ttu-id="e17ff-145">Mer information finns i [ta bort hello-lån från hello VHD](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="e17ff-145">For more information, see [Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="e17ff-146">Gå toohello Azure Lagringsutforskaren, högerklicka på hello VHD och välj sedan ta bort.</span><span class="sxs-lookup"><span data-stu-id="e17ff-146">Go toohello Azure Storage Explorer, right-click hello VHD and then select delete.</span></span>

8. <span data-ttu-id="e17ff-147">Ta bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e17ff-147">Delete hello storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="e17ff-148">Metod 2 – använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e17ff-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="e17ff-149">Steg 1: Identifiera hello VHD som förhindra borttagning av hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="e17ff-149">Step 1: Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="e17ff-150">När du tar bort hello storage-konto visas en dialogruta för meddelande, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="e17ff-150">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![meddelande när du tar bort hello storage-konto](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="e17ff-152">Kontrollera hello **Disk URL** tooidentify hello storage-konto och hello VHD som gör att du tar bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e17ff-152">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="e17ff-153">I följande exempel hello, hello sträng innan ”. blob.core.windows.net” Hej lagringskontonamn och ”SCCM2012-2015-08-28.vhd” hello VHD namn.</span><span class="sxs-lookup"><span data-stu-id="e17ff-153">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="e17ff-154">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e17ff-154">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="e17ff-155">På navmenyn hello väljer **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-155">On hello Hub menu, select **All resources**.</span></span> <span data-ttu-id="e17ff-156">Gå toohello storage-konto och välj sedan **Blobbar** > **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-156">Go toohello storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Skärmbild av hello-portalen med hello storage-konto och hello ”VHD” behållare markerat](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="e17ff-158">Leta upp hello VHD som vi tidigare hämtade från hello disk URL.</span><span class="sxs-lookup"><span data-stu-id="e17ff-158">Locate hello VHD that we obtained from hello disk URL earlier.</span></span> <span data-ttu-id="e17ff-159">Sedan fastställa vilken virtuell dator använder hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e17ff-159">Then, determine which VM is using hello VHD.</span></span> <span data-ttu-id="e17ff-160">Vanligtvis kan du bestämma vilken VM innehåller hello VHD genom att kontrollera att namnet på hello VHD:</span><span class="sxs-lookup"><span data-stu-id="e17ff-160">Usually, you can determine which VM holds hello VHD by checking name of hello VHD:</span></span>

<span data-ttu-id="e17ff-161">VM i modellen för Resource Manager-utveckling</span><span class="sxs-lookup"><span data-stu-id="e17ff-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="e17ff-162">OS-diskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="e17ff-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="e17ff-163">Datadiskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="e17ff-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="e17ff-164">VM i development modell</span><span class="sxs-lookup"><span data-stu-id="e17ff-164">VM in Classic development model</span></span>

   * <span data-ttu-id="e17ff-165">OS-diskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="e17ff-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="e17ff-166">Datadiskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="e17ff-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="e17ff-167">Steg 2: Ta bort hello lån från hello VHD</span><span class="sxs-lookup"><span data-stu-id="e17ff-167">Step 2: Remove hello lease from hello VHD</span></span>

<span data-ttu-id="e17ff-168">[Ta bort hello lån från hello VHD](#remove-the-lease-from-the-vhd), och ta sedan bort hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e17ff-168">[Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd), and then delete hello storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="e17ff-169">Vad är ett lån?</span><span class="sxs-lookup"><span data-stu-id="e17ff-169">What is a lease?</span></span>
<span data-ttu-id="e17ff-170">Ett lån är ett lås som kan använda toocontrol åtkomst tooa blob (till exempel en virtuell Hårddisk).</span><span class="sxs-lookup"><span data-stu-id="e17ff-170">A lease is a lock that can be used toocontrol access tooa blob (for example, a VHD).</span></span> <span data-ttu-id="e17ff-171">När en blob hyrs åtkomst bara hello ägare hello lease hello-blob.</span><span class="sxs-lookup"><span data-stu-id="e17ff-171">When a blob is leased, only hello owners of hello lease can access hello blob.</span></span> <span data-ttu-id="e17ff-172">Ett lån är viktigt för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="e17ff-172">A lease is important for hello following reasons:</span></span>

* <span data-ttu-id="e17ff-173">Det förhindrar att data skadas om flera ägare försöker toowrite toohello samma del av hello blob hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e17ff-173">It prevents data corruption if multiple owners try toowrite toohello same portion of hello blob at hello same time.</span></span>
* <span data-ttu-id="e17ff-174">Det förhindrar att hello blob tas bort om något aktivt använder den (till exempel en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="e17ff-174">It prevents hello blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="e17ff-175">Det förhindrar att hello lagringskontot tas bort om något aktivt använder den (till exempel en virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="e17ff-175">It prevents hello storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="e17ff-176">Ta bort hello lån från hello VHD</span><span class="sxs-lookup"><span data-stu-id="e17ff-176">Remove hello lease from hello VHD</span></span>
<span data-ttu-id="e17ff-177">Om hello VHD är en OS-disk, måste du ta bort hello VM tooremove hello lån:</span><span class="sxs-lookup"><span data-stu-id="e17ff-177">If hello VHD is an OS disk, you must delete hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="e17ff-178">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e17ff-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e17ff-179">På hello **hubb** väljer du **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-179">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="e17ff-180">Välj hello VM som innehåller ett lån på hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e17ff-180">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="e17ff-181">Kontrollera att inget aktivt använder hello virtuella datorn och att du inte längre behöver hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e17ff-181">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
5. <span data-ttu-id="e17ff-182">Hello överst i hello **VM information** bladet väljer **ta bort**, och klicka sedan på **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="e17ff-182">At hello top of hello **VM details** blade, select **Delete**, and then click **Yes** tooconfirm.</span></span>
6. <span data-ttu-id="e17ff-183">hello VM ska tas bort, men hello VHD kan behållas.</span><span class="sxs-lookup"><span data-stu-id="e17ff-183">hello VM should be deleted, but hello VHD can be retained.</span></span> <span data-ttu-id="e17ff-184">Hello VHD bör dock inte längre ha ett lån på den.</span><span class="sxs-lookup"><span data-stu-id="e17ff-184">However, hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="e17ff-185">Det kan ta några minuter för hello lån toobe släpps.</span><span class="sxs-lookup"><span data-stu-id="e17ff-185">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="e17ff-186">tooverify som hello lån släpps finns för**alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-186">tooverify that hello lease is released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="e17ff-187">I hello **Blob-egenskaper** rutan, hello **lån Status** värdet bör vara **olåst**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-187">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="e17ff-188">Om hello VHD är en datadisk, koppla hello VHD från hello VM tooremove hello lån:</span><span class="sxs-lookup"><span data-stu-id="e17ff-188">If hello VHD is a data disk, detach hello VHD from hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="e17ff-189">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e17ff-189">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e17ff-190">På hello **hubb** väljer du **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-190">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="e17ff-191">Välj hello VM som innehåller ett lån på hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e17ff-191">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="e17ff-192">Välj **diskar** på hello **VM information** bladet.</span><span class="sxs-lookup"><span data-stu-id="e17ff-192">Select **Disks** on hello **VM details** blade.</span></span>
5. <span data-ttu-id="e17ff-193">Välj hello datadisk som innehåller ett lån på hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e17ff-193">Select hello data disk that holds a lease on hello VHD.</span></span> <span data-ttu-id="e17ff-194">Du kan bestämma vilka virtuella Hårddisken är ansluten i hello disk genom att kontrollera hello URL för hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e17ff-194">You can determine which VHD is attached in hello disk by checking hello URL of hello VHD.</span></span>
6. <span data-ttu-id="e17ff-195">Kontrollera med säkerhet att inget aktivt använder hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="e17ff-195">Determine with certainty that nothing is actively using hello data disk.</span></span>
7. <span data-ttu-id="e17ff-196">Klicka på **Detach** på hello **Disk information** bladet.</span><span class="sxs-lookup"><span data-stu-id="e17ff-196">Click **Detach** on hello **Disk details** blade.</span></span>
8. <span data-ttu-id="e17ff-197">hello disk ska nu frånkopplas från hello VM och hello VHD inte längre ska ha ett lån på den.</span><span class="sxs-lookup"><span data-stu-id="e17ff-197">hello disk should now be detached from hello VM, and hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="e17ff-198">Det kan ta några minuter för hello lån toobe släpps.</span><span class="sxs-lookup"><span data-stu-id="e17ff-198">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="e17ff-199">tooverify som hello lån har släppts finns för**alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-199">tooverify that hello lease has been released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="e17ff-200">I hello **Blob-egenskaper** rutan, hello **lån Status** värdet bör vara **olåst**.</span><span class="sxs-lookup"><span data-stu-id="e17ff-200">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e17ff-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e17ff-201">Next steps</span></span>
* [<span data-ttu-id="e17ff-202">Ta bort ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e17ff-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="e17ff-203">Hur toobreak hello låst lånet för blob-lagring i Microsoft Azure (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="e17ff-203">How toobreak hello locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
