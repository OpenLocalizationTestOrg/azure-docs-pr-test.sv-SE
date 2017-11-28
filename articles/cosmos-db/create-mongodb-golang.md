---
<span data-ttu-id="8973a-101">Rubrik: aaa ”Azure Cosmos DB: skapa en MongoDB-API-konsolapp med Golang och hello Azure-portalen | Microsoft Docs ”beskrivning: Anger en Golang kodexempel som du kan använda tooconnect tooand fråga Azure Cosmos DB services: cosmos-db författare: Durgaprasad Budhwani manager: jhubbard editor: mimig1</span><span class="sxs-lookup"><span data-stu-id="8973a-101">title: aaa"Azure Cosmos DB: Build a MongoDB API console app with Golang and hello Azure portal | Microsoft Docs" description: Presents a Golang code sample you can use tooconnect tooand query Azure Cosmos DB services: cosmos-db author: Durgaprasad-Budhwani manager: jhubbard editor: mimig1</span></span>

<span data-ttu-id="8973a-102">MS.Service: cosmos-db ms.topic: hjälteartikel ms.date: 07/21/2017 ms.author: mimig</span><span class="sxs-lookup"><span data-stu-id="8973a-102">ms.service: cosmos-db ms.topic: hero-article ms.date: 07/21/2017 ms.author: mimig</span></span>
---

# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-hello-azure-portal"></a><span data-ttu-id="8973a-103">Azure DB Cosmos: Skapa en MongoDB-API-konsolapp med Golang och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8973a-103">Azure Cosmos DB: Build a MongoDB API console app with Golang and hello Azure portal</span></span>

<span data-ttu-id="8973a-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="8973a-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="8973a-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8973a-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span>

<span data-ttu-id="8973a-106">Den här Snabbkurs visar hur toouse som en befintlig [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) app skriven i [Golang](https://golang.org/) och anslut den tooyour Azure DB som Cosmos-databasen, som har stöd för MongoDB-klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="8973a-106">This quick-start demonstrates how toouse an existing [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) app written in [Golang](https://golang.org/) and connect it tooyour Azure Cosmos DB database, which supports MongoDB client connections.</span></span>

<span data-ttu-id="8973a-107">Med andra ord, vet Golang programmet bara att den ansluter tooa databasen med MongoDB APIs.</span><span class="sxs-lookup"><span data-stu-id="8973a-107">In other words, your Golang application only knows that it's connecting tooa database using MongoDB APIs.</span></span> <span data-ttu-id="8973a-108">Den är öppet toohello program som hello data lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8973a-108">It is transparent toohello application that hello data is stored in Azure Cosmos DB.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8973a-109">Krav</span><span class="sxs-lookup"><span data-stu-id="8973a-109">Prerequisites</span></span>

- <span data-ttu-id="8973a-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8973a-110">An Azure subscription.</span></span> <span data-ttu-id="8973a-111">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="8973a-111">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free) before you begin.</span></span>
- <span data-ttu-id="8973a-112">[Gå](https://golang.org/dl/) och grundläggande kunskaper om hello [Gå](https://golang.org/) språk.</span><span class="sxs-lookup"><span data-stu-id="8973a-112">[Go](https://golang.org/dl/) and a basic knowledge of hello [Go](https://golang.org/) language.</span></span>
- <span data-ttu-id="8973a-113">En IDE — [Gogland](https://www.jetbrains.com/go/) från Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) från Microsoft eller [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="8973a-113">An IDE — [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span> <span data-ttu-id="8973a-114">Jag använder Goglang i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="8973a-114">In this tutorial, I'm using Goglang.</span></span>

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="8973a-115">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="8973a-115">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="8973a-116">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="8973a-116">Clone hello sample application</span></span>

<span data-ttu-id="8973a-117">Klona hello exempelprogrammet och installera paket hello krävs.</span><span class="sxs-lookup"><span data-stu-id="8973a-117">Clone hello sample application and install hello required packages.</span></span>

1. <span data-ttu-id="8973a-118">Skapa en mapp med namnet CosmosDBSample hello GOROOT\src mappen som är C:\Go\ som standard.</span><span class="sxs-lookup"><span data-stu-id="8973a-118">Create a folder named CosmosDBSample inside hello GOROOT\src folder, which is C:\Go\ by default.</span></span>
2. <span data-ttu-id="8973a-119">Kör följande kommando med ett terminalfönster git exempelvis git bash tooclone hello exempel lagringsplatsen till hello CosmosDBSample mapp hello.</span><span class="sxs-lookup"><span data-stu-id="8973a-119">Run hello following command using a git terminal window such as git bash tooclone hello sample repository into hello CosmosDBSample folder.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  <span data-ttu-id="8973a-120">Kör följande kommando tooget hello mgo paketet hello.</span><span class="sxs-lookup"><span data-stu-id="8973a-120">Run hello following command tooget hello mgo package.</span></span> 

    ```
    go get gopkg.in/mgo.v2
    ```

<span data-ttu-id="8973a-121">Hej [mgo](http://labix.org/mgo) drivrutin (uttalas som *mango*) är en [MongoDB](http://www.mongodb.org/) drivrutin för hello [gå språk](http://golang.org/) som implementerar en komplett och väl testas val av funktioner under ett väldigt enkelt API som följer standard gå idioms.</span><span class="sxs-lookup"><span data-stu-id="8973a-121">hello [mgo](http://labix.org/mgo) driver (pronounced as *mango*) is a [MongoDB](http://www.mongodb.org/) driver for hello [Go language](http://golang.org/) that implements a rich and well tested selection of features under a very simple API following standard Go idioms.</span></span>

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a><span data-ttu-id="8973a-122">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="8973a-122">Update your connection string</span></span>

<span data-ttu-id="8973a-123">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="8973a-123">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="8973a-124">Klicka på **Snabbstart** i hello vänstra navigeringsmenyn och klicka sedan på **andra** tooview hello Anslutningssträngsinformation krävs av hello gå program.</span><span class="sxs-lookup"><span data-stu-id="8973a-124">Click **Quick start** in hello left navigation menu, and then click **Other** tooview hello connection string information required by hello Go application.</span></span>

2. <span data-ttu-id="8973a-125">Öppna hello main.go fil i hello GOROOT\CosmosDBSample katalog i Goglang, och uppdatera hello följande rader med kod med hjälp av informationen i anslutningssträngen hello från hello Azure-portalen som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="8973a-125">In Goglang, open hello main.go file in hello GOROOT\CosmosDBSample directory and update hello following lines of code using hello connection string information from hello Azure portal as shown in hello following screenshot.</span></span> 

    <span data-ttu-id="8973a-126">hello databasnamnet är hello prefixet hello **värden** värdet hello Azure portal anslutning sträng i fönstret.</span><span class="sxs-lookup"><span data-stu-id="8973a-126">hello Database name is hello prefix of hello **Host** value in hello Azure portal connection string pane.</span></span> <span data-ttu-id="8973a-127">För hello-konto som visas i hello bilden nedan är hello databasnamnet golang-buss.</span><span class="sxs-lookup"><span data-stu-id="8973a-127">For hello account shown in hello image below, hello Database name is golang-coach.</span></span>

    ```go
    Database: "hello prefix of hello Host value in hello Azure portal",
    Username: "hello Username in hello Azure portal",
    Password: "hello Password in hello Azure portal",
    ```

    ![Snabbstart fönstret andra fliken i hello Azure portal som visar hello Anslutningssträngsinformation](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. <span data-ttu-id="8973a-129">Spara hello main.go filen.</span><span class="sxs-lookup"><span data-stu-id="8973a-129">Save hello main.go file.</span></span>

## <a name="review-hello-code"></a><span data-ttu-id="8973a-130">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="8973a-130">Review hello code</span></span>

<span data-ttu-id="8973a-131">Låt oss göra en snabb genomgång av vad som händer i hello main.go fil.</span><span class="sxs-lookup"><span data-stu-id="8973a-131">Let's make a quick review of what's happening in hello main.go file.</span></span> 

### <a name="connecting-hello-go-app-tooazure-cosmos-db"></a><span data-ttu-id="8973a-132">Ansluta hello gå app tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8973a-132">Connecting hello Go app tooAzure Cosmos DB</span></span>

<span data-ttu-id="8973a-133">Azure Cosmos-DB stöder hello SSL-aktiverade MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8973a-133">Azure Cosmos DB supports hello SSL-enabled MongoDB.</span></span> <span data-ttu-id="8973a-134">tooconnect tooan SSL-aktiverade MongoDB, behöver du toodefine hello **DialServer** fungera i [mgo. DialInfo](http://gopkg.in/mgo.v2#DialInfo), och användning av hello [tls. *Ring* ](http://golang.org/pkg/crypto/tls#Dial) fungerar tooperform hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="8973a-134">tooconnect tooan SSL-enabled MongoDB, you need toodefine hello **DialServer** function in [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo), and make use of hello [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) function tooperform hello connection.</span></span>

<span data-ttu-id="8973a-135">hello följande kodavsnitt Golang ansluter hello gå app med Azure Cosmos DB MongoDB API.</span><span class="sxs-lookup"><span data-stu-id="8973a-135">hello following Golang code snippet connects hello Go app with Azure Cosmos DB MongoDB API.</span></span> <span data-ttu-id="8973a-136">Hej *DialInfo* klassen innehåller alternativ för att upprätta en session med ett MongoDB-kluster.</span><span class="sxs-lookup"><span data-stu-id="8973a-136">hello *DialInfo* class holds options for establishing a session with a MongoDB cluster.</span></span>

```go
// DialInfo holds options for establishing a session with a MongoDB cluster.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "database", // It can be anything
    Username: "username", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// tooour Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect toomongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes hello session safety mode.
// If hello safe parameter is nil, hello session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. hello unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

<span data-ttu-id="8973a-137">Hej **mgo. Dial()** metoden används när det finns ingen SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="8973a-137">hello **mgo.Dial()** method is used when there is no SSL connection.</span></span> <span data-ttu-id="8973a-138">En SSL-anslutning hello **mgo. DialWithInfo()** metod krävs.</span><span class="sxs-lookup"><span data-stu-id="8973a-138">For an SSL connection, hello **mgo.DialWithInfo()** method is required.</span></span>

<span data-ttu-id="8973a-139">En instans av hello **DialWIthInfo {}** objektet är används toocreate hello session-objektet.</span><span class="sxs-lookup"><span data-stu-id="8973a-139">An instance of hello **DialWIthInfo{}** object is used toocreate hello session object.</span></span> <span data-ttu-id="8973a-140">När hello-sessionen har upprättats kan du komma åt hello samling med hello följande kodutdrag:</span><span class="sxs-lookup"><span data-stu-id="8973a-140">Once hello session is established, you can access hello collection by using hello following code snippet:</span></span>

```go
collection := session.DB(“database”).C(“package”)
```

<a id="create-document"></a>

### <a name="create-a-document"></a><span data-ttu-id="8973a-141">Skapa ett dokument</span><span class="sxs-lookup"><span data-stu-id="8973a-141">Create a document</span></span>

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

### <a name="query-or-read-a-document"></a><span data-ttu-id="8973a-142">Fråga eller läsa ett dokument</span><span class="sxs-lookup"><span data-stu-id="8973a-142">Query or read a document</span></span>

<span data-ttu-id="8973a-143">Azure Cosmos DB stöder komplexa frågor mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="8973a-143">Azure Cosmos DB supports rich queries against JSON documents stored in each collection.</span></span> <span data-ttu-id="8973a-144">hello visar följande exempelkod en fråga som du kan köra mot hello dokument i samlingen.</span><span class="sxs-lookup"><span data-stu-id="8973a-144">hello following sample code shows a query that you can run against hello documents in your collection.</span></span>

```go
// Get a Document from hello collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a><span data-ttu-id="8973a-145">Uppdatera ett dokument</span><span class="sxs-lookup"><span data-stu-id="8973a-145">Update a document</span></span>

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

### <a name="delete-a-document"></a><span data-ttu-id="8973a-146">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="8973a-146">Delete a document</span></span>

<span data-ttu-id="8973a-147">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="8973a-147">Azure Cosmos DB supports deleting JSON documents.</span></span>

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-hello-app"></a><span data-ttu-id="8973a-148">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="8973a-148">Run hello app</span></span>

1. <span data-ttu-id="8973a-149">I Goglang, kontrollerar du att din GOPATH (tillgängliga under **filen**, **inställningar**, **Gå**, **GOPATH**) Inkludera hello platsen i vilka hello gopkg har installerats, vilket är USERPROFILE\go som standard.</span><span class="sxs-lookup"><span data-stu-id="8973a-149">In Goglang, ensure that your GOPATH (available under **File**, **Settings**, **Go**, **GOPATH**) include hello location in which hello gopkg was installed, which is USERPROFILE\go by default.</span></span> 
2. <span data-ttu-id="8973a-150">Kommentera ut hello rader som tas bort hello dokument, rader 91-96, så att du kan se hello dokument efter hello-app som körs.</span><span class="sxs-lookup"><span data-stu-id="8973a-150">Comment out hello lines that delete hello document, lines 91-96, so that you can see hello document after running hello app.</span></span>
3. <span data-ttu-id="8973a-151">Klicka på **Run** (Kör) i Goglang och sedan på **Run 'Build main.go and run'** (Bygg och kör main.go).</span><span class="sxs-lookup"><span data-stu-id="8973a-151">In Goglang, click **Run**, and then click **Run 'Build main.go and run'**.</span></span>

    <span data-ttu-id="8973a-152">hello app har slutförts och visar hello beskrivning av hello dokument som skapats i [skapa ett dokument](#create-document).</span><span class="sxs-lookup"><span data-stu-id="8973a-152">hello app finishes and displays hello description of hello document created in [Create a document](#create-document).</span></span>
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Goglang som visar hello utdata från hello app](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a><span data-ttu-id="8973a-154">Granska dokumentet i Datautforskaren</span><span class="sxs-lookup"><span data-stu-id="8973a-154">Review your document in Data Explorer</span></span>

<span data-ttu-id="8973a-155">Gå tillbaka toohello Azure portal toosee dokumentet i Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="8973a-155">Go back toohello Azure portal toosee your document in Data Explorer.</span></span>

1. <span data-ttu-id="8973a-156">Klicka på **Data Explorer (förhandsgranskning)** Expandera i hello vänstra navigeringsmenyn **golang buss**, **paketet**, och klicka sedan på **dokument**.</span><span class="sxs-lookup"><span data-stu-id="8973a-156">Click **Data Explorer (Preview)** in hello left navigation menu, expand **golang-coach**, **package**, and then click **Documents**.</span></span> <span data-ttu-id="8973a-157">I hello **dokument** klickar du på hello \_id toodisplay hello dokument i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="8973a-157">In hello **Documents** tab, click hello \_id toodisplay hello document in hello right pane.</span></span> 

    ![Data Explorer som visar hello nyskapade dokumentet](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. <span data-ttu-id="8973a-159">Du kan sedan arbeta med hello dokumentet infogade och klicka på **uppdatering** toosave den.</span><span class="sxs-lookup"><span data-stu-id="8973a-159">You can then work with hello document inline and click **Update** toosave it.</span></span> <span data-ttu-id="8973a-160">Du kan också ta bort hello dokument eller skapa nya dokument eller frågor.</span><span class="sxs-lookup"><span data-stu-id="8973a-160">You can also delete hello document, or create new documents or queries.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="8973a-161">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8973a-161">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="8973a-162">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="8973a-162">Clean up resources</span></span>

<span data-ttu-id="8973a-163">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8973a-163">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="8973a-164">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="8973a-164">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="8973a-165">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="8973a-165">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8973a-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8973a-166">Next steps</span></span>

<span data-ttu-id="8973a-167">I den här snabbstarten du har lärt dig hur toocreate ett Azure DB som Cosmos-konto och kör en Golang appen med hello API för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8973a-167">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and run a Golang app using hello API for MongoDB.</span></span> <span data-ttu-id="8973a-168">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="8973a-168">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="8973a-169">Importera data till Azure Cosmos DB för hello MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="8973a-169">Import data into Azure Cosmos DB for hello MongoDB API</span></span>](mongodb-migrate.md)
