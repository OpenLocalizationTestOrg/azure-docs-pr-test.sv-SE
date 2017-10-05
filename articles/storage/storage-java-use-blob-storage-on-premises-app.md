---
title: Lokala program med blob storage (Java) | Microsoft Docs
description: "Lär dig mer om att skapa ett konsolprogram som laddar upp en bild till Azure och visar bilden i webbläsaren. Kodexempel i Java."
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
ms.openlocfilehash: a172b881fa38a69f4510df94f5797b7a56940c52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="on-premises-application-with-blob-storage"></a>Lokalt program med blob storage
## <a name="overview"></a>Översikt
I följande exempel visas hur du kan använda Azure storage för att lagra avbildningar i Azure. Koden i den här artikeln är för ett konsolprogram som laddar upp en bild till Azure och sedan skapar en HTML-fil som visas i webbläsaren.

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), version 1.6 eller senare har installerats.
* Azure SDK är installerat.
* JAR för Azure-biblioteken för Java och alla tillämpliga beroende burkar har installerats och är i byggsökväg som används av Java-kompilatorn. Information om hur du installerar Azure-biblioteken för Java finns [ladda ner Azure SDK för Java](../java-download-azure-sdk.md).
* Ett Azure storage-konto har ställts in. Kontonamnet och kontonyckel för lagringskontot används av koden i den här artikeln. Se [hur du skapar ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account) information om hur du skapar ett lagringskonto och [visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys) information om hur du hämtar nyckeln för kontot.
* Du har skapat en lokal fil med namnet på sökvägen c:\\myimages\\image1.jpg. Du kan också ändra den **FileInputStream** konstruktorn i exemplet med att använda en annan bild sökvägen och filnamnet.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Använda Azure blob storage för att överföra en fil
Stegvisa anvisningar visas här. Om du vill gå vidare visas hela koden nedan.

Börja koden genom att inkludera import för lagringsklasser Azure kärnor, Azure blob-klientklasser, Java-i/o-klasser och **URISyntaxException** klass.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Deklarera en klass som heter **StorageSample**, och inkludera öppna hakparentes **{**.

```java
public class StorageSample {
```

I den **StorageSample** klassen, deklarera en string-variabel som innehåller endpoint standardprotokoll, namnet på ditt lagringskonto och din lagringsåtkomstnyckel som anges i Azure storage-konto. Ersätta platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** med dina egna namn och kontonyckel, respektive.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Lägga till i en deklaration för **huvudsakliga**, innehåller en **försök** blockera och inkludera behövs öppen hakparenteserna, **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

Deklarera variablerna av följande typ (beskrivningarna är för hur de används i det här exemplet):

* **CloudStorageAccount**: används för att initiera kontoobjektet med Azure storage-kontonamnet och nyckeln och för att skapa klienten blob-objektet.
* **CloudBlobClient**: används för att få åtkomst till blob-tjänsten.
* **CloudBlobContainer**: används för att skapa en blobbbehållare, visa blobbar i behållaren och ta bort behållaren.
* **CloudBlockBlob**: används för att ladda upp en lokal fil till behållaren.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Tilldela ett värde till den **konto** variabeln.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Tilldela ett värde till den **serviceClient** variabeln.

```java
serviceClient = account.createCloudBlobClient();
```

Tilldela ett värde till den **behållare** variabeln. Vi kommer en referens till en behållare med namnet **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

Skapa behållaren. Den här metoden skapar behållaren om den inte finns (och returnera **SANT**). Om behållaren finns, returneras **FALSKT**. Ett alternativ till **createIfNotExists** är den **skapa** metod (som ett fel returneras om det finns redan behållaren).

```java
container.createIfNotExists();
```

Ange anonym åtkomst för behållaren.

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Hämta en referens till blockblob som representerar blobb i Azure storage.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Använd den **filen** konstruktorn för att hämta en referens till lokalt skapade filen som du överför. Se till att du har skapat den här filen innan du kör koden.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

Överför den lokala filen via ett anrop till den **CloudBlockBlob.upload** metod. Den första parametern till den **CloudBlockBlob.upload** metoden är en **FileInputStream** objekt som representerar den lokala filen som överförs till Azure-lagring. Den andra parametern är storlek i byte av filen.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Anropa en helper-funktion med namnet **MakeHTMLPage**, för att göra en grundläggande HTML-sida som innehåller en  **&lt;bild&gt;**  elementet med källan inställt på blob som befinner sig nu i ditt Azure storage konto. Koden för **MakeHTMLPage** kommer att diskuteras senare i den här artikeln.

```java
MakeHTMLPage(container);
```

Skriva ut ett statusmeddelande och information om den skapade HTML-sidan.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

Stäng den **försök** block genom att infoga en Stäng hakparentes: **}**

Hantera följande undantag:

* **FileNotFoundException**: kan uppkomma av **FileInputStream** eller **FileOutputStream** konstruktorer.
* **StorageException**: kan misslyckas på grund av det Azure storage-klientbiblioteket.
* **URISyntaxException**: kan uppkomma av **ListBlobItem.getUri** metod.
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

Skapa en metod med namnet **MakeHTMLPage** att skapa en grundläggande HTML-sida. Den här metoden har en parameter av typen **CloudBlobContainer**, som används för att gå igenom listan över överförda blobbar. Den här metoden genereras undantag av typen **FileNotFoundException**, som kan uppkomma av **FileOutputStream** konstruktorn och **URISyntaxException**, som kan vara utlöstes av den **ListBlobItem.getUri** metod. Inkludera inledande hakparentes **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Skapa en lokal fil med namnet **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Skriva till den lokala filen, lägga till i den  **&lt;html&gt;**,  **&lt;huvud&gt;**, och  **&lt;brödtext&gt;**  element.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

Gå igenom listan över överförda blobbar. För varje blobb i HTML-sidan Skapa ett  **&lt;img&gt;**  element som har dess **src** attribut som skickas till blob-URI eftersom den finns i ditt Azure storage-konto.
Även om du har lagt till bara en bild i det här exemplet, om du har lagt till fler, skulle den här koden iterera alla.

Det här exemplet förutsätter varje uppladdade blobben är en bild för enkelhetens skull. Om du har uppdaterat blobbar än bilder eller sidblobbar i stället för blockblobbar, justera koden efter behov.

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Stäng den  **&lt;brödtext&gt;**  element och  **&lt;html&gt;**  element.

```java
stream.println("</body>");
stream.println("</html>");
```

Stäng filen lokalt.

```java
stream.close();
```

Stäng **MakeHTMLPage** genom att infoga en Stäng hakparentes: **}**

Stäng **StorageSample** genom att infoga en Stäng hakparentes: **}**

Följande är den fullständiga koden för det här exemplet. Kom ihåg att ändra platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** att använda ditt kontonamn och kontonyckel, respektive.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior to running this sample.
// Alternatively, change the value used by the FileInputStream constructor
// to use a different image path and file that you have already created.
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

            // Set anonymous access on the container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point the image is uploaded.
            // Next, create an HTML page that lists all of the uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html to see the images stored in your storage account.");

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

    // Create an HTML page that can be used to display the uploaded images.
    // This example assumes all of the blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create the opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate the uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in the HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete the <html> element and close the file.
        stream.println("</html>");
        stream.close();
    }
}
```

Förutom att ladda upp din lokala image-filen till Azure storage, skapar exempelkoden en lokal fil namedindex.html som du kan öppna i webbläsaren för att se överförda avbildningen.

Eftersom koden innehåller kontonamn och kontonyckel, se till att källkoden är säker.

## <a name="to-delete-a-container"></a>Ta bort en behållare
Eftersom du debiteras för lagring, kanske du vill ta bort den **gettingstarted** behållare när du är klar experimentera med det här exemplet. Ta bort en behållare genom att använda den **CloudBlobContainer.delete** metod.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

Att anropa den **CloudBlobContainer.delete** metod, initieringen **CloudStorageAccount**, **ClodBlobClient**, och **CloudBlobContainer**  objekt är detsamma som visas för den **createIfNotExist** metod. Följande är en komplett exempel som tar bort behållaren med namnet **gettingstarted**.

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

En översikt över andra blob storage-klasser och metoder, se [använda Blob storage från Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Nästa steg
Följa dessa länkar om du vill veta mer om mer komplexa lagringsuppgifter.

* [Azure Storage SDK för Java](https://github.com/azure/azure-storage-java)
* [Referens för Azure Storage Client SDK](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)

