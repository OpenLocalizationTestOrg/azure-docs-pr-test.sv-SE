---
title: "Snabbstart för Azure – Överföra objekt till och från Azure Blob Storage med hjälp av Java | Microsoft Docs"
description: "Lär dig snabbt hur du överför objekt till och från Azure Blob Storage med hjälp av Java"
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: 5676cef446de7a68d3d8fd1a3b6833a5de184ea1
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/17/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Överföra objekt till och från Azure Blob Storage med hjälp av Java

I den här snabbstarten får du lära dig att använda Java för att ladda upp, hämta och lista blockblobbar i Azure Blob-lagring.

## <a name="prerequisites"></a>Krav

För att slutföra den här snabbstarten behöver du:

* Installera en IDE-miljö som har Maven-integration

* Du kan också installera och konfigurera Maven så att det fungerar från kommandoraden

Den här självstudien använder [Eclipse](http://www.eclipse.org/downloads/) med konfigurationen ”Eclipse IDE for Java Developers”.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Hämta exempelprogrammet

[Exempelprogrammet](https://github.com/Azure-Samples/storage-blobs-java-quickstart) som används i den här snabbstarten är ett grundläggande konsolprogram. 

Använd [git](https://git-scm.com/) för att hämta en kopia av programmet till utvecklingsmiljön. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Det här kommandot klonar lagret till den lokala git-mappen. Öppna projektet genom att starta Eclipse och stänga välkomstskärmen. Välj **Arkiv** och sedan **Öppna projekt, under Filsystem...**. Kontrollera att alternativet för att **identifiera och konfigurera projektegenskaper** har markerats. Välj **Directory** (Katalog) och gå sedan till sidan där du har sparat det klonade lagret. Däri väljer du mappen **javaBlobsQuickstart**. Kontrollera att projektet **javaBlobsQuickstarts** visas som ett Eclipse-projekt och välj sedan **Finish** (Slutför).

När projektet har importerats öppnar du **AzureApp.java** (finns i **blobQuickstart.blobAzureApp** inuti **src/main/java**) och ersätter `accountname` och `accountkey` inuti strängen `storageConnectionString`. Kör sedan programmet.
     

## <a name="configure-your-storage-connection-string"></a>Konfigurera anslutningssträngen för lagring
    
Du måste ange anslutningssträngen för ditt lagringskonto i programmet. Öppna filen **AzureApp.Java**. Hitta variabeln `storageConnectionString`. Ersätt värdena `AccountName` och `AccountKey` i anslutningssträngen med de värden du sparade från Azure-portalen. `storageConnectionString` bör se ut ungefär så här:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Kör exemplet

Det här exemplet skapar en testfil i din standardkatalog (Mina dokument för Windows-användare), överför den till blobblagringen, listar blobbarna i behållaren och hämtar sedan filen med ett nytt namn så att du kan jämföra de gamla och nya filerna. 

Kör exemplet genom att trycka på **Ctrl + F11** i Eclipse.

Om du vill köra exemplet med Maven på kommandoraden kan du öppna ett gränssnitt och gå till **blobAzureApp** i den klonade katalogen. Ange sedan `mvn compile exec:java`.

Följande är ett exempel på utdata om du kör programmet i Windows.

```
Azure Blob storage quick start sample
Creating container: quickstartcontainer
Creating a sample file at: C:\Users\<user>\AppData\Local\Temp\sampleFile514658495642546986.txt
Uploading the sample file 
URI of blob is: https://myexamplesacct.blob.core.windows.net/quickstartcontainer/sampleFile514658495642546986.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.

Deleting the container
Deleting the source, and downloaded files
```

 Kontrollera att de två filerna finns i standardkatalogen (Mina dokument för Windows-användare) innan du fortsätter. Du kan öppna dem och se att de är identiska. Kopiera URL:en för blobben från konsolfönstret och klistra in den i en webbläsare om du vill se innehållet i filen i blobblagringen. När du trycker på Retur-tangenten för att fortsätta tas lagringsbehållaren och filerna bort.

Du kan också använda ett verktyg som [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) för att visa filerna i blobblagringen. Azure Storage Explorer är ett kostnadsfritt verktyg för flera plattformar som gör det möjligt att komma åt information på lagringskontot. 

När du har kontrollerat filerna trycker du på Retur-tangenten för att avsluta demonstrationen och ta bort testfilerna. Nu när du vet vad exemplet gör kan du öppna filen **AzureApp.java** och titta på koden. 

## <a name="understand-the-sample-code"></a>Förstå exempelkoden

Härnäst går vi igenom exempelkoden så att du kan förstå hur den fungerar.

### <a name="get-references-to-the-storage-objects"></a>Hämta referenser till lagringsobjekten

Det första du ska göra är att skapa referenser till objekten som används för att komma åt och hantera blobblagringen. De här objekten bygger på varandra – vart och ett används av nästa i listan.

* Skapa en instans av objektet **CloudStorageAccount** som pekar mot [lagringskontot](/java/api/com.microsoft.azure.management.storage._storage_account).

    Objektet **CloudStorageAccount** är en representation av lagringskontot och gör det möjligt att konfigurera och komma åt egenskaper för lagringskontot programmässigt. Med hjälp av objektet **CloudStorageAccount** kan du skapa en instans av klienten **CloudBlobClient**, som krävs för att komma åt blobbtjänsten.

* Skapa en instans av objektet **CloudBlobClient**, som pekar mot [Blob Service](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) i lagringskontot.

    **CloudBlobClient** ger dig en åtkomstpunkt till blobbtjänsten så att du kan konfigurera och komma åt egenskaper för blobblagring programmässigt. Med hjälp av **CloudBlobClient** kan du skapa en instans av objektet **CloudBlobContainer**, som krävs för att skapa behållare.

* Skapa en instans av objektet **CloudBlobContainer**, som representerar den [behållare](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) du får åtkomst till. Behållare används för att organisera dina blobbar på samma sätt som du använder mappar på datorn för att organisera dina filer.    

    När du har **CloudBlobContainer** kan du skapa en instans av objektet **CloudBlockBlob** som pekar mot den specifika [blobb](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) du är intresserad av, och utföra åtgärder som att ladda upp, hämta och kopiera.

> [!IMPORTANT]
> Behållarnamn måste använda gemener. Mer information om behållare och blobbnamn finns i [Namngivning och referens av behållare, blobbar och metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Skapa en behållare 

I det här avsnittet skapar du en instans av objekt, skapar en ny behållare och anger sedan behörigheter för behållaren så att blobbarna är offentliga och går att komma åt med bara en URL. Behållaren heter **quickstartblobs**. 

Det här exemplet använder **CreateIfNotExists** eftersom vi vill skapa en ny behållare varje gång exemplet körs. I en produktionsmiljö där du använder samma behållare i ett program är det bättre att bara anropa **CreateIfNotExists** en gång. Du kan också skapa behållaren i förväg så att du inte behöver skapa den i koden.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Ladda upp blobbar i behållaren

Blob Storage stöder blockblobar, tilläggsblobar och sidblobar. Blockblobbar är vanligast och används i denna snabbstart. 

Hämta en referens till blobben i målbehållaren om du vill överföra en fil till en blobb. När du har blobbreferensen kan du ladda upp data till den med hjälp av [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Den här åtgärden skapar blobben om den inte redan finns, eller skriver över den om den finns.

Exempelkoden skapar en lokal fil som ska användas för överföring och nedladdning. Spara filen som ska överföras som **källa** och namnet på blobben i **blobb**. I följande exempel överförs filen till behållaren med namnet **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Det går att använda flera [uppladdningsmetoder](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) med blobblagring. Om du till exempel har en sträng kan du använda metoden UploadText i stället för metoden Upload. 

Blockblobbar kan vara valfri texttyp eller binär fil. Sidblobbar används främst för VHD-filer som används för att säkerhetskopiera virtuella IaaS-datorer. Tilläggsblobbar används för loggning, till exempel när du vill skriva till en fil och sedan fortsätta att lägga till mer information. De flesta objekt som lagras i blobblagringen är blockblobbar.

### <a name="list-the-blobs-in-a-container"></a>Visa en lista över blobbarna i en behållare

Du kan hämta en lista över filer i behållaren med hjälp av [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). Följande kod hämtar listan över blobbar och går sedan igenom dem och visar URI för de blobbar som har påträffats. Du kan kopiera URI från kommandofönstret och klistra in den i en webbläsare om du vill visa filen.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Ladda ned blobbar

Ladda ned blobbar till den lokala disken med hjälp av [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

Följande kod hämtar blobben som överfördes i föregående avsnitt och lägger till ett suffix med "_DOWNLOADED" i blobbnamnet så att du kan se båda filerna på den lokala disken. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Rensa resurser

Om du inte längre behöver blobbarna som laddades upp i denna snabbstart kan du ta bort hela behållaren med hjälp av [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Detta tar även bort filerna i behållaren.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du lärt dig hur du överför filer mellan en lokal disk och Azure Blob Storage med Java. Om du vill veta mer om att arbeta med Blob Storage kan du fortsätta till anvisningarna om Blob Storage.

> [!div class="nextstepaction"]
> [Anvisningar för Blob Storage-åtgärder](storage-java-how-to-use-blob-storage.md)

Mer information om Storage Explorer och blobbar finns i [Hantera Azure-blobblagringsresurser med Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md).
