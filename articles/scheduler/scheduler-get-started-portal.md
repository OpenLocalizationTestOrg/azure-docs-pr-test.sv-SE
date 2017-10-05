---
title: "Komma igång med Azure Scheduler på Azure-portalen | Microsoft-dokument"
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
ms.openlocfilehash: 3861ee121ed1c4d086ea81640e84d924d7d17ea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="abfe9-103">Komma igång med Azure Scheduler på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="abfe9-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="abfe9-104">Det är enkelt att skapa schemalagda jobb i Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="abfe9-104">It's easy to create scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="abfe9-105">I den här självstudiekursen lär du dig hur du skapar ett jobb.</span><span class="sxs-lookup"><span data-stu-id="abfe9-105">In this tutorial, you'll learn how to create a job.</span></span> <span data-ttu-id="abfe9-106">Du lär dig också om övervaknings- och hanteringsfunktionerna i Scheduler.</span><span class="sxs-lookup"><span data-stu-id="abfe9-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="abfe9-107">Skapa ett jobb</span><span class="sxs-lookup"><span data-stu-id="abfe9-107">Create a job</span></span>
1. <span data-ttu-id="abfe9-108">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="abfe9-108">Sign in to [Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="abfe9-109">Klicka på **+Nytt** > skriv *Scheduler* i sökrutan >  välj **Scheduler** i resultatet > klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="abfe9-109">Click **+New** > type *Scheduler* in the search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="abfe9-110">Vi ska skapa ett jobb som bara skickar en GET-begäran mot http://www.microsoft.com/.</span><span class="sxs-lookup"><span data-stu-id="abfe9-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="abfe9-111">Ange följande information på skärmen **Scheduler-jobb**:</span><span class="sxs-lookup"><span data-stu-id="abfe9-111">In the **Scheduler Job** screen, enter the following information:</span></span>
   
   1. <span data-ttu-id="abfe9-112">**Namn:** `getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="abfe9-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="abfe9-113">**Prenumeration:** Din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="abfe9-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="abfe9-114">**Jobbsamling:** Markera en befintlig jobbsamling eller klicka på **Skapa ny** > ange ett namn.</span><span class="sxs-lookup"><span data-stu-id="abfe9-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="abfe9-115">Nu ska du definiera följande värden i **Åtgärdsinställningar**:</span><span class="sxs-lookup"><span data-stu-id="abfe9-115">Next, in **Action Settings**, define the following values:</span></span>
   
   1. <span data-ttu-id="abfe9-116">**Åtgärdstyp:** ` HTTP`</span><span class="sxs-lookup"><span data-stu-id="abfe9-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="abfe9-117">**Metod:** `GET`</span><span class="sxs-lookup"><span data-stu-id="abfe9-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="abfe9-118">**URL:** ` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="abfe9-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="abfe9-119">Till sist ska vi definiera ett schema.</span><span class="sxs-lookup"><span data-stu-id="abfe9-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="abfe9-120">Jobbet kan definieras som ett engångsjobb, men vi väljer ett upprepningsschema:</span><span class="sxs-lookup"><span data-stu-id="abfe9-120">The job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="abfe9-121">**Återkommande**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="abfe9-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="abfe9-122">**Start**: Dagens datum</span><span class="sxs-lookup"><span data-stu-id="abfe9-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="abfe9-123">**Detta ska upprepas varje**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="abfe9-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="abfe9-124">**Sluta**: Två dagar från dagens datum</span><span class="sxs-lookup"><span data-stu-id="abfe9-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="abfe9-125">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="abfe9-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="abfe9-126">Hantera och övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="abfe9-126">Manage and monitor jobs</span></span>
<span data-ttu-id="abfe9-127">När ett jobb har skapats visas det på den primära Azure-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="abfe9-127">Once a job is created, it appears in the main Azure dashboard.</span></span> <span data-ttu-id="abfe9-128">Klicka på jobbet så öppnas ett nytt fönster med följande flikar:</span><span class="sxs-lookup"><span data-stu-id="abfe9-128">Click the job and a new window opens with the following tabs:</span></span>

1. <span data-ttu-id="abfe9-129">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="abfe9-129">Properties</span></span>  
2. <span data-ttu-id="abfe9-130">Åtgärdsinställningar</span><span class="sxs-lookup"><span data-stu-id="abfe9-130">Action Settings</span></span>  
3. <span data-ttu-id="abfe9-131">Schema</span><span class="sxs-lookup"><span data-stu-id="abfe9-131">Schedule</span></span>  
4. <span data-ttu-id="abfe9-132">Historik</span><span class="sxs-lookup"><span data-stu-id="abfe9-132">History</span></span>
5. <span data-ttu-id="abfe9-133">Användare</span><span class="sxs-lookup"><span data-stu-id="abfe9-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="abfe9-134">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="abfe9-134">Properties</span></span>
<span data-ttu-id="abfe9-135">Dessa skrivskyddade egenskaper beskriver hanteringsmetadata för Scheduler-jobbet.</span><span class="sxs-lookup"><span data-stu-id="abfe9-135">These read-only properties describe the management metadata for the Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="abfe9-136">Åtgärdsinställningar</span><span class="sxs-lookup"><span data-stu-id="abfe9-136">Action settings</span></span>
<span data-ttu-id="abfe9-137">Om du klickar på ett jobb på skärmen **Jobb** kan du konfigurera jobbet.</span><span class="sxs-lookup"><span data-stu-id="abfe9-137">Clicking on a job in the **Jobs** screen allows you to configure that job.</span></span> <span data-ttu-id="abfe9-138">Du kan konfigurera avancerade inställningar om du inte konfigurerade dem i snabbregistreringsguiden.</span><span class="sxs-lookup"><span data-stu-id="abfe9-138">This lets you configure advanced settings, if you didn't configure them in the quick-create wizard.</span></span>

<span data-ttu-id="abfe9-139">För alla åtgärdstyper kan du ändra återförsöksprincipen och felåtgärden.</span><span class="sxs-lookup"><span data-stu-id="abfe9-139">For all action types, you may change the retry policy and the error action.</span></span>

<span data-ttu-id="abfe9-140">För HTTP- och HTTPS-jobbåtgärdstyper kan du ändra metoden till valfritt tillåtet HTTP-verb.</span><span class="sxs-lookup"><span data-stu-id="abfe9-140">For HTTP and HTTPS job action types, you may change the method to any allowed HTTP verb.</span></span> <span data-ttu-id="abfe9-141">Du kan också lägga till, ta bort eller ändra sidhuvudena och grundläggande autentiseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="abfe9-141">You may also add, delete, or change the headers and basic authentication information.</span></span>

<span data-ttu-id="abfe9-142">För åtgärdstyper för lagringskön kan du ändra lagringskontot, könamnet, SAS-token och brödtexten.</span><span class="sxs-lookup"><span data-stu-id="abfe9-142">For storage queue action types, you may change the storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="abfe9-143">För Service Bus-åtgärdstyper kan du ändra namnrymden, ämnes-/kösökvägen, autentiseringsinställningarna, transporttypen, meddelandeegenskaperna och meddelandetexten.</span><span class="sxs-lookup"><span data-stu-id="abfe9-143">For service bus action types, you may change the namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="abfe9-144">Schema</span><span class="sxs-lookup"><span data-stu-id="abfe9-144">Schedule</span></span>
<span data-ttu-id="abfe9-145">Här kan du konfigurera om schemat om du vill ändra schemat som du skapade i snabbregistreringsguiden.</span><span class="sxs-lookup"><span data-stu-id="abfe9-145">This lets you reconfigure the schedule, if you'd like to change the schedule you created in the quick-create wizard.</span></span>

<span data-ttu-id="abfe9-146">Här har du möjlighet att skapa [komplexa scheman och avancerad upprepning för jobbet](scheduler-advanced-complexity.md)</span><span class="sxs-lookup"><span data-stu-id="abfe9-146">This is an opportunity to build [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="abfe9-147">Du kan ändra startdatum och starttid, upprepningsschemat och slutdatumet och sluttiden (om jobbet är återkommande).</span><span class="sxs-lookup"><span data-stu-id="abfe9-147">You may change the start date and time, recurrence schedule, and the end date and time (if the job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="abfe9-148">Historik</span><span class="sxs-lookup"><span data-stu-id="abfe9-148">History</span></span>
<span data-ttu-id="abfe9-149">Fliken **Historik** innehåller utvalda mätvärden för varje jobbkörning i systemet för det valda jobbet.</span><span class="sxs-lookup"><span data-stu-id="abfe9-149">The **History** tab displays selected metrics for every job execution in the system for the selected job.</span></span> <span data-ttu-id="abfe9-150">Dessa mätvärden är realtidsvärden som beskriver följande hälsoindikatorer i Scheduler:</span><span class="sxs-lookup"><span data-stu-id="abfe9-150">These metrics provide real-time values regarding the health of your Scheduler:</span></span>

1. <span data-ttu-id="abfe9-151">Status</span><span class="sxs-lookup"><span data-stu-id="abfe9-151">Status</span></span>  
2. <span data-ttu-id="abfe9-152">Detaljer</span><span class="sxs-lookup"><span data-stu-id="abfe9-152">Details</span></span>  
3. <span data-ttu-id="abfe9-153">Antal återförsök</span><span class="sxs-lookup"><span data-stu-id="abfe9-153">Retry attempts</span></span>
4. <span data-ttu-id="abfe9-154">Förekomst: första, andra, tredje osv.</span><span class="sxs-lookup"><span data-stu-id="abfe9-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="abfe9-155">Starttid för körning</span><span class="sxs-lookup"><span data-stu-id="abfe9-155">Start time of execution</span></span>  
6. <span data-ttu-id="abfe9-156">Sluttid för körning</span><span class="sxs-lookup"><span data-stu-id="abfe9-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="abfe9-157">Du kan klicka på en körning om du vill visa  dess **historikinformation**, inklusive hela svaret för varje körning.</span><span class="sxs-lookup"><span data-stu-id="abfe9-157">You can click on a run to view its **History Details**, including the whole response for every execution.</span></span> <span data-ttu-id="abfe9-158">I den här dialogrutan kan du också kopiera svaret till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="abfe9-158">This dialog box also allows you to copy the response to the clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="abfe9-159">Användare</span><span class="sxs-lookup"><span data-stu-id="abfe9-159">Users</span></span>
<span data-ttu-id="abfe9-160">Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="abfe9-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="abfe9-161">Information om hur du använder fliken Användare finns i [Rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="abfe9-161">To learn how to use the Users tab, refer to [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="abfe9-162">Se även</span><span class="sxs-lookup"><span data-stu-id="abfe9-162">See also</span></span>
 [<span data-ttu-id="abfe9-163">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="abfe9-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="abfe9-164">Begrepp, terminologi och entitetshierarki relaterade till Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="abfe9-165">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="abfe9-166">Skapa komplexa scheman och avancerad upprepning med Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-166">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="abfe9-167">REST API-referens för Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="abfe9-168">Referens för PowerShell-cmdlets för Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="abfe9-169">Hög tillgänglighet och tillförlitlighet i Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="abfe9-170">Gränser, standardinställningar och felkoder i Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="abfe9-171">Utgående autentisering i Scheduler</span><span class="sxs-lookup"><span data-stu-id="abfe9-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

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
