---
title: "Azure AD v2 JS SPA interaktiv installation – konfigurera (ARP) | Microsoft Docs"
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten ARP)"
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
ms.openlocfilehash: 708f4ff606d79639de979918a9cacd4ed75db311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="f0a47-103">Lägg till programmets registreringsinformation i appen</span><span class="sxs-lookup"><span data-stu-id="f0a47-103">Add the application’s registration information to your App</span></span>

<span data-ttu-id="f0a47-104">I det här steget måste du konfigurera omdirigerings-URL för din registreringsinformation för programmet och sedan lägga till det program-Id i JavaScript SPA-programmet.</span><span class="sxs-lookup"><span data-stu-id="f0a47-104">In this step, you need to configure the Redirect URL of your application registration information and then add the Application Id to your JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="f0a47-105">Konfigurera omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="f0a47-105">Configure redirect URL</span></span>

<span data-ttu-id="f0a47-106">Konfigurera den `Redirect URL` fältet ovan med URL-Adressen för index.html sidan baserat på din webbserver och klicka sedan på *uppdatering*.</span><span class="sxs-lookup"><span data-stu-id="f0a47-106">Configure the `Redirect URL` field above with the URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="f0a47-107">Visual Studio-instruktionerna för att skaffa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="f0a47-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="f0a47-108">Följ anvisningarna nedan om du vill hämta omdirigerings-URL:</span><span class="sxs-lookup"><span data-stu-id="f0a47-108">To obtain your redirect URL, follow the instructions below:</span></span>
> 1.    <span data-ttu-id="f0a47-109">I *Solution Explorer*projektet och välj titta på den `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på `F4`)</span><span class="sxs-lookup"><span data-stu-id="f0a47-109">In *Solution Explorer*, select the project and look at the `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="f0a47-110">Kopiera värdet från `URL` till Urklipp:</span><span class="sxs-lookup"><span data-stu-id="f0a47-110">Copy the value from `URL` to the clipboard:</span></span><br/> <span data-ttu-id="f0a47-111">![Egenskaper för](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="f0a47-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="f0a47-112">Klistra in värdet som en `Redirect URL` överst i den här sidan, klicka på`Update`</span><span class="sxs-lookup"><span data-stu-id="f0a47-112">Paste the value as a `Redirect URL` on the top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="f0a47-113">Inställningen omdirigerings-URL för Python</span><span class="sxs-lookup"><span data-stu-id="f0a47-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="f0a47-114">För Python, kan du ange webben serverport via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="f0a47-114">For Python, you can set the web server port via command line.</span></span> <span data-ttu-id="f0a47-115">Den interaktiva installationen använder port 8080 för referens men kan använda någon annan port som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f0a47-115">This guided setup uses the port 8080 for reference but feel free to use any other port available.</span></span> <span data-ttu-id="f0a47-116">I varje fall följer du anvisningarna nedan för att ställa in en omdirigerings-URL i registreringsinformation program:</span><span class="sxs-lookup"><span data-stu-id="f0a47-116">In any case, follow the instructions below to set up a redirect URL in the application registration information:</span></span><br/>
> <span data-ttu-id="f0a47-117">Ange `http://localhost:8080/` som en `Redirect URL` ovanpå den här sidan, eller Använd `http://localhost:[port]/` om du använder en anpassad TCP-port (där *[port]* är anpassade TCP-portnummer), och klicka sedan på ”Uppdatera”</span><span class="sxs-lookup"><span data-stu-id="f0a47-117">Set `http://localhost:8080/` as a `Redirect URL` on the top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is the custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="f0a47-118">Konfigurera JavaScript SPA-program</span><span class="sxs-lookup"><span data-stu-id="f0a47-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="f0a47-119">Skapa en fil med namnet `msalconfig.js` som innehåller programmet registreringsinformation.</span><span class="sxs-lookup"><span data-stu-id="f0a47-119">Create a file named `msalconfig.js` containing the application registration information.</span></span> <span data-ttu-id="f0a47-120">Om du använder Visual Studio, markera projekt (rotmapp projekt), högerklicka och välj: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="f0a47-120">If you are using Visual Studio, select the project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="f0a47-121">Ge den namnet`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="f0a47-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="f0a47-122">Lägg till följande kod i din `msalconfig.js` fil:</span><span class="sxs-lookup"><span data-stu-id="f0a47-122">Add the following code to your `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
