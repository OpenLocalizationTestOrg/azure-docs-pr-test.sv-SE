---
title: "Hur du använder blob storage (objektlagring) från C++ | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
services: storage
documentationcenter: .net
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 53844120-1c48-4e2f-8f77-5359ed0147a4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 3f28fbee4e267ab6962e2f73af5af6461cc16448
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-blob-storage-from-c"></a><span data-ttu-id="1a8d8-103">Hur du använder Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="1a8d8-103">How to use Blob Storage from C++</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="1a8d8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1a8d8-104">Overview</span></span>
<span data-ttu-id="1a8d8-105">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="1a8d8-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="1a8d8-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="1a8d8-108">Den här guiden visar hur du utför vanliga scenarier som använder tjänsten Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-108">This guide will demonstrate how to perform common scenarios using the Azure Blob storage service.</span></span> <span data-ttu-id="1a8d8-109">Exemplen är skrivna i C++ och Använd den [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1a8d8-109">The samples are written in C++ and use the [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="1a8d8-110">Scenarier som tas upp inkluderar **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span>  

> [!NOTE]
> <span data-ttu-id="1a8d8-111">Den här handboken riktar sig mot Azure Storage-klientbibliotek för C++ version 1.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-111">This guide targets the Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="1a8d8-112">Den rekommenderade versionen är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="1a8d8-112">The recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="1a8d8-113">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="1a8d8-113">Create a C++ application</span></span>
<span data-ttu-id="1a8d8-114">I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-114">In this guide, you will use storage features which can be run within a C++ application.</span></span>  

<span data-ttu-id="1a8d8-115">Om du vill göra det, behöver du installera Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-115">To do so, you will need to install the Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>   

<span data-ttu-id="1a8d8-116">Om du vill installera Azure Storage-klientbibliotek för C++ kan du använda följande metoder:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-116">To install the Azure Storage Client Library for C++, you can use the following methods:</span></span>

* <span data-ttu-id="1a8d8-117">**Linux:** Följ instruktionerna den [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-117">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="1a8d8-118">**Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-118">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="1a8d8-119">Skriv följande kommando i den [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-119">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>  
  
     <span data-ttu-id="1a8d8-120">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="1a8d8-120">Install-Package wastorage</span></span>

## <a name="configure-your-application-to-access-blob-storage"></a><span data-ttu-id="1a8d8-121">Konfigurera ditt program för att få åtkomst till Blob Storage</span><span class="sxs-lookup"><span data-stu-id="1a8d8-121">Configure your application to access Blob Storage</span></span>
<span data-ttu-id="1a8d8-122">Lägga till följande uttryck överst i filen C++ där du vill använda Azure storage API: er för att få åtkomst till blobbar:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-122">Add the following include statements to the top of the C++ file where you want to use the Azure storage APIs to access blobs:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="1a8d8-123">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="1a8d8-123">Setup an Azure storage connection string</span></span>
<span data-ttu-id="1a8d8-124">Ett Azure storage-klienten använder en anslutningssträng för lagring för att lagra slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-124">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="1a8d8-125">När den körs i ett klientprogram, du måste ange anslutningssträngen för lagring i följande format, med hjälp av namnet på ditt lagringskonto och åtkomstnyckeln lagring för lagringskontot som anges i den [Azure Portal](https://portal.azure.com) för den *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-125">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the storage access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="1a8d8-126">Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="1a8d8-126">For information on storage accounts and access keys, see [About Azure Storage Accounts](storage-create-storage-account.md).</span></span> <span data-ttu-id="1a8d8-127">Det här exemplet visar hur du kan deklarera statiska fält att lagra anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-127">This example shows how you can declare a static field to hold the connection string:</span></span>  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="1a8d8-128">Om du vill testa ditt program i din lokala Windows-dator, du kan använda Microsoft Azure [lagringsemulatorn](storage-use-emulator.md) som installeras med den [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1a8d8-128">To test your application in your local Windows computer, you can use the Microsoft Azure [storage emulator](storage-use-emulator.md) that is installed with the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="1a8d8-129">Storage-emulatorn är ett verktyg som simulerar Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-129">The storage emulator is a utility that simulates the Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="1a8d8-130">I följande exempel visas hur du kan deklarera statiska fält för anslutningssträngen till din lokala storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-130">The following example shows how you can declare a static field to hold the connection string to your local storage emulator:</span></span>

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="1a8d8-131">Om du vill starta Azure storage-emulatorn, Välj den **starta** knappen eller tryck på den **Windows** nyckel.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-131">To start the Azure storage emulator, Select the **Start** button or press the **Windows** key.</span></span> <span data-ttu-id="1a8d8-132">Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** från listan med program.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-132">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from the list of applications.</span></span>  

<span data-ttu-id="1a8d8-133">Följande exempel förutsätter att du har använt ett av dessa två sätt för att hämta anslutningssträngen för lagring.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-133">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="1a8d8-134">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="1a8d8-134">Retrieve your connection string</span></span>
<span data-ttu-id="1a8d8-135">Du kan använda den **cloud_storage_account** klass för att representera kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-135">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="1a8d8-136">Du kan använda för att hämta information om ditt lagringskonto från anslutningssträngen för lagring av **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-136">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="1a8d8-137">Sedan hämta en referens till en **cloud_blob_client** klassen som du kan hämta objekt som representerar behållare och blobbar som lagras i Blob Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-137">Next, get a reference to a **cloud_blob_client** class as it allows you to retrieve objects that represent containers and blobs stored within the Blob Storage Service.</span></span> <span data-ttu-id="1a8d8-138">Följande kod skapar en **cloud_blob_client** objekt med kontot lagringsobjektet vi hämta ovan:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-138">The following code creates a **cloud_blob_client** object using the storage account object we retrieved above:</span></span>  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a><span data-ttu-id="1a8d8-139">Så här: skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="1a8d8-139">How to: Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="1a8d8-140">Det här exemplet visas hur du skapar en behållare om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-140">This example shows how to create a container if it does not already exist:</span></span>  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create the container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

<span data-ttu-id="1a8d8-141">Den nya behållaren är privat som standard, och du måste ange din lagringsåtkomstnyckel för att ladda ned blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-141">By default, the new container is private and you must specify your storage access key to download blobs from this container.</span></span> <span data-ttu-id="1a8d8-142">Om du vill göra filerna (BLOB) i behållaren tillgängliga för alla kan du ange att behållaren ska vara offentlig med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-142">If you want to make the files (blobs) within the container available to everyone, you can set the container to be public using the following code:</span></span>  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

<span data-ttu-id="1a8d8-143">Alla på Internet kan se blobbar i en offentlig behållare, men du kan ändra eller ta bort dem bara om du har rätt åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-143">Anyone on the Internet can see blobs in a public container, but you can modify or delete them only if you have the appropriate access key.</span></span>  

## <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="1a8d8-144">Så här: ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="1a8d8-144">How to: Upload a blob into a container</span></span>
<span data-ttu-id="1a8d8-145">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-145">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="1a8d8-146">I de flesta fall är blockblobbar den rekommenderade typen.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-146">In the majority of cases, block blob is the recommended type to use.</span></span>  

<span data-ttu-id="1a8d8-147">Om du vill ladda upp en fil till en blockblobb hämtar du en referens för behållaren och använder den för att hämta en referens för blockblobben.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-147">To upload a file to a block blob, get a container reference and use it to get a block blob reference.</span></span> <span data-ttu-id="1a8d8-148">När du har en blobbreferens kan du ladda upp en dataström till den genom att anropa den **upload_from_stream** metod.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-148">Once you have a blob reference, you can upload any stream of data to it by calling the **upload_from_stream** method.</span></span> <span data-ttu-id="1a8d8-149">Med den här åtgärden skapas blobben om den inte fanns tidigare, eller skrivs över om den redan fanns.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-149">This operation will create the blob if it didn't previously exist, or overwrite it if it does exist.</span></span> <span data-ttu-id="1a8d8-150">Följande exempel visar hur du laddar upp en blobb till en behållare och förutsätter att behållaren redan hade skapats.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-150">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite the "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference to a blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

<span data-ttu-id="1a8d8-151">Du kan också använda den **upload_from_file** metod för att överföra en fil till en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-151">Alternatively, you can use the **upload_from_file** method to upload a file to a block blob.</span></span>

## <a name="how-to-list-the-blobs-in-a-container"></a><span data-ttu-id="1a8d8-152">Så här: Visa blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="1a8d8-152">How to: List the blobs in a container</span></span>
<span data-ttu-id="1a8d8-153">Om du vill visa blobbar i en behållare börjar du med att hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-153">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="1a8d8-154">Du kan sedan använda behållarens **list_blobs** metod för att hämta blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-154">You can then use the container's **list_blobs** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="1a8d8-155">Att komma åt den omfattande uppsättningen med egenskaper och metoder för en returnerad **list_blob_item**, måste du anropa den **list_blob_item.as_blob** metod för att hämta en **cloud_blob** objektet, eller **list_blob.as_directory** metod för att hämta ett cloud_blob_directory-objekt.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-155">To access the rich set of properties and methods for a returned **list_blob_item**, you must call the **list_blob_item.as_blob** method to get a  **cloud_blob** object, or the **list_blob.as_directory** method to get a  cloud_blob_directory object.</span></span> <span data-ttu-id="1a8d8-156">Följande kod visar hur du hämtar och returnerar URI: N för varje objekt i den **Mina exempel behållaren** behållare:</span><span class="sxs-lookup"><span data-stu-id="1a8d8-156">The following code demonstrates how to retrieve and output the URI of each item in the **my-sample-container** container:</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

<span data-ttu-id="1a8d8-157">Mer information om hur du visar operations finns [lista Azure Storage-resurser i C++](storage-c-plus-plus-enumeration.md).</span><span class="sxs-lookup"><span data-stu-id="1a8d8-157">For more details on listing operations, see [List Azure Storage Resources in C++](storage-c-plus-plus-enumeration.md).</span></span>

## <a name="how-to-download-blobs"></a><span data-ttu-id="1a8d8-158">Så här: ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="1a8d8-158">How to: Download blobs</span></span>
<span data-ttu-id="1a8d8-159">Om du vill ladda ned blobbar först hämta en blobbreferens och anropar den **download_to_stream** metod.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-159">To download blobs, first retrieve a blob reference and then call the **download_to_stream** method.</span></span> <span data-ttu-id="1a8d8-160">I följande exempel används den **download_to_stream** metod för att överföra blobbinnehållet till ett stream-objekt som du sedan kan spara till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-160">The following example uses the **download_to_stream** method to transfer the blob contents to a stream object that you can then persist to a local file.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents to a file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

<span data-ttu-id="1a8d8-161">Du kan också använda den **download_to_file** metod för att hämta innehållet i en blobb till en fil.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-161">Alternatively, you can use the **download_to_file** method to download the contents of a blob to a file.</span></span>
<span data-ttu-id="1a8d8-162">Dessutom kan du kan också använda den **download_text** metod för att hämta innehållet i en blob som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-162">In addition, you can also use the **download_text** method to download the contents of a blob as a text string.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download the contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a><span data-ttu-id="1a8d8-163">Så här: ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="1a8d8-163">How to: Delete blobs</span></span>
<span data-ttu-id="1a8d8-164">Om du vill ta bort en blobb först hämta en blobbreferens och anropar den **delete_blob** metod på den.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-164">To delete a blob, first get a blob reference and then call the **delete_blob** method on it.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete the blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a><span data-ttu-id="1a8d8-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a8d8-165">Next steps</span></span>
<span data-ttu-id="1a8d8-166">Nu när du har lärt dig grunderna om blob storage kan du följa dessa länkar om du vill veta mer om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1a8d8-166">Now that you've learned the basics of blob storage, follow these links to learn more about Azure Storage.</span></span>  

* [<span data-ttu-id="1a8d8-167">Använda Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="1a8d8-167">How to use Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="1a8d8-168">Använda Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="1a8d8-168">How to use Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="1a8d8-169">Lista över Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="1a8d8-169">List Azure Storage Resources in C++</span></span>](storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="1a8d8-170">Storage-klientbibliotek för C++-referens</span><span class="sxs-lookup"><span data-stu-id="1a8d8-170">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="1a8d8-171">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="1a8d8-171">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="1a8d8-172">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="1a8d8-172">Transfer data with the AzCopy command-line utility</span></span>](storage-use-azcopy.md)

