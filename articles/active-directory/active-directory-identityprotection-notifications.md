---
title: Azure Active Directory Identity Protection-aviseringar | Microsoft Docs
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
ms.openlocfilehash: 079d16bbf75cd2b3b94269d684e1ae1a0e6aa967
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="6bc50-104">Azure Active Directory Identity Protection-aviseringar</span><span class="sxs-lookup"><span data-stu-id="6bc50-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="6bc50-105">Azure AD Identity Protection skickar två typer av automatisk e-postmeddelanden som hjälper dig att hantera användare risk och riskhändelser:</span><span class="sxs-lookup"><span data-stu-id="6bc50-105">Azure AD Identity Protection sends two types of automated notification emails to help you manage user risk and risk events:</span></span>

* <span data-ttu-id="6bc50-106">Användaren komprometteras avisering e-post</span><span class="sxs-lookup"><span data-stu-id="6bc50-106">User compromised alert email</span></span>
* <span data-ttu-id="6bc50-107">Varje vecka sammanfattad e-post</span><span class="sxs-lookup"><span data-stu-id="6bc50-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="6bc50-108">Användaren komprometteras avisering e-post</span><span class="sxs-lookup"><span data-stu-id="6bc50-108">User compromised alert email</span></span>
<span data-ttu-id="6bc50-109">En användare avslöjade e-postavisering genereras när Azure AD Identity Protection identifierar ett konto som komprometterats.</span><span class="sxs-lookup"><span data-stu-id="6bc50-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="6bc50-110">E-postmeddelandet innehåller en länk till de användare som flaggats för risk rapporten i instrumentpanelen Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="6bc50-110">The email includes a link to the Users flagged for risk report in the Identity Protection dashboard.</span></span> <span data-ttu-id="6bc50-111">Vi rekommenderar att du genast undersöka meddelanden om avslöjade konton.</span><span class="sxs-lookup"><span data-stu-id="6bc50-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="6bc50-112">Varje vecka sammanfattad e-post</span><span class="sxs-lookup"><span data-stu-id="6bc50-112">Weekly digest email</span></span>
<span data-ttu-id="6bc50-113">Varje vecka sammanfattad e-postmeddelandet innehåller en sammanfattning av nya riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="6bc50-113">The weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="6bc50-114">Det innehåller:</span><span class="sxs-lookup"><span data-stu-id="6bc50-114">It includes:</span></span>

* <span data-ttu-id="6bc50-115">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="6bc50-115">Users at risk</span></span>
* <span data-ttu-id="6bc50-116">Misstänkta aktiviteter</span><span class="sxs-lookup"><span data-stu-id="6bc50-116">Suspicious activities</span></span>
* <span data-ttu-id="6bc50-117">Identifierade säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="6bc50-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="6bc50-118">Länkar till relaterade rapporter i Identity Protection</span><span class="sxs-lookup"><span data-stu-id="6bc50-118">Links to the related reports in Identity Protection</span></span>

<br><span data-ttu-id="6bc50-119">
![Reparation](./media/active-directory-identityprotection-notifications/400.png "reparation")
</span><span class="sxs-lookup"><span data-stu-id="6bc50-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="6bc50-120">Du kan växla skicka en vecka digest-e-post.</span><span class="sxs-lookup"><span data-stu-id="6bc50-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="6bc50-121">
![Användaren risker](./media/active-directory-identityprotection-notifications/62.png "användaren risker")
</span><span class="sxs-lookup"><span data-stu-id="6bc50-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="6bc50-122">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="6bc50-122">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="6bc50-123">På den **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="6bc50-123">On the **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="6bc50-124">
   ![Användaren riskprincipen](./media/active-directory-identityprotection-notifications/401.png "risk användarprincip")
   </span><span class="sxs-lookup"><span data-stu-id="6bc50-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="6bc50-125">I den **allmänna** klickar du på **meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="6bc50-125">In the **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="6bc50-126">
   ![Användaren riskprincipen](./media/active-directory-identityprotection-notifications/405.png "risk användarprincip")
   </span><span class="sxs-lookup"><span data-stu-id="6bc50-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="6bc50-127">Se även</span><span class="sxs-lookup"><span data-stu-id="6bc50-127">See also</span></span>
* [<span data-ttu-id="6bc50-128">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="6bc50-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
