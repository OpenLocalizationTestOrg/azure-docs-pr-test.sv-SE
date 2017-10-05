---
title: "MongoDB-anslutningssträng för ett konto i Azure Cosmos DB | Microsoft Docs"
description: "Lär dig mer om att ansluta appen MongoDB till ett Azure DB som Cosmos-konto med hjälp av en anslutningssträng för MongoDB."
keywords: "anslutningssträngen för mongodb"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.openlocfilehash: 5a47001705531d971d3181df9c0aa8f957168845
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a><span data-ttu-id="f079f-104">Ansluta ett MongoDB-program till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f079f-104">Connect a MongoDB application to Azure Cosmos DB</span></span>
<span data-ttu-id="f079f-105">Lär dig mer om att ansluta appen MongoDB till ett Azure DB som Cosmos-konto med hjälp av en anslutningssträng för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f079f-105">Learn how to connect your MongoDB app to an Azure Cosmos DB account by using a MongoDB connection string.</span></span> <span data-ttu-id="f079f-106">Du kan sedan använda en Azure Cosmos-DB-databas som data store för MongoDB-app.</span><span class="sxs-lookup"><span data-stu-id="f079f-106">You can then use an Azure Cosmos DB database as the data store for your MongoDB app.</span></span> 

<span data-ttu-id="f079f-107">Den här kursen finns det två sätt att hämta information om anslutningssträngar:</span><span class="sxs-lookup"><span data-stu-id="f079f-107">This tutorial provides two ways to retrieve connection string information:</span></span>

- <span data-ttu-id="f079f-108">[Snabbstartsguide metoden](#QuickstartConnection), för användning med .NET, Node.js, MongoDB-gränssnittet, Java och Python drivrutiner</span><span class="sxs-lookup"><span data-stu-id="f079f-108">[The quick start method](#QuickstartConnection), for use with .NET, Node.js, MongoDB Shell, Java, and Python drivers</span></span>
- <span data-ttu-id="f079f-109">[Anpassad sträng anslutningsmetoden](#GetCustomConnection), för användning med andra drivrutiner</span><span class="sxs-lookup"><span data-stu-id="f079f-109">[The custom connection string method](#GetCustomConnection), for use with other drivers</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f079f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f079f-110">Prerequisites</span></span>

- <span data-ttu-id="f079f-111">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f079f-111">An Azure account.</span></span> <span data-ttu-id="f079f-112">Om du inte har ett Azure-konto kan du skapa en [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) nu.</span><span class="sxs-lookup"><span data-stu-id="f079f-112">If you don't have an Azure account, create a [free Azure account](https://azure.microsoft.com/free/) now.</span></span> 
- <span data-ttu-id="f079f-113">Ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="f079f-113">An Azure Cosmos DB account.</span></span> <span data-ttu-id="f079f-114">Instruktioner finns i [bygga en MongoDB API webbprogram med .NET och Azure portal](create-mongodb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f079f-114">For instructions, see [Build a MongoDB API web app with .NET and the Azure portal](create-mongodb-dotnet.md).</span></span>

## <span data-ttu-id="f079f-115"><a id="QuickstartConnection"></a>Hämta MongoDB-anslutningssträngen med hjälp av Snabbstart</span><span class="sxs-lookup"><span data-stu-id="f079f-115"><a id="QuickstartConnection"></a>Get the MongoDB connection string by using the quick start</span></span>
1. <span data-ttu-id="f079f-116">I en webbläsare, logga in på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f079f-116">In an Internet browser, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f079f-117">I den **Azure Cosmos DB** bladet välj API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="f079f-117">In the **Azure Cosmos DB** blade, select the API for MongoDB account.</span></span> 
3. <span data-ttu-id="f079f-118">I den vänstra rutan på konto-bladet klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="f079f-118">In the left pane of the account blade, click **Quick start**.</span></span> 
4. <span data-ttu-id="f079f-119">Välj din plattform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span><span class="sxs-lookup"><span data-stu-id="f079f-119">Choose your platform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span></span> <span data-ttu-id="f079f-120">Om du inte ser ditt drivrutin eller verktyg, oroa inte dig – dokumentera vi kontinuerligt mer kodfragment för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f079f-120">If you don't see your driver or tool listed, don't worry--we continuously document more connection code snippets.</span></span> <span data-ttu-id="f079f-121">Kommentera nedan på vad du skulle vilja se.</span><span class="sxs-lookup"><span data-stu-id="f079f-121">Please comment below on what you'd like to see.</span></span> <span data-ttu-id="f079f-122">Om du vill lära dig skapa dina egna anslutning, läsa [hämta kontots Anslutningssträngsinformation](#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="f079f-122">To learn how to craft your own connection, read [Get the account's connection string information](#GetCustomConnection).</span></span>
5. <span data-ttu-id="f079f-123">Kopiera och klistra in kodfragmentet i MongoDB-app.</span><span class="sxs-lookup"><span data-stu-id="f079f-123">Copy and paste the code snippet into your MongoDB app.</span></span>

    ![Snabbstart-bladet](./media/connect-mongodb-account/QuickStartBlade.png)

## <span data-ttu-id="f079f-125"><a id="GetCustomConnection"></a>Hämta anslutningssträngen för MongoDB för att anpassa</span><span class="sxs-lookup"><span data-stu-id="f079f-125"><a id="GetCustomConnection"></a> Get the MongoDB connection string to customize</span></span>
1. <span data-ttu-id="f079f-126">I en webbläsare, logga in på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f079f-126">In an Internet browser, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f079f-127">I den **Azure Cosmos DB** bladet välj API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="f079f-127">In the **Azure Cosmos DB** blade, select the API for MongoDB account.</span></span> 
3. <span data-ttu-id="f079f-128">I den vänstra rutan på konto-bladet klickar du på **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="f079f-128">In the left pane of the account blade, click **Connection String**.</span></span> 
4. <span data-ttu-id="f079f-129">Den **anslutningssträngen** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="f079f-129">The **Connection String** blade opens.</span></span> <span data-ttu-id="f079f-130">Den har all information som behövs för att ansluta till kontot genom att använda en drivrutin för MongoDB, inklusive en preconstructed anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="f079f-130">It has all the information necessary to connect to the account by using a driver for MongoDB, including a preconstructed connection string.</span></span>

    ![Anslutningen sträng bladet](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a><span data-ttu-id="f079f-132">Anslutningen sträng krav</span><span class="sxs-lookup"><span data-stu-id="f079f-132">Connection string requirements</span></span>
> [!Important]
> <span data-ttu-id="f079f-133">Azure Cosmos-DB har stränga säkerhetskrav och standarder.</span><span class="sxs-lookup"><span data-stu-id="f079f-133">Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="f079f-134">Azure DB Cosmos-konton som kräver autentisering och säker kommunikation via *SSL*.</span><span class="sxs-lookup"><span data-stu-id="f079f-134">Azure Cosmos DB accounts require authentication and secure communication via *SSL*.</span></span> 
>
>

<span data-ttu-id="f079f-135">Azure Cosmos-DB stöder det standard MongoDB-Anslutningssträngens URI formatet, med några särskilda krav: Azure Cosmos DB konton som kräver autentisering och säker kommunikation via SSL.</span><span class="sxs-lookup"><span data-stu-id="f079f-135">Azure Cosmos DB supports the standard MongoDB connection string URI format, with a couple of specific requirements: Azure Cosmos DB accounts require authentication and secure communication via SSL.</span></span> <span data-ttu-id="f079f-136">Därför är Anslutningssträngens format:</span><span class="sxs-lookup"><span data-stu-id="f079f-136">So, the connection string format is:</span></span>

    mongodb://username:password@host:port/[database]?ssl=true

<span data-ttu-id="f079f-137">Värdena för den här strängen är tillgängliga i den **anslutningssträngen** bladet som visades tidigare:</span><span class="sxs-lookup"><span data-stu-id="f079f-137">The values of this string are available in the **Connection String** blade shown earlier:</span></span>

* <span data-ttu-id="f079f-138">Användarnamn (krävs): Azure Cosmos DB kontonamn.</span><span class="sxs-lookup"><span data-stu-id="f079f-138">Username (required): Azure Cosmos DB account name.</span></span>
* <span data-ttu-id="f079f-139">Lösenord (krävs): Azure Cosmos DB kontolösenord.</span><span class="sxs-lookup"><span data-stu-id="f079f-139">Password (required): Azure Cosmos DB account password.</span></span>
* <span data-ttu-id="f079f-140">Värd (krävs): FQDN för kontot Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f079f-140">Host (required): FQDN of the Azure Cosmos DB account.</span></span>
* <span data-ttu-id="f079f-141">Port (krävs): 10255.</span><span class="sxs-lookup"><span data-stu-id="f079f-141">Port (required): 10255.</span></span>
* <span data-ttu-id="f079f-142">Databas (valfritt): den databas som används i anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f079f-142">Database (optional): The database that the connection uses.</span></span> <span data-ttu-id="f079f-143">Om ingen databas är standarddatabasen ”test”.</span><span class="sxs-lookup"><span data-stu-id="f079f-143">If no database is provided, the default database is "test."</span></span>
* <span data-ttu-id="f079f-144">SSL = true (krävs)</span><span class="sxs-lookup"><span data-stu-id="f079f-144">ssl=true (required)</span></span>

<span data-ttu-id="f079f-145">Anta till exempel att det konto som visas i den **anslutningssträngen** bladet.</span><span class="sxs-lookup"><span data-stu-id="f079f-145">For example, consider the account shown in the **Connection String** blade.</span></span> <span data-ttu-id="f079f-146">Det är en giltig anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="f079f-146">A valid connection string is:</span></span>

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a><span data-ttu-id="f079f-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f079f-147">Next steps</span></span>
* <span data-ttu-id="f079f-148">Lär dig hur du [använder MongoChef](mongodb-mongochef.md) med en Azure Cosmos DB API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="f079f-148">Learn how to [use MongoChef](mongodb-mongochef.md) with an Azure Cosmos DB API for MongoDB account.</span></span>
* <span data-ttu-id="f079f-149">Utforska Azure Cosmos DB API för MongoDB genom att visa [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f079f-149">Explore the Azure Cosmos DB API for MongoDB by viewing [samples](mongodb-samples.md).</span></span>
