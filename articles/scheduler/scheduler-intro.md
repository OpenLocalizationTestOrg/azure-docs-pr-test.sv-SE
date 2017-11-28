---
title: "Vad är Azure Scheduler? | Microsoft Docs"
description: "Med Azure Scheduler kan du deklarativt beskriva åtgärder som ska köras i molnet. Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: a3bf1aacd6978499d7ef77cbcb451a06b857ac38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-scheduler"></a><span data-ttu-id="0dd80-105">Vad är Azure Scheduler?</span><span class="sxs-lookup"><span data-stu-id="0dd80-105">What is Azure Scheduler?</span></span>
<span data-ttu-id="0dd80-106">Med Azure Scheduler kan du deklarativt beskriva åtgärder som ska köras i molnet.</span><span class="sxs-lookup"><span data-stu-id="0dd80-106">Azure Scheduler allows you to declaratively describe actions to run in the cloud.</span></span> <span data-ttu-id="0dd80-107">Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0dd80-107">It then schedules and runs those actions automatically.</span></span>  <span data-ttu-id="0dd80-108">Scheduler gör detta med hjälp av [Azure-portalen](scheduler-get-started-portal.md), kod, [REST-API:et](https://msdn.microsoft.com/library/mt629143.aspx) eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0dd80-108">Scheduler does this by using [the Azure portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), or Azure PowerShell.</span></span>

<span data-ttu-id="0dd80-109">Scheduler skapar, underhåller och anropar schemalagt arbete.</span><span class="sxs-lookup"><span data-stu-id="0dd80-109">Scheduler creates, maintains, and invokes scheduled work.</span></span>  <span data-ttu-id="0dd80-110">Scheduler är inte värd för några arbetsbelastningar och kör ingen kod.</span><span class="sxs-lookup"><span data-stu-id="0dd80-110">Scheduler does not host any workloads or run any code.</span></span> <span data-ttu-id="0dd80-111">Tjänsten *anropar* bara kod som finns på en annan plats – i Azure, lokalt eller med en annan provider.</span><span class="sxs-lookup"><span data-stu-id="0dd80-111">It only *invokes* code hosted elsewhere—in Azure, on-premises, or with another provider.</span></span> <span data-ttu-id="0dd80-112">Den anropar via HTTP, HTTPS, en lagringskö, en Service Bus-kö eller ett Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="0dd80-112">It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.</span></span>

<span data-ttu-id="0dd80-113">Scheduler schemalägger [jobb](scheduler-concepts-terms.md), sparar en historik över resultaten från jobbkörningar som du kan granska och schemalägger arbetsbelastningar som ska köras deterministiskt och med hög tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="0dd80-113">Scheduler schedules [jobs](scheduler-concepts-terms.md), keeps a history of job execution results that one can review, and deterministically and reliably schedules workloads to be run.</span></span> <span data-ttu-id="0dd80-114">Azure WebJobs (del av Web Apps-funktionen i Azure App Service) och andra schemaläggningsfunktioner i Azure använder Scheduler i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="0dd80-114">Azure WebJobs (part of the Web Apps feature in Azure App Service) and other Azure scheduling capabilities use Scheduler in the background.</span></span> <span data-ttu-id="0dd80-115">[REST-API:et för Scheduler](https://msdn.microsoft.com/library/mt629143.aspx) hjälper till att hantera kommunikationen för dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0dd80-115">The [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helps manage the communication for these actions.</span></span> <span data-ttu-id="0dd80-116">Scheduler har stöd för [komplexa scheman och avancerad upprepning](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="0dd80-116">As such, Scheduler supports [complex schedules and advanced recurrence](scheduler-advanced-complexity.md) easily.</span></span>

<span data-ttu-id="0dd80-117">Scheduler är användbart i många olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="0dd80-117">There are several scenarios that lend themselves to the usage of Scheduler.</span></span> <span data-ttu-id="0dd80-118">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0dd80-118">For example:</span></span>

* <span data-ttu-id="0dd80-119">*Åtgärder för återkommande program:* Samla in data från Twitter till en feed med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="0dd80-119">*Recurring application actions:* Periodically gathering data from Twitter into a feed.</span></span>
* <span data-ttu-id="0dd80-120">*Dagligt underhåll:* Daglig loggrensning, säkerhetskopiering och andra underhållsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0dd80-120">*Daily maintenance:* Daily pruning of logs, performing backups, and other maintenance tasks.</span></span> <span data-ttu-id="0dd80-121">En administratör kan exempelvis välja att säkerhetskopiera databasen klockan 01:00</span><span class="sxs-lookup"><span data-stu-id="0dd80-121">For example, an administrator may choose to back up the database at 1:00 A.M.</span></span> <span data-ttu-id="0dd80-122">varje dag under de kommande nio månaderna.</span><span class="sxs-lookup"><span data-stu-id="0dd80-122">every day for the next nine months.</span></span>

<span data-ttu-id="0dd80-123">Med Scheduler kan du skapa, uppdatera, ta bort, visa och hantera jobb och [jobbsamlingar](scheduler-concepts-terms.md) via programmering med hjälp av skript och på portalen.</span><span class="sxs-lookup"><span data-stu-id="0dd80-123">Scheduler allows you to create, update, delete, view, and manage jobs and [job collections](scheduler-concepts-terms.md) programmatically, by using scripts, and in the portal.</span></span>

## <a name="see-also"></a><span data-ttu-id="0dd80-124">Se även</span><span class="sxs-lookup"><span data-stu-id="0dd80-124">See also</span></span>
 [<span data-ttu-id="0dd80-125">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-125">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="0dd80-126">Komma igång med Scheduler på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0dd80-126">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="0dd80-127">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-127">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="0dd80-128">Skapa komplexa scheman och avancerad upprepning med Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-128">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="0dd80-129">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-129">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="0dd80-130">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-130">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="0dd80-131">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-131">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="0dd80-132">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-132">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="0dd80-133">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="0dd80-133">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

