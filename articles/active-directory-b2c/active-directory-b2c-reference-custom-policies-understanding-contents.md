---
title: "Azure Active Directory B2C: Förstå anpassade principer för hello startpaket | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 3484e8cc6fa6a9d57c2aa14c0cc9616065892d10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="a8f93-103">Förstå hello anpassade principer för startpaket för hello Azure AD B2C-anpassad princip</span><span class="sxs-lookup"><span data-stu-id="a8f93-103">Understanding hello custom policies of hello Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="a8f93-104">Det här avsnittet innehåller alla hello grundelementen i hello B2C_1A_base princip som medföljer hello **startpaket** och som utnyttjas för att skapa dina egna principer via hello arv av hello *B2C_1A_base_ princip för tillägg*.</span><span class="sxs-lookup"><span data-stu-id="a8f93-104">This section lists all hello core elements of hello B2C_1A_base policy that comes with hello **Starter Pack** and that is leveraged for authoring your own policies through hello inheritance of hello *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="a8f93-105">Därför det mer fokuserar särskilt på hello som redan har definierats anspråk typer, anspråksomvandlingar, definitioner av innehåll, Anspråksproviders med deras tekniska eller profilerna och hello core användaren resor.</span><span class="sxs-lookup"><span data-stu-id="a8f93-105">As such, it more particularly focusses on hello already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and hello core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8f93-106">Microsoft lämnar inga garantier, uttryckliga eller underförstådda, avseende toohello information tillhandahålls nedan.</span><span class="sxs-lookup"><span data-stu-id="a8f93-106">Microsoft makes no warranties, express or implied, with respect toohello information provided hereafter.</span></span> <span data-ttu-id="a8f93-107">Ändringar kan införas när som helst före GA tiden GA tidpunkt eller efter.</span><span class="sxs-lookup"><span data-stu-id="a8f93-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="a8f93-108">Både dina egna principer och hello B2C_1A_base_extensions princip kan åsidosätta dessa definitioner och utöka överordnade principen genom att tillhandahålla ytterligare dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="a8f93-108">Both your own policies and hello B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="a8f93-109">Hej grundelementen i hello *B2C_1A_base princip* är anspråkstyper och anspråksomvandlingar innehåll definitioner.</span><span class="sxs-lookup"><span data-stu-id="a8f93-109">hello core elements of hello *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="a8f93-110">De här elementen kan mottagliga toobe som refereras i dina egna principer samt som hello *B2C_1A_base_extensions princip*.</span><span class="sxs-lookup"><span data-stu-id="a8f93-110">These elements can susceptible toobe referenced in your own policies as well as in hello *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="a8f93-111">Anspråk scheman</span><span class="sxs-lookup"><span data-stu-id="a8f93-111">Claims schemas</span></span>

<span data-ttu-id="a8f93-112">Detta anspråk scheman är indelat i tre delar:</span><span class="sxs-lookup"><span data-stu-id="a8f93-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="a8f93-113">Ett första avsnitt som visar hello minsta anspråk som krävs för hello användaren resor toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="a8f93-113">A first section that lists hello minimum claims that are required for hello user journeys toowork properly.</span></span>
2.  <span data-ttu-id="a8f93-114">Ett andra avsnitt som visar hello anspråk som krävs för frågan string-parametrar och andra toobe särskilda parametrar skickades tooother Anspråksproviders, särskilt login.microsoftonline.com för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8f93-114">A second section that lists hello claims required for query string parameters and other special parameters toobe passed tooother claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="a8f93-115">**Ändra inte de här anspråken**.</span><span class="sxs-lookup"><span data-stu-id="a8f93-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="a8f93-116">Och slutligen ett tredje avsnitt som visar en lista över ytterligare och valfria anspråk som kan samlas in från hello användare lagras i hello directory skickas i token under inloggningen.</span><span class="sxs-lookup"><span data-stu-id="a8f93-116">And eventually a third section that lists any additional, optional claims that can be collected from hello user, stored in hello directory and sent in tokens during sign in.</span></span> <span data-ttu-id="a8f93-117">Nya anspråk typen toobe samlas in från hello användare och/eller skickas i hello token kan läggas till i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a8f93-117">New claims type toobe collected from hello user and/or sent in hello token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8f93-118">hello anspråk schemat innehåller begränsningar för vissa anspråk som lösenord och användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a8f93-118">hello claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="a8f93-119">hello förtroende Framework (TF) princip behandlar Azure AD som andra anspråksprovider och dess begränsningar utformas i hello premium princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-119">hello Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in hello premium policy.</span></span> <span data-ttu-id="a8f93-120">En princip kunde ändrade tooadd fler begränsningar, eller Använd en annan anspråksprovider för lagring av autentiseringsuppgifter som har sin egen begränsningar.</span><span class="sxs-lookup"><span data-stu-id="a8f93-120">A policy could be modified tooadd more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="a8f93-121">hello tillgängliga anspråkstyper visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a8f93-121">hello available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-hello-user-journeys"></a><span data-ttu-id="a8f93-122">Anspråk som krävs för hello användaren resor</span><span class="sxs-lookup"><span data-stu-id="a8f93-122">Claims that are required for hello user journeys</span></span>

<span data-ttu-id="a8f93-123">Det krävs hello följande anspråk för användaren resor toowork korrekt:</span><span class="sxs-lookup"><span data-stu-id="a8f93-123">hello following claims are required for user journeys toowork properly:</span></span>

| <span data-ttu-id="a8f93-124">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="a8f93-124">Claims type</span></span> | <span data-ttu-id="a8f93-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="a8f93-126">*Användar-ID*</span><span class="sxs-lookup"><span data-stu-id="a8f93-126">*UserId*</span></span> | <span data-ttu-id="a8f93-127">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="a8f93-127">Username</span></span> |
| <span data-ttu-id="a8f93-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-128">*signInName*</span></span> | <span data-ttu-id="a8f93-129">Logga in namn</span><span class="sxs-lookup"><span data-stu-id="a8f93-129">Sign in name</span></span> |
| <span data-ttu-id="a8f93-130">*klient-ID*</span><span class="sxs-lookup"><span data-stu-id="a8f93-130">*tenantId*</span></span> | <span data-ttu-id="a8f93-131">Klient ID-Numret för hello användarobjekt i Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="a8f93-131">Tenant identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="a8f93-132">*objekt-ID*</span><span class="sxs-lookup"><span data-stu-id="a8f93-132">*objectId*</span></span> | <span data-ttu-id="a8f93-133">Objekt-ID (ID) hello användarobjektet i Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="a8f93-133">Object identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="a8f93-134">*lösenord*</span><span class="sxs-lookup"><span data-stu-id="a8f93-134">*password*</span></span> | <span data-ttu-id="a8f93-135">Lösenord</span><span class="sxs-lookup"><span data-stu-id="a8f93-135">Password</span></span> |
| <span data-ttu-id="a8f93-136">*nytt lösenord*</span><span class="sxs-lookup"><span data-stu-id="a8f93-136">*newPassword*</span></span> | |
| <span data-ttu-id="a8f93-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="a8f93-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="a8f93-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="a8f93-138">*passwordPolicies*</span></span> | <span data-ttu-id="a8f93-139">Lösenordsprinciper som används av Azure AD B2C Premium toodetermine lösenordssäkerhet, upphör att gälla, osv.</span><span class="sxs-lookup"><span data-stu-id="a8f93-139">Password policies used by Azure AD B2C Premium toodetermine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="a8f93-140">*Sub*</span><span class="sxs-lookup"><span data-stu-id="a8f93-140">*sub*</span></span> | |
| <span data-ttu-id="a8f93-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="a8f93-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="a8f93-142">*identityProvider*</span></span> | |
| <span data-ttu-id="a8f93-143">*Visningsnamn*</span><span class="sxs-lookup"><span data-stu-id="a8f93-143">*displayName*</span></span> | |
| <span data-ttu-id="a8f93-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="a8f93-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="a8f93-145">Användarens telefonnummer</span><span class="sxs-lookup"><span data-stu-id="a8f93-145">User's telephone number</span></span> |
| <span data-ttu-id="a8f93-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="a8f93-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="a8f93-147">*e-post*</span><span class="sxs-lookup"><span data-stu-id="a8f93-147">*email*</span></span> | <span data-ttu-id="a8f93-148">E-postadress som kan vara används toocontact hello användare</span><span class="sxs-lookup"><span data-stu-id="a8f93-148">Email address that can be used toocontact hello user</span></span> |
| <span data-ttu-id="a8f93-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="a8f93-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="a8f93-150">E-postadress som hello användare kan använda toosign i</span><span class="sxs-lookup"><span data-stu-id="a8f93-150">Email address that hello user can use toosign in</span></span> |
| <span data-ttu-id="a8f93-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="a8f93-151">*otherMails*</span></span> | <span data-ttu-id="a8f93-152">E-postadresser som kan vara används toocontact hello användare</span><span class="sxs-lookup"><span data-stu-id="a8f93-152">Email addresses that can be used toocontact hello user</span></span> |
| <span data-ttu-id="a8f93-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-153">*userPrincipalName*</span></span> | <span data-ttu-id="a8f93-154">Användarnamnet som lagras i hello Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="a8f93-154">Username as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="a8f93-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-155">*upnUserName*</span></span> | <span data-ttu-id="a8f93-156">Användarnamn för att skapa användarens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="a8f93-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="a8f93-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-157">*mailNickName*</span></span> | <span data-ttu-id="a8f93-158">Användarens e-smeknamn som lagras i hello Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="a8f93-158">User's mail nick name as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="a8f93-159">*ny användare*</span><span class="sxs-lookup"><span data-stu-id="a8f93-159">*newUser*</span></span> | |
| <span data-ttu-id="a8f93-160">*köra SelfAsserted-inmatning*</span><span class="sxs-lookup"><span data-stu-id="a8f93-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="a8f93-161">Anspråk som anger om attribut samlades in från hello användare</span><span class="sxs-lookup"><span data-stu-id="a8f93-161">Claim that specifies whether attributes were collected from hello user</span></span> |
| <span data-ttu-id="a8f93-162">*köra PhoneFactor-inmatning*</span><span class="sxs-lookup"><span data-stu-id="a8f93-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="a8f93-163">Anspråk som anger om ett nytt telefonnummer samlats in från hello användare</span><span class="sxs-lookup"><span data-stu-id="a8f93-163">Claim that specifies whether a new phone number was collected from hello user</span></span> |
| <span data-ttu-id="a8f93-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="a8f93-164">*authenticationSource*</span></span> | <span data-ttu-id="a8f93-165">Anger om hello användaren autentiserades på sociala identitetsleverantör, login.microsoftonline.com eller lokalt konto</span><span class="sxs-lookup"><span data-stu-id="a8f93-165">Specifies whether hello user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="a8f93-166">Anspråk som krävs för frågan string-parametrar och andra särskilda parametrar</span><span class="sxs-lookup"><span data-stu-id="a8f93-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="a8f93-167">hello är följande anspråk obligatoriska toopass på särskilda parametrar (inklusive vissa frågeparametrar sträng) tooother Anspråksproviders:</span><span class="sxs-lookup"><span data-stu-id="a8f93-167">hello following claims are required toopass on special parameters (including some query string parameters) tooother claims providers:</span></span>

| <span data-ttu-id="a8f93-168">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="a8f93-168">Claims type</span></span> | <span data-ttu-id="a8f93-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="a8f93-170">*nux*</span><span class="sxs-lookup"><span data-stu-id="a8f93-170">*nux*</span></span> | <span data-ttu-id="a8f93-171">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-171">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-172">*NCA*</span><span class="sxs-lookup"><span data-stu-id="a8f93-172">*nca*</span></span> | <span data-ttu-id="a8f93-173">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-173">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-174">*kommandotolk*</span><span class="sxs-lookup"><span data-stu-id="a8f93-174">*prompt*</span></span> | <span data-ttu-id="a8f93-175">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-175">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="a8f93-176">*mkt*</span></span> | <span data-ttu-id="a8f93-177">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-177">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-178">*LC*</span><span class="sxs-lookup"><span data-stu-id="a8f93-178">*lc*</span></span> | <span data-ttu-id="a8f93-179">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-179">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="a8f93-180">*grant_type*</span></span> | <span data-ttu-id="a8f93-181">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-181">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-182">*omfång*</span><span class="sxs-lookup"><span data-stu-id="a8f93-182">*scope*</span></span> | <span data-ttu-id="a8f93-183">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-183">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="a8f93-184">*client_id*</span></span> | <span data-ttu-id="a8f93-185">Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a8f93-185">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="a8f93-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="a8f93-186">*objectIdFromSession*</span></span> | <span data-ttu-id="a8f93-187">Parametern som angetts av hello standard session management provider tooindicate som hello objekt-id har hämtats från en SSO-session</span><span class="sxs-lookup"><span data-stu-id="a8f93-187">Parameter provided by hello default session management provider tooindicate that hello object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="a8f93-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="a8f93-188">*isActiveMFASession*</span></span> | <span data-ttu-id="a8f93-189">Parametern som tillhandahålls av hello MFA session management tooindicate hello användaren har en aktiv session MFA</span><span class="sxs-lookup"><span data-stu-id="a8f93-189">Parameter provided by hello MFA session management tooindicate that hello user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="a8f93-190">Ytterligare (valfritt) anspråk som kan samlas in</span><span class="sxs-lookup"><span data-stu-id="a8f93-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="a8f93-191">hello följande anspråk ytterligare anspråk som kan samlas in från hello användare lagras i hello directory, och skickas i hello-token.</span><span class="sxs-lookup"><span data-stu-id="a8f93-191">hello following claims are additional claims that can be collected from hello users, stored in hello directory, and sent in hello token.</span></span> <span data-ttu-id="a8f93-192">Enligt innan kan ytterligare anspråk läggas till toothis lista.</span><span class="sxs-lookup"><span data-stu-id="a8f93-192">As outlined before, additional claims can be added toothis list.</span></span>

| <span data-ttu-id="a8f93-193">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="a8f93-193">Claims type</span></span> | <span data-ttu-id="a8f93-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="a8f93-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-195">*givenName*</span></span> | <span data-ttu-id="a8f93-196">Användarens förnamn (även kallat Förnamn)</span><span class="sxs-lookup"><span data-stu-id="a8f93-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="a8f93-197">*Efternamn*</span><span class="sxs-lookup"><span data-stu-id="a8f93-197">*surname*</span></span> | <span data-ttu-id="a8f93-198">Användarens efternamn (även kallat namn eller efternamn)</span><span class="sxs-lookup"><span data-stu-id="a8f93-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="a8f93-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="a8f93-199">*Extension_picture*</span></span> | <span data-ttu-id="a8f93-200">Användarens bild från sociala</span><span class="sxs-lookup"><span data-stu-id="a8f93-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="a8f93-201">Anspråksomvandlingar</span><span class="sxs-lookup"><span data-stu-id="a8f93-201">Claim transformations</span></span>

<span data-ttu-id="a8f93-202">hello tillgängliga anspråksomvandlingar visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a8f93-202">hello available claim transformations are listed below.</span></span>

| <span data-ttu-id="a8f93-203">Anspråksomvandling av</span><span class="sxs-lookup"><span data-stu-id="a8f93-203">Claim transformation</span></span> | <span data-ttu-id="a8f93-204">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="a8f93-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="a8f93-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="a8f93-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="a8f93-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="a8f93-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="a8f93-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="a8f93-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="a8f93-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="a8f93-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="a8f93-211">Definitioner för innehåll</span><span class="sxs-lookup"><span data-stu-id="a8f93-211">Content definitions</span></span>

<span data-ttu-id="a8f93-212">Det här avsnittet beskrivs hello innehåll definitioner har redan deklarerats i hello *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-212">This section describes hello content definitions already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="a8f93-213">Dessa definitioner av innehåll är mottagliga toobe refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-213">These content definitions are susceptible toobe referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="a8f93-214">Anspråksleverantör</span><span class="sxs-lookup"><span data-stu-id="a8f93-214">Claims provider</span></span> | <span data-ttu-id="a8f93-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="a8f93-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="a8f93-216">*Facebook*</span></span> | |
| <span data-ttu-id="a8f93-217">*Logga in lokalt konto*</span><span class="sxs-lookup"><span data-stu-id="a8f93-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="a8f93-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="a8f93-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="a8f93-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="a8f93-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="a8f93-220">*Self vars*</span><span class="sxs-lookup"><span data-stu-id="a8f93-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="a8f93-221">*Lokalt konto*</span><span class="sxs-lookup"><span data-stu-id="a8f93-221">*Local Account*</span></span> | |
| <span data-ttu-id="a8f93-222">*Sessionshantering*</span><span class="sxs-lookup"><span data-stu-id="a8f93-222">*Session Management*</span></span> | |
| <span data-ttu-id="a8f93-223">*Trustframework principmodulen*</span><span class="sxs-lookup"><span data-stu-id="a8f93-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="a8f93-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="a8f93-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="a8f93-225">*Token utfärdare*</span><span class="sxs-lookup"><span data-stu-id="a8f93-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="a8f93-226">Tekniska profiler</span><span class="sxs-lookup"><span data-stu-id="a8f93-226">Technical profiles</span></span>

<span data-ttu-id="a8f93-227">Det här avsnittet visar hello tekniska profiler har redan deklarerats per anspråksleverantör i hello *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-227">This section depicts hello technical profiles already declared per claim provider in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="a8f93-228">Dessa tekniska profiler är mottagliga toobe ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-228">These technical profiles are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="a8f93-229">Tekniska profiler för Facebook</span><span class="sxs-lookup"><span data-stu-id="a8f93-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="a8f93-230">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-230">Technical profile</span></span> | <span data-ttu-id="a8f93-231">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-232">*Facebook-OAUTH*</span><span class="sxs-lookup"><span data-stu-id="a8f93-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="a8f93-233">Tekniska profiler för lokal inloggning för kontot</span><span class="sxs-lookup"><span data-stu-id="a8f93-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="a8f93-234">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-234">Technical profile</span></span> | <span data-ttu-id="a8f93-235">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-236">*Logga in utan interaktivitet*</span><span class="sxs-lookup"><span data-stu-id="a8f93-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="a8f93-237">Tekniska profiler för Phonefactor</span><span class="sxs-lookup"><span data-stu-id="a8f93-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="a8f93-238">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-238">Technical profile</span></span> | <span data-ttu-id="a8f93-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-240">*PhoneFactor-indata*</span><span class="sxs-lookup"><span data-stu-id="a8f93-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="a8f93-241">*PhoneFactor-InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="a8f93-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="a8f93-242">*PhoneFactor-verifiera*</span><span class="sxs-lookup"><span data-stu-id="a8f93-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="a8f93-243">Tekniska profiler för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8f93-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="a8f93-244">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-244">Technical profile</span></span> | <span data-ttu-id="a8f93-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-246">*AAD-gemensamma*</span><span class="sxs-lookup"><span data-stu-id="a8f93-246">*AAD-Common*</span></span> | <span data-ttu-id="a8f93-247">Tekniska profil inkluderas genom hello andra tekniska AAD-xxx-profiler</span><span class="sxs-lookup"><span data-stu-id="a8f93-247">Technical profile included by hello other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="a8f93-248">*AAD-UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="a8f93-249">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="a8f93-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="a8f93-250">*AAD-UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="a8f93-251">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="a8f93-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="a8f93-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span><span class="sxs-lookup"><span data-stu-id="a8f93-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="a8f93-253">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="a8f93-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="a8f93-254">*AAD-UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="a8f93-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="a8f93-255">Tekniska profil för lokala konton</span><span class="sxs-lookup"><span data-stu-id="a8f93-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="a8f93-256">*AAD-UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="a8f93-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="a8f93-257">Tekniska profil för lokala konton</span><span class="sxs-lookup"><span data-stu-id="a8f93-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="a8f93-258">*AAD-UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="a8f93-259">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="a8f93-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="a8f93-260">*AAD-UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="a8f93-261">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="a8f93-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="a8f93-262">*AAD-UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="a8f93-263">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="a8f93-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="a8f93-264">*AAD-UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="a8f93-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="a8f93-265">Tekniska profil är används tooread data när användaren autentiseras</span><span class="sxs-lookup"><span data-stu-id="a8f93-265">Technical profile is used tooread data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="a8f93-266">Tekniska profiler för Self vars</span><span class="sxs-lookup"><span data-stu-id="a8f93-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="a8f93-267">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-267">Technical profile</span></span> | <span data-ttu-id="a8f93-268">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-269">*SelfAsserted Social*</span><span class="sxs-lookup"><span data-stu-id="a8f93-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="a8f93-270">*SelfAsserted ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="a8f93-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="a8f93-271">Tekniska profiler för lokala konton</span><span class="sxs-lookup"><span data-stu-id="a8f93-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="a8f93-272">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-272">Technical profile</span></span> | <span data-ttu-id="a8f93-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="a8f93-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="a8f93-275">Tekniska profiler för sessionshantering</span><span class="sxs-lookup"><span data-stu-id="a8f93-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="a8f93-276">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-276">Technical profile</span></span> | <span data-ttu-id="a8f93-277">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-278">*SM-Noop*</span><span class="sxs-lookup"><span data-stu-id="a8f93-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="a8f93-279">*SM-AAD*</span><span class="sxs-lookup"><span data-stu-id="a8f93-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="a8f93-280">*SM-SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="a8f93-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="a8f93-281">Profilnamnet som används toodisambiguate AAD session mellan logga in och logga in</span><span class="sxs-lookup"><span data-stu-id="a8f93-281">Profile name is being used toodisambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="a8f93-282">*SM-SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="a8f93-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="a8f93-283">*SM-MFA*</span><span class="sxs-lookup"><span data-stu-id="a8f93-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="a8f93-284">Tekniska profiler för Trustframework princip motorn TechnicalProfiles</span><span class="sxs-lookup"><span data-stu-id="a8f93-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="a8f93-285">För närvarande inga tekniska profiler definieras för hello **Trustframework princip motorn TechnicalProfiles** anspråksleverantör.</span><span class="sxs-lookup"><span data-stu-id="a8f93-285">Currently, no technical profiles are defined for hello **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="a8f93-286">Tekniska profiler för tokenutfärdare</span><span class="sxs-lookup"><span data-stu-id="a8f93-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="a8f93-287">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="a8f93-287">Technical profile</span></span> | <span data-ttu-id="a8f93-288">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="a8f93-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="a8f93-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="a8f93-290">Användaren resor</span><span class="sxs-lookup"><span data-stu-id="a8f93-290">User journeys</span></span>

<span data-ttu-id="a8f93-291">Det här avsnittet visar hello användaren resor redan deklarerats i hello *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-291">This section depicts hello user journeys already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="a8f93-292">Dessa användare resor är mottagliga toobe ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="a8f93-292">These user journeys are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="a8f93-293">Användaren resa</span><span class="sxs-lookup"><span data-stu-id="a8f93-293">User journey</span></span> | <span data-ttu-id="a8f93-294">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8f93-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="a8f93-295">*Registreringen*</span><span class="sxs-lookup"><span data-stu-id="a8f93-295">*SignUp*</span></span> | |
| <span data-ttu-id="a8f93-296">*Logga in*</span><span class="sxs-lookup"><span data-stu-id="a8f93-296">*SignIn*</span></span> | |
| <span data-ttu-id="a8f93-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="a8f93-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="a8f93-298">*EditProfile*</span><span class="sxs-lookup"><span data-stu-id="a8f93-298">*EditProfile*</span></span> | |
| <span data-ttu-id="a8f93-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="a8f93-299">*PasswordReset*</span></span> | |
