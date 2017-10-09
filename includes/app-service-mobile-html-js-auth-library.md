### <span data-ttu-id="20238-101"><a name="server-auth"></a>Gör så här för att: autentisera med en provider (Server Flow)</span><span class="sxs-lookup"><span data-stu-id="20238-101"><a name="server-auth"></a>How to: Authenticate with a provider (Server Flow)</span></span>
<span data-ttu-id="20238-102">toohave Mobile Apps hantera hello autentiseringsprocessen i din app, måste du registrera din app med din identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="20238-102">toohave Mobile Apps manage hello authentication process in your app, you must register your app with your identity provider.</span></span> <span data-ttu-id="20238-103">I din Azure App Service måste tooconfigure hello program-ID och hemlighet som tillhandahålls av leverantören.</span><span class="sxs-lookup"><span data-stu-id="20238-103">Then in your Azure App Service, you need tooconfigure hello application ID and secret provided by your provider.</span></span>
<span data-ttu-id="20238-104">Mer information finns i självstudiekursen hello [Lägg till autentisering tooyour app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="20238-104">For more information, see hello tutorial [Add authentication tooyour app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span></span>

<span data-ttu-id="20238-105">När du har registrerat din identitetsleverantör anropa hello `.login()` metod med hello namnet på leverantören.</span><span class="sxs-lookup"><span data-stu-id="20238-105">Once you have registered your identity provider, call hello `.login()` method with hello name of your provider.</span></span> <span data-ttu-id="20238-106">Till exempel använda toologin med Facebook hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="20238-106">For example, toologin with Facebook use hello following code:</span></span>

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

<span data-ttu-id="20238-107">hello giltiga värden för hello-providern är 'aad', 'facebook', 'google', 'microsoftaccount' och 'twitter'.</span><span class="sxs-lookup"><span data-stu-id="20238-107">hello valid values for hello provider are 'aad', 'facebook', 'google', 'microsoftaccount', and 'twitter'.</span></span>

> [!NOTE]
> <span data-ttu-id="20238-108">Google-autentisering fungerar för närvarande inte via Server Flow.</span><span class="sxs-lookup"><span data-stu-id="20238-108">Google Authentication does not currently work via Server Flow.</span></span>  <span data-ttu-id="20238-109">tooauthenticate med Google, måste du använda en [-flödet metoden](#client-auth).</span><span class="sxs-lookup"><span data-stu-id="20238-109">tooauthenticate with Google, you must use a [client-flow method](#client-auth).</span></span>

<span data-ttu-id="20238-110">I detta fall hanterar Azure App Service hello OAuth 2.0-autentiseringsflöde.</span><span class="sxs-lookup"><span data-stu-id="20238-110">In this case, Azure App Service manages hello OAuth 2.0 authentication flow.</span></span>  <span data-ttu-id="20238-111">Den visar hello inloggningssidan för hello vald leverantör och genererar en Apptjänst-autentiseringstoken efter genomförd inloggning med hello identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="20238-111">It displays hello login page of hello selected provider and generates an App Service authentication token after successful login with hello identity provider.</span></span> <span data-ttu-id="20238-112">hello returneras inloggning, när du är klar, ett JSON-objekt som exponerar både hello användar-ID och Apptjänst autentiseringstoken i hello användar-ID- och authenticationToken respektive.</span><span class="sxs-lookup"><span data-stu-id="20238-112">hello login function, when complete, returns a JSON object that exposes both hello user ID and App Service authentication token in hello userId and authenticationToken fields, respectively.</span></span> <span data-ttu-id="20238-113">Den här token kan cachelagras och återanvändas tills den upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="20238-113">This token can be cached and reused until it expires.</span></span>

###<span data-ttu-id="20238-114"><a name="client-auth"></a>Gör så här för att: autentisera med en provider (Client Flow)</span><span class="sxs-lookup"><span data-stu-id="20238-114"><a name="client-auth"></a>How to: Authenticate with a provider (Client Flow)</span></span>

<span data-ttu-id="20238-115">Appen kan även oberoende kontakta hello identitetsleverantör och ange sedan hello returnerade token tooyour Apptjänst för autentisering.</span><span class="sxs-lookup"><span data-stu-id="20238-115">Your app can also independently contact hello identity provider and then provide hello returned token tooyour App Service for authentication.</span></span> <span data-ttu-id="20238-116">Det här flödet kan tooprovide en enkel inloggning för användare eller tooretrieve ytterligare användardata från hello identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="20238-116">This client flow enables you tooprovide a single sign-in experience for users or tooretrieve additional user data from hello identity provider.</span></span>

#### <a name="social-authentication-basic-example"></a><span data-ttu-id="20238-117">Grundläggande exempel på social autentisering</span><span class="sxs-lookup"><span data-stu-id="20238-117">Social Authentication basic example</span></span>

<span data-ttu-id="20238-118">I det här exemplet används Facebook-klientens SDK för autentisering:</span><span class="sxs-lookup"><span data-stu-id="20238-118">This example uses Facebook client SDK for authentication:</span></span>

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
<span data-ttu-id="20238-119">Det här exemplet förutsätter att hello-token under förutsättning av hello respektive providern SDK lagras i hello token variabeln.</span><span class="sxs-lookup"><span data-stu-id="20238-119">This example assumes that hello token provided by hello respective provider SDK is stored in hello token variable.</span></span>

#### <a name="microsoft-account-example"></a><span data-ttu-id="20238-120">Exempel med Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="20238-120">Microsoft Account example</span></span>

<span data-ttu-id="20238-121">följande exempel använder hello hello Live SDK, som stöder enkel inloggning för Windows Store-appar med hjälp av Account:</span><span class="sxs-lookup"><span data-stu-id="20238-121">hello following example uses hello Live SDK, which supports single-sign-on for Windows Store apps by using Microsoft Account:</span></span>

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

<span data-ttu-id="20238-122">Det här exemplet får en token från Live Connect som är angivna tooyour Apptjänst genom att anropa hello inloggningen funktion.</span><span class="sxs-lookup"><span data-stu-id="20238-122">This example gets a token from Live Connect, which is supplied tooyour App Service by calling hello login function.</span></span>

###<span data-ttu-id="20238-123"><a name="auth-getinfo"></a>Så här: hämta information om hello autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="20238-123"><a name="auth-getinfo"></a>How to: Obtain information about hello authenticated user</span></span>

<span data-ttu-id="20238-124">hello autentiseringsinformation kan hämtas från hello `/.auth/me` slutpunkten med hjälp av en HTTP anropa med något AJAX-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="20238-124">hello authentication information can be retrieved from hello `/.auth/me` endpoint using an HTTP call with any AJAX library.</span></span>  <span data-ttu-id="20238-125">Se till att du ställer in hello `X-ZUMO-AUTH` huvud tooyour autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="20238-125">Ensure you set hello `X-ZUMO-AUTH` header tooyour authentication token.</span></span>  <span data-ttu-id="20238-126">hello autentiseringstoken lagras i `client.currentUser.mobileServiceAuthenticationToken`.</span><span class="sxs-lookup"><span data-stu-id="20238-126">hello authentication token is stored in `client.currentUser.mobileServiceAuthenticationToken`.</span></span>  <span data-ttu-id="20238-127">Till exempel toouse hello fetch API:</span><span class="sxs-lookup"><span data-stu-id="20238-127">For example, toouse hello fetch API:</span></span>

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // hello user object contains hello claims for hello authenticated user
    });
```

<span data-ttu-id="20238-128">Fetch är tillgängligt som [ett npm-paket](https://www.npmjs.com/package/whatwg-fetch) eller som nedladdning via en webbläsare från [CDNJS](https://cdnjs.com/libraries/fetch).</span><span class="sxs-lookup"><span data-stu-id="20238-128">Fetch is available as [an npm package](https://www.npmjs.com/package/whatwg-fetch) or for browser download from [CDNJS](https://cdnjs.com/libraries/fetch).</span></span> <span data-ttu-id="20238-129">Du kan också använda jQuery eller en annan AJAX API toofetch hello information.</span><span class="sxs-lookup"><span data-stu-id="20238-129">You could also use jQuery or another AJAX API toofetch hello information.</span></span>  <span data-ttu-id="20238-130">Data tas emot som ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="20238-130">Data is received as a JSON object.</span></span>
