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
## <a name="use-hello-microsoft-authentication-library-msal-toosign-in-hello-user"></a>Använd hello Microsoft Authentication Library (MSAL) toosign i hello användare

1.  Skapa en fil med namnet `app.js`. Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklicka och välj: `Add`  >  `New Item`  >  `JavaScript File`:
2.  Lägg till följande kod tooyour hello `app.js` fil:

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
### <a name="more-information"></a>Mer information

När en användare klickar på hello *'anropa Microsoft Graph API-* knapp för hello första gången `callGraphApi` metodanrop `loginRedirect` toosign i hello användaren. Den här metoden innebär att omdirigera hello användaren toohello *Microsoft Azure Active Directory v2 endpoint* tooprompt och validera hello användarens autentiseringsuppgifter. På grund av en lyckad inloggning, hello användaren är omdirigerade tillbaka toohello ursprungliga *index.html* sidan och en token tas emot ska bearbetas av `msal.js` och hello informationen i hello token cachelagras. Denna token kallas hello *ID token* och innehåller grundläggande information om hello användare, till exempel hello användarens visningsnamn. Om du planerar toouse utfärdades alla data som anges av den här variabeln för andra syften måste toomake till denna token har verifierats av dina backend-servern tooguarantee som hello token tooa giltig användare för programmet.

hello SPA som genererats av den här guiden gör inte använda direkt hello-ID-token – i stället anropas `acquireTokenSilent` och/eller `acquireTokenRedirect` tooacquire en *åtkomsttoken* används tooquery hello Microsoft Graph API. Om du behöver ett exempel som validerar hello-ID-token kan ta en titt på [detta](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 exempel") exempelprogrammet i GitHub – hello används en ASP.NET Web API för token verifiering.

#### <a name="getting-a-user-token-interactively"></a>Hämta token för en användare interaktivt

Efter hello ursprunglig logga in, vill du inte hello be användare tooreauthenticate varje gång de behöver toorequest en token tooaccess en resurs – så *acquireTokenSilent* ska vara används för de flesta av hello tid tooacquire token. Det finns situationer men du behöver tooforce användarna samverkar med Azure Active Directory v2 endpoint – några exempel:
-   Användare kan behöva tooreenter sina autentiseringsuppgifter eftersom hello lösenord har upphört att gälla
-   Ditt program begär åtkomst tooa resurs som hello användaren måste tooconsent till
-   Tvåfaktorsautentisering krävs

Anropar hello *acquireTokenRedirect(scope)* resultera i att omdirigera användare toohello Azure Active Directory-v2-slutpunkt (eller *acquireTokenPopup(scope)* resultatet i ett popup-fönster) där användarna måste toointeract med genom att antingen bekräfta sina autentiseringsuppgifter, som ger hello medgivande toohello krävs resurs eller Slutför hello två Multi-Factor authentication.

#### <a name="getting-a-user-token-silently"></a>Hämta token för en användare tyst
Hej ` acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion. Efter `loginRedirect` (eller `loginPopup`) för hello körs första gången `acquireTokenSilent` hello metod som används ofta tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.
`acquireTokenSilent`kan misslyckas i vissa fall – till exempel hello användarens lösenord har upphört att gälla. Programmet kan hantera det här undantaget på två sätt:

1.  Gör ett anrop för`acquireTokenRedirect` omedelbart, vilket resulterar i att fråga hello användaren toosign i. Det här mönstret är vanligt i Onlineprogram där det finns inget oautentiserade innehåll i hello tillgängliga toohello programanvändare. hello används genereras av den här interaktiv installation det här mönstret.

2. Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `acquireTokenSilent` vid ett senare tillfälle. Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns oautentiserade innehåll i programmet hello. I det här fallet kan hello användare bestämma när de vill toosign i tooaccess hello skyddade resurser eller toorefresh hello gammal information.

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Anropa hello Microsoft Graph API använder du bara fick hello-token

Lägg till följande kod tooyour hello `app.js` fil:

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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Mer information om hur du skapar ett REST-anrop mot ett skyddade API

I hello exempelprogram som skapats av den här guiden, hello `callWebApiWithToken()` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare. Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*. Hello-exempelprogram som skapats av den här guiden, hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.

<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a>Lägg till en metod toosign ut hello användare

Lägg till följande kod tooyour hello `app.js` fil:

```javascript
/**
 * Sign-out hello user
 */
function signOut() {
    userAgentApplication.logout();
}
```
