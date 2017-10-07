---
title: "aaaGet igång med Azure AD i Visual Studio MVC projekt | Microsoft Docs"
description: "Hur tooget igång med Azure Active Directory i MVC-projekt efter anslutning tooor skapa en Azure AD med hjälp av Visual Studio anslutna tjänster"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 807824dd6e4e57e443f8a7322cf2e5326384316d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (MVC-projekt)
> [!div class="op_single_selector"]
> * [Komma igång](vs-active-directory-dotnet-getting-started.md)
> * [Vad hände](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a>Kräver autentisering tooaccess domänkontrollanter
Alla domänkontrollanter i ditt projekt har adorned med hello **auktorisera** attribut. Det här attributet kräver hello användaren toobe autentiseras innan du använder dessa domänkontrollanter. tooallow hello controller toobe ansluta anonymt, ta bort skrivskyddsattributet från hello domänkontrollant. Om du vill tooset hello behörigheter på en mer detaljerad nivå gäller hello attributet tooeach metoden som kräver tillstånd i stället för att tillämpa den toohello kontrollantklassen.

## <a name="adding-signin--signout-controls"></a>Lägga till inloggning / utloggning styr
tooadd hello inloggning/utloggning styr tooyour vy, kan du använda hello **_LoginPartial.cshtml** partiella visa tooadd hello funktioner tooone av dina vyer. Här är ett exempel på hello funktioner lagts till toohello standard **_Layout.cshtml** vyn. (Observera hello sista elementet i hello div med klassen navigeringsfält för komprimera):

<pre>
    &lt;!DOCTYPE html&gt; 
     &lt;html&gt; 
     &lt;head&gt; 
         &lt;meta charset="utf-8" /&gt; 
        &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
        &lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") 
        @Scripts.Render("~/bundles/modernizr") 
    &lt;/head&gt; 
    &lt;body&gt; 
        &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
            &lt;div class="container"&gt; 
                &lt;div class="navbar-header"&gt; 
                    &lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                    &lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) 
                &lt;/div&gt; 
                &lt;div class="navbar-collapse collapse"&gt; 
                    &lt;ul class="nav navbar-nav"&gt; 
                        &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
                    &lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
            &lt;/div&gt; 
        &lt;/div&gt; 
        &lt;div class="container body-content"&gt; 
            @RenderBody() 
            &lt;hr /&gt; 
            &lt;footer&gt; 
                &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
            &lt;/footer&gt; 
        &lt;/div&gt; 
        @Scripts.Render("~/bundles/jquery") 
        @Scripts.Render("~/bundles/bootstrap") 
        @RenderSection("scripts", required: false) 
    &lt;/body&gt; 
    &lt;/html&gt;
</pre>

## <a name="next-steps"></a>Nästa steg
- [Lär dig mer om Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 

