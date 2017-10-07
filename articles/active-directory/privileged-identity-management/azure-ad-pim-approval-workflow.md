---
title: "aaaAzure Privileged Identity Management godkännande arbetsflöden | Microsoft Docs"
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
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="8206b-103">Godkännanden (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="8206b-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="8206b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8206b-104">Overview</span></span>

<span data-ttu-id="8206b-105">Med godkännanden för Privileged Identity Management kan du konfigurera roller toorequire godkännande för aktivering och välja en eller flera användare eller grupper som delegerad godkännare.</span><span class="sxs-lookup"><span data-stu-id="8206b-105">With Approvals for Privileged Identity Management, you can configure roles toorequire approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="8206b-106">Att läsa toolearn hur tooconfigure roller och välj godkännare.</span><span class="sxs-lookup"><span data-stu-id="8206b-106">Keep reading toolearn how tooconfigure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="8206b-107">Kom ihåg den här funktionen är fortfarande under utveckling och du kan stöta på buggar.</span><span class="sxs-lookup"><span data-stu-id="8206b-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="8206b-108">hello-funktioner, inklusive text namngivningskonventioner komma att ändras och ska inte betraktas som sista.</span><span class="sxs-lookup"><span data-stu-id="8206b-108">hello functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="8206b-109">Viktiga termer</span><span class="sxs-lookup"><span data-stu-id="8206b-109">Key Terminology</span></span>

<span data-ttu-id="8206b-110">*Behörig användare i rollen* – en användare med berättigade roll är en användare inom din organisation som har tilldelats rollen tooan Azure AD som kvalificerade (rollen kräver aktivering).</span><span class="sxs-lookup"><span data-stu-id="8206b-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned tooan Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="8206b-111">*Delegerad godkännare* – en delegerad godkännare är en eller flera personer eller grupper i din Azure AD som ansvarar för att godkänna begäranden om rollaktivering.</span><span class="sxs-lookup"><span data-stu-id="8206b-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="8206b-112">Scenarier</span><span class="sxs-lookup"><span data-stu-id="8206b-112">Scenarios</span></span>

<span data-ttu-id="8206b-113">hello privat förhandsversion stöder hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="8206b-113">hello private preview supports hello following scenarios:</span></span>

<span data-ttu-id="8206b-114">**Som en privilegierade rollen Administratör (PRA) kan du:**</span><span class="sxs-lookup"><span data-stu-id="8206b-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="8206b-115">aktivera godkännande för specifika roller</span><span class="sxs-lookup"><span data-stu-id="8206b-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="8206b-116">Ange godkännare användare och/eller grupper tooapprove begäranden</span><span class="sxs-lookup"><span data-stu-id="8206b-116">specify approver users and/or groups tooapprove requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="8206b-117">Visa historiken för begäran och godkännande för alla Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="8206b-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="8206b-118">**Som en utsedda användare kan du:**</span><span class="sxs-lookup"><span data-stu-id="8206b-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="8206b-119">Visa väntande godkännanden (antal begäranden)</span><span class="sxs-lookup"><span data-stu-id="8206b-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="8206b-120">godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)</span><span class="sxs-lookup"><span data-stu-id="8206b-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="8206b-121">Ange en motivering till min godkännande/nekande</span><span class="sxs-lookup"><span data-stu-id="8206b-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="8206b-122">**Som en behörig användare i rollen kan du:**</span><span class="sxs-lookup"><span data-stu-id="8206b-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="8206b-123">begära aktivering av en roll som kräver godkännande</span><span class="sxs-lookup"><span data-stu-id="8206b-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="8206b-124">Visa hello statusen för din begäran tooactivate</span><span class="sxs-lookup"><span data-stu-id="8206b-124">view hello status of your request tooactivate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="8206b-125">slutföra uppgiften i Azure AD om aktivering har godkänts</span><span class="sxs-lookup"><span data-stu-id="8206b-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="8206b-126">Navigering</span><span class="sxs-lookup"><span data-stu-id="8206b-126">Navigation</span></span>

<span data-ttu-id="8206b-127">Vi har uppdaterat hello navigering toosupport godkännanden</span><span class="sxs-lookup"><span data-stu-id="8206b-127">We've updated hello navigation toosupport approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="8206b-128">hello standardsida ger åtkomst tooinformation om PIM och hello nya godkännanden-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="8206b-128">hello default landing page provides convenient access tooinformation about PIM and hello new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="8206b-129">Vi har lagt till ett nytt avsnitt för alla användare av PIM Mina granskningshistorik.</span><span class="sxs-lookup"><span data-stu-id="8206b-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="8206b-130">Här hittar du alla hello information relevanta tooyour identitet.</span><span class="sxs-lookup"><span data-stu-id="8206b-130">Here you can find all hello information relevant tooyour identity.</span></span> <span data-ttu-id="8206b-131">Här ingår alla väntande och slutförda begäranden, alla beslut som du har gjort om hello förfrågningar om du har löst och alla dina senaste roll-aktiveringar i en och samma plats.</span><span class="sxs-lookup"><span data-stu-id="8206b-131">This includes all your pending and completed requests, any decisions you’ve made about hello requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="8206b-132">Aktivera godkännande för specifika roller</span><span class="sxs-lookup"><span data-stu-id="8206b-132">Enable approval for specific roles</span></span>

<span data-ttu-id="8206b-133">tooenable godkännande för en specifik roll, väljer först Directory roller hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="8206b-133">tooenable approval for a specific role, first select Directory Roles from hello left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="8206b-134">Hitta och välja inställningar i hello navigeringen till vänster i katalogen</span><span class="sxs-lookup"><span data-stu-id="8206b-134">Find and select settings in hello Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="8206b-135">Välj Privilegierade roller:</span><span class="sxs-lookup"><span data-stu-id="8206b-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="8206b-136">Välj ”Aktivera” i hello kräver godkännande av:</span><span class="sxs-lookup"><span data-stu-id="8206b-136">Select “Enable” in hello Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="8206b-137">När du har aktiverat, kommer hello bladet Expandera tooshow hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8206b-137">Once enabled, hello blade will expand tooshow hello following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="8206b-138">Om du inte anger någon godkännare bli hello PRA(s) hello standard godkännare.</span><span class="sxs-lookup"><span data-stu-id="8206b-138">If you DO NOT specify any approvers, hello PRA(s) become hello default approver(s).</span></span> <span data-ttu-id="8206b-139">PRA(s) skulle vara nödvändiga tooapprove alla aktivering begäranden för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="8206b-139">PRA(s) would be required tooapprove ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a><span data-ttu-id="8206b-140">Ange godkännare användare och/eller grupper tooapprove begäranden</span><span class="sxs-lookup"><span data-stu-id="8206b-140">Specify approver users and/or groups tooapprove requests</span></span>

<span data-ttu-id="8206b-141">toodelegate godkännande på hello alternativ för ”Välj godkännare”:</span><span class="sxs-lookup"><span data-stu-id="8206b-141">toodelegate approval, click hello option too“Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="8206b-142">När hello väljer godkännare bladet läses in, kan du söka efter en viss användare eller grupp med hello sökfältet vid hello top eller välja hello förifyllda listan och klicka på ”Välj” när du är klar:</span><span class="sxs-lookup"><span data-stu-id="8206b-142">When hello Select approvers blade loads, you may search for a specific user or group using hello search bar at hello top, or selecting from hello pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="8206b-143">Obs: Du kan välja flera användare eller grupper i taget.</span><span class="sxs-lookup"><span data-stu-id="8206b-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="8206b-144">Valet visas i valda hello listan som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="8206b-144">Your selection will appear in hello list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="8206b-145">tooremove godkännare, klicka bara på hello ta bort knappen Nästa tootheir namn.</span><span class="sxs-lookup"><span data-stu-id="8206b-145">tooremove an approver, simply click hello Remove button next tootheir name.</span></span>

<span data-ttu-id="8206b-146">tooadd ytterligare godkännare, upprepa hello-processen.</span><span class="sxs-lookup"><span data-stu-id="8206b-146">tooadd additional approvers, repeat hello process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="8206b-147">Visa historiken för begäran och godkännande för alla Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="8206b-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="8206b-148">Historik för tooview begäran och godkännande för alla Privilegierade roller, Välj granskningshistorik hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="8206b-148">tooview request and approval history for all privileged roles, select Audit History from hello dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="8206b-149">Du kan sortera hello data av åtgärden och leta efter ”aktivering godkända”</span><span class="sxs-lookup"><span data-stu-id="8206b-149">You can sort hello data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="8206b-150">Visa väntande godkännanden (antal begäranden)</span><span class="sxs-lookup"><span data-stu-id="8206b-150">View pending approvals (requests)</span></span>

<span data-ttu-id="8206b-151">Som en delegerad godkännare får du e-postaviseringar när en begäran väntar på ditt godkännande.</span><span class="sxs-lookup"><span data-stu-id="8206b-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="8206b-152">tooview dessa begäranden i hello PIM-portalen från fliken instrumentpanelen (i hello nya navigering) Välj hello ”väntande godkännandebegäranden” i hello navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="8206b-152">tooview these requests in hello PIM portal, from the dashboard (in hello new navigation) select hello “Pending Approval Requests” tab in hello left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="8206b-153">Därifrån visas en lista med begäranden som väntar på godkännande:</span><span class="sxs-lookup"><span data-stu-id="8206b-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="8206b-154">Godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)</span><span class="sxs-lookup"><span data-stu-id="8206b-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="8206b-155">Välj hello begäranden om du vill tooapprove eller neka och klicka hello i Åtgärdsfältet som motsvarar ditt beslut:</span><span class="sxs-lookup"><span data-stu-id="8206b-155">Select hello requests you wish tooapprove or deny, and click hello button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="8206b-156">Ange en motivering till min godkännande/nekande</span><span class="sxs-lookup"><span data-stu-id="8206b-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="8206b-157">Det här öppna ett nytt blad tooapprove eller neka flera begäranden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8206b-157">This will open a new blade tooapprove or deny multiple requests at once.</span></span> <span data-ttu-id="8206b-158">Ange en motivering för ditt beslut och klicka på Godkänn (eller neka) på hello ned eller hello blad:</span><span class="sxs-lookup"><span data-stu-id="8206b-158">Enter a justification for your decision, and click approve (or deny) at hello bottom or hello blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="8206b-159">När hello begäran processen är klar hello Statussymbolen återspeglas beslut som du gjort (i det här exemplet hello beslut är godkänna):</span><span class="sxs-lookup"><span data-stu-id="8206b-159">When hello request process is complete, hello status symbol will reflect the decision you made (in this example, hello decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="8206b-160">Begära aktivering av en roll som kräver godkännande</span><span class="sxs-lookup"><span data-stu-id="8206b-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="8206b-161">Begär aktivering av en roll som kräver godkännande kan initieras från hello gamla PIM navigering eller hello nya navigering hello process rollen aktivering fortfarande hello samma.</span><span class="sxs-lookup"><span data-stu-id="8206b-161">Requesting activation of a role that requires approval may be initiated from either hello old PIM navigation, or hello new navigation, as hello process for role activation remains hello same.</span></span> <span data-ttu-id="8206b-162">Välj bara en roll hello listan över roller för att aktivera:</span><span class="sxs-lookup"><span data-stu-id="8206b-162">Simply select a role from hello list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="8206b-163">Om en privilegierad roll kräver Multi-Factor Authentication, uppmanas du att slutföra uppgiften först:</span><span class="sxs-lookup"><span data-stu-id="8206b-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="8206b-164">Slutföra en gång, klicka på Aktivera och ange en motivering (vid behov):</span><span class="sxs-lookup"><span data-stu-id="8206b-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="8206b-165">hello begärande visas ett meddelande om att hello begäran väntar på godkännande:</span><span class="sxs-lookup"><span data-stu-id="8206b-165">hello requestor will see a notification that hello request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a><span data-ttu-id="8206b-166">Visa hello status för din begäran tooactivate</span><span class="sxs-lookup"><span data-stu-id="8206b-166">View hello status of your request tooactivate</span></span>

<span data-ttu-id="8206b-167">Visa hello status för en väntande begäran tooactivate måste kunna nås från den nya navigeringen.</span><span class="sxs-lookup"><span data-stu-id="8206b-167">Viewing hello status of a pending request tooactivate must be accessed from the new navigation.</span></span> <span data-ttu-id="8206b-168">Välj hello ”Mina förfrågningar” fliken hello vänstra navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="8206b-168">From hello left navigation bar, select hello “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="8206b-169">tillstånd för begäran om hello standardvärden för ”väntande”, men du kan växla toosee alla eller nekas begäranden.</span><span class="sxs-lookup"><span data-stu-id="8206b-169">hello request state defaults too“Pending”, but you can toggle toosee all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="8206b-170">Slutföra uppgiften i Azure AD om aktivering har godkänts</span><span class="sxs-lookup"><span data-stu-id="8206b-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="8206b-171">När hello begäran har godkänts, hello roll är aktiv och du kan fortsätta med allt arbete som kräver den här rollen.</span><span class="sxs-lookup"><span data-stu-id="8206b-171">Once hello request is approved, hello role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="8206b-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8206b-172">Next steps</span></span>

<span data-ttu-id="8206b-173">Din feedback är viktig toous.</span><span class="sxs-lookup"><span data-stu-id="8206b-173">Your feedback is valuable toous.</span></span> <span data-ttu-id="8206b-174">Välkommen ledigt tooshare kommentarer och feedback med oss här!</span><span class="sxs-lookup"><span data-stu-id="8206b-174">Please feel free tooshare comments or feedback with us here!</span></span>
