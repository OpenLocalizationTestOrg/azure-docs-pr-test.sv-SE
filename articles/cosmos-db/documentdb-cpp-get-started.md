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
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-documentdb-api"></a>Azure Cosmos DB: Självstudiekurs för att skapa ett C++-konsolprogram för DocumentDB-API:et
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Välkommen till C++-självstudiekursen om vårt Azure Cosmos DB DocumentDB API-kompatibla SDK för C++! När du har slutfört den här självstudien har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser, inklusive en C++-databas.

Vi tar upp följande:

* Skapa och ansluta till ett Azure Cosmos DB-konto
* Konfigurera ditt program
* Skapa en C++ Azure Cosmos DB-databas
* Skapa en samling
* Skapa JSON-dokument
* Skicka frågor till samlingen
* Ersätta ett dokument
* Ta bort ett dokument
* Ta bort C++ Azure Cosmos DB-databasen

Har du inte tid? Oroa dig inte! Den kompletta lösningen finns på [GitHub](https://github.com/stalker314314/DocumentDBCpp). En snabbguide finns i [Hämta den kompletta lösningen](#GetSolution).

När du har genomfört självstudien om C++ får du gärna ge oss feedback med röstningsknapparna längst ned på den här sidan. 

Om du vill att vi ska kontakta dig direkt kan du skriva din e-postadress i kommentaren eller [kontakta oss här](https://www.research.net/r/8BKRJ3Z). 

Nu sätter vi igång!

## <a name="prerequisites-for-the-c-tutorial"></a>Förutsättningar för självstudien om C++
Se till att du har följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/), med språkkomponenter för C++ installeras.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill använda kan du gå vidare till [Konfigurera C++-programmet](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>Steg 2: Konfigurera din app
1. Öppna Visual Studio och klicka på **Nytt** i menyn **Arkiv** och sedan på **Projekt**. 
2. I fönstret **Nytt projekt** på panelen **Installerade** expanderar du **Visual C++**. Klicka sedan på **Win32** och klicka sedan på **Win32-konsolprogrammet**. Namnge projektet hellodocumentdb och klicka sedan på **OK**. 
   
    ![Skärmdump som visar fönstret Nytt projekt](media/documentdb-cpp-get-started/hello.png)
3. När guiden Win32-program startas, klickar du på **Slutför**.
4. När projektet har skapats öppnar du pakethanteraren NuGet genom att högerklicka på projektet **hellodocumentdb** i **Solution Explorer** och därefter på **Hantera NuGet-paketet**. 
   
    ![Skärmbild som visar Hantera NuGet-paketet på menyn Projekt](media/documentdb-cpp-get-started/nuget.png)
5. På fliken **NuGet: hellodocumentdb** klickar du på **Bläddra**, och sedan söker du efter *documentdbcpp*. Välj DocumentDbCPP bland resultaten, enligt följande skärmbild. Paketet installerar referenser till C++ REST SDK, som är ett beroende för DocumentDbCPP.  
   
    ![Skärmbild som visar DocumentDbCpp-paketet](media/documentdb-cpp-get-started/cpp.png)
   
    När paketen har lagts till i projektet, kan vi börja skriva kod.   

## <a id="Config"></a>Steg 3: Kopiera anslutningsinformationen från Azure Portal för din Azure Cosmos DB-databas
Öppna [Azure Portal](https://portal.azure.com) och gå till Azure Cosmos DB-databaskontot som du skapat. Vi behöver URI och den primära nyckeln från Azure-portalen i nästa steg för att upprätta en anslutning från vårt C++-kodfragment. 

![URI och nycklar för Azure Cosmos DB på Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Steg 4: Ansluta till ett Azure Cosmos DB-konto
1. Lägg till följande rubriker och namnområden i källkoden efter `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Lägg sedan till följande kod i din huvudfunktion och ersätt kontokonfigurationen och den primära nyckeln så att de matchar Azure Cosmos DB-inställningarna från steg 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Nu har du koden för att initiera DocumentDB-klienten och det är dags att gå vidare till arbete med Azure Cosmos DB-resurser.

## <a id="CreateDBColl"></a>Steg 5: Skapa en C++-databas och -samling
Innan vi utför det här steget ska vi gå igenom hur en databas, samling och dokument interagerar för dig som precis har börjat med Azure Cosmos DB. En [databas](documentdb-resources.md#databases) är en logisk behållare för dokumentlagring, partitionerad över samlingarna. En [samling](documentdb-resources.md#collections) är en behållare för JSON-dokument och associerad JavaScript-applogik. Mer information om begrepp och den hierarkiska resursmodellen i Azure Cosmos DB finns i avsnittet [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md) (Begrepp och den hierarkiska resursmodellen i Azure Cosmos DB).

Om du vill skapa en databas och en motsvarande samling ska du lägga till följande kod i slutet av din huvudfunktion. Detta skapar en databas som heter "FamilyRegistry" och en samling som heter "FamilyCollection' med klientkonfigurationen du deklarerade i föregående steg.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Steg 6: Skapa ett dokument
[Dokument](documentdb-resources.md#documents) är användardefinierat (godtyckligt) JSON-innehåll. Nu kan du infoga ett dokument i Azure Cosmos DB. Du kan skapa ett dokument genom att kopiera följande kod till slutet av huvudfunktionen. 

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

Sammanfattningsvis skapar den här koden en Azure Cosmos DB-databas, en Azure Cosmos DB-samling och Azure Cosmos DB-dokument som du kan skicka frågor till i dokumentutforskaren på Azure Portal. 

![Självstudie om C++ – Diagram som illustrerar den hierarkiska relationen mellan kontot, databasen, samlingen och dokumenten](media/documentdb-cpp-get-started/docs.png)

## <a id="QueryDB"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling. Nedanstående exempelkod visar en fråga med SQL-syntax som du kan köra mot dokumenten som infogades i föregående steg.

Funktionen tar som argument den unika identifieraren eller resurs-id för databasen och samlingen tillsammans med dokumentklienten. Lägg till den här koden före huvudfunktionen.

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
Azure Cosmos DB stöder ersättning av JSON-dokument, som du ser i följande kod. Lägg till den här koden efter funktionen executesimplequery.

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
Azure Cosmos DB stöder borttagning av JSON-dokument. Du kan ta bort JSON-dokument genom att kopiera och klistra in följande kod efter replacedocument-funktionen. 

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
Om du tar bort databasen du skapade försvinner databasen och alla underordnade resurser (t.ex. samlingar och dokument).

Kopiera och klistra in följande kodutdrag (funktionen cleanup) efter funktionen deletedocument för att ta bort databasen och alla underordnade resurser.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Steg 11: Kör C++-appen i sin helhet!
Nu har vi lagt till kod för att skapa, skicka frågor mot, ändra och ta bort olika Azure Cosmos DB-resurser.  Nu är det dags att samla detta genom att lägga till anrop till de olika funktionerna från vår huvudfunktion i hellodocumentdb.cpp, tillsammans med vissa diagnostiska meddelanden.

Du kan göra det genom att ersätta huvudfunktionen i ditt program med följande kod. Detta skriver över account_configuration_uri och primary_key som du kopierade till koden i steg 3, så spara raden eller kopiera värdena igen från portalen. 

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

Du bör nu kunna skapa och köra din kod i Visual Studio genom att trycka på F5 eller alternativt i terminalfönstret genom att hitta programmet och köra den körbara filen. 

Du bör nu se utdata från din kom-igång-app. Utdata bör matcha följande skärmbild.

![Utdata från Azure Cosmos DB-baserat C++-program](media/documentdb-cpp-get-started/console.png)

Grattis! Du har slutfört C++-självstudiekursen och har skapat ditt första Azure Cosmos DB-konsolprogram!

## <a id="GetSolution"></a>Hämta den fullständiga lösningen till C++-självstudiekursen
För att bygga GetStarted-lösningen med alla exempel i den här artikeln behöver du följande:

* [Azure Cosmos DB-konto][create-account].
* [GetStarted](https://github.com/stalker314314/DocumentDBCpp)-lösningen som finns på GitHub.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur du [övervakar ett Azure Cosmos DB-konto](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i [Query Playground](https://www.documentdb.com/sql/demo).
* Mer information om programmeringsmodellen finns i avsnittet Utveckla på [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account


