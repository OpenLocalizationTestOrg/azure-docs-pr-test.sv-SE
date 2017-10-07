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
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a>Ansluta en MongoDB programmet tooAzure Cosmos DB
Lär dig hur tooconnect din MongoDB app tooan Azure DB som Cosmos-konto med hjälp av en anslutningssträng för MongoDB. Du kan sedan använda en Azure Cosmos-DB-databas som hello data lagras för MongoDB-app. 

Den här kursen ger dig två sätt tooretrieve informationen i anslutningssträngen:

- [hello quick start-metoden](#QuickstartConnection), för användning med .NET, Node.js, MongoDB-gränssnittet, Java och Python drivrutiner
- [Hej anpassad sträng anslutningsmetod](#GetCustomConnection), för användning med andra drivrutiner

## <a name="prerequisites"></a>Krav

- Ett Azure-konto. Om du inte har ett Azure-konto kan du skapa en [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) nu. 
- Ett Azure Cosmos DB-konto. Instruktioner finns i [bygga en MongoDB API-webbapp med .NET och hello Azure-portalen](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Hämta hello MongoDB-anslutningssträngen med hjälp av hello Snabbstart
1. I en webbläsare, logga in toohello [Azure-portalen](https://portal.azure.com).
2. I hello **Azure Cosmos DB** bladet välj hello API för MongoDB-kontot. 
3. Hello vänster i hello-kontoblad klickar du på **Snabbstart**. 
4. Välj din plattform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**). Om du inte ser ditt drivrutin eller verktyg, oroa inte dig – dokumentera vi kontinuerligt mer kodfragment för anslutningen. Kommentera nedan om du vill att toosee. toolearn hur toocraft egna anslutning läsa [hämta hello konto Anslutningssträngsinformation](#GetCustomConnection).
5. Kopiera och klistra in hello kodstycke i din app för MongoDB.

    ![Snabbstart-bladet](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a>Hämta hello MongoDB anslutning sträng toocustomize
1. I en webbläsare, logga in toohello [Azure-portalen](https://portal.azure.com).
2. I hello **Azure Cosmos DB** bladet välj hello API för MongoDB-kontot. 
3. Hello vänster i hello-kontoblad klickar du på **anslutningssträngen**. 
4. Hej **anslutningssträngen** blad öppnas. Den har alla hello information behövs tooconnect toohello konto med hjälp av en drivrutin för MongoDB, inklusive en preconstructed anslutningssträng.

    ![Anslutningen sträng bladet](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Anslutningen sträng krav
> [!Important]
> Azure Cosmos-DB har stränga säkerhetskrav och standarder. Azure DB Cosmos-konton som kräver autentisering och säker kommunikation via *SSL*. 
>
>

Azure Cosmos-DB stöder hello standard MongoDB-Anslutningssträngens URI format, med några särskilda krav: Azure Cosmos DB konton som kräver autentisering och säker kommunikation via SSL. Därför är hello-Anslutningssträngens format:

    mongodb://username:password@host:port/[database]?ssl=true

hello värdena för den här strängen är tillgängliga i hello **anslutningssträngen** bladet som visades tidigare:

* Användarnamn (krävs): Azure Cosmos DB kontonamn.
* Lösenord (krävs): Azure Cosmos DB kontolösenord.
* Värd (krävs): FQDN för hello Azure DB som Cosmos-konto.
* Port (krävs): 10255.
* Databas (valfritt): hello-databasen som hello anslutningen använder. Om ingen databas är hello standarddatabasen ”test”.
* SSL = true (krävs)

Anta till exempel att hello konto hello **anslutningssträngen** bladet. Det är en giltig anslutningssträng:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[använder MongoChef](mongodb-mongochef.md) med en Azure Cosmos DB API för MongoDB-kontot.
* Utforska hello Azure Cosmos DB API för MongoDB genom att visa [exempel](mongodb-samples.md).
