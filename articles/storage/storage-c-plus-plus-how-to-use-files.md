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
ms.openlocfilehash: 40c3aac94214a91121913e2ded315031aeed1c30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="e5c10-103">Utveckla för Azure File storage med C++</span><span class="sxs-lookup"><span data-stu-id="e5c10-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="e5c10-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="e5c10-104">About this tutorial</span></span>

<span data-ttu-id="e5c10-105">I den här kursen lär du dig hur tooperform grundläggande åtgärder på Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="e5c10-105">In this tutorial, you'll learn how tooperform basic operations on Azure File storage.</span></span> <span data-ttu-id="e5c10-106">Till exempel skrivna i C++, får du lära dig hur toocreate delar och kataloger, överföra, visa och ta bort filer.</span><span class="sxs-lookup"><span data-stu-id="e5c10-106">Through samples written in C++, you'll learn how toocreate shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="e5c10-107">Om du är ny tooAzure File storage kan blir gå igenom hello begrepp i hello avsnitten som följer lättare att förstå hello prover.</span><span class="sxs-lookup"><span data-stu-id="e5c10-107">If you are new tooAzure File storage , going through hello concepts in hello sections that follow will be helpful in understanding hello samples.</span></span>


* <span data-ttu-id="e5c10-108">Skapa och ta bort Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="e5c10-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="e5c10-109">Skapa och ta bort kataloger</span><span class="sxs-lookup"><span data-stu-id="e5c10-109">Create and delete directories</span></span>
* <span data-ttu-id="e5c10-110">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="e5c10-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="e5c10-111">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="e5c10-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="e5c10-112">Ange hello kvot (största tillåtna storleken) för en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="e5c10-112">Set hello quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="e5c10-113">Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats på hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="e5c10-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>

> [!Note]  
> <span data-ttu-id="e5c10-114">Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard C++-i/o-klasser och -funktioner.</span><span class="sxs-lookup"><span data-stu-id="e5c10-114">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard C++ I/O classes and functions.</span></span> <span data-ttu-id="e5c10-115">Den här artikeln beskriver hur toowrite program som använder hello Azure Storage C++ SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span><span class="sxs-lookup"><span data-stu-id="e5c10-115">This article will describe how toowrite applications that use hello Azure Storage C++ SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="e5c10-116">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="e5c10-116">Create a C++ application</span></span>
<span data-ttu-id="e5c10-117">toobuild hello prover måste tooinstall hello Azure Storage-klientbibliotek 2.4.0 för C++.</span><span class="sxs-lookup"><span data-stu-id="e5c10-117">toobuild hello samples, you will need tooinstall hello Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="e5c10-118">Du bör också har skapat ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e5c10-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="e5c10-119">tooinstall hello Azure Storage Client 2.4.0 för C++ kan du använda någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="e5c10-119">tooinstall hello Azure Storage Client 2.4.0 for C++, you can use one of hello following methods:</span></span>

* <span data-ttu-id="e5c10-120">**Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="e5c10-120">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="e5c10-121">**Windows:** i Visual Studio klickar du på **verktyg &gt; NuGet Package Manager &gt; Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="e5c10-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="e5c10-122">Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="e5c10-122">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="e5c10-123">Konfigurera ditt program toouse Azure File storage</span><span class="sxs-lookup"><span data-stu-id="e5c10-123">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="e5c10-124">Lägga till följande hello innehålla instruktioner toohello överkant hello C++ källfilen där du vill att toomanipulate Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="e5c10-124">Add hello following include statements toohello top of hello C++ source file where you want toomanipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="e5c10-125">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="e5c10-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="e5c10-126">toouse fillagring måste tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e5c10-126">toouse File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="e5c10-127">hello första steget är tooconfigure en anslutningssträng som vi använder tooconnect tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e5c10-127">hello first step would be tooconfigure a connection string, which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="e5c10-128">Definiera en statisk variabel toodo som.</span><span class="sxs-lookup"><span data-stu-id="e5c10-128">Let's define a static variable toodo that.</span></span>

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="e5c10-129">Ansluta tooan Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="e5c10-129">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="e5c10-130">Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="e5c10-130">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="e5c10-131">tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="e5c10-131">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="e5c10-132">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="e5c10-132">Create an Azure File share</span></span>
<span data-ttu-id="e5c10-133">Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**.</span><span class="sxs-lookup"><span data-stu-id="e5c10-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="e5c10-134">Storage-konto kan ha så många resurser som gör att din kapacitet.</span><span class="sxs-lookup"><span data-stu-id="e5c10-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="e5c10-135">tooobtain åtkomst tooa resursen och dess innehåll måste toouse en Azure File storage-klient.</span><span class="sxs-lookup"><span data-stu-id="e5c10-135">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="e5c10-136">Med hello Azure File storage client kan hämta du sedan en referens tooa resurs.</span><span class="sxs-lookup"><span data-stu-id="e5c10-136">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="e5c10-137">toocreate hello resurs, Använd hello **create_if_not_exists** metod för hello **cloud_file_share** objekt.</span><span class="sxs-lookup"><span data-stu-id="e5c10-137">toocreate hello share, use hello **create_if_not_exists** method of hello **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="e5c10-138">Nu **dela** innehåller en referens tooa resurs med namnet **Mina exempel resursen**.</span><span class="sxs-lookup"><span data-stu-id="e5c10-138">At this point, **share** holds a reference tooa share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="e5c10-139">Ta bort en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="e5c10-139">Delete an Azure File share</span></span>
<span data-ttu-id="e5c10-140">Om du tar bort en resurs görs genom att anropa hello **delete_if_exists** metod på ett cloud_file_share-objekt.</span><span class="sxs-lookup"><span data-stu-id="e5c10-140">Deleting a share is done by calling hello **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="e5c10-141">Här är exempelkod som gör detta.</span><span class="sxs-lookup"><span data-stu-id="e5c10-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="e5c10-142">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="e5c10-142">Create a directory</span></span>
<span data-ttu-id="e5c10-143">Du kan sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="e5c10-143">You can organize storage by putting files inside subdirectories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="e5c10-144">Azure File storage kan toocreate som många kataloger som ditt konto kommer tillåta.</span><span class="sxs-lookup"><span data-stu-id="e5c10-144">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="e5c10-145">hello koden nedan skapar en katalog med namnet **Mina exempel directory** under rotkatalogen hello samt en undermapp som heter **Mina exempel underkatalog**.</span><span class="sxs-lookup"><span data-stu-id="e5c10-145">hello code below will create a directory named **my-sample-directory** under hello root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

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

## <a name="delete-a-directory"></a><span data-ttu-id="e5c10-146">Ta bort en katalog</span><span class="sxs-lookup"><span data-stu-id="e5c10-146">Delete a directory</span></span>
<span data-ttu-id="e5c10-147">Om du tar bort en katalog är en enkel aktivitet men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.</span><span class="sxs-lookup"><span data-stu-id="e5c10-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="e5c10-148">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="e5c10-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="e5c10-149">Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **list_files_and_directories** på en **cloud_file_directory** referens.</span><span class="sxs-lookup"><span data-stu-id="e5c10-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="e5c10-150">tooaccess hello omfattande uppsättning egenskaper och metoder för en returnerad **list_file_and_directory_item**, måste du anropa hello **list_file_and_directory_item.as_file** metoden tooget en **cloud_ filen** objekt eller hello **list_file_and_directory_item.as_directory** metoden tooget en **cloud_file_directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="e5c10-150">tooaccess hello rich set of properties and methods for a returned **list_file_and_directory_item**, you must call hello **list_file_and_directory_item.as_file** method tooget a **cloud_file** object, or hello **list_file_and_directory_item.as_directory** method tooget a **cloud_file_directory** object.</span></span>

<span data-ttu-id="e5c10-151">hello följande kod visar hur tooretrieve och utdata hello URI: N för varje objekt i rotkatalogen för hello hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="e5c10-151">hello following code demonstrates how tooretrieve and output hello URI of each item in hello root directory of hello share.</span></span>

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

## <a name="upload-a-file"></a><span data-ttu-id="e5c10-152">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="e5c10-152">Upload a file</span></span>
<span data-ttu-id="e5c10-153">Vid hello mycket minst, innehåller en filresurs i Azure en rotkatalog där filer kan finnas.</span><span class="sxs-lookup"><span data-stu-id="e5c10-153">At hello very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="e5c10-154">I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="e5c10-154">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="e5c10-155">hello första steget i att överföra en fil är tooobtain en referens toohello katalog där det ska finnas.</span><span class="sxs-lookup"><span data-stu-id="e5c10-155">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="e5c10-156">Det gör du genom att anropa hello **get_root_directory_reference** metod för hello dela objekt.</span><span class="sxs-lookup"><span data-stu-id="e5c10-156">You do this by calling hello **get_root_directory_reference** method of hello share object.</span></span>

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="e5c10-157">Nu när du har en referens toohello rotkatalog hello-resursen kan överföra du en fil till den.</span><span class="sxs-lookup"><span data-stu-id="e5c10-157">Now that you have a reference toohello root directory of hello share, you can upload a file onto it.</span></span> <span data-ttu-id="e5c10-158">Det här exemplet Överför från en fil, text och en dataström.</span><span class="sxs-lookup"><span data-stu-id="e5c10-158">This example uploads from a file, from text, and from a stream.</span></span>

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

## <a name="download-a-file"></a><span data-ttu-id="e5c10-159">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="e5c10-159">Download a file</span></span>
<span data-ttu-id="e5c10-160">toodownload filer, hämta en filreferens först och sedan anropa hello **download_to_stream** metoden tootransfer hello filen innehållet tooa stream-objekt som du sedan kan spara tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="e5c10-160">toodownload files, first retrieve a file reference and then call hello **download_to_stream** method tootransfer hello file contents tooa stream object, which you can then persist tooa local file.</span></span> <span data-ttu-id="e5c10-161">Du kan också använda hello **download_to_file** metoden toodownload hello innehållet i filen tooa lokala filer.</span><span class="sxs-lookup"><span data-stu-id="e5c10-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a file tooa local file.</span></span> <span data-ttu-id="e5c10-162">Du kan använda hello **download_text** metoden toodownload hello innehållet i en fil som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="e5c10-162">You can use hello **download_text** method toodownload hello contents of a file as a text string.</span></span>

<span data-ttu-id="e5c10-163">hello följande exempel används hello **download_to_stream** och **download_text** metoder toodemonstrate hämtar hello-filer som skapades i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e5c10-163">hello following example uses hello **download_to_stream** and **download_text** methods toodemonstrate downloading hello files, which were created in previous sections.</span></span>

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

## <a name="delete-a-file"></a><span data-ttu-id="e5c10-164">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="e5c10-164">Delete a file</span></span>
<span data-ttu-id="e5c10-165">En annan vanliga Azure File storage-åtgärd är filborttagning.</span><span class="sxs-lookup"><span data-stu-id="e5c10-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="e5c10-166">hello följande kod tar bort en fil med namnet min-exemplet-filen-3 lagras under hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="e5c10-166">hello following code deletes a file named my-sample-file-3 stored under hello root directory.</span></span>

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

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="e5c10-167">Ange hello kvot (största tillåtna storleken) för en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="e5c10-167">Set hello quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="e5c10-168">Du kan ange hello kvoten (eller största storleken) för en filresurs, i gigabyte.</span><span class="sxs-lookup"><span data-stu-id="e5c10-168">You can set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="e5c10-169">Du kan också kontrollera hur mycket data som lagras på resursen hello toosee.</span><span class="sxs-lookup"><span data-stu-id="e5c10-169">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="e5c10-170">Genom att ange hello kvoten för en resurs, kan du begränsa hello totala storleken för hello-filer som lagras på hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="e5c10-170">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="e5c10-171">Om hello totala storleken på filerna på hello resursen överskrider ange hello kvot för hello resursen och klienter ska tooincrease hello storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.</span><span class="sxs-lookup"><span data-stu-id="e5c10-171">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="e5c10-172">hello exemplet nedan visar hur toocheck hello aktuella användningen av en resurs och hur tooset hello kvoten för hello resursen.</span><span class="sxs-lookup"><span data-stu-id="e5c10-172">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

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

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="e5c10-173">Generera en signatur för delad åtkomst för en fil eller filresurs</span><span class="sxs-lookup"><span data-stu-id="e5c10-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="e5c10-174">Du kan generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil.</span><span class="sxs-lookup"><span data-stu-id="e5c10-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="e5c10-175">Du kan också skapa en princip för delad åtkomst på en toomanage delade filresurser signaturer.</span><span class="sxs-lookup"><span data-stu-id="e5c10-175">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="e5c10-176">Skapa en princip för delad åtkomst rekommenderas eftersom det ger dig möjlighet att återkalla hello SAS om det behövs.</span><span class="sxs-lookup"><span data-stu-id="e5c10-176">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="e5c10-177">hello följande exempel skapar en princip för delad åtkomst på en resurs och använder sedan som delar tooprovide hello principbegränsningar för en SAS för en fil i hello.</span><span class="sxs-lookup"><span data-stu-id="e5c10-177">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="e5c10-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5c10-178">Next steps</span></span>
<span data-ttu-id="e5c10-179">toolearn mer om Azure Storage, utforska gärna dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="e5c10-179">toolearn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="e5c10-180">Storage-klientbibliotek för C++</span><span class="sxs-lookup"><span data-stu-id="e5c10-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="e5c10-181">[Azure Storage Service filexempel i C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="e5c10-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="e5c10-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="e5c10-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="e5c10-183">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="e5c10-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)