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
ms.openlocfilehash: eaa41805c92212154ee8d51d3eb3aee1202eef1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="15164-103">Lägg till hello programmet registrering information tooyour app</span><span class="sxs-lookup"><span data-stu-id="15164-103">Add hello application’s registration information tooyour app</span></span>

<span data-ttu-id="15164-104">I det här steget måste tooadd hello klient-ID tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="15164-104">In this step, you need tooadd hello Client ID tooyour project.</span></span>

1.  <span data-ttu-id="15164-105">Öppna `MainActivity` (under `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="15164-105">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
2.  <span data-ttu-id="15164-106">Ersätt hello rad som börjar med `final static String CLIENT_ID` med:</span><span class="sxs-lookup"><span data-stu-id="15164-106">Replace hello line starting with `final static String CLIENT_ID` with:</span></span>
```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
3. <span data-ttu-id="15164-107">Öppna:`app` > `manifests` > `AndroidManifest.xml`</span><span class="sxs-lookup"><span data-stu-id="15164-107">Open: `app` > `manifests` > `AndroidManifest.xml`</span></span>
4. <span data-ttu-id="15164-108">Lägg till följande aktivitet för hello`manifest\application` nod.</span><span class="sxs-lookup"><span data-stu-id="15164-108">Add hello following activity too`manifest\application` node.</span></span> <span data-ttu-id="15164-109">Den här registrera en `BrowserTabActivity` tooallow hello OS tooresume programmet när du har slutfört hello autentisering:</span><span class="sxs-lookup"><span data-stu-id="15164-109">This register a `BrowserTabActivity` tooallow hello OS tooresume your application after completing hello authentication:</span></span>

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

### <a name="what-is-next"></a><span data-ttu-id="15164-110">Vad är nästa</span><span class="sxs-lookup"><span data-stu-id="15164-110">What is Next</span></span>

[<span data-ttu-id="15164-111">Testa och validera</span><span class="sxs-lookup"><span data-stu-id="15164-111">Test and Validate</span></span>](active-directory-mobileanddesktopapp-android-test.md)
