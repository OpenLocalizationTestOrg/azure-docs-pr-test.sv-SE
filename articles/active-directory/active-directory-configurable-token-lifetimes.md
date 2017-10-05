---
title: "Konfigurerbara token livslängd i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du ställer in livslängd för token som utfärdats av Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: d23721eba308096a05211eb6e26e1338a69cae0c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a><span data-ttu-id="ad760-103">Konfigurerbara token livslängd i Azure Active Directory (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="ad760-103">Configurable token lifetimes in Azure Active Directory (Public Preview)</span></span>
<span data-ttu-id="ad760-104">Du kan ange livslängden för en token som utfärdas av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ad760-104">You can specify the lifetime of a token issued by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ad760-105">Du kan ange token livslängd för alla program i din organisation, för ett program för flera innehavare (flera organisation) eller för en specifik tjänstens huvudnamn i din organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-105">You can set token lifetimes for all apps in your organization, for a multi-tenant (multi-organization) application, or for a specific service principal in your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="ad760-106">Den här funktionen är för närvarande i förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="ad760-106">This capability currently is in Public Preview.</span></span> <span data-ttu-id="ad760-107">Var beredd på att återställa eller ta bort alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="ad760-107">Be prepared to revert or remove any changes.</span></span> <span data-ttu-id="ad760-108">Funktionen är tillgänglig i alla Azure Active Directory-prenumeration under Public Preview.</span><span class="sxs-lookup"><span data-stu-id="ad760-108">The feature is available in any Azure Active Directory subscription during Public Preview.</span></span> <span data-ttu-id="ad760-109">Men när funktionen blir allmänt tillgänglig vissa aspekter av funktionen kan kräva en [Azure Active Directory Premium](active-directory-get-started-premium.md) prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad760-109">However, when the feature becomes generally available, some aspects of the feature might require an [Azure Active Directory Premium](active-directory-get-started-premium.md) subscription.</span></span>
>
>

<span data-ttu-id="ad760-110">I Azure AD representerar ett grupprincipobjekt en uppsättning regler som tillämpas på enskilda program eller på alla program i en organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-110">In Azure AD, a policy object represents a set of rules that are enforced on individual applications or on all applications in an organization.</span></span> <span data-ttu-id="ad760-111">Varje princip har ett unikt struktur med en uppsättning egenskaper som tillämpas på objekt som de har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="ad760-111">Each policy type has a unique structure, with a set of properties that are applied to objects to which they are assigned.</span></span>

<span data-ttu-id="ad760-112">Du kan ange en princip som standardprincipen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-112">You can designate a policy as the default policy for your organization.</span></span> <span data-ttu-id="ad760-113">Principen tillämpas på alla program i organisationen, så länge det inte åsidosätts av en princip med högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="ad760-113">The policy is applied to any application in the organization, as long as it is not overridden by a policy with a higher priority.</span></span> <span data-ttu-id="ad760-114">Du kan också tilldela en princip till specifika program.</span><span class="sxs-lookup"><span data-stu-id="ad760-114">You also can assign a policy to specific applications.</span></span> <span data-ttu-id="ad760-115">Prioritetsordningen varierar beroende på principtypen.</span><span class="sxs-lookup"><span data-stu-id="ad760-115">The order of priority varies by policy type.</span></span>


## <a name="token-types"></a><span data-ttu-id="ad760-116">Typer av token</span><span class="sxs-lookup"><span data-stu-id="ad760-116">Token types</span></span>

<span data-ttu-id="ad760-117">Du kan ange principer för livslängd för token för uppdaterings-tokens, åtkomsttoken, session token och ID-token.</span><span class="sxs-lookup"><span data-stu-id="ad760-117">You can set token lifetime policies for refresh tokens, access tokens, session tokens, and ID tokens.</span></span>

### <a name="access-tokens"></a><span data-ttu-id="ad760-118">Åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="ad760-118">Access tokens</span></span>
<span data-ttu-id="ad760-119">Klienter använder åtkomsttoken att få åtkomst till en skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="ad760-119">Clients use access tokens to access a protected resource.</span></span> <span data-ttu-id="ad760-120">En åtkomst-token kan användas endast för en specifik kombination av användare, klienten och resursen.</span><span class="sxs-lookup"><span data-stu-id="ad760-120">An access token can be used only for a specific combination of user, client, and resource.</span></span> <span data-ttu-id="ad760-121">Åtkomst-token kan inte återkallas och är giltiga tills de upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="ad760-121">Access tokens cannot be revoked and are valid until their expiry.</span></span> <span data-ttu-id="ad760-122">En skadlig aktören som har fått en åtkomst-token kan använda det för omfattningen av dess livslängd.</span><span class="sxs-lookup"><span data-stu-id="ad760-122">A malicious actor that has obtained an access token can use it for extent of its lifetime.</span></span> <span data-ttu-id="ad760-123">Justera livslängden för en åtkomst-token är en kompromiss mellan förbättra systemets prestanda och öka mängden tid att klienten behåller åtkomst efter användarens konto har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="ad760-123">Adjusting the lifetime of an access token is a trade-off between improving system performance and increasing the amount of time that the client retains access after the user’s account is disabled.</span></span> <span data-ttu-id="ad760-124">Förbättrad prestanda uppnås genom att minska antalet gånger som en klient behöver skaffa en ny åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="ad760-124">Improved system performance is achieved by reducing the number of times a client needs to acquire a fresh access token.</span></span>

### <a name="refresh-tokens"></a><span data-ttu-id="ad760-125">Uppdatera token</span><span class="sxs-lookup"><span data-stu-id="ad760-125">Refresh tokens</span></span>
<span data-ttu-id="ad760-126">När en klient får en åtkomst-token för att få åtkomst till en skyddad resurs, får klienten både en uppdateringstoken och en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="ad760-126">When a client acquires an access token to access a protected resource, the client receives both a refresh token and an access token.</span></span> <span data-ttu-id="ad760-127">Uppdateringstoken som används för att hämta nya åtkomst/uppdatera token par när den aktuella åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="ad760-127">The refresh token is used to obtain new access/refresh token pairs when the current access token expires.</span></span> <span data-ttu-id="ad760-128">En uppdateringstoken är bunden till en kombination av användar- och klienten.</span><span class="sxs-lookup"><span data-stu-id="ad760-128">A refresh token is bound to a combination of user and client.</span></span> <span data-ttu-id="ad760-129">En uppdateringstoken kan återkallas och token giltighet kontrolleras varje gång token som används.</span><span class="sxs-lookup"><span data-stu-id="ad760-129">A refresh token can be revoked, and the token's validity is checked every time the token is used.</span></span>

<span data-ttu-id="ad760-130">Det är viktigt att se skillnad mellan konfidentiell och offentliga klienter.</span><span class="sxs-lookup"><span data-stu-id="ad760-130">It's important to make a distinction between confidential clients and public clients.</span></span> <span data-ttu-id="ad760-131">Läs mer om olika typer av klienter, [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="ad760-131">For more information about different types of clients, see [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).</span></span>

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a><span data-ttu-id="ad760-132">Token livslängd med konfidentiella klienten uppdatera token</span><span class="sxs-lookup"><span data-stu-id="ad760-132">Token lifetimes with confidential client refresh tokens</span></span>
<span data-ttu-id="ad760-133">Konfidentiell klienter är program som kan lagras säkert lösenord klienten (hemliga).</span><span class="sxs-lookup"><span data-stu-id="ad760-133">Confidential clients are applications that can securely store a client password (secret).</span></span> <span data-ttu-id="ad760-134">De kan visa att förfrågningarna kommer från klientprogrammet och inte från en skadlig aktören.</span><span class="sxs-lookup"><span data-stu-id="ad760-134">They can prove that requests are coming from the client application and not from a malicious actor.</span></span> <span data-ttu-id="ad760-135">Ett webbprogram är till exempel en konfidentiell klient eftersom det kan lagra en klienthemlighet på webbservern.</span><span class="sxs-lookup"><span data-stu-id="ad760-135">For example, a web app is a confidential client because it can store a client secret on the web server.</span></span> <span data-ttu-id="ad760-136">Exponeras inte.</span><span class="sxs-lookup"><span data-stu-id="ad760-136">It is not exposed.</span></span> <span data-ttu-id="ad760-137">Eftersom dessa flöden är säkrare standard livslängd för uppdaterings-tokens som utfärdats till dessa flöden är `until-revoked`, kan inte ändras med hjälp av Grupprincip och kommer inte att återkallas på frivilliga lösenordsåterställning.</span><span class="sxs-lookup"><span data-stu-id="ad760-137">Because these flows are more secure, the default lifetimes of refresh tokens issued to these flows is `until-revoked`, cannot be changed by using policy, and will not be revoked on voluntary password resets.</span></span>

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a><span data-ttu-id="ad760-138">Token livslängd med offentliga klienten uppdatera token</span><span class="sxs-lookup"><span data-stu-id="ad760-138">Token lifetimes with public client refresh tokens</span></span>

<span data-ttu-id="ad760-139">Offentliga klienter kan inte på ett säkert sätt att lagra ett klient-lösenord (hemliga).</span><span class="sxs-lookup"><span data-stu-id="ad760-139">Public clients cannot securely store a client password (secret).</span></span> <span data-ttu-id="ad760-140">En iOS/Android-app kan till exempel obfuscate en hemlighet från resursägare så är det en offentlig klient.</span><span class="sxs-lookup"><span data-stu-id="ad760-140">For example, an iOS/Android app cannot obfuscate a secret from the resource owner, so it is considered a public client.</span></span> <span data-ttu-id="ad760-141">Du kan ange principer resurser för att förhindra uppdaterings-tokens från offentliga klienter som är äldre än en angiven period från att erhålla ett nytt åtkomst/uppdatera token par.</span><span class="sxs-lookup"><span data-stu-id="ad760-141">You can set policies on resources to prevent refresh tokens from public clients older than a specified period from obtaining a new access/refresh token pair.</span></span> <span data-ttu-id="ad760-142">(Gör du genom att använda egenskapen uppdatera Token inaktiva maxtid.) Du kan också använda principer för att ange en tidsperiod efter vilken uppdaterings-tokens accepteras inte längre.</span><span class="sxs-lookup"><span data-stu-id="ad760-142">(To do this, use the Refresh Token Max Inactive Time property.) You also can use policies to set a period beyond which the refresh tokens are no longer accepted.</span></span> <span data-ttu-id="ad760-143">(Det gör du genom att använda egenskapen uppdatera Token Max Age.) Du kan justera livslängden för en uppdateringstoken för att styra när och hur ofta användaren krävs för att ange autentiseringsuppgifter, i stället för att tyst återautentiseras, när du använder en offentlig klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ad760-143">(To do this, use the Refresh Token Max Age property.) You can adjust the lifetime of a refresh token to control when and how often the user is required to reenter credentials, instead of being silently reauthenticated, when using a public client application.</span></span>

### <a name="id-tokens"></a><span data-ttu-id="ad760-144">ID-token</span><span class="sxs-lookup"><span data-stu-id="ad760-144">ID tokens</span></span>
<span data-ttu-id="ad760-145">ID-token skickas till webbplatser och interna klienter.</span><span class="sxs-lookup"><span data-stu-id="ad760-145">ID tokens are passed to websites and native clients.</span></span> <span data-ttu-id="ad760-146">ID-token innehåller profilinformation om en användare.</span><span class="sxs-lookup"><span data-stu-id="ad760-146">ID tokens contain profile information about a user.</span></span> <span data-ttu-id="ad760-147">En ID-token är bunden till en specifik kombination av användar- och klienten.</span><span class="sxs-lookup"><span data-stu-id="ad760-147">An ID token is bound to a specific combination of user and client.</span></span> <span data-ttu-id="ad760-148">ID-token anses giltiga tills de upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="ad760-148">ID tokens are considered valid until their expiry.</span></span> <span data-ttu-id="ad760-149">Vanligtvis ett webbprogram matchar en användare har sessioners livstid i tillämpningsprogrammet att livslängden för ID-token som utfärdas för användaren.</span><span class="sxs-lookup"><span data-stu-id="ad760-149">Usually, a web application matches a user’s session lifetime in the application to the lifetime of the ID token issued for the user.</span></span> <span data-ttu-id="ad760-150">Du kan justera livslängden för en ID-token för att styra hur ofta webbprogrammet upphör sessionen program och hur ofta den kräver att användaren autentiseras med Azure AD (tyst eller interaktivt).</span><span class="sxs-lookup"><span data-stu-id="ad760-150">You can adjust the lifetime of an ID token to control how often the web application expires the application session, and how often it requires the user to be reauthenticated with Azure AD (either silently or interactively).</span></span>

### <a name="single-sign-on-session-tokens"></a><span data-ttu-id="ad760-151">Enkel inloggning session token</span><span class="sxs-lookup"><span data-stu-id="ad760-151">Single sign-on session tokens</span></span>
<span data-ttu-id="ad760-152">När en användare autentiserar med Azure AD och väljer den **Håll mig inloggad** kryssrutan, en enkel inloggning (SSO) upprättas session med användarens webbläsare och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad760-152">When a user authenticates with Azure AD and selects the **Keep me signed in** check box, a single sign-on session (SSO) is established with the user’s browser and Azure AD.</span></span> <span data-ttu-id="ad760-153">SSO-token i form av en cookie representerar den här sessionen.</span><span class="sxs-lookup"><span data-stu-id="ad760-153">The SSO token, in the form of a cookie, represents this session.</span></span> <span data-ttu-id="ad760-154">Observera sessionstoken SSO inte är bunden till en specifik resurs/klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ad760-154">Note that the SSO session token is not bound to a specific resource/client application.</span></span> <span data-ttu-id="ad760-155">SSO session token kan återkallas och deras giltighet kontrolleras varje gång de används.</span><span class="sxs-lookup"><span data-stu-id="ad760-155">SSO session tokens can be revoked, and their validity is checked every time they are used.</span></span>

<span data-ttu-id="ad760-156">Azure AD använder två typer av token för SSO-session: beständiga och Uppdateringsvärdet.</span><span class="sxs-lookup"><span data-stu-id="ad760-156">Azure AD uses two kinds of SSO session tokens: persistent and nonpersistent.</span></span> <span data-ttu-id="ad760-157">Beständiga session token lagras som beständiga cookies i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ad760-157">Persistent session tokens are stored as persistent cookies by the browser.</span></span> <span data-ttu-id="ad760-158">Uppdateringsvärdet session token lagras som sessions-cookies.</span><span class="sxs-lookup"><span data-stu-id="ad760-158">Nonpersistent session tokens are stored as session cookies.</span></span> <span data-ttu-id="ad760-159">(Sessionscookies förstörs när webbläsaren stängs.)</span><span class="sxs-lookup"><span data-stu-id="ad760-159">(Session cookies are destroyed when the browser is closed.)</span></span>

<span data-ttu-id="ad760-160">Uppdateringsvärdet session token har en livslängd på 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="ad760-160">Nonpersistent session tokens have a lifetime of 24 hours.</span></span> <span data-ttu-id="ad760-161">Beständiga token har en livslängd på 180 dagar.</span><span class="sxs-lookup"><span data-stu-id="ad760-161">Persistent tokens have a lifetime of 180 days.</span></span> <span data-ttu-id="ad760-162">Varje gång en SSO sessionstoken används inom sin giltighetstid förlängs giltighetsperioden annat 24 timmar eller 180 dagar, beroende på typen av token.</span><span class="sxs-lookup"><span data-stu-id="ad760-162">Any time an SSO session token is used within its validity period, the validity period is extended another 24 hours or 180 days, depending on the token type.</span></span> <span data-ttu-id="ad760-163">Om en SSO sessionstoken inte används inom sin giltighetstid, anses det upphört att gälla och är inte längre att accepteras.</span><span class="sxs-lookup"><span data-stu-id="ad760-163">If an SSO session token is not used within its validity period, it is considered expired and is no longer accepted.</span></span>

<span data-ttu-id="ad760-164">Du kan använda en princip för att ställa in tiden efter den första sessionstoken utfärdades utöver som sessionstoken längre accepteras.</span><span class="sxs-lookup"><span data-stu-id="ad760-164">You can use a policy to set the time after the first session token was issued beyond which the session token is no longer accepted.</span></span> <span data-ttu-id="ad760-165">(Gör du genom att använda egenskapen Session Token Max Age.) Du kan justera livslängden för en sessionstoken att styra när och hur ofta en användare krävs för att ange autentiseringsuppgifter, i stället för tyst verifierats när du använder ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ad760-165">(To do this, use the Session Token Max Age property.) You can adjust the lifetime of a session token to control when and how often a user is required to reenter credentials, instead of being silently authenticated, when using a web application.</span></span>

### <a name="token-lifetime-policy-properties"></a><span data-ttu-id="ad760-166">Egenskaper för livslängd för token</span><span class="sxs-lookup"><span data-stu-id="ad760-166">Token lifetime policy properties</span></span>
<span data-ttu-id="ad760-167">En princip för livslängd för token är en typ av grupprincipobjekt som innehåller regler för livslängd för token.</span><span class="sxs-lookup"><span data-stu-id="ad760-167">A token lifetime policy is a type of policy object that contains token lifetime rules.</span></span> <span data-ttu-id="ad760-168">Använd egenskaperna för principen för att styra angivna token livslängd.</span><span class="sxs-lookup"><span data-stu-id="ad760-168">Use the properties of the policy to control specified token lifetimes.</span></span> <span data-ttu-id="ad760-169">Om ingen princip har angetts, tillämpar systemet livstid standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="ad760-169">If no policy is set, the system enforces the default lifetime value.</span></span>

### <a name="configurable-token-lifetime-properties"></a><span data-ttu-id="ad760-170">Livslängd för token konfigurerbara egenskaper</span><span class="sxs-lookup"><span data-stu-id="ad760-170">Configurable token lifetime properties</span></span>
| <span data-ttu-id="ad760-171">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ad760-171">Property</span></span> | <span data-ttu-id="ad760-172">Princip för egenskapssträng</span><span class="sxs-lookup"><span data-stu-id="ad760-172">Policy property string</span></span> | <span data-ttu-id="ad760-173">Påverkar</span><span class="sxs-lookup"><span data-stu-id="ad760-173">Affects</span></span> | <span data-ttu-id="ad760-174">Standard</span><span class="sxs-lookup"><span data-stu-id="ad760-174">Default</span></span> | <span data-ttu-id="ad760-175">Minimum</span><span class="sxs-lookup"><span data-stu-id="ad760-175">Minimum</span></span> | <span data-ttu-id="ad760-176">Maximalt</span><span class="sxs-lookup"><span data-stu-id="ad760-176">Maximum</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ad760-177">Livslängd för åtkomst-Token</span><span class="sxs-lookup"><span data-stu-id="ad760-177">Access Token Lifetime</span></span> |<span data-ttu-id="ad760-178">AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="ad760-178">AccessTokenLifetime</span></span> |<span data-ttu-id="ad760-179">Åtkomsttoken, ID-token, SAML2-token</span><span class="sxs-lookup"><span data-stu-id="ad760-179">Access tokens, ID tokens, SAML2 tokens</span></span> |<span data-ttu-id="ad760-180">1 timme</span><span class="sxs-lookup"><span data-stu-id="ad760-180">1 hour</span></span> |<span data-ttu-id="ad760-181">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-181">10 minutes</span></span> |<span data-ttu-id="ad760-182">1 dag</span><span class="sxs-lookup"><span data-stu-id="ad760-182">1 day</span></span> |
| <span data-ttu-id="ad760-183">Uppdatera Token inaktiva Maxtid</span><span class="sxs-lookup"><span data-stu-id="ad760-183">Refresh Token Max Inactive Time</span></span> |<span data-ttu-id="ad760-184">MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="ad760-184">MaxInactiveTime</span></span> |<span data-ttu-id="ad760-185">Uppdatera token</span><span class="sxs-lookup"><span data-stu-id="ad760-185">Refresh tokens</span></span> |<span data-ttu-id="ad760-186">14 dagar</span><span class="sxs-lookup"><span data-stu-id="ad760-186">14 days</span></span> |<span data-ttu-id="ad760-187">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-187">10 minutes</span></span> |<span data-ttu-id="ad760-188">90 dagar</span><span class="sxs-lookup"><span data-stu-id="ad760-188">90 days</span></span> |
| <span data-ttu-id="ad760-189">Enskild faktor uppdatera Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-189">Single-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="ad760-190">MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-190">MaxAgeSingleFactor</span></span> |<span data-ttu-id="ad760-191">Uppdatera token (för alla användare)</span><span class="sxs-lookup"><span data-stu-id="ad760-191">Refresh tokens (for any users)</span></span> |<span data-ttu-id="ad760-192">Tills återkallats</span><span class="sxs-lookup"><span data-stu-id="ad760-192">Until-revoked</span></span> |<span data-ttu-id="ad760-193">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-193">10 minutes</span></span> |<span data-ttu-id="ad760-194">Tills återkallas<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-194">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="ad760-195">Multi-Factor uppdatera Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-195">Multi-Factor Refresh Token Max Age</span></span> |<span data-ttu-id="ad760-196">MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-196">MaxAgeMultiFactor</span></span> |<span data-ttu-id="ad760-197">Uppdatera token (för alla användare)</span><span class="sxs-lookup"><span data-stu-id="ad760-197">Refresh tokens (for any users)</span></span> |<span data-ttu-id="ad760-198">Tills återkallats</span><span class="sxs-lookup"><span data-stu-id="ad760-198">Until-revoked</span></span> |<span data-ttu-id="ad760-199">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-199">10 minutes</span></span> |<span data-ttu-id="ad760-200">Tills återkallas<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-200">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="ad760-201">Enskild faktor Session Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-201">Single-Factor Session Token Max Age</span></span> |<span data-ttu-id="ad760-202">MaxAgeSessionSingleFactor<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-202">MaxAgeSessionSingleFactor<sup>2</sup></span></span> |<span data-ttu-id="ad760-203">Sessionen token (beständiga och Uppdateringsvärdet)</span><span class="sxs-lookup"><span data-stu-id="ad760-203">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="ad760-204">Tills återkallats</span><span class="sxs-lookup"><span data-stu-id="ad760-204">Until-revoked</span></span> |<span data-ttu-id="ad760-205">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-205">10 minutes</span></span> |<span data-ttu-id="ad760-206">Tills återkallas<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-206">Until-revoked<sup>1</sup></span></span> |
| <span data-ttu-id="ad760-207">Multi-Factor Session Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-207">Multi-Factor Session Token Max Age</span></span> |<span data-ttu-id="ad760-208">MaxAgeSessionMultiFactor<sup>3</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-208">MaxAgeSessionMultiFactor<sup>3</sup></span></span> |<span data-ttu-id="ad760-209">Sessionen token (beständiga och Uppdateringsvärdet)</span><span class="sxs-lookup"><span data-stu-id="ad760-209">Session tokens (persistent and nonpersistent)</span></span> |<span data-ttu-id="ad760-210">Tills återkallats</span><span class="sxs-lookup"><span data-stu-id="ad760-210">Until-revoked</span></span> |<span data-ttu-id="ad760-211">10 minuter</span><span class="sxs-lookup"><span data-stu-id="ad760-211">10 minutes</span></span> |<span data-ttu-id="ad760-212">Tills återkallas<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="ad760-212">Until-revoked<sup>1</sup></span></span> |

* <span data-ttu-id="ad760-213"><sup>1</sup>365 dagar är maxlängden explicit som kan anges för dessa attribut.</span><span class="sxs-lookup"><span data-stu-id="ad760-213"><sup>1</sup>365 days is the maximum explicit length that can be set for these attributes.</span></span>
* <span data-ttu-id="ad760-214"><sup>2</sup>om **MaxAgeSessionSingleFactor** är inte ange det här värdet tar den **MaxAgeSingleFactor** värde.</span><span class="sxs-lookup"><span data-stu-id="ad760-214"><sup>2</sup>If  **MaxAgeSessionSingleFactor** is not set, this value takes the **MaxAgeSingleFactor** value.</span></span> <span data-ttu-id="ad760-215">Om varken parametern anges tar egenskapen standardvärdet (förrän har återkallats).</span><span class="sxs-lookup"><span data-stu-id="ad760-215">If neither parameter is set, the property takes the default value (until-revoked).</span></span>
* <span data-ttu-id="ad760-216"><sup>3</sup>om **MaxAgeSessionMultiFactor** är inte ange det här värdet tar den **MaxAgeMultiFactor** värde.</span><span class="sxs-lookup"><span data-stu-id="ad760-216"><sup>3</sup>If  **MaxAgeSessionMultiFactor** is not set, this value takes the **MaxAgeMultiFactor** value.</span></span> <span data-ttu-id="ad760-217">Om varken parametern anges tar egenskapen standardvärdet (förrän har återkallats).</span><span class="sxs-lookup"><span data-stu-id="ad760-217">If neither parameter is set, the property takes the default value (until-revoked).</span></span>

### <a name="exceptions"></a><span data-ttu-id="ad760-218">Undantag</span><span class="sxs-lookup"><span data-stu-id="ad760-218">Exceptions</span></span>
| <span data-ttu-id="ad760-219">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ad760-219">Property</span></span> | <span data-ttu-id="ad760-220">Påverkar</span><span class="sxs-lookup"><span data-stu-id="ad760-220">Affects</span></span> | <span data-ttu-id="ad760-221">Standard</span><span class="sxs-lookup"><span data-stu-id="ad760-221">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad760-222">Uppdatera Token Max Age (utfärdas för federerade användare har inte tillräckligt återkallningsinformation<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="ad760-222">Refresh Token Max Age (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="ad760-223">Uppdatera token (utfärdas för federerade användare har inte tillräckligt återkallningsinformation<sup>1</sup>)</span><span class="sxs-lookup"><span data-stu-id="ad760-223">Refresh tokens (issued for federated users who have insufficient revocation information<sup>1</sup>)</span></span> |<span data-ttu-id="ad760-224">12 timmar</span><span class="sxs-lookup"><span data-stu-id="ad760-224">12 hours</span></span> |
| <span data-ttu-id="ad760-225">Uppdatera Token inaktiva Maxtid (utfärdats för konfidentiell klienter)</span><span class="sxs-lookup"><span data-stu-id="ad760-225">Refresh Token Max Inactive Time (issued for confidential clients)</span></span> |<span data-ttu-id="ad760-226">Uppdatera token (utfärdats för konfidentiell klienter)</span><span class="sxs-lookup"><span data-stu-id="ad760-226">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="ad760-227">90 dagar</span><span class="sxs-lookup"><span data-stu-id="ad760-227">90 days</span></span> |
| <span data-ttu-id="ad760-228">Uppdatera Token Max Age (utfärdats för konfidentiell klienter)</span><span class="sxs-lookup"><span data-stu-id="ad760-228">Refresh Token Max Age (issued for confidential clients)</span></span> |<span data-ttu-id="ad760-229">Uppdatera token (utfärdats för konfidentiell klienter)</span><span class="sxs-lookup"><span data-stu-id="ad760-229">Refresh tokens (issued for confidential clients)</span></span> |<span data-ttu-id="ad760-230">Tills återkallats</span><span class="sxs-lookup"><span data-stu-id="ad760-230">Until-revoked</span></span> |

* <span data-ttu-id="ad760-231"><sup>1</sup>externa användare har inte tillräckligt återkallningsinformation innehåller alla användare som inte har attributet ”LastPasswordChangeTimestamp” synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="ad760-231"><sup>1</sup>Federated users who have insufficient revocation information include any users who do not have the "LastPasswordChangeTimestamp" attribute synced.</span></span> <span data-ttu-id="ad760-232">Dessa användare får den här korta Max Age eftersom AAD inte går att kontrollera när återkalla token som är knutna till gamla autentiseringsuppgifter (till exempel ett lösenord som har ändrats) och måste checka in mer ofta så att användare och associerade token är fortfarande i god position.</span><span class="sxs-lookup"><span data-stu-id="ad760-232">These users are given this short Max Age because AAD is unable to verify when to revoke tokens that are tied to an old credential (such as a password that has been changed) and must check back in more frequently to ensure that the user and associated tokens are still in good standing.</span></span> <span data-ttu-id="ad760-233">För att förbättra upplevelsen innehavaradministratörer se till att de synkroniserar attributet ”LastPasswordChangeTimestamp” (Detta kan ställas in på användarobjekt med hjälp av Powershell eller via AADSync).</span><span class="sxs-lookup"><span data-stu-id="ad760-233">To improve this experience, tenant admins must ensure that they are syncing the “LastPasswordChangeTimestamp” attribute (this can be set on the user object using Powershell or through AADSync).</span></span>

### <a name="policy-evaluation-and-prioritization"></a><span data-ttu-id="ad760-234">För principutvärdering och prioritering</span><span class="sxs-lookup"><span data-stu-id="ad760-234">Policy evaluation and prioritization</span></span>
<span data-ttu-id="ad760-235">Du kan skapa och tilldela sedan en livslängd för token-principen till ett visst program, för din organisation och till tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-235">You can create and then assign a token lifetime policy to a specific application, to your organization, and to service principals.</span></span> <span data-ttu-id="ad760-236">Flera principer kan tillämpas på ett visst program.</span><span class="sxs-lookup"><span data-stu-id="ad760-236">Multiple policies might apply to a specific application.</span></span> <span data-ttu-id="ad760-237">Dessa regler: principen livslängd för token som påverkas</span><span class="sxs-lookup"><span data-stu-id="ad760-237">The token lifetime policy that takes effect follows these rules:</span></span>

* <span data-ttu-id="ad760-238">Om en princip tilldelas explicit till tjänstens huvudnamn, tillämpas.</span><span class="sxs-lookup"><span data-stu-id="ad760-238">If a policy is explicitly assigned to the service principal, it is enforced.</span></span>
* <span data-ttu-id="ad760-239">Om ingen princip uttryckligen har tilldelats till tjänstens huvudnamn, tillämpas en princip som uttryckligen har tilldelats organisationen överordnade för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-239">If no policy is explicitly assigned to the service principal, a policy explicitly assigned to the parent organization of the service principal is enforced.</span></span>
* <span data-ttu-id="ad760-240">Om ingen princip uttryckligen har tilldelats till tjänstens huvudnamn eller till organisationen, tillämpas principen som tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-240">If no policy is explicitly assigned to the service principal or to the organization, the policy assigned to the application is enforced.</span></span>
* <span data-ttu-id="ad760-241">Om ingen princip har tilldelats till tjänstens huvudnamn, organisationen eller programobjektet, tillämpas standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="ad760-241">If no policy has been assigned to the service principal, the organization, or the application object, the default values is enforced.</span></span> <span data-ttu-id="ad760-242">(Se tabellen i [livslängd för token konfigurerbara egenskaper](#configurable-token-lifetime-properties).)</span><span class="sxs-lookup"><span data-stu-id="ad760-242">(See the table in [Configurable token lifetime properties](#configurable-token-lifetime-properties).)</span></span>

<span data-ttu-id="ad760-243">Läs mer om förhållandet mellan program och tjänstens huvudnamn objekt [program och tjänstens huvudnamn objekt i Azure Active Directory](active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="ad760-243">For more information about the relationship between application objects and service principal objects, see [Application and service principal objects in Azure Active Directory](active-directory-application-objects.md).</span></span>

<span data-ttu-id="ad760-244">En token giltigheten utvärderas när token som används.</span><span class="sxs-lookup"><span data-stu-id="ad760-244">A token’s validity is evaluated at the time the token is used.</span></span> <span data-ttu-id="ad760-245">Principen med den högsta prioriteten på det program som används börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="ad760-245">The policy with the highest priority on the application that is being accessed takes effect.</span></span>

> [!NOTE]
> <span data-ttu-id="ad760-246">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="ad760-246">Here's an example scenario.</span></span>
>
> <span data-ttu-id="ad760-247">En användare vill få åtkomst till två webbprogram: A-webbprogram och Web Application B.</span><span class="sxs-lookup"><span data-stu-id="ad760-247">A user wants to access two web applications: Web Application A and Web Application B.</span></span>
> 
> <span data-ttu-id="ad760-248">Faktorer:</span><span class="sxs-lookup"><span data-stu-id="ad760-248">Factors:</span></span>
> * <span data-ttu-id="ad760-249">Både webbprogram finns i samma överordnade organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-249">Both web applications are in the same parent organization.</span></span>
> * <span data-ttu-id="ad760-250">Token livstid princip 1 med en Session Token Max Age åtta timmar har angetts som standard för den överordnade organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-250">Token Lifetime Policy 1 with a Session Token Max Age of eight hours is set as the parent organization’s default.</span></span>
> * <span data-ttu-id="ad760-251">Webbprogrammet A är en vanlig användning webbprogram och inte är länkad till alla principer.</span><span class="sxs-lookup"><span data-stu-id="ad760-251">Web Application A is a regular-use web application and isn’t linked to any policies.</span></span>
> * <span data-ttu-id="ad760-252">Web Application B används för mycket känslig processer.</span><span class="sxs-lookup"><span data-stu-id="ad760-252">Web Application B is used for highly sensitive processes.</span></span> <span data-ttu-id="ad760-253">Dess tjänstens huvudnamn är kopplad till Token livstid princip 2, som har en Session Token Max Age 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="ad760-253">Its service principal is linked to Token Lifetime Policy 2, which has a Session Token Max Age of 30 minutes.</span></span>
>
> <span data-ttu-id="ad760-254">Klockan 12:00, användaren startar en ny webbläsarsession och försöker få åtkomst till webbprogrammet A. Användaren omdirigeras till Azure AD och uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="ad760-254">At 12:00 PM, the user starts a new browser session and tries to access Web Application A. The user is redirected to Azure AD and is asked to sign in.</span></span> <span data-ttu-id="ad760-255">Detta skapar en cookie som har en sessionstoken i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ad760-255">This creates a cookie that has a session token in the browser.</span></span> <span data-ttu-id="ad760-256">Användaren omdirigeras till ett webbprogram med en ID-token som används att få åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-256">The user is redirected back to Web Application A with an ID token that allows the user to access the application.</span></span>
>
> <span data-ttu-id="ad760-257">Klockan 12:15 försöker användaren få åtkomst till Web Application B. Webbläsaren omdirigerar till Azure AD, som identifierar sessions-cookie.</span><span class="sxs-lookup"><span data-stu-id="ad760-257">At 12:15 PM, the user tries to access Web Application B. The browser redirects to Azure AD, which detects the session cookie.</span></span> <span data-ttu-id="ad760-258">Web Application B tjänstens huvudnamn är kopplad till Token livstid princip 2, men det är också en del av den överordnade organisationen med standard Token livstid princip 1.</span><span class="sxs-lookup"><span data-stu-id="ad760-258">Web Application B’s service principal is linked to Token Lifetime Policy 2, but it's also part of the parent organization, with default Token Lifetime Policy 1.</span></span> <span data-ttu-id="ad760-259">Token livstid princip 2 börjar gälla eftersom principer som är länkade till tjänstens huvudnamn har högre prioritet än standardprinciperna för organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-259">Token Lifetime Policy 2 takes effect because policies linked to service principals have a higher priority than organization default policies.</span></span> <span data-ttu-id="ad760-260">Sessionstoken ursprungligen utfärdades under de senaste 30 minuterna, så att det ska vara giltigt.</span><span class="sxs-lookup"><span data-stu-id="ad760-260">The session token was originally issued within the last 30 minutes, so it is considered valid.</span></span> <span data-ttu-id="ad760-261">Användaren omdirigeras tillbaka till Web Application B med en ID-token som ger dem behörighet.</span><span class="sxs-lookup"><span data-stu-id="ad760-261">The user is redirected back to Web Application B with an ID token that grants them access.</span></span>
>
> <span data-ttu-id="ad760-262">Klockan 13:00 försöker användaren få åtkomst till webbprogrammet A. Användaren omdirigeras till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad760-262">At 1:00 PM, the user tries to access Web Application A. The user is redirected to Azure AD.</span></span> <span data-ttu-id="ad760-263">Webbplatsen program inte är länkad till alla principer, men eftersom den är i en organisation med standard Token livstid princip 1 principen träder i kraft.</span><span class="sxs-lookup"><span data-stu-id="ad760-263">Web Application A is not linked to any policies, but because it is in an organization with default Token Lifetime Policy 1, that policy takes effect.</span></span> <span data-ttu-id="ad760-264">Sessions-cookie som ursprungligen utfärdades under de senaste åtta timmarna har identifierats.</span><span class="sxs-lookup"><span data-stu-id="ad760-264">The session cookie that was originally issued within the last eight hours is detected.</span></span> <span data-ttu-id="ad760-265">Tyst omdirigeras användaren till webbprogrammet A med en ny ID-token.</span><span class="sxs-lookup"><span data-stu-id="ad760-265">The user is silently redirected back to Web Application A with a new ID token.</span></span> <span data-ttu-id="ad760-266">Användaren krävs inte för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="ad760-266">The user is not required to authenticate.</span></span>
>
> <span data-ttu-id="ad760-267">Omedelbart efteråt försöker användaren få åtkomst till Web Application B. Användaren omdirigeras till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad760-267">Immediately afterward, the user tries to access Web Application B. The user is redirected to Azure AD.</span></span> <span data-ttu-id="ad760-268">Som börjar tidigare, Token livstid princip 2 gälla.</span><span class="sxs-lookup"><span data-stu-id="ad760-268">As before, Token Lifetime Policy 2 takes effect.</span></span> <span data-ttu-id="ad760-269">Eftersom token utfärdats mer än 30 minuter sedan, uppmanas användaren att ange sina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ad760-269">Because the token was issued more than 30 minutes ago, the user is prompted to reenter their sign-in credentials.</span></span> <span data-ttu-id="ad760-270">En helt ny sessionstoken och ID-token utfärdas.</span><span class="sxs-lookup"><span data-stu-id="ad760-270">A brand-new session token and ID token are issued.</span></span> <span data-ttu-id="ad760-271">Användaren kan sedan komma åt Web Application B.</span><span class="sxs-lookup"><span data-stu-id="ad760-271">The user can then access Web Application B.</span></span>
>
>

## <a name="configurable-policy-property-details"></a><span data-ttu-id="ad760-272">Information om konfigurerbar princip</span><span class="sxs-lookup"><span data-stu-id="ad760-272">Configurable policy property details</span></span>
### <a name="access-token-lifetime"></a><span data-ttu-id="ad760-273">Livslängd för åtkomst-Token</span><span class="sxs-lookup"><span data-stu-id="ad760-273">Access Token Lifetime</span></span>
<span data-ttu-id="ad760-274">**Sträng:** AccessTokenLifetime</span><span class="sxs-lookup"><span data-stu-id="ad760-274">**String:** AccessTokenLifetime</span></span>

<span data-ttu-id="ad760-275">**Påverkar:** åtkomsttoken, ID-token</span><span class="sxs-lookup"><span data-stu-id="ad760-275">**Affects:** Access tokens, ID tokens</span></span>

<span data-ttu-id="ad760-276">**Sammanfattning:** den här principen styr hur länge åtkomst och ID-token för den här resursen är giltiga.</span><span class="sxs-lookup"><span data-stu-id="ad760-276">**Summary:** This policy controls how long access and ID tokens for this resource are considered valid.</span></span> <span data-ttu-id="ad760-277">Minska egenskapen åtkomst livslängd för Token minskar du risken för en åtkomst-token eller ID-token som används av en skadlig aktören under en längre tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="ad760-277">Reducing the Access Token Lifetime property mitigates the risk of an access token or ID token being used by a malicious actor for an extended period of time.</span></span> <span data-ttu-id="ad760-278">(Dessa token kan inte återkallas.) En kompromiss är att prestanda påverkas negativt, eftersom token som behöver ersättas oftare.</span><span class="sxs-lookup"><span data-stu-id="ad760-278">(These tokens cannot be revoked.) The trade-off is that performance is adversely affected, because the tokens have to be replaced more often.</span></span>

### <a name="refresh-token-max-inactive-time"></a><span data-ttu-id="ad760-279">Uppdatera Token inaktiva Maxtid</span><span class="sxs-lookup"><span data-stu-id="ad760-279">Refresh Token Max Inactive Time</span></span>
<span data-ttu-id="ad760-280">**Sträng:** MaxInactiveTime</span><span class="sxs-lookup"><span data-stu-id="ad760-280">**String:** MaxInactiveTime</span></span>

<span data-ttu-id="ad760-281">**Påverkar:** Uppdateringstoken</span><span class="sxs-lookup"><span data-stu-id="ad760-281">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="ad760-282">**Sammanfattning:** den här principen styr hur gammal en uppdateringstoken kan vara innan en klient inte längre använda den för att hämta ett nytt åtkomst/uppdatera token par vid försök att komma åt den här resursen.</span><span class="sxs-lookup"><span data-stu-id="ad760-282">**Summary:** This policy controls how old a refresh token can be before a client can no longer use it to retrieve a new access/refresh token pair when attempting to access this resource.</span></span> <span data-ttu-id="ad760-283">Eftersom en ny uppdateringstoken vanligtvis returneras när en uppdateringstoken används förhindrar den här principen åtkomst om klienten försöker komma åt en resurs med hjälp av den aktuella uppdateringstoken under den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad760-283">Because a new refresh token usually is returned when a refresh token is used, this policy prevents access if the client tries to access any resource by using the current refresh token during the specified period of time.</span></span>

<span data-ttu-id="ad760-284">Den här principen gör att användare som inte har varit aktivt på deras klient autentiseras för att hämta en ny uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="ad760-284">This policy forces users who have not been active on their client to reauthenticate to retrieve a new refresh token.</span></span>

<span data-ttu-id="ad760-285">Uppdatera Token inaktiva Maxtid-egenskapen måste anges till ett lägre värde än en faktor Token Max Age och Multi-Factor uppdatera Token Max Age egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ad760-285">The Refresh Token Max Inactive Time property must be set to a lower value than the Single-Factor Token Max Age and the Multi-Factor Refresh Token Max Age properties.</span></span>

### <a name="single-factor-refresh-token-max-age"></a><span data-ttu-id="ad760-286">Enskild faktor uppdatera Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-286">Single-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="ad760-287">**Sträng:** MaxAgeSingleFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-287">**String:** MaxAgeSingleFactor</span></span>

<span data-ttu-id="ad760-288">**Påverkar:** Uppdateringstoken</span><span class="sxs-lookup"><span data-stu-id="ad760-288">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="ad760-289">**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en uppdateringstoken för att få ett nytt åtkomst/uppdatera token par efter de senaste autentiseras korrekt med hjälp av bara en enda faktor.</span><span class="sxs-lookup"><span data-stu-id="ad760-289">**Summary:** This policy controls how long a user can use a refresh token to get a new access/refresh token pair after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="ad760-290">När en användare autentiseras och tar emot en ny uppdateringstoken kan använda användaren uppdatering tokenflöde för den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad760-290">After a user authenticates and receives a new refresh token, the user can use the refresh token flow for the specified period of time.</span></span> <span data-ttu-id="ad760-291">(Detta är SANT så länge den aktuella uppdateringstoken inte har återkallats och det inte inte används längre än den inaktiva tiden.) Då måste användaren autentiseras för att få en ny uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="ad760-291">(This is true as long as the current refresh token is not revoked, and it is not left unused for longer than the inactive time.) At that point, the user is forced to reauthenticate to receive a new refresh token.</span></span>

<span data-ttu-id="ad760-292">Minska maximal ålder tvingar användare att autentisera oftare.</span><span class="sxs-lookup"><span data-stu-id="ad760-292">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="ad760-293">Eftersom en faktor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du anger egenskapen till ett värde som är lika med eller mindre än egenskapen Multi-Factor uppdatera Token Max Age.</span><span class="sxs-lookup"><span data-stu-id="ad760-293">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or lesser than the Multi-Factor Refresh Token Max Age property.</span></span>

### <a name="multi-factor-refresh-token-max-age"></a><span data-ttu-id="ad760-294">Multi-Factor uppdatera Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-294">Multi-Factor Refresh Token Max Age</span></span>
<span data-ttu-id="ad760-295">**Sträng:** MaxAgeMultiFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-295">**String:** MaxAgeMultiFactor</span></span>

<span data-ttu-id="ad760-296">**Påverkar:** Uppdateringstoken</span><span class="sxs-lookup"><span data-stu-id="ad760-296">**Affects:** Refresh tokens</span></span>

<span data-ttu-id="ad760-297">**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en uppdateringstoken för att få ett nytt åtkomst/uppdatera token par efter de senaste autentiseras korrekt med hjälp av flera faktorer.</span><span class="sxs-lookup"><span data-stu-id="ad760-297">**Summary:** This policy controls how long a user can use a refresh token to get a new access/refresh token pair after they last authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="ad760-298">När en användare autentiseras och tar emot en ny uppdateringstoken kan använda användaren uppdatering tokenflöde för den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad760-298">After a user authenticates and receives a new refresh token, the user can use the refresh token flow for the specified period of time.</span></span> <span data-ttu-id="ad760-299">(Detta är SANT så länge som den aktuella uppdateringstoken inte har återkallats och är inte inte används längre än den inaktiva tiden.) Då tvingas användare att autentiseras för att få en ny uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="ad760-299">(This is true as long as the current refresh token is not revoked, and it is not unused for longer than the inactive time.) At that point, users are forced to reauthenticate to receive a new refresh token.</span></span>

<span data-ttu-id="ad760-300">Minska maximal ålder tvingar användare att autentisera oftare.</span><span class="sxs-lookup"><span data-stu-id="ad760-300">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="ad760-301">Eftersom en faktor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du anger egenskapen till ett värde som är lika med eller större än egenskapen enskild faktor uppdatera Token Max Age.</span><span class="sxs-lookup"><span data-stu-id="ad760-301">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or greater than the Single-Factor Refresh Token Max Age property.</span></span>

### <a name="single-factor-session-token-max-age"></a><span data-ttu-id="ad760-302">Enskild faktor Session Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-302">Single-Factor Session Token Max Age</span></span>
<span data-ttu-id="ad760-303">**Sträng:** MaxAgeSessionSingleFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-303">**String:** MaxAgeSessionSingleFactor</span></span>

<span data-ttu-id="ad760-304">**Påverkar:** Session token (beständiga och Uppdateringsvärdet)</span><span class="sxs-lookup"><span data-stu-id="ad760-304">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="ad760-305">**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en sessionstoken få ett nytt-ID och sessionstoken efter de senaste autentiseras korrekt med hjälp av bara en enda faktor.</span><span class="sxs-lookup"><span data-stu-id="ad760-305">**Summary:** This policy controls how long a user can use a session token to get a new ID and session token after they last authenticated successfully by using only a single factor.</span></span> <span data-ttu-id="ad760-306">När en användare autentiseras och tar emot en ny session-token kan använda användaren session tokenflöde för den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad760-306">After a user authenticates and receives a new session token, the user can use the session token flow for the specified period of time.</span></span> <span data-ttu-id="ad760-307">(Detta är SANT så länge den aktuella sessionstoken inte har återkallats och inte har upphört att gälla.) Efter en angiven tidsperiod måste användaren autentiseras för att få en ny session-token.</span><span class="sxs-lookup"><span data-stu-id="ad760-307">(This is true as long as the current session token is not revoked and has not expired.) After the specified period of time, the user is forced to reauthenticate to receive a new session token.</span></span>

<span data-ttu-id="ad760-308">Minska maximal ålder tvingar användare att autentisera oftare.</span><span class="sxs-lookup"><span data-stu-id="ad760-308">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="ad760-309">Eftersom en faktor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du anger egenskapen till ett värde som är lika med eller mindre än egenskapen Multi-Factor Session Token Max Age.</span><span class="sxs-lookup"><span data-stu-id="ad760-309">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or less than the Multi-Factor Session Token Max Age property.</span></span>

### <a name="multi-factor-session-token-max-age"></a><span data-ttu-id="ad760-310">Multi-Factor Session Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-310">Multi-Factor Session Token Max Age</span></span>
<span data-ttu-id="ad760-311">**Sträng:** MaxAgeSessionMultiFactor</span><span class="sxs-lookup"><span data-stu-id="ad760-311">**String:** MaxAgeSessionMultiFactor</span></span>

<span data-ttu-id="ad760-312">**Påverkar:** Session token (beständiga och Uppdateringsvärdet)</span><span class="sxs-lookup"><span data-stu-id="ad760-312">**Affects:** Session tokens (persistent and nonpersistent)</span></span>

<span data-ttu-id="ad760-313">**Sammanfattning:** den här principen styr hur lång tid en användare kan använda en sessionstoken få ett nytt-ID och session token efter den senaste gången som de har autentiseras med hjälp av flera faktorer.</span><span class="sxs-lookup"><span data-stu-id="ad760-313">**Summary:** This policy controls how long a user can use a session token to get a new ID and session token after the last time they authenticated successfully by using multiple factors.</span></span> <span data-ttu-id="ad760-314">När en användare autentiseras och tar emot en ny session-token kan använda användaren session tokenflöde för den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad760-314">After a user authenticates and receives a new session token, the user can use the session token flow for the specified period of time.</span></span> <span data-ttu-id="ad760-315">(Detta är SANT så länge den aktuella sessionstoken inte har återkallats och inte har upphört att gälla.) Efter en angiven tidsperiod måste användaren autentiseras för att få en ny session-token.</span><span class="sxs-lookup"><span data-stu-id="ad760-315">(This is true as long as the current session token is not revoked and has not expired.) After the specified period of time, the user is forced to reauthenticate to receive a new session token.</span></span>

<span data-ttu-id="ad760-316">Minska maximal ålder tvingar användare att autentisera oftare.</span><span class="sxs-lookup"><span data-stu-id="ad760-316">Reducing the max age forces users to authenticate more often.</span></span> <span data-ttu-id="ad760-317">Eftersom en faktor-autentisering anses vara mindre säkert än multifaktorautentisering, rekommenderar vi att du anger egenskapen till ett värde som är lika med eller större än egenskapen enskild faktor Session Token Max Age.</span><span class="sxs-lookup"><span data-stu-id="ad760-317">Because single-factor authentication is considered less secure than multi-factor authentication, we recommend that you set this property to a value that is equal to or greater than the Single-Factor Session Token Max Age property.</span></span>

## <a name="example-token-lifetime-policies"></a><span data-ttu-id="ad760-318">Exempel på principer livslängd för token</span><span class="sxs-lookup"><span data-stu-id="ad760-318">Example token lifetime policies</span></span>
<span data-ttu-id="ad760-319">Många scenarier är möjliga i Azure AD när du kan skapa och hantera token livslängd för appar, tjänstens huvudnamn och din organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-319">Many scenarios are possible in Azure AD when you can create and manage token lifetimes for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="ad760-320">I det här avsnittet går vi igenom några vanliga princip scenarier som kan hjälpa dig att införa nya regler för:</span><span class="sxs-lookup"><span data-stu-id="ad760-320">In this section, we walk through a few common policy scenarios that can help you impose new rules for:</span></span>

* <span data-ttu-id="ad760-321">Livslängd för token</span><span class="sxs-lookup"><span data-stu-id="ad760-321">Token Lifetime</span></span>
* <span data-ttu-id="ad760-322">Token inaktiva Maxtid</span><span class="sxs-lookup"><span data-stu-id="ad760-322">Token Max Inactive Time</span></span>
* <span data-ttu-id="ad760-323">Token maximal ålder</span><span class="sxs-lookup"><span data-stu-id="ad760-323">Token Max Age</span></span>

<span data-ttu-id="ad760-324">I exemplen, kan du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="ad760-324">In the examples, you can learn how to:</span></span>

* <span data-ttu-id="ad760-325">Hantera standardprincipen för en organisation</span><span class="sxs-lookup"><span data-stu-id="ad760-325">Manage an organization's default policy</span></span>
* <span data-ttu-id="ad760-326">Skapa en princip för web-inloggning</span><span class="sxs-lookup"><span data-stu-id="ad760-326">Create a policy for web sign-in</span></span>
* <span data-ttu-id="ad760-327">Skapa en princip för en intern app som anropar ett webb-API</span><span class="sxs-lookup"><span data-stu-id="ad760-327">Create a policy for a native app that calls a web API</span></span>
* <span data-ttu-id="ad760-328">Hantera en princip för Avancerat</span><span class="sxs-lookup"><span data-stu-id="ad760-328">Manage an advanced policy</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ad760-329">Krav</span><span class="sxs-lookup"><span data-stu-id="ad760-329">Prerequisites</span></span>
<span data-ttu-id="ad760-330">I följande exempel du skapa, uppdatera, länkar och ta bort principer för appar, tjänstens huvudnamn och din organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-330">In the following examples, you create, update, link, and delete policies for apps, service principals, and your overall organization.</span></span> <span data-ttu-id="ad760-331">Om du har använt Azure AD, rekommenderar vi att du lär dig mer om [hur du hämtar en Azure AD-klient](active-directory-howto-tenant.md) innan du fortsätter med de här exemplen.</span><span class="sxs-lookup"><span data-stu-id="ad760-331">If you are new to Azure AD, we recommend that you learn about [how to get an Azure AD tenant](active-directory-howto-tenant.md) before you proceed with these examples.</span></span>  

<span data-ttu-id="ad760-332">Utför följande steg för att komma igång:</span><span class="sxs-lookup"><span data-stu-id="ad760-332">To get started, do the following steps:</span></span>

1. <span data-ttu-id="ad760-333">Ladda ned senaste [Azure AD PowerShell-modulen offentliga förhandsversionen](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="ad760-333">Download the latest [Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>
2. <span data-ttu-id="ad760-334">Kör den `Connect` kommando för att logga in på ditt Azure AD-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="ad760-334">Run the `Connect` command to sign in to your Azure AD admin account.</span></span> <span data-ttu-id="ad760-335">Kör det här kommandot varje gång startar du en ny session.</span><span class="sxs-lookup"><span data-stu-id="ad760-335">Run this command each time you start a new session.</span></span>

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. <span data-ttu-id="ad760-336">Kör följande kommando för att se alla principer som har skapats i din organisation.</span><span class="sxs-lookup"><span data-stu-id="ad760-336">To see all policies that have been created in your organization, run the following command.</span></span> <span data-ttu-id="ad760-337">Kör detta kommando efter de flesta åtgärder i följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="ad760-337">Run this command after most operations in the following scenarios.</span></span> <span data-ttu-id="ad760-338">Kör kommandot hjälper dig också att hämta den ** ** dina principer.</span><span class="sxs-lookup"><span data-stu-id="ad760-338">Running the command also helps you get the ** ** of your policies.</span></span>

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a><span data-ttu-id="ad760-339">Exempel: Hantera standardprincipen för en organisation</span><span class="sxs-lookup"><span data-stu-id="ad760-339">Example: Manage an organization's default policy</span></span>
<span data-ttu-id="ad760-340">I det här exemplet skapar du en princip som gör att användare kan logga in mindre ofta i hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-340">In this example, you create a policy that lets your users sign in less frequently across your entire organization.</span></span> <span data-ttu-id="ad760-341">Om du vill göra det, skapar du en livslängd för token-princip för enskild faktor uppdatera token som används inom organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-341">To do this, create a token lifetime policy for Single-Factor Refresh Tokens, which is applied across your organization.</span></span> <span data-ttu-id="ad760-342">Principen tillämpas på alla program i din organisation och varje tjänstens huvudnamn som inte redan har en princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-342">The policy is applied to every application in your organization, and to each service principal that doesn’t already have a policy set.</span></span>

1. <span data-ttu-id="ad760-343">Skapa en princip för livslängd för token.</span><span class="sxs-lookup"><span data-stu-id="ad760-343">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="ad760-344">Ange Uppdateringstoken som en faktor att ”tills har återkallats”.</span><span class="sxs-lookup"><span data-stu-id="ad760-344">Set the Single-Factor Refresh Token to "until-revoked."</span></span> <span data-ttu-id="ad760-345">Token gälla inte förrän åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="ad760-345">The token doesn't expire until access is revoked.</span></span> <span data-ttu-id="ad760-346">Skapa följande principdefinitionen:</span><span class="sxs-lookup"><span data-stu-id="ad760-346">Create the following policy definition:</span></span>

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  <span data-ttu-id="ad760-347">Om du vill skapa principen, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-347">To create the policy, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  <span data-ttu-id="ad760-348">Att se din nya princip och principen **ObjectId**, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-348">To see your new policy, and to get the policy's **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="ad760-349">Uppdatera principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-349">Update the policy.</span></span>

    <span data-ttu-id="ad760-350">Du kan välja den första principen som du anger i det här exemplet inte är så strikta din tjänst kräver.</span><span class="sxs-lookup"><span data-stu-id="ad760-350">You might decide that the first policy you set in this example is not as strict as your service requires.</span></span> <span data-ttu-id="ad760-351">Ange din enda faktor uppdatera Token att gälla inom två dagar genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-351">To set your Single-Factor Refresh Token to expire in two days, run the following command:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a><span data-ttu-id="ad760-352">Exempel: Skapa en princip för web-inloggning</span><span class="sxs-lookup"><span data-stu-id="ad760-352">Example: Create a policy for web sign-in</span></span>

<span data-ttu-id="ad760-353">I det här exemplet skapar du en princip som kräver att användare autentiseras oftare i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ad760-353">In this example, you create a policy that requires users to authenticate more frequently in your web app.</span></span> <span data-ttu-id="ad760-354">Den här principen anger livslängd för token som åtkomst-ID och ett Multi-Factor sessionstoken maximal ålder till tjänstens huvudnamn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ad760-354">This policy sets the lifetime of the access/ID tokens and the max age of a multi-factor session token to the service principal of your web app.</span></span>

1. <span data-ttu-id="ad760-355">Skapa en princip för livslängd för token.</span><span class="sxs-lookup"><span data-stu-id="ad760-355">Create a token lifetime policy.</span></span>

    <span data-ttu-id="ad760-356">Den här principen för web inloggning, anger åtkomst/ID livslängd för token och token ålder max single-factor-session till två timmar.</span><span class="sxs-lookup"><span data-stu-id="ad760-356">This policy, for web sign-in, sets the access/ID token lifetime and the max single-factor session token age to two hours.</span></span>

    1.  <span data-ttu-id="ad760-357">Om du vill skapa principen, kör du kommandot:</span><span class="sxs-lookup"><span data-stu-id="ad760-357">To create the policy, run this command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="ad760-358">Se din nya principen och för att få principen **ObjectId**, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-358">To see your new policy, and to get the policy **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  <span data-ttu-id="ad760-359">Tilldela principen till tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-359">Assign the policy to your service principal.</span></span> <span data-ttu-id="ad760-360">Du måste också hämta de **ObjectId** för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-360">You also need to get the **ObjectId** of your service principal.</span></span> 

    1.  <span data-ttu-id="ad760-361">Du kan fråga om du vill se alla organisationens tjänstens huvudnamn [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="ad760-361">To see all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="ad760-362">I [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), logga in på Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="ad760-362">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in to your Azure AD account.</span></span>

    2.  <span data-ttu-id="ad760-363">När du har den **ObjectId** av din tjänstens huvudnamn, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-363">When you have the **ObjectId** of your service principal, run the following command:</span></span>

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a><span data-ttu-id="ad760-364">Exempel: Skapa en princip för en intern app som anropar ett webb-API</span><span class="sxs-lookup"><span data-stu-id="ad760-364">Example: Create a policy for a native app that calls a web API</span></span>
<span data-ttu-id="ad760-365">I det här exemplet kan du skapa en princip som kräver att användarna autentiseras mindre ofta.</span><span class="sxs-lookup"><span data-stu-id="ad760-365">In this example, you create a policy that requires users to authenticate less frequently.</span></span> <span data-ttu-id="ad760-366">Principen förlängs också hur lång tid som en användare kan vara inaktiv innan användaren måste autentiseras.</span><span class="sxs-lookup"><span data-stu-id="ad760-366">The policy also lengthens the amount of time a user can be inactive before the user must reauthenticate.</span></span> <span data-ttu-id="ad760-367">Principen tillämpas på webb-API.</span><span class="sxs-lookup"><span data-stu-id="ad760-367">The policy is applied to the web API.</span></span> <span data-ttu-id="ad760-368">När den inbyggda appen begär webb-API som en resurs, används den här principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-368">When the native app requests the web API as a resource, this policy is applied.</span></span>

1. <span data-ttu-id="ad760-369">Skapa en princip för livslängd för token.</span><span class="sxs-lookup"><span data-stu-id="ad760-369">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="ad760-370">Om du vill skapa en strikt princip för en webb-API, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-370">To create a strict policy for a web API, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="ad760-371">Se din nya principen och för att få principen **ObjectId**, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-371">To see your new policy, and to get the policy **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="ad760-372">Tilldela principen till ditt webb-API.</span><span class="sxs-lookup"><span data-stu-id="ad760-372">Assign the policy to your web API.</span></span> <span data-ttu-id="ad760-373">Du måste också hämta de **ObjectId** för programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-373">You also need to get the **ObjectId** of your application.</span></span> <span data-ttu-id="ad760-374">Det bästa sättet att hitta din app **ObjectId** är att använda den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ad760-374">The best way to find your app's **ObjectId** is to use the [Azure portal](https://portal.azure.com/).</span></span>

   <span data-ttu-id="ad760-375">När du har den **ObjectId** för din app, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-375">When you have the **ObjectId** of your app, run the following command:</span></span>

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of the Application> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a><span data-ttu-id="ad760-376">Exempel: Hantera en princip för Avancerat</span><span class="sxs-lookup"><span data-stu-id="ad760-376">Example: Manage an advanced policy</span></span>
<span data-ttu-id="ad760-377">I det här exemplet skapar du några principer för att lära dig hur systemets prioritet fungerar.</span><span class="sxs-lookup"><span data-stu-id="ad760-377">In this example, you create a few policies, to learn how the priority system works.</span></span> <span data-ttu-id="ad760-378">Du kan också lära dig hur du hanterar flera principer som tillämpas på flera objekt.</span><span class="sxs-lookup"><span data-stu-id="ad760-378">You also can learn how to manage multiple policies that are applied to several objects.</span></span>

1. <span data-ttu-id="ad760-379">Skapa en princip för livslängd för token.</span><span class="sxs-lookup"><span data-stu-id="ad760-379">Create a token lifetime policy.</span></span>

    1.  <span data-ttu-id="ad760-380">Om du vill skapa en standardprincip för organisationen som anger livslängd för en faktor uppdatera Token till 30 dagar, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-380">To create an organization default policy that sets the Single-Factor Refresh Token lifetime to 30 days, run the following command:</span></span>

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  <span data-ttu-id="ad760-381">Att se din nya princip och principen **ObjectId**, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-381">To see your new policy, and to get the policy's **ObjectId**, run the following command:</span></span>

        ```PowerShell
        Get-AzureADPolicy
        ```

2. <span data-ttu-id="ad760-382">Tilldela principen till ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ad760-382">Assign the policy to a service principal.</span></span>

    <span data-ttu-id="ad760-383">Du har nu en princip som gäller för hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-383">Now, you have a policy that applies to the entire organization.</span></span> <span data-ttu-id="ad760-384">Du kanske vill bevara 30-dagars principen för en specifik tjänstens huvudnamn, men ändra standardprincipen för organisationen till den övre gränsen för ”tills har återkallats”.</span><span class="sxs-lookup"><span data-stu-id="ad760-384">You might want to preserve this 30-day policy for a specific service principal, but change the organization default policy to the upper limit of "until-revoked."</span></span>

    1.  <span data-ttu-id="ad760-385">Du kan fråga om du vill se alla organisationens tjänstens huvudnamn [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span><span class="sxs-lookup"><span data-stu-id="ad760-385">To see all your organization's service principals, you can query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity).</span></span> <span data-ttu-id="ad760-386">I [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), logga in med hjälp av Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="ad760-386">Or, in [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), sign in by using your Azure AD account.</span></span>

    2.  <span data-ttu-id="ad760-387">När du har den **ObjectId** av din tjänstens huvudnamn, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad760-387">When you have the **ObjectId** of your service principal, run the following command:</span></span>

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
            ```
        
3. <span data-ttu-id="ad760-388">Ange den `IsOrganizationDefault` flaggan false:</span><span class="sxs-lookup"><span data-stu-id="ad760-388">Set the `IsOrganizationDefault` flag to false:</span></span>

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. <span data-ttu-id="ad760-389">Skapa en ny organisation standardprincip:</span><span class="sxs-lookup"><span data-stu-id="ad760-389">Create a new organization default policy:</span></span>

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    <span data-ttu-id="ad760-390">Nu har du den ursprungliga principen som är kopplad till tjänstens huvudnamn och den nya principen har angetts som din organisation standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="ad760-390">You now have the original policy linked to your service principal, and the new policy is set as your organization default policy.</span></span> <span data-ttu-id="ad760-391">Det är viktigt att komma ihåg att principer som tillämpas på tjänstens huvudnamn har högre prioritet än standardprinciperna för organisationen.</span><span class="sxs-lookup"><span data-stu-id="ad760-391">It's important to remember that policies applied to service principals have priority over organization default policies.</span></span>

## <a name="cmdlet-reference"></a><span data-ttu-id="ad760-392">Cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="ad760-392">Cmdlet reference</span></span>

### <a name="manage-policies"></a><span data-ttu-id="ad760-393">Hantera principer</span><span class="sxs-lookup"><span data-stu-id="ad760-393">Manage policies</span></span>

<span data-ttu-id="ad760-394">Du kan använda följande cmdletar för att hantera principer.</span><span class="sxs-lookup"><span data-stu-id="ad760-394">You can use the following cmdlets to manage policies.</span></span>

#### <a name="new-azureadpolicy"></a><span data-ttu-id="ad760-395">Ny AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-395">New-AzureADPolicy</span></span>

<span data-ttu-id="ad760-396">Skapar en ny princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-396">Creates a new policy.</span></span>

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| <span data-ttu-id="ad760-397">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-397">Parameters</span></span> | <span data-ttu-id="ad760-398">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-398">Description</span></span> | <span data-ttu-id="ad760-399">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-399">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Definition</code> |<span data-ttu-id="ad760-400">Matris med stringified JSON som innehåller alla principregler.</span><span class="sxs-lookup"><span data-stu-id="ad760-400">Array of stringified JSON that contains all the policy's rules.</span></span> | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="ad760-401">Principnamnet textsträng.</span><span class="sxs-lookup"><span data-stu-id="ad760-401">String of the policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |<span data-ttu-id="ad760-402">Om värdet är true anger du principen som organisationens standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="ad760-402">If true, sets the policy as the organization's default policy.</span></span> <span data-ttu-id="ad760-403">Om värdet är FALSKT får ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="ad760-403">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |<span data-ttu-id="ad760-404">Typen av princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-404">Type of policy.</span></span> <span data-ttu-id="ad760-405">Token livslängd alltid använda ”TokenLifetimePolicy”.</span><span class="sxs-lookup"><span data-stu-id="ad760-405">For token lifetimes, always use "TokenLifetimePolicy."</span></span> | `-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="ad760-406"><code>&#8209;AlternativeIdentifier</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-406"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="ad760-407">Anger ett alternativt ID för principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-407">Sets an alternative ID for the policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a><span data-ttu-id="ad760-408">Get-AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-408">Get-AzureADPolicy</span></span>
<span data-ttu-id="ad760-409">Hämtar alla Azure AD-principer eller en angiven princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-409">Gets all Azure AD policies or a specified policy.</span></span>

```PowerShell
Get-AzureADPolicy
```

| <span data-ttu-id="ad760-410">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-410">Parameters</span></span> | <span data-ttu-id="ad760-411">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-411">Description</span></span> | <span data-ttu-id="ad760-412">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-412">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad760-413"><code>&#8209;Id</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-413"><code>&#8209;Id</code> [Optional]</span></span> |<span data-ttu-id="ad760-414">**Objekt-ID (Id)** på den princip som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ad760-414">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a><span data-ttu-id="ad760-415">Get-AzureADPolicyAppliedObject</span><span class="sxs-lookup"><span data-stu-id="ad760-415">Get-AzureADPolicyAppliedObject</span></span>
<span data-ttu-id="ad760-416">Hämtar alla appar och tjänstens huvudnamn som är länkade till en princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-416">Gets all apps and service principals that are linked to a policy.</span></span>

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| <span data-ttu-id="ad760-417">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-417">Parameters</span></span> | <span data-ttu-id="ad760-418">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-418">Description</span></span> | <span data-ttu-id="ad760-419">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-419">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-420">**Objekt-ID (Id)** på den princip som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ad760-420">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a><span data-ttu-id="ad760-421">Ange AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-421">Set-AzureADPolicy</span></span>
<span data-ttu-id="ad760-422">Uppdaterar en befintlig princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-422">Updates an existing policy.</span></span>

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| <span data-ttu-id="ad760-423">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-423">Parameters</span></span> | <span data-ttu-id="ad760-424">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-424">Description</span></span> | <span data-ttu-id="ad760-425">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-425">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-426">**Objekt-ID (Id)** på den princip som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ad760-426">**ObjectId (Id)** of the policy you want.</span></span> |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |<span data-ttu-id="ad760-427">Principnamnet textsträng.</span><span class="sxs-lookup"><span data-stu-id="ad760-427">String of the policy name.</span></span> |`-DisplayName "MyTokenPolicy"` |
| <span data-ttu-id="ad760-428"><code>&#8209;Definition</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-428"><code>&#8209;Definition</code> [Optional]</span></span> |<span data-ttu-id="ad760-429">Matris med stringified JSON som innehåller alla principregler.</span><span class="sxs-lookup"><span data-stu-id="ad760-429">Array of stringified JSON that contains all the policy's rules.</span></span> |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <span data-ttu-id="ad760-430"><code>&#8209;IsOrganizationDefault</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-430"><code>&#8209;IsOrganizationDefault</code> [Optional]</span></span> |<span data-ttu-id="ad760-431">Om värdet är true anger du principen som organisationens standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="ad760-431">If true, sets the policy as the organization's default policy.</span></span> <span data-ttu-id="ad760-432">Om värdet är FALSKT får ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="ad760-432">If false, does nothing.</span></span> |`-IsOrganizationDefault $true` |
| <span data-ttu-id="ad760-433"><code>&#8209;Type</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-433"><code>&#8209;Type</code> [Optional]</span></span> |<span data-ttu-id="ad760-434">Typen av princip.</span><span class="sxs-lookup"><span data-stu-id="ad760-434">Type of policy.</span></span> <span data-ttu-id="ad760-435">Token livslängd alltid använda ”TokenLifetimePolicy”.</span><span class="sxs-lookup"><span data-stu-id="ad760-435">For token lifetimes, always use "TokenLifetimePolicy."</span></span> |`-Type "TokenLifetimePolicy"` |
| <span data-ttu-id="ad760-436"><code>&#8209;AlternativeIdentifier</code>[Valfritt]</span><span class="sxs-lookup"><span data-stu-id="ad760-436"><code>&#8209;AlternativeIdentifier</code> [Optional]</span></span> |<span data-ttu-id="ad760-437">Anger ett alternativt ID för principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-437">Sets an alternative ID for the policy.</span></span> |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a><span data-ttu-id="ad760-438">Ta bort AzureADPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-438">Remove-AzureADPolicy</span></span>
<span data-ttu-id="ad760-439">Tar bort den angivna principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-439">Deletes the specified policy.</span></span>

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| <span data-ttu-id="ad760-440">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-440">Parameters</span></span> | <span data-ttu-id="ad760-441">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-441">Description</span></span> | <span data-ttu-id="ad760-442">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-442">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-443">**Objekt-ID (Id)** på den princip som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ad760-443">**ObjectId (Id)** of the policy you want.</span></span> | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a><span data-ttu-id="ad760-444">Användningsprinciper</span><span class="sxs-lookup"><span data-stu-id="ad760-444">Application policies</span></span>
<span data-ttu-id="ad760-445">Du kan använda följande cmdletar för principer för program.</span><span class="sxs-lookup"><span data-stu-id="ad760-445">You can use the following cmdlets for application policies.</span></span></br></br>

#### <a name="add-azureadapplicationpolicy"></a><span data-ttu-id="ad760-446">Lägg till AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-446">Add-AzureADApplicationPolicy</span></span>
<span data-ttu-id="ad760-447">Länkar den angivna principen till ett program.</span><span class="sxs-lookup"><span data-stu-id="ad760-447">Links the specified policy to an application.</span></span>

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="ad760-448">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-448">Parameters</span></span> | <span data-ttu-id="ad760-449">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-449">Description</span></span> | <span data-ttu-id="ad760-450">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-450">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-451">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-451">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="ad760-452">**ObjectId** av principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-452">**ObjectId** of the policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a><span data-ttu-id="ad760-453">Get-AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-453">Get-AzureADApplicationPolicy</span></span>
<span data-ttu-id="ad760-454">Hämtar den princip som har tilldelats ett program.</span><span class="sxs-lookup"><span data-stu-id="ad760-454">Gets the policy that is assigned to an application.</span></span>

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| <span data-ttu-id="ad760-455">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-455">Parameters</span></span> | <span data-ttu-id="ad760-456">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-456">Description</span></span> | <span data-ttu-id="ad760-457">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-457">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-458">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-458">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a><span data-ttu-id="ad760-459">Ta bort AzureADApplicationPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-459">Remove-AzureADApplicationPolicy</span></span>
<span data-ttu-id="ad760-460">Tar bort en princip från ett program.</span><span class="sxs-lookup"><span data-stu-id="ad760-460">Removes a policy from an application.</span></span>

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="ad760-461">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-461">Parameters</span></span> | <span data-ttu-id="ad760-462">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-462">Description</span></span> | <span data-ttu-id="ad760-463">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-463">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-464">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-464">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="ad760-465">**ObjectId** av principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-465">**ObjectId** of the policy.</span></span> | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a><span data-ttu-id="ad760-466">Tjänstens huvudnamn principer</span><span class="sxs-lookup"><span data-stu-id="ad760-466">Service principal policies</span></span>
<span data-ttu-id="ad760-467">Du kan använda följande cmdletar för tjänstens huvudnamn principer.</span><span class="sxs-lookup"><span data-stu-id="ad760-467">You can use the following cmdlets for service principal policies.</span></span>

#### <a name="add-azureadserviceprincipalpolicy"></a><span data-ttu-id="ad760-468">Lägg till AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-468">Add-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="ad760-469">Länkar den angivna principen till ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ad760-469">Links the specified policy to a service principal.</span></span>

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| <span data-ttu-id="ad760-470">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-470">Parameters</span></span> | <span data-ttu-id="ad760-471">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-471">Description</span></span> | <span data-ttu-id="ad760-472">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-472">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-473">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-473">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |<span data-ttu-id="ad760-474">**ObjectId** av principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-474">**ObjectId** of the policy.</span></span> | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a><span data-ttu-id="ad760-475">Get-AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-475">Get-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="ad760-476">Hämtar alla principer som är länkad till angivna tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-476">Gets any policy linked to the specified service principal.</span></span>

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| <span data-ttu-id="ad760-477">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-477">Parameters</span></span> | <span data-ttu-id="ad760-478">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-478">Description</span></span> | <span data-ttu-id="ad760-479">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-479">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-480">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-480">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a><span data-ttu-id="ad760-481">Ta bort AzureADServicePrincipalPolicy</span><span class="sxs-lookup"><span data-stu-id="ad760-481">Remove-AzureADServicePrincipalPolicy</span></span>
<span data-ttu-id="ad760-482">Tar bort principen från den angivna tjänsten huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="ad760-482">Removes the policy from the specified service principal.</span></span>

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| <span data-ttu-id="ad760-483">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ad760-483">Parameters</span></span> | <span data-ttu-id="ad760-484">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad760-484">Description</span></span> | <span data-ttu-id="ad760-485">Exempel</span><span class="sxs-lookup"><span data-stu-id="ad760-485">Example</span></span> |
| --- | --- | --- |
| <code>&#8209;Id</code> |<span data-ttu-id="ad760-486">**Objekt-ID (Id)** av programmet.</span><span class="sxs-lookup"><span data-stu-id="ad760-486">**ObjectId (Id)** of the application.</span></span> | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |<span data-ttu-id="ad760-487">**ObjectId** av principen.</span><span class="sxs-lookup"><span data-stu-id="ad760-487">**ObjectId** of the policy.</span></span> | `-PolicyId <ObjectId of Policy>` |
