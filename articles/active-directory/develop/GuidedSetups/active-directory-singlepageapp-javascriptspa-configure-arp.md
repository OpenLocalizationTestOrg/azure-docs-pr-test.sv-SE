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
## <a name="add-hello-applications-registration-information-tooyour-app"></a>Lägg till hello programmet registrering information tooyour App

I det här steget du behöver tooconfigure hello omdirigerings-URL för din registreringsinformation för programmet och Lägg sedan till hello program-Id tooyour JavaScript SPA program.

### <a name="configure-redirect-url"></a>Konfigurera omdirigerings-URL

Konfigurera hello `Redirect URL` fältet ovan med hello URL för index.html sidan baserat på din webbserver och klicka sedan på *uppdatering*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instruktionerna för att skaffa omdirigerings-URL
> tooobtain omdirigerings-URL, följ hello instruktionerna nedan:
> 1.    I *Solution Explorer*, Välj hello projekt och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på `F4`)
> 2.    Kopiera hello-värde från `URL` toohello Urklipp:<br/> ![Egenskaper för](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Klistra in hello-värde som en `Redirect URL` hello längst upp på sidan, klicka sedan på`Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Inställningen omdirigerings-URL för Python
> För Python, kan du ange hello web server-port via kommandoraden. Den interaktiva installationen använder hello port 8080 för referens men känna sig fria toouse någon annan port som är tillgängliga. I varje fall Följ hello instruktionerna nedan tooset upp en omdirigerings-URL i hello registreringsinformation för programmet:<br/>
> Ange `http://localhost:8080/` som en `Redirect URL` på hello överst på den här sidan eller använda `http://localhost:[port]/` om du använder en anpassad TCP-port (där *[port]* hello anpassade TCP-portnumret), och klicka sedan på ”Uppdatera”

### <a name="configure-your-javascript-spa-application"></a>Konfigurera JavaScript SPA-program

1.  Skapa en fil med namnet `msalconfig.js` som innehåller information om programmet hello. Om du använder Visual Studio, Välj hello projekt (projektets rotmapp), högerklickar och väljer: `Add`  >  `New Item`  >  `JavaScript File`. Ge den namnet`msalconfig.js`
2.  Lägg till följande kod tooyour hello `msalconfig.js` fil:

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 
