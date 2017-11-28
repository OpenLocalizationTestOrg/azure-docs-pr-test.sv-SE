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
# <a name="how-toouse-blob-storage-from-java"></a><span data-ttu-id="0ece3-103">Hur toouse Blob storage från Java</span><span class="sxs-lookup"><span data-stu-id="0ece3-103">How toouse Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="0ece3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0ece3-104">Overview</span></span>
<span data-ttu-id="0ece3-105">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ece3-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="0ece3-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="0ece3-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="0ece3-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="0ece3-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="0ece3-108">Den här artikeln visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0ece3-108">This article will show you how tooperform common scenarios using hello Microsoft Azure Blob storage.</span></span> <span data-ttu-id="0ece3-109">hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="0ece3-109">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="0ece3-110">hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ece3-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="0ece3-111">Mer information om BLOB finns hello [nästa steg](#Next-Steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0ece3-111">For more information on blobs, see hello [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="0ece3-112">En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="0ece3-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="0ece3-113">Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="0ece3-113">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="0ece3-114">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="0ece3-114">Create a Java application</span></span>
<span data-ttu-id="0ece3-115">I den här artikeln använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.</span><span class="sxs-lookup"><span data-stu-id="0ece3-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="0ece3-116">toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure Storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0ece3-116">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="0ece3-117">När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0ece3-117">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="0ece3-118">Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0ece3-118">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="0ece3-119">När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0ece3-119">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="0ece3-120">Konfigurera ditt program tooaccess Blob storage</span><span class="sxs-lookup"><span data-stu-id="0ece3-120">Configure your application tooaccess Blob storage</span></span>
<span data-ttu-id="0ece3-121">Lägg till hello efter import instruktioner toohello överkant hello Java filen där du vill toouse hello Azure Storage-API: er tooaccess blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ece3-121">Add hello following import statements toohello top of hello Java file where you want toouse hello Azure Storage APIs tooaccess blobs.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="0ece3-122">Ställ in en anslutningssträng för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0ece3-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="0ece3-123">Ett Azure Storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="0ece3-123">An Azure Storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="0ece3-124">När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure-portalen](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="0ece3-124">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="0ece3-125">hello följande exempel visas hur du kan deklarera en anslutningssträng för statiska fält toohold hello.</span><span class="sxs-lookup"><span data-stu-id="0ece3-125">hello following example shows how you can declare a static field toohold hello connection string.</span></span>

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="0ece3-126">I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod.</span><span class="sxs-lookup"><span data-stu-id="0ece3-126">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="0ece3-127">hello följande exempel hämtar hello anslutningssträng från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ece3-127">hello following example gets hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="0ece3-128">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0ece3-128">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="0ece3-129">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="0ece3-129">Create a container</span></span>
<span data-ttu-id="0ece3-130">En **CloudBlobClient** objekt kan du hämta referensobjekt för behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ece3-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="0ece3-131">hello följande kod skapar en **CloudBlobClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="0ece3-131">hello following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="0ece3-132">Det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].</span><span class="sxs-lookup"><span data-stu-id="0ece3-132">There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="0ece3-133">Använd hello **CloudBlobClient** objekt tooget en referens toohello behållare som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="0ece3-133">Use hello **CloudBlobClient** object tooget a reference toohello container you want toouse.</span></span> <span data-ttu-id="0ece3-134">Du kan skapa hello behållaren om den inte finns med hello **createIfNotExists** metod som annars returnerar hello befintlig behållare.</span><span class="sxs-lookup"><span data-stu-id="0ece3-134">You can create hello container if it doesn't exist with hello **createIfNotExists** method, which will otherwise return hello existing container.</span></span> <span data-ttu-id="0ece3-135">Som standard hello ny behållare är privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) toodownload blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="0ece3-135">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span>

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

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="0ece3-136">Valfritt: Konfigurera en behållare för allmän åtkomst</span><span class="sxs-lookup"><span data-stu-id="0ece3-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="0ece3-137">Behörigheter för en behållare som är konfigurerade för privat åtkomst som standard, men du kan enkelt konfigurera en behållare behörigheter tooallow offentliga, skrivskyddad åtkomst för alla användare på hello Internet:</span><span class="sxs-lookup"><span data-stu-id="0ece3-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions tooallow public, read-only access for all users on hello Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="0ece3-138">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="0ece3-138">Upload a blob into a container</span></span>
<span data-ttu-id="0ece3-139">tooupload filen tooa blob hämta en referens för behållaren och använder det tooget en blobbreferens.</span><span class="sxs-lookup"><span data-stu-id="0ece3-139">tooupload a file tooa blob, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="0ece3-140">När du har en blobbreferens kan du ladda upp en dataström genom att anropa överföringen på hello blob-referens.</span><span class="sxs-lookup"><span data-stu-id="0ece3-140">Once you have a blob reference, you can upload any stream by calling upload on hello blob reference.</span></span> <span data-ttu-id="0ece3-141">Den här åtgärden skapar hello blob om det inte finns eller skriva över den om den finns.</span><span class="sxs-lookup"><span data-stu-id="0ece3-141">This operation will create hello blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="0ece3-142">hello följande kodexempel visar detta och förutsätter att behållaren hello har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="0ece3-142">hello following code sample shows this, and assumes that hello container has already been created.</span></span>

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

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="0ece3-143">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="0ece3-143">List hello blobs in a container</span></span>
<span data-ttu-id="0ece3-144">toolist hello blobbar i en behållare får först en referens för behållaren som du gjorde tooupload en blob.</span><span class="sxs-lookup"><span data-stu-id="0ece3-144">toolist hello blobs in a container, first get a container reference like you did tooupload a blob.</span></span> <span data-ttu-id="0ece3-145">Du kan använda hello behållaren **listBlobs** metod med en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="0ece3-145">You can use hello container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="0ece3-146">hello visar följande kod hello URI: N för varje blobb i en behållare toohello konsol.</span><span class="sxs-lookup"><span data-stu-id="0ece3-146">hello following code outputs hello Uri of each blob in a container toohello console.</span></span>

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

<span data-ttu-id="0ece3-147">Observera att du namnge blobbar med sökvägsinformation i deras namn.</span><span class="sxs-lookup"><span data-stu-id="0ece3-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="0ece3-148">På så sätt skapar du en virtuell katalogstruktur som du kan organisera och bläddra i precis som i ett traditionellt filsystem.</span><span class="sxs-lookup"><span data-stu-id="0ece3-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="0ece3-149">Observera att hello katalogstrukturen bara är virtuell – hello endast resurser som är tillgängliga i Blob storage är behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ece3-149">Note that hello directory structure is virtual only - hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="0ece3-150">Men hello klientbiblioteket erbjuder en **CloudBlobDirectory** objekt toorefer tooa virtuell katalog och förenkla processen hello arbetet med blobbar som är ordnade på det här sättet.</span><span class="sxs-lookup"><span data-stu-id="0ece3-150">However, hello client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="0ece3-151">Du kan till exempel ha en behållare med namnet ”foton”, där du kan ladda upp blobbar med namnet ”rootphoto1”, ”2010/photo1”, ”2010/photo2” och ”2011/photo1”.</span><span class="sxs-lookup"><span data-stu-id="0ece3-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="0ece3-152">Det skulle skapa hello virtuella kataloger ”2010” och ”2011” i hello ”foton”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0ece3-152">This would create hello virtual directories "2010" and "2011" within hello "photos" container.</span></span> <span data-ttu-id="0ece3-153">När du anropar **listBlobs** hello ”foton” behållaren hello-samling som returneras innehåller **CloudBlobDirectory** och **CloudBlob** objekt som representerar hello kataloger respektive blobbar som finns på hello översta nivån.</span><span class="sxs-lookup"><span data-stu-id="0ece3-153">When you call **listBlobs** on hello "photos" container, hello collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="0ece3-154">I det här fallet returneras ”2010” och ”2011”-kataloger, samt foto ”rootphoto1”.</span><span class="sxs-lookup"><span data-stu-id="0ece3-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="0ece3-155">Du kan använda hello **instanceof** operatorn toodistinguish dessa objekt.</span><span class="sxs-lookup"><span data-stu-id="0ece3-155">You can use hello **instanceof** operator toodistinguish these objects.</span></span>

<span data-ttu-id="0ece3-156">Alternativt kan du överföra i parametrar toohello **listBlobs** metod med hello **useFlatBlobListing** parameterinställning tootrue.</span><span class="sxs-lookup"><span data-stu-id="0ece3-156">Optionally, you can pass in parameters toohello **listBlobs** method with hello **useFlatBlobListing** parameter set tootrue.</span></span> <span data-ttu-id="0ece3-157">Detta resulterar i varje blob som returneras oavsett directory.</span><span class="sxs-lookup"><span data-stu-id="0ece3-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="0ece3-158">Mer information finns i **CloudBlobContainer.listBlobs** i hello [referens för Azure Storage Client SDK].</span><span class="sxs-lookup"><span data-stu-id="0ece3-158">For more information, see **CloudBlobContainer.listBlobs** in hello [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="0ece3-159">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="0ece3-159">Download a blob</span></span>
<span data-ttu-id="0ece3-160">toodownload blobbar, följ samma steg som du gjorde för att ladda upp en blobb i ordning tooget en blobbreferens hello.</span><span class="sxs-lookup"><span data-stu-id="0ece3-160">toodownload blobs, follow hello same steps as you did for uploading a blob in order tooget a blob reference.</span></span> <span data-ttu-id="0ece3-161">I hello överför exempel, kallas överföringen på hello blob-objektet.</span><span class="sxs-lookup"><span data-stu-id="0ece3-161">In hello uploading example, you called upload on hello blob object.</span></span> <span data-ttu-id="0ece3-162">I följande exempel hello, anropa download tootransfer hello blob innehållet tooa stream-objektet som en **FileOutputStream** som du kan använda toopersist hello blob tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="0ece3-162">In hello following example, call download tootransfer hello blob contents tooa stream object such as a **FileOutputStream** that you can use toopersist hello blob tooa local file.</span></span>

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

## <a name="delete-a-blob"></a><span data-ttu-id="0ece3-163">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="0ece3-163">Delete a blob</span></span>
<span data-ttu-id="0ece3-164">toodelete blob, hämta en blob referera och anropa **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="0ece3-164">toodelete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="0ece3-165">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0ece3-165">Delete a blob container</span></span>
<span data-ttu-id="0ece3-166">Slutligen toodelete blob-behållaren får en blob referens för behållaren och anropet **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="0ece3-166">Finally, toodelete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0ece3-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ece3-167">Next steps</span></span>
<span data-ttu-id="0ece3-168">Nu när du har lärt dig grunderna hello i Blob storage, följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0ece3-168">Now that you've learned hello basics of Blob storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="0ece3-169">[Azure Storage SDK för Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="0ece3-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="0ece3-170">[Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]</span><span class="sxs-lookup"><span data-stu-id="0ece3-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="0ece3-171">[Azure Storage REST API][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="0ece3-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="0ece3-172">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="0ece3-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="0ece3-173">Mer information finns också [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="0ece3-173">For more information, see also [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
