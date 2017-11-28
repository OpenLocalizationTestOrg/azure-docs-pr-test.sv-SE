---
title: 'Azure Active Directory B2C: Inbyggda principer | Microsoft Docs'
description: "Ett ämne på hello expanderbara principramverk av Azure Active Directory B2C och hur toocreate olika principtyper"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: 24bb85eba30f888c6ea7d0401e05235e8eb6ea79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a><span data-ttu-id="8a6ac-103">Azure Active Directory B2C: Inbyggda principer</span><span class="sxs-lookup"><span data-stu-id="8a6ac-103">Azure Active Directory B2C: Built-in policies</span></span>


<span data-ttu-id="8a6ac-104">hello expanderbara principramverk av Azure Active Directory (AD Azure) B2C är hello core styrkan hos hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-104">hello extensible policy framework of Azure Active Directory (Azure AD) B2C is hello core strength of hello service.</span></span> <span data-ttu-id="8a6ac-105">Principer fullständigt beskriver konsumenten identitetsupplevelser som registrering, inloggning eller Redigera profil.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-105">Policies fully describe consumer identity experiences such as sign-up, sign-in, or profile editing.</span></span> <span data-ttu-id="8a6ac-106">Till exempel kan en princip för registrering du toocontrol beteenden genom att konfigurera hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="8a6ac-106">For instance, a sign-up policy allows you toocontrol behaviors by configuring hello following settings:</span></span>

* <span data-ttu-id="8a6ac-107">Kontotyp (sociala konton som Facebook) eller lokala konton, till exempel e-postadresser som konsumenterna kan använda toosign för programmet hello</span><span class="sxs-lookup"><span data-stu-id="8a6ac-107">Account types (social accounts such as Facebook or local accounts such as email addresses) that consumers can use toosign up for hello application</span></span>
* <span data-ttu-id="8a6ac-108">Attribut (till exempel förnamn, postnummer och sko storlek) toobe samlas in från hello konsumenten under registreringen</span><span class="sxs-lookup"><span data-stu-id="8a6ac-108">Attributes (for example, first name, postal code, and shoe size) toobe collected from hello consumer during sign-up</span></span>
* <span data-ttu-id="8a6ac-109">Användning av Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="8a6ac-109">Use of Azure Multi-Factor Authentication</span></span>
* <span data-ttu-id="8a6ac-110">hello utseendet på alla sidor för registrering</span><span class="sxs-lookup"><span data-stu-id="8a6ac-110">hello look and feel of all sign-up pages</span></span>
* <span data-ttu-id="8a6ac-111">Information (som visar som anspråk i en token) som hello programmet tar emot när hello princip kör är klar</span><span class="sxs-lookup"><span data-stu-id="8a6ac-111">Information (which manifests as claims in a token) that hello application receives when hello policy run finishes</span></span>

<span data-ttu-id="8a6ac-112">Du kan skapa flera principer för olika typer i din klient och använda dem i dina program efter behov.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-112">You can create multiple policies of different types in your tenant and use them in your applications as needed.</span></span> <span data-ttu-id="8a6ac-113">Principer kan återanvändas i program.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-113">Policies can be reused across applications.</span></span> <span data-ttu-id="8a6ac-114">Den här flexibiliteten gör det möjligt för utvecklare toodefine och ändra konsumenten identitetsupplevelser med minimal eller inga ändringar tootheir kod.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-114">This flexibility enables developers toodefine and modify consumer identity experiences with minimal or no changes tootheir code.</span></span>

<span data-ttu-id="8a6ac-115">Principer kan användas via en enkel developer-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-115">Policies are available for use via a simple developer interface.</span></span> <span data-ttu-id="8a6ac-116">Programmet utlöser en princip med hjälp av en standard HTTP autentiseringsbegäran (skicka en princip för parameter i hello begäran) och får en anpassad token som svar.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-116">Your application triggers a policy by using a standard HTTP authentication request (passing a policy parameter in hello request) and receives a customized token as response.</span></span> <span data-ttu-id="8a6ac-117">Hello enda skillnaden mellan förfrågningar som anropar en princip för registrering och förfrågningar som anropar en princip för inloggning är till exempel hello principnamn som används i hello ”p” frågesträngparametern:</span><span class="sxs-lookup"><span data-stu-id="8a6ac-117">For example, hello only difference between requests that invoke a sign-up policy and requests that invoke a sign-in policy is hello policy name that's used in hello "p" query string parameter:</span></span>

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

<span data-ttu-id="8a6ac-118">Läs mer om hello principramverk [i det här blogginlägget om Azure AD B2C på hello Enterprise Mobility och Säkerhetsblogg](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a6ac-118">For more information about hello policy framework, see [this blog post about Azure AD B2C on hello Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span></span>

## <a name="create-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="8a6ac-119">Skapa en princip för registrering eller inloggning</span><span class="sxs-lookup"><span data-stu-id="8a6ac-119">Create a sign-up or sign-in policy</span></span>

<span data-ttu-id="8a6ac-120">Den här principen hanterar upplevelser för registrering och inloggning i båda konsumenten med en enda konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-120">This policy handles both consumer sign-up & sign-in experiences with a single configuration.</span></span> <span data-ttu-id="8a6ac-121">Konsumenterna vägleds ned hello rätt sökväg (registrering eller inloggning) beroende på hello kontext.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-121">Consumers are led down hello right path (sign-up or sign-in) depending on hello context.</span></span> <span data-ttu-id="8a6ac-122">Här beskrivs också hello innehållet i token som programmet hello får lyckats logga in eller inloggningar.  En kodexempel för hello-princip för registrering eller inloggning är [tillgänglig här](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="8a6ac-122">It also describes hello contents of tokens that hello application will receive upon successful sign-ups or sign-ins.  A code sample for hello sign-up or sign-in policy is [available here](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>  <span data-ttu-id="8a6ac-123">Är det rekommenderade att du använder den här principen via en principen för registrering och inloggning princip.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-123">It is recommened that you use this policy over a sign-up policy and sign-in policy.</span></span>  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a><span data-ttu-id="8a6ac-124">Skapa en princip för registrering</span><span class="sxs-lookup"><span data-stu-id="8a6ac-124">Create a sign-up policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a><span data-ttu-id="8a6ac-125">Skapa en princip för inloggning</span><span class="sxs-lookup"><span data-stu-id="8a6ac-125">Create a sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a><span data-ttu-id="8a6ac-126">Skapa en profil Redigera princip</span><span class="sxs-lookup"><span data-stu-id="8a6ac-126">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a><span data-ttu-id="8a6ac-127">Skapa en princip för lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="8a6ac-127">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a><span data-ttu-id="8a6ac-128">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8a6ac-128">Frequently asked questions</span></span>

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a><span data-ttu-id="8a6ac-129">Hur länkar en princip för registrering eller inloggning med en princip för lösenordsåterställning?</span><span class="sxs-lookup"><span data-stu-id="8a6ac-129">How do I link a sign-up or sign-in policy with a password reset policy?</span></span>
<span data-ttu-id="8a6ac-130">När du skapar en princip för registrering eller inloggning (med lokala konton) visas en **har du glömt lösenordet?** länk på hello första sidan i hello upplevelse.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-130">When you create a sign-up or sign-in policy (with local accounts), you see a **Forgot password?** link on hello first page of hello experience.</span></span> <span data-ttu-id="8a6ac-131">Klicka på den här länken återställs inte utlösaren lösenord automatiskt princip.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-131">Clicking this link doesn't automatically trigger a password reset policy.</span></span> 

<span data-ttu-id="8a6ac-132">I stället hello felkoden  **`AADB2C90118`**  returneras tooyour app.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-132">Instead, hello error code **`AADB2C90118`** is returned tooyour app.</span></span> <span data-ttu-id="8a6ac-133">Appen måste toohandle felet genom att aktivera en specifik princip för lösenordsåterställning.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-133">Your app needs toohandle this error code by invoking a specific password reset policy.</span></span> <span data-ttu-id="8a6ac-134">Mer information finns i en [exempel som visar hello-metod länka principer för](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span><span class="sxs-lookup"><span data-stu-id="8a6ac-134">For more information, see a [sample that demonstrates hello approach of linking policies](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span></span>

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a><span data-ttu-id="8a6ac-135">Bör jag använda en princip för registrering eller inloggning eller en princip för registrering och en princip för inloggning?</span><span class="sxs-lookup"><span data-stu-id="8a6ac-135">Should I use a sign-up or sign-in policy or a sign-up policy and a sign-in policy?</span></span>
<span data-ttu-id="8a6ac-136">Vi rekommenderar att du använder en princip för registrering eller inloggning via en princip för registrering och en princip för inloggning.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-136">We recommend that you use a sign-up or sign-in policy over a sign-up policy and a sign-in policy.</span></span>  

<span data-ttu-id="8a6ac-137">hello-princip för registrering eller inloggning har fler funktioner än hello inloggningsprincip.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-137">hello sign-up or sign-in policy has more capabilities than hello sign-in policy.</span></span> <span data-ttu-id="8a6ac-138">Dessutom kan du toouse sidan anpassningar och har bättre stöd för lokalisering.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-138">It also enables you toouse page UI customization and has better support for localization.</span></span> 

<span data-ttu-id="8a6ac-139">Hej inloggningsprincip rekommenderas om du inte behöver toolocalize dina principer, bara behöver mindre anpassningsmöjligheter för anpassning och vill lösenord återställning som är inbyggda i den.</span><span class="sxs-lookup"><span data-stu-id="8a6ac-139">hello sign-in policy is recommended if you don't need toolocalize your policies, only need minor customization capabilities for branding, and want password reset built into it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a6ac-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a6ac-140">Next steps</span></span>
* [<span data-ttu-id="8a6ac-141">Token, session och konfiguration för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8a6ac-141">Token, session, and single sign-on configuration</span></span>](active-directory-b2c-token-session-sso.md)
* [<span data-ttu-id="8a6ac-142">Inaktivera e-Postverifiering under konsumenten registrering</span><span class="sxs-lookup"><span data-stu-id="8a6ac-142">Disable email verification during consumer sign-up</span></span>](active-directory-b2c-reference-disable-ev.md)

