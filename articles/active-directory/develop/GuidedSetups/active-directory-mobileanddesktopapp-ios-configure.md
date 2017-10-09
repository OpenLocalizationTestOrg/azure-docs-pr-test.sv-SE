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
## <a name="create-an-application-express"></a>Skapa ett program (snabb)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Ange ett namn för ditt program och din e-post
3.  Kontrollera att hello alternativ för interaktiv installation är markerat
4.  Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Lägg till din registrering information tooyour lösning (Avancerat)

1.  Gå för[Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app)
2.  Ange ett namn för ditt program och din e-post
3.  Kontrollera att hello alternativet för interaktiv installation är markerat
4.  Klicka på `Add Platform`och välj `Native Application` och klicka på`Save`
5.  Gå tillbaka tooXcode. I `ViewController.swift`, Ersätt hello rad som börjar med '`let kClientID`' med hello program-ID som du just har registrerat:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
CTRL + klicka <code>Info.plist</code> toobring upp hello popup-menyn och klicka sedan på: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Under hello <code>dict</code> rot nod, lägga till hello följande:
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
Ersätt <i> <code>[Your_Application_Id_Here]</code> </i> med hello program-Id som du precis har registrerats
</li>
</ol>
