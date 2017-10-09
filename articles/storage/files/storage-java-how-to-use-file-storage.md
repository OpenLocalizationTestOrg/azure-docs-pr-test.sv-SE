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
ms.openlocfilehash: be71a946604da8af0130f101f2eb6135c5e08abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a>Utveckla för Azure File storage med Java
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar hello grunderna i toodevelop Java-program eller tjänster som använder Azure File storage toostore fildata. I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med Java och Azure File storage:

* Skapa och ta bort Azure-filresurser
* Skapa och ta bort kataloger
* Räkna upp filer och kataloger i en filresurs på Azure
* Ladda upp, hämta och ta bort en fil

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard Java-i/o-klasser. Den här artikeln beskriver hur toowrite program som använder hello Azure Storage Java SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.

## <a name="create-a-java-application"></a>Skapa ett Java-program
toobuild hello prover måste hello Java Development Kit (JDK) och hello [Azure Storage-SDK för Java] []. Du bör också har skapat ett Azure storage-konto.

## <a name="setup-your-application-toouse-azure-file-storage"></a>Konfigurera ditt program toouse Azure File storage
toouse hello Azure storage-API: er, lägga till hello följande instruktion toohello överkant hello Java-fil där du ska tooaccess hello lagringstjänsten från.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
toouse Azure File storage måste tooconnect tooyour Azure storage-konto. hello första steget är tooconfigure en anslutningssträng som vi använder tooconnect tooyour storage-konto. Definiera en statisk variabel toodo som.

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Ersätt your_storage_account_name och your_storage_account_key med hello faktiska värden för ditt lagringskonto.
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a>Ansluta tooan Azure storage-konto
tooconnect tooyour storage-konto behöver du toouse hello **CloudStorageAccount** objekt som passerar en anslutning sträng tooits **parsa** metod.

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

**CloudStorageAccount.parse** returnerar ett InvalidKeyException måste tooput den inuti en trycatch blockera.

## <a name="create-an-azure-file-share"></a>Skapa en filresurs på Azure
Alla filer och kataloger i Azure File storage finns i en behållare som kallas en **resursen**. Storage-konto kan ha så mycket resurser som gör att din kapacitet. tooobtain åtkomst tooa resursen och dess innehåll måste toouse en Azure File storage-klient.

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Med hello Azure File storage client kan hämta du sedan en referens tooa resurs.

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

tooactually skapa hello resurs använder hello **createIfNotExists** -metoden i hello CloudFileShare objektet.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Nu **dela** innehåller en referens tooa resurs med namnet **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Ta bort en Azure-filresurs
Om du tar bort en resurs görs genom att anropa hello **deleteIfExists** metod på ett CloudFileShare-objekt. Här är exempelkod som gör detta.

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

## <a name="create-a-directory"></a>Skapa en katalog
Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog. Azure File storage kan toocreate som mycket kataloger som ditt konto kommer tillåta. hello koden nedan skapar en underkatalog med namnet **sampledir** under hello rotkatalog.

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

## <a name="delete-a-directory"></a>Ta bort en katalog
Om du tar bort en katalog är ganska enkel aktivitet, men det bör noteras att du inte kan ta bort en katalog som fortfarande innehåller filer eller andra kataloger.

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Räkna upp filer och kataloger i en filresurs på Azure
Hämtar en lista över filer och kataloger i en resurs görs enkelt genom att anropa **listFilesAndDirectories** för en CloudFileDirectory-referens. hello-metoden returnerar en lista över ListFileItem-objekt som du kan söka på. Exempelvis hello följande kod visar en lista över filer och kataloger i rotkatalogen för hello.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Överför en fil
Resursen innehåller på hello mycket minst en Azure-fil, en rotkatalog där filer kan finnas. I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.

hello första steget i att överföra en fil är tooobtain en referens toohello katalog där det ska finnas. Det gör du genom att anropa hello **getRootDirectoryReference** metod för hello dela objekt.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Nu när du har en referens toohello rotkatalog hello-resursen kan överföra du en fil till den med hjälp av följande kod hello.

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Hämta en fil
En av hello mer frekventa åtgärder du ska utföra mot Azure File storage är toodownload filer. I följande exempel hello, hello kod hämtar SampleFile.txt och dess innehåll visas.

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

## <a name="delete-a-file"></a>Ta bort en fil
En annan vanliga Azure File storage-åtgärd är filborttagning. hello följande kod tar bort en fil med namnet SampleFile.txt som lagrats i en katalog med namnet **sampledir**.

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

## <a name="next-steps"></a>Nästa steg
Om du vill ha mer information om andra Azure storage-API: er toolearn följa dessa länkar.

* [Azure för Java-utvecklare](/java/azure)/)
* [Azure Storage SDK för Java](https://github.com/azure/azure-storage-java)
* [Azure Storage SDK för Android](https://github.com/azure/azure-storage-android)
* [Referens för Azure Storage Client SDK](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)
* [Överföra data med kommandoradsverktyget Azcopy hello](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)