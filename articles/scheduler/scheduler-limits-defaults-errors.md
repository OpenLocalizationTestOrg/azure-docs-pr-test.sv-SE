---
title: "aaaScheduler begränsar och är som standard"
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
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a><span data-ttu-id="ab8fd-103">Schemaläggaren gränser och standardvärden</span><span class="sxs-lookup"><span data-stu-id="ab8fd-103">Scheduler Limits and Defaults</span></span>
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a><span data-ttu-id="ab8fd-104">Schemaläggaren kvoter, gränser, standarder och begränsningar</span><span class="sxs-lookup"><span data-stu-id="ab8fd-104">Scheduler Quotas, Limits, Defaults, and Throttles</span></span>
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a><span data-ttu-id="ab8fd-105">Hej x-ms-begäran-id sidhuvud</span><span class="sxs-lookup"><span data-stu-id="ab8fd-105">hello x-ms-request-id Header</span></span>
<span data-ttu-id="ab8fd-106">Alla begäranden som görs mot hello Schemaläggaren returnerar ett svarshuvud med namnet**x-ms-begäran-id**. Det här sidhuvudet innehåller ett täckande värde som unikt identifierar hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="ab8fd-106">Every request made against hello Scheduler service returns a response header named**x-ms-request-id**. This header contains an opaque value that uniquely identifies hello request.</span></span>

<span data-ttu-id="ab8fd-107">Om en begäran är konsekvent har misslyckas och du verifierat att begäran hello korrekt formulerade, du kan använda det här värdet tooreport hello fel tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="ab8fd-107">If a request is consistently failing and you have verified that hello request is properly formulated, you may use this value tooreport hello error tooMicrosoft.</span></span> <span data-ttu-id="ab8fd-108">Inkludera i rapporten, hello värdet för x-ms-begäran-id hello ungefärliga tid hello begäran gjordes hello hello prenumeration, jobbsamlingen och/eller jobb-ID och hello typ av åtgärd som hello begäran gjordes ett försök.</span><span class="sxs-lookup"><span data-stu-id="ab8fd-108">In your report, include hello value of x-ms-request-id, hello approximate time that hello request was made, hello identifier of hello subscription, job collection, and/or job, and hello type of operation that hello request attempted.</span></span>

## <a name="see-also"></a><span data-ttu-id="ab8fd-109">Se även</span><span class="sxs-lookup"><span data-stu-id="ab8fd-109">See Also</span></span>
 [<span data-ttu-id="ab8fd-110">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="ab8fd-110">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="ab8fd-111">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-111">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="ab8fd-112">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ab8fd-112">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="ab8fd-113">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-113">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="ab8fd-114">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-114">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="ab8fd-115">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-115">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="ab8fd-116">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-116">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="ab8fd-117">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="ab8fd-117">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

