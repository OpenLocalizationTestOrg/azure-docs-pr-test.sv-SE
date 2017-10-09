---
title: aaaAuthentication och auktorisering i Azure Mobile Apps | Microsoft Docs
description: "Konceptuell referens- och översikt över hello autentisering / auktorisering funktion för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5255734481ada11afb65982aebe45c2a349402fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Autentisering och auktorisering i Azure Mobile Apps
## <a name="what-is-app-service-authentication--authorization"></a>Vad är App Service autentisering / auktorisering?
> [!NOTE]
> Det här avsnittet kommer att migrerade tooa konsoliderade [App Service autentisering / auktorisering](../app-service/app-service-authentication-overview.md) avsnittet, som omfattar webb-, mobil- och API Apps.
> 
> 

App Service autentisering / auktorisering är en funktion som gör att dina program toolog användare utan kod ändringar som krävs på hello-appserverdelen. Det ger ett enkelt sätt tooprotect programmet och arbete data per användare.

Apptjänst använder federerad identitet som 3 parts **identitetsleverantör** (”IDP”) lagrar konton och autentiserar användare och hello programmet använder den här identiteten i stället för en egen. Apptjänst fem identitetsleverantörer out of box hello: *Azure Active Directory*, *Facebook*, *Google*, *Account*, och *Twitter*. Du kan också expandera det här stödet för dina appar genom att integrera en annan identitetsleverantör eller din egen lösning för anpassad identitet.

Din app kan använda valfritt antal dessa identitetsleverantörer, så du kan ge slutanvändarna alternativ för hur de logga in.

Om du vill tooget igång nu direkt kan se något av följande kurser hello:

* [Lägg till autentisering tooyour iOS-app]
* [Lägg till autentisering tooyour Xamarin.iOS-app]
* [Lägg till autentisering tooyour Xamarin.Android-app]
* [Lägg till autentisering tooyour Windows-app]

## <a name="how-authentication-works"></a>Så här fungerar autentisering
I ordning tooauthenticate med någon av hello identitetsleverantörer, måste du först tooconfigure hello identitet providern tooknow för om ditt program. hello identitetsleverantör ger dig sedan med ID: N och hemligheter som du anger den bakre toohello program. Det har slutförts hello förtroenderelation och gör Apptjänst toovalidate identiteter som tooit.

Dessa steg beskrivs i följande avsnitt hello:

* [Hur tooconfigure din app toouse Azure Active Directory-inloggning]
* [Hur tooconfigure din app toouse Facebook-inloggning]
* [Hur tooconfigure din app toouse Google-inloggning]
* [Hur tooconfigure din app toouse Account inloggning]
* [Hur tooconfigure din app toouse Twitter-inloggning]

Du kan ändra din klient toolog i när allting har konfigurerats på hello serverdel. Det finns två tillvägagångssätt här:

* Klient-SDK med hjälp av en rad med kod, kan hello Mobile Apps logga in användare.
* Dra nytta av en SDK som publicerats av en viss identitet providern tooestablish identitet och få åtkomst till tooApp Service.

> [!TIP]
> De flesta program ska använda en provider SDK tooget Avbrottsfritt mer intern känslan inloggning och tooleverage uppdatera support och andra provider-specifik fördelar.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Hur autentisering utan en provider SDK fungerar
Om du inte vill att tooset upp en provider SDK kan du tillåta Mobile Apps tooperform hello inloggningen för dig. hello Mobile Apps klient-SDK öppnas en web visa toohello provider för att välja och fullständig hello inloggningen. Ibland på bloggar och forum visas detta som anges tooas hello ”server flödet” eller ”server-riktade flödet” sedan hello server hanterar hello inloggningen och hello klient-SDK får aldrig hello providern token.

hello koden behövs toostart detta flöde ingår i hello autentisering självstudier för varje plattform. Hello slutet av hello flödet, hello klient-SDK har en Apptjänst-token och hello token är automatiskt bifogade tooall begäranden toohello backend.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Hur autentisering med provider SDK fungerar
Arbeta med en provider SDK kan hello inloggning toointeract tätare med hello plattform OS hello appen körs på. Detta ger dig också en provider-token och viss användarinformation på hello-klienten, vilket gör det mycket enklare tooconsume graph API: er och anpassa hello användarupplevelsen. Ibland på bloggar och forum kommer att se den här hänvisade tooas hello ”flödet” eller ”klienten dirigeras flödet” eftersom koden på hello-klienten hanterar hello inloggning och hello klientkod har åtkomsttoken tooa provider.

När en provider-token har erhållit måste toobe skickas tooApp Service för verifiering. Hello slutet av hello flödet, hello klient-SDK har en Apptjänst-token och hello token är automatiskt bifogade tooall begäranden toohello backend. hello utvecklare kan också behålla en referens toohello providern token om de vill.

## <a name="how-authorization-works"></a>Så här fungerar auktorisering
App Service autentisering / auktorisering visar flera alternativ för **åtgärd tootake när begäran inte har autentiserats**. Innan koden tar emot en viss begäran, kan ha Apptjänst Kontrollera toosee om hello begäran har autentiserats och om inte, avvisa den och försök toohave hello användare logga in innan du försöker igen.

Ett alternativ är toohave oautentiserade begäranden omdirigera tooone av hello identitetsleverantörer. I en webbläsare, skulle det faktiskt ta hello användaren tooa ny sida. Dock mobila klienten inte omdirigeras på detta sätt och oautentiserade svar får en HTTP *401 obehörig* svar. Detta hello första förfrågan som gör att klienten ska alltid vara toohello inloggningen endpoint och gör sedan anropar tooany andra API: er. Om du försöker toocall en annan API innan du loggar in får klienten ett fel.

Om du inte vill toohave mer detaljerad kontroll över vilka slutpunkter kräver autentisering, du kan också hämta ”ingen åtgärd (Tillåt begäranden)” för oautentiserade begäranden. I så fall måste alla beslut om autentisering uppskjutna tooyour programkod. Du kan också tooallow-toospecific användare baserat på regler för anpassad auktorisering.

## <a name="documentation"></a>Dokumentation
Hej följande kurser visar hur tooadd autentisering tooyour mobila klienter använder App Service:

* [Lägg till autentisering tooyour iOS-app]
* [Lägg till autentisering tooyour Xamarin.iOS-app]
* [Lägg till autentisering tooyour Xamarin.Android-app]
* [Lägg till autentisering tooyour Windows-app]

Hej följande kurser visar hur tooconfigure App tooleverage olika leverantörer:

* [Hur tooconfigure din app toouse Azure Active Directory-inloggning]
* [Hur tooconfigure din app toouse Facebook-inloggning]
* [Hur tooconfigure din app toouse Google-inloggning]
* [Hur tooconfigure din app toouse Account inloggning]
* [Hur tooconfigure din app toouse Twitter-inloggning]

Om du inte vill toouse ett identitetssystem än hello de som anges här, kan du också använda hello [Förhandsgranska stöd för anpassad autentisering i hello .NET server SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Lägg till autentisering tooyour iOS-app]: app-service-mobile-ios-get-started-users.md
[Lägg till autentisering tooyour Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started-users.md
[Lägg till autentisering tooyour Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started-users.md
[Lägg till autentisering tooyour Windows-app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Hur tooconfigure din app toouse Azure Active Directory-inloggning]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hur tooconfigure din app toouse Facebook-inloggning]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hur tooconfigure din app toouse Google-inloggning]: app-service-mobile-how-to-configure-google-authentication.md
[Hur tooconfigure din app toouse Account inloggning]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hur tooconfigure din app toouse Twitter-inloggning]: app-service-mobile-how-to-configure-twitter-authentication.md
