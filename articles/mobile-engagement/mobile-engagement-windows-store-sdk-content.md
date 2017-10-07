---
title: "aaaWindows universella appar SDK innehåll"
description: "Läs mer om hello innehållet i hello Windows Universal-appar SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a8013d2433c0be62d737c8bc6e8360ed79bbe532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-content"></a>Windows Universal SDK för appar innehåll
Det här dokumentet listar och beskriver hello innehåll som distribueras av hello SDK i ditt program.

## <a name="hello-resources-folder"></a>Hej `/Resources` mapp
Den här mappen innehåller alla hello-resurser som krävs för Mobile Engagement. Du kan också anpassa dem toofit din app.

* `EngagementConfiguration.xml`: Hej Mobile Engagement konfigurationsfil, det är där du kan anpassa Mobile Engagement-inställningar (Mobile Engagement anslutningssträngen, rapporten krascher...).

### <a name="html-folder"></a>/ HTML-mappen
* `EngagementNotification.html`: hello `Notification` webbdesign visa html för sidhuvud i appen.
* `EngagementAnnouncement.html`: hello `Announcement` webbdesign visa html för app mellan enskilda muskler vyer.

### <a name="images-folder"></a>/ Images mapp
* `EngagementIconNotification.png`: hello varumärkesikon som visas på hello till vänster i ett meddelande, ersätter det här objektet med varumärken-ikonen.
* `EngagementIconOk.png`: hello `Ok` ikon för hello reach innehållssidor för hello åtgärd eller validering knapp.
* `EngagementIconNOK.png`: hello `NOK` ikonen som används när hello verifiering för hello reach innehållssidor är inaktiverat.
* `EngagementIconClose.png`: hello `Close` ikonen för hello nå meddelanden och innehållet för hello Ignorera knappen.

### <a name="overlay-folder"></a>/overlay mapp
* `EngagementPageOverlay.cs`: hello överlägget sidan ansvarar för att lägga till hello Engagement nå i appen UI tooits underordnade.

