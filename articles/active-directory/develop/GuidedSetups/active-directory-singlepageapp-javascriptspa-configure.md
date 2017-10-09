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
## <a name="register-your-application"></a>Registrera ditt program

Det finns flera sätt toocreate ett program, Välj en av dem:

### <a name="option-1-register-your-application-express-mode"></a>Alternativ 1: Registrera ditt program (Express-läge)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:

1.  Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Ange ett namn för ditt program och din e-post
3.  Se till att hello-alternativet *interaktiv installation* kontrolleras
4.  Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod

### <a name="option-2-register-your-application-advanced-mode"></a>Alternativ 2: Registrera ditt program (Avancerat läge)

1. Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program
2. Ange ett namn för ditt program och din e-post 
3. Se till att hello-alternativet *interaktiv installation* är avmarkerat
4.  Klicka på `Add Platform`och välj`Web`
5. Lägg till hello `Redirect URL` som motsvarar toohello programmets URL baserat på din webbserver. Visa hello avsnitt nedan för instruktioner om hur tooset / hämta hello omdirigerings-URL i Visual Studio och Python.
6. Klicka på *spara*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instruktionerna för att skaffa omdirigerings-URL
> Följ hello instruktioner tooobtain omdirigerings-URL:
> 1.    I *Solution Explorer*, Välj hello projekt och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på `F4`)
> 2.    Kopiera hello-värde från `URL` toohello Urklipp:<br/> ![Egenskaper för](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Växla tillbaka toohello *Programregistreringsportalen* och klistra in hello-värde som en `Redirect URL` och klicka på ”Spara”

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Inställningen omdirigerings-URL för Python
> För Python, kan du ange hello web server-port via kommandoraden. Den interaktiva installationen använder hello port 8080 för referens men känna sig fria toouse någon annan port som är tillgängliga. I varje fall Följ hello instruktionerna nedan tooset upp en omdirigerings-URL i hello registreringsinformation för programmet:<br/>
> - Växla tillbaka toohello *Programregistreringsportalen* och ange `http://localhost:8080/` som en `Redirect URL`, eller Använd `http://localhost:[port]/` om du använder en anpassad TCP-port (där *[port]* är hello anpassade TCP-port antalet) och klicka på ”Spara”


#### <a name="configure-your-javascript-spa"></a>Konfigurera din JavaScript SPA

1.  Skapa en fil med namnet `msalconfig.js` som innehåller information om programmet hello. Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklickar och väljer: `Add`  >  `New Item`  >  `JavaScript File`. Ge den namnet`msalconfig.js`
2.  Lägg till följande kod tooyour hello `msalconfig.js` fil:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Ersätt <code>Enter_the_Application_Id_here</code> med hello program-Id som du precis har registrerats
</li>
</ol>
