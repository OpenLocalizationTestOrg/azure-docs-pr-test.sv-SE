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
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a><span data-ttu-id="2a0af-103">Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (MVC-projekt)</span><span class="sxs-lookup"><span data-stu-id="2a0af-103">Getting Started with Azure Active Directory and Visual Studio connected services (MVC Projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a0af-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2a0af-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="2a0af-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="2a0af-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a><span data-ttu-id="2a0af-106">Kräver autentisering tooaccess domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="2a0af-106">Requiring authentication tooaccess controllers</span></span>
<span data-ttu-id="2a0af-107">Alla domänkontrollanter i ditt projekt har adorned med hello **auktorisera** attribut.</span><span class="sxs-lookup"><span data-stu-id="2a0af-107">All controllers in your project were adorned with hello **Authorize** attribute.</span></span> <span data-ttu-id="2a0af-108">Det här attributet kräver hello användaren toobe autentiseras innan du använder dessa domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="2a0af-108">This attribute requires hello user toobe authenticated before accessing these controllers.</span></span> <span data-ttu-id="2a0af-109">tooallow hello controller toobe ansluta anonymt, ta bort skrivskyddsattributet från hello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="2a0af-109">tooallow hello controller toobe accessed anonymously, remove this attribute from hello controller.</span></span> <span data-ttu-id="2a0af-110">Om du vill tooset hello behörigheter på en mer detaljerad nivå gäller hello attributet tooeach metoden som kräver tillstånd i stället för att tillämpa den toohello kontrollantklassen.</span><span class="sxs-lookup"><span data-stu-id="2a0af-110">If you want tooset hello permissions at a more granular level, apply hello attribute tooeach method that requires authorization instead of applying it toohello controller class.</span></span>

## <a name="adding-signin--signout-controls"></a><span data-ttu-id="2a0af-111">Lägga till inloggning / utloggning styr</span><span class="sxs-lookup"><span data-stu-id="2a0af-111">Adding SignIn / SignOut Controls</span></span>
<span data-ttu-id="2a0af-112">tooadd hello inloggning/utloggning styr tooyour vy, kan du använda hello **_LoginPartial.cshtml** partiella visa tooadd hello funktioner tooone av dina vyer.</span><span class="sxs-lookup"><span data-stu-id="2a0af-112">tooadd hello SignIn/SignOut controls tooyour view, you can use hello **_LoginPartial.cshtml** partial view tooadd hello functionality tooone of your views.</span></span> <span data-ttu-id="2a0af-113">Här är ett exempel på hello funktioner lagts till toohello standard **_Layout.cshtml** vyn.</span><span class="sxs-lookup"><span data-stu-id="2a0af-113">Here is an example of hello functionality added toohello standard **_Layout.cshtml** view.</span></span> <span data-ttu-id="2a0af-114">(Observera hello sista elementet i hello div med klassen navigeringsfält för komprimera):</span><span class="sxs-lookup"><span data-stu-id="2a0af-114">(Note hello last element in hello div with class navbar-collapse):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2a0af-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a0af-115">Next steps</span></span>
- [<span data-ttu-id="2a0af-116">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2a0af-116">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/) 

