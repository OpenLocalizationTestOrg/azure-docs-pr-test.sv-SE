---
title: "aaaHow toouse blob storage (objektlagring) från C++ | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
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
ms.openlocfilehash: e63df4683e5c10c9f8fbe4106c655df61be0e1a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-c"></a><span data-ttu-id="3139d-103">Hur toouse Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="3139d-103">How toouse Blob Storage from C++</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="3139d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3139d-104">Overview</span></span>
<span data-ttu-id="3139d-105">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="3139d-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="3139d-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="3139d-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="3139d-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="3139d-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="3139d-108">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Blob storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3139d-108">This guide will demonstrate how tooperform common scenarios using hello Azure Blob storage service.</span></span> <span data-ttu-id="3139d-109">hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="3139d-109">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="3139d-110">hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="3139d-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span>  

> [!NOTE]
> <span data-ttu-id="3139d-111">Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="3139d-111">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="3139d-112">hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="3139d-112">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="3139d-113">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="3139d-113">Create a C++ application</span></span>
<span data-ttu-id="3139d-114">I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="3139d-114">In this guide, you will use storage features which can be run within a C++ application.</span></span>  

<span data-ttu-id="3139d-115">toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3139d-115">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>   

<span data-ttu-id="3139d-116">tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="3139d-116">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="3139d-117">**Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="3139d-117">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="3139d-118">**Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="3139d-118">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="3139d-119">Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3139d-119">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>  
  
     <span data-ttu-id="3139d-120">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="3139d-120">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="3139d-121">Konfigurera ditt program tooaccess Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3139d-121">Configure your application tooaccess Blob Storage</span></span>
<span data-ttu-id="3139d-122">Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess BLOB:</span><span class="sxs-lookup"><span data-stu-id="3139d-122">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess blobs:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="3139d-123">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="3139d-123">Setup an Azure storage connection string</span></span>
<span data-ttu-id="3139d-124">Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="3139d-124">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="3139d-125">När den körs i ett klientprogram, du måste ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="3139d-125">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="3139d-126">Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3139d-126">For information on storage accounts and access keys, see [About Azure Storage Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span> <span data-ttu-id="3139d-127">Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:</span><span class="sxs-lookup"><span data-stu-id="3139d-127">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="3139d-128">tootest ditt program i din lokala Windows-dator kan du använda hello Microsoft Azure [lagringsemulatorn](../storage-use-emulator.md) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3139d-128">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](../storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="3139d-129">hello storage-emulatorn är ett verktyg som simulerar hello Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="3139d-129">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="3139d-130">hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="3139d-130">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="3139d-131">toostart hello Azure storage-emulatorn väljer hello **starta** eller tryck på knappen hello **Windows** nyckel.</span><span class="sxs-lookup"><span data-stu-id="3139d-131">toostart hello Azure storage emulator, Select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="3139d-132">Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** hello listan med program.</span><span class="sxs-lookup"><span data-stu-id="3139d-132">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="3139d-133">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="3139d-133">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="3139d-134">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="3139d-134">Retrieve your connection string</span></span>
<span data-ttu-id="3139d-135">Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="3139d-135">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="3139d-136">tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="3139d-136">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="3139d-137">Sedan hämta en referens tooa **cloud_blob_client** klassen eftersom du tooretrieve objekt som representerar behållare och blobbar som lagras i hello Blob Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3139d-137">Next, get a reference tooa **cloud_blob_client** class as it allows you tooretrieve objects that represent containers and blobs stored within hello Blob Storage Service.</span></span> <span data-ttu-id="3139d-138">hello följande kod skapar en **cloud_blob_client** objekt med hello konto lagringsobjekt vi hämta ovan:</span><span class="sxs-lookup"><span data-stu-id="3139d-138">hello following code creates a **cloud_blob_client** object using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a><span data-ttu-id="3139d-139">Så här: skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="3139d-139">How to: Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="3139d-140">Det här exemplet illustrerar hur toocreate en behållare om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="3139d-140">This example shows how toocreate a container if it does not already exist:</span></span>  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create hello blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference tooa container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create hello container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

<span data-ttu-id="3139d-141">Som standard hello ny behållare är privat och du måste ange lagring få åtkomst till viktiga toodownload blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="3139d-141">By default, hello new container is private and you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="3139d-142">Om du vill toomake hello-filer (BLOB) inom hello behållaren tillgängliga tooeveryone, kan du ange hello behållaren toobe offentliga med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="3139d-142">If you want toomake hello files (blobs) within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

<span data-ttu-id="3139d-143">Vem som helst på hello Internet kan se blobbar i en offentlig behållare, men du kan ändra eller ta bort dem bara om du har rätt åtkomstnyckel för hello.</span><span class="sxs-lookup"><span data-stu-id="3139d-143">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>  

## <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="3139d-144">Så här: ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="3139d-144">How to: Upload a blob into a container</span></span>
<span data-ttu-id="3139d-145">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="3139d-145">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="3139d-146">I hello flertalet fall är blockblob hello rekommenderade typen toouse.</span><span class="sxs-lookup"><span data-stu-id="3139d-146">In hello majority of cases, block blob is hello recommended type toouse.</span></span>  

<span data-ttu-id="3139d-147">tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben.</span><span class="sxs-lookup"><span data-stu-id="3139d-147">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="3139d-148">När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **upload_from_stream** metod.</span><span class="sxs-lookup"><span data-stu-id="3139d-148">Once you have a blob reference, you can upload any stream of data tooit by calling hello **upload_from_stream** method.</span></span> <span data-ttu-id="3139d-149">Den här åtgärden skapar hello blob om den tidigare inte finns eller skriva över den om den finns.</span><span class="sxs-lookup"><span data-stu-id="3139d-149">This operation will create hello blob if it didn't previously exist, or overwrite it if it does exist.</span></span> <span data-ttu-id="3139d-150">följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="3139d-150">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite hello "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite hello "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference tooa blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

<span data-ttu-id="3139d-151">Du kan också använda hello **upload_from_file** metoden tooupload en fil tooa block-blob.</span><span class="sxs-lookup"><span data-stu-id="3139d-151">Alternatively, you can use hello **upload_from_file** method tooupload a file tooa block blob.</span></span>

## <a name="how-to-list-hello-blobs-in-a-container"></a><span data-ttu-id="3139d-152">Så här: Visa hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="3139d-152">How to: List hello blobs in a container</span></span>
<span data-ttu-id="3139d-153">toolist hello blobbar i en behållare först hämta en referens för behållaren.</span><span class="sxs-lookup"><span data-stu-id="3139d-153">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="3139d-154">Du kan sedan använda hello behållaren **list_blobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den.</span><span class="sxs-lookup"><span data-stu-id="3139d-154">You can then use hello container's **list_blobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="3139d-155">tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **list_blob_item**, måste du anropa hello **list_blob_item.as_blob** metoden tooget en **cloud_blob** objekt eller hello **list_blob.as_directory** metoden tooget ett cloud_blob_directory-objekt.</span><span class="sxs-lookup"><span data-stu-id="3139d-155">tooaccess hello rich set of properties and methods for a returned **list_blob_item**, you must call hello **list_blob_item.as_blob** method tooget a  **cloud_blob** object, or hello **list_blob.as_directory** method tooget a  cloud_blob_directory object.</span></span> <span data-ttu-id="3139d-156">hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello **Mina exempel behållaren** behållare:</span><span class="sxs-lookup"><span data-stu-id="3139d-156">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **my-sample-container** container:</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
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

<span data-ttu-id="3139d-157">Mer information om hur du visar operations finns [lista Azure Storage-resurser i C++](../storage-c-plus-plus-enumeration.md).</span><span class="sxs-lookup"><span data-stu-id="3139d-157">For more details on listing operations, see [List Azure Storage Resources in C++](../storage-c-plus-plus-enumeration.md).</span></span>

## <a name="how-to-download-blobs"></a><span data-ttu-id="3139d-158">Så här: ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="3139d-158">How to: Download blobs</span></span>
<span data-ttu-id="3139d-159">toodownload blobbar först hämta en blobbreferens och sedan anropa hello **download_to_stream** metod.</span><span class="sxs-lookup"><span data-stu-id="3139d-159">toodownload blobs, first retrieve a blob reference and then call hello **download_to_stream** method.</span></span> <span data-ttu-id="3139d-160">hello följande exempel används hello **download_to_stream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="3139d-160">hello following example uses hello **download_to_stream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents tooa file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

<span data-ttu-id="3139d-161">Du kan också använda hello **download_to_file** metoden toodownload hello innehållet i en blob tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="3139d-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a blob tooa file.</span></span>
<span data-ttu-id="3139d-162">Dessutom kan du också använda hello **download_text** metoden toodownload hello innehållet i en blob som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="3139d-162">In addition, you can also use hello **download_text** method toodownload hello contents of a blob as a text string.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download hello contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a><span data-ttu-id="3139d-163">Så här: ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="3139d-163">How to: Delete blobs</span></span>
<span data-ttu-id="3139d-164">toodelete blob först hämta en blobbreferens och sedan anropa hello **delete_blob** metod på den.</span><span class="sxs-lookup"><span data-stu-id="3139d-164">toodelete a blob, first get a blob reference and then call hello **delete_blob** method on it.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete hello blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a><span data-ttu-id="3139d-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3139d-165">Next steps</span></span>
<span data-ttu-id="3139d-166">Nu när du har lärt dig hello grunderna om blob storage, följa dessa länkar toolearn mer om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3139d-166">Now that you've learned hello basics of blob storage, follow these links toolearn more about Azure Storage.</span></span>  

* [<span data-ttu-id="3139d-167">Hur toouse Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="3139d-167">How toouse Queue Storage from C++</span></span>](../storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="3139d-168">Hur toouse Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="3139d-168">How toouse Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="3139d-169">Lista över Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="3139d-169">List Azure Storage Resources in C++</span></span>](../storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="3139d-170">Storage-klientbibliotek för C++-referens</span><span class="sxs-lookup"><span data-stu-id="3139d-170">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="3139d-171">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="3139d-171">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="3139d-172">Överföra data med hello kommandoradsverktyget azcopy</span><span class="sxs-lookup"><span data-stu-id="3139d-172">Transfer data with hello AzCopy command-line utility</span></span>](../storage-use-azcopy.md)

