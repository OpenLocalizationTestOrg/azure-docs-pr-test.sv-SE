---
title: "Aktivera offentlig läsbehörighet för behållare och blobbar i Azure Blob storage | Microsoft Docs"
description: "Lär dig hur du gör behållare och blobbar som är tillgängliga för anonym åtkomst och hur du kommer åt dem via programmering."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.openlocfilehash: 8d4f4c7c208baf0db6155eb78a53e37c4ec1e023
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a><span data-ttu-id="59472-103">Hantera anonym läsåtkomst till behållare och blob-objekt</span><span class="sxs-lookup"><span data-stu-id="59472-103">Manage anonymous read access to containers and blobs</span></span>
<span data-ttu-id="59472-104">Du kan aktivera anonym, offentlig läsbehörighet till en behållare och dess blobbar i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="59472-104">You can enable anonymous, public read access to a container and its blobs in Azure Blob storage.</span></span> <span data-ttu-id="59472-105">Då kan bevilja du skrivskyddad åtkomst till dessa resurser utan att dela din kontonyckel och utan att kräva en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="59472-105">By doing so, you can grant read-only access to these resources without sharing your account key, and without requiring a shared access signature (SAS).</span></span>

<span data-ttu-id="59472-106">Offentlig läsbehörighet är bäst för scenarier där du vill att vissa BLOB ska alltid vara tillgänglig för anonym läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="59472-106">Public read access is best for scenarios where you want certain blobs to always be available for anonymous read access.</span></span> <span data-ttu-id="59472-107">Du kan skapa en signatur för delad åtkomst för mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="59472-107">For more fine-grained control, you can create a shared access signature.</span></span> <span data-ttu-id="59472-108">Signaturer för delad åtkomst kan du ange begränsad åtkomst med olika behörigheter för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="59472-108">Shared access signatures enable you to provide restricted access using different permissions, for a specific time period.</span></span> <span data-ttu-id="59472-109">Mer information om hur du skapar delad åtkomst till signaturer, se [använder signaturer för delad åtkomst (SAS) i Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59472-109">For more information about creating shared access signatures, see [Using shared access signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a><span data-ttu-id="59472-110">Bevilja behörighet för anonyma användare till behållare och blobbar</span><span class="sxs-lookup"><span data-stu-id="59472-110">Grant anonymous users permissions to containers and blobs</span></span>
<span data-ttu-id="59472-111">Som standard kan en behållare och alla blobbar i den endast användas av ägaren till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="59472-111">By default, a container and any blobs within it may be accessed only by the owner of the storage account.</span></span> <span data-ttu-id="59472-112">Du kan ange behörigheter för behållaren att tillåta offentlig åtkomst om du vill ge anonyma användare som har läsbehörighet till en behållare och dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="59472-112">To give anonymous users read permissions to a container and its blobs, you can set the container permissions to allow public access.</span></span> <span data-ttu-id="59472-113">Anonyma användare kan läsa blobbar i en behållare som är offentligt tillgänglig utan att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="59472-113">Anonymous users can read blobs within a publicly accessible container without authenticating the request.</span></span>

<span data-ttu-id="59472-114">Du kan konfigurera en behållare med följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="59472-114">You can configure a container with the following permissions:</span></span>

* <span data-ttu-id="59472-115">**Ingen offentlig läsbehörighet:** behållaren och dess blobbar kan endast användas av lagringskontoägaren.</span><span class="sxs-lookup"><span data-stu-id="59472-115">**No public read access:** The container and its blobs can be accessed only by the storage account owner.</span></span> <span data-ttu-id="59472-116">Detta är standard för alla nya behållare.</span><span class="sxs-lookup"><span data-stu-id="59472-116">This is the default for all new containers.</span></span>
* <span data-ttu-id="59472-117">**Offentlig läsbehörighet för blobbar endast:** Blobbar i behållaren kan läsas av anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="59472-117">**Public read access for blobs only:** Blobs within the container can be read by anonymous request, but container data is not available.</span></span> <span data-ttu-id="59472-118">Anonyma klienter kan inte räkna upp blobbar i behållaren.</span><span class="sxs-lookup"><span data-stu-id="59472-118">Anonymous clients cannot enumerate the blobs within the container.</span></span>
* <span data-ttu-id="59472-119">**Fullständig offentlig läsbehörighet:** alla behållare och blob-data kan läsas av anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="59472-119">**Full public read access:** All container and blob data can be read by anonymous request.</span></span> <span data-ttu-id="59472-120">Klienter kan räkna upp blobbar i behållaren av anonym begäran, men det går inte att räkna upp behållare i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="59472-120">Clients can enumerate blobs within the container by anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="59472-121">Du kan använda följande för att ange behörigheter för behållaren:</span><span class="sxs-lookup"><span data-stu-id="59472-121">You can use the following to set container permissions:</span></span>

* [<span data-ttu-id="59472-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59472-122">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="59472-123">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="59472-123">Azure PowerShell</span></span>](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#how-to-manage-azure-blobs)
* [<span data-ttu-id="59472-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="59472-124">Azure CLI 2.0</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* <span data-ttu-id="59472-125">Programmässigt med någon av storage-klientbibliotek eller REST API</span><span class="sxs-lookup"><span data-stu-id="59472-125">Programmatically, by using one of the storage client libraries or the REST API</span></span>

### <a name="set-container-permissions-in-the-azure-portal"></a><span data-ttu-id="59472-126">Ange behörigheter för behållaren i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="59472-126">Set container permissions in the Azure portal</span></span>
<span data-ttu-id="59472-127">Ange behörigheter för behållaren den [Azure-portalen](https://portal.azure.com), gör du följande:</span><span class="sxs-lookup"><span data-stu-id="59472-127">To set container permissions in the [Azure portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="59472-128">Öppna din **lagringskonto** blad i portalen.</span><span class="sxs-lookup"><span data-stu-id="59472-128">Open your **Storage account** blade in the portal.</span></span> <span data-ttu-id="59472-129">Du kan hitta ditt lagringskonto genom att välja **lagringskonton** i bladet portal huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="59472-129">You can find your storage account by selecting **Storage accounts** in the main portal menu blade.</span></span>
1. <span data-ttu-id="59472-130">Under **BLOB-tjänsten** på bladet menyn väljer **behållare**.</span><span class="sxs-lookup"><span data-stu-id="59472-130">Under **BLOB SERVICE** on the menu blade, select **Containers**.</span></span>
1. <span data-ttu-id="59472-131">Högerklicka på behållaren raden eller välj på knappen Öppna behållarens **snabbmenyn**.</span><span class="sxs-lookup"><span data-stu-id="59472-131">Right-click on the container row or select the ellipsis to open the container's **Context menu**.</span></span>
1. <span data-ttu-id="59472-132">Välj **princip** på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="59472-132">Select **Access policy** in the context menu.</span></span>
1. <span data-ttu-id="59472-133">Välj en **komma åt typen** från nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="59472-133">Select an **Access type** from the drop down menu.</span></span>

    ![Metadata för behållaren dialogrutan Redigera](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a><span data-ttu-id="59472-135">Behörigheter för behållaren med .NET</span><span class="sxs-lookup"><span data-stu-id="59472-135">Set container permissions with .NET</span></span>
<span data-ttu-id="59472-136">Om du vill ange behörigheter för en behållare med C# och Storage-klientbiblioteket för .NET, först hämta behållarens befintliga behörigheter genom att anropa den **GetPermissions** metod.</span><span class="sxs-lookup"><span data-stu-id="59472-136">To set permissions for a container using C# and the Storage Client Library for .NET, first retrieve the container's existing permissions by calling the **GetPermissions** method.</span></span> <span data-ttu-id="59472-137">Ange den **PublicAccess** -egenskapen för den **BlobContainerPermissions** objekt som returneras av den **GetPermissions** metod.</span><span class="sxs-lookup"><span data-stu-id="59472-137">Then set the **PublicAccess** property for the **BlobContainerPermissions** object that is returned by the **GetPermissions** method.</span></span> <span data-ttu-id="59472-138">Slutligen anropa den **behörighetsgruppbehörighet** metod med de uppdaterade behörigheterna.</span><span class="sxs-lookup"><span data-stu-id="59472-138">Finally, call the **SetPermissions** method with the updated permissions.</span></span>

<span data-ttu-id="59472-139">I följande exempel anger behållarens behörigheter till fullständig offentlig läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="59472-139">The following example sets the container's permissions to full public read access.</span></span> <span data-ttu-id="59472-140">Att ställa in behörigheter till offentlig läsbehörighet för blobbar endast den **PublicAccess** egenskapen **BlobContainerPublicAccessType.Blob**.</span><span class="sxs-lookup"><span data-stu-id="59472-140">To set permissions to public read access for blobs only, set the **PublicAccess** property to **BlobContainerPublicAccessType.Blob**.</span></span> <span data-ttu-id="59472-141">Ta bort alla behörigheter för anonyma användare genom att ange egenskapen till **BlobContainerPublicAccessType.Off**.</span><span class="sxs-lookup"><span data-stu-id="59472-141">To remove all permissions for anonymous users, set the property to **BlobContainerPublicAccessType.Off**.</span></span>

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a><span data-ttu-id="59472-142">Åtkomst till behållare och blobbar anonymt</span><span class="sxs-lookup"><span data-stu-id="59472-142">Access containers and blobs anonymously</span></span>
<span data-ttu-id="59472-143">En klient som ansluter till behållare och blobbar anonymt kan använda konstruktorer som inte kräver autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="59472-143">A client that accesses containers and blobs anonymously can use constructors that do not require credentials.</span></span> <span data-ttu-id="59472-144">Följande exempel visar några olika sätt att hänvisa till resurser för Blob-tjänsten anonymt.</span><span class="sxs-lookup"><span data-stu-id="59472-144">The following examples show a few different ways to reference Blob service resources anonymously.</span></span>

### <a name="create-an-anonymous-client-object"></a><span data-ttu-id="59472-145">Skapa en anonym klient-objekt</span><span class="sxs-lookup"><span data-stu-id="59472-145">Create an anonymous client object</span></span>
<span data-ttu-id="59472-146">Du kan skapa ett nytt objekt för tjänsten för anonym åtkomst genom att tillhandahålla Blob-tjänsteslutpunkt för kontot.</span><span class="sxs-lookup"><span data-stu-id="59472-146">You can create a new service client object for anonymous access by providing the Blob service endpoint for the account.</span></span> <span data-ttu-id="59472-147">Du måste dock också känna till namnet på en behållare i det konto som är tillgängliga för anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="59472-147">However, you must also know the name of a container in that account that's available for anonymous access.</span></span>

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a><span data-ttu-id="59472-148">Referera till en behållare anonymt</span><span class="sxs-lookup"><span data-stu-id="59472-148">Reference a container anonymously</span></span>
<span data-ttu-id="59472-149">Du kan använda den för att referera till behållaren direkt om du har rätt Webbadress till en behållare som är tillgänglig anonymt.</span><span class="sxs-lookup"><span data-stu-id="59472-149">If you have the URL to a container that is anonymously available, you can use it to reference the container directly.</span></span>

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a><span data-ttu-id="59472-150">Referera till en blob anonymt</span><span class="sxs-lookup"><span data-stu-id="59472-150">Reference a blob anonymously</span></span>
<span data-ttu-id="59472-151">Om du har rätt Webbadress till en blobb som är tillgängliga för anonym åtkomst, kan du referera blob direkt med hjälp av denna Webbadress:</span><span class="sxs-lookup"><span data-stu-id="59472-151">If you have the URL to a blob that is available for anonymous access, you can reference the blob directly using that URL:</span></span>

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a><span data-ttu-id="59472-152">Funktioner som är tillgängliga för anonyma användare</span><span class="sxs-lookup"><span data-stu-id="59472-152">Features available to anonymous users</span></span>
<span data-ttu-id="59472-153">I följande tabell visas vilka åtgärder som kan anropas av anonyma användare när en behållar-ACL är inställd på att tillåta offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="59472-153">The following table shows which operations may be called by anonymous users when a container's ACL is set to allow public access.</span></span>

| <span data-ttu-id="59472-154">REST-åtgärd</span><span class="sxs-lookup"><span data-stu-id="59472-154">REST Operation</span></span> | <span data-ttu-id="59472-155">Behörighet med fullständig offentlig läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="59472-155">Permission with full public read access</span></span> | <span data-ttu-id="59472-156">Offentlig läsbehörighet för endast BLOB-behörigheten</span><span class="sxs-lookup"><span data-stu-id="59472-156">Permission with public read access for blobs only</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59472-157">Lista behållare</span><span class="sxs-lookup"><span data-stu-id="59472-157">List Containers</span></span> |<span data-ttu-id="59472-158">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-158">Owner only</span></span> |<span data-ttu-id="59472-159">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-159">Owner only</span></span> |
| <span data-ttu-id="59472-160">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="59472-160">Create Container</span></span> |<span data-ttu-id="59472-161">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-161">Owner only</span></span> |<span data-ttu-id="59472-162">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-162">Owner only</span></span> |
| <span data-ttu-id="59472-163">Hämta egenskaper för behållaren</span><span class="sxs-lookup"><span data-stu-id="59472-163">Get Container Properties</span></span> |<span data-ttu-id="59472-164">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-164">All</span></span> |<span data-ttu-id="59472-165">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-165">Owner only</span></span> |
| <span data-ttu-id="59472-166">Hämta Metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="59472-166">Get Container Metadata</span></span> |<span data-ttu-id="59472-167">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-167">All</span></span> |<span data-ttu-id="59472-168">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-168">Owner only</span></span> |
| <span data-ttu-id="59472-169">Ange Metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="59472-169">Set Container Metadata</span></span> |<span data-ttu-id="59472-170">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-170">Owner only</span></span> |<span data-ttu-id="59472-171">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-171">Owner only</span></span> |
| <span data-ttu-id="59472-172">Hämta behållar-ACL</span><span class="sxs-lookup"><span data-stu-id="59472-172">Get Container ACL</span></span> |<span data-ttu-id="59472-173">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-173">Owner only</span></span> |<span data-ttu-id="59472-174">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-174">Owner only</span></span> |
| <span data-ttu-id="59472-175">Ange behållar-ACL</span><span class="sxs-lookup"><span data-stu-id="59472-175">Set Container ACL</span></span> |<span data-ttu-id="59472-176">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-176">Owner only</span></span> |<span data-ttu-id="59472-177">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-177">Owner only</span></span> |
| <span data-ttu-id="59472-178">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="59472-178">Delete Container</span></span> |<span data-ttu-id="59472-179">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-179">Owner only</span></span> |<span data-ttu-id="59472-180">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-180">Owner only</span></span> |
| <span data-ttu-id="59472-181">Lista över Blobbar</span><span class="sxs-lookup"><span data-stu-id="59472-181">List Blobs</span></span> |<span data-ttu-id="59472-182">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-182">All</span></span> |<span data-ttu-id="59472-183">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-183">Owner only</span></span> |
| <span data-ttu-id="59472-184">Placera Blob</span><span class="sxs-lookup"><span data-stu-id="59472-184">Put Blob</span></span> |<span data-ttu-id="59472-185">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-185">Owner only</span></span> |<span data-ttu-id="59472-186">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-186">Owner only</span></span> |
| <span data-ttu-id="59472-187">Hämta Blob</span><span class="sxs-lookup"><span data-stu-id="59472-187">Get Blob</span></span> |<span data-ttu-id="59472-188">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-188">All</span></span> |<span data-ttu-id="59472-189">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-189">All</span></span> |
| <span data-ttu-id="59472-190">Hämta Blob-egenskaper</span><span class="sxs-lookup"><span data-stu-id="59472-190">Get Blob Properties</span></span> |<span data-ttu-id="59472-191">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-191">All</span></span> |<span data-ttu-id="59472-192">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-192">All</span></span> |
| <span data-ttu-id="59472-193">Ange Blob-egenskaper</span><span class="sxs-lookup"><span data-stu-id="59472-193">Set Blob Properties</span></span> |<span data-ttu-id="59472-194">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-194">Owner only</span></span> |<span data-ttu-id="59472-195">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-195">Owner only</span></span> |
| <span data-ttu-id="59472-196">Hämta Blobbmetadata</span><span class="sxs-lookup"><span data-stu-id="59472-196">Get Blob Metadata</span></span> |<span data-ttu-id="59472-197">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-197">All</span></span> |<span data-ttu-id="59472-198">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-198">All</span></span> |
| <span data-ttu-id="59472-199">Ange Blobbmetadata</span><span class="sxs-lookup"><span data-stu-id="59472-199">Set Blob Metadata</span></span> |<span data-ttu-id="59472-200">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-200">Owner only</span></span> |<span data-ttu-id="59472-201">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-201">Owner only</span></span> |
| <span data-ttu-id="59472-202">Placera Block</span><span class="sxs-lookup"><span data-stu-id="59472-202">Put Block</span></span> |<span data-ttu-id="59472-203">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-203">Owner only</span></span> |<span data-ttu-id="59472-204">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-204">Owner only</span></span> |
| <span data-ttu-id="59472-205">Hämta listan över blockerade (endast allokerad block)</span><span class="sxs-lookup"><span data-stu-id="59472-205">Get Block List (committed blocks only)</span></span> |<span data-ttu-id="59472-206">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-206">All</span></span> |<span data-ttu-id="59472-207">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-207">All</span></span> |
| <span data-ttu-id="59472-208">Hämta listan över blockerade (ogenomförda block eller alla block)</span><span class="sxs-lookup"><span data-stu-id="59472-208">Get Block List (uncommitted blocks only or all blocks)</span></span> |<span data-ttu-id="59472-209">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-209">Owner only</span></span> |<span data-ttu-id="59472-210">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-210">Owner only</span></span> |
| <span data-ttu-id="59472-211">Placera Blockeringslista</span><span class="sxs-lookup"><span data-stu-id="59472-211">Put Block List</span></span> |<span data-ttu-id="59472-212">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-212">Owner only</span></span> |<span data-ttu-id="59472-213">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-213">Owner only</span></span> |
| <span data-ttu-id="59472-214">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="59472-214">Delete Blob</span></span> |<span data-ttu-id="59472-215">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-215">Owner only</span></span> |<span data-ttu-id="59472-216">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-216">Owner only</span></span> |
| <span data-ttu-id="59472-217">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="59472-217">Copy Blob</span></span> |<span data-ttu-id="59472-218">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-218">Owner only</span></span> |<span data-ttu-id="59472-219">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-219">Owner only</span></span> |
| <span data-ttu-id="59472-220">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="59472-220">Snapshot Blob</span></span> |<span data-ttu-id="59472-221">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-221">Owner only</span></span> |<span data-ttu-id="59472-222">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-222">Owner only</span></span> |
| <span data-ttu-id="59472-223">Lånet Blob</span><span class="sxs-lookup"><span data-stu-id="59472-223">Lease Blob</span></span> |<span data-ttu-id="59472-224">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-224">Owner only</span></span> |<span data-ttu-id="59472-225">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-225">Owner only</span></span> |
| <span data-ttu-id="59472-226">Placera sida</span><span class="sxs-lookup"><span data-stu-id="59472-226">Put Page</span></span> |<span data-ttu-id="59472-227">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-227">Owner only</span></span> |<span data-ttu-id="59472-228">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-228">Owner only</span></span> |
| <span data-ttu-id="59472-229">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="59472-229">Get Page Ranges</span></span> |<span data-ttu-id="59472-230">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-230">All</span></span> |<span data-ttu-id="59472-231">Alla</span><span class="sxs-lookup"><span data-stu-id="59472-231">All</span></span> |
| <span data-ttu-id="59472-232">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="59472-232">Append Blob</span></span> |<span data-ttu-id="59472-233">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-233">Owner only</span></span> |<span data-ttu-id="59472-234">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="59472-234">Owner only</span></span> |

## <a name="next-steps"></a><span data-ttu-id="59472-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59472-235">Next steps</span></span>

* [<span data-ttu-id="59472-236">Autentisering för Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="59472-236">Authentication for the Azure Storage Services</span></span>](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [<span data-ttu-id="59472-237">Använda signaturer för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="59472-237">Using Shared Access Signatures (SAS)</span></span>](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="59472-238">Delegera åtkomst med signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="59472-238">Delegating Access with a Shared Access Signature</span></span>](https://msdn.microsoft.com/library/azure/ee395415.aspx)
