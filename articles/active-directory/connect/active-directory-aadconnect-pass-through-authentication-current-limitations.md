---
title: "Azure AD Connect: Direkt autentisering - aktuella begränsningar | Microsoft Docs"
description: "Den här artikeln beskriver hello aktuella begränsningar i Azure Active Directory (AD Azure) direkt-autentisering."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 2933745d071aae205c44659e6ea92697f390effb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a><span data-ttu-id="21bef-104">Azure Active Directory direkt-autentisering: Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="21bef-104">Azure Active Directory Pass-through Authentication: Current limitations</span></span>

>[!IMPORTANT]
><span data-ttu-id="21bef-105">Azure AD direkt-autentisering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="21bef-105">Azure AD Pass-through Authentication is currently in preview.</span></span> <span data-ttu-id="21bef-106">Det är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den.</span><span class="sxs-lookup"><span data-stu-id="21bef-106">It is a free feature, and you don't need any paid editions of Azure AD toouse it.</span></span> <span data-ttu-id="21bef-107">Direkt-autentisering är endast tillgängligt i hello world wide instans av Azure AD och inte på [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) och [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).</span><span class="sxs-lookup"><span data-stu-id="21bef-107">Pass-through Authentication is only available in hello world-wide instance of Azure AD, and not on [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) and [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="21bef-108">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="21bef-108">Supported scenarios</span></span>

<span data-ttu-id="21bef-109">hello följande scenarier stöds fullt ut under förhandsgranskningen:</span><span class="sxs-lookup"><span data-stu-id="21bef-109">hello following scenarios are fully supported during preview:</span></span>

- <span data-ttu-id="21bef-110">Användarinloggningar i alla webbläsarbaserad webbprogram.</span><span class="sxs-lookup"><span data-stu-id="21bef-110">User sign-ins into all web browser-based applications.</span></span>
- <span data-ttu-id="21bef-111">Användarinloggningar i Office 365-klientprogram som stöder [modern autentisering](https://aka.ms/modernauthga).</span><span class="sxs-lookup"><span data-stu-id="21bef-111">User sign-ins into Office 365 client applications that support [modern authentication](https://aka.ms/modernauthga).</span></span>
- <span data-ttu-id="21bef-112">Azure AD-anslutning för Windows 10-enheter.</span><span class="sxs-lookup"><span data-stu-id="21bef-112">Azure AD Join for Windows 10 devices.</span></span>
- <span data-ttu-id="21bef-113">Stöd för Exchange ActiveSync.</span><span class="sxs-lookup"><span data-stu-id="21bef-113">Exchange ActiveSync support.</span></span>

## <a name="unsupported-scenarios"></a><span data-ttu-id="21bef-114">Scenarier som inte stöds</span><span class="sxs-lookup"><span data-stu-id="21bef-114">Unsupported scenarios</span></span>

<span data-ttu-id="21bef-115">hello följande scenarier är _inte_ stöds under förhandsgranskningen:</span><span class="sxs-lookup"><span data-stu-id="21bef-115">hello following scenarios are _not_ supported during preview:</span></span>

- <span data-ttu-id="21bef-116">Användarinloggningar till äldre Office-program (Office 2013 eller tidigare).</span><span class="sxs-lookup"><span data-stu-id="21bef-116">User sign-ins into legacy Office client applications (Office 2013 or earlier).</span></span> <span data-ttu-id="21bef-117">Organisationer är rekommenderar tooswitch toomodern autentisering, om möjligt.</span><span class="sxs-lookup"><span data-stu-id="21bef-117">Organizations are encouraged tooswitch toomodern authentication, if possible.</span></span> <span data-ttu-id="21bef-118">Modern autentisering tillåter stöd för direkt-autentisering, men också kan du skydda din användare användarkonton med [villkorlig åtkomst](../active-directory-conditional-access.md) funktioner, till exempel Multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="21bef-118">Modern authentication allows for Pass-through Authentication support but also helps you secure your user accounts using [conditional access](../active-directory-conditional-access.md) features such as Multi-Factor Authentication (MFA).</span></span>
- <span data-ttu-id="21bef-119">Användarinloggningar till Skype för företag-klientprogram, inklusive Skype för företag 2016.</span><span class="sxs-lookup"><span data-stu-id="21bef-119">User sign-ins into Skype for Business client applications, including Skype for Business 2016.</span></span>
- <span data-ttu-id="21bef-120">Användarinloggningar i PowerShell v1.0.</span><span class="sxs-lookup"><span data-stu-id="21bef-120">User sign-ins into PowerShell v1.0.</span></span> <span data-ttu-id="21bef-121">Det rekommenderas att du använder PowerShell version 2.0 i stället.</span><span class="sxs-lookup"><span data-stu-id="21bef-121">It is recommended that you use PowerShell v2.0 instead.</span></span>

>[!IMPORTANT]
><span data-ttu-id="21bef-122">Aktivera synkronisering av lösenords-Hash som en lösning för scenarier som inte stöds på hello [valfria funktioner](active-directory-aadconnect-get-started-custom.md#optional-features) sida i hello Azure AD Connect-guiden.</span><span class="sxs-lookup"><span data-stu-id="21bef-122">As a workaround for unsupported scenarios, enable Password Hash Synchronization on hello [Optional features](active-directory-aadconnect-get-started-custom.md#optional-features) page in hello Azure AD Connect wizard.</span></span> <span data-ttu-id="21bef-123">Hash-synkronisering av lösenord fungerar som en reserv för hello föregående scenarier _endast_ (och _inte_ som en allmän tooPass via Reservautentisering).</span><span class="sxs-lookup"><span data-stu-id="21bef-123">Password Hash Synchronization acts as a fallback for hello preceding scenarios _only_ (and _not_ as a generic fallback tooPass-through Authentication).</span></span> <span data-ttu-id="21bef-124">Om du inte behöver dessa scenarier kan du inaktivera synkronisering av lösenords-Hash.</span><span class="sxs-lookup"><span data-stu-id="21bef-124">If you don't need these scenarios, turn off Password Hash Synchronization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21bef-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21bef-125">Next steps</span></span>
- <span data-ttu-id="21bef-126">[**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="21bef-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="21bef-127">[**Tekniska ingående** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -förstå hur funktionen fungerar.</span><span class="sxs-lookup"><span data-stu-id="21bef-127">[**Technical Deep Dive**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="21bef-128">[**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="21bef-128">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="21bef-129">[**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="21bef-129">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="21bef-130">[**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="21bef-130">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="21bef-131">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="21bef-131">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
