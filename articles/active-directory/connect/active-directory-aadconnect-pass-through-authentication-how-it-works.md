---
title: "Azure AD Connect: Direkt-autentisering - så här fungerar det? | Microsoft Docs"
description: "Den här artikeln beskriver hur Azure Active Directory direkt-autentisering fungerar."
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
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="e33c1-105">Azure Active Directory direkt-autentisering: Tekniska ingående</span><span class="sxs-lookup"><span data-stu-id="e33c1-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="e33c1-106">Azure AD direkt-autentisering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="e33c1-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="e33c1-107">Hur fungerar Azure Active Directory direkt-autentisering?</span><span class="sxs-lookup"><span data-stu-id="e33c1-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="e33c1-108">När en användare försöker toosign i ett program som skyddas av Azure Active Directory (Azure AD) och om direkt-autentisering är aktiverat på hello-klient hello utförs följande steg:</span><span class="sxs-lookup"><span data-stu-id="e33c1-108">When a user attempts toosign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on hello tenant, hello following steps occur:</span></span>

1. <span data-ttu-id="e33c1-109">hello användare försöker tooaccess ett program (till exempel hello Outlook Web App - https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="e33c1-109">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="e33c1-110">Om hello användaren inte redan har loggat in är hello användaren omdirigerade toohello Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e33c1-110">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>
3. <span data-ttu-id="e33c1-111">hello användaren anger sitt användarnamn och lösenord i hello Azure AD-inloggningssida och klickar på hello ”logga in” knappen.</span><span class="sxs-lookup"><span data-stu-id="e33c1-111">hello user enters their username and password into hello Azure AD sign-in page and clicks hello "Sign in" button.</span></span>
4. <span data-ttu-id="e33c1-112">Azure AD på hello inloggning begäran tas emot placerar hello användarnamn och lösenord (krypterade med en offentlig nyckel) i en kö.</span><span class="sxs-lookup"><span data-stu-id="e33c1-112">Azure AD, on receiving hello sign-in request, places hello username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="e33c1-113">En lokal direkt autentiseringsagent gör en utgående samtal toohello kö och hämtar hello användarnamn och lösenord för krypterade.</span><span class="sxs-lookup"><span data-stu-id="e33c1-113">An on-premises Pass-through Authentication agent makes an outbound call toohello queue and retrieves hello username and encrypted password.</span></span>
6. <span data-ttu-id="e33c1-114">hello agent dekrypterar hello lösenord med hjälp av den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e33c1-114">hello agent decrypts hello password using its private key.</span></span>
7. <span data-ttu-id="e33c1-115">hello agent verifierar sedan hello användarnamn och lösenord mot Active Directory använder standard Windows API: er (en liknande mekanism toowhat används av Active Directory Federation Services).</span><span class="sxs-lookup"><span data-stu-id="e33c1-115">hello agent then validates hello username and password against Active Directory using standard Windows APIs (a similar mechanism toowhat is used by Active Directory Federation Services).</span></span> <span data-ttu-id="e33c1-116">hello användarnamnet kan vara antingen hello lokalt Standardanvändarnamnet (vanligtvis `userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (kallas även `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="e33c1-116">hello username can be either hello on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="e33c1-117">hello lokala Active Directory domänkontrollanten (DC) sedan utvärderar hello begäran och returnerar hello lämplig respons (lyckad, misslyckad, lösenord upphört att gälla eller användaren utelåst) toohello agent.</span><span class="sxs-lookup"><span data-stu-id="e33c1-117">hello on-premises Active Directory Domain Controller (DC) then evaluates hello request and returns hello appropriate response (success, failure, password expired or user locked out) toohello agent.</span></span>
9. <span data-ttu-id="e33c1-118">hello agent returnerar det här svaret tillbaka tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="e33c1-118">hello agent, in turn, returns this response back tooAzure AD.</span></span>
10. <span data-ttu-id="e33c1-119">Azure AD utvärderar hello svar och svarar toohello användaren efter behov – exempelvis den loggar hello användaren omedelbart eller begär för multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="e33c1-119">Azure AD evaluates hello response and responds toohello user as appropriate - for example, it either signs hello user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="e33c1-120">Om hello användarens inloggning lyckas är hello användaren kan tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="e33c1-120">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="e33c1-121">hello följande diagram illustrerar alla hello komponenter och hello stegen.</span><span class="sxs-lookup"><span data-stu-id="e33c1-121">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Direktautentisering](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="e33c1-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e33c1-123">Next steps</span></span>
- <span data-ttu-id="e33c1-124">[**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="e33c1-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="e33c1-125">Läs om vilka scenarier som stöds och vilka som inte är.</span><span class="sxs-lookup"><span data-stu-id="e33c1-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="e33c1-126">[**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="e33c1-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="e33c1-127">[**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="e33c1-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="e33c1-128">[**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="e33c1-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="e33c1-129">[**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e33c1-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="e33c1-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="e33c1-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
