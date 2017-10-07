---
title: "aaaProcess stora datauppsättningar som använder Data Factory och Batch | Microsoft Docs"
description: "Beskriver hur tooprocess stora mängder data i ett Azure Data Factory pipeline med hjälp av kapaciteten för parallell bearbetning av Azure Batch."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Bearbeta datauppsättningar i stor skala med Data Factory och Batch
Den här artikeln beskriver en arkitektur på en exempellösning som flyttar och bearbetar stora datauppsättningar på automatisk och schemalagda sätt. Det ger också en slutpunkt till slutpunkt genomgången tooimplement hello-lösning med hjälp av Azure Data Factory och Azure Batch.

Den här artikeln är längre än våra vanliga artikeln eftersom den innehåller en genomgång av en lösning för hela exemplet. Om du är ny tooBatch och Data Factory, du kan lära dig om dessa tjänster och hur de fungerar tillsammans. Om du vet något om hello tjänster och designa/utforma en lösning, kan du fokusera just på hello [arkitektur avsnittet](#architecture-of-sample-solution) hello artikel och om du utvecklar en prototyp eller en lösning, du kan också tootry stegvisa instruktioner i hello [genomgången](#implementation-of-sample-solution). Vi erbjuder kommentarer om det här innehållet och hur du använder den.

Först ska vi titta på hur Data Factory och Batch-tjänster kan hjälpa med bearbetning av stora datamängder i hello molnet.     

## <a name="why-azure-batch"></a>Varför Azure Batch?
Azure Batch kan toorun storskaliga parallella och högpresterande datorbearbetning (HPC) program effektivt i hello molnet. Det är en platform-tjänst som schemalägger beräkningsintensiva arbete toorun på en hanterad samling av virtuella datorer och kan automatiskt skala beräkna resurser toomeet hello behoven hos dina jobb.

Med hello Batch-tjänsten definierar du Azure compute resurser tooexecute dina program parallellt, och i större skala. Du kan köra på begäran eller schemalagt jobb, och du behöver inte toomanually skapa, konfigurera och hantera ett HPC-kluster, enskilda virtuella datorer, virtuella nätverk eller ett komplexa jobb och uppgift schemaläggning infrastruktur.

Se hello följande artiklar om du inte är bekant med Azure Batch eftersom det hjälper med att förstå hello arkitektur/implementering av hello-lösningen som beskrivs i den här artikeln.   

* [Grunderna i Azure Batch](../batch/batch-technical-overview.md)
* [Översikt över funktionen för batch](../batch/batch-api-basics.md)

(valfritt) toolearn mer om Azure Batch finns hello [Utbildningsväg för Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Varför Azure Data Factory?
Data Factory är en molnbaserad integration datatjänst som samordnar och automatiserar hello flytt och transformering av data. Med hello Data Factory-tjänsten kan du skapa hanterade data pipelines som flytta data från lokala och molnet lagrar tooa centraliserad data datalager (till exempel: Azure Blob Storage), och processen/Transformera data med tjänster som Azure HDInsight och Azure Machine Learning. Du kan också schemalägga data pipelines toorun i en schemalagd tid (varje timme, varje dag, varje vecka, etc.) och övervaka och hantera dem på en snabb överblick över tooidentify problem och vidta åtgärder.

Se hello efter artiklar om du inte är bekant med Azure Data Factory eftersom det hjälper med att förstå hello arkitektur/implementering av hello-lösningen som beskrivs i den här artikeln.  

* [Införandet av Azure Data Factory](data-factory-introduction.md)
* [Skapa din första pipeline för data](data-factory-build-your-first-pipeline.md)   

(valfritt) toolearn mer om Azure Data Factory finns hello [för Azure Data Factory-Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Data Factory och Batch tillsammans
Data Factory innehåller inbyggda aktiviteter, t.ex. Kopieringsaktiviteten toocopy/flytta data från en källdata tooa målarkiv data och Hive aktiviteten tooprocess data med Hadoop-kluster (HDInsight) på Azure. Se [Data Transformation aktiviteter](data-factory-data-transformation-activities.md) en lista över aktiviteter för omvandling av stöds.

Dessutom kan du toocreate anpassade .NET aktiviteter toomove eller process data med egna logik och köra dessa aktiviteter på Azure HDInsight-kluster eller ett Azure Batch-pool för virtuella datorer. När du använder Azure Batch, kan du konfigurera hello poolen tooauto skalning (lägga till eller ta bort virtuella datorer baserat på arbetsbelastning hello) utifrån en formel som du anger.     

## <a name="architecture-of-sample-solution"></a>Arkitektur för exempellösning
Även om hello-arkitekturen som beskrivs i den här artikeln är avsedd för en enkel lösning, är det relevanta toocomplex scenarier, till exempel risk modellering av finansiella tjänster, bild bearbetning och återgivning och genom analys.

hello diagram illustrerar 1) hur Data Factory samordnar dataförflyttning och bearbetning och 2) hur Azure Batch bearbetar hello data på ett sätt som parallellt. Hämta och skriva ut hello diagram för enkel referens (11 x 17 i. eller A3-storlek): [HPC och data orchestration med hjälp av Azure Batch och Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram för storskalig databearbetning](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

hello innehåller följande lista hello grundläggande stegen hello-processen. hello lösningen innehåller koden och förklaringar toobuild hello slutpunkt till slutpunkt-lösning.

1. **Konfigurera Azure Batch med en pool med compute-noder (VM)**. Du kan ange hello antal noder och storleken på varje nod.
2. **Skapa en instans av Azure Data Factory** som har konfigurerats med entiteter som representerar Azure blob-lagring, beräkning Azure Batch-tjänsten, i/o-data och arbetsflöde/pipeline med aktiviteter som att flytta och transformera data.
3. **Skapa en anpassad .NET-aktivitet i hello Data Factory-pipelinen**. hello-aktiviteten är din användarkod som körs på hello Azure Batch-pool.
4. **Lagra stora mängder inkommande data som blobbar i Azure storage**. Data är uppdelat i logiska segment (vanligtvis genom tid).
5. **Data Factory kopierar data som bearbetas parallellt** toohello sekundär plats.
6. **Data Factory körs hello anpassad aktivitet använder hello allokerats av Batch**. Data Factory kan köra aktiviteter samtidigt. Varje aktivitet bearbetar ett segment av data. hello resultat lagras i Azure-lagring.
7. **Data Factory flyttar hello slutresultatet tooa tredje plats**, antingen för distribution via en app eller för ytterligare bearbetning av andra verktyg.

## <a name="implementation-of-sample-solution"></a>Implementering av exempellösning
hello exempellösning är avsiktligt enkla och tooshow du hur toouse Data Factory och Batch tillsammans tooprocess datauppsättningar. hello lösningen är helt enkelt hello antalet förekomster av en sökterm (”Microsoft”) i indatafiler ordnade i en tidsserie. Den matar ut hello antal toooutput filer.

**Tid**: Om du är bekant med grunderna i Azure Data Factory och Batch och ha slutfört hello krav som anges nedan, beräkna den här lösningen tar 1 – 2 timmar toocomplete.

### <a name="prerequisites"></a>Krav
#### <a name="azure-subscription"></a>Azure-prenumeration
Om du inte har en Azure-prenumeration kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Se [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure Storage-konto
Du kan använda ett Azure storage-konto för att lagra hello data i den här självstudiekursen. Om du inte har ett Azure storage-konto, se [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account). hello exempellösning använder blob-lagring.

#### <a name="azure-batch-account"></a>Azure Batch-kontot
Skapa ett Azure Batch-konto med hello [Azure-portalen](http://manage.windowsazure.com/). Se [skapa och hantera Azure Batch-kontot](../batch/batch-account-create-portal.md). Observera hello Azure Batch-kontot namn och åtkomstnyckel. Du kan också använda [ny AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet toocreate Azure Batch-kontot. Se [Kom igång med Azure Batch-PowerShell-cmdlets](../batch/batch-powershell-cmdlets-get-started.md) detaljerade anvisningar om hur du använder denna cmdlet.

hello exempellösning använder Azure Batch (indirekt via ett Azure Data Factory-pipelinen) tooprocess data i en parallell sätt i en pool med compute-noder (en hanterad samling med virtuella datorer).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure Batch-pool för virtuella datorer (VM)
Skapa en **Azure Batch-pool** med minst 2 compute-noder.

1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **Bläddra** i hello vänstra menyn och klickar på **Batch-konton**.
2. Välj din Azure Batch-kontot tooopen hello **Batch-kontot** bladet.
3. Klicka på **pooler** panelen.
4. I hello **pooler** bladet, klickar du på knappen Lägg till på hello verktygsfältet tooadd poolen.
   1. Ange ett ID för hello pool (**programpoolens ID**). Obs hello **ID för hello pool**; du behöver den när du skapar hello Data Factory-lösning.
   2. Ange **Windows Server 2012 R2** för hello Operativsystemsfamilj inställningen.
   3. Välj en **nodprisnivå**.
   4. Ange **2** som värde för hello **mål dedikerade** inställningen.
   5. Ange **2** som värde för hello **maximalt antal uppgifter per nod** inställningen.
   6. Klicka på **OK** toocreate hello poolen.

#### <a name="azure-storage-explorer"></a>Azure Lagringsutforskaren
[Azure Storage Explorer 6 (Verktyg)](https://azurestorageexplorer.codeplex.com/) eller [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (från ClumsyLeaf programvara). Du kan använda dessa verktyg för att kontrollera och ändra hello data i Azure Storage projekt inklusive hello-loggarna för din molnbaserade program.

1. Skapa en behållare med namnet **minbehållare** med privat åtkomst (ingen anonym åtkomst)
2. Om du använder **CloudXplorer**skapar mappar och undermappar med hello följande struktur:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`och `outputfolder` är mappar på högsta nivå i `mycontainer`. Hej `inputfolder` har undermappar med datum-/ tidsstämplar (åååå-MM-DD-HH).

   Om du använder **Azure Lagringsutforskaren**, i hello nästa steg, behöver du tooupload filer med namn: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` och så vidare. Det här steget skapar automatiskt hello mappar.
3. Skapa en textfil **fil.txt** på datorn med innehåll som har hello nyckelordet **Microsoft**. Till exempel: ”test anpassad aktivitet Microsoft test anpassad aktivitet Microsoft”.
4. Överför hello filen toohello efter inkommande mappar i Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Om du använder **Azure Lagringsutforskaren**, ladda upp filen hello **fil.txt** för**minbehållare**. Klicka på **kopiera** på hello verktygsfältet toocreate en kopia av hello-blob. I hello **kopiera Blob** dialogrutan, ändra hello **blob målnamn** för`inputfolder/2015-11-16-00/file.txt`. Upprepa det här steget toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` och så vidare. Den här åtgärden skapar automatiskt hello mappar.
5. Skapa en annan behållare med namnet: `customactivitycontainer`. Du kan överföra hello anpassad aktivitet zip-filen toothis behållare.

#### <a name="visual-studio"></a>Visual Studio
Installera Microsoft Visual Studio 2012 eller senare toocreate hello anpassade Batch aktiviteten toobe används i hello Data Factory-lösning.

### <a name="high-level-steps-toocreate-hello-solution"></a>Anvisningar toocreate hello lösning
1. Skapa en anpassad aktivitet som innehåller hello databearbetning logik.
2. Skapa ett Azure data factory som använder hello anpassad aktivitet:

### <a name="create-hello-custom-activity"></a>Skapa hello anpassad aktivitet
hello Data Factory anpassad aktivitet är hello hjärtat i det här exemplet lösning. hello exempellösningen använder Azure Batch toorun hello anpassad aktivitet. Se [använda anpassade aktiviteter i ett Azure Data Factory-pipelinen](data-factory-use-custom-activities.md) för hello grundläggande information toodevelop anpassade aktiviteter och Använd dem i Azure Data Factory rörledningar.

toocreate en anpassad .NET-aktivitet som du kan använda i ett Azure Data Factory-pipelinen, behöver du toocreate en **.NET-klassbibliotek** projekt med en klass som implementerar som **IDotNetActivity** gränssnitt. Det här gränssnittet har endast en metod: **kör**. Här är hello signaturen för hello-metoden:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

hello-metoden innehåller några viktiga komponenter som du behöver toounderstand.

* hello-metoden har fyra parametrar:

  1. **linkedServices**. En enumerable lista över länkade tjänster som länkar i/o-datakällor (till exempel: Azure Blob Storage) toohello data factory. I det här exemplet finns bara en länkad tjänst av typen Azure Storage som används för både inkommande och utgående.
  2. **datauppsättningar**. Detta är en enumerable lista över datauppsättningar. Du kan använda den här parametern tooget hello platser och scheman som definieras av inkommande och utgående datauppsättningar.
  3. **aktiviteten**. Den här parametern representerar hello aktuella beräkning enhet – i det här fallet Azure Batch-tjänsten.
  4. **loggaren**. hello loggaren kan du skriva debug kommentarer som yta som hello ”användare” i felloggen hello pipeline.
* hello-metoden returnerar en ordlista som kan vara tillsammans används toochain anpassade aktiviteter i framtiden hello. Den här funktionen har inte implementerats ännu, så returnerar ett tomt Ordbok från hello-metod.

#### <a name="procedure-create-hello-custom-activity"></a>Metod: Skapa hello anpassad aktivitet
1. Skapa ett .NET-klassbibliotek-projekt i Visual Studio.

   1. Starta **Visual Studio 2012**/**2013/2015**.
   2. Klicka på **filen**, peka för**ny**, och klicka på **projekt**.
   3. Expandera **mallar**, och välj **Visual C\#**. I den här genomgången ska du använda C\#, men du kan använda alla .NET språk toodevelop hello anpassad aktivitet.
   4. Välj **klassbiblioteket** hello listan över projekttyper på hello rätt.
   5. Ange **MyDotNetActivity** för hello **namn**.
   6. Välj **C:\\ADF** för hello **plats**. Skapa hello mapp **ADF** om den inte finns.
   7. Klicka på **OK** toocreate hello projektet.
2. Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.
3. I hello **Pakethanterarkonsolen**, kör följande kommando tooimport hello **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importera hello **Azure Storage** NuGet-paketet i toohello projekt. Du behöver det här paketet eftersom du använder hello Blob storage-API i det här exemplet.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Lägg till följande hello **med** direktiven toohello källfilen i hello-projekt.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Ändra hello namnet på hello **namnområde** för**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Ändra hello namnet på hello-klass för**MyDotNetActivity** och härledd från hello **IDotNetActivity** gränssnitt som visas nedan.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementera (Lägg till) hello **kör** metod för hello **IDotNetActivity** gränssnitt toohello **MyDotNetActivity** klass och kopiera hello följande exempel kod toohello metod. Se hello [Kör metod](#execute-method) avsnittet förklaring för hello logik som används i den här metoden.

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass hello connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize hello continuation token before using it in hello do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get hello list of input blobs from hello input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns hello number of occurrences of
           // hello search term (“Microsoft”) in each blob associated
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload hello output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} toohello output blob", output);
       outputBlob.UploadText(output);
    
       // hello dictionary can be used toochain custom activities together in hello future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. Lägg till följande hjälpklass metoder toohello hello. De här metoderna anropas av hello **Execute** metod. Viktigast av allt hello **Calculate** metoden isolerar hello-kod som går igenom varje blob.

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets hello fileName value from hello input/output dataset.
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    Hej **GetFolderPath** returnerar metoden hello toohello sökväg som hello dataset pekar tooand hello **GetFileName** metoden returnerar hello namnet på hello blob/fil hello dataset pekar på.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Hej **Calculate** metoden beräknar hello antal instanser av nyckelordet **Microsoft** i hello indatafiler (blobbar i hello mapp). hello sökordet (”Microsoft”) är hårdkodat i hello kod.

1. Kompilera hello-projekt. Klicka på **skapa** från hello-menyn och klicka på **skapa lösning**.
2. Starta **Windows Explorer**, och navigera för**bin\\felsöka** eller **bin\\släpper** beroende på hello typ av version.
3. Skapa en zip-fil **MyDotNetActivity.zip** som innehåller alla hello-binärfiler i hello  **\\bin\\felsöka** mapp. Du kanske vill tooinclude hello MyDotNetActivity. **pdb** filen så att du får ytterligare information, till exempel radnumret i hello källkod som orsakade hello problemet när ett fel uppstår.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Överför **MyDotNetActivity.zip** som en blob toohello blob-behållare: `customactivitycontainer` i hello Azure blob storage som hello **StorageLinkedService** länkade tjänsten i hello  **ADFTutorialDataFactory** använder. Skapa hello blob-behållaren `customactivitycontainer` om den inte redan finns.

#### <a name="execute-method"></a>Execute-metoden
Det här avsnittet innehåller mer information och information om hello koden i hello Execute-metoden.

1. hello-medlemmar för att söka igenom inkommande hello-samling finns i hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namnområde. Söka igenom hello blob samling kräver att du använder hello **BlobContinuationToken** klass. I princip måste du använda en gör-när loop med hello token som hello mekanism för att avsluta hello loop. Mer information finns i [hur toouse Blob storage från .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). En grundläggande loop visas här:

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Hello i dokumentationen för hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metod för information.
2. hello koden fungerar via hello uppsättning blobbar logiskt hamnar inom hello do-while-slinga. I hello **kör** metod, hello-medan loop skickar hello lista över BLOB tooa metod med namnet **beräkna**. hello-metoden returnerar en variabel med namnet **utdata** som hello resultatet av att ha hävdade via alla hello BLOB i hello segment.

   Den returnerar hello antalet förekomster av hello sökterm (**Microsoft**) i hello blob skickades toohello **Calculate** metod.

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. En gång hello **Calculate** metod har gjort hello arbete, den måste vara skriven tooa nya blob. Så att en ny blob kan skrivas med hello resultat för varje uppsättning blobbar som bearbetas. toowrite tooa ny blob, första hitta hello utdata datauppsättningen.

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. hello koden anropar också en hjälpmetod: **GetFolderPath** tooretrieve hello mappsökvägen (hello namnet på lagringsbehållaren).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   Hej **GetFolderPath** webbsändningar hello DataSet-objektet tooan AzureBlobDataSet som har en egenskap med namnet FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. hello koden anropar hello **GetFileName** metoden tooretrieve hello filnamn (blob-namn). hello koden är liknande toohello ovan kod tooget hello mappsökväg.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. hello namnet på hello fil skrivs genom att skapa ett URI-objekt. hello URI konstruktorn använder hello **BlobEndpoint** egenskapen tooreturn hello behållarens namn. hello mappen sökvägen och filnamnet läggs tooconstruct hello utdata blob-URI.  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. hello namnet på hello-filen har skrivits och nu kan du skriva hello utdata-strängen från hello **Calculate** metoden tooa nya blob:

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a>Skapa hello data factory
I hello [skapa hello anpassad aktivitet](#create-the-custom-activity) avsnittet du har skapat en anpassad aktivitet och överförda hello zip-filen med binärfiler och hello PDB-filen tooan Azure blob-behållare. I det här avsnittet skapar du en Azure **datafabriken** med en **pipeline** som använder hello **anpassad aktivitet**.

Hej inkommande dataset för hello anpassad aktivitet representerar hello BLOB (filer) i hello inkommande mapp (`mycontainer\\inputfolder`) i blob storage. Hej utdatauppsättningen för hello aktivitet representerar hello utdata blobbar i hello utdatamapp (`mycontainer\\outputfolder`) i blob storage.

Ta bort en eller flera filer i hello inkommande mappar:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Till exempel ta bort en fil (fil.txt) med följande innehåll i mappar hello hello.

```
test custom activity Microsoft test custom activity Microsoft
```

Varje inkommande mapp motsvarar tooa segment i Azure Data Factory även om hello-mappen har 2 eller flera filer. När varje segment bearbetas av hello pipeline går hello anpassad aktivitet igenom alla hello BLOB i hello inkommande mapp för sektorn.

Du kan se utdata fem filer med hello samma innehåll. Hello utdatafilen från att bearbeta hello-filen i mappen hello 2015-11-16-00 har till exempel hello följande innehåll:

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

Om du släpper flera filer (fil.txt, file2.txt, file3.txt) med hello samma innehållsmapp toohello inkommande ser du hello följande innehåll i hello utdatafilen. Varje mapp (2015-11-16-00, etc.) motsvarar tooa sektor i det här exemplet, även om hello-mappen har flera inkommande filer.

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

hello utdatafilen har tre rader nu, ett för varje indatafilen (blob) i hello mapp som associeras med hello sektorn (2015-11-16-00).

En aktivitet skapas för varje aktivitet körs. I det här exemplet finns bara en aktivitet i hello pipeline. När ett segment bearbetas av hello pipeline körs hello anpassad aktivitet på Azure Batch tooprocess hello sektorn. Eftersom det finns fem segment (varje segment kan ha flera blobbar eller -fil), det finns fem aktiviteter som skapats i Azure Batch. När en aktivitet körs på Batch är faktiskt anpassad aktivitet för hello som körs.

hello följande genomgång innehåller ytterligare information.

#### <a name="step-1-create-hello-data-factory"></a>Steg 1: Skapa hello data factory
1. När du loggar in toohello [Azure-portalen](https://portal.azure.com/), hello följande steg:

   1. Klicka på **ny** på hello vänstra menyn.
   2. Klicka på **Data + analys** i hello **ny** bladet.
   3. Klicka på **Datafabriken** på hello **dataanalys** bladet.
2. I hello **nya datafabriken** bladet ange **CustomActivityFactory** för hello namn. hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: **datafabriksnamnet ”CustomActivityFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel **yournameCustomActivityFactory**) och försök att skapa igen.
3. Klicka på **RESURSGRUPPENS namn**, och välj en befintlig resursgrupp eller skapa en resursgrupp.
4. Kontrollera att du använder rätt hello-prenumeration och region där du vill skapa hello data factory toobe.
5. Klicka på **skapa** på hello **nya data factory** bladet.
6. Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen.
7. När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Steg 2: Skapa länkade tjänster
Länkade tjänster länka datalager eller compute services tooan Azure data factory. I det här steget kan du länka din **Azure Storage** konto och **Azure Batch** konto tooyour data factory.

#### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
1. Klicka på hello **författare och distribuera** panelen på hello **DATAFABRIKEN** bladet för **CustomActivityFactory**. Du kan se hello Data Factory-redigeraren.
2. Klicka på **Nytt datalager** hello kommandofältet och välj **Azure storage.** Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto. toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Skapa Azure Batch länkad tjänst
I det här steget skapar du en länkad tjänst för din **Azure Batch** konto som används toorun hello Data Factory anpassad aktivitet.

1. Klicka på **nya beräkning** hello kommandofältet och välj **Azure Batch.** Du bör se hello JSON-skript för att skapa ett Azure Batch länkade tjänsten i hello-redigeraren.
2. I hello JSON-skript:

   1. Ersätt **kontonamn** med hello namnet på Azure Batch-kontot.
   2. Ersätt **åtkomstnyckeln** med hello snabbtangent av hello Azure Batch-kontot.
   3. Ange hello-ID för hello pool för hello **poolName** egenskapen**.** För den här egenskapen kan du ange antingen namnet på programpoolen eller poolens ID.
   4. Ange hello batch URI för hello **batchUri** JSON-egenskapen.

      > [!IMPORTANT]
      > Hej **URL** från hello **Azure Batch-kontoblad** i hello följande format: \<accountname\>.\< region\>. batch.azure.com. För hello **batchUri** egenskap i hello JSON du behöver för**ta bort ”accountname”.** från hello-URL. Exempel: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      För hello **poolName** egenskap, du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.

      > [!NOTE]
      > hello Data Factory-tjänsten stöder inte ett alternativ på begäran för Azure Batch som för HDInsight. Du kan bara använda din egen Azure Batch-pool i ett Azure data factory.
      >
      >
   5. Ange **StorageLinkedService** för hello **linkedServiceName** egenskapen. Du har skapat den här länkade tjänsten i hello föregående steg. Den här används som ett mellanlagringsområde för filer och loggar.
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

#### <a name="step-3-create-datasets"></a>Steg 3: Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent indata och utdata.

#### <a name="create-input-dataset"></a>Skapa indatauppsättning
1. I hello **Editor** hello Data Factory, klickar du på **ny datamängd** hello verktygsfält och klicka på knappen **Azure Blob storage** hello nedrullningsbara menyn.
2. Ersätt hello JSON i hello högra med hello följande JSON-fragment:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Du skapar en pipeline senare i den här genomgången med starttid: 2015-11-16T00:00:00Z-och Sluttid: 2015-11-16T05:00:00Z. Det är schemalagda tooproduce data **varje timme**, så att det finns 5 i/o-segment (mellan **00**: 00:00 -\> **05**: 00:00).

    Hej **frekvens** och **intervall** hello inkommande dataset är inställd för**timme** och **1**, vilket innebär att hello indata sektorn är tillgängliga varje timme.

    Här följer hello starttider för varje segment som representeras av **SliceStart** systemvariabel i hello ovan JSON-fragment.

    | **Sektorn** | **Starttid**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**: 00:00 |
    | 2         | 2015-11-16T**01**: 00:00 |
    | 3         | 2015-11-16T**02**: 00:00 |
    | 4         | 2015-11-16T**03**: 00:00 |
    | 5         | 2015-11-16T**04**: 00:00 |

    Hej **folderPath** beräknas med hjälp av hello år, månad, dag och timme tillhör hello tidsintervallet för start (**SliceStart**). Här är därför hur en inkommande mappen är mappade tooa sektorn.

    | **Sektorn** | **Starttid**          | **Inkommande mapp**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16-**04** |

1. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset** tabell.

#### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
I det här steget skapar du en annan dataset av typen AzureBlob toorepresent hello-utdata.

1. I hello **Editor** hello Data Factory, klickar du på **ny datamängd** hello verktygsfält och klicka på knappen **Azure Blob storage** hello nedrullningsbara menyn.
2. Ersätt hello JSON i hello högra med hello följande JSON-fragment:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    En blob/utdatafil genereras för varje inkommande segment. Här är hur en utdatafil heter för varje segment. Alla hello utdatafiler skapas i en utdatamapp: `mycontainer\\outputfolder`.

    | **Sektorn** | **Starttid**          | **Utdatafil**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Kom ihåg att alla hello filer i en mapp för inkommande (till exempel: 2015-11-16-00) är en del av ett segment med hello starttiden: 2015-11-16-00. När den här sektorn behandlas hello anpassad aktivitet söker igenom varje fil och ger en rad i hello utdatafil med hello antal förekomster av sökterm (”Microsoft”). Om det finns tre filer i mappen hello 2015-11-16-00, det finns tre rader i hello utdatafilen: 2015-11-16-00.txt.

1. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **OutputDataset**.

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a>Steg 4: Skapa och köra hello pipeline med anpassad aktivitet
I det här steget skapar du en pipeline med en aktivitet, hello anpassad aktivitet som du skapade tidigare.

> [!IMPORTANT]
> Om du inte har överfört hello **fil.txt** tooinput mappar i hello blob-behållaren göra det innan du skapar hello pipeline. Hej **isPaused** egenskapen toofalse i pipeline-hello JSON, så hello pipeline körs omedelbart som hello **starta** datumet infaller under hello tidigare.
>
>

1. I hello Data Factory-redigeraren, klickar du på **ny pipeline** i hello kommandofält. Om du inte ser hello kommando klickar du på **... (Tre punkter)**  toosee den.
2. Ersätt hello JSON i hello högra med hello följande JSON-skript:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Observera följande punkter hello:

   * Det finns bara en aktivitet i hello pipeline och som är av typen: **DotNetActivity**.
   * **AssemblyName** anges toohello namnet på hello DLL: **MyDotNetActivity.dll**.
   * **EntryPoint** har angetts för**MyDotNetActivityNS.MyDotNetActivity**. Det är i princip \<namnområde\>.\< ClassName\> i koden.
   * **PackageLinkedService** har angetts för**StorageLinkedService** som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen. Om du använder olika Azure Storage-konton för i/o-filer och hello anpassad aktivitet zip-filen, har du toocreate en annan Azure Storage länkade tjänsten. Den här artikeln förutsätter att du använder hello samma Azure Storage-konto.
   * **PackageFile** har angetts för**customactivitycontainer/MyDotNetActivity.zip**. Den är i hello format: \<containerforthezip\>/\<nameofthezip.zip\>.
   * hello anpassad aktivitet tar **InputDataset** som indata och **OutputDataset** som utdata.
   * Hej **linkedServiceName** -egenskapen för hello anpassad aktivitet pekar toohello **AzureBatchLinkedService**, som talar om Azure Data Factory som hello anpassade aktiviteten måste toorun på Azure Batch.
   * Hej **samtidighet** inställning är viktig. Om du använder hello standardvärdet, vilket är 1, även om du har 2 eller mer compute-noder i hello Azure Batch-pool, hello segment bearbetas efter varandra. Du därför inte att utnyttja mätfunktioner på hello parallell bearbetning av Azure Batch. Om du ställer in **samtidighet** tooa högre värde, säg 2 innebär det att två sektorer (motsvarar tootwo aktiviteter i Azure Batch) kan bearbetas hos hello samma tid, i vilket fall båda hello virtuella datorer i hello Azure Batch-pool används. Därför egenskapen hello samtidighet på lämpligt sätt.
   * Endast en aktivitet (segment) körs på en virtuell dator när som helst som standard. hello orsak är att, som standard hello **maximalt uppgifter per VM** anges too1 för en Azure Batch-pool. Som en del av krav, du har skapat en pool med den här egenskapen set too2 så två Data Factory-segment kan köras på en virtuell dator på hello samtidigt.

    -   **isPaused** egenskapen toofalse som standard. hello pipelinen körs direkt i det här exemplet eftersom hello segment starta i hello tidigare. Du kan ange den här egenskapen tootrue toopause hello pipeline och ange den bakre toofalse toorestart.

    -   Hej **starta** tid och **end** segment produceras varje timma, så att fem segment som produceras av hello pipeline och tider är fem timmar från varandra.

1. Klicka på **distribuera** på hello i kommandofältet toodeploy hello pipeline.

#### <a name="step-5-test-hello-pipeline"></a>Steg 5: Testa hello pipeline
I det här steget kan testa du hello pipeline genom att släppa filer till hello inkommande mappar. Vi börjar med tester hello pipeline med en fil per inkommande mappar.

1. I hello Data Factory-bladet i hello Azure-portalen klickar du på **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. Dubbelklicka på inkommande dataset i hello diagramvyn: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Du bör se hello **InputDataset** bladet med alla fem sektorer redo. Meddelande hello **sektorn starttid** och **sektorn SLUTTID** för varje segment.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. I hello **diagramvyn**, klicka på **OutputDataset**.
5. Du bör se att hello fem utdata segment är i tillståndet för hello Ready om de redan har skapats.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Använd Azure portal tooview hello **uppgifter** som är associerade med hello **segment** och se vilka VM varje segment som kördes på. Se [Data Factory och Batch-integrering](#data-factory-and-batch-integration) information.
7. Du bör se hello utdatafilerna i hello `outputfolder` av `mycontainer` i din Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Du bör se fem utdatafiler, en för varje inkommande segment. Var och en av hello utdata filen måste ha innehåll liknande toohello följande utdata:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   hello följande diagram illustrerar hur hello Data Factory segment mappa tootasks i Azure Batch. I det här exemplet har ett segment endast en körning.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Nu ska vi försöker med flera filer i en mapp. Skapa filer: **file2.txt**, **file3.txt**, **file4.txt**, och **file5.txt** med hello samma innehåll som fil.txt i hello mapp: **2015-11-06-01**.
9. I hello utdatamapp **ta bort** hello utdatafilen: **2015-11-16-01.txt**.
10. Nu i hello **OutputDataset** bladet, högerklicka på hello segmentet med **sektorn starttid** ställa in också**2015-11/16 01:00:00 AM**, och klicka på **kör**toorerun/återexport-process hello sektorn. Nu har hello sektorn fem filer i stället för en fil.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. När hello sektorn körs och dess status är **klar**, kontrollera hello innehållet i hello utdatafil för det här segmentet (**2015-11-16-01.txt**) i hello `outputfolder` av `mycontainer` i blob storage. Det bör finnas en rad för varje fil i hello sektorn.

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Om du inte ta bort hello utdata filen 2015-11-16-01.txt innan du försöker med fem indatafiler, visas en rad från hello tidigare segmentet kör och fem rader från hello aktuellt segment kör. Som standard är hello innehåll läggs toooutput fil om den redan finns.
>
>

#### <a name="data-factory-and-batch-integration"></a>Data Factory och Batch-integrering
hello Data Factory-tjänsten skapar ett jobb i Azure Batch med hello namn: `adf-poolname:job-xxx`.

![Azure Data Factory - batchjobb](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

En aktivitet i hello jobb skapas för varje aktivitet körning av ett segment. Om det finns 10 segment-redo toobe bearbetas, har 10 aktiviteter skapats i hello jobb. Du kan ha fler än ett segment som körs parallellt om du har flera compute-noder i hello pool. Om hello maximala uppgifter per compute-nod har angetts för > 1, det kan finnas mer än en segmentera körs på hello samma beräkning.

I det här exemplet finns fem segment, så fem aktiviteter i Azure Batch. Med hello **samtidighet** ställa in också**5** i hello pipeline-JSON i Azure Data Factory och **maximalt uppgifter per VM** ställa in också**2** i Azure Batch med **2** virtuella datorer, hello aktiviteter körs snabb (kontrollera start- och sluttider för aktiviteter).

Använd hello portal tooview hello batchjobb och dess uppgifter som är associerade med hello **segment** och se vilka VM varje segment som kördes på.

![Azure Data Factory - jobbet batchaktiviteter](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a>Felsöka hello pipeline
Felsökning består av några grundläggande metoder:

1. Om hello inkommande sektorn inte har angetts för**klar**, bekräfta att hello inkommande mappstrukturen är korrekt och fil.txt finns i hello inkommande mappar.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. I hello **kör** metod för din anpassade aktivitet, Använd hello **IActivityLogger** objekt toolog information som hjälper dig att felsöka problem. hälsningsmeddelande loggas visas i hello användaren\_0. loggfilen.

   I hello **OutputDataset** bladet, klickar du på hello sektorn toosee hello **DATASEKTORN** bladet för den sektorn. Du ser **aktivitetskörningar** för den sektorn. Du bör se en aktivitet som körs för hello sektorn. Om du klickar på **kör** i kommandofältet hello börjar du en annan aktivitet som körs för hello samma segment.

   När du klickar på hello aktivitet kör du ser hello **kör AKTIVITETSINFORMATION** bladet med en lista över loggfilerna. Du ser loggade meddelanden i hello **användare\_0. loggen** fil. När ett fel uppstår finns tre aktivitetskörningar eftersom hello återförsöksvärde anges too3 i hello pipeline/aktivitets-JSON. När du klickar på hello aktivitet kör se du hello loggfiler att du kan granska tootroubleshoot hello fel.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   Hello loggfiler, klicka på hello **användare 0.log**. Hello högra panelen är hello resultatet av att använda hello **IActivityLogger.Write** metod.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Kontrollera system-0.log för alla system felmeddelanden och undantag.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Inkludera hello **PDB** filen i hello zip-filen så att hello felinformation har information som **anropsstacken** när ett fel inträffar.
4. Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Se till att hello **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) och **packageLinkedService** (bör punkt toohello Azure blob-lagring som innehåller hello zip-filen) har angetts toocorrect värden.
6. Om du har löst ett fel och vill tooreprocess hello segment Högerklicka hello sektor i hello **OutputDataset** bladet och klicka på **kör**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Du ser en **behållare** i din Azure Blob storage med namnet: `adfjobs`. Den här behållaren tas inte bort automatiskt, men du kan ta bort när du är klar med testet hello-lösning. På liknande sätt hello Data Factory lösning skapar ett Azure Batch **jobbet** med namnet: `adf-\<pool ID/name\>:job-0000000001`. Du kan ta bort det här jobbet när du har testat hello lösning om du vill.
   >
   >
7. hello anpassad aktivitet använder inte hello **app.config** filen från paketet. Därför om koden läser eventuella anslutningssträngar från hello konfigurationsfil, fungerar det inte vid körning. Hej bäst när med hjälp av Azure Batch är toohold alla hemligheter i en **Azure KeyVault**, använda en certifikatbaserad service principal tooprotect hello keyvault och distribuera hello certifikat tooAzure Batch-pool. hello kan .NET anpassad aktivitet sedan använda hemligheter från hello KeyVault vid körning. Den här lösningen är en generisk och kan skalas tooany typ av hemlighet, inte bara anslutningssträngen.

    Det finns en enklare lösning (men inte rekommenderas): du kan skapa en **Azure SQL länkade tjänsten** med strängen anslutningsinställningar, skapa en datamängd som använder hello länkad tjänst och kedjan hello datamängd som en dummy inkommande datauppsättning toohello anpassad .NET-aktivitet. Du kan sedan åtkomst hello länkade tjänstens anslutningssträngen i hello anpassad Aktivitetskod och det bör fungera bra vid körning.  

#### <a name="extend-hello-sample"></a>Utöka hello-exempel
Du kan utöka det här exemplet toolearn mer om Azure Data Factory och Azure Batch-funktioner. Till exempel tooprocess segment i ett annat tidsintervall hello följande steg:

1. Lägg till följande undermappar i hello hello `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 och placera inkommande filer i mapparna. Ändra hello sluttiden för pipelinen hello från `2015-11-16T05:00:00Z` för`2015-11-16T10:00:00Z`. I hello **diagramvyn**, dubbelklicka på hello **InputDataset**, och bekräfta att hello inkommande segment är klara. Dubbelklicka på **OuptutDataset** toosee hello tillståndet för utdata segment. Om de är i tillståndet Ready Kontrollera hello utdatamapp för hello utdatafilerna.
2. Öka eller minska hello **samtidighet** inställningen toounderstand hur den påverkar hello prestandan för din lösning särskilt hello bearbetning som uppstår på Azure Batch. (Se steg 4: skapa och köra hello pipeline mer information om hello **samtidighet** inställningen.)
3. Skapa en pool med högre/nedre **maximalt uppgifter per VM**. toouse hello ny pool som du skapade uppdateringen hello Azure Batch länkad tjänst i hello Data Factory-lösning. (Se steg 4: skapa och köra hello pipeline mer information om hello **maximalt uppgifter per VM** inställningen.)
4. Skapa en Azure Batch-pool med **Autoskala** funktion. Skala automatiskt beräkningsnoder i en pool med Azure Batch är hello justeras dynamiskt processorkraft som används av ditt program. 

    här hello formeln uppnår hello följande beteende: när hello poolen skapas, det börjar med 1 VM. $PendingTasks mått definierar hello antalet aktiviteter i körs + aktiv (i kö) tillstånd.  hello formeln hittar hello Genomsnittligt antal väntande aktiviteter i hello senaste 180 sekunder och anger därefter TargetDedicated. Det garanterar att TargetDedicated aldrig är mer omfattande än 25 virtuella datorer. Så som nya aktiviteter skickas pool växer automatiskt och som slutförda uppgifter, virtuella datorer blir ledigt i taget och hello autoskalning krymper dessa virtuella datorer. startingNumberOfVMs och maxNumberofVMs kan vara justerade tooyour behov.
 
    Autoskala formel:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Se [automatiskt skala compute-noder i en Azure Batch-pool](../batch/batch-automatic-scaling.md) mer information.

   Om hello pool använder hello standard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch-tjänsten kan ta 15 30 minuter tooprepare hello VM innan du kör hello anpassad aktivitet.  Om hello pool använder en annan autoScaleEvaluationInterval, kan hello Batch-tjänsten ta autoScaleEvaluationInterval + 10 minuter.
5. I hello exempellösning hello **kör** metoden startar hello **Calculate** metod som bearbetar en indata sektorn tooproduce datasektorn en utdata. Du kan skriva egna metoden tooprocess indata och Ersätt hello Calculate metodanrop i hello Execute-metoden med en anropet tooyour metod.

### <a name="next-steps-consume-hello-data"></a>Nästa steg: använda hello data
När du bearbetar data kan du använda den med online verktyg som **Microsoft Power BI**. Här är länkar toohelp du förstår Power BI och hur toouse den i Azure:

* [Utforska en datamängd i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Komma igång med hello Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Uppdatera data i Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure och Power BI - översikt](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Referenser
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Introduktion tooAzure Data Factory-tjänsten](data-factory-introduction.md)
  * [Kom igång med Azure Data Factory](data-factory-build-your-first-pipeline.md)
  * [Använd anpassade aktiviteter i en Azure Data Factory-pipeline](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Grunderna i Azure Batch](../batch/batch-technical-overview.md)
  * [Översikt över Azure Batch-funktioner](../batch/batch-api-basics.md)
  * [Skapa och hantera Azure Batch-kontot i hello Azure-portalen](../batch/batch-account-create-portal.md)
  * [Kom igång med Azure Batch-biblioteket .NET](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
