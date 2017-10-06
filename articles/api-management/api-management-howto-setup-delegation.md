---
title: "aaaHow toodelegate användarens registrering och produkten prenumeration"
description: "Lär dig hur toodelegate användaren registrering och produkten prenumeration tooa tredjeparts i Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a><span data-ttu-id="92a59-103">Hur toodelegate användarens registrering och produkt-prenumeration</span><span class="sxs-lookup"><span data-stu-id="92a59-103">How toodelegate user registration and product subscription</span></span>
<span data-ttu-id="92a59-104">Delegering kan du toouse din befintliga webbplats för att hantera developer logga-i/sign-upp och prenumeration tooproducts som motsats toousing hello inbyggda funktioner i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="92a59-104">Delegation allows you toouse your existing website for handling developer sign-in/sign-up and subscription tooproducts as opposed toousing hello built-in functionality in hello developer portal.</span></span> <span data-ttu-id="92a59-105">Detta aktiverar webbplats tooown hello användardata och utföra hello verifiering av stegen i ett anpassat sätt.</span><span class="sxs-lookup"><span data-stu-id="92a59-105">This enables your website tooown hello user data and perform hello validation of these steps in a custom way.</span></span>

## <span data-ttu-id="92a59-106"><a name="delegate-signin-up"></a>Delegera developer inloggning och registrering</span><span class="sxs-lookup"><span data-stu-id="92a59-106"><a name="delegate-signin-up"> </a>Delegating developer sign-in and sign-up</span></span>
<span data-ttu-id="92a59-107">toodelegate developer inloggning och registrering tooyour befintlig webbplats måste toocreate en slutpunkt för särskilda delegering på webbplatsen som fungerar som hello startpunkt för en sådan begäran som initieras från hello API Management developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="92a59-107">toodelegate developer sign-in and sign-up tooyour existing website you will need toocreate a special delegation endpoint on your site that acts as hello entry-point for any such request initiated from hello API Management developer portal.</span></span>

<span data-ttu-id="92a59-108">hello slutliga arbetsflödet är följande:</span><span class="sxs-lookup"><span data-stu-id="92a59-108">hello final workflow will be as follows:</span></span>

1. <span data-ttu-id="92a59-109">Utvecklare som klickar på hello logga in eller registrera länk på hello API Management developer-portalen</span><span class="sxs-lookup"><span data-stu-id="92a59-109">Developer clicks on hello sign-in or sign-up link at hello API Management developer portal</span></span>
2. <span data-ttu-id="92a59-110">Webbläsaren är omdirigerade toohello delegering slutpunkt</span><span class="sxs-lookup"><span data-stu-id="92a59-110">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="92a59-111">Delegering slutpunkt i RETUR omdirigerar tooor anger Användargränssnittet frågar användaren toosign i eller registrera</span><span class="sxs-lookup"><span data-stu-id="92a59-111">Delegation endpoint in return redirects tooor presents UI asking user toosign-in or sign-up</span></span>
4. <span data-ttu-id="92a59-112">Om åtgärden lyckades kan är hello användaren omdirigerade tillbaka toohello API Management developer Portalsida de startas från</span><span class="sxs-lookup"><span data-stu-id="92a59-112">On success, hello user is redirected back toohello API Management developer portal page they started from</span></span>

<span data-ttu-id="92a59-113">toobegin, låt oss först ställa in API-hantering tooroute begäranden via din slutpunkt för delegering.</span><span class="sxs-lookup"><span data-stu-id="92a59-113">toobegin, let's first set-up API Management tooroute requests via your delegation endpoint.</span></span> <span data-ttu-id="92a59-114">I hello API Management publisher-portalen klickar du på **säkerhet** och klicka sedan på hello **delegering** fliken. Klicka på hello kryssrutan tooenable ombud inloggning & registrering.</span><span class="sxs-lookup"><span data-stu-id="92a59-114">In hello API Management publisher portal, click on **Security** and then click hello **Delegation** tab. Click hello checkbox tooenable 'Delegate sign-in & sign-up'.</span></span>

![Delegering sida][api-management-delegation-signin-up]

* <span data-ttu-id="92a59-116">Bestäm vad hello URL för din slutpunkt för särskilda delegering tas och ange den i hello **delegering slutpunkts-URL** fältet.</span><span class="sxs-lookup"><span data-stu-id="92a59-116">Decide what hello URL of your special delegation endpoint will be and enter it in hello **Delegation endpoint URL** field.</span></span> 
* <span data-ttu-id="92a59-117">Inom hello **delegering autentiseringsnyckel** ange en hemlighet som kommer att använda toocompute en signatur som tooyou för verifiering tooensure som hello begäran verkligen kommer från Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="92a59-117">Within hello **Delegation authentication key** field enter a secret that will be used toocompute a signature provided tooyou for verification tooensure that hello request is indeed coming from Azure API Management.</span></span> <span data-ttu-id="92a59-118">Du kan klicka på hello **generera** knappen toohave API Managemnet slumpmässigt Generera en nyckel för dig.</span><span class="sxs-lookup"><span data-stu-id="92a59-118">You can click hello **generate** button toohave API Managemnet randomly generate a key for you.</span></span>

<span data-ttu-id="92a59-119">Nu måste toocreate hello **delegering endpoint**.</span><span class="sxs-lookup"><span data-stu-id="92a59-119">Now you need toocreate hello **delegation endpoint**.</span></span> <span data-ttu-id="92a59-120">Tooperform har ett antal åtgärder:</span><span class="sxs-lookup"><span data-stu-id="92a59-120">It has tooperform a number of actions:</span></span>

1. <span data-ttu-id="92a59-121">Ta emot en begäran i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="92a59-121">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="92a59-122">*http://www.yourwebsite.com/apimdelegation?operation=Signin&returnUrl= {URL för datakälla på sidan} & salt = {sträng} & sig = {sträng}*</span><span class="sxs-lookup"><span data-stu-id="92a59-122">*http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL of source page}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="92a59-123">Frågeparametrar för hello inloggning / registrering fall:</span><span class="sxs-lookup"><span data-stu-id="92a59-123">Query parameters for hello sign-in / sign-up case:</span></span>
   
   * <span data-ttu-id="92a59-124">**åtgärden**: identifierar vilken typ av begäran om delegering är det - vara **SignIn** i det här fallet</span><span class="sxs-lookup"><span data-stu-id="92a59-124">**operation**: identifies what type of delegation request it is - it can only be **SignIn** in this case</span></span>
   * <span data-ttu-id="92a59-125">**returnUrl**: hello URL till hello sida där hello användaren har klickat på en länk för inloggning eller registrering</span><span class="sxs-lookup"><span data-stu-id="92a59-125">**returnUrl**: hello URL of hello page where hello user clicked on a sign-in or sign-up link</span></span>
   * <span data-ttu-id="92a59-126">**salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet</span><span class="sxs-lookup"><span data-stu-id="92a59-126">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="92a59-127">**sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde</span><span class="sxs-lookup"><span data-stu-id="92a59-127">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="92a59-128">Kontrollera att hello förfrågan kommer från Azure API Management (valfritt, men rekommenderas för säkerhet)</span><span class="sxs-lookup"><span data-stu-id="92a59-128">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="92a59-129">Beräkna en SHA512 HMAC hash för en sträng baserat på hello **returnUrl** och **salt** fråga parametrar ([exempelkoden nedan]):</span><span class="sxs-lookup"><span data-stu-id="92a59-129">Compute an HMAC-SHA512 hash of a string based on hello **returnUrl** and **salt** query parameters ([example code provided below]):</span></span>
     
     > <span data-ttu-id="92a59-130">HMAC (**salt** + ”\n” + **returnUrl**)</span><span class="sxs-lookup"><span data-stu-id="92a59-130">HMAC(**salt** + '\n' + **returnUrl**)</span></span>
     > 
     > 
   * <span data-ttu-id="92a59-131">Jämför hello ovan beräknad toohello hashvärde hello **sig** Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="92a59-131">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="92a59-132">Om hello två hash-värdena matchar vidare toohello nästa steg, annars neka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="92a59-132">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="92a59-133">Kontrollera att du tar emot en begäran om inloggning-i/logga-up: hello **åtgärden** frågeparameter anges för ”**SignIn**”.</span><span class="sxs-lookup"><span data-stu-id="92a59-133">Verify that you are receiving a request for sign-in/sign-up: hello **operation** query parameter will be set too"**SignIn**".</span></span>
4. <span data-ttu-id="92a59-134">Aktuell hello användare med användargränssnitt toosign i eller registrera</span><span class="sxs-lookup"><span data-stu-id="92a59-134">Present hello user with UI toosign-in or sign-up</span></span>
5. <span data-ttu-id="92a59-135">Om hello användaren har registrerat dig har du toocreate motsvarande konto för dem i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="92a59-135">If hello user is signing-up you have toocreate a corresponding account for them in API Management.</span></span> <span data-ttu-id="92a59-136">[Skapa en användare] med hello API Management REST API.</span><span class="sxs-lookup"><span data-stu-id="92a59-136">[Create a user] with hello API Management REST API.</span></span> <span data-ttu-id="92a59-137">När du gör det, se till att du hello användar-ID toohello samma i din användararkivet eller tooan-ID som du kan hålla reda på.</span><span class="sxs-lookup"><span data-stu-id="92a59-137">When doing so, ensure that you set hello user ID toohello same it is in your user store or tooan ID that you can keep track of.</span></span>
6. <span data-ttu-id="92a59-138">När hello användaren kan autentiseras:</span><span class="sxs-lookup"><span data-stu-id="92a59-138">When hello user is successfully authenticated:</span></span>
   
   * <span data-ttu-id="92a59-139">[begär en enkel inloggning (SSO) token] via hello API Management REST API</span><span class="sxs-lookup"><span data-stu-id="92a59-139">[request a single-sign-on (SSO) token] via hello API Management REST API</span></span>
   * <span data-ttu-id="92a59-140">Lägg till en returnUrl fråga parametern toohello SSO-URL som du har fått från hello API-anrop ovan:</span><span class="sxs-lookup"><span data-stu-id="92a59-140">append a returnUrl query parameter toohello SSO URL you have received from hello API call above:</span></span>
     
     > <span data-ttu-id="92a59-141">t.ex. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span><span class="sxs-lookup"><span data-stu-id="92a59-141">e.g. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span></span> 
     > 
     > 
   * <span data-ttu-id="92a59-142">omdirigera hello användaren toohello ovan producerade URL</span><span class="sxs-lookup"><span data-stu-id="92a59-142">redirect hello user toohello above produced URL</span></span>

<span data-ttu-id="92a59-143">I tillägg toohello **SignIn** igen, du kan också utföra kontohantering genom att följa stegen i hello och med någon av följande åtgärder hello.</span><span class="sxs-lookup"><span data-stu-id="92a59-143">In addition toohello **SignIn** operation, you can also perform account management by following hello previous steps and using one of hello following operations.</span></span>

* <span data-ttu-id="92a59-144">**ChangePassword**</span><span class="sxs-lookup"><span data-stu-id="92a59-144">**ChangePassword**</span></span>
* <span data-ttu-id="92a59-145">**ChangeProfile**</span><span class="sxs-lookup"><span data-stu-id="92a59-145">**ChangeProfile**</span></span>
* <span data-ttu-id="92a59-146">**CloseAccount**</span><span class="sxs-lookup"><span data-stu-id="92a59-146">**CloseAccount**</span></span>

<span data-ttu-id="92a59-147">Du måste överföra hello följande frågeparametrar för kontohanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="92a59-147">You must pass hello following query parameters for account management operations.</span></span>

* <span data-ttu-id="92a59-148">**åtgärden**: identifierar vilken typ av begäran om delegering (ChangePassword, ChangeProfile eller CloseAccount)</span><span class="sxs-lookup"><span data-stu-id="92a59-148">**operation**: identifies what type of delegation request it is (ChangePassword, ChangeProfile, or CloseAccount)</span></span>
* <span data-ttu-id="92a59-149">**användar-ID**: hello användar-id för hello konto toomanage</span><span class="sxs-lookup"><span data-stu-id="92a59-149">**userId**: hello user id of hello account toomanage</span></span>
* <span data-ttu-id="92a59-150">**salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet</span><span class="sxs-lookup"><span data-stu-id="92a59-150">**salt**: a special salt string used for computing a security hash</span></span>
* <span data-ttu-id="92a59-151">**sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde</span><span class="sxs-lookup"><span data-stu-id="92a59-151">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>

## <span data-ttu-id="92a59-152"><a name="delegate-product-subscription"></a>Delegera produkten prenumeration</span><span class="sxs-lookup"><span data-stu-id="92a59-152"><a name="delegate-product-subscription"> </a>Delegating product subscription</span></span>
<span data-ttu-id="92a59-153">Delegera produkten prenumeration fungerar på samma sätt toodelegating användaren logga in/ut.</span><span class="sxs-lookup"><span data-stu-id="92a59-153">Delegating product subscription works similarly toodelegating user sign-in/-up.</span></span> <span data-ttu-id="92a59-154">hello slutliga arbetsflöde skulle vara följande:</span><span class="sxs-lookup"><span data-stu-id="92a59-154">hello final workflow would be as follows:</span></span>

1. <span data-ttu-id="92a59-155">Utvecklare väljer en produkt i hello API Management developer-portalen och klickar på knappen för hello prenumerera</span><span class="sxs-lookup"><span data-stu-id="92a59-155">Developer selects a product in hello API Management developer portal and clicks on hello Subscribe button</span></span>
2. <span data-ttu-id="92a59-156">Webbläsaren är omdirigerade toohello delegering slutpunkt</span><span class="sxs-lookup"><span data-stu-id="92a59-156">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="92a59-157">Delegering endpoint utför nödvändiga produkten prenumeration steg – detta är tooyou och kan medföra att tooanother sidan toorequest faktureringsinformation, frågar andra frågor eller bara lagra hello information och inte kräver någon åtgärd från användarens sida</span><span class="sxs-lookup"><span data-stu-id="92a59-157">Delegation endpoint performs required product subscription steps - this is up tooyou and may entail redirecting tooanother page toorequest billing information, asking additional questions, or simply storing hello information and not requiring any user action</span></span>

<span data-ttu-id="92a59-158">tooenable hello funktionalitet på hello **delegering** klickar **delegera produkten prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="92a59-158">tooenable hello functionality, on hello **Delegation** page click **Delegate product subscription**.</span></span>

<span data-ttu-id="92a59-159">Kontrollera sedan hello delegering endpoint utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="92a59-159">Then ensure hello delegation endpoint performs hello following actions:</span></span>

1. <span data-ttu-id="92a59-160">Ta emot en begäran i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="92a59-160">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="92a59-161">*http://www.yourwebsite.com/apimdelegation?operation= {åtgärd} & productId = {produkten toosubscribe till} & userId = {användare som gjort begäran} & salt = {sträng} & sig = {sträng}*</span><span class="sxs-lookup"><span data-stu-id="92a59-161">*http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product toosubscribe to}&userId={user making request}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="92a59-162">Frågeparametrar Hej produkten prenumeration fall:</span><span class="sxs-lookup"><span data-stu-id="92a59-162">Query parameters for hello product subscription case:</span></span>
   
   * <span data-ttu-id="92a59-163">**åtgärden**: identifierar vilken typ av begäran om delegering.</span><span class="sxs-lookup"><span data-stu-id="92a59-163">**operation**: identifies what type of delegation request it is.</span></span> <span data-ttu-id="92a59-164">För produkten prenumeration är begäranden hello giltiga alternativ:</span><span class="sxs-lookup"><span data-stu-id="92a59-164">For product subscription requests hello valid options are:</span></span>
     * <span data-ttu-id="92a59-165">”Prenumerera”: en begäran toosubscribe hello användaren tooa angivna produkt med angett ID (se nedan)</span><span class="sxs-lookup"><span data-stu-id="92a59-165">"Subscribe": a request toosubscribe hello user tooa given product with provided ID (see below)</span></span>
     * <span data-ttu-id="92a59-166">”Avbryta prenumerationen”: en begäran toounsubscribe en användare från en produkt</span><span class="sxs-lookup"><span data-stu-id="92a59-166">"Unsubscribe": a request toounsubscribe a user from a product</span></span>
     * <span data-ttu-id="92a59-167">”Förnya”: en requst toorenew en prenumeration (t.ex. Det kan ut)</span><span class="sxs-lookup"><span data-stu-id="92a59-167">"Renew": a requst toorenew a subscription (e.g. that may be expiring)</span></span>
   * <span data-ttu-id="92a59-168">**productId**: hello-ID för hello produkten hello användare begärde toosubscribe till</span><span class="sxs-lookup"><span data-stu-id="92a59-168">**productId**: hello ID of hello product hello user requested toosubscribe to</span></span>
   * <span data-ttu-id="92a59-169">**användar-ID**: hello-ID för hello användare för vilka hello begäran har gjorts</span><span class="sxs-lookup"><span data-stu-id="92a59-169">**userId**: hello ID of hello user for whom hello request is made</span></span>
   * <span data-ttu-id="92a59-170">**salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet</span><span class="sxs-lookup"><span data-stu-id="92a59-170">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="92a59-171">**sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde</span><span class="sxs-lookup"><span data-stu-id="92a59-171">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="92a59-172">Kontrollera att hello förfrågan kommer från Azure API Management (valfritt, men rekommenderas för säkerhet)</span><span class="sxs-lookup"><span data-stu-id="92a59-172">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="92a59-173">Beräkna en HMAC SHA512 av en sträng som baseras på hello **productId**, **userId** och **salt** fråga parametrar:</span><span class="sxs-lookup"><span data-stu-id="92a59-173">Compute an HMAC-SHA512 of a string based on hello **productId**, **userId** and **salt** query parameters:</span></span>
     
     > <span data-ttu-id="92a59-174">HMAC (**salt** + ”\n” + **productId** + ”\n” + **userId**)</span><span class="sxs-lookup"><span data-stu-id="92a59-174">HMAC(**salt** + '\n' + **productId** + '\n' + **userId**)</span></span>
     > 
     > 
   * <span data-ttu-id="92a59-175">Jämför hello ovan beräknad toohello hashvärde hello **sig** Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="92a59-175">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="92a59-176">Om hello två hash-värdena matchar vidare toohello nästa steg, annars neka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="92a59-176">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="92a59-177">Utföra någon prenumeration bearbetning utifrån hello typ av åtgärd som angavs i **åtgärden** -t.ex. fakturering, ytterligare frågor, osv.</span><span class="sxs-lookup"><span data-stu-id="92a59-177">Perform any product subscription processing based on hello type of operation requested in **operation** - e.g. billing, further questions, etc.</span></span>
4. <span data-ttu-id="92a59-178">Prenumerera på har prenumerera hello användaren toohello produkten på din sida, hello användaren toohello API Management produkten av [anropa hello REST API för produkten prenumeration].</span><span class="sxs-lookup"><span data-stu-id="92a59-178">On successfully subscribing hello user toohello product on your side, subscribe hello user toohello API Management product by [calling hello REST API for product subscription].</span></span>

## <span data-ttu-id="92a59-179"><a name="delegate-example-code"></a> Exempelkod</span><span class="sxs-lookup"><span data-stu-id="92a59-179"><a name="delegate-example-code"> </a> Example Code</span></span>
<span data-ttu-id="92a59-180">Dessa code exempel visar hur tootake hello *delegering valideringsnyckel*, som har angetts i hello delegering skärmen i hello publisher portal, toocreate en HMAC som sedan används toovalidate hello signatur, vilket bevisar hello giltighet hello skickade returnUrl.</span><span class="sxs-lookup"><span data-stu-id="92a59-180">These code samples show how tootake hello *delegation validation key*, which is set in hello Delegation screen of hello publisher portal, toocreate a HMAC which is then used toovalidate hello signature, proving hello validity of hello passed returnUrl.</span></span> <span data-ttu-id="92a59-181">hello samma kod fungerar i hello productId och användar-ID med små ändringar.</span><span class="sxs-lookup"><span data-stu-id="92a59-181">hello same code works for hello productId and userId with slight modification.</span></span>

<span data-ttu-id="92a59-182">**C#-kod toogenerate-hash för returnUrl**</span><span class="sxs-lookup"><span data-stu-id="92a59-182">**C# code toogenerate hash of returnUrl**</span></span>

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

<span data-ttu-id="92a59-183">**NodeJS kod toogenerate hash för returnUrl**</span><span class="sxs-lookup"><span data-stu-id="92a59-183">**NodeJS code toogenerate hash of returnUrl**</span></span>

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a><span data-ttu-id="92a59-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92a59-184">Next steps</span></span>
<span data-ttu-id="92a59-185">Mer information om delegering finns i följande video hello.</span><span class="sxs-lookup"><span data-stu-id="92a59-185">For more information on delegation, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[begär en enkel inloggning (SSO) token]: http://go.microsoft.com/fwlink/?LinkId=507409
[Skapa en användare]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[anropa hello REST API för produkten prenumeration]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[exempelkoden nedan]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
