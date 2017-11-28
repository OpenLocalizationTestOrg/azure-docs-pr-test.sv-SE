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
# <a name="azure-cosmos-db-getting-started-with-hello-documentdb-api-and-net-core"></a><span data-ttu-id="20196-103">Azure Cosmos DB: Komma igång med hello DocumentDB-API och .NET Core</span><span class="sxs-lookup"><span data-stu-id="20196-103">Azure Cosmos DB: Getting started with hello DocumentDB API and .NET Core</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="20196-104">.NET</span><span class="sxs-lookup"><span data-stu-id="20196-104">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="20196-105">.NET Core</span><span class="sxs-lookup"><span data-stu-id="20196-105">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="20196-106">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="20196-106">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="20196-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="20196-107">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="20196-108">Java</span><span class="sxs-lookup"><span data-stu-id="20196-108">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="20196-109">C++</span><span class="sxs-lookup"><span data-stu-id="20196-109">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="20196-110">Välkommen till toohello DocumentDB API: et för Azure Cosmos DB komma igång med .NET Core självstudiekursen!</span><span class="sxs-lookup"><span data-stu-id="20196-110">Welcome toohello DocumentDB API for Azure Cosmos DB getting started with .NET Core tutorial!</span></span> <span data-ttu-id="20196-111">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="20196-111">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="20196-112">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="20196-112">We'll cover:</span></span>

* <span data-ttu-id="20196-113">Skapa och ansluta tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="20196-113">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="20196-114">Konfigurera en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="20196-114">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="20196-115">Skapa en onlinedatabas</span><span class="sxs-lookup"><span data-stu-id="20196-115">Creating an online database</span></span>
* <span data-ttu-id="20196-116">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="20196-116">Creating a collection</span></span>
* <span data-ttu-id="20196-117">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="20196-117">Creating JSON documents</span></span>
* <span data-ttu-id="20196-118">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="20196-118">Querying hello collection</span></span>
* <span data-ttu-id="20196-119">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="20196-119">Replacing a document</span></span>
* <span data-ttu-id="20196-120">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="20196-120">Deleting a document</span></span>
* <span data-ttu-id="20196-121">Tar bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="20196-121">Deleting hello database</span></span>

<span data-ttu-id="20196-122">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="20196-122">Don't have time?</span></span> <span data-ttu-id="20196-123">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="20196-123">Don't worry!</span></span> <span data-ttu-id="20196-124">hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="20196-124">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span> <span data-ttu-id="20196-125">Hoppa toohello [hämta hello kompletta lösningen avsnittet](#GetSolution) snabbguide.</span><span class="sxs-lookup"><span data-stu-id="20196-125">Jump toohello [Get hello complete solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="20196-126">Vill toobuild en Xamarin iOS, Android eller formulär med hjälp av programmet hello DocumentDB-API och SDK för .NET Core?</span><span class="sxs-lookup"><span data-stu-id="20196-126">Want toobuild a Xamarin iOS, Android, or Forms application using hello DocumentDB API and .NET Core SDK?</span></span> <span data-ttu-id="20196-127">Se [bygga mobila program med Xamarin och Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="20196-127">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>

> [!NOTE]
> <span data-ttu-id="20196-128">hello Azure Cosmos DB .NET Core SDK används i den här kursen är inte kompatibel med den universella Windowsplattformen (UWP) appar ännu.</span><span class="sxs-lookup"><span data-stu-id="20196-128">hello Azure Cosmos DB .NET Core SDK used in this tutorial is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="20196-129">För en förhandsversion av hello .NET Core SDK som stöder UWP-appar, skicka e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="20196-129">For a preview version of hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

<span data-ttu-id="20196-130">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="20196-130">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20196-131">Krav</span><span class="sxs-lookup"><span data-stu-id="20196-131">Prerequisites</span></span>
<span data-ttu-id="20196-132">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="20196-132">Please make sure you have hello following:</span></span>

* <span data-ttu-id="20196-133">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="20196-133">An active Azure account.</span></span> <span data-ttu-id="20196-134">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="20196-134">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="20196-135">Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="20196-135">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="20196-136">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="20196-136">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/) 
    * <span data-ttu-id="20196-137">Om du arbetar på MacOS- eller Linux, kan du utveckla .NET Core appar från kommandoraden hello genom att installera hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) för hello plattform som du väljer.</span><span class="sxs-lookup"><span data-stu-id="20196-137">If you're working on MacOS or Linux, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) for hello platform of your choice.</span></span> 
    * <span data-ttu-id="20196-138">Om du arbetar i Windows kan du utveckla .NET Core appar från kommandoraden hello genom att installera hello [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span><span class="sxs-lookup"><span data-stu-id="20196-138">If you're working on Windows, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span></span> 
    * <span data-ttu-id="20196-139">Du kan använda din egen redigerare eller hämta [Visual Studio Code](https://code.visualstudio.com/) som är kostnadsfritt och fungerar i Windows, Linux och MacOS.</span><span class="sxs-lookup"><span data-stu-id="20196-139">You can use your own editor, or download [Visual Studio Code](https://code.visualstudio.com/) which is free and works on Windows, Linux, and MacOS.</span></span> 

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="20196-140">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="20196-140">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="20196-141">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="20196-141">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="20196-142">Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="20196-142">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="20196-143">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="20196-143">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="20196-144"><a id="SetupVS"></a>Steg 2: Konfigurera en lösning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20196-144"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="20196-145">Öppna **Visual Studio 2017** i datorn.</span><span class="sxs-lookup"><span data-stu-id="20196-145">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="20196-146">På hello **filen** väljer du **ny**, och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="20196-146">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="20196-147">I hello **nytt projekt** markerar **mallar** / **Visual C#** / **.NET Core** / **Konsolprogram (.NET Core)**, namnge projektet **DocumentDBGettingStarted**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="20196-147">In hello **New Project** dialog, select **Templates** / **Visual C#** / **.NET Core**/**Console Application (.NET Core)**, name your project **DocumentDBGettingStarted**, and then click **OK**.</span></span>

   ![Skärmbild som visar hello-fönstret nytt projekt](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. <span data-ttu-id="20196-149">I hello **Solution Explorer**, högerklickar du på **DocumentDBGettingStarted**.</span><span class="sxs-lookup"><span data-stu-id="20196-149">In hello **Solution Explorer**, right click on **DocumentDBGettingStarted**.</span></span>
5. <span data-ttu-id="20196-150">Klicka på utan att lämna menyn hello **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="20196-150">Then without leaving hello menu, click on **Manage NuGet Packages...**.</span></span>

   ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. <span data-ttu-id="20196-152">I hello **NuGet** klickar du på **Bläddra** hello överst i hello och skriver **azure documentdb** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="20196-152">In hello **NuGet** tab, click **Browse** at hello top of hello window, and type **azure documentdb** in hello search box.</span></span>
7. <span data-ttu-id="20196-153">I hello resultat hitta **Microsoft.Azure.DocumentDB.Core** och på **installera**.</span><span class="sxs-lookup"><span data-stu-id="20196-153">Within hello results, find **Microsoft.Azure.DocumentDB.Core** and click **Install**.</span></span>
   <span data-ttu-id="20196-154">hello paket-ID för hello DocumentDB klientbibliotek för .NET Core är [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span><span class="sxs-lookup"><span data-stu-id="20196-154">hello package ID for hello DocumentDB Client Library for .NET Core is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span></span> <span data-ttu-id="20196-155">Om du arbetar med en .NET Framework-version (t.ex. net461) som inte stöds av det här .NET Core NuGet-paketet använder du sedan [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) som har stöd för alla .NET Framework-versioner från och med .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="20196-155">If you are targeting a .NET Framework version (like net461) that is not supported by this .NET Core NuGet package, then use [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) that supports all .NET Framework versions starting .NET Framework 4.5.</span></span>
8. <span data-ttu-id="20196-156">Vid hello prompter Godkänn hello NuGet-paketet installationer hello licensavtalet (EULA).</span><span class="sxs-lookup"><span data-stu-id="20196-156">At hello prompts, accept hello NuGet package installations and hello license agreement.</span></span>

<span data-ttu-id="20196-157">Bra!</span><span class="sxs-lookup"><span data-stu-id="20196-157">Great!</span></span> <span data-ttu-id="20196-158">Nu när vi avslutat hello inställningarna Låt oss börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="20196-158">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="20196-159">Det finns ett färdigt kodprojekt för den här självstudiekursen i [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="20196-159">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span>

## <span data-ttu-id="20196-160"><a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="20196-160"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="20196-161">Lägg först till dessa refererar till toohello början av C#-appen, i hello Program.cs-filen:</span><span class="sxs-lookup"><span data-stu-id="20196-161">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

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
> <span data-ttu-id="20196-162">I ordning toocomplete självstudierna, Lägg till hello ovanstående beroenden.</span><span class="sxs-lookup"><span data-stu-id="20196-162">In order toocomplete this tutorial, make sure you add hello dependencies above.</span></span>

<span data-ttu-id="20196-163">Lägg till nedanstående två konstanter och *klientvariabeln* under den offentliga klassen *Program*.</span><span class="sxs-lookup"><span data-stu-id="20196-163">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

```csharp
class Program
{
    // ADD THIS PART tooYOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

<span data-ttu-id="20196-164">Gå sedan toohello [Azure Portal](https://portal.azure.com) tooretrieve din URI och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="20196-164">Next, head toohello [Azure Portal](https://portal.azure.com) tooretrieve your URI and primary key.</span></span> <span data-ttu-id="20196-165">hello Azure Cosmos DB URI och primärnyckel krävs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.</span><span class="sxs-lookup"><span data-stu-id="20196-165">hello Azure Cosmos DB URI and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="20196-166">I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="20196-166">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="20196-167">Kopiera hello URI från hello-portalen och klistrar in det i `<your endpoint URI>` i hello program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="20196-167">Copy hello URI from hello portal and paste it into `<your endpoint URI>` in hello program.cs file.</span></span> <span data-ttu-id="20196-168">Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `<your key>`.</span><span class="sxs-lookup"><span data-stu-id="20196-168">Then copy hello PRIMARY KEY from hello portal and paste it into `<your key>`.</span></span> <span data-ttu-id="20196-169">Om du använder hello Azure Cosmos DB emulatorn använder `https://localhost:8081` som hello slutpunkt och hello väldefinierade auktoriseringsnyckeln från [hur toodevelop med hello Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="20196-169">If you are using hello Azure Cosmos DB Emulator, use `https://localhost:8081` as hello endpoint, and hello well-defined authorization key from [How toodevelop using hello Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="20196-170">Se till att tooremove Hej < och > men lämna hello dubbla citattecken runt din slutpunkt och nyckel.</span><span class="sxs-lookup"><span data-stu-id="20196-170">Make sure tooremove hello < and > but leave hello double quotes around your endpoint and key.</span></span>

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram.][keys]

<span data-ttu-id="20196-173">Vi börjar hello komma igång program genom att skapa en ny instans av hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="20196-173">We'll start hello getting started application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="20196-174">Nedan hello **Main** metod, lägga till den här nya asynkrona aktiviteten kallad **GetStartedDemo**, som instantierar vår nya **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="20196-174">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

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

<span data-ttu-id="20196-175">Lägg till hello följande kod toorun den asynkrona aktiviteten från din **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-175">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="20196-176">Hej **Main** metod ska fånga undantag och skriva dem toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="20196-176">hello **Main** method will catch exceptions and write them toohello console.</span></span>

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

<span data-ttu-id="20196-177">Tryck på hello **DocumentDBGettingStarted** knappen toobuild och kör programmet hello.</span><span class="sxs-lookup"><span data-stu-id="20196-177">Press hello **DocumentDBGettingStarted** button toobuild and run hello application.</span></span>

<span data-ttu-id="20196-178">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-178">Congratulations!</span></span> <span data-ttu-id="20196-179">Du har anslutit tooan Azure Cosmos DB konto nu ska gå vi igenom hur man arbetar med Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="20196-179">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="20196-180">Steg 4: Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="20196-180">Step 4: Create a database</span></span>
<span data-ttu-id="20196-181">Lägg till en hjälpmetod för skrivning toohello konsolen innan du lägger till hello-kod för att skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="20196-181">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="20196-182">Kopiera och klistra in hello **WriteToConsoleAndPromptToContinue** under hello metoden **GetStartedDemo** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-182">Copy and paste hello **WriteToConsoleAndPromptToContinue** method underneath hello **GetStartedDemo** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="20196-183">Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="20196-183">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="20196-184">En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="20196-184">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="20196-185">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod under hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="20196-185">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello client creation.</span></span> <span data-ttu-id="20196-186">Då skapas en databas med namnet *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="20196-186">This will create a database named *FamilyDB*.</span></span>

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

<span data-ttu-id="20196-187">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-187">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-188">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-188">Congratulations!</span></span> <span data-ttu-id="20196-189">Du har skapat en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="20196-189">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="20196-190"><a id="CreateColl"></a>Steg 5: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="20196-190"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="20196-191">**CreateDocumentCollectionAsync** skapar en ny samling med reserverat dataflöde, vilket får konsekvenser för priset.</span><span class="sxs-lookup"><span data-stu-id="20196-191">**CreateDocumentCollectionAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="20196-192">Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="20196-192">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>

<span data-ttu-id="20196-193">En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="20196-193">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="20196-194">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="20196-194">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="20196-195">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello databasen skapas metoden.</span><span class="sxs-lookup"><span data-stu-id="20196-195">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello database creation.</span></span> <span data-ttu-id="20196-196">Då skapas en dokumentsamling som heter *FamilyCollection_oa*.</span><span class="sxs-lookup"><span data-stu-id="20196-196">This will create a document collection named *FamilyCollection_oa*.</span></span>

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

<span data-ttu-id="20196-197">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-197">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-198">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-198">Congratulations!</span></span> <span data-ttu-id="20196-199">Du har skapat en Azure Cosmos DB-dokumentsamling.</span><span class="sxs-lookup"><span data-stu-id="20196-199">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="20196-200"><a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="20196-200"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="20196-201">En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="20196-201">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="20196-202">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="20196-202">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="20196-203">Vi kan nu infoga ett eller flera dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-203">We can now insert one or more documents.</span></span> <span data-ttu-id="20196-204">Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="20196-204">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md).</span></span>

<span data-ttu-id="20196-205">Vi måste först, toocreate en **familj** klass som ska representera objekt som lagras i Azure Cosmos DB i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="20196-205">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="20196-206">Vi kommer även att skapa underklasserna **Förälder**, **Barn**, **Husdjur** och **Adress** som används inom **Familj**.</span><span class="sxs-lookup"><span data-stu-id="20196-206">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="20196-207">Observera att dokument måste ha en **id**-egenskap serialiserad som **id** i JSON.</span><span class="sxs-lookup"><span data-stu-id="20196-207">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="20196-208">Skapa dessa klasser genom att lägga till följande interna undergrupper efter hello hello **GetStartedDemo** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-208">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="20196-209">Kopiera och klistra in hello **familj**, **överordnade**, **underordnade**, **husdjur**, och **adress** klasser under Hej **WriteToConsoleAndPromptToContinue** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-209">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes underneath hello **WriteToConsoleAndPromptToContinue** method.</span></span>

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

<span data-ttu-id="20196-210">Kopiera och klistra in hello **CreateFamilyDocumentIfNotExists** under metoden din **CreateDocumentCollectionIfNotExists** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-210">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **CreateDocumentCollectionIfNotExists** method.</span></span>

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

<span data-ttu-id="20196-211">Och infoga två dokument, ett för hello familjen Andersen och hello familjen Wakefield.</span><span class="sxs-lookup"><span data-stu-id="20196-211">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="20196-212">Kopiera och klistra in hello-kod som följer `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** under hello dokumentsamlingen metoden.</span><span class="sxs-lookup"><span data-stu-id="20196-212">Copy and paste hello code that follows `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** method underneath hello document collection creation.</span></span>

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

<span data-ttu-id="20196-213">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-213">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-214">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-214">Congratulations!</span></span> <span data-ttu-id="20196-215">Du har skapat två Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-215">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello onlinedatabasen, hello samlingen och hello dokument används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="20196-217"><a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="20196-217"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="20196-218">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="20196-218">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="20196-219">hello följande exempelkod visar olika frågor – med både Azure Cosmos-Databasens SQL syntax samt LINQ – som du kan köra mot hello dokument som vi infogas i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="20196-219">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="20196-220">Kopiera och klistra in hello **ExecuteSimpleQuery** under metoden din **CreateFamilyDocumentIfNotExists** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-220">Copy and paste hello **ExecuteSimpleQuery** method underneath your **CreateFamilyDocumentIfNotExists** method.</span></span>

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

<span data-ttu-id="20196-221">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** metod under hello andra dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-221">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second document creation.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART tooYOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="20196-222">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-222">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-223">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-223">Congratulations!</span></span> <span data-ttu-id="20196-224">Du har skickat frågor mot en Azure Cosmos DB-samling.</span><span class="sxs-lookup"><span data-stu-id="20196-224">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="20196-225">hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade och hello samma logik gäller även toohello LINQ-fråga.</span><span class="sxs-lookup"><span data-stu-id="20196-225">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![Diagram som illustrerar hello omfånget och innebörden av hello-fråga som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="20196-227">Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling.</span><span class="sxs-lookup"><span data-stu-id="20196-227">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="20196-228">”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="20196-228">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="20196-229">DocumentDB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.</span><span class="sxs-lookup"><span data-stu-id="20196-229">DocumentDB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="20196-230"><a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="20196-230"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="20196-231">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-231">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="20196-232">Kopiera och klistra in hello **ReplaceFamilyDocument** under metoden din **ExecuteSimpleQuery** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-232">Copy and paste hello **ReplaceFamilyDocument** method underneath your **ExecuteSimpleQuery** method.</span></span>

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

<span data-ttu-id="20196-233">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello Frågekörningen metoden.</span><span class="sxs-lookup"><span data-stu-id="20196-233">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello query execution.</span></span> <span data-ttu-id="20196-234">När du ersätter dokumentet hello hello körs samma fråga igen tooview hello ändra dokumentet.</span><span class="sxs-lookup"><span data-stu-id="20196-234">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
// Update hello Grade of hello Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="20196-235">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-235">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-236">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-236">Congratulations!</span></span> <span data-ttu-id="20196-237">Du har ersatt ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-237">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="20196-238"><a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="20196-238"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="20196-239">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-239">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="20196-240">Kopiera och klistra in hello **DeleteFamilyDocument** under metoden din **ReplaceFamilyDocument** metod.</span><span class="sxs-lookup"><span data-stu-id="20196-240">Copy and paste hello **DeleteFamilyDocument** method underneath your **ReplaceFamilyDocument** method.</span></span>

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

<span data-ttu-id="20196-241">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello andra Frågekörningen metoden.</span><span class="sxs-lookup"><span data-stu-id="20196-241">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second query execution.</span></span>

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooCODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

<span data-ttu-id="20196-242">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-242">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-243">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-243">Congratulations!</span></span> <span data-ttu-id="20196-244">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="20196-244">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="20196-245"><a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="20196-245"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="20196-246">Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).</span><span class="sxs-lookup"><span data-stu-id="20196-246">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="20196-247">Kopiera och klistra in hello följande kod tooyour **GetStartedDemo** under hello dokumentet metoden ta bort toodelete hello hela databasen och alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="20196-247">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello document delete toodelete hello entire database and all children resources.</span></span>

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART tooCODE
// Clean up/delete hello database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

<span data-ttu-id="20196-248">Tryck på hello **DocumentDBGettingStarted** knappen toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="20196-248">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="20196-249">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-249">Congratulations!</span></span> <span data-ttu-id="20196-250">Du har tagit bort en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="20196-250">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="20196-251"><a id="Run"></a>Steg 11: Kör C#-konsolappen i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="20196-251"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="20196-252">Tryck på hello **DocumentDBGettingStarted** knapp i Visual Studio toobuild hello appen i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="20196-252">Press hello **DocumentDBGettingStarted** button in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="20196-253">Du bör se hello utdata från get started-appens i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="20196-253">You should see hello output of your get started app in hello console window.</span></span> <span data-ttu-id="20196-254">hello utdata visar hello resultaten av hello frågor som vi har lagt till och bör matcha hello exempeltexten nedan.</span><span class="sxs-lookup"><span data-stu-id="20196-254">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

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

<span data-ttu-id="20196-255">Grattis!</span><span class="sxs-lookup"><span data-stu-id="20196-255">Congratulations!</span></span> <span data-ttu-id="20196-256">Du har slutfört hello självstudier och har en fungerande C#-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="20196-256">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="20196-257"><a id="GetSolution"></a>Hämta hello fullständiga lösningen till självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="20196-257"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="20196-258">toobuild hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="20196-258">toobuild hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="20196-259">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="20196-259">An active Azure account.</span></span> <span data-ttu-id="20196-260">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="20196-260">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="20196-261">En [Azure Cosmos DB konto][create-documentdb-dotnet.md#create-account].</span><span class="sxs-lookup"><span data-stu-id="20196-261">An [Azure Cosmos DB account][create-documentdb-dotnet.md#create-account].</span></span>
* <span data-ttu-id="20196-262">Hej [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="20196-262">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="20196-263">toorestore hello referenser toohello DocumentDB API: et för Azure Cosmos DB .NET Core SDK i Visual Studio högerklickar du på hello **GetStarted** lösningen i Solution Explorer och klicka sedan på **aktivera NuGet-Paketåterställning**.</span><span class="sxs-lookup"><span data-stu-id="20196-263">toorestore hello references toohello DocumentDB API for Azure Cosmos DB .NET Core SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="20196-264">I Program.cs-filen för hello uppdatera värdena för hello EndpointUrl och AuthorizationKey enligt beskrivningen i [ansluta tooan Azure Cosmos DB konto](#Connect).</span><span class="sxs-lookup"><span data-stu-id="20196-264">Next, in hello Program.cs file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

## <a name="next-steps"></a><span data-ttu-id="20196-265">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20196-265">Next steps</span></span>
* <span data-ttu-id="20196-266">Behöver du en mer komplex ASP.NET MVC-självstudiekurs?</span><span class="sxs-lookup"><span data-stu-id="20196-266">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="20196-267">Se [självstudiekurs om ASP.NET MVC: utveckling av Webbappar med Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="20196-267">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="20196-268">Vill toodevelop en Xamarin iOS, Android eller formulär med hjälp av programmet hello DocumentDB API för Azure Cosmos DB .NET Core SDK?</span><span class="sxs-lookup"><span data-stu-id="20196-268">Want toodevelop a Xamarin iOS, Android, or Forms application using hello DocumentDB API for Azure Cosmos DB .NET Core SDK?</span></span> <span data-ttu-id="20196-269">Se [bygga mobila program med Xamarin och Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="20196-269">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>
* <span data-ttu-id="20196-270">Vill tooperform skala och prestandatester med Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="20196-270">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="20196-271">Se [prestanda och skalning testa med Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="20196-271">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="20196-272">Lär dig hur för[övervakaren Azure Cosmos DB begäranden, användning och lagring](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="20196-272">Learn how too[Monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="20196-273">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="20196-273">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="20196-274">toolearn mer om hello programmeringsmodell, se [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="20196-274">toolearn more about hello programming model, see [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
