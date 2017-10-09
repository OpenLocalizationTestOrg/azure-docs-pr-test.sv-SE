---
title: "aaaMove Data tooand från Azure Blob Storage | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;sachouks
ms.openlocfilehash: e12b8c157955195e826f8b108075afaf25ca7bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage"></a>Flytta data tooand från Azure Blob Storage
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Vilken metod som passar bäst för dig beror på ditt scenario. Hej [scenarier för avancerade analyser i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) artikeln hjälper dig att avgöra hello-resurser som du behöver för en mängd olika datavetenskap arbetsflöden som används i hello advanced analytics processen.

> [!NOTE]
> Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Alternativt kan du använda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) till: 

* Skapa och schemalägga en pipeline som hämtar data från Azure blob storage. 
* Överför den tooa publicerade Azure Machine Learning-webbtjänst 
* ta emot hello förutsägelseanalyser resultat och 
* Överför hello resultat toostorage. 

Mer information finns i [skapa förutsägande pipelines med hjälp av Azure Data Factory och Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Krav
Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot. Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.

* tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).

