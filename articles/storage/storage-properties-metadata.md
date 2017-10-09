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
# <a name="set-and-retrieve-properties-and-metadata"></a><span data-ttu-id="80428-103">Ange och hämta egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="80428-103">Set and retrieve properties and metadata</span></span>

<span data-ttu-id="80428-104">Objekt i Azure Storage stödsystem egenskaper och användardefinierade metadata dessutom toohello data de innehåller.</span><span class="sxs-lookup"><span data-stu-id="80428-104">Objects in Azure Storage support system properties and user-defined metadata, in addition toohello data they contain.</span></span> <span data-ttu-id="80428-105">Den här artikeln beskrivs hantera Systemegenskaper och användardefinierade metadata med hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="80428-105">This article discusses managing system properties and user-defined metadata with hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span>

* <span data-ttu-id="80428-106">**Systemegenskaper**: Systemegenskaper finns på varje storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="80428-106">**System properties**: System properties exist on each storage resource.</span></span> <span data-ttu-id="80428-107">Några av dem kan läsa eller ange, medan andra är skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="80428-107">Some of them can be read or set, while others are read-only.</span></span> <span data-ttu-id="80428-108">Under hello omfattar motsvarar vissa Systemegenskaper toocertain standard-HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="80428-108">Under hello covers, some system properties correspond toocertain standard HTTP headers.</span></span> <span data-ttu-id="80428-109">hello Azure storage-klientbibliotek underhåller dessa åt dig.</span><span class="sxs-lookup"><span data-stu-id="80428-109">hello Azure storage client library maintains these for you.</span></span>

* <span data-ttu-id="80428-110">**Användardefinierade metadata**: användardefinierade metadata är metadata som du anger på en viss resurs i hello form av ett namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="80428-110">**User-defined metadata**: User-defined metadata is metadata that you specify on a given resource in hello form of a name-value pair.</span></span> <span data-ttu-id="80428-111">Du kan använda metadata toostore ytterligare värden med en lagringsresurs.</span><span class="sxs-lookup"><span data-stu-id="80428-111">You can use metadata toostore additional values with a storage resource.</span></span> <span data-ttu-id="80428-112">Värdena ytterligare metadata egna endast för, och påverkar inte hur hello resursen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="80428-112">These additional metadata values are for your own purposes only, and do not affect how hello resource behaves.</span></span>

<span data-ttu-id="80428-113">Hämtar värden för egenskapen och metadata för en lagringsresurs är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="80428-113">Retrieving property and metadata values for a storage resource is a two-step process.</span></span> <span data-ttu-id="80428-114">Innan du kan läsa dessa värden, du måste uttryckligen hämta dem genom att anropa hello **FetchAttributes** metod.</span><span class="sxs-lookup"><span data-stu-id="80428-114">Before you can read these values, you must explicitly fetch them by calling hello **FetchAttributes** method.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80428-115">Värden för egenskapen och metadata för en lagringsresurs fylls inte om du anropar en hello **FetchAttributes** metoder.</span><span class="sxs-lookup"><span data-stu-id="80428-115">Property and metadata values for a storage resource are not populated unless you call one of hello **FetchAttributes** methods.</span></span>
>
> <span data-ttu-id="80428-116">Du får en `400 Bad Request` om alla namn/värde-par innehåller icke-ASCII-tecken.</span><span class="sxs-lookup"><span data-stu-id="80428-116">You will receive a `400 Bad Request` if any name/value pairs contain non-ASCII characters.</span></span> <span data-ttu-id="80428-117">Metadata namn/värde-par är giltig HTTP-huvuden och så måste följa tooall begränsningar för HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="80428-117">Metadata name/value pairs are valid HTTP headers, and so must adhere tooall restrictions governing HTTP headers.</span></span> <span data-ttu-id="80428-118">Du bör därför att du använder URL-kodning eller Base64-kodning för namn och värden som innehåller icke-ASCII-tecken.</span><span class="sxs-lookup"><span data-stu-id="80428-118">It is therefore recommended that you use URL encoding or Base64 encoding for names and values containing non-ASCII characters.</span></span>
>

## <a name="setting-and-retrieving-properties"></a><span data-ttu-id="80428-119">Ange och läsa egenskaper</span><span class="sxs-lookup"><span data-stu-id="80428-119">Setting and retrieving properties</span></span>
<span data-ttu-id="80428-120">tooretrieve egenskapsvärden, anrop hello **FetchAttributes** metod på din blob-behållaren toopopulate hello egenskaper eller Läs hello värden.</span><span class="sxs-lookup"><span data-stu-id="80428-120">tooretrieve property values, call hello **FetchAttributes** method on your blob or container toopopulate hello properties, then read hello values.</span></span>

<span data-ttu-id="80428-121">tooset egenskaper för ett objekt, ange hello egenskapsvärdet och sedan anropa hello **SetProperties** metod.</span><span class="sxs-lookup"><span data-stu-id="80428-121">tooset properties on an object, specify hello property value, then call hello **SetProperties** method.</span></span>

<span data-ttu-id="80428-122">hello följande kodexempel skapar en behållare och sedan skriver några av dess egenskapen värden tooa konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="80428-122">hello following code example creates a container, then writes some of its property values tooa console window.</span></span>

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

## <a name="setting-and-retrieving-metadata"></a><span data-ttu-id="80428-123">Ange och läsa metadata</span><span class="sxs-lookup"><span data-stu-id="80428-123">Setting and retrieving metadata</span></span>
<span data-ttu-id="80428-124">Du kan ange metadata som en eller flera namn-värdepar för en blob eller behållaren resurs.</span><span class="sxs-lookup"><span data-stu-id="80428-124">You can specify metadata as one or more name-value pairs on a blob or container resource.</span></span> <span data-ttu-id="80428-125">tooset metadata, lägga till namn-värdepar toohello **Metadata** samlingen på hello resursen sedan anropa hello **SetMetadata** metoden toosave hello värden toohello service.</span><span class="sxs-lookup"><span data-stu-id="80428-125">tooset metadata, add name-value pairs toohello **Metadata** collection on hello resource, then call hello **SetMetadata** method toosave hello values toohello service.</span></span>

> [!NOTE]
> <span data-ttu-id="80428-126">hello namnet på dina metadata måste följa toohello namnkonventioner för C#-identifierare.</span><span class="sxs-lookup"><span data-stu-id="80428-126">hello name of your metadata must conform toohello naming conventions for C# identifiers.</span></span>
>
>

<span data-ttu-id="80428-127">hello anger följande kodexempel metadata för en behållare.</span><span class="sxs-lookup"><span data-stu-id="80428-127">hello following code example sets metadata on a container.</span></span> <span data-ttu-id="80428-128">Ett värde ställs in med hello samling **Lägg till** metod.</span><span class="sxs-lookup"><span data-stu-id="80428-128">One value is set using hello collection's **Add** method.</span></span> <span data-ttu-id="80428-129">hello värdet andra med implicit nyckel/värde-syntax.</span><span class="sxs-lookup"><span data-stu-id="80428-129">hello other value is set using implicit key/value syntax.</span></span> <span data-ttu-id="80428-130">Båda är giltiga.</span><span class="sxs-lookup"><span data-stu-id="80428-130">Both are valid.</span></span>

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

<span data-ttu-id="80428-131">tooretrieve metadata, anrop hello **FetchAttributes** metod på din blob eller behållaren toopopulate hello **Metadata** samling, Läs hello värden som visas i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="80428-131">tooretrieve metadata, call hello **FetchAttributes** method on your blob or container toopopulate hello **Metadata** collection, then read hello values, as shown in hello example below.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="80428-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80428-132">Next steps</span></span>
* [<span data-ttu-id="80428-133">Azure Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="80428-133">Azure Storage Client Library for .NET reference</span></span>](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [<span data-ttu-id="80428-134">Azure Storage-klientbibliotek för .NET-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="80428-134">Azure Storage Client Library for .NET NuGet package</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
