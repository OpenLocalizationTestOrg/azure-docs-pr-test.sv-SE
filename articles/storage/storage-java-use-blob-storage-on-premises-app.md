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
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="c52d9-104">Lokalt program med blob storage</span><span class="sxs-lookup"><span data-stu-id="c52d9-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="c52d9-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="c52d9-105">Overview</span></span>
<span data-ttu-id="c52d9-106">hello följande exempel visar hur du kan använda Azure storage för att lagra avbildningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="c52d9-106">hello following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="c52d9-107">hello är i den här artikeln för ett konsolprogram som överför en bild tooAzure och sedan skapar en HTML-fil som visar hello bilden i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c52d9-107">hello code in this article is for a console application that uploads an image tooAzure, and then creates an HTML file that displays hello image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c52d9-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c52d9-108">Prerequisites</span></span>
* <span data-ttu-id="c52d9-109">En Java Developer Kit (JDK), version 1.6 eller senare har installerats.</span><span class="sxs-lookup"><span data-stu-id="c52d9-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="c52d9-110">hello Azure SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="c52d9-110">hello Azure SDK is installed.</span></span>
* <span data-ttu-id="c52d9-111">hello JAR för hello Azure-biblioteken för Java och alla tillämpliga beroende burkar har installerats och är i hello byggsökväg som används av Java-kompilatorn.</span><span class="sxs-lookup"><span data-stu-id="c52d9-111">hello JAR for hello Azure Libraries for Java, and any applicable dependency JARs, are installed and are in hello build path used by your Java compiler.</span></span> <span data-ttu-id="c52d9-112">Information om hur du installerar hello Azure-biblioteken för Java finns [Download hello Azure SDK för Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c52d9-112">For information about installing hello Azure Libraries for Java, see [Download hello Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="c52d9-113">Ett Azure storage-konto har ställts in.</span><span class="sxs-lookup"><span data-stu-id="c52d9-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="c52d9-114">hello används kontonamn och din kontonyckel för hello storage-konto av hello kod i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c52d9-114">hello account name and account key for hello storage account will be used by hello code in this article.</span></span> <span data-ttu-id="c52d9-115">Se [hur tooCreate ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account) information om hur du skapar ett lagringskonto och [visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys) information om hur du hämtar hello kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="c52d9-115">See [How tooCreate a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving hello account key.</span></span>
* <span data-ttu-id="c52d9-116">Du har skapat en lokal fil med namnet på hello sökvägen c:\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="c52d9-116">You have created a local image file named stored at hello path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="c52d9-117">Du kan också ändra den **FileInputStream** konstruktor i hello exempel toouse en annan bild sökvägen och filnamnet.</span><span class="sxs-lookup"><span data-stu-id="c52d9-117">Alternatively, modify the **FileInputStream** constructor in hello example toouse a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a><span data-ttu-id="c52d9-118">toouse Azure blob storage tooupload en fil</span><span class="sxs-lookup"><span data-stu-id="c52d9-118">toouse Azure blob storage tooupload a file</span></span>
<span data-ttu-id="c52d9-119">Stegvisa anvisningar visas här.</span><span class="sxs-lookup"><span data-stu-id="c52d9-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="c52d9-120">Om du vill tooskip vidare presenteras hello hela koden nedan.</span><span class="sxs-lookup"><span data-stu-id="c52d9-120">If you'd like tooskip ahead, hello entire code is presented later in this article.</span></span>

<span data-ttu-id="c52d9-121">Börja hello kod genom att inkludera importer för hello Azure storage-kärnklasser, hello Azure blob-klientklasser, hello Java-i/o-klasser och hello **URISyntaxException** klass.</span><span class="sxs-lookup"><span data-stu-id="c52d9-121">Begin hello code by including imports for hello Azure core storage classes, hello Azure blob client classes, hello Java IO classes, and hello **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="c52d9-122">Deklarera en klass som heter **StorageSample**, och inkludera hello inledande, **{**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-122">Declare a class named **StorageSample**, and include hello open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="c52d9-123">Inom hello **StorageSample** klassen, deklarera en string-variabel som innehåller hello standardprotokoll för slutpunkten, namnet på ditt lagringskonto och din lagringsåtkomstnyckel som anges i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c52d9-123">Within hello **StorageSample** class, declare a string variable that will contain hello default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="c52d9-124">Ersätt hello platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** med dina egna namn och kontonyckel, respektive.</span><span class="sxs-lookup"><span data-stu-id="c52d9-124">Replace hello placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="c52d9-125">Lägga till i en deklaration för **huvudsakliga**, innehåller en **försök** blockera och inkludera hello behövs öppen hakparenteserna, **{**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-125">Add in your declaration for **main**, include a **try** block, and include hello necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="c52d9-126">Deklarera variabler av hello efter typ (hello beskrivningarna är för hur de används i det här exemplet):</span><span class="sxs-lookup"><span data-stu-id="c52d9-126">Declare variables of hello following type (hello descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="c52d9-127">**CloudStorageAccount**: använda tooinitialize hello-kontot med Azure storage-kontonamnet och nyckeln och toocreate klienten blob-objektet.</span><span class="sxs-lookup"><span data-stu-id="c52d9-127">**CloudStorageAccount**: Used tooinitialize hello account object with your Azure storage account name and key, and toocreate the blob client object.</span></span>
* <span data-ttu-id="c52d9-128">**CloudBlobClient**: används tooaccess hello blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c52d9-128">**CloudBlobClient**: Used tooaccess hello blob service.</span></span>
* <span data-ttu-id="c52d9-129">**CloudBlobContainer**: använda toocreate blob-behållaren visa blobbar i hello-behållaren och ta bort hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c52d9-129">**CloudBlobContainer**: Used toocreate a blob container, list the blobs in hello container, and delete hello container.</span></span>
* <span data-ttu-id="c52d9-130">**CloudBlockBlob**: använda tooupload en lokal image-filen toothe behållare.</span><span class="sxs-lookup"><span data-stu-id="c52d9-130">**CloudBlockBlob**: Used tooupload a local image file toothe container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="c52d9-131">Tilldela ett värde toohello **konto** variabeln.</span><span class="sxs-lookup"><span data-stu-id="c52d9-131">Assign a value toohello **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="c52d9-132">Tilldela ett värde toohello **serviceClient** variabeln.</span><span class="sxs-lookup"><span data-stu-id="c52d9-132">Assign a value toohello **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="c52d9-133">Tilldela ett värde toohello **behållare** variabeln.</span><span class="sxs-lookup"><span data-stu-id="c52d9-133">Assign a value toohello **container** variable.</span></span> <span data-ttu-id="c52d9-134">Vi kommer en referens tooa behållare med namnet **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-134">We'll get a reference tooa container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="c52d9-135">Skapa hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c52d9-135">Create hello container.</span></span> <span data-ttu-id="c52d9-136">Den här metoden skapar hello behållaren om den inte finns (och returnera **SANT**).</span><span class="sxs-lookup"><span data-stu-id="c52d9-136">This method will create hello container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="c52d9-137">Om hello behållaren finns, returneras **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-137">If hello container does exist, it will return **false**.</span></span> <span data-ttu-id="c52d9-138">Ett alternativ för**createIfNotExists** är hello **skapa** metod (som ett fel returneras om hello-behållaren finns redan).</span><span class="sxs-lookup"><span data-stu-id="c52d9-138">An alternative too**createIfNotExists** is hello **create** method (which will return an error if hello container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="c52d9-139">Ange anonym åtkomst för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c52d9-139">Set anonymous access for hello container.</span></span>

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="c52d9-140">Hämta en referens toohello blockblob, som representerar hello blobb i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="c52d9-140">Get a reference toohello block blob, which will represent hello blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="c52d9-141">Använd hello **filen** konstruktorn tooget en referens toohello lokalt skapade-fil som du överför.</span><span class="sxs-lookup"><span data-stu-id="c52d9-141">Use hello **File** constructor tooget a reference toohello locally created file that you will upload.</span></span> <span data-ttu-id="c52d9-142">Se till att du har skapat den här filen innan du kör hello kod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-142">Ensure you have created this file before running hello code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="c52d9-143">Ladda upp hello lokala filen via ett anrop toohello **CloudBlockBlob.upload** metod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-143">Upload hello local file through a call toohello **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="c52d9-144">Hej den första parametern toohello **CloudBlockBlob.upload** metoden är en **FileInputStream** objekt som representerar hello lokal fil som ska överföras tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="c52d9-144">hello first parameter toohello **CloudBlockBlob.upload** method is a **FileInputStream** object that represents hello local file that will be uploaded tooAzure storage.</span></span> <span data-ttu-id="c52d9-145">hello andra parametern är hello storlek i byte av hello-fil.</span><span class="sxs-lookup"><span data-stu-id="c52d9-145">hello second parameter is hello size, in bytes, of hello file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="c52d9-146">Anropa en helper-funktion med namnet **MakeHTMLPage**, toomake grundläggande HTML-sida som innehåller en  **&lt;bild&gt;**  element med hello källa set toohello blob som är nu i din Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c52d9-146">Call a helper function named **MakeHTMLPage**, toomake a basic HTML page that contains an **&lt;image&gt;** element with hello source set toohello blob that is now in your Azure storage account.</span></span> <span data-ttu-id="c52d9-147">Hej koden för **MakeHTMLPage** kommer att diskuteras senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c52d9-147">hello code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="c52d9-148">Skriva ut ett statusmeddelande och information om hello skapa HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="c52d9-148">Print out a status message and information about hello created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

<span data-ttu-id="c52d9-149">Stäng hello **försök** block genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="c52d9-149">Close hello **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="c52d9-150">Hantera hello följande undantag:</span><span class="sxs-lookup"><span data-stu-id="c52d9-150">Handle hello following exceptions:</span></span>

* <span data-ttu-id="c52d9-151">**FileNotFoundException**: kan uppkomma hello **FileInputStream** eller **FileOutputStream** konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="c52d9-151">**FileNotFoundException**: Can be thrown by hello **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="c52d9-152">**StorageException**: kan uppkomma hello Azure storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="c52d9-152">**StorageException**: Can be thrown by hello Azure client storage library.</span></span>
* <span data-ttu-id="c52d9-153">**URISyntaxException**: kan uppkomma hello **ListBlobItem.getUri** metod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-153">**URISyntaxException**: Can be thrown by hello **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="c52d9-154">**Undantag**: Allmänt undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="c52d9-154">**Exception**: Generic exception handling.</span></span>

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

<span data-ttu-id="c52d9-155">Stäng **huvudsakliga** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="c52d9-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="c52d9-156">Skapa en metod med namnet **MakeHTMLPage** toocreate grundläggande HTML-sidan.</span><span class="sxs-lookup"><span data-stu-id="c52d9-156">Create a method named **MakeHTMLPage** toocreate a basic HTML page.</span></span> <span data-ttu-id="c52d9-157">Den här metoden har en parameter av typen **CloudBlobContainer**, som kommer att använda tooiterate via hello lista över överförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="c52d9-157">This method has a parameter of type **CloudBlobContainer**, which will be used tooiterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="c52d9-158">Den här metoden genereras undantag av typen **FileNotFoundException**, vilket kan uppkomma hello **FileOutputStream** konstruktorn och **URISyntaxException**, vilket kan uppkomma hello **ListBlobItem.getUri** metod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by hello **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by hello **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="c52d9-159">Inkludera inledande hakparentes **{**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="c52d9-160">Skapa en lokal fil med namnet **index.html**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="c52d9-161">Skriva toohello lokal fil, lägger till i hello  **&lt;html&gt;**,  **&lt;huvud&gt;**, och  **&lt;brödtext&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="c52d9-161">Write toohello local file, adding in hello **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="c52d9-162">Gå igenom hello lista över överförda blobbar.</span><span class="sxs-lookup"><span data-stu-id="c52d9-162">Iterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="c52d9-163">För varje blob hello HTML-sidan Skapa i en  **&lt;img&gt;**  element som har dess **src** attribut som skickas till hello hello blob-URI eftersom den finns i ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c52d9-163">For each blob, in hello HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to hello URI of hello blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="c52d9-164">Även om du har lagt till bara en bild i det här exemplet, om du har lagt till fler, skulle den här koden iterera alla.</span><span class="sxs-lookup"><span data-stu-id="c52d9-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="c52d9-165">Det här exemplet förutsätter varje uppladdade blobben är en bild för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="c52d9-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="c52d9-166">Om du har uppdaterat blobbar än bilder eller sidblobbar i stället för blockblobbar, justera hello kod efter behov.</span><span class="sxs-lookup"><span data-stu-id="c52d9-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust hello code as needed.</span></span>

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="c52d9-167">Stäng hello  **&lt;brödtext&gt;**  element och hello  **&lt;html&gt;**  element.</span><span class="sxs-lookup"><span data-stu-id="c52d9-167">Close hello **&lt;body&gt;** element and hello **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="c52d9-168">Stäng hello lokal fil.</span><span class="sxs-lookup"><span data-stu-id="c52d9-168">Close hello local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="c52d9-169">Stäng **MakeHTMLPage** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="c52d9-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="c52d9-170">Stäng **StorageSample** genom att infoga en Stäng hakparentes: **}**</span><span class="sxs-lookup"><span data-stu-id="c52d9-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="c52d9-171">hello följer hello fullständiga koden för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c52d9-171">hello following is hello complete code for this example.</span></span> <span data-ttu-id="c52d9-172">Kom ihåg toomodify hello platshållarvärdena **din\_konto\_namn** och **din\_konto\_nyckeln** toouse ditt kontonamn och -konto nyckel, respektive.</span><span class="sxs-lookup"><span data-stu-id="c52d9-172">Remember toomodify hello placeholder values **your\_account\_name** and **your\_account\_key** toouse your account name and account key, respectively.</span></span>

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

<span data-ttu-id="c52d9-173">I tillägg toouploading lagringsenheterna lokal image-filen tooAzure hello exempelkod skapar en lokal fil namedindex.html som du kan öppna i din webbläsare toosee överförda avbildningen.</span><span class="sxs-lookup"><span data-stu-id="c52d9-173">In addition toouploading your local image file tooAzure storage, hello example code creates a local file namedindex.html, which you can open in your browser toosee your uploaded image.</span></span>

<span data-ttu-id="c52d9-174">Eftersom hello kod innehåller kontonamn och kontonyckel, se till att källkoden är säker.</span><span class="sxs-lookup"><span data-stu-id="c52d9-174">Because hello code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="toodelete-a-container"></a><span data-ttu-id="c52d9-175">toodelete en behållare</span><span class="sxs-lookup"><span data-stu-id="c52d9-175">toodelete a container</span></span>
<span data-ttu-id="c52d9-176">Eftersom debiteras du för lagring av du toodelete den **gettingstarted** behållare när du är klar experimentera med det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c52d9-176">Because you are charged for storage, you may want toodelete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="c52d9-177">toodelete en behållare använder hello **CloudBlobContainer.delete** metod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-177">toodelete a container, use hello **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="c52d9-178">toocall hello **CloudBlobContainer.delete** metod, hello processen för att initiera **CloudStorageAccount**, **ClodBlobClient**, och  **CloudBlobContainer** objekt är hello samma värde som visas för den **createIfNotExist** metod.</span><span class="sxs-lookup"><span data-stu-id="c52d9-178">toocall hello **CloudBlobContainer.delete** method, hello process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is hello same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="c52d9-179">hello följande är en komplett exempel som tar bort hello behållare med namnet **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="c52d9-179">hello following is a complete example that deletes hello container named **gettingstarted**.</span></span>

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

<span data-ttu-id="c52d9-180">En översikt över andra blob storage-klasser och metoder, se [hur toouse Blob storage från Java](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c52d9-180">For an overview of other blob storage classes and methods, see [How toouse Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c52d9-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c52d9-181">Next steps</span></span>
<span data-ttu-id="c52d9-182">Följ dessa länkar toolearn mer om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c52d9-182">Follow these links toolearn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="c52d9-183">Azure Storage SDK för Java</span><span class="sxs-lookup"><span data-stu-id="c52d9-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="c52d9-184">Referens för Azure Storage Client SDK</span><span class="sxs-lookup"><span data-stu-id="c52d9-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="c52d9-185">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="c52d9-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="c52d9-186">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="c52d9-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

