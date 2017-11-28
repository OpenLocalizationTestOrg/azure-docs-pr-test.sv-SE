---
title: aaaAzure AD v2 JS SPA interaktiv installation - konfigurera ARP () | Microsoft Docs
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
ms.openlocfilehash: 157f4e342cd684294e24da6ee1fad8a7c2fc266a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="19bc0-103">Lägg till hello programmet registrering information tooyour App</span><span class="sxs-lookup"><span data-stu-id="19bc0-103">Add hello application’s registration information tooyour App</span></span>

<span data-ttu-id="19bc0-104">I det här steget du behöver tooconfigure hello omdirigerings-URL för din registreringsinformation för programmet och Lägg sedan till hello program-Id tooyour JavaScript SPA program.</span><span class="sxs-lookup"><span data-stu-id="19bc0-104">In this step, you need tooconfigure hello Redirect URL of your application registration information and then add hello Application Id tooyour JavaScript SPA application.</span></span>

### <a name="configure-redirect-url"></a><span data-ttu-id="19bc0-105">Konfigurera omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="19bc0-105">Configure redirect URL</span></span>

<span data-ttu-id="19bc0-106">Konfigurera hello `Redirect URL` fältet ovan med hello URL för index.html sidan baserat på din webbserver och klicka sedan på *uppdatering*.</span><span class="sxs-lookup"><span data-stu-id="19bc0-106">Configure hello `Redirect URL` field above with hello URL for your index.html page based on your web server, then click *Update*.</span></span>


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a><span data-ttu-id="19bc0-107">Visual Studio-instruktionerna för att skaffa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="19bc0-107">Visual Studio instructions for obtaining redirect URL</span></span>
> <span data-ttu-id="19bc0-108">tooobtain omdirigerings-URL, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="19bc0-108">tooobtain your redirect URL, follow hello instructions below:</span></span>
> 1.    <span data-ttu-id="19bc0-109">I *Solution Explorer*, Välj hello projekt och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på `F4`)</span><span class="sxs-lookup"><span data-stu-id="19bc0-109">In *Solution Explorer*, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press `F4`)</span></span>
> 2.    <span data-ttu-id="19bc0-110">Kopiera hello-värde från `URL` toohello Urklipp:</span><span class="sxs-lookup"><span data-stu-id="19bc0-110">Copy hello value from `URL` toohello clipboard:</span></span><br/> <span data-ttu-id="19bc0-111">![Egenskaper för](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span><span class="sxs-lookup"><span data-stu-id="19bc0-111">![Project properties](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)</span></span><br />
> 3.    <span data-ttu-id="19bc0-112">Klistra in hello-värde som en `Redirect URL` hello längst upp på sidan, klicka sedan på`Update`</span><span class="sxs-lookup"><span data-stu-id="19bc0-112">Paste hello value as a `Redirect URL` on hello top of this page, then click `Update`</span></span>

<p/>

> #### <a name="setting-redirect-url-for-python"></a><span data-ttu-id="19bc0-113">Inställningen omdirigerings-URL för Python</span><span class="sxs-lookup"><span data-stu-id="19bc0-113">Setting Redirect URL for Python</span></span>
> <span data-ttu-id="19bc0-114">För Python, kan du ange hello web server-port via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="19bc0-114">For Python, you can set hello web server port via command line.</span></span> <span data-ttu-id="19bc0-115">Den interaktiva installationen använder hello port 8080 för referens men känna sig fria toouse någon annan port som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="19bc0-115">This guided setup uses hello port 8080 for reference but feel free toouse any other port available.</span></span> <span data-ttu-id="19bc0-116">I varje fall Följ hello instruktionerna nedan tooset upp en omdirigerings-URL i hello registreringsinformation för programmet:</span><span class="sxs-lookup"><span data-stu-id="19bc0-116">In any case, follow hello instructions below tooset up a redirect URL in hello application registration information:</span></span><br/>
> <span data-ttu-id="19bc0-117">Ange `http://localhost:8080/` som en `Redirect URL` på hello överst på den här sidan eller använda `http://localhost:[port]/` om du använder en anpassad TCP-port (där *[port]* hello anpassade TCP-portnumret), och klicka sedan på ”Uppdatera”</span><span class="sxs-lookup"><span data-stu-id="19bc0-117">Set `http://localhost:8080/` as a `Redirect URL` on hello top of this page, or use `http://localhost:[port]/` if you are using a custom TCP port (where *[port]* is hello custom TCP port number), and then click 'Update'</span></span>

### <a name="configure-your-javascript-spa-application"></a><span data-ttu-id="19bc0-118">Konfigurera JavaScript SPA-program</span><span class="sxs-lookup"><span data-stu-id="19bc0-118">Configure your JavaScript SPA application</span></span>

1.  <span data-ttu-id="19bc0-119">Skapa en fil med namnet `msalconfig.js` som innehåller information om programmet hello.</span><span class="sxs-lookup"><span data-stu-id="19bc0-119">Create a file named `msalconfig.js` containing hello application registration information.</span></span> <span data-ttu-id="19bc0-120">Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklickar och väljer: `Add`  >  `New Item`  >  `JavaScript File`.</span><span class="sxs-lookup"><span data-stu-id="19bc0-120">If you are using Visual Studio, select hello project (project root folder), right-click and select: `Add` > `New Item` > `JavaScript File`.</span></span> <span data-ttu-id="19bc0-121">Ge den namnet`msalconfig.js`</span><span class="sxs-lookup"><span data-stu-id="19bc0-121">Name it `msalconfig.js`</span></span>
2.  <span data-ttu-id="19bc0-122">Lägg till följande kod tooyour hello `msalconfig.js` fil:</span><span class="sxs-lookup"><span data-stu-id="19bc0-122">Add hello following code tooyour `msalconfig.js` file:</span></span>

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 
