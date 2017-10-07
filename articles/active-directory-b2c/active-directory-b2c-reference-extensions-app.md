---
title: "Tillägg app – Azure AD B2C | Microsoft Docs"
description: "Återställa hello b2c-tillägg-app"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Tillägg app

När en Azure AD B2C-katalog har skapats kan en app kallas `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` skapas automatiskt i hello ny katalog. Den här appen, enligt tooas hello **b2c-tillägg-app**, visas i *App registreringar*. Den används av hello Azure AD B2C-tjänsten toostore information om användare och anpassade attribut. Om hello appen tas bort, Azure AD B2C kommer inte att fungera på rätt sätt och produktionsmiljön kommer att påverkas.

> [!IMPORTANT]
> Ta inte bort hello b2c-tillägg-app om du planerar att ta bort tooimmediately din klient. Hello app är borttagna mer än 30 dagar, kommer användarinformation att gå förlorade.

## <a name="verifying-that-hello-extensions-app-is-present"></a>Verifiera hello tillägg appen finns

Det finns tooverify som hello b2c-tillägg-app:

1. Klicka på på i din Azure AD B2C-klient **fler tjänster** i hello vänstra navigeringsmenyn.
1. Söka efter och öppna **App registreringar**.
1. Leta efter en app som börjar med **b2c-tillägg-app**

## <a name="recover-hello-extensions-app"></a>Återställa hello tillägg app

Om du av misstag tas bort hello b2c-tillägg-app du har 30 dagar toorecover den. Du kan återställa hello-app med hello Graph API:

1. Bläddra för[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Logga in toohello platsen som en global administratör för hello Azure AD B2C-katalog som du vill toorestore hello bort app för.
1. Utfärda en HTTP GET mot hello URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` = med api-version 1.6. Se till att tooreplace `{tenantName}` med innehavarens namn. Den här åtgärden visar en lista över alla hello-program som har tagits bort inom hello senaste 30 dagarna.
1. Hitta hello program i hello lista där hello namn börjar med ”b2c-tillägg-app” och kopiera dess `objectid` egenskapsvärde.
1. Utfärda en HTTP POST mot hello URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Ersätt hello `{OBJECTID}` del av hello URL: en med hello `objectid` hello föregående steg. 

Du bör nu kunna för[Se hello återställts](#verifying-that-the-extensions-app-is-present) i hello Azure-portalen.

> [!NOTE]
> Ett program kan endast återställas om det har tagits bort inom hello senaste 30 dagarna. Om det är mer än 30 dagar, blir data förlorade permanent. Filen ett supportärende för mer hjälp.
