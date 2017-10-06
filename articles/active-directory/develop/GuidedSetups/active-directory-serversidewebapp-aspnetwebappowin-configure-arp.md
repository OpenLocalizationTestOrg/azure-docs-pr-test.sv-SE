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
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a>Konfigurera ditt ASP.NET-webbprogram med registreringsinformation Hej program

I det här steget ska du konfigurera ditt projekt toouse SSL och använda hello SSL URL tooconfigure registreringsinformation för ditt program. Sedan lägger du till hello program ' registrering information tooyour lösning via *web.config*.

1.  Välj hello-projekt i Solution Explorer och titta på hello `Properties` fönstret (om du inte ser en egenskapsfönstret trycker du på F4)
2.  Ändra `SSL Enabled` för`True`
3.  Kopiera hello-värde från `SSL URL` ovan och klistra in den i hello `Redirect URL` fältet hello längst upp på sidan och klicka sedan på *uppdatering*:<br/><br/>![Egenskaper för](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
4.  Lägg till följande hello i `web.config` fil i rotens mappen under avsnittet `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
