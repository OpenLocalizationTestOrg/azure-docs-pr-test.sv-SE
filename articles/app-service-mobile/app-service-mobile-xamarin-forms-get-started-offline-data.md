---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Xamarin.Forms) | Microsoft Docs"
description: "Lär dig hur toouse Apptjänst Mobile App toocache och synkronisera offlinedata i Xamarin.Forms-program"
documentationcenter: xamarin
author: ggailey777
manager: yochayk
editor: 
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: glenga
ms.openlocfilehash: 4b41404fc9507e82068fdf8aa612d57cbeb366bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Aktivera synkronisering offline för din Xamarin.Forms-mobilapp
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobilappar för Xamarin.Forms. Offlinesynkronisering kan slutanvändarna interagera med en mobil app – visa, lägga till eller ändra data, även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas. När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.

Den här kursen är baserad på hello quickstart Xamarin.Forms-lösningen för Mobilappar som du skapar när du har slutfört vägledningen [skapa en Xamarin iOS-app]. hello quickstart lösning för Xamarin.Forms innehåller hello kod toosupport offlinesynkronisering, som bara behöver toobe aktiverat. I den här kursen kan du uppdatera hello quickstart lösning tooturn på hello offline funktioner i Azure Mobile Apps. Vi kan också markera hello offline-specifik kod i hello app. Om du inte använder hello hämtas quickstart lösningen måste du lägga till hello data access paket tooyour tilläggsprojekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][1].

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a>Aktivera offlinesynkronisering funktioner i hello quickstart lösning
hello offlinesynkronisering kod ingår i hello-projekt med hjälp av C# preprocessor-direktiv. När hello **OFFLINE\_SYNC\_AKTIVERAD** symbol har definierats, dessa kodsökvägar ingår i hello build. För Windows-appar måste du också installera hello SQLite plattform.

1. Högerklicka på hello lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i hello-lösning.
2. I hello Solution Explorer, öppna hello TodoItemManager.cs filen från hello-projekt med **bärbara** i hello namn som är bärbara klassbiblioteket projekt, ta sedan bort kommentarerna hello följande preprocessor direktiv:

        #define OFFLINE_SYNC_ENABLED
3. (Valfritt) toosupport Windows-enheter, installera en av följande SQLite runtime paket hello:

   * **Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].
   * **Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].
   * **Uwp** installera [SQLite för hello Universal Windows Universal][5].

     Även om hello Snabbstart inte innehåller en universell Windows-projektet, stöds hello universella Windows-plattformen med Xamarin Forms.
4. (Valfritt) Högerklicka i varje Windows-approjekt **referenser** > **Lägg till referens...** , expandera hello **Windows** mappen > **tillägg**.
    Aktivera lämpliga hello **SQLite för Windows** SDK tillsammans med hello **Visual C++ 2013 Runtime för Windows** SDK.
    Hej varierar SQLite SDK namn något med varje Windows-plattformen.

## <a name="review-hello-client-sync-code"></a>Granska hello klientkod för synkronisering
Här är en kort översikt över vad ingår redan i hello självstudiekursen kod inuti hello `#if OFFLINE_SYNC_ENABLED` direktiven. Offlinesynkronisering-funktionen är i hello TodoItemManager.cs projektfilen i hello bärbara klassbiblioteket projekt. En översikt över funktionen hello finns [Offline datasynkronisering i Azure Mobile Apps][2].

* Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras. hello lokalt Arkiv databasen initieras i hello **TodoItemManager** klasskonstruktor med hjälp av hello följande kod:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Den här koden skapar en ny lokal SQLite-databas med hello **MobileServiceSQLiteStore** klass.

    Hej **DefineTable** metoden skapar en tabell i hello lokala arkivet som matchar hello fält i hello angivna typen.  hello typen har ingen tooinclude alla hello-kolumner i hello fjärrdatabasen. Det är möjligt toostore en delmängd med kolumner.
* Hej **todoTable** i **TodoItemManager** är en **IMobileServiceSyncTable** Skriv i stället för **IMobileServiceTable**. Den här klassen använder hello lokal databas för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder. Du bestämmer dig för när ändringarna är pushas toohello mobilappsserverdel genom att anropa **PushAsync** på hello **IMobileServiceSyncContext**. hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när **PushAsync** anropas.

    hello följande **SyncAsync** metoden anropas toosync med hello mobilappsserverdel:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting tooserver's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Det här exemplet använder enkla felhantering med hello standardprogram för synkronisering. Ett riktigt program skulle hantera hello olika fel som nätverksförhållanden och servern är i konflikt med hjälp av en anpassad **IMobileServiceSyncHandler** implementering.

## <a name="offline-sync-considerations"></a>Offlinesynkronisering överväganden
I exemplet hello hello **SyncAsync** är bara att anropa metoden på Start- och när en synkronisering har begärts.  tooinitiate en synkronisering i en Android eller iOS-app, nedrullningsbara på listan över hello objekt; för Windows, Använd hello **Sync** knappen. I ett riktigt program kan du också göra hello sync utlösare när hello nätverket tillstånd ändras.

När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av hello kontext som hämtar åtgärden automatiskt utlösare en föregående kontexten push. När du uppdaterar, lägga till och slutföra objekt i det här exemplet, kan du utelämna hello explicit **PushAsync** anropa.

I hello tillhandahålls kod alla poster i TodoItem hello fjärrtabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för**PushAsync**. Mer information finns i avsnittet hello *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps][2].

## <a name="run-hello-client-app"></a>Kör hello-klientappen
Med offlinesynkronisering har nu aktiverats, kör du hello klientprogrammet minst en gång för varje plattform toopopulate hello lokalt Arkiv-databasen. Senare, simulera ett offline scenario och ändra hello data i hello lokalt Arkiv medan hello appen är offline.

## <a name="update-hello-sync-behavior-of-hello-client-app"></a>Uppdatera hello sync funktionssätt hello-klientappen
I det här avsnittet Ändra hello klienten projektet toosimulate ett offline-scenario med en ogiltig programmets URL för din serverdel. Du kan också du kan stänga av Nätverksanslutningar genom att flytta din enhet för ”Flygplansläge”.  När du lägger till eller ändra dataobjekt ändringarna lagras i hello lokalt arkiv men har inte synkroniserats toohello backend datalager tills hello anslutningen har återupprättats.

1. I hello Solution Explorer, Öppna projektfilen för hello Constants.cs från hello **bärbar** projektet och ändra hello värdet för `ApplicationURL` toopoint tooan ogiltig URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Öppna hello TodoItemManager.cs filen från hello **bärbar** projektet och sedan lägga till en **fånga** för grundläggande hello **undantag** klassen toohello **försök... catch** blockera i **SyncAsync**. Detta **fånga** block skriver hello undantag meddelandet toohello-konsolen på följande sätt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Skapa och köra hello-klientappen.  Lägga till några nya objekt. Observera att ett undantag loggas i hello konsol för varje försök toosync med hello-serverdelen. Dessa nya objekt finns bara i hello lokalt Arkiv tills de flyttas toohello mobilserverdel. hello-klientappen fungerar som om den är ansluten toohello backend, stöder alla skapa, läsa, uppdatera, borttagningsåtgärder (CRUD).
4. Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.
5. (Valfritt) Använd Visual Studio tooview toosee din Azure SQL Database-tabellen som hello data i hello backend-databasen inte har ändrats.

    Öppna i Visual Studio **Server Explorer**. Navigera tooyour databasen i **Azure**->**SQL-databaser**. Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**. Nu kan du bläddra tooyour SQL-databastabell och dess innehåll.

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a>Uppdatera hello klienten app tooreconnect din mobila serverdel
Återanslut hello app toohello mobilserverdel, som simulerar hello app komma tillbaka tooan online tillstånd i det här avsnittet. När du utför hello uppdatera gest är data synkroniserade tooyour mobilserverdel.

1. Öppna Constants.cs. Rätt hello `applicationURL` toopoint toohello korrigera URL.
2. Bygg och kör hello klientapp. hello app försöker toosync med hello mobilappsserverdel när du startar. Kontrollera att inga undantag loggas i hello Felsökningskonsolen.
3. (Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler eller [Postman][6]. Meddelande hello data har synkroniserats mellan hello backend-databas och hello lokalt Arkiv.

    Observera hello data har synkroniserats mellan hello databasen och hello lokalt Arkiv och innehåller hello-objekt som du har lagt till medan kopplades från din app.

## <a name="additional-resources"></a>Ytterligare resurser
* [Datasynkronisering offline i Azure Mobile Apps][2]
* [Azure Mobile Apps .NET SDK ta][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
