---
title: "Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
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
ms.openlocfilehash: f833027203441e12340bd93f3570de073d297223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="2681a-103">Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="2681a-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="2681a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2681a-104">Overview</span></span>
<span data-ttu-id="2681a-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i världen via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2681a-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span>
<span data-ttu-id="2681a-106">Du kan använda Blob Storage för att exponera data offentligt eller lagra programdata privat.</span><span class="sxs-lookup"><span data-stu-id="2681a-106">You can use Blob storage to expose data publicly to the world, or to store application data privately.</span></span> <span data-ttu-id="2681a-107">I den här artikeln lär du dig att använda Lagringsutforskaren (förhandsversion) att arbeta med blob-behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="2681a-107">In this article, you'll learn how to use Storage Explorer (Preview) to work with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2681a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="2681a-108">Prerequisites</span></span>
<span data-ttu-id="2681a-109">Du behöver följande för att slutföra stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="2681a-109">To complete the steps in this article, you'll need the following:</span></span>

* [<span data-ttu-id="2681a-110">Hämta och installera Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="2681a-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="2681a-111">Ansluta till ett Azure-lagringskonto eller en Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="2681a-111">Connect to a Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="2681a-112">Skapa en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="2681a-112">Create a blob container</span></span>
<span data-ttu-id="2681a-113">Alla blobbar måste finnas i en blobbbehållare som är en logisk gruppering av blobbar.</span><span class="sxs-lookup"><span data-stu-id="2681a-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="2681a-114">Ett konto kan innehålla ett obegränsat antal behållare, och varje behållare kan lagra ett obegränsat antal blobbar.</span><span class="sxs-lookup"><span data-stu-id="2681a-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="2681a-115">Följande steg visar hur du skapar en blobbbehållare i Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-115">The following steps illustrate how to create a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="2681a-116">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-117">Expandera det lagringskonto där du vill skapa blob-behållaren i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="2681a-117">In the left pane, expand the storage account within which you wish to create the blob container.</span></span>
3. <span data-ttu-id="2681a-118">Högerklicka på **Blobbbehållare**, och på snabbmenyn - väljer **skapa Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="2681a-118">Right-click **Blob Containers**, and - from the context menu - select **Create Blob Container**.</span></span>

   ![Skapa snabbmenyn för blob-behållare][0]
4. <span data-ttu-id="2681a-120">En textruta ska visas under den **Blobbbehållare** mapp.</span><span class="sxs-lookup"><span data-stu-id="2681a-120">A text box will appear below the **Blob Containers** folder.</span></span> <span data-ttu-id="2681a-121">Ange namn för blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="2681a-121">Enter the name for your blob container.</span></span> <span data-ttu-id="2681a-122">Finns det [behållare namngivningsregler](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) avsnittet för en lista över regler och begränsningar för namngivning av blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="2681a-122">See the [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Skapa Blob-behållare textruta][1]
5. <span data-ttu-id="2681a-124">Tryck på **RETUR** när du är klar för att skapa blob-behållare eller **Esc** att avbryta.</span><span class="sxs-lookup"><span data-stu-id="2681a-124">Press **Enter** when done to create the blob container, or **Esc** to cancel.</span></span> <span data-ttu-id="2681a-125">När blob-behållaren har skapats visas den den **Blobbbehållare** mapp för det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="2681a-125">Once the blob container has been successfully created, it will be displayed under the **Blob Containers** folder for the selected storage account.</span></span>

   ![BLOB-behållare skapas][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="2681a-127">Visa innehållet i en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="2681a-127">View a blob container's contents</span></span>
<span data-ttu-id="2681a-128">BLOB-behållare innehåller blobbar och mappar (som kan också innehålla BLOB).</span><span class="sxs-lookup"><span data-stu-id="2681a-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="2681a-129">Följande steg visar hur du visar innehållet i en blobbbehållare i Lagringsutforskaren (förhandsversion):</span><span class="sxs-lookup"><span data-stu-id="2681a-129">The following steps illustrate how to view the contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="2681a-130">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-131">Expandera det lagringskonto som innehåller blob-behållare du vill visa i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="2681a-131">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="2681a-132">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-132">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-133">Högerklicka på blob-behållare som du vill visa, och på snabbmenyn - väljer **Öppna Redigeraren för Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="2681a-133">Right-click the blob container you wish to view, and - from the context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="2681a-134">Du kan dubbelklicka på blob-behållare som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="2681a-134">You can also double-click the blob container you wish to view.</span></span>

   ![Öppna blob-behållaren editor snabbmenyn][19]
5. <span data-ttu-id="2681a-136">I huvudfönstret visas innehållet för blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2681a-136">The main pane will display the blob container's contents.</span></span>

   ![Redigeraren för BLOB-behållare][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="2681a-138">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="2681a-138">Delete a blob container</span></span>
<span data-ttu-id="2681a-139">BLOB-behållare kan enkelt skapas och tas bort efter behov.</span><span class="sxs-lookup"><span data-stu-id="2681a-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="2681a-140">(Att se hur finns i avsnittet om du vill ta bort enskilda blobbar [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="2681a-140">(To see how to delete individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="2681a-141">Följande steg visar hur du tar bort en blobbbehållare i Lagringsutforskaren (förhandsversion):</span><span class="sxs-lookup"><span data-stu-id="2681a-141">The following steps illustrate how to delete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="2681a-142">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-143">Expandera det lagringskonto som innehåller blob-behållare du vill visa i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="2681a-143">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="2681a-144">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-144">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-145">Högerklicka på blob-behållare som du vill ta bort, och på snabbmenyn - väljer **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2681a-145">Right-click the blob container you wish to delete, and - from the context menu - select **Delete**.</span></span>
   <span data-ttu-id="2681a-146">Du kan även trycka **ta bort** att ta bort markerade blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2681a-146">You can also press **Delete** to delete the currently selected blob container.</span></span>

   ![Ta bort snabbmenyn för blob-behållare][4]
5. <span data-ttu-id="2681a-148">Välj **Ja** i bekräftelsedialogen.</span><span class="sxs-lookup"><span data-stu-id="2681a-148">Select **Yes** to the confirmation dialog.</span></span>

   ![Ta bort blobben behållare bekräftelse][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="2681a-150">Kopiera en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="2681a-150">Copy a blob container</span></span>
<span data-ttu-id="2681a-151">Lagringsutforskaren (förhandsversion) kan du kopiera en blob-behållare till Urklipp och klistra in den blob-behållaren i ett annat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2681a-151">Storage Explorer (Preview) enables you to copy a blob container to the clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="2681a-152">(Se hur du kopiera enskilda blobbar, finns i avsnittet [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="2681a-152">(To see how to copy individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="2681a-153">Följande steg visar hur du kopierar en blob-behållare från ett lagringskonto till en annan.</span><span class="sxs-lookup"><span data-stu-id="2681a-153">The following steps illustrate how to copy a blob container from one storage account to another.</span></span>

1. <span data-ttu-id="2681a-154">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-155">I den vänstra rutan, expanderar du lagringskontot som innehåller blob-behållare du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="2681a-155">In the left pane, expand the storage account containing the blob container you wish to copy.</span></span>
3. <span data-ttu-id="2681a-156">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-156">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-157">Högerklicka på blob-behållare som du vill kopiera, och på snabbmenyn - väljer **kopiera Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="2681a-157">Right-click the blob container you wish to copy, and - from the context menu - select **Copy Blob Container**.</span></span>

   ![Kopiera blob-behållaren snabbmenyn][6]
5. <span data-ttu-id="2681a-159">Högerklicka på önskat ”mål” storage-konto som du vill klistra in blob-behållare, och på snabbmenyn - väljer **klistra in Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="2681a-159">Right-click the desired "target" storage account into which you want to paste the blob container, and - from the context menu - select **Paste Blob Container**.</span></span>

   ![Klistra in snabbmenyn för blob-behållare][7]

## <a name="get-the-sas-for-a-blob-container"></a><span data-ttu-id="2681a-161">Hämta SAS för en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="2681a-161">Get the SAS for a blob container</span></span>
<span data-ttu-id="2681a-162">En [signatur för delad åtkomst (Shared Access Signature, SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) ger delegerad åtkomst till resurser på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2681a-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access to resources in your storage account.</span></span>
<span data-ttu-id="2681a-163">Det innebär att du kan ge en klient begränsad behörighet till objekt på ditt lagringskonto under en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva dela nycklarna för åtkomst till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2681a-163">This means that you can grant a client limited permissions to objects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="2681a-164">Följande steg visar hur du skapar en SAS för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="2681a-164">The following steps illustrate how to create a SAS for a blob container:</span></span>

1. <span data-ttu-id="2681a-165">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-166">I den vänstra rutan, expanderar du lagringskontot som innehåller blob-behållare som du vill hämta en SAS.</span><span class="sxs-lookup"><span data-stu-id="2681a-166">In the left pane, expand the storage account containing the blob container for which you wish to get a SAS.</span></span>
3. <span data-ttu-id="2681a-167">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-167">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-168">Högerklicka på den önskade blobbehållaren, och på snabbmenyn - väljer **hämta signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="2681a-168">Right-click the desired blob container, and - from the context menu - select **Get Shared Access Signature**.</span></span>

   ![Hämta SAS snabbmenyn][8]
5. <span data-ttu-id="2681a-170">I dialogrutan **Signatur för delad åtkomst** anger du princip, start- och förfallodatum, tidszon och åtkomstnivåer som du vill använda för resursen.</span><span class="sxs-lookup"><span data-stu-id="2681a-170">In the **Shared Access Signature** dialog, specify the policy, start and expiration dates, time zone, and access levels you want for the resource.</span></span>

   ![Hämta SAS-alternativ][9]
6. <span data-ttu-id="2681a-172">När du är klar med att ange SAS-alternativen väljer du **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2681a-172">When you're finished specifying the SAS options, select **Create**.</span></span>
7. <span data-ttu-id="2681a-173">En andra **signatur för delad åtkomst** dialogrutan därefter visas med en lista över blob-behållaren tillsammans med URL och QueryStrings som du kan använda för att komma åt storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="2681a-173">A second **Shared Access Signature** dialog will then display that lists the blob container along with the URL and QueryStrings you can use to access the storage resource.</span></span>
   <span data-ttu-id="2681a-174">Välj **Kopiera** bredvid den URL som du vill kopiera till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="2681a-174">Select **Copy** next to the URL you wish to copy to the clipboard.</span></span>

   ![Kopiera SAS-URL: er][10]
8. <span data-ttu-id="2681a-176">När du är klar väljer du **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="2681a-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="2681a-177">Hantera principer för åtkomst för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="2681a-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="2681a-178">Nedan visas hur du hanterar (lägga till och ta bort) åtkomstprinciper för en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="2681a-178">The following steps illustrate how to manage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="2681a-179">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-180">I den vänstra rutan, expanderar du lagringskontot som innehåller blob-behållaren vars åtkomstprinciper som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="2681a-180">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="2681a-181">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-181">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-182">Välj önskad blob-behållaren och - snabbmenyn - **hantera åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="2681a-182">Select the desired blob container, and - from the context menu - select **Manage Access Policies**.</span></span>

   ![Snabbmeny för hantering av åtkomstprinciper][11]
5. <span data-ttu-id="2681a-184">Den **åtkomstprinciper** dialogrutan visar en lista över alla åtkomstprinciper som redan har skapats för valda blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2681a-184">The **Access Policies** dialog will list any access policies already created for the selected blob container.</span></span>

   ![Alternativ för åtkomstprincipen][12]        
6. <span data-ttu-id="2681a-186">Gör följande beroende på åtkomstprincipens hanteringsuppgift:</span><span class="sxs-lookup"><span data-stu-id="2681a-186">Follow these steps depending on the access policy management task:</span></span>

   * <span data-ttu-id="2681a-187">**Lägg till en ny åtkomstprincip** – Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2681a-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="2681a-188">När åtkomstprinciepn har skapats visas den som nyligen tillagd i dialogen **Åtkomstprinciper** (med standardinställningarna).</span><span class="sxs-lookup"><span data-stu-id="2681a-188">Once generated, the **Access Policies** dialog will display the newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="2681a-189">**Redigera en åtkomstprincip** - göra alla önskade ändringar, och välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="2681a-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="2681a-190">**Ta bort en åtkomstprincip** – Välj **Ta bort** bredvid den åtkomstprincip som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="2681a-190">**Remove an access policy** - Select **Remove** next to the access policy you wish to remove.</span></span>

## <a name="set-the-public-access-level-for-a-blob-container"></a><span data-ttu-id="2681a-191">Att ställa in allmän åtkomst för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="2681a-191">Set the Public Access Level for a blob container</span></span>
<span data-ttu-id="2681a-192">Varje blob-behållaren är som standard ”ingen offentlig åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="2681a-192">By default, every blob container is set to "No public access".</span></span>

<span data-ttu-id="2681a-193">Följande steg visar hur du anger en offentlig åtkomstnivån för en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="2681a-193">The following steps illustrate how to specify a public access level for a blob container.</span></span>

1. <span data-ttu-id="2681a-194">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-195">I den vänstra rutan, expanderar du lagringskontot som innehåller blob-behållaren vars åtkomstprinciper som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="2681a-195">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="2681a-196">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-196">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-197">Välj önskad blob-behållaren och - snabbmenyn - **ange offentliga åtkomstnivå**.</span><span class="sxs-lookup"><span data-stu-id="2681a-197">Select the desired blob container, and - from the context menu - select **Set Public Access Level**.</span></span>

   ![Ange allmän åtkomst på snabbmenyn][13]
5. <span data-ttu-id="2681a-199">I den **ange behållaren offentliga åtkomstnivå** dialogrutan Ange önskad åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="2681a-199">In the **Set Container Public Access Level** dialog, specify the desired access level.</span></span>

   ![Ställa in allmän åtkomst][14]
6. <span data-ttu-id="2681a-201">Välj **Använd**.</span><span class="sxs-lookup"><span data-stu-id="2681a-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="2681a-202">Hantera blobbar i en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="2681a-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="2681a-203">När du har skapat en blob-behållare kan du ladda upp en blobb till att blob-behållare, ladda ned en blob till den lokala datorn, öppna en blob på den lokala datorn och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="2681a-203">Once you've created a blob container, you can upload a blob to that blob container, download a blob to your local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="2681a-204">Följande steg visar hur du hanterar blobbar (och mappar) i en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="2681a-204">The following steps illustrate how to manage the blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="2681a-205">Öppna Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2681a-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="2681a-206">I den vänstra rutan, expanderar du lagringskontot som innehåller blob-behållare du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="2681a-206">In the left pane, expand the storage account containing the blob container you wish to manage.</span></span>
3. <span data-ttu-id="2681a-207">Expandera lagringskontot **Blobbbehållare**.</span><span class="sxs-lookup"><span data-stu-id="2681a-207">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="2681a-208">Dubbelklicka på blob-behållare som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="2681a-208">Double-click the blob container you wish to view.</span></span>
5. <span data-ttu-id="2681a-209">I huvudfönstret visas innehållet för blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2681a-209">The main pane will display the blob container's contents.</span></span>

   ![Visa blob-behållare][3]
6. <span data-ttu-id="2681a-211">I huvudfönstret visas innehållet för blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="2681a-211">The main pane will display the blob container's contents.</span></span>
7. <span data-ttu-id="2681a-212">Gör följande beroende på vilken aktivitet du vill utföra:</span><span class="sxs-lookup"><span data-stu-id="2681a-212">Follow these steps depending on the task you wish to perform:</span></span>

   * <span data-ttu-id="2681a-213">**Ladda upp filer till en blob-behållare**</span><span class="sxs-lookup"><span data-stu-id="2681a-213">**Upload files to a blob container**</span></span>

     1. <span data-ttu-id="2681a-214">Gå till verktygsfältet i huvudfönstret och välj **Överför**, och sedan **Överför filer** i den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="2681a-214">On the main pane's toolbar, select **Upload**, and then **Upload Files** from the drop-down menu.</span></span>

        ![Överföra filer][15]
     2. <span data-ttu-id="2681a-216">I dialogen **Överför filer** dialogrutan klickar du på knappen med tre punkter (**...** ) på höger sida av textrutan **Filer** och markerar den eller de filer du vill överföra.</span><span class="sxs-lookup"><span data-stu-id="2681a-216">In the **Upload files** dialog, select the ellipsis (**…**) button on the right side of the **Files** text box to select the file(s) you wish to upload.</span></span>

        ![Ladda upp filer alternativ][16]
     3. <span data-ttu-id="2681a-218">Ange vilken typ av **Blob typen**.</span><span class="sxs-lookup"><span data-stu-id="2681a-218">Specify the type of **Blob type**.</span></span> <span data-ttu-id="2681a-219">Artikeln [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beskrivs skillnaderna mellan de olika typerna av blob.</span><span class="sxs-lookup"><span data-stu-id="2681a-219">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="2681a-220">Du kan också ange målmapp som de markerade filerna laddas upp.</span><span class="sxs-lookup"><span data-stu-id="2681a-220">Optionally, specify a target folder into which the selected file(s) will be uploaded.</span></span> <span data-ttu-id="2681a-221">Om målmappen inte finns skapas den.</span><span class="sxs-lookup"><span data-stu-id="2681a-221">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="2681a-222">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="2681a-222">Select **Upload**.</span></span>
   * <span data-ttu-id="2681a-223">**Ladda upp en mapp till en blob-behållare**</span><span class="sxs-lookup"><span data-stu-id="2681a-223">**Upload a folder to a blob container**</span></span>

     1. <span data-ttu-id="2681a-224">Gå till verktygsfältet i huvudfönstret och klicka på **Överför**, och sedan på **Överför mapp** i den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="2681a-224">On the main pane's toolbar, select **Upload**, and then **Upload Folder** from the drop-down menu.</span></span>

        ![Menyn för mappöverföring][17]
     2. <span data-ttu-id="2681a-226">I dialogen **Överför mapp** klickar du på knappen med tre punkter (**...** ) på höger sida av textrutan **Mapp** och väljer den mapp vars innehåll du vill överföra.</span><span class="sxs-lookup"><span data-stu-id="2681a-226">In the **Upload folder** dialog, select the ellipsis (**…**) button on the right side of the **Folder** text box to select the folder whose contents you wish to upload.</span></span>

        ![Överför Mappalternativ][18]
     3. <span data-ttu-id="2681a-228">Ange vilken typ av **Blob typen**.</span><span class="sxs-lookup"><span data-stu-id="2681a-228">Specify the type of **Blob type**.</span></span> <span data-ttu-id="2681a-229">Artikeln [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beskrivs skillnaderna mellan de olika typerna av blob.</span><span class="sxs-lookup"><span data-stu-id="2681a-229">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="2681a-230">Du kan även ange en målmapp som den markerade mappens innehåll ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="2681a-230">Optionally, specify a target folder into which the selected folder's contents will be uploaded.</span></span> <span data-ttu-id="2681a-231">Om målmappen inte finns skapas den.</span><span class="sxs-lookup"><span data-stu-id="2681a-231">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="2681a-232">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="2681a-232">Select **Upload**.</span></span>
   * <span data-ttu-id="2681a-233">**Hämta en blobb till den lokala datorn**</span><span class="sxs-lookup"><span data-stu-id="2681a-233">**Download a blob to your local computer**</span></span>

     1. <span data-ttu-id="2681a-234">Välj blob som du vill hämta.</span><span class="sxs-lookup"><span data-stu-id="2681a-234">Select the blob you wish to download.</span></span>
     2. <span data-ttu-id="2681a-235">Gå till verktygsfältet i huvudfönstret och klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="2681a-235">On the main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="2681a-236">I den **ange var du vill spara den hämta blobben** dialogrutan, ange den plats där du vill blob hämtas och du vill ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="2681a-236">In the **Specify where to save the downloaded blob** dialog, specify the location where you want the blob downloaded, and the name you wish to give it.</span></span>  
     4. <span data-ttu-id="2681a-237">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2681a-237">Select **Save**.</span></span>
   * <span data-ttu-id="2681a-238">**Öppna en blob på den lokala datorn**</span><span class="sxs-lookup"><span data-stu-id="2681a-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="2681a-239">Välj blob som du vill öppna.</span><span class="sxs-lookup"><span data-stu-id="2681a-239">Select the blob you wish to open.</span></span>
     2. <span data-ttu-id="2681a-240">Gå till verktygsfältet i huvudfönstret och välj **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="2681a-240">On the main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="2681a-241">Blob kommer att hämtas och öppnas med hjälp av programmet som är associerade med den blob underliggande filtyp.</span><span class="sxs-lookup"><span data-stu-id="2681a-241">The blob will be downloaded and opened using the application associated with the blob's underlying file type.</span></span>
   * <span data-ttu-id="2681a-242">**Kopiera en blobb till Urklipp**</span><span class="sxs-lookup"><span data-stu-id="2681a-242">**Copy a blob to the clipboard**</span></span>

     1. <span data-ttu-id="2681a-243">Välj blob som du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="2681a-243">Select the blob you wish to copy.</span></span>
     2. <span data-ttu-id="2681a-244">Gå till verktygsfältet i huvudfönstret och klicka på **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2681a-244">On the main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="2681a-245">Navigera till en annan blob-behållaren i den vänstra rutan, och dubbelklicka på den om du vill visa i huvudfönstret.</span><span class="sxs-lookup"><span data-stu-id="2681a-245">In the left pane, navigate to another blob container, and double-click it to view it in the main pane.</span></span>
     4. <span data-ttu-id="2681a-246">I huvudfönstret i verktygsfältet väljer **klistra in** att skapa en kopia av blobben.</span><span class="sxs-lookup"><span data-stu-id="2681a-246">On the main pane's toolbar, select **Paste** to create a copy of the blob.</span></span>
   * <span data-ttu-id="2681a-247">**Ta bort en blobb**</span><span class="sxs-lookup"><span data-stu-id="2681a-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="2681a-248">Välj blob som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="2681a-248">Select the blob you wish to delete.</span></span>
     2. <span data-ttu-id="2681a-249">Gå till verktygsfältet i huvudfönstret och klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2681a-249">On the main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="2681a-250">Välj **Ja** i bekräftelsedialogen.</span><span class="sxs-lookup"><span data-stu-id="2681a-250">Select **Yes** to the confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2681a-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2681a-251">Next steps</span></span>
* <span data-ttu-id="2681a-252">Visa [ viktig information och videor för den senaste Storage Explorer-versionen (förhandsutgåva)](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="2681a-252">View the [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="2681a-253">Läs mer om hur du [skapar program med Azure-blobbar, tabeller köer och filer](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="2681a-253">Learn how to [create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

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
