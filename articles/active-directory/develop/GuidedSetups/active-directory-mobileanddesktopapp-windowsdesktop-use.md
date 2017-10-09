---
title: "aaaAzure AD v2 Windows Desktop komma igång - Använd | Microsoft Docs"
description: "Hur Windows Desktop .NET (XAML) program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: bb258fe5f523ec727ca02716fd823d853d3349b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="a4c43-103">Använd hello Microsoft Authentication Library (MSAL) tooget en token för hello Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="a4c43-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="a4c43-104">Det här avsnittet visar hur toouse MSAL tooget en token hello Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="a4c43-104">This section shows how toouse MSAL tooget a token hello Microsoft Graph API.</span></span>

1.  <span data-ttu-id="a4c43-105">I `MainWindow.xaml.cs`, Lägg till hello-referens för MSAL biblioteket toohello klass:</span><span class="sxs-lookup"><span data-stu-id="a4c43-105">In `MainWindow.xaml.cs`, add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="a4c43-106">Ersätt <code>MainWindow</code> klassen kod med:</span><span class="sxs-lookup"><span data-stu-id="a4c43-106">Replace <code>MainWindow</code> class code with:</span></span>
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set hello API Endpoint tooGraph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set hello scope for API call toouser.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - tooacquire a token requiring user toosign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need toocall AcquireTokenAsync tooacquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="a4c43-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="a4c43-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="a4c43-108">Hämta en användartoken interaktiva</span><span class="sxs-lookup"><span data-stu-id="a4c43-108">Getting a user token interactive</span></span>
<span data-ttu-id="a4c43-109">Anropar hello `AcquireTokenAsync` metoden resulterar i ett fönster som uppmanar hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="a4c43-109">Calling hello `AcquireTokenAsync` method results in a window prompting hello user toosign in.</span></span> <span data-ttu-id="a4c43-110">Program kräver vanligen en användare toosign i interaktivt hello första gången som de behöver tooaccess en skyddad resurs eller när en tyst åtgärden tooacquire en token misslyckas (t.ex. hello användarens lösenord har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="a4c43-110">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="a4c43-111">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="a4c43-111">Getting a user token silently</span></span>
<span data-ttu-id="a4c43-112">`AcquireTokenSilentAsync`hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="a4c43-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="a4c43-113">Efter `AcquireTokenAsync` för hello körs första gången `AcquireTokenSilentAsync` hello vanliga metod som används för tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="a4c43-113">After `AcquireTokenAsync` is executed for hello first time, `AcquireTokenSilentAsync` is hello usual method used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="a4c43-114">Slutligen `AcquireTokenSilentAsync` misslyckas – t.ex. hello användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="a4c43-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="a4c43-115">När MSAL känner av att hello problemet kan lösas genom att kräva en interaktiv åtgärd den utlöses en `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="a4c43-115">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="a4c43-116">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a4c43-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="a4c43-117">Gör ett anrop mot `AcquireTokenAsync` omedelbart, vilket innebär att fråga användaren hello toosign i.</span><span class="sxs-lookup"><span data-stu-id="a4c43-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting hello user toosign-in.</span></span> <span data-ttu-id="a4c43-118">Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i hello program tillgängliga för hello användare.</span><span class="sxs-lookup"><span data-stu-id="a4c43-118">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="a4c43-119">hello exemplet skapas vid interaktiv installationen använder det här mönstret: du kan se den i åtgärden hello första gången du kör hello exempel: eftersom ingen användare har använt hello programmet `PublicClientApp.Users.FirstOrDefault()` innehåller ett null-värde och ett `MsalUiRequiredException` undantag.</span><span class="sxs-lookup"><span data-stu-id="a4c43-119">hello sample generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello sample: because no user ever used hello application, `PublicClientApp.Users.FirstOrDefault()` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="a4c43-120">Hej koden i hello exempel sedan handtag hello undantag genom att anropa `AcquireTokenAsync` vilket resulterar i att fråga användaren hello toosign i.</span><span class="sxs-lookup"><span data-stu-id="a4c43-120">hello code in hello sample then handles hello exception by calling `AcquireTokenAsync` resulting in prompting hello user toosign-in.</span></span>

2.  <span data-ttu-id="a4c43-121">Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `AcquireTokenSilentAsync` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="a4c43-121">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="a4c43-122">Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns innehåll offline i hello program.</span><span class="sxs-lookup"><span data-stu-id="a4c43-122">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="a4c43-123">I det här fallet hello användare kan bestämma när de vill toosign i tooaccess hello skyddade resursen toorefresh hello inaktuell information eller ditt program kan bestämma tooretry `AcquireTokenSilentAsync` när nätverket återställs efter att ha tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="a4c43-123">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="a4c43-124">Anropa hello Microsoft Graph API använder du bara fick hello-token</span><span class="sxs-lookup"><span data-stu-id="a4c43-124">Call hello Microsoft Graph API using hello token you just obtained</span></span>

1. <span data-ttu-id="a4c43-125">Lägga till nya hello-metoden nedan tooyour `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="a4c43-125">Add hello new method below tooyour `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="a4c43-126">hello metoden är används toomake en `GET` begäran mot Graph API med hjälp av ett auktorisera-huvud:</span><span class="sxs-lookup"><span data-stu-id="a4c43-126">hello method is used toomake a `GET` request against Graph API using an Authorize header:</span></span>

```csharp
/// <summary>
/// Perform an HTTP GET request tooa URL using an HTTP Authorization header
/// </summary>
/// <param name="url">hello URL</param>
/// <param name="token">hello token</param>
/// <returns>String containing hello results of hello GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add hello token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="a4c43-127">Mer information om hur du skapar ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="a4c43-127">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="a4c43-128">I det här exempelprogrammet hello `GetHttpContentWithToken` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare.</span><span class="sxs-lookup"><span data-stu-id="a4c43-128">In this sample application, hello `GetHttpContentWithToken` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="a4c43-129">Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="a4c43-129">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="a4c43-130">För det här exemplet hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.</span><span class="sxs-lookup"><span data-stu-id="a4c43-130">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="a4c43-131">Lägg till en metod toosign ut hello användare</span><span class="sxs-lookup"><span data-stu-id="a4c43-131">Add a method toosign out hello user</span></span>

1. <span data-ttu-id="a4c43-132">Lägg till följande metod tooyour hello `MainWindow.xaml.cs` toosign ut hello användare:</span><span class="sxs-lookup"><span data-stu-id="a4c43-132">Add hello following method tooyour `MainWindow.xaml.cs` toosign out hello user:</span></span>

```csharp
/// <summary>
/// Sign out hello current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a><span data-ttu-id="a4c43-133">Mer information om utloggning</span><span class="sxs-lookup"><span data-stu-id="a4c43-133">More info on Sign-Out</span></span>

<span data-ttu-id="a4c43-134">`SignOutButton_Click`tar bort hello användare från MSAL användarcachen – detta talar effektivt MSAL tooforget hello aktuell användare så att en begäran om framtida tooacquire en token lyckas bara om det görs toobe interaktiva.</span><span class="sxs-lookup"><span data-stu-id="a4c43-134">`SignOutButton_Click` removes hello user from MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>
<span data-ttu-id="a4c43-135">Hello-programmet i det här exemplet stöder en enskild användare, MSAL har stöd för scenarier där flera konton kan vara inloggad på hello samtidigt – ett exempel är ett e-postprogram där en användare har flera konton.</span><span class="sxs-lookup"><span data-stu-id="a4c43-135">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="display-basic-token-information"></a><span data-ttu-id="a4c43-136">Visa grundläggande Information om Token</span><span class="sxs-lookup"><span data-stu-id="a4c43-136">Display Basic Token Information</span></span>

1. <span data-ttu-id="a4c43-137">Lägg till följande metod tootooyour hello `MainWindow.xaml.cs` toodisplay grundläggande information om hello token:</span><span class="sxs-lookup"><span data-stu-id="a4c43-137">Add hello following method tootooyour `MainWindow.xaml.cs` toodisplay basic information about hello token:</span></span>

```csharp
/// <summary>
/// Display basic information contained in hello token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="a4c43-138">Mer information</span><span class="sxs-lookup"><span data-stu-id="a4c43-138">More Information</span></span>

<span data-ttu-id="a4c43-139">Token har köpt *OpenID Connect* också innehålla en liten del av information om relevant toohello användare.</span><span class="sxs-lookup"><span data-stu-id="a4c43-139">Tokens acquired via *OpenID Connect* also contain a small subset of information pertinent toohello user.</span></span> <span data-ttu-id="a4c43-140">`DisplayBasicTokenInfo`Visar grundläggande information som finns i hello token: till exempel hello användare visar namn och ID, samt hello giltighetstid för token datum och hello sträng som representerar hello åtkomsttoken sig själv.</span><span class="sxs-lookup"><span data-stu-id="a4c43-140">`DisplayBasicTokenInfo` displays basic information contained in hello token: for example, hello user's display name and ID, as well as hello token expiration date and hello string representing hello access token itself.</span></span> <span data-ttu-id="a4c43-141">Den här informationen visas för du toosee.</span><span class="sxs-lookup"><span data-stu-id="a4c43-141">This information is displayed for you toosee.</span></span> <span data-ttu-id="a4c43-142">Du kan träffa hello *anropa Microsoft Graph API* knappen flera gånger och se den hello samma token återanvänts för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="a4c43-142">You can hit hello *Call Microsoft Graph API* button multiple times and see that hello same token was reused for subsequent requests.</span></span> <span data-ttu-id="a4c43-143">Du kan också se hello förfallodatum förlängs när MSAL bestämmer det är tid toorenew hello token.</span><span class="sxs-lookup"><span data-stu-id="a4c43-143">You can also see hello expiration date being extended when MSAL decides it is time toorenew hello token.</span></span>
<!--end-collapse-->

