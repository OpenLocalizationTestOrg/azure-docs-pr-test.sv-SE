---
title: aaaChanges toohello Azure AD v2.0-slutpunkten | Microsoft Docs
description: "En beskrivning av de ändringar som görs toohello app model v2.0 förhandsversion protokoll."
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
ms.openlocfilehash: d7b28a481e12d5dbbc4a10110193bdbd754f4929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="important-updates-toohello-v20-authentication-protocols"></a><span data-ttu-id="44f33-103">Viktiga uppdateringar toohello v2.0-autentiseringsprotokoll</span><span class="sxs-lookup"><span data-stu-id="44f33-103">Important Updates toohello v2.0 Authentication Protocols</span></span>
<span data-ttu-id="44f33-104">Uppmärksamhet utvecklare!</span><span class="sxs-lookup"><span data-stu-id="44f33-104">Attention developers!</span></span> <span data-ttu-id="44f33-105">Över hello kommer nästa två veckor, vi att göra några uppdateringar tooour v2.0-autentiseringsprotokoll som kan innebära att bryta ändringar för alla program som du har skrivit under våra förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="44f33-105">Over hello next two weeks, we will be making a few updates tooour v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="44f33-106">Som påverkar detta?</span><span class="sxs-lookup"><span data-stu-id="44f33-106">Who does this affect?</span></span>
<span data-ttu-id="44f33-107">Alla appar som har skrivits toouse hello v2.0 konvergerat autentisering slutpunkt</span><span class="sxs-lookup"><span data-stu-id="44f33-107">Any apps that have been written toouse hello v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="44f33-108">Mer information om hello v2.0-slutpunkten kan hittas [här](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44f33-108">More information on hello v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="44f33-109">Om du har skapat en app med hello v2.0-slutpunkten genom att skriva direkt toohello v2.0-protokollet med hjälp av vår OpenID Connect och OAuth web middlewares eller genom att använda andra 3 part bibliotek tooperform, bör du vara beredd tootest ditt projekt och gör nödvändiga ändringar.</span><span class="sxs-lookup"><span data-stu-id="44f33-109">If you have built an app using hello v2.0 endpoint by coding directly toohello v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries tooperform authentication, you should be prepared tootest your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="44f33-110">Som detta påverkar inte?</span><span class="sxs-lookup"><span data-stu-id="44f33-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="44f33-111">Alla appar som har skrivits mot hello produktion Azure AD authentication slutpunkt</span><span class="sxs-lookup"><span data-stu-id="44f33-111">Any apps that have been written against hello production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="44f33-112">Det här protokollet anges i bricka och ska inte ha några ändringar.</span><span class="sxs-lookup"><span data-stu-id="44f33-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="44f33-113">Dessutom om din app **endast** använder våra ADAL-biblioteket tooperform autentisering, behöver du inte toochange något.</span><span class="sxs-lookup"><span data-stu-id="44f33-113">Furthermore, if your app **only** uses our ADAL library tooperform authentication, you won\`t have toochange anything.</span></span>  <span data-ttu-id="44f33-114">ADAL har skärmad appen från hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="44f33-114">ADAL has shielded your app from hello changes.</span></span>  

## <a name="what-are-hello-changes"></a><span data-ttu-id="44f33-115">Vad är hello ändringar?</span><span class="sxs-lookup"><span data-stu-id="44f33-115">What are hello changes?</span></span>
### <a name="removing-hello-x5t-value-from-jwt-headers"></a><span data-ttu-id="44f33-116">Ta bort hello x5t värdet från JWT rubriker</span><span class="sxs-lookup"><span data-stu-id="44f33-116">Removing hello x5t value from JWT headers</span></span>
<span data-ttu-id="44f33-117">hello v2.0-slutpunkten använder JWT-token i stor utsträckning, som innehåller ett huvud parametrar avsnitt med relevanta metadata om hello-token.</span><span class="sxs-lookup"><span data-stu-id="44f33-117">hello v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about hello token.</span></span>  <span data-ttu-id="44f33-118">Om du avkoda hello rubriken för en av våra nuvarande JWTs hittade något som liknar:</span><span class="sxs-lookup"><span data-stu-id="44f33-118">If you decode hello header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="44f33-119">Där båda hello ”x5t” och ”barn” egenskaperna identifiera hello offentlig nyckel som ska använda toovalidate hello token signatur, som hämtas från hello OpenID Connect metadata endpoint.</span><span class="sxs-lookup"><span data-stu-id="44f33-119">Where both hello "x5t" and "kid" properties identify hello public key that should be used toovalidate hello token\`s signature, as retrieved from hello OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="44f33-120">hello ändra vi gör här är tooremove hello ”x5t”-egenskap.</span><span class="sxs-lookup"><span data-stu-id="44f33-120">hello change we are making here is tooremove hello "x5t" property.</span></span>  <span data-ttu-id="44f33-121">Du kan fortsätta toouse hello samma mekanismer toovalidate token, men bör enbart använda hello ”barn” egenskapen tooretrieve hello rätt offentlig nyckel, som anges i hello OpenID Connect-protokollet.</span><span class="sxs-lookup"><span data-stu-id="44f33-121">You may continue toouse hello same mechanisms toovalidate tokens, but should rely only on hello "kid" property tooretrieve hello correct public key, as specified in hello OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="44f33-122">**Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello x5t värde.**</span><span class="sxs-lookup"><span data-stu-id="44f33-122">**Your job: Make sure your app does not depend on hello existence of hello x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="44f33-123">Ta bort profile_info</span><span class="sxs-lookup"><span data-stu-id="44f33-123">Removing profile_info</span></span>
<span data-ttu-id="44f33-124">Tidigare hello v2.0-slutpunkten har returnerar ett JSON-objekt med base64-kodade i token svar som anropas `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="44f33-124">Previously, hello v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="44f33-125">När du begär en åtkomst-token från hello v2.0-slutpunkten genom att skicka en begäran om att:</span><span class="sxs-lookup"><span data-stu-id="44f33-125">When requesting an access token from hello v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="44f33-126">hello svar ser ut som hello följande JSON-objekt:</span><span class="sxs-lookup"><span data-stu-id="44f33-126">hello response would look like hello following JSON object:</span></span>

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

<span data-ttu-id="44f33-127">Hej `profile_info` värdet finns information om hello användare inloggad hello app – deras visningsnamn, förnamn, efternamn, e-postadress, identifierare och så vidare.</span><span class="sxs-lookup"><span data-stu-id="44f33-127">hello `profile_info` value contained information about hello user who signed into hello app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="44f33-128">Främst hello `profile_info` användes för cachelagring av token och visa syften.</span><span class="sxs-lookup"><span data-stu-id="44f33-128">Primarily, hello `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="44f33-129">Vi nu bort hello `profile_info` värdet – men oroa dig inte, vi fortfarande lämnar den här informationen toodevelopers i en något annan plats.</span><span class="sxs-lookup"><span data-stu-id="44f33-129">We are now removing hello `profile_info` value – but don't worry, we are still providing this information toodevelopers in a slightly different place.</span></span>  <span data-ttu-id="44f33-130">I stället för `profile_info`, hello v2.0-slutpunkten nu returnerar ett `id_token` i varje token svar:</span><span class="sxs-lookup"><span data-stu-id="44f33-130">Instead of `profile_info`, hello v2.0 endpoint will now return an `id_token` in each token response:</span></span>

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

<span data-ttu-id="44f33-131">Du kan avkoda och parsa hello id_token tooretrieve hello samma information som du har fått från profile_info.</span><span class="sxs-lookup"><span data-stu-id="44f33-131">You may decode and parse hello id_token tooretrieve hello same information that you received from profile_info.</span></span>  <span data-ttu-id="44f33-132">Hej id_token är en JSON-Webbtoken (JWT), med innehåll som anges av OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="44f33-132">hello id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="44f33-133">hello kod för att göra det bör vara mycket lik – du behöver tooextract hello mitten segmentet (hello brödtext) i hello id_token och base64 avkoda den tooaccess hello JSON-objekt i.</span><span class="sxs-lookup"><span data-stu-id="44f33-133">hello code for doing so should be very similar – you simply need tooextract hello middle segment (hello body) of hello id_token and base64 decode it tooaccess hello JSON object within.</span></span>

<span data-ttu-id="44f33-134">Över hello nästa två veckor, bör du skapa kod din app tooretrieve hello användarinformation från antingen hello `id_token` eller `profile_info`, beroende på vad som finns.</span><span class="sxs-lookup"><span data-stu-id="44f33-134">Over hello next two weeks, you should code your app tooretrieve hello user information from either hello `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="44f33-135">På så sätt när hello ändring görs kan din app kan sömlöst hantera hello övergången från `profile_info` för`id_token` utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="44f33-135">That way when hello change is made, your app can seamlessly handle hello transition from `profile_info` too`id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44f33-136">**Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello `profile_info` värde.**</span><span class="sxs-lookup"><span data-stu-id="44f33-136">**Your job: Make sure your app does not depend on hello existence of hello `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="44f33-137">Ta bort id_token_expires_in</span><span class="sxs-lookup"><span data-stu-id="44f33-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="44f33-138">Liknande för`profile_info`, vi också bort hello `id_token_expires_in` parametern från svar.</span><span class="sxs-lookup"><span data-stu-id="44f33-138">Similar too`profile_info`, we are also removing hello `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="44f33-139">Tidigare hello v2.0-slutpunkten kan returnera ett värde för `id_token_expires_in` tillsammans med varje id_token svar, till exempel på ett auktorisera svar:</span><span class="sxs-lookup"><span data-stu-id="44f33-139">Previously, hello v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="44f33-140">Eller i ett token svar:</span><span class="sxs-lookup"><span data-stu-id="44f33-140">Or in a token response:</span></span>

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

<span data-ttu-id="44f33-141">Hej `id_token_expires_in` värdet visar hello antalet sekunder som hello id_token gälla för.</span><span class="sxs-lookup"><span data-stu-id="44f33-141">hello `id_token_expires_in` value would indicate hello number of seconds hello id_token would remain valid for.</span></span>  <span data-ttu-id="44f33-142">Nu ska vi bort hello `id_token_expires_in` värdet helt.</span><span class="sxs-lookup"><span data-stu-id="44f33-142">Now, we are removing hello `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="44f33-143">Du kan i stället använda hello OpenID Connect standard `nbf` och `exp` tooexamine hello giltigheten för en id_token-anspråk.</span><span class="sxs-lookup"><span data-stu-id="44f33-143">Instead, you may use hello OpenID Connect standard `nbf` and `exp` claims tooexamine hello validity of an id_token.</span></span>  <span data-ttu-id="44f33-144">Se hello [v2.0 tokenreferens](active-directory-v2-tokens.md) mer information om dessa anspråk.</span><span class="sxs-lookup"><span data-stu-id="44f33-144">See hello [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44f33-145">**Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello `id_token_expires_in` värde.**</span><span class="sxs-lookup"><span data-stu-id="44f33-145">**Your job: Make sure your app does not depend on hello existence of hello `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a><span data-ttu-id="44f33-146">Ändra hello anspråk som returneras av omfånget = openid</span><span class="sxs-lookup"><span data-stu-id="44f33-146">Changing hello claims returned by scope=openid</span></span>
<span data-ttu-id="44f33-147">Den här ändringen kommer att hello viktigaste – faktum, påverkar nästan alla program som använder hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="44f33-147">This change will be hello most significant – in fact, it will affect almost every app that uses hello v2.0 endpoint.</span></span>  <span data-ttu-id="44f33-148">Många program skicka begäranden toohello v2.0-slutpunkten med hello `openid` omfång som:</span><span class="sxs-lookup"><span data-stu-id="44f33-148">Many applications send requests toohello v2.0 endpoint using hello `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="44f33-149">Idag, när användaren hello ger ditt medgivande för hello `openid` omfång, appen tar emot en mängd information om hello användare i hello resulterande id_token.</span><span class="sxs-lookup"><span data-stu-id="44f33-149">Today, when hello user grants consent for hello `openid` scope, your app receives a wealth of information about hello user in hello resulting id_token.</span></span>  <span data-ttu-id="44f33-150">Dessa anspråk kan innehålla användarens namn, prioriterade användarnamn, e-postadress, objekt-ID och mer.</span><span class="sxs-lookup"><span data-stu-id="44f33-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="44f33-151">I den här uppdateringen kan vi ändrar hello information som hello `openid` omfång ger din appåtkomst till toobetter comform med hello OpenID Connect-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="44f33-151">In this update, we are changing hello information that hello `openid` scope affords your app access to, toobetter comform with hello OpenID Connect specification.</span></span>  <span data-ttu-id="44f33-152">Hej `openid` omfång kommer endast att appen toosign hello användare i och får en appspecifika identifierare för hello användare på hello `sub` anspråk av hello id_token.</span><span class="sxs-lookup"><span data-stu-id="44f33-152">hello `openid` scope will only allow your app toosign hello user in, and receive an app-specific identifier for hello user in hello `sub` claim of hello id_token.</span></span>  <span data-ttu-id="44f33-153">Hej anspråk i en id_token med endast hello `openid` omfång beviljas kommer att vara fritt från personligt identifierbar information.</span><span class="sxs-lookup"><span data-stu-id="44f33-153">hello claims in an id_token with only hello `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="44f33-154">Exempel id_token anspråk är:</span><span class="sxs-lookup"><span data-stu-id="44f33-154">Example id_token claims are:</span></span>

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

<span data-ttu-id="44f33-155">Om du vill tooobtain personligt identifierbar information (PII) om hello användare i din app behöver din app toorequest ytterligare behörigheter från hello användare.</span><span class="sxs-lookup"><span data-stu-id="44f33-155">If you want tooobtain personally identifiable information (PII) about hello user in your app, your app will need toorequest additional permissions from hello user.</span></span>  <span data-ttu-id="44f33-156">Vi introducerar stöd för två nya scope från hello OpenID Connect spec – hello `email` och `profile` scope – som gör att du toodo så.</span><span class="sxs-lookup"><span data-stu-id="44f33-156">We are introducing support for two new scopes from hello OpenID Connect spec – hello `email` and `profile` scopes – which allow you toodo so.</span></span>

* <span data-ttu-id="44f33-157">Hej `email` scope är mycket enkelt – det gör att din app åtkomst toohello användarens primära e-postadress via hello `email` anspråk i hello id_token.</span><span class="sxs-lookup"><span data-stu-id="44f33-157">hello `email` scope is very straightforward – it allows your app access toohello user's primary email address via hello `email` claim in hello id_token.</span></span>  <span data-ttu-id="44f33-158">Observera att hello `email` anspråk alltid visas inte i id_tokens – den endast ska ingå om de är tillgängliga i hello användarprofil.</span><span class="sxs-lookup"><span data-stu-id="44f33-158">Note that hello `email` claim will not always be present in id_tokens – it will only be included if available in hello user's profile.</span></span>
* <span data-ttu-id="44f33-159">Hej `profile` omfång ger din app åtkomst tooall andra grundläggande information om hello användare – deras namn, prioriterade username, objekt-ID och så vidare.</span><span class="sxs-lookup"><span data-stu-id="44f33-159">hello `profile` scope affords your app access tooall other basic information about hello user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="44f33-160">Detta gör att du toocode din app på en minimal avslöjande sätt – du kan be hello användaren för bara hello uppsättning information att appen kräver toodo sitt jobb.</span><span class="sxs-lookup"><span data-stu-id="44f33-160">This allows you toocode your app in a minimal-disclosure fashion – you can ask hello user for just hello set of information that your app requires toodo its job.</span></span>  <span data-ttu-id="44f33-161">Om du vill hämta hello hela uppsättningen av användarinformation som din app erhåller toocontinue, bör du inkludera alla tre scope i din auktoriseringsförfrågningar:</span><span class="sxs-lookup"><span data-stu-id="44f33-161">If you want toocontinue getting hello full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="44f33-162">Din app kan börja skicka hello `email` och `profile` scope omedelbart och hello v2.0-slutpunkten accepterar dessa två scope och börja begär behörighet från användare efter behov.</span><span class="sxs-lookup"><span data-stu-id="44f33-162">Your app can begin sending hello `email` and `profile` scopes immediately, and hello v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="44f33-163">Dock hello ändra i hello tolkning av hello `openid` scope börjar inte gälla för några veckor.</span><span class="sxs-lookup"><span data-stu-id="44f33-163">However, hello change in hello interpretation of hello `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44f33-164">**Jobbet: Lägg till hello `profile` och `email` scope om information om hello användare krävs för din app.**</span><span class="sxs-lookup"><span data-stu-id="44f33-164">**Your job: Add hello `profile` and `email` scopes if your app requires information about hello user.**</span></span>  <span data-ttu-id="44f33-165">Observera att ADAL innehåller båda dessa behörigheter i begäranden som standard.</span><span class="sxs-lookup"><span data-stu-id="44f33-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a><span data-ttu-id="44f33-166">Ta bort hello utfärdaren avslutande snedstreck.</span><span class="sxs-lookup"><span data-stu-id="44f33-166">Removing hello issuer trailing slash.</span></span>
<span data-ttu-id="44f33-167">Tidigare tog hello utfärdaren värde som visas i token från hello v2.0-slutpunkten hello formulär</span><span class="sxs-lookup"><span data-stu-id="44f33-167">Previously, hello issuer value that appears in tokens from hello v2.0 endpoint took hello form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="44f33-168">Hello guid var där hello tenantId för hello Azure AD-klienten som utfärdade hello-token.</span><span class="sxs-lookup"><span data-stu-id="44f33-168">Where hello guid was hello tenantId of hello Azure AD tenant which issued hello token.</span></span>  <span data-ttu-id="44f33-169">Med de här ändringarna blir hello utfärdaren värdet</span><span class="sxs-lookup"><span data-stu-id="44f33-169">With these changes, hello issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="44f33-170">i båda token och hello OpenID Connect discovery-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="44f33-170">in both tokens and in hello OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44f33-171">**Jobbet: Kontrollera att din app tillåter hello utfärdaren både med och utan avslutande snedstreck vid verifiering av utfärdare.**</span><span class="sxs-lookup"><span data-stu-id="44f33-171">**Your job: Make sure your app accepts hello issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="44f33-172">Varför ändra?</span><span class="sxs-lookup"><span data-stu-id="44f33-172">Why change?</span></span>
<span data-ttu-id="44f33-173">hello Huvudsyftet med introduktion till dessa ändringar är toobe som är kompatibla med hello OpenID Connect standard specifikation.</span><span class="sxs-lookup"><span data-stu-id="44f33-173">hello primary motivation for introducing these changes is toobe compliant with hello OpenID Connect standard specification.</span></span>  <span data-ttu-id="44f33-174">Genom OpenID Connect kompatibla, hoppas vi toominimize skillnaderna mellan integrera med Microsoft identity-tjänster och andra tjänster identitet i hello bransch.</span><span class="sxs-lookup"><span data-stu-id="44f33-174">By being OpenID Connect compliant, we hope toominimize differences between integrating with Microsoft identity services and with other identity services in hello industry.</span></span>  <span data-ttu-id="44f33-175">Vi vill tooenable utvecklare toouse sina favorit öppen källkod autentiseringsbibliotek utan tooalter hello bibliotek tooaccommodate Microsoft skillnader.</span><span class="sxs-lookup"><span data-stu-id="44f33-175">We want tooenable developers toouse their favorite open source authentication libraries without having tooalter hello libraries tooaccommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="44f33-176">Vad kan du göra?</span><span class="sxs-lookup"><span data-stu-id="44f33-176">What can you do?</span></span>
<span data-ttu-id="44f33-177">Från och med idag, kan du börja att göra alla hello ändringar som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="44f33-177">As of today, you can begin making all of hello changes described above.</span></span>  <span data-ttu-id="44f33-178">Du bör omedelbart:</span><span class="sxs-lookup"><span data-stu-id="44f33-178">You should immediately:</span></span>

1. <span data-ttu-id="44f33-179">**Ta bort eventuella beroenden på hello `x5t` huvud-parametern.**</span><span class="sxs-lookup"><span data-stu-id="44f33-179">**Remove any dependencies on hello `x5t` header parameter.**</span></span>
2. <span data-ttu-id="44f33-180">**Hantera hello övergången från `profile_info` för`id_token` i token svar.**</span><span class="sxs-lookup"><span data-stu-id="44f33-180">**Gracefully handle hello transition from `profile_info` too`id_token` in token responses.**</span></span>
3. <span data-ttu-id="44f33-181">**Ta bort eventuella beroenden på hello `id_token_expires_in` parametern svar.**</span><span class="sxs-lookup"><span data-stu-id="44f33-181">**Remove any dependencies on hello `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="44f33-182">**Lägg till hello `profile` och `email` scope tooyour app om din app behöver grundläggande information.**</span><span class="sxs-lookup"><span data-stu-id="44f33-182">**Add hello `profile` and `email` scopes tooyour app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="44f33-183">**Acceptera utfärdaren värden i token både med och utan avslutande snedstreck.**</span><span class="sxs-lookup"><span data-stu-id="44f33-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="44f33-184">Vår [v2.0-protokollet dokumentationen](active-directory-v2-protocols.md) redan har uppdaterats tooreflect ändringarna, så att du kan använda den som en referens i hjälpa uppdatera din kod.</span><span class="sxs-lookup"><span data-stu-id="44f33-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated tooreflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="44f33-185">Om du har fler frågor på hello omfattning hello ändringar kan du antingen ledigt tooreach out toous på Twitter på @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="44f33-185">If you have any further questions on hello scope of hello changes, please feel free tooreach out toous on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="44f33-186">Hur ofta sker protokollet ändringar?</span><span class="sxs-lookup"><span data-stu-id="44f33-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="44f33-187">Vi förutser inte några ytterligare bryta ändras toohello autentiseringsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="44f33-187">We do not foresee any further breaking changes toohello authentication protocols.</span></span>  <span data-ttu-id="44f33-188">Vi avsiktligt paketering ändringarna i en versionen så att du inte toogo via den här typen av uppdateringen igen när snart.</span><span class="sxs-lookup"><span data-stu-id="44f33-188">We are intentionally bundling these changes into one release so that you won\`t have toogo through this type of update process again any time soon.</span></span>  <span data-ttu-id="44f33-189">Naturligtvis vi kommer att fortsätta tooadd funktioner toohello konvergerat v2.0 authentication-tjänsten som du kan dra nytta av, men ändringarna bör vara tillsatsen och inte break befintlig kod.</span><span class="sxs-lookup"><span data-stu-id="44f33-189">Of course, we will continue tooadd features toohello converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="44f33-190">Slutligen vill vi toosay Tack för provat saker under hello förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="44f33-190">Lastly, we would like toosay thank you for trying things out during hello preview period.</span></span>  <span data-ttu-id="44f33-191">hello insikter och erfarenheter från våra tidiga brukare har ovärderlig hittills och vi hoppas att du kommer att fortsätta tooshare dina åsikter och idéer.</span><span class="sxs-lookup"><span data-stu-id="44f33-191">hello insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue tooshare your opinions and ideas.</span></span>

<span data-ttu-id="44f33-192">Kodning Grattis!</span><span class="sxs-lookup"><span data-stu-id="44f33-192">Happy coding!</span></span>

<span data-ttu-id="44f33-193">hello Microsoft Identity Division</span><span class="sxs-lookup"><span data-stu-id="44f33-193">hello Microsoft Identity Division</span></span>

