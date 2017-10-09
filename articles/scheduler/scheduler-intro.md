---
title: "aaaWhat är Azure Schemaläggaren? | Microsoft Docs"
description: "Azure Schemaläggaren kan du toodeclaratively beskrivs åtgärder toorun i hello molnet. Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt."
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
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a><span data-ttu-id="97bbf-105">Vad är Azure Scheduler?</span><span class="sxs-lookup"><span data-stu-id="97bbf-105">What is Azure Scheduler?</span></span>
<span data-ttu-id="97bbf-106">Azure Schemaläggaren kan du toodeclaratively beskrivs åtgärder toorun i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="97bbf-106">Azure Scheduler allows you toodeclaratively describe actions toorun in hello cloud.</span></span> <span data-ttu-id="97bbf-107">Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97bbf-107">It then schedules and runs those actions automatically.</span></span>  <span data-ttu-id="97bbf-108">Schemaläggaren gör detta med hjälp av [hello Azure-portalen](scheduler-get-started-portal.md), kod, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="97bbf-108">Scheduler does this by using [hello Azure portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), or Azure PowerShell.</span></span>

<span data-ttu-id="97bbf-109">Scheduler skapar, underhåller och anropar schemalagt arbete.</span><span class="sxs-lookup"><span data-stu-id="97bbf-109">Scheduler creates, maintains, and invokes scheduled work.</span></span>  <span data-ttu-id="97bbf-110">Scheduler är inte värd för några arbetsbelastningar och kör ingen kod.</span><span class="sxs-lookup"><span data-stu-id="97bbf-110">Scheduler does not host any workloads or run any code.</span></span> <span data-ttu-id="97bbf-111">Tjänsten *anropar* bara kod som finns på en annan plats – i Azure, lokalt eller med en annan provider.</span><span class="sxs-lookup"><span data-stu-id="97bbf-111">It only *invokes* code hosted elsewhere—in Azure, on-premises, or with another provider.</span></span> <span data-ttu-id="97bbf-112">Den anropar via HTTP, HTTPS, en lagringskö, en Service Bus-kö eller ett Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="97bbf-112">It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.</span></span>

<span data-ttu-id="97bbf-113">Schemaläggaren scheman [jobb](scheduler-concepts-terms.md), sparar en historik över jobb Körningsresultat att någon kan granska och deterministiskt och tillförlitligt scheman arbetsbelastningar toobe kör.</span><span class="sxs-lookup"><span data-stu-id="97bbf-113">Scheduler schedules [jobs](scheduler-concepts-terms.md), keeps a history of job execution results that one can review, and deterministically and reliably schedules workloads toobe run.</span></span> <span data-ttu-id="97bbf-114">Använda Schemaläggaren i bakgrunden hello Azure WebJobs (del av funktionen för hello Web Apps i Azure App Service) och andra funktioner i Azure schemaläggning.</span><span class="sxs-lookup"><span data-stu-id="97bbf-114">Azure WebJobs (part of hello Web Apps feature in Azure App Service) and other Azure scheduling capabilities use Scheduler in hello background.</span></span> <span data-ttu-id="97bbf-115">Hej [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) hjälper dig att hantera hello kommunikation för dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="97bbf-115">hello [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helps manage hello communication for these actions.</span></span> <span data-ttu-id="97bbf-116">Scheduler har stöd för [komplexa scheman och avancerad upprepning](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="97bbf-116">As such, Scheduler supports [complex schedules and advanced recurrence](scheduler-advanced-complexity.md) easily.</span></span>

<span data-ttu-id="97bbf-117">Det finns flera scenarier som lämpar sig toohello användning av Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="97bbf-117">There are several scenarios that lend themselves toohello usage of Scheduler.</span></span> <span data-ttu-id="97bbf-118">Exempel:</span><span class="sxs-lookup"><span data-stu-id="97bbf-118">For example:</span></span>

* <span data-ttu-id="97bbf-119">*Åtgärder för återkommande program:* Samla in data från Twitter till en feed med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="97bbf-119">*Recurring application actions:* Periodically gathering data from Twitter into a feed.</span></span>
* <span data-ttu-id="97bbf-120">*Dagligt underhåll:* Daglig loggrensning, säkerhetskopiering och andra underhållsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="97bbf-120">*Daily maintenance:* Daily pruning of logs, performing backups, and other maintenance tasks.</span></span> <span data-ttu-id="97bbf-121">En administratör kan till exempel välja tooback hello databasen klockan 1:00</span><span class="sxs-lookup"><span data-stu-id="97bbf-121">For example, an administrator may choose tooback up hello database at 1:00 A.M.</span></span> <span data-ttu-id="97bbf-122">varje dag för hello nästa nio månader.</span><span class="sxs-lookup"><span data-stu-id="97bbf-122">every day for hello next nine months.</span></span>

<span data-ttu-id="97bbf-123">Schemaläggaren kan du toocreate, uppdatera, ta bort, visa och hantera jobb och [jobbet samlingar](scheduler-concepts-terms.md) programmässigt med hjälp av skript och hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="97bbf-123">Scheduler allows you toocreate, update, delete, view, and manage jobs and [job collections](scheduler-concepts-terms.md) programmatically, by using scripts, and in hello portal.</span></span>

## <a name="see-also"></a><span data-ttu-id="97bbf-124">Se även</span><span class="sxs-lookup"><span data-stu-id="97bbf-124">See also</span></span>
 [<span data-ttu-id="97bbf-125">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-125">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="97bbf-126">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97bbf-126">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="97bbf-127">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-127">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="97bbf-128">Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="97bbf-128">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="97bbf-129">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-129">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="97bbf-130">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-130">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="97bbf-131">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-131">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="97bbf-132">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-132">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="97bbf-133">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97bbf-133">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

