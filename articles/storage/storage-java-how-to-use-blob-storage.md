---
title: "Använda Azure Blob storage (objektlagring) från Java | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: b8a4eca600b458802a7a23851bb80ea4da2664ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-java"></a><span data-ttu-id="cff05-103">Använda Blob Storage från Java</span><span class="sxs-lookup"><span data-stu-id="cff05-103">How to use Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="cff05-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="cff05-104">Overview</span></span>
<span data-ttu-id="cff05-105">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="cff05-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="cff05-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="cff05-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="cff05-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="cff05-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="cff05-108">Den här artikeln visar hur du utför vanliga scenarier med hjälp av Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="cff05-108">This article will show you how to perform common scenarios using the Microsoft Azure Blob storage.</span></span> <span data-ttu-id="cff05-109">Exemplen är skrivna i Java och Använd den [Azure Storage SDK för Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="cff05-109">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="cff05-110">Scenarier som tas upp inkluderar **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="cff05-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="cff05-111">Mer information om blobbar finns i [nästa steg](#Next-Steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cff05-111">For more information on blobs, see the [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="cff05-112">En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="cff05-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="cff05-113">Mer information finns i [Azure Storage SDK för Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="cff05-113">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="cff05-114">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="cff05-114">Create a Java application</span></span>
<span data-ttu-id="cff05-115">I den här artikeln använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.</span><span class="sxs-lookup"><span data-stu-id="cff05-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="cff05-116">Om du vill göra det, behöver du installera Java Development Kit (JDK) och skapa ett Azure Storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cff05-116">To do so, you will need to install the Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="cff05-117">När du har gjort det, behöver du kontrollera att utvecklingssystemet uppfyller minsta krav och beroenden som visas i den [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="cff05-117">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="cff05-118">Om datorn uppfyller dessa krav, kan du följa instruktionerna för att hämta och installera Azure Storage-biblioteken för Java på datorn från den lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="cff05-118">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="cff05-119">När du har slutfört uppgifterna, kommer du att kunna skapa ett Java-program som använder exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="cff05-119">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-blob-storage"></a><span data-ttu-id="cff05-120">Konfigurera ditt program för att få åtkomst till Blob storage</span><span class="sxs-lookup"><span data-stu-id="cff05-120">Configure your application to access Blob storage</span></span>
<span data-ttu-id="cff05-121">Lägg till följande importuttryck överst i Java-filen där du vill använda Azure Storage-API: er för att få tillgång till blobbar.</span><span class="sxs-lookup"><span data-stu-id="cff05-121">Add the following import statements to the top of the Java file where you want to use the Azure Storage APIs to access blobs.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="cff05-122">Ställ in en anslutningssträng för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="cff05-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="cff05-123">Ett Azure Storage-klienten använder en anslutningssträng för lagring för att lagra slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="cff05-123">An Azure Storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="cff05-124">När den körs i ett klientprogram, du måste ange anslutningssträngen för lagring i följande format, med hjälp av namnet på ditt lagringskonto och den primära åtkomstnyckeln för lagringskontot som anges i den [Azure-portalen](https://portal.azure.com) för den *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="cff05-124">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="cff05-125">I följande exempel visas hur du kan deklarera statiska fält för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="cff05-125">The following example shows how you can declare a static field to hold the connection string.</span></span>

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="cff05-126">I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i konfigurationsfilen service *ServiceConfiguration.cscfg*, och kan nås med ett anrop till den **RoleEnvironment.getConfigurationSettings** metod.</span><span class="sxs-lookup"><span data-stu-id="cff05-126">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="cff05-127">I följande exempel hämtas anslutningssträng från en **inställningen** element med namnet *StorageConnectionString* i tjänstekonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="cff05-127">The following example gets the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="cff05-128">Följande exempel förutsätter att du har använt ett av dessa två sätt för att hämta anslutningssträngen för lagring.</span><span class="sxs-lookup"><span data-stu-id="cff05-128">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="cff05-129">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="cff05-129">Create a container</span></span>
<span data-ttu-id="cff05-130">En **CloudBlobClient** objekt kan du hämta referensobjekt för behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="cff05-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="cff05-131">Följande kod skapar en **CloudBlobClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="cff05-131">The following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="cff05-132">Det finns flera sätt att skapa **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i den [referens för Azure Storage Client SDK].</span><span class="sxs-lookup"><span data-stu-id="cff05-132">There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="cff05-133">Använd den **CloudBlobClient** objekt för att hämta en referens till den behållare som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="cff05-133">Use the **CloudBlobClient** object to get a reference to the container you want to use.</span></span> <span data-ttu-id="cff05-134">Du kan skapa behållaren om den inte finns med i **createIfNotExists** metod som annars returnerar befintlig behållare.</span><span class="sxs-lookup"><span data-stu-id="cff05-134">You can create the container if it doesn't exist with the **createIfNotExists** method, which will otherwise return the existing container.</span></span> <span data-ttu-id="cff05-135">Som standard är den nya behållaren privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) för att ladda ned blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="cff05-135">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="cff05-136">Valfritt: Konfigurera en behållare för allmän åtkomst</span><span class="sxs-lookup"><span data-stu-id="cff05-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="cff05-137">Behörigheter för en behållare som är konfigurerade för privat åtkomst som standard, men du kan enkelt konfigurera en behållare behörigheter för att tillåta offentlig, skrivskyddad åtkomst för alla användare på Internet:</span><span class="sxs-lookup"><span data-stu-id="cff05-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions to allow public, read-only access for all users on the Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="cff05-138">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="cff05-138">Upload a blob into a container</span></span>
<span data-ttu-id="cff05-139">Om du vill överföra en fil till en blob, hämta en referens för behållaren och använda den för att hämta en blobbreferens.</span><span class="sxs-lookup"><span data-stu-id="cff05-139">To upload a file to a blob, get a container reference and use it to get a blob reference.</span></span> <span data-ttu-id="cff05-140">När du har en blobbreferens kan du ladda upp en dataström genom att anropa överför för blobbreferens.</span><span class="sxs-lookup"><span data-stu-id="cff05-140">Once you have a blob reference, you can upload any stream by calling upload on the blob reference.</span></span> <span data-ttu-id="cff05-141">Den här åtgärden skapas blobben om den inte finns eller skriva över den om så inte.</span><span class="sxs-lookup"><span data-stu-id="cff05-141">This operation will create the blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="cff05-142">Följande kodexempel visar detta och förutsätter att behållaren redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="cff05-142">The following code sample shows this, and assumes that the container has already been created.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="cff05-143">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="cff05-143">List the blobs in a container</span></span>
<span data-ttu-id="cff05-144">Om du vill visa blobbar i en behållare, att hämta en referens för behållaren som du gjorde för att ladda upp en blob.</span><span class="sxs-lookup"><span data-stu-id="cff05-144">To list the blobs in a container, first get a container reference like you did to upload a blob.</span></span> <span data-ttu-id="cff05-145">Du kan använda behållarens **listBlobs** metod med en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="cff05-145">You can use the container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="cff05-146">Följande kod visar Uri för varje blobb i en behållare i konsolen.</span><span class="sxs-lookup"><span data-stu-id="cff05-146">The following code outputs the Uri of each blob in a container to the console.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="cff05-147">Observera att du namnge blobbar med sökvägsinformation i deras namn.</span><span class="sxs-lookup"><span data-stu-id="cff05-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="cff05-148">På så sätt skapar du en virtuell katalogstruktur som du kan organisera och bläddra i precis som i ett traditionellt filsystem.</span><span class="sxs-lookup"><span data-stu-id="cff05-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="cff05-149">Observera att katalogstrukturen bara är virtuell – de enda tillgängliga resurserna i Blob Storage är behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="cff05-149">Note that the directory structure is virtual only - the only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="cff05-150">Men klientbiblioteket erbjuder en **CloudBlobDirectory** objektet att referera till en virtuell katalog och förenkla arbetet med blobbar som är ordnade på det här sättet.</span><span class="sxs-lookup"><span data-stu-id="cff05-150">However, the client library offers a **CloudBlobDirectory** object to refer to a virtual directory and simplify the process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="cff05-151">Du kan till exempel ha en behållare med namnet ”foton”, där du kan ladda upp blobbar med namnet ”rootphoto1”, ”2010/photo1”, ”2010/photo2” och ”2011/photo1”.</span><span class="sxs-lookup"><span data-stu-id="cff05-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="cff05-152">Då skapas de virtuella katalogerna ”2010” och ”2011” i ”foton”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="cff05-152">This would create the virtual directories "2010" and "2011" within the "photos" container.</span></span> <span data-ttu-id="cff05-153">När du anropar **listBlobs** ”foton”-behållaren innehåller samlingen returnerade **CloudBlobDirectory** och **CloudBlob** objekt som representerar kataloger och blobbar som finns på den översta nivån.</span><span class="sxs-lookup"><span data-stu-id="cff05-153">When you call **listBlobs** on the "photos" container, the collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing the directories and blobs contained at the top level.</span></span> <span data-ttu-id="cff05-154">I det här fallet returneras ”2010” och ”2011”-kataloger, samt foto ”rootphoto1”.</span><span class="sxs-lookup"><span data-stu-id="cff05-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="cff05-155">Du kan använda den **instanceof** operatorn för att skilja dessa objekt.</span><span class="sxs-lookup"><span data-stu-id="cff05-155">You can use the **instanceof** operator to distinguish these objects.</span></span>

<span data-ttu-id="cff05-156">Alternativt kan du överföra i parametrar för att den **listBlobs** metod med den **useFlatBlobListing** parametern inställd på true.</span><span class="sxs-lookup"><span data-stu-id="cff05-156">Optionally, you can pass in parameters to the **listBlobs** method with the **useFlatBlobListing** parameter set to true.</span></span> <span data-ttu-id="cff05-157">Detta resulterar i varje blob som returneras oavsett directory.</span><span class="sxs-lookup"><span data-stu-id="cff05-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="cff05-158">Mer information finns i **CloudBlobContainer.listBlobs** i den [referens för Azure Storage Client SDK].</span><span class="sxs-lookup"><span data-stu-id="cff05-158">For more information, see **CloudBlobContainer.listBlobs** in the [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="cff05-159">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="cff05-159">Download a blob</span></span>
<span data-ttu-id="cff05-160">Följ samma steg för att ladda ned blobbar som du gjorde för att överföra en blob för att hämta en blobbreferens.</span><span class="sxs-lookup"><span data-stu-id="cff05-160">To download blobs, follow the same steps as you did for uploading a blob in order to get a blob reference.</span></span> <span data-ttu-id="cff05-161">I exemplet överför anropa överför för blob-objektet.</span><span class="sxs-lookup"><span data-stu-id="cff05-161">In the uploading example, you called upload on the blob object.</span></span> <span data-ttu-id="cff05-162">I följande exempel anropar hämta för att överföra blobbinnehållet till ett stream-objektet som en **FileOutputStream** som du kan använda för att bevara blob till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="cff05-162">In the following example, call download to transfer the blob contents to a stream object such as a **FileOutputStream** that you can use to persist the blob to a local file.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="cff05-163">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="cff05-163">Delete a blob</span></span>
<span data-ttu-id="cff05-164">Ta bort en blob och hämta en blobbreferens anropet **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="cff05-164">To delete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="cff05-165">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="cff05-165">Delete a blob container</span></span>
<span data-ttu-id="cff05-166">Hämta slutligen en blob för att ta bort en blob-behållare, referens för behållaren och anropet **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="cff05-166">Finally, to delete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="cff05-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cff05-167">Next steps</span></span>
<span data-ttu-id="cff05-168">Nu när du har lärt dig grunderna om Blob storage kan du följa dessa länkar att lära dig mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cff05-168">Now that you've learned the basics of Blob storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="cff05-169">[Azure Storage SDK för Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="cff05-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="cff05-170">[Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]</span><span class="sxs-lookup"><span data-stu-id="cff05-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="cff05-171">[Azure Storage REST API][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="cff05-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="cff05-172">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="cff05-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="cff05-173">Mer information finns också i [Java-Utvecklingscenter](/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="cff05-173">For more information, see also the [Java Developer Center](/develop/java/).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
<span data-ttu-id="cff05-174">[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/</span><span class="sxs-lookup"><span data-stu-id="cff05-174">[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/</span></span>
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
