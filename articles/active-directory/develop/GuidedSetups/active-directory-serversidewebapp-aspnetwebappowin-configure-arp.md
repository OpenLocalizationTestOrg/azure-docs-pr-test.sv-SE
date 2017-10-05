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
ms.openlocfilehash: 8a1650a65e7980f4a13fa4edc7918b0099bb5464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a><span data-ttu-id="26823-103">Konfigurera ditt ASP.NET-webbprogram med programmets registreringsinformation</span><span class="sxs-lookup"><span data-stu-id="26823-103">Configure your ASP.NET Web App with the application's registration information</span></span>

<span data-ttu-id="26823-104">I det här steget ska du konfigurera ditt projekt för att använda SSL och sedan använda SSL-URL: en för att konfigurera ditt program registreringsinformation.</span><span class="sxs-lookup"><span data-stu-id="26823-104">In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information.</span></span> <span data-ttu-id="26823-105">Därefter kan du lägga till programmet ' registreringsinformation i lösningen via *web.config*.</span><span class="sxs-lookup"><span data-stu-id="26823-105">After this, add the application’ registration information to your solution via *web.config*.</span></span>

1.  <span data-ttu-id="26823-106">Välj projektet i Solution Explorer och titta på den `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på F4)</span><span class="sxs-lookup"><span data-stu-id="26823-106">In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="26823-107">Ändra `SSL Enabled` till`True`</span><span class="sxs-lookup"><span data-stu-id="26823-107">Change `SSL Enabled` to `True`</span></span>
3.  <span data-ttu-id="26823-108">Kopiera värdet från `SSL URL` ovan och klistra in den i den `Redirect URL` fältet överst i den här sidan och klicka sedan på *uppdatering*:</span><span class="sxs-lookup"><span data-stu-id="26823-108">Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="26823-109">![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="26823-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="26823-110">Lägg till följande i `web.config` fil i rotens mappen under avsnittet `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="26823-110">Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
