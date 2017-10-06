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
ms.openlocfilehash: badc47e131290a56a507592f944a0fc7093260a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a><span data-ttu-id="79176-103">Konfigurera ditt ASP.NET-webbprogram med registreringsinformation Hej program</span><span class="sxs-lookup"><span data-stu-id="79176-103">Configure your ASP.NET Web App with hello application's registration information</span></span>

<span data-ttu-id="79176-104">I det här steget ska du konfigurera ditt projekt toouse SSL och använda hello SSL URL tooconfigure registreringsinformation för ditt program.</span><span class="sxs-lookup"><span data-stu-id="79176-104">In this step, you will configure your project toouse SSL, and then use hello SSL URL tooconfigure your application’s registration information.</span></span> <span data-ttu-id="79176-105">Sedan lägger du till hello program ' registrering information tooyour lösning via *web.config*.</span><span class="sxs-lookup"><span data-stu-id="79176-105">After this, add hello application’ registration information tooyour solution via *web.config*.</span></span>

1.  <span data-ttu-id="79176-106">Välj hello-projekt i Solution Explorer och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på F4)</span><span class="sxs-lookup"><span data-stu-id="79176-106">In Solution Explorer, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="79176-107">Ändra `SSL Enabled` för`True`</span><span class="sxs-lookup"><span data-stu-id="79176-107">Change `SSL Enabled` too`True`</span></span>
3.  <span data-ttu-id="79176-108">Kopiera hello-värde från `SSL URL` ovan och klistra in den i hello `Redirect URL` fältet hello längst upp på sidan och klicka sedan på *uppdatering*:</span><span class="sxs-lookup"><span data-stu-id="79176-108">Copy hello value from `SSL URL` above and paste it in hello `Redirect URL` field on hello top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="79176-109">![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="79176-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="79176-110">Lägg till följande hello i `web.config` fil i rotens mappen under avsnittet `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="79176-110">Add hello following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
