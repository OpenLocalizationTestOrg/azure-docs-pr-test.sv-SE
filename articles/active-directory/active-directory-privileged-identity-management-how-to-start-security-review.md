---
title: "Så här startar du en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur du skapar en åtkomst-granskning för privilegierade identiteter med Azure Privileged Identity Management-programmet."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2b516e2f05aa883c5e37f5864e5ee8a2b37d3a46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="8cdc5-103">Hur du startar en åtkomst-granskning i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="8cdc5-103">How to start an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="8cdc5-104">Rolltilldelningar blir ”inaktiva” när användare privilegierad åtkomst som de inte behöver längre.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="8cdc5-105">För att minska riskerna i samband med dessa inaktuella rolltilldelningar bör Privilegierade rollen administratörer regelbundet granska de roller som användarna har fått.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-105">In order to reduce the risk associated with these stale role assignments, privileged role administrators should regularly review the roles that users have been given.</span></span> <span data-ttu-id="8cdc5-106">Det här dokumentet beskriver steg för att starta en åtkomst-granskning i Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="8cdc5-106">This document covers the steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="8cdc5-107">Starta en åtkomst-granskning</span><span class="sxs-lookup"><span data-stu-id="8cdc5-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="8cdc5-108">Om du inte har lagt till programmet PIM på instrumentpanelen i Azure-portalen, hittar du i [komma igång med Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="8cdc5-108">If you haven't added the PIM application to your dashboard in the Azure portal, see the steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="8cdc5-109">Från PIM programmet huvudsidan finns det tre sätt att starta en åtkomst-granskning:</span><span class="sxs-lookup"><span data-stu-id="8cdc5-109">From the PIM application main page, there are three ways to start an access review:</span></span>

* <span data-ttu-id="8cdc5-110">**Åtkomst till granskningar** > **Lägg till**</span><span class="sxs-lookup"><span data-stu-id="8cdc5-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="8cdc5-111">**Roller** > **granska** knappen</span><span class="sxs-lookup"><span data-stu-id="8cdc5-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="8cdc5-112">Välj den specifika rollen som ska granskas från listan över roller > **granska** knappen</span><span class="sxs-lookup"><span data-stu-id="8cdc5-112">Select the specific role to be reviewed from the roles list > **Review** button</span></span>

<span data-ttu-id="8cdc5-113">Om du klickar på den **granska** knappen, den **startar en åtkomst-granskning** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-113">When you click on the **Review** button, the **Start an access review** blade appears.</span></span> <span data-ttu-id="8cdc5-114">På det här bladet ska du konfigurera granskning med ett namn och en tid, Välj en roll för att granska och bestämma vem som utför granskningen.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-114">On this blade, you're going to configure the review with a name and time limit, choose a role to review, and decide who will perform the review.</span></span>

![Starta en åtkomst-granskning – skärmbild][1]

### <a name="configure-the-review"></a><span data-ttu-id="8cdc5-116">Konfigurera granskning</span><span class="sxs-lookup"><span data-stu-id="8cdc5-116">Configure the review</span></span>
<span data-ttu-id="8cdc5-117">Om du vill skapa en åtkomst-granskning, måste du ge den namnet och ange start-och.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-117">To create an access review, you need to name it and set a start and end date.</span></span>

![Konfigurera granskning – skärmbild][2]

<span data-ttu-id="8cdc5-119">Kontrollera längden på Granska tillräckligt länge för användare att slutföra den.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-119">Make the length of the review long enough for users to complete it.</span></span> <span data-ttu-id="8cdc5-120">Om du är klar innan slutdatum stoppar du alltid granskningen tidigt.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-120">If you finish before the end date, you can always stop the review early.</span></span>

### <a name="choose-a-role-to-review"></a><span data-ttu-id="8cdc5-121">Välj en roll för att granska</span><span class="sxs-lookup"><span data-stu-id="8cdc5-121">Choose a role to review</span></span>
<span data-ttu-id="8cdc5-122">Varje granska fokuserar på endast en roll.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-122">Each review focuses on only one role.</span></span> <span data-ttu-id="8cdc5-123">Såvida du inte startade granskningen åtkomst från en viss roll-bladet, måste du välja en roll nu.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-123">Unless you started the access review from a specific role blade, you'll need to choose a role now.</span></span>

1. <span data-ttu-id="8cdc5-124">Navigera till **granska rollmedlemskap**</span><span class="sxs-lookup"><span data-stu-id="8cdc5-124">Navigate to **Review role membership**</span></span>
   
    ![Granska rollmedlemskap – skärmbild][3]
2. <span data-ttu-id="8cdc5-126">Välj en roll i listan.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-126">Choose one role from the list.</span></span>

### <a name="decide-who-will-perform-the-review"></a><span data-ttu-id="8cdc5-127">Bestämma vem som utför granskningen</span><span class="sxs-lookup"><span data-stu-id="8cdc5-127">Decide who will perform the review</span></span>
<span data-ttu-id="8cdc5-128">Det finns tre alternativ för att utföra en granskning.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-128">There are three options for performing a review.</span></span> <span data-ttu-id="8cdc5-129">Du kan tilldela granskningen till någon annan att slutföra, så kan du göra det själv eller du kan låta användarna granska sina egna åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-129">You can assign the review to someone else to complete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="8cdc5-130">Gå till **Välj granskare**</span><span class="sxs-lookup"><span data-stu-id="8cdc5-130">Navigate to **Select reviewers**</span></span>
   
    ![Välj granskare – skärmbild][4]
2. <span data-ttu-id="8cdc5-132">Välj något av alternativen:</span><span class="sxs-lookup"><span data-stu-id="8cdc5-132">Choose one of the options:</span></span>
   
   * <span data-ttu-id="8cdc5-133">**Välj granskare**: Använd det här alternativet när du inte vet vem som behöver åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="8cdc5-134">Med det här alternativet kan du tilldela granskningen till en resursägare eller grupp manager för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-134">With this option, you can assign the review to a resource owner or group manager to complete.</span></span>
   * <span data-ttu-id="8cdc5-135">**Mig**: användbart om du vill förhandsgranska hur åtkomst granskar arbete eller du vill granska för personer som inte.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-135">**Me**: Useful if you want to preview how access reviews work, or you want to review on behalf of people who can't.</span></span>
   * <span data-ttu-id="8cdc5-136">**Medlemmar Läs själva**: Använd det här alternativet för att be användarna att granska sina egna rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-136">**Members review themselves**: Use this option to have the users review their own role assignments.</span></span>

### <a name="start-the-review"></a><span data-ttu-id="8cdc5-137">Starta granskningen</span><span class="sxs-lookup"><span data-stu-id="8cdc5-137">Start the review</span></span>
<span data-ttu-id="8cdc5-138">Slutligen har möjlighet att kräva att användare anger en orsak till om de godkänner deras åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-138">Finally, you have the option to require that users provide a reason if they approve their access.</span></span> <span data-ttu-id="8cdc5-139">Om du vill lägga till en beskrivning för granska och välj **starta**.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-139">Add a description of the review if you like, and select **Start**.</span></span>

<span data-ttu-id="8cdc5-140">Kontrollera att du ger användarna om att det finns en åtkomst-granskning väntar på dem och visa dem [hur du utför en åtkomst-granskning](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="8cdc5-140">Make sure you let your users know that there's an access review waiting for them, and show them [How to perform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-the-access-review"></a><span data-ttu-id="8cdc5-141">Hantera åtkomst-granskning</span><span class="sxs-lookup"><span data-stu-id="8cdc5-141">Manage the access review</span></span>
<span data-ttu-id="8cdc5-142">Du kan följa förloppet när granskare Slutför granskningarna i Azure AD PIM-instrumentpanelen i avsnittet åtkomst granskningar.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-142">You can track the progress as the reviewers complete their reviews in the Azure AD PIM dashboard, in the access reviews section.</span></span> <span data-ttu-id="8cdc5-143">Ingen behörighet kommer att ändras i katalogen tills [granskningen är klar](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="8cdc5-143">No access rights will be changed in the directory until [the review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="8cdc5-144">Tills omprövningsperioden är över, du påminna användare att slutföra granskning eller stoppa granska tidigt från avsnittet åtkomst granskningar.</span><span class="sxs-lookup"><span data-stu-id="8cdc5-144">Until the review period is over, you can remind users to complete their review, or stop the review early from the access reviews section.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="8cdc5-145">PIM innehållsförteckning</span><span class="sxs-lookup"><span data-stu-id="8cdc5-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
