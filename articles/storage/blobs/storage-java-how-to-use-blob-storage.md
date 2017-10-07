---
title: "aaaHow toouse Azure Blob storage (objektlagring) från Java | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: java
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 2e223b38-92de-4c2f-9254-346374545d32
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: a905d318abdaa7538ec3f6b53b5186b965b8b86e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-java"></a>Hur toouse Blob storage från Java
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Översikt
Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här artikeln visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure Blob storage. hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java]. hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar. Mer information om BLOB finns hello [nästa steg](#Next-Steps) avsnitt.

> [!NOTE]
> En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter. Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Skapa ett Java-program
I den här artikeln använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.

toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure Storage-konto i din Azure-prenumeration. När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub. Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen. När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.

## <a name="configure-your-application-tooaccess-blob-storage"></a>Konfigurera ditt program tooaccess Blob storage
Lägg till hello efter import instruktioner toohello överkant hello Java filen där du vill toouse hello Azure Storage-API: er tooaccess blobbar.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure Storage
Ett Azure Storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure-portalen](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden. hello följande exempel visas hur du kan deklarera en anslutningssträng för statiska fält toohold hello.

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod. hello följande exempel hämtar hello anslutningssträng från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.

## <a name="create-a-container"></a>Skapa en behållare
En **CloudBlobClient** objekt kan du hämta referensobjekt för behållare och blobbar. hello följande kod skapar en **CloudBlobClient** objekt.

> [!NOTE]
> Det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Använd hello **CloudBlobClient** objekt tooget en referens toohello behållare som du vill toouse. Du kan skapa hello behållaren om den inte finns med hello **createIfNotExists** metod som annars returnerar hello befintlig behållare. Som standard hello ny behållare är privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) toodownload blobbar från den här behållaren.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference tooa container.
    // hello container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create hello container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a>Valfritt: Konfigurera en behållare för allmän åtkomst
Behörigheter för en behållare som är konfigurerade för privat åtkomst som standard, men du kan enkelt konfigurera en behållare behörigheter tooallow offentliga, skrivskyddad åtkomst för alla användare på hello Internet:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
tooupload filen tooa blob hämta en referens för behållaren och använder det tooget en blobbreferens. När du har en blobbreferens kan du ladda upp en dataström genom att anropa överföringen på hello blob-referens. Den här åtgärden skapar hello blob om det inte finns eller skriva över den om den finns. hello följande kodexempel visar detta och förutsätter att behållaren hello har redan skapats.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define hello path tooa local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite hello "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare får först en referens för behållaren som du gjorde tooupload en blob. Du kan använda hello behållaren **listBlobs** metod med en **för** loop. hello visar följande kod hello URI: N för varje blobb i en behållare toohello konsol.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within hello container and output hello URI tooeach of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

Observera att du namnge blobbar med sökvägsinformation i deras namn. På så sätt skapar du en virtuell katalogstruktur som du kan organisera och bläddra i precis som i ett traditionellt filsystem. Observera att hello katalogstrukturen bara är virtuell – hello endast resurser som är tillgängliga i Blob storage är behållare och blobbar. Men hello klientbiblioteket erbjuder en **CloudBlobDirectory** objekt toorefer tooa virtuell katalog och förenkla processen hello arbetet med blobbar som är ordnade på det här sättet.

Du kan till exempel ha en behållare med namnet ”foton”, där du kan ladda upp blobbar med namnet ”rootphoto1”, ”2010/photo1”, ”2010/photo2” och ”2011/photo1”. Det skulle skapa hello virtuella kataloger ”2010” och ”2011” i hello ”foton”-behållaren. När du anropar **listBlobs** hello ”foton” behållaren hello-samling som returneras innehåller **CloudBlobDirectory** och **CloudBlob** objekt som representerar hello kataloger respektive blobbar som finns på hello översta nivån. I det här fallet returneras ”2010” och ”2011”-kataloger, samt foto ”rootphoto1”. Du kan använda hello **instanceof** operatorn toodistinguish dessa objekt.

Alternativt kan du överföra i parametrar toohello **listBlobs** metod med hello **useFlatBlobListing** parameterinställning tootrue. Detta resulterar i varje blob som returneras oavsett directory. Mer information finns i **CloudBlobContainer.listBlobs** i hello [referens för Azure Storage Client SDK].

## <a name="download-a-blob"></a>Ladda ned en blob
toodownload blobbar, följ samma steg som du gjorde för att ladda upp en blobb i ordning tooget en blobbreferens hello. I hello överför exempel, kallas överföringen på hello blob-objektet. I följande exempel hello, anropa download tootransfer hello blob innehållet tooa stream-objektet som en **FileOutputStream** som du kan använda toopersist hello blob tooa lokal fil.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in hello container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If hello item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download hello item and save it tooa file with hello same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a>Ta bort en blob
toodelete blob, hämta en blob referera och anropa **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference tooa blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete hello blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a>Ta bort en blob-behållare
Slutligen toodelete blob-behållaren får en blob referens för behållaren och anropet **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete hello blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Blob storage, följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* [Azure Storage SDK för Java][Azure Storage SDK for Java]
* [Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]
* [Azure Storage REST API][Azure Storage REST API]
* [Azure Storage-teamets blogg][Azure Storage Team Blog]

Mer information finns också [Azure för Java-utvecklare](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
