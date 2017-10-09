---
title: "aaaAdd autentisering på Android med Mobile Apps | Microsoft Docs"
description: "Lär dig hur toouse hello Mobile Apps-funktionen i Azure App Service tooauthenticate användare av din Android-app via en mängd olika identitetsleverantörer, inklusive Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a>Lägg till autentisering tooyour Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Sammanfattning
I kursen får du lägga till autentisering toohello todolist snabbstartsprojekt på Android med hjälp av en stöds identitetsleverantör. Den här kursen är baserad på hello [Kom igång med Mobile Apps] självstudien måste du slutföra först.

## <a name="register"></a>Registrera din app för autentisering och konfigurera Azure App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL

Säker autentisering måste du definiera en ny URL-schema för din app. Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar. I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela. Du kan dock använda alla URL-schema som du väljer. Det bör vara unikt tooyour mobila program. tooenable hello omdirigering på serversidan hello:

1. I hello [Azure portal], väljer du din Apptjänst.

2. Klicka på hello **autentisering / auktorisering** menyalternativet.

3. I hello **tillåtna externa omdirigerings-URL: er**, ange `appname://easyauth.callback`.  Hej _appname_ i den här strängen är hello URL-schema för din mobila program.  Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).  Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.

4. Klicka på **OK**.

5. Klicka på **Spara**.

## <a name="permissions"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Öppna hello-projektet som du har slutfört med hello kursen i Android Studio [Kom igång med Mobile Apps]. Från hello **kör** -menyn klickar du på **kör app**, och kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.

     Det här undantaget inträffar eftersom hello app försök tooaccess hello tillbaka som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.

Därefter uppdaterar du hello app tooauthenticate användare innan du begär resurser från hello Mobile Apps tillbaka avslutas. 

## <a name="add-authentication-toohello-app"></a>Lägg till autentisering toohello app
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>Cache-autentiseringstoken på hello-klient
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:

* [Lägg till push-meddelanden tooyour Android app](app-service-mobile-android-get-started-push.md).
  Lär dig hur tooconfigure Mobile Apps tillbaka avslutas toouse Azure notification hubs toosend push-meddelanden.
* [Aktivera offlinesynkronisering av din Android-app](app-service-mobile-android-get-started-offline-data.md).
  Lär dig hur tooadd offline stöder tooyour appen med hjälp av en Mobile Apps-serverdel. Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Kom igång med Mobile Apps]: app-service-mobile-android-get-started.md
