---
title: "Hur du utför en åtkomst-granskning | Microsoft Docs"
description: "När du har startat en åtkomst-granskning i Azure AD Privileged Identity Management lär du dig hur du slutför den och visa resultaten"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: ca2a1c7c287e4cf6b1b50cfb0068861b6f525596
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="7e2f2-103">Hur du utför en åtkomst-granskning i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="7e2f2-103">How to complete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="7e2f2-104">Privilegierade rollen administratörer kan granska privilegierad åtkomst när en [säkerhetsgranskning har startats](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="7e2f2-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="7e2f2-105">Azure AD Privileged Identity Management (PIM) skickar automatiskt ett e-postmeddelande som uppmanar användare att granska deras åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users to review their access.</span></span> <span data-ttu-id="7e2f2-106">Om en användare inte fick ett e-postmeddelande, kan du skicka dem instruktionerna [så här utför du en säkerhetsgranskning](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="7e2f2-106">If a user did not get an email, you can send them the instructions in [how to perform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="7e2f2-107">När omprövningsperioden säkerhet är över, eller alla användare har slutfört sin egen granska, följer du stegen i den här artikeln för att hantera granskningen och se resultaten.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-107">After the security review period is over, or all the users have finished their self-review, follow the steps in this article to manage the review and see the results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="7e2f2-108">Hantera säkerhet granskningar</span><span class="sxs-lookup"><span data-stu-id="7e2f2-108">Manage security reviews</span></span>
1. <span data-ttu-id="7e2f2-109">Gå till den [Azure-portalen](https://portal.azure.com/) och välj den **Azure AD Privileged Identity Management** på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-109">Go to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="7e2f2-110">Välj den **åtkomst till granskningar** på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-110">Select the **Access reviews** section of the dashboard.</span></span>
3. <span data-ttu-id="7e2f2-111">Välj den granskning av åtkomst som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-111">Select the access review that you want to manage.</span></span>

<span data-ttu-id="7e2f2-112">På bladet för åtkomst-granska detalj finns ett antal alternativ för att hantera granskningen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-112">On the access review's detail blade there are a number options for managing that review.</span></span>

![PIM-knappar för granskning av åtkomst – skärmbild][1]

### <a name="remind"></a><span data-ttu-id="7e2f2-114">Påminn</span><span class="sxs-lookup"><span data-stu-id="7e2f2-114">Remind</span></span>
<span data-ttu-id="7e2f2-115">Om en åtkomst-granskning har konfigurerats så att användarna granska själva den **Påminn** knappen skickas ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-115">If an access review is set up so that the users review themselves, the **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="7e2f2-116">Stoppa</span><span class="sxs-lookup"><span data-stu-id="7e2f2-116">Stop</span></span>
<span data-ttu-id="7e2f2-117">Alla åtkomst omdömen har ett slutdatum, men du kan använda den **stoppa** för att slutföra den tidigt.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-117">All access reviews have an end date, but you can use the **Stop** button to finish it early.</span></span> <span data-ttu-id="7e2f2-118">Om användare inte har granskats vid den tidpunkten, kommer inte de att kunna när du stoppar granskningen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-118">If any users haven't been reviewed by this time, they won't be able to after you stop the review.</span></span> <span data-ttu-id="7e2f2-119">Du kan inte starta om ett omdöme när den stoppas.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="7e2f2-120">Ansök</span><span class="sxs-lookup"><span data-stu-id="7e2f2-120">Apply</span></span>
<span data-ttu-id="7e2f2-121">När en åtkomst-granskning har slutförts, antingen eftersom du har nått slutdatum eller hindrade den manuellt på **tillämpa** knappen implementerar resultatet av granskningen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-121">After an access review is completed, either because you reached the end date or stopped it manually, the **Apply** button implements the outcome of the review.</span></span> <span data-ttu-id="7e2f2-122">Om en användares åtkomst nekades i granskningen, är det steg som tar bort sin rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-122">If a user's access was denied in the review, this is the step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="7e2f2-123">Exportera</span><span class="sxs-lookup"><span data-stu-id="7e2f2-123">Export</span></span>
<span data-ttu-id="7e2f2-124">Om du vill använda resultatet av säkerhetsgranskning manuellt, kan du exportera granskningen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-124">If you want to apply the results of the security review manually, you can export the review.</span></span> <span data-ttu-id="7e2f2-125">Den **exportera** knappen startar hämtningen av en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-125">The **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="7e2f2-126">Du kan hantera resultaten i Excel eller andra program som öppnar CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-126">You can manage the results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="7e2f2-127">Ta bort</span><span class="sxs-lookup"><span data-stu-id="7e2f2-127">Delete</span></span>
<span data-ttu-id="7e2f2-128">Om du inte är intresserad av att granska ytterligare kan du ta bort den.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-128">If you are not interested in the review any further, delete it.</span></span> <span data-ttu-id="7e2f2-129">Den **ta bort** knappen tar du bort granskningen av PIM-programmet.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-129">The **Delete** button removes the review from the PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7e2f2-130">Du kommer inte får en varning innan borttagningen sker, så att du vill ta bort granskningen.</span><span class="sxs-lookup"><span data-stu-id="7e2f2-130">You will not get a warning before deletion occurs, so be sure that you want to delete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7e2f2-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e2f2-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
