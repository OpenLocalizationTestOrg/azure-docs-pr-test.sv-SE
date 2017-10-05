---
title: "Ändringar av Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "En beskrivning av ändringar som görs i app model v2.0 förhandsversion protokoll."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a><span data-ttu-id="bc798-103">Viktiga uppdateringar till v2.0-autentiseringsprotokoll</span><span class="sxs-lookup"><span data-stu-id="bc798-103">Important Updates to the v2.0 Authentication Protocols</span></span>
<span data-ttu-id="bc798-104">Uppmärksamhet utvecklare!</span><span class="sxs-lookup"><span data-stu-id="bc798-104">Attention developers!</span></span> <span data-ttu-id="bc798-105">Under de kommande två veckorna kommer vi att göra några uppdateringar till vår v2.0-autentiseringsprotokoll som kan innebära att bryta ändringar för alla program som du har skrivit under våra förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="bc798-105">Over the next two weeks, we will be making a few updates to our v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="bc798-106">Som påverkar detta?</span><span class="sxs-lookup"><span data-stu-id="bc798-106">Who does this affect?</span></span>
<span data-ttu-id="bc798-107">Alla appar som har skrivits för att använda v2.0 konvergerat autentisering slutpunkt</span><span class="sxs-lookup"><span data-stu-id="bc798-107">Any apps that have been written to use the v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="bc798-108">Mer information om v2.0-slutpunkten kan hittas [här](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bc798-108">More information on the v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="bc798-109">Om du har skapat en app med v2.0-slutpunkten genom att skriva direkt till v2.0-protokollet bör med hjälp av vår OpenID Connect och OAuth web middlewares eller använda andra 3 part-bibliotek för att utföra autentisering, du förberedas att testa dina projekt och göra ändringar Om det behövs.</span><span class="sxs-lookup"><span data-stu-id="bc798-109">If you have built an app using the v2.0 endpoint by coding directly to the v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries to perform authentication, you should be prepared to test your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="bc798-110">Som detta påverkar inte?</span><span class="sxs-lookup"><span data-stu-id="bc798-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="bc798-111">Alla appar som har skrivits mot autentiseringsslutpunkten produktion Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc798-111">Any apps that have been written against the production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="bc798-112">Det här protokollet anges i bricka och ska inte ha några ändringar.</span><span class="sxs-lookup"><span data-stu-id="bc798-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="bc798-113">Dessutom om din app **endast** använder våra ADAL-biblioteket för att utföra autentisering, du behöver ändra något.</span><span class="sxs-lookup"><span data-stu-id="bc798-113">Furthermore, if your app **only** uses our ADAL library to perform authentication, you won\`t have to change anything.</span></span>  <span data-ttu-id="bc798-114">ADAL har skärmad appen från ändringarna.</span><span class="sxs-lookup"><span data-stu-id="bc798-114">ADAL has shielded your app from the changes.</span></span>  

## <a name="what-are-the-changes"></a><span data-ttu-id="bc798-115">Vilka är ändringarna?</span><span class="sxs-lookup"><span data-stu-id="bc798-115">What are the changes?</span></span>
### <a name="removing-the-x5t-value-from-jwt-headers"></a><span data-ttu-id="bc798-116">Ta bort värdet x5t från JWT rubriker</span><span class="sxs-lookup"><span data-stu-id="bc798-116">Removing the x5t value from JWT headers</span></span>
<span data-ttu-id="bc798-117">V2.0-slutpunkten använder JWT-token i stor utsträckning, som innehåller ett huvud parametrar avsnitt med relevanta metadata om token.</span><span class="sxs-lookup"><span data-stu-id="bc798-117">The v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about the token.</span></span>  <span data-ttu-id="bc798-118">Om du avkoda rubriken för en av våra nuvarande JWTs hittade något som liknar:</span><span class="sxs-lookup"><span data-stu-id="bc798-118">If you decode the header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="bc798-119">Där både ”x5t” och ”barn” identifiera den offentliga nyckeln som ska användas för att validera token signatur, som hämtas från metadataslutpunkten OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bc798-119">Where both the "x5t" and "kid" properties identify the public key that should be used to validate the token\`s signature, as retrieved from the OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="bc798-120">Ändra vi gör här är att ta bort egenskapen ”x5t”.</span><span class="sxs-lookup"><span data-stu-id="bc798-120">The change we are making here is to remove the "x5t" property.</span></span>  <span data-ttu-id="bc798-121">Du kan fortsätta att använda samma mekanismer för att validera token, men bör enbart använda egenskapen ”barn” för att hämta rätt offentlig nyckel som anges i protokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bc798-121">You may continue to use the same mechanisms to validate tokens, but should rely only on the "kid" property to retrieve the correct public key, as specified in the OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="bc798-122">**Jobbet: Kontrollera att din app inte beror på förekomsten av x5t-värdet.**</span><span class="sxs-lookup"><span data-stu-id="bc798-122">**Your job: Make sure your app does not depend on the existence of the x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="bc798-123">Ta bort profile_info</span><span class="sxs-lookup"><span data-stu-id="bc798-123">Removing profile_info</span></span>
<span data-ttu-id="bc798-124">Tidigare v2.0-slutpunkten har tagits returnerar ett JSON-objekt med base64-kodade i token svar som anropas `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="bc798-124">Previously, the v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="bc798-125">När du begär en åtkomst-token från v2.0-slutpunkten genom att skicka en begäran om att:</span><span class="sxs-lookup"><span data-stu-id="bc798-125">When requesting an access token from the v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="bc798-126">Svaret ser ut som följande JSON-objekt:</span><span class="sxs-lookup"><span data-stu-id="bc798-126">The response would look like the following JSON object:</span></span>

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="bc798-127">Den `profile_info` värdet finns information om den användare som signerade i app - deras visningsnamn, förnamn, efternamn, e-postadress, identifierare och så vidare.</span><span class="sxs-lookup"><span data-stu-id="bc798-127">The `profile_info` value contained information about the user who signed into the app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="bc798-128">Främst i `profile_info` användes för cachelagring av token och visa syften.</span><span class="sxs-lookup"><span data-stu-id="bc798-128">Primarily, the `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="bc798-129">Vi nu bort den `profile_info` värdet – men oroa dig inte, vi fortfarande tillhandahåller denna information för utvecklare i en något annan plats.</span><span class="sxs-lookup"><span data-stu-id="bc798-129">We are now removing the `profile_info` value – but don't worry, we are still providing this information to developers in a slightly different place.</span></span>  <span data-ttu-id="bc798-130">I stället för `profile_info`, v2.0-slutpunkten nu returnerar ett `id_token` i varje token svar:</span><span class="sxs-lookup"><span data-stu-id="bc798-130">Instead of `profile_info`, the v2.0 endpoint will now return an `id_token` in each token response:</span></span>

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="bc798-131">Du kan avkoda och parsa id_token för att hämta samma information som du har fått från profile_info.</span><span class="sxs-lookup"><span data-stu-id="bc798-131">You may decode and parse the id_token to retrieve the same information that you received from profile_info.</span></span>  <span data-ttu-id="bc798-132">Id_token är en JSON-Webbtoken (JWT), med innehåll som anges av OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bc798-132">The id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="bc798-133">Koden för detta så bör vara mycket lik – du behöver att extrahera id_token mellersta segmentet (brödtext) och base64 avkoda komma åt JSON-objekt i.</span><span class="sxs-lookup"><span data-stu-id="bc798-133">The code for doing so should be very similar – you simply need to extract the middle segment (the body) of the id_token and base64 decode it to access the JSON object within.</span></span>

<span data-ttu-id="bc798-134">Under de kommande två veckorna bör du skapa kod din app att hämta informationen från antingen den `id_token` eller `profile_info`, beroende på vad som finns.</span><span class="sxs-lookup"><span data-stu-id="bc798-134">Over the next two weeks, you should code your app to retrieve the user information from either the `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="bc798-135">På så sätt när ändringen har gjorts kan din app kan sömlöst hantera övergången från `profile_info` till `id_token` utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="bc798-135">That way when the change is made, your app can seamlessly handle the transition from `profile_info` to `id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc798-136">**Jobbet: Kontrollera att din app inte beror på förekomsten av den `profile_info` värde.**</span><span class="sxs-lookup"><span data-stu-id="bc798-136">**Your job: Make sure your app does not depend on the existence of the `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="bc798-137">Ta bort id_token_expires_in</span><span class="sxs-lookup"><span data-stu-id="bc798-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="bc798-138">Liknar `profile_info`, vi också bort den `id_token_expires_in` parametern från svar.</span><span class="sxs-lookup"><span data-stu-id="bc798-138">Similar to `profile_info`, we are also removing the `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="bc798-139">Tidigare v2.0-slutpunkten kan returnera ett värde för `id_token_expires_in` tillsammans med varje id_token svar, till exempel på ett auktorisera svar:</span><span class="sxs-lookup"><span data-stu-id="bc798-139">Previously, the v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="bc798-140">Eller i ett token svar:</span><span class="sxs-lookup"><span data-stu-id="bc798-140">Or in a token response:</span></span>

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="bc798-141">Den `id_token_expires_in` värdet anger hur många sekunder som id_token gälla för.</span><span class="sxs-lookup"><span data-stu-id="bc798-141">The `id_token_expires_in` value would indicate the number of seconds the id_token would remain valid for.</span></span>  <span data-ttu-id="bc798-142">Nu ska vi bort den `id_token_expires_in` värdet helt.</span><span class="sxs-lookup"><span data-stu-id="bc798-142">Now, we are removing the `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="bc798-143">Du kan i stället använda OpenID Connect-standard `nbf` och `exp` utger sig för att kontrollera giltigheten för en id_token.</span><span class="sxs-lookup"><span data-stu-id="bc798-143">Instead, you may use the OpenID Connect standard `nbf` and `exp` claims to examine the validity of an id_token.</span></span>  <span data-ttu-id="bc798-144">Finns det [v2.0 tokenreferens](active-directory-v2-tokens.md) mer information om dessa anspråk.</span><span class="sxs-lookup"><span data-stu-id="bc798-144">See the [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc798-145">**Jobbet: Kontrollera att din app inte beror på förekomsten av den `id_token_expires_in` värde.**</span><span class="sxs-lookup"><span data-stu-id="bc798-145">**Your job: Make sure your app does not depend on the existence of the `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a><span data-ttu-id="bc798-146">Ändra de anspråk som returneras av omfånget = openid</span><span class="sxs-lookup"><span data-stu-id="bc798-146">Changing the claims returned by scope=openid</span></span>
<span data-ttu-id="bc798-147">Den här ändringen kommer att de viktigaste – faktum, påverkar nästan alla program som använder v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bc798-147">This change will be the most significant – in fact, it will affect almost every app that uses the v2.0 endpoint.</span></span>  <span data-ttu-id="bc798-148">Många program skicka begäranden till v2.0-slutpunkten med den `openid` omfång som:</span><span class="sxs-lookup"><span data-stu-id="bc798-148">Many applications send requests to the v2.0 endpoint using the `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="bc798-149">Idag, när användaren ger ditt medgivande för det `openid` omfång, appen tar emot en mängd information om användaren i den resulterande id_token.</span><span class="sxs-lookup"><span data-stu-id="bc798-149">Today, when the user grants consent for the `openid` scope, your app receives a wealth of information about the user in the resulting id_token.</span></span>  <span data-ttu-id="bc798-150">Dessa anspråk kan innehålla användarens namn, prioriterade användarnamn, e-postadress, objekt-ID och mer.</span><span class="sxs-lookup"><span data-stu-id="bc798-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="bc798-151">I den här uppdateringen kan vi ändrar informationen som den `openid` omfång ger din appåtkomst till, att bättre comform med OpenID Connect-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="bc798-151">In this update, we are changing the information that the `openid` scope affords your app access to, to better comform with the OpenID Connect specification.</span></span>  <span data-ttu-id="bc798-152">Den `openid` omfång kommer endast att din app kan logga in användaren i och ta emot en appspecifika identifierare för användare i den `sub` anspråk för id_token.</span><span class="sxs-lookup"><span data-stu-id="bc798-152">The `openid` scope will only allow your app to sign the user in, and receive an app-specific identifier for the user in the `sub` claim of the id_token.</span></span>  <span data-ttu-id="bc798-153">Anspråk i en id_token med endast den `openid` omfång beviljas kommer att vara fritt från personligt identifierbar information.</span><span class="sxs-lookup"><span data-stu-id="bc798-153">The claims in an id_token with only the `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="bc798-154">Exempel id_token anspråk är:</span><span class="sxs-lookup"><span data-stu-id="bc798-154">Example id_token claims are:</span></span>

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

<span data-ttu-id="bc798-155">Om du vill hämta personligt identifierbar information (PII) om användaren i din app behöver din app och begär ytterligare behörighet från användaren.</span><span class="sxs-lookup"><span data-stu-id="bc798-155">If you want to obtain personally identifiable information (PII) about the user in your app, your app will need to request additional permissions from the user.</span></span>  <span data-ttu-id="bc798-156">Vi introducerar stöd för två nya scope från OpenID Connect-specifikationen – den `email` och `profile` scope – som gör att du gör.</span><span class="sxs-lookup"><span data-stu-id="bc798-156">We are introducing support for two new scopes from the OpenID Connect spec – the `email` and `profile` scopes – which allow you to do so.</span></span>

* <span data-ttu-id="bc798-157">Den `email` scope är mycket enkelt – den kan din appåtkomst till användarens primära e-postadress via den `email` anspråk i id_token.</span><span class="sxs-lookup"><span data-stu-id="bc798-157">The `email` scope is very straightforward – it allows your app access to the user's primary email address via the `email` claim in the id_token.</span></span>  <span data-ttu-id="bc798-158">Observera att den `email` anspråk alltid visas inte i id_tokens – den endast ska ingå om sådana finns i användarens profil.</span><span class="sxs-lookup"><span data-stu-id="bc798-158">Note that the `email` claim will not always be present in id_tokens – it will only be included if available in the user's profile.</span></span>
* <span data-ttu-id="bc798-159">Den `profile` omfång ger din appåtkomst till andra grundläggande information om användare – deras namn, prioriterade username, objekt-ID och så vidare.</span><span class="sxs-lookup"><span data-stu-id="bc798-159">The `profile` scope affords your app access to all other basic information about the user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="bc798-160">Du kan code din app på en minimal avslöjande sätt – du kan be användaren för just uppsättningen information att din app kräver för att utföra sitt jobb.</span><span class="sxs-lookup"><span data-stu-id="bc798-160">This allows you to code your app in a minimal-disclosure fashion – you can ask the user for just the set of information that your app requires to do its job.</span></span>  <span data-ttu-id="bc798-161">Om du vill fortsätta få en fullständig uppsättning användarinformation som din app erhåller bör du inkludera alla tre scope i din auktoriseringsförfrågningar:</span><span class="sxs-lookup"><span data-stu-id="bc798-161">If you want to continue getting the full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="bc798-162">Din app kan börja skicka den `email` och `profile` scope omedelbart och v2.0-slutpunkten accepterar dessa två scope och börja begär behörighet från användare efter behov.</span><span class="sxs-lookup"><span data-stu-id="bc798-162">Your app can begin sending the `email` and `profile` scopes immediately, and the v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="bc798-163">Men ändringen i tolkning av den `openid` scope börjar inte gälla för några veckor.</span><span class="sxs-lookup"><span data-stu-id="bc798-163">However, the change in the interpretation of the `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc798-164">**Jobbet: lägga till den `profile` och `email` scope om information om användaren krävs för din app.**</span><span class="sxs-lookup"><span data-stu-id="bc798-164">**Your job: Add the `profile` and `email` scopes if your app requires information about the user.**</span></span>  <span data-ttu-id="bc798-165">Observera att ADAL innehåller båda dessa behörigheter i begäranden som standard.</span><span class="sxs-lookup"><span data-stu-id="bc798-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a><span data-ttu-id="bc798-166">Ta bort utfärdaren avslutande snedstreck.</span><span class="sxs-lookup"><span data-stu-id="bc798-166">Removing the issuer trailing slash.</span></span>
<span data-ttu-id="bc798-167">Tidigare utformades utfärdaren värdet som visas i token från v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="bc798-167">Previously, the issuer value that appears in tokens from the v2.0 endpoint took the form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="bc798-168">Guid var där klient-ID för Azure AD-klienten som utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="bc798-168">Where the guid was the tenantId of the Azure AD tenant which issued the token.</span></span>  <span data-ttu-id="bc798-169">Med de här ändringarna blir utfärdaren värdet</span><span class="sxs-lookup"><span data-stu-id="bc798-169">With these changes, the issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="bc798-170">i båda token och OpenID Connect discovery-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="bc798-170">in both tokens and in the OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc798-171">**Jobbet: Kontrollera att din app accepterar utfärdaren värde både med och utan avslutande snedstreck vid verifiering av utfärdare.**</span><span class="sxs-lookup"><span data-stu-id="bc798-171">**Your job: Make sure your app accepts the issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="bc798-172">Varför ändra?</span><span class="sxs-lookup"><span data-stu-id="bc798-172">Why change?</span></span>
<span data-ttu-id="bc798-173">Huvudsyftet med introduktion till dessa ändringar är att vara kompatibel med OpenID Connect standard-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="bc798-173">The primary motivation for introducing these changes is to be compliant with the OpenID Connect standard specification.</span></span>  <span data-ttu-id="bc798-174">Genom OpenID Connect kompatibla, hoppas vi att minimera skillnaderna mellan integrera med Microsoft identity-tjänster och andra tjänster identitet i branschen.</span><span class="sxs-lookup"><span data-stu-id="bc798-174">By being OpenID Connect compliant, we hope to minimize differences between integrating with Microsoft identity services and with other identity services in the industry.</span></span>  <span data-ttu-id="bc798-175">Vi vill att utvecklare kan använda sina favorit öppen källkod autentiseringsbibliotek utan att behöva ändra biblioteken för att hantera Microsoft skillnader.</span><span class="sxs-lookup"><span data-stu-id="bc798-175">We want to enable developers to use their favorite open source authentication libraries without having to alter the libraries to accommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="bc798-176">Vad kan du göra?</span><span class="sxs-lookup"><span data-stu-id="bc798-176">What can you do?</span></span>
<span data-ttu-id="bc798-177">Från och med idag, kan du börja gör alla ändringar som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="bc798-177">As of today, you can begin making all of the changes described above.</span></span>  <span data-ttu-id="bc798-178">Du bör omedelbart:</span><span class="sxs-lookup"><span data-stu-id="bc798-178">You should immediately:</span></span>

1. <span data-ttu-id="bc798-179">**Ta bort eventuella beroenden på den `x5t` huvud-parametern.**</span><span class="sxs-lookup"><span data-stu-id="bc798-179">**Remove any dependencies on the `x5t` header parameter.**</span></span>
2. <span data-ttu-id="bc798-180">**Hantera övergången från `profile_info` till `id_token` i token svar.**</span><span class="sxs-lookup"><span data-stu-id="bc798-180">**Gracefully handle the transition from `profile_info` to `id_token` in token responses.**</span></span>
3. <span data-ttu-id="bc798-181">**Ta bort eventuella beroenden på den `id_token_expires_in` parametern svar.**</span><span class="sxs-lookup"><span data-stu-id="bc798-181">**Remove any dependencies on the `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="bc798-182">**Lägg till den `profile` och `email` scope till din app om din app behöver grundläggande information.**</span><span class="sxs-lookup"><span data-stu-id="bc798-182">**Add the `profile` and `email` scopes to your app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="bc798-183">**Acceptera utfärdaren värden i token både med och utan avslutande snedstreck.**</span><span class="sxs-lookup"><span data-stu-id="bc798-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="bc798-184">Vår [v2.0-protokollet dokumentationen](active-directory-v2-protocols.md) har redan uppdaterats för att återspegla ändringarna, så att du kan använda den som en referens i hjälpa uppdatera din kod.</span><span class="sxs-lookup"><span data-stu-id="bc798-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated to reflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="bc798-185">Om du har fler frågor om omfattningen av ändringarna gärna nå oss på Twitter på @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="bc798-185">If you have any further questions on the scope of the changes, please feel free to reach out to us on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="bc798-186">Hur ofta sker protokollet ändringar?</span><span class="sxs-lookup"><span data-stu-id="bc798-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="bc798-187">Vi förutser inte några ytterligare bryta ändras till autentiseringsprotokollet.</span><span class="sxs-lookup"><span data-stu-id="bc798-187">We do not foresee any further breaking changes to the authentication protocols.</span></span>  <span data-ttu-id="bc798-188">Vi avsiktligt paketering ändringarna i en versionen så att du inte behöver gå igenom den här typen av uppdateringen igen när snart.</span><span class="sxs-lookup"><span data-stu-id="bc798-188">We are intentionally bundling these changes into one release so that you won\`t have to go through this type of update process again any time soon.</span></span>  <span data-ttu-id="bc798-189">Naturligtvis vi kommer att fortsätta att lägga till funktioner till Autentiseringstjänsten konvergerade v2.0 som du kan dra nytta av, men ändringarna bör vara tillsatsen och inte break befintlig kod.</span><span class="sxs-lookup"><span data-stu-id="bc798-189">Of course, we will continue to add features to the converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="bc798-190">Slutligen vill vi säga Tack för provat saker i förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="bc798-190">Lastly, we would like to say thank you for trying things out during the preview period.</span></span>  <span data-ttu-id="bc798-191">Insikter och erfarenheter från våra tidiga brukare har ovärderlig hittills och vi hoppas att du kommer att fortsätta att dela dina åsikter och idéer.</span><span class="sxs-lookup"><span data-stu-id="bc798-191">The insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue to share your opinions and ideas.</span></span>

<span data-ttu-id="bc798-192">Kodning Grattis!</span><span class="sxs-lookup"><span data-stu-id="bc798-192">Happy coding!</span></span>

<span data-ttu-id="bc798-193">Microsoft Identity-delning</span><span class="sxs-lookup"><span data-stu-id="bc798-193">The Microsoft Identity Division</span></span>

