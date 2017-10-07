---
title: "aaaMongoDB anslutningssträngen för ett konto i Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur tooconnect din MongoDB app tooan Azure DB som Cosmos-konto med hjälp av en anslutningssträng för MongoDB."
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
ms.openlocfilehash: c0b81cb49a10e09e3f02411b91731c7f980ec47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a><span data-ttu-id="224cf-104">Ansluta en MongoDB programmet tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="224cf-104">Connect a MongoDB application tooAzure Cosmos DB</span></span>
<span data-ttu-id="224cf-105">Lär dig hur tooconnect din MongoDB app tooan Azure DB som Cosmos-konto med hjälp av en anslutningssträng för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="224cf-105">Learn how tooconnect your MongoDB app tooan Azure Cosmos DB account by using a MongoDB connection string.</span></span> <span data-ttu-id="224cf-106">Du kan sedan använda en Azure Cosmos-DB-databas som hello data lagras för MongoDB-app.</span><span class="sxs-lookup"><span data-stu-id="224cf-106">You can then use an Azure Cosmos DB database as hello data store for your MongoDB app.</span></span> 

<span data-ttu-id="224cf-107">Den här kursen ger dig två sätt tooretrieve informationen i anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="224cf-107">This tutorial provides two ways tooretrieve connection string information:</span></span>

- <span data-ttu-id="224cf-108">[hello quick start-metoden](#QuickstartConnection), för användning med .NET, Node.js, MongoDB-gränssnittet, Java och Python drivrutiner</span><span class="sxs-lookup"><span data-stu-id="224cf-108">[hello quick start method](#QuickstartConnection), for use with .NET, Node.js, MongoDB Shell, Java, and Python drivers</span></span>
- <span data-ttu-id="224cf-109">[Hej anpassad sträng anslutningsmetod](#GetCustomConnection), för användning med andra drivrutiner</span><span class="sxs-lookup"><span data-stu-id="224cf-109">[hello custom connection string method](#GetCustomConnection), for use with other drivers</span></span>

## <a name="prerequisites"></a><span data-ttu-id="224cf-110">Krav</span><span class="sxs-lookup"><span data-stu-id="224cf-110">Prerequisites</span></span>

- <span data-ttu-id="224cf-111">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="224cf-111">An Azure account.</span></span> <span data-ttu-id="224cf-112">Om du inte har ett Azure-konto kan du skapa en [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) nu.</span><span class="sxs-lookup"><span data-stu-id="224cf-112">If you don't have an Azure account, create a [free Azure account](https://azure.microsoft.com/free/) now.</span></span> 
- <span data-ttu-id="224cf-113">Ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="224cf-113">An Azure Cosmos DB account.</span></span> <span data-ttu-id="224cf-114">Instruktioner finns i [bygga en MongoDB API-webbapp med .NET och hello Azure-portalen](create-mongodb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="224cf-114">For instructions, see [Build a MongoDB API web app with .NET and hello Azure portal](create-mongodb-dotnet.md).</span></span>

## <span data-ttu-id="224cf-115"><a id="QuickstartConnection"></a>Hämta hello MongoDB-anslutningssträngen med hjälp av hello Snabbstart</span><span class="sxs-lookup"><span data-stu-id="224cf-115"><a id="QuickstartConnection"></a>Get hello MongoDB connection string by using hello quick start</span></span>
1. <span data-ttu-id="224cf-116">I en webbläsare, logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="224cf-116">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="224cf-117">I hello **Azure Cosmos DB** bladet välj hello API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="224cf-117">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="224cf-118">Hello vänster i hello-kontoblad klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="224cf-118">In hello left pane of hello account blade, click **Quick start**.</span></span> 
4. <span data-ttu-id="224cf-119">Välj din plattform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span><span class="sxs-lookup"><span data-stu-id="224cf-119">Choose your platform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span></span> <span data-ttu-id="224cf-120">Om du inte ser ditt drivrutin eller verktyg, oroa inte dig – dokumentera vi kontinuerligt mer kodfragment för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="224cf-120">If you don't see your driver or tool listed, don't worry--we continuously document more connection code snippets.</span></span> <span data-ttu-id="224cf-121">Kommentera nedan om du vill att toosee.</span><span class="sxs-lookup"><span data-stu-id="224cf-121">Please comment below on what you'd like toosee.</span></span> <span data-ttu-id="224cf-122">toolearn hur toocraft egna anslutning läsa [hämta hello konto Anslutningssträngsinformation](#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="224cf-122">toolearn how toocraft your own connection, read [Get hello account's connection string information](#GetCustomConnection).</span></span>
5. <span data-ttu-id="224cf-123">Kopiera och klistra in hello kodstycke i din app för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="224cf-123">Copy and paste hello code snippet into your MongoDB app.</span></span>

    ![Snabbstart-bladet](./media/connect-mongodb-account/QuickStartBlade.png)

## <span data-ttu-id="224cf-125"><a id="GetCustomConnection"></a>Hämta hello MongoDB anslutning sträng toocustomize</span><span class="sxs-lookup"><span data-stu-id="224cf-125"><a id="GetCustomConnection"></a> Get hello MongoDB connection string toocustomize</span></span>
1. <span data-ttu-id="224cf-126">I en webbläsare, logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="224cf-126">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="224cf-127">I hello **Azure Cosmos DB** bladet välj hello API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="224cf-127">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="224cf-128">Hello vänster i hello-kontoblad klickar du på **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="224cf-128">In hello left pane of hello account blade, click **Connection String**.</span></span> 
4. <span data-ttu-id="224cf-129">Hej **anslutningssträngen** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="224cf-129">hello **Connection String** blade opens.</span></span> <span data-ttu-id="224cf-130">Den har alla hello information behövs tooconnect toohello konto med hjälp av en drivrutin för MongoDB, inklusive en preconstructed anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="224cf-130">It has all hello information necessary tooconnect toohello account by using a driver for MongoDB, including a preconstructed connection string.</span></span>

    ![Anslutningen sträng bladet](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a><span data-ttu-id="224cf-132">Anslutningen sträng krav</span><span class="sxs-lookup"><span data-stu-id="224cf-132">Connection string requirements</span></span>
> [!Important]
> <span data-ttu-id="224cf-133">Azure Cosmos-DB har stränga säkerhetskrav och standarder.</span><span class="sxs-lookup"><span data-stu-id="224cf-133">Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="224cf-134">Azure DB Cosmos-konton som kräver autentisering och säker kommunikation via *SSL*.</span><span class="sxs-lookup"><span data-stu-id="224cf-134">Azure Cosmos DB accounts require authentication and secure communication via *SSL*.</span></span> 
>
>

<span data-ttu-id="224cf-135">Azure Cosmos-DB stöder hello standard MongoDB-Anslutningssträngens URI format, med några särskilda krav: Azure Cosmos DB konton som kräver autentisering och säker kommunikation via SSL.</span><span class="sxs-lookup"><span data-stu-id="224cf-135">Azure Cosmos DB supports hello standard MongoDB connection string URI format, with a couple of specific requirements: Azure Cosmos DB accounts require authentication and secure communication via SSL.</span></span> <span data-ttu-id="224cf-136">Därför är hello-Anslutningssträngens format:</span><span class="sxs-lookup"><span data-stu-id="224cf-136">So, hello connection string format is:</span></span>

    mongodb://username:password@host:port/[database]?ssl=true

<span data-ttu-id="224cf-137">hello värdena för den här strängen är tillgängliga i hello **anslutningssträngen** bladet som visades tidigare:</span><span class="sxs-lookup"><span data-stu-id="224cf-137">hello values of this string are available in hello **Connection String** blade shown earlier:</span></span>

* <span data-ttu-id="224cf-138">Användarnamn (krävs): Azure Cosmos DB kontonamn.</span><span class="sxs-lookup"><span data-stu-id="224cf-138">Username (required): Azure Cosmos DB account name.</span></span>
* <span data-ttu-id="224cf-139">Lösenord (krävs): Azure Cosmos DB kontolösenord.</span><span class="sxs-lookup"><span data-stu-id="224cf-139">Password (required): Azure Cosmos DB account password.</span></span>
* <span data-ttu-id="224cf-140">Värd (krävs): FQDN för hello Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="224cf-140">Host (required): FQDN of hello Azure Cosmos DB account.</span></span>
* <span data-ttu-id="224cf-141">Port (krävs): 10255.</span><span class="sxs-lookup"><span data-stu-id="224cf-141">Port (required): 10255.</span></span>
* <span data-ttu-id="224cf-142">Databas (valfritt): hello-databasen som hello anslutningen använder.</span><span class="sxs-lookup"><span data-stu-id="224cf-142">Database (optional): hello database that hello connection uses.</span></span> <span data-ttu-id="224cf-143">Om ingen databas är hello standarddatabasen ”test”.</span><span class="sxs-lookup"><span data-stu-id="224cf-143">If no database is provided, hello default database is "test."</span></span>
* <span data-ttu-id="224cf-144">SSL = true (krävs)</span><span class="sxs-lookup"><span data-stu-id="224cf-144">ssl=true (required)</span></span>

<span data-ttu-id="224cf-145">Anta till exempel att hello konto hello **anslutningssträngen** bladet.</span><span class="sxs-lookup"><span data-stu-id="224cf-145">For example, consider hello account shown in hello **Connection String** blade.</span></span> <span data-ttu-id="224cf-146">Det är en giltig anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="224cf-146">A valid connection string is:</span></span>

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a><span data-ttu-id="224cf-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="224cf-147">Next steps</span></span>
* <span data-ttu-id="224cf-148">Lär dig hur för[använder MongoChef](mongodb-mongochef.md) med en Azure Cosmos DB API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="224cf-148">Learn how too[use MongoChef](mongodb-mongochef.md) with an Azure Cosmos DB API for MongoDB account.</span></span>
* <span data-ttu-id="224cf-149">Utforska hello Azure Cosmos DB API för MongoDB genom att visa [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="224cf-149">Explore hello Azure Cosmos DB API for MongoDB by viewing [samples](mongodb-samples.md).</span></span>
