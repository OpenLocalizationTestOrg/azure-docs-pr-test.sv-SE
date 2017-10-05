---
title: "Azure AD v2 Android komma igång - konfigurera | Microsoft Docs"
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
ms.openlocfilehash: c09937582118ebcc5b8cbc1f43a0a2019f2f7a89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="4f279-103">Lägg till programmets registreringsinformation i appen</span><span class="sxs-lookup"><span data-stu-id="4f279-103">Add the application’s registration information to your app</span></span>

<span data-ttu-id="4f279-104">Du måste lägga till klient-ID till ditt projekt i det här steget.</span><span class="sxs-lookup"><span data-stu-id="4f279-104">In this step, you need to add the Client ID to your project.</span></span>

1.  <span data-ttu-id="4f279-105">Öppna `MainActivity` (under `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="4f279-105">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
2.  <span data-ttu-id="4f279-106">Ersätt den rad som börjar med `final static String CLIENT_ID` med:</span><span class="sxs-lookup"><span data-stu-id="4f279-106">Replace the line starting with `final static String CLIENT_ID` with:</span></span>
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. <span data-ttu-id="4f279-107">Öppna:`app` > `manifests` > `AndroidManifest.xml`</span><span class="sxs-lookup"><span data-stu-id="4f279-107">Open: `app` > `manifests` > `AndroidManifest.xml`</span></span>
4. <span data-ttu-id="4f279-108">Lägg till att följande aktiviteter `manifest\application` nod.</span><span class="sxs-lookup"><span data-stu-id="4f279-108">Add the following activity to `manifest\application` node.</span></span> <span data-ttu-id="4f279-109">Den här registrera en `BrowserTabActivity` så att operativsystem och återuppta programmet när autentiseringen har slutförts:</span><span class="sxs-lookup"><span data-stu-id="4f279-109">This register a `BrowserTabActivity` to allow the OS to resume your application after completing the authentication:</span></span>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

### <a name="what-is-next"></a><span data-ttu-id="4f279-110">Vad är nästa</span><span class="sxs-lookup"><span data-stu-id="4f279-110">What is Next</span></span>

[<span data-ttu-id="4f279-111">Testa och validera</span><span class="sxs-lookup"><span data-stu-id="4f279-111">Test and Validate</span></span>](active-directory-mobileanddesktopapp-android-test.md)
