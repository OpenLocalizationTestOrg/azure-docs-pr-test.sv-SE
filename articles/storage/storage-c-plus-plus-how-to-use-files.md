---
title: "Utveckla för Azure File storage med C++ | Microsoft Docs"
description: "Lär dig hur du utvecklar C++-program och tjänster som använder Azure File storage för att lagra fildata."
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
ms.openlocfilehash: fc0d8451442f1337db4a36718c3fc746f8eb5125
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="4879b-103">Utveckla för Azure File storage med C++</span><span class="sxs-lookup"><span data-stu-id="4879b-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="4879b-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="4879b-104">About this tutorial</span></span>

<span data-ttu-id="4879b-105">I den här kursen lär du dig hur du utför grundläggande åtgärder på Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="4879b-105">In this tutorial, you'll learn how to perform basic operations on Azure File storage.</span></span> <span data-ttu-id="4879b-106">Till exempel skrivna i C++, lär du dig hur du skapar resurser och kataloger, överföra, lista och ta bort filer.</span><span class="sxs-lookup"><span data-stu-id="4879b-106">Through samples written in C++, you'll learn how to create shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="4879b-107">Om du har använt Azure File storage, blir gå igenom begreppen i avsnitten som följer lättare att förstå exemplen.</span><span class="sxs-lookup"><span data-stu-id="4879b-107">If you are new to Azure File storage , going through the concepts in the sections that follow will be helpful in understanding the samples.</span></span>


* <span data-ttu-id="4879b-108">Skapa och ta bort Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="4879b-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="4879b-109">Skapa och ta bort kataloger</span><span class="sxs-lookup"><span data-stu-id="4879b-109">Create and delete directories</span></span>
* <span data-ttu-id="4879b-110">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="4879b-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="4879b-111">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="4879b-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="4879b-112">Ange kvoten (största tillåtna storleken) för en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="4879b-112">Set the quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="4879b-113">Skapa en signatur för delad åtkomst (SAS-nyckel) för en fil som använder en princip för delad åtkomst som definierats för resursen.</span><span class="sxs-lookup"><span data-stu-id="4879b-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on the share.</span></span>

> [!Note]  
> <span data-ttu-id="4879b-114">Eftersom Azure File storage kan nås över SMB, är det möjligt att skriva enkla program som har åtkomst till Azure filresursen med standard C++-i/o-klasser och funktioner.</span><span class="sxs-lookup"><span data-stu-id="4879b-114">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard C++ I/O classes and functions.</span></span> <span data-ttu-id="4879b-115">Den här artikeln beskriver hur du skriver program som använder Azure Storage C++ SDK, som använder den [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tala med Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="4879b-115">This article will describe how to write applications that use the Azure Storage C++ SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="4879b-116">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="4879b-116">Create a C++ application</span></span>
<span data-ttu-id="4879b-117">Om du vill skapa exemplen behöver du installera Azure Storage-klientbiblioteket 2.4.0 för C++.</span><span class="sxs-lookup"><span data-stu-id="4879b-117">To build the samples, you will need to install the Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="4879b-118">Du bör också har skapat ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4879b-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="4879b-119">Om du vill installera Azure Storage klienten 2.4.0 för C++, kan du använda någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="4879b-119">To install the Azure Storage Client 2.4.0 for C++, you can use one of the following methods:</span></span>

* <span data-ttu-id="4879b-120">**Linux:** Följ instruktionerna den [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="4879b-120">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="4879b-121">**Windows:** i Visual Studio klickar du på **verktyg &gt; NuGet Package Manager &gt; Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="4879b-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="4879b-122">Skriv följande kommando i den [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="4879b-122">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="4879b-123">Konfigurera din app att använda Azure File storage</span><span class="sxs-lookup"><span data-stu-id="4879b-123">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="4879b-124">Lägga till följande uttryck överst i C++ källfilen där du vill ändra Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="4879b-124">Add the following include statements to the top of the C++ source file where you want to manipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="4879b-125">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="4879b-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="4879b-126">Du måste ansluta till Azure storage-konto om du vill använda File storage.</span><span class="sxs-lookup"><span data-stu-id="4879b-126">To use File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="4879b-127">Det första steget är att konfigurera en anslutningssträng som vi använder för att ansluta till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4879b-127">The first step would be to configure a connection string, which we'll use to connect to your storage account.</span></span> <span data-ttu-id="4879b-128">Definiera en statisk variabel för att göra det.</span><span class="sxs-lookup"><span data-stu-id="4879b-128">Let's define a static variable to do that.</span></span>

```cpp
// Define the connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="4879b-129">Ansluta till ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="4879b-129">Connecting to an Azure storage account</span></span>
<span data-ttu-id="4879b-130">Du kan använda den **cloud_storage_account** klass för att representera kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="4879b-130">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="4879b-131">Du kan använda för att hämta information om ditt lagringskonto från anslutningssträngen för lagring av **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="4879b-131">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="4879b-132">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="4879b-132">Create an Azure File share</span></span>
<span data-ttu-id="4879b-133">Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**.</span><span class="sxs-lookup"><span data-stu-id="4879b-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="4879b-134">Storage-konto kan ha så många resurser som gör att din kapacitet.</span><span class="sxs-lookup"><span data-stu-id="4879b-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="4879b-135">Du måste använda en Azure File storage-klient för att få åtkomst till en resurs och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="4879b-135">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```cpp
// Create the Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="4879b-136">Med Azure File storage client kan hämta du sedan en referens till en resurs.</span><span class="sxs-lookup"><span data-stu-id="4879b-136">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="4879b-137">Använd för att skapa resursen i **create_if_not_exists** metod för den **cloud_file_share** objekt.</span><span class="sxs-lookup"><span data-stu-id="4879b-137">To create the share, use the **create_if_not_exists** method of the **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="4879b-138">Nu **dela** innehåller en referens till en resurs med namnet **Mina exempel resursen**.</span><span class="sxs-lookup"><span data-stu-id="4879b-138">At this point, **share** holds a reference to a share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="4879b-139">Ta bort en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="4879b-139">Delete an Azure File share</span></span>
<span data-ttu-id="4879b-140">Om du tar bort en resurs görs genom att anropa den **delete_if_exists** metod på ett cloud_file_share-objekt.</span><span class="sxs-lookup"><span data-stu-id="4879b-140">Deleting a share is done by calling the **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="4879b-141">Här är exempelkod som gör detta.</span><span class="sxs-lookup"><span data-stu-id="4879b-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="4879b-142">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="4879b-142">Create a directory</span></span>
<span data-ttu-id="4879b-143">Du kan sortera lagring genom att lägga till filer i underkataloger i stället för att alla i rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="4879b-143">You can organize storage by putting files inside subdirectories instead of having all of them in the root directory.</span></span> <span data-ttu-id="4879b-144">Azure File storage kan du skapa så många kataloger som ditt konto tillåter.</span><span class="sxs-lookup"><span data-stu-id="4879b-144">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="4879b-145">Koden nedan skapar en katalog med namnet **Mina exempel directory** under rotkatalogen samt en undermapp som heter **Mina exempel underkatalog**.</span><span class="sxs-lookup"><span data-stu-id="4879b-145">The code below will create a directory named **my-sample-directory** under the root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a><span data-ttu-id="4879b-146">Ta bort en katalog</span><span class="sxs-lookup"><span data-stu-id="4879b-146">Delete a directory</span></span>
<span data-ttu-id="4879b-147">Om du tar bort en katalog är en enkel aktivitet men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.</span><span class="sxs-lookup"><span data-stu-id="4879b-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="4879b-148">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="4879b-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="4879b-149">Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **list_files_and_directories** på en **cloud_file_directory** referens.</span><span class="sxs-lookup"><span data-stu-id="4879b-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="4879b-150">Att komma åt den omfattande uppsättningen med egenskaper och metoder för en returnerad **list_file_and_directory_item**, måste du anropa den **list_file_and_directory_item.as_file** metod för att hämta en **cloud_file**  objektet, eller **list_file_and_directory_item.as_directory** metod för att hämta en **cloud_file_directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4879b-150">To access the rich set of properties and methods for a returned **list_file_and_directory_item**, you must call the **list_file_and_directory_item.as_file** method to get a **cloud_file** object, or the **list_file_and_directory_item.as_directory** method to get a **cloud_file_directory** object.</span></span>

<span data-ttu-id="4879b-151">Följande kod visar hur du hämtar och returnerar URI: N för varje objekt i rotkatalogen för resursen.</span><span class="sxs-lookup"><span data-stu-id="4879b-151">The following code demonstrates how to retrieve and output the URI of each item in the root directory of the share.</span></span>

```cpp
//Get a reference to the root directory for the share.
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

## <a name="upload-a-file"></a><span data-ttu-id="4879b-152">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="4879b-152">Upload a file</span></span>
<span data-ttu-id="4879b-153">En filresurs på Azure innehåller en rotkatalog där filer kan finnas på minst.</span><span class="sxs-lookup"><span data-stu-id="4879b-153">At the very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="4879b-154">I det här avsnittet lär du dig hur du överför en fil från lokal lagring till rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="4879b-154">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="4879b-155">Det första steget i att överföra en fil är att hämta en referens till katalogen där det ska finnas.</span><span class="sxs-lookup"><span data-stu-id="4879b-155">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="4879b-156">Du kan göra detta genom att anropa den **get_root_directory_reference** metod för att dela objekt.</span><span class="sxs-lookup"><span data-stu-id="4879b-156">You do this by calling the **get_root_directory_reference** method of the share object.</span></span>

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="4879b-157">Nu när du har en referens till rotkatalogen för resursen kan överföra du en fil till den.</span><span class="sxs-lookup"><span data-stu-id="4879b-157">Now that you have a reference to the root directory of the share, you can upload a file onto it.</span></span> <span data-ttu-id="4879b-158">Det här exemplet Överför från en fil, text och en dataström.</span><span class="sxs-lookup"><span data-stu-id="4879b-158">This example uploads from a file, from text, and from a stream.</span></span>

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

## <a name="download-a-file"></a><span data-ttu-id="4879b-159">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="4879b-159">Download a file</span></span>
<span data-ttu-id="4879b-160">För att hämta filer, hämta en filreferens först och sedan anropa den **download_to_stream** metod för att överföra filen till en stream-objekt som du sedan kan spara till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="4879b-160">To download files, first retrieve a file reference and then call the **download_to_stream** method to transfer the file contents to a stream object, which you can then persist to a local file.</span></span> <span data-ttu-id="4879b-161">Du kan också använda den **download_to_file** metod för att hämta innehållet i en fil till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="4879b-161">Alternatively, you can use the **download_to_file** method to download the contents of a file to a local file.</span></span> <span data-ttu-id="4879b-162">Du kan använda den **download_text** metod för att hämta innehållet i en fil som en textsträng.</span><span class="sxs-lookup"><span data-stu-id="4879b-162">You can use the **download_text** method to download the contents of a file as a text string.</span></span>

<span data-ttu-id="4879b-163">I följande exempel används den **download_to_stream** och **download_text** metoder för att demonstrera hur du hämtar filerna som skapades i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4879b-163">The following example uses the **download_to_stream** and **download_text** methods to demonstrate downloading the files, which were created in previous sections.</span></span>

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

## <a name="delete-a-file"></a><span data-ttu-id="4879b-164">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="4879b-164">Delete a file</span></span>
<span data-ttu-id="4879b-165">En annan vanliga Azure File storage-åtgärd är filborttagning.</span><span class="sxs-lookup"><span data-stu-id="4879b-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="4879b-166">Följande kod tar bort en fil med namnet my-exemplet-filen-3 lagras under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="4879b-166">The following code deletes a file named my-sample-file-3 stored under the root directory.</span></span>

```cpp
// Get a reference to the root directory for the share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-the-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="4879b-167">Ange kvoten (största tillåtna storleken) för en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="4879b-167">Set the quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="4879b-168">Du kan ange kvoten (eller största storleken) för en filresurs, i gigabyte.</span><span class="sxs-lookup"><span data-stu-id="4879b-168">You can set the quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="4879b-169">Du kan också kontrollera hur mycket data som lagras på resursen för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4879b-169">You can also check to see how much data is currently stored on the share.</span></span>

<span data-ttu-id="4879b-170">Genom att ange kvoten för en resurs kan du begränsa den totala storleken på filerna som lagras på resursen.</span><span class="sxs-lookup"><span data-stu-id="4879b-170">By setting the quota for a share, you can limit the total size of the files stored on the share.</span></span> <span data-ttu-id="4879b-171">Om den totala storleken på filerna som lagras på resursen överskrider kvoten som angetts för resursen kan klienterna inte öka storleken på befintliga filer eller skapa nya filer såvida inte dessa filer är tomma.</span><span class="sxs-lookup"><span data-stu-id="4879b-171">If the total size of files on the share exceeds the quota set on the share, then clients will be unable to increase the size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="4879b-172">Exemplet nedan visar hur du kontrollerar användningen av en resurs och hur du ställer in kvoten för resursen.</span><span class="sxs-lookup"><span data-stu-id="4879b-172">The example below shows how to check the current usage for a share and how to set the quota for the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="4879b-173">Generera en signatur för delad åtkomst för en fil eller filresurs</span><span class="sxs-lookup"><span data-stu-id="4879b-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="4879b-174">Du kan generera en signatur för delad åtkomst (SAS) för en filresurs eller för en enskild fil.</span><span class="sxs-lookup"><span data-stu-id="4879b-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="4879b-175">Du kan också skapa en princip för delad åtkomst på en filresurs för att hantera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4879b-175">You can also create a shared access policy on a file share to manage shared access signatures.</span></span> <span data-ttu-id="4879b-176">Vi rekommenderar att du skapar en princip för delad åtkomst eftersom det ger dig möjlighet att återkalla signaturen för delad åtkomst om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4879b-176">Creating a shared access policy is recommended, as it provides a means of revoking the SAS if it should be compromised.</span></span>

<span data-ttu-id="4879b-177">I följande exempel skapar vi en princip för delad åtkomst på en resurs och använder sedan principen för att ange begränsningarna för en signatur för delad åtkomst på en fil i resursen.</span><span class="sxs-lookup"><span data-stu-id="4879b-177">The following example creates a shared access policy on a share, and then uses that policy to provide the constraints for a SAS on a file in the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
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

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;    

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a><span data-ttu-id="4879b-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4879b-178">Next steps</span></span>
<span data-ttu-id="4879b-179">Utforska gärna dessa resurser om du vill veta mer om Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="4879b-179">To learn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="4879b-180">Storage-klientbibliotek för C++</span><span class="sxs-lookup"><span data-stu-id="4879b-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="4879b-181">[Azure Storage Service filexempel i C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="4879b-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="4879b-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="4879b-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="4879b-183">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="4879b-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)