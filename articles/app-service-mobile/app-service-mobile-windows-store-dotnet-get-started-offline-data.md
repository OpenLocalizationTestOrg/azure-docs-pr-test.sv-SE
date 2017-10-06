---
title: aaaEnable offlinesynkronisering av appen universella Windowsplattformen (UWP) med Mobilappar | Microsoft Docs
description: "Lär dig hur toouse en Azure Mobile App toocache och sync offline data i appen universella Windowsplattformen (UWP)."
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: a9f4ad02e92c2c423f10f07b7f1a4270aafd6c6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Aktivera offlinesynkronisering av Windows-appen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd offline stöder tooa universella Windowsplattformen (UWP)-app med en mobilappsserverdel i Azure. Offlinesynkronisering kan slutet användare toointeract med en mobil app – visa, lägga till eller ändra data – även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas. När hello enheten är online igen, synkroniseras ändringarna med fjärråtkomst hello-serverdelen.

I kursen får du uppdatera hello UWP-app-projekt från hello kursen [skapa en Windows-app] toosupport hello offline funktioner i Azure Mobile Apps. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello data access paket tooyour tilläggsprojekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="requirements"></a>Krav
Den här kursen kräver hello följande förutsättningar:

* Visual Studio 2013 körs på Windows 8.1 eller senare.
* Slutförande av [skapa en Windows-app][Skapa en windows-app].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite för Uwp-utveckling](http://www.sqlite.org/downloads)

## <a name="update-hello-client-app-toosupport-offline-features"></a>Uppdatera hello app toosupport offline klientfunktioner
Azure Mobile App offline funktionerna kan du toointeract med en lokal databas när du är i ett offline-scenario. toouse funktionerna i appen du initiera en [SyncContext] [ synccontext] tooa lokalt Arkiv. Referenstabell via hello [IMobileServiceSyncTable][IMobileServiceSyncTable] gränssnitt. SQLite används som hello lokala arkivet på hello enhet.

1. Installera hello [SQLite runtime för hello Uwp](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. Öppna i Visual Studio hello NuGet package manager för hello UWP-app-projekt som du har slutfört i hello [skapa en Windows-app] kursen.
    Sök efter och installera hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paketet.
3. I Solution Explorer högerklickar du på **referenser** > **Lägg till referens...** >**Universell Windows** > **tillägg**, aktivera båda **SQLite för Uwp** och **Visual C++ 2015 Runtime för Universal Windows Platform appar**.

    ![Lägg till SQLite UWP-referens][1]
4. Öppna hello MainPage.xaml.cs-filen och ta bort kommentarerna hello `#define OFFLINE_SYNC_ENABLED` definition.
5. I Visual Studio trycker du på hello **F5** toorebuild och kör hello-klientappen. hello appen fungerar hello samma som det gjorde före du aktiverat offlinesynkronisering. Hello lokala databasen fylls nu med data som kan användas i ett offline-scenario.

## <a name="update-sync"></a>Uppdatera hello app toodisconnect från hello serverdel
I det här avsnittet kan du dela hello anslutning tooyour Mobilapp backend toosimulate en offline-situation. När du lägger till dataobjekt visar din undantagshanterare hello appen är i offlineläge. I det här tillståndet nya objekt som lagts till i hello lokala lagra och kommer att synkroniseras om du vill hello mobilappsserverdel när push kör nästa gång i anslutet tillstånd.

1. Redigera App.xaml.cs i hello delat projekt. Kommentera hello initieringen av hello **MobileServiceClient** och Lägg till hello följande rad som använder en ogiltig mobila app-URL:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Du kan också visa offline beteende genom att inaktivera Wi-Fi och mobila nätverk på hello enhet eller använda Flygplansläge.
2. Tryck på **F5** toobuild och kör hello-app. Observera synkroniseringen misslyckades vid uppdatering hello när appen startas.
3. Ange nya objekt och Lägg märke till att push misslyckas med en [CancelledByNetworkError] varje gång du klickar på **spara**. Dock finns hello nya todo-objekt i hello lokalt Arkiv tills de flyttas toohello mobilappsserverdel.  I en ansluten app om du förhindra undantagen hello-klientappen fungerar som om det är fortfarande toohello mobilappsserverdel.
4. Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.
5. (Valfritt) Öppna i Visual Studio **Server Explorer**. Navigera tooyour databasen i **Azure**->**SQL-databaser**. Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**. Nu kan du bläddra tooyour SQL-databastabell och dess innehåll. Kontrollera att hello data i hello backend-databas inte har ändrats.
6. (Valfritt) Använd ett REST-verktyg som Fiddler eller Postman tooquery din mobila serverdel med en GET-fråga i formuläret `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Uppdatera hello app tooreconnect din mobilappsserverdel
I det här avsnittet kan du återansluta hello app toohello mobilappsserverdel. De här ändringarna simulera en nätverket återanslutning hello app.

När du först kör programmet hello hello `OnNavigatedTo` händelsehanteraranrop `InitLocalStoreAsync`. Den här metoden i sin tur anropar `SyncAsync` toosync din lokala lagra med hello backend-databas. hello app försöker toosync vid start.

1. Öppna App.xaml.cs i hello delat projekt och Avkommentera din tidigare initieringen av `MobileServiceClient` toouse hello rätt hello mobila app-URL.
2. Tryck på hello **F5** toorebuild och kör hello app. hello app synkroniserar dina lokala ändringar med hello Azure mobilappsserverdel med push och pull-åtgärder när hello `OnNavigatedTo` händelsehanterare körs.
3. (Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler. Meddelande hello data har synkroniserats mellan hello Azure Mobile App backend-databas och hello lokalt Arkiv.
4. I hello appen klickar du på Kontrollera hello rutan bredvid några objekt toocomplete dem i hello lokala arkivet.

   `UpdateCheckedTodoItem`anrop `SyncAsync` toosync varje slutförd objektet med hello mobilappsserverdel. `SyncAsync`anropar både sändning och mottagning. Dock **när du kör en pull mot en tabell hello klienten har gjort ändringar i, en push utförs alltid automatiskt**. Det här beteendet garanteras att alla tabeller i hello lokalt Arkiv tillsammans med relationer förblir konsekventa. Detta kan resultera i ett oväntat push.  Mer information om det här problemet finns i [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="api-summary"></a>API-översikten
toosupport hello offline funktioner i mobila tjänster, använde vi hello [IMobileServiceSyncTable] gränssnitt och initieras [MobileServiceClient.SyncContext] [ synccontext] med en lokal SQLite-databas. När offline, hello normal CRUD-åtgärder för Mobile Apps arbeta som om hello app fortfarande är ansluten medan hello åtgärder mot hello lokalt Arkiv. hello följande metoder är används toosynchronize hello lokalt Arkiv med hello-server:

* **[PushAsync]**  eftersom den här metoden är en medlem av [IMobileServicesSyncContext], toohello backend push-ändringar i alla tabeller. Endast poster med lokala ändringar skickas toohello server.
* **[PullAsync]**  en pull startas från en [IMobileServiceSyncTable]. När det finns ändringar i hello tabell, körs ett implicit push toomake till att alla tabeller i hello lokalt Arkiv tillsammans med relationer förblir konsekventa. Hej *pushOtherTables* parameter styr huruvida andra tabeller i hello kontexten push i en implicit push. Hej *frågan* parametern tar ett [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] eller OData-frågan sträng toofilter hello returnerade data. Hej *fråge-ID* parametern används toodefine inkrementell synkronisering. Mer information finns i [Offline datasynkronisering i Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  appen regelbundet anropa den här metoden toopurge inaktuella data från lokalt Arkiv för hello. Använd hello *tvinga* parameter när du behöver toopurge alla ändringar som inte har synkroniserats.

Mer information om de här koncepten finns [Offline datasynkronisering i Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Mer information
hello följande avsnitt innehåller bakgrundsinformation om hello offlinesynkronisering funktionen för Mobile Apps:

* [offlinesynkronisering Data i Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK ta][8]

<!-- Anchors. -->
[Update hello app toosupport offline features]: #enable-offline-app
[Update hello sync behavior of hello app]: #update-sync
[Update hello app tooreconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[offlinesynkronisering Data i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Skapa en windows-app]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
