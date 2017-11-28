---
title: "aaaHow toostart en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur toocreate åtkomsten granska för privilegierade identiteter med hello Azure Privileged Identity Management-program."
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
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="fc19d-103">Hur toostart åtkomst finns i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="fc19d-103">How toostart an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="fc19d-104">Rolltilldelningar blir ”inaktiva” när användare privilegierad åtkomst som de inte behöver längre.</span><span class="sxs-lookup"><span data-stu-id="fc19d-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="fc19d-105">Privilegierade rollen administratörer bör regelbundet granska hello roller som användarna har fått i ordning tooreduce hello risk som är kopplade till dessa inaktuella rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="fc19d-105">In order tooreduce hello risk associated with these stale role assignments, privileged role administrators should regularly review hello roles that users have been given.</span></span> <span data-ttu-id="fc19d-106">Det här dokumentet beskriver hello steg för att starta en åtkomst-granskning i Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="fc19d-106">This document covers hello steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="fc19d-107">Starta en åtkomst-granskning</span><span class="sxs-lookup"><span data-stu-id="fc19d-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="fc19d-108">Om du inte har lagt till hello PIM programmet tooyour instrumentpanelen i hello Azure-portalen, se hello stegen i [komma igång med Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="fc19d-108">If you haven't added hello PIM application tooyour dashboard in hello Azure portal, see hello steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="fc19d-109">Från hello PIM programmet huvudsidan finns det tre sätt toostart en åtkomst-granskning:</span><span class="sxs-lookup"><span data-stu-id="fc19d-109">From hello PIM application main page, there are three ways toostart an access review:</span></span>

* <span data-ttu-id="fc19d-110">**Åtkomst till granskningar** > **Lägg till**</span><span class="sxs-lookup"><span data-stu-id="fc19d-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="fc19d-111">**Roller** > **granska** knappen</span><span class="sxs-lookup"><span data-stu-id="fc19d-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="fc19d-112">Välj hello serverroll toobe granskat hello roller listan > **granska** knappen</span><span class="sxs-lookup"><span data-stu-id="fc19d-112">Select hello specific role toobe reviewed from hello roles list > **Review** button</span></span>

<span data-ttu-id="fc19d-113">Om du klickar på hello **granska** knappen hello **startar en åtkomst-granskning** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="fc19d-113">When you click on hello **Review** button, hello **Start an access review** blade appears.</span></span> <span data-ttu-id="fc19d-114">På det här bladet du är pågående tooconfigure hello granska med ett namn och tid gränsen, Välj en roll tooreview och bestämma vem som ska utföra hello, granska.</span><span class="sxs-lookup"><span data-stu-id="fc19d-114">On this blade, you're going tooconfigure hello review with a name and time limit, choose a role tooreview, and decide who will perform hello review.</span></span>

![Starta en åtkomst-granskning – skärmbild][1]

### <a name="configure-hello-review"></a><span data-ttu-id="fc19d-116">Konfigurera hello, granska</span><span class="sxs-lookup"><span data-stu-id="fc19d-116">Configure hello review</span></span>
<span data-ttu-id="fc19d-117">Granska toocreate-åtkomst måste du tooname den och ange en start-och slutdatum.</span><span class="sxs-lookup"><span data-stu-id="fc19d-117">toocreate an access review, you need tooname it and set a start and end date.</span></span>

![Konfigurera granskning – skärmbild][2]

<span data-ttu-id="fc19d-119">Se hello tidslängd hello granska tillräckligt länge för att användare toocomplete den.</span><span class="sxs-lookup"><span data-stu-id="fc19d-119">Make hello length of hello review long enough for users toocomplete it.</span></span> <span data-ttu-id="fc19d-120">Om du är klar innan hello slutdatum stoppar du alltid hello, granska tidigt.</span><span class="sxs-lookup"><span data-stu-id="fc19d-120">If you finish before hello end date, you can always stop hello review early.</span></span>

### <a name="choose-a-role-tooreview"></a><span data-ttu-id="fc19d-121">Välj en roll tooreview</span><span class="sxs-lookup"><span data-stu-id="fc19d-121">Choose a role tooreview</span></span>
<span data-ttu-id="fc19d-122">Varje granska fokuserar på endast en roll.</span><span class="sxs-lookup"><span data-stu-id="fc19d-122">Each review focuses on only one role.</span></span> <span data-ttu-id="fc19d-123">Såvida du inte startade hello åtkomst granska från en viss roll-bladet, behöver du toochoose en roll nu.</span><span class="sxs-lookup"><span data-stu-id="fc19d-123">Unless you started hello access review from a specific role blade, you'll need toochoose a role now.</span></span>

1. <span data-ttu-id="fc19d-124">Navigera för**granska rollmedlemskap**</span><span class="sxs-lookup"><span data-stu-id="fc19d-124">Navigate too**Review role membership**</span></span>
   
    ![Granska rollmedlemskap – skärmbild][3]
2. <span data-ttu-id="fc19d-126">Välj en roll i hello lista.</span><span class="sxs-lookup"><span data-stu-id="fc19d-126">Choose one role from hello list.</span></span>

### <a name="decide-who-will-perform-hello-review"></a><span data-ttu-id="fc19d-127">Bestämma vem som ska utföra hello, granska</span><span class="sxs-lookup"><span data-stu-id="fc19d-127">Decide who will perform hello review</span></span>
<span data-ttu-id="fc19d-128">Det finns tre alternativ för att utföra en granskning.</span><span class="sxs-lookup"><span data-stu-id="fc19d-128">There are three options for performing a review.</span></span> <span data-ttu-id="fc19d-129">Du kan tilldela hello granska toosomeone annan toocomplete göra det själv eller du kan låta användarna granska sina egna åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fc19d-129">You can assign hello review toosomeone else toocomplete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="fc19d-130">Navigera för**Markera granskare**</span><span class="sxs-lookup"><span data-stu-id="fc19d-130">Navigate too**Select reviewers**</span></span>
   
    ![Välj granskare – skärmbild][4]
2. <span data-ttu-id="fc19d-132">Välj något av hello alternativ:</span><span class="sxs-lookup"><span data-stu-id="fc19d-132">Choose one of hello options:</span></span>
   
   * <span data-ttu-id="fc19d-133">**Välj granskare**: Använd det här alternativet när du inte vet vem som behöver åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fc19d-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="fc19d-134">Med det här alternativet kan du tilldela hello granska tooa resursägare eller grupp manager toocomplete.</span><span class="sxs-lookup"><span data-stu-id="fc19d-134">With this option, you can assign hello review tooa resource owner or group manager toocomplete.</span></span>
   * <span data-ttu-id="fc19d-135">**Mig**: användbart om du vill toopreview hur åtkomst granskar arbete eller om du vill tooreview för personer som inte.</span><span class="sxs-lookup"><span data-stu-id="fc19d-135">**Me**: Useful if you want toopreview how access reviews work, or you want tooreview on behalf of people who can't.</span></span>
   * <span data-ttu-id="fc19d-136">**Medlemmar Läs själva**: Använd det här alternativet toohave hello användarna granska sina egna rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="fc19d-136">**Members review themselves**: Use this option toohave hello users review their own role assignments.</span></span>

### <a name="start-hello-review"></a><span data-ttu-id="fc19d-137">Starta hello granskning</span><span class="sxs-lookup"><span data-stu-id="fc19d-137">Start hello review</span></span>
<span data-ttu-id="fc19d-138">Slutligen har du hello alternativet toorequire att användare måste ange en orsak till om de godkänner deras åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fc19d-138">Finally, you have hello option toorequire that users provide a reason if they approve their access.</span></span> <span data-ttu-id="fc19d-139">Om du vill lägga till en beskrivning av hello, granska och välj **starta**.</span><span class="sxs-lookup"><span data-stu-id="fc19d-139">Add a description of hello review if you like, and select **Start**.</span></span>

<span data-ttu-id="fc19d-140">Kontrollera att du ger användarna om att det finns en åtkomst-granskning väntar på dem och visa dem [hur tooperform åtkomsten granska](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="fc19d-140">Make sure you let your users know that there's an access review waiting for them, and show them [How tooperform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-hello-access-review"></a><span data-ttu-id="fc19d-141">Hantera hello åtkomst granskning</span><span class="sxs-lookup"><span data-stu-id="fc19d-141">Manage hello access review</span></span>
<span data-ttu-id="fc19d-142">Du kan följa förloppet för hello när hello granskare Slutför granskningarna i hello Azure AD PIM-instrumentpanelen, i hello åtkomst går igenom avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fc19d-142">You can track hello progress as hello reviewers complete their reviews in hello Azure AD PIM dashboard, in hello access reviews section.</span></span> <span data-ttu-id="fc19d-143">Ingen behörighet kommer att ändras i hello directory förrän [hello, granska är klar](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="fc19d-143">No access rights will be changed in hello directory until [hello review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="fc19d-144">Tills hello omprövningsperioden är över, kan du påminna användare toocomplete granskning eller stoppa hello granska från hello åtkomst går igenom tidigt avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fc19d-144">Until hello review period is over, you can remind users toocomplete their review, or stop hello review early from hello access reviews section.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="fc19d-145">PIM innehållsförteckning</span><span class="sxs-lookup"><span data-stu-id="fc19d-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
