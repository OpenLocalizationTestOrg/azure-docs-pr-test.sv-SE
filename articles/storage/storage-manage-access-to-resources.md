---
title: "aaaEnable offentlig läsbehörighet för behållare och blobbar i Azure Blob storage | Microsoft Docs"
description: "Lär dig hur toomake behållare och blobbar som är tillgängliga för anonym åtkomst och hur tooaccess dem via programmering."
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
ms.openlocfilehash: 0675b5dc4d32a3a0a34376ae4c049542b07ba03a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-anonymous-read-access-toocontainers-and-blobs"></a><span data-ttu-id="4e9dd-103">Hantera anonym läsbehörighet toocontainers och blobbar</span><span class="sxs-lookup"><span data-stu-id="4e9dd-103">Manage anonymous read access toocontainers and blobs</span></span>
<span data-ttu-id="4e9dd-104">Du kan aktivera anonym, offentlig läsbehörighet tooa behållaren och dess blobbar i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-104">You can enable anonymous, public read access tooa container and its blobs in Azure Blob storage.</span></span> <span data-ttu-id="4e9dd-105">Då kan bevilja du läsåtkomst toothese resurser utan att dela din kontonyckel och utan att kräva en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="4e9dd-105">By doing so, you can grant read-only access toothese resources without sharing your account key, and without requiring a shared access signature (SAS).</span></span>

<span data-ttu-id="4e9dd-106">Offentlig läsbehörighet är bäst för scenarier där du vill att vissa blobbar tooalways vara tillgängliga för anonym läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-106">Public read access is best for scenarios where you want certain blobs tooalways be available for anonymous read access.</span></span> <span data-ttu-id="4e9dd-107">Du kan skapa en signatur för delad åtkomst för mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-107">For more fine-grained control, you can create a shared access signature.</span></span> <span data-ttu-id="4e9dd-108">Signaturer för delad åtkomst kan du tooprovide begränsad åtkomst med olika behörigheter för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-108">Shared access signatures enable you tooprovide restricted access using different permissions, for a specific time period.</span></span> <span data-ttu-id="4e9dd-109">Mer information om hur du skapar delad åtkomst till signaturer, se [använder signaturer för delad åtkomst (SAS) i Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="4e9dd-109">For more information about creating shared access signatures, see [Using shared access signatures (SAS) in Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span></span>

## <a name="grant-anonymous-users-permissions-toocontainers-and-blobs"></a><span data-ttu-id="4e9dd-110">Bevilja anonyma användare behörighet toocontainers och blobbar</span><span class="sxs-lookup"><span data-stu-id="4e9dd-110">Grant anonymous users permissions toocontainers and blobs</span></span>
<span data-ttu-id="4e9dd-111">Som standard kan en behållare och alla blobbar i den endast användas av hello ägare hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-111">By default, a container and any blobs within it may be accessed only by hello owner of hello storage account.</span></span> <span data-ttu-id="4e9dd-112">Du kan ange hello behållaren behörigheter tooallow offentlig åtkomst toogive anonyma användare behörighet att läsa tooa behållaren och dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-112">toogive anonymous users read permissions tooa container and its blobs, you can set hello container permissions tooallow public access.</span></span> <span data-ttu-id="4e9dd-113">Anonyma användare kan läsa blobbar i en behållare som är offentligt tillgänglig utan autentiseras hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-113">Anonymous users can read blobs within a publicly accessible container without authenticating hello request.</span></span>

<span data-ttu-id="4e9dd-114">Du kan konfigurera en behållare med hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="4e9dd-114">You can configure a container with hello following permissions:</span></span>

* <span data-ttu-id="4e9dd-115">**Ingen offentlig läsbehörighet:** hello-behållaren och dess blobbar kan endast användas av hello lagringskontoägaren.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-115">**No public read access:** hello container and its blobs can be accessed only by hello storage account owner.</span></span> <span data-ttu-id="4e9dd-116">Detta är hello standard för alla nya behållare.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-116">This is hello default for all new containers.</span></span>
* <span data-ttu-id="4e9dd-117">**Offentlig läsbehörighet för blobbar endast:** Blobbar i behållaren hello kan läsas av anonym begäran, men behållardata är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-117">**Public read access for blobs only:** Blobs within hello container can be read by anonymous request, but container data is not available.</span></span> <span data-ttu-id="4e9dd-118">Anonyma klienter kan inte räkna upp hello blobbar i behållaren hello.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-118">Anonymous clients cannot enumerate hello blobs within hello container.</span></span>
* <span data-ttu-id="4e9dd-119">**Fullständig offentlig läsbehörighet:** alla behållare och blob-data kan läsas av anonym begäran.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-119">**Full public read access:** All container and blob data can be read by anonymous request.</span></span> <span data-ttu-id="4e9dd-120">Klienter kan räkna upp blobbar i behållaren hello av anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-120">Clients can enumerate blobs within hello container by anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="4e9dd-121">Du kan använda hello följande tooset behållare behörigheter:</span><span class="sxs-lookup"><span data-stu-id="4e9dd-121">You can use hello following tooset container permissions:</span></span>

* [<span data-ttu-id="4e9dd-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4e9dd-122">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="4e9dd-123">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e9dd-123">Azure PowerShell</span></span>](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [<span data-ttu-id="4e9dd-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4e9dd-124">Azure CLI 2.0</span></span>](storage-azure-cli.md#create-and-manage-blobs)
* <span data-ttu-id="4e9dd-125">Programmässigt med någon av hello storage-klientbibliotek eller hello REST API</span><span class="sxs-lookup"><span data-stu-id="4e9dd-125">Programmatically, by using one of hello storage client libraries or hello REST API</span></span>

### <a name="set-container-permissions-in-hello-azure-portal"></a><span data-ttu-id="4e9dd-126">Ange behörigheter för behållaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4e9dd-126">Set container permissions in hello Azure portal</span></span>
<span data-ttu-id="4e9dd-127">tooset behållaren behörigheter i hello [Azure-portalen](https://portal.azure.com), gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4e9dd-127">tooset container permissions in hello [Azure portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="4e9dd-128">Öppna din **lagringskonto** bladet i hello portal.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-128">Open your **Storage account** blade in hello portal.</span></span> <span data-ttu-id="4e9dd-129">Du kan hitta ditt lagringskonto genom att välja **lagringskonton** hello portal huvudmenyn-bladet.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-129">You can find your storage account by selecting **Storage accounts** in hello main portal menu blade.</span></span>
1. <span data-ttu-id="4e9dd-130">Under **BLOB-tjänsten** på bladet för hello-menyn, Välj **behållare**.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-130">Under **BLOB SERVICE** on hello menu blade, select **Containers**.</span></span>
1. <span data-ttu-id="4e9dd-131">Högerklicka på hello behållaren raden eller välj hello ellips tooopen hello behållarens **snabbmenyn**.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-131">Right-click on hello container row or select hello ellipsis tooopen hello container's **Context menu**.</span></span>
1. <span data-ttu-id="4e9dd-132">Välj **princip** hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-132">Select **Access policy** in hello context menu.</span></span>
1. <span data-ttu-id="4e9dd-133">Välj en **komma åt typen** från hello nedrullningsbara meny.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-133">Select an **Access type** from hello drop down menu.</span></span>

    ![Metadata för behållaren dialogrutan Redigera](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a><span data-ttu-id="4e9dd-135">Behörigheter för behållaren med .NET</span><span class="sxs-lookup"><span data-stu-id="4e9dd-135">Set container permissions with .NET</span></span>
<span data-ttu-id="4e9dd-136">tooset behörigheter för en behållare med C# och hello Storage-klientbibliotek för .NET, först hämta hello behållaren befintliga behörigheter genom att anropa hello **GetPermissions** metod.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-136">tooset permissions for a container using C# and hello Storage Client Library for .NET, first retrieve hello container's existing permissions by calling hello **GetPermissions** method.</span></span> <span data-ttu-id="4e9dd-137">Därefter uppsättning hello **PublicAccess** -egenskapen för hello **BlobContainerPermissions** objekt som returneras av hello **GetPermissions** metod.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-137">Then set hello **PublicAccess** property for hello **BlobContainerPermissions** object that is returned by hello **GetPermissions** method.</span></span> <span data-ttu-id="4e9dd-138">Slutligen anropa hello **behörighetsgruppbehörighet** metod med hello uppdateras behörigheter.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-138">Finally, call hello **SetPermissions** method with hello updated permissions.</span></span>

<span data-ttu-id="4e9dd-139">hello följande exempel anger hello behållaren behörigheter toofull offentlig läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-139">hello following example sets hello container's permissions toofull public read access.</span></span> <span data-ttu-id="4e9dd-140">tooset behörigheter toopublic läsbehörighet för blobbar enbart anges hello **PublicAccess** egenskapen för**BlobContainerPublicAccessType.Blob**.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-140">tooset permissions toopublic read access for blobs only, set hello **PublicAccess** property too**BlobContainerPublicAccessType.Blob**.</span></span> <span data-ttu-id="4e9dd-141">tooremove alla behörigheter för anonyma användare anges hello egenskapen för**BlobContainerPublicAccessType.Off**.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-141">tooremove all permissions for anonymous users, set hello property too**BlobContainerPublicAccessType.Off**.</span></span>

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a><span data-ttu-id="4e9dd-142">Åtkomst till behållare och blobbar anonymt</span><span class="sxs-lookup"><span data-stu-id="4e9dd-142">Access containers and blobs anonymously</span></span>
<span data-ttu-id="4e9dd-143">En klient som ansluter till behållare och blobbar anonymt kan använda konstruktorer som inte kräver autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-143">A client that accesses containers and blobs anonymously can use constructors that do not require credentials.</span></span> <span data-ttu-id="4e9dd-144">hello som följande exempel visar några olika sätt tooreference Blob-tjänstresurserna anonymt.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-144">hello following examples show a few different ways tooreference Blob service resources anonymously.</span></span>

### <a name="create-an-anonymous-client-object"></a><span data-ttu-id="4e9dd-145">Skapa en anonym klient-objekt</span><span class="sxs-lookup"><span data-stu-id="4e9dd-145">Create an anonymous client object</span></span>
<span data-ttu-id="4e9dd-146">Du kan skapa ett nytt objekt för tjänsten för anonym åtkomst genom att ange hello Blobbtjänstens slutpunkt för hello-konto.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-146">You can create a new service client object for anonymous access by providing hello Blob service endpoint for hello account.</span></span> <span data-ttu-id="4e9dd-147">Du måste också känna hello namnet på en behållare i det konto som är tillgängliga för anonym åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-147">However, you must also know hello name of a container in that account that's available for anonymous access.</span></span>

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create hello client object using hello Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read hello container's properties. Note this is only possible when hello container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a><span data-ttu-id="4e9dd-148">Referera till en behållare anonymt</span><span class="sxs-lookup"><span data-stu-id="4e9dd-148">Reference a container anonymously</span></span>
<span data-ttu-id="4e9dd-149">Om du har hello URL tooa behållare som är tillgängliga anonymt, kan du använda den tooreference hello behållaren direkt.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-149">If you have hello URL tooa container that is anonymously available, you can use it tooreference hello container directly.</span></span>

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in hello container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a><span data-ttu-id="4e9dd-150">Referera till en blob anonymt</span><span class="sxs-lookup"><span data-stu-id="4e9dd-150">Reference a blob anonymously</span></span>
<span data-ttu-id="4e9dd-151">Om du har hello URL tooa blob som är tillgängliga för anonym åtkomst, kan du referera hello blob direkt med hjälp av denna Webbadress:</span><span class="sxs-lookup"><span data-stu-id="4e9dd-151">If you have hello URL tooa blob that is available for anonymous access, you can reference hello blob directly using that URL:</span></span>

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-tooanonymous-users"></a><span data-ttu-id="4e9dd-152">Funktioner tillgängliga tooanonymous användare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-152">Features available tooanonymous users</span></span>
<span data-ttu-id="4e9dd-153">hello i den följande tabellen visar vilka åtgärder som kan anropas av anonyma användare när en behållar-ACL är tooallow offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4e9dd-153">hello following table shows which operations may be called by anonymous users when a container's ACL is set tooallow public access.</span></span>

| <span data-ttu-id="4e9dd-154">REST-åtgärd</span><span class="sxs-lookup"><span data-stu-id="4e9dd-154">REST Operation</span></span> | <span data-ttu-id="4e9dd-155">Behörighet med fullständig offentlig läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="4e9dd-155">Permission with full public read access</span></span> | <span data-ttu-id="4e9dd-156">Offentlig läsbehörighet för endast BLOB-behörigheten</span><span class="sxs-lookup"><span data-stu-id="4e9dd-156">Permission with public read access for blobs only</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4e9dd-157">Lista behållare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-157">List Containers</span></span> |<span data-ttu-id="4e9dd-158">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-158">Owner only</span></span> |<span data-ttu-id="4e9dd-159">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-159">Owner only</span></span> |
| <span data-ttu-id="4e9dd-160">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-160">Create Container</span></span> |<span data-ttu-id="4e9dd-161">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-161">Owner only</span></span> |<span data-ttu-id="4e9dd-162">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-162">Owner only</span></span> |
| <span data-ttu-id="4e9dd-163">Hämta egenskaper för behållaren</span><span class="sxs-lookup"><span data-stu-id="4e9dd-163">Get Container Properties</span></span> |<span data-ttu-id="4e9dd-164">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-164">All</span></span> |<span data-ttu-id="4e9dd-165">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-165">Owner only</span></span> |
| <span data-ttu-id="4e9dd-166">Hämta Metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="4e9dd-166">Get Container Metadata</span></span> |<span data-ttu-id="4e9dd-167">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-167">All</span></span> |<span data-ttu-id="4e9dd-168">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-168">Owner only</span></span> |
| <span data-ttu-id="4e9dd-169">Ange Metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="4e9dd-169">Set Container Metadata</span></span> |<span data-ttu-id="4e9dd-170">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-170">Owner only</span></span> |<span data-ttu-id="4e9dd-171">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-171">Owner only</span></span> |
| <span data-ttu-id="4e9dd-172">Hämta behållar-ACL</span><span class="sxs-lookup"><span data-stu-id="4e9dd-172">Get Container ACL</span></span> |<span data-ttu-id="4e9dd-173">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-173">Owner only</span></span> |<span data-ttu-id="4e9dd-174">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-174">Owner only</span></span> |
| <span data-ttu-id="4e9dd-175">Ange behållar-ACL</span><span class="sxs-lookup"><span data-stu-id="4e9dd-175">Set Container ACL</span></span> |<span data-ttu-id="4e9dd-176">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-176">Owner only</span></span> |<span data-ttu-id="4e9dd-177">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-177">Owner only</span></span> |
| <span data-ttu-id="4e9dd-178">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="4e9dd-178">Delete Container</span></span> |<span data-ttu-id="4e9dd-179">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-179">Owner only</span></span> |<span data-ttu-id="4e9dd-180">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-180">Owner only</span></span> |
| <span data-ttu-id="4e9dd-181">Lista över Blobbar</span><span class="sxs-lookup"><span data-stu-id="4e9dd-181">List Blobs</span></span> |<span data-ttu-id="4e9dd-182">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-182">All</span></span> |<span data-ttu-id="4e9dd-183">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-183">Owner only</span></span> |
| <span data-ttu-id="4e9dd-184">Placera Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-184">Put Blob</span></span> |<span data-ttu-id="4e9dd-185">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-185">Owner only</span></span> |<span data-ttu-id="4e9dd-186">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-186">Owner only</span></span> |
| <span data-ttu-id="4e9dd-187">Hämta Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-187">Get Blob</span></span> |<span data-ttu-id="4e9dd-188">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-188">All</span></span> |<span data-ttu-id="4e9dd-189">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-189">All</span></span> |
| <span data-ttu-id="4e9dd-190">Hämta Blob-egenskaper</span><span class="sxs-lookup"><span data-stu-id="4e9dd-190">Get Blob Properties</span></span> |<span data-ttu-id="4e9dd-191">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-191">All</span></span> |<span data-ttu-id="4e9dd-192">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-192">All</span></span> |
| <span data-ttu-id="4e9dd-193">Ange Blob-egenskaper</span><span class="sxs-lookup"><span data-stu-id="4e9dd-193">Set Blob Properties</span></span> |<span data-ttu-id="4e9dd-194">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-194">Owner only</span></span> |<span data-ttu-id="4e9dd-195">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-195">Owner only</span></span> |
| <span data-ttu-id="4e9dd-196">Hämta Blobbmetadata</span><span class="sxs-lookup"><span data-stu-id="4e9dd-196">Get Blob Metadata</span></span> |<span data-ttu-id="4e9dd-197">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-197">All</span></span> |<span data-ttu-id="4e9dd-198">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-198">All</span></span> |
| <span data-ttu-id="4e9dd-199">Ange Blobbmetadata</span><span class="sxs-lookup"><span data-stu-id="4e9dd-199">Set Blob Metadata</span></span> |<span data-ttu-id="4e9dd-200">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-200">Owner only</span></span> |<span data-ttu-id="4e9dd-201">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-201">Owner only</span></span> |
| <span data-ttu-id="4e9dd-202">Placera Block</span><span class="sxs-lookup"><span data-stu-id="4e9dd-202">Put Block</span></span> |<span data-ttu-id="4e9dd-203">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-203">Owner only</span></span> |<span data-ttu-id="4e9dd-204">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-204">Owner only</span></span> |
| <span data-ttu-id="4e9dd-205">Hämta listan över blockerade (endast allokerad block)</span><span class="sxs-lookup"><span data-stu-id="4e9dd-205">Get Block List (committed blocks only)</span></span> |<span data-ttu-id="4e9dd-206">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-206">All</span></span> |<span data-ttu-id="4e9dd-207">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-207">All</span></span> |
| <span data-ttu-id="4e9dd-208">Hämta listan över blockerade (ogenomförda block eller alla block)</span><span class="sxs-lookup"><span data-stu-id="4e9dd-208">Get Block List (uncommitted blocks only or all blocks)</span></span> |<span data-ttu-id="4e9dd-209">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-209">Owner only</span></span> |<span data-ttu-id="4e9dd-210">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-210">Owner only</span></span> |
| <span data-ttu-id="4e9dd-211">Placera Blockeringslista</span><span class="sxs-lookup"><span data-stu-id="4e9dd-211">Put Block List</span></span> |<span data-ttu-id="4e9dd-212">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-212">Owner only</span></span> |<span data-ttu-id="4e9dd-213">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-213">Owner only</span></span> |
| <span data-ttu-id="4e9dd-214">Ta bort blobben</span><span class="sxs-lookup"><span data-stu-id="4e9dd-214">Delete Blob</span></span> |<span data-ttu-id="4e9dd-215">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-215">Owner only</span></span> |<span data-ttu-id="4e9dd-216">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-216">Owner only</span></span> |
| <span data-ttu-id="4e9dd-217">Kopiera Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-217">Copy Blob</span></span> |<span data-ttu-id="4e9dd-218">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-218">Owner only</span></span> |<span data-ttu-id="4e9dd-219">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-219">Owner only</span></span> |
| <span data-ttu-id="4e9dd-220">Snapshot-Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-220">Snapshot Blob</span></span> |<span data-ttu-id="4e9dd-221">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-221">Owner only</span></span> |<span data-ttu-id="4e9dd-222">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-222">Owner only</span></span> |
| <span data-ttu-id="4e9dd-223">Lånet Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-223">Lease Blob</span></span> |<span data-ttu-id="4e9dd-224">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-224">Owner only</span></span> |<span data-ttu-id="4e9dd-225">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-225">Owner only</span></span> |
| <span data-ttu-id="4e9dd-226">Placera sida</span><span class="sxs-lookup"><span data-stu-id="4e9dd-226">Put Page</span></span> |<span data-ttu-id="4e9dd-227">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-227">Owner only</span></span> |<span data-ttu-id="4e9dd-228">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-228">Owner only</span></span> |
| <span data-ttu-id="4e9dd-229">Get-sidintervall</span><span class="sxs-lookup"><span data-stu-id="4e9dd-229">Get Page Ranges</span></span> |<span data-ttu-id="4e9dd-230">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-230">All</span></span> |<span data-ttu-id="4e9dd-231">Alla</span><span class="sxs-lookup"><span data-stu-id="4e9dd-231">All</span></span> |
| <span data-ttu-id="4e9dd-232">Lägg till Blob</span><span class="sxs-lookup"><span data-stu-id="4e9dd-232">Append Blob</span></span> |<span data-ttu-id="4e9dd-233">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-233">Owner only</span></span> |<span data-ttu-id="4e9dd-234">Endast ägare</span><span class="sxs-lookup"><span data-stu-id="4e9dd-234">Owner only</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4e9dd-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e9dd-235">Next steps</span></span>

* [<span data-ttu-id="4e9dd-236">Autentisering för hello Azure Storage-tjänster</span><span class="sxs-lookup"><span data-stu-id="4e9dd-236">Authentication for hello Azure Storage Services</span></span>](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [<span data-ttu-id="4e9dd-237">Använda signaturer för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="4e9dd-237">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="4e9dd-238">Delegera åtkomst med signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="4e9dd-238">Delegating Access with a Shared Access Signature</span></span>](https://msdn.microsoft.com/library/azure/ee395415.aspx)
