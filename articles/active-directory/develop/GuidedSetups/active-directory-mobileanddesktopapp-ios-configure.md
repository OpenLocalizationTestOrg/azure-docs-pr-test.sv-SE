---
title: "Komma igång-aaaAzure AD v2 iOS – konfigurera | Microsoft Docs"
description: "Hur iOS (Swift)-program kan anropa ett API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 537cc7f0de6cd947fe340566c9e93f8bb08d57a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="4f910-103">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="4f910-103">Create an application (Express)</span></span>
<span data-ttu-id="4f910-104">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="4f910-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4f910-105">Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span><span class="sxs-lookup"><span data-stu-id="4f910-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span></span>
2.  <span data-ttu-id="4f910-106">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4f910-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4f910-107">Kontrollera att hello alternativ för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4f910-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="4f910-108">Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod</span><span class="sxs-lookup"><span data-stu-id="4f910-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="4f910-109">Lägg till din registrering information tooyour lösning (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="4f910-109">Add your application registration information tooyour solution (Advanced)</span></span>

1.  <span data-ttu-id="4f910-110">Gå för[Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app)</span><span class="sxs-lookup"><span data-stu-id="4f910-110">Go too[Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app)</span></span>
2.  <span data-ttu-id="4f910-111">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4f910-111">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4f910-112">Kontrollera att hello alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4f910-112">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="4f910-113">Klicka på `Add Platform`och välj `Native Application` och klicka på`Save`</span><span class="sxs-lookup"><span data-stu-id="4f910-113">Click `Add Platform`, then select `Native Application` and click `Save`</span></span>
5.  <span data-ttu-id="4f910-114">Gå tillbaka tooXcode.</span><span class="sxs-lookup"><span data-stu-id="4f910-114">Go back tooXcode.</span></span> <span data-ttu-id="4f910-115">I `ViewController.swift`, Ersätt hello rad som börjar med '`let kClientID`' med hello program-ID som du just har registrerat:</span><span class="sxs-lookup"><span data-stu-id="4f910-115">In `ViewController.swift`, replace hello line starting with '`let kClientID`' with hello application ID you just registered:</span></span>

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
<span data-ttu-id="4f910-116">CTRL + klicka <code>Info.plist</code> toobring upp hello popup-menyn och klicka sedan på: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="4f910-116">Control+click <code>Info.plist</code> toobring up hello contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="4f910-117">Under hello <code>dict</code> rot nod, lägga till hello följande:</span><span class="sxs-lookup"><span data-stu-id="4f910-117">Under hello <code>dict</code> root node, add hello following:</span></span>
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
<span data-ttu-id="4f910-118">Ersätt <i> <code>[Your_Application_Id_Here]</code> </i> med hello program-Id som du precis har registrerats</span><span class="sxs-lookup"><span data-stu-id="4f910-118">Replace <i><code>[Your_Application_Id_Here]</code></i> with hello Application Id you just registered</span></span>
</li>
</ol>
