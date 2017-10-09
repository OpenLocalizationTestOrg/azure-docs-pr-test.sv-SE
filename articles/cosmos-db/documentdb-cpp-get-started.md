---
title: "aaaC ++ självstudiekurs för Azure Cosmos DB | Microsoft Docs"
description: "En C++-självstudiekurs som beskriver hur du skapar en C++-databas och ett C++-konsolprogram med ett Azure Cosmos DB SDK för C++. Azure Cosmos DB är en databastjänst med global skalningskapacitet."
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: 2d5eeff349b7753e39936b7eb77557ad30c5830a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a><span data-ttu-id="afcde-104">Azure Cosmos DB: C++ konsolen självstudien för hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="afcde-104">Azure Cosmos DB: C++ console application tutorial for hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="afcde-105">.NET</span><span class="sxs-lookup"><span data-stu-id="afcde-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="afcde-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="afcde-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="afcde-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="afcde-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="afcde-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="afcde-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="afcde-109">Java</span><span class="sxs-lookup"><span data-stu-id="afcde-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="afcde-110">C++</span><span class="sxs-lookup"><span data-stu-id="afcde-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="afcde-111">Välkommen till toohello C++ självstudier för hello Azure Cosmos DB DocumentDB API godkända SDK för C++!</span><span class="sxs-lookup"><span data-stu-id="afcde-111">Welcome toohello C++ tutorial for hello Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="afcde-112">När du har slutfört den här självstudien har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser, inklusive en C++-databas.</span><span class="sxs-lookup"><span data-stu-id="afcde-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="afcde-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="afcde-113">We'll cover:</span></span>

* <span data-ttu-id="afcde-114">Skapa och ansluta tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="afcde-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="afcde-115">Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="afcde-115">Setting up your application</span></span>
* <span data-ttu-id="afcde-116">Skapa en C++ Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="afcde-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="afcde-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="afcde-117">Creating a collection</span></span>
* <span data-ttu-id="afcde-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-118">Creating JSON documents</span></span>
* <span data-ttu-id="afcde-119">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="afcde-119">Querying hello collection</span></span>
* <span data-ttu-id="afcde-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-120">Replacing a document</span></span>
* <span data-ttu-id="afcde-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-121">Deleting a document</span></span>
* <span data-ttu-id="afcde-122">Tar bort hello C++ Azure Cosmos DB-databasen</span><span class="sxs-lookup"><span data-stu-id="afcde-122">Deleting hello C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="afcde-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="afcde-123">Don't have time?</span></span> <span data-ttu-id="afcde-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="afcde-124">Don't worry!</span></span> <span data-ttu-id="afcde-125">hello kompletta lösningen finns på [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="afcde-125">hello complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="afcde-126">Se [Hämta fullständiga lösningen till hello](#GetSolution) snabbguide.</span><span class="sxs-lookup"><span data-stu-id="afcde-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="afcde-127">När du har slutfört hello C++ kursen, knappar Kontrollera Använd hello röstning längst ned hello i den här sidan toogive oss feedback.</span><span class="sxs-lookup"><span data-stu-id="afcde-127">After you've completed hello C++ tutorial, please use hello voting buttons at hello bottom of this page toogive us feedback.</span></span> 

<span data-ttu-id="afcde-128">Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i kommentaren eller [nå ut toous här](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="afcde-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments or [reach out toous here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="afcde-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="afcde-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-c-tutorial"></a><span data-ttu-id="afcde-130">Förutsättningar för självstudiekursen hello C++</span><span class="sxs-lookup"><span data-stu-id="afcde-130">Prerequisites for hello C++ tutorial</span></span>
<span data-ttu-id="afcde-131">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="afcde-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="afcde-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="afcde-132">An active Azure account.</span></span> <span data-ttu-id="afcde-133">Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="afcde-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="afcde-134">[Visual Studio](https://www.visualstudio.com/downloads/), där hello C++ språkkomponenter är installerade.</span><span class="sxs-lookup"><span data-stu-id="afcde-134">[Visual Studio](https://www.visualstudio.com/downloads/), with hello C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="afcde-135">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="afcde-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="afcde-136">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="afcde-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="afcde-137">Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera C++-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="afcde-137">If you already have an account you want toouse, you can skip ahead too[Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="afcde-138"><a id="SetupC++"></a>Steg 2: Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="afcde-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="afcde-139">Öppna Visual Studio och klicka sedan på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="afcde-139">Open Visual Studio, and then on hello **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="afcde-140">I hello **nytt projekt** i hello fönstret **installerad** rutan Expandera **Visual C++**, klickar du på **Win32**, och klicka sedan på  **Win32-konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="afcde-140">In hello **New Project** window, in hello **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="afcde-141">Namnge hello projektet hellodocumentdb och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="afcde-141">Name hello project hellodocumentdb and then click **OK**.</span></span> 
   
    ![Skärmbild av guiden för ny hello-projekt](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="afcde-143">När hello guiden för Win32-program startas, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="afcde-143">When hello Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="afcde-144">När hello projektet har skapats, öppnar du hello NuGet-Pakethanteraren genom att högerklicka på hello **hellodocumentdb** projektet i **Solution Explorer** och klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="afcde-144">Once hello project has been created, open hello NuGet package manager by right-clicking hello **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![Skärmbild som visar hantera NuGet-paketet på hello projekt-menyn](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="afcde-146">I hello **NuGet: hellodocumentdb** klickar du på **Bläddra**, och sök sedan efter *documentdbcpp*.</span><span class="sxs-lookup"><span data-stu-id="afcde-146">In hello **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="afcde-147">Välj DocumentDbCPP, som visas i följande skärmbild hello i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="afcde-147">In hello results, select DocumentDbCPP, as shown in hello following screenshot.</span></span> <span data-ttu-id="afcde-148">Det här paketet installerar referenser tooC ++ REST SDK, vilket är ett beroende för hello DocumentDbCPP.</span><span class="sxs-lookup"><span data-stu-id="afcde-148">This package installs references tooC++ REST SDK, which is a dependency for hello DocumentDbCPP.</span></span>  
   
    ![Skärmbild som visar hello DocumentDbCpp paket markerat](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="afcde-150">När hello-paket har lagts till tooyour projekt, är vi alla set toostart skriva kod.</span><span class="sxs-lookup"><span data-stu-id="afcde-150">Once hello packages have been added tooyour project, we are all set toostart writing some code.</span></span>   

## <span data-ttu-id="afcde-151"><a id="Config"></a>Steg 3: Kopiera anslutningsinformationen från Azure Portal för din Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="afcde-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="afcde-152">Ta fram [Azure-portalen](https://portal.azure.com) och passerar toohello Azure Cosmos DB databaskonto som du skapade.</span><span class="sxs-lookup"><span data-stu-id="afcde-152">Bring up [Azure portal](https://portal.azure.com) and traverse toohello Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="afcde-153">Vi behöver hello URI och primärnyckel för hello från Azure-portalen i hello nästa steg tooestablish en anslutning från våra kodstycke i C++.</span><span class="sxs-lookup"><span data-stu-id="afcde-153">We will need hello URI and hello primary key from Azure portal in hello next step tooestablish a connection from our C++ code snippet.</span></span> 

![Azure Cosmos DB URI och nycklar i hello Azure-portalen](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="afcde-155"><a id="Connect"></a>Steg 4: Anslut tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="afcde-155"><a id="Connect"></a>Step 4: Connect tooan Azure Cosmos DB account</span></span>
1. <span data-ttu-id="afcde-156">Lägg till hello följande rubriker och namnområden tooyour källkoden efter `#include "stdafx.h"`.</span><span class="sxs-lookup"><span data-stu-id="afcde-156">Add hello following headers and namespaces tooyour source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="afcde-157">Härnäst ska du lägga till hello följande kod tooyour huvudsakliga funktion och Ersätt hello kontokonfiguration och primär nyckel toomatch Azure Cosmos DB inställningarna från steg 3.</span><span class="sxs-lookup"><span data-stu-id="afcde-157">Next add hello following code tooyour main function and replace hello account configuration and primary key toomatch your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="afcde-158">Nu när du har hello kod tooinitialize hello documentdb-klienten ska vi titta på arbeta med Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="afcde-158">Now that you have hello code tooinitialize hello documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="afcde-159"><a id="CreateDBColl"></a>Steg 5: Skapa en C++-databas och -samling</span><span class="sxs-lookup"><span data-stu-id="afcde-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="afcde-160">Innan vi utför det här steget ska vi gå igenom hur en databas, samling och dokument samverkar för de som är nya tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="afcde-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new tooAzure Cosmos DB.</span></span> <span data-ttu-id="afcde-161">En [databas](documentdb-resources.md#databases) är en logisk behållare för dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="afcde-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="afcde-162">En [samlingen](documentdb-resources.md#collections) är en behållare för JSON-dokument och hello associerad JavaScript-programlogik.</span><span class="sxs-lookup"><span data-stu-id="afcde-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and hello associated JavaScript application logic.</span></span> <span data-ttu-id="afcde-163">Du kan lära dig mer om hello Azure Cosmos DB hierarkiska resursmodell och begrepp i [Azure Cosmos DB hierarkiska resursmodell och begrepp](documentdb-resources.md).</span><span class="sxs-lookup"><span data-stu-id="afcde-163">You can learn more about hello Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="afcde-164">Lägg till hello efter koden toohello ditt huvudsakliga funktion toocreate en databas och en motsvarande samling.</span><span class="sxs-lookup"><span data-stu-id="afcde-164">toocreate a database and a corresponding collection add hello following code toohello end of your main function.</span></span> <span data-ttu-id="afcde-165">Detta skapar en databas som heter ”FamilyRegistry” och en samling som kallas ”FamilyCollection' med hello klientkonfigurationen som du har deklarerats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="afcde-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using hello client configuration you declared in hello previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="afcde-166"><a id="CreateDoc"></a>Steg 6: Skapa ett dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="afcde-167">[Dokument](documentdb-resources.md#documents) är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="afcde-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="afcde-168">Nu kan du infoga ett dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="afcde-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="afcde-169">Du kan skapa ett dokument genom att kopiera hello följande kod i hello slutet av hello huvudsakliga funktion.</span><span class="sxs-lookup"><span data-stu-id="afcde-169">You can create a document by copying hello following code into hello end of hello main function.</span></span> 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

<span data-ttu-id="afcde-170">toosummarize, den här koden skapar en Azure Cosmos-DB-databas, samling och dokument, som du kan fråga i Dokumentutforskaren i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="afcde-170">toosummarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![Självstudier för C++ - Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello-databasen, hello samlingen och hello dokument](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="afcde-172"><a id="QueryDB"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="afcde-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="afcde-173">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="afcde-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="afcde-174">hello visar följande exempelkod en fråga som skapats med hjälp av SQL-syntax som du kan köra mot hello dokument vi skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="afcde-174">hello following sample code shows a query made using SQL syntax that you can run against hello documents we created in hello previous step.</span></span>

<span data-ttu-id="afcde-175">hello funktionen använder som argument hello Unik identifierare eller resurs-id för hello databas och hello samling tillsammans med hello dokumentet klienten.</span><span class="sxs-lookup"><span data-stu-id="afcde-175">hello function takes in as arguments hello unique identifier or resource id for hello database and hello collection along with hello document client.</span></span> <span data-ttu-id="afcde-176">Lägg till den här koden före huvudfunktionen.</span><span class="sxs-lookup"><span data-stu-id="afcde-176">Add this code before main function.</span></span>

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="afcde-177"><a id="Replace"></a>Steg 8: Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="afcde-178">Azure Cosmos-DB stöder ersätta JSON-dokument som visas i följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="afcde-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in hello following code.</span></span> <span data-ttu-id="afcde-179">Lägg till den här koden efter hello executesimplequery funktion.</span><span class="sxs-lookup"><span data-stu-id="afcde-179">Add this code after hello executesimplequery function.</span></span>

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <span data-ttu-id="afcde-180"><a id="Delete"></a>Steg 9: Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="afcde-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="afcde-181">Azure DB Cosmos stöder ta bort JSON-dokument kan göra du det genom att kopiera och klistra in hello följande kod efter hello replacedocument funktionen.</span><span class="sxs-lookup"><span data-stu-id="afcde-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting hello following code after hello replacedocument function.</span></span> 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="afcde-182"><a id="DeleteDB"></a>Steg 10: Ta bort en databas</span><span class="sxs-lookup"><span data-stu-id="afcde-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="afcde-183">Ta bort hello skapade databasen tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).</span><span class="sxs-lookup"><span data-stu-id="afcde-183">Deleting hello created database removes hello database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="afcde-184">Kopiera och klistra in följande kodutdrag (funktionen cleanup) efter hello deletedocument funktionen tooremove hello databasen och alla underordnade resurser för hello hello.</span><span class="sxs-lookup"><span data-stu-id="afcde-184">Copy and paste hello following code snippet (function cleanup) after hello deletedocument function tooremove hello database and all hello child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="afcde-185"><a id="Run"></a>Steg 11: Kör C++-appen i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="afcde-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="afcde-186">Vi har nu lagt till kod toocreate, fråga, ändra och ta bort olika Azure DB som Cosmos-resurser.</span><span class="sxs-lookup"><span data-stu-id="afcde-186">We have now added code toocreate, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="afcde-187">Låt oss nu tråd detta upp genom att lägga till anrop toothese olika funktioner från vår huvudsakliga funktion i hellodocumentdb.cpp tillsammans med vissa diagnostiska meddelanden.</span><span class="sxs-lookup"><span data-stu-id="afcde-187">Let us now wire this up by adding calls toothese different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="afcde-188">Du kan göra det genom att ersätta hello Huvudsyftet med ditt program med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="afcde-188">You can do so by replacing hello main function of your application with hello following code.</span></span> <span data-ttu-id="afcde-189">Detta skriver över hello account_configuration_uri och primary_key som du kopierade till hello kod i steg3, så Spara som raden eller kopiera hello-värden i igen från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="afcde-189">This writes over hello account_configuration_uri and primary_key you copied into hello code in Step 3, so save that line or copy hello values in again from hello portal.</span></span> 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

<span data-ttu-id="afcde-190">Du bör nu vara kan toobuild och köra din kod i Visual Studio genom att trycka på F5 eller hello också körbara i hello terminalfönster hittar programmet hello och körs.</span><span class="sxs-lookup"><span data-stu-id="afcde-190">You should now be able toobuild and run your code in Visual Studio by pressing F5 or alternatively in hello terminal window by locating hello application and running hello executable.</span></span> 

<span data-ttu-id="afcde-191">Du bör se get started-appens hello utdata.</span><span class="sxs-lookup"><span data-stu-id="afcde-191">You should see hello output of your get started app.</span></span> <span data-ttu-id="afcde-192">hello utdata bör motsvara hello följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="afcde-192">hello output should match hello following screenshot.</span></span>

![Utdata från Azure Cosmos DB-baserat C++-program](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="afcde-194">Grattis!</span><span class="sxs-lookup"><span data-stu-id="afcde-194">Congratulations!</span></span> <span data-ttu-id="afcde-195">Du har slutfört hello C++ självstudier och har ditt första Azure DB som Cosmos-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="afcde-195">You've completed hello C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="afcde-196"><a id="GetSolution"></a>Hämta hello fullständiga C++ självstudiekursen lösningen</span><span class="sxs-lookup"><span data-stu-id="afcde-196"><a id="GetSolution"></a>Get hello complete C++ tutorial solution</span></span>
<span data-ttu-id="afcde-197">toobuild hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="afcde-197">toobuild hello GetStarted solution that contains all hello samples in this article, you need hello following:</span></span>

* <span data-ttu-id="afcde-198">[Azure Cosmos DB-konto][create-account].</span><span class="sxs-lookup"><span data-stu-id="afcde-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="afcde-199">Hej [GetStarted](https://github.com/stalker314314/DocumentDBCpp) lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="afcde-199">hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="afcde-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="afcde-200">Next steps</span></span>
* <span data-ttu-id="afcde-201">Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="afcde-201">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="afcde-202">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="afcde-202">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="afcde-203">Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="afcde-203">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


