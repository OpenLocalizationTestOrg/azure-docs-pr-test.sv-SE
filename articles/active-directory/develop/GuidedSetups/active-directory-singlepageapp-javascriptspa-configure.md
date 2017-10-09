---
title: "aaaAzure AD v2 JS SPA interaktiv installation – konfigurera | Microsoft Docs"
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 1b93298d4bd4e17dd261dbb75502a122c30aac97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="register-your-application"></a><span data-ttu-id="cc1d8-103">Registrera ditt program</span><span class="sxs-lookup"><span data-stu-id="cc1d8-103">Register your application</span></span>

<span data-ttu-id="cc1d8-104">Det finns flera sätt toocreate ett program, Välj en av dem:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-104">There are multiple ways toocreate an application, please select one of them:</span></span>

### <a name="option-1-register-your-application-express-mode"></a><span data-ttu-id="cc1d8-105">Alternativ 1: Registrera ditt program (Express-läge)</span><span class="sxs-lookup"><span data-stu-id="cc1d8-105">Option 1: Register your application (Express mode)</span></span>
<span data-ttu-id="cc1d8-106">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-106">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>

1.  <span data-ttu-id="cc1d8-107">Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span><span class="sxs-lookup"><span data-stu-id="cc1d8-107">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)</span></span>
2.  <span data-ttu-id="cc1d8-108">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="cc1d8-108">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="cc1d8-109">Se till att hello-alternativet *interaktiv installation* kontrolleras</span><span class="sxs-lookup"><span data-stu-id="cc1d8-109">Make sure hello option for *Guided Setup* is checked</span></span>
4.  <span data-ttu-id="cc1d8-110">Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod</span><span class="sxs-lookup"><span data-stu-id="cc1d8-110">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="option-2-register-your-application-advanced-mode"></a><span data-ttu-id="cc1d8-111">Alternativ 2: Registrera ditt program (Avancerat läge)</span><span class="sxs-lookup"><span data-stu-id="cc1d8-111">Option 2: Register your application (Advanced mode)</span></span>

1. <span data-ttu-id="cc1d8-112">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program</span><span class="sxs-lookup"><span data-stu-id="cc1d8-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="cc1d8-113">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="cc1d8-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="cc1d8-114">Se till att hello-alternativet *interaktiv installation* är avmarkerat</span><span class="sxs-lookup"><span data-stu-id="cc1d8-114">Make sure hello option for *Guided Setup* is unchecked</span></span>
4.  <span data-ttu-id="cc1d8-115">Klicka på `Add Platform`och välj`Web`</span><span class="sxs-lookup"><span data-stu-id="cc1d8-115">Click `Add Platform`, then select `Web`</span></span>
5. <span data-ttu-id="cc1d8-116">Lägg till hello `Redirect URL` som motsvarar toohello programmets URL baserat på din webbserver.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-116">Add hello `Redirect URL` that correspond toohello application's URL based on your web server.</span></span> <span data-ttu-id="cc1d8-117">Visa hello avsnitt nedan för instruktioner om hur tooset / hämta hello omdirigerings-URL i Visual Studio och Python.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-117">See hello sections below for instructions on how tooset/ obtain hello redirect URL in Visual Studio and Python.</span></span>
6. <span data-ttu-id="cc1d8-118">Klicka på *spara*</span><span class="sxs-lookup"><span data-stu-id="cc1d8-118">Click *Save*</span></span>

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="cc1d8-119">Visual Studio-instruktionerna för att skaffa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="cc1d8-119">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="cc1d8-120">Följ hello instruktioner tooobtain omdirigerings-URL:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-120">Follow hello instructions tooobtain your redirect URL:</span></span>
> 1.    <span data-ttu-id="cc1d8-121">I *Solution Explorer*, Välj hello projekt och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på `F4`)</span><span class="sxs-lookup"><span data-stu-id="cc1d8-121">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="cc1d8-122">Kopiera hello-värde från `URL` toohello Urklipp:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-122">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="cc1d8-123">![Egenskaper för](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="cc1d8-123">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="cc1d8-124">Växla tillbaka toohello *Programregistreringsportalen* och klistra in hello-värde som en `Redirect URL` och klicka på ”Spara”</span><span class="sxs-lookup"><span data-stu-id="cc1d8-124">Switch back toohello *Application Registration Portal* and paste hello value as a `Redirect URL` and click 'Save'</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="cc1d8-125">Inställningen omdirigerings-URL för Python</span><span class="sxs-lookup"><span data-stu-id="cc1d8-125">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="cc1d8-126">För Python, kan du ange hello web server-port via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-126">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="cc1d8-127">Den interaktiva installationen använder hello port 8080 för referens men känna sig fria toouse någon annan port som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-127">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="cc1d8-128">I varje fall Följ hello instruktionerna nedan tooset upp en omdirigerings-URL i hello registreringsinformation för programmet:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-128">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> - <span data-ttu-id="cc1d8-129">Växla tillbaka toohello *Programregistreringsportalen* och ange `http://localhost:8080/` som en `Redirect URL`, eller Använd `http://localhost:[port]/` om du använder en anpassad TCP-port (där *[port]* är hello anpassade TCP-port antalet) och klicka på ”Spara”</span><span class="sxs-lookup"><span data-stu-id="cc1d8-129">Switch back toohello *Application Registration Portal* and set `http://localhost:8080/` as a `Redirect URL`, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number) and click 'Save'</span></span>


#### <a name="configure-your-javascript-spa"></a><span data-ttu-id="cc1d8-130">Konfigurera din JavaScript SPA</span><span class="sxs-lookup"><span data-stu-id="cc1d8-130">Configure your JavaScript SPA</span></span>

1.  <span data-ttu-id="cc1d8-131">Skapa en fil med namnet `msalconfig.js` som innehåller information om programmet hello.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-131">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="cc1d8-132">Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklickar och väljer: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="cc1d8-132">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="cc1d8-133">Ge den namnet`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="cc1d8-133">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="cc1d8-134">Lägg till följande kod tooyour hello `msalconfig.js` fil:</span><span class="sxs-lookup"><span data-stu-id="cc1d8-134">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
<span data-ttu-id="cc1d8-135">Ersätt <code>Enter_the_Application_Id_here</code> med hello program-Id som du precis har registrerats</span><span class="sxs-lookup"><span data-stu-id="cc1d8-135">Replace <code>Enter_the_Application_Id_here</code> with hello Application Id you just registered</span></span>
</li>
</ol>
