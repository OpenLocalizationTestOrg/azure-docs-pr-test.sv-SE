---
title: "aaaSet och hämta objekt egenskaper och metadata i Azure Storage | Microsoft Docs"
description: "Lagra anpassade metadata för objekt i Azure Storage, och ange och hämta Systemegenskaper."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 44f9243183014845964f337b476a6b0069dc0902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Ange och hämta egenskaper och metadata

Objekt i Azure Storage stödsystem egenskaper och användardefinierade metadata dessutom toohello data de innehåller. Den här artikeln beskrivs hantera Systemegenskaper och användardefinierade metadata med hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Systemegenskaper**: Systemegenskaper finns på varje storage-resurs. Några av dem kan läsa eller ange, medan andra är skrivskyddad. Under hello omfattar motsvarar vissa Systemegenskaper toocertain standard-HTTP-huvuden. hello Azure storage-klientbibliotek underhåller dessa åt dig.

* **Användardefinierade metadata**: användardefinierade metadata är metadata som du anger på en viss resurs i hello form av ett namn / värde-par. Du kan använda metadata toostore ytterligare värden med en lagringsresurs. Värdena ytterligare metadata egna endast för, och påverkar inte hur hello resursen ska fungera.

Hämtar värden för egenskapen och metadata för en lagringsresurs är en tvåstegsprocess. Innan du kan läsa dessa värden, du måste uttryckligen hämta dem genom att anropa hello **FetchAttributes** metod.

> [!IMPORTANT]
> Värden för egenskapen och metadata för en lagringsresurs fylls inte om du anropar en hello **FetchAttributes** metoder.
>
> Du får en `400 Bad Request` om alla namn/värde-par innehåller icke-ASCII-tecken. Metadata namn/värde-par är giltig HTTP-huvuden och så måste följa tooall begränsningar för HTTP-huvuden. Du bör därför att du använder URL-kodning eller Base64-kodning för namn och värden som innehåller icke-ASCII-tecken.
>

## <a name="setting-and-retrieving-properties"></a>Ange och läsa egenskaper
tooretrieve egenskapsvärden, anrop hello **FetchAttributes** metod på din blob-behållaren toopopulate hello egenskaper eller Läs hello värden.

tooset egenskaper för ett objekt, ange hello egenskapsvärdet och sedan anropa hello **SetProperties** metod.

hello följande kodexempel skapar en behållare och sedan skriver några av dess egenskapen värden tooa konsolfönstret.

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create hello service client object for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Ange och läsa metadata
Du kan ange metadata som en eller flera namn-värdepar för en blob eller behållaren resurs. tooset metadata, lägga till namn-värdepar toohello **Metadata** samlingen på hello resursen sedan anropa hello **SetMetadata** metoden toosave hello värden toohello service.

> [!NOTE]
> hello namnet på dina metadata måste följa toohello namnkonventioner för C#-identifierare.
>
>

hello anger följande kodexempel metadata för en behållare. Ett värde ställs in med hello samling **Lägg till** metod. hello värdet andra med implicit nyckel/värde-syntax. Båda är giltiga.

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata toohello container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set hello container's metadata.
    container.SetMetadata();
}
```

tooretrieve metadata, anrop hello **FetchAttributes** metod på din blob eller behållaren toopopulate hello **Metadata** samling, Läs hello värden som visas i hello exemplet nedan.

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order toopopulate hello container's properties and metadata.
    container.FetchAttributes();

    //Enumerate hello container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>Nästa steg
* [Azure Storage-klientbibliotek för .NET-referens](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [Azure Storage-klientbibliotek för .NET-NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.Storage/)
