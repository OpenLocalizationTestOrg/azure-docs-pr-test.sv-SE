---
title: "Azure AD v2 JS SPA interaktiv installation - Använd | Microsoft Docs"
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
ms.openlocfilehash: f52157df298ddfc1c1b29a18dc9a54aae59b52a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a><span data-ttu-id="c1d40-103">Använd Microsoft Authentication Library (MSAL) för att logga in användaren</span><span class="sxs-lookup"><span data-stu-id="c1d40-103">Use the Microsoft Authentication Library (MSAL) to sign-in the user</span></span>

1.  <span data-ttu-id="c1d40-104">Skapa en fil med namnet `app.js`.</span><span class="sxs-lookup"><span data-stu-id="c1d40-104">Create a file named `app.js`.</span></span> <span data-ttu-id="c1d40-105">Om du använder Visual Studio, Välj projekt (rotmapp projekt), högerklicka och välj: `Add`  >  `New Item`  >  `JavaScript File`:</span><span class="sxs-lookup"><span data-stu-id="c1d40-105">If you are using Visual Studio, select the project (project root folder), right click and select: `Add` > `New Item` > `JavaScript File`:</span></span>
2.  <span data-ttu-id="c1d40-106">Lägg till följande kod i din `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="c1d40-106">Add the following code to your `app.js` file:</span></span>

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
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
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from login
 * @param {object} error - The error string
 * @param {string} tokenType - the token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
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
### <a name="more-information"></a><span data-ttu-id="c1d40-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="c1d40-107">More Information</span></span>

<span data-ttu-id="c1d40-108">När en användare klickar på den *'anropa Microsoft Graph API-* knappen för första gången `callGraphApi` metodanrop `loginRedirect` att logga in användaren.</span><span class="sxs-lookup"><span data-stu-id="c1d40-108">After a user clicks the *‘Call Microsoft Graph API’* button for the first time, `callGraphApi` method calls `loginRedirect` to sign in the user.</span></span> <span data-ttu-id="c1d40-109">Den här metoden innebär att omdirigera användare till den *Microsoft Azure Active Directory v2 endpoint* till Kommandotolken och verifiera användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c1d40-109">This method results in redirecting the user to the *Microsoft Azure Active Directory v2 endpoint* to prompt and validate the user's credentials.</span></span> <span data-ttu-id="c1d40-110">På grund av en lyckad inloggning, omdirigeras användaren till ursprungligt *index.html* sidan och en token tas emot ska bearbetas av `msal.js` och den information som finns i token cachelagras.</span><span class="sxs-lookup"><span data-stu-id="c1d40-110">As a result of a successful sign-in, the user is redirected back to the original *index.html* page, and a token is received, processed by `msal.js` and the information contained in the token is cached.</span></span> <span data-ttu-id="c1d40-111">Denna token kallas den *ID token* och innehåller grundläggande information om användare, till exempel användarens visningsnamn.</span><span class="sxs-lookup"><span data-stu-id="c1d40-111">This token is known as the *ID token* and contains basic information about the user, such as the user display name.</span></span> <span data-ttu-id="c1d40-112">Om du planerar att använda några data som tillhandahålls av denna token för andra syften måste du kontrollera att denna token har verifierats av backend-servern för att garantera att token har utfärdats till en giltig användare för ditt program.</span><span class="sxs-lookup"><span data-stu-id="c1d40-112">If you plan to use any data provided by this token for any purposes, you need to make sure this token is validated by your backend server to guarantee that the token was issued to a valid user for your application.</span></span>

<span data-ttu-id="c1d40-113">SPA som genererats av den här guiden gör inte användas direkt av ID-token – i stället anropas `acquireTokenSilent` och/eller `acquireTokenRedirect` att erhålla ett *åtkomsttoken* frågar Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="c1d40-113">The SPA generated by this guide does not make use directly of the ID token – instead, it calls `acquireTokenSilent` and/or `acquireTokenRedirect` to acquire an *access token* used to query the Microsoft Graph API.</span></span> <span data-ttu-id="c1d40-114">Om du behöver ett exempel som validerar ID-token kan ta en titt på [detta](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 exempel") exempelprogrammet i GitHub-exemplet använder en ASP .NET web API för token verifiering.</span><span class="sxs-lookup"><span data-stu-id="c1d40-114">If you need a sample that validates the ID token, take a look at [this](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample") sample application in GitHub – the sample uses an ASP.NET Web API for token validation.</span></span>

#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="c1d40-115">Hämta token för en användare interaktivt</span><span class="sxs-lookup"><span data-stu-id="c1d40-115">Getting a user token interactively</span></span>

<span data-ttu-id="c1d40-116">Efter den första inloggningen, du inte vill be användare att autentiseras varje gång de behöver för att begära en token för att komma åt en resurs – så *acquireTokenSilent* ska användas för de flesta fall för att hämta token.</span><span class="sxs-lookup"><span data-stu-id="c1d40-116">After the initial sign-in, you do not want the ask users to reauthenticate every time they need to request a token to access a resource – so *acquireTokenSilent* should be used most of the time to acquire tokens.</span></span> <span data-ttu-id="c1d40-117">Det finns situationer men som behövs för att tvinga användare interagerar med Azure Active Directory v2 slutpunkt – några exempel:</span><span class="sxs-lookup"><span data-stu-id="c1d40-117">There are situations however that you need to force users interact with Azure Active Directory v2 endpoint – some examples include:</span></span>
-   <span data-ttu-id="c1d40-118">Användare kan behöva ange sina autentiseringsuppgifter på eftersom lösenordet har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="c1d40-118">Users may need to reenter their credentials because the password has expired</span></span>
-   <span data-ttu-id="c1d40-119">Ditt program begär åtkomst till en resurs som användaren behöver samtycker till att</span><span class="sxs-lookup"><span data-stu-id="c1d40-119">Your application is requesting access to a resource that the user needs to consent to</span></span>
-   <span data-ttu-id="c1d40-120">Tvåfaktorsautentisering krävs</span><span class="sxs-lookup"><span data-stu-id="c1d40-120">Two factor authentication is required</span></span>

<span data-ttu-id="c1d40-121">Anropar den *acquireTokenRedirect(scope)* resultera i att omdirigera användare till Azure Active Directory v2 slutpunkten (eller *acquireTokenPopup(scope)* resultatet i ett popup-fönster) när användare behöver interagera med genom att antingen bekräfta sina autentiseringsuppgifter, ge medgivande till den begärda resursen eller Slutför tvåfaktorsautentisering de två.</span><span class="sxs-lookup"><span data-stu-id="c1d40-121">Calling the *acquireTokenRedirect(scope)* result in redirecting users to the Azure Active Directory v2 endpoint (or *acquireTokenPopup(scope)* result on a popup window) where users need to interact with by either confirming their credentials, giving the consent to the required resource, or completing the two factor authentication.</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="c1d40-122">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="c1d40-122">Getting a user token silently</span></span>
<span data-ttu-id="c1d40-123">Den ` acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="c1d40-123">The ` acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="c1d40-124">Efter `loginRedirect` (eller `loginPopup`) körs för första gången `acquireTokenSilent` är den metod som används ofta för att hämta token som används för att komma åt skyddade resurser för efterföljande anrop - eftersom anrop till begära eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="c1d40-124">After `loginRedirect` (or `loginPopup`) is executed for the first time, `acquireTokenSilent` is the method commonly used to obtain tokens used to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>
<span data-ttu-id="c1d40-125">`acquireTokenSilent`misslyckas i vissa fall – till exempel användarens lösenord har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="c1d40-125">`acquireTokenSilent` may fail in some cases – for example, the user's password has expired.</span></span> <span data-ttu-id="c1d40-126">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="c1d40-126">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="c1d40-127">Gör ett anrop till `acquireTokenRedirect` direkt, vilket innebär att användaren uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="c1d40-127">Make a call to `acquireTokenRedirect` immediately, which results in prompting the user to sign in.</span></span> <span data-ttu-id="c1d40-128">Det här mönstret är vanligt i Onlineprogram där det finns inget oautentiserade innehåll i programmet tillgängligt för användaren.</span><span class="sxs-lookup"><span data-stu-id="c1d40-128">This pattern is commonly used in online applications where there is no unauthenticated content in the application available to the user.</span></span> <span data-ttu-id="c1d40-129">Genereras av den här interaktiv installation används det här mönstret.</span><span class="sxs-lookup"><span data-stu-id="c1d40-129">The sample generated by this guided setup uses this pattern.</span></span>

2. <span data-ttu-id="c1d40-130">Program kan också göra en indikering för användaren som en interaktiv inloggning krävs, så att användaren kan välja att logga in rätt tidpunkt eller programmet kan försöka `acquireTokenSilent` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="c1d40-130">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="c1d40-131">Detta används vanligtvis när användaren kan använda andra funktioner i programmet utan att något stör – t.ex, det finns oautentiserade innehåll i programmet.</span><span class="sxs-lookup"><span data-stu-id="c1d40-131">This is commonly used when the user can use other functionality of the application without being disrupted - for example, there is unauthenticated content available in the application.</span></span> <span data-ttu-id="c1d40-132">I det här fallet kan användaren välja när de vill logga in till den skydda resursen eller uppdatera inaktuell information.</span><span class="sxs-lookup"><span data-stu-id="c1d40-132">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information.</span></span>

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="c1d40-133">Anropa använder token som du precis har köpt Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="c1d40-133">Call the Microsoft Graph API using the token you just obtained</span></span>

<span data-ttu-id="c1d40-134">Lägg till följande kod i din `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="c1d40-134">Add the following code to your `app.js` file:</span></span>

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
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
                        // Display response in the page
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
                        // Display response as error in the page
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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="c1d40-135">Mer information om hur du skapar ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="c1d40-135">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="c1d40-136">I det exempelprogram som skapats av den här guiden, den `callWebApiWithToken()` metoden används för att en HTTP `GET` begäran mot en skyddad resurs som kräver ett token och returnera innehållet till anroparen.</span><span class="sxs-lookup"><span data-stu-id="c1d40-136">In the sample application created by this guide, the `callWebApiWithToken()` method is used to make an HTTP `GET` request against a protected resource that requires a token and then return the content to the caller.</span></span> <span data-ttu-id="c1d40-137">Den här metoden lägger till anskaffats token i den *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="c1d40-137">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="c1d40-138">För det exempelprogram som skapats av den här guiden, resursen är Microsoft Graph API *mig* slutpunkt – som visar information om användarens profil.</span><span class="sxs-lookup"><span data-stu-id="c1d40-138">For the sample application created by this guide, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a><span data-ttu-id="c1d40-139">Lägg till en metod för att logga ut användaren</span><span class="sxs-lookup"><span data-stu-id="c1d40-139">Add a method to sign out the user</span></span>

<span data-ttu-id="c1d40-140">Lägg till följande kod i din `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="c1d40-140">Add the following code to your `app.js` file:</span></span>

```javascript
/**
 * Sign-out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
