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
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Hantera förfallodatum för Azure Storage-blobbar i Azure CDN
> [!div class="op_single_selector"]
> * [Azure Web Apps/molntjänster, ASP.NET och IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage blob-tjänst](cdn-manage-expiration-of-blob-content.md)
> 
> 

Hej [blob-tjänst](../storage/common/storage-introduction.md#blob-storage) i [Azure Storage](../storage/common/storage-introduction.md) är en av flera Azure-baserade ursprung integrerat med Azure CDN.  Alla offentliga blob-innehåll cachelagras i Azure CDN tills dess time to live (TTL) går ut.  hello TTL bestäms av hello [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello HTTP-svar från Azure Storage.

> [!TIP]
> Du kan välja tooset inga TTL för en blob.  I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.
> 
> Mer information om hur fungerar Azure CDN toospeed åtkomst tooblobs och andra filer finns hello [översikt över Azure CDN](cdn-overview.md).
> 
> Mer information om hello Azure Storage blob-tjänsten finns [Blob-Tjänstkoncept](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Den här kursen visar flera olika sätt att du kan ange hello TTL-värde för en blobb i Azure Storage.  

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) är en av hello snabbaste, mest kraftfulla sätt tooadminister Azure-tjänster.  Använd hello `Get-AzureStorageBlob` cmdlet tooget toohello referensblobben, ange hello `.ICloudBlob.Properties.CacheControl` egenskapen. 

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
> Du kan också använda PowerShell för[hantera dina CDN-profiler och slutpunkter](cdn-manage-powershell.md).
> 
> 

## <a name="azure-storage-client-library-for-net"></a>Azure Storage-klientbibliotek för .NET
tooset en blob har TTL-värde med hjälp av .NET, Använd hello [Azure Storage-klientbibliotek för .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) egenskapen.

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
> Det finns många fler .NET-kodexempel i hello [Azure Blob Storage-exempel för .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Andra metoder
* [Azure kommandoradsgränssnittet](../cli-install-nodejs.md)
  
    När du överför hello blob, ange hello *cacheControl* egenskap med hello `-p` växla.  Det här exemplet anger hello TTL tooone timme (3600 sekunder).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Uttryckligen ange hello *x-ms-blob-cache-control* -egenskapen i en [placera Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [placera Blockeringslista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), eller [ange egenskaper för Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) begäran.
* Verktyg för lagringshantering från tredje part
  
    Vissa tredjeparts-hanteringsverktyg för Azure Storage kan du tooset hello *CacheControl* egenskapen för BLOB. 

## <a name="testing-hello-cache-control-header"></a>Tester hello *Cache-Control* sidhuvud
Du kan enkelt kontrollera hello TTL-värde på dina blobbar.  Använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), testa att din blob omfattar hello *Cache-Control* Svarsrubrik.  Du kan också använda ett verktyg som **wget**, [Postman](https://www.getpostman.com/), eller [Fiddler](http://www.telerik.com/fiddler) tooexamine hello-svarshuvuden.

## <a name="next-steps"></a>Nästa steg
* [Läs mer om hello *Cache-Control* sidhuvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Lär dig hur toomanage förfallotiden för Molntjänsten innehåll i Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

