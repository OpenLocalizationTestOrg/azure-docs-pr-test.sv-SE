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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Använd hello Microsoft Authentication Library (MSAL) tooget en token för hello Microsoft Graph API

Det här avsnittet visar hur toouse MSAL tooget en token hello Microsoft Graph API.

1.  I `MainWindow.xaml.cs`, Lägg till hello-referens för MSAL biblioteket toohello klass:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Ersätt <code>MainWindow</code> klassen kod med:
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
### <a name="more-information"></a>Mer information
#### <a name="getting-a-user-token-interactive"></a>Hämta en användartoken interaktiva
Anropar hello `AcquireTokenAsync` metoden resulterar i ett fönster som uppmanar hello användaren toosign i. Program kräver vanligen en användare toosign i interaktivt hello första gången som de behöver tooaccess en skyddad resurs eller när en tyst åtgärden tooacquire en token misslyckas (t.ex. hello användarens lösenord har upphört att gälla).

#### <a name="getting-a-user-token-silently"></a>Hämta token för en användare tyst
`AcquireTokenSilentAsync`hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion. Efter `AcquireTokenAsync` för hello körs första gången `AcquireTokenSilentAsync` hello vanliga metod som används för tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.
Slutligen `AcquireTokenSilentAsync` misslyckas – t.ex. hello användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet. När MSAL känner av att hello problemet kan lösas genom att kräva en interaktiv åtgärd den utlöses en `MsalUiRequiredException`. Programmet kan hantera det här undantaget på två sätt:

1.  Gör ett anrop mot `AcquireTokenAsync` omedelbart, vilket innebär att fråga användaren hello toosign i. Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i hello program tillgängliga för hello användare. hello exemplet skapas vid interaktiv installationen använder det här mönstret: du kan se den i åtgärden hello första gången du kör hello exempel: eftersom ingen användare har använt hello programmet `PublicClientApp.Users.FirstOrDefault()` innehåller ett null-värde och ett `MsalUiRequiredException` undantag. Hej koden i hello exempel sedan handtag hello undantag genom att anropa `AcquireTokenAsync` vilket resulterar i att fråga användaren hello toosign i.

2.  Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `AcquireTokenSilentAsync` vid ett senare tillfälle. Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns innehåll offline i hello program. I det här fallet hello användare kan bestämma när de vill toosign i tooaccess hello skyddade resursen toorefresh hello inaktuell information eller ditt program kan bestämma tooretry `AcquireTokenSilentAsync` när nätverket återställs efter att ha tillfälligt otillgänglig.
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Anropa hello Microsoft Graph API använder du bara fick hello-token

1. Lägga till nya hello-metoden nedan tooyour `MainWindow.xaml.cs`. hello metoden är används toomake en `GET` begäran mot Graph API med hjälp av ett auktorisera-huvud:

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
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Mer information om hur du skapar ett REST-anrop mot ett skyddade API

I det här exempelprogrammet hello `GetHttpContentWithToken` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare. Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*. För det här exemplet hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a>Lägg till en metod toosign ut hello användare

1. Lägg till följande metod tooyour hello `MainWindow.xaml.cs` toosign ut hello användare:

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
### <a name="more-info-on-sign-out"></a>Mer information om utloggning

`SignOutButton_Click`tar bort hello användare från MSAL användarcachen – detta talar effektivt MSAL tooforget hello aktuell användare så att en begäran om framtida tooacquire en token lyckas bara om det görs toobe interaktiva.
Hello-programmet i det här exemplet stöder en enskild användare, MSAL har stöd för scenarier där flera konton kan vara inloggad på hello samtidigt – ett exempel är ett e-postprogram där en användare har flera konton.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Visa grundläggande Information om Token

1. Lägg till följande metod tootooyour hello `MainWindow.xaml.cs` toodisplay grundläggande information om hello token:

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
### <a name="more-information"></a>Mer information

Token har köpt *OpenID Connect* också innehålla en liten del av information om relevant toohello användare. `DisplayBasicTokenInfo`Visar grundläggande information som finns i hello token: till exempel hello användare visar namn och ID, samt hello giltighetstid för token datum och hello sträng som representerar hello åtkomsttoken sig själv. Den här informationen visas för du toosee. Du kan träffa hello *anropa Microsoft Graph API* knappen flera gånger och se den hello samma token återanvänts för efterföljande förfrågningar. Du kan också se hello förfallodatum förlängs när MSAL bestämmer det är tid toorenew hello token.
<!--end-collapse-->

