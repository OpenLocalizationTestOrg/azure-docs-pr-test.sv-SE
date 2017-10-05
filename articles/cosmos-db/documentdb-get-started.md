---
title: "Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et | Microsoft Docs"
description: "En självstudiekurs som beskriver hur du skapar en onlinedatabas och ett C#-konsolprogram med hjälp av DocumentDB-API:et."
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
ms.openlocfilehash: 72f66081a6409f980ec6bca5188f585489245a36
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a><span data-ttu-id="1f016-104">Azure Cosmos DB: Självstudiekurs för att komma igång med DocumentDB-API:et</span><span class="sxs-lookup"><span data-stu-id="1f016-104">Azure Cosmos DB: DocumentDB API getting started tutorial</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1f016-105">.NET</span><span class="sxs-lookup"><span data-stu-id="1f016-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="1f016-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f016-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="1f016-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="1f016-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="1f016-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="1f016-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="1f016-109">Java</span><span class="sxs-lookup"><span data-stu-id="1f016-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="1f016-110">C++</span><span class="sxs-lookup"><span data-stu-id="1f016-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="1f016-111">Välkommen till självstudiekursen som hjälper dig att komma igång med Azure Cosmos DB DocumentDB-API:et!</span><span class="sxs-lookup"><span data-stu-id="1f016-111">Welcome to the Azure Cosmos DB DocumentDB API getting started tutorial!</span></span> <span data-ttu-id="1f016-112">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="1f016-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="1f016-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="1f016-113">We'll cover:</span></span>

* <span data-ttu-id="1f016-114">Skapa och ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="1f016-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="1f016-115">Konfigurera en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="1f016-115">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="1f016-116">Skapa en onlinedatabas</span><span class="sxs-lookup"><span data-stu-id="1f016-116">Creating an online database</span></span>
* <span data-ttu-id="1f016-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="1f016-117">Creating a collection</span></span>
* <span data-ttu-id="1f016-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-118">Creating JSON documents</span></span>
* <span data-ttu-id="1f016-119">Skicka frågor till samlingen</span><span class="sxs-lookup"><span data-stu-id="1f016-119">Querying the collection</span></span>
* <span data-ttu-id="1f016-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-120">Replacing a document</span></span>
* <span data-ttu-id="1f016-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-121">Deleting a document</span></span>
* <span data-ttu-id="1f016-122">Ta bort databasen</span><span class="sxs-lookup"><span data-stu-id="1f016-122">Deleting the database</span></span>

<span data-ttu-id="1f016-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="1f016-123">Don't have time?</span></span> <span data-ttu-id="1f016-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="1f016-124">Don't worry!</span></span> <span data-ttu-id="1f016-125">Den kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1f016-125">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> <span data-ttu-id="1f016-126">En snabbguide finns i avsnittet [Hämta den fullständiga lösningen till NoSQL-självstudiekursen](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="1f016-126">Jump to the [Get the complete NoSQL tutorial solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="1f016-127">Ge oss sedan feedback med röstningsknapparna högst uppe och nere på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="1f016-127">Afterwards, please use the voting buttons at the top or bottom of this page to give us feedback.</span></span> <span data-ttu-id="1f016-128">Om du vill att vi ska kontakta dig direkt kan du skriva din e-postadress i kommentaren.</span><span class="sxs-lookup"><span data-stu-id="1f016-128">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="1f016-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="1f016-129">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f016-130">Krav</span><span class="sxs-lookup"><span data-stu-id="1f016-130">Prerequisites</span></span>
<span data-ttu-id="1f016-131">Se till att du har följande:</span><span class="sxs-lookup"><span data-stu-id="1f016-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="1f016-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1f016-132">An active Azure account.</span></span> <span data-ttu-id="1f016-133">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1f016-133">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="1f016-134">Du kan också använda [Azure Cosmos DB-emulatorn](local-emulator.md) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="1f016-134">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="1f016-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="1f016-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="1f016-136">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="1f016-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="1f016-137">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="1f016-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="1f016-138">Om du redan har ett konto som du vill använda kan du gå vidare till [Konfigurera en lösning i Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="1f016-138">If you already have an account you want to use, you can skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="1f016-139">Om du använder Azure Cosmos DB-emulatorn följer du stegen i artikeln om [Azure Cosmos DB-emulatorn](local-emulator.md) för att konfigurera emulatorn och gå vidare till [konfigurationen av Visual Studio-lösningen](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="1f016-139">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="1f016-140"><a id="SetupVS"></a>Steg 2: Konfigurera en lösning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f016-140"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="1f016-141">Öppna **Visual Studio 2017** i datorn.</span><span class="sxs-lookup"><span data-stu-id="1f016-141">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="1f016-142">I menyn **Arkiv** väljer du **Nytt** och sedan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1f016-142">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="1f016-143">I dialogen **Nytt projekt** väljer du **Mallar** / **Visual C#** / **Konsolapp**. Namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f016-143">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console Application**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="1f016-144">![Skärmdump som visar fönstret Nytt projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="1f016-144">![Screen shot of the New Project window](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span></span>
4. <span data-ttu-id="1f016-145">I **Solution Explorer** högerklickar du på den nya konsolappen, som finns under din Visual Studio-lösning, och klickar sedan på **Hantera NuGet-paket ...**</span><span class="sxs-lookup"><span data-stu-id="1f016-145">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Skärmdump som visar den högerklickade menyn för projektet](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="1f016-147">På fliken **Nuget** klickar du på **Bläddra** och skriver **azure documentdb** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="1f016-147">In the **Nuget** tab, click **Browse**, and type **azure documentdb** in the search box.</span></span>
6. <span data-ttu-id="1f016-148">Leta reda på **Microsoft.Azure.DocumentDB** i resultatet och klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="1f016-148">Within the results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="1f016-149">Paket-ID för klientbiblioteket för Azure Cosmos DB DocumentDB API är [Microsoft Azure DocumentDB klientbibliotek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span><span class="sxs-lookup"><span data-stu-id="1f016-149">The package ID for the Azure Cosmos DB DocumentDB API Client Library is [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span></span>
   <span data-ttu-id="1f016-150">![Skärmbild av Nuget-menyn där du hittar Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="1f016-150">![Screen shot of the Nuget Menu for finding Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="1f016-151">Om du får meddelanden om att granska ändringar i lösningen klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f016-151">If you get a messages about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="1f016-152">Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.</span><span class="sxs-lookup"><span data-stu-id="1f016-152">If you get a message about license acceptance, click **I accept**.</span></span>

<span data-ttu-id="1f016-153">Bra!</span><span class="sxs-lookup"><span data-stu-id="1f016-153">Great!</span></span> <span data-ttu-id="1f016-154">Konfigurationen är slutförd, så vi kan börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="1f016-154">Now that we finished the setup, let's start writing some code.</span></span> <span data-ttu-id="1f016-155">Det finns ett färdigt kodprojekt för den här självstudiekursen i [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="1f016-155">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span></span>

## <span data-ttu-id="1f016-156"><a id="Connect"></a>Steg 3: Ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="1f016-156"><a id="Connect"></a>Step 3: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="1f016-157">Lägg först till dessa referenser till början av C#-appen, i filen Program.cs:</span><span class="sxs-lookup"><span data-stu-id="1f016-157">First, add these references to the beginning of your C# application, in the Program.cs file:</span></span>

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> <span data-ttu-id="1f016-158">För att kunna slutföra den här självstudiekursen är det viktigt att du lägger till beroendena ovan.</span><span class="sxs-lookup"><span data-stu-id="1f016-158">In order to complete the tutorial, make sure you add the dependencies above.</span></span>
> 
> 

<span data-ttu-id="1f016-159">Lägg till nedanstående två konstanter och *klientvariabeln* under den offentliga klassen *Program*.</span><span class="sxs-lookup"><span data-stu-id="1f016-159">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

<span data-ttu-id="1f016-160">Gå sedan tillbaka till [Azure Portal](https://portal.azure.com) för att hämta slutpunkts-URL:en och primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1f016-160">Next, head back to the [Azure Portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="1f016-161">Slutpunkts-URL:en och den primära nyckeln behövs för att programmet ska veta vart ditt program ska ansluta, och för att Azure Cosmos DB ska lita på programmets anslutning.</span><span class="sxs-lookup"><span data-stu-id="1f016-161">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="1f016-162">Gå till ditt Azure Cosmos DB-konto på Azure Portal och klicka på **Nycklar**.</span><span class="sxs-lookup"><span data-stu-id="1f016-162">In the Azure Portal, navigate to your Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="1f016-163">Kopiera URI från portalen och klistra in den i `<your endpoint URL>` i filen program.cs.</span><span class="sxs-lookup"><span data-stu-id="1f016-163">Copy the URI from the portal and paste it into `<your endpoint URL>` in the program.cs file.</span></span> <span data-ttu-id="1f016-164">Kopiera sedan PRIMÄRNYCKELN från portalen och klistra in den i `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="1f016-164">Then copy the PRIMARY KEY from the portal and paste it into `<your primary key>`.</span></span>

![Skärmdump av Azure Portal som används i självstudiekursen om NoSQL för att skapa en C#-konsolapp.][keys]

<span data-ttu-id="1f016-167">Härnäst ska vi starta programmet genom att skapa en ny instans av **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="1f016-167">Next, we'll start the application by creating a new instance of the **DocumentClient**.</span></span>

<span data-ttu-id="1f016-168">Under metoden **Main** lägger du till den här nya asynkrona aktiviteten kallad **GetStartedDemo**, som instantierar vår nya **dokumentklient**.</span><span class="sxs-lookup"><span data-stu-id="1f016-168">Below the **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

<span data-ttu-id="1f016-169">Lägg till nedanstående kod för att köra den asynkrona aktiviteten från metoden **Main**.</span><span class="sxs-lookup"><span data-stu-id="1f016-169">Add the following code to run your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="1f016-170">Metoden **Main** ska fånga upp undantag och skriva dem till konsolen.</span><span class="sxs-lookup"><span data-stu-id="1f016-170">The **Main** method will catch exceptions and write them to the console.</span></span>

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
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
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

<span data-ttu-id="1f016-171">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-171">Press **F5** to run your application.</span></span> <span data-ttu-id="1f016-172">Konsolfönstrets utdata visar meddelandet `End of demo, press any key to exit.` som bekräftar att anslutningen har gjorts.</span><span class="sxs-lookup"><span data-stu-id="1f016-172">The console window output displays the message `End of demo, press any key to exit.` confirming that the connection was made.</span></span>  <span data-ttu-id="1f016-173">Därefter kan du stänga konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="1f016-173">You can then close the console window.</span></span> 

<span data-ttu-id="1f016-174">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-174">Congratulations!</span></span> <span data-ttu-id="1f016-175">Nu när du har anslutit till ett Azure Cosmos DB-konto ska vi gå vidare och se hur du arbetar med Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="1f016-175">You have successfully connected to an Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="1f016-176">Steg 4: Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="1f016-176">Step 4: Create a database</span></span>
<span data-ttu-id="1f016-177">Lägg till en hjälpmetod för skrivning till konsolen innan du lägger till kod som skapar en databas.</span><span class="sxs-lookup"><span data-stu-id="1f016-177">Before you add the code for creating a database, add a helper method for writing to the console.</span></span>

<span data-ttu-id="1f016-178">Kopiera och klistra in metoden **WriteToConsoleAndPromptToContinue** efter metoden **GetStartedDemo**.</span><span class="sxs-lookup"><span data-stu-id="1f016-178">Copy and paste the **WriteToConsoleAndPromptToContinue** method after the **GetStartedDemo** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

<span data-ttu-id="1f016-179">Du kan skapa Azure Cosmos DB-[databasen](documentdb-resources.md#databases) med hjälp av metoden [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) i klassen **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="1f016-179">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using the [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1f016-180">En databas är en logisk behållare för JSON-dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="1f016-180">A database is the logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="1f016-181">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter den skapade klienten.</span><span class="sxs-lookup"><span data-stu-id="1f016-181">Copy and paste the following code to your **GetStartedDemo** method after the client creation.</span></span> <span data-ttu-id="1f016-182">Då skapas en databas med namnet *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="1f016-182">This will create a database named *FamilyDB*.</span></span>

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

<span data-ttu-id="1f016-183">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-183">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-184">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-184">Congratulations!</span></span> <span data-ttu-id="1f016-185">Du har skapat en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="1f016-185">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="1f016-186"><a id="CreateColl"></a>Steg 5: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="1f016-186"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="1f016-187">**CreateDocumentCollectionIfNotExistsAsync** skapar en ny samling med reserverat dataflöde, vilket får konsekvenser för priset.</span><span class="sxs-lookup"><span data-stu-id="1f016-187">**CreateDocumentCollectionIfNotExistsAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="1f016-188">Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="1f016-188">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="1f016-189">Du kan skapa en [samling](documentdb-resources.md#collections) med metoden [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) för klassen **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="1f016-189">A [collection](documentdb-resources.md#collections) can be created by using the [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1f016-190">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="1f016-190">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="1f016-191">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter koden som skapade databasen.</span><span class="sxs-lookup"><span data-stu-id="1f016-191">Copy and paste the following code to your **GetStartedDemo** method after the database creation.</span></span> <span data-ttu-id="1f016-192">Då skapas en dokumentsamling som heter *FamilyCollection*.</span><span class="sxs-lookup"><span data-stu-id="1f016-192">This will create a document collection named *FamilyCollection*.</span></span>

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

<span data-ttu-id="1f016-193">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-193">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-194">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-194">Congratulations!</span></span> <span data-ttu-id="1f016-195">Du har skapat en Azure Cosmos DB-dokumentsamling.</span><span class="sxs-lookup"><span data-stu-id="1f016-195">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="1f016-196"><a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-196"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="1f016-197">Du kan skapa [dokument](documentdb-resources.md#documents) med metoden [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) för klassen **dokumentklient**.</span><span class="sxs-lookup"><span data-stu-id="1f016-197">A [document](documentdb-resources.md#documents) can be created by using the [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="1f016-198">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="1f016-198">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="1f016-199">Vi kan nu infoga ett eller flera dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-199">We can now insert one or more documents.</span></span> <span data-ttu-id="1f016-200">Om du redan har data som du vill lagra i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md) att importera data till en databas.</span><span class="sxs-lookup"><span data-stu-id="1f016-200">If you already have data you'd like to store in your database, you can use the Azure Cosmos DB [Data Migration tool](import-data.md) to import the data into a database.</span></span>

<span data-ttu-id="1f016-201">Först måste vi skapa klassen **Familj** som ska representera objekt som lagras i Azure Cosmos DB i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1f016-201">First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="1f016-202">Vi kommer även att skapa underklasserna **Förälder**, **Barn**, **Husdjur** och **Adress** som används inom **Familj**.</span><span class="sxs-lookup"><span data-stu-id="1f016-202">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="1f016-203">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="1f016-203">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="1f016-204">Skapa dessa klasser genom att lägga till nedanstående interna undergrupper efter metoden **GetStartedDemo**.</span><span class="sxs-lookup"><span data-stu-id="1f016-204">Create these classes by adding the following internal sub-classes after the **GetStartedDemo** method.</span></span>

<span data-ttu-id="1f016-205">Kopiera och klistra in klasserna **Familj**, **Förälder**, **Barn**, **Husdjur** och **Adress** efter metoden **WriteToConsoleAndPromptToContinue**.</span><span class="sxs-lookup"><span data-stu-id="1f016-205">Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** classes after the **WriteToConsoleAndPromptToContinue** method.</span></span>

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="1f016-206">Kopiera och klistra in metoden **CreateFamilyDocumentIfNotExists** under klassen **Adress**.</span><span class="sxs-lookup"><span data-stu-id="1f016-206">Copy and paste the **CreateFamilyDocumentIfNotExists** method underneath your **Address** class.</span></span>

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="1f016-207">Infoga också två dokument, ett för familjen Andersen och ett för familjen Wakefield.</span><span class="sxs-lookup"><span data-stu-id="1f016-207">And insert two documents, one each for the Andersen Family and the Wakefield Family.</span></span>

<span data-ttu-id="1f016-208">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter koden som skapade dokumentsamlingen.</span><span class="sxs-lookup"><span data-stu-id="1f016-208">Copy and paste the following code to your **GetStartedDemo** method after the document collection creation.</span></span>

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="1f016-209">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-209">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-210">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-210">Congratulations!</span></span> <span data-ttu-id="1f016-211">Du har skapat två Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-211">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram som illustrerar den hierarkiska relationen mellan kontot, onlinedatabasen, samlingen och dokumenten som används i NoSQL-självstudiekursen för att skapa en C#-konsolapp](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="1f016-213"><a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="1f016-213"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="1f016-214">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="1f016-214">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="1f016-215">Nedanstående exempelkod visar olika frågor – med både Azure Cosmos DB SQL-syntax och LINQ – som du kan köra mot dokumenten som infogades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="1f016-215">The following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against the documents we inserted in the previous step.</span></span>

<span data-ttu-id="1f016-216">Kopiera och klistra in metoden **ExecuteSimpleQuery** efter metoden **CreateFamilyDocumentIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="1f016-216">Copy and paste the **ExecuteSimpleQuery** method after your **CreateFamilyDocumentIfNotExists** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

<span data-ttu-id="1f016-217">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter koden som skapade det andra dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1f016-217">Copy and paste the following code to your **GetStartedDemo** method after the second document creation.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="1f016-218">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-218">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-219">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-219">Congratulations!</span></span> <span data-ttu-id="1f016-220">Du har skickat frågor mot en Azure Cosmos DB-samling.</span><span class="sxs-lookup"><span data-stu-id="1f016-220">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="1f016-221">Nedanstående diagram illustrerar hur Azure Cosmos DB SQL-frågesyntaxen anropas mot samlingen som du skapade. Samma logik gäller även för LINQ-frågan.</span><span class="sxs-lookup"><span data-stu-id="1f016-221">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created, and the same logic applies to the LINQ query as well.</span></span>

![Diagram som illustrerar omfånget och innebörden av frågan som används i NoSQL-självstudiekursen för att skapa en C#-konsolapp](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="1f016-223">Den [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan eftersom Azure DB som Cosmos-frågor redan är begränsade till en enda samling.</span><span class="sxs-lookup"><span data-stu-id="1f016-223">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="1f016-224">”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="1f016-224">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="1f016-225">Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet som du har valt, referera till den aktuella samlingen som standard.</span><span class="sxs-lookup"><span data-stu-id="1f016-225">Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

## <span data-ttu-id="1f016-226"><a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-226"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="1f016-227">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-227">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="1f016-228">Kopiera och klistra in metoden **ReplaceFamilyDocument** efter metoden **ExecuteSimpleQuery**.</span><span class="sxs-lookup"><span data-stu-id="1f016-228">Copy and paste the **ReplaceFamilyDocument** method after your **ExecuteSimpleQuery** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

<span data-ttu-id="1f016-229">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter frågekörningskoden i slutet av metoden.</span><span class="sxs-lookup"><span data-stu-id="1f016-229">Copy and paste the following code to your **GetStartedDemo** method after the query execution, at the end of the method.</span></span> <span data-ttu-id="1f016-230">När du ersätter dokumentet körs samma fråga igen för att visa det ändrade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1f016-230">After replacing the document, this will run the same query again to view the changed document.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="1f016-231">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-231">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-232">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-232">Congratulations!</span></span> <span data-ttu-id="1f016-233">Du har ersatt ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-233">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="1f016-234"><a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="1f016-234"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="1f016-235">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-235">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="1f016-236">Kopiera och klistra in metoden **DeleteFamilyDocument** efter metoden **ReplaceFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="1f016-236">Copy and paste the **DeleteFamilyDocument** method after your **ReplaceFamilyDocument** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

<span data-ttu-id="1f016-237">Kopiera och klistra in följande kod till metoden **GetStartedDemo** efter den andra frågekörningskoden i slutet av metoden.</span><span class="sxs-lookup"><span data-stu-id="1f016-237">Copy and paste the following code to your **GetStartedDemo** method after the second query execution, at the end of the method.</span></span>

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

<span data-ttu-id="1f016-238">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-238">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-239">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-239">Congratulations!</span></span> <span data-ttu-id="1f016-240">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="1f016-240">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="1f016-241"><a id="DeleteDatabase"></a>Steg 10: Ta bort databasen</span><span class="sxs-lookup"><span data-stu-id="1f016-241"><a id="DeleteDatabase"></a>Step 10: Delete the database</span></span>
<span data-ttu-id="1f016-242">Om du tar bort databasen du skapade försvinner databasen och alla underordnade resurser (t.ex. samlingar och dokument).</span><span class="sxs-lookup"><span data-stu-id="1f016-242">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="1f016-243">Kopiera och klistra in nedanstående kod till metoden **GetStartedDemo** efter dokumentborttagningskoden om du vill ta bort hela databasen och alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="1f016-243">Copy and paste the following code to your **GetStartedDemo** method after the document delete to delete the entire database and all children resources.</span></span>

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

<span data-ttu-id="1f016-244">Kör appen genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="1f016-244">Press **F5** to run your application.</span></span>

<span data-ttu-id="1f016-245">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-245">Congratulations!</span></span> <span data-ttu-id="1f016-246">Du har tagit bort en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="1f016-246">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="1f016-247"><a id="Run"></a>Steg 11: Kör C#-konsolappen i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="1f016-247"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="1f016-248">Tryck på F5 i Visual Studio för att bygga appen i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="1f016-248">Hit F5 in Visual Studio to build the application in debug mode.</span></span>

<span data-ttu-id="1f016-249">Du bör se utdata från get started-appens i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="1f016-249">You should see the output of your get started app in a console window.</span></span> <span data-ttu-id="1f016-250">Dessa utdata visar resultaten för de frågor som vi har lagt till och bör motsvara exempeltexten nedan.</span><span class="sxs-lookup"><span data-stu-id="1f016-250">The output will show the results of the queries we added and should match the example text below.</span></span>

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

<span data-ttu-id="1f016-251">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1f016-251">Congratulations!</span></span> <span data-ttu-id="1f016-252">Du har slutfört självstudiekursen och har ett fungerande C#-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="1f016-252">You've completed the tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="1f016-253"><a id="GetSolution"></a>Hämta den fullständiga lösningen för självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="1f016-253"><a id="GetSolution"></a> Get the complete tutorial solution</span></span>
<span data-ttu-id="1f016-254">Om du inte har tid att slutföra stegen i den här självstudien eller bara vill ladda ned kodexemplen kan du hämta den från [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1f016-254">If you didn't have time to complete the steps in this tutorial, or just want to download the code samples, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> 

<span data-ttu-id="1f016-255">För att bygga GetStarted-lösningen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1f016-255">To build the GetStarted solution, you will need the following:</span></span>

* <span data-ttu-id="1f016-256">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1f016-256">An active Azure account.</span></span> <span data-ttu-id="1f016-257">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1f016-257">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="1f016-258">En [Azure Cosmos DB konto][cosmos-db-create-account].</span><span class="sxs-lookup"><span data-stu-id="1f016-258">An [Azure Cosmos DB account][cosmos-db-create-account].</span></span>
* <span data-ttu-id="1f016-259">[GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)-lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1f016-259">The [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="1f016-260">Om du vill återställa referenser till Azure Cosmos DB .NET SDK i Visual Studio högerklickar du på den **GetStarted** lösningen i Solution Explorer och klicka sedan på **aktivera NuGet-Paketåterställning**.</span><span class="sxs-lookup"><span data-stu-id="1f016-260">To restore the references to the Azure Cosmos DB .NET SDK in Visual Studio, right-click the **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="1f016-261">I filen App.config uppdaterar du sedan värdena EndpointUrl och AuthorizationKey enligt anvisningarna i [Ansluta till ett Azure Cosmos DB-konto](#Connect).</span><span class="sxs-lookup"><span data-stu-id="1f016-261">Next, in the App.config file, update the EndpointUrl and AuthorizationKey values as described in [Connect to an Azure Cosmos DB account](#Connect).</span></span>

<span data-ttu-id="1f016-262">Då är det bara att bygga den, så är du på väg!</span><span class="sxs-lookup"><span data-stu-id="1f016-262">That's it, build it and you're on your way!</span></span>


## <a name="next-steps"></a><span data-ttu-id="1f016-263">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f016-263">Next steps</span></span>
* <span data-ttu-id="1f016-264">Behöver du en mer komplex ASP.NET MVC-självstudiekurs?</span><span class="sxs-lookup"><span data-stu-id="1f016-264">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="1f016-265">Se [självstudiekurs om ASP.NET MVC: utveckling av Webbappar med Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="1f016-265">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="1f016-266">Vill du testa skalning och prestanda med Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="1f016-266">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="1f016-267">Se [prestanda och skalning testa med Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="1f016-267">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="1f016-268">Lär dig hur du [övervaka Azure Cosmos DB begäranden, användning och lagring](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="1f016-268">Learn how to [monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="1f016-269">Kör frågor mot vår exempeldatauppsättning i [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="1f016-269">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="1f016-270">Läs mer om Azure Cosmos DB i [Välkommen till Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="1f016-270">To learn more about Azure Cosmos DB, see [Welcome to Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
