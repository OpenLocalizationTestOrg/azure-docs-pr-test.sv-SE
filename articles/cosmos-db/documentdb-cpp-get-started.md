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
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a>Azure Cosmos DB: C++ konsolen självstudien för hello DocumentDB-API
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Välkommen till toohello C++ självstudier för hello Azure Cosmos DB DocumentDB API godkända SDK för C++! När du har slutfört den här självstudien har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser, inklusive en C++-databas.

Vi tar upp följande:

* Skapa och ansluta tooan Azure DB som Cosmos-konto
* Konfigurera ditt program
* Skapa en C++ Azure Cosmos DB-databas
* Skapa en samling
* Skapa JSON-dokument
* Frågar hello samling
* Ersätta ett dokument
* Ta bort ett dokument
* Tar bort hello C++ Azure Cosmos DB-databasen

Har du inte tid? Oroa dig inte! hello kompletta lösningen finns på [GitHub](https://github.com/stalker314314/DocumentDBCpp). Se [Hämta fullständiga lösningen till hello](#GetSolution) snabbguide.

När du har slutfört hello C++ kursen, knappar Kontrollera Använd hello röstning längst ned hello i den här sidan toogive oss feedback. 

Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i kommentaren eller [nå ut toous här](https://www.research.net/r/8BKRJ3Z). 

Nu sätter vi igång!

## <a name="prerequisites-for-hello-c-tutorial"></a>Förutsättningar för självstudiekursen hello C++
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/), där hello C++ språkkomponenter är installerade.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera C++-programmet](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>Steg 2: Konfigurera din app
1. Öppna Visual Studio och klicka sedan på hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**. 
2. I hello **nytt projekt** i hello fönstret **installerad** rutan Expandera **Visual C++**, klickar du på **Win32**, och klicka sedan på  **Win32-konsolprogram**. Namnge hello projektet hellodocumentdb och klicka på **OK**. 
   
    ![Skärmbild av guiden för ny hello-projekt](media/documentdb-cpp-get-started/hello.png)
3. När hello guiden för Win32-program startas, klickar du på **Slutför**.
4. När hello projektet har skapats, öppnar du hello NuGet-Pakethanteraren genom att högerklicka på hello **hellodocumentdb** projektet i **Solution Explorer** och klicka på **hantera NuGet-paket**. 
   
    ![Skärmbild som visar hantera NuGet-paketet på hello projekt-menyn](media/documentdb-cpp-get-started/nuget.png)
5. I hello **NuGet: hellodocumentdb** klickar du på **Bläddra**, och sök sedan efter *documentdbcpp*. Välj DocumentDbCPP, som visas i följande skärmbild hello i hello resultat. Det här paketet installerar referenser tooC ++ REST SDK, vilket är ett beroende för hello DocumentDbCPP.  
   
    ![Skärmbild som visar hello DocumentDbCpp paket markerat](media/documentdb-cpp-get-started/cpp.png)
   
    När hello-paket har lagts till tooyour projekt, är vi alla set toostart skriva kod.   

## <a id="Config"></a>Steg 3: Kopiera anslutningsinformationen från Azure Portal för din Azure Cosmos DB-databas
Ta fram [Azure-portalen](https://portal.azure.com) och passerar toohello Azure Cosmos DB databaskonto som du skapade. Vi behöver hello URI och primärnyckel för hello från Azure-portalen i hello nästa steg tooestablish en anslutning från våra kodstycke i C++. 

![Azure Cosmos DB URI och nycklar i hello Azure-portalen](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Steg 4: Anslut tooan Azure DB som Cosmos-konto
1. Lägg till hello följande rubriker och namnområden tooyour källkoden efter `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Härnäst ska du lägga till hello följande kod tooyour huvudsakliga funktion och Ersätt hello kontokonfiguration och primär nyckel toomatch Azure Cosmos DB inställningarna från steg 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Nu när du har hello kod tooinitialize hello documentdb-klienten ska vi titta på arbeta med Azure Cosmos DB resurser.

## <a id="CreateDBColl"></a>Steg 5: Skapa en C++-databas och -samling
Innan vi utför det här steget ska vi gå igenom hur en databas, samling och dokument samverkar för de som är nya tooAzure Cosmos DB. En [databas](documentdb-resources.md#databases) är en logisk behållare för dokumentlagring, partitionerad över samlingarna. En [samlingen](documentdb-resources.md#collections) är en behållare för JSON-dokument och hello associerad JavaScript-programlogik. Du kan lära dig mer om hello Azure Cosmos DB hierarkiska resursmodell och begrepp i [Azure Cosmos DB hierarkiska resursmodell och begrepp](documentdb-resources.md).

Lägg till hello efter koden toohello ditt huvudsakliga funktion toocreate en databas och en motsvarande samling. Detta skapar en databas som heter ”FamilyRegistry” och en samling som kallas ”FamilyCollection' med hello klientkonfigurationen som du har deklarerats i hello föregående steg.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Steg 6: Skapa ett dokument
[Dokument](documentdb-resources.md#documents) är användardefinierat (godtyckligt) JSON-innehåll. Nu kan du infoga ett dokument i Azure Cosmos DB. Du kan skapa ett dokument genom att kopiera hello följande kod i hello slutet av hello huvudsakliga funktion. 

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

toosummarize, den här koden skapar en Azure Cosmos-DB-databas, samling och dokument, som du kan fråga i Dokumentutforskaren i Azure-portalen. 

![Självstudier för C++ - Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello-databasen, hello samlingen och hello dokument](media/documentdb-cpp-get-started/docs.png)

## <a id="QueryDB"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling. hello visar följande exempelkod en fråga som skapats med hjälp av SQL-syntax som du kan köra mot hello dokument vi skapade i föregående steg i hello.

hello funktionen använder som argument hello Unik identifierare eller resurs-id för hello databas och hello samling tillsammans med hello dokumentet klienten. Lägg till den här koden före huvudfunktionen.

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

## <a id="Replace"></a>Steg 8: Ersätta ett dokument
Azure Cosmos-DB stöder ersätta JSON-dokument som visas i följande kod hello. Lägg till den här koden efter hello executesimplequery funktion.

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

## <a id="Delete"></a>Steg 9: Ta bort ett dokument
Azure DB Cosmos stöder ta bort JSON-dokument kan göra du det genom att kopiera och klistra in hello följande kod efter hello replacedocument funktionen. 

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

## <a id="DeleteDB"></a>Steg 10: Ta bort en databas
Ta bort hello skapade databasen tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).

Kopiera och klistra in följande kodutdrag (funktionen cleanup) efter hello deletedocument funktionen tooremove hello databasen och alla underordnade resurser för hello hello.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Steg 11: Kör C++-appen i sin helhet!
Vi har nu lagt till kod toocreate, fråga, ändra och ta bort olika Azure DB som Cosmos-resurser.  Låt oss nu tråd detta upp genom att lägga till anrop toothese olika funktioner från vår huvudsakliga funktion i hellodocumentdb.cpp tillsammans med vissa diagnostiska meddelanden.

Du kan göra det genom att ersätta hello Huvudsyftet med ditt program med följande kod hello. Detta skriver över hello account_configuration_uri och primary_key som du kopierade till hello kod i steg3, så Spara som raden eller kopiera hello-värden i igen från hello-portalen. 

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

Du bör nu vara kan toobuild och köra din kod i Visual Studio genom att trycka på F5 eller hello också körbara i hello terminalfönster hittar programmet hello och körs. 

Du bör se get started-appens hello utdata. hello utdata bör motsvara hello följande skärmbild.

![Utdata från Azure Cosmos DB-baserat C++-program](media/documentdb-cpp-get-started/console.png)

Grattis! Du har slutfört hello C++ självstudier och har ditt första Azure DB som Cosmos-konsolprogram!

## <a id="GetSolution"></a>Hämta hello fullständiga C++ självstudiekursen lösningen
toobuild hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:

* [Azure Cosmos DB-konto][create-account].
* Hej [GetStarted](https://github.com/stalker314314/DocumentDBCpp) lösningen som finns på GitHub.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account


