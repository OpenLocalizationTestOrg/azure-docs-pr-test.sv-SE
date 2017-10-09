---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - Använd | Microsoft Docs"
description: "Implementera Microsoft logga In på en ASP.NET-lösning med ett traditionellt webbläsarbaserade program med OpenID Connect standard"
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
ms.openlocfilehash: 03afce6fa6598215e8c4af841c00762c143a0cd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="add-a-controller-toohandle-sign-in-and-sign-out-requests"></a>Lägg till en domänkontrollant toohandle inloggning och utloggning begäranden

Det här steget visar hur toocreate en ny domänkontrollant tooexpose inloggning och utloggning metoder.

1.  Högerklicka på hello `Controllers` och väljer`Add` > `Controller`
2.  Välj `MVC (.NET version) Controller – Empty`.
3.  Klicka på *Lägg till*
4.  Ge den namnet `HomeController` och på *Lägg till*
5.  Lägg till *OWIN* refererar toohello klassen:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Lägg till hello två metoderna nedan toohandle inloggning och utloggning tooyour domänkontrollant genom att initiera en autentiseringsfråga via kod:
</li>
</ol>

```csharp
/// <summary>
/// Send an OpenID Connect sign-in request.
/// Alternatively, you can just decorate hello SignIn method with hello [Authorize] attribute
/// </summary>
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
    HttpContext.GetOwinContext().Authentication.SignOut(
            OpenIdConnectAuthenticationDefaults.AuthenticationType,
            CookieAuthenticationDefaults.AuthenticationType);
}
```

## <a name="create-hello-apps-home-page-toosign-in-users-via-a-sign-in-button"></a>Skapa hello app startsidan toosign användare via en knapp för inloggning

Knapp för att skapa en ny vy tooadd hello inloggning i Visual Studio och visa information om användare efter autentisering:

1.  Högerklicka på hello `Views\Home` och väljer`Add View`
2.  Ge den namnet `Index`.
3.  Lägg till följande HTML-format, vilket innefattar hello-knappen Logga in, toohello filen hello:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
</head>
<body>
@if (!Request.IsAuthenticated)
{
    <!-- If hello user is not authenticated, display hello sign-in button -->
    <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
        <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
        <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
        <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
        <rect x="150" y="129" width="122" height="122" fill="#F35325" />
        <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
        <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
        <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
        <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
        </svg>
    </a>
}
else
{
    <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
    <br /><br />
    @Html.ActionLink("See Your Claims", "Index", "Claims")
    <br /><br />
    @Html.ActionLink("Sign out", "SignOut", "Home")
}
@if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
{
    <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
}
</body>
</html>
```
<!--start-collapse-->
### <a name="more-information"></a>Mer information
> Den här sidan lägger till en knapp för inloggning i SVG-format med svart bakgrund:<br/>![Logga in med Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-use/aspnetsigninbuttonsample.png)<br/> Flera logga in, på knappar gå toohello [den här sidan](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "företagsanpassning riktlinjer").
<!--end-collapse-->

## <a name="add-a-controller-toodisplay-users-claims"></a>Lägga till en domänkontrollant toodisplay användarens anspråk
Den här domänkontrollanten visar hello används av hello `[Authorize]` attributet tooprotect en domänkontrollant. Det här attributet begränsar åtkomst toohello domänkontrollant genom att bara tillåta autentiserade användare. Hej koden nedan använder hello attributet toodisplay användaranspråk som har hämtats som en del av hello-inloggning.

1.  Högerklicka på hello `Controllers` mapp:`Add` > `Controller`
2.  Välj `MVC {version} Controller – Empty`.
3.  Klicka på *Lägg till*
4.  Ge den namnet`ClaimsController`
5.  Ersätt hello kod för klassen med hello koden nedan - domänkontrollant läggs hello `[Authorize]` attributet toohello klass:

```csharp
[Authorize]
public class ClaimsController : Controller
{
    /// <summary>
    /// Add user's claims tooviewbag
    /// </summary>
    /// <returns></returns>
    public ActionResult Index()
    {
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get hello user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // hello 'preferred_username' claim can be used for showing hello username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // hello subject claim can be used toouniquely identify hello user across hello web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is hello unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Mer information
> På grund av hello användning av hello `[Authorize]` attribut, alla metoderna för den här styrenheten kan endast köras om hello användaren autentiseras. Om hello användaren inte har autentiserats och försöker tooaccess hello styrenhet, initiera en autentiseringsfråga OWIN och tvinga hello användaren tooauthenticate. hello koden ovan för att söka efter hello anspråk samling hello `ClaimsPrincipal.Current` instans för en specifik användarattribut som ingår i hello användar-token. Dessa attribut omfatta hello användarens fullständiga namn och användarnamn samt hello global användare identifierare ämne. Den innehåller också hello *klient-ID*, som representerar hello-ID för hello användarens organisation. 
<!--end-collapse-->

## <a name="create-a-view-toodisplay-hello-users-claims"></a>Skapa en vy toodisplay hello användarens anspråk

Skapa en ny vy toodisplay hello användarens anspråk på en webbsida i Visual Studio:

1.  Högerklicka på hello `Views\Claims` mapp och:`Add View`
2.  Ge den namnet `Index`.
3.  Lägg till följande HTML toohello hello:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
    <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
</head>
<body style="padding:50px">
    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
    @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
    {
        <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
    }
</table>
    <br />
    <br />
    @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
</body>
</html>
```
