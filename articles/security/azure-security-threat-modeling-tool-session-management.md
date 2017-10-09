---
title: aaaSession Management - hotet Modeling verktyget - Azure | Microsoft Docs
description: "ändringar för hot som exponeras i hello hot Modeling verktyget"
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
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="068e3-103">Säkerhet ram: Sessionshantering | Artiklar</span><span class="sxs-lookup"><span data-stu-id="068e3-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="068e3-104">Produkter eller tjänster</span><span class="sxs-lookup"><span data-stu-id="068e3-104">Product/Service</span></span> | <span data-ttu-id="068e3-105">Artikel</span><span class="sxs-lookup"><span data-stu-id="068e3-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="068e3-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="068e3-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="068e3-107">Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD</span><span class="sxs-lookup"><span data-stu-id="068e3-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="068e3-108">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="068e3-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="068e3-109">Använd begränsad livslängd för genererade SaS-token</span><span class="sxs-lookup"><span data-stu-id="068e3-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="068e3-110">**Azure dokumentet DB**</span><span class="sxs-lookup"><span data-stu-id="068e3-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="068e3-111">Använda minsta livstid för token för den genererade resurs token</span><span class="sxs-lookup"><span data-stu-id="068e3-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="068e3-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="068e3-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="068e3-113">Implementera rätt logga ut med WsFederation metoder när du använder AD FS</span><span class="sxs-lookup"><span data-stu-id="068e3-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="068e3-114">**Identity Server**</span><span class="sxs-lookup"><span data-stu-id="068e3-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="068e3-115">Implementera rätt logga ut när du använder Identity Server</span><span class="sxs-lookup"><span data-stu-id="068e3-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="068e3-116">**Webbprogram**</span><span class="sxs-lookup"><span data-stu-id="068e3-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="068e3-117">Program som är tillgängliga via HTTPS måste använda säkra cookies</span><span class="sxs-lookup"><span data-stu-id="068e3-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="068e3-118">Alla HTTP-baserade program bör ange http endast för cookie-definition</span><span class="sxs-lookup"><span data-stu-id="068e3-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="068e3-119">Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor</span><span class="sxs-lookup"><span data-stu-id="068e3-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="068e3-120">Ställ in sessionen för inaktivitet livslängd</span><span class="sxs-lookup"><span data-stu-id="068e3-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="068e3-121">Implementera rätt logga ut från hello program</span><span class="sxs-lookup"><span data-stu-id="068e3-121">Implement proper logout from hello application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="068e3-122">**Webb-API**</span><span class="sxs-lookup"><span data-stu-id="068e3-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="068e3-123">Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er</span><span class="sxs-lookup"><span data-stu-id="068e3-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="068e3-124"><a id="logout-adal"></a>Implementera rätt logga ut med hjälp av ADAL metoder när du använder Azure AD</span><span class="sxs-lookup"><span data-stu-id="068e3-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="068e3-125">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-125">Title</span></span>                   | <span data-ttu-id="068e3-126">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-127">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-127">**Component**</span></span>               | <span data-ttu-id="068e3-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="068e3-128">Azure AD</span></span> | 
| <span data-ttu-id="068e3-129">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-129">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-130">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-130">Build</span></span> |  
| <span data-ttu-id="068e3-131">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-131">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-132">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-132">Generic</span></span> |
| <span data-ttu-id="068e3-133">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-133">**Attributes**</span></span>              | <span data-ttu-id="068e3-134">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-134">N/A</span></span>  |
| <span data-ttu-id="068e3-135">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-135">**References**</span></span>              | <span data-ttu-id="068e3-136">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-136">N/A</span></span>  |
| <span data-ttu-id="068e3-137">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-137">**Steps**</span></span> | <span data-ttu-id="068e3-138">Om programmet hello beroende åtkomst-token som utfärdas av Azure AD, anropa händelsehanteraren för hello logga ut</span><span class="sxs-lookup"><span data-stu-id="068e3-138">If hello application relies on access token issued by Azure AD, hello logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="068e3-139">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="068e3-140">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-140">Example</span></span>
<span data-ttu-id="068e3-141">Det bör också förstöra användarsession genom att anropa metoden Session.Abandon().</span><span class="sxs-lookup"><span data-stu-id="068e3-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="068e3-142">Följande metod visar säker implementering av användaren logga ut:</span><span class="sxs-lookup"><span data-stu-id="068e3-142">Following method shows secure implementation of user logout:</span></span>
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

## <span data-ttu-id="068e3-143"><a id="finite-tokens"></a>Använd begränsad livslängd för genererade SaS-token</span><span class="sxs-lookup"><span data-stu-id="068e3-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="068e3-144">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-144">Title</span></span>                   | <span data-ttu-id="068e3-145">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-146">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-146">**Component**</span></span>               | <span data-ttu-id="068e3-147">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="068e3-147">IoT Device</span></span> | 
| <span data-ttu-id="068e3-148">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-148">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-149">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-149">Build</span></span> |  
| <span data-ttu-id="068e3-150">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-150">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-151">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-151">Generic</span></span> |
| <span data-ttu-id="068e3-152">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-152">**Attributes**</span></span>              | <span data-ttu-id="068e3-153">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-153">N/A</span></span>  |
| <span data-ttu-id="068e3-154">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-154">**References**</span></span>              | <span data-ttu-id="068e3-155">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-155">N/A</span></span>  |
| <span data-ttu-id="068e3-156">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-156">**Steps**</span></span> | <span data-ttu-id="068e3-157">SaS-token genereras för att autentisera tooAzure IoT-hubben ska ha en begränsad upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="068e3-157">SaS tokens generated for authenticating tooAzure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="068e3-158">Behåll hello SaS token livslängd tooa minsta toolimit hello tidsperiod de spelas om hello token komprometteras.</span><span class="sxs-lookup"><span data-stu-id="068e3-158">Keep hello SaS token lifetimes tooa minimum toolimit hello amount of time they can be replayed in case hello tokens are compromised.</span></span>|

## <span data-ttu-id="068e3-159"><a id="resource-tokens"></a>Använda minsta livstid för token för den genererade resurs token</span><span class="sxs-lookup"><span data-stu-id="068e3-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="068e3-160">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-160">Title</span></span>                   | <span data-ttu-id="068e3-161">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-162">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-162">**Component**</span></span>               | <span data-ttu-id="068e3-163">Azure dokumentet DB</span><span class="sxs-lookup"><span data-stu-id="068e3-163">Azure Document DB</span></span> | 
| <span data-ttu-id="068e3-164">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-164">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-165">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-165">Build</span></span> |  
| <span data-ttu-id="068e3-166">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-166">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-167">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-167">Generic</span></span> |
| <span data-ttu-id="068e3-168">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-168">**Attributes**</span></span>              | <span data-ttu-id="068e3-169">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-169">N/A</span></span>  |
| <span data-ttu-id="068e3-170">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-170">**References**</span></span>              | <span data-ttu-id="068e3-171">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-171">N/A</span></span>  |
| <span data-ttu-id="068e3-172">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-172">**Steps**</span></span> | <span data-ttu-id="068e3-173">Minska hello timespan av resursen token tooa lägsta värde som krävs.</span><span class="sxs-lookup"><span data-stu-id="068e3-173">Reduce hello timespan of resource token tooa minimum value required.</span></span> <span data-ttu-id="068e3-174">Resurs-token har ett giltigt timespan standardvärdet 1 timme.</span><span class="sxs-lookup"><span data-stu-id="068e3-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="068e3-175"><a id="wsfederation-logout"></a>Implementera rätt logga ut med WsFederation metoder när du använder AD FS</span><span class="sxs-lookup"><span data-stu-id="068e3-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="068e3-176">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-176">Title</span></span>                   | <span data-ttu-id="068e3-177">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-178">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-178">**Component**</span></span>               | <span data-ttu-id="068e3-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="068e3-179">ADFS</span></span> | 
| <span data-ttu-id="068e3-180">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-180">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-181">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-181">Build</span></span> |  
| <span data-ttu-id="068e3-182">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-182">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-183">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-183">Generic</span></span> |
| <span data-ttu-id="068e3-184">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-184">**Attributes**</span></span>              | <span data-ttu-id="068e3-185">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-185">N/A</span></span>  |
| <span data-ttu-id="068e3-186">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-186">**References**</span></span>              | <span data-ttu-id="068e3-187">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-187">N/A</span></span>  |
| <span data-ttu-id="068e3-188">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-188">**Steps**</span></span> | <span data-ttu-id="068e3-189">Om hello programmet är beroende av STS-token som utfärdas av AD FS, anropa hello logga ut händelsehanteraren WSFederationAuthenticationModule.FederatedSignOut() metoden toolog ut hello användare.</span><span class="sxs-lookup"><span data-stu-id="068e3-189">If hello application relies on STS token issued by ADFS, hello logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method toolog out hello user.</span></span> <span data-ttu-id="068e3-190">Även hello aktuell session förstöras och hello session token-värde ska återställa och undanröjda.</span><span class="sxs-lookup"><span data-stu-id="068e3-190">Also hello current session should be destroyed, and hello session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-191">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="068e3-192"><a id="proper-logout"></a>Implementera rätt logga ut när du använder Identity Server</span><span class="sxs-lookup"><span data-stu-id="068e3-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="068e3-193">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-193">Title</span></span>                   | <span data-ttu-id="068e3-194">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-195">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-195">**Component**</span></span>               | <span data-ttu-id="068e3-196">Identity Server</span><span class="sxs-lookup"><span data-stu-id="068e3-196">Identity Server</span></span> | 
| <span data-ttu-id="068e3-197">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-197">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-198">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-198">Build</span></span> |  
| <span data-ttu-id="068e3-199">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-199">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-200">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-200">Generic</span></span> |
| <span data-ttu-id="068e3-201">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-201">**Attributes**</span></span>              | <span data-ttu-id="068e3-202">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-202">N/A</span></span>  |
| <span data-ttu-id="068e3-203">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-203">**References**</span></span>              | [<span data-ttu-id="068e3-204">Federerad IdentityServer3 logga ut</span><span class="sxs-lookup"><span data-stu-id="068e3-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="068e3-205">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-205">**Steps**</span></span> | <span data-ttu-id="068e3-206">IdentityServer stöder hello möjlighet toofederate med externa identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="068e3-206">IdentityServer supports hello ability toofederate with external identity providers.</span></span> <span data-ttu-id="068e3-207">När en användare loggar ut från en överordnad identitetsleverantör, kan beroende på hello-protokoll som används, det vara möjligt tooreceive ett meddelande när hello användaren loggar ut. Det gör att IdentityServer toonotify klienter så att de kan också logga hello användaren ut. Hello dokumentationen i hello referensavsnittet för hello implementeringsdetaljer.</span><span class="sxs-lookup"><span data-stu-id="068e3-207">When a user signs out of an upstream identity provider, depending upon hello protocol used, it might be possible tooreceive a notification when hello user signs out. It allows IdentityServer toonotify its clients so they can also sign hello user out. Check hello documentation in hello references section for hello implementation details.</span></span>|

## <span data-ttu-id="068e3-208"><a id="https-secure-cookies"></a>Program som är tillgängliga via HTTPS måste använda säkra cookies</span><span class="sxs-lookup"><span data-stu-id="068e3-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="068e3-209">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-209">Title</span></span>                   | <span data-ttu-id="068e3-210">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-211">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-211">**Component**</span></span>               | <span data-ttu-id="068e3-212">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-212">Web Application</span></span> | 
| <span data-ttu-id="068e3-213">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-213">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-214">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-214">Build</span></span> |  
| <span data-ttu-id="068e3-215">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-215">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-216">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-216">Generic</span></span> |
| <span data-ttu-id="068e3-217">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-217">**Attributes**</span></span>              | <span data-ttu-id="068e3-218">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="068e3-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="068e3-219">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-219">**References**</span></span>              | <span data-ttu-id="068e3-220">[httpCookies Element (inställningsschema för ASP.NET)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure egenskapen](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="068e3-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="068e3-221">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-221">**Steps**</span></span> | <span data-ttu-id="068e3-222">Cookies är normalt bara tillgänglig toohello domän som de har begränsats.</span><span class="sxs-lookup"><span data-stu-id="068e3-222">Cookies are normally only accessible toohello domain for which they were scoped.</span></span> <span data-ttu-id="068e3-223">Tyvärr inkluderas hello definitionen för ”domain” inte hello protokollet så att cookies som skapas via HTTPS är tillgänglig via HTTP.</span><span class="sxs-lookup"><span data-stu-id="068e3-223">Unfortunately, hello definition of "domain" does not include hello protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="068e3-224">Hej ”säker” attributet anger toohello webbläsare som hello cookie ska endast göras tillgängliga via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="068e3-224">hello "secure" attribute indicates toohello browser that hello cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="068e3-225">Kontrollera att alla cookies som via HTTPS använder hello **säker** attribut.</span><span class="sxs-lookup"><span data-stu-id="068e3-225">Ensure that all cookies set over HTTPS use hello **secure** attribute.</span></span> <span data-ttu-id="068e3-226">hello krav kan tillämpas i hello web.config-filen genom att ange hello requireSSL attributet tootrue.</span><span class="sxs-lookup"><span data-stu-id="068e3-226">hello requirement can be enforced in hello web.config file by setting hello requireSSL attribute tootrue.</span></span> <span data-ttu-id="068e3-227">Det är hello önskade metoden eftersom den kommer att verkställa hello **säker** attribut för alla aktuella och framtida cookies utan hello måste toomake ändringar ytterligare kod.</span><span class="sxs-lookup"><span data-stu-id="068e3-227">It is hello preferred approach because it will enforce hello **secure** attribute for all current and future cookies without hello need toomake any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-228">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="068e3-229">hello inställningen tillämpas även om HTTP används tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="068e3-229">hello setting is enforced even if HTTP is used tooaccess hello application.</span></span> <span data-ttu-id="068e3-230">Om du använder HTTP tooaccess Hej program, hello inställningen radbrytningar hello programmet eftersom hello cookies konfigureras med hello säker attribut och hello webbläsare inte kommer att skicka dem tillbaka toohello program.</span><span class="sxs-lookup"><span data-stu-id="068e3-230">If HTTP is used tooaccess hello application, hello setting breaks hello application because hello cookies are set with hello secure attribute and hello browser will not send them back toohello application.</span></span>

| <span data-ttu-id="068e3-231">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-231">Title</span></span>                   | <span data-ttu-id="068e3-232">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-233">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-233">**Component**</span></span>               | <span data-ttu-id="068e3-234">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-234">Web Application</span></span> | 
| <span data-ttu-id="068e3-235">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-235">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-236">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-236">Build</span></span> |  
| <span data-ttu-id="068e3-237">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-237">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-238">Webbformulär, MVC5</span><span class="sxs-lookup"><span data-stu-id="068e3-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="068e3-239">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-239">**Attributes**</span></span>              | <span data-ttu-id="068e3-240">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="068e3-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="068e3-241">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-241">**References**</span></span>              | <span data-ttu-id="068e3-242">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-242">N/A</span></span>  |
| <span data-ttu-id="068e3-243">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-243">**Steps**</span></span> | <span data-ttu-id="068e3-244">När hello webbprogrammet är hello förlitande part och hello IdP är AD FS-servern, secure hello FedAuth token-attributet kan konfigureras genom att ange requireSSL tooTrue i `system.identityModel.services` avsnitt i web.config:</span><span class="sxs-lookup"><span data-stu-id="068e3-244">When hello web application is hello Relying Party, and hello IdP is ADFS server, hello FedAuth token's secure attribute can be configured by setting requireSSL tooTrue in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-245">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="068e3-246"><a id="cookie-definition"></a>Alla HTTP-baserade program bör ange http endast för cookie-definition</span><span class="sxs-lookup"><span data-stu-id="068e3-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="068e3-247">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-247">Title</span></span>                   | <span data-ttu-id="068e3-248">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-249">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-249">**Component**</span></span>               | <span data-ttu-id="068e3-250">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-250">Web Application</span></span> | 
| <span data-ttu-id="068e3-251">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-251">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-252">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-252">Build</span></span> |  
| <span data-ttu-id="068e3-253">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-253">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-254">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-254">Generic</span></span> |
| <span data-ttu-id="068e3-255">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-255">**Attributes**</span></span>              | <span data-ttu-id="068e3-256">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-256">N/A</span></span>  |
| <span data-ttu-id="068e3-257">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-257">**References**</span></span>              | [<span data-ttu-id="068e3-258">Säker Cookie-attribut</span><span class="sxs-lookup"><span data-stu-id="068e3-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="068e3-259">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-259">**Steps**</span></span> | <span data-ttu-id="068e3-260">toomitigate hello risken för avslöjande av information med en attack med globala webbplatsskript (XSS), ett nytt attribut - httpOnly - var introducerades toocookies och stöds av alla större webbläsare.</span><span class="sxs-lookup"><span data-stu-id="068e3-260">toomitigate hello risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced toocookies and is supported by all major browsers.</span></span> <span data-ttu-id="068e3-261">hello attribut anger en cookie inte är tillgänglig via skript.</span><span class="sxs-lookup"><span data-stu-id="068e3-261">hello attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="068e3-262">Med hjälp av HttpOnly cookies, minskar ett webbprogram hello risk för att känslig information i hello cookie kan stulen via skript och skickas tooan angripare webbplats.</span><span class="sxs-lookup"><span data-stu-id="068e3-262">By using HttpOnly cookies, a web application reduces hello possibility that sensitive information contained in hello cookie can be stolen via script and sent tooan attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="068e3-263">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-263">Example</span></span>
<span data-ttu-id="068e3-264">Alla HTTP-baserade program som använder cookies ska ange HttpOnly i hello cookie definition genom att implementera följande konfiguration i web.config:</span><span class="sxs-lookup"><span data-stu-id="068e3-264">All HTTP-based applications that use cookies should specify HttpOnly in hello cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="068e3-265">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-265">Title</span></span>                   | <span data-ttu-id="068e3-266">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-267">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-267">**Component**</span></span>               | <span data-ttu-id="068e3-268">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-268">Web Application</span></span> | 
| <span data-ttu-id="068e3-269">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-269">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-270">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-270">Build</span></span> |  
| <span data-ttu-id="068e3-271">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-271">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-272">Web Forms</span><span class="sxs-lookup"><span data-stu-id="068e3-272">Web Forms</span></span> |
| <span data-ttu-id="068e3-273">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-273">**Attributes**</span></span>              | <span data-ttu-id="068e3-274">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-274">N/A</span></span>  |
| <span data-ttu-id="068e3-275">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-275">**References**</span></span>              | [<span data-ttu-id="068e3-276">Egenskapen FormsAuthentication.RequireSSL</span><span class="sxs-lookup"><span data-stu-id="068e3-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="068e3-277">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-277">**Steps**</span></span> | <span data-ttu-id="068e3-278">Hej RequireSSL egenskapsvärde anges i hello konfigurationsfilen för ett ASP.NET-program med hjälp av hello requireSSL attribut för hello konfigurationselementet.</span><span class="sxs-lookup"><span data-stu-id="068e3-278">hello RequireSSL property value is set in hello configuration file for an ASP.NET application by using hello requireSSL attribute of hello configuration element.</span></span> <span data-ttu-id="068e3-279">Du kan ange i hello Web.config-filen för ASP.NET-program om SSL (Secure Sockets Layer) är nödvändiga tooreturn hello formulärautentisering cookie toohello servern genom att inställningen hello requireSSL attribut.</span><span class="sxs-lookup"><span data-stu-id="068e3-279">You can specify in hello Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required tooreturn hello forms-authentication cookie toohello server by setting hello requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-280">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-280">Example</span></span> 
<span data-ttu-id="068e3-281">hello anger följande kodexempel hello requireSSL attribut i hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="068e3-281">hello following code example sets hello requireSSL attribute in hello Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="068e3-282">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-282">Title</span></span>                   | <span data-ttu-id="068e3-283">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-284">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-284">**Component**</span></span>               | <span data-ttu-id="068e3-285">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-285">Web Application</span></span> | 
| <span data-ttu-id="068e3-286">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-286">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-287">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-287">Build</span></span> |  
| <span data-ttu-id="068e3-288">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-288">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="068e3-289">MVC5</span></span> |
| <span data-ttu-id="068e3-290">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-290">**Attributes**</span></span>              | <span data-ttu-id="068e3-291">EnvironmentType - OnPrem</span><span class="sxs-lookup"><span data-stu-id="068e3-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="068e3-292">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-292">**References**</span></span>              | [<span data-ttu-id="068e3-293">Windows Identity Foundation (WIF) konfiguration – del II</span><span class="sxs-lookup"><span data-stu-id="068e3-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="068e3-294">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-294">**Steps**</span></span> | <span data-ttu-id="068e3-295">tooset httpOnly attributet för FedAuth cookies, hideFromCsript attributvärdet ska ställas in tooTrue.</span><span class="sxs-lookup"><span data-stu-id="068e3-295">tooset httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set tooTrue.</span></span> |

### <a name="example"></a><span data-ttu-id="068e3-296">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-296">Example</span></span>
<span data-ttu-id="068e3-297">Följande konfiguration visar hello korrekt konfiguration:</span><span class="sxs-lookup"><span data-stu-id="068e3-297">Following configuration shows hello correct configuration:</span></span>
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

## <span data-ttu-id="068e3-298"><a id="csrf-asp"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) för ASP.NET-webbsidor</span><span class="sxs-lookup"><span data-stu-id="068e3-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="068e3-299">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-299">Title</span></span>                   | <span data-ttu-id="068e3-300">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-301">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-301">**Component**</span></span>               | <span data-ttu-id="068e3-302">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-302">Web Application</span></span> | 
| <span data-ttu-id="068e3-303">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-303">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-304">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-304">Build</span></span> |  
| <span data-ttu-id="068e3-305">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-305">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-306">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-306">Generic</span></span> |
| <span data-ttu-id="068e3-307">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-307">**Attributes**</span></span>              | <span data-ttu-id="068e3-308">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-308">N/A</span></span>  |
| <span data-ttu-id="068e3-309">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-309">**References**</span></span>              | <span data-ttu-id="068e3-310">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-310">N/A</span></span>  |
| <span data-ttu-id="068e3-311">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-311">**Steps**</span></span> | <span data-ttu-id="068e3-312">Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i hello säkerhetskontext för en annan användare upprättad session på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="068e3-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="068e3-313">hello målet är toomodify eller ta bort innehåll, om hello riktade webbplats använder uteslutande session cookies tooauthenticate tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-313">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="068e3-314">En angripare kan utnyttja detta problem genom att hämta en annan användare webbläsare tooload en URL med ett kommando från en sårbar plats där hello användaren är redan inloggad.</span><span class="sxs-lookup"><span data-stu-id="068e3-314">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="068e3-315">Det finns många sätt för en angripare toodo att, exempelvis av en annan webbplats som läses in en resurs från hello sårbara server eller hämtning hello användaren tooclick en länk.</span><span class="sxs-lookup"><span data-stu-id="068e3-315">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="068e3-316">hello attack kan förhindras om hello servern skickar klienten en ytterligare token toohello, kräver hello klienten tooinclude den token i alla kommande begäranden och verifierar att alla kommande begäranden inkluderar en token som gäller toohello aktuella sessionen, exempelvis genom att använder hello ASP.NET AntiForgeryToken eller ViewState.</span><span class="sxs-lookup"><span data-stu-id="068e3-316">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="068e3-317">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-317">Title</span></span>                   | <span data-ttu-id="068e3-318">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-319">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-319">**Component**</span></span>               | <span data-ttu-id="068e3-320">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-320">Web Application</span></span> | 
| <span data-ttu-id="068e3-321">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-321">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-322">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-322">Build</span></span> |  
| <span data-ttu-id="068e3-323">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-323">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-324">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="068e3-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="068e3-325">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-325">**Attributes**</span></span>              | <span data-ttu-id="068e3-326">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-326">N/A</span></span>  |
| <span data-ttu-id="068e3-327">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-327">**References**</span></span>              | [<span data-ttu-id="068e3-328">XSRF/CSRF förebyggande i ASP.NET MVC och webbsidor</span><span class="sxs-lookup"><span data-stu-id="068e3-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="068e3-329">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-329">**Steps**</span></span> | <span data-ttu-id="068e3-330">Formulär för skydd mot CSRF och ASP.NET MVC - Använd hello `AntiForgeryToken` hjälpmetod på vyer; placera en `Html.AntiForgeryToken()` till hello format, t.ex.</span><span class="sxs-lookup"><span data-stu-id="068e3-330">Anti-CSRF and ASP.NET MVC forms - Use hello `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into hello form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-331">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="068e3-332">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="068e3-333">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-333">Example</span></span>
<span data-ttu-id="068e3-334">Vid hello samma tid, Html.AntiForgeryToken() ger hello besökare en cookie kallas __RequestVerificationToken med hello samma värde som hello slumpmässiga dolda som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="068e3-334">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="068e3-335">Sedan lägger till toovalidate en inkommande formuläret post hello [ValidateAntiForgeryToken] filter toohello mål åtgärdsmetod.</span><span class="sxs-lookup"><span data-stu-id="068e3-335">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="068e3-336">Exempel:</span><span class="sxs-lookup"><span data-stu-id="068e3-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="068e3-337">Auktoriseringsfilter som kontrollerar att:</span><span class="sxs-lookup"><span data-stu-id="068e3-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="068e3-338">hello inkommande begäran har en cookie som kallas __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="068e3-338">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="068e3-339">hello inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="068e3-339">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="068e3-340">Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, hello begäran går igenom som vanligt.</span><span class="sxs-lookup"><span data-stu-id="068e3-340">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="068e3-341">Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”.</span><span class="sxs-lookup"><span data-stu-id="068e3-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="068e3-342">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-342">Example</span></span>
<span data-ttu-id="068e3-343">Skydd mot CSRF och AJAX: hello formuläret token kan vara ett problem för AJAX-begäranden, eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="068e3-343">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="068e3-344">En lösning är toosend hello token i ett anpassat HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="068e3-344">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="068e3-345">hello följande kod använder Razor syntax toogenerate hello token och lägger sedan till hello token tooan AJAX-begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-345">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> 
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

### <a name="example"></a><span data-ttu-id="068e3-346">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-346">Example</span></span>
<span data-ttu-id="068e3-347">När du bearbetar hello begäran extrahera hello token från hello huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-347">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="068e3-348">Anropa sedan hello AntiForgery.Validate metoden toovalidate hello token.</span><span class="sxs-lookup"><span data-stu-id="068e3-348">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="068e3-349">hello Validate-metoden genereras ett undantag om hello token inte är giltiga.</span><span class="sxs-lookup"><span data-stu-id="068e3-349">hello Validate method throws an exception if hello tokens are not valid.</span></span>
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

| <span data-ttu-id="068e3-350">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-350">Title</span></span>                   | <span data-ttu-id="068e3-351">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-352">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-352">**Component**</span></span>               | <span data-ttu-id="068e3-353">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-353">Web Application</span></span> | 
| <span data-ttu-id="068e3-354">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-354">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-355">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-355">Build</span></span> |  
| <span data-ttu-id="068e3-356">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-356">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-357">Web Forms</span><span class="sxs-lookup"><span data-stu-id="068e3-357">Web Forms</span></span> |
| <span data-ttu-id="068e3-358">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-358">**Attributes**</span></span>              | <span data-ttu-id="068e3-359">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-359">N/A</span></span>  |
| <span data-ttu-id="068e3-360">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-360">**References**</span></span>              | [<span data-ttu-id="068e3-361">Dra nytta av ASP.NET inbyggda funktioner tooFend av Web-attacker</span><span class="sxs-lookup"><span data-stu-id="068e3-361">Take Advantage of ASP.NET Built-in Features tooFend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="068e3-362">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-362">**Steps**</span></span> | <span data-ttu-id="068e3-363">CSRF attacker i WebForm baserat program kan begränsas genom att ange ViewStateUserKey tooa slumpmässig sträng som varierar för varje användare - användar-ID eller bättre ännu, sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="068e3-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey tooa random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="068e3-364">För ett antal tekniska och är sessions-ID en mycket bättre anpassning eftersom en session-ID är oförutsägbart, timeout och varierar per användare.</span><span class="sxs-lookup"><span data-stu-id="068e3-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-365">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-365">Example</span></span>
<span data-ttu-id="068e3-366">Här är hello-kod som du behöver toohave i alla sidor:</span><span class="sxs-lookup"><span data-stu-id="068e3-366">Here's hello code you need toohave in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="068e3-367"><a id="inactivity-lifetime"></a>Ställ in sessionen för inaktivitet livslängd</span><span class="sxs-lookup"><span data-stu-id="068e3-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="068e3-368">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-368">Title</span></span>                   | <span data-ttu-id="068e3-369">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-370">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-370">**Component**</span></span>               | <span data-ttu-id="068e3-371">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-371">Web Application</span></span> | 
| <span data-ttu-id="068e3-372">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-372">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-373">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-373">Build</span></span> |  
| <span data-ttu-id="068e3-374">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-374">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-375">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-375">Generic</span></span> |
| <span data-ttu-id="068e3-376">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-376">**Attributes**</span></span>              | <span data-ttu-id="068e3-377">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-377">N/A</span></span>  |
| <span data-ttu-id="068e3-378">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-378">**References**</span></span>              | <span data-ttu-id="068e3-379">[Egenskapen HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="068e3-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="068e3-380">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-380">**Steps**</span></span> | <span data-ttu-id="068e3-381">Tidsgräns för session representerar hello händelse inträffar när en användare inte utför någon åtgärd på en webbplats under ett intervall (definieras av webbserver).</span><span class="sxs-lookup"><span data-stu-id="068e3-381">Session timeout represents hello event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="068e3-382">Hej händelse på serversidan, ändra hello status för hello användarens session too'invalid' (till exempel ”används inte längre”) och instruera hello web server toodestroy den (ta bort alla data som finns i den).</span><span class="sxs-lookup"><span data-stu-id="068e3-382">hello event, on server side, change hello status of hello user session too'invalid' (for example  "not used anymore") and instruct hello web server toodestroy it (deleting all data contained into it).</span></span> <span data-ttu-id="068e3-383">hello anger följande kodexempel hello tidsgränsen för sessionen attributet too15 minuter i hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="068e3-383">hello following code example sets hello timeout session attribute too15 minutes in hello Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-384">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-384">Example</span></span>
<span data-ttu-id="068e3-385">'''XML-koden <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="068e3-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="068e3-386">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-386">Title</span></span>                   | <span data-ttu-id="068e3-387">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-388">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-388">**Component**</span></span>               | <span data-ttu-id="068e3-389">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-389">Web Application</span></span> | 
| <span data-ttu-id="068e3-390">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-390">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-391">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-391">Build</span></span> |  
| <span data-ttu-id="068e3-392">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-392">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-393">Web Forms</span><span class="sxs-lookup"><span data-stu-id="068e3-393">Web Forms</span></span> |
| <span data-ttu-id="068e3-394">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-394">**Attributes**</span></span>              | <span data-ttu-id="068e3-395">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-395">N/A</span></span>  |
| <span data-ttu-id="068e3-396">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-396">**References**</span></span>              | <span data-ttu-id="068e3-397">[Element för formulär för autentisering (inställningsschema för ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="068e3-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="068e3-398">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-398">**Steps**</span></span> | <span data-ttu-id="068e3-399">Ange hello biljetten för formulär för autentisering cookie-timeout too15 minuter</span><span class="sxs-lookup"><span data-stu-id="068e3-399">Set hello Forms Authentication Ticket cookie timeout too15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="068e3-400">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-400">Example</span></span>
<span data-ttu-id="068e3-401">''' XML-kod<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="068e3-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="068e3-402">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-402">Example</span></span>
<span data-ttu-id="068e3-403">Även hello ADFS utfärdade token SAML-anspråk livstid ska anges too15 minuter, genom att köra följande powershell-kommando på ADFS-server för hello hello:</span><span class="sxs-lookup"><span data-stu-id="068e3-403">Also hello ADFS issued SAML claim token's lifetime should be set too15 minutes, by executing hello following powershell command on hello ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="068e3-404"><a id="proper-app-logout"></a>Implementera rätt logga ut från hello program</span><span class="sxs-lookup"><span data-stu-id="068e3-404"><a id="proper-app-logout"></a>Implement proper logout from hello application</span></span>

| <span data-ttu-id="068e3-405">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-405">Title</span></span>                   | <span data-ttu-id="068e3-406">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-407">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-407">**Component**</span></span>               | <span data-ttu-id="068e3-408">Webbprogram</span><span class="sxs-lookup"><span data-stu-id="068e3-408">Web Application</span></span> | 
| <span data-ttu-id="068e3-409">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-409">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-410">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-410">Build</span></span> |  
| <span data-ttu-id="068e3-411">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-411">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-412">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-412">Generic</span></span> |
| <span data-ttu-id="068e3-413">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-413">**Attributes**</span></span>              | <span data-ttu-id="068e3-414">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-414">N/A</span></span>  |
| <span data-ttu-id="068e3-415">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-415">**References**</span></span>              | <span data-ttu-id="068e3-416">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-416">N/A</span></span>  |
| <span data-ttu-id="068e3-417">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-417">**Steps**</span></span> | <span data-ttu-id="068e3-418">Utföra rätt logga ut från programmet hello, när användaren trycker på Logga ut knappen.</span><span class="sxs-lookup"><span data-stu-id="068e3-418">Perform proper Sign Out from hello application, when user presses log out button.</span></span> <span data-ttu-id="068e3-419">På Logga ut, bör program förstör användarens session, och även återställa och upphäver session cookie-värde, tillsammans med att återställa och också ångras autentisering cookie-värde.</span><span class="sxs-lookup"><span data-stu-id="068e3-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="068e3-420">Dessutom när flera sessioner är bundet tooa enanvändarläge identitet, måste de gemensamt avslutas på serversidan för hello på timeout eller logga ut.</span><span class="sxs-lookup"><span data-stu-id="068e3-420">Also, when multiple sessions are tied tooa single user identity, they must be collectively terminated on hello server side at timeout or logout.</span></span> <span data-ttu-id="068e3-421">Kontrollera slutligen att logga ut funktionalitet är tillgänglig på varje sida.</span><span class="sxs-lookup"><span data-stu-id="068e3-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="068e3-422"><a id="csrf-api"></a>Skyddar mot attacker webbplatser begära förfalskning (CSRF) på ASP.NET Web API: er</span><span class="sxs-lookup"><span data-stu-id="068e3-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="068e3-423">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-423">Title</span></span>                   | <span data-ttu-id="068e3-424">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-425">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-425">**Component**</span></span>               | <span data-ttu-id="068e3-426">Webb-API</span><span class="sxs-lookup"><span data-stu-id="068e3-426">Web API</span></span> | 
| <span data-ttu-id="068e3-427">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-427">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-428">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-428">Build</span></span> |  
| <span data-ttu-id="068e3-429">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-429">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-430">Generisk</span><span class="sxs-lookup"><span data-stu-id="068e3-430">Generic</span></span> |
| <span data-ttu-id="068e3-431">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-431">**Attributes**</span></span>              | <span data-ttu-id="068e3-432">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-432">N/A</span></span>  |
| <span data-ttu-id="068e3-433">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-433">**References**</span></span>              | <span data-ttu-id="068e3-434">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-434">N/A</span></span>  |
| <span data-ttu-id="068e3-435">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-435">**Steps**</span></span> | <span data-ttu-id="068e3-436">Begäran förfalskning (CSRF eller XSRF) är en typ av angrepp där angriparen kan utföra åtgärder i hello säkerhetskontext för en annan användare upprättad session på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="068e3-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="068e3-437">hello målet är toomodify eller ta bort innehåll, om hello riktade webbplats använder uteslutande session cookies tooauthenticate tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-437">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="068e3-438">En angripare kan utnyttja detta problem genom att hämta en annan användare webbläsare tooload en URL med ett kommando från en sårbar plats där hello användaren är redan inloggad.</span><span class="sxs-lookup"><span data-stu-id="068e3-438">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="068e3-439">Det finns många sätt för en angripare toodo att, exempelvis av en annan webbplats som läses in en resurs från hello sårbara server eller hämtning hello användaren tooclick en länk.</span><span class="sxs-lookup"><span data-stu-id="068e3-439">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="068e3-440">hello attack kan förhindras om hello servern skickar klienten en ytterligare token toohello, kräver hello klienten tooinclude den token i alla kommande begäranden och verifierar att alla kommande begäranden inkluderar en token som gäller toohello aktuella sessionen, exempelvis genom att använder hello ASP.NET AntiForgeryToken eller ViewState.</span><span class="sxs-lookup"><span data-stu-id="068e3-440">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="068e3-441">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-441">Title</span></span>                   | <span data-ttu-id="068e3-442">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-443">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-443">**Component**</span></span>               | <span data-ttu-id="068e3-444">Webb-API</span><span class="sxs-lookup"><span data-stu-id="068e3-444">Web API</span></span> | 
| <span data-ttu-id="068e3-445">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-445">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-446">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-446">Build</span></span> |  
| <span data-ttu-id="068e3-447">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-447">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-448">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="068e3-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="068e3-449">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-449">**Attributes**</span></span>              | <span data-ttu-id="068e3-450">Saknas</span><span class="sxs-lookup"><span data-stu-id="068e3-450">N/A</span></span>  |
| <span data-ttu-id="068e3-451">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-451">**References**</span></span>              | [<span data-ttu-id="068e3-452">Förhindra attacker med förfalskning (CSRF) av begäran i ASP.NET webb-API</span><span class="sxs-lookup"><span data-stu-id="068e3-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="068e3-453">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-453">**Steps**</span></span> | <span data-ttu-id="068e3-454">Skydd mot CSRF och AJAX: hello formuläret token kan vara ett problem för AJAX-begäranden, eftersom en AJAX-begäran kan skicka JSON-data, inte data i HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="068e3-454">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="068e3-455">En lösning är toosend hello token i ett anpassat HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="068e3-455">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="068e3-456">hello följande kod använder Razor syntax toogenerate hello token och lägger sedan till hello token tooan AJAX-begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-456">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="068e3-457">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-457">Example</span></span>
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

### <a name="example"></a><span data-ttu-id="068e3-458">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-458">Example</span></span>
<span data-ttu-id="068e3-459">När du bearbetar hello begäran extrahera hello token från hello huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-459">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="068e3-460">Anropa sedan hello AntiForgery.Validate metoden toovalidate hello token.</span><span class="sxs-lookup"><span data-stu-id="068e3-460">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="068e3-461">hello Validate-metoden genereras ett undantag om hello token inte är giltiga.</span><span class="sxs-lookup"><span data-stu-id="068e3-461">hello Validate method throws an exception if hello tokens are not valid.</span></span>
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

### <a name="example"></a><span data-ttu-id="068e3-462">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-462">Example</span></span>
<span data-ttu-id="068e3-463">Anti-CSRF och formulär för ASP.NET MVC - Använd hello AntiForgeryToken hjälpmetod på vyer. Placera en Html.AntiForgeryToken() i hello formulär, t.ex.</span><span class="sxs-lookup"><span data-stu-id="068e3-463">Anti-CSRF and ASP.NET MVC forms - Use hello AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into hello form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="068e3-464">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-464">Example</span></span>
<span data-ttu-id="068e3-465">hello-exemplet ovan kommer att skrivas ut ungefär så hello följande:</span><span class="sxs-lookup"><span data-stu-id="068e3-465">hello example above will output something like hello following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="068e3-466">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-466">Example</span></span>
<span data-ttu-id="068e3-467">Vid hello samma tid, Html.AntiForgeryToken() ger hello besökare en cookie kallas __RequestVerificationToken med hello samma värde som hello slumpmässiga dolda som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="068e3-467">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="068e3-468">Sedan lägger till toovalidate en inkommande formuläret post hello [ValidateAntiForgeryToken] filter toohello mål åtgärdsmetod.</span><span class="sxs-lookup"><span data-stu-id="068e3-468">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="068e3-469">Exempel:</span><span class="sxs-lookup"><span data-stu-id="068e3-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="068e3-470">Auktoriseringsfilter som kontrollerar att:</span><span class="sxs-lookup"><span data-stu-id="068e3-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="068e3-471">hello inkommande begäran har en cookie som kallas __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="068e3-471">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="068e3-472">hello inkommande begäran har en `Request.Form` post med namnet __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="068e3-472">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="068e3-473">Dessa cookies och `Request.Form` värden matchar förutsatt att alla är korrekt, hello begäran går igenom som vanligt.</span><span class="sxs-lookup"><span data-stu-id="068e3-473">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="068e3-474">Men om det inte finns sedan ett auktoriseringsfel med meddelandet ”en obligatorisk antiförfalskningstoken har inte angetts eller var ogiltig”.</span><span class="sxs-lookup"><span data-stu-id="068e3-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="068e3-475">Rubrik</span><span class="sxs-lookup"><span data-stu-id="068e3-475">Title</span></span>                   | <span data-ttu-id="068e3-476">Information</span><span class="sxs-lookup"><span data-stu-id="068e3-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="068e3-477">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="068e3-477">**Component**</span></span>               | <span data-ttu-id="068e3-478">Webb-API</span><span class="sxs-lookup"><span data-stu-id="068e3-478">Web API</span></span> | 
| <span data-ttu-id="068e3-479">**SDL fas**</span><span class="sxs-lookup"><span data-stu-id="068e3-479">**SDL Phase**</span></span>               | <span data-ttu-id="068e3-480">Utveckla</span><span class="sxs-lookup"><span data-stu-id="068e3-480">Build</span></span> |  
| <span data-ttu-id="068e3-481">**Tillämpliga tekniker**</span><span class="sxs-lookup"><span data-stu-id="068e3-481">**Applicable Technologies**</span></span> | <span data-ttu-id="068e3-482">MVC5 MVC6</span><span class="sxs-lookup"><span data-stu-id="068e3-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="068e3-483">**Attribut**</span><span class="sxs-lookup"><span data-stu-id="068e3-483">**Attributes**</span></span>              | <span data-ttu-id="068e3-484">Identitet Provider - ADFS, identitetsleverantör - Azure AD</span><span class="sxs-lookup"><span data-stu-id="068e3-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="068e3-485">**Referenser**</span><span class="sxs-lookup"><span data-stu-id="068e3-485">**References**</span></span>              | [<span data-ttu-id="068e3-486">Skydda ett webb-API med enskilda konton och lokala inloggning i ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="068e3-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="068e3-487">**Steg**</span><span class="sxs-lookup"><span data-stu-id="068e3-487">**Steps**</span></span> | <span data-ttu-id="068e3-488">Om hello Web API skyddas med hjälp av OAuth 2.0, då förväntar sig ett ägartoken i tillståndet begäran sidhuvud och beviljar åtkomst toohello begäran endast om hello token är giltig.</span><span class="sxs-lookup"><span data-stu-id="068e3-488">If hello Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access toohello request only if hello token is valid.</span></span> <span data-ttu-id="068e3-489">Till skillnad från cookie-baserad autentisering Anslut inte webbläsare hello ägar-token toorequests.</span><span class="sxs-lookup"><span data-stu-id="068e3-489">Unlike cookie based authentication, browsers do not attach hello bearer tokens toorequests.</span></span> <span data-ttu-id="068e3-490">hello begär klienten måste tooexplicitly bifoga hello ägar-token i hello huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="068e3-490">hello requesting client needs tooexplicitly attach hello bearer token in hello request header.</span></span> <span data-ttu-id="068e3-491">ASP.NET Web API: er skyddas med hjälp av OAuth 2.0, betraktas därför ägar-token som skydd mot attacker CSRF.</span><span class="sxs-lookup"><span data-stu-id="068e3-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="068e3-492">Observera att om hello MVC-delen av programmet hello använder formulärautentisering (d.v.s. använder cookies), skydd mot förfalskning token har toobe som används av hello MVC-webbapp.</span><span class="sxs-lookup"><span data-stu-id="068e3-492">Please note that if hello MVC portion of hello application uses forms authentication (i.e., uses cookies), anti-forgery tokens have toobe used by hello MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="068e3-493">Exempel</span><span class="sxs-lookup"><span data-stu-id="068e3-493">Example</span></span>
<span data-ttu-id="068e3-494">hello Web API har toobe informeras toorely bara på ägar-token och inte på cookies.</span><span class="sxs-lookup"><span data-stu-id="068e3-494">hello Web API has toobe informed toorely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="068e3-495">Det kan göras med följande konfiguration i hello `WebApiConfig.Register` metod: '''C skarpa kod config. SuppressDefaultHostAuthentication(); Config. Filters.Add (nya HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="068e3-495">It can be done by hello following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
