---
title: "aaaUse MongoChef för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur toouse MongoChef med en Azure-Cosmos-DB: API för MongoDB-konto"
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
ms.openlocfilehash: 4b047797b231c34ccc6f2ed02416525c6228d596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="ca1df-104">Använda MongoChef med en Azure-Cosmos-DB: API för MongoDB-konto</span><span class="sxs-lookup"><span data-stu-id="ca1df-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="ca1df-105">tooconnect tooan Azure Cosmos DB: API för MongoDB-konto, måste du:</span><span class="sxs-lookup"><span data-stu-id="ca1df-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="ca1df-106">Hämta och installera [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="ca1df-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="ca1df-107">Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information</span><span class="sxs-lookup"><span data-stu-id="ca1df-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-hello-connection-in-mongochef"></a><span data-ttu-id="ca1df-108">Skapa hello anslutning i MongoChef</span><span class="sxs-lookup"><span data-stu-id="ca1df-108">Create hello connection in MongoChef</span></span>
<span data-ttu-id="ca1df-109">tooadd Azure Cosmos-DB: API för MongoDB konto toohello MongoChef Anslutningshanteraren, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ca1df-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello MongoChef connection manager, perform hello following steps.</span></span>

1. <span data-ttu-id="ca1df-110">Hämta Azure Cosmos-DB: API: et för MongoDB anslutningsinformationen använder hello instruktioner [här](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="ca1df-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Skärmbild av bladet för hello anslutning sträng](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="ca1df-112">Klicka på **Anslut** tooopen hello Connection Manager och klicka sedan på **ny anslutning**</span><span class="sxs-lookup"><span data-stu-id="ca1df-112">Click **Connect** tooopen hello Connection Manager, then click **New Connection**</span></span>

    ![Skärmbild som visar hello MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="ca1df-114">I hello **ny anslutning** fönstret på hello **Server** ange hello värden (FQDN) på hello Azure Cosmos DB: API för MongoDB-konto och hello PORT.</span><span class="sxs-lookup"><span data-stu-id="ca1df-114">In hello **New Connection** window, on hello **Server** tab, enter hello HOST (FQDN) of hello Azure Cosmos DB: API for MongoDB account and hello PORT.</span></span>

    ![Skärmbild som visar hello MongoChef manager-servern på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="ca1df-116">I hello **ny anslutning** fönstret på hello **autentisering** väljer autentiseringsläge **Standard (CR MONGODB eller SCARM-SHA-1)** och ange hello användarnamn och LÖSENORDET.</span><span class="sxs-lookup"><span data-stu-id="ca1df-116">In hello **New Connection** window, on hello **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter hello USERNAME and PASSWORD.</span></span>  <span data-ttu-id="ca1df-117">Acceptera hello standard autentisering db (admin) eller ange ett eget värde.</span><span class="sxs-lookup"><span data-stu-id="ca1df-117">Accept hello default authentication db (admin) or provide your own value.</span></span>

    ![Skärmbild som visar hello MongoChef manager autentisering på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="ca1df-119">I hello **ny anslutning** fönstret på hello **SSL** kontrollerar hello **Använd SSL-protokollet tooconnect** kryssrutan och hello **acceptera server självsignerat SSL certifikat** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca1df-119">In hello **New Connection** window, on hello **SSL** tab, check hello **Use SSL protocol tooconnect** check box and hello **Accept server self-signed SSL certificates** radio button.</span></span>

    ![Skärmbild som visar hello MongoChef manager SSL på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="ca1df-121">Klicka på hello **Testanslutningen** toovalidate hello anslutningsinformation, klicka på **OK** tooreturn toohello nya fönstret anslutning och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-121">Click hello **Test Connection** button toovalidate hello connection information, click **OK** tooreturn toohello New Connection window, and then click **Save**.</span></span>

    ![Skärmbild som visar fönstret för hello MongoChef Testa anslutning](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a><span data-ttu-id="ca1df-123">Använd MongoChef toocreate en databas, samling och dokument</span><span class="sxs-lookup"><span data-stu-id="ca1df-123">Use MongoChef toocreate a database, collection, and documents</span></span>
<span data-ttu-id="ca1df-124">toocreate en databas, samling och dokument med hjälp av MongoChef, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ca1df-124">toocreate a database, collection, and documents using MongoChef, perform hello following steps.</span></span>

1. <span data-ttu-id="ca1df-125">I **Connection Manager**, markera hello anslutningen och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-125">In **Connection Manager**, highlight hello connection and click **Connect**.</span></span>

    ![Skärmbild som visar hello MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="ca1df-127">Högerklicka på hello värden och välj **lägga till databas**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-127">Right click hello host and choose **Add Database**.</span></span>  <span data-ttu-id="ca1df-128">Ange ett namn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-128">Provide a database name and click **OK**.</span></span>

    ![Skärmbild som visar hello databasalternativet MongoChef Lägg till](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="ca1df-130">Högerklicka på hello databas och välj **lägga till samlingen**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-130">Right click hello database and choose **Add Collection**.</span></span>  <span data-ttu-id="ca1df-131">Ange ett samlingsnamn och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-131">Provide a collection name and click **Create**.</span></span>

    ![Skärmbild som visar hello MongoChef lägga till samlingen alternativet](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="ca1df-133">Klicka på hello **samling** menyn, klicka på **Lägg till dokument**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-133">Click hello **Collection** menu item, then click **Add Document**.</span></span>

    ![Skärmbild som visar hello MongoChef Lägg till dokument menyobjekt](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="ca1df-135">Klistra in hello följande i dialogrutan Lägg till dokument hello och klicka sedan på **Lägg till dokument**.</span><span class="sxs-lookup"><span data-stu-id="ca1df-135">In hello Add Document dialog, paste hello following and then click **Add Document**.</span></span>

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
6. <span data-ttu-id="ca1df-136">Lägg till ett annat dokument med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="ca1df-136">Add another document, this time with hello following content.</span></span>

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
7. <span data-ttu-id="ca1df-137">Köra en exempelfråga.</span><span class="sxs-lookup"><span data-stu-id="ca1df-137">Execute a sample query.</span></span> <span data-ttu-id="ca1df-138">Till exempel söka efter familjer med hello efternamn 'Andersen' och returnera hello överordnade och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ca1df-138">For example, search for families with hello last name 'Andersen' and return hello parents and state fields.</span></span>

    ![Skärmbild av Mongo Chef frågeresultat](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="ca1df-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca1df-140">Next steps</span></span>
* <span data-ttu-id="ca1df-141">Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ca1df-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
