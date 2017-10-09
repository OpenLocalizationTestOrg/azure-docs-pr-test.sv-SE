---
title: aaaAzure Active Directory Identity Protection-aviseringar | Microsoft Docs
description: "Lär dig hur meddelanden stöder undersökningen-aktiviteter."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 19e62374873f034591c658a779f97d935766c612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="7d4ed-104">Azure Active Directory Identity Protection-aviseringar</span><span class="sxs-lookup"><span data-stu-id="7d4ed-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="7d4ed-105">Azure AD Identity Protection skickar två typer av automatisk anmälan e-post toohelp som du hanterar användare risk och riskhändelser:</span><span class="sxs-lookup"><span data-stu-id="7d4ed-105">Azure AD Identity Protection sends two types of automated notification emails toohelp you manage user risk and risk events:</span></span>

* <span data-ttu-id="7d4ed-106">Användaren komprometteras avisering e-post</span><span class="sxs-lookup"><span data-stu-id="7d4ed-106">User compromised alert email</span></span>
* <span data-ttu-id="7d4ed-107">Varje vecka sammanfattad e-post</span><span class="sxs-lookup"><span data-stu-id="7d4ed-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="7d4ed-108">Användaren komprometteras avisering e-post</span><span class="sxs-lookup"><span data-stu-id="7d4ed-108">User compromised alert email</span></span>
<span data-ttu-id="7d4ed-109">En användare avslöjade e-postavisering genereras när Azure AD Identity Protection identifierar ett konto som komprometterats.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="7d4ed-110">hello e-post innehåller en länk toohello användare som har flaggats för risk rapporten i hello identitetsskydd instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-110">hello email includes a link toohello Users flagged for risk report in hello Identity Protection dashboard.</span></span> <span data-ttu-id="7d4ed-111">Vi rekommenderar att du genast undersöka meddelanden om avslöjade konton.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="7d4ed-112">Varje vecka sammanfattad e-post</span><span class="sxs-lookup"><span data-stu-id="7d4ed-112">Weekly digest email</span></span>
<span data-ttu-id="7d4ed-113">hello veckovisa sammanfattad e-post innehåller en sammanfattning av nya riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-113">hello weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="7d4ed-114">Det innehåller:</span><span class="sxs-lookup"><span data-stu-id="7d4ed-114">It includes:</span></span>

* <span data-ttu-id="7d4ed-115">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="7d4ed-115">Users at risk</span></span>
* <span data-ttu-id="7d4ed-116">Misstänkta aktiviteter</span><span class="sxs-lookup"><span data-stu-id="7d4ed-116">Suspicious activities</span></span>
* <span data-ttu-id="7d4ed-117">Identifierade säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="7d4ed-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="7d4ed-118">Länkar toohello relaterade rapporter i Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7d4ed-118">Links toohello related reports in Identity Protection</span></span>

<br><span data-ttu-id="7d4ed-119">
![Reparation](./media/active-directory-identityprotection-notifications/400.png "reparation")
</span><span class="sxs-lookup"><span data-stu-id="7d4ed-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="7d4ed-120">Du kan växla skicka en vecka digest-e-post.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="7d4ed-121">
![Användaren risker](./media/active-directory-identityprotection-notifications/62.png "användaren risker")
</span><span class="sxs-lookup"><span data-stu-id="7d4ed-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="7d4ed-122">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="7d4ed-122">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="7d4ed-123">På hello **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-123">On hello **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="7d4ed-124">
   ![Användaren riskprincipen](./media/active-directory-identityprotection-notifications/401.png "risk användarprincip")
   </span><span class="sxs-lookup"><span data-stu-id="7d4ed-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="7d4ed-125">I hello **allmänna** klickar du på **meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="7d4ed-125">In hello **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="7d4ed-126">
   ![Användaren riskprincipen](./media/active-directory-identityprotection-notifications/405.png "risk användarprincip")
   </span><span class="sxs-lookup"><span data-stu-id="7d4ed-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="7d4ed-127">Se även</span><span class="sxs-lookup"><span data-stu-id="7d4ed-127">See also</span></span>
* [<span data-ttu-id="7d4ed-128">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="7d4ed-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
