---
title: "aaaDevelop för Azure File storage med C++ | Microsoft Docs"
description: "Lär dig hur toodevelop C++-program och tjänster som använder Azure File storage toostore fildata."
services: storage
documentationcenter: .net
author: renashahmsft
manager: aungoo
editor: tysonn
ms.assetid: a1e8c99e-47a6-43a9-9541-c9262eb00b38
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: renashahmsft
ms.openlocfilehash: 48f668cf9359f88baeaaa08ee0d44e70bd0f5f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a>Utveckla för Azure File storage med C++
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen

I den här kursen lär du dig hur tooperform grundläggande åtgärder på Azure File storage. Till exempel skrivna i C++, får du lära dig hur toocreate delar och kataloger, överföra, visa och ta bort filer. Om du är ny tooAzure File storage kan blir gå igenom hello begrepp i hello avsnitten som följer lättare att förstå hello prover.


* Skapa och ta bort Azure-filresurser
* Skapa och ta bort kataloger
* Räkna upp filer och kataloger i en filresurs på Azure
* Ladda upp, hämta och ta bort en fil
* Ange hello kvot (största tillåtna storleken) för en Azure-filresurs
* Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats på hello-resurs.

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard C++-i/o-klasser och -funktioner. Den här artikeln beskriver hur toowrite program som använder hello Azure Storage C++ SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.

## <a name="create-a-c-application"></a>Skapa ett C++-program
toobuild hello prover måste tooinstall hello Azure Storage-klientbibliotek 2.4.0 för C++. Du bör också har skapat ett Azure storage-konto.

tooinstall hello Azure Storage Client 2.4.0 för C++ kan du använda någon av följande metoder hello:

* **Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.
* **Windows:** i Visual Studio klickar du på **verktyg &gt; NuGet Package Manager &gt; Pakethanterarkonsolen**. Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a>Konfigurera ditt program toouse Azure File storage
Lägga till följande hello innehålla instruktioner toohello överkant hello C++ källfilen där du vill att toomanipulate Azure File storage:

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
toouse fillagring måste tooconnect tooyour Azure storage-konto. hello första steget är tooconfigure en anslutningssträng som vi använder tooconnect tooyour storage-konto. Definiera en statisk variabel toodo som.

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a>Ansluta tooan Azure storage-konto
Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring. tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a>Skapa en filresurs på Azure
Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**. Storage-konto kan ha så många resurser som gör att din kapacitet. tooobtain åtkomst tooa resursen och dess innehåll måste toouse en Azure File storage-klient.

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

Med hello Azure File storage client kan hämta du sedan en referens tooa resurs.

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

toocreate hello resurs, Använd hello **create_if_not_exists** metod för hello **cloud_file_share** objekt.

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

Nu **dela** innehåller en referens tooa resurs med namnet **Mina exempel resursen**.

## <a name="delete-an-azure-file-share"></a>Ta bort en Azure-filresurs
Om du tar bort en resurs görs genom att anropa hello **delete_if_exists** metod på ett cloud_file_share-objekt. Här är exempelkod som gör detta.

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a>Skapa en katalog
Du kan sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog. Azure File storage kan toocreate som många kataloger som ditt konto kommer tillåta. hello koden nedan skapar en katalog med namnet **Mina exempel directory** under rotkatalogen hello samt en undermapp som heter **Mina exempel underkatalog**.

```cpp
// Retrieve a reference tooa directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if hello share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a>Ta bort en katalog
Om du tar bort en katalog är en enkel aktivitet men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference toohello directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference toohello subdirectory you want toodelete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete hello subdirectory and hello sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Räkna upp filer och kataloger i en filresurs på Azure
Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **list_files_and_directories** på en **cloud_file_directory** referens. tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **list_file_and_directory_item**, måste du anropa hello **list_file_and_directory_item.as_file** metoden tooget en **cloud_ filen** objekt eller hello **list_file_and_directory_item.as_directory** metoden tooget en **cloud_file_directory** objekt.

hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i rotkatalogen för hello hello-resursen.

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_diretory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }        
}
```

## <a name="upload-a-file"></a>Överför en fil
Vid hello mycket minst, innehåller en filresurs i Azure en rotkatalog där filer kan finnas. I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.

hello första steget i att överföra en fil är tooobtain en referens toohello katalog där det ska finnas. Det gör du genom att anropa hello **get_root_directory_reference** metod för hello dela objekt.

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Nu när du har en referens toohello rotkatalog hello-resursen kan överföra du en fil till den. Det här exemplet Överför från en fil, text och en dataström.

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream = 
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));    
```

## <a name="download-a-file"></a>Hämta en fil
toodownload filer, hämta en filreferens först och sedan anropa hello **download_to_stream** metoden tootransfer hello filen innehållet tooa stream-objekt som du sedan kan spara tooa lokal fil. Du kan också använda hello **download_to_file** metoden toodownload hello innehållet i filen tooa lokala filer. Du kan använda hello **download_text** metoden toodownload hello innehållet i en fil som en textsträng.

hello följande exempel används hello **download_to_stream** och **download_text** metoder toodemonstrate hämtar hello-filer som skapades i föregående avsnitt.

```cpp
// Download as text
azure::storage::cloud_file text_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="delete-a-file"></a>Ta bort en fil
En annan vanliga Azure File storage-åtgärd är filborttagning. hello följande kod tar bort en fil med namnet min-exemplet-filen-3 lagras under hello rotkatalog.

```cpp
// Get a reference toohello root directory for hello share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a>Ange hello kvot (största tillåtna storleken) för en Azure-filresurs
Du kan ange hello kvoten (eller största storleken) för en filresurs, i gigabyte. Du kan också kontrollera hur mycket data som lagras på resursen hello toosee.

Genom att ange hello kvoten för en resurs, kan du begränsa hello totala storleken för hello-filer som lagras på hello-resurs. Om hello totala storleken på filerna på hello resursen överskrider ange hello kvot för hello resursen och klienter ska tooincrease hello storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.

hello exemplet nedan visar hur toocheck hello aktuella användningen av en resurs och hur tooset hello kvoten för hello resursen.

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets hello quota too2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generera en signatur för delad åtkomst för en fil eller filresurs
Du kan generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil. Du kan också skapa en princip för delad åtkomst på en toomanage delade filresurser signaturer. Skapa en princip för delad åtkomst rekommenderas eftersom det ger dig möjlighet att återkalla hello SAS om det behövs.

hello följande exempel skapar en princip för delad åtkomst på en resurs och använder sedan som delar tooprovide hello principbegränsningar för en SAS för en fil i hello.

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client and get a reference toohello share
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy = 
      azure::storage::file_shared_access_policy();

    //set permissions tooexpire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for hello share
    azure::storage::file_share_permissions permissions;    

    //retrieve hello current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add hello new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save hello updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve hello root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in hello share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Storage, utforska gärna dessa resurser:

* [Storage-klientbibliotek för C++](https://github.com/Azure/azure-storage-cpp)
* [Azure Storage Service filexempel i C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)
* [Azure Storage Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)