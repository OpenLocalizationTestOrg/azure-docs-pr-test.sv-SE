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
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt

Azure blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med Azure blob storage. Scenarier som inkluderar en blob-behållare och ladda upp, lista, hämtar och tar bort blobbar.

##<a name="prerequisites"></a>Krav

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Skapa en MVC-enhet 

1. I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *BlobsController*, och välj **Lägg till**.

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Lägg till följande hello *med* direktiven toohello `BlobsController.cs` fil:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Skapa en blobbbehållare

En blob-behållare är en kapslad hierarki med blobbar och mappar. hello följande steg visar hur toocreate en blob-behållare:

> [!NOTE]
> 
> hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `BlobsController.cs` fil.

1. Lägg till en metod som kallas **CreateBlobContainer** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **CreateBlobContainer** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration hello. (Ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage-konto som du försöker komma åt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello önskade blob-behållarnamn. Hej **CloudBlobClient.GetContainerReference** inte gör en begäran mot blob storage. hello referens returneras oavsett hello blob-behållaren finns. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Anropa hello **CloudBlobContainer.CreateIfNotExists** metoden toocreate hello behållaren om den inte finns ännu. Hej **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** om hello behållaren finns inte och har skapats. Annars **FALSKT** returneras.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Uppdatera hello **ViewBag** med hello namnet hello blob-behållare.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **CreateBlobContainer** hello vynamn och välj **Lägg till**.

1. Öppna `CreateBlobContainer.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Kör hello programmet och välj **skapa Blob-behållaren** toosee resulterar liknande toohello följande skärmbild:
  
    ![Skapa blob-behållare](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Som tidigare nämnts hello **CloudBlobContainer.CreateIfNotExists** metoden returnerar **SANT** endast när hello behållaren finns inte och har skapats. Om du kör hello app när hello-behållaren finns hello-metoden returnerar därför **FALSKT**. toorun hello app flera gånger, måste du ta bort hello behållare innan du kör hello appen igen. Ta bort hello behållare kan göras via hello **CloudBlobContainer.Delete** metod. Du kan också ta bort hello behållaren med hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Ladda upp en blobb till en blob-behållare

När du har [skapas en blob-behållare](#create-a-blob-container), kan du överföra filer till behållaren. Det här avsnittet vägleder dig genom att ladda upp en lokal fil tooa blob-behållare. hello stegen förutsätter att du har skapat en blobbbehållare med namnet *test blobbehållaren*. 

> [!NOTE]
> 
> hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `BlobsController.cs` fil.

1. Lägg till en metod som kallas **UploadBlob** som returnerar en **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Inom hello **UploadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Som beskrivits tidigare stöder Azure storage olika blob-typer. tooretrieve referens tooa sidan blob anropet hello **CloudBlobContainer.GetPageBlobReference** metod. tooretrieve referens tooa blockblob anropet hello **CloudBlobContainer.GetBlockBlobReference** metod. Blockblob är vanligtvis hello rekommenderade typen toouse. (Ändra < blob-namn > * toohello namn som du vill toogive hello blob överförs en gång.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. När du har en blobbreferens kan du överföra alla data dataströmmen tooit genom att anropa hello blob referens objektets **UploadFromStream** metod. Hej **UploadFromStream** metoden skapar hello blob om det inte finns, eller skrivs över om den finns. (Ändra  *&lt;till filuppladdning >* tooa fullständigt kvalificerade sökvägen toohello-fil som du vill tooupload.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Kör hello programmet och välj **överför blob**.  
  
Hej avsnittet - [visa hello blobbar i en blob-behållaren](#list-the-blobs-in-a-blob-container) -illustrerar hur toolist hello-blobbar i en blob-behållaren.  

## <a name="list-hello-blobs-in-a-blob-container"></a>Lista hello blobbar i en blobbbehållare

Detta avsnitt visar hur toolist hello-blobbar i en blob-behållaren. hello exempelkod refererar till hello *test blobbehållaren* hello avsnitt [skapa en blobbbehållare](#create-a-blob-container).

> [!NOTE]
> 
> hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `BlobsController.cs` fil.

1. Lägg till en metod som kallas **ListBlobs** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **ListBlobs** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. toolist hello blobbar i en blobbbehållare använder hello **CloudBlobContainer.ListBlobs** metod. Hej **CloudBlobContainer.ListBlobs** metoden returnerar en **IListBlobItem** objekt du omvandlar tooa **CloudBlockBlob**, **CloudPageBlob**, eller **CloudBlobDirectory** objekt. hello följande kodstycke räknar upp alla hello blobbar i en blobbbehållare. Varje blobb har omvandlingen toohello lämpligt objekt baserat på dess typ och dess namn (eller URI hello gäller en **CloudBlobDirectory**) läggs tooa lista.

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

    I tillägg tooblobs kan blob-behållare innehålla kataloger. Anta att du har en blobbbehållare som kallas *test blobbehållaren* med hello följande hierarki:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Med hello föregående kodexempel hello **blobbar** stränglistan innehåller värden liknande toohello följande:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Som du ser innehåller hello listan endast hello översta enheter. inte hello kapslade viktiga (*bar.png* och *baz.png*). toolist alla Hej entiteter i en blobbbehållare, måste du anropa hello **CloudBlobContainer.ListBlobs** metod och pass **SANT** för hello **useFlatBlobListing** parameter.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Inställningen hello **useFlatBlobListing** parameter för**SANT** returnerar en platt lista över alla entiteter i hello blob-behållaren och ger hello följande resultat:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **Blobbar**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **ListBlobs** hello vynamn och välj **Lägg till**.

1. Öppna `ListBlobs.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

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

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Kör hello programmet och välj **visa blobbar** toosee resulterar liknande toohello följande skärmbild:
  
    ![BLOB-lista](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Ladda ned blobbar

Detta avsnitt visar hur toodownload en blob och antingen spara den toolocal lagring eller Läs hello innehållet till en sträng. hello exempelkod refererar till hello *test blobbehållaren* hello avsnitt [skapa en blobbbehållare](#create-a-blob-container).

1. Öppna hello `BlobsController.cs` fil.

1. Lägg till en metod som kallas **DownloadBlob** som returnerar en **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Inom hello **DownloadBlob** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod. (Ändra  *&lt;blob-name >* toohello namnet på hello blob som du hämtar.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. toodownload blob använda hello **CloudBlockBlob.DownloadToStream** eller **CloudPageBlob.DownloadToStream** metod beroende på hello blob-datatyp. hello följande kodavsnitt använder hello **CloudBlockBlob.DownloadToStream** metoden tootransfer en blob innehållet tooa stream-objekt som är beständiga sedan tooa lokal fil: (ändra  *&lt;lokala filnamn >* toohello fullständigt kvalificerade filen namnet som representerar där du vill att hello blob hämtas.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Kör hello programmet och välj **Download blob** toodownload hello blob. hello blob som anges i hello **CloudBlobContainer.GetBlockBlobReference** metodanrop hämtar toohello plats som du anger i hello **File.OpenWrite** metodanrop. 

## <a name="delete-blobs"></a>Ta bort blobbar

hello följande steg visar hur toodelete en blob:

> [!NOTE]
> 
> hello koden i det här avsnittet förutsätter att du har slutfört hello stegen i avsnittet hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `BlobsController.cs` fil.

1. Lägg till en metod som kallas **DeleteBlob** som returnerar en **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudBlobClient** -objektet representerar en klient för blob-tjänsten.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Hämta en **CloudBlobContainer** objekt som representerar ett referens toohello blob-behållarnamn. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Hämta en referens för blob-objektet genom att anropa **CloudBlobContainer.GetBlockBlobReference** eller **CloudBlobContainer.GetPageBlobReference** metod. (Ändra  *&lt;blob-name >* toohello namnet på hello blob som tas bort.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Kör hello programmet och välj **ta bort blob** toodelete hello blob som anges i hello **CloudBlobContainer.GetBlockBlobReference** metodanrop. 

## <a name="next-steps"></a>Nästa steg
Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.

  * [Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)](./vs-storage-aspnet-getting-started-tables.md)
  * [Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)](./vs-storage-aspnet-getting-started-queues.md)
