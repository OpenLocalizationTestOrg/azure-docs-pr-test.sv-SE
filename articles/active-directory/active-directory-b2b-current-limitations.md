---
title: aaaLimitations av Azure Active Directory B2B-samarbete | Microsoft Docs
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
ms.openlocfilehash: 322081f32fbacfe67ee1300993c7df1870e498bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="8c424-103">Begränsningar i Azure AD B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8c424-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="8c424-104">Azure Active Directory (AD Azure) B2B-samarbete är för närvarande ämne toohello begränsningar som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8c424-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject toohello limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="8c424-105">Möjliga dubbla multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="8c424-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="8c424-106">Du kan tillämpa Multi-Factor authentication på hello resursorganisationen (hello bjuda in organisation) med Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="8c424-106">With Azure AD B2B, you can enforce multi-factor authentication at hello resource organization (hello inviting organization).</span></span> <span data-ttu-id="8c424-107">hello orsaker till den här metoden beskrivs i [villkorlig åtkomst för användare för B2B-samarbete](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="8c424-107">hello reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="8c424-108">Om en partner har redan multifaktorautentisering ställa in och tillämpas, användarna kan ha tooperform hello autentisering en gång i organisationen hem och sedan igen i din egen.</span><span class="sxs-lookup"><span data-stu-id="8c424-108">If a partner already has multi-factor authentication set up and enforced, their users might have tooperform hello authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="8c424-109">Startar omedelbart</span><span class="sxs-lookup"><span data-stu-id="8c424-109">Instant-on</span></span>
<span data-ttu-id="8c424-110">I hello B2B-samarbete flöden vi lägga till användare toohello katalog och uppdatera dynamiskt under inbjudan inlösning, tilldelning av appen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8c424-110">In hello B2B collaboration flows, we add users toohello directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="8c424-111">hello uppdateringar och skrivningar normalt sker i en kataloginstans och måste replikeras över alla instanser.</span><span class="sxs-lookup"><span data-stu-id="8c424-111">hello updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="8c424-112">Replikeringen är klar när alla instanser uppdateras.</span><span class="sxs-lookup"><span data-stu-id="8c424-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="8c424-113">Ibland är när hello objekt skrivs eller uppdateras i en instans och hello anropa tooretrieve det här objektet tooanother instans replikeringsfördröjningar kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="8c424-113">Sometimes when hello object is written or updated in one instance and hello call tooretrieve this object is tooanother instance, replication latencies can occur.</span></span> <span data-ttu-id="8c424-114">I så fall, uppdatera eller försök toohelp.</span><span class="sxs-lookup"><span data-stu-id="8c424-114">If that happens, refresh or retry toohelp.</span></span> <span data-ttu-id="8c424-115">Om du skriver en app med våra API: T och försök med några inte är en bra, och defensiva tooalleviate problemet.</span><span class="sxs-lookup"><span data-stu-id="8c424-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice tooalleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c424-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c424-116">Next steps</span></span>

<span data-ttu-id="8c424-117">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="8c424-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="8c424-118">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="8c424-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="8c424-119">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8c424-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="8c424-120">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="8c424-120">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="8c424-121">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="8c424-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="8c424-122">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8c424-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="8c424-123">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="8c424-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="8c424-124">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8c424-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="8c424-125">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8c424-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="8c424-126">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="8c424-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="8c424-127">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="8c424-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
