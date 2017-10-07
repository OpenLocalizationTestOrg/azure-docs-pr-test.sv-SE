---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Xamarin Android)"
description: "Lär dig hur toouse Apptjänst Mobile App toocache och synkronisera offlinedata i Xamarin Android-program"
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 216ba76ae49f583273cc61b63114a415eca2477b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Aktivera offlinesynkronisering av din mobila Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobilappar för Xamarin.Android. Offlinesynkronisering kan slutet användare toointeract med en mobil app – visa, lägga till eller ändra data, även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas.
När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.

I kursen får du uppdatera hello klientprojektet från hello kursen [skapa en Xamarin Android-app] toosupport hello offline funktioner i Azure Mobile Apps. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello data access paket tooyour tilläggsprojekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="update-hello-client-app-toosupport-offline-features"></a>Uppdatera hello app toosupport offline klientfunktioner
Azure Mobile App offline funktionerna kan du toointeract med en lokal databas när du är i ett offline-scenario. toouse funktionerna i appen du initiera en [SyncContext] tooa lokalt Arkiv. Referenstabell via hello [IMobileServiceSyncTable] [IMobileServiceSyncTable] gränssnitt. SQLite används som hello lokala arkivet på hello enhet.

1. Öppna i Visual Studio hello NuGet-Pakethanteraren i hello-projekt som du har slutfört i hello [skapa en Xamarin Android-app] kursen.  Sök efter och installera hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paketet.
2. Öppna hello ToDoActivity.cs-filen och ta bort kommentarerna hello `#define OFFLINE_SYNC_ENABLED` definition.
3. I Visual Studio trycker du på hello **F5** toorebuild och kör hello-klientappen. hello appen fungerar hello samma som det gjorde före du aktiverat offlinesynkronisering. Hello lokala databasen fylls nu med data som kan användas i ett offline-scenario.

## <a name="update-sync"></a>Uppdatera hello app toodisconnect från hello serverdel
I det här avsnittet kan du dela hello anslutning tooyour Mobilapp backend toosimulate en offline-situation. När du lägger till dataobjekt visar din undantagshanterare hello appen är i offlineläge. I det här tillståndet nya objekt som lagts till i hello lokala lagra och synkroniseras till hello mobilappsserverdel när en push körs i ett anslutet tillstånd.

1. Redigera ToDoActivity.cs i hello delat projekt. Ändra hello **applicationURL** toopoint tooan ogiltig URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Du kan också visa offline beteende genom att inaktivera Wi-Fi och mobila nätverk på hello enhet eller använda Flygplansläge.
2. Tryck på **F5** toobuild och kör hello-app. Observera synkroniseringen misslyckades vid uppdatering hello när appen startas.
3. Ange nya objekt och Lägg märke till att push misslyckas med status [CancelledByNetworkError] varje gång du klickar på **spara**. Dock finns hello nya todo-objekt i hello lokalt Arkiv tills de flyttas toohello mobilappsserverdel.  I en ansluten app om du förhindra undantagen hello-klientappen fungerar som om det är fortfarande toohello mobilappsserverdel.
4. Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.
5. (Valfritt) Öppna i Visual Studio **Server Explorer**. Navigera tooyour databasen i **Azure**->**SQL-databaser**. Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**. Nu kan du bläddra tooyour SQL-databastabell och dess innehåll. Kontrollera att hello data i hello backend-databas inte har ändrats.
6. (Valfritt) Använd ett REST-verktyg som Fiddler eller Postman tooquery din mobila serverdel med en GET-fråga i formuläret `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Uppdatera hello app tooreconnect din mobilappsserverdel
Återanslut hello app toohello mobilappserverdel i det här avsnittet. När du först kör programmet hello hello `OnCreate` händelsehanteraranrop `OnRefreshItemsSelected`. Den här metoden anropar `SyncAsync` toosync din lokala lagra med hello backend-databas.

1. Öppna ToDoActivity.cs i hello delat projekt och återställa din ändring av hello **applicationURL** egenskapen.
2. Tryck på hello **F5** toorebuild och kör hello app. hello app synkroniserar dina lokala ändringar med hello Azure mobilappsserverdel med push och pull-åtgärder när hello `OnRefreshItemsSelected` körs metoden.
3. (Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler. Meddelande hello data har synkroniserats mellan hello Azure Mobile App backend-databas och hello lokalt Arkiv.
4. I hello appen klickar du på Kontrollera hello rutan bredvid några objekt toocomplete dem i hello lokala arkivet.

   `CheckItem`anrop `SyncAsync` toosync varje slutförd objektet med hello mobilappsserverdel. `SyncAsync`anropar både sändning och mottagning. **När du kör en pull mot en tabell hello klienten har gjort ändringar i, en push utförs alltid automatiskt**. Detta säkerställer att alla tabeller i det lokala arkivet tillsammans med relationer förblir konsekventa. Detta kan resultera i ett oväntat push. Mer information om det här problemet finns i [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="review-hello-client-sync-code"></a>Granska hello klientkod för synkronisering
Hej Xamarin klient-projektet som du hämtade när du slutfört självstudiekursen hello [skapa en Xamarin Android-app] redan innehåller kod som stöder offline synkronisering med en lokal SQLite-databas. Här är en kort översikt över vad ingår redan i självstudiekursen hello-kod. En översikt över funktionen hello finns [offlinesynkronisering Data i Azure Mobile Apps].

* Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras. hello lokalt Arkiv databasen initieras när `ToDoActivity.OnCreate()` kör `ToDoActivity.InitLocalStoreAsync()`. Den här metoden skapar en lokal SQLite-databas med hjälp av hello `MobileServiceSQLiteStore` klass som tillhandahålls av hello Azure Mobile Apps klient-SDK.

    Hej `DefineTable` metoden skapar en tabell i hello lokala arkivet som matchar hello fält i hello tillhandahålls typen `ToDoItem` i det här fallet. hello typen har ingen tooinclude alla hello-kolumner i hello fjärrdatabasen. Det är möjligt toostore bara en delmängd med kolumner.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code tooinitialize hello SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses hello default conflict handler, which fails on conflict
            // toouse a different conflict handler, pass a parameter tooInitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* Hej `toDoTable` tillhör `ToDoActivity` är av hello `IMobileServiceSyncTable` Skriv i stället för `IMobileServiceTable`. Hej IMobileServiceSyncTable dirigerar alla skapa, läsa, uppdatera och ta bort databasen för lokal lagring (CRUD) tabell operations toohello.

    Du bestämmer dig för när ändringarna är pushas toohello Azure mobilappsserverdel genom att anropa `IMobileServiceSyncContext.PushAsync()`. hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när `PushAsync` anropas.

    hello tillhandahålls koden anropar `ToDoActivity.SyncAsync()` toosync när hello todoitem listan uppdateras eller ett todoitem läggs till eller slutförts. hello kod synkroniseringar efter alla lokala ändringar.

    Hello tillhandahålls kod, alla poster i hello remote `TodoItem` tabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för`PushAsync`. Mer information finns i avsnittet hello *inkrementell synkronisering* i [offlinesynkronisering Data i Azure Mobile Apps].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating hello Mobile Service. Verify hello URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Ytterligare resurser
* [offlinesynkronisering Data i Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK ta][8]

<!-- URLs. -->
[skapa en Xamarin Android-app]: ../app-service-mobile-xamarin-android-get-started.md
[offlinesynkronisering Data i Azure Mobile Apps]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Skapa en Xamarin Android-app]: app-service-mobile-xamarin-android-get-started.md
[Datasynkronisering offline i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
