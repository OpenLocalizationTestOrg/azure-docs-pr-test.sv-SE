---
title: "Azure CLI prover för Azure Cosmos DB | Microsoft Docs"
description: "Azure CLI-exempel - skapa och hantera Azure Cosmos DB konton, databaser, behållare, regioner och brandväggar."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 11/29/2017
ms.author: mimig
ms.openlocfilehash: 432ffc80d602a9e4eaf83fba15f0e6ebabd13603
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-exempel för Azure Cosmos DB

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

Följande tabell innehåller länkar till exempel Azure CLI-skript för Azure Cosmos DB. För referenssidor för alla Azure Cosmos DB CLI-kommandon finns i den [referens för Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Skapa Azure DB som Cosmos-kontot, databas och behållare**||
|[Skapa ett konto för SQL-API](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en enda Azure Cosmos DB API-kontot, databas och behållare för användning med SQL-API. |
| [Skapa ett konto för MongoDB-API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en enda Azure Cosmos DB MongoDB API kontot, databas och samling. |
|**Skala Azure Cosmos DB**||
| [Skala behållaren genomflöde](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Ändrar den etablerade througput i en behållare.|
|[Replikera Azure Cosmos DB databaskonto i flera områden och konfigurera redundans prioriteter](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replikerar globalt kontodata i flera regioner med angivna redundans prioritet.|
|**Skydda Azure Cosmos DB**||
| [Hämta nycklar](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hämtar den primära och sekundära master skrivning och primära och sekundära skrivskyddade nycklar för kontot.|
| [Hämta anslutningssträngen för MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hämtar anslutningssträngen till Anslut MongoDB-appen till ditt konto i Azure Cosmos DB.|
|[Återskapa nycklar](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Återskapar nyckeln master eller skrivskyddat för kontot.|
|[Skapa en brandvägg](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en inkommande IP-principer för åtkomstkontroll för att begränsa åtkomst till kontot från en godkänd uppsättning datorer eller molntjänster.|
|**Hög tillgänglighet, katastrofåterställning, säkerhetskopiering och återställning**||
|[Konfigurera principen för växling vid fel](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Anger redundans prioriteten för varje region där kontot replikeras.|
|**Ansluta Azure Cosmos DB till resurser**||
|[Ansluta en webbapp till Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Skapa och ansluta en Azure Cosmos-DB-databas och ett Azure-webbapp.|
|||
