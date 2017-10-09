---
title: "aaaAzure AD v2 JS SPA interaktiv installation - inställningar | Microsoft Docs"
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
ms.openlocfilehash: 19e15c6c8db8bea2975f30e7505af79ccad17e02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-web-server-or-project"></a><span data-ttu-id="079e7-103">Konfigurera webbservern eller projekt</span><span class="sxs-lookup"><span data-stu-id="079e7-103">Setting up your web server or project</span></span>

> <span data-ttu-id="079e7-104">Föredrar toodownload projekt för det här exemplet i stället?</span><span class="sxs-lookup"><span data-stu-id="079e7-104">Prefer toodownload this sample's project instead?</span></span> 
> - [<span data-ttu-id="079e7-105">Hämta hello Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="079e7-105">Download hello Visual Studio project</span></span>](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> <span data-ttu-id="079e7-106">eller</span><span class="sxs-lookup"><span data-stu-id="079e7-106">or</span></span>
> - <span data-ttu-id="079e7-107">[Hämta hello projektfiler](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) för en lokal webbserver, till exempel Python</span><span class="sxs-lookup"><span data-stu-id="079e7-107">[Download hello project files](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) for a local web server, such as Python</span></span>
>
> <span data-ttu-id="079e7-108">Och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan den körs.</span><span class="sxs-lookup"><span data-stu-id="079e7-108">And then  skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="079e7-109">Krav</span><span class="sxs-lookup"><span data-stu-id="079e7-109">Prerequisites</span></span>
<span data-ttu-id="079e7-110">En lokal webbserver som [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), eller IIS Express integrering med [Visual Studio 2017](https://www.visualstudio.com/downloads/) är obligatoriska toorun interaktiv installationen.</span><span class="sxs-lookup"><span data-stu-id="079e7-110">A local web server such as [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), or IIS Express integration with [Visual Studio 2017](https://www.visualstudio.com/downloads/) is required toorun this guided setup.</span></span> 

<span data-ttu-id="079e7-111">Anvisningarna i guiden är baserade på både Python och Visual Studio 2017 men känna sig fria toouse andra utvecklingsmiljö eller en webbserver.</span><span class="sxs-lookup"><span data-stu-id="079e7-111">Instructions in this guide are based on both Python and Visual Studio 2017, but feel free toouse any other development environment or Web Server.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="079e7-112">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="079e7-112">Create your project</span></span> 

> ### <a name="option-1-visual-studio"></a><span data-ttu-id="079e7-113">Alternativ 1: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="079e7-113">Option 1: Visual Studio</span></span> 
> <span data-ttu-id="079e7-114">Om du använder Visual Studio och skapar ett nytt projekt gör hello nedan toocreate en ny Visual Studio-lösning:</span><span class="sxs-lookup"><span data-stu-id="079e7-114">If you are using Visual Studio and are creating a new project, follow hello steps below toocreate a new Visual Studio solution:</span></span>
> 1.    <span data-ttu-id="079e7-115">I Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="079e7-115">In Visual Studio:  `File` > `New` > `Project`</span></span>
> 2.    <span data-ttu-id="079e7-116">Under `Visual C#\Web`väljer`ASP.NET Web Application (.NET Framework)`</span><span class="sxs-lookup"><span data-stu-id="079e7-116">Under `Visual C#\Web`, select `ASP.NET Web Application (.NET Framework)`</span></span>
> 3.    <span data-ttu-id="079e7-117">Namnge ditt program och klicka på *OK*</span><span class="sxs-lookup"><span data-stu-id="079e7-117">Name your application and click *OK*</span></span>
> 4.    <span data-ttu-id="079e7-118">Under `New ASP.NET Web Application`väljer`Empty`</span><span class="sxs-lookup"><span data-stu-id="079e7-118">Under `New ASP.NET Web Application`, select `Empty`</span></span>

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a><span data-ttu-id="079e7-119">Alternativ 2: Python / andra webbservrar</span><span class="sxs-lookup"><span data-stu-id="079e7-119">Option 2: Python/ other web servers</span></span>
> <span data-ttu-id="079e7-120">Kontrollera att du har installerat [Python](https://www.python.org/downloads/), följ hello steg nedan:</span><span class="sxs-lookup"><span data-stu-id="079e7-120">Make sure you have installed [Python](https://www.python.org/downloads/), then follow hello step below:</span></span>
> - <span data-ttu-id="079e7-121">Skapa en mapp toohost ditt program.</span><span class="sxs-lookup"><span data-stu-id="079e7-121">Create a folder toohost your application.</span></span>


## <a name="create-your-single-page-applications-ui"></a><span data-ttu-id="079e7-122">Skapa sida programmets användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="079e7-122">Create your single page application’s UI</span></span>
1.  <span data-ttu-id="079e7-123">Skapa en *index.html* -filen för JavaScript-SPA.</span><span class="sxs-lookup"><span data-stu-id="079e7-123">Create an *index.html* file for your JavaScript SPA.</span></span> <span data-ttu-id="079e7-124">Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklicka och välj: `Add`  >  `New Item`  >  `HTML page` och ge den namnet index.html</span><span class="sxs-lookup"><span data-stu-id="079e7-124">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `HTML page` and name it index.html</span></span>
2.  <span data-ttu-id="079e7-125">Lägg till följande tooyour teckentabellen hello:</span><span class="sxs-lookup"><span data-stu-id="079e7-125">Add hello following code tooyour page:</span></span>
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling hello page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn tooreference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>

    <!-- hello 'bluebird' and 'fetch' references below are required if you need toorun this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
