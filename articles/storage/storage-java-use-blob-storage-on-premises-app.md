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
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="dc3e7-104">Lokalt program med blob storage</span><span class="sxs-lookup"><span data-stu-id="dc3e7-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="dc3e7-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="dc3e7-105">Overview</span></span>
<span data-ttu-id="dc3e7-106">I följande exempel visas hur du kan använda Azure storage för att lagra avbildningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-106">The following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="dc3e7-107">Koden i den här artikeln är för ett konsolprogram som laddar upp en bild till Azure och sedan skapar en HTML-fil som visas i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-107">The code in this article is for a console application that uploads an image to Azure, and then creates an HTML file that displays the image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc3e7-108">Krav</span><span class="sxs-lookup"><span data-stu-id="dc3e7-108">Prerequisites</span></span>
* <span data-ttu-id="dc3e7-109">En Java Developer Kit (JDK), version 1.6 eller senare har installerats.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="dc3e7-110">Azure SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-110">The Azure SDK is installed.</span></span>
* <span data-ttu-id="dc3e7-111">JAR för Azure-biblioteken för Java och alla tillämpliga beroende burkar har installerats och är i byggsökväg som används av Java-kompilatorn.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-111">The JAR for the Azure Libraries for Java, and any applicable dependency JARs, are installed and are in the build path used by your Java compiler.</span></span> <span data-ttu-id="dc3e7-112">Information om hur du installerar Azure-biblioteken för Java finns [ladda ner Azure SDK för Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="dc3e7-112">For information about installing the Azure Libraries for Java, see [Download the Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="dc3e7-113">Ett Azure storage-konto har ställts in.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="dc3e7-114">Kontonamnet och kontonyckel för lagringskontot används av koden i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-114">The account name and account key for the storage account will be used by the code in this article.</span></span> <span data-ttu-id="dc3e7-115">Se [hur du skapar ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account) information om hur du skapar ett lagringskonto och [visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys) information om hur du hämtar nyckeln för kontot.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-115">See [How to Create a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving the account key.</span></span>
* <span data-ttu-id="dc3e7-116">Du har skapat en lokal fil med namnet på sökvägen c:\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-116">You have created a local image file named stored at the path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="dc3e7-117">Du kan också ändra den **FileInputStream** konstruktorn i exemplet med att använda en annan bild sökvägen och filnamnet.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-117">Alternatively, modify the **FileInputStream** constructor in the example to use a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a><span data-ttu-id="dc3e7-118">Använda Azure blob storage för att överföra en fil</span><span class="sxs-lookup"><span data-stu-id="dc3e7-118">To use Azure blob storage to upload a file</span></span>
<span data-ttu-id="dc3e7-119">Stegvisa anvisningar visas här.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="dc3e7-120">Om du vill gå vidare visas hela koden nedan.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-120">If you'd like to skip ahead, the entire code is presented later in this article.</span></span>

<span data-ttu-id="dc3e7-121">Börja koden genom att inkludera import för lagringsklasser Azure kärnor, Azure blob-klientklasser, Java-i/o-klasser och **URISyntaxException** klass.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-121">Begin the code by including imports for the Azure core storage classes, the Azure blob client classes, the Java IO classes, and the **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="dc3e7-122">Deklarera en klass som heter **StorageSample**, och inkludera öppna hakparentes **{**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-122">Declare a class named **StorageSample**, and include the open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="dc3e7-123">I den **StorageSample** klassen, deklarera en string-variabel som innehåller endpoint standardprotokoll, namnet på ditt lagringskonto och din lagringsåtkomstnyckel som anges i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-123">Within the **StorageSample** class, declare a string variable that will contain the default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="dc3e7-124">Ersätta platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** med dina egna namn och kontonyckel, respektive.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-124">Replace the placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="dc3e7-125">Lägga till i en deklaration för **huvudsakliga**, innehåller en **försök** blockera och inkludera behövs öppen hakparenteserna, **{**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-125">Add in your declaration for **main**, include a **try** block, and include the necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="dc3e7-126">Deklarera variablerna av följande typ (beskrivningarna är för hur de används i det här exemplet):</span><span class="sxs-lookup"><span data-stu-id="dc3e7-126">Declare variables of the following type (the descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="dc3e7-127">**CloudStorageAccount**: används för att initiera kontoobjektet med Azure storage-kontonamnet och nyckeln och för att skapa klienten blob-objektet.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-127">**CloudStorageAccount**: Used to initialize the account object with your Azure storage account name and key, and to create the blob client object.</span></span>
* <span data-ttu-id="dc3e7-128">**CloudBlobClient**: används för att få åtkomst till blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-128">**CloudBlobClient**: Used to access the blob service.</span></span>
* <span data-ttu-id="dc3e7-129">**CloudBlobContainer**: används för att skapa en blobbbehållare, visa blobbar i behållaren och ta bort behållaren.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-129">**CloudBlobContainer**: Used to create a blob container, list the blobs in the container, and delete the container.</span></span>
* <span data-ttu-id="dc3e7-130">**CloudBlockBlob**: används för att ladda upp en lokal fil till behållaren.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-130">**CloudBlockBlob**: Used to upload a local image file to the container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="dc3e7-131">Tilldela ett värde till den **konto** variabeln.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-131">Assign a value to the **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="dc3e7-132">Tilldela ett värde till den **serviceClient** variabeln.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-132">Assign a value to the **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="dc3e7-133">Tilldela ett värde till den **behållare** variabeln.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-133">Assign a value to the **container** variable.</span></span> <span data-ttu-id="dc3e7-134">Vi kommer en referens till en behållare med namnet **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-134">We'll get a reference to a container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="dc3e7-135">Skapa behållaren.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-135">Create the container.</span></span> <span data-ttu-id="dc3e7-136">Den här metoden skapar behållaren om den inte finns (och returnera **SANT**).</span><span class="sxs-lookup"><span data-stu-id="dc3e7-136">This method will create the container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="dc3e7-137">Om behållaren finns, returneras **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-137">If the container does exist, it will return **false**.</span></span> <span data-ttu-id="dc3e7-138">Ett alternativ till **createIfNotExists** är den **skapa** metod (som ett fel returneras om det finns redan behållaren).</span><span class="sxs-lookup"><span data-stu-id="dc3e7-138">An alternative to **createIfNotExists** is the **create** method (which will return an error if the container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="dc3e7-139">Ange anonym åtkomst för behållaren.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-139">Set anonymous access for the container.</span></span>

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="dc3e7-140">Hämta en referens till blockblob som representerar blobb i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-140">Get a reference to the block blob, which will represent the blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="dc3e7-141">Använd den **filen** konstruktorn för att hämta en referens till lokalt skapade filen som du överför.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-141">Use the **File** constructor to get a reference to the locally created file that you will upload.</span></span> <span data-ttu-id="dc3e7-142">Se till att du har skapat den här filen innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-142">Ensure you have created this file before running the code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="dc3e7-143">Överför den lokala filen via ett anrop till den **CloudBlockBlob.upload** metod.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-143">Upload the local file through a call to the **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="dc3e7-144">Den första parametern till den **CloudBlockBlob.upload** metoden är en **FileInputStream** objekt som representerar den lokala filen som överförs till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-144">The first parameter to the **CloudBlockBlob.upload** method is a **FileInputStream** object that represents the local file that will be uploaded to Azure storage.</span></span> <span data-ttu-id="dc3e7-145">Den andra parametern är storlek i byte av filen.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-145">The second parameter is the size, in bytes, of the file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="dc3e7-146">Anropa en helper-funktion med namnet **MakeHTMLPage**, för att göra en grundläggande HTML-sida som innehåller en  **&lt;bild&gt;**  elementet med källan inställt på blob som befinner sig nu i ditt Azure storage konto.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-146">Call a helper function named **MakeHTMLPage**, to make a basic HTML page that contains an **&lt;image&gt;** element with the source set to the blob that is now in your Azure storage account.</span></span> <span data-ttu-id="dc3e7-147">Koden för **MakeHTMLPage** kommer att diskuteras senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-147">The code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="dc3e7-148">Skriva ut ett statusmeddelande och information om den skapade HTML-sidan.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-148">Print out a status message and information about the created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

<span data-ttu-id="dc3e7-149">Stäng den **försök** block genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="dc3e7-149">Close the **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dc3e7-150">Hantera följande undantag:</span><span class="sxs-lookup"><span data-stu-id="dc3e7-150">Handle the following exceptions:</span></span>

* <span data-ttu-id="dc3e7-151">**FileNotFoundException**: kan uppkomma av **FileInputStream** eller **FileOutputStream** konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-151">**FileNotFoundException**: Can be thrown by the **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="dc3e7-152">**StorageException**: kan misslyckas på grund av det Azure storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-152">**StorageException**: Can be thrown by the Azure client storage library.</span></span>
* <span data-ttu-id="dc3e7-153">**URISyntaxException**: kan uppkomma av **ListBlobItem.getUri** metod.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-153">**URISyntaxException**: Can be thrown by the **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="dc3e7-154">**Undantag**: Allmänt undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-154">**Exception**: Generic exception handling.</span></span>

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

<span data-ttu-id="dc3e7-155">Stäng **huvudsakliga** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="dc3e7-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dc3e7-156">Skapa en metod med namnet **MakeHTMLPage** att skapa en grundläggande HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-156">Create a method named **MakeHTMLPage** to create a basic HTML page.</span></span> <span data-ttu-id="dc3e7-157">Den här metoden har en parameter av typen **CloudBlobContainer**, som används för att gå igenom listan över överförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-157">This method has a parameter of type **CloudBlobContainer**, which will be used to iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="dc3e7-158">Den här metoden genereras undantag av typen **FileNotFoundException**, som kan uppkomma av **FileOutputStream** konstruktorn och **URISyntaxException**, som kan vara utlöstes av den **ListBlobItem.getUri** metod.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by the **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by the **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="dc3e7-159">Inkludera inledande hakparentes **{**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="dc3e7-160">Skapa en lokal fil med namnet **index.html**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="dc3e7-161">Skriva till den lokala filen, lägga till i den  **&lt;html&gt;**,  **&lt;huvud&gt;**, och  **&lt;brödtext&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-161">Write to the local file, adding in the **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="dc3e7-162">Gå igenom listan över överförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-162">Iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="dc3e7-163">För varje blobb i HTML-sidan Skapa ett  **&lt;img&gt;**  element som har dess **src** attribut som skickas till blob-URI eftersom den finns i ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-163">For each blob, in the HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to the URI of the blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="dc3e7-164">Även om du har lagt till bara en bild i det här exemplet, om du har lagt till fler, skulle den här koden iterera alla.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="dc3e7-165">Det här exemplet förutsätter varje uppladdade blobben är en bild för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="dc3e7-166">Om du har uppdaterat blobbar än bilder eller sidblobbar i stället för blockblobbar, justera koden efter behov.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust the code as needed.</span></span>

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="dc3e7-167">Stäng den  **&lt;brödtext&gt;**  element och  **&lt;html&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-167">Close the **&lt;body&gt;** element and the **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="dc3e7-168">Stäng filen lokalt.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-168">Close the local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="dc3e7-169">Stäng **MakeHTMLPage** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="dc3e7-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dc3e7-170">Stäng **StorageSample** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="dc3e7-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="dc3e7-171">Följande är den fullständiga koden för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-171">The following is the complete code for this example.</span></span> <span data-ttu-id="dc3e7-172">Kom ihåg att ändra platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** att använda ditt kontonamn och kontonyckel, respektive.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-172">Remember to modify the placeholder values **your\_account\_name** and **your\_account\_key** to use your account name and account key, respectively.</span></span>

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

<span data-ttu-id="dc3e7-173">Förutom att ladda upp din lokala image-filen till Azure storage, skapar exempelkoden en lokal fil namedindex.html som du kan öppna i webbläsaren för att se överförda avbildningen.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-173">In addition to uploading your local image file to Azure storage, the example code creates a local file namedindex.html, which you can open in your browser to see your uploaded image.</span></span>

<span data-ttu-id="dc3e7-174">Eftersom koden innehåller kontonamn och kontonyckel, se till att källkoden är säker.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-174">Because the code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="to-delete-a-container"></a><span data-ttu-id="dc3e7-175">Ta bort en behållare</span><span class="sxs-lookup"><span data-stu-id="dc3e7-175">To delete a container</span></span>
<span data-ttu-id="dc3e7-176">Eftersom du debiteras för lagring, kanske du vill ta bort den **gettingstarted** behållare när du är klar experimentera med det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-176">Because you are charged for storage, you may want to delete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="dc3e7-177">Ta bort en behållare genom att använda den **CloudBlobContainer.delete** metod.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-177">To delete a container, use the **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="dc3e7-178">Att anropa den **CloudBlobContainer.delete** metod, initieringen **CloudStorageAccount**, **ClodBlobClient**, och **CloudBlobContainer**  objekt är detsamma som visas för den **createIfNotExist** metod.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-178">To call the **CloudBlobContainer.delete** method, the process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is the same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="dc3e7-179">Följande är en komplett exempel som tar bort behållaren med namnet **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-179">The following is a complete example that deletes the container named **gettingstarted**.</span></span>

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

<span data-ttu-id="dc3e7-180">En översikt över andra blob storage-klasser och metoder, se [använda Blob storage från Java](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="dc3e7-180">For an overview of other blob storage classes and methods, see [How to use Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc3e7-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc3e7-181">Next steps</span></span>
<span data-ttu-id="dc3e7-182">Följa dessa länkar om du vill veta mer om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3e7-182">Follow these links to learn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="dc3e7-183">Azure Storage SDK för Java</span><span class="sxs-lookup"><span data-stu-id="dc3e7-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="dc3e7-184">Referens för Azure Storage Client SDK</span><span class="sxs-lookup"><span data-stu-id="dc3e7-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="dc3e7-185">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="dc3e7-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="dc3e7-186">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="dc3e7-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

