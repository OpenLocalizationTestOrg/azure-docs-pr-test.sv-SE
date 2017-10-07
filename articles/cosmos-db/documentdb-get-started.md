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
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a><span data-ttu-id="707f5-104">Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et</span><span class="sxs-lookup"><span data-stu-id="707f5-104">Azure Cosmos DB: DocumentDB API getting started tutorial</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="707f5-105">.NET</span><span class="sxs-lookup"><span data-stu-id="707f5-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="707f5-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="707f5-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="707f5-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="707f5-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="707f5-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="707f5-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="707f5-109">Java</span><span class="sxs-lookup"><span data-stu-id="707f5-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="707f5-110">C++</span><span class="sxs-lookup"><span data-stu-id="707f5-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="707f5-111">Välkommen till toohello Azure Cosmos DB DocumentDB API komma igång-självstudiekurs!</span><span class="sxs-lookup"><span data-stu-id="707f5-111">Welcome toohello Azure Cosmos DB DocumentDB API getting started tutorial!</span></span> <span data-ttu-id="707f5-112">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="707f5-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="707f5-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="707f5-113">We'll cover:</span></span>

* <span data-ttu-id="707f5-114">Skapa och ansluta tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="707f5-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="707f5-115">Konfigurera en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="707f5-115">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="707f5-116">Skapa en onlinedatabas</span><span class="sxs-lookup"><span data-stu-id="707f5-116">Creating an online database</span></span>
* <span data-ttu-id="707f5-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="707f5-117">Creating a collection</span></span>
* <span data-ttu-id="707f5-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-118">Creating JSON documents</span></span>
* <span data-ttu-id="707f5-119">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="707f5-119">Querying hello collection</span></span>
* <span data-ttu-id="707f5-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-120">Replacing a document</span></span>
* <span data-ttu-id="707f5-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-121">Deleting a document</span></span>
* <span data-ttu-id="707f5-122">Tar bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="707f5-122">Deleting hello database</span></span>

<span data-ttu-id="707f5-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="707f5-123">Don't have time?</span></span> <span data-ttu-id="707f5-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="707f5-124">Don't worry!</span></span> <span data-ttu-id="707f5-125">hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="707f5-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> <span data-ttu-id="707f5-126">Hoppa toohello [hämta hello fullständig NoSQL självstudiekursen lösning avsnittet](#GetSolution) snabbguide.</span><span class="sxs-lookup"><span data-stu-id="707f5-126">Jump toohello [Get hello complete NoSQL tutorial solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="707f5-127">Därefter kan knappar du använda hello röstning hello upp eller längst ned på den här sidan toogive oss feedback.</span><span class="sxs-lookup"><span data-stu-id="707f5-127">Afterwards, please use hello voting buttons at hello top or bottom of this page toogive us feedback.</span></span> <span data-ttu-id="707f5-128">Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.</span><span class="sxs-lookup"><span data-stu-id="707f5-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="707f5-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="707f5-129">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="707f5-130">Krav</span><span class="sxs-lookup"><span data-stu-id="707f5-130">Prerequisites</span></span>
<span data-ttu-id="707f5-131">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="707f5-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="707f5-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="707f5-132">An active Azure account.</span></span> <span data-ttu-id="707f5-133">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="707f5-133">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="707f5-134">Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="707f5-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="707f5-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="707f5-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="707f5-136">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="707f5-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="707f5-137">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="707f5-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="707f5-138">Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="707f5-138">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="707f5-139">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="707f5-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="707f5-140"><a id="SetupVS"></a>Steg 2: Konfigurera en lösning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="707f5-140"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="707f5-141">Öppna **Visual Studio 2017** i datorn.</span><span class="sxs-lookup"><span data-stu-id="707f5-141">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="707f5-142">På hello **filen** väljer du **ny**, och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="707f5-142">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="707f5-143">I hello **nytt projekt** markerar **mallar** / **Visual C#** / **konsolprogram**, namn projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="707f5-143">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console Application**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="707f5-144">![Skärmbild som visar hello-fönstret nytt projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="707f5-144">![Screen shot of hello New Project window](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span></span>
4. <span data-ttu-id="707f5-145">I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="707f5-145">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="707f5-147">I hello **Nuget** klickar du på **Bläddra**, och skriv **azure documentdb** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="707f5-147">In hello **Nuget** tab, click **Browse**, and type **azure documentdb** in hello search box.</span></span>
6. <span data-ttu-id="707f5-148">I hello resultat hitta **Microsoft.Azure.DocumentDB** och på **installera**.</span><span class="sxs-lookup"><span data-stu-id="707f5-148">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="707f5-149">hello paket-ID för hello Azure Cosmos DB DocumentDB API-klientbiblioteket är [Microsoft Azure DocumentDB klientbibliotek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span><span class="sxs-lookup"><span data-stu-id="707f5-149">hello package ID for hello Azure Cosmos DB DocumentDB API Client Library is [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span></span>
   <span data-ttu-id="707f5-150">![Skärmbild som visar hello Nuget-menyn för att hitta Azure Cosmos DB klient-SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="707f5-150">![Screen shot of hello Nuget Menu for finding Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="707f5-151">Om du får meddelanden om granskning av ändringar toohello lösning, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="707f5-151">If you get a messages about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="707f5-152">Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.</span><span class="sxs-lookup"><span data-stu-id="707f5-152">If you get a message about license acceptance, click **I accept**.</span></span>

<span data-ttu-id="707f5-153">Bra!</span><span class="sxs-lookup"><span data-stu-id="707f5-153">Great!</span></span> <span data-ttu-id="707f5-154">Nu när vi avslutat hello inställningarna Låt oss börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="707f5-154">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="707f5-155">Det finns ett färdigt kodprojekt för den här självstudiekursen i [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="707f5-155">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span></span>

## <span data-ttu-id="707f5-156"><a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="707f5-156"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="707f5-157">Lägg först till dessa refererar till toohello början av C#-appen, i hello Program.cs-filen:</span><span class="sxs-lookup"><span data-stu-id="707f5-157">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART tooYOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> <span data-ttu-id="707f5-158">Kontrollera att du lägger till hello ovanstående beroenden i ordning toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="707f5-158">In order toocomplete hello tutorial, make sure you add hello dependencies above.</span></span>
> 
> 

<span data-ttu-id="707f5-159">Lägg till nedanstående två konstanter och *klientvariabeln* under den offentliga klassen *Program*.</span><span class="sxs-lookup"><span data-stu-id="707f5-159">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

    public class Program
    {
        // ADD THIS PART tooYOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

<span data-ttu-id="707f5-160">Därefter head tillbaka toohello [Azure Portal](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="707f5-160">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="707f5-161">hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.</span><span class="sxs-lookup"><span data-stu-id="707f5-161">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="707f5-162">I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="707f5-162">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="707f5-163">Kopiera hello URI från hello-portalen och klistrar in det i `<your endpoint URL>` i hello program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="707f5-163">Copy hello URI from hello portal and paste it into `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="707f5-164">Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="707f5-164">Then copy hello PRIMARY KEY from hello portal and paste it into `<your primary key>`.</span></span>

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram.][keys]

<span data-ttu-id="707f5-167">Nu ska vi börjar hello program genom att skapa en ny instans av hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="707f5-167">Next, we'll start hello application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="707f5-168">Nedan hello **Main** metod, lägga till den här nya asynkrona aktiviteten kallad **GetStartedDemo**, som instantierar vår nya **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="707f5-168">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

    static void Main(string[] args)
    {
    }

    // ADD THIS PART tooYOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

<span data-ttu-id="707f5-169">Lägg till hello följande kod toorun den asynkrona aktiviteten från din **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-169">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="707f5-170">Hej **Main** metod ska fånga undantag och skriva dem toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="707f5-170">hello **Main** method will catch exceptions and write them toohello console.</span></span>

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

<span data-ttu-id="707f5-171">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-171">Press **F5** toorun your application.</span></span> <span data-ttu-id="707f5-172">hello konsolen utdata visar hello-meddelande `End of demo, press any key tooexit.` bekräftar att hello anslutningen gjordes.</span><span class="sxs-lookup"><span data-stu-id="707f5-172">hello console window output displays hello message `End of demo, press any key tooexit.` confirming that hello connection was made.</span></span>  <span data-ttu-id="707f5-173">Sedan kan du stänga hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="707f5-173">You can then close hello console window.</span></span> 

<span data-ttu-id="707f5-174">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-174">Congratulations!</span></span> <span data-ttu-id="707f5-175">Du har anslutit tooan Azure Cosmos DB konto nu ska gå vi igenom hur man arbetar med Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="707f5-175">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="707f5-176">Steg 4: Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="707f5-176">Step 4: Create a database</span></span>
<span data-ttu-id="707f5-177">Lägg till en hjälpmetod för skrivning toohello konsolen innan du lägger till hello-kod för att skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="707f5-177">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="707f5-178">Kopiera och klistra in hello **WriteToConsoleAndPromptToContinue** metod efter hello **GetStartedDemo** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-178">Copy and paste hello **WriteToConsoleAndPromptToContinue** method after hello **GetStartedDemo** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

<span data-ttu-id="707f5-179">Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="707f5-179">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="707f5-180">En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="707f5-180">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="707f5-181">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello klienten har skapats.</span><span class="sxs-lookup"><span data-stu-id="707f5-181">Copy and paste hello following code tooyour **GetStartedDemo** method after hello client creation.</span></span> <span data-ttu-id="707f5-182">Då skapas en databas med namnet *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="707f5-182">This will create a database named *FamilyDB*.</span></span>

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART tooYOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

<span data-ttu-id="707f5-183">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-183">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-184">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-184">Congratulations!</span></span> <span data-ttu-id="707f5-185">Du har skapat en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="707f5-185">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="707f5-186"><a id="CreateColl"></a>Steg 5: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="707f5-186"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="707f5-187">**CreateDocumentCollectionIfNotExistsAsync** skapar en ny samling med reserverat dataflöde, vilket får konsekvenser för priset.</span><span class="sxs-lookup"><span data-stu-id="707f5-187">**CreateDocumentCollectionIfNotExistsAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="707f5-188">Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="707f5-188">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="707f5-189">En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="707f5-189">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="707f5-190">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="707f5-190">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="707f5-191">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello databasen skapas.</span><span class="sxs-lookup"><span data-stu-id="707f5-191">Copy and paste hello following code tooyour **GetStartedDemo** method after hello database creation.</span></span> <span data-ttu-id="707f5-192">Då skapas en dokumentsamling som heter *FamilyCollection*.</span><span class="sxs-lookup"><span data-stu-id="707f5-192">This will create a document collection named *FamilyCollection*.</span></span>

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART tooYOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

<span data-ttu-id="707f5-193">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-193">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-194">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-194">Congratulations!</span></span> <span data-ttu-id="707f5-195">Du har skapat en Azure Cosmos DB-dokumentsamling.</span><span class="sxs-lookup"><span data-stu-id="707f5-195">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="707f5-196"><a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-196"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="707f5-197">En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="707f5-197">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="707f5-198">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="707f5-198">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="707f5-199">Vi kan nu infoga ett eller flera dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-199">We can now insert one or more documents.</span></span> <span data-ttu-id="707f5-200">Om du redan har data som du vill att toostore i databasen kan du använda hello Azure Cosmos DB [datamigreringsverktyget](import-data.md) tooimport hello data i en databas.</span><span class="sxs-lookup"><span data-stu-id="707f5-200">If you already have data you'd like toostore in your database, you can use hello Azure Cosmos DB [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

<span data-ttu-id="707f5-201">Vi måste först, toocreate en **familj** klass som ska representera objekt som lagras i Azure Cosmos DB i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="707f5-201">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="707f5-202">Vi kommer även att skapa underklasserna **Förälder**, **Barn**, **Husdjur** och **Adress** som används inom **Familj**.</span><span class="sxs-lookup"><span data-stu-id="707f5-202">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="707f5-203">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="707f5-203">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="707f5-204">Skapa dessa klasser genom att lägga till följande interna undergrupper efter hello hello **GetStartedDemo** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-204">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="707f5-205">Kopiera och klistra in hello **familj**, **överordnade**, **underordnade**, **husdjur**, och **adress** klasser efter hello **WriteToConsoleAndPromptToContinue** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-205">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes after hello **WriteToConsoleAndPromptToContinue** method.</span></span>

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

<span data-ttu-id="707f5-206">Kopiera och klistra in hello **CreateFamilyDocumentIfNotExists** under metoden din **adress** klass.</span><span class="sxs-lookup"><span data-stu-id="707f5-206">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **Address** class.</span></span>

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

<span data-ttu-id="707f5-207">Och infoga två dokument, ett för hello familjen Andersen och hello familjen Wakefield.</span><span class="sxs-lookup"><span data-stu-id="707f5-207">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="707f5-208">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello dokumentsamlingen.</span><span class="sxs-lookup"><span data-stu-id="707f5-208">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document collection creation.</span></span>

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

<span data-ttu-id="707f5-209">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-209">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-210">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-210">Congratulations!</span></span> <span data-ttu-id="707f5-211">Du har skapat två Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-211">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello onlinedatabasen, hello samlingen och hello dokument används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="707f5-213"><a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="707f5-213"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="707f5-214">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="707f5-214">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="707f5-215">hello följande exempelkod visar olika frågor – med både Azure Cosmos-Databasens SQL syntax samt LINQ – som du kan köra mot hello dokument som vi infogas i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="707f5-215">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="707f5-216">Kopiera och klistra in hello **ExecuteSimpleQuery** metod efter din **CreateFamilyDocumentIfNotExists** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-216">Copy and paste hello **ExecuteSimpleQuery** method after your **CreateFamilyDocumentIfNotExists** method.</span></span>

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

<span data-ttu-id="707f5-217">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello andra dokument har skapats.</span><span class="sxs-lookup"><span data-stu-id="707f5-217">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second document creation.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART tooYOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="707f5-218">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-218">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-219">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-219">Congratulations!</span></span> <span data-ttu-id="707f5-220">Du har skickat frågor mot en Azure Cosmos DB-samling.</span><span class="sxs-lookup"><span data-stu-id="707f5-220">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="707f5-221">hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade och hello samma logik gäller även toohello LINQ-fråga.</span><span class="sxs-lookup"><span data-stu-id="707f5-221">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![Diagram som illustrerar hello omfånget och innebörden av hello-fråga som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="707f5-223">Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling.</span><span class="sxs-lookup"><span data-stu-id="707f5-223">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="707f5-224">”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="707f5-224">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="707f5-225">Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.</span><span class="sxs-lookup"><span data-stu-id="707f5-225">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="707f5-226"><a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-226"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="707f5-227">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-227">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="707f5-228">Kopiera och klistra in hello **ReplaceFamilyDocument** metod efter din **ExecuteSimpleQuery** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-228">Copy and paste hello **ReplaceFamilyDocument** method after your **ExecuteSimpleQuery** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

<span data-ttu-id="707f5-229">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello Frågekörningen hello slutet av hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="707f5-229">Copy and paste hello following code tooyour **GetStartedDemo** method after hello query execution, at hello end of hello method.</span></span> <span data-ttu-id="707f5-230">När du ersätter dokumentet hello hello körs samma fråga igen tooview hello ändra dokumentet.</span><span class="sxs-lookup"><span data-stu-id="707f5-230">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART tooYOUR CODE
    // Update hello Grade of hello Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="707f5-231">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-231">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-232">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-232">Congratulations!</span></span> <span data-ttu-id="707f5-233">Du har ersatt ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-233">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="707f5-234"><a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="707f5-234"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="707f5-235">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-235">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="707f5-236">Kopiera och klistra in hello **DeleteFamilyDocument** metod efter din **ReplaceFamilyDocument** metod.</span><span class="sxs-lookup"><span data-stu-id="707f5-236">Copy and paste hello **DeleteFamilyDocument** method after your **ReplaceFamilyDocument** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

<span data-ttu-id="707f5-237">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello andra Frågekörningen hello slutet av hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="707f5-237">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second query execution, at hello end of hello method.</span></span>

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART tooCODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

<span data-ttu-id="707f5-238">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-238">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-239">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-239">Congratulations!</span></span> <span data-ttu-id="707f5-240">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="707f5-240">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="707f5-241"><a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="707f5-241"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="707f5-242">Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).</span><span class="sxs-lookup"><span data-stu-id="707f5-242">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="707f5-243">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod efter hello dokumentet kan du ta bort toodelete hello hela databasen och alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="707f5-243">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document delete toodelete hello entire database and all children resources.</span></span>

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART tooCODE
    // Clean up/delete hello database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

<span data-ttu-id="707f5-244">Tryck på **F5** toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="707f5-244">Press **F5** toorun your application.</span></span>

<span data-ttu-id="707f5-245">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-245">Congratulations!</span></span> <span data-ttu-id="707f5-246">Du har tagit bort en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="707f5-246">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="707f5-247"><a id="Run"></a>Steg 11: Kör C#-konsolappen i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="707f5-247"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="707f5-248">Tryck på F5 i Visual Studio toobuild hello programmet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="707f5-248">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="707f5-249">Du bör se get started-appens i konsolfönstret hello utdata.</span><span class="sxs-lookup"><span data-stu-id="707f5-249">You should see hello output of your get started app in a console window.</span></span> <span data-ttu-id="707f5-250">hello utdata visar hello resultaten av hello frågor som vi har lagt till och bör matcha hello exempeltexten nedan.</span><span class="sxs-lookup"><span data-stu-id="707f5-250">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

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

<span data-ttu-id="707f5-251">Grattis!</span><span class="sxs-lookup"><span data-stu-id="707f5-251">Congratulations!</span></span> <span data-ttu-id="707f5-252">Du har slutfört hello självstudier och har en fungerande C#-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="707f5-252">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="707f5-253"><a id="GetSolution"></a>Hämta hello fullständiga lösningen till självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="707f5-253"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="707f5-254">Om du inte har toocomplete hello stegen i den här självstudiekursen, eller bara vill toodownload hello-kodexempel kan du hämta det från [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="707f5-254">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code samples, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> 

<span data-ttu-id="707f5-255">toobuild hello GetStarted-lösningen behöver hello följande:</span><span class="sxs-lookup"><span data-stu-id="707f5-255">toobuild hello GetStarted solution, you will need hello following:</span></span>

* <span data-ttu-id="707f5-256">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="707f5-256">An active Azure account.</span></span> <span data-ttu-id="707f5-257">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="707f5-257">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="707f5-258">En [Azure Cosmos DB konto][cosmos-db-create-account].</span><span class="sxs-lookup"><span data-stu-id="707f5-258">An [Azure Cosmos DB account][cosmos-db-create-account].</span></span>
* <span data-ttu-id="707f5-259">Hej [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="707f5-259">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="707f5-260">toorestore hello referenser toohello Cosmos Azure DB .NET SDK i Visual Studio högerklickar du på hello **GetStarted** lösningen i Solution Explorer och klicka sedan på **aktivera NuGet-Paketåterställning**.</span><span class="sxs-lookup"><span data-stu-id="707f5-260">toorestore hello references toohello Azure Cosmos DB .NET SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="707f5-261">Därefter i hello App.config-fil, uppdaterar hello värdena för EndpointUrl och AuthorizationKey enligt beskrivningen i [ansluta tooan Azure Cosmos DB konto](#Connect).</span><span class="sxs-lookup"><span data-stu-id="707f5-261">Next, in hello App.config file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

<span data-ttu-id="707f5-262">Då är det bara att bygga den, så är du på väg!</span><span class="sxs-lookup"><span data-stu-id="707f5-262">That's it, build it and you're on your way!</span></span>


## <a name="next-steps"></a><span data-ttu-id="707f5-263">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="707f5-263">Next steps</span></span>
* <span data-ttu-id="707f5-264">Behöver du en mer komplex ASP.NET MVC-självstudiekurs?</span><span class="sxs-lookup"><span data-stu-id="707f5-264">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="707f5-265">Se [självstudiekurs om ASP.NET MVC: utveckling av Webbappar med Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="707f5-265">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="707f5-266">Vill tooperform skala och prestandatester med Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="707f5-266">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="707f5-267">Se [prestanda och skalning testa med Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="707f5-267">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="707f5-268">Lär dig hur för[övervaka Azure Cosmos DB begäranden, användning och lagring](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="707f5-268">Learn how too[monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="707f5-269">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="707f5-269">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="707f5-270">toolearn mer om Azure Cosmos DB finns [Välkommen tooAzure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="707f5-270">toolearn more about Azure Cosmos DB, see [Welcome tooAzure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
