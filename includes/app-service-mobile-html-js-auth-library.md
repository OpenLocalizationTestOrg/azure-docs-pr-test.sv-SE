### <span data-ttu-id="bfdd8-101"><a name="server-auth"></a>Gör så här för att: autentisera med en provider (Server Flow)</span><span class="sxs-lookup"><span data-stu-id="bfdd8-101"><a name="server-auth"></a>How to: Authenticate with a provider (Server Flow)</span></span>
<span data-ttu-id="bfdd8-102">För att använda Mobile Apps för att hantera autentiseringsprocessen i din app måste du registrera din app med din identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-102">To have Mobile Apps manage the authentication process in your app, you must register your app with your identity provider.</span></span> <span data-ttu-id="bfdd8-103">Sedan måste du konfigurera program-ID och -hemligheten som du fått av din provider i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-103">Then in your Azure App Service, you need to configure the application ID and secret provided by your provider.</span></span>
<span data-ttu-id="bfdd8-104">Mer information finns i självstudierna [Lägg till autentisering till din app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="bfdd8-104">For more information, see the tutorial [Add authentication to your app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span></span>

<span data-ttu-id="bfdd8-105">När du har registrerat din identitetsprovider anropar du `.login()`-metoden med namnet på din provider.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-105">Once you have registered your identity provider, call the `.login()` method with the name of your provider.</span></span> <span data-ttu-id="bfdd8-106">För att till exempel logga in med Facebook använder du följande kod:</span><span class="sxs-lookup"><span data-stu-id="bfdd8-106">For example, to login with Facebook use the following code:</span></span>

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

<span data-ttu-id="bfdd8-107">Giltiga värden för providern är 'aad', 'facebook', 'google', 'microsoftaccount' och 'twitter'.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-107">The valid values for the provider are 'aad', 'facebook', 'google', 'microsoftaccount', and 'twitter'.</span></span>

> [!NOTE]
> <span data-ttu-id="bfdd8-108">Google-autentisering fungerar för närvarande inte via Server Flow.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-108">Google Authentication does not currently work via Server Flow.</span></span>  <span data-ttu-id="bfdd8-109">Om du vill autentisera med Google måste du använda en [klientflödesmetod](#client-auth).</span><span class="sxs-lookup"><span data-stu-id="bfdd8-109">To authenticate with Google, you must use a [client-flow method](#client-auth).</span></span>

<span data-ttu-id="bfdd8-110">I det här fallet hanterar Azure App Service OAuth 2.0-autentiseringsflödet.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-110">In this case, Azure App Service manages the OAuth 2.0 authentication flow.</span></span>  <span data-ttu-id="bfdd8-111">Den visar inloggningssidan för den valda providern och genererar en App Service-autentiseringstoken efter genomförd inloggning med identitetsprovidern.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-111">It displays the login page of the selected provider and generates an App Service authentication token after successful login with the identity provider.</span></span> <span data-ttu-id="bfdd8-112">När inloggningsfunktionen är färdig returnerar den ett JSON-objekt som både visar användar-ID och App Service-autentiseringstoken i fälten userId respektive authenticationToken.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-112">The login function, when complete, returns a JSON object that exposes both the user ID and App Service authentication token in the userId and authenticationToken fields, respectively.</span></span> <span data-ttu-id="bfdd8-113">Den här token kan cachelagras och återanvändas tills den upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-113">This token can be cached and reused until it expires.</span></span>

###<span data-ttu-id="bfdd8-114"><a name="client-auth"></a>Gör så här för att: autentisera med en provider (Client Flow)</span><span class="sxs-lookup"><span data-stu-id="bfdd8-114"><a name="client-auth"></a>How to: Authenticate with a provider (Client Flow)</span></span>

<span data-ttu-id="bfdd8-115">Din app kan även oberoende kontakta identitetsprovidern och sedan tillhandahålla den returnerade token till App Service för autentisering.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-115">Your app can also independently contact the identity provider and then provide the returned token to your App Service for authentication.</span></span> <span data-ttu-id="bfdd8-116">Det här klientflödet gör det möjligt för dig att tillhandahålla enkel inloggning för användare eller hämta ytterligare användardata från identitetsprovidern.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-116">This client flow enables you to provide a single sign-in experience for users or to retrieve additional user data from the identity provider.</span></span>

#### <a name="social-authentication-basic-example"></a><span data-ttu-id="bfdd8-117">Grundläggande exempel på social autentisering</span><span class="sxs-lookup"><span data-stu-id="bfdd8-117">Social Authentication basic example</span></span>

<span data-ttu-id="bfdd8-118">I det här exemplet används Facebook-klientens SDK för autentisering:</span><span class="sxs-lookup"><span data-stu-id="bfdd8-118">This example uses Facebook client SDK for authentication:</span></span>

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
<span data-ttu-id="bfdd8-119">Det här exemplet förutsätter att den token som tillhandahålls av respektive provider-SDK lagras i token-variabeln.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-119">This example assumes that the token provided by the respective provider SDK is stored in the token variable.</span></span>

#### <a name="microsoft-account-example"></a><span data-ttu-id="bfdd8-120">Exempel med Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="bfdd8-120">Microsoft Account example</span></span>

<span data-ttu-id="bfdd8-121">Följande exempel använder Live SDK som har stöd för enkel inloggning för Windows Store-appar vid användning av Microsoft-konton:</span><span class="sxs-lookup"><span data-stu-id="bfdd8-121">The following example uses the Live SDK, which supports single-sign-on for Windows Store apps by using Microsoft Account:</span></span>

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});

```

<span data-ttu-id="bfdd8-122">I det här exemplet hämtas en token från Live Connect som sedan levereras till App Service genom att anropa inloggningsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-122">This example gets a token from Live Connect, which is supplied to your App Service by calling the login function.</span></span>

###<span data-ttu-id="bfdd8-123"><a name="auth-getinfo"></a>Gör så här för att: Få mer information om den autentiserade användaren</span><span class="sxs-lookup"><span data-stu-id="bfdd8-123"><a name="auth-getinfo"></a>How to: Obtain information about the authenticated user</span></span>

<span data-ttu-id="bfdd8-124">Autentiseringsinformationen kan hämtas från `/.auth/me`-slutpunkten med hjälp av HTTP-anrop med ett AJAX-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-124">The authentication information can be retrieved from the `/.auth/me` endpoint using an HTTP call with any AJAX library.</span></span>  <span data-ttu-id="bfdd8-125">Se till att du ställer in `X-ZUMO-AUTH`-rubriken till din autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-125">Ensure you set the `X-ZUMO-AUTH` header to your authentication token.</span></span>  <span data-ttu-id="bfdd8-126">Autentiseringstoken lagras i `client.currentUser.mobileServiceAuthenticationToken`.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-126">The authentication token is stored in `client.currentUser.mobileServiceAuthenticationToken`.</span></span>  <span data-ttu-id="bfdd8-127">För att till exempel använda Fetch API:</span><span class="sxs-lookup"><span data-stu-id="bfdd8-127">For example, to use the fetch API:</span></span>

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

<span data-ttu-id="bfdd8-128">Fetch är tillgängligt som [ett npm-paket](https://www.npmjs.com/package/whatwg-fetch) eller som nedladdning via en webbläsare från [CDNJS](https://cdnjs.com/libraries/fetch).</span><span class="sxs-lookup"><span data-stu-id="bfdd8-128">Fetch is available as [an npm package](https://www.npmjs.com/package/whatwg-fetch) or for browser download from [CDNJS](https://cdnjs.com/libraries/fetch).</span></span> <span data-ttu-id="bfdd8-129">Du kan även använda jQuery eller en annan AJAX API för att hämta informationen.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-129">You could also use jQuery or another AJAX API to fetch the information.</span></span>  <span data-ttu-id="bfdd8-130">Data tas emot som ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="bfdd8-130">Data is received as a JSON object.</span></span>
