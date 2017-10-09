---
title: "aaaManage Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Hantera Azure Blob-behållare och Blobbar med Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="b4d30-103">Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="b4d30-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="b4d30-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b4d30-104">Overview</span></span>
<span data-ttu-id="b4d30-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b4d30-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span>
<span data-ttu-id="b4d30-106">Du kan använda Blob storage tooexpose data offentligt toohello world eller toostore programdata privat.</span><span class="sxs-lookup"><span data-stu-id="b4d30-106">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="b4d30-107">I den här artikeln lär du dig hur toouse Lagringsutforskaren (förhandsversion) toowork med blobbbehållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="b4d30-107">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4d30-108">Krav</span><span class="sxs-lookup"><span data-stu-id="b4d30-108">Prerequisites</span></span>
<span data-ttu-id="b4d30-109">toocomplete hello stegen i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="b4d30-109">toocomplete hello steps in this article, you'll need hello following:</span></span>

* [<span data-ttu-id="b4d30-110">Hämta och installera Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="b4d30-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="b4d30-111">Ansluta tooa Azure storage-konto eller tjänst</span><span class="sxs-lookup"><span data-stu-id="b4d30-111">Connect tooa Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="b4d30-112">Skapa en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-112">Create a blob container</span></span>
<span data-ttu-id="b4d30-113">Alla blobbar måste finnas i en blobbbehållare som är en logisk gruppering av blobbar.</span><span class="sxs-lookup"><span data-stu-id="b4d30-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="b4d30-114">Ett konto kan innehålla ett obegränsat antal behållare, och varje behållare kan lagra ett obegränsat antal blobbar.</span><span class="sxs-lookup"><span data-stu-id="b4d30-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="b4d30-115">hello följande steg visar hur toocreate en blobbbehållare i Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-115">hello following steps illustrate how toocreate a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="b4d30-116">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-117">Expandera hello storage-konto som du vill toocreate hello blob-behållaren i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-117">In hello left pane, expand hello storage account within which you wish toocreate hello blob container.</span></span>
3. <span data-ttu-id="b4d30-118">Högerklicka på **Blobbbehållare**, och snabbmenyn hello - väljer **skapa Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-118">Right-click **Blob Containers**, and - from hello context menu - select **Create Blob Container**.</span></span>

   ![Skapa snabbmenyn för blob-behållare][0]
4. <span data-ttu-id="b4d30-120">En textruta ska visas under hello **Blobbbehållare** mapp.</span><span class="sxs-lookup"><span data-stu-id="b4d30-120">A text box will appear below hello **Blob Containers** folder.</span></span> <span data-ttu-id="b4d30-121">Ange hello namn för blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-121">Enter hello name for your blob container.</span></span> <span data-ttu-id="b4d30-122">Se hello [behållare namngivningsregler](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) avsnittet för en lista över regler och begränsningar för namngivning av blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-122">See hello [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Skapa Blob-behållare textruta][1]
5. <span data-ttu-id="b4d30-124">Tryck på **RETUR** när du är klar toocreate hello blob-behållaren eller **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="b4d30-124">Press **Enter** when done toocreate hello blob container, or **Esc** toocancel.</span></span> <span data-ttu-id="b4d30-125">När hello blob-behållaren har skapats visas den under hello **Blobbbehållare** mapp för hello markerad storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b4d30-125">Once hello blob container has been successfully created, it will be displayed under hello **Blob Containers** folder for hello selected storage account.</span></span>

   ![BLOB-behållare skapas][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="b4d30-127">Visa innehållet i en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-127">View a blob container's contents</span></span>
<span data-ttu-id="b4d30-128">BLOB-behållare innehåller blobbar och mappar (som kan också innehålla BLOB).</span><span class="sxs-lookup"><span data-stu-id="b4d30-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="b4d30-129">hello följande steg visar hur tooview hello innehållet i en blobbbehållare i Lagringsutforskaren (förhandsversion):</span><span class="sxs-lookup"><span data-stu-id="b4d30-129">hello following steps illustrate how tooview hello contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="b4d30-130">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-131">Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooview hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-131">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="b4d30-132">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-132">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-133">Högerklicka på hello blob-behållare du vill tooview och - snabbmenyn hello - **Öppna Redigeraren för Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-133">Right-click hello blob container you wish tooview, and - from hello context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="b4d30-134">Du kan dubbelklicka på hello blob-behållare som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="b4d30-134">You can also double-click hello blob container you wish tooview.</span></span>

   ![Öppna blob-behållaren editor snabbmenyn][19]
5. <span data-ttu-id="b4d30-136">hello huvudfönstret visar hello blob behållarens innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4d30-136">hello main pane will display hello blob container's contents.</span></span>

   ![Redigeraren för BLOB-behållare][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="b4d30-138">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-138">Delete a blob container</span></span>
<span data-ttu-id="b4d30-139">BLOB-behållare kan enkelt skapas och tas bort efter behov.</span><span class="sxs-lookup"><span data-stu-id="b4d30-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="b4d30-140">(toosee hur toodelete individuella blobbar finns toohello avsnittet [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="b4d30-140">(toosee how toodelete individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="b4d30-141">hello följande steg visar hur toodelete en blobbbehållare i Lagringsutforskaren (förhandsversion):</span><span class="sxs-lookup"><span data-stu-id="b4d30-141">hello following steps illustrate how toodelete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="b4d30-142">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-143">Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooview hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-143">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="b4d30-144">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-144">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-145">Högerklicka på hello blob-behållare du vill toodelete och - snabbmenyn hello - **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-145">Right-click hello blob container you wish toodelete, and - from hello context menu - select **Delete**.</span></span>
   <span data-ttu-id="b4d30-146">Du kan även trycka **ta bort** toodelete hello markerade blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-146">You can also press **Delete** toodelete hello currently selected blob container.</span></span>

   ![Ta bort snabbmenyn för blob-behållare][4]
5. <span data-ttu-id="b4d30-148">Välj **Ja** toohello dialogruta.</span><span class="sxs-lookup"><span data-stu-id="b4d30-148">Select **Yes** toohello confirmation dialog.</span></span>

   ![Ta bort blobben behållare bekräftelse][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="b4d30-150">Kopiera en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-150">Copy a blob container</span></span>
<span data-ttu-id="b4d30-151">Lagringsutforskaren (förhandsversion) kan du toocopy en blob-behållaren toohello Urklipp och klistra in som blob-behållare till ett annat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b4d30-151">Storage Explorer (Preview) enables you toocopy a blob container toohello clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="b4d30-152">(toosee hur toocopy individuella blobbar finns toohello avsnittet [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="b4d30-152">(toosee how toocopy individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="b4d30-153">hello följande steg visar hur toocopy en blob-behållare från en lagring kontot tooanother.</span><span class="sxs-lookup"><span data-stu-id="b4d30-153">hello following steps illustrate how toocopy a blob container from one storage account tooanother.</span></span>

1. <span data-ttu-id="b4d30-154">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-155">Expandera hello storage-konto som innehåller hello blob-behållare som du vill toocopy hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-155">In hello left pane, expand hello storage account containing hello blob container you wish toocopy.</span></span>
3. <span data-ttu-id="b4d30-156">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-156">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-157">Högerklicka på hello blob-behållare du vill toocopy och - snabbmenyn hello - **kopiera Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-157">Right-click hello blob container you wish toocopy, and - from hello context menu - select **Copy Blob Container**.</span></span>

   ![Kopiera blob-behållaren snabbmenyn][6]
5. <span data-ttu-id="b4d30-159">Högerklicka på önskat hello ”mål” storage-konto som du vill toopaste hello blob-behållaren och - snabbmenyn hello - väljer **klistra in Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-159">Right-click hello desired "target" storage account into which you want toopaste hello blob container, and - from hello context menu - select **Paste Blob Container**.</span></span>

   ![Klistra in snabbmenyn för blob-behållare][7]

## <a name="get-hello-sas-for-a-blob-container"></a><span data-ttu-id="b4d30-161">Hämta hello SAS för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-161">Get hello SAS for a blob container</span></span>
<span data-ttu-id="b4d30-162">En [signatur för delad åtkomst (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) ger delegerad åtkomst tooresources i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b4d30-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access tooresources in your storage account.</span></span>
<span data-ttu-id="b4d30-163">Det innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva dela åtkomstnycklarna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b4d30-163">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="b4d30-164">hello följande steg visar hur toocreate en SAS för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="b4d30-164">hello following steps illustrate how toocreate a SAS for a blob container:</span></span>

1. <span data-ttu-id="b4d30-165">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-166">Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooget en SAS hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-166">In hello left pane, expand hello storage account containing hello blob container for which you wish tooget a SAS.</span></span>
3. <span data-ttu-id="b4d30-167">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-167">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-168">Högerklicka på hello önskade blob-behållaren och ange-snabbmenyn hello - **hämta signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-168">Right-click hello desired blob container, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

   ![Hämta SAS snabbmenyn][8]
5. <span data-ttu-id="b4d30-170">I hello **signatur för delad åtkomst** dialogrutan Ange hello princip, start-och förfallodatum, tidszon och åtkomstnivåer som du vill använda för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="b4d30-170">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

   ![Hämta SAS-alternativ][9]
6. <span data-ttu-id="b4d30-172">När du är klar med att ange hello SAS alternativ väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-172">When you're finished specifying hello SAS options, select **Create**.</span></span>
7. <span data-ttu-id="b4d30-173">En andra **signatur för delad åtkomst** därefter dialogrutan visas som visar hello blob-behållaren tillsammans med hello URL och QueryStrings som du kan använda tooaccess hello storage-resursen.</span><span class="sxs-lookup"><span data-stu-id="b4d30-173">A second **Shared Access Signature** dialog will then display that lists hello blob container along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span>
   <span data-ttu-id="b4d30-174">Välj **kopiera** nästa toohello URL som du vill toocopy toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="b4d30-174">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>

   ![Kopiera SAS-URL: er][10]
8. <span data-ttu-id="b4d30-176">När du är klar väljer du **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="b4d30-177">Hantera principer för åtkomst för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="b4d30-178">hello följande steg visar hur toomanage (lägga till och ta bort) åtkomstprinciper för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="b4d30-178">hello following steps illustrate how toomanage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="b4d30-179">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-180">Expandera hello storage-konto som innehåller hello blob-behållare vars åtkomstprinciper som du vill toomanage hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-180">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="b4d30-181">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-181">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-182">Välj hello önskade blob-behållaren och - snabbmenyn hello - **hantera åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-182">Select hello desired blob container, and - from hello context menu - select **Manage Access Policies**.</span></span>

   ![Snabbmeny för hantering av åtkomstprinciper][11]
5. <span data-ttu-id="b4d30-184">Hej **åtkomstprinciper** dialogrutan visar en lista över alla åtkomstprinciper som redan har skapats för hello valda blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-184">hello **Access Policies** dialog will list any access policies already created for hello selected blob container.</span></span>

   ![Alternativ för åtkomstprincipen][12]        
6. <span data-ttu-id="b4d30-186">Följ dessa steg beroende på hello åtkomst princip hanteringsaktivitet:</span><span class="sxs-lookup"><span data-stu-id="b4d30-186">Follow these steps depending on hello access policy management task:</span></span>

   * <span data-ttu-id="b4d30-187">**Lägg till en ny åtkomstprincip** – Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="b4d30-188">När genereras, hello **åtkomstprinciper** dialogrutan visar hello nyligen tillagda princip (med standardinställningarna).</span><span class="sxs-lookup"><span data-stu-id="b4d30-188">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="b4d30-189">**Redigera en åtkomstprincip** - göra alla önskade ändringar, och välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="b4d30-190">**Ta bort en åtkomstprincip** – Välj **ta bort** nästa toohello åtkomstprincip som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="b4d30-190">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

## <a name="set-hello-public-access-level-for-a-blob-container"></a><span data-ttu-id="b4d30-191">Ange hello offentliga åtkomstnivån för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-191">Set hello Public Access Level for a blob container</span></span>
<span data-ttu-id="b4d30-192">Varje blob-behållaren är som standard för ”ingen offentlig åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="b4d30-192">By default, every blob container is set too"No public access".</span></span>

<span data-ttu-id="b4d30-193">hello följande steg visar hur toospecify en offentlig åt för en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-193">hello following steps illustrate how toospecify a public access level for a blob container.</span></span>

1. <span data-ttu-id="b4d30-194">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-195">Expandera hello storage-konto som innehåller hello blob-behållare vars åtkomstprinciper som du vill toomanage hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-195">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="b4d30-196">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-196">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-197">Välj hello önskade blob-behållaren och - snabbmenyn hello - **ange offentliga åtkomstnivå**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-197">Select hello desired blob container, and - from hello context menu - select **Set Public Access Level**.</span></span>

   ![Ange allmän åtkomst på snabbmenyn][13]
5. <span data-ttu-id="b4d30-199">I hello **ange behållaren offentliga åtkomstnivå** dialogrutan Ange hello önskad åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="b4d30-199">In hello **Set Container Public Access Level** dialog, specify hello desired access level.</span></span>

   ![Ställa in allmän åtkomst][14]
6. <span data-ttu-id="b4d30-201">Välj **Använd**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="b4d30-202">Hantera blobbar i en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="b4d30-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="b4d30-203">När du har skapat en blob-behållare kan du överföra en blob toothat blob-behållare, hämta en lokal dator för blob-tooyour, öppna en blob på den lokala datorn och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="b4d30-203">Once you've created a blob container, you can upload a blob toothat blob container, download a blob tooyour local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="b4d30-204">hello följande steg visar hur toomanage hello blobbar (och mappar) i en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="b4d30-204">hello following steps illustrate how toomanage hello blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="b4d30-205">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="b4d30-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="b4d30-206">Expandera hello storage-konto som innehåller hello blob-behållare som du vill toomanage hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b4d30-206">In hello left pane, expand hello storage account containing hello blob container you wish toomanage.</span></span>
3. <span data-ttu-id="b4d30-207">Expandera hello lagringskontots **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-207">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="b4d30-208">Dubbelklicka på hello blob-behållare som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="b4d30-208">Double-click hello blob container you wish tooview.</span></span>
5. <span data-ttu-id="b4d30-209">hello huvudfönstret visar hello blob behållarens innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4d30-209">hello main pane will display hello blob container's contents.</span></span>

   ![Visa blob-behållare][3]
6. <span data-ttu-id="b4d30-211">hello huvudfönstret visar hello blob behållarens innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4d30-211">hello main pane will display hello blob container's contents.</span></span>
7. <span data-ttu-id="b4d30-212">Följ dessa steg beroende på hello uppgiften som du vill att tooperform:</span><span class="sxs-lookup"><span data-stu-id="b4d30-212">Follow these steps depending on hello task you wish tooperform:</span></span>

   * <span data-ttu-id="b4d30-213">**Ladda upp filer tooa blob-behållare**</span><span class="sxs-lookup"><span data-stu-id="b4d30-213">**Upload files tooa blob container**</span></span>

     1. <span data-ttu-id="b4d30-214">Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **Överför filer** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="b4d30-214">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Överföra filer][15]
     2. <span data-ttu-id="b4d30-216">I hello **ladda upp filer** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **filer** textrutan tooselect hello fil(er) som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="b4d30-216">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Ladda upp filer alternativ][16]
     3. <span data-ttu-id="b4d30-218">Ange hello **Blob typen**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-218">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="b4d30-219">hello artikel [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) förklarar hello skillnaderna mellan hello olika blob-typer.</span><span class="sxs-lookup"><span data-stu-id="b4d30-219">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="b4d30-220">Du kan också ange målmapp som hello markerade filerna laddas upp.</span><span class="sxs-lookup"><span data-stu-id="b4d30-220">Optionally, specify a target folder into which hello selected file(s) will be uploaded.</span></span> <span data-ttu-id="b4d30-221">Om hello målmappen inte finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="b4d30-221">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="b4d30-222">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-222">Select **Upload**.</span></span>
   * <span data-ttu-id="b4d30-223">**Ladda upp en mapp tooa blob-behållare**</span><span class="sxs-lookup"><span data-stu-id="b4d30-223">**Upload a folder tooa blob container**</span></span>

     1. <span data-ttu-id="b4d30-224">Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **överför mappen** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="b4d30-224">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Menyn för mappöverföring][17]
     2. <span data-ttu-id="b4d30-226">I hello **överför mappen** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **mappen** text rutan tooselect hello mapp vars innehåll som du vill tooupload.</span><span class="sxs-lookup"><span data-stu-id="b4d30-226">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        ![Överför Mappalternativ][18]
     3. <span data-ttu-id="b4d30-228">Ange hello **Blob typen**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-228">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="b4d30-229">hello artikel [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) förklarar hello skillnaderna mellan hello olika blob-typer.</span><span class="sxs-lookup"><span data-stu-id="b4d30-229">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="b4d30-230">Du kan också ange en målmapp i vilka hello valda mappens innehåll överförs.</span><span class="sxs-lookup"><span data-stu-id="b4d30-230">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="b4d30-231">Om hello målmappen inte finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="b4d30-231">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="b4d30-232">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-232">Select **Upload**.</span></span>
   * <span data-ttu-id="b4d30-233">**Hämta en blob tooyour lokal dator**</span><span class="sxs-lookup"><span data-stu-id="b4d30-233">**Download a blob tooyour local computer**</span></span>

     1. <span data-ttu-id="b4d30-234">Välj hello blob som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="b4d30-234">Select hello blob you wish toodownload.</span></span>
     2. <span data-ttu-id="b4d30-235">Välj hello huvudfönstret i verktygsfältet **hämta**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-235">On hello main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="b4d30-236">I hello **ange där toosave hello hämtade blob** dialogrutan Ange hello plats där du vill att hello blob hämtas och hello namn som du vill toogive den.</span><span class="sxs-lookup"><span data-stu-id="b4d30-236">In hello **Specify where toosave hello downloaded blob** dialog, specify hello location where you want hello blob downloaded, and hello name you wish toogive it.</span></span>  
     4. <span data-ttu-id="b4d30-237">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-237">Select **Save**.</span></span>
   * <span data-ttu-id="b4d30-238">**Öppna en blob på den lokala datorn**</span><span class="sxs-lookup"><span data-stu-id="b4d30-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="b4d30-239">Välj hello blob som du vill tooopen.</span><span class="sxs-lookup"><span data-stu-id="b4d30-239">Select hello blob you wish tooopen.</span></span>
     2. <span data-ttu-id="b4d30-240">Välj hello huvudfönstret i verktygsfältet **öppna**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-240">On hello main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="b4d30-241">hello blob kommer att hämtas och öppnas med hello program som är associerade med hello blob underliggande filtyp.</span><span class="sxs-lookup"><span data-stu-id="b4d30-241">hello blob will be downloaded and opened using hello application associated with hello blob's underlying file type.</span></span>
   * <span data-ttu-id="b4d30-242">**Kopiera en blob toohello Urklipp**</span><span class="sxs-lookup"><span data-stu-id="b4d30-242">**Copy a blob toohello clipboard**</span></span>

     1. <span data-ttu-id="b4d30-243">Välj hello blob som du vill toocopy.</span><span class="sxs-lookup"><span data-stu-id="b4d30-243">Select hello blob you wish toocopy.</span></span>
     2. <span data-ttu-id="b4d30-244">Välj hello huvudfönstret i verktygsfältet **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-244">On hello main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="b4d30-245">Navigera tooanother blob-behållaren i hello till vänster och dubbelklicka på den tooview i hello huvudfönstret.</span><span class="sxs-lookup"><span data-stu-id="b4d30-245">In hello left pane, navigate tooanother blob container, and double-click it tooview it in hello main pane.</span></span>
     4. <span data-ttu-id="b4d30-246">Välj hello huvudfönstret i verktygsfältet **klistra in** toocreate en kopia av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="b4d30-246">On hello main pane's toolbar, select **Paste** toocreate a copy of hello blob.</span></span>
   * <span data-ttu-id="b4d30-247">**Ta bort en blobb**</span><span class="sxs-lookup"><span data-stu-id="b4d30-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="b4d30-248">Välj hello blob som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="b4d30-248">Select hello blob you wish toodelete.</span></span>
     2. <span data-ttu-id="b4d30-249">Välj hello huvudfönstret i verktygsfältet **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b4d30-249">On hello main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="b4d30-250">Välj **Ja** toohello dialogruta.</span><span class="sxs-lookup"><span data-stu-id="b4d30-250">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4d30-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4d30-251">Next steps</span></span>
* <span data-ttu-id="b4d30-252">Visa hello [senaste Lagringsutforskaren (förhandsversion) viktig information och videor](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="b4d30-252">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="b4d30-253">Lär dig hur för[skapa program med Azure-blobbar, tabeller, köer och filer](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="b4d30-253">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
