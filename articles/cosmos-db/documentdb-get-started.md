---
title: "Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et | Microsoft Docs"
description: "En självstudiekurs som skapar en onlinedatabas och C#-konsolprogram har med hello DocumentDB-API."
keywords: "självstudier för nosql, onlinedatabas, c#-konsolprogram"
services: cosmos-db
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2017
ms.author: anhoh
ms.openlocfilehash: 65a181f715a670987492ad7815ef2ec94498e84d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a>Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Välkommen till toohello Azure Cosmos DB DocumentDB API komma igång-självstudiekurs! När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.

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

Har du inte tid? Oroa dig inte! hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). Hoppa toohello [hämta hello fullständig NoSQL självstudiekursen lösning avsnittet](#GetSolution) snabbguide.

Därefter kan knappar du använda hello röstning hello upp eller längst ned på den här sidan toogive oss feedback. Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.

Nu sätter vi igång!

## <a name="prerequisites"></a>Krav
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/). 
    * Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.
* [Visual Studio Community 2017](http://www.visualstudio.com/).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS). Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Steg 2: Konfigurera en lösning i Visual Studio
1. Öppna **Visual Studio 2017** i datorn.
2. På hello **filen** väljer du **ny**, och välj sedan **projekt**.
3. I hello **nytt projekt** markerar **mallar** / **Visual C#** / **konsolprogram**, namn projektet och klicka sedan på **OK**.
   ![Skärmbild som visar hello-fönstret nytt projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**
    
    ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. I hello **Nuget** klickar du på **Bläddra**, och skriv **azure documentdb** i hello sökrutan.
6. I hello resultat hitta **Microsoft.Azure.DocumentDB** och på **installera**.
   hello paket-ID för hello Azure Cosmos DB DocumentDB API-klientbiblioteket är [Microsoft Azure DocumentDB klientbibliotek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).
   ![Skärmbild som visar hello Nuget-menyn för att hitta Azure Cosmos DB klient-SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Om du får meddelanden om granskning av ändringar toohello lösning, klickar du på **OK**. Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.

Bra! Nu när vi avslutat hello inställningarna Låt oss börja skriva kod. Det finns ett färdigt kodprojekt för den här självstudiekursen i [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

## <a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto
Lägg först till dessa refererar till toohello början av C#-appen, i hello Program.cs-filen:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART tooYOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> Kontrollera att du lägger till hello ovanstående beroenden i ordning toocomplete hello kursen.
> 
> 

Lägg till nedanstående två konstanter och *klientvariabeln* under den offentliga klassen *Program*.

    public class Program
    {
        // ADD THIS PART tooYOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

Därefter head tillbaka toohello [Azure Portal](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel. hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.

I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**.

Kopiera hello URI från hello-portalen och klistrar in det i `<your endpoint URL>` i hello program.cs-filen. Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `<your primary key>`.

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram. Visar en Cosmos-databas med Azure-konto med hello hubben aktiv markerad, hello nycklar knappen är markerad i hello Azure DB som Cosmos-kontoblad och värden för hello URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i hello bladet nycklar][keys]

Nu ska vi börjar hello program genom att skapa en ny instans av hello **DocumentClient**.

Nedan hello **Main** metod, lägga till den här nya asynkrona aktiviteten kallad **GetStartedDemo**, som instantierar vår nya **DocumentClient**.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART tooYOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

Lägg till hello följande kod toorun den asynkrona aktiviteten från din **Main** metod. Hej **Main** metod ska fånga undantag och skriva dem toohello-konsolen.

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

Tryck på **F5** toorun ditt program. hello konsolen utdata visar hello-meddelande `End of demo, press any key tooexit.` bekräftar att hello anslutningen gjordes.  Sedan kan du stänga hello konsolfönstret. 

Grattis! Du har anslutit tooan Azure Cosmos DB konto nu ska gå vi igenom hur man arbetar med Azure Cosmos DB resurser.  

## <a name="step-4-create-a-database"></a>Steg 4: Skapa en databas
Lägg till en hjälpmetod för skrivning toohello konsolen innan du lägger till hello-kod för att skapa en databas.

Kopiera och klistra in hello **WriteToConsoleAndPromptToContinue** metod efter hello **GetStartedDemo** metod.

    // ADD THIS PART tooYOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello **DocumentClient** klass. En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello klienten har skapats. Då skapas en databas med namnet *FamilyDB*.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART tooYOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

Tryck på **F5** toorun ditt program.

Grattis! Du har skapat en Azure Cosmos DB-databas.  

## <a id="CreateColl"></a>Steg 5: Skapa en samling
> [!WARNING]
> **CreateDocumentCollectionIfNotExistsAsync** skapar en ny samling med reserverat dataflöde, vilket får konsekvenser för priset. Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient** klass. En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello databasen skapas. Då skapas en dokumentsamling som heter *FamilyCollection*.

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART tooYOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

Tryck på **F5** toorun ditt program.

Grattis! Du har skapat en Azure Cosmos DB-dokumentsamling.  

## <a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument
En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass. Dokument är användardefinierat (godtyckligt) JSON-innehåll. Vi kan nu infoga ett eller flera dokument. Om du redan har data som du vill att toostore i databasen kan du använda hello Azure Cosmos DB [datamigreringsverktyget](import-data.md) tooimport hello data i en databas.

Vi måste först, toocreate en **familj** klass som ska representera objekt som lagras i Azure Cosmos DB i det här exemplet. Vi kommer även att skapa underklasserna **Förälder**, **Barn**, **Husdjur** och **Adress** som används inom **Familj**. Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON. Skapa dessa klasser genom att lägga till följande interna undergrupper efter hello hello **GetStartedDemo** metod.

Kopiera och klistra in hello **familj**, **överordnade**, **underordnade**, **husdjur**, och **adress** klasser efter hello **WriteToConsoleAndPromptToContinue** metod.

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

Kopiera och klistra in hello **CreateFamilyDocumentIfNotExists** under metoden din **adress** klass.

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

Och infoga två dokument, ett för hello familjen Andersen och hello familjen Wakefield.

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello dokumentsamlingen.

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Tryck på **F5** toorun ditt program.

Grattis! Du har skapat två Azure Cosmos DB-dokument.  

![Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello onlinedatabasen, hello samlingen och hello dokument används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.  hello följande exempelkod visar olika frågor – med både Azure Cosmos-Databasens SQL syntax samt LINQ – som du kan köra mot hello dokument som vi infogas i hello föregående steg.

Kopiera och klistra in hello **ExecuteSimpleQuery** metod efter din **CreateFamilyDocumentIfNotExists** metod.

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

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello andra dokument har skapats.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART tooYOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Tryck på **F5** toorun ditt program.

Grattis! Du har skickat frågor mot en Azure Cosmos DB-samling.

hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade och hello samma logik gäller även toohello LINQ-fråga.

![Diagram som illustrerar hello omfånget och innebörden av hello-fråga som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling. ”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer. Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.

## <a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument
Azure Cosmos DB har stöd för ersättning av JSON-dokument.  

Kopiera och klistra in hello **ReplaceFamilyDocument** metod efter din **ExecuteSimpleQuery** metod.

    // ADD THIS PART tooYOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello Frågekörningen hello slutet av hello-metoden. När du ersätter dokumentet hello hello körs samma fråga igen tooview hello ändra dokumentet.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART tooYOUR CODE
    // Update hello Grade of hello Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Tryck på **F5** toorun ditt program.

Grattis! Du har ersatt ett Azure Cosmos DB-dokument.

## <a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument
Azure Cosmos DB har stöd för borttagning av JSON-dokument.  

Kopiera och klistra in hello **DeleteFamilyDocument** metod efter din **ReplaceFamilyDocument** metod.

    // ADD THIS PART tooYOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello andra Frågekörningen hello slutet av hello-metoden.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART tooCODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Tryck på **F5** toorun ditt program.

Grattis! Du har tagit bort ett Azure Cosmos DB-dokument.

## <a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen
Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).

Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello dokumentet kan du ta bort toodelete hello hela databasen och alla underordnade resurser.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART tooCODE
    // Clean up/delete hello database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Tryck på **F5** toorun ditt program.

Grattis! Du har tagit bort en Azure Cosmos DB-databas.

## <a id="Run"></a>Steg 11: Kör C#-konsolappen i sin helhet!
Tryck på F5 i Visual Studio toobuild hello programmet i felsökningsläge.

Du bör se get started-appens i konsolfönstret hello utdata. hello utdata visar hello resultaten av hello frågor som vi har lagt till och bör matcha hello exempeltexten nedan.

    Created FamilyDB
    Press any key toocontinue ...
    Created FamilyCollection
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

Grattis! Du har slutfört hello självstudier och har en fungerande C#-konsolprogram!

## <a id="GetSolution"></a>Hämta hello fullständiga lösningen till självstudiekursen
Om du inte har toocomplete hello stegen i den här självstudiekursen, eller bara vill toodownload hello-kodexempel kan du hämta det från [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

toobuild hello GetStarted-lösningen behöver hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).
* En [Azure Cosmos DB konto][cosmos-db-create-account].
* Hej [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) lösningen som finns på GitHub.

toorestore hello referenser toohello Cosmos Azure DB .NET SDK i Visual Studio högerklickar du på hello **GetStarted** lösningen i Solution Explorer och klicka sedan på **aktivera NuGet-Paketåterställning**. Därefter i hello App.config-fil, uppdaterar hello värdena för EndpointUrl och AuthorizationKey enligt beskrivningen i [ansluta tooan Azure Cosmos DB konto](#Connect).

Då är det bara att bygga den, så är du på väg!


## <a name="next-steps"></a>Nästa steg
* Behöver du en mer komplex ASP.NET MVC-självstudiekurs? Se [självstudiekurs om ASP.NET MVC: utveckling av Webbappar med Azure Cosmos DB](documentdb-dotnet-application.md).
* Vill tooperform skala och prestandatester med Azure Cosmos DB? Se [prestanda och skalning testa med Azure Cosmos DB](performance-testing.md)
* Lär dig hur för[övervaka Azure Cosmos DB begäranden, användning och lagring](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* toolearn mer om Azure Cosmos DB finns [Välkommen tooAzure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
