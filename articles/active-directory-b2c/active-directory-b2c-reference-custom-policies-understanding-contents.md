---
title: "Azure Active Directory B2C: Förstå anpassade principer för starter pack | Microsoft Docs"
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
ms.openlocfilehash: 9847bcfcc139a769847678c1cca6a8b9c3a30e93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="212cd-103">Så här fungerar på Azure AD B2C anpassad princip startpaket anpassade principer</span><span class="sxs-lookup"><span data-stu-id="212cd-103">Understanding the custom policies of the Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="212cd-104">Det här avsnittet innehåller alla kärnor element i B2C_1A_base principen som medföljer den **startpaket** och som utnyttjas för att skapa dina egna principer genom arv av den *B2C_1A_base_extensions princip* .</span><span class="sxs-lookup"><span data-stu-id="212cd-104">This section lists all the core elements of the B2C_1A_base policy that comes with the **Starter Pack** and that is leveraged for authoring your own policies through the inheritance of the *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="212cd-105">Därför fokuserar den särskilt på redan definierad anspråkstyper, anspråksomvandlingar, definitioner av innehåll, Anspråksproviders med deras tekniska eller profilerna och core användaren resor.</span><span class="sxs-lookup"><span data-stu-id="212cd-105">As such, it more particularly focusses on the already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and the core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="212cd-106">Microsoft lämnar inga garantier, uttryckliga eller underförstådda, avseende informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="212cd-106">Microsoft makes no warranties, express or implied, with respect to the information provided hereafter.</span></span> <span data-ttu-id="212cd-107">Ändringar kan införas när som helst före GA tiden GA tidpunkt eller efter.</span><span class="sxs-lookup"><span data-stu-id="212cd-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="212cd-108">Både dina egna principer och principen B2C_1A_base_extensions kan åsidosätta dessa definitioner och utöka överordnade principen genom att tillhandahålla ytterligare dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="212cd-108">Both your own policies and the B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="212cd-109">Grundelementen i den *B2C_1A_base princip* är anspråkstyper och anspråksomvandlingar innehåll definitioner.</span><span class="sxs-lookup"><span data-stu-id="212cd-109">The core elements of the *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="212cd-110">De här elementen kan mottagliga refereras i dina egna principer samt som i den *B2C_1A_base_extensions princip*.</span><span class="sxs-lookup"><span data-stu-id="212cd-110">These elements can susceptible to be referenced in your own policies as well as in the *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="212cd-111">Anspråk scheman</span><span class="sxs-lookup"><span data-stu-id="212cd-111">Claims schemas</span></span>

<span data-ttu-id="212cd-112">Detta anspråk scheman är indelat i tre delar:</span><span class="sxs-lookup"><span data-stu-id="212cd-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="212cd-113">Ett första avsnitt som visar minsta anspråk som krävs för användaren resor ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="212cd-113">A first section that lists the minimum claims that are required for the user journeys to work properly.</span></span>
2.  <span data-ttu-id="212cd-114">Ett andra avsnitt som visar anspråk krävs för frågan string-parametrar och andra särskilda parametrar som ska skickas till andra anspråksleverantörer, särskilt login.microsoftonline.com för autentisering.</span><span class="sxs-lookup"><span data-stu-id="212cd-114">A second section that lists the claims required for query string parameters and other special parameters to be passed to other claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="212cd-115">**Ändra inte de här anspråken**.</span><span class="sxs-lookup"><span data-stu-id="212cd-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="212cd-116">Och slutligen ett tredje avsnitt som visar en lista över ytterligare och valfria anspråk som kan samlas in från användare, lagras i katalogen och skickas i token under inloggningen.</span><span class="sxs-lookup"><span data-stu-id="212cd-116">And eventually a third section that lists any additional, optional claims that can be collected from the user, stored in the directory and sent in tokens during sign in.</span></span> <span data-ttu-id="212cd-117">Ny typ av anspråk till samlas in från användaren och skickas i token som kan läggas till i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="212cd-117">New claims type to be collected from the user and/or sent in the token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="212cd-118">Anspråk schemat innehåller begränsningar för vissa anspråk som lösenord och användarnamn.</span><span class="sxs-lookup"><span data-stu-id="212cd-118">The claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="212cd-119">Principen förtroende Framework (TF) behandlar Azure AD som andra anspråksprovider och dess begränsningar utformas i premium-principen.</span><span class="sxs-lookup"><span data-stu-id="212cd-119">The Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in the premium policy.</span></span> <span data-ttu-id="212cd-120">En princip kan ändras om du vill lägga till fler begränsningar eller Använd en annan anspråksprovider för lagring av autentiseringsuppgifter som har sin egen begränsningar.</span><span class="sxs-lookup"><span data-stu-id="212cd-120">A policy could be modified to add more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="212cd-121">Nedan visas de tillgängliga anspråkstyper.</span><span class="sxs-lookup"><span data-stu-id="212cd-121">The available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-the-user-journeys"></a><span data-ttu-id="212cd-122">Anspråk som krävs för användaren resor</span><span class="sxs-lookup"><span data-stu-id="212cd-122">Claims that are required for the user journeys</span></span>

<span data-ttu-id="212cd-123">Följande anspråk krävs för användaren resor ska fungera korrekt:</span><span class="sxs-lookup"><span data-stu-id="212cd-123">The following claims are required for user journeys to work properly:</span></span>

| <span data-ttu-id="212cd-124">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="212cd-124">Claims type</span></span> | <span data-ttu-id="212cd-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="212cd-126">*Användar-ID*</span><span class="sxs-lookup"><span data-stu-id="212cd-126">*UserId*</span></span> | <span data-ttu-id="212cd-127">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="212cd-127">Username</span></span> |
| <span data-ttu-id="212cd-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="212cd-128">*signInName*</span></span> | <span data-ttu-id="212cd-129">Logga in namn</span><span class="sxs-lookup"><span data-stu-id="212cd-129">Sign in name</span></span> |
| <span data-ttu-id="212cd-130">*klient-ID*</span><span class="sxs-lookup"><span data-stu-id="212cd-130">*tenantId*</span></span> | <span data-ttu-id="212cd-131">Klient ID-Numret för användarobjektet i Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="212cd-131">Tenant identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="212cd-132">*objekt-ID*</span><span class="sxs-lookup"><span data-stu-id="212cd-132">*objectId*</span></span> | <span data-ttu-id="212cd-133">Objekt-ID (ID) för användarobjektet i Azure AD B2C Premium</span><span class="sxs-lookup"><span data-stu-id="212cd-133">Object identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="212cd-134">*lösenord*</span><span class="sxs-lookup"><span data-stu-id="212cd-134">*password*</span></span> | <span data-ttu-id="212cd-135">Lösenord</span><span class="sxs-lookup"><span data-stu-id="212cd-135">Password</span></span> |
| <span data-ttu-id="212cd-136">*nytt lösenord*</span><span class="sxs-lookup"><span data-stu-id="212cd-136">*newPassword*</span></span> | |
| <span data-ttu-id="212cd-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="212cd-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="212cd-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="212cd-138">*passwordPolicies*</span></span> | <span data-ttu-id="212cd-139">Lösenordsprinciper som används av Azure AD B2C Premium för att fastställa lösenordssäkerhet, upphör att gälla, osv.</span><span class="sxs-lookup"><span data-stu-id="212cd-139">Password policies used by Azure AD B2C Premium to determine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="212cd-140">*Sub*</span><span class="sxs-lookup"><span data-stu-id="212cd-140">*sub*</span></span> | |
| <span data-ttu-id="212cd-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="212cd-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="212cd-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="212cd-142">*identityProvider*</span></span> | |
| <span data-ttu-id="212cd-143">*Visningsnamn*</span><span class="sxs-lookup"><span data-stu-id="212cd-143">*displayName*</span></span> | |
| <span data-ttu-id="212cd-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="212cd-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="212cd-145">Användarens telefonnummer</span><span class="sxs-lookup"><span data-stu-id="212cd-145">User's telephone number</span></span> |
| <span data-ttu-id="212cd-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="212cd-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="212cd-147">*e-post*</span><span class="sxs-lookup"><span data-stu-id="212cd-147">*email*</span></span> | <span data-ttu-id="212cd-148">E-postadress som kan användas för att kontakta användaren</span><span class="sxs-lookup"><span data-stu-id="212cd-148">Email address that can be used to contact the user</span></span> |
| <span data-ttu-id="212cd-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="212cd-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="212cd-150">E-postadress som användaren kan använda för att logga in</span><span class="sxs-lookup"><span data-stu-id="212cd-150">Email address that the user can use to sign in</span></span> |
| <span data-ttu-id="212cd-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="212cd-151">*otherMails*</span></span> | <span data-ttu-id="212cd-152">E-postadresser som kan användas för att kontakta användaren</span><span class="sxs-lookup"><span data-stu-id="212cd-152">Email addresses that can be used to contact the user</span></span> |
| <span data-ttu-id="212cd-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="212cd-153">*userPrincipalName*</span></span> | <span data-ttu-id="212cd-154">Användarnamnet som lagras i Azure AD B2C-Premium</span><span class="sxs-lookup"><span data-stu-id="212cd-154">Username as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="212cd-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="212cd-155">*upnUserName*</span></span> | <span data-ttu-id="212cd-156">Användarnamn för att skapa användarens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="212cd-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="212cd-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="212cd-157">*mailNickName*</span></span> | <span data-ttu-id="212cd-158">Användarens e-smeknamn som lagras i Azure AD B2C-Premium</span><span class="sxs-lookup"><span data-stu-id="212cd-158">User's mail nick name as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="212cd-159">*ny användare*</span><span class="sxs-lookup"><span data-stu-id="212cd-159">*newUser*</span></span> | |
| <span data-ttu-id="212cd-160">*köra SelfAsserted-inmatning*</span><span class="sxs-lookup"><span data-stu-id="212cd-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="212cd-161">Anspråk som anger om attribut samlades in från användaren</span><span class="sxs-lookup"><span data-stu-id="212cd-161">Claim that specifies whether attributes were collected from the user</span></span> |
| <span data-ttu-id="212cd-162">*köra PhoneFactor-inmatning*</span><span class="sxs-lookup"><span data-stu-id="212cd-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="212cd-163">Anspråk som anger om ett nytt telefonnummer samlats in från användaren</span><span class="sxs-lookup"><span data-stu-id="212cd-163">Claim that specifies whether a new phone number was collected from the user</span></span> |
| <span data-ttu-id="212cd-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="212cd-164">*authenticationSource*</span></span> | <span data-ttu-id="212cd-165">Anger om användaren har autentiserats på sociala identitetsleverantör, login.microsoftonline.com eller lokalt konto</span><span class="sxs-lookup"><span data-stu-id="212cd-165">Specifies whether the user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="212cd-166">Anspråk som krävs för frågan string-parametrar och andra särskilda parametrar</span><span class="sxs-lookup"><span data-stu-id="212cd-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="212cd-167">Följande anspråk krävs för att vidarebefordra särskilda parametrar (inklusive vissa sträng frågeparametrar) till andra anspråksleverantörer:</span><span class="sxs-lookup"><span data-stu-id="212cd-167">The following claims are required to pass on special parameters (including some query string parameters) to other claims providers:</span></span>

| <span data-ttu-id="212cd-168">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="212cd-168">Claims type</span></span> | <span data-ttu-id="212cd-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="212cd-170">*nux*</span><span class="sxs-lookup"><span data-stu-id="212cd-170">*nux*</span></span> | <span data-ttu-id="212cd-171">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-171">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-172">*NCA*</span><span class="sxs-lookup"><span data-stu-id="212cd-172">*nca*</span></span> | <span data-ttu-id="212cd-173">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-173">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-174">*kommandotolk*</span><span class="sxs-lookup"><span data-stu-id="212cd-174">*prompt*</span></span> | <span data-ttu-id="212cd-175">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-175">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="212cd-176">*mkt*</span></span> | <span data-ttu-id="212cd-177">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-177">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-178">*LC*</span><span class="sxs-lookup"><span data-stu-id="212cd-178">*lc*</span></span> | <span data-ttu-id="212cd-179">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-179">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="212cd-180">*grant_type*</span></span> | <span data-ttu-id="212cd-181">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-181">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-182">*omfång*</span><span class="sxs-lookup"><span data-stu-id="212cd-182">*scope*</span></span> | <span data-ttu-id="212cd-183">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-183">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="212cd-184">*client_id*</span></span> | <span data-ttu-id="212cd-185">Särskilda parameter som skickades för lokalt konto autentisering för login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="212cd-185">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="212cd-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="212cd-186">*objectIdFromSession*</span></span> | <span data-ttu-id="212cd-187">Parametern som angetts av standardprovidern för sessionen management att indikera att objekt-id har hämtats från en SSO-session</span><span class="sxs-lookup"><span data-stu-id="212cd-187">Parameter provided by the default session management provider to indicate that the object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="212cd-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="212cd-188">*isActiveMFASession*</span></span> | <span data-ttu-id="212cd-189">Parametern som angetts av MFA sessionshantering som visar att användaren har en aktiv session MFA</span><span class="sxs-lookup"><span data-stu-id="212cd-189">Parameter provided by the MFA session management to indicate that the user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="212cd-190">Ytterligare (valfritt) anspråk som kan samlas in</span><span class="sxs-lookup"><span data-stu-id="212cd-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="212cd-191">Följande anspråk är ytterligare anspråk som kan samlas in från användarna, lagras i katalogen och skickas i token.</span><span class="sxs-lookup"><span data-stu-id="212cd-191">The following claims are additional claims that can be collected from the users, stored in the directory, and sent in the token.</span></span> <span data-ttu-id="212cd-192">Enligt innan kan ytterligare anspråk läggas till i listan.</span><span class="sxs-lookup"><span data-stu-id="212cd-192">As outlined before, additional claims can be added to this list.</span></span>

| <span data-ttu-id="212cd-193">Typ av anspråk</span><span class="sxs-lookup"><span data-stu-id="212cd-193">Claims type</span></span> | <span data-ttu-id="212cd-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="212cd-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="212cd-195">*givenName*</span></span> | <span data-ttu-id="212cd-196">Användarens förnamn (även kallat Förnamn)</span><span class="sxs-lookup"><span data-stu-id="212cd-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="212cd-197">*Efternamn*</span><span class="sxs-lookup"><span data-stu-id="212cd-197">*surname*</span></span> | <span data-ttu-id="212cd-198">Användarens efternamn (även kallat namn eller efternamn)</span><span class="sxs-lookup"><span data-stu-id="212cd-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="212cd-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="212cd-199">*Extension_picture*</span></span> | <span data-ttu-id="212cd-200">Användarens bild från sociala</span><span class="sxs-lookup"><span data-stu-id="212cd-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="212cd-201">Anspråksomvandlingar</span><span class="sxs-lookup"><span data-stu-id="212cd-201">Claim transformations</span></span>

<span data-ttu-id="212cd-202">Nedan visas de tillgängliga anspråksomvandlingar.</span><span class="sxs-lookup"><span data-stu-id="212cd-202">The available claim transformations are listed below.</span></span>

| <span data-ttu-id="212cd-203">Anspråksomvandling av</span><span class="sxs-lookup"><span data-stu-id="212cd-203">Claim transformation</span></span> | <span data-ttu-id="212cd-204">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="212cd-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="212cd-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="212cd-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="212cd-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="212cd-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="212cd-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="212cd-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="212cd-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="212cd-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="212cd-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="212cd-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="212cd-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="212cd-211">Definitioner för innehåll</span><span class="sxs-lookup"><span data-stu-id="212cd-211">Content definitions</span></span>

<span data-ttu-id="212cd-212">Det här avsnittet beskrivs de innehåll definitionerna redan deklarerats i den *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-212">This section describes the content definitions already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="212cd-213">Dessa definitioner av innehåll är sårbara för refererar till, åsidosätts och utökad som behövs i dina egna principer samt som i den *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-213">These content definitions are susceptible to be referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="212cd-214">Anspråksleverantör</span><span class="sxs-lookup"><span data-stu-id="212cd-214">Claims provider</span></span> | <span data-ttu-id="212cd-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="212cd-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="212cd-216">*Facebook*</span></span> | |
| <span data-ttu-id="212cd-217">*Logga in lokalt konto*</span><span class="sxs-lookup"><span data-stu-id="212cd-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="212cd-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="212cd-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="212cd-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="212cd-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="212cd-220">*Self vars*</span><span class="sxs-lookup"><span data-stu-id="212cd-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="212cd-221">*Lokalt konto*</span><span class="sxs-lookup"><span data-stu-id="212cd-221">*Local Account*</span></span> | |
| <span data-ttu-id="212cd-222">*Sessionshantering*</span><span class="sxs-lookup"><span data-stu-id="212cd-222">*Session Management*</span></span> | |
| <span data-ttu-id="212cd-223">*Trustframework principmodulen*</span><span class="sxs-lookup"><span data-stu-id="212cd-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="212cd-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="212cd-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="212cd-225">*Token utfärdare*</span><span class="sxs-lookup"><span data-stu-id="212cd-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="212cd-226">Tekniska profiler</span><span class="sxs-lookup"><span data-stu-id="212cd-226">Technical profiles</span></span>

<span data-ttu-id="212cd-227">Det här avsnittet visar profilerna tekniska redan deklarerats per anspråksleverantör i den *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-227">This section depicts the technical profiles already declared per claim provider in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="212cd-228">Dessa tekniska profiler är sårbara för att ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som i den *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-228">These technical profiles are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="212cd-229">Tekniska profiler för Facebook</span><span class="sxs-lookup"><span data-stu-id="212cd-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="212cd-230">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-230">Technical profile</span></span> | <span data-ttu-id="212cd-231">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-232">*Facebook-OAUTH*</span><span class="sxs-lookup"><span data-stu-id="212cd-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="212cd-233">Tekniska profiler för lokal inloggning för kontot</span><span class="sxs-lookup"><span data-stu-id="212cd-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="212cd-234">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-234">Technical profile</span></span> | <span data-ttu-id="212cd-235">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-236">*Logga in utan interaktivitet*</span><span class="sxs-lookup"><span data-stu-id="212cd-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="212cd-237">Tekniska profiler för Phonefactor</span><span class="sxs-lookup"><span data-stu-id="212cd-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="212cd-238">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-238">Technical profile</span></span> | <span data-ttu-id="212cd-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-240">*PhoneFactor-indata*</span><span class="sxs-lookup"><span data-stu-id="212cd-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="212cd-241">*PhoneFactor-InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="212cd-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="212cd-242">*PhoneFactor-verifiera*</span><span class="sxs-lookup"><span data-stu-id="212cd-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="212cd-243">Tekniska profiler för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="212cd-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="212cd-244">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-244">Technical profile</span></span> | <span data-ttu-id="212cd-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-246">*AAD-gemensamma*</span><span class="sxs-lookup"><span data-stu-id="212cd-246">*AAD-Common*</span></span> | <span data-ttu-id="212cd-247">Tekniska profil ingår som de andra AAD-xxx tekniska profilerna</span><span class="sxs-lookup"><span data-stu-id="212cd-247">Technical profile included by the other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="212cd-248">*AAD-UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="212cd-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="212cd-249">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="212cd-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="212cd-250">*AAD-UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="212cd-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="212cd-251">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="212cd-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="212cd-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span><span class="sxs-lookup"><span data-stu-id="212cd-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="212cd-253">Tekniska profil för sociala inloggningar</span><span class="sxs-lookup"><span data-stu-id="212cd-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="212cd-254">*AAD-UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="212cd-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="212cd-255">Tekniska profil för lokala konton</span><span class="sxs-lookup"><span data-stu-id="212cd-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="212cd-256">*AAD-UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="212cd-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="212cd-257">Tekniska profil för lokala konton</span><span class="sxs-lookup"><span data-stu-id="212cd-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="212cd-258">*AAD-UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="212cd-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="212cd-259">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="212cd-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="212cd-260">*AAD-UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="212cd-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="212cd-261">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="212cd-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="212cd-262">*AAD-UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="212cd-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="212cd-263">Tekniska profil för att uppdatera användarpost med objekt-ID</span><span class="sxs-lookup"><span data-stu-id="212cd-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="212cd-264">*AAD-UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="212cd-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="212cd-265">Tekniska profilen används för att läsa data när användaren autentiseras</span><span class="sxs-lookup"><span data-stu-id="212cd-265">Technical profile is used to read data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="212cd-266">Tekniska profiler för Self vars</span><span class="sxs-lookup"><span data-stu-id="212cd-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="212cd-267">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-267">Technical profile</span></span> | <span data-ttu-id="212cd-268">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-269">*SelfAsserted Social*</span><span class="sxs-lookup"><span data-stu-id="212cd-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="212cd-270">*SelfAsserted ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="212cd-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="212cd-271">Tekniska profiler för lokala konton</span><span class="sxs-lookup"><span data-stu-id="212cd-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="212cd-272">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-272">Technical profile</span></span> | <span data-ttu-id="212cd-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="212cd-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="212cd-275">Tekniska profiler för sessionshantering</span><span class="sxs-lookup"><span data-stu-id="212cd-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="212cd-276">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-276">Technical profile</span></span> | <span data-ttu-id="212cd-277">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-278">*SM-Noop*</span><span class="sxs-lookup"><span data-stu-id="212cd-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="212cd-279">*SM-AAD*</span><span class="sxs-lookup"><span data-stu-id="212cd-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="212cd-280">*SM-SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="212cd-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="212cd-281">Profilnamnet används för att undvika tvetydigheten AAD session mellan logga in och logga in</span><span class="sxs-lookup"><span data-stu-id="212cd-281">Profile name is being used to disambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="212cd-282">*SM-SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="212cd-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="212cd-283">*SM-MFA*</span><span class="sxs-lookup"><span data-stu-id="212cd-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="212cd-284">Tekniska profiler för Trustframework princip motorn TechnicalProfiles</span><span class="sxs-lookup"><span data-stu-id="212cd-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="212cd-285">För närvarande inga tekniska profiler har definierats för den **Trustframework princip motorn TechnicalProfiles** anspråksleverantör.</span><span class="sxs-lookup"><span data-stu-id="212cd-285">Currently, no technical profiles are defined for the **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="212cd-286">Tekniska profiler för tokenutfärdare</span><span class="sxs-lookup"><span data-stu-id="212cd-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="212cd-287">Tekniska profil</span><span class="sxs-lookup"><span data-stu-id="212cd-287">Technical profile</span></span> | <span data-ttu-id="212cd-288">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="212cd-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="212cd-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="212cd-290">Användaren resor</span><span class="sxs-lookup"><span data-stu-id="212cd-290">User journeys</span></span>

<span data-ttu-id="212cd-291">Det här avsnittet visar de användare körningar redan deklarerats i den *B2C_1A_base* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-291">This section depicts the user journeys already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="212cd-292">Dessa användare resor är sårbara för att ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som i den *B2C_1A_base_extensions* princip.</span><span class="sxs-lookup"><span data-stu-id="212cd-292">These user journeys are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="212cd-293">Användaren resa</span><span class="sxs-lookup"><span data-stu-id="212cd-293">User journey</span></span> | <span data-ttu-id="212cd-294">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="212cd-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="212cd-295">*Registreringen*</span><span class="sxs-lookup"><span data-stu-id="212cd-295">*SignUp*</span></span> | |
| <span data-ttu-id="212cd-296">*Logga in*</span><span class="sxs-lookup"><span data-stu-id="212cd-296">*SignIn*</span></span> | |
| <span data-ttu-id="212cd-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="212cd-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="212cd-298">*EditProfile*</span><span class="sxs-lookup"><span data-stu-id="212cd-298">*EditProfile*</span></span> | |
| <span data-ttu-id="212cd-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="212cd-299">*PasswordReset*</span></span> | |
