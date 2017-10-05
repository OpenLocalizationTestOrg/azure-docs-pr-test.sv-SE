---
title: "Begränsningar i Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Aktuella begränsningar för Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 581e5d1fb5fb08d0dc89ed2c85edcb5f0005650b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="5e1d6-103">Begränsningar i Azure AD B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="5e1d6-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="5e1d6-104">Azure Active Directory (AD Azure) B2B-samarbete är för närvarande de begränsningar som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject to the limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="5e1d6-105">Möjliga dubbla multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="5e1d6-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="5e1d6-106">Du kan använda multifaktorautentisering i resursorganisationen (bjuda in organisation) med Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-106">With Azure AD B2B, you can enforce multi-factor authentication at the resource organization (the inviting organization).</span></span> <span data-ttu-id="5e1d6-107">Orsaker till den här metoden beskrivs i [villkorlig åtkomst för användare för B2B-samarbete](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="5e1d6-107">The reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="5e1d6-108">Om en partner har redan multifaktorautentisering ställa in och tillämpas, kanske användarna att autentisera en gång i organisationen hem och sedan igen i din egen.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-108">If a partner already has multi-factor authentication set up and enforced, their users might have to perform the authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="5e1d6-109">Startar omedelbart</span><span class="sxs-lookup"><span data-stu-id="5e1d6-109">Instant-on</span></span>
<span data-ttu-id="5e1d6-110">I B2B-samarbete flöden, vi lägga till användare i katalogen och uppdatera dynamiskt under inbjudan inlösning, tilldelning av appen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-110">In the B2B collaboration flows, we add users to the directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="5e1d6-111">Uppdateringar och skrivningar normalt sker i en kataloginstans och måste replikeras över alla instanser.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-111">The updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="5e1d6-112">Replikeringen är klar när alla instanser uppdateras.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="5e1d6-113">Ibland när objektet skrivs eller uppdateras i en instans och anrop till hämta det här objektet till en annan instans, kan replikeringsfördröjningar uppstå.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-113">Sometimes when the object is written or updated in one instance and the call to retrieve this object is to another instance, replication latencies can occur.</span></span> <span data-ttu-id="5e1d6-114">I så fall, uppdatera eller försök att.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-114">If that happens, refresh or retry to help.</span></span> <span data-ttu-id="5e1d6-115">Om du skriver en app med vårt API är en bra och defensiva idé att lösa problemet med återförsök med vissa inte.</span><span class="sxs-lookup"><span data-stu-id="5e1d6-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice to alleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e1d6-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e1d6-116">Next steps</span></span>

<span data-ttu-id="5e1d6-117">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="5e1d6-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="5e1d6-118">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="5e1d6-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="5e1d6-119">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="5e1d6-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="5e1d6-120">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="5e1d6-120">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="5e1d6-121">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="5e1d6-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="5e1d6-122">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="5e1d6-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="5e1d6-123">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="5e1d6-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="5e1d6-124">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="5e1d6-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="5e1d6-125">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="5e1d6-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="5e1d6-126">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="5e1d6-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="5e1d6-127">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="5e1d6-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
