---
title: "aaaAzure AD v2 JS SPA interaktiv installation - Använd | Microsoft Docs"
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 4f7f824ed787d998dc4aea3dc21c95d7dfe70ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-toosign-in-hello-user"></a><span data-ttu-id="f7c6c-103">Använd hello Microsoft Authentication Library (MSAL) toosign i hello användare</span><span class="sxs-lookup"><span data-stu-id="f7c6c-103">Use hello Microsoft Authentication Library (MSAL) toosign-in hello user</span></span>

1.  <span data-ttu-id="f7c6c-104">Skapa en fil med namnet `app.js`.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-104">Create a file named `app.js`.</span></span> <span data-ttu-id="f7c6c-105">Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklicka och välj: `Add`  >  `New Item`  >  `JavaScript File`:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-105">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `JavaScript File`:</span></span>
2.  <span data-ttu-id="f7c6c-106">Lägg till följande kod tooyour hello `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-106">Add hello following code tooyour `app.js` file:</span></span>

```javascript
// Graph API endpoint tooshow user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used tooobtain hello access token tooread user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue toodisplay user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call hello Microsoft Graph API and display hello results on hello page. Sign hello user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user toosign in via loginRedirect.
        // This will redirect user toohello Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // hello call toologinRedirect above frontloads hello consent tooquery Graph API during hello sign-in.
        // If you want toouse dynamic consent, just remove hello graphAPIScopes from loginRedirect call.
        // As such, user will be prompted toogive consent when requested access tooa resource that 
        // he/she hasn't consented before. In hello case of this application - 
        // hello first time hello Graph API call tooobtain user's profile is executed.
    } else {
        // If user is already signed in, display hello user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API tooshow hello user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order toocall hello Graph API, an access token needs toobe acquired.
        // Try tooacquire hello token used tooquery Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After hello access token is acquired, call hello Web API, sending hello acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If hello acquireTokenSilent() method fails, then acquire hello token interactively via acquireTokenRedirect().
                // In this case, hello browser will redirect user back toohello Azure Active Directory v2 Endpoint so hello user 
                // can reenter hello current username/ password and/ or give consent toonew permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get hello token silently, hello Graph API call results will be made and results will be displayed in hello page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() tooshow results.
 * @param {string} errorDesc - If error occur, hello error message
 * @param {object} token - hello token received from login
 * @param {object} error - hello error string
 * @param {string} tokenType - hello token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in hello page
 * @param {string} endpoint - hello endpoint used for hello error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="f7c6c-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="f7c6c-107">More Information</span></span>

<span data-ttu-id="f7c6c-108">När en användare klickar på hello *'anropa Microsoft Graph API-* knapp för hello första gången `callGraphApi` metodanrop `loginRedirect` toosign i hello användaren.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-108">After a user clicks hello *‘Call Microsoft Graph API’* button for hello first time, `callGraphApi` method calls `loginRedirect` toosign in hello user.</span></span> <span data-ttu-id="f7c6c-109">Den här metoden innebär att omdirigera hello användaren toohello *Microsoft Azure Active Directory v2 endpoint* tooprompt och validera hello användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-109">This method results in redirecting hello user toohello *Microsoft Azure Active Directory v2 endpoint* tooprompt and validate hello user's credentials.</span></span> <span data-ttu-id="f7c6c-110">På grund av en lyckad inloggning, hello användaren är omdirigerade tillbaka toohello ursprungliga *index.html* sidan och en token tas emot ska bearbetas av `msal.js` och hello informationen i hello token cachelagras.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-110">As a result of a successful sign-in, hello user is redirected back toohello original *index.html* page, and a token is received, processed by `msal.js` and hello information contained in hello token is cached.</span></span> <span data-ttu-id="f7c6c-111">Denna token kallas hello *ID token* och innehåller grundläggande information om hello användare, till exempel hello användarens visningsnamn.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-111">This token is known as hello *ID token* and contains basic information about hello user, such as hello user display name.</span></span> <span data-ttu-id="f7c6c-112">Om du planerar toouse utfärdades alla data som anges av den här variabeln för andra syften måste toomake till denna token har verifierats av dina backend-servern tooguarantee som hello token tooa giltig användare för programmet.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-112">If you plan toouse any data provided by this token for any purposes, you need toomake sure this token is validated by your backend server tooguarantee that hello token was issued tooa valid user for your application.</span></span>

<span data-ttu-id="f7c6c-113">hello SPA som genererats av den här guiden gör inte använda direkt hello-ID-token – i stället anropas `acquireTokenSilent` och/eller `acquireTokenRedirect` tooacquire en *åtkomsttoken* används tooquery hello Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-113">hello SPA generated by this guide does not make use directly of hello ID token – instead, it calls `acquireTokenSilent` and/or `acquireTokenRedirect` tooacquire an *access token* used tooquery hello Microsoft Graph API.</span></span> <span data-ttu-id="f7c6c-114">Om du behöver ett exempel som validerar hello-ID-token kan ta en titt på [detta](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 exempel") exempelprogrammet i GitHub – hello används en ASP.NET Web API för token verifiering.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-114">If you need a sample that validates hello ID token, take a look at [this](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample") sample application in GitHub – hello sample uses an ASP.NET Web API for token validation.</span></span>

#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="f7c6c-115">Hämta token för en användare interaktivt</span><span class="sxs-lookup"><span data-stu-id="f7c6c-115">Getting a user token interactively</span></span>

<span data-ttu-id="f7c6c-116">Efter hello ursprunglig logga in, vill du inte hello be användare tooreauthenticate varje gång de behöver toorequest en token tooaccess en resurs – så *acquireTokenSilent* ska vara används för de flesta av hello tid tooacquire token.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-116">After hello initial sign-in, you do not want hello ask users tooreauthenticate every time they need toorequest a token tooaccess a resource – so *acquireTokenSilent* should be used most of hello time tooacquire tokens.</span></span> <span data-ttu-id="f7c6c-117">Det finns situationer men du behöver tooforce användarna samverkar med Azure Active Directory v2 endpoint – några exempel:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-117">There are situations however that you need tooforce users interact with Azure Active Directory v2 endpoint – some examples include:</span></span>
-   <span data-ttu-id="f7c6c-118">Användare kan behöva tooreenter sina autentiseringsuppgifter eftersom hello lösenord har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="f7c6c-118">Users may need tooreenter their credentials because hello password has expired</span></span>
-   <span data-ttu-id="f7c6c-119">Ditt program begär åtkomst tooa resurs som hello användaren måste tooconsent till</span><span class="sxs-lookup"><span data-stu-id="f7c6c-119">Your application is requesting access tooa resource that hello user needs tooconsent to</span></span>
-   <span data-ttu-id="f7c6c-120">Tvåfaktorsautentisering krävs</span><span class="sxs-lookup"><span data-stu-id="f7c6c-120">Two factor authentication is required</span></span>

<span data-ttu-id="f7c6c-121">Anropar hello *acquireTokenRedirect(scope)* resultera i att omdirigera användare toohello Azure Active Directory-v2-slutpunkt (eller *acquireTokenPopup(scope)* resultatet i ett popup-fönster) där användarna måste toointeract med genom att antingen bekräfta sina autentiseringsuppgifter, som ger hello medgivande toohello krävs resurs eller Slutför hello två Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-121">Calling hello *acquireTokenRedirect(scope)* result in redirecting users toohello Azure Active Directory v2 endpoint (or *acquireTokenPopup(scope)* result on a popup window) where users need toointeract with by either confirming their credentials, giving hello consent toohello required resource, or completing hello two factor authentication.</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="f7c6c-122">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="f7c6c-122">Getting a user token silently</span></span>
<span data-ttu-id="f7c6c-123">Hej ` acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-123">hello ` acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="f7c6c-124">Efter `loginRedirect` (eller `loginPopup`) för hello körs första gången `acquireTokenSilent` hello metod som används ofta tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-124">After `loginRedirect` (or `loginPopup`) is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="f7c6c-125">`acquireTokenSilent`kan misslyckas i vissa fall – till exempel hello användarens lösenord har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-125">`acquireTokenSilent` may fail in some cases – for example, hello user's password has expired.</span></span> <span data-ttu-id="f7c6c-126">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-126">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="f7c6c-127">Gör ett anrop för`acquireTokenRedirect` omedelbart, vilket resulterar i att fråga hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-127">Make a call too`acquireTokenRedirect` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="f7c6c-128">Det här mönstret är vanligt i Onlineprogram där det finns inget oautentiserade innehåll i hello tillgängliga toohello programanvändare.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-128">This pattern is commonly used in online applications where there is no unauthenticated content in hello application available toohello user.</span></span> <span data-ttu-id="f7c6c-129">hello används genereras av den här interaktiv installation det här mönstret.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-129">hello sample generated by this guided setup uses this pattern.</span></span>

2. <span data-ttu-id="f7c6c-130">Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `acquireTokenSilent` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-130">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="f7c6c-131">Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns oautentiserade innehåll i programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-131">This is commonly used when hello user can use other functionality of hello application without being disrupted - for example, there is unauthenticated content available in hello application.</span></span> <span data-ttu-id="f7c6c-132">I det här fallet kan hello användare bestämma när de vill toosign i tooaccess hello skyddade resurser eller toorefresh hello gammal information.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-132">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="f7c6c-133">Anropa hello Microsoft Graph API använder du bara fick hello-token</span><span class="sxs-lookup"><span data-stu-id="f7c6c-133">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="f7c6c-134">Lägg till följande kod tooyour hello `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-134">Add hello following code tooyour `app.js` file:</span></span>

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used toodisplay hello results
 * @param {object} showTokenElement = HTML element used toodisplay hello RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in hello page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in hello page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="f7c6c-135">Mer information om hur du skapar ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="f7c6c-135">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="f7c6c-136">I hello exempelprogram som skapats av den här guiden, hello `callWebApiWithToken()` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-136">In hello sample application created by this guide, hello `callWebApiWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="f7c6c-137">Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-137">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="f7c6c-138">Hello-exempelprogram som skapats av den här guiden, hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.</span><span class="sxs-lookup"><span data-stu-id="f7c6c-138">For hello sample application created by this guide, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>

<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="f7c6c-139">Lägg till en metod toosign ut hello användare</span><span class="sxs-lookup"><span data-stu-id="f7c6c-139">Add a method toosign out hello user</span></span>

<span data-ttu-id="f7c6c-140">Lägg till följande kod tooyour hello `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="f7c6c-140">Add hello following code tooyour `app.js` file:</span></span>

```javascript
/**
 * Sign-out hello user
 */
function signOut() {
    userAgentApplication.logout();
}
```
