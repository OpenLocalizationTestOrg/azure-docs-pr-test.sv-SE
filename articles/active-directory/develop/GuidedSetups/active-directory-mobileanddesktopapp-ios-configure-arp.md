---
title: "aaaAzure AD v2 iOS komma igång - konfigurera ARP () | Microsoft Docs"
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
ms.openlocfilehash: e5087e13160243d808b1d02771fa66fb332cfad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="925a6-103">Lägg till hello programmet registrering information tooyour app</span><span class="sxs-lookup"><span data-stu-id="925a6-103">Add hello application’s registration information tooyour app</span></span>

<span data-ttu-id="925a6-104">I det här steget måste tooadd hello program-Id tooyour projektet:</span><span class="sxs-lookup"><span data-stu-id="925a6-104">In this step, you need tooadd hello Application Id tooyour project:</span></span>

1.  <span data-ttu-id="925a6-105">I `ViewController.swift`, Ersätt hello rad som börjar med '`let kClientID`' med:</span><span class="sxs-lookup"><span data-stu-id="925a6-105">In `ViewController.swift`, replace hello line starting with '`let kClientID`' with:</span></span>
```swift
let kClientID = "[Enter hello application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="925a6-106">CTRL + klicka <code>Info.plist</code> toobring upp hello popup-menyn och klicka sedan på: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="925a6-106">Control+click <code>Info.plist</code> toobring up hello contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="925a6-107">Under hello <code>dict</code> rot nod, lägga till hello följande:</span><span class="sxs-lookup"><span data-stu-id="925a6-107">Under hello <code>dict</code> root node, add hello following:</span></span>
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
            <string>msal[Enter hello application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
