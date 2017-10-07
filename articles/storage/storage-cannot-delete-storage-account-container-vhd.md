---
title: "tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution aaaTroubleshoot | Microsoft Docs"
description: "Felsöka tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a><span data-ttu-id="ec940-103">Felsöka tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution</span><span class="sxs-lookup"><span data-stu-id="ec940-103">Troubleshoot deleting Azure storage accounts, containers, or VHDs in a classic deployment</span></span>
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

<span data-ttu-id="ec940-104">Fel kan visas när du försöker toodelete hello Azure storage-konto, behållare eller VHD i hello [Azure-portalen](https://portal.azure.com/) eller hello [klassiska Azure-portalen](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec940-104">You might receive errors when you try toodelete hello Azure storage account, container, or VHD in hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](https://manage.windowsazure.com/).</span></span> <span data-ttu-id="ec940-105">hello problem kan orsakas av hello följande omständigheter:</span><span class="sxs-lookup"><span data-stu-id="ec940-105">hello issues can be caused by hello following circumstances:</span></span>

* <span data-ttu-id="ec940-106">När du tar bort en virtuell dator tas hello disk och VHD inte bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ec940-106">When you delete a VM, hello disk and VHD are not automatically deleted.</span></span> <span data-ttu-id="ec940-107">Som kan vara hello orsaken till felet på borttagning av storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec940-107">That might be hello reason for failure on storage account deletion.</span></span> <span data-ttu-id="ec940-108">Vi ta inte bort hello disk så att du kan använda hello disk toomount en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ec940-108">We don't delete hello disk so that you can use hello disk toomount another VM.</span></span>
* <span data-ttu-id="ec940-109">Det finns fortfarande ett lån på en disk eller hello blob som är kopplad till hello disk.</span><span class="sxs-lookup"><span data-stu-id="ec940-109">There is still a lease on a disk or hello blob that's associated with hello disk.</span></span>
* <span data-ttu-id="ec940-110">Det finns fortfarande en VM-avbildning som använder ett konto för blob-behållare eller lagring.</span><span class="sxs-lookup"><span data-stu-id="ec940-110">There is still a VM image that is using a blob, container, or storage account.</span></span>

<span data-ttu-id="ec940-111">Om din Azure problemet inte åtgärdas i den här artikeln, gå hello Azure-forum på [MSDN och hello Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="ec940-111">If your Azure issue is not addressed in this article, visit hello Azure forums on [MSDN and hello Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="ec940-112">Du kan publicera problemet på dessa forum eller too@AzureSupport på Twitter.</span><span class="sxs-lookup"><span data-stu-id="ec940-112">You can post your issue on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="ec940-113">Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.</span><span class="sxs-lookup"><span data-stu-id="ec940-113">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="ec940-114">Symtom</span><span class="sxs-lookup"><span data-stu-id="ec940-114">Symptoms</span></span>
<span data-ttu-id="ec940-115">hello efter avsnittet listas vanliga fel som kan visas när du försöker toodelete hello Azure storage-konton, behållare eller virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ec940-115">hello following section lists common errors that you might receive when you try toodelete hello Azure storage accounts, containers, or VHDs.</span></span>

### <a name="scenario-1-unable-toodelete-a-storage-account"></a><span data-ttu-id="ec940-116">Scenario 1: Toodelete ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ec940-116">Scenario 1: Unable toodelete a storage account</span></span>
<span data-ttu-id="ec940-117">När du navigerar toohello klassiska storage-konto i hello [Azure-portalen](https://portal.azure.com/) och välj **ta bort**, visas en lista med objekt som hindrar borttagning av hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="ec940-117">When you navigate toohello classic storage account in hello [Azure portal](https://portal.azure.com/) and select **Delete**, you may be presented with a list of objects that are preventing deletion of hello storage account:</span></span>

  ![Bild av fel när tar bort hello Storage-konto](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

<span data-ttu-id="ec940-119">När du navigerar toohello storage-konto i hello [klassiska Azure-portalen](https://manage.windowsazure.com/) och välj **ta bort**, något av följande fel hello kan visas:</span><span class="sxs-lookup"><span data-stu-id="ec940-119">When you navigate toohello storage account in hello [Azure classic portal](https://manage.windowsazure.com/) and select **Delete**, you might see one of hello following errors:</span></span>

- <span data-ttu-id="ec940-120">*Lagringskontot StorageAccountName innehåller VM-avbildningar. Kontrollera att dessa VM-avbildningar avlägsnas innan lagringskontot.*</span><span class="sxs-lookup"><span data-stu-id="ec940-120">*Storage account StorageAccountName contains VM Images. Ensure these VM Images are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="ec940-121">*Det gick inte toodelete storage-konto < vm-storage-konto-name >. Det går inte toodelete storage-konto < vm-storage-konto-name > ”: Storage-konto < vm-storage-konto-name > har vissa aktiva avbildningar och/eller diskar. Kontrollera att dessa bilder och/eller diskar avlägsnas innan lagringskontot.'.*</span><span class="sxs-lookup"><span data-stu-id="ec940-121">*Failed toodelete storage account <vm-storage-account-name>. Unable toodelete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.*</span></span>

- <span data-ttu-id="ec940-122">*Storage-konto < vm-storage-konto-name > har vissa aktiva avbildningar och/eller diskar, t.ex. xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Se till att dessa avbildningar och/eller diskarna har tagits bort innan du tar bort det här lagringskontot.*</span><span class="sxs-lookup"><span data-stu-id="ec940-122">*Storage account <vm-storage-account-name> has some active image(s) and/or disk(s), e.g. xxxxxxxxx- xxxxxxxxx-O-209490240936090599. Ensure these image(s) and/or disk(s) are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="ec940-123">*Storage-konto < vm-storage-konto-name > har 1 behållare som har en aktiv avbildning och/eller disk artefakter. Kontrollera att dessa artefakter avlägsnas från avbildningslagringsplatsen hello innan du tar bort det här lagringskontot*.</span><span class="sxs-lookup"><span data-stu-id="ec940-123">*Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account*.</span></span>

- <span data-ttu-id="ec940-124">*Skicka misslyckades Storage-konto < vm-storage-konto-name > har 1 behållare som har en aktiv avbildning och/eller disk artefakter. Se till att dessa artefakter avlägsnas från avbildningslagringsplatsen hello innan du tar bort det här lagringskontot. När du försöker toodelete ett lagringskonto och det finns fortfarande aktiva diskar som är kopplade till den, visas ett meddelande om det finns aktiva diskar som behöver toobe bort*.</span><span class="sxs-lookup"><span data-stu-id="ec940-124">*Submit Failed Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account. When you attempt toodelete a storage account and there are still active disks associated with it, you will see a message telling you there are active disks that need toobe deleted*.</span></span>

### <a name="scenario-2-unable-toodelete-a-container"></a><span data-ttu-id="ec940-125">Scenario 2: Det gick inte toodelete en behållare</span><span class="sxs-lookup"><span data-stu-id="ec940-125">Scenario 2: Unable toodelete a container</span></span>
<span data-ttu-id="ec940-126">När du försöker toodelete hello lagringsbehållaren, kan du se hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="ec940-126">When you try toodelete hello storage container, you might see hello following error:</span></span>

<span data-ttu-id="ec940-127">*Misslyckade toodelete lagringsbehållaren <container name>. Fel ”: det finns för närvarande ett lån på hello behållare och inget lease-ID har angetts i begäran hello*.</span><span class="sxs-lookup"><span data-stu-id="ec940-127">*Failed toodelete storage container <container name>. Error: 'There is currently a lease on hello container and no lease ID was specified in hello request*.</span></span>

<span data-ttu-id="ec940-128">Eller</span><span class="sxs-lookup"><span data-stu-id="ec940-128">Or</span></span>

<span data-ttu-id="ec940-129">*hello följande virtuella diskar som använder blobbar i den här behållaren så hello behållaren inte kan tas bort: VirtualMachineDiskName1, VirtualMachineDiskName2,...*</span><span class="sxs-lookup"><span data-stu-id="ec940-129">*hello following virtual machine disks use blobs in this container, so hello container cannot be deleted: VirtualMachineDiskName1, VirtualMachineDiskName2, ...*</span></span>

### <a name="scenario-3-unable-toodelete-a-vhd"></a><span data-ttu-id="ec940-130">Scenario 3: Det gick inte toodelete en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="ec940-130">Scenario 3: Unable toodelete a VHD</span></span>
<span data-ttu-id="ec940-131">När du tar bort en virtuell dator och sedan försöka toodelete hello BLOB för hello tillhörande virtuella hårddiskar, kan du få hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="ec940-131">After you delete a VM and then try toodelete hello blobs for hello associated VHDs, you might receive hello following message:</span></span>

<span data-ttu-id="ec940-132">*Misslyckade toodelete blob-sökvägen/XXXXXX-XXXXXX-os-1447379084699.vhd'. Fel ”: det finns för närvarande ett lån på hello blob och inget lease-ID har angetts i hello-begäran.*</span><span class="sxs-lookup"><span data-stu-id="ec940-132">*Failed toodelete blob 'path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Error: 'There is currently a lease on hello blob and no lease ID was specified in hello request.*</span></span>

<span data-ttu-id="ec940-133">Eller</span><span class="sxs-lookup"><span data-stu-id="ec940-133">Or</span></span>

<span data-ttu-id="ec940-134">*BLOB BlobName.vhd används som virtuell disk VirtualMachineDiskName, så hello blob inte kan tas bort.*</span><span class="sxs-lookup"><span data-stu-id="ec940-134">*Blob 'BlobName.vhd' is in use as virtual machine disk 'VirtualMachineDiskName', so hello blob cannot be deleted.*</span></span>

## <a name="solution"></a><span data-ttu-id="ec940-135">Lösning</span><span class="sxs-lookup"><span data-stu-id="ec940-135">Solution</span></span>
<span data-ttu-id="ec940-136">tooresolve hello de flesta vanliga problem, försök hello följande metod:</span><span class="sxs-lookup"><span data-stu-id="ec940-136">tooresolve hello most common issues, try hello following method:</span></span>

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a><span data-ttu-id="ec940-137">Steg 1: Ta bort diskar som hindrar borttagning av hello storage-konto, behållare eller virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="ec940-137">Step 1: Delete any disks that are preventing deletion of hello storage account, container, or VHD</span></span>
1. <span data-ttu-id="ec940-138">Växla toohello [klassiska Azure-portalen](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec940-138">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="ec940-139">Välj **VIRTUELLA** > **DISKAR**.</span><span class="sxs-lookup"><span data-stu-id="ec940-139">Select **VIRTUAL MACHINE** > **DISKS**.</span></span>

    ![Bild av diskar på virtuella datorer på den klassiska Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. <span data-ttu-id="ec940-141">Leta upp hello diskar som är associerade med hello storage-konto, behållare eller VHD som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="ec940-141">Locate hello disks that are associated with hello storage account, container, or VHD that you want toodelete.</span></span> <span data-ttu-id="ec940-142">När du markerar hello platsen för hello disk hittar du hello associerade lagringskontot, behållare eller VHD.</span><span class="sxs-lookup"><span data-stu-id="ec940-142">When you check hello location of hello disk, you will find hello associated storage account, container, or VHD.</span></span>

    ![Bild som visar information om diskar på den klassiska Azure-portalen](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. <span data-ttu-id="ec940-144">Ta bort hello diskar med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="ec940-144">Delete hello disks by using one of hello following methods:</span></span>

  - <span data-ttu-id="ec940-145">Om det finns ingen VM finns på hello **ansluten till** fält av hello disk kan du ta bort hello disk direkt.</span><span class="sxs-lookup"><span data-stu-id="ec940-145">If  there is no VM listed on hello **Attached To** field of hello disk, you can delete hello disk directly.</span></span>

  - <span data-ttu-id="ec940-146">Om hello är en datadisk, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ec940-146">If hello disk is a data disk, follow these steps:</span></span>

    1. <span data-ttu-id="ec940-147">Kontrollera hello namn hello VM som hello disk som är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="ec940-147">Check hello name of hello VM that hello disk is attached to.</span></span>
    2. <span data-ttu-id="ec940-148">Gå för**virtuella datorer** > **instanser**, och leta sedan upp hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec940-148">Go too**Virtual Machines** > **Instances**, and then locate hello VM.</span></span>
    3. <span data-ttu-id="ec940-149">Kontrollera att inget aktivt använder hello disk.</span><span class="sxs-lookup"><span data-stu-id="ec940-149">Make sure that nothing is actively using hello disk.</span></span>
    4. <span data-ttu-id="ec940-150">Välj **koppla från Disk** längst hello hello portal toodetach hello disk.</span><span class="sxs-lookup"><span data-stu-id="ec940-150">Select **Detach Disk** at hello bottom of hello portal toodetach hello disk.</span></span>
    5. <span data-ttu-id="ec940-151">Gå för**virtuella datorer** > **diskar**, och vänta tills hello **ansluten till** fältet tooturn tomt.</span><span class="sxs-lookup"><span data-stu-id="ec940-151">Go too**Virtual Machines** > **Disks**, and wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="ec940-152">Detta anger hello disk har kopplats från hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec940-152">This indicates hello disk has successfully detached from hello VM.</span></span>
    6. <span data-ttu-id="ec940-153">Välj **ta bort** längst hello **virtuella datorer** > **diskar** toodelete hello disk.</span><span class="sxs-lookup"><span data-stu-id="ec940-153">Select **Delete** at hello bottom of **Virtual Machines** > **Disks** toodelete hello disk.</span></span>

  - <span data-ttu-id="ec940-154">Om hello är en OS-disk (hello **innehåller OS** fältet har ett värde som Windows) och anslutna tooa VM, Följ dessa steg toodelete hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec940-154">If hello disk is an OS disk (hello **Contains OS** field has a value like Windows) and attached tooa VM, follow these steps toodelete hello VM.</span></span> <span data-ttu-id="ec940-155">kan inte frånkopplas hello OS-disken så att vi har toodelete hello VM toorelease hello lån.</span><span class="sxs-lookup"><span data-stu-id="ec940-155">hello OS disk cannot be detached, so we have toodelete hello VM toorelease hello lease.</span></span>

    1. <span data-ttu-id="ec940-156">Kontrollera hello namn hello virtuella hello datadisk är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="ec940-156">Check hello name of hello Virtual Machine hello Data Disk is attached to.</span></span>  
    2. <span data-ttu-id="ec940-157">Gå för**virtuella datorer** > **instanser**, och sedan väljer hello VM som hello disk är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="ec940-157">Go too**Virtual Machines** > **Instances**, and then select hello VM that hello disk is attached to.</span></span>
    3. <span data-ttu-id="ec940-158">Kontrollera att inget aktivt använder hello virtuella datorn och att du inte längre behöver hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ec940-158">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
    4. <span data-ttu-id="ec940-159">Välj hello VM hello disken är ansluten till, välj sedan **ta bort** > **ta bort hello anslutna diskar**.</span><span class="sxs-lookup"><span data-stu-id="ec940-159">Select hello VM hello disk is attached to, then select **Delete** > **Delete hello attached disks**.</span></span>
    5. <span data-ttu-id="ec940-160">Gå för**virtuella datorer** > **diskar**, och vänta tills hello disk toodisappear.</span><span class="sxs-lookup"><span data-stu-id="ec940-160">Go too**Virtual Machines** > **Disks**, and wait for hello disk toodisappear.</span></span>  <span data-ttu-id="ec940-161">Det kan ta några minuter för den här toooccur och du kan behöva toorefresh hello sidan.</span><span class="sxs-lookup"><span data-stu-id="ec940-161">It may take a few minutes for this toooccur, and you may need toorefresh hello page.</span></span>
    6. <span data-ttu-id="ec940-162">Om hello disk inte försvinner väntar hello **ansluten till** fältet tooturn tomt.</span><span class="sxs-lookup"><span data-stu-id="ec940-162">If hello disk does not disappear, wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="ec940-163">Detta anger hello disk har helt oberoende från hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec940-163">This indicates hello disk has fully detached from hello VM.</span></span>  <span data-ttu-id="ec940-164">Sedan, Välj hello disken och markera **ta bort** längst hello hello sidan toodelete hello disk.</span><span class="sxs-lookup"><span data-stu-id="ec940-164">Then, select hello disk, and select **Delete** at hello bottom of hello page toodelete hello disk.</span></span>


   > [!NOTE]
   > <span data-ttu-id="ec940-165">Om en disk är anslutna tooa VM kan du inte kan toodelete den.</span><span class="sxs-lookup"><span data-stu-id="ec940-165">If a disk is attached tooa VM, you will not be able toodelete it.</span></span> <span data-ttu-id="ec940-166">Diskar har kopplats bort från en borttagen virtuell dator asynkront.</span><span class="sxs-lookup"><span data-stu-id="ec940-166">Disks are detached from a deleted VM asynchronously.</span></span> <span data-ttu-id="ec940-167">Det kan ta några minuter efter hello VM har tagits bort för det här fältet tooclear upp.</span><span class="sxs-lookup"><span data-stu-id="ec940-167">It might take a few minutes after hello VM is deleted for this field tooclear up.</span></span>
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a><span data-ttu-id="ec940-168">Steg 2: Ta bort eventuella VM-avbildningar som hindrar borttagning av hello storage-konto eller behållaren</span><span class="sxs-lookup"><span data-stu-id="ec940-168">Step 2: Delete any VM Images that are preventing deletion of hello storage account or container</span></span>
1. <span data-ttu-id="ec940-169">Växla toohello [klassiska Azure-portalen](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec940-169">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="ec940-170">Välj **VIRTUELLA** > **bilder**, och ta sedan bort hello-avbildningar som är associerade med hello storage-konto, behållare eller VHD.</span><span class="sxs-lookup"><span data-stu-id="ec940-170">Select **VIRTUAL MACHINE** > **IMAGES**, and then delete hello images that are associated with hello storage account, container, or VHD.</span></span>

    <span data-ttu-id="ec940-171">Sedan kan försöka toodelete hello storage-konto, behållare eller VHD.</span><span class="sxs-lookup"><span data-stu-id="ec940-171">After that, try toodelete hello storage account, container, or VHD again.</span></span>

> [!WARNING]
> <span data-ttu-id="ec940-172">Vara säker på att tooback in något du toosave innan du tar bort hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="ec940-172">Be sure tooback up anything you want toosave before you delete hello account.</span></span> <span data-ttu-id="ec940-173">När du tar bort en virtuell Hårddisk, blob, tabell, kö eller fil bort den permanent.</span><span class="sxs-lookup"><span data-stu-id="ec940-173">Once you delete a VHD, blob, table, queue, or file, it is permanently deleted.</span></span> <span data-ttu-id="ec940-174">Se till att resursen hello inte används.</span><span class="sxs-lookup"><span data-stu-id="ec940-174">Ensure that hello resource is not in use.</span></span>
>
>

## <a name="about-hello-stopped-deallocated-status"></a><span data-ttu-id="ec940-175">Om hello Stoppad (frigjord) status</span><span class="sxs-lookup"><span data-stu-id="ec940-175">About hello Stopped (deallocated) status</span></span>
<span data-ttu-id="ec940-176">Virtuella datorer som har skapats i hello klassiska distributionsmodellen och som har behållits har hello **Stoppad (frigjord)** status på antingen hello [Azure-portalen](https://portal.azure.com/) eller [klassiska Azure-portalen ](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec940-176">VMs that were created in hello classic deployment model and that have been retained will have hello **Stopped (deallocated)** status on either hello [Azure portal](https://portal.azure.com/) or [Azure classic portal](https://manage.windowsazure.com/).</span></span>

<span data-ttu-id="ec940-177">**Klassiska Azure-portalen**:</span><span class="sxs-lookup"><span data-stu-id="ec940-177">**Azure classic portal**:</span></span>

![Stoppad (Deallocated) status för virtuella datorer på Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

<span data-ttu-id="ec940-179">**Azure-portalen**:</span><span class="sxs-lookup"><span data-stu-id="ec940-179">**Azure portal**:</span></span>

![Stoppats (frigjorts) status för virtuella datorer på den klassiska Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

<span data-ttu-id="ec940-181">Statusen ”Stoppad (frigjord)” Frigör hello datorresurser, till exempel hello CPU, minne och nätverk.</span><span class="sxs-lookup"><span data-stu-id="ec940-181">A "Stopped (deallocated)" status releases hello computer resources, such as hello CPU, memory, and network.</span></span> <span data-ttu-id="ec940-182">hello diskar, men är fortfarande kvar så att du snabbt återskapa hello VM om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ec940-182">hello disks, however, are still retained so that you can quickly re-create hello VM if necessary.</span></span> <span data-ttu-id="ec940-183">Diskarna har skapats på virtuella hårddiskar som backas upp av Azure storage.</span><span class="sxs-lookup"><span data-stu-id="ec940-183">These disks are created on top of VHDs, which are backed by Azure storage.</span></span> <span data-ttu-id="ec940-184">hello storage-konto har dessa virtuella hårddiskar och hello diskar har lån på de virtuella hårddiskarna.</span><span class="sxs-lookup"><span data-stu-id="ec940-184">hello storage account has these VHDs, and hello disks have leases on those VHDs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec940-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec940-185">Next steps</span></span>
* [<span data-ttu-id="ec940-186">Ta bort ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ec940-186">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
