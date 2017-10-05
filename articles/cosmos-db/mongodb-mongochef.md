---
title: "Använd MongoChef för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du använder MongoChef med en Azure-Cosmos-DB: API för MongoDB-konto"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="75256-104">Använda MongoChef med en Azure-Cosmos-DB: API för MongoDB-konto</span><span class="sxs-lookup"><span data-stu-id="75256-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="75256-105">Att ansluta till en Azure-Cosmos-DB: API för MongoDB-konto, måste du:</span><span class="sxs-lookup"><span data-stu-id="75256-105">To connect to an Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="75256-106">Hämta och installera [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="75256-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="75256-107">Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information</span><span class="sxs-lookup"><span data-stu-id="75256-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-the-connection-in-mongochef"></a><span data-ttu-id="75256-108">Skapa anslutningen i MongoChef</span><span class="sxs-lookup"><span data-stu-id="75256-108">Create the connection in MongoChef</span></span>
<span data-ttu-id="75256-109">Att lägga till Azure Cosmos-DB: API MongoDB-kontot Anslutningshanteraren MongoChef utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="75256-109">To add your Azure Cosmos DB: API for MongoDB account to the MongoChef connection manager, perform the following steps.</span></span>

1. <span data-ttu-id="75256-110">Hämta Azure Cosmos-DB: API: et för MongoDB anslutningsinformationen med instruktioner [här](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="75256-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![Skärmbild av bladet anslutning sträng](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="75256-112">Klicka på **Anslut** öppna Connection Manager och sedan klicka på **ny anslutning**</span><span class="sxs-lookup"><span data-stu-id="75256-112">Click **Connect** to open the Connection Manager, then click **New Connection**</span></span>

    ![Skärmbild av MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="75256-114">I den **ny anslutning** fönstret på den **Server** ange värden (FQDN) på Azure Cosmos DB: API för MongoDB-kontot och PORT.</span><span class="sxs-lookup"><span data-stu-id="75256-114">In the **New Connection** window, on the **Server** tab, enter the HOST (FQDN) of the Azure Cosmos DB: API for MongoDB account and the PORT.</span></span>

    ![Skärmbild av fliken MongoChef anslutning manager server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="75256-116">I den **ny anslutning** fönstret på den **autentisering** väljer autentiseringsläge **Standard (CR MONGODB eller SCARM-SHA-1)** och ange användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="75256-116">In the **New Connection** window, on the **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter the USERNAME and PASSWORD.</span></span>  <span data-ttu-id="75256-117">Acceptera standardvärdet autentisering db (admin) eller ange ett eget värde.</span><span class="sxs-lookup"><span data-stu-id="75256-117">Accept the default authentication db (admin) or provide your own value.</span></span>

    ![Skärmbild av fliken MongoChef connection manager autentisering](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="75256-119">I den **ny anslutning** fönstret på den **SSL** markerar den **Använd SSL-protokollet för att ansluta** kryssrutan och **accepterar server självsignerade SSL-certifikat** knappen.</span><span class="sxs-lookup"><span data-stu-id="75256-119">In the **New Connection** window, on the **SSL** tab, check the **Use SSL protocol to connect** check box and the **Accept server self-signed SSL certificates** radio button.</span></span>

    ![Skärmbild av fliken MongoChef connection manager SSL](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="75256-121">Klicka på den **Testa anslutning** för att verifiera informationen klickar du på **OK** återgå till fönstret Ny anslutning och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="75256-121">Click the **Test Connection** button to validate the connection information, click **OK** to return to the New Connection window, and then click **Save**.</span></span>

    ![Skärmdump av fönstret MongoChef Testa anslutning](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a><span data-ttu-id="75256-123">Använd MongoChef för att skapa en databas, samling och dokument</span><span class="sxs-lookup"><span data-stu-id="75256-123">Use MongoChef to create a database, collection, and documents</span></span>
<span data-ttu-id="75256-124">Utför följande steg för att skapa en databas, samling och dokument med hjälp av MongoChef.</span><span class="sxs-lookup"><span data-stu-id="75256-124">To create a database, collection, and documents using MongoChef, perform the following steps.</span></span>

1. <span data-ttu-id="75256-125">I **Connection Manager**, markera anslutningen och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="75256-125">In **Connection Manager**, highlight the connection and click **Connect**.</span></span>

    ![Skärmbild av MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="75256-127">Högerklicka på värden och välj **lägga till databas**.</span><span class="sxs-lookup"><span data-stu-id="75256-127">Right click the host and choose **Add Database**.</span></span>  <span data-ttu-id="75256-128">Ange ett namn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="75256-128">Provide a database name and click **OK**.</span></span>

    ![Skärmdump av alternativet MongoChef lägga till databas](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="75256-130">Högerklicka på databasen och välj **lägga till samlingen**.</span><span class="sxs-lookup"><span data-stu-id="75256-130">Right click the database and choose **Add Collection**.</span></span>  <span data-ttu-id="75256-131">Ange ett samlingsnamn och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="75256-131">Provide a collection name and click **Create**.</span></span>

    ![Skärmdump av alternativet MongoChef lägga till samlingen](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="75256-133">Klicka på den **samling** menyn, klicka på **Lägg till dokument**.</span><span class="sxs-lookup"><span data-stu-id="75256-133">Click the **Collection** menu item, then click **Add Document**.</span></span>

    ![Skärmbild som visar menyalternativet MongoChef Lägg till dokument](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="75256-135">Klistra in följande i dialogrutan Lägg till dokument och klicka sedan på **Lägg till dokument**.</span><span class="sxs-lookup"><span data-stu-id="75256-135">In the Add Document dialog, paste the following and then click **Add Document**.</span></span>

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. <span data-ttu-id="75256-136">Lägg till ett annat dokument nu med följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="75256-136">Add another document, this time with the following content.</span></span>

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. <span data-ttu-id="75256-137">Köra en exempelfråga.</span><span class="sxs-lookup"><span data-stu-id="75256-137">Execute a sample query.</span></span> <span data-ttu-id="75256-138">Till exempel söka efter familjer med efternamn 'Andersen' och returnerar det överordnade och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="75256-138">For example, search for families with the last name 'Andersen' and return the parents and state fields.</span></span>

    ![Skärmbild av Mongo Chef frågeresultat](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="75256-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75256-140">Next steps</span></span>
* <span data-ttu-id="75256-141">Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="75256-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
