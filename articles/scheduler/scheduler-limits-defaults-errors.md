---
title: "Schemaläggaren gränser och standardvärden"
description: "Schemaläggaren gränser och standardvärden"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: db6b1c196cb468f41c7a7ce34758de346b522abb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-limits-and-defaults"></a><span data-ttu-id="e87c5-103">Schemaläggaren gränser och standardvärden</span><span class="sxs-lookup"><span data-stu-id="e87c5-103">Scheduler Limits and Defaults</span></span>
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a><span data-ttu-id="e87c5-104">Schemaläggaren kvoter, gränser, standarder och begränsningar</span><span class="sxs-lookup"><span data-stu-id="e87c5-104">Scheduler Quotas, Limits, Defaults, and Throttles</span></span>
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a><span data-ttu-id="e87c5-105">Huvudet x-ms-begäran-id</span><span class="sxs-lookup"><span data-stu-id="e87c5-105">The x-ms-request-id Header</span></span>
<span data-ttu-id="e87c5-106">Alla begäranden som görs mot Schemaläggaren returnerar ett svarshuvud med namnet**x-ms-begäran-id**.</span><span class="sxs-lookup"><span data-stu-id="e87c5-106">Every request made against the Scheduler service returns a response header named**x-ms-request-id**.</span></span> <span data-ttu-id="e87c5-107">Det här sidhuvudet innehåller ett täckande värde som unikt identifierar begäran.</span><span class="sxs-lookup"><span data-stu-id="e87c5-107">This header contains an opaque value that uniquely identifies the request.</span></span>

<span data-ttu-id="e87c5-108">Om en begäran konsekvent misslyckas och du har verifierat att begäran är korrekt formulerade, kan du använda det här värdet rapportera felet till Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e87c5-108">If a request is consistently failing and you have verified that the request is properly formulated, you may use this value to report the error to Microsoft.</span></span> <span data-ttu-id="e87c5-109">I rapporten, inkluderar du värdet för x-ms-begäran-id, den ungefärliga tid som begäran gjordes, identifierare för prenumerationen, jobbsamlingen, och/eller jobb och typ av åtgärd som misslyckades med begäran.</span><span class="sxs-lookup"><span data-stu-id="e87c5-109">In your report, include the value of x-ms-request-id, the approximate time that the request was made, the identifier of the subscription, job collection, and/or job, and the type of operation that the request attempted.</span></span>

## <a name="see-also"></a><span data-ttu-id="e87c5-110">Se även</span><span class="sxs-lookup"><span data-stu-id="e87c5-110">See Also</span></span>
 [<span data-ttu-id="e87c5-111">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="e87c5-111">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="e87c5-112">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-112">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="e87c5-113">Komma igång med Scheduler på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e87c5-113">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="e87c5-114">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-114">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="e87c5-115">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-115">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="e87c5-116">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-116">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="e87c5-117">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-117">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="e87c5-118">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="e87c5-118">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

