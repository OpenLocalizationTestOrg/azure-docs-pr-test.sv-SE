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
## <a name="create-an-application-express"></a>Skapa ett program (snabb)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Ange ett namn för ditt program och din e-post
3.  Kontrollera att hello alternativ för interaktiv installation är markerat
4.  Följ hello instruktioner tooadd ett tooyour omdirigerings-URL-program

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Lägg till din registrering information tooyour lösning (Avancerat)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program
2. Ange ett namn för ditt program och din e-post 
3.  Kontrollera att hello alternativet för interaktiv installation är markerat
4.  Klicka på `Add Platform`och välj`Web`
5.  Gå tillbaka tooVisual Studio och markera hello-projekt i Solution Explorer och titta på hello egenskapsfönstret (om du inte ser en egenskapsfönstret trycker du på F4)
6.  Ändra SSL aktiverat för`True`
7.  Kopiera hello SSL-URL och lägger till den här URL: en toohello listan över omdirigerings-URL: er hello Registreringsportal omdirigerings-URL-listan:<br/><br/>![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Lägg till följande hello i `web.config` finns i hello rotmappen under hello avsnittet `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Ersätt `ClientId` med hello program-Id som du precis har registrerats
</li>
<li>
Ersätt `redirectUri` med hello SSL-URL för ditt projekt
</li>
</ol>
<!-- End Docs -->
