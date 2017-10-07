---
title: "aaaHow toomanage Azure File storage från hello Azure-portalen | Microsoft Docs"
description: "Lär dig toouse hello Azure portal toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 6ad2fbbf9ee2f86748b1b175d0f63274144dc5eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a><span data-ttu-id="387d2-103">Hur toouse Azure File storage från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="387d2-103">How toouse Azure File storage from hello Azure Portal</span></span>
<span data-ttu-id="387d2-104">Hej [Azure-portalen](https://portal.azure.com) ger ett användargränssnitt för hantering av Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="387d2-104">hello [Azure portal](https://portal.azure.com) provides a user interface for managing Azure File storage.</span></span> <span data-ttu-id="387d2-105">Du kan utföra följande åtgärder från din webbläsare hello:</span><span class="sxs-lookup"><span data-stu-id="387d2-105">You can perform hello following actions from your web browser:</span></span>

* <span data-ttu-id="387d2-106">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="387d2-106">Create a File Share</span></span>
* <span data-ttu-id="387d2-107">Ladda upp och hämta filer tooand från filresursen.</span><span class="sxs-lookup"><span data-stu-id="387d2-107">Upload and download files tooand from your file share.</span></span>
* <span data-ttu-id="387d2-108">Övervaka hello faktiska användningen av varje filresurs.</span><span class="sxs-lookup"><span data-stu-id="387d2-108">Monitor hello actual usage of each file share.</span></span>
* <span data-ttu-id="387d2-109">Justera storlekskvoten för hello-filen.</span><span class="sxs-lookup"><span data-stu-id="387d2-109">Adjust hello file share size quota.</span></span>
* <span data-ttu-id="387d2-110">Kopiera hello montera kommandon toouse toomount din filresurs från en Windows- eller Linux-klient.</span><span class="sxs-lookup"><span data-stu-id="387d2-110">Copy hello mount commands toouse toomount your file share from a Windows or Linux client.</span></span>

## <a name="create-file-share"></a><span data-ttu-id="387d2-111">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="387d2-111">Create file share</span></span>
1. <span data-ttu-id="387d2-112">Logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="387d2-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="387d2-113">Klicka på hello navigeringsmenyn **lagringskonton** eller **Storage-konton (klassiska)**.</span><span class="sxs-lookup"><span data-stu-id="387d2-113">On hello navigation menu, click **Storage accounts** or **Storage accounts (classic)**.</span></span>
    
    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. <span data-ttu-id="387d2-115">Välj ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="387d2-115">Choose your storage account.</span></span>

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. <span data-ttu-id="387d2-117">Välj tjänsten ”Filer”.</span><span class="sxs-lookup"><span data-stu-id="387d2-117">Choose "Files" service.</span></span>

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. <span data-ttu-id="387d2-119">Klicka på ”filresurser” och följ hello länken toocreate din första filresurs.</span><span class="sxs-lookup"><span data-stu-id="387d2-119">Click "File shares" and follow hello link toocreate your first file share.</span></span>

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. <span data-ttu-id="387d2-121">Fyll i hello filresursnamn och hello storleken på hello filen filresursen (upp too5120 GB) toocreate din första filresurs.</span><span class="sxs-lookup"><span data-stu-id="387d2-121">Fill in hello file share name and hello size of hello file share (up too5120 GB) toocreate your first file share.</span></span> <span data-ttu-id="387d2-122">När hello filresurs har skapats, kan du montera den från alla filsystem som stöder SMB 2.1 eller SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="387d2-122">Once hello file share has been created, you can mount it from any file system that supports SMB 2.1 or SMB 3.0.</span></span> <span data-ttu-id="387d2-123">Du kan klicka på **kvot** på hello toochange hello filresursstorleken hello filen upp too5120 GB.</span><span class="sxs-lookup"><span data-stu-id="387d2-123">You could click on **Quota** on hello file share toochange hello size of hello file up too5120 GB.</span></span> <span data-ttu-id="387d2-124">Se för[Priskalkylatorn för Azure](https://azure.microsoft.com/pricing/calculator/) tooestimate hello lagringskostnaden för att använda Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="387d2-124">Please refer too[Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) tooestimate hello storage cost of using Azure File storage.</span></span>

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a><span data-ttu-id="387d2-126">Ladda upp och ladda ned filer</span><span class="sxs-lookup"><span data-stu-id="387d2-126">Upload and download files</span></span>
1. <span data-ttu-id="387d2-127">Välj en filresurs som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="387d2-127">Choose one file share your have already created.</span></span>

    ![Skärmbild som visar hur hello tooupload och hämta filer från portalen](./media/storage-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. <span data-ttu-id="387d2-129">Klicka på **överför** att öppna hello användargränssnittet för filöverföring.</span><span class="sxs-lookup"><span data-stu-id="387d2-129">Click **Upload** to open hello user interface for files uploading.</span></span>

    ![Skärmbild som visar hur tooupload filer från hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a><span data-ttu-id="387d2-131">Ansluta toofile resursen</span><span class="sxs-lookup"><span data-stu-id="387d2-131">Connect toofile share</span></span>
-  <span data-ttu-id="387d2-132">Klicka på **Anslut** att hämta hello kommandoraden för montering hello filresursen från Windows- och Linux.</span><span class="sxs-lookup"><span data-stu-id="387d2-132">Click **Connect** to get hello command line for mounting hello file share from Windows and Linux.</span></span> <span data-ttu-id="387d2-133">För Linux-användare kan du också läsa för[hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md) mer montering information för andra Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="387d2-133">For Linux users, you can also refer too[How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md) for more mounting instructions for other Linux distributions.</span></span>

    ![Skärmbild som visar hur toomount hello filresurs](./media/storage-how-to-use-files-portal/use-files-portal-connect.png)
-  <span data-ttu-id="387d2-135">Du kan kopiera hello kommandon för att montera filen dela på Windows- eller Linux och köra den från du virtuella Azure-datorn eller den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="387d2-135">You can copy hello commands for mounting file share on Windows or Linux and run it from you Azure VM or on-premises machine.</span></span>

    ![Skärmbild som visar hello mount-kommandon för Windows och Linux](./media/storage-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

<span data-ttu-id="387d2-137">**Tips:**</span><span class="sxs-lookup"><span data-stu-id="387d2-137">**Tip:**</span></span>  
<span data-ttu-id="387d2-138">toofind hello åtkomstnyckeln för lagringskontot för montering, klicka på **visa åtkomstnycklar för lagringskontot** längst hello hello ansluta sidan.</span><span class="sxs-lookup"><span data-stu-id="387d2-138">toofind hello storage account access key for mounting, click on **View access keys for this storage account** at hello bottom of hello connect page.</span></span>

## <a name="see-also"></a><span data-ttu-id="387d2-139">Se även</span><span class="sxs-lookup"><span data-stu-id="387d2-139">See also</span></span>
<span data-ttu-id="387d2-140">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="387d2-140">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="387d2-141">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="387d2-141">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="387d2-142">Felsökning i Windows</span><span class="sxs-lookup"><span data-stu-id="387d2-142">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="387d2-143">Felsökning i Linux</span><span class="sxs-lookup"><span data-stu-id="387d2-143">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    