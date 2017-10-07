---
title: "aaaAzure CLI prover för Azure Cosmos DB | Microsoft Docs"
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
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: d6eefc3274e0b66eec4e69166bb7d4ddd58a522e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-exempel för Azure Cosmos DB

hello följande tabell innehåller länkar toosample Azure CLI-skript för Azure Cosmos DB. För referenssidor för alla Azure Cosmos DB CLI-kommandon finns i hello [referens för Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Skapa Azure DB som Cosmos-kontot, databas och behållare**||
|[Skapa ett DocumentDB-API, diagram och tabellen API-konto](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en enda Azure Cosmos DB API-kontot, databas och behållare för användning med hello DocumentDB diagrammet eller API: er för tabellen. |
| [Skapa ett konto för MongoDB-API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en enda Azure Cosmos DB MongoDB API kontot, databas och samling. |
|**Skala Azure Cosmos DB**||
| [Skala behållaren genomflöde](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Ändringar hello etableras througput i en behållare.|
|[Replikera Azure Cosmos DB databaskonto i flera områden och konfigurera redundans prioriteter](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replikerar globalt kontodata i flera regioner med angivna redundans prioritet.|
|**Skydda Azure Cosmos DB**||
| [Hämta nycklar](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hämtar hello primära och sekundära master skrivning och primära och sekundära skrivskyddade nycklar för hello-kontot.|
| [Hämta anslutningssträngen för MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hämtar hello anslutning sträng tooconnect din MongoDB app tooyour Azure DB som Cosmos-konto.|
|[Återskapa nycklar](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Återskapar hello master eller skrivskyddad nyckel för hello-konto.|
|[Skapa en brandvägg](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar ett inkommande IP-access control princip toolimit toohello åtkomstkonto från godkända uppsättning datorer eller molntjänster.|
|**Hög tillgänglighet, katastrofåterställning, säkerhetskopiering och återställning**||
|[Konfigurera principen för växling vid fel](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Anger hello redundans prioritet för varje region där hello-konto replikeras.|
|**Ansluta Azure Cosmos DB till resurser**||
|[Ansluta en web app tooAzure Cosmos DB](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|Skapa och ansluta en Azure Cosmos-DB-databas och ett Azure-webbapp.|
|||
