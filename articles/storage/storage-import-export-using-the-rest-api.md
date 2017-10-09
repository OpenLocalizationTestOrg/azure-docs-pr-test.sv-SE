---
title: aaaUsing hello Azure Import/Export service REST API | Microsoft Docs
description: "Lär dig mer om toofind resurser för att använda hello Azure Import/Export service REST-API, inklusive både hur tooand referensmaterialet."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: a01c170b1bc9c2b2ce9086d39de78a39fafb2c8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-service-rest-api"></a><span data-ttu-id="ab634-103">Med hjälp av hello Azure Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="ab634-103">Using hello Azure Import/Export service REST API</span></span>

<span data-ttu-id="ab634-104">hello Microsoft Azure Import/Export service Exponerar en REST API tooenable programmässiga kontroll av import/export av jobb.</span><span class="sxs-lookup"><span data-stu-id="ab634-104">hello Microsoft Azure Import/Export service exposes a REST API tooenable programmatic control of import/export jobs.</span></span> <span data-ttu-id="ab634-105">Du kan använda hello REST API tooperform alla hello import/export-åtgärder som du kan utföra med hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ab634-105">You can use hello REST API tooperform all of hello import/export operations that you can perform with hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="ab634-106">Du kan dessutom använda hello REST API tooperform vissa detaljerade funktioner såsom frågar hello procentandel slutförande av ett jobb som inte är tillgängliga i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="ab634-106">Additionally, you can use hello REST API tooperform certain granular operations, such as querying hello percentage completion of a job, which are not currently available in hello classic portal.</span></span>

<span data-ttu-id="ab634-107">Se [med hello Microsoft Azure Import/Export service tooTransfer Data tooBlob lagring](storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello klassiska portal toocreate och hantera import och exportera jobben.</span><span class="sxs-lookup"><span data-stu-id="ab634-107">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello classic portal toocreate and manage import and export jobs.</span></span>

## <a name="service-endpoints"></a><span data-ttu-id="ab634-108">slutpunkter</span><span class="sxs-lookup"><span data-stu-id="ab634-108">Service endpoints</span></span>

<span data-ttu-id="ab634-109">hello Azure Import/Export service är en resursleverantör för Azure Resource Manager och innehåller en uppsättning REST API: er på hello efter HTTPS-slutpunkt för att hantera import/export av jobb:</span><span class="sxs-lookup"><span data-stu-id="ab634-109">hello Azure Import/Export service is a resource provider for Azure Resource Manager and provides a set of REST APIs at hello following HTTPS endpoint for managing import/export jobs:</span></span>

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a><span data-ttu-id="ab634-110">Versionshantering</span><span class="sxs-lookup"><span data-stu-id="ab634-110">Versioning</span></span>

<span data-ttu-id="ab634-111">Begäranden toohello Import/Export service måste ange hello `api-version` parametern och ange värdet för`2016-11-01`.</span><span class="sxs-lookup"><span data-stu-id="ab634-111">Requests toohello Import/Export service must specify hello `api-version` parameter and set its value too`2016-11-01`.</span></span>

## <a name="importexport-service-operations"></a><span data-ttu-id="ab634-112">Importera och exportera tjänståtgärder</span><span class="sxs-lookup"><span data-stu-id="ab634-112">Import/Export service operations</span></span>

[<span data-ttu-id="ab634-113">Skapa ett importjobb</span><span class="sxs-lookup"><span data-stu-id="ab634-113">Creating an import job</span></span>](storage-import-export-creating-an-import-job.md)

[<span data-ttu-id="ab634-114">Skapa ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="ab634-114">Creating an export job</span></span>](storage-import-export-creating-an-export-job.md)

[<span data-ttu-id="ab634-115">Hämta statusinformation för ett jobb</span><span class="sxs-lookup"><span data-stu-id="ab634-115">Retrieving state information for a job</span></span>](storage-import-export-retrieving-state-info-for-a-job.md)

[<span data-ttu-id="ab634-116">Räkna upp jobb</span><span class="sxs-lookup"><span data-stu-id="ab634-116">Enumerating jobs</span></span>](storage-import-export-enumerating-jobs.md)

[<span data-ttu-id="ab634-117">Avbryta och ta bort jobb</span><span class="sxs-lookup"><span data-stu-id="ab634-117">Cancelling and deleting jobs</span></span>](storage-import-export-cancelling-and-deleting-jobs.md)

[<span data-ttu-id="ab634-118">Säkerhetskopiering av enheten manifest</span><span class="sxs-lookup"><span data-stu-id="ab634-118">Backing Up drive manifests</span></span>](storage-import-export-backing-up-drive-manifests.md)

[<span data-ttu-id="ab634-119">Diagnostik och felåterställning av Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="ab634-119">Diagnostics and error recovery for Import/Export jobs</span></span>](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="ab634-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab634-120">Next steps</span></span>

* [<span data-ttu-id="ab634-121">Storage Import/Export REST</span><span class="sxs-lookup"><span data-stu-id="ab634-121">Storage Import/Export REST</span></span>](/rest/api/storageimportexport)
