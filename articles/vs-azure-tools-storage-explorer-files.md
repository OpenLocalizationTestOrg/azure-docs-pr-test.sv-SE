---
title: "aaaUsing Lagringsutforskaren (förhandsversion) med Azure File storage | Microsoft Docs"
description: "Lär dig hur lära dig hur toouse Lagringsutforskaren (förhandsversion) toowork med filen delar och filer."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a><span data-ttu-id="a4fa4-103">Använd Storage Explorer (förhandsutgåva) med Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="a4fa4-103">Using Storage Explorer (Preview) with Azure File storage</span></span>

<span data-ttu-id="a4fa4-104">Azure File storage är en tjänst som erbjuder filen delar in hello moln med hello standardprotokoll för Server Message Block (SMB).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-104">Azure File storage is a service that offers file shares in hello cloud using hello standard Server Message Block (SMB) Protocol.</span></span> <span data-ttu-id="a4fa4-105">Både SMB 2.1 och SMB 3.0 stöds.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-105">Both SMB 2.1 and SMB 3.0 are supported.</span></span> <span data-ttu-id="a4fa4-106">Du kan migrera äldre program som förlitar sig på filen resurser tooAzure snabbt och utan kostsamma omskrivningar med Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-106">With Azure File storage, you can migrate legacy applications that rely on file shares tooAzure quickly and without costly rewrites.</span></span> <span data-ttu-id="a4fa4-107">Du kan använda File storage tooexpose data offentligt toohello world eller toostore programdata privat.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-107">You can use File storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="a4fa4-108">I den här artikeln lär du dig hur toouse Lagringsutforskaren (förhandsversion) toowork med filen delar och filer.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-108">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with file shares and files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4fa4-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a4fa4-109">Prerequisites</span></span>

<span data-ttu-id="a4fa4-110">toocomplete hello stegen i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a4fa4-110">toocomplete hello steps in this article, you'll need hello following:</span></span>

- [<span data-ttu-id="a4fa4-111">Hämta och installera Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="a4fa4-111">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com/)

- [<span data-ttu-id="a4fa4-112">Ansluta tooa Azure storage-konto eller tjänst</span><span class="sxs-lookup"><span data-stu-id="a4fa4-112">Connect tooa Azure storage account or service</span></span>](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a><span data-ttu-id="a4fa4-113">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-113">Create a File Share</span></span>

<span data-ttu-id="a4fa4-114">Alla filer måste finnas i en filresurs, vilket är en logisk gruppering av filer.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-114">All files must reside in a file share, which is simply a logical grouping of files.</span></span> <span data-ttu-id="a4fa4-115">Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-115">An account can contain an unlimited number of file shares, and each share can store an unlimited number of files.</span></span>

<span data-ttu-id="a4fa4-116">hello följande steg visar hur toocreate en filresurs i Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-116">hello following steps illustrate how toocreate a file share within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="a4fa4-117">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-117">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-118">I hello till vänster och expandera hello storage-konto som du vill toocreate hello filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-118">In hello left pane, expand hello storage account within which you wish toocreate hello File Share</span></span>

3. <span data-ttu-id="a4fa4-119">Högerklicka på **filresurser**, och snabbmenyn hello - väljer **skapa filresurs**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-119">Right-click **File Shares**, and - from hello context menu - select **Create File Share**.</span></span>

    ![Skapa en filresurs](media/vs-azure-tools-storage-explorer-files/image1.png)

4. <span data-ttu-id="a4fa4-121">En textruta ska visas under hello **filresurser** mapp.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-121">A text box will appear below hello **File Shares** folder.</span></span> <span data-ttu-id="a4fa4-122">Ange hello namn på filresursen.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-122">Enter hello name for your file share.</span></span> <span data-ttu-id="a4fa4-123">Se hello [dela namngivningsregler](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) avsnittet för en lista över regler och begränsningar för namngivning av filresurser.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-123">See hello [Share naming rules](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) section for a list of rules and restrictions on naming file shares.</span></span>

    ![Namnge hello resursen](media/vs-azure-tools-storage-explorer-files/image2.png)

5. <span data-ttu-id="a4fa4-125">Tryck på **RETUR** när klart toocreate hello filresurs, eller **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-125">Press **Enter** when done toocreate hello file share, or **Esc** toocancel.</span></span> <span data-ttu-id="a4fa4-126">När hello filresurs har skapats visas den under hello **filresurser** mapp för hello markerad storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-126">Once hello file share has been successfully created, it will be displayed under hello **File Shares** folder for hello selected storage account.</span></span>

    ![hello ny resurs](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a><span data-ttu-id="a4fa4-128">Visa innehållet i en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-128">View a file share's contents</span></span>

<span data-ttu-id="a4fa4-129">Filresurser innehåller filer och mappar (som kan också innehålla filer).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-129">File shares contain files and folders (that can also contain files).</span></span>

<span data-ttu-id="a4fa4-130">hello följande steg visar hur tooview hello innehållet i en fil för att dela i Lagringsutforskaren (förhandsversion): +</span><span class="sxs-lookup"><span data-stu-id="a4fa4-130">hello following steps illustrate how tooview hello contents of a file share within Storage Explorer (Preview):+</span></span>

1. <span data-ttu-id="a4fa4-131">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-131">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-132">Expandera hello storage-konto som innehåller hello-filresurs som du vill tooview hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-132">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="a4fa4-133">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-133">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="a4fa4-134">Högerklicka på hello filresursen du vill tooview och - snabbmenyn hello - väljer **öppna**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-134">Right-click hello file share you wish tooview, and - from hello context menu - select **Open**.</span></span> <span data-ttu-id="a4fa4-135">Du kan dubbelklicka på hello-filresurs som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-135">You can also double-click hello file share you wish tooview.</span></span>

    ![Öppna resursen](media/vs-azure-tools-storage-explorer-files/image4.png)

5. <span data-ttu-id="a4fa4-137">hello huvudfönstret visar hello filresursens innehållet.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-137">hello main pane will display hello file share's contents.</span></span>
    
    ![Hej resursens innehållet](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a><span data-ttu-id="a4fa4-139">Ta bort en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-139">Delete a file share</span></span>

<span data-ttu-id="a4fa4-140">Filresurser kan enkelt skapas och tas bort efter behov.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-140">File shares can be easily created and deleted as needed.</span></span> <span data-ttu-id="a4fa4-141">(toosee hur toodelete enskilda filer, se avsnittet toohello [hantera filer i en filresurs](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="a4fa4-141">(toosee how toodelete individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="a4fa4-142">hello följande steg visar hur toodelete en filresurs i Lagringsutforskaren (förhandsversion):</span><span class="sxs-lookup"><span data-stu-id="a4fa4-142">hello following steps illustrate how toodelete a file share within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="a4fa4-143">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-143">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-144">Expandera hello storage-konto som innehåller hello-filresurs som du vill tooview hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-144">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="a4fa4-145">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-145">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="a4fa4-146">Högerklicka på hello filresursen du vill toodelete och - snabbmenyn hello - väljer **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-146">Right-click hello file share you wish toodelete, and - from hello context menu - select **Delete**.</span></span> <span data-ttu-id="a4fa4-147">Du kan även trycka **ta bort** toodelete hello markerade filresurs.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-147">You can also press **Delete** toodelete hello currently selected file share.</span></span>

    ![Ta bort](media/vs-azure-tools-storage-explorer-files/image6.png)

5. <span data-ttu-id="a4fa4-149">Välj **Ja** toohello dialogruta.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-149">Select **Yes** toohello confirmation dialog.</span></span>
    
    ![Bekräftelsedialog](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a><span data-ttu-id="a4fa4-151">Kopiera en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-151">Copy a file share</span></span>

<span data-ttu-id="a4fa4-152">Lagringsutforskaren (förhandsversion) kan du toocopy en filresursen toohello Urklipp och klistra in den filresursen i ett annat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-152">Storage Explorer (Preview) enables you toocopy a file share toohello clipboard, and then paste that file share into another storage account.</span></span> <span data-ttu-id="a4fa4-153">(toosee hur toocopy enskilda filer, se avsnittet toohello [hantera filer i en filresurs](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="a4fa4-153">(toosee how toocopy individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="a4fa4-154">hello följande steg visar hur toocopy en fil för att dela från en tooanother för storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-154">hello following steps illustrate how toocopy a file share from one storage account tooanother.</span></span>

1. <span data-ttu-id="a4fa4-155">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-155">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-156">Expandera hello storage-konto som innehåller hello-filresurs som du vill toocopy hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-156">In hello left pane, expand hello storage account containing hello file share you wish toocopy.</span></span>

3. <span data-ttu-id="a4fa4-157">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-157">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="a4fa4-158">Högerklicka på hello filresursen du vill toocopy och - snabbmenyn hello - väljer **kopia av filresursen**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-158">Right-click hello file share you wish toocopy, and - from hello context menu - select **Copy File Share**.</span></span>

    ![Kopiera filresurs](media/vs-azure-tools-storage-explorer-files/image8.png)

5. <span data-ttu-id="a4fa4-160">Högerklicka på önskat hello ”mål” storage-konto som du vill toopaste hello filresursen och - snabbmenyn hello - väljer **klistra in filresursen**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-160">Right-click hello desired "target" storage account into which you want toopaste hello file share, and - from hello context menu - select **Paste File Share**.</span></span>

    ![Klistra in filresurs](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a><span data-ttu-id="a4fa4-162">Hämta hello SAS för en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-162">Get hello SAS for a file share</span></span>

<span data-ttu-id="a4fa4-163">En [signatur för delad åtkomst (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) ger delegerad åtkomst tooresources i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-163">A [shared access signature (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) provides delegated access tooresources in your storage account.</span></span> <span data-ttu-id="a4fa4-164">Det innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva tooshare åtkomstnycklarna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-164">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having tooshare your account access keys.</span></span>

<span data-ttu-id="a4fa4-165">hello följande steg visar hur toocreate en SAS för en fil för att dela: +</span><span class="sxs-lookup"><span data-stu-id="a4fa4-165">hello following steps illustrate how toocreate a SAS for a file share:+</span></span>

1. <span data-ttu-id="a4fa4-166">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-166">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-167">Expandera hello storage-konto som innehåller hello filresurs som du vill tooget en SAS hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-167">In hello left pane, expand hello storage account containing hello file share for which you wish tooget a SAS.</span></span>

3. <span data-ttu-id="a4fa4-168">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-168">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="a4fa4-169">Högerklicka på hello önskade filresursen och - snabbmenyn hello - **hämta signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-169">Right-click hello desired file share, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

    ![Hämta signatur för delad åtkomst](media/vs-azure-tools-storage-explorer-files/image10.png)

5. <span data-ttu-id="a4fa4-171">I hello **signatur för delad åtkomst** dialogrutan Ange hello princip, start-och förfallodatum, tidszon och åtkomstnivåer som du vill använda för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-171">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

    ![SAS-dialog](media/vs-azure-tools-storage-explorer-files/image11.png)

6. <span data-ttu-id="a4fa4-173">När du är klar med att ange hello SAS alternativ väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-173">When you're finished specifying hello SAS options, select **Create**.</span></span>

7. <span data-ttu-id="a4fa4-174">En andra **signatur för delad åtkomst** därefter dialogrutan visas som visar hello filresurs tillsammans med hello URL och QueryStrings som du kan använda tooaccess hello storage-resursen.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-174">A second **Shared Access Signature** dialog will then display that lists hello file share along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span> <span data-ttu-id="a4fa4-175">Välj **kopiera** nästa toohello URL som du vill toocopy toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-175">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>
    
    ![Andra SAS-dialog](media/vs-azure-tools-storage-explorer-files/image12.png)

8. <span data-ttu-id="a4fa4-177">När du är klar väljer du **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-177">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-file-share"></a><span data-ttu-id="a4fa4-178">Hantera åtkomstprinciper för en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-178">Manage Access Policies for a file share</span></span>

<span data-ttu-id="a4fa4-179">hello följande steg visar hur toomanage (lägga till och ta bort) åtkomstprinciper för en filresurs: +.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-179">hello following steps illustrate how toomanage (add and remove) access policies for a file share:+ .</span></span> <span data-ttu-id="a4fa4-180">hello åtkomstprinciper används för att skapa SAS-URL: er som användare kan använda tooaccess hello Storage-resurs under en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-180">hello Access Policies is used for creating SAS URLs through which people can use tooaccess hello Storage File resource during a defined period of time.</span></span>

1. <span data-ttu-id="a4fa4-181">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-181">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="a4fa4-182">Expandera hello storage-konto som innehåller hello filresurs vars åtkomstprinciper som du vill toomanage hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-182">In hello left pane, expand hello storage account containing hello file share whose access policies you wish toomanage.</span></span>

3. <span data-ttu-id="a4fa4-183">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-183">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="a4fa4-184">Välj önskad hello-filresurs och - snabbmenyn hello - **hantera åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-184">Select hello desired file share, and - from hello context menu - select **Manage Access Policies**.</span></span>

    ![Snabbmeny för hantering av åtkomstprinciper](media/vs-azure-tools-storage-explorer-files/image13.png)

5. <span data-ttu-id="a4fa4-186">Hej **åtkomstprinciper** dialogrutan visar en lista över alla åtkomstprinciper som redan har skapats för hello valda filresurs.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-186">hello **Access Policies** dialog will list any access policies already created for hello selected file share.</span></span>
    
    ![Åtkomstprinciper](media/vs-azure-tools-storage-explorer-files/image14.png)

6. <span data-ttu-id="a4fa4-188">Följ dessa steg beroende på hello åtkomst princip hanteringsaktivitet:</span><span class="sxs-lookup"><span data-stu-id="a4fa4-188">Follow these steps depending on hello access policy management task:</span></span>
    
    - <span data-ttu-id="a4fa4-189">**Lägg till en ny åtkomstprincip** – Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-189">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="a4fa4-190">När genereras, hello **åtkomstprinciper** dialogrutan visar hello nyligen tillagda princip (med standardinställningarna).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-190">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>

    - <span data-ttu-id="a4fa4-191">**Redigera en åtkomstprincip** – Gör önskade redigeringar och välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-191">**Edit an access policy** - Make any desired edits, and select **Save**.</span></span>

    - <span data-ttu-id="a4fa4-192">**Ta bort en åtkomstprincip** – Välj **ta bort** nästa toohello åtkomstprincip som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-192">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

7. <span data-ttu-id="a4fa4-193">Skapa en ny SAS-URL med hello åtkomstprincip som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="a4fa4-193">Create a new SAS URL using hello Access Policy you created earlier:</span></span>
    
    ![Hämta SAS](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![SAS-namn och -egenskaper](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a><span data-ttu-id="a4fa4-196">Hantera filer i en filresurs</span><span class="sxs-lookup"><span data-stu-id="a4fa4-196">Managing files in a file share</span></span>

<span data-ttu-id="a4fa4-197">När du har skapat en filresurs kan du överföra en fil toothat filresurs, ladda ned en fil tooyour lokal dator, öppna en fil på den lokala datorn och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-197">Once you've created a file share, you can upload a file toothat file share, download a file tooyour local computer, open a file on your local computer, and much more.</span></span>

<span data-ttu-id="a4fa4-198">hello visar följande steg hur toomanage hello filer (och mappar) i en fil för att dela.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-198">hello following steps illustrate how toomanage hello files (and folders) within a file share.</span></span>

1.  <span data-ttu-id="a4fa4-199">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-199">Open Storage Explorer (Preview).</span></span>

2.  <span data-ttu-id="a4fa4-200">Expandera hello storage-konto som innehåller hello-filresurs som du vill toomanage hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-200">In hello left pane, expand hello storage account containing hello file share you wish toomanage.</span></span>

3.  <span data-ttu-id="a4fa4-201">Expandera hello lagringskontots **filresurser**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-201">Expand hello storage account's **File Shares**.</span></span>

4.  <span data-ttu-id="a4fa4-202">Dubbelklicka på hello-filresurs som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-202">Double-click hello file share you wish tooview.</span></span>

5.  <span data-ttu-id="a4fa4-203">hello huvudfönstret visar hello filresursens innehållet.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-203">hello main pane will display hello file share's contents.</span></span>

    ![Hej resursens innehållet](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  <span data-ttu-id="a4fa4-205">hello huvudfönstret visar hello filresursens innehållet.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-205">hello main pane will display hello file share's contents.</span></span>

7.  <span data-ttu-id="a4fa4-206">Följ dessa steg beroende på hello uppgiften som du vill att tooperform:</span><span class="sxs-lookup"><span data-stu-id="a4fa4-206">Follow these steps depending on hello task you wish tooperform:</span></span>

    - <span data-ttu-id="a4fa4-207">**Ladda upp filer tooa filresurs**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-207">**Upload files tooa file share**</span></span>

        <span data-ttu-id="a4fa4-208">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-208">a.</span></span>  <span data-ttu-id="a4fa4-209">Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **Överför filer** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-209">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Överföra filer](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        <span data-ttu-id="a4fa4-211">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-211">b.</span></span> <span data-ttu-id="a4fa4-212">I hello **ladda upp filer** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **filer** textrutan tooselect hello fil(er) som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-212">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Lägga till filer](media/vs-azure-tools-storage-explorer-files/image19.png)

        <span data-ttu-id="a4fa4-214">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-214">c.</span></span> <span data-ttu-id="a4fa4-215">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-215">Select **Upload**.</span></span>

    - <span data-ttu-id="a4fa4-216">**Ladda upp en mapp tooa filresurs**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-216">**Upload a folder tooa file share**</span></span>
        
        <span data-ttu-id="a4fa4-217">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-217">a.</span></span> <span data-ttu-id="a4fa4-218">Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **överför mappen** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-218">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Menyn för mappöverföring](media/vs-azure-tools-storage-explorer-files/image20.png)

        <span data-ttu-id="a4fa4-220">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-220">b.</span></span> <span data-ttu-id="a4fa4-221">I hello **överför mappen** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **mappen** text rutan tooselect hello mapp vars innehåll som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-221">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        <span data-ttu-id="a4fa4-222">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-222">c.</span></span> <span data-ttu-id="a4fa4-223">Du kan också ange en målmapp i vilka hello valda mappens innehåll överförs.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-223">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="a4fa4-224">Om hello målmappen inte finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-224">If hello target folder doesn’t exist, it will be created.</span></span>

        <span data-ttu-id="a4fa4-225">d.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-225">d.</span></span> <span data-ttu-id="a4fa4-226">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-226">Select **Upload**.</span></span>

    - <span data-ttu-id="a4fa4-227">**Hämta en fil tooyour lokal dator**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-227">**Download a file tooyour local computer**</span></span>
        
        <span data-ttu-id="a4fa4-228">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-228">a.</span></span> <span data-ttu-id="a4fa4-229">Välj hello-fil som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-229">Select hello file you wish toodownload.</span></span>
        
        <span data-ttu-id="a4fa4-230">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-230">b.</span></span> <span data-ttu-id="a4fa4-231">Välj hello huvudfönstret i verktygsfältet **hämta**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-231">On hello main pane's toolbar, select **Download**.</span></span>
        
        <span data-ttu-id="a4fa4-232">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-232">c.</span></span> <span data-ttu-id="a4fa4-233">I hello **ange där toosave hello hämtade filen** dialogrutan Ange hello plats där du vill att hello-fil som hämtas och hello namn som du vill toogive den.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-233">In hello **Specify where toosave hello downloaded file** dialog, specify hello location where you want hello file downloaded, and hello name you wish toogive it.</span></span>

        <span data-ttu-id="a4fa4-234">d.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-234">d.</span></span> <span data-ttu-id="a4fa4-235">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-235">Select **Save**.</span></span>

    - <span data-ttu-id="a4fa4-236">**Öppna en fil på den lokala datorn**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-236">**Open a file on your local computer**</span></span>
        
        <span data-ttu-id="a4fa4-237">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-237">a.</span></span>  <span data-ttu-id="a4fa4-238">Välj hello-fil som du vill tooopen.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-238">Select hello file you wish tooopen.</span></span>
        
        <span data-ttu-id="a4fa4-239">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-239">b.</span></span>  <span data-ttu-id="a4fa4-240">Välj hello huvudfönstret i verktygsfältet **öppna**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-240">On hello main pane's toolbar, select **Open**.</span></span>
        
        <span data-ttu-id="a4fa4-241">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-241">c.</span></span>  <span data-ttu-id="a4fa4-242">hello-filen kommer att hämtas och öppnas med hello program som är associerade med hello filens underliggande filtyp.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-242">hello file will be downloaded and opened using hello application associated with hello file's underlying file type.</span></span>

    - <span data-ttu-id="a4fa4-243">**Kopiera en fil toohello Urklipp**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-243">**Copy a file toohello clipboard**</span></span>

        <span data-ttu-id="a4fa4-244">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-244">a.</span></span> <span data-ttu-id="a4fa4-245">Välj hello-fil som du vill toocopy.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-245">Select hello file you wish toocopy.</span></span>

        <span data-ttu-id="a4fa4-246">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-246">b.</span></span> <span data-ttu-id="a4fa4-247">Välj hello huvudfönstret i verktygsfältet **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-247">On hello main pane's toolbar, select **Copy**.</span></span>

        <span data-ttu-id="a4fa4-248">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-248">c.</span></span> <span data-ttu-id="a4fa4-249">Navigera tooanother filresurs hello vänster och dubbelklicka på den tooview i hello huvudfönstret.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-249">In hello left pane, navigate tooanother file share, and double-click it tooview it in hello main pane.</span></span>

        <span data-ttu-id="a4fa4-250">d.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-250">d.</span></span> <span data-ttu-id="a4fa4-251">Välj hello huvudfönstret i verktygsfältet **klistra in** toocreate en kopia av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-251">On hello main pane's toolbar, select **Paste** toocreate a copy of hello file.</span></span>

    - <span data-ttu-id="a4fa4-252">**Ta bort en fil**</span><span class="sxs-lookup"><span data-stu-id="a4fa4-252">**Delete a file**</span></span>

        <span data-ttu-id="a4fa4-253">a.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-253">a.</span></span> <span data-ttu-id="a4fa4-254">Välj hello-fil som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-254">Select hello file you wish toodelete.</span></span>

        <span data-ttu-id="a4fa4-255">b.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-255">b.</span></span> <span data-ttu-id="a4fa4-256">Välj hello huvudfönstret i verktygsfältet **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-256">On hello main pane's toolbar, select **Delete**.</span></span>

        <span data-ttu-id="a4fa4-257">c.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-257">c.</span></span> <span data-ttu-id="a4fa4-258">Välj **Ja** toohello dialogruta.</span><span class="sxs-lookup"><span data-stu-id="a4fa4-258">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4fa4-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4fa4-259">Next steps</span></span>

- <span data-ttu-id="a4fa4-260">Visa hello [senaste Lagringsutforskaren (förhandsversion) viktig information och videor](http://www.storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-260">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com/).</span></span>

- <span data-ttu-id="a4fa4-261">Lär dig hur för[skapa program med Azure-blobbar, tabeller, köer och filer](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="a4fa4-261">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>
