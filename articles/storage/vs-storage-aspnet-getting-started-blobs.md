---
title: "aaaGet igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur tooget igång med Azure blob-lagring i en ASP.NET-projekt i Visual Studio när du har anslutit tooa lagringskonto med hjälp av Visual Studio anslutna Services"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: tarcher
ms.openlocfilehash: eb38889f239a63852d6928e8be10c3d3f1746e9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="b258a-103">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="b258a-103">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="b258a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b258a-104">Overview</span></span>

<span data-ttu-id="b258a-105">Azure blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="b258a-105">Azure blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="b258a-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="b258a-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="b258a-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="b258a-108">Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="b258a-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure blob storage.</span></span> <span data-ttu-id="b258a-109">Scenarier som inkluderar en blob-behållare och ladda upp, lista, hämtar och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="b258a-109">Scenarios include creating a blob container, and uploading, listing, downloading, and deleting blobs.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="b258a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b258a-110">Prerequisites</span></span>

* [<span data-ttu-id="b258a-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b258a-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="b258a-112">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="b258a-112">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="b258a-113">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="b258a-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="b258a-114">I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="b258a-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. <span data-ttu-id="b258a-116">På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b258a-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. <span data-ttu-id="b258a-118">På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *BlobsController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b258a-118">On hello **Add Controller** dialog, name hello controller *BlobsController*, and select **Add**.</span></span>

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. <span data-ttu-id="b258a-120">Lägg till följande hello *med* direktiven toohello `BlobsController.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="b258a-120">Add hello following *using* directives toohello `BlobsController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a><span data-ttu-id="b258a-121">Skapa en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="b258a-121">Create a blob container</span></span>

<span data-ttu-id="b258a-122">En blob-behållare är en kapslad hierarki med blobbar och mappar.</span><span class="sxs-lookup"><span data-stu-id="b258a-122">A blob container is a nested hierarchy of blobs and folders.</span></span> <span data-ttu-id="b258a-123">hello följande steg visar hur toocreate en blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="b258a-123">hello following steps illustrate how toocreate a blob container:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b258a-124">hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="b258a-124">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="b258a-125">Öppna hello `BlobsController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="b258a-125">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="b258a-126">Lägg till en metod som kallas **CreateBlobContainer** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="b258a-126">Add a method called **CreateBlobContainer** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="b258a-127">Inom hello **CreateBlobContainer** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-127">Within hello **CreateBlobContainer** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="b258a-128">Använd följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="b258a-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span> <span data-ttu-id="b258a-129">(Ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage-konto som du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="b258a-129">(Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="b258a-130">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b258a-130">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="b258a-131">Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello önskade blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="b258a-131">Get a **CloudBlobContainer** object that represents a reference toohello desired blob container name.</span></span> <span data-ttu-id="b258a-132">Hej **CloudBlobClient.GetContainerReference** inte gör en begäran mot blob storage.</span><span class="sxs-lookup"><span data-stu-id="b258a-132">hello **CloudBlobClient.GetContainerReference** method does not make a request against blob storage.</span></span> <span data-ttu-id="b258a-133">hello referens returneras oavsett hello blob-behållaren finns.</span><span class="sxs-lookup"><span data-stu-id="b258a-133">hello reference is returned whether or not hello blob container exists.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="b258a-134">Anropa hello **CloudBlobContainer.CreateIfNotExists** metoden toocreate hello behållaren om den inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="b258a-134">Call hello **CloudBlobContainer.CreateIfNotExists** method toocreate hello container if it does not yet exist.</span></span> <span data-ttu-id="b258a-135">Hej **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** om hello behållaren finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="b258a-135">hello **CloudBlobContainer.CreateIfNotExists** method returns **true** if hello container does not exist, and is successfully created.</span></span> <span data-ttu-id="b258a-136">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="b258a-136">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. <span data-ttu-id="b258a-137">Uppdatera hello **ViewBag** med hello namnet hello blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b258a-137">Update hello **ViewBag** with hello name of hello blob container.</span></span>

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. <span data-ttu-id="b258a-138">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="b258a-138">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="b258a-139">På hello **Lägg till vy** dialogrutan Ange **CreateBlobContainer** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b258a-139">On hello **Add View** dialog, enter **CreateBlobContainer** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="b258a-140">Öppna `CreateBlobContainer.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="b258a-140">Open `CreateBlobContainer.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="b258a-141">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b258a-141">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="b258a-142">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="b258a-142">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. <span data-ttu-id="b258a-143">Kör hello programmet och välj **skapa Blob-behållaren** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="b258a-143">Run hello application, and select **Create Blob Container** toosee results similar toohello following screen shot:</span></span>
  
    ![Skapa blob-behållare](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    <span data-ttu-id="b258a-145">Som tidigare nämnts hello **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** endast när hello behållaren finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="b258a-145">As mentioned previously, hello **CloudBlobContainer.CreateIfNotExists** method returns **true** only when hello container doesn't exist and is created.</span></span> <span data-ttu-id="b258a-146">Om du kör hello app när hello-behållaren finns hello-metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="b258a-146">Therefore, if you run hello app when hello container exists, hello method returns **false**.</span></span> <span data-ttu-id="b258a-147">toorun hello app flera gånger, måste du ta bort hello behållare innan du kör hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="b258a-147">toorun hello app multiple times, you must delete hello container before running hello app again.</span></span> <span data-ttu-id="b258a-148">Ta bort hello behållare kan göras via hello **CloudBlobContainer.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-148">Deleting hello container can be done via hello **CloudBlobContainer.Delete** method.</span></span> <span data-ttu-id="b258a-149">Du kan också ta bort hello behållaren med hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="b258a-149">You can also delete hello container using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="upload-a-blob-into-a-blob-container"></a><span data-ttu-id="b258a-150">Ladda upp en blobb till en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="b258a-150">Upload a blob into a blob container</span></span>

<span data-ttu-id="b258a-151">När du har [skapas en blob-behållare](#create-a-blob-container), kan du överföra filer till behållaren.</span><span class="sxs-lookup"><span data-stu-id="b258a-151">Once you've [created a blob container](#create-a-blob-container), you can upload files into that container.</span></span> <span data-ttu-id="b258a-152">Det här avsnittet vägleder dig genom att ladda upp en lokal fil tooa blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="b258a-152">This section walks you through uploading a local file tooa blob container.</span></span> <span data-ttu-id="b258a-153">hello stegen förutsätter att du har skapat en blobbbehållare med namnet *test blobbehållaren*.</span><span class="sxs-lookup"><span data-stu-id="b258a-153">hello steps assume you've created a blob container named *test-blob-container*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="b258a-154">hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="b258a-154">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="b258a-155">Öppna hello `BlobsController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="b258a-155">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="b258a-156">Lägg till en metod som kallas **UploadBlob** som returnerar en **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="b258a-156">Add a method called **UploadBlob** that returns an **EmptyResult**.</span></span>

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="b258a-157">Inom hello **UploadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-157">Within hello **UploadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="b258a-158">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="b258a-158">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="b258a-159">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b258a-159">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="b258a-160">Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="b258a-160">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="b258a-161">Som beskrivits tidigare stöder Azure storage olika blob-typer.</span><span class="sxs-lookup"><span data-stu-id="b258a-161">As explained earlier, Azure storage supports different blob types.</span></span> <span data-ttu-id="b258a-162">tooretrieve referens tooa sidan blob anropet hello **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-162">tooretrieve a reference tooa page blob, call hello **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="b258a-163">tooretrieve referens tooa blockblob anropet hello **CloudBlobContainer.GetBlockBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-163">tooretrieve a reference tooa block blob, call hello **CloudBlobContainer.GetBlockBlobReference** method.</span></span> <span data-ttu-id="b258a-164">Blockblob är vanligtvis hello rekommenderade typen toouse.</span><span class="sxs-lookup"><span data-stu-id="b258a-164">Usually, block blob is hello recommended type toouse.</span></span> <span data-ttu-id="b258a-165">(Ändra < blob-namn > * toohello namn som du vill toogive hello blob överförs en gång.)</span><span class="sxs-lookup"><span data-stu-id="b258a-165">(Change <blob-name>* toohello name you want toogive hello blob once uploaded.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="b258a-166">När du har en blobbreferens kan du överföra alla data dataströmmen tooit genom att anropa hello blob referens objektets **UploadFromStream** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-166">Once you have a blob reference, you can upload any data stream tooit by calling hello blob reference object's **UploadFromStream** method.</span></span> <span data-ttu-id="b258a-167">Hej **UploadFromStream** metoden skapar hello blob om det inte finns, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="b258a-167">hello **UploadFromStream** method creates hello blob if it doesn't exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="b258a-168">(Ändra  *&lt;till filuppladdning >* tooa fullständigt kvalificerade sökvägen toohello-fil som du vill tooupload.)</span><span class="sxs-lookup"><span data-stu-id="b258a-168">(Change *&lt;file-to-upload>* tooa fully qualified path toohello file you want tooupload.)</span></span>

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. <span data-ttu-id="b258a-169">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="b258a-169">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="b258a-170">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b258a-170">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="b258a-171">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="b258a-171">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="b258a-172">Kör hello programmet och välj **överför blob**.</span><span class="sxs-lookup"><span data-stu-id="b258a-172">Run hello application, and select **Upload blob**.</span></span>  
  
<span data-ttu-id="b258a-173">Hej avsnittet - [visa hello blobbar i en blob-behållaren](#list-the-blobs-in-a-blob-container) -illustrerar hur toolist hello-blobbar i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="b258a-173">hello section - [List hello blobs in a blob container](#list-the-blobs-in-a-blob-container) - illustrates how toolist hello blobs in a blob container.</span></span>  

## <a name="list-hello-blobs-in-a-blob-container"></a><span data-ttu-id="b258a-174">Lista hello blobbar i en blobbbehållare</span><span class="sxs-lookup"><span data-stu-id="b258a-174">List hello blobs in a blob container</span></span>

<span data-ttu-id="b258a-175">Detta avsnitt visar hur toolist hello-blobbar i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="b258a-175">This section illustrates how toolist hello blobs in a blob container.</span></span> <span data-ttu-id="b258a-176">hello exempelkod refererar till hello *test blobbehållaren* hello avsnitt [skapa en blobbbehållare](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="b258a-176">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b258a-177">hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="b258a-177">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="b258a-178">Öppna hello `BlobsController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="b258a-178">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="b258a-179">Lägg till en metod som kallas **ListBlobs** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="b258a-179">Add a method called **ListBlobs** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="b258a-180">Inom hello **ListBlobs** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-180">Within hello **ListBlobs** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="b258a-181">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="b258a-181">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="b258a-182">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b258a-182">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="b258a-183">Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="b258a-183">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="b258a-184">toolist hello blobbar i en blobbbehållare använder hello **CloudBlobContainer.ListBlobs** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-184">toolist hello blobs in a blob container, use hello **CloudBlobContainer.ListBlobs** method.</span></span> <span data-ttu-id="b258a-185">Hej **CloudBlobContainer.ListBlobs** metoden returnerar en **IListBlobItem** objekt du omvandlar tooa **CloudBlockBlob**, **CloudPageBlob**, eller **CloudBlobDirectory** objekt.</span><span class="sxs-lookup"><span data-stu-id="b258a-185">hello **CloudBlobContainer.ListBlobs** method returns an **IListBlobItem** object that you cast tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="b258a-186">hello följande kodstycke räknar upp alla hello blobbar i en blobbbehållare.</span><span class="sxs-lookup"><span data-stu-id="b258a-186">hello following code snippet enumerates all hello blobs in a blob container.</span></span> <span data-ttu-id="b258a-187">Varje blobb har omvandlingen toohello lämpligt objekt baserat på dess typ och dess namn (eller URI hello gäller en **CloudBlobDirectory**) läggs tooa lista.</span><span class="sxs-lookup"><span data-stu-id="b258a-187">Each blob is cast toohello appropriate object based on its type, and its name (or URI in hello case of a **CloudBlobDirectory**) is added tooa list.</span></span>

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

    <span data-ttu-id="b258a-188">I tillägg tooblobs kan blob-behållare innehålla kataloger.</span><span class="sxs-lookup"><span data-stu-id="b258a-188">In addition tooblobs, blob containers can contain directories.</span></span> <span data-ttu-id="b258a-189">Anta att du har en blobbbehållare som kallas *test blobbehållaren* med hello följande hierarki:</span><span class="sxs-lookup"><span data-stu-id="b258a-189">Let's suppose you have a blob container called *test-blob-container* with hello following hierarchy:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

    <span data-ttu-id="b258a-190">Med hello föregående kodexempel hello **blobbar** stränglistan innehåller värden liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b258a-190">Using hello preceding code example, hello **blobs** string list contains values similar toohello following:</span></span>

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    <span data-ttu-id="b258a-191">Som du ser innehåller hello listan endast hello översta enheter. inte hello kapslade viktiga (*bar.png* och *baz.png*).</span><span class="sxs-lookup"><span data-stu-id="b258a-191">As you can see, hello list includes only hello top-level entities; not hello nested ones (*bar.png* and *baz.png*).</span></span> <span data-ttu-id="b258a-192">toolist alla Hej entiteter i en blobbbehållare, måste du anropa hello **CloudBlobContainer.ListBlobs** metod och pass **SANT** för hello **useFlatBlobListing** parameter.</span><span class="sxs-lookup"><span data-stu-id="b258a-192">toolist all hello entities within a blob container, you must call hello **CloudBlobContainer.ListBlobs** method and pass **true** for hello **useFlatBlobListing** parameter.</span></span>    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    <span data-ttu-id="b258a-193">Inställningen hello **useFlatBlobListing** parameter för**SANT** returnerar en platt lista över alla entiteter i hello blob-behållaren och ger hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="b258a-193">Setting hello **useFlatBlobListing** parameter too**true** returns a flat listing of all entities in hello blob container, and yields hello following results:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

1. <span data-ttu-id="b258a-194">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="b258a-194">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="b258a-195">På hello **Lägg till vy** dialogrutan Ange **ListBlobs** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b258a-195">On hello **Add View** dialog, enter **ListBlobs** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="b258a-196">Öppna `ListBlobs.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="b258a-196">Open `ListBlobs.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="b258a-197">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b258a-197">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="b258a-198">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="b258a-198">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. <span data-ttu-id="b258a-199">Kör hello programmet och välj **visa blobbar** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="b258a-199">Run hello application, and select **List blobs** toosee results similar toohello following screen shot:</span></span>
  
    ![BLOB-lista](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a><span data-ttu-id="b258a-201">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="b258a-201">Download blobs</span></span>

<span data-ttu-id="b258a-202">Detta avsnitt visar hur toodownload en blob och antingen spara den toolocal lagring eller Läs hello innehållet till en sträng.</span><span class="sxs-lookup"><span data-stu-id="b258a-202">This section illustrates how toodownload a blob and either persist it toolocal storage or read hello contents into a string.</span></span> <span data-ttu-id="b258a-203">hello exempelkod refererar till hello *test blobbehållaren* hello avsnitt [skapa en blobbbehållare](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="b258a-203">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

1. <span data-ttu-id="b258a-204">Öppna hello `BlobsController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="b258a-204">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="b258a-205">Lägg till en metod som kallas **DownloadBlob** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="b258a-205">Add a method called **DownloadBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="b258a-206">Inom hello **DownloadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-206">Within hello **DownloadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="b258a-207">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="b258a-207">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="b258a-208">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b258a-208">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="b258a-209">Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="b258a-209">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="b258a-210">Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-210">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="b258a-211">(Ändra  *&lt;blob-name >* toohello namnet på hello blob som du hämtar.)</span><span class="sxs-lookup"><span data-stu-id="b258a-211">(Change *&lt;blob-name>* toohello name of hello blob you are downloading.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="b258a-212">toodownload blob använda hello **CloudBlockBlob.DownloadToStream** eller **CloudPageBlob.DownloadToStream** metod beroende på hello blob-datatyp.</span><span class="sxs-lookup"><span data-stu-id="b258a-212">toodownload a blob, use hello **CloudBlockBlob.DownloadToStream** or **CloudPageBlob.DownloadToStream** method, depending on hello blob type.</span></span> <span data-ttu-id="b258a-213">hello följande kodavsnitt använder hello **CloudBlockBlob.DownloadToStream** metoden tootransfer en blob innehållet tooa stream-objekt som är beständiga sedan tooa lokal fil: (ändra  *&lt;lokala filnamn >* toohello fullständigt kvalificerade filen namnet som representerar där du vill att hello blob hämtas.)</span><span class="sxs-lookup"><span data-stu-id="b258a-213">hello following code snippet uses hello **CloudBlockBlob.DownloadToStream** method tootransfer a blob's contents tooa stream object that is then persisted tooa local file: (Change *&lt;local-file-name>* toohello fully qualified file name representing where you want hello blob downloaded.)</span></span> 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. <span data-ttu-id="b258a-214">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b258a-214">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="b258a-215">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="b258a-215">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="b258a-216">Kör hello programmet och välj **Download blob** toodownload hello blob.</span><span class="sxs-lookup"><span data-stu-id="b258a-216">Run hello application, and select **Download blob** toodownload hello blob.</span></span> <span data-ttu-id="b258a-217">hello blob som anges i hello **CloudBlobContainer.GetBlockBlobReference** metodanrop hämtar toohello plats som du anger i hello **File.OpenWrite** metodanrop.</span><span class="sxs-lookup"><span data-stu-id="b258a-217">hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call downloads toohello location you specify in hello **File.OpenWrite** method call.</span></span> 

## <a name="delete-blobs"></a><span data-ttu-id="b258a-218">Ta bort blobbar</span><span class="sxs-lookup"><span data-stu-id="b258a-218">Delete blobs</span></span>

<span data-ttu-id="b258a-219">hello följande steg visar hur toodelete en blob:</span><span class="sxs-lookup"><span data-stu-id="b258a-219">hello following steps illustrate how toodelete a blob:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="b258a-220">hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="b258a-220">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="b258a-221">Öppna hello `BlobsController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="b258a-221">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="b258a-222">Lägg till en metod som kallas **DeleteBlob** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="b258a-222">Add a method called **DeleteBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. <span data-ttu-id="b258a-223">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="b258a-223">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="b258a-224">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="b258a-224">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="b258a-225">Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b258a-225">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="b258a-226">Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="b258a-226">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="b258a-227">Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod.</span><span class="sxs-lookup"><span data-stu-id="b258a-227">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="b258a-228">(Ändra  *&lt;blob-name >* toohello namnet på hello blob som tas bort.)</span><span class="sxs-lookup"><span data-stu-id="b258a-228">(Change *&lt;blob-name>* toohello name of hello blob you are deleting.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. <span data-ttu-id="b258a-229">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b258a-229">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="b258a-230">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="b258a-230">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="b258a-231">Kör hello programmet och välj **ta bort blob** toodelete hello blob som anges i hello **CloudBlobContainer.GetBlockBlobReference** metodanrop.</span><span class="sxs-lookup"><span data-stu-id="b258a-231">Run hello application, and select **Delete blob** toodelete hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b258a-232">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b258a-232">Next steps</span></span>
<span data-ttu-id="b258a-233">Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="b258a-233">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="b258a-234">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="b258a-234">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
  * [<span data-ttu-id="b258a-235">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="b258a-235">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-queues.md)
