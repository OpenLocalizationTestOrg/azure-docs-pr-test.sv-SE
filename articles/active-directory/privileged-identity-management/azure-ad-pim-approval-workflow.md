---
title: "Azure Privileged Identity Management godkännande arbetsflöden | Microsoft Docs"
description: "Lär dig mer om godkännandearbetsflöden i Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: cf6a9213fa0a1cba8725aabb42abe51b805ece7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="1cafc-103">Godkännanden (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="1cafc-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="1cafc-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1cafc-104">Overview</span></span>

<span data-ttu-id="1cafc-105">Med godkännanden för Privileged Identity Management kan du konfigurera roller för att kräva godkännande för aktivering och välja en eller flera användare eller grupper som delegerad godkännare.</span><span class="sxs-lookup"><span data-stu-id="1cafc-105">With Approvals for Privileged Identity Management, you can configure roles to require approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="1cafc-106">Vill du fortsätta läsa information om hur du konfigurerar roller och välj godkännare.</span><span class="sxs-lookup"><span data-stu-id="1cafc-106">Keep reading to learn how to configure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="1cafc-107">Kom ihåg den här funktionen är fortfarande under utveckling och du kan stöta på buggar.</span><span class="sxs-lookup"><span data-stu-id="1cafc-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="1cafc-108">Funktioner, inklusive text namngivningskonventioner komma att ändras och ska inte betraktas som sista.</span><span class="sxs-lookup"><span data-stu-id="1cafc-108">The functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="1cafc-109">Viktiga termer</span><span class="sxs-lookup"><span data-stu-id="1cafc-109">Key Terminology</span></span>

<span data-ttu-id="1cafc-110">*Behörig användare i rollen* – en användare med berättigade roll är en användare inom din organisation som har tilldelats till en Azure AD-roll som kvalificerade (rollen kräver aktivering).</span><span class="sxs-lookup"><span data-stu-id="1cafc-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned to an Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="1cafc-111">*Delegerad godkännare* – en delegerad godkännare är en eller flera personer eller grupper i din Azure AD som ansvarar för att godkänna begäranden om rollaktivering.</span><span class="sxs-lookup"><span data-stu-id="1cafc-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="1cafc-112">Scenarier</span><span class="sxs-lookup"><span data-stu-id="1cafc-112">Scenarios</span></span>

<span data-ttu-id="1cafc-113">Privata förhandsgranskningen stöder följande scenarion:</span><span class="sxs-lookup"><span data-stu-id="1cafc-113">The private preview supports the following scenarios:</span></span>

<span data-ttu-id="1cafc-114">**Som en privilegierade rollen Administratör (PRA) kan du:**</span><span class="sxs-lookup"><span data-stu-id="1cafc-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="1cafc-115">aktivera godkännande för specifika roller</span><span class="sxs-lookup"><span data-stu-id="1cafc-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="1cafc-116">Ange godkännare användare och/eller grupper för att godkänna begäranden</span><span class="sxs-lookup"><span data-stu-id="1cafc-116">specify approver users and/or groups to approve requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="1cafc-117">Visa historiken för begäran och godkännande för alla Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="1cafc-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="1cafc-118">**Som en utsedda användare kan du:**</span><span class="sxs-lookup"><span data-stu-id="1cafc-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="1cafc-119">Visa väntande godkännanden (antal begäranden)</span><span class="sxs-lookup"><span data-stu-id="1cafc-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="1cafc-120">godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)</span><span class="sxs-lookup"><span data-stu-id="1cafc-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="1cafc-121">Ange en motivering till min godkännande/nekande</span><span class="sxs-lookup"><span data-stu-id="1cafc-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="1cafc-122">**Som en behörig användare i rollen kan du:**</span><span class="sxs-lookup"><span data-stu-id="1cafc-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="1cafc-123">begära aktivering av en roll som kräver godkännande</span><span class="sxs-lookup"><span data-stu-id="1cafc-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="1cafc-124">Visa status för din begäran om att aktivera</span><span class="sxs-lookup"><span data-stu-id="1cafc-124">view the status of your request to activate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="1cafc-125">slutföra uppgiften i Azure AD om aktivering har godkänts</span><span class="sxs-lookup"><span data-stu-id="1cafc-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="1cafc-126">Navigering</span><span class="sxs-lookup"><span data-stu-id="1cafc-126">Navigation</span></span>

<span data-ttu-id="1cafc-127">Vi har uppdaterat navigering för att stödja godkännanden</span><span class="sxs-lookup"><span data-stu-id="1cafc-127">We've updated the navigation to support approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="1cafc-128">Standard landningssida ger åtkomst till information om PIM och den nya godkännande-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="1cafc-128">The default landing page provides convenient access to information about PIM and the new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="1cafc-129">Vi har lagt till ett nytt avsnitt för alla användare av PIM Mina granskningshistorik.</span><span class="sxs-lookup"><span data-stu-id="1cafc-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="1cafc-130">Här hittar du alla uppgifter som är relevanta för din identitet.</span><span class="sxs-lookup"><span data-stu-id="1cafc-130">Here you can find all the information relevant to your identity.</span></span> <span data-ttu-id="1cafc-131">Här ingår alla väntande och slutförda begäranden, alla beslut som du har gjort om begäranden som du har löst och alla dina senaste roll-aktiveringar i en och samma plats.</span><span class="sxs-lookup"><span data-stu-id="1cafc-131">This includes all your pending and completed requests, any decisions you’ve made about the requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="1cafc-132">Aktivera godkännande för specifika roller</span><span class="sxs-lookup"><span data-stu-id="1cafc-132">Enable approval for specific roles</span></span>

<span data-ttu-id="1cafc-133">Om du vill aktivera godkännande för en viss roll, Välj Directory roller från det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="1cafc-133">To enable approval for a specific role, first select Directory Roles from the left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="1cafc-134">Sök efter och välj inställningar i det vänstra navigeringsfönstret i katalogen</span><span class="sxs-lookup"><span data-stu-id="1cafc-134">Find and select settings in the Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="1cafc-135">Välj Privilegierade roller:</span><span class="sxs-lookup"><span data-stu-id="1cafc-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="1cafc-136">Välj ”Aktivera” i den kräver godkännande av:</span><span class="sxs-lookup"><span data-stu-id="1cafc-136">Select “Enable” in the Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="1cafc-137">När du har aktiverat, expanderar bladet för att visa följande information:</span><span class="sxs-lookup"><span data-stu-id="1cafc-137">Once enabled, the blade will expand to show the following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="1cafc-138">Om du inte anger någon godkännare blir PRA(s) standard godkännare.</span><span class="sxs-lookup"><span data-stu-id="1cafc-138">If you DO NOT specify any approvers, the PRA(s) become the default approver(s).</span></span> <span data-ttu-id="1cafc-139">PRA(s) krävs för att godkänna alla aktiveringsbegäranden för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="1cafc-139">PRA(s) would be required to approve ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a><span data-ttu-id="1cafc-140">Ange godkännare användare och/eller grupper för att godkänna begäranden</span><span class="sxs-lookup"><span data-stu-id="1cafc-140">Specify approver users and/or groups to approve requests</span></span>

<span data-ttu-id="1cafc-141">Klicka på alternativet för ”Välj godkännare” om du vill delegera godkännande:</span><span class="sxs-lookup"><span data-stu-id="1cafc-141">To delegate approval, click the option to “Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="1cafc-142">När bladet välj godkännare läses in, kan du söka efter en specifik användare eller grupp med sökfältet längst upp och att välja i listan i förväg och klicka på ”Välj” när du är klar:</span><span class="sxs-lookup"><span data-stu-id="1cafc-142">When the Select approvers blade loads, you may search for a specific user or group using the search bar at the top, or selecting from the pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="1cafc-143">Obs: Du kan välja flera användare eller grupper i taget.</span><span class="sxs-lookup"><span data-stu-id="1cafc-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="1cafc-144">Valet visas i listan över valda godkännare som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="1cafc-144">Your selection will appear in the list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="1cafc-145">Om du vill ta bort en granskare, klickar du på knappen Ta bort bredvid användarens namn.</span><span class="sxs-lookup"><span data-stu-id="1cafc-145">To remove an approver, simply click the Remove button next to their name.</span></span>

<span data-ttu-id="1cafc-146">Upprepa proceduren för att lägga till ytterligare godkännare.</span><span class="sxs-lookup"><span data-stu-id="1cafc-146">To add additional approvers, repeat the process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="1cafc-147">Visa historiken för begäran och godkännande för alla Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="1cafc-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="1cafc-148">Om du vill visa historik för begäran och godkännande för alla Privilegierade roller, Välj granskningshistorik från instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="1cafc-148">To view request and approval history for all privileged roles, select Audit History from the dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="1cafc-149">Du kan sortera data av åtgärden och leta efter ”aktivering godkända”</span><span class="sxs-lookup"><span data-stu-id="1cafc-149">You can sort the data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="1cafc-150">Visa väntande godkännanden (antal begäranden)</span><span class="sxs-lookup"><span data-stu-id="1cafc-150">View pending approvals (requests)</span></span>

<span data-ttu-id="1cafc-151">Som en delegerad godkännare får du e-postaviseringar när en begäran väntar på ditt godkännande.</span><span class="sxs-lookup"><span data-stu-id="1cafc-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="1cafc-152">Välj fliken ”väntande godkännandebegäranden” i det vänstra navigeringsfältet från instrumentpanelen (i det nya navigeringsfältet) om du vill visa dessa begäranden i PIM-portalen.</span><span class="sxs-lookup"><span data-stu-id="1cafc-152">To view these requests in the PIM portal, from the dashboard (in the new navigation) select the “Pending Approval Requests” tab in the left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="1cafc-153">Därifrån visas en lista med begäranden som väntar på godkännande:</span><span class="sxs-lookup"><span data-stu-id="1cafc-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="1cafc-154">Godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)</span><span class="sxs-lookup"><span data-stu-id="1cafc-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="1cafc-155">Välj de begäranden som du vill godkänna eller neka och klicka på knappen i Åtgärdsfältet som motsvarar ditt beslut:</span><span class="sxs-lookup"><span data-stu-id="1cafc-155">Select the requests you wish to approve or deny, and click the button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="1cafc-156">Ange en motivering till min godkännande/nekande</span><span class="sxs-lookup"><span data-stu-id="1cafc-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="1cafc-157">Då öppnas ett nytt blad om du vill godkänna eller neka flera begäranden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1cafc-157">This will open a new blade to approve or deny multiple requests at once.</span></span> <span data-ttu-id="1cafc-158">Ange en motivering för ditt beslut och klicka på Godkänn (eller neka) längst ned eller bladet:</span><span class="sxs-lookup"><span data-stu-id="1cafc-158">Enter a justification for your decision, and click approve (or deny) at the bottom or the blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="1cafc-159">När processen för begäran är klar Statussymbolen återspeglas beslut som du gjort (i det här exemplet beslutet är godkänna):</span><span class="sxs-lookup"><span data-stu-id="1cafc-159">When the request process is complete, the status symbol will reflect the decision you made (in this example, the decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="1cafc-160">Begära aktivering av en roll som kräver godkännande</span><span class="sxs-lookup"><span data-stu-id="1cafc-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="1cafc-161">Begär aktivering av en roll som kräver godkännande kan initieras från den gamla PIM-navigeringen eller nya navigering som processen för rollaktivering är densamma.</span><span class="sxs-lookup"><span data-stu-id="1cafc-161">Requesting activation of a role that requires approval may be initiated from either the old PIM navigation, or the new navigation, as the process for role activation remains the same.</span></span> <span data-ttu-id="1cafc-162">Välj bara en roll i listan över roller för att aktivera:</span><span class="sxs-lookup"><span data-stu-id="1cafc-162">Simply select a role from the list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="1cafc-163">Om en privilegierad roll kräver Multi-Factor Authentication, uppmanas du att slutföra uppgiften först:</span><span class="sxs-lookup"><span data-stu-id="1cafc-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="1cafc-164">Slutföra en gång, klicka på Aktivera och ange en motivering (vid behov):</span><span class="sxs-lookup"><span data-stu-id="1cafc-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="1cafc-165">Begäranden visas ett meddelande om att begäran väntar på godkännande:</span><span class="sxs-lookup"><span data-stu-id="1cafc-165">The requestor will see a notification that the request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a><span data-ttu-id="1cafc-166">Visa status för din begäran om att aktivera</span><span class="sxs-lookup"><span data-stu-id="1cafc-166">View the status of your request to activate</span></span>

<span data-ttu-id="1cafc-167">Visa status för en väntande begäran om att aktivera måste kunna nås från den nya navigeringen.</span><span class="sxs-lookup"><span data-stu-id="1cafc-167">Viewing the status of a pending request to activate must be accessed from the new navigation.</span></span> <span data-ttu-id="1cafc-168">Välj fliken ”Mina förfrågningar” i det vänstra navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="1cafc-168">From the left navigation bar, select the “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="1cafc-169">Tillstånd för begäran som standard ”väntande”, men du kan växla mellan för att se alla eller nekade begäranden.</span><span class="sxs-lookup"><span data-stu-id="1cafc-169">The request state defaults to “Pending”, but you can toggle to see all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="1cafc-170">Slutföra uppgiften i Azure AD om aktivering har godkänts</span><span class="sxs-lookup"><span data-stu-id="1cafc-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="1cafc-171">När begäran har godkänts rollen är aktiv och du kan fortsätta med allt arbete som kräver den här rollen.</span><span class="sxs-lookup"><span data-stu-id="1cafc-171">Once the request is approved, the role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="1cafc-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1cafc-172">Next steps</span></span>

<span data-ttu-id="1cafc-173">Din feedback är viktig för oss.</span><span class="sxs-lookup"><span data-stu-id="1cafc-173">Your feedback is valuable to us.</span></span> <span data-ttu-id="1cafc-174">Gärna att dela kommentarer och feedback med oss här!</span><span class="sxs-lookup"><span data-stu-id="1cafc-174">Please feel free to share comments or feedback with us here!</span></span>
