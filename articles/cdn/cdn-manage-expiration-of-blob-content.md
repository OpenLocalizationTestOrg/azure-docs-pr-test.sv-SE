---
title: "Hantera förfallodatum för Azure Storage-blobbar i Azure CDN | Microsoft Docs"
description: "Läs mer om alternativen för att styra time to live för BLOB i Azure CDN cachelagring."
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
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="12c9b-103">Hantera förfallodatum för Azure Storage-blobbar i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="12c9b-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="12c9b-104">Azure Web Apps/molntjänster, ASP.NET och IIS</span><span class="sxs-lookup"><span data-stu-id="12c9b-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="12c9b-105">Azure Storage blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="12c9b-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="12c9b-106">Den [blob-tjänst](../storage/common/storage-introduction.md#blob-storage) i [Azure Storage](../storage/common/storage-introduction.md) är en av flera Azure-baserade ursprung integrerat med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="12c9b-106">The [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="12c9b-107">Alla offentliga blob-innehåll cachelagras i Azure CDN tills dess time to live (TTL) går ut.</span><span class="sxs-lookup"><span data-stu-id="12c9b-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="12c9b-108">TTL-värdet bestäms av den [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) i HTTP-svaret från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="12c9b-108">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="12c9b-109">Du kan välja att ange inga TTL-värde för en blob.</span><span class="sxs-lookup"><span data-stu-id="12c9b-109">You may choose to set no TTL on a blob.</span></span>  <span data-ttu-id="12c9b-110">I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.</span><span class="sxs-lookup"><span data-stu-id="12c9b-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="12c9b-111">Mer information om hur Azure CDN fungerar för att påskynda åtkomst till blobbar och andra filer finns i [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12c9b-111">For more information about how Azure CDN works to speed up access to blobs and other files, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="12c9b-112">Mer information om Azure Storage blob-tjänsten finns [Blob-Tjänstkoncept](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="12c9b-112">For more details on the Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="12c9b-113">Den här kursen visar flera olika sätt som du kan ange TTL-värdet för en blobb i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="12c9b-113">This tutorial demonstrates several ways that you can set the TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="12c9b-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="12c9b-114">Azure PowerShell</span></span>
<span data-ttu-id="12c9b-115">[Azure PowerShell](/powershell/azure/overview) är en av de snabbaste, mest kraftfulla sätten att administrera din Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="12c9b-115">[Azure PowerShell](/powershell/azure/overview) is one of the quickest, most powerful ways to administer your Azure services.</span></span>  <span data-ttu-id="12c9b-116">Använd den `Get-AzureStorageBlob` för att hämta en referens till blob, ange den `.ICloudBlob.Properties.CacheControl` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12c9b-116">Use the `Get-AzureStorageBlob` cmdlet to get a reference to the blob, then set the `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="12c9b-117">Du kan också använda PowerShell för att [hantera dina CDN-profiler och slutpunkter](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="12c9b-117">You can also use PowerShell to [manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="12c9b-118">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="12c9b-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="12c9b-119">Om du vill ange en blob TTL-värde med hjälp av .NET använder den [Azure Storage-klientbibliotek för .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) att ange den [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12c9b-119">To set a blob's TTL using .NET, use the [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) to set the [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="12c9b-120">Det finns många fler .NET-kodexempel i den [Azure Blob Storage-exempel för .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="12c9b-120">There are many more .NET code samples available in the [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="12c9b-121">Andra metoder</span><span class="sxs-lookup"><span data-stu-id="12c9b-121">Other methods</span></span>
* [<span data-ttu-id="12c9b-122">Azure kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="12c9b-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="12c9b-123">När du hämtar blob, ange den *cacheControl* egenskapen med det `-p` växla.</span><span class="sxs-lookup"><span data-stu-id="12c9b-123">When uploading the blob, set the *cacheControl* property using the `-p` switch.</span></span>  <span data-ttu-id="12c9b-124">Det här exemplet anger TTL-värdet till en timme (3600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="12c9b-124">This example sets the TTL to one hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="12c9b-125">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="12c9b-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="12c9b-126">Uttryckligen ställa in den *x-ms-blob-cache-control* -egenskapen i en [placera Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [placera Blockeringslista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), eller [ange egenskaper för Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) begäran.</span><span class="sxs-lookup"><span data-stu-id="12c9b-126">Explicitly set the *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="12c9b-127">Verktyg för lagringshantering från tredje part</span><span class="sxs-lookup"><span data-stu-id="12c9b-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="12c9b-128">Vissa tredjeparts-hanteringsverktyg för Azure Storage kan du ange den *CacheControl* egenskapen för BLOB.</span><span class="sxs-lookup"><span data-stu-id="12c9b-128">Some third-party Azure Storage management tools allow you to set the *CacheControl* property on blobs.</span></span> 

## <a name="testing-the-cache-control-header"></a><span data-ttu-id="12c9b-129">Testa den *Cache-Control* sidhuvud</span><span class="sxs-lookup"><span data-stu-id="12c9b-129">Testing the *Cache-Control* header</span></span>
<span data-ttu-id="12c9b-130">Du kan enkelt kontrollera TTL-värde på dina blobbar.</span><span class="sxs-lookup"><span data-stu-id="12c9b-130">You can easily verify the TTL of your blobs.</span></span>  <span data-ttu-id="12c9b-131">Använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test som din blob inklusive den *Cache-Control* Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="12c9b-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including the *Cache-Control* response header.</span></span>  <span data-ttu-id="12c9b-132">Du kan också använda ett verktyg som **wget**, [Postman](https://www.getpostman.com/), eller [Fiddler](http://www.telerik.com/fiddler) att undersöka svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="12c9b-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) to examine the response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12c9b-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12c9b-133">Next Steps</span></span>
* [<span data-ttu-id="12c9b-134">Läs mer om den *Cache-Control* sidhuvud</span><span class="sxs-lookup"><span data-stu-id="12c9b-134">Read about the *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="12c9b-135">Lär dig hur du hanterar du förfallodatum för Molntjänsten innehåll i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="12c9b-135">Learn how to manage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

