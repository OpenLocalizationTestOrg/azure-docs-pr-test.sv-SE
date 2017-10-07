---
title: "aaaAdd autentisering på Apache Cordova med Mobilappar | Microsoft Docs"
description: "Lär dig hur toouse Mobile Apps i Azure App Service tooauthenticate användare i din Apache Cordova-app via en mängd olika identitetsleverantörer, inklusive Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a>Lägg till autentisering tooyour Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Sammanfattning
I kursen får du lägga till autentisering toohello todolist snabbstartsprojekt på Apache Cordova med hjälp av en stöds identitetsleverantör. Den här kursen är baserad på hello [Kom igång med Mobile Apps] självstudien måste du slutföra först.

## <a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Titta på en video som visar liknande steg](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kan du kontrollera att anonym åtkomst tooyour backend har inaktiverats. I Visual Studio:

* Öppna hello-projekt som du skapade när du slutfört självstudiekursen hello [Kom igång med Mobile Apps].
* Kör ditt program i hello **Google Android-emulatorn**.
* Kontrollera att ett oväntat fel i anslutningen visas när hello appen startar.

Därefter uppdatera hello app tooauthenticate användare innan du begär resurser från hello mobilappsserverdel.

## <a name="add-authentication"></a>Lägg till autentisering toohello app
1. Öppna projektet i **Visual Studio**sedan öppnar hello `www/index.html` filen för redigering.
2. Leta upp hello `Content-Security-Policy` meta-tagg i head hello-avsnitt.  Lägg till hello OAuth värden toohello listan över tillåtna källor.

   | Leverantör | Namn på SDK-Provider | OAuth-värden |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.Facebook.com |
   | Google | Google | https://accounts.Google.com |
   | Microsoft | MicrosoftAccount | https://login.live.com |
   | Twitter | Twitter | https://API.Twitter.com |

    Ett exempel innehåll--säkerhetsprincip (implementerat för Azure Active Directory) är följande:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Ersätt `https://login.microsoftonline.com` med hello OAuth-värden från hello föregående tabell.  Mer information om hello innehåll säkerhetsprincip meta-tagg finns hello [innehåll säkerhetsprincip dokumentationen].

    Vissa autentiseringsproviders kräver inte innehåll säkerhetsprincip ändras när den används på lämplig mobila enheter.  Inget innehåll säkerhetsprincip ändringar krävs till exempel vid användning av Google-autentisering på en Android-enhet.

3. Öppna hello `www/js/index.js` filen för redigering, leta upp hello `onDeviceReady()` metod, och under hello klienten skapas kod lägger du till följande kod hello:

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Den här koden ersätter hello befintlig kod som skapar hello tabellreferens och uppdaterar hello Användargränssnittet.

    Hej login() metoden startar autentisering med hello-providern. Hej login() metoden är en async-funktion som returnerar en JavaScript-Promise.  hello resten av hello initieringen placeras inuti hello promise svar så att den inte körs förrän hello login() metod.

4. I hello kod som du just lagt till, ersätta `SDK_Provider_Name` med hello namnet på leverantören inloggningen. Till exempel för Azure Active Directory, använda `client.login('aad')`.
5. Kör projektet.  När hello projektet har slutfört initiering, visar programmet hello OAuth-inloggningssidan för hello valt autentiseringsprovider.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer [om verifiering] med Azure App Service.
* Fortsätta hello kursen genom att lägga till [Push-meddelanden] tooyour Apache Cordova-app.

Lär dig hur toouse hello SDK: er.

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- URLs. -->
[Kom igång med Mobile Apps]: app-service-mobile-cordova-get-started.md
[innehåll säkerhetsprincip dokumentationen]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-meddelanden]: app-service-mobile-cordova-get-started-push.md
[om verifiering]: app-service-mobile-auth.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
