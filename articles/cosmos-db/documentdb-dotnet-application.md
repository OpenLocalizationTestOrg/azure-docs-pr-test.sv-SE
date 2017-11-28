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
# <span data-ttu-id="e0682-105"><a name="_Toc395809351"></a>Självstudiekurs om ASP.NET MVC: Utveckling av webbprogram med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e0682-105"><a name="_Toc395809351"></a>ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0682-106">.NET</span><span class="sxs-lookup"><span data-stu-id="e0682-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="e0682-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="e0682-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="e0682-108">Java</span><span class="sxs-lookup"><span data-stu-id="e0682-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="e0682-109">Python</span><span class="sxs-lookup"><span data-stu-id="e0682-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="e0682-110">toohighlight hur du effektivt utnyttja Azure Cosmos DB toostore och fråga JSON-dokument, den här artikeln innehåller en slutpunkt till slutpunkt genomgången visar hur toobuild en todo-app med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e0682-110">toohighlight how you can efficiently leverage Azure Cosmos DB toostore and query JSON documents, this article provides an end-to-end walk-through showing you how toobuild a todo app using Azure Cosmos DB.</span></span> <span data-ttu-id="e0682-111">hello uppgifter lagras som JSON-dokument i Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="e0682-111">hello tasks will be stored as JSON documents in Azure Cosmos DB.</span></span>

![Skärmbild som visar hello todo-listan MVC-webbapp som skapats i denna självstudiekurs – stegvis självstudiekurs i ASP NET MVC](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

<span data-ttu-id="e0682-113">Den här genomgången visar hur toouse hello Azure Cosmos DB service toostore och komma åt data från en ASP.NET MVC-webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="e0682-113">This walk-through shows you how toouse hello Azure Cosmos DB service toostore and access data from an ASP.NET MVC web application hosted on Azure.</span></span> <span data-ttu-id="e0682-114">Om du letar efter en självstudiekurs som endast fokuserar på Azure Cosmos DB och inte hello ASP.NET MVC-komponenter, se [skapa ett Azure Cosmos DB C#-konsolprogram](documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e0682-114">If you're looking for a tutorial that focuses only on Azure Cosmos DB, and not hello ASP.NET MVC components, see [Build an Azure Cosmos DB C# console application](documentdb-get-started.md).</span></span>

> [!TIP]
> <span data-ttu-id="e0682-115">Den här självstudiekursen förutsätter att du har tidigare erfarenhet av MVC i ASP.NET och Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="e0682-115">This tutorial assumes that you have prior experience using ASP.NET MVC and Azure Websites.</span></span> <span data-ttu-id="e0682-116">Om du är ny tooASP.NET eller hello [nödvändiga verktyg](#_Toc395637760), vi rekommenderar att du hämtar hello fullständiga exempelprojektet från [GitHub] [ GitHub] och hello instruktionerna i Det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="e0682-116">If you are new tooASP.NET or hello [prerequisite tools](#_Toc395637760), we recommend downloading hello complete sample project from [GitHub][GitHub] and following hello instructions in this sample.</span></span> <span data-ttu-id="e0682-117">När du har skapat kan granska du den här artikeln toogain information om hello koden i hello ramen för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="e0682-117">Once you have it built, you can review this article toogain insight on hello code in hello context of hello project.</span></span>
> 
> 

## <span data-ttu-id="e0682-118"><a name="_Toc395637760"></a>Förhandskrav för den här databas-självstudien</span><span class="sxs-lookup"><span data-stu-id="e0682-118"><a name="_Toc395637760"></a>Prerequisites for this database tutorial</span></span>
<span data-ttu-id="e0682-119">Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="e0682-120">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e0682-120">An active Azure account.</span></span> <span data-ttu-id="e0682-121">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e0682-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e0682-122">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0682-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

    <span data-ttu-id="e0682-123">ELLER</span><span class="sxs-lookup"><span data-stu-id="e0682-123">OR</span></span>

    <span data-ttu-id="e0682-124">En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="e0682-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="e0682-125">[Visual Studio-2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e0682-125">[Visual Studio 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="e0682-126">Microsoft Azure SDK för .NET för Visual Studio 2017 tillgängliga via hello Visual Studio-installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e0682-126">Microsoft Azure SDK for .NET for Visual Studio 2017, available through hello Visual Studio Installer.</span></span>

<span data-ttu-id="e0682-127">Alla hello skärmdumpar i den här artikeln har vidtagits med hjälp av Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="e0682-127">All hello screen shots in this article have been taken using Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="e0682-128">Om din dator har konfigurerats med en annan version är det möjligt att skärmbilder och alternativ inte ser riktigt likadana ut, men om du uppfyller hello ovan krav ska lösningen fungera.</span><span class="sxs-lookup"><span data-stu-id="e0682-128">If your system is configured with a different version it is possible that your screens and options won't match entirely, but if you meet hello above prerequisites this solution should work.</span></span>

## <span data-ttu-id="e0682-129"><a name="_Toc395637761"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="e0682-129"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="e0682-130">Vi ska börja med att skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="e0682-130">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="e0682-131">Om du redan har ett konto för SQL (DocumentDB) för Azure Cosmos DB eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[skapa ett nytt ASP.NET MVC-program](#_Toc395637762).</span><span class="sxs-lookup"><span data-stu-id="e0682-131">If you already have a SQL (DocumentDB) account for Azure Cosmos DB or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Create a new ASP.NET MVC application](#_Toc395637762).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
<span data-ttu-id="e0682-132">Vi kommer att gå igenom hur toocreate ett nytt ASP.NET MVC-program från hello grunden.</span><span class="sxs-lookup"><span data-stu-id="e0682-132">We will now walk through how toocreate a new ASP.NET MVC application from hello ground-up.</span></span> 

## <span data-ttu-id="e0682-133"><a name="_Toc395637762"></a>Steg 2: Skapa ett nytt ASP.NET MVC-program</span><span class="sxs-lookup"><span data-stu-id="e0682-133"><a name="_Toc395637762"></a>Step 2: Create a new ASP.NET MVC application</span></span>

1. <span data-ttu-id="e0682-134">I Visual Studio på hello **filen** -menyn, peka för**ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="e0682-134">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span> <span data-ttu-id="e0682-135">Hej **nytt projekt** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e0682-135">hello **New Project** dialog box appears.</span></span>

2. <span data-ttu-id="e0682-136">I hello **projekttyper** rutan Expandera **mallar**, **Visual C#**, **Web**, och välj sedan **ASP.NET-webbprogram** .</span><span class="sxs-lookup"><span data-stu-id="e0682-136">In hello **Project types** pane, expand **Templates**, **Visual C#**, **Web**, and then select **ASP.NET Web Application**.</span></span>

      ![Skärmbild av dialogrutan för hello nytt projekt med projekttypen hello ASP.NET-webbprogram markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. <span data-ttu-id="e0682-138">I hello **namn** rutan, hello-typnamn för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="e0682-138">In hello **Name** box, type hello name of hello project.</span></span> <span data-ttu-id="e0682-139">Den här kursen använder hello namnet ”todo”.</span><span class="sxs-lookup"><span data-stu-id="e0682-139">This tutorial uses hello name "todo".</span></span> <span data-ttu-id="e0682-140">Om du väljer toouse något annat än detta, oavsett var nämns namnområdet todo hello måste tooadjust hello tillhandahålls kod prover toouse det namn du gav ditt program.</span><span class="sxs-lookup"><span data-stu-id="e0682-140">If you choose toouse something other than this, then wherever this tutorial talks about hello todo namespace, you need tooadjust hello provided code samples toouse whatever you named your application.</span></span> 
4. <span data-ttu-id="e0682-141">Klicka på **Bläddra** toonavigate toohello mappen där du gillar toocreate hello projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0682-141">Click **Browse** toonavigate toohello folder where you would like toocreate hello project, and then click **OK**.</span></span>
   
      <span data-ttu-id="e0682-142">Hej **nytt ASP.NET-webbprogram** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e0682-142">hello **New ASP.NET Web Application** dialog box appears.</span></span>
   
    ![Skärmbild som visar dialogrutan Nytt ASP.NET-webbprogram för hello med hello MVC-appmallen markerad](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. <span data-ttu-id="e0682-144">I hello mallar, markerar du **MVC**.</span><span class="sxs-lookup"><span data-stu-id="e0682-144">In hello templates pane, select **MVC**.</span></span>

6. <span data-ttu-id="e0682-145">Klicka på **OK** och låt Visual Studio gör dess saker runt scaffold-teknik hello tomma ASP.NET MVC-mallen.</span><span class="sxs-lookup"><span data-stu-id="e0682-145">Click **OK** and let Visual Studio do its thing around scaffolding hello empty ASP.NET MVC template.</span></span> 

          
7. <span data-ttu-id="e0682-146">När Visual Studio har skapat hello formaterad MVC-program har en tom ASP.NET-program som du kan köra lokalt.</span><span class="sxs-lookup"><span data-stu-id="e0682-146">Once Visual Studio has finished creating hello boilerplate MVC application you have an empty ASP.NET application that you can run locally.</span></span>
   
    <span data-ttu-id="e0682-147">Vi hoppar över körs hello projektet lokalt nu eftersom du säkert har vi alla upptäckta hello ASP.NET ”Hello World” program.</span><span class="sxs-lookup"><span data-stu-id="e0682-147">We'll skip running hello project locally because I'm sure we've all seen hello ASP.NET "Hello World" application.</span></span> <span data-ttu-id="e0682-148">Nu ska raka tooadding Azure Cosmos DB toothis projektet och bygga vår App.</span><span class="sxs-lookup"><span data-stu-id="e0682-148">Let's go straight tooadding Azure Cosmos DB toothis project and building our application.</span></span>

## <span data-ttu-id="e0682-149"><a name="_Toc395637767"></a>Steg 3: Lägg till Azure Cosmos DB tooyour MVC-webbapprojektet</span><span class="sxs-lookup"><span data-stu-id="e0682-149"><a name="_Toc395637767"></a>Step 3: Add Azure Cosmos DB tooyour MVC web application project</span></span>
<span data-ttu-id="e0682-150">Nu när vi har de flesta av hello ASP.NET MVC-grunder vi behöver för lösningen ska få toohello verkliga syftet med den här självstudiekursen kommer att lägga till Azure Cosmos DB tooour MVC-webbapp.</span><span class="sxs-lookup"><span data-stu-id="e0682-150">Now that we have most of hello ASP.NET MVC plumbing that we need for this solution, let's get toohello real purpose of this tutorial, adding Azure Cosmos DB tooour MVC web application.</span></span>

1. <span data-ttu-id="e0682-151">hello Azure Cosmos DB .NET SDK paketeras och distribueras som ett NuGet paket.</span><span class="sxs-lookup"><span data-stu-id="e0682-151">hello Azure Cosmos DB .NET SDK is packaged and distributed as a NuGet package.</span></span> <span data-ttu-id="e0682-152">tooget hello NuGet-paketet i Visual Studio kan du använda hello NuGet-Pakethanteraren i Visual Studio genom att högerklicka på projektet hello i **Solution Explorer** och sedan klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="e0682-152">tooget hello NuGet package in Visual Studio, use hello NuGet package manager in Visual Studio by right-clicking on hello project in **Solution Explorer** and then clicking **Manage NuGet Packages**.</span></span>
   
    ![Skärmbild som visar hello Högerklicka alternativ för hello webbapprojektet i Solution Explorer, med hantera NuGet-paket markerat.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    <span data-ttu-id="e0682-154">Hej **hantera NuGet-paket** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e0682-154">hello **Manage NuGet Packages** dialog box appears.</span></span>
2. <span data-ttu-id="e0682-155">I hello NuGet **Bläddra** skriver ***Azure DocumentDB***.</span><span class="sxs-lookup"><span data-stu-id="e0682-155">In hello NuGet **Browse** box, type ***Azure DocumentDB***.</span></span> <span data-ttu-id="e0682-156">(hello paketnamn inte har uppdaterats tooAzure Cosmos DB.)</span><span class="sxs-lookup"><span data-stu-id="e0682-156">(hello package name has not been updated tooAzure Cosmos DB.)</span></span>
   
    <span data-ttu-id="e0682-157">Hello resultat för att installera hello **Microsoft.Azure.DocumentDB av Microsoft** paketet.</span><span class="sxs-lookup"><span data-stu-id="e0682-157">From hello results, install hello **Microsoft.Azure.DocumentDB by Microsoft** package.</span></span> <span data-ttu-id="e0682-158">Detta kommer att hämta och installera hello Azure Cosmos DB paket samt alla beroenden, till exempel Newtonsoft.Json.</span><span class="sxs-lookup"><span data-stu-id="e0682-158">This will download and install hello Azure Cosmos DB package as well as all dependencies, such as Newtonsoft.Json.</span></span> <span data-ttu-id="e0682-159">Klicka på **OK** i hello **Preview** fönstret och **jag accepterar** i hello **godkännande av licens** fönstret toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="e0682-159">Click **OK** in hello **Preview** window, and **I Accept** in hello **License Acceptance** window toocomplete hello install.</span></span>
   
    ![Skärmdump av hello hantera NuGet-paket fönster med hello Microsoft Azure DocumentDB klientbiblioteket markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      <span data-ttu-id="e0682-161">Du kan också använda hello Package Manager-konsolen tooinstall hello paketet.</span><span class="sxs-lookup"><span data-stu-id="e0682-161">Alternatively you can use hello Package Manager Console tooinstall hello package.</span></span> <span data-ttu-id="e0682-162">toodo i så fall på hello **verktyg** -menyn klickar du på **NuGet Package Manager**, och klicka sedan på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="e0682-162">toodo so, on hello **Tools** menu, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span> <span data-ttu-id="e0682-163">Skriv hello följande i Kommandotolken hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-163">At hello prompt, type hello following.</span></span>
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. <span data-ttu-id="e0682-164">När hello paketet har installerats bör din Visual Studio-lösning likna hello följande med två nya referenser: Microsoft.Azure.Documents.Client och Newtonsoft.Json.</span><span class="sxs-lookup"><span data-stu-id="e0682-164">Once hello package is installed, your Visual Studio solution should resemble hello following with two new references added, Microsoft.Azure.Documents.Client and Newtonsoft.Json.</span></span>
   
    ![Skärmdump av hello två referenser lades till toohello JSON-dataprojektet i Solution Explorer](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <span data-ttu-id="e0682-166"><a name="_Toc395637763"></a>Steg 4: Konfigurera hello ASP.NET MVC-program</span><span class="sxs-lookup"><span data-stu-id="e0682-166"><a name="_Toc395637763"></a>Step 4: Set up hello ASP.NET MVC application</span></span>
<span data-ttu-id="e0682-167">Nu ska vi lägga till hello modeller, vyer och styrenheter toothis MVC-program:</span><span class="sxs-lookup"><span data-stu-id="e0682-167">Now let's add hello models, views, and controllers toothis MVC application:</span></span>

* <span data-ttu-id="e0682-168">[Lägga till en modell](#_Toc395637764).</span><span class="sxs-lookup"><span data-stu-id="e0682-168">[Add a model](#_Toc395637764).</span></span>
* <span data-ttu-id="e0682-169">[Lägga till en styrenhet](#_Toc395637765).</span><span class="sxs-lookup"><span data-stu-id="e0682-169">[Add a controller](#_Toc395637765).</span></span>
* <span data-ttu-id="e0682-170">[Lägga till vyer](#_Toc395637766).</span><span class="sxs-lookup"><span data-stu-id="e0682-170">[Add views](#_Toc395637766).</span></span>

### <span data-ttu-id="e0682-171"><a name="_Toc395637764"></a>Lägg till en JSON-datamodell</span><span class="sxs-lookup"><span data-stu-id="e0682-171"><a name="_Toc395637764"></a>Add a JSON data model</span></span>
<span data-ttu-id="e0682-172">Vi börjar med att skapa hello **M** hello modellen i MVC, dvs.</span><span class="sxs-lookup"><span data-stu-id="e0682-172">Let's begin by creating hello **M** in MVC, hello model.</span></span> 

1. <span data-ttu-id="e0682-173">I **Solution Explorer**, högerklicka på hello **modeller** mappen, klicka på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="e0682-173">In **Solution Explorer**, right-click hello **Models** folder, click **Add**, and then click **Class**.</span></span>
   
      <span data-ttu-id="e0682-174">Hej **Lägg till nytt objekt** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e0682-174">hello **Add New Item** dialog box appears.</span></span>
2. <span data-ttu-id="e0682-175">Namnge den nya klassen **Objekt.cs** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0682-175">Name your new class **Item.cs** and click **Add**.</span></span> 
3. <span data-ttu-id="e0682-176">I den nya **Item.cs** lägger du till följande hello efter hello senast *med hjälp av instruktionen*.</span><span class="sxs-lookup"><span data-stu-id="e0682-176">In this new **Item.cs** file, add hello following after hello last *using statement*.</span></span>
   
        using Newtonsoft.Json;
4. <span data-ttu-id="e0682-177">Ersätt nu den här koden</span><span class="sxs-lookup"><span data-stu-id="e0682-177">Now replace this code</span></span> 
   
        public class Item
        {
        }
   
    <span data-ttu-id="e0682-178">med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-178">with hello following code.</span></span>
   
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
   
    <span data-ttu-id="e0682-179">Alla data i Azure Cosmos DB skickas via kabeln hello och lagras som JSON.</span><span class="sxs-lookup"><span data-stu-id="e0682-179">All data in Azure Cosmos DB is passed over hello wire and stored as JSON.</span></span> <span data-ttu-id="e0682-180">toocontrol hello hur objekten serialiseras/deserialiseras av JSON.NET som du kan använda hello **JsonProperty** attribut som visas i hello **objektet** klass vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="e0682-180">toocontrol hello way your objects are serialized/deserialized by JSON.NET you can use hello **JsonProperty** attribute as demonstrated in hello **Item** class we just created.</span></span> <span data-ttu-id="e0682-181">Du behöver inte **har** toodo detta men jag vill att Mina egenskaper följer hello JSON camelCase namngivningskonventioner tooensure.</span><span class="sxs-lookup"><span data-stu-id="e0682-181">You don't **have** toodo this but I want tooensure that my properties follow hello JSON camelCase naming conventions.</span></span> 
   
    <span data-ttu-id="e0682-182">Inte bara kan du styra hello format hello egenskapsnamn när den går till JSON, men du kan helt byta namn på dina .NET-egenskaper som jag gjorde med hello **beskrivning** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="e0682-182">Not only can you control hello format of hello property name when it goes into JSON, but you can entirely rename your .NET properties like I did with hello **Description** property.</span></span> 

### <span data-ttu-id="e0682-183"><a name="_Toc395637765"></a>Lägg till en controller</span><span class="sxs-lookup"><span data-stu-id="e0682-183"><a name="_Toc395637765"></a>Add a controller</span></span>
<span data-ttu-id="e0682-184">Som tar hand om hello **M**, nu skapar vi hello **C** i MVC, dvs. en styrenhetsklass.</span><span class="sxs-lookup"><span data-stu-id="e0682-184">That takes care of hello **M**, now let's create hello **C** in MVC, a controller class.</span></span>

1. <span data-ttu-id="e0682-185">I **Solution Explorer**, högerklicka på hello **domänkontrollanter** mapp, klickar du på **Lägg till**, och klicka sedan på **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="e0682-185">In **Solution Explorer**, right-click hello **Controllers** folder, click **Add**, and then click **Controller**.</span></span>
   
    <span data-ttu-id="e0682-186">Hej **Lägg till Kodskelett** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e0682-186">hello **Add Scaffold** dialog box appears.</span></span>
2. <span data-ttu-id="e0682-187">Välj **MVC 5 styrenhet – tom** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0682-187">Select **MVC 5 Controller - Empty** and then click **Add**.</span></span>
   
    ![Skärmbild som visar hello dialogruta för Lägg till Kodskelett med hello MVC 5 styrenhet – tom markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. <span data-ttu-id="e0682-189">Ge din nya styrenhet namnet **Objektstyrning.**</span><span class="sxs-lookup"><span data-stu-id="e0682-189">Name your new Controller, **ItemController.**</span></span>
   
    ![Skärmbild av dialogrutan Lägg till styrenhet för hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    <span data-ttu-id="e0682-191">När hello fil skapas, Visual Studio-lösning bör likna följande hello med hello nya ItemController.cs-filen i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e0682-191">Once hello file is created, your Visual Studio solution should resemble hello following with hello new ItemController.cs file in **Solution Explorer**.</span></span> <span data-ttu-id="e0682-192">hello nya Item.cs-filen skapades tidigare visas också.</span><span class="sxs-lookup"><span data-stu-id="e0682-192">hello new Item.cs file created earlier is also shown.</span></span>
   
    ![Skärmbild som visar hello Visual Studio - lösningen i Solution Explorer med hello nya ItemController.cs-filen och Item.cs-filen markerade](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    <span data-ttu-id="e0682-194">Du kan stänga ItemController.cs, vi återkommer tooit senare.</span><span class="sxs-lookup"><span data-stu-id="e0682-194">You can close ItemController.cs, we'll come back tooit later.</span></span> 

### <span data-ttu-id="e0682-195"><a name="_Toc395637766"></a>Lägg till vyer</span><span class="sxs-lookup"><span data-stu-id="e0682-195"><a name="_Toc395637766"></a>Add views</span></span>
<span data-ttu-id="e0682-196">Nu skapar vi hello **V** i MVC, dvs hello vyer:</span><span class="sxs-lookup"><span data-stu-id="e0682-196">Now, let's create hello **V** in MVC, hello views:</span></span>

* <span data-ttu-id="e0682-197">[Lägga till Vyn Objektindex](#AddItemIndexView).</span><span class="sxs-lookup"><span data-stu-id="e0682-197">[Add an Item Index view](#AddItemIndexView).</span></span>
* <span data-ttu-id="e0682-198">[Lägga till vyn Nytt objekt](#AddNewIndexView).</span><span class="sxs-lookup"><span data-stu-id="e0682-198">[Add a New Item view](#AddNewIndexView).</span></span>
* <span data-ttu-id="e0682-199">[Lägga till vyn Redigera objekt](#_Toc395888515).</span><span class="sxs-lookup"><span data-stu-id="e0682-199">[Add an Edit Item view](#_Toc395888515).</span></span>

#### <span data-ttu-id="e0682-200"><a name="AddItemIndexView"></a>Lägg till en objektindexvy</span><span class="sxs-lookup"><span data-stu-id="e0682-200"><a name="AddItemIndexView"></a>Add an Item Index view</span></span>
1. <span data-ttu-id="e0682-201">I **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på hello tom **objektet** mapp som Visual Studio skapade när du har lagt till hello  **ItemController** tidigare, klicka på **Lägg till**, och klicka sedan på **visa**.</span><span class="sxs-lookup"><span data-stu-id="e0682-201">In **Solution Explorer**, expand hello **Views**  folder, right-click hello empty **Item** folder that Visual Studio created for you when you added hello **ItemController** earlier, click **Add**, and then click **View**.</span></span>
   
    ![Skärmdump av Solution Explorer visar hello objektmapp som Visual Studio skapade med hello Lägg till vy markerat](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. <span data-ttu-id="e0682-203">I hello **Lägg till vy** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-203">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="e0682-204">I hello **vynamn** skriver ***Index***.</span><span class="sxs-lookup"><span data-stu-id="e0682-204">In hello **View name** box, type ***Index***.</span></span>
   * <span data-ttu-id="e0682-205">I hello **mallen** väljer ***listan***.</span><span class="sxs-lookup"><span data-stu-id="e0682-205">In hello **Template** box, select ***List***.</span></span>
   * <span data-ttu-id="e0682-206">I hello **Modellklass** väljer ***objekt (todo. Modeller)***.</span><span class="sxs-lookup"><span data-stu-id="e0682-206">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="e0682-207">Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="e0682-207">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
     
   ![Skärmbild som visar hello Lägg till vy dialogrutan](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. <span data-ttu-id="e0682-209">När du har angett samtliga dessa värden klickar du på **Lägg till**  och låter Visual Studio skapa en ny mallvy.</span><span class="sxs-lookup"><span data-stu-id="e0682-209">Once all these values are set, click **Add** and let Visual Studio create a new template view.</span></span> <span data-ttu-id="e0682-210">När det är klart öppnas hello cshtml-filen som skapades.</span><span class="sxs-lookup"><span data-stu-id="e0682-210">Once it is done, it will open hello cshtml file  that was created.</span></span> <span data-ttu-id="e0682-211">Vi kan stänga filen i Visual Studio eftersom vi återkommer tooit senare.</span><span class="sxs-lookup"><span data-stu-id="e0682-211">We can close that file in Visual Studio as we will come back tooit later.</span></span>

#### <span data-ttu-id="e0682-212"><a name="AddNewIndexView"></a>Lägg till en nytt objektsvy</span><span class="sxs-lookup"><span data-stu-id="e0682-212"><a name="AddNewIndexView"></a>Add a New Item view</span></span>
<span data-ttu-id="e0682-213">Liknande toohow som vi har skapat en **Objektindex** vyn nu ska vi skapa en ny vy för att skapa nya **objekt**.</span><span class="sxs-lookup"><span data-stu-id="e0682-213">Similar toohow we created an **Item Index** view, we will now create a new view for creating new **Items**.</span></span>

1. <span data-ttu-id="e0682-214">I **Solution Explorer**, högerklicka på hello **objektet** mappen igen och klicka på **Lägg till**, och klicka sedan på **visa**.</span><span class="sxs-lookup"><span data-stu-id="e0682-214">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="e0682-215">I hello **Lägg till vy** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-215">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="e0682-216">I hello **vynamn** skriver ***skapa***.</span><span class="sxs-lookup"><span data-stu-id="e0682-216">In hello **View name** box, type ***Create***.</span></span>
   * <span data-ttu-id="e0682-217">I hello **mallen** väljer ***skapa***.</span><span class="sxs-lookup"><span data-stu-id="e0682-217">In hello **Template** box, select ***Create***.</span></span>
   * <span data-ttu-id="e0682-218">I hello **Modellklass** väljer ***objekt (todo. Modeller)***.</span><span class="sxs-lookup"><span data-stu-id="e0682-218">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="e0682-219">Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="e0682-219">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="e0682-220">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0682-220">Click **Add**.</span></span>
   
#### <span data-ttu-id="e0682-221"><a name="_Toc395888515"></a>Lägg till en redigera objektsvy</span><span class="sxs-lookup"><span data-stu-id="e0682-221"><a name="_Toc395888515"></a>Add an Edit Item view</span></span>
<span data-ttu-id="e0682-222">Lägg slutligen till en vy för redigering av ett **objektet** i hello samma sätt som tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0682-222">And finally, add one last view for editing an **Item** in hello same way as before.</span></span>

1. <span data-ttu-id="e0682-223">I **Solution Explorer**, högerklicka på hello **objektet** mappen igen och klicka på **Lägg till**, och klicka sedan på **visa**.</span><span class="sxs-lookup"><span data-stu-id="e0682-223">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="e0682-224">I hello **Lägg till vy** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-224">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="e0682-225">I hello **vynamn** skriver ***redigera***.</span><span class="sxs-lookup"><span data-stu-id="e0682-225">In hello **View name** box, type ***Edit***.</span></span>
   * <span data-ttu-id="e0682-226">I hello **mallen** väljer ***redigera***.</span><span class="sxs-lookup"><span data-stu-id="e0682-226">In hello **Template** box, select ***Edit***.</span></span>
   * <span data-ttu-id="e0682-227">I hello **Modellklass** väljer ***objekt (todo. Modeller)***.</span><span class="sxs-lookup"><span data-stu-id="e0682-227">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="e0682-228">Skriv i hello layout sidan ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="e0682-228">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="e0682-229">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0682-229">Click **Add**.</span></span>

<span data-ttu-id="e0682-230">När det är klart stänger du alla hello cshtml-dokument i Visual Studio eftersom vi återkommer toothese vyer senare.</span><span class="sxs-lookup"><span data-stu-id="e0682-230">Once this is done, close all hello cshtml documents in Visual Studio as we will return toothese views later.</span></span>

## <span data-ttu-id="e0682-231"><a name="_Toc395637769"></a>Steg 5: Lägga till kod för Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e0682-231"><a name="_Toc395637769"></a>Step 5: Wiring up Azure Cosmos DB</span></span>
<span data-ttu-id="e0682-232">Nu när du har åtgärdat hello standard MVC är klara, dags tooadding hello koden för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e0682-232">Now that hello standard MVC stuff is taken care of, let's turn tooadding hello code for Azure Cosmos DB.</span></span> 

<span data-ttu-id="e0682-233">I det här avsnittet vi lägger till kod toohandle hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-233">In this section, we'll add code toohandle hello following:</span></span>

* <span data-ttu-id="e0682-234">[Lista ofullständiga objekt](#_Toc395637770).</span><span class="sxs-lookup"><span data-stu-id="e0682-234">[Listing incomplete Items](#_Toc395637770).</span></span>
* <span data-ttu-id="e0682-235">[Lägga till objekt](#_Toc395637771).</span><span class="sxs-lookup"><span data-stu-id="e0682-235">[Adding Items](#_Toc395637771).</span></span>
* <span data-ttu-id="e0682-236">[Redigera objekt](#_Toc395637772).</span><span class="sxs-lookup"><span data-stu-id="e0682-236">[Editing Items](#_Toc395637772).</span></span>

### <span data-ttu-id="e0682-237"><a name="_Toc395637770"></a>Lista ofullständiga objekt i ditt MVC-webbprogram</span><span class="sxs-lookup"><span data-stu-id="e0682-237"><a name="_Toc395637770"></a>Listing incomplete Items in your MVC web application</span></span>
<span data-ttu-id="e0682-238">hello första sak toodo här är lägga till en klass som innehåller alla hello logik tooconnect tooand Använd Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e0682-238">hello first thing toodo here is add a class that contains all hello logic tooconnect tooand use Azure Cosmos DB.</span></span> <span data-ttu-id="e0682-239">Den här självstudiekursen kapslar vi in all denna logik i tooa centrallagerklass kallad DocumentDBRepository.</span><span class="sxs-lookup"><span data-stu-id="e0682-239">For this tutorial we'll encapsulate all this logic in tooa repository class called DocumentDBRepository.</span></span> 

1. <span data-ttu-id="e0682-240">I **Solution Explorer**, högerklicka på hello projekt, klickar du på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="e0682-240">In **Solution Explorer**, right-click on hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="e0682-241">Namnge hello nya klassen **DocumentDBRepository** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0682-241">Name hello new class **DocumentDBRepository** and click **Add**.</span></span>
2. <span data-ttu-id="e0682-242">I hello nyskapad **DocumentDBRepository** klassen och Lägg till följande hello *using-instruktioner* ovan hello *namnområde* förklaring</span><span class="sxs-lookup"><span data-stu-id="e0682-242">In hello newly created **DocumentDBRepository** class and add hello following *using statements* above hello *namespace* declaration</span></span>
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    <span data-ttu-id="e0682-243">Ersätt nu den här koden</span><span class="sxs-lookup"><span data-stu-id="e0682-243">Now replace this code</span></span> 
   
        public class DocumentDBRepository
        {
        }
   
    <span data-ttu-id="e0682-244">med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-244">with hello following code.</span></span>
   
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
   
    
3. <span data-ttu-id="e0682-245">Vi läser in vissa värden från konfigurationen, så att öppna hello **Web.config** för appen och Lägg till följande rader under hello hello `<AppSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e0682-245">We're reading some values from configuration, so open hello **Web.config** file of your application and add hello following lines under hello `<AppSettings>` section.</span></span>
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. <span data-ttu-id="e0682-246">Uppdatera nu hello värden för *endpoint* och *auktoriseringsnyckel* med hello nycklar bladet för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0682-246">Now, update hello values for *endpoint* and *authKey* using hello Keys blade of hello Azure Portal.</span></span> <span data-ttu-id="e0682-247">Använd hello **URI** från bladet för hello nycklar som hello värdet för inställningen för hello slutpunkt och Använd hello **PRIMÄRNYCKEL**, eller **SEKUNDÄRNYCKELN** från bladet för hello nycklar som hello värdet för hello inställningen auktoriseringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="e0682-247">Use hello **URI** from hello Keys blade as hello value of hello endpoint setting, and use hello **PRIMARY KEY**, or **SECONDARY KEY** from hello Keys blade as hello value of hello authKey setting.</span></span>

    <span data-ttu-id="e0682-248">Tar hand om att koppla samman hello Azure Cosmos DB databasen nu ska vi lägga till vår applogik.</span><span class="sxs-lookup"><span data-stu-id="e0682-248">That takes care of wiring up hello Azure Cosmos DB repository, now let's add our application logic.</span></span>

1. <span data-ttu-id="e0682-249">hello vill först öppna vi toobe kan toodo med en todo-App är toodisplay hello ofullständiga objekt.</span><span class="sxs-lookup"><span data-stu-id="e0682-249">hello first thing we want toobe able toodo with a todo list application is toodisplay hello incomplete items.</span></span>  <span data-ttu-id="e0682-250">Kopiera och klistra in följande kodutdrag var som helst i hello hello **DocumentDBRepository** klass.</span><span class="sxs-lookup"><span data-stu-id="e0682-250">Copy and paste hello following code snippet anywhere within hello **DocumentDBRepository** class.</span></span>
   
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
2. <span data-ttu-id="e0682-251">Öppna hello **ItemController** vi lade till tidigare och Lägg till följande hello *using-instruktioner* ovan hello namnområdesdeklaration.</span><span class="sxs-lookup"><span data-stu-id="e0682-251">Open hello **ItemController** we added earlier and add hello following *using statements* above hello namespace declaration.</span></span>
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    <span data-ttu-id="e0682-252">Om ditt projekt inte är med namnet ”todo” måste tooupdate med ”todo. Modeller ”. tooreflect hello namnet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="e0682-252">If your project is not named "todo", then you need tooupdate using "todo.Models"; tooreflect hello name of your project.</span></span>
   
    <span data-ttu-id="e0682-253">Ersätt nu den här koden</span><span class="sxs-lookup"><span data-stu-id="e0682-253">Now replace this code</span></span>
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    <span data-ttu-id="e0682-254">med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-254">with hello following code.</span></span>
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. <span data-ttu-id="e0682-255">Öppna **Global.asax.cs** och Lägg till följande rad toohello hello **Application_Start** metod</span><span class="sxs-lookup"><span data-stu-id="e0682-255">Open **Global.asax.cs** and add hello following line toohello **Application_Start** method</span></span> 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

<span data-ttu-id="e0682-256">Din lösning ska nu kunna toobuild utan fel.</span><span class="sxs-lookup"><span data-stu-id="e0682-256">At this point your solution should be able toobuild without any errors.</span></span>

<span data-ttu-id="e0682-257">Om du körde hello programmet nu hamnar toohello **HomeController** och hello **Index** från den styrenheten.</span><span class="sxs-lookup"><span data-stu-id="e0682-257">If you ran hello application now, you would go toohello **HomeController** and hello **Index** view of that controller.</span></span> <span data-ttu-id="e0682-258">Detta är hello standardbeteendet för hello MVC-mallprojektet vi valde hello början men som bör inte!</span><span class="sxs-lookup"><span data-stu-id="e0682-258">This is hello default behavior for hello MVC template project we chose at hello start but we don't want that!</span></span> <span data-ttu-id="e0682-259">Nu ska vi ändra hello routning på den här MVC-program tooalter problemet.</span><span class="sxs-lookup"><span data-stu-id="e0682-259">Let's change hello routing on this MVC application tooalter this behavior.</span></span>

<span data-ttu-id="e0682-260">Öppna ***App\_Start\RouteConfig.cs*** och leta upp hello rad som börjar med ”standardvärden”: och ändra den tooresemble hello följande.</span><span class="sxs-lookup"><span data-stu-id="e0682-260">Open ***App\_Start\RouteConfig.cs*** and locate hello line starting with "defaults:" and change it tooresemble hello following.</span></span>

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

<span data-ttu-id="e0682-261">Det här nu berättar för ASP.NET MVC som om du inte har angett ett värde i hello URL toocontrol hello som i stället för routningsbeteendet **Start**, använda **objektet** som hello styrenhet och användarens **Index** som hello vy.</span><span class="sxs-lookup"><span data-stu-id="e0682-261">This now tells ASP.NET MVC that if you have not specified a value in hello URL toocontrol hello routing behavior that instead of **Home**, use **Item** as hello controller and user **Index** as hello view.</span></span>

<span data-ttu-id="e0682-262">Nu om du kör programmet hello anropar din **ItemController** som anropar i toohello centrallagerklassen och använder hello GetItems metoden tooreturn alla hello ofullständiga objekt toohello **vyer** \\ **Objektet**\\**Index** vyn.</span><span class="sxs-lookup"><span data-stu-id="e0682-262">Now if you run hello application, it will call into your **ItemController** which will call in toohello repository class and use hello GetItems method tooreturn all hello incomplete items toohello **Views**\\**Item**\\**Index** view.</span></span> 

<span data-ttu-id="e0682-263">Om du bygger och kör det här projektet nu bör det se ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="e0682-263">If you build and run this project now, you should now see something that looks this.</span></span>    

![Skärmbild som visar hello listan webbapp skapats i denna självstudie](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <span data-ttu-id="e0682-265"><a name="_Toc395637771"></a>Lägg till objekt</span><span class="sxs-lookup"><span data-stu-id="e0682-265"><a name="_Toc395637771"></a>Adding Items</span></span>
<span data-ttu-id="e0682-266">Vi lägger in några objekt i vår databas så att vi har något mer än ett tomt rutnät toolook på.</span><span class="sxs-lookup"><span data-stu-id="e0682-266">Let's put some items into our database so we have something more than an empty grid toolook at.</span></span>

<span data-ttu-id="e0682-267">Lägg till kod för Azure Cosmos DBRepository och ItemController toopersist hello post i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e0682-267">Let's add some code too Azure Cosmos DBRepository and ItemController toopersist hello record in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="e0682-268">Lägg till följande metod tooyour hello **DocumentDBRepository** klass.</span><span class="sxs-lookup"><span data-stu-id="e0682-268">Add hello following method tooyour **DocumentDBRepository** class.</span></span>
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   <span data-ttu-id="e0682-269">Den här metoden tar ett objekt som överförs tooit bara och sparar det i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e0682-269">This method simply takes an object passed tooit and persists it in Azure Cosmos DB.</span></span>
2. <span data-ttu-id="e0682-270">Öppna hello ItemController.cs-filen och Lägg till följande kodutdrag i klassen hello hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-270">Open hello ItemController.cs file and add hello following code snippet within hello class.</span></span> <span data-ttu-id="e0682-271">Detta är så ASP.NET MVC vet vilken toodo för hello **skapa** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e0682-271">This is how ASP.NET MVC knows what toodo for hello **Create** action.</span></span> <span data-ttu-id="e0682-272">I det här fallet associerade bara render hello Create.cshtml-vyn som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0682-272">In this case just render hello associated Create.cshtml view created earlier.</span></span>
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    <span data-ttu-id="e0682-273">Vi behöver nu lite mer kod i styrenheten som godtar inmatningen hello från hello **skapa** vyn.</span><span class="sxs-lookup"><span data-stu-id="e0682-273">We now need some more code in this controller that will accept hello submission from hello **Create** view.</span></span>
3. <span data-ttu-id="e0682-274">Lägg till hello nästa block med koden toohello ItemController.cs-klass som berättar för ASP.NET MVC vad toodo med formuläret POST för den här domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="e0682-274">Add hello next block of code toohello ItemController.cs class that tells ASP.NET MVC what toodo with a form POST for this controller.</span></span>
   
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
   
    <span data-ttu-id="e0682-275">Den här koden anropar i toohello DocumentDBRepository och använder hello CreateItemAsync metoden toopersist hello nya todo objektet toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="e0682-275">This code calls in toohello DocumentDBRepository and uses hello CreateItemAsync method toopersist hello new todo item toohello database.</span></span> 
   
    <span data-ttu-id="e0682-276">**Säkerhetsmeddelande**: hello **ValidateAntiForgeryToken** attributet används här toohelp skydda appen mot attacker med förfalskning av begäran.</span><span class="sxs-lookup"><span data-stu-id="e0682-276">**Security Note**: hello **ValidateAntiForgeryToken** attribute is used here toohelp protect this application against cross-site request forgery attacks.</span></span> <span data-ttu-id="e0682-277">Det finns flera tooit bara att lägga till det här attributet, dina vyer behöver toowork med denna antiförfalskningstoken samt.</span><span class="sxs-lookup"><span data-stu-id="e0682-277">There is more tooit than just adding this attribute, your views need toowork with this anti-forgery token as well.</span></span> <span data-ttu-id="e0682-278">Mer information om hello ämnet och exempel på hur tooimplement detta korrekt finns [hindrar webbplatser förfalskning av begäran mellan][Preventing Cross-Site Request Forgery].</span><span class="sxs-lookup"><span data-stu-id="e0682-278">For more on hello subject, and examples of how tooimplement this correctly, please see [Preventing Cross-Site Request Forgery][Preventing Cross-Site Request Forgery].</span></span> <span data-ttu-id="e0682-279">Hej källkoden på [GitHub] [ GitHub] har hello fullständigt genomförande.</span><span class="sxs-lookup"><span data-stu-id="e0682-279">hello source code provided on [GitHub][GitHub] has hello full implementation in place.</span></span>
   
    <span data-ttu-id="e0682-280">**Säkerhetsmeddelande**: Vi använder också hello **binda** attributet på hello metoden parametern toohelp skydda mot overpostingattacker.</span><span class="sxs-lookup"><span data-stu-id="e0682-280">**Security Note**: We also use hello **Bind** attribute on hello method parameter toohelp protect against over-posting attacks.</span></span> <span data-ttu-id="e0682-281">Mer information finns i [Grundläggande CRUD-åtgärder i ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span><span class="sxs-lookup"><span data-stu-id="e0682-281">For more details please see [Basic CRUD Operations in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span></span>

<span data-ttu-id="e0682-282">Detta avslutar hello kod krävs tooadd nya objekt tooour databasen.</span><span class="sxs-lookup"><span data-stu-id="e0682-282">This concludes hello code required tooadd new Items tooour database.</span></span>

### <span data-ttu-id="e0682-283"><a name="_Toc395637772"></a>Redigera objekt</span><span class="sxs-lookup"><span data-stu-id="e0682-283"><a name="_Toc395637772"></a>Editing Items</span></span>
<span data-ttu-id="e0682-284">Det finns en sista åtgärd vi toodo och som är tooadd hello möjlighet tooedit **objekt** i hello-databasen och toomark dem som Slutför.</span><span class="sxs-lookup"><span data-stu-id="e0682-284">There is one last thing for us toodo, and that is tooadd hello ability tooedit **Items** in hello database and toomark them as complete.</span></span> <span data-ttu-id="e0682-285">hello redigeringsvyn har redan lagts till toohello projekt, så vi behöver bara tooadd vissa code tooour domänkontrollant och toohello **DocumentDBRepository** klassen igen.</span><span class="sxs-lookup"><span data-stu-id="e0682-285">hello view for editing was already added toohello project, so we just need tooadd some code tooour controller and toohello **DocumentDBRepository** class again.</span></span>

1. <span data-ttu-id="e0682-286">Lägg till följande toohello hello **DocumentDBRepository** klass.</span><span class="sxs-lookup"><span data-stu-id="e0682-286">Add hello following toohello **DocumentDBRepository** class.</span></span>
   
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
   
    <span data-ttu-id="e0682-287">Hej första av dessa metoder **GetItem** hämtar ett objekt från Azure Cosmos-databasen som skickas tillbaka toohello **ItemController** och klicka sedan på toohello **redigera** vyn.</span><span class="sxs-lookup"><span data-stu-id="e0682-287">hello first of these methods, **GetItem** fetches an Item from Azure Cosmos DB which is passed back toohello **ItemController** and then on toohello **Edit** view.</span></span>
   
    <span data-ttu-id="e0682-288">Hej sekund av hello metoder vi just lade till ersätter hello **dokument** i Azure Cosmos-databasen med hello version av hello **dokument** som överförts från hello **ItemController**.</span><span class="sxs-lookup"><span data-stu-id="e0682-288">hello second of hello methods we just added replaces hello **Document** in Azure Cosmos DB with hello version of hello **Document** passed in from hello **ItemController**.</span></span>
2. <span data-ttu-id="e0682-289">Lägg till följande toohello hello **ItemController** klass.</span><span class="sxs-lookup"><span data-stu-id="e0682-289">Add hello following toohello **ItemController** class.</span></span>
   
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
   
    <span data-ttu-id="e0682-290">hello första metoden hanterar hello Http GET som inträffar när hello användare klickar på hello **redigera** länk från hello **Index** vyn.</span><span class="sxs-lookup"><span data-stu-id="e0682-290">hello first method handles hello Http GET that happens when hello user clicks on hello **Edit** link from hello **Index** view.</span></span> <span data-ttu-id="e0682-291">Den här metoden hämtar en [ **dokument** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) från Azure Cosmos-databasen och skickar den toohello **redigera** vyn.</span><span class="sxs-lookup"><span data-stu-id="e0682-291">This method fetches a [**Document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) from Azure Cosmos DB and passes it toohello **Edit** view.</span></span>
   
    <span data-ttu-id="e0682-292">Hej **redigera** gör sedan en Http POST-toohello **IndexController**.</span><span class="sxs-lookup"><span data-stu-id="e0682-292">hello **Edit** view will then do an Http POST toohello **IndexController**.</span></span> 
   
    <span data-ttu-id="e0682-293">hello andra metoden som vi har lagt till hanterar skicka hello uppdaterade objekt tooAzure Cosmos DB toobe beständig i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e0682-293">hello second method we added handles passing hello updated object tooAzure Cosmos DB toobe persisted in hello database.</span></span>

<span data-ttu-id="e0682-294">Som är den som är allt som behövs toorun vårt program, lista ofullständiga **objekt**, lägga till nya **objekt**, och redigera **objekt**.</span><span class="sxs-lookup"><span data-stu-id="e0682-294">That's it, that is everything we need toorun our application, list incomplete **Items**, add new **Items**, and edit **Items**.</span></span>

## <span data-ttu-id="e0682-295"><a name="_Toc395637773"></a>Steg 6: Kör programmet hello lokalt</span><span class="sxs-lookup"><span data-stu-id="e0682-295"><a name="_Toc395637773"></a>Step 6: Run hello application locally</span></span>
<span data-ttu-id="e0682-296">tootest hello program på den lokala datorn hello följande:</span><span class="sxs-lookup"><span data-stu-id="e0682-296">tootest hello application on your local machine, do hello following:</span></span>

1. <span data-ttu-id="e0682-297">Tryck på F5 i Visual Studio toobuild hello programmet i felsökningsläge.</span><span class="sxs-lookup"><span data-stu-id="e0682-297">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span> <span data-ttu-id="e0682-298">Den ska bygga hello program och starta en webbläsare med hello tomma rutnätssidan vi såg tidigare:</span><span class="sxs-lookup"><span data-stu-id="e0682-298">It should build hello application and launch a browser with hello empty grid page we saw before:</span></span>
   
    ![Skärmbild som visar hello listan webbapp skapats i denna självstudie](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. <span data-ttu-id="e0682-300">Klicka på hello **Skapa nytt** länkar och Lägg till värden toohello **namn** och **beskrivning** fält.</span><span class="sxs-lookup"><span data-stu-id="e0682-300">Click hello **Create New** link and add values toohello **Name** and **Description** fields.</span></span> <span data-ttu-id="e0682-301">Lämna hello **slutförd** Markera inte kryssrutan annars hello nya **objektet** kommer att läggas till i slutfört tillstånd och visas inte på hello ursprungliga listan.</span><span class="sxs-lookup"><span data-stu-id="e0682-301">Leave hello **Completed** check box unselected otherwise hello new **Item** will be added in a completed state and will not appear on hello initial list.</span></span>
   
    ![Skärmbild som visar hello skapa vy](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. <span data-ttu-id="e0682-303">Klicka på **skapa** och du är omdirigerade tillbaka toohello **Index** vyn och dina **objektet** visas i listan hello.</span><span class="sxs-lookup"><span data-stu-id="e0682-303">Click **Create** and you are redirected back toohello **Index** view and your **Item** appears in hello list.</span></span>
   
    ![Skärmbild som visar hello indexvy](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    <span data-ttu-id="e0682-305">Känna sig fria tooadd några fler **objekt** tooyour todo-listan.</span><span class="sxs-lookup"><span data-stu-id="e0682-305">Feel free tooadd a few more **Items** tooyour todo list.</span></span>
    
4. <span data-ttu-id="e0682-306">Klicka på **redigera** nästa tooan **objektet** på hello lista och du kommer toohello **redigera** vy där du kan uppdatera alla egenskaper för objektet, inklusive hello  **Slutföra** flaggan.</span><span class="sxs-lookup"><span data-stu-id="e0682-306">Click **Edit** next tooan **Item** on hello list and you are taken toohello **Edit** view where you can update any property of your object, including hello **Completed** flag.</span></span> <span data-ttu-id="e0682-307">Om du markerar hello **Slutför** flagga och på **spara**, hello **objektet** tas bort från hello listan över ofullständiga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e0682-307">If you mark hello **Complete** flag and click **Save**, hello **Item** is removed from hello list of incomplete tasks.</span></span>
   
    ![Skärmbild som visar hello indexvyn med hello slutförd ikryssad](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. <span data-ttu-id="e0682-309">Tryck på Ctrl + F5 toostop felsökning hello app när du har testat hello app.</span><span class="sxs-lookup"><span data-stu-id="e0682-309">Once you've tested hello app, press Ctrl+F5 toostop debugging hello app.</span></span> <span data-ttu-id="e0682-310">Du är klar toodeploy!</span><span class="sxs-lookup"><span data-stu-id="e0682-310">You're ready toodeploy!</span></span>

## <span data-ttu-id="e0682-311"><a name="_Toc395637774"></a>Steg 7: Distribuera hello programmet tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="e0682-311"><a name="_Toc395637774"></a>Step 7: Deploy hello application tooAzure App Service</span></span> 
<span data-ttu-id="e0682-312">Nu när du har hello hela appen vi fungerar med Azure Cosmos DB toodeploy denna web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e0682-312">Now that you have hello complete application working correctly with Azure Cosmos DB we're going toodeploy this web app tooAzure App Service.</span></span>  

1. <span data-ttu-id="e0682-313">toopublish alla programmet behöver du toodo är Högerklicka på hello-projekt i **Solution Explorer** och på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="e0682-313">toopublish this application all you need toodo is right-click on hello project in **Solution Explorer** and click **Publish**.</span></span>
   
    ![Skärmbild som visar hello alternativet Publicera i Solution Explorer](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. <span data-ttu-id="e0682-315">I hello **publicera** dialogrutan klickar du på **Microsoft Azure App Service**och välj **Skapa nytt** toocreate en Apptjänst-profil, eller klicka på **Välj Befintliga** toouse en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="e0682-315">In hello **Publish** dialog box, click **Microsoft Azure App Service**, then select **Create New** toocreate an App Service profile, or click **Select Existing** toouse an existing profile.</span></span>

    ![Dialogrutan Publicera i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. <span data-ttu-id="e0682-317">Om du har en befintlig Azure App Service-profil anger du namnet på din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e0682-317">If you have an existing Azure App Service profile, enter your subscription name.</span></span> <span data-ttu-id="e0682-318">Använd hello **visa** filtrera toosort av resursgruppen eller resursen och sedan välja Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e0682-318">Use hello **View** filter toosort by resource group or resource type, then select your Azure App Service.</span></span> 
   
    ![Dialogrutan för Apptjänst i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. <span data-ttu-id="e0682-320">toocreate en ny Azure App Service-profil klickar du på **Skapa nytt** i hello **publicera** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0682-320">toocreate a new Azure App Service profile, click **Create New** in hello **Publish** dialog box.</span></span> <span data-ttu-id="e0682-321">I hello **skapa Apptjänst** dialogrutan, ange ditt webbprogram namn och lämpliga prenumerationen, resursgruppen och App Service-plan och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0682-321">In hello **Create App Service** dialog, enter your Web App name and appropriate subscription, resource group, and App Service plan, then click **Create**.</span></span>

    ![Skapa en dialogruta för Apptjänst i Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

<span data-ttu-id="e0682-323">I några sekunder har Visual Studio publicerat din webbapp och öppnar en webbläsare där du kan se ditt arbete som körs i Azure!</span><span class="sxs-lookup"><span data-stu-id="e0682-323">In a few seconds, Visual Studio will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>



## <span data-ttu-id="e0682-324"><a name="_Toc395637775"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0682-324"><a name="_Toc395637775"></a>Next steps</span></span>
<span data-ttu-id="e0682-325">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e0682-325">Congratulations!</span></span> <span data-ttu-id="e0682-326">Du precis skapat din första ASP.NET MVC-webbapp med Azure Cosmos DB och publicerat den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e0682-326">You just built your first ASP.NET MVC web application using Azure Cosmos DB and published it tooAzure.</span></span> <span data-ttu-id="e0682-327">Hej källkoden för hello hela appen, inklusive hello detaljer och ta bort funktioner som inte fanns med i den här självstudiekursen kan hämtas eller klonas från [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="e0682-327">hello source code for hello complete application, including hello detail and delete functionality that were not included in this tutorial can be downloaded or cloned from [GitHub][GitHub].</span></span> <span data-ttu-id="e0682-328">Så om du är intresserad av att lägga till tooyour appen hämtar hello koden och Lägg den toothis app.</span><span class="sxs-lookup"><span data-stu-id="e0682-328">So if you're interested in adding that tooyour app, grab hello code and add it toothis app.</span></span>

<span data-ttu-id="e0682-329">tooadd ytterligare funktioner tooyour program, granska hello API: er som är tillgängliga i hello [Azure Cosmos DB .NET-biblioteket](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) och känna sig fria toocontribute toohello Azure Cosmos DB .NET-biblioteket på [GitHub] [GitHub].</span><span class="sxs-lookup"><span data-stu-id="e0682-329">tooadd additional functionality tooyour application, review hello APIs available in hello [Azure Cosmos DB .NET Library](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) and feel free toocontribute toohello Azure Cosmos DB .NET Library on [GitHub][GitHub].</span></span> 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
