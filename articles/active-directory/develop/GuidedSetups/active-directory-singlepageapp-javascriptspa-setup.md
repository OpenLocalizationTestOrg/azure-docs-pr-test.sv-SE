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
## <a name="setting-up-your-web-server-or-project"></a>Konfigurera webbservern eller projekt

> Föredrar toodownload projekt för det här exemplet i stället? 
> - [Hämta hello Visual Studio-projekt](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> eller
> - [Hämta hello projektfiler](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) för en lokal webbserver, till exempel Python
>
> Och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan den körs.

## <a name="prerequisites"></a>Krav
En lokal webbserver som [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), eller IIS Express integrering med [Visual Studio 2017](https://www.visualstudio.com/downloads/) är obligatoriska toorun interaktiv installationen. 

Anvisningarna i guiden är baserade på både Python och Visual Studio 2017 men känna sig fria toouse andra utvecklingsmiljö eller en webbserver.

## <a name="create-your-project"></a>Skapa projektet 

> ### <a name="option-1-visual-studio"></a>Alternativ 1: Visual Studio 
> Om du använder Visual Studio och skapar ett nytt projekt gör hello nedan toocreate en ny Visual Studio-lösning:
> 1.    I Visual Studio:`File` > `New` > `Project`
> 2.    Under `Visual C#\Web`väljer`ASP.NET Web Application (.NET Framework)`
> 3.    Namnge ditt program och klicka på *OK*
> 4.    Under `New ASP.NET Web Application`väljer`Empty`

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a>Alternativ 2: Python / andra webbservrar
> Kontrollera att du har installerat [Python](https://www.python.org/downloads/), följ hello steg nedan:
> - Skapa en mapp toohost ditt program.


## <a name="create-your-single-page-applications-ui"></a>Skapa sida programmets användargränssnitt
1.  Skapa en *index.html* -filen för JavaScript-SPA. Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklicka och välj: `Add`  >  `New Item`  >  `HTML page` och ge den namnet index.html
2.  Lägg till följande tooyour teckentabellen hello:
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
