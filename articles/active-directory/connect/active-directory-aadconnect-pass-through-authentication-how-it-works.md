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
ms.openlocfilehash: d34ccd40082edbe036d963ad548bff648119bdd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="4d727-105">Azure Active Directory direkt-autentisering: Tekniska ingående</span><span class="sxs-lookup"><span data-stu-id="4d727-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="4d727-106">Azure AD direkt-autentisering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="4d727-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="4d727-107">Hur fungerar Azure Active Directory direkt-autentisering?</span><span class="sxs-lookup"><span data-stu-id="4d727-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="4d727-108">När en användare försöker logga in på ett program som skyddas av Azure Active Directory (Azure AD), och om direkt-autentisering är aktiverat på klienten, utförs följande steg:</span><span class="sxs-lookup"><span data-stu-id="4d727-108">When a user attempts to sign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on the tenant, the following steps occur:</span></span>

1. <span data-ttu-id="4d727-109">Användaren försöker komma åt ett program (till exempel Outlook Web App - https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="4d727-109">The user tries to access an application (for example, the Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="4d727-110">Om användaren inte redan har signerats omdirigeras användaren till sidan för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d727-110">If the user is not already signed in, the user is redirected to the Azure AD sign-in page.</span></span>
3. <span data-ttu-id="4d727-111">Användaren anger sitt användarnamn och lösenord i Azure AD-inloggningssida och klickar på knappen ”Logga in”.</span><span class="sxs-lookup"><span data-stu-id="4d727-111">The user enters their username and password into the Azure AD sign-in page and clicks the "Sign in" button.</span></span>
4. <span data-ttu-id="4d727-112">Azure AD på begäran inloggning placerar användarnamn och lösenord (krypterade med en offentlig nyckel) i en kö.</span><span class="sxs-lookup"><span data-stu-id="4d727-112">Azure AD, on receiving the sign-in request, places the username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="4d727-113">En lokal direkt autentiseringsagent gör ett anrop till kön för utgående och hämtar användarnamn och lösenord för krypterade.</span><span class="sxs-lookup"><span data-stu-id="4d727-113">An on-premises Pass-through Authentication agent makes an outbound call to the queue and retrieves the username and encrypted password.</span></span>
6. <span data-ttu-id="4d727-114">Agenten dekrypterar lösenord med dess privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="4d727-114">The agent decrypts the password using its private key.</span></span>
7. <span data-ttu-id="4d727-115">Agenten verifierar sedan användarnamnet och lösenordet mot Active Directory använder standard Windows API: er (en liknande mekanism för som används av Active Directory Federation Services).</span><span class="sxs-lookup"><span data-stu-id="4d727-115">The agent then validates the username and password against Active Directory using standard Windows APIs (a similar mechanism to what is used by Active Directory Federation Services).</span></span> <span data-ttu-id="4d727-116">Användarnamnet kan vara antingen lokalt Standardanvändarnamnet (vanligtvis `userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (kallas även `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="4d727-116">The username can be either the on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="4d727-117">Den lokala Active Directory domänkontrollanten (DC) sedan utvärderar begäran och returnerar lämplig respons (lyckad, misslyckad, lösenord upphört att gälla eller användaren utelåst) till agenten.</span><span class="sxs-lookup"><span data-stu-id="4d727-117">The on-premises Active Directory Domain Controller (DC) then evaluates the request and returns the appropriate response (success, failure, password expired or user locked out) to the agent.</span></span>
9. <span data-ttu-id="4d727-118">Agenten returnerar svaret tillbaka till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d727-118">The agent, in turn, returns this response back to Azure AD.</span></span>
10. <span data-ttu-id="4d727-119">Azure AD svaret utvärderas och svarar på användare efter behov – till exempel det loggar du in omedelbart eller förfrågningar för multi-Factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="4d727-119">Azure AD evaluates the response and responds to the user as appropriate - for example, it either signs the user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="4d727-120">Om användaren loggar in lyckas kan användaren komma åt programmet.</span><span class="sxs-lookup"><span data-stu-id="4d727-120">If the user sign-in is successful, the user is able to access the application.</span></span>

<span data-ttu-id="4d727-121">Följande diagram illustrerar alla komponenter och steg som ingår.</span><span class="sxs-lookup"><span data-stu-id="4d727-121">The following diagram illustrates all the components and the steps involved.</span></span>

![Direkt-autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="4d727-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d727-123">Next steps</span></span>
- <span data-ttu-id="4d727-124">[**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="4d727-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="4d727-125">Läs om vilka scenarier som stöds och vilka som inte är.</span><span class="sxs-lookup"><span data-stu-id="4d727-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="4d727-126">[**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="4d727-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="4d727-127">[**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -svar på vanliga frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="4d727-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="4d727-128">[**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig att lösa vanliga problem med funktionen.</span><span class="sxs-lookup"><span data-stu-id="4d727-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="4d727-129">[**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4d727-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="4d727-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="4d727-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
