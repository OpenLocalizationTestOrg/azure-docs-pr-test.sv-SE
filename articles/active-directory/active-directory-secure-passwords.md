---
title: "Azure AD nivåer lösenordssäkerhet | Microsoft Docs"
description: "Förklarar hur Azure AD tillämpar starka lösenord och skyddar användarnas lösenord från cyber kriminella"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 32464307ccb082b25538eaa522c1cdedef1ca555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a><span data-ttu-id="16612-103">En metod med flera nivåer för säkerhet för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-103">A multi-tiered approach to Azure AD password security</span></span>

<span data-ttu-id="16612-104">Den här artikeln beskrivs några metoder du kan följa som en användare eller administratör för att skydda din Azure Active Directory (AD Azure) eller Account.</span><span class="sxs-lookup"><span data-stu-id="16612-104">This article discusses some best practices you can follow as a user or as an administrator to protect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="16612-105">Azure AD-administratörer kan återställa användarlösenord med hjälp av anvisningarna i artikeln [återställa lösenordet för en användare i Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="16612-105">Azure AD administrators can reset user passwords using the guidance in the article [Reset the password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="16612-106">Användare kan återställa sina egna lösenord med hjälp av anvisningarna i artikeln [hjälp som jag har glömt mitt Azure AD-lösenord](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="16612-106">Users can reset their own password using the guidance in the article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="16612-107">Krav på lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-107">Password requirements</span></span>

<span data-ttu-id="16612-108">Azure AD använder följande vanliga metoder för att skydda lösenord:</span><span class="sxs-lookup"><span data-stu-id="16612-108">Azure AD incorporates the following common approaches to securing passwords:</span></span>

* <span data-ttu-id="16612-109">Krav på lösenordslängd</span><span class="sxs-lookup"><span data-stu-id="16612-109">Password length requirements</span></span>
* <span data-ttu-id="16612-110">Kraven på lösenordskomplexitet</span><span class="sxs-lookup"><span data-stu-id="16612-110">Password complexity requirements</span></span>
* <span data-ttu-id="16612-111">Regelbundet och periodiskt upphörande av lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="16612-112">Information om lösenordsåterställning i Azure Active Directory finns i avsnittet [Azure AD lösenordsåterställning via Självbetjäning för IT-proffs](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="16612-112">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="16612-113">Skydd med Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-113">Azure AD password protections</span></span>

<span data-ttu-id="16612-114">Azure AD och Microsofts kontosystem använder branschen beprövade metoder för att säkerställa säker skydd av användar- och lösenord, inklusive:</span><span class="sxs-lookup"><span data-stu-id="16612-114">Azure AD and the Microsoft Account System use industry proven approaches to ensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="16612-115">Lösenord som förbjuds dynamiskt</span><span class="sxs-lookup"><span data-stu-id="16612-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="16612-116">Smart lösenordsutelåsning</span><span class="sxs-lookup"><span data-stu-id="16612-116">Smart Password Lockout</span></span>

<span data-ttu-id="16612-117">Information om lösenordshantering baserat på aktuella research finns i vitboken [lösenord vägledning](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="16612-117">For information about password management based on current research, see the whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="16612-118">Lösenord som förbjuds dynamiskt</span><span class="sxs-lookup"><span data-stu-id="16612-118">Dynamically banned passwords</span></span>

<span data-ttu-id="16612-119">Azure AD och Microsoft Accounts skydda lösenordsskydd genom dynamiskt förbjuda vanliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="16612-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="16612-120">Azure ID Identity Protection-teamet analyserar rutinmässigt listor med förbjudna lösenord, och hindrar användare från att välja vanliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="16612-120">The Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="16612-121">Den här tjänsten är tillgänglig för Azure AD- och Microsoft Account Service-kunder.</span><span class="sxs-lookup"><span data-stu-id="16612-121">This service is available to Azure AD and the Microsoft Account Service customers.</span></span>

<span data-ttu-id="16612-122">När du skapar lösenord, är det viktigt för administratörer att uppmana användare att välja lösenord fraser som innehåller en unik kombination av bokstäver, siffror, tecknen eller orden.</span><span class="sxs-lookup"><span data-stu-id="16612-122">When creating passwords, it is important for administrators to encourage users to choose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="16612-123">Den här metoden kan du göra användarlösenord nästan omöjligt att vara komprometterad men enklare för användare att komma ihåg.</span><span class="sxs-lookup"><span data-stu-id="16612-123">This approach helps to make user passwords nearly impossible to be compromised but easier for users to remember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="16612-124">Överträdelser av lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-124">Password breaches</span></span>

<span data-ttu-id="16612-125">Microsoft arbetar alltid för att hålla ett steg i cyber kriminella.</span><span class="sxs-lookup"><span data-stu-id="16612-125">Microsoft is always working to stay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="16612-126">Azure AD Identity Protection-teamet analyserar kontinuerligt lösenord som används ofta.</span><span class="sxs-lookup"><span data-stu-id="16612-126">The Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="16612-127">Cyberbrottslingar använder också liknande strategier för sina attacker, t.ex. genom att skapa en [regnbågstabell](https://en.wikipedia.org/wiki/Rainbow_table) för att knäcka lösenordshashar.</span><span class="sxs-lookup"><span data-stu-id="16612-127">Cyber-criminals also use similar strategies to inform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="16612-128">Microsoft analyserar kontinuerligt [dataöverträdelser](https://www.privacyrights.org/data-breaches) och använder en dynamiskt uppdaterad lista med förbjudna lösenord, som gör att sårbara lösenord förbjuds innan de utgör ett verkligt hot för Azure AD-kunder.</span><span class="sxs-lookup"><span data-stu-id="16612-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) to maintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat to Azure AD customers.</span></span> <span data-ttu-id="16612-129">Mer information om vårt säkerhetsarbete finns i [Microsofts säkerhetsinformationsrapport](https://www.microsoft.com/security/sir/default.aspx).</span><span class="sxs-lookup"><span data-stu-id="16612-129">For more information about our current security efforts, see the [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="16612-130">Smart lösenordsutelåsning</span><span class="sxs-lookup"><span data-stu-id="16612-130">Smart Password Lockout</span></span>

<span data-ttu-id="16612-131">Om Azure AD upptäcker att en potentiell cyberbrottsling försöker hacka ett användarlösenord låser vi användarkontot med hjälp av smart lösenordsutelåsning.</span><span class="sxs-lookup"><span data-stu-id="16612-131">When Azure AD detects a potential cyber-criminal trying to hack into a user password, we lock the user account with Smart Password Lockout.</span></span> <span data-ttu-id="16612-132">Azure AD har utformats för att bedöma riskerna med specifika inloggningssessioner.</span><span class="sxs-lookup"><span data-stu-id="16612-132">Azure AD is designed to determine the risk associated with specific login sessions.</span></span> <span data-ttu-id="16612-133">Sedan använder den senaste informationen om säkerhet, det använda semantiken för kontoutelåsning för att stoppa cyber hot.</span><span class="sxs-lookup"><span data-stu-id="16612-133">Then using the most up-to-date security data, we apply lockout semantics to stop cyber threats.</span></span>

<span data-ttu-id="16612-134">Om en användare har låsts ute från Azure AD, liknar skärmen det som följer:</span><span class="sxs-lookup"><span data-stu-id="16612-134">If a user is locked out of Azure AD, their screen looks similar to the one that follows:</span></span>

  ![Utelåst från Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="16612-136">För andra Microsoft-konton liknar skärmen det som följer:</span><span class="sxs-lookup"><span data-stu-id="16612-136">For other Microsoft accounts, their screen looks similar to the one that follows:</span></span>

  ![Utelåst från ett Microsoft-konto](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="16612-138">Information om lösenordsåterställning i Azure Active Directory finns i avsnittet [Azure AD lösenordsåterställning via Självbetjäning för IT-proffs](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="16612-138">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="16612-139">Om du är en Azure AD-administratör kan du använda [Windows Hello](https://www.microsoft.com/windows/windows-hello) för att helt och hållet förhindra att användare skapar traditionella lösenord.</span><span class="sxs-lookup"><span data-stu-id="16612-139">If you are an Azure AD administrator, you may want to use [Windows Hello](https://www.microsoft.com/windows/windows-hello) to avoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="16612-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16612-140">Next steps</span></span>

* [<span data-ttu-id="16612-141">Uppdatera ditt eget lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-141">How to update your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="16612-142">Grunderna i Azures identitetshantering</span><span class="sxs-lookup"><span data-stu-id="16612-142">The fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="16612-143">Återställningsaktivitet för rapporten på lösenord</span><span class="sxs-lookup"><span data-stu-id="16612-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


