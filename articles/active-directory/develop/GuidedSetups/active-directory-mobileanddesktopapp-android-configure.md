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
## <a name="create-an-application-express"></a><span data-ttu-id="84233-103">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="84233-103">Create an application (Express)</span></span>
<span data-ttu-id="84233-104">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="84233-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="84233-105">Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span><span class="sxs-lookup"><span data-stu-id="84233-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span></span>
2.  <span data-ttu-id="84233-106">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="84233-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="84233-107">Kontrollera att hello alternativ för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="84233-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="84233-108">Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod</span><span class="sxs-lookup"><span data-stu-id="84233-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="84233-109">Lägg till din registrering information tooyour lösning (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="84233-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="84233-110">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="84233-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="84233-111">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program</span><span class="sxs-lookup"><span data-stu-id="84233-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="84233-112">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="84233-112">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="84233-113">Kontrollera att hello alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="84233-113">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="84233-114">Klicka på `Add Platform`och välj `Native Application` och tryck på Spara</span><span class="sxs-lookup"><span data-stu-id="84233-114">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5.  <span data-ttu-id="84233-115">Öppna `MainActivity` (under `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="84233-115">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
6.  <span data-ttu-id="84233-116">Ersätt hello *[Ange hello program-Id här]* i hello rad som börjar med `final static String CLIENT_ID` med hello program-ID som du just har registrerat:</span><span class="sxs-lookup"><span data-stu-id="84233-116">Replace hello *[Enter hello application Id here]* in hello line starting with `final static String CLIENT_ID` with hello application ID you just registered:</span></span>

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="84233-117">Öppna `AndroidManifest.xml` (under `app`  >  `manifests`) Lägg till hello följande aktivitet för`manifest\application` nod.</span><span class="sxs-lookup"><span data-stu-id="84233-117">Open `AndroidManifest.xml` (under `app` > `manifests`) Add hello following activity too`manifest\application` node.</span></span> <span data-ttu-id="84233-118">Detta registrerar en `BrowserTabActivity` tooallow hello OS tooresume programmet när du har slutfört hello autentisering:</span><span class="sxs-lookup"><span data-stu-id="84233-118">This registers a `BrowserTabActivity` tooallow hello OS tooresume your application after completing hello authentication:</span></span>
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
<span data-ttu-id="84233-119">I hello `BrowserTabActivity`, Ersätt `[Enter hello application Id here]` med hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="84233-119">In hello `BrowserTabActivity`, replace `[Enter hello application Id here]` with hello application ID.</span></span>
</li>
</ol>
