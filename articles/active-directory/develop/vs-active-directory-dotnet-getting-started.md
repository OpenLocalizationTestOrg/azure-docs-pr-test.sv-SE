---
title: "Kom igång med Azure AD i Visual Studio MVC projekt | Microsoft Docs"
description: "Hur du kommer igång med Azure Active Directory i MVC projekt efter anslutning till eller skapa en Azure AD med hjälp av Visual Studio anslutna tjänster"
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
ms.openlocfilehash: c4d49cfc9887e422b3eaed2b96348c99eca48881
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a><span data-ttu-id="2afe9-103">Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (MVC-projekt)</span><span class="sxs-lookup"><span data-stu-id="2afe9-103">Getting Started with Azure Active Directory and Visual Studio connected services (MVC Projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2afe9-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2afe9-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="2afe9-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="2afe9-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a><span data-ttu-id="2afe9-106">Kräva autentisering till access-domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="2afe9-106">Requiring authentication to access controllers</span></span>
<span data-ttu-id="2afe9-107">Alla domänkontrollanter i ditt projekt har adorned med den **auktorisera** attribut.</span><span class="sxs-lookup"><span data-stu-id="2afe9-107">All controllers in your project were adorned with the **Authorize** attribute.</span></span> <span data-ttu-id="2afe9-108">Det här attributet kräver att användaren autentiseras innan du använder dessa domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="2afe9-108">This attribute requires the user to be authenticated before accessing these controllers.</span></span> <span data-ttu-id="2afe9-109">Ta bort skrivskyddsattributet från styrenheten för att tillåta att kontrollanten kan användas anonymt.</span><span class="sxs-lookup"><span data-stu-id="2afe9-109">To allow the controller to be accessed anonymously, remove this attribute from the controller.</span></span> <span data-ttu-id="2afe9-110">Om du vill ange behörigheter på en mer detaljerad nivå gäller attributet för varje metod som kräver tillstånd i stället för att tillämpas på klassen domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="2afe9-110">If you want to set the permissions at a more granular level, apply the attribute to each method that requires authorization instead of applying it to the controller class.</span></span>

## <a name="adding-signin--signout-controls"></a><span data-ttu-id="2afe9-111">Lägga till inloggning / utloggning styr</span><span class="sxs-lookup"><span data-stu-id="2afe9-111">Adding SignIn / SignOut Controls</span></span>
<span data-ttu-id="2afe9-112">Om du vill lägga till inloggning/utloggning kontroller i vyn kan du använda den **_LoginPartial.cshtml** partiella vyn för att lägga till funktionen i en av dina vyer.</span><span class="sxs-lookup"><span data-stu-id="2afe9-112">To add the SignIn/SignOut controls to your view, you can use the **_LoginPartial.cshtml** partial view to add the functionality to one of your views.</span></span> <span data-ttu-id="2afe9-113">Här är ett exempel på funktioner som lagts till i vanlig **_Layout.cshtml** vyn.</span><span class="sxs-lookup"><span data-stu-id="2afe9-113">Here is an example of the functionality added to the standard **_Layout.cshtml** view.</span></span> <span data-ttu-id="2afe9-114">(Observera det sista elementet i div med klassen navigeringsfält för komprimera):</span><span class="sxs-lookup"><span data-stu-id="2afe9-114">(Note the last element in the div with class navbar-collapse):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2afe9-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2afe9-115">Next steps</span></span>
- [<span data-ttu-id="2afe9-116">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2afe9-116">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/) 

