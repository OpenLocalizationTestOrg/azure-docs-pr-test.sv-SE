---
title: "aaaManage förfallodatum för Azure Storage-blobbar i Azure CDN | Microsoft Docs"
description: "Läs mer om hello alternativ för att styra time to live för BLOB i Azure CDN cachelagring."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9fecae9639deb28977da7f851e1da4a823ddc4e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="7e295-103">Hantera förfallodatum för Azure Storage-blobbar i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7e295-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e295-104">Azure Web Apps/molntjänster, ASP.NET och IIS</span><span class="sxs-lookup"><span data-stu-id="7e295-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="7e295-105">Azure Storage blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="7e295-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="7e295-106">Hej [blob-tjänst](../storage/common/storage-introduction.md#blob-storage) i [Azure Storage](../storage/common/storage-introduction.md) är en av flera Azure-baserade ursprung integrerat med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7e295-106">hello [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="7e295-107">Alla offentliga blob-innehåll cachelagras i Azure CDN tills dess time to live (TTL) går ut.</span><span class="sxs-lookup"><span data-stu-id="7e295-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="7e295-108">hello TTL bestäms av hello [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello HTTP-svar från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7e295-108">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="7e295-109">Du kan välja tooset inga TTL för en blob.</span><span class="sxs-lookup"><span data-stu-id="7e295-109">You may choose tooset no TTL on a blob.</span></span>  <span data-ttu-id="7e295-110">I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.</span><span class="sxs-lookup"><span data-stu-id="7e295-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="7e295-111">Mer information om hur fungerar Azure CDN toospeed åtkomst tooblobs och andra filer finns hello [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e295-111">For more information about how Azure CDN works toospeed up access tooblobs and other files, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="7e295-112">Mer information om hello Azure Storage blob-tjänsten finns [Blob-Tjänstkoncept](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e295-112">For more details on hello Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="7e295-113">Den här kursen visar flera olika sätt att du kan ange hello TTL-värde för en blobb i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7e295-113">This tutorial demonstrates several ways that you can set hello TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="7e295-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e295-114">Azure PowerShell</span></span>
<span data-ttu-id="7e295-115">[Azure PowerShell](/powershell/azure/overview) är en av hello snabbaste, mest kraftfulla sätt tooadminister Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e295-115">[Azure PowerShell](/powershell/azure/overview) is one of hello quickest, most powerful ways tooadminister your Azure services.</span></span>  <span data-ttu-id="7e295-116">Använd hello `Get-AzureStorageBlob` cmdlet tooget toohello referensblobben, ange hello `.ICloudBlob.Properties.CacheControl` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7e295-116">Use hello `Get-AzureStorageBlob` cmdlet tooget a reference toohello blob, then set hello `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference toohello blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send hello update toohello cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="7e295-117">Du kan också använda PowerShell för[hantera dina CDN-profiler och slutpunkter](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7e295-117">You can also use PowerShell too[manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="7e295-118">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="7e295-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="7e295-119">tooset en blob har TTL-värde med hjälp av .NET, Använd hello [Azure Storage-klientbibliotek för .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7e295-119">tooset a blob's TTL using .NET, use hello [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with hello blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference toohello container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference toohello blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update hello blob's properties in hello cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="7e295-120">Det finns många fler .NET-kodexempel i hello [Azure Blob Storage-exempel för .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="7e295-120">There are many more .NET code samples available in hello [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="7e295-121">Andra metoder</span><span class="sxs-lookup"><span data-stu-id="7e295-121">Other methods</span></span>
* [<span data-ttu-id="7e295-122">Azure kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="7e295-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="7e295-123">När du överför hello blob, ange hello *cacheControl* egenskap med hello `-p` växla.</span><span class="sxs-lookup"><span data-stu-id="7e295-123">When uploading hello blob, set hello *cacheControl* property using hello `-p` switch.</span></span>  <span data-ttu-id="7e295-124">Det här exemplet anger hello TTL tooone timme (3600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="7e295-124">This example sets hello TTL tooone hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="7e295-125">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="7e295-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="7e295-126">Uttryckligen ange hello *x-ms-blob-cache-control* -egenskapen i en [placera Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [placera Blockeringslista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), eller [ange egenskaper för Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) begäran.</span><span class="sxs-lookup"><span data-stu-id="7e295-126">Explicitly set hello *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="7e295-127">Verktyg för lagringshantering från tredje part</span><span class="sxs-lookup"><span data-stu-id="7e295-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="7e295-128">Vissa tredjeparts-hanteringsverktyg för Azure Storage kan du tooset hello *CacheControl* egenskapen för BLOB.</span><span class="sxs-lookup"><span data-stu-id="7e295-128">Some third-party Azure Storage management tools allow you tooset hello *CacheControl* property on blobs.</span></span> 

## <a name="testing-hello-cache-control-header"></a><span data-ttu-id="7e295-129">Tester hello *Cache-Control* sidhuvud</span><span class="sxs-lookup"><span data-stu-id="7e295-129">Testing hello *Cache-Control* header</span></span>
<span data-ttu-id="7e295-130">Du kan enkelt kontrollera hello TTL-värde på dina blobbar.</span><span class="sxs-lookup"><span data-stu-id="7e295-130">You can easily verify hello TTL of your blobs.</span></span>  <span data-ttu-id="7e295-131">Använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testa att din blob omfattar hello *Cache-Control* Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="7e295-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including hello *Cache-Control* response header.</span></span>  <span data-ttu-id="7e295-132">Du kan också använda ett verktyg som **wget**, [Postman](https://www.getpostman.com/), eller [Fiddler](http://www.telerik.com/fiddler) tooexamine hello-svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="7e295-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) tooexamine hello response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e295-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e295-133">Next Steps</span></span>
* [<span data-ttu-id="7e295-134">Läs mer om hello *Cache-Control* sidhuvud</span><span class="sxs-lookup"><span data-stu-id="7e295-134">Read about hello *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="7e295-135">Lär dig hur toomanage förfallotiden för Molntjänsten innehåll i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7e295-135">Learn how toomanage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

