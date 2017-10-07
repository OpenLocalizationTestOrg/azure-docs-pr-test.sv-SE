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
ms.openlocfilehash: 0d7e7436a109ef54fc07cc238c03cfc7cf2caac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-c"></a>Hur toouse Blob Storage från C++
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Blob storage-tjänst. hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar.  

> [!NOTE]
> Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare. hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Skapa ett C++-program
I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.  

toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.   

tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:

* **Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.  
* **Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**. Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-blob-storage"></a>Konfigurera ditt program tooaccess Blob Storage
Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess BLOB:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. När den körs i ett klientprogram, du måste ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden. Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](storage-create-storage-account.md). Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest ditt program i din lokala Windows-dator kan du använda hello Microsoft Azure [lagringsemulatorn](storage-use-emulator.md) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/). hello storage-emulatorn är ett verktyg som simulerar hello Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala. hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello Azure storage-emulatorn väljer hello **starta** eller tryck på knappen hello **Windows** nyckel. Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** hello listan med program.  

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.  

## <a name="retrieve-your-connection-string"></a>Hämta anslutningssträngen
Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring. tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Sedan hämta en referens tooa **cloud_blob_client** klassen eftersom du tooretrieve objekt som representerar behållare och blobbar som lagras i hello Blob Storage-tjänst. hello följande kod skapar en **cloud_blob_client** objekt med hello konto lagringsobjekt vi hämta ovan:  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Så här: skapa en behållare
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Det här exemplet illustrerar hur toocreate en behållare om den inte redan finns:  

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

Som standard hello ny behållare är privat och du måste ange lagring få åtkomst till viktiga toodownload blobbar från den här behållaren. Om du vill toomake hello-filer (BLOB) inom hello behållaren tillgängliga tooeveryone, kan du ange hello behållaren toobe offentliga med hello följande kod:  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Vem som helst på hello Internet kan se blobbar i en offentlig behållare, men du kan ändra eller ta bort dem bara om du har rätt åtkomstnyckel för hello.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Så här: ladda upp en blobb till en behållare
Azure Blob Storage stöder blockblobbar och sidblobbar. I hello flertalet fall är blockblob hello rekommenderade typen toouse.  

tooupload en fil tooa blockblob, hämta en referens för behållaren och använder det tooget en blockblobben. När du har en blobbreferens kan du ladda upp en dataström data tooit genom att anropa hello **upload_from_stream** metod. Den här åtgärden skapar hello blob om den tidigare inte finns eller skriva över den om den finns. följande exempel visar hur hello tooupload en blobb till en behållare och förutsätter hello behållaren har redan skapats.  

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

Du kan också använda hello **upload_from_file** metoden tooupload en fil tooa block-blob.

## <a name="how-to-list-hello-blobs-in-a-container"></a>Så här: Visa hello blobbar i en behållare
toolist hello blobbar i en behållare först hämta en referens för behållaren. Du kan sedan använda hello behållaren **list_blobs** metoden tooretrieve hello blobbarna och/eller katalogerna i den. tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **list_blob_item**, måste du anropa hello **list_blob_item.as_blob** metoden tooget en **cloud_blob** objekt eller hello **list_blob.as_directory** metoden tooget ett cloud_blob_directory-objekt. hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i hello **Mina exempel behållaren** behållare:

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

Mer information om hur du visar operations finns [lista Azure Storage-resurser i C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Så här: ladda ned blobbar
toodownload blobbar först hämta en blobbreferens och sedan anropa hello **download_to_stream** metod. hello följande exempel används hello **download_to_stream** metoden tootransfer hello blob innehållet tooa stream-objektet som du sedan kan spara tooa lokal fil.  

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

Du kan också använda hello **download_to_file** metoden toodownload hello innehållet i en blob tooa-fil.
Dessutom kan du också använda hello **download_text** metoden toodownload hello innehållet i en blob som en textsträng.  

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

## <a name="how-to-delete-blobs"></a>Så här: ta bort blobbar
toodelete blob först hämta en blobbreferens och sedan anropa hello **delete_blob** metod på den.  

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

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna om blob storage, följa dessa länkar toolearn mer om Azure Storage.  

* [Hur toouse Queue Storage från C++](storage-c-plus-plus-how-to-use-queues.md)
* [Hur toouse Table Storage från C++](storage-c-plus-plus-how-to-use-tables.md)
* [Lista över Azure Storage-resurser i C++](storage-c-plus-plus-enumeration.md)
* [Storage-klientbibliotek för C++-referens](http://azure.github.io/azure-storage-cpp)
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [Överföra data med hello kommandoradsverktyget azcopy](storage-use-azcopy.md)

