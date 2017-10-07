---
title: "Självstudiekurs om ASP.NET MVC för Azure Cosmos DB: Utveckling av webbprogram | Microsoft Docs"
description: "ASP.NET MVC självstudiekursen toocreate en MVC-webbapp med Azure Cosmos DB. Du kommer att lagra JSON- och åtkomstdata från en ”att göra”-app på Azure Websites – stegvis självstudiekurs om ASP NET MVC."
keywords: "asp.net mvc självstudier, webbprogramsutveckling, mvc-webbprogram, asp net mvc självstudier steg för steg"
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: mimig
ms.openlocfilehash: dac2a9599b395524533e6fe14983789ff095331f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="_Toc395809351"></a>Självstudiekurs om ASP.NET MVC: Utveckling av webbprogram med Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

toohighlight hur du effektivt utnyttja Azure Cosmos DB toostore och fråga JSON-dokument, den här artikeln innehåller en slutpunkt till slutpunkt genomgången visar hur toobuild en todo-app med Azure Cosmos DB. hello uppgifter lagras som JSON-dokument i Azure Cosmos-databasen.

![Skärmbild som visar hello todo-listan MVC-webbapp som skapats i denna självstudiekurs – stegvis självstudiekurs i ASP NET MVC](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

Den här genomgången visar hur toouse hello Azure Cosmos DB service toostore och komma åt data från en ASP.NET MVC-webbapp i Azure. Om du letar efter en självstudiekurs som endast fokuserar på Azure Cosmos DB och inte hello ASP.NET MVC-komponenter, se [skapa ett Azure Cosmos DB C#-konsolprogram](documentdb-get-started.md).

> [!TIP]
> Den här självstudiekursen förutsätter att du har tidigare erfarenhet av MVC i ASP.NET och Azure Websites. Om du är ny tooASP.NET eller hello [nödvändiga verktyg](#_Toc395637760), vi rekommenderar att du hämtar hello fullständiga exempelprojektet från [GitHub] [ GitHub] och hello instruktionerna i Det här exemplet. När du har skapat kan granska du den här artikeln toogain information om hello koden i hello ramen för hello-projekt.
> 
> 

## <a name="_Toc395637760"></a>Förhandskrav för den här databas-självstudien
Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 

    ELLER

    En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).
* [Visual Studio-2017](http://www.visualstudio.com/).  
* Microsoft Azure SDK för .NET för Visual Studio 2017 tillgängliga via hello Visual Studio-installationsprogrammet.

Alla hello skärmdumpar i den här artikeln har vidtagits med hjälp av Microsoft Visual Studio Community 2017. Om din dator har konfigurerats med en annan version är det möjligt att skärmbilder och alternativ inte ser riktigt likadana ut, men om du uppfyller hello ovan krav ska lösningen fungera.

## <a name="_Toc395637761"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto
Vi ska börja med att skapa ett Azure Cosmos DB-konto. Om du redan har ett konto för SQL (DocumentDB) för Azure Cosmos DB eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[skapa ett nytt ASP.NET MVC-program](#_Toc395637762).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
Vi kommer att gå igenom hur toocreate ett nytt ASP.NET MVC-program från hello grunden. 

## <a name="_Toc395637762"></a>Steg 2: Skapa ett nytt ASP.NET MVC-program

1. I Visual Studio på hello **filen** -menyn, peka för**ny**, och klicka sedan på **projekt**. Hej **nytt projekt** dialogrutan visas.

2. I hello **projekttyper** rutan Expandera **mallar**, **Visual C#**, **Web**, och välj sedan **ASP.NET-webbprogram** .

      ![Skärmbild av dialogrutan för hello nytt projekt med projekttypen hello ASP.NET-webbprogram markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. I hello **namn** rutan, hello-typnamn för hello-projekt. Den här kursen använder hello namnet ”todo”. Om du väljer toouse något annat än detta, oavsett var nämns namnområdet todo hello måste tooadjust hello tillhandahålls kod prover toouse det namn du gav ditt program. 
4. Klicka på **Bläddra** toonavigate toohello mappen där du gillar toocreate hello projektet och klicka sedan på **OK**.
   
      Hej **nytt ASP.NET-webbprogram** dialogrutan visas.
   
    ![Skärmbild som visar dialogrutan Nytt ASP.NET-webbprogram för hello med hello MVC-appmallen markerad](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. I hello mallar, markerar du **MVC**.

6. Klicka på **OK** och låt Visual Studio gör dess saker runt scaffold-teknik hello tomma ASP.NET MVC-mallen. 

          
7. När Visual Studio har skapat hello formaterad MVC-program har en tom ASP.NET-program som du kan köra lokalt.
   
    Vi hoppar över körs hello projektet lokalt nu eftersom du säkert har vi alla upptäckta hello ASP.NET ”Hello World” program. Nu ska raka tooadding Azure Cosmos DB toothis projektet och bygga vår App.

## <a name="_Toc395637767"></a>Steg 3: Lägg till Azure Cosmos DB tooyour MVC-webbapprojektet
Nu när vi har de flesta av hello ASP.NET MVC-grunder vi behöver för lösningen ska få toohello verkliga syftet med den här självstudiekursen kommer att lägga till Azure Cosmos DB tooour MVC-webbapp.

1. hello Azure Cosmos DB .NET SDK paketeras och distribueras som ett NuGet paket. tooget hello NuGet-paketet i Visual Studio kan du använda hello NuGet-Pakethanteraren i Visual Studio genom att högerklicka på projektet hello i **Solution Explorer** och sedan klicka på **hantera NuGet-paket**.
   
    ![Skärmbild som visar hello Högerklicka alternativ för hello webbapprojektet i Solution Explorer, med hantera NuGet-paket markerat.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    Hej **hantera NuGet-paket** dialogrutan visas.
2. I hello NuGet **Bläddra** skriver ***Azure DocumentDB***. (hello paketnamn inte har uppdaterats tooAzure Cosmos DB.)
   
    Hello resultat för att installera hello **Microsoft.Azure.DocumentDB av Microsoft** paketet. Detta kommer att hämta och installera hello Azure Cosmos DB paket samt alla beroenden, till exempel Newtonsoft.Json. Klicka på **OK** i hello **Preview** fönstret och **jag accepterar** i hello **godkännande av licens** fönstret toocomplete hello installation.
   
    ![Skärmdump av hello hantera NuGet-paket fönster med hello Microsoft Azure DocumentDB klientbiblioteket markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      Du kan också använda hello Package Manager-konsolen tooinstall hello paketet. toodo i så fall på hello **verktyg** -menyn klickar du på **NuGet Package Manager**, och klicka sedan på **Pakethanterarkonsolen**. Skriv hello följande i Kommandotolken hello.
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. När hello paketet har installerats bör din Visual Studio-lösning likna hello följande med två nya referenser: Microsoft.Azure.Documents.Client och Newtonsoft.Json.
   
    ![Skärmdump av hello två referenser lades till toohello JSON-dataprojektet i Solution Explorer](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <a name="_Toc395637763"></a>Steg 4: Konfigurera hello ASP.NET MVC-program
Nu ska vi lägga till hello modeller, vyer och styrenheter toothis MVC-program:

* [Lägga till en modell](#_Toc395637764).
* [Lägga till en styrenhet](#_Toc395637765).
* [Lägga till vyer](#_Toc395637766).

### <a name="_Toc395637764"></a>Lägg till en JSON-datamodell
Vi börjar med att skapa hello **M** hello modellen i MVC, dvs. 

1. I **Solution Explorer**, högerklicka på hello **modeller** mappen, klicka på **Lägg till**, och klicka sedan på **klassen**.
   
      Hej **Lägg till nytt objekt** dialogrutan visas.
2. Namnge den nya klassen **Objekt.cs** och klicka på **Lägg till**. 
3. I den nya **Item.cs** lägger du till följande hello efter hello senast *med hjälp av instruktionen*.
   
        using Newtonsoft.Json;
4. Ersätt nu den här koden 
   
        public class Item
        {
        }
   
    med följande kod hello.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Alla data i Azure Cosmos DB skickas via kabeln hello och lagras som JSON. toocontrol hello hur objekten serialiseras/deserialiseras av JSON.NET som du kan använda hello **JsonProperty** attribut som visas i hello **objektet** klass vi just skapade. Du behöver inte **har** toodo detta men jag vill att Mina egenskaper följer hello JSON camelCase namngivningskonventioner tooensure. 
   
    Inte bara kan du styra hello format hello egenskapsnamn när den går till JSON, men du kan helt byta namn på dina .NET-egenskaper som jag gjorde med hello **beskrivning** egenskapen. 

### <a name="_Toc395637765"></a>Lägg till en controller
Som tar hand om hello **M**, nu skapar vi hello **C** i MVC, dvs. en styrenhetsklass.

1. I **Solution Explorer**, högerklicka på hello **domänkontrollanter** mapp, klickar du på **Lägg till**, och klicka sedan på **domänkontrollant**.
   
    Hej **Lägg till Kodskelett** dialogrutan visas.
2. Välj **MVC 5 styrenhet – tom** och klicka sedan på **Lägg till**.
   
    ![Skärmbild som visar hello dialogruta för Lägg till Kodskelett med hello MVC 5 styrenhet – tom markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. Ge din nya styrenhet namnet **Objektstyrning.**
   
    ![Skärmbild av dialogrutan Lägg till styrenhet för hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    När hello fil skapas, Visual Studio-lösning bör likna följande hello med hello nya ItemController.cs-filen i **Solution Explorer**. hello nya Item.cs-filen skapades tidigare visas också.
   
    ![Skärmbild som visar hello Visual Studio - lösningen i Solution Explorer med hello nya ItemController.cs-filen och Item.cs-filen markerade](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    Du kan stänga ItemController.cs, vi återkommer tooit senare. 

### <a name="_Toc395637766"></a>Lägg till vyer
Nu skapar vi hello **V** i MVC, dvs hello vyer:

* [Lägga till Vyn Objektindex](#AddItemIndexView).
* [Lägga till vyn Nytt objekt](#AddNewIndexView).
* [Lägga till vyn Redigera objekt](#_Toc395888515).

#### <a name="AddItemIndexView"></a>Lägg till en objektindexvy
1. I **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på hello tom **objektet** mapp som Visual Studio skapade när du har lagt till hello  **ItemController** tidigare, klicka på **Lägg till**, och klicka sedan på **visa**.
   
    ![Skärmdump av Solution Explorer visar hello objektmapp som Visual Studio skapade med hello Lägg till vy markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. I hello **Lägg till vy** dialogrutan rutan, hello följande:
   
   * I hello **vynamn** skriver ***Index***.
   * I hello **mallen** väljer ***listan***.
   * I hello **Modellklass** väljer ***objekt (todo. Modeller)***.
   * Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.
     
   ![Skärmbild som visar hello Lägg till vy dialogrutan](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. När du har angett samtliga dessa värden klickar du på **Lägg till**  och låter Visual Studio skapa en ny mallvy. När det är klart öppnas hello cshtml-filen som skapades. Vi kan stänga filen i Visual Studio eftersom vi återkommer tooit senare.

#### <a name="AddNewIndexView"></a>Lägg till en nytt objektsvy
Liknande toohow som vi har skapat en **Objektindex** vyn nu ska vi skapa en ny vy för att skapa nya **objekt**.

1. I **Solution Explorer**, högerklicka på hello **objektet** mappen igen och klicka på **Lägg till**, och klicka sedan på **visa**.
2. I hello **Lägg till vy** dialogrutan rutan, hello följande:
   
   * I hello **vynamn** skriver ***skapa***.
   * I hello **mallen** väljer ***skapa***.
   * I hello **Modellklass** väljer ***objekt (todo. Modeller)***.
   * Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.
   * Klicka på **Lägg till**.
   
#### <a name="_Toc395888515"></a>Lägg till en redigera objektsvy
Lägg slutligen till en vy för redigering av ett **objektet** i hello samma sätt som tidigare.

1. I **Solution Explorer**, högerklicka på hello **objektet** mappen igen och klicka på **Lägg till**, och klicka sedan på **visa**.
2. I hello **Lägg till vy** dialogrutan rutan, hello följande:
   
   * I hello **vynamn** skriver ***redigera***.
   * I hello **mallen** väljer ***redigera***.
   * I hello **Modellklass** väljer ***objekt (todo. Modeller)***.
   * Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.
   * Klicka på **Lägg till**.

När det är klart stänger du alla hello cshtml-dokument i Visual Studio eftersom vi återkommer toothese vyer senare.

## <a name="_Toc395637769"></a>Steg 5: Lägga till kod för Azure Cosmos DB
Nu när du har åtgärdat hello standard MVC är klara, dags tooadding hello koden för Azure Cosmos DB. 

I det här avsnittet vi lägger till kod toohandle hello följande:

* [Lista ofullständiga objekt](#_Toc395637770).
* [Lägga till objekt](#_Toc395637771).
* [Redigera objekt](#_Toc395637772).

### <a name="_Toc395637770"></a>Lista ofullständiga objekt i ditt MVC-webbprogram
hello första sak toodo här är lägga till en klass som innehåller alla hello logik tooconnect tooand Använd Azure Cosmos DB. Den här självstudiekursen kapslar vi in all denna logik i tooa centrallagerklass kallad DocumentDBRepository. 

1. I **Solution Explorer**, högerklicka på hello projekt, klickar du på **Lägg till**, och klicka sedan på **klassen**. Namnge hello nya klassen **DocumentDBRepository** och på **Lägg till**.
2. I hello nyskapad **DocumentDBRepository** klassen och Lägg till följande hello *using-instruktioner* ovan hello *namnområde* förklaring
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    Ersätt nu den här koden 
   
        public class DocumentDBRepository
        {
        }
   
    med följande kod hello.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. Vi läser in vissa värden från konfigurationen, så att öppna hello **Web.config** för appen och Lägg till följande rader under hello hello `<AppSettings>` avsnitt.
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Uppdatera nu hello värden för *endpoint* och *auktoriseringsnyckel* med hello nycklar bladet för hello Azure-portalen. Använd hello **URI** från bladet för hello nycklar som hello värdet för inställningen för hello slutpunkt och Använd hello **PRIMÄRNYCKEL**, eller **SEKUNDÄRNYCKELN** från bladet för hello nycklar som hello värdet för hello inställningen auktoriseringsnyckel.

    Tar hand om att koppla samman hello Azure Cosmos DB databasen nu ska vi lägga till vår applogik.

1. hello vill först öppna vi toobe kan toodo med en todo-App är toodisplay hello ofullständiga objekt.  Kopiera och klistra in följande kodutdrag var som helst i hello hello **DocumentDBRepository** klass.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Öppna hello **ItemController** vi lade till tidigare och Lägg till följande hello *using-instruktioner* ovan hello namnområdesdeklaration.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Om ditt projekt inte är med namnet ”todo” måste tooupdate med ”todo. Modeller ”. tooreflect hello namnet på ditt projekt.
   
    Ersätt nu den här koden
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    med följande kod hello.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Öppna **Global.asax.cs** och Lägg till följande rad toohello hello **Application_Start** metod 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

Din lösning ska nu kunna toobuild utan fel.

Om du körde hello programmet nu hamnar toohello **HomeController** och hello **Index** från den styrenheten. Detta är hello standardbeteendet för hello MVC-mallprojektet vi valde hello början men som bör inte! Nu ska vi ändra hello routning på den här MVC-program tooalter problemet.

Öppna ***App\_Start\RouteConfig.cs*** och leta upp hello rad som börjar med ”standardvärden”: och ändra den tooresemble hello följande.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Det här nu berättar för ASP.NET MVC som om du inte har angett ett värde i hello URL toocontrol hello som i stället för routningsbeteendet **Start**, använda **objektet** som hello styrenhet och användarens **Index** som hello vy.

Nu om du kör programmet hello anropar din **ItemController** som anropar i toohello centrallagerklassen och använder hello GetItems metoden tooreturn alla hello ofullständiga objekt toohello **vyer** \\ **Objektet**\\**Index** vyn. 

Om du bygger och kör det här projektet nu bör det se ut ungefär så här.    

![Skärmbild som visar hello listan webbapp skapats i denna självstudie](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <a name="_Toc395637771"></a>Lägg till objekt
Vi lägger in några objekt i vår databas så att vi har något mer än ett tomt rutnät toolook på.

Lägg till kod för Azure Cosmos DBRepository och ItemController toopersist hello post i Azure Cosmos DB.

1. Lägg till följande metod tooyour hello **DocumentDBRepository** klass.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Den här metoden tar ett objekt som överförs tooit bara och sparar det i Azure Cosmos DB.
2. Öppna hello ItemController.cs-filen och Lägg till följande kodutdrag i klassen hello hello. Detta är så ASP.NET MVC vet vilken toodo för hello **skapa** åtgärd. I det här fallet associerade bara render hello Create.cshtml-vyn som skapades tidigare.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Vi behöver nu lite mer kod i styrenheten som godtar inmatningen hello från hello **skapa** vyn.
3. Lägg till hello nästa block med koden toohello ItemController.cs-klass som berättar för ASP.NET MVC vad toodo med formuläret POST för den här domänkontrollanten.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Den här koden anropar i toohello DocumentDBRepository och använder hello CreateItemAsync metoden toopersist hello nya todo objektet toohello databasen. 
   
    **Säkerhetsmeddelande**: hello **ValidateAntiForgeryToken** attributet används här toohelp skydda appen mot attacker med förfalskning av begäran. Det finns flera tooit bara att lägga till det här attributet, dina vyer behöver toowork med denna antiförfalskningstoken samt. Mer information om hello ämnet och exempel på hur tooimplement detta korrekt finns [hindrar webbplatser förfalskning av begäran mellan][Preventing Cross-Site Request Forgery]. Hej källkoden på [GitHub] [ GitHub] har hello fullständigt genomförande.
   
    **Säkerhetsmeddelande**: Vi använder också hello **binda** attributet på hello metoden parametern toohelp skydda mot overpostingattacker. Mer information finns i [Grundläggande CRUD-åtgärder i ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].

Detta avslutar hello kod krävs tooadd nya objekt tooour databasen.

### <a name="_Toc395637772"></a>Redigera objekt
Det finns en sista åtgärd vi toodo och som är tooadd hello möjlighet tooedit **objekt** i hello-databasen och toomark dem som Slutför. hello redigeringsvyn har redan lagts till toohello projekt, så vi behöver bara tooadd vissa code tooour domänkontrollant och toohello **DocumentDBRepository** klassen igen.

1. Lägg till följande toohello hello **DocumentDBRepository** klass.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    Hej första av dessa metoder **GetItem** hämtar ett objekt från Azure Cosmos-databasen som skickas tillbaka toohello **ItemController** och klicka sedan på toohello **redigera** vyn.
   
    Hej sekund av hello metoder vi just lade till ersätter hello **dokument** i Azure Cosmos-databasen med hello version av hello **dokument** som överförts från hello **ItemController**.
2. Lägg till följande toohello hello **ItemController** klass.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    hello första metoden hanterar hello Http GET som inträffar när hello användare klickar på hello **redigera** länk från hello **Index** vyn. Den här metoden hämtar en [ **dokument** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) från Azure Cosmos-databasen och skickar den toohello **redigera** vyn.
   
    Hej **redigera** gör sedan en Http POST-toohello **IndexController**. 
   
    hello andra metoden som vi har lagt till hanterar skicka hello uppdaterade objekt tooAzure Cosmos DB toobe beständig i hello-databasen.

Som är den som är allt som behövs toorun vårt program, lista ofullständiga **objekt**, lägga till nya **objekt**, och redigera **objekt**.

## <a name="_Toc395637773"></a>Steg 6: Kör programmet hello lokalt
tootest hello program på den lokala datorn hello följande:

1. Tryck på F5 i Visual Studio toobuild hello programmet i felsökningsläge. Den ska bygga hello program och starta en webbläsare med hello tomma rutnätssidan vi såg tidigare:
   
    ![Skärmbild som visar hello listan webbapp skapats i denna självstudie](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. Klicka på hello **Skapa nytt** länkar och Lägg till värden toohello **namn** och **beskrivning** fält. Lämna hello **slutförd** Markera inte kryssrutan annars hello nya **objektet** kommer att läggas till i slutfört tillstånd och visas inte på hello ursprungliga listan.
   
    ![Skärmbild som visar hello skapa vy](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. Klicka på **skapa** och du är omdirigerade tillbaka toohello **Index** vyn och dina **objektet** visas i listan hello.
   
    ![Skärmbild som visar hello indexvy](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    Känna sig fria tooadd några fler **objekt** tooyour todo-listan.
    
4. Klicka på **redigera** nästa tooan **objektet** på hello lista och du kommer toohello **redigera** vy där du kan uppdatera alla egenskaper för objektet, inklusive hello  **Slutföra** flaggan. Om du markerar hello **Slutför** flagga och på **spara**, hello **objektet** tas bort från hello listan över ofullständiga uppgifter.
   
    ![Skärmbild som visar hello indexvyn med hello slutförd ikryssad](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. Tryck på Ctrl + F5 toostop felsökning hello app när du har testat hello app. Du är klar toodeploy!

## <a name="_Toc395637774"></a>Steg 7: Distribuera hello programmet tooAzure Apptjänst 
Nu när du har hello hela appen vi fungerar med Azure Cosmos DB toodeploy denna web app tooAzure Apptjänst.  

1. toopublish alla programmet behöver du toodo är Högerklicka på hello-projekt i **Solution Explorer** och på **publicera**.
   
    ![Skärmbild som visar hello alternativet Publicera i Solution Explorer](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. I hello **publicera** dialogrutan klickar du på **Microsoft Azure App Service**och välj **Skapa nytt** toocreate en Apptjänst-profil, eller klicka på **Välj Befintliga** toouse en befintlig profil.

    ![Dialogrutan Publicera i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. Om du har en befintlig Azure App Service-profil anger du namnet på din prenumeration. Använd hello **visa** filtrera toosort av resursgruppen eller resursen och sedan välja Azure App Service. 
   
    ![Dialogrutan för Apptjänst i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. toocreate en ny Azure App Service-profil klickar du på **Skapa nytt** i hello **publicera** dialogrutan. I hello **skapa Apptjänst** dialogrutan, ange ditt webbprogram namn och lämpliga prenumerationen, resursgruppen och App Service-plan och klicka sedan på **skapa**.

    ![Skapa en dialogruta för Apptjänst i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

I några sekunder har Visual Studio publicerat din webbapp och öppnar en webbläsare där du kan se ditt arbete som körs i Azure!



## <a name="_Toc395637775"></a>Nästa steg
Grattis! Du precis skapat din första ASP.NET MVC-webbapp med Azure Cosmos DB och publicerat den tooAzure. Hej källkoden för hello hela appen, inklusive hello detaljer och ta bort funktioner som inte fanns med i den här självstudiekursen kan hämtas eller klonas från [GitHub][GitHub]. Så om du är intresserad av att lägga till tooyour appen hämtar hello koden och Lägg den toothis app.

tooadd ytterligare funktioner tooyour program, granska hello API: er som är tillgängliga i hello [Azure Cosmos DB .NET-biblioteket](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) och känna sig fria toocontribute toohello Azure Cosmos DB .NET-biblioteket på [GitHub] [GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
