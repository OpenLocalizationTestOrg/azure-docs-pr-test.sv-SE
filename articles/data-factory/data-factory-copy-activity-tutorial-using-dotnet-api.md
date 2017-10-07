---
title: "Självstudiekurs: Skapa en pipeline med en kopieringsaktivitet med hjälp av .NET-API:et | Microsoft Docs"
description: "I den här självstudiekursen ska du skapa en Azure Data Factory-pipeline med en kopieringsaktivitet med hjälp av .NET-API:et."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Självstudiekurs: Skapa en pipeline med en kopieringsaktivitet med hjälp av .NET-API:et
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

I den här artikeln får du lära dig hur toouse [.NET API](https://portal.azure.com) toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas. Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Fullständig dokumentation för .NET-API för Data Factory finns [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).
> 
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Krav
* Gå igenom [kursen översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget en översikt över hello självstudier och fullständig hello **nödvändiga** steg.
* Visual Studio 2012, 2013 eller 2015
* Ladda ned och installera [Azure .NET SDK](http://azure.microsoft.com/downloads/)
* Azure PowerShell. Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](../powershell-install-configure.md) artikel tooinstall Azure PowerShell på datorn. Du kan använda Azure PowerShell toocreate ett Azure Active Directory-program.

### <a name="create-an-application-in-azure-active-directory"></a>Skapa ett program i Azure Active Directory
Skapa ett Azure Active Directory-program, skapa ett huvudnamn för tjänsten för hello program och tilldela den toohello **Data Factory deltagare** roll.

1. Starta **PowerShell**.
2. Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Hello kör följande kommando tooselect hello prenumeration som du vill toowork med. Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Notera **SubscriptionId** och **TenantId** från hello utdata från kommandot.

5. Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Om hello resursgruppen finns redan, anger du om tooupdate den (Y) eller låta (n).

    Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.
6. Skapa ett Azure Active Directory-program.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    Om du får följande fel hello kan ange en annan URL och kör hello kommandot igen.
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. Skapa hello AD tjänstens huvudnamn.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Lägga till tjänstens huvudnamn toohello **Data Factory deltagare** roll.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Hämta hello program-ID.

    ```PowerShell
    $azureAdApplication 
    ```
    Notera hello program-ID (applicationID) från hello-utdata.

Du bör nu ha tillgång till följande fyra värden efter de här stegen:

* Klient-ID:t
* Prenumerations-ID:t
* Program-ID:t
* Lösenord (anges i hello första kommandot)

## <a name="walkthrough"></a>Genomgång
1. Skapa ett C# .NET-konsolprogram med hjälp av Visual Studio 2012/2013/2015.
   1. Starta **Visual Studio** 2012/2013/2015.
   2. Klicka på **filen**, peka för**ny**, och klicka på **projekt**.
   3. Expandera **Mallar** och välj **Visual C#**. I den här genomgången ska du använda C#, men du kan använda valfritt .NET-språk.
   4. Välj **konsolprogram** hello listan över projekttyper på hello rätt.
   5. Ange **DataFactoryAPITestApp** för hello namn.
   6. Välj **C:\ADFGetStarted** för hello plats.
   7. Klicka på **OK** toocreate hello projektet.
2. Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.
3. I hello **Pakethanterarkonsolen**, hello följande steg:
   1. Kör hello efter kommandot tooinstall Data Factory paketet:`Install-Package Microsoft.Azure.Management.DataFactories`
   2. Kör hello efter kommandot tooinstall Azure Active Directory-paket (använder Active Directory API hello kod):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Lägg till följande hello **appSetttings** avsnittet toohello **App.config** fil. De här inställningarna används av hello hjälpmetod: **GetAuthorizationHeader**.

    Ersätt värdena för **&lt;Program-ID&gt;**, **&lt;Lösenord&gt;**, **&lt;Prenumerations-ID&gt;** och **&lt;Klient-ID&gt;** med dina egna värden.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```

5. Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projekt.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```

6. Lägg till följande kod som skapar en instans av hello **DataPipelineManagementClient** klassen toohello **Main** metod. Du kan använda det här objektet toocreate en datafabrik, en länkad tjänst, inkommande och utgående datauppsättningar och en pipeline. Du kan också använda det här objektet toomonitor segment i en dataset vid körning.

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Ersätt hello värdet för **resourceGroupName** med hello namnet på ditt Azure-resursgrupp.
   >
   > Uppdatera hello data factory (dataFactoryName) toobe unika namn. Namnet på hello data factory måste vara globalt unika. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.

7. Lägg till följande kod som skapar hello en **datafabriken** toohello **Main** metod.

    ```csharp
    // create a data factory
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
    ```

    En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata. Låt oss börja med att skapa hello data factory i det här steget.
8. Lägg till följande kod som skapar hello en **Azure länkade lagringstjänsten** toohello **Main** metod.

   > [!IMPORTANT]
   > Ersätt **storageaccountname** och **accountkey** med namnet och nyckeln för ditt Azure Storage-konto.

    ```csharp
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
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```

    Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). 

    Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.  

    Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
9. Lägg till följande kod som skapar hello en **Azure SQL länkade tjänsten** toohello **Main** metod.

   > [!IMPORTANT]
   > Ersätt **servername**, **databasename**, **username** och **password** med namnen för Azure SQL-servern, databasen, användaren och lösenordet.

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
10. Lägg till följande kod som skapar hello **inkommande och utgående datauppsättningar** toohello **Main** metod.

    ```csharp
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
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },

                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
                    },
                    Availability = new Availability()
                    {
                        Frequency = SchedulePeriod.Hour,
                        Interval = 1,
                    },
                }
            }
        });
    ```
    
    I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory. I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.

    hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.  

    På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.

    I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten. Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål. I kursen får ange du ett värde för hello filnamnet.    

    I det här steget ska du skapa en utdatauppsättning med namnet **OutputDataset**. Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService**.
11. Lägg till hello följande kod som **skapar och aktiverar en pipeline** toohello **Main** metod. I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new PipelineCreateOrUpdateParameters()
        {
            Pipeline = new Pipeline()
            {
                Name = PipelineName,
                Properties = new PipelineProperties()
                {
                    Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need tooset slice status
                    Start = PipelineActivePeriodStartTime,
                    End = PipelineActivePeriodEndTime,

                    Activities = new List<Activity>()
                    {
                        new Activity()
                        {
                            Name = "BlobToAzureSql",
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
                            TypeProperties = new CopyActivity()
                            {
                                Source = new BlobSource(),
                                Sink = new BlobSink()
                                {
                                    WriteBatchSize = 10000,
                                    WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                }
                            }
                        }
                    }
                }
            }
        });
    ```

    Observera följande punkter hello:
   
    - Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**. Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md). I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).
    - Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**. 
    - I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen. En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.  
   
    Datamängd för utdata är för närvarande vilka enheter hello schema. I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen. hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av hello pipeline.
12. Lägg till följande kod toohello hello **Main** metoden tooget hello status för en datasektorn av hello utdatauppsättningen. Endast en sektor förväntas i det här exemplet.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;

    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");        
        // wait before hello next status check
        Thread.Sleep(1000 * 12);

        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });

        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```

13. Lägg till följande kod tooget kör information för en data-segmentet toohello hello **Main** metod.

    ```csharp
    Console.WriteLine("Getting run details of a data slice");

    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
    Console.ReadKey();

    var datasliceRunListResponse = client.DataSliceRuns.List(
            resourceGroupName,
            dataFactoryName,
            Dataset_Destination,
            new DataSliceRunListParameters()
            {
                DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
            }
        );

    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }

    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```

14. Lägg till följande helper-metod som används av hello hello **Main** metoden toohello **programmet** klass.

    > [!NOTE] 
    > När du kopiera och klistra in följande kod hello, se till att hello kopierade koden är på samma nivå som hello Main-metoden hello.

    ```csharp
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
    ```

15. I hello Solution Explorer, expandera hello projekt (DataFactoryAPITestApp), högerklicka på **referenser**, och klicka på **Lägg till referens**. Markera kryssrutan för **System.Configuration**-sammansättningen. och klicka på **OK**.
16. Skapa hello konsolprogram. Klicka på **skapa** på hello-menyn och klicka på **skapa lösning**.
17. Bekräfta att det finns minst en fil i hello **adftutorial** behållare i Azure blob-lagring. Om inte, skapa **Emp.txt** fil i anteckningar med hello efter innehåll och överföra den toohello adftutorial behållare.

    ```
    John, Doe
    Jane, Doe
    ```
18. Kör hello exempel genom att klicka på **felsöka** -> **Start Debugging** på hello-menyn. När du ser hello **komma kör information om en datasektorn**, Vänta några minuter och tryck på **RETUR**.
19. Använd hello Azure portal tooverify hello data factory **APITutorialFactory** skapas med följande artefakter hello:
   * Länkad tjänst: **LinkedService_AzureStorage**
   * DataSet: **InputDataset** och **OutputDataset**.
   * Pipeline: **PipelineBlobSample**
20. Kontrollera att hello två medarbetarposter skapas i hello **tomma** tabellen i hello angetts Azure SQL-databas.

## <a name="next-steps"></a>Nästa steg
Fullständig dokumentation för .NET-API för Data Factory finns [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).

I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.

