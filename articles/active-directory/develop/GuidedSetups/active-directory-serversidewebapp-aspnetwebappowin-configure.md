---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - Config | Microsoft Docs"
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
ms.openlocfilehash: e666be4622ad30aaa1e12e49ae56bbe1e129b2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="4491a-103">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="4491a-103">Create an application (Express)</span></span>
<span data-ttu-id="4491a-104">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="4491a-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4491a-105">Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span><span class="sxs-lookup"><span data-stu-id="4491a-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span></span>
2.  <span data-ttu-id="4491a-106">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4491a-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4491a-107">Kontrollera att hello alternativ för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4491a-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="4491a-108">Följ hello instruktioner tooadd ett tooyour omdirigerings-URL-program</span><span class="sxs-lookup"><span data-stu-id="4491a-108">Follow hello instructions tooadd a Redirect URL tooyour application</span></span>

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="4491a-109">Lägg till din registrering information tooyour lösning (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="4491a-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="4491a-110">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="4491a-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4491a-111">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program</span><span class="sxs-lookup"><span data-stu-id="4491a-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="4491a-112">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4491a-112">Enter a name for your application and your email</span></span> 
3.  <span data-ttu-id="4491a-113">Kontrollera att hello alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4491a-113">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="4491a-114">Klicka på `Add Platform`och välj`Web`</span><span class="sxs-lookup"><span data-stu-id="4491a-114">Click `Add Platform`, then select `Web`</span></span>
5.  <span data-ttu-id="4491a-115">Gå tillbaka tooVisual Studio och markera hello-projekt i Solution Explorer och titta på hello egenskapsfönstret (om du inte ser en egenskapsfönstret trycker du på F4)</span><span class="sxs-lookup"><span data-stu-id="4491a-115">Go back tooVisual Studio and, in Solution Explorer, select hello project and look at hello Properties window (if you don’t see a Properties window, press F4)</span></span>
6.  <span data-ttu-id="4491a-116">Ändra SSL aktiverat för`True`</span><span class="sxs-lookup"><span data-stu-id="4491a-116">Change SSL Enabled too`True`</span></span>
7.  <span data-ttu-id="4491a-117">Kopiera hello SSL-URL och lägger till den här URL: en toohello listan över omdirigerings-URL: er hello Registreringsportal omdirigerings-URL-listan:</span><span class="sxs-lookup"><span data-stu-id="4491a-117">Copy hello SSL URL and add this URL toohello list of Redirect URLs in hello Registration Portal’s list of Redirect URLs:</span></span><br/><br/>![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  <span data-ttu-id="4491a-119">Lägg till följande hello i `web.config` finns i hello rotmappen under hello avsnittet `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="4491a-119">Add hello following in `web.config` located in hello root folder under hello section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
<span data-ttu-id="4491a-120">Ersätt `ClientId` med hello program-Id som du precis har registrerats</span><span class="sxs-lookup"><span data-stu-id="4491a-120">Replace `ClientId` with hello Application Id you just registered</span></span>
</li>
<li>
<span data-ttu-id="4491a-121">Ersätt `redirectUri` med hello SSL-URL för ditt projekt</span><span class="sxs-lookup"><span data-stu-id="4491a-121">Replace `redirectUri` with hello SSL URL of your project</span></span>
</li>
</ol>
<!-- End Docs -->
