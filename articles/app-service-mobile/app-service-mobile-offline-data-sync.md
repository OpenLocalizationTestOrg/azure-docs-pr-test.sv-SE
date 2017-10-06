---
title: aaaOffline datasynkronisering i Azure Mobile Apps | Microsoft Docs
description: "Konceptuell referens- och översikt över funktionen för synkronisering av hello offlinedata för Azure Mobile Apps"
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 58673240ba433651faf1f619ca5da33dd6459d2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Datasynkronisering offline i Azure Mobile Apps
## <a name="what-is-offline-data-sync"></a>Vad är datasynkronisering offline?
Datasynkronisering offline är en klient och server SDK-funktionen i Azure Mobilappar som gör det lätt för utvecklare toocreate appar som fungerar utan en nätverksanslutning.

När din app är i offlineläge kan du fortfarande skapa och ändra data som sparas tooa lokalt Arkiv. När hello appen är online igen, synkroniserar lokala ändringar med din mobilappsserverdel i Azure. hello funktionen omfattar också stöd för att upptäcka konflikter när hello samma post ändras på både hello klienten och hello backend. Konflikter kan sedan hanteras på hello server eller hello-klienten.

Offlinesynkronisering har flera fördelar:

* Förbättra app svarstider genom cachelagring serverdata lokalt på hello-enhet
* Skapa robust appar som kan vara användbar när det finns problem med nätverk
* Tillåt end användare toocreate och ändra data, även om det finns ingen nätverksåtkomst stöd för scenarier med lite eller ingen anslutning
* Synkronisera data över flera enheter och identifiera konflikter när hello samma post ändras av två enheter
* Begränsa nätverksanvändning på tidsfördröjning eller förbrukade nätverk

hello följande kurser visar hur tooadd offline synkroniseras tooyour mobila klienter som använder Azure Mobile Apps:

* [Android: Aktivera offlinesynkronisering]
* [Apache Cordova: Aktivera offlinesynkronisering](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: aktivera offlinesynkronisering]
* [Xamarin-iOS: aktivera offlinesynkronisering]
* [Xamarin Android: Aktivera offlinesynkronisering]
* [Xamarin.Forms: Aktivera offlinesynkronisering](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Uwp: aktivera offlinesynkronisering]

## <a name="what-is-a-sync-table"></a>Vad är en tabell med synkronisering?
tooaccess hello ”/ tabeller” endpoint hello Azure Mobile klient-SDK: er anger gränssnitt exempelvis `IMobileServiceTable` (.NET klient SDK) eller `MSTable` (iOS-klient). API: erna anslutas direkt toohello Azure mobilappsserverdel och misslyckas om hello klientenhet inte har en nätverksanslutning.

toosupport användning offline, din app ska i stället använda hello *sync tabell* API: er, t.ex `IMobileServiceSyncTable` (.NET klient SDK) eller `MSSyncTable` (iOS-klient). Alla hello samma CRUD-åtgärder (skapa, läsa, uppdatera, ta bort) arbeta mot sync tabell API: er, förutom nu de läsa från eller skriva tooa *lokalt Arkiv*. Hello lokalt arkiv måste initieras innan alla synkroniseringsåtgärder tabellen kan utföras.

## <a name="what-is-a-local-store"></a>Vad är ett lokalt Arkiv?
Ett lokalt Arkiv är hello persistencelager på hello klientenhet. hello Azure Mobile Apps klient-SDK: er anger en lokal lagra implementering. Den är baserad på SQLite i Windows, Xamarin och Android. På iOS, är den baserad på grundläggande information.

toouse hello SQLite-implementering baserad på Windows Phone eller Windows Store 8.1, behöver du tooinstall filnamnstillägget SQLite. Mer information finns i [Uwp: aktivera offlinesynkronisering]. Android och iOS levereras med en version av SQLite i hello enhetens operativsystem, så det inte är nödvändigt tooreference en egen version av SQLite.

Utvecklare kan också implementeras sina egna lokalt Arkiv. Du kan till exempel definiera ett lokalt arkiv som använder SQLCipher för kryptering om du vill toostore data i ett krypterat format på hello mobila klienten.

## <a name="what-is-a-sync-context"></a>Vad är en kontext som synkronisering?
A *sync kontexten* är associerad med ett objekt för mobila (t.ex `IMobileServiceClient` eller `MSClient`) och spårar ändringar som görs med synkronisering tabeller. hello sync kontexten upprätthåller en *åtgärden kön*, vilket håller en sorterad lista över CUD åtgärder (skapa, uppdatera, ta bort) som senare skickas toohello server.

Ett lokalt arkiv som har associerats med hello sync kontext med hjälp av en initieringsmetoden som `IMobileServicesSyncContext.InitializeAsync(localstore)` i hello [.NET klient-SDK].

## <a name="how-sync-works"></a>Hur offline synkronisering fungerar
När du använder sync tabeller, kontrollerar klientkoden när lokala ändringar synkroniseras med en mobilappsserverdel i Azure. Inget skickas toohello backend tills det är ett anrop för*push* lokala ändringar. På liknande sätt hello lokalt Arkiv fylls med nya data bara när det är ett anrop för*pull* data.

* **Push**: Push är en åtgärd på hello sync kontext och skickar alla CUD ändringar sedan den senaste hello-push. Observera att det är inte möjlig toosend endast en enskild tabell ändringar eftersom annars operations kunde skickas i rätt ordning. Push kör en serie av REST-anrop tooyour Azure mobilappsserverdel, som i sin tur ändrar server-databas.
* **Hämtar**: Pull utförs på grundval av per tabell och kan vara anpassade med en fråga tooretrieve endast en delmängd av hello serverdata. hello Azure Mobile client SDK: er och infoga hello resulterande data i hello lokalt Arkiv.
* **Implicit push-meddelanden**: om en pull körs mot en tabell som har väntande uppdateringar lokala hello pull först kör en `push()` på hello sync-kontext. Den här push bidrar till att minimera konflikter mellan ändringar som redan finns i kön och nya data från hello-servern.
* **Inkrementell synkronisering**: hello första parametern toohello pull-åtgärd är en *Frågenamnet* som används endast på hello-klienter. Om du använder en icke-null Frågenamnet hello Azure Mobile SDK utför en *inkrementell synkronisering*. Varje gång en pull-åtgärd returnerar en uppsättning resultat hello senaste `updatedAt` tidsstämpel som resultatmängden lagras i hello SDK lokalt systemtabeller. Efterföljande pull operations hämta bara poster efter tidsstämpeln.

  toouse inkrementell synkronisering, servern måste returnera meningsfulla `updatedAt` värden och måste också ha stöd för sortering av det här fältet. Eftersom hello SDK lägger till sin egen sortera på hello updatedAt fält, kan du använda en pull-fråga som har sin egen `orderBy` satsen.

  hello frågans namn kan vara valfri sträng som du väljer, men det måste vara unikt för varje logisk fråga i din app.
  Annars olika pull-åtgärder kan du skriva över hello samma inkrementell synkronisering tidsstämpel och dina frågor kan returnera felaktiga resultat.

  Om hello frågan har en parameter, är ett sätt toocreate ett unikt namn tooincorporate hello parametervärdet.
  Om du filtrerar på användar-ID kunde till exempel frågans namn på följande sätt (i C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Om du vill tooopt utanför inkrementell synkronisering, skicka `null` som hello fråga-ID. I så fall måste alla poster som hämtas vid varje anrop för`PullAsync`, som är potentiellt ineffektiv.
* **Rensa**: du kan avmarkera hello innehållet i hello lokalt Arkiv `IMobileServiceSyncTable.PurgeAsync`.
  Rensa kan vara nödvändigt om du har inaktuella data i hello klientdatabasen eller om du inte vill toodiscard alla väntande ändringar.

  En rensning tar bort en tabell från hello lokalt Arkiv. Om det finns åtgärder som väntar på synkronisering med hello server-databas, hello Rensa returnerar ett undantag såvida inte hello *tvinga Rensa* parameter har angetts.

  Som ett exempel på inaktuella data på hello klienten anta att i hello todo-list ”exempel Device1 hämtar endast objekt som inte är slutförda. Ett todoitem ”köpa mjölk” är markerad på hello server har slutförts av en annan enhet. Device1 har dock fortfarande hello ”köpa mjölk” todoitem i lokala arkivet eftersom det bara hämta objekt som inte är markerat som slutförda. En rensning tar bort inaktuella objektet.

## <a name="next-steps"></a>Nästa steg
* [iOS: aktivera offlinesynkronisering]
* [Xamarin-iOS: aktivera offlinesynkronisering]
* [Xamarin Android: Aktivera offlinesynkronisering]
* [Uwp: aktivera offlinesynkronisering]

<!-- Links -->
[.NET klient-SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Aktivera offlinesynkronisering]: app-service-mobile-android-get-started-offline-data.md
[iOS: aktivera offlinesynkronisering]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin-iOS: aktivera offlinesynkronisering]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Aktivera offlinesynkronisering]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Uwp: aktivera offlinesynkronisering]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
