---
title: aaaUse anpassade aktiviteter i ett Azure Data Factory-pipelinen
description: "Lär dig hur toocreate anpassade aktiviteter och Använd dem i ett Azure Data Factory-pipelinen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md) 
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

Det finns två typer av aktiviteter som du kan använda i ett Azure Data Factory-pipelinen.

- [Data Movement aktiviteter](data-factory-data-movement-activities.md) toomove data mellan [stöds källa och mottagare datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Data Transformation aktiviteter](data-factory-data-transformation-activities.md) tootransform data med hjälp av compute-tjänster som Azure HDInsight, Azure Batch och Azure Machine Learning. 

Skapa toomove data till/från ett dataarkiv som Data Factory inte stöder en **anpassad aktivitet** med dina egna data movement logik och Använd hello aktivitet i en pipeline. På samma sätt kan skapa en anpassad aktivitet med dina egna data transformation logik tootransform/bearbetning av data på ett sätt som inte stöds av Data Factory och använda hello aktivitet i en pipeline. 

Du kan konfigurera en anpassad aktivitet toorun på en **Azure Batch** poolen med virtuella datorer eller en Windows-baserad **Azure HDInsight** klustret. När du använder Azure Batch kan använda du endast en befintlig Azure Batch-pool. När du använder HDInsight kan du använda ett befintligt HDInsight-kluster eller ett kluster som skapas automatiskt för du på-begäran vid körning.  

hello innehåller följande genomgång stegvisa instruktioner för att skapa en anpassad .NET-aktivitet och använder hello anpassad aktivitet i en pipeline. hello genomgången använder en **Azure Batch** länkade tjänsten. toouse ett Azure HDInsight i stället länkade tjänsten skapar du en länkad tjänst av typen **HDInsight** (egna HDInsight-kluster) eller **HDInsightOnDemand** (Data Factory skapar ett HDInsight-kluster på begäran). Konfigurera sedan anpassad aktivitet toouse hello länkad HDInsight-tjänst. Se [Använd Azure HDInsight länkade tjänster](#use-hdinsight-compute-service) information om hur du använder Azure HDInsight toorun hello anpassad aktivitet.

> [!IMPORTANT]
> - hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster. En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster. Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.
> - Det är inte möjligt toouse en Data Management Gateway från en anpassad aktivitet tooaccess lokala datakällor. För närvarande [Data Management Gateway](data-factory-data-management-gateway.md) stöder endast hello kopia och lagrade proceduraktiviteten i Data Factory.   

## <a name="walkthrough-create-a-custom-activity"></a>Genomgång: skapa en anpassad aktivitet
### <a name="prerequisites"></a>Krav
* Visual Studio 2012/2013/2015
* Ladda ned och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/)

### <a name="azure-batch-prerequisites"></a>Krav för Azure Batch
I hello genomgången kan du köra dina anpassade .NET-aktiviteter med hjälp av Azure Batch som en resurs för beräkning. **Azure Batch** är en plattform för tjänsten för att köra storskaliga parallellt och högpresterande datorbearbetning (HPC) program effektivt i hello molnet. Azure Batch schemalägger beräkningsintensiva arbete toorun på en hanterad **samling av virtuella datorer**, och kan automatiskt skala beräkna resurser toomeet hello behoven hos dina jobb. Se [grunderna i Azure Batch] [ batch-technical-overview] artikel en detaljerad översikt över hello Azure Batch-tjänsten.

Hello genomgång kan skapa ett Azure Batch-konto med en pool för virtuella datorer. Här är hello steg:

1. Skapa en **Azure Batch-kontot** med hello [Azure-portalen](http://portal.azure.com). Se [skapa och hantera Azure Batch-kontot] [ batch-create-account] artikel anvisningar.
2. Notera hello Azure Batch-kontonamn, kontonyckel, URI: N och namnet på programpoolen. Du behöver dem toocreate en Azure Batch länkad tjänst.
    1. Hello hemsidan för Azure Batch-kontot som du ser en **URL** i hello följande format: `https://myaccount.westus.batch.azure.com`. I det här exemplet **MITTKONTO** är hello namnet på hello Azure Batch-kontot. URI som du använder i hello länkade tjänstdefinitionen är hello URL utan hello namnet på hello-kontot. Till exempel: `https://<region>.batch.azure.com`.
    2. Klicka på **nycklar** på hello vänstra menyn och kopiera hello **primära ÅTKOMSTNYCKELN**.
    3. toouse en befintlig adresspool klickar du på **pooler** på hello-menyn och Skriv ner hello **ID** för hello pool. Om du inte har en befintlig adresspool kan du flytta toohello nästa steg.     
2. Skapa en **Azure Batch-pool**.

   1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **Bläddra** i hello vänstra menyn och klickar på **Batch-konton**.
   2. Välj din Azure Batch-kontot tooopen hello **Batch-kontot** bladet.
   3. Klicka på **pooler** panelen.
   4. I hello **pooler** bladet, klickar du på knappen Lägg till på hello verktygsfältet tooadd poolen.
      1. Ange ett ID för hello-pool (Pool-ID). Obs hello **ID för hello pool**; du behöver den när du skapar hello Data Factory-lösning.
      2. Ange **Windows Server 2012 R2** för hello Operativsystemsfamilj inställningen.
      3. Välj en **nodprisnivå**.
      4. Ange **2** som värde för hello **mål dedikerade** inställningen.
      5. Ange **2** som värde för hello **maximalt antal uppgifter per nod** inställningen.
   5. Klicka på **OK** toocreate hello poolen.
   6. Skriv ner hello **ID** för hello pool. 



### <a name="high-level-steps"></a>Anvisningar
Här följer hello två övergripande steg du utför som en del av den här genomgången: 

1. Skapa en anpassad aktivitet som innehåller enkla data transformation/standardbearbetningslogiken.
2. Skapa ett Azure data factory med en pipeline som använder hello anpassad aktivitet.

### <a name="create-a-custom-activity"></a>Skapa en anpassad aktivitet
toocreate en anpassad aktivitet för .NET, skapa en **.NET-klassbibliotek** projekt med en klass som implementerar som **IDotNetActivity** gränssnitt. Det här gränssnittet har endast en metod: [kör](https://msdn.microsoft.com/library/azure/mt603945.aspx) och signaturen är:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


hello-metoden har fyra parametrar:

- **linkedServices**. Den här egenskapen är en enumerable lista över datalager som är länkade tjänster som refereras av in-/ utdata-datauppsättningar för hello aktiviteten.   
- **datauppsättningar**. Den här egenskapen är en enumerable lista av in-/ utdata-datauppsättningar för hello aktiviteten. Du kan använda den här parametern tooget hello platser och scheman som definieras av inkommande och utgående datauppsättningar.
- **aktiviteten**. Denna egenskap representerar hello pågående aktivitet. Det kan vara används tooaccess utökade egenskaper som är associerade med hello anpassad aktivitet. Se [åtkomst utökade egenskaper](#access-extended-properties) mer information.
- **loggaren**. Det här objektet kan du skriva debug kommentarer som yta i hello användarinloggning för hello pipelinen.

hello-metoden returnerar en ordlista som kan vara tillsammans används toochain anpassade aktiviteter i framtiden hello. Den här funktionen har inte implementerats ännu, så returnerar ett tomt Ordbok från hello-metod.  

### <a name="procedure"></a>Procedur
1. Skapa en **.NET-klassbibliotek** projekt.
   <ol type="a">
     <li>Starta <b>Visual Studio 2017</b> eller <b>Visual Studio 2015</b> eller <b>Visual Studio 2013</b> eller <b>Visual Studio 2012</b>.</li>
     <li>Klicka på <b>filen</b>, peka för<b>ny</b>, och klicka på <b>projekt</b>.</li>
     <li>Expandera <b>Mallar</b> och välj <b>Visual C#</b>. I den här genomgången ska du använda C#, men du kan använda alla .NET språk toodevelop hello anpassad aktivitet.</li>
     <li>Välj <b>klassbiblioteket</b> hello listan över projekttyper på hello rätt. Välj i VS 2017 <b>Class Library (.NET Framework)</b> </li>
     <li>Ange <b>MyDotNetActivity</b> för hello <b>namn</b>.</li>
     <li>Välj <b>C:\ADFGetStarted</b> för hello <b>plats</b>.</li>
     <li>Klicka på <b>OK</b> toocreate hello projektet.</li>
   </ol>
2.Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.
3. Kör följande kommando tooimport hello i hello Package Manager-konsolen, **Microsoft.Azure.Management.DataFactories**.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importera hello **Azure Storage** NuGet-paketet i toohello projekt.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Startprogram för data Factory-tjänsten kräver hello 4.3 version av WindowsAzure.Storage. Om du lägger till en referens tooa senare version av Azure Storage sammansättning i projektet anpassad aktivitet visas ett fel när hello aktiviteten körs. tooresolve hello fel finns [Appdomain isolering](#appdomain-isolation) avsnitt. 
5. Lägg till följande hello **med** instruktioner toohello källfilen i hello-projekt.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Ändra hello namnet på hello **namnområde** för**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Ändra hello namnet på hello-klass för**MyDotNetActivity** och härledd från hello **IDotNetActivity** gränssnitt som visas i följande kodutdrag hello:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementera (Lägg till) hello **kör** metod för hello **IDotNetActivity** gränssnitt toohello **MyDotNetActivity** klass och kopiera hello följande exempel kod toohello metod.

    hello räknar följande exempel hello antalet förekomster av hello sökterm (”Microsoft”) i varje blob som är associerade med en datasektorn.

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
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // toolog information, use hello logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
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
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
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
9. Lägg till följande hjälpmetoder hello: 

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

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
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
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
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

    Hej GetFolderPath metoden returnerar hello toohello sökväg som hello dataset pekar tooand hello GetFileName metoden returnerar hello namnet på hello blob/fil hello dataset pekar på. Om du havefolderPath definierar använda variabler, till exempel {Year}, {Month}, {Day} osv., hello-metoden returnerar hello sträng eftersom den utan att ersätta dem med runtime-värden. Se [åtkomst utökade egenskaper](#access-extended-properties) information om att komma åt SliceStart, SliceEnd osv.    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    hello Calculate metoden beräknar hello antal instanser av nyckelordet Microsoft i hello indatafiler (blobbar i hello mapp). hello sökordet (”Microsoft”) är hårdkodat i hello kod.
10. Kompilera hello-projekt. Klicka på **skapa** från hello-menyn och klicka på **skapa lösning**.

    > [!IMPORTANT]
    > Ange 4.5.2 version av .NET Framework som hello Målversionen av framework för ditt projekt: Högerklicka på hello-projektet och klicka på **egenskaper** tooset hello Målversionen av framework. Data Factory stöder inte anpassade aktiviteter kompileras mot .NET Framework-versioner senare än 4.5.2.

11. Starta **Windows Explorer**, och navigera för**bin\debug** eller **bin\release** beroende på hello typ av version.
12. Skapa en zip-fil **MyDotNetActivity.zip** som innehåller alla hello-binärfiler i hello <project folder>\bin\Debug mapp. Inkludera hello **MyDotNetActivity.pdb** filen så att du får ytterligare information, till exempel radnumret i hello källkod som har orsakat problemet hello om ett fel uppstod. 

    > [!IMPORTANT]
    > Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.

    ![Binära filer](./media/data-factory-use-custom-activities/Binaries.png)
14. Skapa en blobbbehållare med namnet **customactivitycontainer** om den inte redan finns. 
15. Ladda upp MyDotNetActivity.zip som en blob toohello customactivitycontainer i en **allmänna** Azure blob-lagring (hot inte/lågfrekvent Blob-lagring) som refereras av AzureStorageLinkedService.  

> [!IMPORTANT]
> Om du lägger till .NET aktivitet projektet tooa lösningen i Visual Studio som innehåller ett projekt som Data Factory och lägga till en referens too.NET aktivitet projekt från hello Data Factory-programprojekt, behöver du inte tooperform hello två sista stegen för att skapa manuellt hello zip-filen och överföra den toohello allmänna Azure blob storage. När du publicerar Data Factory-enheter med hjälp av Visual Studio, utförs automatiskt de här stegen av hello publiceringsprocessen. Mer information finns i [Data Factory-projekt i Visual Studio](#data-factory-project-in-visual-studio) avsnitt.

## <a name="create-a-pipeline-with-custom-activity"></a>Skapa en pipeline med anpassad aktivitet
Du har skapat en anpassad aktivitet och överföra hello zip-filen med binärfiler tooa blob-behållare i en **allmänna** Azure Storage-konto. I det här avsnittet skapar du ett Azure data factory med en pipeline som använder hello anpassad aktivitet.

hello inkommande datamängden för hello anpassad aktivitet representerar blobbar (filer) i hello customactivityinput mapp för adftutorial behållare i hello blob storage. Hej utdatauppsättningen för hello aktivitet representerar utdata blobbar i hello customactivityoutput mapp för adftutorial behållare i hello blob storage.

Skapa **fil.txt** fil med hello följande innehåll och ladda upp det för**customactivityinput** för hello **adftutorial** behållare. Skapa hello adftutorial behållaren om den inte redan finns. 

```
test custom activity Microsoft test custom activity Microsoft
```

hello inkommande mappen motsvarar tooa segment i Azure Data Factory även om hello-mappen har två eller flera filer. När varje segment bearbetas av hello pipeline går hello anpassad aktivitet igenom alla hello BLOB i hello inkommande mapp för sektorn.

Du ser en utdatafil med i hello adftutorial\customactivityoutput mapp med en eller flera rader (samma som antal blobbar i hello inkommande mapp):

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


Här är hello steg du utför i det här avsnittet:

1. Skapa en **datafabriken**.
2. Skapa **länkade tjänster** för hello Azure Batch-pool för virtuella datorer på vilka hello anpassad aktivitet körs och hello Azure Storage där hello-i/o-BLOB.
3. Skapa ingående och utgående **datauppsättningar** som representerar indata och utdata för hello anpassad aktivitet.
4. Skapa en **pipeline** som använder hello anpassad aktivitet.

> [!NOTE]
> Skapa hello **fil.txt** och ladda upp den tooa blob-behållaren om du inte redan gjort det.. Se anvisningarna i föregående avsnitt hello.   

### <a name="step-1-create-hello-data-factory"></a>Steg 1: Skapa hello data factory
1. Efter inloggningen toohello Azure-portalen hello följande steg:
   1. Klicka på **ny** på hello vänstra menyn.
   2. Klicka på **Data + analys** i hello **ny** bladet.
   3. Klicka på **Datafabriken** på hello **dataanalys** bladet.
   
    ![Nya Azure Data Factory-menyn](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. I hello **nya datafabriken** bladet ange **CustomActivityFactory** för hello namn. hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: **datafabriksnamnet ”CustomActivityFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel **yournameCustomActivityFactory**) och försök att skapa igen.

    ![Nya Azure Data Factory-bladet](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Klicka på **RESURSGRUPPENS namn**, och välj en befintlig resursgrupp eller skapa en resursgrupp.
4. Kontrollera att du använder rätt hello **prenumeration** och **region** där du vill att hello data factory toobe skapas.
5. Klicka på **skapa** på hello **nya data factory** bladet.
6. Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen.
7. När hello datafabriken har skapats, visas hello Data Factory blad som visar hello innehållet i hello data factory.
    
    ![Bladet Datafabrik](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>Steg 2: Skapa länkade tjänster
Länkade tjänster länka datalager eller compute services tooan Azure data factory. I det här steget kan länka du ditt Azure Storage-konto och Azure Batch-kontot tooyour data factory.

#### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
1. Klicka på hello **författare och distribuera** panelen på hello **DATAFABRIKEN** bladet för **CustomActivityFactory**. Du kan se hello Data Factory-redigeraren.
2. Klicka på **Nytt datalager** hello kommandofältet och välj **Azure storage**. Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.
    
    ![Nytt datalager - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Ersätt `<accountname>` med namnet på ditt Azure storage-konto och `<accountkey>` med åtkomstnyckeln för hello Azure storage-konto. toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

    ![Azure Storage tyckte om tjänsten](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

#### <a name="create-azure-batch-linked-service"></a>Skapa Azure Batch länkad tjänst
1. Hello Data Factory-redigeraren, klicka på **... Flera** på hello kommandofältet klickar du på **nya beräkning**, och välj sedan **Azure Batch** hello-menyn.

    ![Nya beräknings - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. Gör följande ändringar toohello JSON-skript hello:

   1. Ange Azure Batch-kontonamnet för hello **accountName** egenskapen. Hej **URL** från hello **Azure Batch-kontoblad** i hello följande format: `http://accountname.region.batch.azure.com`. För hello **batchUri** egenskap i hello JSON, måste du tooremove `accountname.` från hello URL och Använd hello `accountname` för hello `accountName` JSON-egenskapen.
   2. Ange hello Azure Batch-kontonyckel för hello **accessKey** egenskapen.
   3. Ange hello namn för hello-pool som du har skapat som en del av krav för hello **poolName** egenskapen. Du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.
   4. Ange Azure Batch-URI för hello **batchUri** egenskapen. Exempel: `https://westus.batch.azure.com`.  
   5. Ange hello **AzureStorageLinkedService** för hello **linkedServiceName** egenskapen.

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       För hello **poolName** egenskap, du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.

      > [!IMPORTANT]
      > hello Data Factory-tjänsten stöder inte ett alternativ på begäran för Azure Batch som för HDInsight. Du kan bara använda din egen Azure Batch-pool i ett Azure data factory.   
    

### <a name="step-3-create-datasets"></a>Steg 3: Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent indata och utdata.

#### <a name="create-input-dataset"></a>Skapa indatauppsättning
1. I hello **Editor** hello Data Factory, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage** hello nedrullningsbara menyn.
2. Ersätt hello JSON i hello högra med hello följande JSON-fragment:

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
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

   Du skapar en pipeline senare i den här genomgången med starttid: 2016-11-16T00:00:00Z-och Sluttid: 2016-11-16T05:00:00Z. Det är schemalagda tooproduce data varje timma, så att det finns fem i/o-segment (mellan **00**: 00:00 -> **05**: 00:00).

   Hej **frekvens** och **intervall** hello inkommande dataset är inställd för**timme** och **1**, vilket innebär att hello indata sektorn är tillgängliga varje timme. I det här exemplet är det hello samma fil (fil.txt) i hello intputfolder.

   Här följer hello starttider för varje segment som representeras av SliceStart systemvariabel i hello ovan JSON-fragment.
3. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset**. Bekräfta att du ser hello **tabell skapas har** meddelandet på hello namnlist hello Editor.

#### <a name="create-an-output-dataset"></a>Skapa en datauppsättning för utdata
1. I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage**.
2. Ersätt hello JSON-skript i hello högra med hello följande JSON-skript:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
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

     Platsen är **adftutorial/customactivityoutput/** och utdata-filnamn är åååå-MM-dd-HH.txt där åååå-MM-dd-HH är hello år, månad, datum och timmes hello segment som skapas. Se [för utvecklare] [ adf-developer-reference] mer information.

    En blob/utdatafil genereras för varje inkommande segment. Här är hur en utdatafil heter för varje segment. Alla hello utdatafiler skapas i en utdatamapp: **adftutorial\customactivityoutput**.

   | Sektorn | Starttid | Utdatafil |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Kom ihåg att alla hello-filer i en inkommande mapp är en del av ett segment med hello starttiderna som nämns ovan. När den här sektorn behandlas hello anpassad aktivitet söker igenom varje fil och ger en rad i hello utdatafil med hello antal förekomster av sökterm (”Microsoft”). Om det finns tre filer i hello inputfolder, det finns tre rader i hello utdatafil för varje timme segment: 2016-11-16-00.txt 2016-11-16:01:00:00.txt, etc.
3. toodeploy hello **OutputDataset**, klickar du på **distribuera** i hello kommandofält.

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a>Skapa och köra en pipeline som använder hello anpassad aktivitet
1. Hello Data Factory-redigeraren, klicka på **... Flera**, och välj sedan **ny pipeline** i hello kommandofält. 
2. Ersätt hello JSON i hello högra med hello följande JSON-skript:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Observera följande punkter hello:

   * **Concurrency** har angetts för**2** så att två segment bearbetas parallellt med 2 virtuella datorer i hello Azure Batch-pool.
   * Det finns en aktivitet i området för hello aktiviteter och är av typen: **DotNetActivity**.
   * **AssemblyName** anges toohello namnet på hello DLL: **MyDotnetActivity.dll**.
   * **EntryPoint** har angetts för**MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** har angetts för**AzureStorageLinkedService** som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen. Om du använder olika Azure Storage-konton för i/o filer och hello zip-filen för anpassad aktivitet du skapar en annan länkad Azure Storage-tjänst. Den här artikeln förutsätter att du använder hello samma Azure Storage-konto.
   * **PackageFile** har angetts för**customactivitycontainer/MyDotNetActivity.zip**. Den är i hello format: containerforthezip/nameofthezip.zip.
   * hello anpassad aktivitet tar **InputDataset** som indata och **OutputDataset** som utdata.
   * Hej linkedServiceName-egenskapen för hello anpassad aktivitet pekar toohello **AzureBatchLinkedService**, som talar om Azure Data Factory som hello anpassade aktiviteten måste toorun på Azure Batch virtuella datorer.
   * **isPaused** egenskapen för**FALSKT** som standard. hello pipelinen körs direkt i det här exemplet eftersom hello segment starta i hello tidigare. Du kan ange den här egenskapen tootrue toopause hello pipeline och ange den bakre toofalse toorestart.
   * Hej **starta** tid och **end** tider är **fem** timmar från varandra och segment produceras varje timma, så att fem segment som produceras av hello pipeline.
3. toodeploy hello pipeline, klickar du på **distribuera** i hello kommandofält.

### <a name="monitor-hello-pipeline"></a>Övervakaren hello pipeline
1. I hello Data Factory-bladet i hello Azure-portalen klickar du på **Diagram**.

    ![Ikonen Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Klicka på hello OutputDataset i hello diagramvyn.

    ![Diagramvy](./media/data-factory-use-custom-activities/diagram.png)
3. Du bör se att hello fem utdata segment är i tillståndet för hello Ready. Om de inte är i tillståndet för hello Ready har inte de producerats ännu. 

   ![Utdata segment](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Kontrollera att hello utdatafilerna genereras i hello blob storage i hello **adftutorial** behållare.

   ![utdata från anpassad aktivitet][image-data-factory-ouput-from-custom-activity]
5. Om du öppnar hello utdatafilen, bör du se hello utdata liknande toohello följande utdata:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. Använd hello [Azure-portalen] [ azure-preview-portal] eller Azure PowerShell-cmdlets toomonitor din data factory, rörledningar och datauppsättningar. Du kan ta emot meddelanden från hello **ActivityLogger** i hello koden för hello anpassad aktivitet i hello-loggar (särskilt användar-0.log) som du kan hämta från hello-portalen eller med hjälp av cmdlet: ar.

   ![Hämta loggar från anpassad aktivitet][image-data-factory-download-logs-from-custom-activity]

Se [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md) detaljerade anvisningar för att övervaka datamängd och pipelinor.      

## <a name="data-factory-project-in-visual-studio"></a>Data Factory-projekt i Visual Studio  
Du kan skapa och publicera Data Factory-enheter med hjälp av Visual Studio istället för att använda Azure-portalen. För detaljerad information om hur du skapar och publicerar Data Factory-enheter med hjälp av Visual Studio finns [skapa din första pipeline med Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) och [kopiera data från Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) artiklar.

Hello följande ytterligare steg om du skapar Data Factory-projekt i Visual Studio:
 
1. Lägg till hello Data Factory-projekt toohello Visual Studio-lösning som innehåller hello anpassad aktivitet projekt. 
2. Lägg till en referens toohello .NET aktivitet projektet från hello Data Factory-projekt. Högerklicka på Data Factory-projektet, peka för**Lägg till**, och klicka sedan på **referens**. 
3. I hello **Lägg till referens** dialogrutan, Välj hello **MyDotNetActivity** projektet och klicka på **OK**.
4. Skapa och publicera hello lösning.

    > [!IMPORTANT]
    > När du publicerar Data Factory-entiteter, en zip-fil skapas automatiskt för dig och är överförda toohello blob-behållare: customactivitycontainer. Om hello blob-behållaren inte finns, skapas den automatiskt för.  


## <a name="data-factory-and-batch-integration"></a>Data Factory och Batch-integrering
hello Data Factory-tjänsten skapar ett jobb i Azure Batch med hello namn: **poolname adf: jobbet xxx**. Klicka på **jobb** hello vänstra menyn. 

![Azure Data Factory - batchjobb](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

En aktivitet skapas för varje aktivitet kör för ett segment. Om det finns fem segment-redo toobe bearbetas, skapas fem uppgifter i jobbet. Om det finns flera compute-noder i hello Batch-pool, kan två eller flera segment köras parallellt. Om hello maximala uppgifter per compute-nod har angetts för > 1, du kan också ha fler än ett segment som körs på hello samma beräkning.

![Azure Data Factory - jobbet batchaktiviteter](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

hello följande diagram illustrerar hello relationen mellan Azure Data Factory och Batch-aktiviteter.

![Data Factory & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Felsöka fel
Felsökning består av några grundläggande tekniker:

1. Om du ser hello följande fel kan kanske du använder en aktiv/lågfrekvent blob-lagring i stället för med ett allmänt Azure blob storage. Överför hello zip-filen tooa **allmänna Azure Storage-konto**. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. Om du ser följande fel hello bekräfta hello namnet på hello klass i hello CS matchar hello det angivna filnamnet för hello **EntryPoint** egenskap i hello pipeline-JSON. I hello genomgången hello klassen heter: MyDotNetActivity och hello EntryPoint i hello JSON: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Om hello namnen stämmer överens, bekräfta att alla hello binärfiler hello **rotmappen** av hello zip-filen. När du öppnar hello zip-filen som är bör du se alla hello-filer i rotmappen för hello, inte i alla undermappar.   
3. Om hello inkommande sektorn inte har angetts för**klar**, bekräfta att hello inkommande mappstrukturen är korrekt och **fil.txt** finns i hello inkommande mappar.
3. I hello **kör** metod för din anpassade aktivitet, Använd hello **IActivityLogger** objekt toolog information som hjälper dig att felsöka problem. hälsningsmeddelande loggas visas i loggfilerna för hello användare (en eller flera filer med namnet: användaren 0.log, användaren 1.log, användaren 2.log osv.).

   I hello **OutputDataset** bladet, klickar du på hello sektorn toosee hello **DATASEKTORN** bladet för den sektorn. Du ser **aktivitetskörningar** för den sektorn. Du bör se en aktivitet som körs för hello sektorn. Om du klickar på Kör i kommandofältet hello kan du starta en annan aktivitet som körs för hello samma segment.

   När du klickar på hello aktivitet kör du ser hello **kör AKTIVITETSINFORMATION** bladet med en lista över loggfilerna. Du ser loggade meddelanden i hello user_0.log-filen. När ett fel uppstår finns tre aktivitetskörningar eftersom hello återförsöksvärde anges too3 i hello pipeline/aktivitets-JSON. När du klickar på hello aktivitet kör se du hello loggfiler att du kan granska tootroubleshoot hello fel.

   Hello loggfiler, klicka på hello **användare 0.log**. Hello högra panelen är hello resultatet av att använda hello **IActivityLogger.Write** metod. Om du inte ser alla meddelanden, kontrollera om du har flera loggfiler med namnet: user_1.log, user_2.log osv. Annars kanske hello kod inte när hello senast loggade meddelandet.

   Du kan också kontrollera **system 0.log** för alla system felmeddelanden och undantag.
4. Inkludera hello **PDB** filen i hello zip-filen så att hello felinformation har information som **anropsstacken** när ett fel inträffar.
5. Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.
6. Se till att hello **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) och **packageLinkedService** (ska peka toohello **allmänna**Azure blob-lagring som innehåller hello zip-filen) har angetts toocorrect värden.
7. Om du har löst ett fel och vill tooreprocess hello segment Högerklicka hello sektor i hello **OutputDataset** bladet och klicka på **kör**.
8. Om du ser följande fel hello använder du hello Azure Storage-paketet med version > 4.3.0. Startprogram för data Factory-tjänsten kräver hello 4.3 version av WindowsAzure.Storage. Se [Appdomain isolering](#appdomain-isolation) avsnittet för en arbete runt om du måste använda hello senare version av Azure Storage-sammansättningen. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    Om du kan använda hello 4.3.0 versionen av Azure Storage, ta bort hello befintliga referens tooAzure lagring paketet med version > 4.3.0. Kör sedan följande kommando från NuGet Package Manager-konsolen hello. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Skapa hello-projekt. Ta bort Azure.Storage sammansättningen av version > 4.3.0 från hello bin\Debug mapp. Skapa en zip-fil med binärfiler och hello PDB-filen. Ersätta hello gamla zip-filen med den här i hello blob-behållaren (customactivitycontainer). Kör hello sektorer som inte kunde (Högerklicka segment och klicka på Kör).   
8. hello anpassad aktivitet använder inte hello **app.config** filen från paketet. Därför om koden läser eventuella anslutningssträngar från hello konfigurationsfil, fungerar det inte vid körning. Hej bäst när med hjälp av Azure Batch är toohold alla hemligheter i en **Azure KeyVault**, använda en certifikatbaserad service principal tooprotect hello **keyvault**, och distribuera hello certifikat tooAzure Batch-pool. hello kan .NET anpassad aktivitet sedan använda hemligheter från hello KeyVault vid körning. Den här lösningen är en allmän lösning och kan skala tooany typ av hemlighet, inte bara anslutningssträngen.

   Det finns en enklare lösning (men inte rekommenderas): du kan skapa en **Azure SQL länkade tjänsten** med strängen anslutningsinställningar, skapa en datamängd som använder hello länkad tjänst och kedjan hello datamängd som en dummy inkommande datauppsättning toohello anpassad .NET-aktivitet. Du kan sedan åtkomst hello länkade tjänstens anslutningssträngen i hello anpassad Aktivitetskod.  

## <a name="update-custom-activity"></a>Uppdatera anpassad aktivitet
Om du uppdaterar hello koden för hello anpassad aktivitet skapar det och överför hello zip-fil som innehåller nya binärfiler toohello blob-lagring.

## <a name="appdomain-isolation"></a>AppDomain isolering
Se [mellan AppDomain-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) som visar hur toocreate en anpassad aktivitet som inte är begränsad tooassembly-versioner som används av hello Data Factory programstart (exempel: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x osv.).

## <a name="access-extended-properties"></a>Åtkomst till utökade egenskaper
Du kan deklarera utökade egenskaper i hello aktivitets-JSON som visas i följande exempel hello:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


Det finns två utökade egenskaper i hello exempelvis: **SliceStart** och **DataFactoryName**. hello-värdet för SliceStart baseras på hello SliceStart systemvariabeln. Se [systemvariabler](data-factory-functions-variables.md) för en lista över variabler system som stöds. hello-värdet för DataFactoryName är hårdkodat tooCustomActivityFactory.

tooaccess dessa utökade egenskaper i hello **Execute** metod, Använd koden liknande toohello följande kod:

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Automatisk skalning av Azure Batch
Du kan också skapa en Azure Batch-pool med **Autoskala** funktion. Du kan till exempel skapa en azure batch-pool med 0 dedikerade virtuella datorer och en Autoskala formel baserat på hello antal väntande aktiviteter. 

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

## <a name="use-hdinsight-compute-service"></a>Använda HDInsight beräknings-tjänst
I hello genomgången används Azure Batch beräkning toorun hello anpassad aktivitet. Du kan också använda egna Windows-baserade HDInsight-kluster eller har Data Factory skapar på-begäran-Windows-baserade HDInsight-kluster och har hello anpassad aktivitet som körs på hello HDInsight-kluster. Här följer hello anvisningar för att använda ett HDInsight-kluster.

> [!IMPORTANT]
> hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster. En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster. Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.
 

1. Skapa en Azure HDInsight länkad tjänst.   
2. Använd HDInsight länkade tjänsten i stället för **AzureBatchLinkedService** i hello pipeline-JSON.

Om du vill tootest med hello genomgången ändra **starta** och **end** tidsgränsen för hello pipelinen, så att du kan testa hello scenario med hello Azure HDInsight-tjänst.

#### <a name="create-azure-hdinsight-linked-service"></a>Skapa en Azure HDInsight-länkad tjänst
hello Azure Data Factory-tjänsten stöder skapandet av ett kluster på begäran och använder det tooprocess inkommande tooproduce utdata. Du kan också använda ditt eget kluster tooperform hello samma. När du använder HDInsight-kluster på begäran, skapas ett kluster för varje segment. Om du använder egna HDInsight-kluster hello klustret är klar hello tooprocess sektorn omedelbart. Därför när du använder kluster på begäran måste kanske du inte visas hello utdata så snabbt som när du använder ditt eget kluster.

> [!NOTE]
> Vid körning körs en instans av en .NET-aktiviteten bara på en worker-nod i hello HDInsight-klustret. inte kan skaländras toorun på flera noder. Flera instanser av .NET-aktiviteten kan köras parallellt på olika noder i hello HDInsight-kluster.
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a>toouse ett HDInsight-kluster på begäran
1. I hello **Azure-portalen**, klickar du på **författare och distribution** i hello Data Factory-startsidan.
2. I hello Data Factory-redigeraren, klickar du på **nya beräkning** från hello kommandot och välj **på begäran HDInsight-kluster** hello-menyn.
3. Gör följande ändringar toohello JSON-skript hello:

   1. För hello **clusterSize** -egenskapen anger hello storleken på hello HDInsight-kluster.
   2. För hello **timeToLive** -egenskapen anger hur länge hello kunden kan vara inaktiv innan den tas bort.
   3. För hello **version** egenskap, ange hello HDInsight-version som du vill använda toouse. Om du utesluter den här egenskapen används hello senaste versionen.  
   4. För hello **linkedServiceName**, ange **AzureStorageLinkedService**.

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster. En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster. Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.

4. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

##### <a name="toouse-your-own-hdinsight-cluster"></a>toouse egna HDInsight-kluster:
1. I hello **Azure-portalen**, klickar du på **författare och distribution** i hello Data Factory-startsidan.
2. I hello **Data Factory-redigeraren**, klickar du på **nya beräkning** från hello kommandot och välj **HDInsight-kluster** hello-menyn.
3. Gör följande ändringar toohello JSON-skript hello:

   1. För hello **clusterUri** egenskap, ange hello URL för din HDInsight. Till exempel: https://<clustername>.azurehdinsight.net/     
   2. För hello **användarnamn** egenskap, ange hello användarnamn som har åtkomst toohello HDInsight-kluster.
   3. För hello **lösenord** egenskap, ange hello lösenord för hello användaren.
   4. För hello **LinkedServiceName** egenskap, ange **AzureStorageLinkedService**.
4. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

Se [Compute länkade tjänster](data-factory-compute-linked-services.md) mer information.

I hello **pipeline-JSON**, använda HDInsight (på begäran eller egna) länkade tjänsten:

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a>Skapa en anpassad aktivitet med hjälp av .NET SDK
I hello genomgången i den här artikeln skapa en datafabrik med en pipeline som använder hello anpassad aktivitet genom att använda hello Azure-portalen. hello följande kod visar hur toocreate hello data factory med hjälp av .NET SDK i stället. Du hittar mer information om hur du använder SDK tooprogrammatically skapa pipelines i hello [skapar en pipeline med kopieringsaktiviteten med hjälp av .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md) artikel. 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>Felsöka anpassad aktivitet i Visual Studio
Hej [Azure Data Factory - lokal miljö](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) prov på GitHub innehåller ett verktyg som gör att du toodebug anpassad .NET-aktiviteter i Visual Studio.  


## <a name="sample-custom-activities-on-github"></a>Exempel anpassade aktiviteter på GitHub
| Exempel | Vilka anpassade aktiviteten har |
| --- | --- |
| [HTTP-Data Installationshämtaren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Hämtar data från en HTTP-slutpunkt tooAzure Blob Storage med hjälp av anpassade C# aktiviteten i Data Factory. |
| [Twitter-Sentiment Analysis-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Anropar en Azure ML-modell och vill sentiment analys, bedömningen, förutsägelse osv. |
| [Kör R-skriptet](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |Anropar R-skriptet genom att köra RScript.exe på ditt HDInsight-kluster som redan har R installerat på den. |
| [Mellan AppDomain .NET-aktiviteten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Använder olika sammansättningen versioner från de som används av hello Data Factory programstart |
| [Bearbeta en modell i Azure Analysis Services](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Ombearbeta en modell i Azure Analysis Services. |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
