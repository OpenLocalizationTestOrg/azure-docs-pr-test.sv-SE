---
title: "Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur du kommer igång med Azure blob storage i ett ASP.NET-projekt i Visual Studio efter anslutning till ett lagringskonto med hjälp av Visual Studio anslutna Services"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: e953c7978705379a28581213e8f1c665473ddd60
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="0132b-103">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0132b-103">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="0132b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0132b-104">Overview</span></span>

<span data-ttu-id="0132b-105">Azure blob storage är en tjänst som lagrar Ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="0132b-105">Azure blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="0132b-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="0132b-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="0132b-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="0132b-108">Den här kursen visar hur du skriver ASP.NET-kod för några vanliga scenarier med Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="0132b-108">This tutorial shows how to write ASP.NET code for some common scenarios using Azure blob storage.</span></span> <span data-ttu-id="0132b-109">Scenarier som inkluderar en blob-behållare och ladda upp, lista, hämtar och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="0132b-109">Scenarios include creating a blob container, and uploading, listing, downloading, and deleting blobs.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="0132b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0132b-110">Prerequisites</span></span>

* [<span data-ttu-id="0132b-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0132b-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="0132b-112">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="0132b-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="0132b-113">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="0132b-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="0132b-114">I den **Solution Explorer**, högerklicka på **domänkontrollanter**, och på snabbmenyn Välj **Lägg till -> styrenhet**.</span><span class="sxs-lookup"><span data-stu-id="0132b-114">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant i en ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. <span data-ttu-id="0132b-116">På den **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0132b-116">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. <span data-ttu-id="0132b-118">På den **Lägg till styrenhet** dialogrutan namn styrenheten *BlobsController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0132b-118">On the **Add Controller** dialog, name the controller *BlobsController*, and select **Add**.</span></span>

    ![Namnet på MVC-enhet](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. <span data-ttu-id="0132b-120">Lägg till följande *med* direktiven till den `BlobsController.cs` filen:</span><span class="sxs-lookup"><span data-stu-id="0132b-120">Add the following *using* directives to the `BlobsController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a><span data-ttu-id="0132b-121">Skapa en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="0132b-121">Create a blob container</span></span>

<span data-ttu-id="0132b-122">En blob-behållare är en kapslad hierarki med blobbar och mappar.</span><span class="sxs-lookup"><span data-stu-id="0132b-122">A blob container is a nested hierarchy of blobs and folders.</span></span> <span data-ttu-id="0132b-123">Följande steg visar hur du skapar en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="0132b-123">The following steps illustrate how to create a blob container:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="0132b-124">Koden i det här avsnittet förutsätter att du har slutfört stegen i avsnittet [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="0132b-124">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="0132b-125">Öppna filen `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0132b-125">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="0132b-126">Lägg till en metod som kallas **CreateBlobContainer** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="0132b-126">Add a method called **CreateBlobContainer** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="0132b-127">I den **CreateBlobContainer** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-127">Within the **CreateBlobContainer** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="0132b-128">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0132b-128">Use the following code to get the storage connection string and storage account information from the Azure service configuration.</span></span> <span data-ttu-id="0132b-129">(Ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="0132b-129">(Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="0132b-130">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0132b-130">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0132b-131">Hämta en **CloudBlobContainer** objekt som representerar en referens till önskad blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="0132b-131">Get a **CloudBlobContainer** object that represents a reference to the desired blob container name.</span></span> <span data-ttu-id="0132b-132">Den **CloudBlobClient.GetContainerReference** inte gör en begäran mot blob storage.</span><span class="sxs-lookup"><span data-stu-id="0132b-132">The **CloudBlobClient.GetContainerReference** method does not make a request against blob storage.</span></span> <span data-ttu-id="0132b-133">Referensen returneras oavsett blob-behållaren finns.</span><span class="sxs-lookup"><span data-stu-id="0132b-133">The reference is returned whether or not the blob container exists.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="0132b-134">Anropa den **CloudBlobContainer.CreateIfNotExists** metod för att skapa behållaren om den inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="0132b-134">Call the **CloudBlobContainer.CreateIfNotExists** method to create the container if it does not yet exist.</span></span> <span data-ttu-id="0132b-135">Den **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** om behållaren finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="0132b-135">The **CloudBlobContainer.CreateIfNotExists** method returns **true** if the container does not exist, and is successfully created.</span></span> <span data-ttu-id="0132b-136">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="0132b-136">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. <span data-ttu-id="0132b-137">Uppdatering av **ViewBag** med namnet på blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0132b-137">Update the **ViewBag** with the name of the blob container.</span></span>

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. <span data-ttu-id="0132b-138">I den **Solution Explorer**, expandera den **vyer** mapp, högerklickar du på **Blobbar**, och på snabbmenyn Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="0132b-138">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="0132b-139">På den **Lägg till vy** dialogrutan Ange **CreateBlobContainer** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0132b-139">On the **Add View** dialog, enter **CreateBlobContainer** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="0132b-140">Öppna `CreateBlobContainer.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="0132b-140">Open `CreateBlobContainer.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="0132b-141">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0132b-141">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="0132b-142">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="0132b-142">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. <span data-ttu-id="0132b-143">Kör programmet och välj **skapa Blob-behållaren** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="0132b-143">Run the application, and select **Create Blob Container** to see results similar to the following screen shot:</span></span>
  
    ![Skapa blob-behållare](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    <span data-ttu-id="0132b-145">Som nämnts tidigare i **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** endast när behållaren finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="0132b-145">As mentioned previously, the **CloudBlobContainer.CreateIfNotExists** method returns **true** only when the container doesn't exist and is created.</span></span> <span data-ttu-id="0132b-146">Om du kör appen när behållaren finns metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="0132b-146">Therefore, if you run the app when the container exists, the method returns **false**.</span></span> <span data-ttu-id="0132b-147">Om du vill köra appen flera gånger, måste du ta bort behållaren innan du kör appen igen.</span><span class="sxs-lookup"><span data-stu-id="0132b-147">To run the app multiple times, you must delete the container before running the app again.</span></span> <span data-ttu-id="0132b-148">Ta bort behållaren kan göras den **CloudBlobContainer.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-148">Deleting the container can be done via the **CloudBlobContainer.Delete** method.</span></span> <span data-ttu-id="0132b-149">Du kan också ta bort behållaren med hjälp av den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="0132b-149">You can also delete the container using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="upload-a-blob-into-a-blob-container"></a><span data-ttu-id="0132b-150">Ladda upp en blobb till en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0132b-150">Upload a blob into a blob container</span></span>

<span data-ttu-id="0132b-151">När du har [skapas en blob-behållare](#create-a-blob-container), kan du överföra filer till behållaren.</span><span class="sxs-lookup"><span data-stu-id="0132b-151">Once you've [created a blob container](#create-a-blob-container), you can upload files into that container.</span></span> <span data-ttu-id="0132b-152">Det här avsnittet vägleder dig genom att ladda upp en lokal fil till en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="0132b-152">This section walks you through uploading a local file to a blob container.</span></span> <span data-ttu-id="0132b-153">Anvisningarna förutsätter att du har skapat en blobbbehållare med namnet *test blobbehållaren*.</span><span class="sxs-lookup"><span data-stu-id="0132b-153">The steps assume you've created a blob container named *test-blob-container*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="0132b-154">Koden i det här avsnittet förutsätter att du har slutfört stegen i avsnittet [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="0132b-154">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="0132b-155">Öppna filen `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0132b-155">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="0132b-156">Lägg till en metod som kallas **UploadBlob** som returnerar en **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="0132b-156">Add a method called **UploadBlob** that returns an **EmptyResult**.</span></span>

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="0132b-157">I den **UploadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-157">Within the **UploadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="0132b-158">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="0132b-158">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="0132b-159">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0132b-159">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0132b-160">Hämta en **CloudBlobContainer** objekt som representerar en referens till blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="0132b-160">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="0132b-161">Som beskrivits tidigare stöder Azure storage olika blob-typer.</span><span class="sxs-lookup"><span data-stu-id="0132b-161">As explained earlier, Azure storage supports different blob types.</span></span> <span data-ttu-id="0132b-162">Om du vill hämta en referens till en sidblob anropa den **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-162">To retrieve a reference to a page blob, call the **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="0132b-163">Om du vill hämta en referens till en blockblobb anropa den **CloudBlobContainer.GetBlockBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-163">To retrieve a reference to a block blob, call the **CloudBlobContainer.GetBlockBlobReference** method.</span></span> <span data-ttu-id="0132b-164">Blockblob är vanligtvis den rekommenderade typen.</span><span class="sxs-lookup"><span data-stu-id="0132b-164">Usually, block blob is the recommended type to use.</span></span> <span data-ttu-id="0132b-165">(Ändra < blob-namn > * namnet som du vill ge blob överförs en gång.)</span><span class="sxs-lookup"><span data-stu-id="0132b-165">(Change <blob-name>* to the name you want to give the blob once uploaded.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="0132b-166">När du har en blobbreferens kan du överföra alla dataström till den genom att anropa blob-referensobjekt **UploadFromStream** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-166">Once you have a blob reference, you can upload any data stream to it by calling the blob reference object's **UploadFromStream** method.</span></span> <span data-ttu-id="0132b-167">Den **UploadFromStream** metoden skapas blobben om den finns inte eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="0132b-167">The **UploadFromStream** method creates the blob if it doesn't exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="0132b-168">(Ändra  *&lt;till filuppladdning >* till en fullständigt kvalificerad sökväg till filen som du vill överföra.)</span><span class="sxs-lookup"><span data-stu-id="0132b-168">(Change *&lt;file-to-upload>* to a fully qualified path to the file you want to upload.)</span></span>

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. <span data-ttu-id="0132b-169">I den **Solution Explorer**, expandera den **vyer** mapp, högerklickar du på **Blobbar**, och på snabbmenyn Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="0132b-169">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="0132b-170">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0132b-170">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="0132b-171">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="0132b-171">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="0132b-172">Kör programmet och välj **överför blob**.</span><span class="sxs-lookup"><span data-stu-id="0132b-172">Run the application, and select **Upload blob**.</span></span>  
  
<span data-ttu-id="0132b-173">Avsnittet - [visa blobbar i en blob-behållare](#list-the-blobs-in-a-blob-container) -visar hur du kan visa blobbar i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="0132b-173">The section - [List the blobs in a blob container](#list-the-blobs-in-a-blob-container) - illustrates how to list the blobs in a blob container.</span></span>    

## <a name="list-the-blobs-in-a-blob-container"></a><span data-ttu-id="0132b-174">Visa blobbar i en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0132b-174">List the blobs in a blob container</span></span>

<span data-ttu-id="0132b-175">Det här avsnittet visar hur du kan visa blobbar i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="0132b-175">This section illustrates how to list the blobs in a blob container.</span></span> <span data-ttu-id="0132b-176">Exempel koden refererar till den *test blobbehållaren* skapade i avsnittet [skapa en blobbbehållare](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="0132b-176">The sample code references the *test-blob-container* created in the section, [Create a blob container](#create-a-blob-container).</span></span>

> [!NOTE]
> 
> <span data-ttu-id="0132b-177">Koden i det här avsnittet förutsätter att du har slutfört stegen i avsnittet [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="0132b-177">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="0132b-178">Öppna filen `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0132b-178">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="0132b-179">Lägg till en metod som kallas **ListBlobs** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="0132b-179">Add a method called **ListBlobs** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="0132b-180">I den **ListBlobs** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-180">Within the **ListBlobs** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="0132b-181">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="0132b-181">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="0132b-182">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0132b-182">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0132b-183">Hämta en **CloudBlobContainer** objekt som representerar en referens till blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="0132b-183">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="0132b-184">Om du vill visa blobbar i en blob-behållare, Använd den **CloudBlobContainer.ListBlobs** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-184">To list the blobs in a blob container, use the **CloudBlobContainer.ListBlobs** method.</span></span> <span data-ttu-id="0132b-185">Den **CloudBlobContainer.ListBlobs** metoden returnerar en **IListBlobItem** objekt som du omvandlas till en **CloudBlockBlob**, **CloudPageBlob**, eller **CloudBlobDirectory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0132b-185">The **CloudBlobContainer.ListBlobs** method returns an **IListBlobItem** object that you cast to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="0132b-186">Följande kodstycke räknar upp alla blobbar i en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="0132b-186">The following code snippet enumerates all the blobs in a blob container.</span></span> <span data-ttu-id="0132b-187">Varje blobb omvandlas till lämpligt objekt baserat på dess typ och dess namn (eller URI i händelse av en **CloudBlobDirectory**) har lagts till i en lista.</span><span class="sxs-lookup"><span data-stu-id="0132b-187">Each blob is cast to the appropriate object based on its type, and its name (or URI in the case of a **CloudBlobDirectory**) is added to a list.</span></span>

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    <span data-ttu-id="0132b-188">Förutom blobbar, kan blob-behållare innehålla kataloger.</span><span class="sxs-lookup"><span data-stu-id="0132b-188">In addition to blobs, blob containers can contain directories.</span></span> <span data-ttu-id="0132b-189">Anta att du har en blobbbehållare som kallas *test blobbehållaren* med följande hierarki:</span><span class="sxs-lookup"><span data-stu-id="0132b-189">Let's suppose you have a blob container called *test-blob-container* with the following hierarchy:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

    <span data-ttu-id="0132b-190">Med hjälp av föregående kodexempel i **blobbar** stränglista innehåller värden som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="0132b-190">Using the preceding code example, the **blobs** string list contains values similar to the following:</span></span>

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    <span data-ttu-id="0132b-191">Som du kan se innehåller listan endast de översta entiteterna; inte de kapslade (*bar.png* och *baz.png*).</span><span class="sxs-lookup"><span data-stu-id="0132b-191">As you can see, the list includes only the top-level entities; not the nested ones (*bar.png* and *baz.png*).</span></span> <span data-ttu-id="0132b-192">Om du vill visa en lista över alla enheter i en blob-behållare, måste du anropa den **CloudBlobContainer.ListBlobs** metod och pass **SANT** för den **useFlatBlobListing** parameter.</span><span class="sxs-lookup"><span data-stu-id="0132b-192">To list all the entities within a blob container, you must call the **CloudBlobContainer.ListBlobs** method and pass **true** for the **useFlatBlobListing** parameter.</span></span>    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    <span data-ttu-id="0132b-193">Ange den **useFlatBlobListing** parameter till **SANT** returnerar en platt lista över alla entiteter i blob-behållaren och ger följande resultat:</span><span class="sxs-lookup"><span data-stu-id="0132b-193">Setting the **useFlatBlobListing** parameter to **true** returns a flat listing of all entities in the blob container, and yields the following results:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

1. <span data-ttu-id="0132b-194">I den **Solution Explorer**, expandera den **vyer** mapp, högerklickar du på **Blobbar**, och på snabbmenyn Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="0132b-194">In the **Solution Explorer**, expand the **Views** folder, right-click **Blobs**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="0132b-195">På den **Lägg till vy** dialogrutan Ange **ListBlobs** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0132b-195">On the **Add View** dialog, enter **ListBlobs** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="0132b-196">Öppna `ListBlobs.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="0132b-196">Open `ListBlobs.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. <span data-ttu-id="0132b-197">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0132b-197">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="0132b-198">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="0132b-198">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. <span data-ttu-id="0132b-199">Kör programmet och välj **visa blobbar** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="0132b-199">Run the application, and select **List blobs** to see results similar to the following screen shot:</span></span>
  
    ![BLOB-lista](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a><span data-ttu-id="0132b-201">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="0132b-201">Download blobs</span></span>

<span data-ttu-id="0132b-202">Det här avsnittet beskriver hur du hämtar en blob och antingen spara den lokal lagring eller läsa in innehållet i en sträng.</span><span class="sxs-lookup"><span data-stu-id="0132b-202">This section illustrates how to download a blob and either persist it to local storage or read the contents into a string.</span></span> <span data-ttu-id="0132b-203">Exempel koden refererar till den *test blobbehållaren* skapade i avsnittet [skapa en blobbbehållare](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="0132b-203">The sample code references the *test-blob-container* created in the section, [Create a blob container](#create-a-blob-container).</span></span>

1. <span data-ttu-id="0132b-204">Öppna filen `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0132b-204">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="0132b-205">Lägg till en metod som kallas **DownloadBlob** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="0132b-205">Add a method called **DownloadBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="0132b-206">I den **DownloadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-206">Within the **DownloadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="0132b-207">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="0132b-207">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="0132b-208">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0132b-208">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0132b-209">Hämta en **CloudBlobContainer** objekt som representerar en referens till blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="0132b-209">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="0132b-210">Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-210">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="0132b-211">(Ändra  *&lt;blob-name >* hämtar namnet på blob.)</span><span class="sxs-lookup"><span data-stu-id="0132b-211">(Change *&lt;blob-name>* to the name of the blob you are downloading.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="0132b-212">Du kan hämta en blob den **CloudBlockBlob.DownloadToStream** eller **CloudPageBlob.DownloadToStream** metod beroende på blob-datatyp.</span><span class="sxs-lookup"><span data-stu-id="0132b-212">To download a blob, use the **CloudBlockBlob.DownloadToStream** or **CloudPageBlob.DownloadToStream** method, depending on the blob type.</span></span> <span data-ttu-id="0132b-213">I följande kod fragment används den **CloudBlockBlob.DownloadToStream** metod för att överföra en blob-innehåll till en stream-objektet som sparas sedan till en lokal fil: (ändra  *&lt;lokala filnamn >* till filen fullständigt kvalificerade namnet som representerar där du vill att blob hämtas.)</span><span class="sxs-lookup"><span data-stu-id="0132b-213">The following code snippet uses the **CloudBlockBlob.DownloadToStream** method to transfer a blob's contents to a stream object that is then persisted to a local file: (Change *&lt;local-file-name>* to the fully qualified file name representing where you want the blob downloaded.)</span></span> 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. <span data-ttu-id="0132b-214">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0132b-214">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="0132b-215">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="0132b-215">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="0132b-216">Kör programmet och välj **Download blob** att hämta blobben.</span><span class="sxs-lookup"><span data-stu-id="0132b-216">Run the application, and select **Download blob** to download the blob.</span></span> <span data-ttu-id="0132b-217">Blob som anges i den **CloudBlobContainer.GetBlockBlobReference** metodanrop filhämtningar till den plats du anger i den **File.OpenWrite** metodanrop.</span><span class="sxs-lookup"><span data-stu-id="0132b-217">The blob specified in the **CloudBlobContainer.GetBlockBlobReference** method call downloads to the location you specify in the **File.OpenWrite** method call.</span></span> 

## <a name="delete-blobs"></a><span data-ttu-id="0132b-218">Ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="0132b-218">Delete blobs</span></span>

<span data-ttu-id="0132b-219">Följande steg visar hur du tar bort en blobb:</span><span class="sxs-lookup"><span data-stu-id="0132b-219">The following steps illustrate how to delete a blob:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="0132b-220">Koden i det här avsnittet förutsätter att du har slutfört stegen i avsnittet [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="0132b-220">The code in this section assumes that you have completed the steps in the section, [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="0132b-221">Öppna filen `BlobsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="0132b-221">Open the `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="0132b-222">Lägg till en metod som kallas **DeleteBlob** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="0132b-222">Add a method called **DeleteBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. <span data-ttu-id="0132b-223">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="0132b-223">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="0132b-224">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="0132b-224">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="0132b-225">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0132b-225">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="0132b-226">Hämta en **CloudBlobContainer** objekt som representerar en referens till blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="0132b-226">Get a **CloudBlobContainer** object that represents a reference to the blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="0132b-227">Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="0132b-227">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="0132b-228">(Ändra  *&lt;blob-name >* till namnet på blob som tas bort.)</span><span class="sxs-lookup"><span data-stu-id="0132b-228">(Change *&lt;blob-name>* to the name of the blob you are deleting.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. <span data-ttu-id="0132b-229">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="0132b-229">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="0132b-230">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="0132b-230">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="0132b-231">Kör programmet och välj **ta bort blob** att ta bort blobben som anges i den **CloudBlobContainer.GetBlockBlobReference** metodanrop.</span><span class="sxs-lookup"><span data-stu-id="0132b-231">Run the application, and select **Delete blob** to delete the blob specified in the **CloudBlobContainer.GetBlockBlobReference** method call.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0132b-232">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0132b-232">Next steps</span></span>
<span data-ttu-id="0132b-233">Visa fler funktionsguider och lär dig mer om andra alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="0132b-233">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="0132b-234">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0132b-234">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-tables.md)
  * [<span data-ttu-id="0132b-235">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="0132b-235">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-queues.md)
