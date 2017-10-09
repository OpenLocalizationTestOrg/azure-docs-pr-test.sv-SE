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
# <a name="move-data-tooand-from-azure-blob-storage"></a><span data-ttu-id="8d2af-103">Flytta data tooand från Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8d2af-103">Move data tooand from Azure Blob Storage</span></span>
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="8d2af-104">Vilken metod som passar bäst för dig beror på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="8d2af-104">Which method is best for you depends on your scenario.</span></span> <span data-ttu-id="8d2af-105">Hej [scenarier för avancerade analyser i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) artikeln hjälper dig att avgöra hello-resurser som du behöver för en mängd olika datavetenskap arbetsflöden som används i hello advanced analytics processen.</span><span class="sxs-lookup"><span data-stu-id="8d2af-105">hello [Scenarios for advanced analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) article helps you determine hello resources you need for a variety of data science workflows used in hello advanced analytics process.</span></span>

> [!NOTE]
> <span data-ttu-id="8d2af-106">Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d2af-106">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

<span data-ttu-id="8d2af-107">Alternativt kan du använda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) till:</span><span class="sxs-lookup"><span data-stu-id="8d2af-107">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to:</span></span> 

* <span data-ttu-id="8d2af-108">Skapa och schemalägga en pipeline som hämtar data från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8d2af-108">create and schedule a pipeline that downloads data from Azure blob storage,</span></span> 
* <span data-ttu-id="8d2af-109">Överför den tooa publicerade Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="8d2af-109">pass it tooa published Azure Machine Learning web service,</span></span> 
* <span data-ttu-id="8d2af-110">ta emot hello förutsägelseanalyser resultat och</span><span class="sxs-lookup"><span data-stu-id="8d2af-110">receive hello predictive analytics results, and</span></span> 
* <span data-ttu-id="8d2af-111">Överför hello resultat toostorage.</span><span class="sxs-lookup"><span data-stu-id="8d2af-111">upload hello results toostorage.</span></span> 

<span data-ttu-id="8d2af-112">Mer information finns i [skapa förutsägande pipelines med hjälp av Azure Data Factory och Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="8d2af-112">For more information, see [Create predictive pipelines using Azure Data Factory and Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d2af-113">Krav</span><span class="sxs-lookup"><span data-stu-id="8d2af-113">Prerequisites</span></span>
<span data-ttu-id="8d2af-114">Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot.</span><span class="sxs-lookup"><span data-stu-id="8d2af-114">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="8d2af-115">Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="8d2af-115">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="8d2af-116">tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8d2af-116">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8d2af-117">Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="8d2af-117">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

