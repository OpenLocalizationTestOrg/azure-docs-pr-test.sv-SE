---
title: "aaaDevelop för Azure File storage med Java | Microsoft Docs"
description: "Lär dig hur toodevelop Java-program och tjänster som använder Azure File storage toostore fildata."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 05/27/2017
ms.author: robinsh
ms.openlocfilehash: b50703815daf2c829e7e9a9a4196c31a2b8727e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="7b151-103">Utveckla för Azure File storage med Java</span><span class="sxs-lookup"><span data-stu-id="7b151-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="7b151-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="7b151-104">About this tutorial</span></span>
<span data-ttu-id="7b151-105">Den här kursen visar hello grunderna i toodevelop Java-program eller tjänster som använder Azure File storage toostore fildata.</span><span class="sxs-lookup"><span data-stu-id="7b151-105">This tutorial will demonstrate hello basics of using Java toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="7b151-106">I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med Java och Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="7b151-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="7b151-107">Skapa och ta bort Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="7b151-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="7b151-108">Skapa och ta bort kataloger</span><span class="sxs-lookup"><span data-stu-id="7b151-108">Create and delete directories</span></span>
* <span data-ttu-id="7b151-109">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="7b151-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="7b151-110">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="7b151-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="7b151-111">Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard Java-i/o-klasser.</span><span class="sxs-lookup"><span data-stu-id="7b151-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Java I/O classes.</span></span> <span data-ttu-id="7b151-112">Den här artikeln beskriver hur toowrite program som använder hello Azure Storage Java SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span><span class="sxs-lookup"><span data-stu-id="7b151-112">This article will describe how toowrite applications that use hello Azure Storage Java SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="7b151-113">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="7b151-113">Create a Java application</span></span>
<span data-ttu-id="7b151-114">toobuild hello prover måste hello Java Development Kit (JDK) och hello [Azure Storage-SDK för Java] [].</span><span class="sxs-lookup"><span data-stu-id="7b151-114">toobuild hello samples, you will need hello Java Development Kit (JDK) and hello [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="7b151-115">Du bör också har skapat ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7b151-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-toouse-azure-file-storage"></a><span data-ttu-id="7b151-116">Konfigurera ditt program toouse Azure File storage</span><span class="sxs-lookup"><span data-stu-id="7b151-116">Setup your application toouse Azure File storage</span></span>
<span data-ttu-id="7b151-117">toouse hello Azure storage-API: er, lägga till hello följande instruktion toohello överkant hello Java-fil där du ska tooaccess hello lagringstjänsten från.</span><span class="sxs-lookup"><span data-stu-id="7b151-117">toouse hello Azure storage APIs, add hello following statement toohello top of hello Java file where you intend tooaccess hello storage service from.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="7b151-118">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="7b151-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="7b151-119">toouse Azure File storage måste tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7b151-119">toouse Azure File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="7b151-120">hello första steget är tooconfigure en anslutningssträng som vi använder tooconnect tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7b151-120">hello first step would be tooconfigure a connection string which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="7b151-121">Definiera en statisk variabel toodo som.</span><span class="sxs-lookup"><span data-stu-id="7b151-121">Let's define a static variable toodo that.</span></span>

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="7b151-122">Ersätt your_storage_account_name och your_storage_account_key med hello faktiska värden för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7b151-122">Replace your_storage_account_name and your_storage_account_key with hello actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="7b151-123">Ansluta tooan Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="7b151-123">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="7b151-124">tooconnect tooyour storage-konto behöver du toouse hello **CloudStorageAccount** objekt som passerar en anslutning sträng tooits **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="7b151-124">tooconnect tooyour storage account, you need toouse hello **CloudStorageAccount** object, passing a connection string tooits **parse** method.</span></span>

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

<span data-ttu-id="7b151-125">**CloudStorageAccount.parse** returnerar ett InvalidKeyException måste tooput den inuti en trycatch blockera.</span><span class="sxs-lookup"><span data-stu-id="7b151-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need tooput it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="7b151-126">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="7b151-126">Create an Azure File share</span></span>
<span data-ttu-id="7b151-127">Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**.</span><span class="sxs-lookup"><span data-stu-id="7b151-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="7b151-128">Storage-konto kan ha så mycket resurser som gör att din kapacitet.</span><span class="sxs-lookup"><span data-stu-id="7b151-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="7b151-129">tooobtain åtkomst tooa resursen och dess innehåll måste toouse en Azure File storage-klient.</span><span class="sxs-lookup"><span data-stu-id="7b151-129">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="7b151-130">Med hello Azure File storage client kan hämta du sedan en referens tooa resurs.</span><span class="sxs-lookup"><span data-stu-id="7b151-130">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="7b151-131">tooactually skapa hello resurs använder hello **createIfNotExists** -metoden i hello CloudFileShare objektet.</span><span class="sxs-lookup"><span data-stu-id="7b151-131">tooactually create hello share, use hello **createIfNotExists** method of hello CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="7b151-132">Nu **dela** innehåller en referens tooa resurs med namnet **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="7b151-132">At this point, **share** holds a reference tooa share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="7b151-133">Ta bort en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="7b151-133">Delete an Azure File share</span></span>
<span data-ttu-id="7b151-134">Om du tar bort en resurs görs genom att anropa hello **deleteIfExists** metod på ett CloudFileShare-objekt.</span><span class="sxs-lookup"><span data-stu-id="7b151-134">Deleting a share is done by calling hello **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="7b151-135">Här är exempelkod som gör detta.</span><span class="sxs-lookup"><span data-stu-id="7b151-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference toohello file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="7b151-136">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="7b151-136">Create a directory</span></span>
<span data-ttu-id="7b151-137">Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="7b151-137">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="7b151-138">Azure File storage kan toocreate som mycket kataloger som ditt konto kommer tillåta.</span><span class="sxs-lookup"><span data-stu-id="7b151-138">Azure File storage allows you toocreate as much directories as your account will allow.</span></span> <span data-ttu-id="7b151-139">hello koden nedan skapar en underkatalog med namnet **sampledir** under hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="7b151-139">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="7b151-140">Ta bort en katalog</span><span class="sxs-lookup"><span data-stu-id="7b151-140">Delete a directory</span></span>
<span data-ttu-id="7b151-141">Om du tar bort en katalog är ganska enkel aktivitet, men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.</span><span class="sxs-lookup"><span data-stu-id="7b151-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory you want toodelete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete hello directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="7b151-142">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="7b151-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="7b151-143">Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **listFilesAndDirectories** för en CloudFileDirectory-referens.</span><span class="sxs-lookup"><span data-stu-id="7b151-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="7b151-144">hello-metoden returnerar en lista över ListFileItem-objekt som du kan söka på.</span><span class="sxs-lookup"><span data-stu-id="7b151-144">hello method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="7b151-145">Exempelvis hello följande kod visar en lista över filer och kataloger i rotkatalogen för hello.</span><span class="sxs-lookup"><span data-stu-id="7b151-145">As an example, hello following code will list files and directories inside hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="7b151-146">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="7b151-146">Upload a file</span></span>
<span data-ttu-id="7b151-147">Resursen innehåller på hello mycket minst en Azure-fil, en rotkatalog där filer kan finnas.</span><span class="sxs-lookup"><span data-stu-id="7b151-147">An Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="7b151-148">I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="7b151-148">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="7b151-149">hello första steget i att överföra en fil är tooobtain en referens toohello katalog där det ska finnas.</span><span class="sxs-lookup"><span data-stu-id="7b151-149">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="7b151-150">Det gör du genom att anropa hello **getRootDirectoryReference** metod för hello dela objekt.</span><span class="sxs-lookup"><span data-stu-id="7b151-150">You do this by calling hello **getRootDirectoryReference** method of hello share object.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="7b151-151">Nu när du har en referens toohello rotkatalog hello-resursen kan överföra du en fil till den med hjälp av följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="7b151-151">Now that you have a reference toohello root directory of hello share, you can upload a file onto it using hello following code.</span></span>

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="7b151-152">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="7b151-152">Download a file</span></span>
<span data-ttu-id="7b151-153">En av hello mer frekventa åtgärder du ska utföra mot Azure File storage är toodownload filer.</span><span class="sxs-lookup"><span data-stu-id="7b151-153">One of hello more frequent operations you will perform against Azure File storage is toodownload files.</span></span> <span data-ttu-id="7b151-154">I följande exempel hello, hello kod hämtar SampleFile.txt och dess innehåll visas.</span><span class="sxs-lookup"><span data-stu-id="7b151-154">In hello following example, hello code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello directory that contains hello file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference toohello file you want toodownload
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write hello contents of hello file toohello console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="7b151-155">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="7b151-155">Delete a file</span></span>
<span data-ttu-id="7b151-156">En annan vanliga Azure File storage-åtgärd är filborttagning.</span><span class="sxs-lookup"><span data-stu-id="7b151-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="7b151-157">hello följande kod tar bort en fil med namnet SampleFile.txt som lagrats i en katalog med namnet **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="7b151-157">hello following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory where hello file toobe deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="7b151-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b151-158">Next steps</span></span>
<span data-ttu-id="7b151-159">Om du vill ha mer information om andra Azure storage-API: er toolearn följa dessa länkar.</span><span class="sxs-lookup"><span data-stu-id="7b151-159">If you would like toolearn more about other Azure storage APIs, follow these links.</span></span>

* [<span data-ttu-id="7b151-160">Java-utvecklingscenter</span><span class="sxs-lookup"><span data-stu-id="7b151-160">Java Developer Center</span></span>](http://azure.microsoft.com/develop/java/)
* [<span data-ttu-id="7b151-161">Azure Storage SDK för Java</span><span class="sxs-lookup"><span data-stu-id="7b151-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="7b151-162">Azure Storage SDK för Android</span><span class="sxs-lookup"><span data-stu-id="7b151-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="7b151-163">Referens för Azure Storage Client SDK</span><span class="sxs-lookup"><span data-stu-id="7b151-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="7b151-164">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="7b151-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="7b151-165">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="7b151-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="7b151-166">Överföra data med kommandoradsverktyget Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="7b151-166">Transfer data with hello AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)
