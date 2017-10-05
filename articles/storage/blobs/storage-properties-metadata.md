---
title: "Ange och hämta objektets egenskaper och metadata i Azure Storage | Microsoft Docs"
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
ms.openlocfilehash: 6af66607478c58874f00bcf017a35abfc37888df
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a><span data-ttu-id="540c7-103">Ange och hämta egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="540c7-103">Set and retrieve properties and metadata</span></span>

<span data-ttu-id="540c7-104">Objekt i Azure Storage stödsystem egenskaper och användardefinierade metadata, förutom de data som de innehåller.</span><span class="sxs-lookup"><span data-stu-id="540c7-104">Objects in Azure Storage support system properties and user-defined metadata, in addition to the data they contain.</span></span> <span data-ttu-id="540c7-105">Den här artikeln beskrivs hantera Systemegenskaper och användardefinierade metadata med den [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span><span class="sxs-lookup"><span data-stu-id="540c7-105">This article discusses managing system properties and user-defined metadata with the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).</span></span>

* <span data-ttu-id="540c7-106">**Systemegenskaper**: Systemegenskaper finns på varje storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="540c7-106">**System properties**: System properties exist on each storage resource.</span></span> <span data-ttu-id="540c7-107">Några av dem kan läsa eller ange, medan andra är skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="540c7-107">Some of them can be read or set, while others are read-only.</span></span> <span data-ttu-id="540c7-108">Under försättsbladen motsvarar vissa Systemegenskaper vissa vanliga HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="540c7-108">Under the covers, some system properties correspond to certain standard HTTP headers.</span></span> <span data-ttu-id="540c7-109">Azure storage-klientbiblioteket underhåller dessa åt dig.</span><span class="sxs-lookup"><span data-stu-id="540c7-109">The Azure storage client library maintains these for you.</span></span>

* <span data-ttu-id="540c7-110">**Användardefinierade metadata**: användardefinierade metadata är metadata som du anger på en viss resurs i form av ett namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="540c7-110">**User-defined metadata**: User-defined metadata is metadata that you specify on a given resource in the form of a name-value pair.</span></span> <span data-ttu-id="540c7-111">Du kan använda metadata för att lagra ytterligare värden med en lagringsresurs.</span><span class="sxs-lookup"><span data-stu-id="540c7-111">You can use metadata to store additional values with a storage resource.</span></span> <span data-ttu-id="540c7-112">Värdena ytterligare metadata egna endast för, och påverkar inte hur resursen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="540c7-112">These additional metadata values are for your own purposes only, and do not affect how the resource behaves.</span></span>

<span data-ttu-id="540c7-113">Hämtar värden för egenskapen och metadata för en lagringsresurs är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="540c7-113">Retrieving property and metadata values for a storage resource is a two-step process.</span></span> <span data-ttu-id="540c7-114">Innan du kan läsa dessa värden, du måste uttryckligen hämta dem genom att anropa den **FetchAttributes** metod.</span><span class="sxs-lookup"><span data-stu-id="540c7-114">Before you can read these values, you must explicitly fetch them by calling the **FetchAttributes** method.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="540c7-115">Värden för egenskapen och metadata för en lagringsresurs fylls inte om du anropar en av de **FetchAttributes** metoder.</span><span class="sxs-lookup"><span data-stu-id="540c7-115">Property and metadata values for a storage resource are not populated unless you call one of the **FetchAttributes** methods.</span></span>
>
> <span data-ttu-id="540c7-116">Du får en `400 Bad Request` om alla namn/värde-par innehåller icke-ASCII-tecken.</span><span class="sxs-lookup"><span data-stu-id="540c7-116">You will receive a `400 Bad Request` if any name/value pairs contain non-ASCII characters.</span></span> <span data-ttu-id="540c7-117">Metadata namn/värde-par är giltig HTTP-huvuden och därför måste följa alla begränsningar för HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="540c7-117">Metadata name/value pairs are valid HTTP headers, and so must adhere to all restrictions governing HTTP headers.</span></span> <span data-ttu-id="540c7-118">Du bör därför att du använder URL-kodning eller Base64-kodning för namn och värden som innehåller icke-ASCII-tecken.</span><span class="sxs-lookup"><span data-stu-id="540c7-118">It is therefore recommended that you use URL encoding or Base64 encoding for names and values containing non-ASCII characters.</span></span>
>

## <a name="setting-and-retrieving-properties"></a><span data-ttu-id="540c7-119">Ange och läsa egenskaper</span><span class="sxs-lookup"><span data-stu-id="540c7-119">Setting and retrieving properties</span></span>
<span data-ttu-id="540c7-120">För att hämta egenskapsvärden anropa den **FetchAttributes** metod på blob eller behållare för att fylla i egenskaperna sedan läsa värdena.</span><span class="sxs-lookup"><span data-stu-id="540c7-120">To retrieve property values, call the **FetchAttributes** method on your blob or container to populate the properties, then read the values.</span></span>

<span data-ttu-id="540c7-121">Ange egenskaper för ett objekt genom att ange egenskapen värdet och sedan anropa den **SetProperties** metod.</span><span class="sxs-lookup"><span data-stu-id="540c7-121">To set properties on an object, specify the property value, then call the **SetProperties** method.</span></span>

<span data-ttu-id="540c7-122">Följande exempel skapar en behållare och sedan skriver några av dess egenskapsvärden till konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="540c7-122">The following code example creates a container, then writes some of its property values to a console window.</span></span>

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a><span data-ttu-id="540c7-123">Ange och läsa metadata</span><span class="sxs-lookup"><span data-stu-id="540c7-123">Setting and retrieving metadata</span></span>
<span data-ttu-id="540c7-124">Du kan ange metadata som en eller flera namn-värdepar för en blob eller behållaren resurs.</span><span class="sxs-lookup"><span data-stu-id="540c7-124">You can specify metadata as one or more name-value pairs on a blob or container resource.</span></span> <span data-ttu-id="540c7-125">Lägg till namn / värde-par för att ange metadata, den **Metadata** samlingen på resursen, anropar den **SetMetadata** metod för att spara värdena till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="540c7-125">To set metadata, add name-value pairs to the **Metadata** collection on the resource, then call the **SetMetadata** method to save the values to the service.</span></span>

> [!NOTE]
> <span data-ttu-id="540c7-126">Namnet på dina metadata måste följa namnkonventionerna för C#-identifierare.</span><span class="sxs-lookup"><span data-stu-id="540c7-126">The name of your metadata must conform to the naming conventions for C# identifiers.</span></span>
>
>

<span data-ttu-id="540c7-127">Följande exempel anger metadata för en behållare.</span><span class="sxs-lookup"><span data-stu-id="540c7-127">The following code example sets metadata on a container.</span></span> <span data-ttu-id="540c7-128">Ett värde ställs in med mängdens **Lägg till** metod.</span><span class="sxs-lookup"><span data-stu-id="540c7-128">One value is set using the collection's **Add** method.</span></span> <span data-ttu-id="540c7-129">Det andra värdet med implicit nyckel/värde-syntax.</span><span class="sxs-lookup"><span data-stu-id="540c7-129">The other value is set using implicit key/value syntax.</span></span> <span data-ttu-id="540c7-130">Båda är giltiga.</span><span class="sxs-lookup"><span data-stu-id="540c7-130">Both are valid.</span></span>

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set the container's metadata.
    container.SetMetadata();
}
```

<span data-ttu-id="540c7-131">För att hämta metadata anropa den **FetchAttributes** metod på blob eller behållare för att fylla i **Metadata** samling, Läs värden som visas i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="540c7-131">To retrieve metadata, call the **FetchAttributes** method on your blob or container to populate the **Metadata** collection, then read the values, as shown in the example below.</span></span>

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order to populate the container's properties and metadata.
    container.FetchAttributes();

    //Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="540c7-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="540c7-132">Next steps</span></span>
* [<span data-ttu-id="540c7-133">Azure Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="540c7-133">Azure Storage Client Library for .NET reference</span></span>](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [<span data-ttu-id="540c7-134">Azure Storage-klientbibliotek för .NET-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="540c7-134">Azure Storage Client Library for .NET NuGet package</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
