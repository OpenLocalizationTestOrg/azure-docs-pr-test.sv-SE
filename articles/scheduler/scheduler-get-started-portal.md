---
title: "aaaGet igång med Azure Schemaläggaren i Azure-portalen | Microsoft Docs"
description: "Komma igång med Azure Scheduler på Azure-portalen"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="c28fb-103">Komma igång med Azure Scheduler på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c28fb-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="c28fb-104">Det är enkelt toocreate schemalagda jobb i Schemaläggaren på Azure.</span><span class="sxs-lookup"><span data-stu-id="c28fb-104">It's easy toocreate scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="c28fb-105">I den här kursen lär du dig hur toocreate ett jobb.</span><span class="sxs-lookup"><span data-stu-id="c28fb-105">In this tutorial, you'll learn how toocreate a job.</span></span> <span data-ttu-id="c28fb-106">Du lär dig också om övervaknings- och hanteringsfunktionerna i Scheduler.</span><span class="sxs-lookup"><span data-stu-id="c28fb-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="c28fb-107">Skapa ett jobb</span><span class="sxs-lookup"><span data-stu-id="c28fb-107">Create a job</span></span>
1. <span data-ttu-id="c28fb-108">Logga in för[Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c28fb-108">Sign in too[Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="c28fb-109">Klicka på **+ ny** > typen *Scheduler* i sökrutan hello > Välj **Scheduler** i resultaten > klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c28fb-109">Click **+New** > type *Scheduler* in hello search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="c28fb-110">Vi ska skapa ett jobb som bara skickar en GET-begäran mot http://www.microsoft.com/.</span><span class="sxs-lookup"><span data-stu-id="c28fb-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="c28fb-111">I hello **jobb i Schemaläggaren** anger hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c28fb-111">In hello **Scheduler Job** screen, enter hello following information:</span></span>
   
   1. <span data-ttu-id="c28fb-112">**Namn:** `getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="c28fb-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="c28fb-113">**Prenumeration:** Din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c28fb-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="c28fb-114">**Jobbsamling:** Markera en befintlig jobbsamling eller klicka på **Skapa ny** > ange ett namn.</span><span class="sxs-lookup"><span data-stu-id="c28fb-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="c28fb-115">Nästa i **åtgärdsinställningar**, definiera hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="c28fb-115">Next, in **Action Settings**, define hello following values:</span></span>
   
   1. <span data-ttu-id="c28fb-116">**Åtgärdstyp:** ` HTTP`</span><span class="sxs-lookup"><span data-stu-id="c28fb-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="c28fb-117">**Metod:** `GET`</span><span class="sxs-lookup"><span data-stu-id="c28fb-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="c28fb-118">**URL:** ` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="c28fb-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="c28fb-119">Till sist ska vi definiera ett schema.</span><span class="sxs-lookup"><span data-stu-id="c28fb-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="c28fb-120">hello jobb kan definieras som ett enstaka jobb, men väljer vi ett återkommande schema:</span><span class="sxs-lookup"><span data-stu-id="c28fb-120">hello job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="c28fb-121">**Återkommande**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="c28fb-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="c28fb-122">**Start**: Dagens datum</span><span class="sxs-lookup"><span data-stu-id="c28fb-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="c28fb-123">**Detta ska upprepas varje**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="c28fb-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="c28fb-124">**Sluta**: Två dagar från dagens datum</span><span class="sxs-lookup"><span data-stu-id="c28fb-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="c28fb-125">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="c28fb-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="c28fb-126">Hantera och övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="c28fb-126">Manage and monitor jobs</span></span>
<span data-ttu-id="c28fb-127">När ett jobb har skapats visas den i hello Azure huvudinstrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c28fb-127">Once a job is created, it appears in hello main Azure dashboard.</span></span> <span data-ttu-id="c28fb-128">Klicka på hello jobbet och en ny öppnas med hello följande flikar:</span><span class="sxs-lookup"><span data-stu-id="c28fb-128">Click hello job and a new window opens with hello following tabs:</span></span>

1. <span data-ttu-id="c28fb-129">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="c28fb-129">Properties</span></span>  
2. <span data-ttu-id="c28fb-130">Åtgärdsinställningar</span><span class="sxs-lookup"><span data-stu-id="c28fb-130">Action Settings</span></span>  
3. <span data-ttu-id="c28fb-131">Schema</span><span class="sxs-lookup"><span data-stu-id="c28fb-131">Schedule</span></span>  
4. <span data-ttu-id="c28fb-132">Historik</span><span class="sxs-lookup"><span data-stu-id="c28fb-132">History</span></span>
5. <span data-ttu-id="c28fb-133">Användare</span><span class="sxs-lookup"><span data-stu-id="c28fb-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="c28fb-134">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="c28fb-134">Properties</span></span>
<span data-ttu-id="c28fb-135">Dessa skrivskyddade egenskaper beskrivs hello management metadata för hello jobb i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="c28fb-135">These read-only properties describe hello management metadata for hello Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="c28fb-136">Åtgärdsinställningar</span><span class="sxs-lookup"><span data-stu-id="c28fb-136">Action settings</span></span>
<span data-ttu-id="c28fb-137">Klicka på ett jobb i hello **jobb** sidan kan du tooconfigure jobbet.</span><span class="sxs-lookup"><span data-stu-id="c28fb-137">Clicking on a job in hello **Jobs** screen allows you tooconfigure that job.</span></span> <span data-ttu-id="c28fb-138">På så sätt kan du konfigurera avancerade inställningar, om du inte konfigurerar dem i hello-Snabbregistrering guiden.</span><span class="sxs-lookup"><span data-stu-id="c28fb-138">This lets you configure advanced settings, if you didn't configure them in hello quick-create wizard.</span></span>

<span data-ttu-id="c28fb-139">Du kan ändra hello återförsöksprincip och hello felåtgärd för alla åtgärdstyper av.</span><span class="sxs-lookup"><span data-stu-id="c28fb-139">For all action types, you may change hello retry policy and hello error action.</span></span>

<span data-ttu-id="c28fb-140">Du kan ändra hello metoden tooany tillåten HTTP-verb för HTTP och HTTPS Jobbåtgärden typer.</span><span class="sxs-lookup"><span data-stu-id="c28fb-140">For HTTP and HTTPS job action types, you may change hello method tooany allowed HTTP verb.</span></span> <span data-ttu-id="c28fb-141">Du kan också lägga till, ta bort eller ändra hello sidhuvuden och information om grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="c28fb-141">You may also add, delete, or change hello headers and basic authentication information.</span></span>

<span data-ttu-id="c28fb-142">Du kan ändra hello lagringskonto, könamn, SAS-token och brödtext för kön åtgärd lagringstyper.</span><span class="sxs-lookup"><span data-stu-id="c28fb-142">For storage queue action types, you may change hello storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="c28fb-143">Du kan ändra hello namnområdet, avsnittet/kösökvägen, autentiseringsinställningar, transporttyp, meddelandeegenskaper och meddelandetexten för åtgärdstyper för service bus.</span><span class="sxs-lookup"><span data-stu-id="c28fb-143">For service bus action types, you may change hello namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="c28fb-144">Schema</span><span class="sxs-lookup"><span data-stu-id="c28fb-144">Schedule</span></span>
<span data-ttu-id="c28fb-145">På så sätt kan du konfigurera om hello schema, om du vill att toochange hello schema som du skapade i hello-Snabbregistrering guiden.</span><span class="sxs-lookup"><span data-stu-id="c28fb-145">This lets you reconfigure hello schedule, if you'd like toochange hello schedule you created in hello quick-create wizard.</span></span>

<span data-ttu-id="c28fb-146">Detta är en möjlighet toobuild [komplexa scheman och avancerade upprepning i jobbet](scheduler-advanced-complexity.md)</span><span class="sxs-lookup"><span data-stu-id="c28fb-146">This is an opportunity toobuild [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="c28fb-147">Du kan ändra hello startdatum och tid, återkommande schema och hello slutdatum och tid (om hello jobbet är återkommande.)</span><span class="sxs-lookup"><span data-stu-id="c28fb-147">You may change hello start date and time, recurrence schedule, and hello end date and time (if hello job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="c28fb-148">Historik</span><span class="sxs-lookup"><span data-stu-id="c28fb-148">History</span></span>
<span data-ttu-id="c28fb-149">Hej **historik** fliken visas valda måtten för varje jobbkörningen i hello system för hello valda jobbet.</span><span class="sxs-lookup"><span data-stu-id="c28fb-149">hello **History** tab displays selected metrics for every job execution in hello system for hello selected job.</span></span> <span data-ttu-id="c28fb-150">De här måtten ange realtid värden om hello hälsotillståndet för din Schemaläggaren:</span><span class="sxs-lookup"><span data-stu-id="c28fb-150">These metrics provide real-time values regarding hello health of your Scheduler:</span></span>

1. <span data-ttu-id="c28fb-151">Status</span><span class="sxs-lookup"><span data-stu-id="c28fb-151">Status</span></span>  
2. <span data-ttu-id="c28fb-152">Detaljer</span><span class="sxs-lookup"><span data-stu-id="c28fb-152">Details</span></span>  
3. <span data-ttu-id="c28fb-153">Antal återförsök</span><span class="sxs-lookup"><span data-stu-id="c28fb-153">Retry attempts</span></span>
4. <span data-ttu-id="c28fb-154">Förekomst: första, andra, tredje osv.</span><span class="sxs-lookup"><span data-stu-id="c28fb-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="c28fb-155">Starttid för körning</span><span class="sxs-lookup"><span data-stu-id="c28fb-155">Start time of execution</span></span>  
6. <span data-ttu-id="c28fb-156">Sluttid för körning</span><span class="sxs-lookup"><span data-stu-id="c28fb-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="c28fb-157">Du kan klicka på Kör tooview dess **historikinformation**, inklusive hello hela svaret för varje körning.</span><span class="sxs-lookup"><span data-stu-id="c28fb-157">You can click on a run tooview its **History Details**, including hello whole response for every execution.</span></span> <span data-ttu-id="c28fb-158">Den här dialogrutan kan du också toocopy hello svar toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="c28fb-158">This dialog box also allows you toocopy hello response toohello clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="c28fb-159">Användare</span><span class="sxs-lookup"><span data-stu-id="c28fb-159">Users</span></span>
<span data-ttu-id="c28fb-160">Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="c28fb-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="c28fb-161">toolearn hur toouse hello användare på fliken finns för[rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="c28fb-161">toolearn how toouse hello Users tab, refer too[Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="c28fb-162">Se även</span><span class="sxs-lookup"><span data-stu-id="c28fb-162">See also</span></span>
 [<span data-ttu-id="c28fb-163">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="c28fb-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="c28fb-164">Begrepp, terminologi och entitetshierarki relaterade till Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="c28fb-165">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="c28fb-166">Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="c28fb-166">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="c28fb-167">REST API-referens för Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="c28fb-168">Referens för PowerShell-cmdlets för Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="c28fb-169">Hög tillgänglighet och tillförlitlighet i Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="c28fb-170">Gränser, standardinställningar och felkoder i Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="c28fb-171">Utgående autentisering i Scheduler</span><span class="sxs-lookup"><span data-stu-id="c28fb-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
