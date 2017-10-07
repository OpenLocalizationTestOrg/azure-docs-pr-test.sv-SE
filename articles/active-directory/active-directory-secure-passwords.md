---
title: "aaaAzure AD nivåer lösenordssäkerhet | Microsoft Docs"
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
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a><span data-ttu-id="7f0e8-103">En säkerhet för flera nivåer metoden tooAzure AD lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-103">A multi-tiered approach tooAzure AD password security</span></span>

<span data-ttu-id="7f0e8-104">Den här artikeln beskrivs några metoder du kan följa som en användare eller som en administratör tooprotect din Azure Active Directory (AD Azure) eller Account.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-104">This article discusses some best practices you can follow as a user or as an administrator tooprotect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="7f0e8-105">Azure AD-administratörer kan återställa användarlösenord med hello vägledning i hello artikel [hello Återställ lösenord för en användare i Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-105">Azure AD administrators can reset user passwords using hello guidance in hello article [Reset hello password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="7f0e8-106">Användare kan återställa sina egna lösenord med hjälp av hello vägledning i hello artikeln [hjälp som jag har glömt mitt Azure AD-lösenord](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-106">Users can reset their own password using hello guidance in hello article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="7f0e8-107">Krav på lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-107">Password requirements</span></span>

<span data-ttu-id="7f0e8-108">Azure AD omfattar hello följande vanliga metoder toosecuring lösenord:</span><span class="sxs-lookup"><span data-stu-id="7f0e8-108">Azure AD incorporates hello following common approaches toosecuring passwords:</span></span>

* <span data-ttu-id="7f0e8-109">Krav på lösenordslängd</span><span class="sxs-lookup"><span data-stu-id="7f0e8-109">Password length requirements</span></span>
* <span data-ttu-id="7f0e8-110">Kraven på lösenordskomplexitet</span><span class="sxs-lookup"><span data-stu-id="7f0e8-110">Password complexity requirements</span></span>
* <span data-ttu-id="7f0e8-111">Regelbundet och periodiskt upphörande av lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="7f0e8-112">Information om lösenordsåterställning i Azure Active Directory finns i avsnittet hello [Azure AD lösenordsåterställning via Självbetjäning för hello IT-proffs](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-112">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="7f0e8-113">Skydd med Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-113">Azure AD password protections</span></span>

<span data-ttu-id="7f0e8-114">Azure AD och hello Microsofts kontosystem använda branschen beprövade närmar sig tooensure säker skydd av användar- och lösenord, inklusive:</span><span class="sxs-lookup"><span data-stu-id="7f0e8-114">Azure AD and hello Microsoft Account System use industry proven approaches tooensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="7f0e8-115">Lösenord som förbjuds dynamiskt</span><span class="sxs-lookup"><span data-stu-id="7f0e8-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="7f0e8-116">Smart lösenordsutelåsning</span><span class="sxs-lookup"><span data-stu-id="7f0e8-116">Smart Password Lockout</span></span>

<span data-ttu-id="7f0e8-117">Information om lösenordshantering baserat på aktuella research finns hello whitepaper [lösenord vägledning](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-117">For information about password management based on current research, see hello whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="7f0e8-118">Lösenord som förbjuds dynamiskt</span><span class="sxs-lookup"><span data-stu-id="7f0e8-118">Dynamically banned passwords</span></span>

<span data-ttu-id="7f0e8-119">Azure AD och Microsoft Accounts skydda lösenordsskydd genom dynamiskt förbjuda vanliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="7f0e8-120">hello Azure ID Identity Protection-teamet analyserar regelbundet förbjudna lösenordslistor, vilket förhindrar användare från att välja vanliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-120">hello Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="7f0e8-121">Den här tjänsten är tillgänglig tooAzure AD och hello Microsoft-konto-kunder.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-121">This service is available tooAzure AD and hello Microsoft Account Service customers.</span></span>

<span data-ttu-id="7f0e8-122">När du skapar lösenord, är det viktigt för administratörer tooencourage användare toochoose lösenord fraser som innehåller en unik kombination av bokstäver, siffror, tecknen eller orden.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-122">When creating passwords, it is important for administrators tooencourage users toochoose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="7f0e8-123">Den här metoden kan nästan omöjligt toobe komprometteras men enklare för användare tooremember toomake lösenord.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-123">This approach helps toomake user passwords nearly impossible toobe compromised but easier for users tooremember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="7f0e8-124">Överträdelser av lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-124">Password breaches</span></span>

<span data-ttu-id="7f0e8-125">Microsoft arbetar alltid toostay ett steg i cyber kriminella.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-125">Microsoft is always working toostay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="7f0e8-126">hello Azure AD Identity Protection-teamet analyserar kontinuerligt lösenord som används ofta.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-126">hello Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="7f0e8-127">Cyber brottslingar också använder liknande strategier tooinform sina attacker, till exempel skapa en [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) för sprickbildning lösenordshashvärden.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-127">Cyber-criminals also use similar strategies tooinform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="7f0e8-128">Microsoft analyserar kontinuerligt [dataintrång](https://www.privacyrights.org/data-breaches) toomaintain en lista med dynamiskt uppdaterade förbjudna lösenord, vilket garanterar att sårbara lösenord är förbjuden innan de blir en verklig hot tooAzure AD-kunder.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) toomaintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat tooAzure AD customers.</span></span> <span data-ttu-id="7f0e8-129">Mer information om våra nuvarande säkerhet åtgärder finns hello [Microsoft Security Intelligence-rapporten](https://www.microsoft.com/security/sir/default.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-129">For more information about our current security efforts, see hello [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="7f0e8-130">Smart lösenordsutelåsning</span><span class="sxs-lookup"><span data-stu-id="7f0e8-130">Smart Password Lockout</span></span>

<span data-ttu-id="7f0e8-131">När Azure AD upptäcker ett potentiellt cyber straffrättsliga försök toohack till en användarlösenord, låsa vi hello användarkonto med Smart lösenord kontoutelåsning.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-131">When Azure AD detects a potential cyber-criminal trying toohack into a user password, we lock hello user account with Smart Password Lockout.</span></span> <span data-ttu-id="7f0e8-132">Azure AD är utformad toodetermine hello risker som är kopplade till specifika inloggningsessioner.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-132">Azure AD is designed toodetermine hello risk associated with specific login sessions.</span></span> <span data-ttu-id="7f0e8-133">Sedan använder hello senaste säkerhetsdata det använda kontoutelåsning semantik toostop cyber hot.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-133">Then using hello most up-to-date security data, we apply lockout semantics toostop cyber threats.</span></span>

<span data-ttu-id="7f0e8-134">Om en användare har låsts ute från Azure AD, ser skärmen liknande toohello som följer:</span><span class="sxs-lookup"><span data-stu-id="7f0e8-134">If a user is locked out of Azure AD, their screen looks similar toohello one that follows:</span></span>

  ![Utelåst från Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="7f0e8-136">För andra Microsoft-konton ut skärmen liknande toohello som följer:</span><span class="sxs-lookup"><span data-stu-id="7f0e8-136">For other Microsoft accounts, their screen looks similar toohello one that follows:</span></span>

  ![Utelåst från ett Microsoft-konto](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="7f0e8-138">Information om lösenordsåterställning i Azure Active Directory finns i avsnittet hello [Azure AD lösenordsåterställning via Självbetjäning för hello IT-proffs](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="7f0e8-138">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="7f0e8-139">Om du är en Azure AD-administratör vill du kanske toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid med dina användare skapa traditionella lösenord helt och hållet.</span><span class="sxs-lookup"><span data-stu-id="7f0e8-139">If you are an Azure AD administrator, you may want toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="7f0e8-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f0e8-140">Next steps</span></span>

* [<span data-ttu-id="7f0e8-141">Hur tooupdate ditt eget lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-141">How tooupdate your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="7f0e8-142">hello grunderna i Azure Identitetshantering</span><span class="sxs-lookup"><span data-stu-id="7f0e8-142">hello fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="7f0e8-143">Återställningsaktivitet för rapporten på lösenord</span><span class="sxs-lookup"><span data-stu-id="7f0e8-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


