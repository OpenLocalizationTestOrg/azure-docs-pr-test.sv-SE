---
title: "Lista över alla dina Azure Import/Export-jobb | MicrosoftDocs"
description: "Lär dig mer om att visa en lista över Azure Import/Export service jobb i en prenumeration."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f2e619be-1bbd-4a54-9472-9e2f70a83b64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1977bfc0e516088310f45ecdd960287eeed2c2d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a><span data-ttu-id="da245-103">Uppräkning av jobb i tjänsten Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="da245-103">Enumerating jobs in the Azure Import/Export service</span></span>
<span data-ttu-id="da245-104">För att räkna upp alla jobb i en prenumeration anropar den [lista jobb](/rest/api/storageimportexport/jobs#Jobs_List) igen.</span><span class="sxs-lookup"><span data-stu-id="da245-104">To enumerate all jobs in a subscription, call the [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) operation.</span></span> <span data-ttu-id="da245-105">`List Jobs`Returnerar en lista över jobb samt följande attribut:</span><span class="sxs-lookup"><span data-stu-id="da245-105">`List Jobs` returns a list of jobs as well as the following attributes:</span></span>

-   <span data-ttu-id="da245-106">Typ av jobb (importera och exportera)</span><span class="sxs-lookup"><span data-stu-id="da245-106">The type of job (Import or Export)</span></span>

-   <span data-ttu-id="da245-107">Jobbets aktuella tillstånd</span><span class="sxs-lookup"><span data-stu-id="da245-107">The current job state</span></span>

-   <span data-ttu-id="da245-108">Projektet har kopplats till lagringskontot</span><span class="sxs-lookup"><span data-stu-id="da245-108">The job's associated storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="da245-109">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da245-109">Next steps</span></span>

* [<span data-ttu-id="da245-110">Med hjälp av tjänsten Import/Export REST API</span><span class="sxs-lookup"><span data-stu-id="da245-110">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
