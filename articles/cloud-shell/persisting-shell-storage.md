---
title: "aaaPersist filer i Azure Cloud Shell (förhandsversion) | Microsoft Docs"
description: "Genomgång av hur Azure Cloud Shell kvarstår filer."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a><span data-ttu-id="53186-103">Spara filer i Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="53186-103">Persist files in Azure Cloud Shell</span></span>
<span data-ttu-id="53186-104">Molnet Shell utnyttjar Azure File storage toopersist filer mellan sessioner.</span><span class="sxs-lookup"><span data-stu-id="53186-104">Cloud Shell takes advantage of Azure File storage toopersist files across sessions.</span></span>

## <a name="set-up-a-clouddrive-file-share"></a><span data-ttu-id="53186-105">Konfigurera en `clouddrive` filresurs</span><span class="sxs-lookup"><span data-stu-id="53186-105">Set up a `clouddrive` file share</span></span>
<span data-ttu-id="53186-106">På första start efterfrågar molnet Shell tooassociate en ny eller befintlig filresursen toopersist filer mellan sessioner.</span><span class="sxs-lookup"><span data-stu-id="53186-106">On initial start, Cloud Shell prompts you tooassociate a new or existing file share toopersist files across sessions.</span></span>

### <a name="create-new-storage"></a><span data-ttu-id="53186-107">Skapa nya lagringsenheter</span><span class="sxs-lookup"><span data-stu-id="53186-107">Create new storage</span></span>

<span data-ttu-id="53186-108">När du använder grundläggande inställningar och välj endast en prenumeration skapar molnet Shell tre resurser å dina vägnar i hello stöds region som är närmast tooyou:</span><span class="sxs-lookup"><span data-stu-id="53186-108">When you use basic settings and select only a subscription, Cloud Shell creates three resources on your behalf in hello supported region that's nearest tooyou:</span></span>
* <span data-ttu-id="53186-109">Resursgrupp:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="53186-109">Resource group: `cloud-shell-storage-<region>`</span></span>
* <span data-ttu-id="53186-110">Storage-konto:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="53186-110">Storage account: `cs<uniqueGuid>`</span></span>
* <span data-ttu-id="53186-111">Filresurs:`cs-<user>-<domain>-com-<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="53186-111">File share: `cs-<user>-<domain>-com-<uniqueGuid>`</span></span>

![hello prenumeration inställningen](media/basic-storage.png)

<span data-ttu-id="53186-113">hello filresurs monterar som `clouddrive` i din `$Home` directory.</span><span class="sxs-lookup"><span data-stu-id="53186-113">hello file share mounts as `clouddrive` in your `$Home` directory.</span></span> <span data-ttu-id="53186-114">hello filresursen är också använda toostore en 5 GB-avbildning som skapas automatiskt för dig och som uppdateras och kvarstår din `$Home` directory.</span><span class="sxs-lookup"><span data-stu-id="53186-114">hello file share is also used toostore a 5-GB image that's created for you and that automatically updates and persists your `$Home` directory.</span></span> <span data-ttu-id="53186-115">Detta är en engångsåtgärd och hello filresursen automatiskt monterar i efterföljande sessioner.</span><span class="sxs-lookup"><span data-stu-id="53186-115">This is a one-time action, and hello file share mounts automatically in subsequent sessions.</span></span>

### <a name="use-existing-resources"></a><span data-ttu-id="53186-116">Använda befintliga resurser</span><span class="sxs-lookup"><span data-stu-id="53186-116">Use existing resources</span></span>

<span data-ttu-id="53186-117">Du kan associera befintliga resurser med hjälp av hello Avancerat alternativ.</span><span class="sxs-lookup"><span data-stu-id="53186-117">By using hello advanced option, you can associate existing resources.</span></span> <span data-ttu-id="53186-118">När hello lagring installationsprogrammet uppmanas att välja **visa avancerade inställningar** tooview ytterligare alternativ.</span><span class="sxs-lookup"><span data-stu-id="53186-118">When hello storage setup prompt appears, select **Show advanced settings** tooview additional options.</span></span> <span data-ttu-id="53186-119">Befintliga filresurser får en användare med 5 GB avbildningen toopersist din `$Home` directory.</span><span class="sxs-lookup"><span data-stu-id="53186-119">Existing file shares receive a 5-GB user image toopersist your `$Home` directory.</span></span> <span data-ttu-id="53186-120">hello nedrullningsbara menyerna filtreras för molntjänster Shell-region och lokala redundant & geo-redundant storage-konton.</span><span class="sxs-lookup"><span data-stu-id="53186-120">hello drop-down menus are filtered for your Cloud Shell region and for local-redundant & geo-redundant storage accounts.</span></span>

![hello resurs grupp-inställning](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a><span data-ttu-id="53186-122">Begränsa att skapa en resurs med en princip för Azure-resurs</span><span class="sxs-lookup"><span data-stu-id="53186-122">Restrict resource creation with an Azure resource policy</span></span>
<span data-ttu-id="53186-123">Storage-konton som du skapar i molnet Shell märks med `ms-resource-usage:azure-cloud-shell`.</span><span class="sxs-lookup"><span data-stu-id="53186-123">Storage accounts that you create in Cloud Shell are tagged with `ms-resource-usage:azure-cloud-shell`.</span></span> <span data-ttu-id="53186-124">Om du vill toodisallow användare från att skapa storage-konton i molnet Shell, skapa en [Azure resursprincip för taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) som utlöses av den här specifika taggen.</span><span class="sxs-lookup"><span data-stu-id="53186-124">If you want toodisallow users from creating storage accounts in Cloud Shell, create an [Azure resource policy for tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) that are triggered by this specific tag.</span></span>

## <a name="how-cloud-shell-works"></a><span data-ttu-id="53186-125">Så här fungerar Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="53186-125">How Cloud Shell works</span></span>
<span data-ttu-id="53186-126">Molnet Shell kvarstår filer via båda hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="53186-126">Cloud Shell persists files through both of hello following methods:</span></span>
* <span data-ttu-id="53186-127">Skapa en avbildning av din `$Home` directory toopersist alla innehåll i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="53186-127">Creating a disk image of your `$Home` directory toopersist all contents within hello directory.</span></span> <span data-ttu-id="53186-128">hello diskavbildning sparas i angivna filresursen som `acc_<User>.img` på `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, och den automatiskt synkroniserar ändringar.</span><span class="sxs-lookup"><span data-stu-id="53186-128">hello disk image is saved in your specified file share as `acc_<User>.img` at `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, and it automatically syncs changes.</span></span>

* <span data-ttu-id="53186-129">Montera filresursen angivna som `clouddrive` i din `$Home` katalogen för direkta filinteraktion för resursen.</span><span class="sxs-lookup"><span data-stu-id="53186-129">Mounting your specified file share as `clouddrive` in your `$Home` directory for direct file share interaction.</span></span> <span data-ttu-id="53186-130">`/Home/<User>/clouddrive`har mappats för`fileshare.storage.windows.net/fileshare`.</span><span class="sxs-lookup"><span data-stu-id="53186-130">`/Home/<User>/clouddrive` is mapped too`fileshare.storage.windows.net/fileshare`.</span></span>
 
> [!NOTE]
> <span data-ttu-id="53186-131">Alla filer i din `$Home` katalog, t.ex SSH-nycklar finns kvar i din användare diskavbildning som lagras i monterade filresursen.</span><span class="sxs-lookup"><span data-stu-id="53186-131">All files in your `$Home` directory, such as SSH keys, are persisted in your user disk image, which is stored in your mounted file share.</span></span> <span data-ttu-id="53186-132">Tillämpa metodtips när du bevara informationen i dina `$Home` directory och monterade filresurs.</span><span class="sxs-lookup"><span data-stu-id="53186-132">Apply best practices when you persist information in your `$Home` directory and mounted file share.</span></span>

## <a name="use-hello-clouddrive-command"></a><span data-ttu-id="53186-133">Använd hello `clouddrive` kommando</span><span class="sxs-lookup"><span data-stu-id="53186-133">Use hello `clouddrive` command</span></span>
<span data-ttu-id="53186-134">Moln-gränssnittet kan du köra ett kommando som kallas `clouddrive`, vilket aktiverar du toomanually uppdatering hello-filresurs som monterade tooCloud Shell.</span><span class="sxs-lookup"><span data-stu-id="53186-134">With Cloud Shell, you can run a command called `clouddrive`, which enables you toomanually update hello file share that's mounted tooCloud Shell.</span></span>
<span data-ttu-id="53186-135">![Köra hello ”clouddrive” kommando](media/clouddrive-h.png)</span><span class="sxs-lookup"><span data-stu-id="53186-135">![Running hello "clouddrive" command](media/clouddrive-h.png)</span></span>

## <a name="mount-a-new-clouddrive"></a><span data-ttu-id="53186-136">Montera en ny`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="53186-136">Mount a new `clouddrive`</span></span>

### <a name="prerequisites-for-manual-mounting"></a><span data-ttu-id="53186-137">Krav för manuell montering</span><span class="sxs-lookup"><span data-stu-id="53186-137">Prerequisites for manual mounting</span></span>
<span data-ttu-id="53186-138">Du kan uppdatera hello-filresurs som är kopplad till molnet Shell med hjälp av hello `clouddrive mount` kommando.</span><span class="sxs-lookup"><span data-stu-id="53186-138">You can update hello file share that's associated with Cloud Shell by using hello `clouddrive mount` command.</span></span>

<span data-ttu-id="53186-139">Om du monterar en befintlig resurs måste hello storage-konton vara:</span><span class="sxs-lookup"><span data-stu-id="53186-139">If you mount an existing file share, hello storage accounts must be:</span></span>
* <span data-ttu-id="53186-140">Lokalt redundant lagring eller geo-redundant lagring toosupport filresurser.</span><span class="sxs-lookup"><span data-stu-id="53186-140">Locally-redundant storage or geo-redundant storage toosupport file shares.</span></span>
* <span data-ttu-id="53186-141">Finns i din tilldelade region.</span><span class="sxs-lookup"><span data-stu-id="53186-141">Located in your assigned region.</span></span> <span data-ttu-id="53186-142">När du är onboarding hello region tilldelas du toois som anges i hello resursgruppens namn `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="53186-142">When you are onboarding, hello region you are assigned toois listed in hello resource group name `cloud-shell-storage-<region>`.</span></span>

### <a name="supported-storage-regions"></a><span data-ttu-id="53186-143">Regioner som stöds</span><span class="sxs-lookup"><span data-stu-id="53186-143">Supported storage regions</span></span>
<span data-ttu-id="53186-144">hello Azure filer måste finnas i hello samma region som hello molnet Shell-dator som du montera dem till.</span><span class="sxs-lookup"><span data-stu-id="53186-144">hello Azure files must reside in hello same region as hello Cloud Shell machine that you're mounting them to.</span></span> <span data-ttu-id="53186-145">Molnet Shell kluster finns för närvarande i hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="53186-145">Cloud Shell clusters currently exist in hello following regions:</span></span>
|<span data-ttu-id="53186-146">Område</span><span class="sxs-lookup"><span data-stu-id="53186-146">Area</span></span>|<span data-ttu-id="53186-147">Region</span><span class="sxs-lookup"><span data-stu-id="53186-147">Region</span></span>|
|---|---|
|<span data-ttu-id="53186-148">Nord- och Sydamerika</span><span class="sxs-lookup"><span data-stu-id="53186-148">Americas</span></span>|<span data-ttu-id="53186-149">Östra USA, södra centrala USA, västra USA</span><span class="sxs-lookup"><span data-stu-id="53186-149">East US, South Central US, West US</span></span>|
|<span data-ttu-id="53186-150">Europa</span><span class="sxs-lookup"><span data-stu-id="53186-150">Europe</span></span>|<span data-ttu-id="53186-151">Norra Europa, västra Europa</span><span class="sxs-lookup"><span data-stu-id="53186-151">North Europe, West Europe</span></span>|
|<span data-ttu-id="53186-152">Asien och stillahavsområdet</span><span class="sxs-lookup"><span data-stu-id="53186-152">Asia Pacific</span></span>|<span data-ttu-id="53186-153">Indien Central, sydöstra Asien</span><span class="sxs-lookup"><span data-stu-id="53186-153">India Central, Southeast Asia</span></span>|

### <a name="hello-clouddrive-mount-command"></a><span data-ttu-id="53186-154">Hej `clouddrive mount` kommando</span><span class="sxs-lookup"><span data-stu-id="53186-154">hello `clouddrive mount` command</span></span>

> [!NOTE]
> <span data-ttu-id="53186-155">Om du montera en ny filresurs, skapas en ny användaravbildning för din `$Home` katalogen, eftersom den föregående `$Home` avbildningen sparas i hello tidigare filresurs.</span><span class="sxs-lookup"><span data-stu-id="53186-155">If you're mounting a new file share, a new user image is created for your `$Home` directory, because your previous `$Home` image is kept in hello previous file share.</span></span>

<span data-ttu-id="53186-156">Kör hello `clouddrive mount` med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="53186-156">Run hello `clouddrive mount` command with hello following parameters:</span></span>

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

<span data-ttu-id="53186-157">tooview mer information, kör `clouddrive mount -h`, som visas här:</span><span class="sxs-lookup"><span data-stu-id="53186-157">tooview more details, run `clouddrive mount -h`, as shown here:</span></span>

![Körs hello ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a><span data-ttu-id="53186-159">Demontera`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="53186-159">Unmount `clouddrive`</span></span>
<span data-ttu-id="53186-160">Du kan montera en filresurs som monterade tooCloud Shell när som helst.</span><span class="sxs-lookup"><span data-stu-id="53186-160">You can unmount a file share that's mounted tooCloud Shell at any time.</span></span> <span data-ttu-id="53186-161">När filresursen har demonterats, kommer du att ange toomount en ny filresursen tidigare tooyour nästa session.</span><span class="sxs-lookup"><span data-stu-id="53186-161">Once your file share is unmounted, you will be prompted toomount a new file share prior tooyour next session.</span></span>

<span data-ttu-id="53186-162">tooremove en fil dela från molnet Shell:</span><span class="sxs-lookup"><span data-stu-id="53186-162">tooremove a file share from Cloud Shell:</span></span>
1. <span data-ttu-id="53186-163">Kör `clouddrive unmount`.</span><span class="sxs-lookup"><span data-stu-id="53186-163">Run `clouddrive unmount`.</span></span>
2. <span data-ttu-id="53186-164">Bekräfta och bekräfta hello anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="53186-164">Acknowledge and confirm hello prompts.</span></span>

<span data-ttu-id="53186-165">Filresursen fortsätter tooexist om du tar bort den manuellt.</span><span class="sxs-lookup"><span data-stu-id="53186-165">Your file share will continue tooexist unless you delete it manually.</span></span> <span data-ttu-id="53186-166">Molnet Shell efter inte längre filresursen på efterföljande sessioner.</span><span class="sxs-lookup"><span data-stu-id="53186-166">Cloud Shell will no longer search for this file share on subsequent sessions.</span></span>

<span data-ttu-id="53186-167">tooview mer information, kör `clouddrive unmount -h`, som visas här:</span><span class="sxs-lookup"><span data-stu-id="53186-167">tooview more details, run `clouddrive unmount -h`, as shown here:</span></span>

![Körs hello ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> <span data-ttu-id="53186-169">Kör detta kommando raderar inte några resurser.</span><span class="sxs-lookup"><span data-stu-id="53186-169">Running this command will not delete any resources.</span></span> <span data-ttu-id="53186-170">Manuellt tar bort en resursgrupp, storage-konto eller filresurs som är mappad tooCloud Shell tar permanent bort din `$Home` directory avbildningen och andra filer i filresursen.</span><span class="sxs-lookup"><span data-stu-id="53186-170">Manually deleting a resource group, storage account, or file share that is mapped tooCloud Shell will permanently delete your `$Home` directory image and any other files in your file share.</span></span> <span data-ttu-id="53186-171">Det går inte att ångra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="53186-171">This action cannot be undone.</span></span>

## <a name="list-clouddrive-file-shares"></a><span data-ttu-id="53186-172">Lista `clouddrive` filresurser</span><span class="sxs-lookup"><span data-stu-id="53186-172">List `clouddrive` file shares</span></span>
<span data-ttu-id="53186-173">toodiscover vilka filresursen är monterad som `clouddrive`, kör följande hello `df` kommando.</span><span class="sxs-lookup"><span data-stu-id="53186-173">toodiscover which file share is mounted as `clouddrive`, run hello following `df` command.</span></span> 

<span data-ttu-id="53186-174">hello filen sökvägen tooclouddrive visar dina lagringskontonamnet och filresursen i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="53186-174">hello file path tooclouddrive shows your storage account name and file share in hello URL.</span></span> <span data-ttu-id="53186-175">Till exempel, `//storageaccountname.file.core.windows.net/filesharename`</span><span class="sxs-lookup"><span data-stu-id="53186-175">For example, `//storageaccountname.file.core.windows.net/filesharename`</span></span>

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-toocloud-shell"></a><span data-ttu-id="53186-176">Överför lokala filer tooCloud Shell</span><span class="sxs-lookup"><span data-stu-id="53186-176">Transfer local files tooCloud Shell</span></span>
<span data-ttu-id="53186-177">Hej `clouddrive` directory synkroniseras med hello Azure portal lagring-bladet.</span><span class="sxs-lookup"><span data-stu-id="53186-177">hello `clouddrive` directory syncs with hello Azure portal storage blade.</span></span> <span data-ttu-id="53186-178">Använd det här bladet tootransfer lokala filer tooor från filresursen.</span><span class="sxs-lookup"><span data-stu-id="53186-178">Use this blade tootransfer local files tooor from your file share.</span></span> <span data-ttu-id="53186-179">Uppdatera filer i molnet Shell avspeglas i hello fillagring GUI när du uppdaterar hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="53186-179">Updating files from within Cloud Shell is reflected in hello file storage GUI when you refresh hello blade.</span></span>

### <a name="download-files"></a><span data-ttu-id="53186-180">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="53186-180">Download files</span></span>

![Lista över lokala filer](media/download.png)
1. <span data-ttu-id="53186-182">Gå toohello monterade filresurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="53186-182">In hello Azure portal, go toohello mounted file share.</span></span>
2. <span data-ttu-id="53186-183">Välj hello målfilen.</span><span class="sxs-lookup"><span data-stu-id="53186-183">Select hello target file.</span></span>
3. <span data-ttu-id="53186-184">Välj hello **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="53186-184">Select hello **Download** button.</span></span>

### <a name="upload-files"></a><span data-ttu-id="53186-185">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="53186-185">Upload files</span></span>

![Toobe för lokala filer har överförts](media/upload.png)
1. <span data-ttu-id="53186-187">Gå tooyour monterade filresurs.</span><span class="sxs-lookup"><span data-stu-id="53186-187">Go tooyour mounted file share.</span></span>
2. <span data-ttu-id="53186-188">Välj hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="53186-188">Select hello **Upload** button.</span></span>
3. <span data-ttu-id="53186-189">Välj hello eller filer som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="53186-189">Select hello file or files that you want tooupload.</span></span>
4. <span data-ttu-id="53186-190">Bekräfta hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="53186-190">Confirm hello upload.</span></span>

<span data-ttu-id="53186-191">Du bör nu se hello-filer som är tillgängliga i din `clouddrive` katalog i molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="53186-191">You should now see hello files that are accessible in your `clouddrive` directory in Cloud Shell.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53186-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53186-192">Next steps</span></span>
[<span data-ttu-id="53186-193">Molnet Shell Snabbstart</span><span class="sxs-lookup"><span data-stu-id="53186-193">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="53186-194">
[Lär dig mer om Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span><span class="sxs-lookup"><span data-stu-id="53186-194">
[Learn about Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span></span> <br><span data-ttu-id="53186-195">
[Lär dig mer om lagring taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span><span class="sxs-lookup"><span data-stu-id="53186-195">
[Learn about storage tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span></span> <br>
