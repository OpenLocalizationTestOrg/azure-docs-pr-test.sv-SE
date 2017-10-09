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
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="e1aab-103">Återställ Licensieringskrav för självbetjäning Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="e1aab-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="e1aab-104">För Azure AD för lösenordsåterställning toofunction du **måste ha minst en licens för i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="e1aab-104">In order for Azure AD Password Reset toofunction, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="e1aab-105">Vi behöver inte använda per användare-licensiering på hello lösenord Återställ upplevelse.</span><span class="sxs-lookup"><span data-stu-id="e1aab-105">We do not enforce per-user licensing on hello password reset experience.</span></span> <span data-ttu-id="e1aab-106">toomaintain följer licensavtalet Microsoft, behöver du tooassign licenser tooany användare som använder premiumfunktioner.</span><span class="sxs-lookup"><span data-stu-id="e1aab-106">toomaintain compliance with your Microsoft licensing agreement, you need tooassign licenses tooany users that use premium features.</span></span>

* <span data-ttu-id="e1aab-107">**Molnet endast användare** -Office 365 (O365) någon betald SKU eller Azure AD Basic</span><span class="sxs-lookup"><span data-stu-id="e1aab-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="e1aab-108">**Molnet** och/eller **lokala användare** -Azure AD Premium P1 eller P2, Enterprise Mobility + Security (EMS) eller säker produktiva Enterprise (a)</span><span class="sxs-lookup"><span data-stu-id="e1aab-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="e1aab-109">Licenser som krävs för tillbakaskrivning av lösenord</span><span class="sxs-lookup"><span data-stu-id="e1aab-109">Licenses required for password writeback</span></span>

<span data-ttu-id="e1aab-110">toouse tillbakaskrivning av lösenord, måste du ha en hello följande licenser som tilldelats i din klient.</span><span class="sxs-lookup"><span data-stu-id="e1aab-110">toouse password writeback, you must have one of hello following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="e1aab-111">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="e1aab-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="e1aab-112">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="e1aab-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="e1aab-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="e1aab-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="e1aab-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="e1aab-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="e1aab-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="e1aab-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="e1aab-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="e1aab-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="e1aab-117">Fristående Office 365-licensiering planer **har inte stöd för tillbakaskrivning av lösenord** och kräver en hello föregående planer för den här funktionen toowork.</span><span class="sxs-lookup"><span data-stu-id="e1aab-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of hello preceding plans for this functionality toowork.</span></span>

<span data-ttu-id="e1aab-118">Ytterligare licenser uppgifter om kostnaderna kan hittas på följande sidor hello</span><span class="sxs-lookup"><span data-stu-id="e1aab-118">Additional licensing info including costs can be found on hello following pages</span></span>

* [<span data-ttu-id="e1aab-119">Platsen för Azure Active Directory-priser</span><span class="sxs-lookup"><span data-stu-id="e1aab-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="e1aab-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="e1aab-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="e1aab-121">Säker produktiva Enterprise</span><span class="sxs-lookup"><span data-stu-id="e1aab-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="e1aab-122">Aktivera grupp eller användarbaserade licensiering</span><span class="sxs-lookup"><span data-stu-id="e1aab-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="e1aab-123">Azure AD nu stöder gruppbaserade licensiering vilket gör att administratörer tooassign licenser i bulk tooa grupp av användare, snarare än tilldela dem en i taget.</span><span class="sxs-lookup"><span data-stu-id="e1aab-123">Azure AD now supports group-based licensing allowing administrators tooassign licenses in bulk tooa group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="e1aab-124">Tilldela kontrollerar och lösa problem med licenser</span><span class="sxs-lookup"><span data-stu-id="e1aab-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="e1aab-125">Vissa Microsoft-tjänster är inte tillgängliga på alla platser.</span><span class="sxs-lookup"><span data-stu-id="e1aab-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="e1aab-126">Innan en licens kan tilldelas tooa användare, anger Hej administratör hello ”användning” Platsegenskapen hello användaren.</span><span class="sxs-lookup"><span data-stu-id="e1aab-126">Before a license can be assigned tooa user, hello administrator must specify hello “Usage location” property on hello user.</span></span> <span data-ttu-id="e1aab-127">Tilldelning av licenser kan göras under användare > Profil > Inställningar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e1aab-127">Assignment of licenses can be done under User > Profile > Settings section in hello Azure portal.</span></span> <span data-ttu-id="e1aab-128">**När du använder gruppen licenstilldelning ärver alla användare utan en användningsplats anges hello platsen för hello katalog.**</span><span class="sxs-lookup"><span data-stu-id="e1aab-128">**When using group license assignment, any users without a usage location specified inherit hello location of hello directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1aab-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1aab-129">Next steps</span></span>

<span data-ttu-id="e1aab-130">hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1aab-130">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="e1aab-131">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1aab-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="e1aab-132">[**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="e1aab-132">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="e1aab-133">[**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här</span><span class="sxs-lookup"><span data-stu-id="e1aab-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="e1aab-134">[**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.</span><span class="sxs-lookup"><span data-stu-id="e1aab-134">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="e1aab-135">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="e1aab-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="e1aab-136">[**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="e1aab-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="e1aab-137">[**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man?</span><span class="sxs-lookup"><span data-stu-id="e1aab-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="e1aab-138">Varför?</span><span class="sxs-lookup"><span data-stu-id="e1aab-138">Why?</span></span> <span data-ttu-id="e1aab-139">Vad?</span><span class="sxs-lookup"><span data-stu-id="e1aab-139">What?</span></span> <span data-ttu-id="e1aab-140">Var?</span><span class="sxs-lookup"><span data-stu-id="e1aab-140">Where?</span></span> <span data-ttu-id="e1aab-141">Vem?</span><span class="sxs-lookup"><span data-stu-id="e1aab-141">Who?</span></span> <span data-ttu-id="e1aab-142">När?</span><span class="sxs-lookup"><span data-stu-id="e1aab-142">When?</span></span> <span data-ttu-id="e1aab-143">-Svar tooquestions du alltid vill ha tooask</span><span class="sxs-lookup"><span data-stu-id="e1aab-143">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="e1aab-144">[**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR</span><span class="sxs-lookup"><span data-stu-id="e1aab-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

