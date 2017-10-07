---
title: "aaaHow tooUse hello JavaScript SDK för Azure Mobile Apps"
description: "Hur tooUse v för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a>Hur tooUse hello JavaScript-klientbiblioteket för Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [JavaScript SDK för Azure Mobile Apps]. Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel och skapa en tabell. I den här guiden fokuserar vi på med hello mobilserverdel i HTML/JavaScript-webbprogram.

## <a name="supported-platforms"></a>Plattformar som stöds
Vi begränsa webbläsare stöd toohello aktuella och senaste versioner av hello större webbläsare: Google Chrome, Microsoft Edge, Microsoft Internet Explorer och Mozilla Firefox.  Vi räknar hello SDK toofunction med relativt moderna webbläsare.

hello paketet distribueras som en Universal JavaScript-modul, så att den stöder globals AMD, och CommonJS format.

## <a name="Setup"></a>Installationen och förutsättningar
Den här handboken förutsätts att du har skapat en serverdel med en tabell. Den här handboken utgår från tabellen hello hello samma schema som hello tabeller i dessa självstudier.

Installera hello Azure Mobile Apps JavaScript SDK kan göras via hello `npm` kommando:

```
npm install azure-mobile-apps-client --save
```

hello biblioteket kan också användas som en ES2015-modul i CommonJS miljöer, till exempel Browserify och Webpack och som en AMD-bibliotek.  Exempel:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Du kan också använda en förskapad hello SDK-version genom att hämta direkt från våra CDN:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Så här: autentisera användare
Azure Apptjänst har stöd för autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account och Twitter. Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare. Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript. Mer information finns i hello [komma igång med autentisering] kursen.

Två autentisering flöden stöds: ett flöde för server och ett klient-flöde.  hello server flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering. hello flödet tillåter djupare integrering med specifika funktioner som enkel inloggning som den förlitar sig på provider-specifik SDK: er.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Så här: konfigurera din mobila App Service för externa omdirigerings-URL.
Flera typer av JavaScript-program använder en loopback-funktionen toohandle OAuth UI flödar.  Dessa funktioner är:

* Kör tjänsten lokalt
* Med hello joniska Framework ladda Live
* Omdirigera tooApp Service för autentisering.

Kör lokalt kan orsaka problem Eftersom som standard App Service-autentisering är endast konfigurerat tooallow åtkomst från din mobilappsserverdel. Använd hello följande steg toochange hello Apptjänst inställningar tooenable autentisering när du kör hello server lokalt:

1. Logga in toohello [Azure-portalen]
2. Navigera tooyour mobilappsserverdel.
3. Välj **resursläsaren** i hello **UTVECKLINGSVERKTYG** menyn.
4. Klicka på **Gå** tooopen hello resursutforskaren för din mobilappsserverdel i en ny flik eller ett fönster.
5. Expandera hello **config** > **authsettings** noden för din app.
6. Klicka på hello **redigera** knappen tooenable redigering av hello resurs.
7. Hitta hello **allowedExternalRedirectUrls** element som ska vara null. Lägg till URL-adresserna i en matris:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersätt hello URL: er i hello matris med hello URL: er för tjänsten, som i det här exemplet är `http://localhost:3000` för hello lokala Node.js exempel tjänsten. Du kan också använda `http://localhost:4400` för hello små service eller några andra URL, beroende på hur din app har konfigurerats.
8. Hello överst på hello-sidan, klickar du på **Skrivskyddstyp**, klicka sedan på **PLACERA** toosave uppdateringarna.

Du måste också tooadd hello samma loopback-URL: er toohello CORS godkända inställningar:

1. Gå tillbaka toohello [Azure-portalen].
2. Navigera tooyour mobilappsserverdel.
3. Klicka på **CORS** i hello **API** menyn.
4. Ange varje URL i hello tom **tillåtna ursprung** textruta.  En ny textruta skapas.
5. Klicka på **spara**

När hello backend uppdateringar, kommer du att kunna toouse hello nya loopback-URL: er i din app.

<!-- URLs. -->
[Azure Mobile Apps Snabbstart]: app-service-mobile-cordova-get-started.md
[komma igång med autentisering]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Azure-portalen]: https://portal.azure.com/
[JavaScript SDK för Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
