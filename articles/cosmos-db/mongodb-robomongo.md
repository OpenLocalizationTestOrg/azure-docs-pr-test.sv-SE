---
title: "Använd Robomongo för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du använder Robomongo med en Azure-Cosmos-DB: API för MongoDB-konto"
keywords: robomongo
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
ms.openlocfilehash: 8983594776a1bbe413a6d7cf2cd518f0e327648a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="7ee17-104">Använda Robomongo med en Azure-Cosmos-DB: API för MongoDB-konto</span><span class="sxs-lookup"><span data-stu-id="7ee17-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="7ee17-105">Att ansluta till en Azure-Cosmos-DB: API för MongoDB-konto med hjälp av Robomongo, måste du:</span><span class="sxs-lookup"><span data-stu-id="7ee17-105">To connect to an Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="7ee17-106">Hämta och installera [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="7ee17-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="7ee17-107">Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information</span><span class="sxs-lookup"><span data-stu-id="7ee17-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="7ee17-108">Ansluta med Robomongo</span><span class="sxs-lookup"><span data-stu-id="7ee17-108">Connect using Robomongo</span></span>
<span data-ttu-id="7ee17-109">Att lägga till Azure Cosmos-DB: API MongoDB-kontot Robomongo MongoDB anslutningar, utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="7ee17-109">To add your Azure Cosmos DB: API for MongoDB account to the Robomongo MongoDB Connections, perform the following steps.</span></span>

1. <span data-ttu-id="7ee17-110">Hämta Azure Cosmos-DB: API: et för anslutningsinformationen för MongoDB-konto med hjälp av anvisningarna [här](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="7ee17-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![Skärmbild av bladet anslutning sträng](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="7ee17-112">Kör *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="7ee17-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="7ee17-113">Klicka på anslutningsknappen under **filen** att hantera dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7ee17-113">Click the connection button under **File** to manage your connections.</span></span> <span data-ttu-id="7ee17-114">Klicka på **skapa** i den **MongoDB anslutningar** fönster som öppnas i **anslutningsinställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="7ee17-114">Then, click **Create** in the **MongoDB Connections** window, which will open up the **Connection Settings** window.</span></span>

4. <span data-ttu-id="7ee17-115">I den **anslutningsinställningar** fönster, Välj ett namn.</span><span class="sxs-lookup"><span data-stu-id="7ee17-115">In the **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="7ee17-116">Leta sedan upp den **värden** och **Port** från informationen i steg 1 och ange dem till **adress** och **Port**respektive.</span><span class="sxs-lookup"><span data-stu-id="7ee17-116">Then, find the **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Skärmbild av Robomongo hantera anslutningar](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="7ee17-118">På den **autentisering** klickar du på **utför autentisering**.</span><span class="sxs-lookup"><span data-stu-id="7ee17-118">On the **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="7ee17-119">Ange sedan databasen (standard är *Admin*), **användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7ee17-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="7ee17-120">Båda **användarnamn** och **lösenord** finns i informationen i steg 1.</span><span class="sxs-lookup"><span data-stu-id="7ee17-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![Skärmbild av fliken Robomongo autentisering](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="7ee17-122">På den **SSL** markerar **Använd SSL-protokollet**, ändrar den **autentiseringsmetod** till **ett självsignerat certifikat**.</span><span class="sxs-lookup"><span data-stu-id="7ee17-122">On the **SSL** tab, check **Use SSL protocol**, then change the **Authentication Method** to **Self-signed Certificate**.</span></span>

    ![Skärmbild av fliken Robomongo SSL](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="7ee17-124">Klicka slutligen på **Test** verifiera att du kan ansluta sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="7ee17-124">Finally, click **Test** to verify that you are able to connect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ee17-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ee17-125">Next steps</span></span>
* <span data-ttu-id="7ee17-126">Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7ee17-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
