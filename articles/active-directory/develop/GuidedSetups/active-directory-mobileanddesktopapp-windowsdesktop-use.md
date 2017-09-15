---
title: "Azure AD v2 Windows Desktop komma igång - använda | Microsoft Docs"
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
ms.openlocfilehash: 826ba0a00b26993d4f37f0a8ce587d7bb77e7eb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a><span data-ttu-id="a8067-103">Använd Microsoft Authentication Library (MSAL) för att hämta en token för Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="a8067-103">Use the Microsoft Authentication Library (MSAL) to get a token for the Microsoft Graph API</span></span>

<span data-ttu-id="a8067-104">Det här avsnittet visar hur du använder MSAL för att hämta en token Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="a8067-104">This section shows how to use MSAL to get a token the Microsoft Graph API.</span></span>

1.  <span data-ttu-id="a8067-105">I `MainWindow.xaml.cs`, Lägg till referens för MSAL bibliotek i klassen:</span><span class="sxs-lookup"><span data-stu-id="a8067-105">In `MainWindow.xaml.cs`, add the reference for MSAL library to the class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="a8067-106">Ersätt <code>MainWindow</code> klassen kod med:</span><span class="sxs-lookup"><span data-stu-id="a8067-106">Replace <code>MainWindow</code> class code with:</span></span>
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
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
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
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
### <a name="more-information"></a><span data-ttu-id="a8067-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="a8067-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="a8067-108">Hämta en användartoken interaktiva</span><span class="sxs-lookup"><span data-stu-id="a8067-108">Getting a user token interactive</span></span>
<span data-ttu-id="a8067-109">Anropar den `AcquireTokenAsync` metoden resulterar i ett fönster som uppmanar användaren att logga in.</span><span class="sxs-lookup"><span data-stu-id="a8067-109">Calling the `AcquireTokenAsync` method results in a window prompting the user to sign in.</span></span> <span data-ttu-id="a8067-110">Program kräver vanligen en användare att logga in interaktivt första gången de behöver för att få åtkomst till en skyddad resurs, eller när en tyst åtgärd att hämta en token misslyckas (t.ex. användarens lösenord har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="a8067-110">Applications usually require a user to sign in interactively the first time they need to access a protected resource, or when a silent operation to acquire a token fails (e.g. the user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="a8067-111">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="a8067-111">Getting a user token silently</span></span>
<span data-ttu-id="a8067-112">`AcquireTokenSilentAsync`hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="a8067-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="a8067-113">Efter `AcquireTokenAsync` körs för första gången `AcquireTokenSilentAsync` är vanliga metod för att hämta token som används för att komma åt skyddade resurser för efterföljande anrop - eftersom anrop till begära eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="a8067-113">After `AcquireTokenAsync` is executed for the first time, `AcquireTokenSilentAsync` is the usual method used to obtain tokens used to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>
<span data-ttu-id="a8067-114">Slutligen `AcquireTokenSilentAsync` misslyckas – t.ex. användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="a8067-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. the user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="a8067-115">När MSAL upptäcker att problemet kan lösas genom att kräva en interaktiv åtgärd, den utlöses en `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="a8067-115">When MSAL detects that the issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="a8067-116">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a8067-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="a8067-117">Gör ett anrop mot `AcquireTokenAsync` direkt, vilket innebär att användaren uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="a8067-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting the user to sign-in.</span></span> <span data-ttu-id="a8067-118">Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i programmet för användaren.</span><span class="sxs-lookup"><span data-stu-id="a8067-118">This pattern is usually used in online applications where there is no offline content in the application available for the user.</span></span> <span data-ttu-id="a8067-119">Genereras av den här interaktiv installation används det här mönstret: du kan se den i åtgärden första gången du köra exemplet: eftersom ingen användare har använt programmet, `PublicClientApp.Users.FirstOrDefault()` innehåller ett null-värde och ett `MsalUiRequiredException` undantag.</span><span class="sxs-lookup"><span data-stu-id="a8067-119">The sample generated by this guided setup uses this pattern: you can see it in action the first time you execute the sample: because no user ever used the application, `PublicClientApp.Users.FirstOrDefault()` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="a8067-120">Koden i exemplet hanterar undantaget genom att anropa `AcquireTokenAsync` ledde till att användaren uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="a8067-120">The code in the sample then handles the exception by calling `AcquireTokenAsync` resulting in prompting the user to sign-in.</span></span>

2.  <span data-ttu-id="a8067-121">Program kan också göra en indikering för användaren som en interaktiv inloggning krävs, så att användaren kan välja att logga in rätt tidpunkt eller programmet kan försöka `AcquireTokenSilentAsync` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="a8067-121">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="a8067-122">Detta används vanligtvis när användaren kan använda andra funktioner i programmet utan att något stör – t.ex, det finns offline innehåll i programmet.</span><span class="sxs-lookup"><span data-stu-id="a8067-122">This is usually used when the user can use other functionality of the application without being disrupted - for example, there is offline content available in the application.</span></span> <span data-ttu-id="a8067-123">I det här fallet användaren kan välja när de vill logga in till den skydda resursen eller uppdatera inaktuell information eller ditt program kan välja att försök `AcquireTokenSilentAsync` när nätverket återställs efter att ha tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="a8067-123">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information, or your application can decide to retry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="a8067-124">Anropa använder token som du precis har köpt Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="a8067-124">Call the Microsoft Graph API using the token you just obtained</span></span>

1. <span data-ttu-id="a8067-125">Lägg till den nya metoden nedan till dina `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="a8067-125">Add the new method below to your `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="a8067-126">Metoden används för att göra en `GET` begäran mot Graph API med hjälp av ett auktorisera-huvud:</span><span class="sxs-lookup"><span data-stu-id="a8067-126">The method is used to make a `GET` request against Graph API using an Authorize header:</span></span>

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
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
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="a8067-127">Mer information om hur du skapar ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="a8067-127">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="a8067-128">I det här exempelprogrammet i `GetHttpContentWithToken` metoden används för att en HTTP `GET` begäran mot en skyddad resurs som kräver ett token och returnera innehållet till anroparen.</span><span class="sxs-lookup"><span data-stu-id="a8067-128">In this sample application, the `GetHttpContentWithToken` method is used to make an HTTP `GET` request against a protected resource that requires a token and then return the content to the caller.</span></span> <span data-ttu-id="a8067-129">Den här metoden lägger till anskaffats token i den *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="a8067-129">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="a8067-130">Resursen för det här exemplet är Microsoft Graph API *mig* slutpunkt – som visar information om användarens profil.</span><span class="sxs-lookup"><span data-stu-id="a8067-130">For this sample, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a><span data-ttu-id="a8067-131">Lägg till en metod för att logga ut användaren</span><span class="sxs-lookup"><span data-stu-id="a8067-131">Add a method to sign out the user</span></span>

1. <span data-ttu-id="a8067-132">Lägg till följande metod för att din `MainWindow.xaml.cs` logga ut användaren:</span><span class="sxs-lookup"><span data-stu-id="a8067-132">Add the following method to your `MainWindow.xaml.cs` to sign out the user:</span></span>

```csharp
/// <summary>
/// Sign out the current user
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
### <a name="more-info-on-sign-out"></a><span data-ttu-id="a8067-133">Mer information om utloggning</span><span class="sxs-lookup"><span data-stu-id="a8067-133">More info on Sign-Out</span></span>

<span data-ttu-id="a8067-134">`SignOutButton_Click`tar bort användaren från MSAL användarcachen – detta talar effektivt MSAL för att den aktuella användaren har glömt så att en framtida begäran om att hämta en token lyckas bara om det görs för att interaktivt.</span><span class="sxs-lookup"><span data-stu-id="a8067-134">`SignOutButton_Click` removes the user from MSAL user cache – this will effectively tell MSAL to forget the current user so a future request to acquire a token will only succeed if it is made to be interactive.</span></span>
<span data-ttu-id="a8067-135">Även om programmet i det här exemplet har stöd för en enskild användare, stöder MSAL scenarier där flera konton kan vara inloggad på samma gång – ett exempel är ett e-postprogram där en användare har flera konton.</span><span class="sxs-lookup"><span data-stu-id="a8067-135">Although the application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at the same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="display-basic-token-information"></a><span data-ttu-id="a8067-136">Visa grundläggande Information om Token</span><span class="sxs-lookup"><span data-stu-id="a8067-136">Display Basic Token Information</span></span>

1. <span data-ttu-id="a8067-137">Lägg till följande metod för att till din `MainWindow.xaml.cs` att visa grundläggande information om token:</span><span class="sxs-lookup"><span data-stu-id="a8067-137">Add the following method to to your `MainWindow.xaml.cs` to display basic information about the token:</span></span>

```csharp
/// <summary>
/// Display basic information contained in the token
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
### <a name="more-information"></a><span data-ttu-id="a8067-138">Mer information</span><span class="sxs-lookup"><span data-stu-id="a8067-138">More Information</span></span>

<span data-ttu-id="a8067-139">Token har köpt *OpenID Connect* också innehålla en liten del av information som är relevant för användaren.</span><span class="sxs-lookup"><span data-stu-id="a8067-139">Tokens acquired via *OpenID Connect* also contain a small subset of information pertinent to the user.</span></span> <span data-ttu-id="a8067-140">`DisplayBasicTokenInfo`Visar grundläggande information som finns i token: exempelvis användarens namn och ID, samt token upphör att gälla och den sträng som representerar åtkomst token sig själv.</span><span class="sxs-lookup"><span data-stu-id="a8067-140">`DisplayBasicTokenInfo` displays basic information contained in the token: for example, the user's display name and ID, as well as the token expiration date and the string representing the access token itself.</span></span> <span data-ttu-id="a8067-141">Den här informationen visas att se.</span><span class="sxs-lookup"><span data-stu-id="a8067-141">This information is displayed for you to see.</span></span> <span data-ttu-id="a8067-142">Du kan träffa på *anropa Microsoft Graph API* knappen flera gånger och se att samma token återanvänts för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="a8067-142">You can hit the *Call Microsoft Graph API* button multiple times and see that the same token was reused for subsequent requests.</span></span> <span data-ttu-id="a8067-143">Du kan också se förfallodatum förlängs när MSAL bestämmer det är dags att förnya token.</span><span class="sxs-lookup"><span data-stu-id="a8067-143">You can also see the expiration date being extended when MSAL decides it is time to renew the token.</span></span>
<!--end-collapse-->

