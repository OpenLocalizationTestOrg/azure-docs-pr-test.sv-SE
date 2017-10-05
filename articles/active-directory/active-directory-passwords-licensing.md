---
title: 'Licensiering: Azure AD SSPR | Microsoft Docs'
description: "Azure AD Självbetjäning för lösenordsåterställning licenskrav"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="e6543-103">Återställ Licensieringskrav för självbetjäning Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="e6543-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="e6543-104">För återställning av Azure AD-lösenord ska fungera, du **måste ha minst en licens för i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="e6543-104">In order for Azure AD Password Reset to function, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="e6543-105">Vi behöver inte använda per användare-licensiering om återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="e6543-105">We do not enforce per-user licensing on the password reset experience.</span></span> <span data-ttu-id="e6543-106">Du måste tilldela licenser till användare som använder premiumfunktioner för att bibehålla kompatibilitet med ditt Microsoft-licensavtalet.</span><span class="sxs-lookup"><span data-stu-id="e6543-106">To maintain compliance with your Microsoft licensing agreement, you need to assign licenses to any users that use premium features.</span></span>

* <span data-ttu-id="e6543-107">**Molnet endast användare** -Office 365 (O365) någon betald SKU eller Azure AD Basic</span><span class="sxs-lookup"><span data-stu-id="e6543-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="e6543-108">**Molnet** och/eller **lokala användare** -Azure AD Premium P1 eller P2, Enterprise Mobility + Security (EMS) eller säker produktiva Enterprise (a)</span><span class="sxs-lookup"><span data-stu-id="e6543-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="e6543-109">Licenser som krävs för tillbakaskrivning av lösenord</span><span class="sxs-lookup"><span data-stu-id="e6543-109">Licenses required for password writeback</span></span>

<span data-ttu-id="e6543-110">Om du vill använda tillbakaskrivning av lösenord, måste du ha en av de följande licenser som tilldelats i din klient.</span><span class="sxs-lookup"><span data-stu-id="e6543-110">To use password writeback, you must have one of the following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="e6543-111">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="e6543-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="e6543-112">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="e6543-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="e6543-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="e6543-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="e6543-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="e6543-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="e6543-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="e6543-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="e6543-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="e6543-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="e6543-117">Fristående Office 365-licensiering planer **har inte stöd för tillbakaskrivning av lösenord** och kräver en av de föregående planerna för den här funktionen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e6543-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of the preceding plans for this functionality to work.</span></span>

<span data-ttu-id="e6543-118">Ytterligare licenser uppgifter om kostnaderna kan hittas på följande sidor</span><span class="sxs-lookup"><span data-stu-id="e6543-118">Additional licensing info including costs can be found on the following pages</span></span>

* [<span data-ttu-id="e6543-119">Platsen för Azure Active Directory-priser</span><span class="sxs-lookup"><span data-stu-id="e6543-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="e6543-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="e6543-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="e6543-121">Säker produktiva Enterprise</span><span class="sxs-lookup"><span data-stu-id="e6543-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="e6543-122">Aktivera grupp eller användarbaserade licensiering</span><span class="sxs-lookup"><span data-stu-id="e6543-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="e6543-123">Azure AD nu stöder gruppbaserade licensiering vilket gör att administratörer för att tilldela licenser gruppvis till en grupp användare, snarare än tilldela dem en i taget.</span><span class="sxs-lookup"><span data-stu-id="e6543-123">Azure AD now supports group-based licensing allowing administrators to assign licenses in bulk to a group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="e6543-124">Tilldela kontrollerar och lösa problem med licenser</span><span class="sxs-lookup"><span data-stu-id="e6543-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="e6543-125">Vissa Microsoft-tjänster är inte tillgängliga på alla platser.</span><span class="sxs-lookup"><span data-stu-id="e6543-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="e6543-126">Innan en användare kan tilldelas en licens, måste administratören ange egenskapen ”användningsplats” för användaren.</span><span class="sxs-lookup"><span data-stu-id="e6543-126">Before a license can be assigned to a user, the administrator must specify the “Usage location” property on the user.</span></span> <span data-ttu-id="e6543-127">Tilldelning av licenser kan göras under användare > Profil > Inställningar i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6543-127">Assignment of licenses can be done under User > Profile > Settings section in the Azure portal.</span></span> <span data-ttu-id="e6543-128">**När du använder gruppen licenstilldelning ärver alla användare utan en användningsplats anges platsen för katalogen.**</span><span class="sxs-lookup"><span data-stu-id="e6543-128">**When using group license assignment, any users without a usage location specified inherit the location of the directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6543-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6543-129">Next steps</span></span>

<span data-ttu-id="e6543-130">Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6543-130">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="e6543-131">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6543-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="e6543-132">[**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="e6543-132">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="e6543-133">[**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här</span><span class="sxs-lookup"><span data-stu-id="e6543-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="e6543-134">[**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.</span><span class="sxs-lookup"><span data-stu-id="e6543-134">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="e6543-135">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="e6543-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="e6543-136">[**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar</span><span class="sxs-lookup"><span data-stu-id="e6543-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="e6543-137">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="e6543-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="e6543-138">Varför?</span><span class="sxs-lookup"><span data-stu-id="e6543-138">Why?</span></span> <span data-ttu-id="e6543-139">Vad?</span><span class="sxs-lookup"><span data-stu-id="e6543-139">What?</span></span> <span data-ttu-id="e6543-140">Var?</span><span class="sxs-lookup"><span data-stu-id="e6543-140">Where?</span></span> <span data-ttu-id="e6543-141">Vem?</span><span class="sxs-lookup"><span data-stu-id="e6543-141">Who?</span></span> <span data-ttu-id="e6543-142">När?</span><span class="sxs-lookup"><span data-stu-id="e6543-142">When?</span></span> <span data-ttu-id="e6543-143">– Svar på allt du någonsin velat fråga</span><span class="sxs-lookup"><span data-stu-id="e6543-143">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="e6543-144">[**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR</span><span class="sxs-lookup"><span data-stu-id="e6543-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

