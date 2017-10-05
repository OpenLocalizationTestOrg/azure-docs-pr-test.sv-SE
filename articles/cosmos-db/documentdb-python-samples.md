---
title: "DocumentDB API Python-exempel för Azure Cosmos DB | Microsoft Docs"
description: "Hitta Python exemplen på github för vanliga uppgifter i Azure Cosmos-databasen, inklusive CRUD-åtgärder."
keywords: Python-exempel
services: cosmos-db
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2016
ms.author: moderakh
ms.openlocfilehash: d1577eeeb8fe8007394431ce70a1c7a6ee61776b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-python-examples"></a>Azure Cosmos DB Python-exempel
> [!div class="op_single_selector"]
> * [.NET-exempel](documentdb-dotnet-samples.md)
> * [Node.js-exempel](documentdb-nodejs-samples.md)
> * [Python-exempel](documentdb-python-samples.md)
> * [Azure kod exempel-galleriet](https://azure.microsoft.com/documentation/samples/?service=documentdb)
> 
> 

Exempel lösningar som utför CRUD-åtgärder och andra vanliga åtgärder på Azure DB som Cosmos-resurser som ingår i den [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub-lagringsplatsen. Den här artikeln innehåller:

* Länkar till aktiviteter i varje Python-exempel projektfiler. 
* Länkar till relaterade API: N referera till innehåll.

**Förutsättningar**

1. Du behöver ett Azure-konto du använder dessa Python-exempel:
   * Du kan [kostnadsfritt registrera ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/): du får då krediter som kan användas för att pröva Azure-betaltjänster och när du har använt upp dem så kan du behålla kontot och använda kostnadsfria Azure-tjänster, till exempel Websites. Ditt kreditkort kommer aldrig att debiteras om du inte specifikt ändrar dina inställningar och ber om det.
     * Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
2. Du måste också den [Python SDK](documentdb-sdk-python.md). 
   
   > [!NOTE]
   > Varje exempel är självständigt, den konfigurerar sig själv och rensar efter sig själv. Därför exemplen utfärda flera anrop till [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Varje gång det är klart prenumerationen kommer att debiteras för 1 timme användning per prestandanivån på samlingen som skapas. 
   > 
   > 

## <a name="database-examples"></a>Databas-exempel
Den [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) -filen för den [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projektet visar hur du utför följande uppgifter.

| Aktivitet | API-referens |
| --- | --- |
| [Skapa en databas](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Fråga ett konto för en databas](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Läs en databas-ID: t](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Lista databaser för ett konto](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Ta bort en databas](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Exempel för samlingen
Den [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) -filen för den [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projektet visar hur du utför följande uppgifter.

| Aktivitet | API-referens |
| --- | --- |
| [Skapa en samling](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Läs en lista med alla samlingar i en-databas](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) |[document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Hämta en samling med Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Hämta prestandanivån i en samling](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) |[DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Ändra nivå av prestanda i en samling](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) |[document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Ta bort en samling](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |

