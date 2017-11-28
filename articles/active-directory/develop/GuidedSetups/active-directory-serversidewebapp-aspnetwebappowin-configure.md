---
title: "Azure AD v2 ASP.NET Web Server komma igång - Config | Microsoft Docs"
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
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="b5e04-103">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="b5e04-103">Create an application (Express)</span></span>
<span data-ttu-id="b5e04-104">Nu måste du registrera ditt program i den *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="b5e04-104">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="b5e04-105">Registrera ditt program via den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span><span class="sxs-lookup"><span data-stu-id="b5e04-105">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span></span>
2.  <span data-ttu-id="b5e04-106">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="b5e04-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="b5e04-107">Kontrollera att alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="b5e04-107">Make sure the option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="b5e04-108">Följ instruktionerna för att lägga till en omdirigerings-URL för ditt program</span><span class="sxs-lookup"><span data-stu-id="b5e04-108">Follow the instructions to add a Redirect URL to your application</span></span>

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a><span data-ttu-id="b5e04-109">Lägga till registreringsinformationen program i lösningen (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="b5e04-109">Add your application registration information to your solution (Advanced)</span></span>
<span data-ttu-id="b5e04-110">Nu måste du registrera ditt program i den *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="b5e04-110">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="b5e04-111">Gå till den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) registrera ett program</span><span class="sxs-lookup"><span data-stu-id="b5e04-111">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) to register an application</span></span>
2. <span data-ttu-id="b5e04-112">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="b5e04-112">Enter a name for your application and your email</span></span> 
3.  <span data-ttu-id="b5e04-113">Kontrollera att alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="b5e04-113">Make sure the option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="b5e04-114">Klicka på `Add Platform`och välj`Web`</span><span class="sxs-lookup"><span data-stu-id="b5e04-114">Click `Add Platform`, then select `Web`</span></span>
5.  <span data-ttu-id="b5e04-115">Gå tillbaka till Visual Studio och i Solution Explorer, välj projektet och titta på fönstret Egenskaper (om du inte ser en egenskapsfönstret trycker du på F4)</span><span class="sxs-lookup"><span data-stu-id="b5e04-115">Go back to Visual Studio and, in Solution Explorer, select the project and look at the Properties window (if you don’t see a Properties window, press F4)</span></span>
6.  <span data-ttu-id="b5e04-116">Ändra SSL aktiverat för`True`</span><span class="sxs-lookup"><span data-stu-id="b5e04-116">Change SSL Enabled to `True`</span></span>
7.  <span data-ttu-id="b5e04-117">Kopiera den URL som SSL och lägga till denna URL i listan över omdirigerings-URL: er i portalen för registrering omdirigerings-URL-listan:</span><span class="sxs-lookup"><span data-stu-id="b5e04-117">Copy the SSL URL and add this URL to the list of Redirect URLs in the Registration Portal’s list of Redirect URLs:</span></span><br/><br/>![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  <span data-ttu-id="b5e04-119">Lägg till följande i `web.config` finns i rotmappen under avsnittet `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="b5e04-119">Add the following in `web.config` located in the root folder under the section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
<span data-ttu-id="b5e04-120">Ersätt `ClientId` med program-Id som du precis har registrerats</span><span class="sxs-lookup"><span data-stu-id="b5e04-120">Replace `ClientId` with the Application Id you just registered</span></span>
</li>
<li>
<span data-ttu-id="b5e04-121">Ersätt `redirectUri` med SSL-URL för ditt projekt</span><span class="sxs-lookup"><span data-stu-id="b5e04-121">Replace `redirectUri` with the SSL URL of your project</span></span>
</li>
</ol>
<!-- End Docs -->
