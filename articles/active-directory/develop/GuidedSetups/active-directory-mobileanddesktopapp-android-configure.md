---
title: "aaaAzure AD v2 Android komma igång - konfigurera | Microsoft Docs"
description: "Hur en Android-app kan få en åtkomst-token och anropa API: erna som kräver åtkomst-token från Azure Active Directory v2 slutpunkten eller Microsoft Graph API"
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
ms.openlocfilehash: e14796c37ab0c30d948b6f783dac80059375afa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Skapa ett program (snabb)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Ange ett namn för ditt program och din e-post
3.  Kontrollera att hello alternativ för interaktiv installation är markerat
4.  Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Lägg till din registrering information tooyour lösning (Avancerat)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program
2. Ange ett namn för ditt program och din e-post 
3. Kontrollera att hello alternativet för interaktiv installation är markerat
4. Klicka på `Add Platform`och välj `Native Application` och tryck på Spara
5.  Öppna `MainActivity` (under `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Ersätt hello *[Ange hello program-Id här]* i hello rad som börjar med `final static String CLIENT_ID` med hello program-ID som du just har registrerat:

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Öppna `AndroidManifest.xml` (under `app`  >  `manifests`) Lägg till hello följande aktivitet för`manifest\application` nod. Detta registrerar en `BrowserTabActivity` tooallow hello OS tooresume programmet när du har slutfört hello autentisering:
</li>
</ol>

```xml
<!--Intent filter toocapture System Browser calling back tooour app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, hello scheme should be similar too'msal[appId]' -->
        <data android:scheme="msal[Enter hello application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
I hello `BrowserTabActivity`, Ersätt `[Enter hello application Id here]` med hello program-ID.
</li>
</ol>
