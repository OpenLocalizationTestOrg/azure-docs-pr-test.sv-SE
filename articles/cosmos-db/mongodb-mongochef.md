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
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Använda MongoChef med en Azure-Cosmos-DB: API för MongoDB-konto

tooconnect tooan Azure Cosmos DB: API för MongoDB-konto, måste du:

* Hämta och installera [MongoChef](http://3t.io/mongochef)
* Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information

## <a name="create-hello-connection-in-mongochef"></a>Skapa hello anslutning i MongoChef
tooadd Azure Cosmos-DB: API för MongoDB konto toohello MongoChef Anslutningshanteraren, utföra hello följande steg.

1. Hämta Azure Cosmos-DB: API: et för MongoDB anslutningsinformationen använder hello instruktioner [här](connect-mongodb-account.md).

    ![Skärmbild av bladet för hello anslutning sträng](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klicka på **Anslut** tooopen hello Connection Manager och klicka sedan på **ny anslutning**

    ![Skärmbild som visar hello MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectionManager.png)
3. I hello **ny anslutning** fönstret på hello **Server** ange hello värden (FQDN) på hello Azure Cosmos DB: API för MongoDB-konto och hello PORT.

    ![Skärmbild som visar hello MongoChef manager-servern på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. I hello **ny anslutning** fönstret på hello **autentisering** väljer autentiseringsläge **Standard (CR MONGODB eller SCARM-SHA-1)** och ange hello användarnamn och LÖSENORDET.  Acceptera hello standard autentisering db (admin) eller ange ett eget värde.

    ![Skärmbild som visar hello MongoChef manager autentisering på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. I hello **ny anslutning** fönstret på hello **SSL** kontrollerar hello **Använd SSL-protokollet tooconnect** kryssrutan och hello **acceptera server självsignerat SSL certifikat** knappen.

    ![Skärmbild som visar hello MongoChef manager SSL på fliken anslutning](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klicka på hello **Testanslutningen** toovalidate hello anslutningsinformation, klicka på **OK** tooreturn toohello nya fönstret anslutning och klicka sedan på **spara**.

    ![Skärmbild som visar fönstret för hello MongoChef Testa anslutning](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a>Använd MongoChef toocreate en databas, samling och dokument
toocreate en databas, samling och dokument med hjälp av MongoChef, utföra hello följande steg.

1. I **Connection Manager**, markera hello anslutningen och klicka på **Anslut**.

    ![Skärmbild som visar hello MongoChef Anslutningshanteraren](./media/mongodb-mongochef/ConnectToAccount.png)
2. Högerklicka på hello värden och välj **lägga till databas**.  Ange ett namn och klicka på **OK**.

    ![Skärmbild som visar hello databasalternativet MongoChef Lägg till](./media/mongodb-mongochef/AddDatabase1.png)
3. Högerklicka på hello databas och välj **lägga till samlingen**.  Ange ett samlingsnamn och klicka på **skapa**.

    ![Skärmbild som visar hello MongoChef lägga till samlingen alternativet](./media/mongodb-mongochef/AddCollection.png)
4. Klicka på hello **samling** menyn, klicka på **Lägg till dokument**.

    ![Skärmbild som visar hello MongoChef Lägg till dokument menyobjekt](./media/mongodb-mongochef/AddDocument1.png)
5. Klistra in hello följande i dialogrutan Lägg till dokument hello och klicka sedan på **Lägg till dokument**.

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
6. Lägg till ett annat dokument med hello följande innehåll.

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
7. Köra en exempelfråga. Till exempel söka efter familjer med hello efternamn 'Andersen' och returnera hello överordnade och tillstånd.

    ![Skärmbild av Mongo Chef frågeresultat](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Nästa steg
* Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).
