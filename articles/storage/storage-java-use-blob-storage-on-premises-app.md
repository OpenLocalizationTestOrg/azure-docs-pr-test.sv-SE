---
title: aaaOn lokala program med blob storage (Java) | Microsoft Docs
description: "Lär dig hur toocreate ett konsolprogram som överför en bild tooAzure och visar hello bilden i webbläsaren. Kodexempel i Java."
services: storage
documentationcenter: java
author: mmacy
manager: carmonm
editor: tysonn
ms.assetid: ccc9a7d7-6fe4-467b-b7fd-a73f17539e3f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/17/2016
ms.author: marsma
ms.openlocfilehash: ed8eb4c1045691c25abe94bf6c1b18b797adc3e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="on-premises-application-with-blob-storage"></a>Lokalt program med blob storage
## <a name="overview"></a>Översikt
hello följande exempel visar hur du kan använda Azure storage för att lagra avbildningar i Azure. hello är i den här artikeln för ett konsolprogram som överför en bild tooAzure och sedan skapar en HTML-fil som visar hello bilden i webbläsaren.

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), version 1.6 eller senare har installerats.
* hello Azure SDK är installerat.
* hello JAR för hello Azure-biblioteken för Java och alla tillämpliga beroende burkar har installerats och är i hello byggsökväg som används av Java-kompilatorn. Information om hur du installerar hello Azure-biblioteken för Java finns [Download hello Azure SDK för Java](../java-download-azure-sdk.md).
* Ett Azure storage-konto har ställts in. hello används kontonamn och din kontonyckel för hello storage-konto av hello kod i den här artikeln. Se [hur tooCreate ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account) information om hur du skapar ett lagringskonto och [visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys) information om hur du hämtar hello kontonyckel.
* Du har skapat en lokal fil med namnet på hello sökvägen c:\\myimages\\image1.jpg. Du kan också ändra den **FileInputStream** konstruktor i hello exempel toouse en annan bild sökvägen och filnamnet.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a>toouse Azure blob storage tooupload en fil
Stegvisa anvisningar visas här. Om du vill tooskip vidare presenteras hello hela koden nedan.

Börja hello kod genom att inkludera importer för hello Azure storage-kärnklasser, hello Azure blob-klientklasser, hello Java-i/o-klasser och hello **URISyntaxException** klass.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Deklarera en klass som heter **StorageSample**, och inkludera hello inledande, **{**.

```java
public class StorageSample {
```

Inom hello **StorageSample** klassen, deklarera en string-variabel som innehåller hello standardprotokoll för slutpunkten, namnet på ditt lagringskonto och din lagringsåtkomstnyckel som anges i Azure storage-konto. Ersätt hello platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** med dina egna namn och kontonyckel, respektive.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Lägga till i en deklaration för **huvudsakliga**, innehåller en **försök** blockera och inkludera hello behövs öppen hakparenteserna, **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

Deklarera variabler av hello efter typ (hello beskrivningarna är för hur de används i det här exemplet):

* **CloudStorageAccount**: använda tooinitialize hello-kontot med Azure storage-kontonamnet och nyckeln och toocreate klienten blob-objektet.
* **CloudBlobClient**: används tooaccess hello blob-tjänsten.
* **CloudBlobContainer**: använda toocreate blob-behållaren visa blobbar i hello-behållaren och ta bort hello behållare.
* **CloudBlockBlob**: använda tooupload en lokal image-filen toothe behållare.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Tilldela ett värde toohello **konto** variabeln.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Tilldela ett värde toohello **serviceClient** variabeln.

```java
serviceClient = account.createCloudBlobClient();
```

Tilldela ett värde toohello **behållare** variabeln. Vi kommer en referens tooa behållare med namnet **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

Skapa hello behållare. Den här metoden skapar hello behållaren om den inte finns (och returnera **SANT**). Om hello behållaren finns, returneras **FALSKT**. Ett alternativ för**createIfNotExists** är hello **skapa** metod (som ett fel returneras om hello-behållaren finns redan).

```java
container.createIfNotExists();
```

Ange anonym åtkomst för hello behållare.

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Hämta en referens toohello blockblob, som representerar hello blobb i Azure storage.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Använd hello **filen** konstruktorn tooget en referens toohello lokalt skapade-fil som du överför. Se till att du har skapat den här filen innan du kör hello kod.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

Ladda upp hello lokala filen via ett anrop toohello **CloudBlockBlob.upload** metod. Hej den första parametern toohello **CloudBlockBlob.upload** metoden är en **FileInputStream** objekt som representerar hello lokal fil som ska överföras tooAzure lagring. hello andra parametern är hello storlek i byte av hello-fil.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Anropa en helper-funktion med namnet **MakeHTMLPage**, toomake grundläggande HTML-sida som innehåller en  **&lt;bild&gt;**  element med hello källa set toohello blob som är nu i din Azure Storage-konto. Hej koden för **MakeHTMLPage** kommer att diskuteras senare i den här artikeln.

```java
MakeHTMLPage(container);
```

Skriva ut ett statusmeddelande och information om hello skapa HTML-sida.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

Stäng hello **försök** block genom att infoga en Stäng hakparentes: **}**

Hantera hello följande undantag:

* **FileNotFoundException**: kan uppkomma hello **FileInputStream** eller **FileOutputStream** konstruktorer.
* **StorageException**: kan uppkomma hello Azure storage-klientbiblioteket.
* **URISyntaxException**: kan uppkomma hello **ListBlobItem.getUri** metod.
* **Undantag**: Allmänt undantagshantering.

<!-- -->

```java
catch (FileNotFoundException fileNotFoundException)
{
    System.out.print("FileNotFoundException encountered: ");
    System.out.println(fileNotFoundException.getMessage());
    System.exit(-1);
}
catch (StorageException storageException)
{
    System.out.print("StorageException encountered: ");
    System.out.println(storageException.getMessage());
    System.exit(-1);
}
catch (URISyntaxException uriSyntaxException)
{
    System.out.print("URISyntaxException encountered: ");
    System.out.println(uriSyntaxException.getMessage());
    System.exit(-1);
}
catch (Exception e)
{
    System.out.print("Exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Stäng **huvudsakliga** genom att infoga en Stäng hakparentes: **}**

Skapa en metod med namnet **MakeHTMLPage** toocreate grundläggande HTML-sidan. Den här metoden har en parameter av typen **CloudBlobContainer**, som kommer att använda tooiterate via hello lista över överförda blobbar. Den här metoden genereras undantag av typen **FileNotFoundException**, vilket kan uppkomma hello **FileOutputStream** konstruktorn och **URISyntaxException**, vilket kan uppkomma hello **ListBlobItem.getUri** metod. Inkludera inledande hakparentes **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Skapa en lokal fil med namnet **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Skriva toohello lokal fil, lägger till i hello  **&lt;html&gt;**,  **&lt;huvud&gt;**, och  **&lt;brödtext&gt;**  element.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

Gå igenom hello lista över överförda blobbar. För varje blob hello HTML-sidan Skapa i en  **&lt;img&gt;**  element som har dess **src** attribut som skickas till hello hello blob-URI eftersom den finns i ditt Azure storage-konto.
Även om du har lagt till bara en bild i det här exemplet, om du har lagt till fler, skulle den här koden iterera alla.

Det här exemplet förutsätter varje uppladdade blobben är en bild för enkelhetens skull. Om du har uppdaterat blobbar än bilder eller sidblobbar i stället för blockblobbar, justera hello kod efter behov.

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Stäng hello  **&lt;brödtext&gt;**  element och hello  **&lt;html&gt;**  element.

```java
stream.println("</body>");
stream.println("</html>");
```

Stäng hello lokal fil.

```java
stream.close();
```

Stäng **MakeHTMLPage** genom att infoga en Stäng hakparentes: **}**

Stäng **StorageSample** genom att infoga en Stäng hakparentes: **}**

hello följer hello fullständiga koden för det här exemplet. Kom ihåg toomodify hello platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** toouse ditt kontonamn och -konto nyckel, respektive.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior toorunning this sample.
// Alternatively, change hello value used by hello FileInputStream constructor
// toouse a different image path and file that you have already created.
public class StorageSample {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                    "AccountName=your_account_name;" +
                    "AccountKey=your_account_name";

    public static void main(String[] args) {
        try {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;
            CloudBlockBlob blob;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.createIfNotExists();

            // Set anonymous access on hello container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point hello image is uploaded.
            // Next, create an HTML page that lists all of hello uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html toosee hello images stored in your storage account.");

        } catch (FileNotFoundException fileNotFoundException) {
            System.out.print("FileNotFoundException encountered: ");
            System.out.println(fileNotFoundException.getMessage());
            System.exit(-1);
        } catch (StorageException storageException) {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        } catch (URISyntaxException uriSyntaxException) {
            System.out.print("URISyntaxException encountered: ");
            System.out.println(uriSyntaxException.getMessage());
            System.exit(-1);
        } catch (Exception e) {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }

    // Create an HTML page that can be used toodisplay hello uploaded images.
    // This example assumes all of hello blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create hello opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate hello uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in hello HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete hello <html> element and close hello file.
        stream.println("</html>");
        stream.close();
    }
}
```

I tillägg toouploading lagringsenheterna lokal image-filen tooAzure hello exempelkod skapar en lokal fil namedindex.html som du kan öppna i din webbläsare toosee överförda avbildningen.

Eftersom hello kod innehåller kontonamn och kontonyckel, se till att källkoden är säker.

## <a name="toodelete-a-container"></a>toodelete en behållare
Eftersom debiteras du för lagring av du toodelete den **gettingstarted** behållare när du är klar experimentera med det här exemplet. toodelete en behållare använder hello **CloudBlobContainer.delete** metod.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

toocall hello **CloudBlobContainer.delete** metod, hello processen för att initiera **CloudStorageAccount**, **ClodBlobClient**, och  **CloudBlobContainer** objekt är hello samma värde som visas för den **createIfNotExist** metod. hello följande är en komplett exempel som tar bort hello behållare med namnet **gettingstarted**.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

public class DeleteContainer {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                "AccountName=your_account_name;" +
                "AccountKey=your_account_key";

    public static void main(String[] args)
    {
        try
        {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.delete();

            System.out.println("Container deleted.");

        }
        catch (StorageException storageException)
        {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        }
        catch (Exception e)
        {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }
}
```

En översikt över andra blob storage-klasser och metoder, se [hur toouse Blob storage från Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om mer komplexa lagringsuppgifter.

* [Azure Storage SDK för Java](https://github.com/azure/azure-storage-java)
* [Referens för Azure Storage Client SDK](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)

