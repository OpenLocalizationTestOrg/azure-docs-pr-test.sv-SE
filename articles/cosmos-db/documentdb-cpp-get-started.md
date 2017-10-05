---
title: "C++-självstudiekurs för Azure Cosmos DB | Microsoft Docs"
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
ms.openlocfilehash: 7d8de973765830ccd7983182bc1bb19b1e01e505
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-documentdb-api"></a><span data-ttu-id="30327-104">Azure Cosmos DB: Självstudiekurs för att skapa ett C++-konsolprogram för DocumentDB-API:et</span><span class="sxs-lookup"><span data-stu-id="30327-104">Azure Cosmos DB: C++ console application tutorial for the DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="30327-105">.NET</span><span class="sxs-lookup"><span data-stu-id="30327-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="30327-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="30327-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="30327-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="30327-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="30327-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="30327-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="30327-109">Java</span><span class="sxs-lookup"><span data-stu-id="30327-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="30327-110">C++</span><span class="sxs-lookup"><span data-stu-id="30327-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="30327-111">Välkommen till C++-självstudiekursen om vårt Azure Cosmos DB DocumentDB API-kompatibla SDK för C++!</span><span class="sxs-lookup"><span data-stu-id="30327-111">Welcome to the C++ tutorial for the Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="30327-112">När du har slutfört den här självstudien har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser, inklusive en C++-databas.</span><span class="sxs-lookup"><span data-stu-id="30327-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="30327-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="30327-113">We'll cover:</span></span>

* <span data-ttu-id="30327-114">Skapa och ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="30327-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="30327-115">Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="30327-115">Setting up your application</span></span>
* <span data-ttu-id="30327-116">Skapa en C++ Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="30327-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="30327-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="30327-117">Creating a collection</span></span>
* <span data-ttu-id="30327-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="30327-118">Creating JSON documents</span></span>
* <span data-ttu-id="30327-119">Skicka frågor till samlingen</span><span class="sxs-lookup"><span data-stu-id="30327-119">Querying the collection</span></span>
* <span data-ttu-id="30327-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="30327-120">Replacing a document</span></span>
* <span data-ttu-id="30327-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="30327-121">Deleting a document</span></span>
* <span data-ttu-id="30327-122">Ta bort C++ Azure Cosmos DB-databasen</span><span class="sxs-lookup"><span data-stu-id="30327-122">Deleting the C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="30327-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="30327-123">Don't have time?</span></span> <span data-ttu-id="30327-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="30327-124">Don't worry!</span></span> <span data-ttu-id="30327-125">Den kompletta lösningen finns på [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="30327-125">The complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="30327-126">En snabbguide finns i [Hämta den kompletta lösningen](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="30327-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="30327-127">När du har genomfört självstudien om C++ får du gärna ge oss feedback med röstningsknapparna längst ned på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="30327-127">After you've completed the C++ tutorial, please use the voting buttons at the bottom of this page to give us feedback.</span></span> 

<span data-ttu-id="30327-128">Om du vill att vi ska kontakta dig direkt kan du skriva din e-postadress i kommentaren eller [kontakta oss här](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="30327-128">If you'd like us to contact you directly, feel free to include your email address in your comments or [reach out to us here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="30327-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="30327-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-c-tutorial"></a><span data-ttu-id="30327-130">Förutsättningar för självstudien om C++</span><span class="sxs-lookup"><span data-stu-id="30327-130">Prerequisites for the C++ tutorial</span></span>
<span data-ttu-id="30327-131">Se till att du har följande:</span><span class="sxs-lookup"><span data-stu-id="30327-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="30327-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="30327-132">An active Azure account.</span></span> <span data-ttu-id="30327-133">Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30327-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="30327-134">[Visual Studio](https://www.visualstudio.com/downloads/), med språkkomponenter för C++ installeras.</span><span class="sxs-lookup"><span data-stu-id="30327-134">[Visual Studio](https://www.visualstudio.com/downloads/), with the C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="30327-135">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="30327-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="30327-136">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="30327-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="30327-137">Om du redan har ett konto som du vill använda kan du gå vidare till [Konfigurera C++-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="30327-137">If you already have an account you want to use, you can skip ahead to [Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="30327-138"><a id="SetupC++"></a>Steg 2: Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="30327-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="30327-139">Öppna Visual Studio och klicka på **Nytt** i menyn **Arkiv** och sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="30327-139">Open Visual Studio, and then on the **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="30327-140">I fönstret **Nytt projekt** på panelen **Installerade** expanderar du **Visual C++**. Klicka sedan på **Win32** och klicka sedan på **Win32-konsolprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="30327-140">In the **New Project** window, in the **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="30327-141">Namnge projektet hellodocumentdb och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="30327-141">Name the project hellodocumentdb and then click **OK**.</span></span> 
   
    ![Skärmdump som visar fönstret Nytt projekt](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="30327-143">När guiden Win32-program startas, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="30327-143">When the Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="30327-144">När projektet har skapats öppnar du pakethanteraren NuGet genom att högerklicka på projektet **hellodocumentdb** i **Solution Explorer** och därefter på **Hantera NuGet-paketet**.</span><span class="sxs-lookup"><span data-stu-id="30327-144">Once the project has been created, open the NuGet package manager by right-clicking the **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![Skärmbild som visar Hantera NuGet-paketet på menyn Projekt](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="30327-146">På fliken **NuGet: hellodocumentdb** klickar du på **Bläddra**, och sedan söker du efter *documentdbcpp*.</span><span class="sxs-lookup"><span data-stu-id="30327-146">In the **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="30327-147">Välj DocumentDbCPP bland resultaten, enligt följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="30327-147">In the results, select DocumentDbCPP, as shown in the following screenshot.</span></span> <span data-ttu-id="30327-148">Paketet installerar referenser till C++ REST SDK, som är ett beroende för DocumentDbCPP.</span><span class="sxs-lookup"><span data-stu-id="30327-148">This package installs references to C++ REST SDK, which is a dependency for the DocumentDbCPP.</span></span>  
   
    ![Skärmbild som visar DocumentDbCpp-paketet](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="30327-150">När paketen har lagts till i projektet, kan vi börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="30327-150">Once the packages have been added to your project, we are all set to start writing some code.</span></span>   

## <span data-ttu-id="30327-151"><a id="Config"></a>Steg 3: Kopiera anslutningsinformationen från Azure Portal för din Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="30327-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="30327-152">Öppna [Azure Portal](https://portal.azure.com) och gå till Azure Cosmos DB-databaskontot som du skapat.</span><span class="sxs-lookup"><span data-stu-id="30327-152">Bring up [Azure portal](https://portal.azure.com) and traverse to the Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="30327-153">Vi behöver URI och den primära nyckeln från Azure-portalen i nästa steg för att upprätta en anslutning från vårt C++-kodfragment.</span><span class="sxs-lookup"><span data-stu-id="30327-153">We will need the URI and the primary key from Azure portal in the next step to establish a connection from our C++ code snippet.</span></span> 

![URI och nycklar för Azure Cosmos DB på Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="30327-155"><a id="Connect"></a>Steg 4: Ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="30327-155"><a id="Connect"></a>Step 4: Connect to an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="30327-156">Lägg till följande rubriker och namnområden i källkoden efter `#include "stdafx.h"`.</span><span class="sxs-lookup"><span data-stu-id="30327-156">Add the following headers and namespaces to your source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="30327-157">Lägg sedan till följande kod i din huvudfunktion och ersätt kontokonfigurationen och den primära nyckeln så att de matchar Azure Cosmos DB-inställningarna från steg 3.</span><span class="sxs-lookup"><span data-stu-id="30327-157">Next add the following code to your main function and replace the account configuration and primary key to match your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="30327-158">Nu har du koden för att initiera DocumentDB-klienten och det är dags att gå vidare till arbete med Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="30327-158">Now that you have the code to initialize the documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="30327-159"><a id="CreateDBColl"></a>Steg 5: Skapa en C++-databas och -samling</span><span class="sxs-lookup"><span data-stu-id="30327-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="30327-160">Innan vi utför det här steget ska vi gå igenom hur en databas, samling och dokument interagerar för dig som precis har börjat med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="30327-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new to Azure Cosmos DB.</span></span> <span data-ttu-id="30327-161">En [databas](documentdb-resources.md#databases) är en logisk behållare för dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="30327-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="30327-162">En [samling](documentdb-resources.md#collections) är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="30327-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and the associated JavaScript application logic.</span></span> <span data-ttu-id="30327-163">Mer information om begrepp och den hierarkiska resursmodellen i Azure Cosmos DB finns i avsnittet [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md) (Begrepp och den hierarkiska resursmodellen i Azure Cosmos DB).</span><span class="sxs-lookup"><span data-stu-id="30327-163">You can learn more about the Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="30327-164">Om du vill skapa en databas och en motsvarande samling ska du lägga till följande kod i slutet av din huvudfunktion.</span><span class="sxs-lookup"><span data-stu-id="30327-164">To create a database and a corresponding collection add the following code to the end of your main function.</span></span> <span data-ttu-id="30327-165">Detta skapar en databas som heter "FamilyRegistry" och en samling som heter "FamilyCollection' med klientkonfigurationen du deklarerade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="30327-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using the client configuration you declared in the previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="30327-166"><a id="CreateDoc"></a>Steg 6: Skapa ett dokument</span><span class="sxs-lookup"><span data-stu-id="30327-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="30327-167">[Dokument](documentdb-resources.md#documents) är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="30327-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="30327-168">Nu kan du infoga ett dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="30327-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="30327-169">Du kan skapa ett dokument genom att kopiera följande kod till slutet av huvudfunktionen.</span><span class="sxs-lookup"><span data-stu-id="30327-169">You can create a document by copying the following code into the end of the main function.</span></span> 

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

<span data-ttu-id="30327-170">Sammanfattningsvis skapar den här koden en Azure Cosmos DB-databas, en Azure Cosmos DB-samling och Azure Cosmos DB-dokument som du kan skicka frågor till i dokumentutforskaren på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="30327-170">To summarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![Självstudie om C++ – Diagram som illustrerar den hierarkiska relationen mellan kontot, databasen, samlingen och dokumenten](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="30327-172"><a id="QueryDB"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="30327-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="30327-173">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="30327-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="30327-174">Nedanstående exempelkod visar en fråga med SQL-syntax som du kan köra mot dokumenten som infogades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="30327-174">The following sample code shows a query made using SQL syntax that you can run against the documents we created in the previous step.</span></span>

<span data-ttu-id="30327-175">Funktionen tar som argument den unika identifieraren eller resurs-id för databasen och samlingen tillsammans med dokumentklienten.</span><span class="sxs-lookup"><span data-stu-id="30327-175">The function takes in as arguments the unique identifier or resource id for the database and the collection along with the document client.</span></span> <span data-ttu-id="30327-176">Lägg till den här koden före huvudfunktionen.</span><span class="sxs-lookup"><span data-stu-id="30327-176">Add this code before main function.</span></span>

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

## <span data-ttu-id="30327-177"><a id="Replace"></a>Steg 8: Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="30327-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="30327-178">Azure Cosmos DB stöder ersättning av JSON-dokument, som du ser i följande kod.</span><span class="sxs-lookup"><span data-stu-id="30327-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in the following code.</span></span> <span data-ttu-id="30327-179">Lägg till den här koden efter funktionen executesimplequery.</span><span class="sxs-lookup"><span data-stu-id="30327-179">Add this code after the executesimplequery function.</span></span>

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

## <span data-ttu-id="30327-180"><a id="Delete"></a>Steg 9: Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="30327-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="30327-181">Azure Cosmos DB stöder borttagning av JSON-dokument. Du kan ta bort JSON-dokument genom att kopiera och klistra in följande kod efter replacedocument-funktionen.</span><span class="sxs-lookup"><span data-stu-id="30327-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting the following code after the replacedocument function.</span></span> 

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

## <span data-ttu-id="30327-182"><a id="DeleteDB"></a>Steg 10: Ta bort en databas</span><span class="sxs-lookup"><span data-stu-id="30327-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="30327-183">Om du tar bort databasen du skapade försvinner databasen och alla underordnade resurser (t.ex. samlingar och dokument).</span><span class="sxs-lookup"><span data-stu-id="30327-183">Deleting the created database removes the database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="30327-184">Kopiera och klistra in följande kodutdrag (funktionen cleanup) efter funktionen deletedocument för att ta bort databasen och alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="30327-184">Copy and paste the following code snippet (function cleanup) after the deletedocument function to remove the database and all the child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="30327-185"><a id="Run"></a>Steg 11: Kör C++-appen i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="30327-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="30327-186">Nu har vi lagt till kod för att skapa, skicka frågor mot, ändra och ta bort olika Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="30327-186">We have now added code to create, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="30327-187">Nu är det dags att samla detta genom att lägga till anrop till de olika funktionerna från vår huvudfunktion i hellodocumentdb.cpp, tillsammans med vissa diagnostiska meddelanden.</span><span class="sxs-lookup"><span data-stu-id="30327-187">Let us now wire this up by adding calls to these different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="30327-188">Du kan göra det genom att ersätta huvudfunktionen i ditt program med följande kod.</span><span class="sxs-lookup"><span data-stu-id="30327-188">You can do so by replacing the main function of your application with the following code.</span></span> <span data-ttu-id="30327-189">Detta skriver över account_configuration_uri och primary_key som du kopierade till koden i steg 3, så spara raden eller kopiera värdena igen från portalen.</span><span class="sxs-lookup"><span data-stu-id="30327-189">This writes over the account_configuration_uri and primary_key you copied into the code in Step 3, so save that line or copy the values in again from the portal.</span></span> 

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

<span data-ttu-id="30327-190">Du bör nu kunna skapa och köra din kod i Visual Studio genom att trycka på F5 eller alternativt i terminalfönstret genom att hitta programmet och köra den körbara filen.</span><span class="sxs-lookup"><span data-stu-id="30327-190">You should now be able to build and run your code in Visual Studio by pressing F5 or alternatively in the terminal window by locating the application and running the executable.</span></span> 

<span data-ttu-id="30327-191">Du bör nu se utdata från din kom-igång-app.</span><span class="sxs-lookup"><span data-stu-id="30327-191">You should see the output of your get started app.</span></span> <span data-ttu-id="30327-192">Utdata bör matcha följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="30327-192">The output should match the following screenshot.</span></span>

![Utdata från Azure Cosmos DB-baserat C++-program](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="30327-194">Grattis!</span><span class="sxs-lookup"><span data-stu-id="30327-194">Congratulations!</span></span> <span data-ttu-id="30327-195">Du har slutfört C++-självstudiekursen och har skapat ditt första Azure Cosmos DB-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="30327-195">You've completed the C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="30327-196"><a id="GetSolution"></a>Hämta den fullständiga lösningen till C++-självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="30327-196"><a id="GetSolution"></a>Get the complete C++ tutorial solution</span></span>
<span data-ttu-id="30327-197">För att bygga GetStarted-lösningen med alla exempel i den här artikeln behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="30327-197">To build the GetStarted solution that contains all the samples in this article, you need the following:</span></span>

* <span data-ttu-id="30327-198">[Azure Cosmos DB-konto][create-account].</span><span class="sxs-lookup"><span data-stu-id="30327-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="30327-199">[GetStarted](https://github.com/stalker314314/DocumentDBCpp)-lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="30327-199">The [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30327-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30327-200">Next steps</span></span>
* <span data-ttu-id="30327-201">Lär dig hur du [övervakar ett Azure Cosmos DB-konto](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="30327-201">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="30327-202">Kör frågor mot vår exempeldatauppsättning i [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="30327-202">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="30327-203">Mer information om programmeringsmodellen finns i avsnittet Utveckla på [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="30327-203">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


