---
title: "aaaCreate data pipelines med hjälp av Azure .NET SDK | Microsoft Docs"
description: "Lär dig hur tooprogrammatically skapa, övervaka och hantera Azure datafabriker med hjälp av Data Factory SDK."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Skapa, övervaka och hantera Azure datafabriker med Azure Data Factory .NET SDK
## <a name="overview"></a>Översikt
Du kan skapa, övervaka och hantera Azure datafabriker genom programmering med Data Factory .NET SDK. Den här artikeln innehåller en genomgång som du kan följa toocreate ett exempelkonsol .NET-program som skapar och övervakar en datafabrik. 

> [!NOTE]
> Den här artikeln täcker inte alla hello Data Factory .NET API: et. Se [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) omfattande dokumentation för .NET-API för Data Factory. 

## <a name="prerequisites"></a>Krav
* Visual Studio 2012, 2013 eller 2015
* Hämta och installera [Azure .NET SDK](http://azure.microsoft.com/downloads/).
* Azure PowerShell. Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall Azure PowerShell på datorn. Du kan använda Azure PowerShell toocreate ett Azure Active Directory-program.

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
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
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
I hello genomgång skapar du en datafabrik med en rörledning som innehåller en kopia-aktivitet. Hej kopieringsaktiviteten kopierar data från en mapp i Azure blob storage tooanother-mappen i hello samma blob storage. 

Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory. hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.

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
4. Ersätt hello innehållet i **App.config** fil i hello-projekt med hello följande innehåll: 
    
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
5. Uppdatera värden för i hello App.Config-filen  **&lt;program-ID&gt;**,  **&lt;lösenord&gt;**,  **&lt; Prenumerations-ID&gt;**, och  **&lt;klient-ID&gt;**  med egna värden.
6. Lägg till följande hello **med** instruktioner toohello **Program.cs** fil i hello-projekt.

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

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Ersätt hello värdet för **resourceGroupName** med hello namnet på ditt Azure-resursgrupp. Du kan skapa en resursgrupp med hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.
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
9. Lägg till följande kod som skapar hello **inkommande och utgående datauppsättningar** toohello **Main** metod.

    Hej **FolderPath** för inkommande hello-blob har angetts för**adftutorial /** där **adftutorial** är hello namnet på behållaren hello i blob storage. Om den här behållaren inte finns i din Azure blob storage, skapa en behållare med namnet: **adftutorial** och ladda upp toohello textbehållaren.

    hello FolderPath för hello utdata blob har angetts till: **adftutorial/apifactoryoutput / {segmentera}** där **sektorn** är dynamiskt beräknat utifrån hello värdet för **SliceStart**(starta datum och tid för varje segment.)

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
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
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
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
    ```
10. Lägg till hello följande kod som **skapar och aktiverar en pipeline** toohello **Main** metod. Den här pipelinen har en **CopyActivity** som använder **BlobSource** som källa och **BlobSink** som mottagare.

    Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory. hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
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
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
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
    
                },
            }
        }
    });
    ```
12. Lägg till följande kod toohello hello **Main** metoden tooget hello status för en datasektorn av hello utdatauppsättningen. Det finns endast en sektor som förväntas i det här exemplet.

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
13. **(valfritt)**  Lägg till hello följande kod tooget kör information för en data-segmentet toohello **Main** metod.

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
        });
    
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
14. Lägg till följande helper-metod som används av hello hello **Main** metoden toohello **programmet** klass. Den här metoden visas en dialogruta som låter dig ange **användarnamn** och **lösenord** att du använder toolog i tooAzure portal.

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

15. Hello Solution Explorer, expandera hello projekt: **DataFactoryAPITestApp**, högerklicka på **referenser**, och klicka på **Lägg till referens**. Markera kryssrutan för `System.Configuration` sammansättningen och klicka på **OK**.
15. Skapa hello konsolprogram. Klicka på **skapa** på hello-menyn och klicka på **skapa lösning**.
16. Bekräfta att det finns minst en fil i hello adftutorial behållare i Azure blob-lagring. Om inte, skapa Emp.txt fil i anteckningar med hello efter innehåll och överför den toohello adftutorial behållare.

    ```
    John, Doe
    Jane, Doe
    ```
17. Kör hello exempel genom att klicka på **felsöka** -> **Start Debugging** på hello-menyn. När du ser hello **komma kör information om en datasektorn**, Vänta några minuter och tryck på **RETUR**.
18. Använd hello Azure portal tooverify hello data factory **APITutorialFactory** skapas med följande artefakter hello:
    * Länkade tjänsten: **AzureStorageLinkedService**
    * Datauppsättning: **DatasetBlobSource** och **DatasetBlobDestination**.
    * Pipeline: **PipelineBlobSample**
19. Kontrollera att en utdatafil skapas i hello **apifactoryoutput** mapp i hello **adftutorial** behållare.

## <a name="get-a-list-of-failed-data-slices"></a>Hämta en lista över misslyckade datasektorer 

```csharp
// Parse hello resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a>Nästa steg
Se följande exempel för att skapa en pipeline med .NET SDK som kopierar data från en Azure blob storage tooan Azure SQL database hello: 

- [Skapa en pipeline toocopy data från Blob Storage tooSQL databas](data-factory-copy-activity-tutorial-using-dotnet-api.md)
