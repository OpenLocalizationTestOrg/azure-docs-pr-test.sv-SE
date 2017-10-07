---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Xamarin iOS)"
description: "Lär dig hur toouse Apptjänst Mobile App toocache och synkronisera offlinedata i ditt Xamarin iOS-program"
documentationcenter: xamarin
author: ggailey777
manager: cfowler
editor: 
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5243f2d292377d8de103a40f45d649394f11b97c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Aktivera offlinesynkronisering för en Xamarin.iOS-mobilapp
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobilappar för Xamarin.iOS. Offlinesynkronisering kan slutet användare toointeract med en mobil app – visa, lägga till eller ändra data, även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas. När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.

I den här självstudiekursen uppdatera hello Xamarin.iOS-app-projekt från [skapa en Xamarin iOS-app] toosupport hello offline funktioner i Azure Mobile Apps. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello data access-tilläggspaket i projektet. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="update-hello-client-app-toosupport-offline-features"></a>Uppdatera hello app toosupport offline klientfunktioner
Azure Mobile App offline funktionerna kan du toointeract med en lokal databas när du är i ett offline-scenario. toouse funktionerna i appen, initiera en [SyncContext] tooa lokalt Arkiv. Referera till tabellen via hello [IMobileServiceSyncTable]-gränssnittet. SQLite används som hello lokala arkivet på hello enhet.

1. Öppna hello NuGet-Pakethanteraren i hello-projekt som du har slutfört i hello [skapa en Xamarin iOS-app] kursen, och sedan söka efter och installera hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketet.
2. Öppna hello QSTodoService.cs-filen och ta bort kommentarerna hello `#define OFFLINE_SYNC_ENABLED` definition.
3. Bygg och kör hello klientapp. hello appen fungerar hello samma som det gjorde före du aktiverat offlinesynkronisering. Hello lokala databasen fylls nu med data som kan användas i ett offline-scenario.

## <a name="update-sync"></a>Uppdatera hello app toodisconnect från hello serverdel
I det här avsnittet kan du dela hello anslutning tooyour Mobilapp backend toosimulate en offline-situation. När du lägger till dataobjekt visar din undantagshanterare hello appen är i offlineläge. I det här tillståndet nya objekt som lagts till i hello lokala lagra och kommer att synkroniseras om du vill hello mobilappsserverdel när push kör nästa gång i anslutet tillstånd.

1. Redigera QSToDoService.cs i hello delat projekt. Ändra hello **applicationURL** toopoint tooan ogiltig URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Du kan också visa offline beteende genom att inaktivera Wi-Fi och mobila nätverk på hello enhet eller använda Flygplansläge.
2. Skapa och köra hello app. Observera synkroniseringen misslyckades vid uppdatering hello när appen startas.
3. Ange nya objekt och Lägg märke till att push misslyckas med status [CancelledByNetworkError] varje gång du klickar på **spara**. Dock finns hello nya todo-objekt i hello lokalt Arkiv tills de flyttas toohello mobilappsserverdel.  I en ansluten app om du förhindra undantagen hello-klientappen fungerar som om det är fortfarande toohello mobilappsserverdel.
4. Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.
5. (Valfritt) Om du har Visual Studio installerat på en dator kan du öppna **Server Explorer**. Navigera tooyour databasen i **Azure**-> **SQL-databaser**. Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**. Nu kan du bläddra tooyour SQL-databastabell och dess innehåll. Kontrollera att hello data i hello backend-databas inte har ändrats.
6. (Valfritt) Använd ett REST-verktyg som Fiddler eller Postman tooquery din mobila serverdel med en GET-fråga i formuläret `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Uppdatera hello app tooreconnect din mobilappsserverdel
Återanslut hello app toohello mobilappserverdel i det här avsnittet. Det här simulerar hello app flyttar från en offline-tillstånd tooan online tillstånd med hello mobilappsserverdel.   Om du simulerade hello brott på nätverket genom att inaktivera nätverksanslutningen, behövs inga kodändringar.
Aktivera hello nätverket igen.  När du först kör programmet hello hello `RefreshDataAsync` metoden anropas. Detta i sin tur anropar `SyncAsync` toosync din lokala lagra med hello backend-databas.

1. Öppna QSToDoService.cs i hello delat projekt och återställa din ändring av hello **applicationURL** egenskapen.
2. Bygg och kör hello app. hello app synkroniserar dina lokala ändringar med hello Azure mobilappsserverdel med push och pull-åtgärder när hello `OnRefreshItemsSelected` körs metoden.
3. (Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler. Meddelande hello data har synkroniserats mellan hello Azure Mobile App backend-databas och hello lokalt Arkiv.
4. I hello appen klickar du på Kontrollera hello rutan bredvid några objekt toocomplete dem i hello lokala arkivet.

   `CompleteItemAsync`anrop `SyncAsync` toosync varje slutförd objektet med hello mobilappsserverdel. `SyncAsync`anropar både sändning och mottagning.
   **När du kör en pull mot en tabell hello klienten har gjort ändringar i, en push på hello klientkontexten synkronisering utförs alltid först automatiskt**. hello implicit push säkerställer att alla tabeller i hello lokalt Arkiv tillsammans med relationer förblir konsekventa. Mer information om det här problemet finns i [Offline datasynkronisering i Azure Mobile Apps].

## <a name="review-hello-client-sync-code"></a>Granska hello klientkod för synkronisering
Hej Xamarin klient-projektet som du hämtade när du slutfört självstudiekursen hello [skapa en Xamarin iOS-app] redan innehåller kod som stöder offline synkronisering med en lokal SQLite-databas. Här är en kort översikt över vad ingår redan i självstudiekursen hello-kod. En översikt över funktionen hello finns [Offline datasynkronisering i Azure Mobile Apps].

* Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras. hello lokalt Arkiv databasen initieras när `QSTodoListViewController.ViewDidLoad()` kör `QSTodoService.InitializeStoreAsync()`. Den här metoden skapar en ny lokal SQLite-databas med hello `MobileServiceSQLiteStore` klass som tillhandahålls av hello Azure Mobile App klient-SDK.

    Hej `DefineTable` metoden skapar en tabell i hello lokala arkivet som matchar hello fält i hello tillhandahålls typen `ToDoItem` i det här fallet. hello typen har ingen tooinclude alla hello-kolumner i hello fjärrdatabasen. Det är möjligt toostore bara en delmängd med kolumner.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses hello default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* Hej `todoTable` tillhör `QSTodoService` är av hello `IMobileServiceSyncTable` Skriv i stället för `IMobileServiceTable`. Hej IMobileServiceSyncTable dirigerar alla skapa, läsa, uppdatera och ta bort databasen för lokal lagring (CRUD) tabell operations toohello.

    Du bestämmer dig för när ändringarna är pushas toohello Azure mobilappsserverdel genom att anropa `IMobileServiceSyncContext.PushAsync()`. hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när `PushAsync` anropas.

    hello tillhandahålls koden anropar `QSTodoService.SyncAsync()` toosync när hello todoitem listan uppdateras eller ett todoitem läggs till eller slutförts. Appen synkroniseringar efter alla lokala ändringar. Om en pull körs mot en tabell som har väntande uppdateringar lokala spåras av hello sammanhanget, utlöser pull åtgärden automatiskt en kontext push först.

    Hello tillhandahålls kod, alla poster i hello remote `TodoItem` tabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för`PushAsync`. Mer information finns i avsnittet hello *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>Ytterligare resurser
* [Datasynkronisering offline i Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK ta][8]

<!-- Images -->

<!-- URLs. -->
[Skapa en Xamarin iOS-app]: app-service-mobile-xamarin-ios-get-started.md
[Datasynkronisering offline i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
