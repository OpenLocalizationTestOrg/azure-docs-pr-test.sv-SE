---
title: Sessionen Management - verktyget Microsoft Threat modellering - Azure | Microsoft Docs
description: "ändringar för hot som exponeras i verktyget Modeling hot"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 56471d8ef68eacacb3ecebad5056d7e7a9f3ca40
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="de545-103">Säkerhet ram: Sessionshantering | Artiklar</span><span class="sxs-lookup"><span data-stu-id="de545-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="de545-104">Produkter eller tjänster</span><span class="sxs-lookup"><span data-stu-id="de545-104">Product/Service</span></span> | <span data-ttu-id="de545-105">Artikel</span><span class="sxs-lookup"><span data-stu-id="de545-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="de545-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="de545-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="de545-107">Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD</span><span class="sxs-lookup"><span data-stu-id="de545-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="de545-108">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="de545-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="de545-109">Använd begränsad livslängd för genererade SaS-token</span><span class="sxs-lookup"><span data-stu-id="de545-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="de545-110">**Azure dokumentet DB**</span><span class="sxs-lookup"><span data-stu-id="de545-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="de545-111">Använda minsta livstid för token för den genererade resurs token</span><span class="sxs-lookup"><span data-stu-id="de545-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="de545-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="de545-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="de545-113">Implementera rätt logga ut med WsFederation metoder när du använder AD FS</span><span class="sxs-lookup"><span data-stu-id="de545-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="de545-114">**Identity Server**</span><span class="sxs-lookup"><span data-stu-id="de545-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="de545-115">Implementera rätt logga ut när du använder Identity Server</span><span class="sxs-lookup"><span data-stu-id="de545-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="de545-116">**Webbprogram**</span><span class="sxs-lookup"><span data-stu-id="de545-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="de545-117">Program som är tillgängliga via HTTPS måste använda säkra cookies</span><span class="sxs-lookup"><span data-stu-id="de545-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="de545-118">Alla HTTP-baserade program bör ange http endast för cookie-definition</span><span class="sxs-lookup"><span data-stu-id="de545-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="de545-119">Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor</span><span class="sxs-lookup"><span data-stu-id="de545-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="de545-120">Ställ in sessionen för inaktivitet livslängd</span><span class="sxs-lookup"><span data-stu-id="de545-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="de545-121">Implementera rätt logga ut från programmet</span><span class="sxs-lookup"><span data-stu-id="de545-121">Implement proper logout from the application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="de545-122">**Webb-API**</span><span class="sxs-lookup"><span data-stu-id="de545-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="de545-123">Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er</span><span class="sxs-lookup"><span data-stu-id="de545-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="de545-124"><a id="logout-adal"></a>Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD</span><span class="sxs-lookup"><span data-stu-id="de545-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="de545-125">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-125">Title</span></span>                   | <span data-ttu-id="de545-126">Information</span><span class="sxs-lookup"><span data-stu-id="de545-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-127">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-127">**Component**</span></span>               | <span data-ttu-id="de545-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="de545-128">Azure AD</span></span> | 
| <span data-ttu-id="de545-129">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-129">**SDL Phase**</span></span>               | <span data-ttu-id="de545-130">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-130">Build</span></span> |  
| <span data-ttu-id="de545-131">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-131">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-132">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-132">Generic</span></span> |
| <span data-ttu-id="de545-133">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-133">**Attributes**</span></span>              | <span data-ttu-id="de545-134">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-134">N/A</span></span>  |
| <span data-ttu-id="de545-135">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-135">**References**</span></span>              | <span data-ttu-id="de545-136">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-136">N/A</span></span>  |
| <span data-ttu-id="de545-137">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-137">**Steps**</span></span> | <span data-ttu-id="de545-138">Om programmet är beroende av åtkomst-token som utfärdas av Azure AD, anropa händelsehanteraren logga ut</span><span class="sxs-lookup"><span data-stu-id="de545-138">If the application relies on access token issued by Azure AD, the logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="de545-139">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="de545-140">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-140">Example</span></span>
<span data-ttu-id="de545-141">Det bör också förstöra användarsession genom att anropa metoden Session.Abandon().</span><span class="sxs-lookup"><span data-stu-id="de545-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="de545-142">Följande metod visar säker implementering av användaren logga ut:</span><span class="sxs-lookup"><span data-stu-id="de545-142">Following method shows secure implementation of user logout:</span></span>
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <span data-ttu-id="de545-143"><a id="finite-tokens"></a>Använd begränsad livslängd för genererade SaS-token</span><span class="sxs-lookup"><span data-stu-id="de545-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="de545-144">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-144">Title</span></span>                   | <span data-ttu-id="de545-145">Information</span><span class="sxs-lookup"><span data-stu-id="de545-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-146">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-146">**Component**</span></span>               | <span data-ttu-id="de545-147">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="de545-147">IoT Device</span></span> | 
| <span data-ttu-id="de545-148">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-148">**SDL Phase**</span></span>               | <span data-ttu-id="de545-149">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-149">Build</span></span> |  
| <span data-ttu-id="de545-150">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-150">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-151">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-151">Generic</span></span> |
| <span data-ttu-id="de545-152">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-152">**Attributes**</span></span>              | <span data-ttu-id="de545-153">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-153">N/A</span></span>  |
| <span data-ttu-id="de545-154">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-154">**References**</span></span>              | <span data-ttu-id="de545-155">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-155">N/A</span></span>  |
| <span data-ttu-id="de545-156">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-156">**Steps**</span></span> | <span data-ttu-id="de545-157">SaS-token genereras för att autentisera till Azure IoT-hubben ska ha en begränsad upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="de545-157">SaS tokens generated for authenticating to Azure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="de545-158">Använd SaS-token livslängd som möjligt att begränsa den tid som de kan återupprepas om token komprometteras.</span><span class="sxs-lookup"><span data-stu-id="de545-158">Keep the SaS token lifetimes to a minimum to limit the amount of time they can be replayed in case the tokens are compromised.</span></span>|

## <span data-ttu-id="de545-159"><a id="resource-tokens"></a>Använda minsta livstid för token för den genererade resurs token</span><span class="sxs-lookup"><span data-stu-id="de545-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="de545-160">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-160">Title</span></span>                   | <span data-ttu-id="de545-161">Information</span><span class="sxs-lookup"><span data-stu-id="de545-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-162">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-162">**Component**</span></span>               | <span data-ttu-id="de545-163">Azure dokumentet DB</span><span class="sxs-lookup"><span data-stu-id="de545-163">Azure Document DB</span></span> | 
| <span data-ttu-id="de545-164">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-164">**SDL Phase**</span></span>               | <span data-ttu-id="de545-165">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-165">Build</span></span> |  
| <span data-ttu-id="de545-166">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-166">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-167">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-167">Generic</span></span> |
| <span data-ttu-id="de545-168">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-168">**Attributes**</span></span>              | <span data-ttu-id="de545-169">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-169">N/A</span></span>  |
| <span data-ttu-id="de545-170">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-170">**References**</span></span>              | <span data-ttu-id="de545-171">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-171">N/A</span></span>  |
| <span data-ttu-id="de545-172">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-172">**Steps**</span></span> | <span data-ttu-id="de545-173">Minska tidsintervallet för resurs-token till ett minsta värde som krävs.</span><span class="sxs-lookup"><span data-stu-id="de545-173">Reduce the timespan of resource token to a minimum value required.</span></span> <span data-ttu-id="de545-174">Resurs-token har ett giltigt timespan standardvärdet 1 timme.</span><span class="sxs-lookup"><span data-stu-id="de545-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="de545-175"><a id="wsfederation-logout"></a>Implementera rätt logga ut med WsFederation metoder när du använder AD FS</span><span class="sxs-lookup"><span data-stu-id="de545-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="de545-176">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-176">Title</span></span>                   | <span data-ttu-id="de545-177">Information</span><span class="sxs-lookup"><span data-stu-id="de545-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-178">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-178">**Component**</span></span>               | <span data-ttu-id="de545-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="de545-179">ADFS</span></span> | 
| <span data-ttu-id="de545-180">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-180">**SDL Phase**</span></span>               | <span data-ttu-id="de545-181">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-181">Build</span></span> |  
| <span data-ttu-id="de545-182">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-182">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-183">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-183">Generic</span></span> |
| <span data-ttu-id="de545-184">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-184">**Attributes**</span></span>              | <span data-ttu-id="de545-185">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-185">N/A</span></span>  |
| <span data-ttu-id="de545-186">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-186">**References**</span></span>              | <span data-ttu-id="de545-187">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-187">N/A</span></span>  |
| <span data-ttu-id="de545-188">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-188">**Steps**</span></span> | <span data-ttu-id="de545-189">Om programmet är beroende av STS-token som utfärdas av AD FS, anropa logga ut händelsehanteraren WSFederationAuthenticationModule.FederatedSignOut() metod för att logga ut användaren.</span><span class="sxs-lookup"><span data-stu-id="de545-189">If the application relies on STS token issued by ADFS, the logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method to log out the user.</span></span> <span data-ttu-id="de545-190">Även den aktuella sessionen förstöras och token-värde för session ska återställa och undanröjda.</span><span class="sxs-lookup"><span data-stu-id="de545-190">Also the current session should be destroyed, and the session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="de545-191">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="de545-192"><a id="proper-logout"></a>Implementera rätt logga ut när du använder Identity Server</span><span class="sxs-lookup"><span data-stu-id="de545-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="de545-193">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-193">Title</span></span>                   | <span data-ttu-id="de545-194">Information</span><span class="sxs-lookup"><span data-stu-id="de545-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-195">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-195">**Component**</span></span>               | <span data-ttu-id="de545-196">Identity Server</span><span class="sxs-lookup"><span data-stu-id="de545-196">Identity Server</span></span> | 
| <span data-ttu-id="de545-197">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-197">**SDL Phase**</span></span>               | <span data-ttu-id="de545-198">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-198">Build</span></span> |  
| <span data-ttu-id="de545-199">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-199">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-200">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-200">Generic</span></span> |
| <span data-ttu-id="de545-201">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-201">**Attributes**</span></span>              | <span data-ttu-id="de545-202">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-202">N/A</span></span>  |
| <span data-ttu-id="de545-203">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-203">**References**</span></span>              | [<span data-ttu-id="de545-204">Federerad IdentityServer3 logga ut</span><span class="sxs-lookup"><span data-stu-id="de545-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="de545-205">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-205">**Steps**</span></span> | <span data-ttu-id="de545-206">IdentityServer stöder möjligheten att federera med externa identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="de545-206">IdentityServer supports the ability to federate with external identity providers.</span></span> <span data-ttu-id="de545-207">När en användare loggar ut från en överordnad identitetsleverantör, kan beroende på det protokoll som används, det vara möjligt att få ett meddelande när användaren loggar ut. Det gör IdentityServer att meddela sina klienter så att de kan också logga ut användaren. I referensavsnittet för implementeringen finns i dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="de545-207">When a user signs out of an upstream identity provider, depending upon the protocol used, it might be possible to receive a notification when the user signs out. It allows IdentityServer to notify its clients so they can also sign the user out. Check the documentation in the references section for the implementation details.</span></span>|

## <span data-ttu-id="de545-208"><a id="https-secure-cookies"></a>Program som är tillgängliga via HTTPS måste använda säkra cookies</span><span class="sxs-lookup"><span data-stu-id="de545-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="de545-209">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-209">Title</span></span>                   | <span data-ttu-id="de545-210">Information</span><span class="sxs-lookup"><span data-stu-id="de545-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-211">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-211">**Component**</span></span>               | <span data-ttu-id="de545-212">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-212">Web Application</span></span> | 
| <span data-ttu-id="de545-213">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-213">**SDL Phase**</span></span>               | <span data-ttu-id="de545-214">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-214">Build</span></span> |  
| <span data-ttu-id="de545-215">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-215">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-216">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-216">Generic</span></span> |
| <span data-ttu-id="de545-217">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-217">**Attributes**</span></span>              | <span data-ttu-id="de545-218">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="de545-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="de545-219">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-219">**References**</span></span>              | <span data-ttu-id="de545-220">[httpCookies Element (inställningsschema för ASP.NET)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure egenskapen](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="de545-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="de545-221">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-221">**Steps**</span></span> | <span data-ttu-id="de545-222">Cookies är endast tillgänglig för domänen som de har begränsats normalt.</span><span class="sxs-lookup"><span data-stu-id="de545-222">Cookies are normally only accessible to the domain for which they were scoped.</span></span> <span data-ttu-id="de545-223">Definitionen för ”domain” inkluderar tyvärr inte protokollet så att cookies som skapas via HTTPS är tillgänglig via HTTP.</span><span class="sxs-lookup"><span data-stu-id="de545-223">Unfortunately, the definition of "domain" does not include the protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="de545-224">Attributet ”secure” indikerar att webbläsaren som cookien ska endast göras tillgängliga via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="de545-224">The "secure" attribute indicates to the browser that the cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="de545-225">Kontrollera att alla cookies över HTTPS används den **säker** attribut.</span><span class="sxs-lookup"><span data-stu-id="de545-225">Ensure that all cookies set over HTTPS use the **secure** attribute.</span></span> <span data-ttu-id="de545-226">Kravet kan tillämpas i web.config-filen genom att attributet requireSSL till true.</span><span class="sxs-lookup"><span data-stu-id="de545-226">The requirement can be enforced in the web.config file by setting the requireSSL attribute to true.</span></span> <span data-ttu-id="de545-227">Det är att föredra eftersom den kommer att verkställa den **säker** attribut för alla aktuella och framtida cookies utan att behöva göra några ytterligare kodändringar.</span><span class="sxs-lookup"><span data-stu-id="de545-227">It is the preferred approach because it will enforce the **secure** attribute for all current and future cookies without the need to make any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="de545-228">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="de545-229">Inställningen tillämpas även om HTTP används för att få åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="de545-229">The setting is enforced even if HTTP is used to access the application.</span></span> <span data-ttu-id="de545-230">Om HTTP används för att få åtkomst till programmet, bryts inställningen för programmet, eftersom cookies anges med attributet secure och webbläsaren kommer inte att skicka dem till programmet.</span><span class="sxs-lookup"><span data-stu-id="de545-230">If HTTP is used to access the application, the setting breaks the application because the cookies are set with the secure attribute and the browser will not send them back to the application.</span></span>

| <span data-ttu-id="de545-231">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-231">Title</span></span>                   | <span data-ttu-id="de545-232">Information</span><span class="sxs-lookup"><span data-stu-id="de545-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-233">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-233">**Component**</span></span>               | <span data-ttu-id="de545-234">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-234">Web Application</span></span> | 
| <span data-ttu-id="de545-235">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-235">**SDL Phase**</span></span>               | <span data-ttu-id="de545-236">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-236">Build</span></span> |  
| <span data-ttu-id="de545-237">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-237">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-238">Webbformulär, MVC5</span><span class="sxs-lookup"><span data-stu-id="de545-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="de545-239">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-239">**Attributes**</span></span>              | <span data-ttu-id="de545-240">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="de545-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="de545-241">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-241">**References**</span></span>              | <span data-ttu-id="de545-242">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-242">N/A</span></span>  |
| <span data-ttu-id="de545-243">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-243">**Steps**</span></span> | <span data-ttu-id="de545-244">När webbprogrammet är den förlitande parten och IdP är AD FS-servern, secure token FedAuth-attributet kan konfigureras genom att ange requireSSL till True i `system.identityModel.services` avsnitt i web.config:</span><span class="sxs-lookup"><span data-stu-id="de545-244">When the web application is the Relying Party, and the IdP is ADFS server, the FedAuth token's secure attribute can be configured by setting requireSSL to True in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="de545-245">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="de545-246"><a id="cookie-definition"></a>Alla HTTP-baserade program bör ange http endast för cookie-definition</span><span class="sxs-lookup"><span data-stu-id="de545-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="de545-247">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-247">Title</span></span>                   | <span data-ttu-id="de545-248">Information</span><span class="sxs-lookup"><span data-stu-id="de545-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-249">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-249">**Component**</span></span>               | <span data-ttu-id="de545-250">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-250">Web Application</span></span> | 
| <span data-ttu-id="de545-251">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-251">**SDL Phase**</span></span>               | <span data-ttu-id="de545-252">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-252">Build</span></span> |  
| <span data-ttu-id="de545-253">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-253">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-254">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-254">Generic</span></span> |
| <span data-ttu-id="de545-255">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-255">**Attributes**</span></span>              | <span data-ttu-id="de545-256">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-256">N/A</span></span>  |
| <span data-ttu-id="de545-257">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-257">**References**</span></span>              | [<span data-ttu-id="de545-258">Säker Cookie-attribut</span><span class="sxs-lookup"><span data-stu-id="de545-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="de545-259">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-259">**Steps**</span></span> | <span data-ttu-id="de545-260">Om du vill minska risken för avslöjande av information med en attack med globala webbplatsskript (XSS), ett nytt attribut - httpOnly - introducerades så att cookies som stöds av alla större webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de545-260">To mitigate the risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced to cookies and is supported by all major browsers.</span></span> <span data-ttu-id="de545-261">Attributet anger en cookie inte är tillgänglig via skript.</span><span class="sxs-lookup"><span data-stu-id="de545-261">The attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="de545-262">Ett webbprogram minskar risken att känslig information i cookien stulen via skript och skickas till en angripares webbplats med hjälp av HttpOnly cookies.</span><span class="sxs-lookup"><span data-stu-id="de545-262">By using HttpOnly cookies, a web application reduces the possibility that sensitive information contained in the cookie can be stolen via script and sent to an attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="de545-263">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-263">Example</span></span>
<span data-ttu-id="de545-264">Alla HTTP-baserade program som använder cookies ska ange HttpOnly i cookie-definition genom att implementera följande konfiguration i web.config:</span><span class="sxs-lookup"><span data-stu-id="de545-264">All HTTP-based applications that use cookies should specify HttpOnly in the cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="de545-265">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-265">Title</span></span>                   | <span data-ttu-id="de545-266">Information</span><span class="sxs-lookup"><span data-stu-id="de545-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-267">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-267">**Component**</span></span>               | <span data-ttu-id="de545-268">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-268">Web Application</span></span> | 
| <span data-ttu-id="de545-269">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-269">**SDL Phase**</span></span>               | <span data-ttu-id="de545-270">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-270">Build</span></span> |  
| <span data-ttu-id="de545-271">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-271">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-272">Web Forms</span><span class="sxs-lookup"><span data-stu-id="de545-272">Web Forms</span></span> |
| <span data-ttu-id="de545-273">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-273">**Attributes**</span></span>              | <span data-ttu-id="de545-274">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-274">N/A</span></span>  |
| <span data-ttu-id="de545-275">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-275">**References**</span></span>              | [<span data-ttu-id="de545-276">Egenskapen FormsAuthentication.RequireSSL</span><span class="sxs-lookup"><span data-stu-id="de545-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="de545-277">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-277">**Steps**</span></span> | <span data-ttu-id="de545-278">Egenskapsvärdet RequireSSL anges i konfigurationsfilen för ett ASP.NET-program genom att använda attributet requireSSL för konfigurationselementet.</span><span class="sxs-lookup"><span data-stu-id="de545-278">The RequireSSL property value is set in the configuration file for an ASP.NET application by using the requireSSL attribute of the configuration element.</span></span> <span data-ttu-id="de545-279">Du kan ange i Web.config-filen för ASP.NET-program om SSL (Secure Sockets Layer) måste returnera cookien för formulär för autentisering till servern genom att ange requireSSL-attributet.</span><span class="sxs-lookup"><span data-stu-id="de545-279">You can specify in the Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required to return the forms-authentication cookie to the server by setting the requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="de545-280">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-280">Example</span></span> 
<span data-ttu-id="de545-281">Följande exempel anger attributet requireSSL i Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="de545-281">The following code example sets the requireSSL attribute in the Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="de545-282">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-282">Title</span></span>                   | <span data-ttu-id="de545-283">Information</span><span class="sxs-lookup"><span data-stu-id="de545-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-284">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-284">**Component**</span></span>               | <span data-ttu-id="de545-285">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-285">Web Application</span></span> | 
| <span data-ttu-id="de545-286">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-286">**SDL Phase**</span></span>               | <span data-ttu-id="de545-287">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-287">Build</span></span> |  
| <span data-ttu-id="de545-288">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-288">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="de545-289">MVC5</span></span> |
| <span data-ttu-id="de545-290">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-290">**Attributes**</span></span>              | <span data-ttu-id="de545-291">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="de545-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="de545-292">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-292">**References**</span></span>              | [<span data-ttu-id="de545-293">Windows Identity Foundation (WIF) konfiguration – del II</span><span class="sxs-lookup"><span data-stu-id="de545-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="de545-294">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-294">**Steps**</span></span> | <span data-ttu-id="de545-295">Om du vill ange httpOnly attribut för FedAuth cookies ska hideFromCsript attributvärdet anges till True.</span><span class="sxs-lookup"><span data-stu-id="de545-295">To set httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set to True.</span></span> |

### <a name="example"></a><span data-ttu-id="de545-296">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-296">Example</span></span>
<span data-ttu-id="de545-297">Följande konfiguration visar korrekt konfiguration:</span><span class="sxs-lookup"><span data-stu-id="de545-297">Following configuration shows the correct configuration:</span></span>
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <span data-ttu-id="de545-298"><a id="csrf-asp"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor</span><span class="sxs-lookup"><span data-stu-id="de545-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="de545-299">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-299">Title</span></span>                   | <span data-ttu-id="de545-300">Information</span><span class="sxs-lookup"><span data-stu-id="de545-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-301">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-301">**Component**</span></span>               | <span data-ttu-id="de545-302">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-302">Web Application</span></span> | 
| <span data-ttu-id="de545-303">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-303">**SDL Phase**</span></span>               | <span data-ttu-id="de545-304">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-304">Build</span></span> |  
| <span data-ttu-id="de545-305">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-305">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-306">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-306">Generic</span></span> |
| <span data-ttu-id="de545-307">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-307">**Attributes**</span></span>              | <span data-ttu-id="de545-308">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-308">N/A</span></span>  |
| <span data-ttu-id="de545-309">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-309">**References**</span></span>              | <span data-ttu-id="de545-310">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-310">N/A</span></span>  |
| <span data-ttu-id="de545-311">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-311">**Steps**</span></span> | <span data-ttu-id="de545-312">Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i säkerhetskontexten för en annan användare upprättad session på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="de545-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="de545-313">Målet är att ändra eller ta bort innehåll, om den aktuella webbplatsen använder uteslutande sessionscookies att autentisera tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-313">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="de545-314">En angripare kan utnyttja detta problem genom att få en annan användare webbläsaren att läsa in en URL med ett kommando från en sårbar plats där användaren är redan inloggad.</span><span class="sxs-lookup"><span data-stu-id="de545-314">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="de545-315">Det finns många sätt för en angripare att göra det, som värd för en annan webbplats som läser in en resurs från sårbara servern eller att användaren klickar på en länk.</span><span class="sxs-lookup"><span data-stu-id="de545-315">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="de545-316">Angrepp kan förhindras om servern skickar en ytterligare token till klienten kräver att klienten inkluderar den token i alla framtida förfrågningar och verifierar att alla kommande begäranden inkluderar en token som rör den aktuella sessionen, som med hjälp av ASP.NET AntiForgeryToken eller ViewState.</span><span class="sxs-lookup"><span data-stu-id="de545-316">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="de545-317">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-317">Title</span></span>                   | <span data-ttu-id="de545-318">Information</span><span class="sxs-lookup"><span data-stu-id="de545-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-319">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-319">**Component**</span></span>               | <span data-ttu-id="de545-320">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-320">Web Application</span></span> | 
| <span data-ttu-id="de545-321">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-321">**SDL Phase**</span></span>               | <span data-ttu-id="de545-322">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-322">Build</span></span> |  
| <span data-ttu-id="de545-323">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-323">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-324">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="de545-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="de545-325">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-325">**Attributes**</span></span>              | <span data-ttu-id="de545-326">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-326">N/A</span></span>  |
| <span data-ttu-id="de545-327">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-327">**References**</span></span>              | [<span data-ttu-id="de545-328">XSRF/CSRF förebyggande i ASP.NET MVC och webbsidor</span><span class="sxs-lookup"><span data-stu-id="de545-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="de545-329">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-329">**Steps**</span></span> | <span data-ttu-id="de545-330">Anti-CSRF och formulär för ASP.NET MVC - använder den `AntiForgeryToken` hjälpmetod på vyer; placera en `Html.AntiForgeryToken()` i formuläret, t.ex.</span><span class="sxs-lookup"><span data-stu-id="de545-330">Anti-CSRF and ASP.NET MVC forms - Use the `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into the form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="de545-331">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="de545-332">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="de545-333">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-333">Example</span></span>
<span data-ttu-id="de545-334">Samtidigt ger Html.AntiForgeryToken() användaren en cookie som kallas __RequestVerificationToken med samma värde som slumpmässiga dolda värdet som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="de545-334">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="de545-335">Lägg sedan för att verifiera en inkommande formuläret post till filtret [ValidateAntiForgeryToken] till målmetoden för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="de545-335">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="de545-336">Exempel:</span><span class="sxs-lookup"><span data-stu-id="de545-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="de545-337">Auktoriseringsfilter som kontrollerar att:</span><span class="sxs-lookup"><span data-stu-id="de545-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="de545-338">Inkommande begäran har en cookie som kallas __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="de545-338">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="de545-339">Inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="de545-339">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="de545-340">Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, begäran går igenom som vanligt.</span><span class="sxs-lookup"><span data-stu-id="de545-340">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="de545-341">Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”.</span><span class="sxs-lookup"><span data-stu-id="de545-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="de545-342">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-342">Example</span></span>
<span data-ttu-id="de545-343">Skydd mot CSRF och AJAX: formulär-token kan vara ett problem för AJAX-begäranden eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="de545-343">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="de545-344">En lösning är att skicka tokens i ett anpassat HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="de545-344">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="de545-345">Följande kod använder Razor-syntaxen för att generera token och lägger sedan till token i en AJAX-begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-345">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="de545-346">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-346">Example</span></span>
<span data-ttu-id="de545-347">När du bearbetar begäran extrahera token från begärandehuvudet.</span><span class="sxs-lookup"><span data-stu-id="de545-347">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="de545-348">Sedan anropa metoden AntiForgery.Validate för att validera token.</span><span class="sxs-lookup"><span data-stu-id="de545-348">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="de545-349">Validate-metoden genereras ett undantag om token inte är giltiga.</span><span class="sxs-lookup"><span data-stu-id="de545-349">The Validate method throws an exception if the tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| <span data-ttu-id="de545-350">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-350">Title</span></span>                   | <span data-ttu-id="de545-351">Information</span><span class="sxs-lookup"><span data-stu-id="de545-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-352">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-352">**Component**</span></span>               | <span data-ttu-id="de545-353">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-353">Web Application</span></span> | 
| <span data-ttu-id="de545-354">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-354">**SDL Phase**</span></span>               | <span data-ttu-id="de545-355">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-355">Build</span></span> |  
| <span data-ttu-id="de545-356">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-356">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-357">Web Forms</span><span class="sxs-lookup"><span data-stu-id="de545-357">Web Forms</span></span> |
| <span data-ttu-id="de545-358">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-358">**Attributes**</span></span>              | <span data-ttu-id="de545-359">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-359">N/A</span></span>  |
| <span data-ttu-id="de545-360">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-360">**References**</span></span>              | [<span data-ttu-id="de545-361">Dra nytta av ASP.NET inbyggda funktioner för att slippa webbattacker</span><span class="sxs-lookup"><span data-stu-id="de545-361">Take Advantage of ASP.NET Built-in Features to Fend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="de545-362">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-362">**Steps**</span></span> | <span data-ttu-id="de545-363">CSRF attacker i WebForm baserat program kan begränsas genom att ange ViewStateUserKey till en slumpmässig sträng som varierar för varje användare - användar-ID eller ännu bättre sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="de545-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey to a random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="de545-364">För ett antal tekniska och är sessions-ID en mycket bättre anpassning eftersom en session-ID är oförutsägbart, timeout och varierar per användare.</span><span class="sxs-lookup"><span data-stu-id="de545-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="de545-365">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-365">Example</span></span>
<span data-ttu-id="de545-366">Här är koden som du behöver i alla sidor:</span><span class="sxs-lookup"><span data-stu-id="de545-366">Here's the code you need to have in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="de545-367"><a id="inactivity-lifetime"></a>Ställ in sessionen för inaktivitet livslängd</span><span class="sxs-lookup"><span data-stu-id="de545-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="de545-368">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-368">Title</span></span>                   | <span data-ttu-id="de545-369">Information</span><span class="sxs-lookup"><span data-stu-id="de545-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-370">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-370">**Component**</span></span>               | <span data-ttu-id="de545-371">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-371">Web Application</span></span> | 
| <span data-ttu-id="de545-372">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-372">**SDL Phase**</span></span>               | <span data-ttu-id="de545-373">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-373">Build</span></span> |  
| <span data-ttu-id="de545-374">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-374">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-375">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-375">Generic</span></span> |
| <span data-ttu-id="de545-376">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-376">**Attributes**</span></span>              | <span data-ttu-id="de545-377">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-377">N/A</span></span>  |
| <span data-ttu-id="de545-378">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-378">**References**</span></span>              | <span data-ttu-id="de545-379">[Egenskapen HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="de545-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="de545-380">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-380">**Steps**</span></span> | <span data-ttu-id="de545-381">Tidsgräns för session representerar händelsen inträffar när en användare inte utför någon åtgärd på en webbplats under ett intervall (definieras av webbserver).</span><span class="sxs-lookup"><span data-stu-id="de545-381">Session timeout represents the event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="de545-382">En händelse på serversidan, ändra status för användarsessionen till 'ogiltigt' (till exempel ”används inte längre”) och att webbservern att ta bort den (ta bort alla data som finns i den).</span><span class="sxs-lookup"><span data-stu-id="de545-382">The event, on server side, change the status of the user session to 'invalid' (for example  "not used anymore") and instruct the web server to destroy it (deleting all data contained into it).</span></span> <span data-ttu-id="de545-383">Följande exempel anger attributet timeout-session till 15 minuter i Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="de545-383">The following code example sets the timeout session attribute to 15 minutes in the Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="de545-384">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-384">Example</span></span>
<span data-ttu-id="de545-385">'''XML-koden <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="de545-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="de545-386">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-386">Title</span></span>                   | <span data-ttu-id="de545-387">Information</span><span class="sxs-lookup"><span data-stu-id="de545-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-388">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-388">**Component**</span></span>               | <span data-ttu-id="de545-389">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-389">Web Application</span></span> | 
| <span data-ttu-id="de545-390">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-390">**SDL Phase**</span></span>               | <span data-ttu-id="de545-391">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-391">Build</span></span> |  
| <span data-ttu-id="de545-392">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-392">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-393">Web Forms</span><span class="sxs-lookup"><span data-stu-id="de545-393">Web Forms</span></span> |
| <span data-ttu-id="de545-394">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-394">**Attributes**</span></span>              | <span data-ttu-id="de545-395">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-395">N/A</span></span>  |
| <span data-ttu-id="de545-396">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-396">**References**</span></span>              | <span data-ttu-id="de545-397">[Element för formulär för autentisering (inställningsschema för ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="de545-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="de545-398">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-398">**Steps**</span></span> | <span data-ttu-id="de545-399">Ange tidsgränsen för cookie biljetten för formulär för autentisering till 15 minuter</span><span class="sxs-lookup"><span data-stu-id="de545-399">Set the Forms Authentication Ticket cookie timeout to 15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="de545-400">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-400">Example</span></span>
<span data-ttu-id="de545-401">''' XML-kod<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="de545-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When the web application is Relying Party and ADFS is the STS, the lifetime of the authentication cookies - FedAuth tokens - can be set by the following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="de545-402">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-402">Example</span></span>
<span data-ttu-id="de545-403">Också begära ADFS utfärdat SAML-token livstid ska anges till 15 minuter, genom att köra följande powershell-kommando på AD FS-servern:</span><span class="sxs-lookup"><span data-stu-id="de545-403">Also the ADFS issued SAML claim token's lifetime should be set to 15 minutes, by executing the following powershell command on the ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="de545-404"><a id="proper-app-logout"></a>Implementera rätt logga ut från programmet</span><span class="sxs-lookup"><span data-stu-id="de545-404"><a id="proper-app-logout"></a>Implement proper logout from the application</span></span>

| <span data-ttu-id="de545-405">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-405">Title</span></span>                   | <span data-ttu-id="de545-406">Information</span><span class="sxs-lookup"><span data-stu-id="de545-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-407">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-407">**Component**</span></span>               | <span data-ttu-id="de545-408">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="de545-408">Web Application</span></span> | 
| <span data-ttu-id="de545-409">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-409">**SDL Phase**</span></span>               | <span data-ttu-id="de545-410">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-410">Build</span></span> |  
| <span data-ttu-id="de545-411">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-411">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-412">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-412">Generic</span></span> |
| <span data-ttu-id="de545-413">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-413">**Attributes**</span></span>              | <span data-ttu-id="de545-414">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-414">N/A</span></span>  |
| <span data-ttu-id="de545-415">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-415">**References**</span></span>              | <span data-ttu-id="de545-416">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-416">N/A</span></span>  |
| <span data-ttu-id="de545-417">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-417">**Steps**</span></span> | <span data-ttu-id="de545-418">Utföra rätt logga ut från programmet, när användaren trycker på Logga ut knappen.</span><span class="sxs-lookup"><span data-stu-id="de545-418">Perform proper Sign Out from the application, when user presses log out button.</span></span> <span data-ttu-id="de545-419">På Logga ut, bör program förstör användarens session, och även återställa och upphäver session cookie-värde, tillsammans med att återställa och också ångras autentisering cookie-värde.</span><span class="sxs-lookup"><span data-stu-id="de545-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="de545-420">Dessutom när flera sessioner är knutna till en enskild användaridentitet, måste de gemensamt avslutas på serversidan på timeout eller logga ut.</span><span class="sxs-lookup"><span data-stu-id="de545-420">Also, when multiple sessions are tied to a single user identity, they must be collectively terminated on the server side at timeout or logout.</span></span> <span data-ttu-id="de545-421">Kontrollera slutligen att logga ut funktionalitet är tillgänglig på varje sida.</span><span class="sxs-lookup"><span data-stu-id="de545-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="de545-422"><a id="csrf-api"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er</span><span class="sxs-lookup"><span data-stu-id="de545-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="de545-423">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-423">Title</span></span>                   | <span data-ttu-id="de545-424">Information</span><span class="sxs-lookup"><span data-stu-id="de545-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-425">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-425">**Component**</span></span>               | <span data-ttu-id="de545-426">Webb-API</span><span class="sxs-lookup"><span data-stu-id="de545-426">Web API</span></span> | 
| <span data-ttu-id="de545-427">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-427">**SDL Phase**</span></span>               | <span data-ttu-id="de545-428">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-428">Build</span></span> |  
| <span data-ttu-id="de545-429">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-429">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-430">Generisk</span><span class="sxs-lookup"><span data-stu-id="de545-430">Generic</span></span> |
| <span data-ttu-id="de545-431">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-431">**Attributes**</span></span>              | <span data-ttu-id="de545-432">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-432">N/A</span></span>  |
| <span data-ttu-id="de545-433">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-433">**References**</span></span>              | <span data-ttu-id="de545-434">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-434">N/A</span></span>  |
| <span data-ttu-id="de545-435">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-435">**Steps**</span></span> | <span data-ttu-id="de545-436">Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i säkerhetskontexten för en annan användare upprättad session på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="de545-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="de545-437">Målet är att ändra eller ta bort innehåll, om den aktuella webbplatsen använder uteslutande sessionscookies att autentisera tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-437">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="de545-438">En angripare kan utnyttja detta problem genom att få en annan användare webbläsaren att läsa in en URL med ett kommando från en sårbar plats där användaren är redan inloggad.</span><span class="sxs-lookup"><span data-stu-id="de545-438">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="de545-439">Det finns många sätt för en angripare att göra det, som värd för en annan webbplats som läser in en resurs från sårbara servern eller att användaren klickar på en länk.</span><span class="sxs-lookup"><span data-stu-id="de545-439">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="de545-440">Angrepp kan förhindras om servern skickar en ytterligare token till klienten kräver att klienten inkluderar den token i alla framtida förfrågningar och verifierar att alla kommande begäranden inkluderar en token som rör den aktuella sessionen, som med hjälp av ASP.NET AntiForgeryToken eller ViewState.</span><span class="sxs-lookup"><span data-stu-id="de545-440">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="de545-441">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-441">Title</span></span>                   | <span data-ttu-id="de545-442">Information</span><span class="sxs-lookup"><span data-stu-id="de545-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-443">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-443">**Component**</span></span>               | <span data-ttu-id="de545-444">Webb-API</span><span class="sxs-lookup"><span data-stu-id="de545-444">Web API</span></span> | 
| <span data-ttu-id="de545-445">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-445">**SDL Phase**</span></span>               | <span data-ttu-id="de545-446">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-446">Build</span></span> |  
| <span data-ttu-id="de545-447">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-447">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-448">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="de545-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="de545-449">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-449">**Attributes**</span></span>              | <span data-ttu-id="de545-450">Saknas</span><span class="sxs-lookup"><span data-stu-id="de545-450">N/A</span></span>  |
| <span data-ttu-id="de545-451">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-451">**References**</span></span>              | [<span data-ttu-id="de545-452">Förhindra attacker med förfalskning (CSRF) av begäran i ASP.NET webb-API</span><span class="sxs-lookup"><span data-stu-id="de545-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="de545-453">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-453">**Steps**</span></span> | <span data-ttu-id="de545-454">Skydd mot CSRF och AJAX: formulär-token kan vara ett problem för AJAX-begäranden eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="de545-454">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="de545-455">En lösning är att skicka tokens i ett anpassat HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="de545-455">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="de545-456">Följande kod använder Razor-syntaxen för att generera token och lägger sedan till token i en AJAX-begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-456">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="de545-457">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-457">Example</span></span>
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="de545-458">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-458">Example</span></span>
<span data-ttu-id="de545-459">När du bearbetar begäran extrahera token från begärandehuvudet.</span><span class="sxs-lookup"><span data-stu-id="de545-459">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="de545-460">Sedan anropa metoden AntiForgery.Validate för att validera token.</span><span class="sxs-lookup"><span data-stu-id="de545-460">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="de545-461">Validate-metoden genereras ett undantag om token inte är giltiga.</span><span class="sxs-lookup"><span data-stu-id="de545-461">The Validate method throws an exception if the tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a><span data-ttu-id="de545-462">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-462">Example</span></span>
<span data-ttu-id="de545-463">Skydd mot CSRF och formulär för ASP.NET MVC - Hjälpmetoden AntiForgeryToken på vyer. Placera en Html.AntiForgeryToken() i formuläret, t.ex.</span><span class="sxs-lookup"><span data-stu-id="de545-463">Anti-CSRF and ASP.NET MVC forms - Use the AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into the form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="de545-464">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-464">Example</span></span>
<span data-ttu-id="de545-465">Exemplet ovan kommer att skrivas ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="de545-465">The example above will output something like the following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="de545-466">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-466">Example</span></span>
<span data-ttu-id="de545-467">Samtidigt ger Html.AntiForgeryToken() användaren en cookie som kallas __RequestVerificationToken med samma värde som slumpmässiga dolda värdet som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="de545-467">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="de545-468">Lägg sedan för att verifiera en inkommande formuläret post till filtret [ValidateAntiForgeryToken] till målmetoden för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="de545-468">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="de545-469">Exempel:</span><span class="sxs-lookup"><span data-stu-id="de545-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="de545-470">Auktoriseringsfilter som kontrollerar att:</span><span class="sxs-lookup"><span data-stu-id="de545-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="de545-471">Inkommande begäran har en cookie som kallas __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="de545-471">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="de545-472">Inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="de545-472">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="de545-473">Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, begäran går igenom som vanligt.</span><span class="sxs-lookup"><span data-stu-id="de545-473">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="de545-474">Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”.</span><span class="sxs-lookup"><span data-stu-id="de545-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="de545-475">Rubrik</span><span class="sxs-lookup"><span data-stu-id="de545-475">Title</span></span>                   | <span data-ttu-id="de545-476">Information</span><span class="sxs-lookup"><span data-stu-id="de545-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="de545-477">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="de545-477">**Component**</span></span>               | <span data-ttu-id="de545-478">Webb-API</span><span class="sxs-lookup"><span data-stu-id="de545-478">Web API</span></span> | 
| <span data-ttu-id="de545-479">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="de545-479">**SDL Phase**</span></span>               | <span data-ttu-id="de545-480">Utveckla</span><span class="sxs-lookup"><span data-stu-id="de545-480">Build</span></span> |  
| <span data-ttu-id="de545-481">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="de545-481">**Applicable Technologies**</span></span> | <span data-ttu-id="de545-482">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="de545-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="de545-483">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="de545-483">**Attributes**</span></span>              | <span data-ttu-id="de545-484">Identitet Provider - ADFS, identitetsleverantör - Azure AD</span><span class="sxs-lookup"><span data-stu-id="de545-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="de545-485">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="de545-485">**References**</span></span>              | [<span data-ttu-id="de545-486">Skydda ett webb-API med enskilda konton och lokala inloggning i ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="de545-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="de545-487">**Steg**</span><span class="sxs-lookup"><span data-stu-id="de545-487">**Steps**</span></span> | <span data-ttu-id="de545-488">Om webb-API kan skyddas med OAuth 2.0 sedan förväntar sig ett ägartoken i Authorization-huvud för begäran och ger åtkomst till begäran om token är giltig.</span><span class="sxs-lookup"><span data-stu-id="de545-488">If the Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access to the request only if the token is valid.</span></span> <span data-ttu-id="de545-489">Till skillnad från cookie-baserad autentisering Anslut webbläsare inte ägar-token på begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-489">Unlike cookie based authentication, browsers do not attach the bearer tokens to requests.</span></span> <span data-ttu-id="de545-490">Den begärande klienten måste koppla explicit ägartoken i huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="de545-490">The requesting client needs to explicitly attach the bearer token in the request header.</span></span> <span data-ttu-id="de545-491">ASP.NET Web API: er skyddas med hjälp av OAuth 2.0, betraktas därför ägar-token som skydd mot attacker CSRF.</span><span class="sxs-lookup"><span data-stu-id="de545-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="de545-492">Observera att om den MVC-delen av programmet använder formulärautentisering (d.v.s. använder cookies), skydd mot förfalskning token måste användas av MVC-webbapp.</span><span class="sxs-lookup"><span data-stu-id="de545-492">Please note that if the MVC portion of the application uses forms authentication (i.e., uses cookies), anti-forgery tokens have to be used by the MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="de545-493">Exempel</span><span class="sxs-lookup"><span data-stu-id="de545-493">Example</span></span>
<span data-ttu-id="de545-494">Webb-API har informeras lita enbart på ägar-token och inte på cookies.</span><span class="sxs-lookup"><span data-stu-id="de545-494">The Web API has to be informed to rely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="de545-495">Det kan göras med följande konfiguration i `WebApiConfig.Register` metod: '''C skarpa kod config. SuppressDefaultHostAuthentication(); Config. Filters.Add (nya HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="de545-495">It can be done by the following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
The SuppressDefaultHostAuthentication method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API to authenticate only using bearer tokens.