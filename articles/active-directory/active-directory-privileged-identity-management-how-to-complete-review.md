---
title: "aaaHow toocomplete en åtkomst-granskning | Microsoft Docs"
description: "När du har startat en åtkomst-granskning i Azure AD Privileged Identity Management lär du dig hur toocomplete det och visa hello resultat"
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
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="738f9-103">Hur toocomplete åtkomst finns i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="738f9-103">How toocomplete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="738f9-104">Privilegierade rollen administratörer kan granska privilegierad åtkomst när en [säkerhetsgranskning har startats](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="738f9-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="738f9-105">Azure AD Privileged Identity Management (PIM) skickar automatiskt ett e-postmeddelande som uppmanar användare tooreview deras åtkomst.</span><span class="sxs-lookup"><span data-stu-id="738f9-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users tooreview their access.</span></span> <span data-ttu-id="738f9-106">Om en användare inte fick ett e-postmeddelande, kan du skicka dem hello instruktioner [hur tooperform en säkerhets-och granska](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="738f9-106">If a user did not get an email, you can send them hello instructions in [how tooperform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="738f9-107">När hello säkerhet granska perioden är över, eller alla hello användare är klar med sin egen granska, följ hello stegen i den här artikeln toomanage hello granska och se hello resultat.</span><span class="sxs-lookup"><span data-stu-id="738f9-107">After hello security review period is over, or all hello users have finished their self-review, follow hello steps in this article toomanage hello review and see hello results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="738f9-108">Hantera säkerhet granskningar</span><span class="sxs-lookup"><span data-stu-id="738f9-108">Manage security reviews</span></span>
1. <span data-ttu-id="738f9-109">Gå toohello [Azure-portalen](https://portal.azure.com/) och välj hello **Azure AD Privileged Identity Management** på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="738f9-109">Go toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="738f9-110">Välj hello **åtkomst till granskningar** avsnittet hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="738f9-110">Select hello **Access reviews** section of hello dashboard.</span></span>
3. <span data-ttu-id="738f9-111">Välj hello åtkomst granska som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="738f9-111">Select hello access review that you want toomanage.</span></span>

<span data-ttu-id="738f9-112">På hello åtkomst granska detalj bladet finns ett antal alternativ för att hantera granskningen.</span><span class="sxs-lookup"><span data-stu-id="738f9-112">On hello access review's detail blade there are a number options for managing that review.</span></span>

![PIM-knappar för granskning av åtkomst – skärmbild][1]

### <a name="remind"></a><span data-ttu-id="738f9-114">Påminn</span><span class="sxs-lookup"><span data-stu-id="738f9-114">Remind</span></span>
<span data-ttu-id="738f9-115">Om en åtkomst-granskning har konfigurerats så att användare hello granska själva hello **Påminn** knappen skickas ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="738f9-115">If an access review is set up so that hello users review themselves, hello **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="738f9-116">Stoppa</span><span class="sxs-lookup"><span data-stu-id="738f9-116">Stop</span></span>
<span data-ttu-id="738f9-117">Alla åtkomst omdömen har ett slutdatum, men du kan använda hello **stoppa** knappen toofinish den tidigt.</span><span class="sxs-lookup"><span data-stu-id="738f9-117">All access reviews have an end date, but you can use hello **Stop** button toofinish it early.</span></span> <span data-ttu-id="738f9-118">Om användare inte har granskats vid den tidpunkten, kan de inte att kunna tooafter du stoppar hello, granska.</span><span class="sxs-lookup"><span data-stu-id="738f9-118">If any users haven't been reviewed by this time, they won't be able tooafter you stop hello review.</span></span> <span data-ttu-id="738f9-119">Du kan inte starta om ett omdöme när den stoppas.</span><span class="sxs-lookup"><span data-stu-id="738f9-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="738f9-120">Ansök</span><span class="sxs-lookup"><span data-stu-id="738f9-120">Apply</span></span>
<span data-ttu-id="738f9-121">När en åtkomst-granskning har slutförts, antingen eftersom du har nått hello slutdatum eller hindrade den manuellt, hello **tillämpa** knappen implementerar hello resultatet av hello, granska.</span><span class="sxs-lookup"><span data-stu-id="738f9-121">After an access review is completed, either because you reached hello end date or stopped it manually, hello **Apply** button implements hello outcome of hello review.</span></span> <span data-ttu-id="738f9-122">Om en användares åtkomst nekades i hello, granska är hello steg som tar bort sin rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="738f9-122">If a user's access was denied in hello review, this is hello step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="738f9-123">Exportera</span><span class="sxs-lookup"><span data-stu-id="738f9-123">Export</span></span>
<span data-ttu-id="738f9-124">Om du vill tooapply hello resultaten av hello säkerhetsgranskning manuellt, kan du exportera hello, granska.</span><span class="sxs-lookup"><span data-stu-id="738f9-124">If you want tooapply hello results of hello security review manually, you can export hello review.</span></span> <span data-ttu-id="738f9-125">Hej **exportera** knappen startar hämtningen av en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="738f9-125">hello **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="738f9-126">Du kan hantera hello resultat i Excel eller andra program som öppnar CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="738f9-126">You can manage hello results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="738f9-127">Ta bort</span><span class="sxs-lookup"><span data-stu-id="738f9-127">Delete</span></span>
<span data-ttu-id="738f9-128">Om du inte är intresserad av hello granska någon ytterligare kan du ta bort den.</span><span class="sxs-lookup"><span data-stu-id="738f9-128">If you are not interested in hello review any further, delete it.</span></span> <span data-ttu-id="738f9-129">Hej **ta bort** knappen tar bort hello, granska från hello PIM-program.</span><span class="sxs-lookup"><span data-stu-id="738f9-129">hello **Delete** button removes hello review from hello PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="738f9-130">Du kommer inte får en varning innan borttagningen sker, så som du vill toodelete finns.</span><span class="sxs-lookup"><span data-stu-id="738f9-130">You will not get a warning before deletion occurs, so be sure that you want toodelete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="738f9-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="738f9-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
