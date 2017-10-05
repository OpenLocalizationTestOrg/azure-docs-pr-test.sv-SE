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
# <a name="develop-for-azure-file-storage-with-java"></a>Utveckla för Azure File storage med Java
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar grunderna i Java för att utveckla program eller tjänster som använder Azure File storage för att lagra fildata. I den här kursen ska vi skapa ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med Java och Azure File storage:

* Skapa och ta bort Azure-filresurser
* Skapa och ta bort kataloger
* Räkna upp filer och kataloger i en filresurs på Azure
* Ladda upp, hämta och ta bort en fil

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt att skriva enkla program som har åtkomst till Azure filresursen med standard Java-i/o-klasser. Den här artikeln beskriver hur du skriver program som använder Azure Storage Java SDK, som använder den [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tala med Azure File storage.

## <a name="create-a-java-application"></a>Skapa ett Java-program
Du behöver för att skapa exemplen Java Development Kit (JDK) och [Azure Storage-SDK för Java] [-]. Du bör också har skapat ett Azure storage-konto.

## <a name="setup-your-application-to-use-azure-file-storage"></a>Konfigurera programmet att använda Azure File storage
Lägg till följande uttryck överst i Java-filen där du vill komma åt tjänsten storage från om du vill använda Azure storage API: er.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Du måste ansluta till Azure storage-konto om du vill använda Azure File storage. Det första steget är att konfigurera en anslutningssträng som vi använder för att ansluta till ditt lagringskonto. Definiera en statisk variabel för att göra det.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Ersätt your_storage_account_name och your_storage_account_key med de faktiska värdena för ditt lagringskonto.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Ansluta till ett Azure storage-konto
För att ansluta till ditt lagringskonto, måste du använda den **CloudStorageAccount** objekt som passerar en anslutningssträng för dess **parsa** metod.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** utlöser ett InvalidKeyException så måste du placera den i ett försök/catch-block.

## <a name="create-an-azure-file-share"></a>Skapa en filresurs på Azure
Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**. Storage-konto kan ha så mycket resurser som gör att din kapacitet. Du måste använda en Azure File storage-klient för att få åtkomst till en resurs och dess innehåll.

```java
// Create the Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Med Azure File storage client kan hämta du sedan en referens till en resurs.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Använd för att skapa resursen faktiskt den **createIfNotExists** -metoden i CloudFileShare-objektet.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Nu **dela** innehåller en referens till en resurs med namnet **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Ta bort en Azure-filresurs
Om du tar bort en resurs görs genom att anropa den **deleteIfExists** metod på ett CloudFileShare-objekt. Här är exempelkod som gör detta.

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

## <a name="create-a-directory"></a>Skapa en katalog
Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i rotkatalogen. Azure File storage kan du skapa så mycket kataloger som ditt konto tillåter. Koden nedan för att skapa en underkatalog med namnet **sampledir** under rotkatalogen.

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

## <a name="delete-a-directory"></a>Ta bort en katalog
Om du tar bort en katalog är ganska enkel aktivitet, men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Räkna upp filer och kataloger i en filresurs på Azure
Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **listFilesAndDirectories** för en CloudFileDirectory-referens. Metoden returnerar en lista över ListFileItem-objekt som du kan söka på. Exempelvis följande kod visar en lista över filer och kataloger i rotkatalogen.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Överför en fil
Resursen innehåller minst en Azure-fil, en rotkatalog där filer kan finnas. I det här avsnittet lär du dig hur du överför en fil från lokal lagring till rotkatalogen för en resurs.

Det första steget i att överföra en fil är att hämta en referens till katalogen där det ska finnas. Du kan göra detta genom att anropa den **getRootDirectoryReference** metod för att dela objekt.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Nu när du har en referens till rotkatalogen för resursen kan överföra du en fil till den med hjälp av följande kod.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Hämta en fil
En av oftare åtgärder du ska utföra mot Azure File storage är att hämta filer. I följande exempel koden hämtar SampleFile.txt och dess innehåll visas.

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

## <a name="delete-a-file"></a>Ta bort en fil
En annan vanliga Azure File storage-åtgärd är filborttagning. Följande kod tar bort en fil med namnet SampleFile.txt som lagrats i en katalog med namnet **sampledir**.

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

## <a name="next-steps"></a>Nästa steg
Följa dessa länkar om du vill lära dig mer om andra Azure storage-API: er.

* [Java-utvecklingscenter](http://azure.microsoft.com/develop/java/)
* [Azure Storage SDK för Java](https://github.com/azure/azure-storage-java)
* [Azure Storage SDK för Android](https://github.com/azure/azure-storage-android)
* [Referens för Azure Storage Client SDK](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)
* [Överföra data med kommandoradsverktyget AzCopy](storage-use-azcopy.md)