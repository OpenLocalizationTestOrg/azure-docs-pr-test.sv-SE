---
title: "Utveckla för Azure File storage med Java | Microsoft Docs"
description: "Lär dig hur du utvecklar Java-program och tjänster som använder Azure File storage för att lagra fildata."
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
ms.openlocfilehash: 16924599e49990265e07f7a58613756d93c46942
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="ea65d-103">Utveckla för Azure File storage med Java</span><span class="sxs-lookup"><span data-stu-id="ea65d-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="ea65d-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="ea65d-104">About this tutorial</span></span>
<span data-ttu-id="ea65d-105">Den här kursen visar grunderna i Java för att utveckla program eller tjänster som använder Azure File storage för att lagra fildata.</span><span class="sxs-lookup"><span data-stu-id="ea65d-105">This tutorial will demonstrate the basics of using Java to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="ea65d-106">I den här kursen ska vi skapa ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med Java och Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="ea65d-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="ea65d-107">Skapa och ta bort Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="ea65d-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="ea65d-108">Skapa och ta bort kataloger</span><span class="sxs-lookup"><span data-stu-id="ea65d-108">Create and delete directories</span></span>
* <span data-ttu-id="ea65d-109">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="ea65d-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="ea65d-110">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="ea65d-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="ea65d-111">Eftersom Azure File storage kan nås över SMB, är det möjligt att skriva enkla program som har åtkomst till Azure filresursen med standard Java-i/o-klasser.</span><span class="sxs-lookup"><span data-stu-id="ea65d-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Java I/O classes.</span></span> <span data-ttu-id="ea65d-112">Den här artikeln beskriver hur du skriver program som använder Azure Storage Java SDK, som använder den [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tala med Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="ea65d-112">This article will describe how to write applications that use the Azure Storage Java SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="ea65d-113">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="ea65d-113">Create a Java application</span></span>
<span data-ttu-id="ea65d-114">Du behöver för att skapa exemplen Java Development Kit (JDK) och [Azure Storage-SDK för Java] [-].</span><span class="sxs-lookup"><span data-stu-id="ea65d-114">To build the samples, you will need the Java Development Kit (JDK) and the [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="ea65d-115">Du bör också har skapat ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ea65d-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-to-use-azure-file-storage"></a><span data-ttu-id="ea65d-116">Konfigurera programmet att använda Azure File storage</span><span class="sxs-lookup"><span data-stu-id="ea65d-116">Setup your application to use Azure File storage</span></span>
<span data-ttu-id="ea65d-117">Lägg till följande uttryck överst i Java-filen där du vill komma åt tjänsten storage från om du vill använda Azure storage API: er.</span><span class="sxs-lookup"><span data-stu-id="ea65d-117">To use the Azure storage APIs, add the following statement to the top of the Java file where you intend to access the storage service from.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="ea65d-118">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="ea65d-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="ea65d-119">Du måste ansluta till Azure storage-konto om du vill använda Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="ea65d-119">To use Azure File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="ea65d-120">Det första steget är att konfigurera en anslutningssträng som vi använder för att ansluta till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea65d-120">The first step would be to configure a connection string which we'll use to connect to your storage account.</span></span> <span data-ttu-id="ea65d-121">Definiera en statisk variabel för att göra det.</span><span class="sxs-lookup"><span data-stu-id="ea65d-121">Let's define a static variable to do that.</span></span>

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="ea65d-122">Ersätt your_storage_account_name och your_storage_account_key med de faktiska värdena för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea65d-122">Replace your_storage_account_name and your_storage_account_key with the actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="ea65d-123">Ansluta till ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="ea65d-123">Connecting to an Azure storage account</span></span>
<span data-ttu-id="ea65d-124">För att ansluta till ditt lagringskonto, måste du använda den **CloudStorageAccount** objekt som passerar en anslutningssträng för dess **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="ea65d-124">To connect to your storage account, you need to use the **CloudStorageAccount** object, passing a connection string to its **parse** method.</span></span>

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

<span data-ttu-id="ea65d-125">**CloudStorageAccount.parse** utlöser ett InvalidKeyException så måste du placera den i ett försök/catch-block.</span><span class="sxs-lookup"><span data-stu-id="ea65d-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need to put it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="ea65d-126">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="ea65d-126">Create an Azure File share</span></span>
<span data-ttu-id="ea65d-127">Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**.</span><span class="sxs-lookup"><span data-stu-id="ea65d-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="ea65d-128">Storage-konto kan ha så mycket resurser som gör att din kapacitet.</span><span class="sxs-lookup"><span data-stu-id="ea65d-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="ea65d-129">Du måste använda en Azure File storage-klient för att få åtkomst till en resurs och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="ea65d-129">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```java
// Create the Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="ea65d-130">Med Azure File storage client kan hämta du sedan en referens till en resurs.</span><span class="sxs-lookup"><span data-stu-id="ea65d-130">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="ea65d-131">Använd för att skapa resursen faktiskt den **createIfNotExists** -metoden i CloudFileShare-objektet.</span><span class="sxs-lookup"><span data-stu-id="ea65d-131">To actually create the share, use the **createIfNotExists** method of the CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="ea65d-132">Nu **dela** innehåller en referens till en resurs med namnet **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="ea65d-132">At this point, **share** holds a reference to a share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="ea65d-133">Ta bort en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="ea65d-133">Delete an Azure File share</span></span>
<span data-ttu-id="ea65d-134">Om du tar bort en resurs görs genom att anropa den **deleteIfExists** metod på ett CloudFileShare-objekt.</span><span class="sxs-lookup"><span data-stu-id="ea65d-134">Deleting a share is done by calling the **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="ea65d-135">Här är exempelkod som gör detta.</span><span class="sxs-lookup"><span data-stu-id="ea65d-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="ea65d-136">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="ea65d-136">Create a directory</span></span>
<span data-ttu-id="ea65d-137">Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ea65d-137">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="ea65d-138">Azure File storage kan du skapa så mycket kataloger som ditt konto tillåter.</span><span class="sxs-lookup"><span data-stu-id="ea65d-138">Azure File storage allows you to create as much directories as your account will allow.</span></span> <span data-ttu-id="ea65d-139">Koden nedan för att skapa en underkatalog med namnet **sampledir** under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ea65d-139">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="ea65d-140">Ta bort en katalog</span><span class="sxs-lookup"><span data-stu-id="ea65d-140">Delete a directory</span></span>
<span data-ttu-id="ea65d-141">Om du tar bort en katalog är ganska enkel aktivitet, men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.</span><span class="sxs-lookup"><span data-stu-id="ea65d-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="ea65d-142">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="ea65d-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="ea65d-143">Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **listFilesAndDirectories** för en CloudFileDirectory-referens.</span><span class="sxs-lookup"><span data-stu-id="ea65d-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="ea65d-144">Metoden returnerar en lista över ListFileItem-objekt som du kan söka på.</span><span class="sxs-lookup"><span data-stu-id="ea65d-144">The method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="ea65d-145">Exempelvis följande kod visar en lista över filer och kataloger i rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ea65d-145">As an example, the following code will list files and directories inside the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="ea65d-146">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="ea65d-146">Upload a file</span></span>
<span data-ttu-id="ea65d-147">Resursen innehåller minst en Azure-fil, en rotkatalog där filer kan finnas.</span><span class="sxs-lookup"><span data-stu-id="ea65d-147">An Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="ea65d-148">I det här avsnittet lär du dig hur du överför en fil från lokal lagring till rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="ea65d-148">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="ea65d-149">Det första steget i att överföra en fil är att hämta en referens till katalogen där det ska finnas.</span><span class="sxs-lookup"><span data-stu-id="ea65d-149">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="ea65d-150">Du kan göra detta genom att anropa den **getRootDirectoryReference** metod för att dela objekt.</span><span class="sxs-lookup"><span data-stu-id="ea65d-150">You do this by calling the **getRootDirectoryReference** method of the share object.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="ea65d-151">Nu när du har en referens till rotkatalogen för resursen kan överföra du en fil till den med hjälp av följande kod.</span><span class="sxs-lookup"><span data-stu-id="ea65d-151">Now that you have a reference to the root directory of the share, you can upload a file onto it using the following code.</span></span>

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="ea65d-152">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="ea65d-152">Download a file</span></span>
<span data-ttu-id="ea65d-153">En av oftare åtgärder du ska utföra mot Azure File storage är att hämta filer.</span><span class="sxs-lookup"><span data-stu-id="ea65d-153">One of the more frequent operations you will perform against Azure File storage is to download files.</span></span> <span data-ttu-id="ea65d-154">I följande exempel koden hämtar SampleFile.txt och dess innehåll visas.</span><span class="sxs-lookup"><span data-stu-id="ea65d-154">In the following example, the code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="ea65d-155">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="ea65d-155">Delete a file</span></span>
<span data-ttu-id="ea65d-156">En annan vanliga Azure File storage-åtgärd är filborttagning.</span><span class="sxs-lookup"><span data-stu-id="ea65d-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="ea65d-157">Följande kod tar bort en fil med namnet SampleFile.txt som lagrats i en katalog med namnet **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="ea65d-157">The following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="ea65d-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea65d-158">Next steps</span></span>
<span data-ttu-id="ea65d-159">Följa dessa länkar om du vill lära dig mer om andra Azure storage-API: er.</span><span class="sxs-lookup"><span data-stu-id="ea65d-159">If you would like to learn more about other Azure storage APIs, follow these links.</span></span>

* [<span data-ttu-id="ea65d-160">Java-utvecklingscenter</span><span class="sxs-lookup"><span data-stu-id="ea65d-160">Java Developer Center</span></span>](http://azure.microsoft.com/develop/java/)
* [<span data-ttu-id="ea65d-161">Azure Storage SDK för Java</span><span class="sxs-lookup"><span data-stu-id="ea65d-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="ea65d-162">Azure Storage SDK för Android</span><span class="sxs-lookup"><span data-stu-id="ea65d-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="ea65d-163">Referens för Azure Storage Client SDK</span><span class="sxs-lookup"><span data-stu-id="ea65d-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="ea65d-164">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="ea65d-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="ea65d-165">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="ea65d-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="ea65d-166">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="ea65d-166">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)