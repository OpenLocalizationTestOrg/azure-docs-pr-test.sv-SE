---
title: "Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et med .NET Core | Microsoft Docs"
description: "En självstudiekurs som skapar en onlinedatabas och C#-konsolprogram har med hello Azure Cosmos DB DocumentDB API .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 5e7efb327252e9e73e9b2a340820eeecc6937fa5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-getting-started-with-hello-documentdb-api-and-net-core"></a>Azure Cosmos DB: Komma igång med hello DocumentDB-API och .NET Core
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Välkommen till toohello DocumentDB API: et för Azure Cosmos DB komma igång med .NET Core självstudiekursen! När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.

Vi tar upp följande:

* Skapa och ansluta tooan Azure DB som Cosmos-konto
* Konfigurera en Visual Studio-lösning
* Skapa en onlinedatabas
* Skapa en samling
* Skapa JSON-dokument
* Frågar hello samling
* Ersätta ett dokument
* Ta bort ett dokument
* Tar bort hello-databasen

Har du inte tid? Oroa dig inte! hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). Hoppa toohello [hämta hello kompletta lösningen avsnittet](#GetSolution) snabbguide.

Vill toobuild en Xamarin iOS, Android eller formulär med hjälp av programmet hello DocumentDB-API och SDK för .NET Core? Se [bygga mobila program med Xamarin och Azure Cosmos DB](mobile-apps-with-xamarin.md).

> [!NOTE]
> hello Azure Cosmos DB .NET Core SDK används i den här kursen är inte kompatibel med den universella Windowsplattformen (UWP) appar ännu. För en förhandsversion av hello .NET Core SDK som stöder UWP-appar, skicka e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

Nu sätter vi igång!

## <a name="prerequisites"></a>Krav
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/). 
    * Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.
* [Visual Studio 2017](https://www.visualstudio.com/vs/) 
    * Om du arbetar på MacOS- eller Linux, kan du utveckla .NET Core appar från kommandoraden hello genom att installera hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) för hello plattform som du väljer. 
    * Om du arbetar i Windows kan du utveckla .NET Core appar från kommandoraden hello genom att installera hello [.NET Core SDK](https://www.microsoft.com/net/core#windows). 
    * Du kan använda din egen redigerare eller hämta [Visual Studio Code](https://code.visualstudio.com/) som är kostnadsfritt och fungerar i Windows, Linux och MacOS. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS). Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Steg 2: Konfigurera en lösning i Visual Studio
1. Öppna **Visual Studio 2017** i datorn.
2. På hello **filen** väljer du **ny**, och välj sedan **projekt**.
3. I hello **nytt projekt** markerar **mallar** / **Visual C#** / **.NET Core** / **Konsolprogram (.NET Core)**, namnge projektet **DocumentDBGettingStarted**, och klicka sedan på **OK**.

   ![Skärmbild som visar hello-fönstret nytt projekt](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. I hello **Solution Explorer**, högerklickar du på **DocumentDBGettingStarted**.
5. Klicka på utan att lämna menyn hello **hantera NuGet-paket...** .

   ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. I hello **NuGet** klickar du på **Bläddra** hello överst i hello och skriver **azure documentdb** i hello sökrutan.
7. I hello resultat hitta **Microsoft.Azure.DocumentDB.Core** och på **installera**.
   hello paket-ID för hello DocumentDB klientbibliotek för .NET Core är [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Om du arbetar med en .NET Framework-version (t.ex. net461) som inte stöds av det här .NET Core NuGet-paketet använder du sedan [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) som har stöd för alla .NET Framework-versioner från och med .NET Framework 4.5.
8. Vid hello prompter Godkänn hello NuGet-paketet installationer hello licensavtalet (EULA).

Bra! Nu när vi avslutat hello inställningarna Låt oss börja skriva kod. Det finns ett färdigt kodprojekt för den här självstudiekursen i [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).

## <a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto
Lägg först till dessa refererar till toohello början av C#-appen, i hello Program.cs-filen:

```csharp
using System;

// ADD THIS PART tooYOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

> [!IMPORTANT]
> I ordning toocomplete självstudierna, Lägg till hello ovanstående beroenden.

Lägg till nedanstående två konstanter och *klientvariabeln* under den offentliga klassen *Program*.

```csharp
class Program
{
    // ADD THIS PART tooYOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Gå sedan toohello [Azure Portal](https://portal.azure.com) tooretrieve din URI och primärnyckel. hello Azure Cosmos DB URI och primärnyckel krävs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.

I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**.

Kopiera hello URI från hello-portalen och klistrar in det i `<your endpoint URI>` i hello program.cs-filen. Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `<your key>`. Om du använder hello Azure Cosmos DB emulatorn använder `https://localhost:8081` som hello slutpunkt och hello väldefinierade auktoriseringsnyckeln från [hur toodevelop med hello Azure Cosmos DB emulatorn](local-emulator.md). Se till att tooremove Hej < och > men lämna hello dubbla citattecken runt din slutpunkt och nyckel.

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram. Visar en Cosmos-databas med Azure-konto med hello hubben aktiv markerad, hello nycklar knappen är markerad i hello Azure DB som Cosmos-kontoblad och värden för hello URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i hello bladet nycklar][keys]

Vi börjar hello komma igång program genom att skapa en ny instans av hello **DocumentClient**.

Nedan hello **Main** metod, lägga till den här nya asynkrona aktiviteten kallad **GetStartedDemo**, som instantierar vår nya **DocumentClient**.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART tooYOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Lägg till hello följande kod toorun den asynkrona aktiviteten från din **Main** metod. Hej **Main** metod ska fånga undantag och skriva dem toohello-konsolen.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART tooYOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key tooexit.");
                Console.ReadKey();
        }
```

Tryck på hello **DocumentDBGettingStarted** knappen toobuild och kör programmet hello.

Grattis! Du har anslutit tooan Azure Cosmos DB konto nu ska gå vi igenom hur man arbetar med Azure Cosmos DB resurser.  

## <a name="step-4-create-a-database"></a>Steg 4: Skapa en databas
Lägg till en hjälpmetod för skrivning toohello konsolen innan du lägger till hello-kod för att skapa en databas.

Kopiera och klistra in hello **WriteToConsoleAndPromptToContinue** under hello metoden **GetStartedDemo** metod.

```csharp
// ADD THIS PART tooYOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod för hello **DocumentClient** klass. En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod under hello-klienten. Då skapas en databas med namnet *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har skapat en Azure Cosmos DB-databas.  

## <a id="CreateColl"></a>Steg 5: Skapa en samling
> [!WARNING]
> **CreateDocumentCollectionAsync** skapar en ny samling med reserverat dataflöde, vilket får konsekvenser för priset. Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).

En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod för hello **DocumentClient** klass. En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello databasen skapas metoden. Då skapas en dokumentsamling som heter *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har skapat en Azure Cosmos DB-dokumentsamling.  

## <a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument
En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass. Dokument är användardefinierat (godtyckligt) JSON-innehåll. Vi kan nu infoga ett eller flera dokument. Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md).

Vi måste först, toocreate en **familj** klass som ska representera objekt som lagras i Azure Cosmos DB i det här exemplet. Vi kommer även att skapa underklasserna **Förälder**, **Barn**, **Husdjur** och **Adress** som används inom **Familj**. Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON. Skapa dessa klasser genom att lägga till följande interna undergrupper efter hello hello **GetStartedDemo** metod.

Kopiera och klistra in hello **familj**, **överordnade**, **underordnade**, **husdjur**, och **adress** klasser under Hej **WriteToConsoleAndPromptToContinue** metod.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key toocontinue ...");
    Console.ReadKey();
}

// ADD THIS PART tooYOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

Kopiera och klistra in hello **CreateFamilyDocumentIfNotExists** under metoden din **CreateDocumentCollectionIfNotExists** metod.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

Och infoga två dokument, ett för hello familjen Andersen och hello familjen Wakefield.

Kopiera och klistra in hello-kod som följer `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** under hello dokumentsamlingen metoden.

```csharp
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har skapat två Azure Cosmos DB-dokument.  

![Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello onlinedatabasen, hello samlingen och hello dokument används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.  hello följande exempelkod visar olika frågor – med både Azure Cosmos-Databasens SQL syntax samt LINQ – som du kan köra mot hello dokument som vi infogas i hello föregående steg.

Kopiera och klistra in hello **ExecuteSimpleQuery** under metoden din **CreateFamilyDocumentIfNotExists** metod.

```csharp
// ADD THIS PART tooYOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find hello Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // hello query is executed synchronously here, but can also be executed asynchronously via hello IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute hello same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod under hello andra dokument.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART tooYOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har skickat frågor mot en Azure Cosmos DB-samling.

hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade och hello samma logik gäller även toohello LINQ-fråga.

![Diagram som illustrerar hello omfånget och innebörden av hello-fråga som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling. ”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer. DocumentDB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.

## <a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument
Azure Cosmos DB har stöd för ersättning av JSON-dokument.  

Kopiera och klistra in hello **ReplaceFamilyDocument** under metoden din **ExecuteSimpleQuery** metod.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello Frågekörningen metoden. När du ersätter dokumentet hello hello körs samma fråga igen tooview hello ändra dokumentet.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
// Update hello Grade of hello Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har ersatt ett Azure Cosmos DB-dokument.

## <a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument
Azure Cosmos DB har stöd för borttagning av JSON-dokument.  

Kopiera och klistra in hello **DeleteFamilyDocument** under metoden din **ReplaceFamilyDocument** metod.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello andra Frågekörningen metoden.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooCODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har tagit bort ett Azure Cosmos DB-dokument.

## <a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen
Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello dokumentet metoden ta bort toodelete hello hela databasen och alla underordnade resurser.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART tooCODE
// Clean up/delete hello database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.

Grattis! Du har tagit bort en Azure Cosmos DB-databas.

## <a id="Run"></a>Steg 11: Kör C#-konsolappen i sin helhet!
Tryck på hello **DocumentDBGettingStarted** knapp i Visual Studio toobuild hello appen i felsökningsläge.

Du bör se hello utdata från get started-appens i hello konsolfönstret. hello utdata visar hello resultaten av hello frågor som vi har lagt till och bör matcha hello exempeltexten nedan.

```
Created FamilyDB_oa
Press any key toocontinue ...
Created FamilyCollection_oa
Press any key toocontinue ...
Created Family Andersen.1
Press any key toocontinue ...
Created Family Wakefield.7
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key tooexit.
```

Grattis! Du har slutfört hello självstudier och har en fungerande C#-konsolprogram!

## <a id="GetSolution"></a>Hämta hello fullständiga lösningen till självstudiekursen
toobuild hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).
* En [Azure Cosmos DB konto][create-documentdb-dotnet.md#create-account].
* Hej [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) lösningen som finns på GitHub.

toorestore hello referenser toohello DocumentDB API: et för Azure Cosmos DB .NET Core SDK i Visual Studio högerklickar du på hello **GetStarted** lösningen i Solution Explorer och klicka sedan på **aktivera NuGet-Paketåterställning**. I Program.cs-filen för hello uppdatera värdena för hello EndpointUrl och AuthorizationKey enligt beskrivningen i [ansluta tooan Azure Cosmos DB konto](#Connect).

## <a name="next-steps"></a>Nästa steg
* Behöver du en mer komplex ASP.NET MVC-självstudiekurs? Se [självstudiekurs om ASP.NET MVC: utveckling av Webbappar med Azure Cosmos DB](documentdb-dotnet-application.md).
* Vill toodevelop en Xamarin iOS, Android eller formulär med hjälp av programmet hello DocumentDB API för Azure Cosmos DB .NET Core SDK? Se [bygga mobila program med Xamarin och Azure Cosmos DB](mobile-apps-with-xamarin.md).
* Vill tooperform skala och prestandatester med Azure Cosmos DB? Se [prestanda och skalning testa med Azure Cosmos DB](performance-testing.md)
* Lär dig hur för[övervakaren Azure Cosmos DB begäranden, användning och lagring](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* toolearn mer om hello programmeringsmodell, se [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
