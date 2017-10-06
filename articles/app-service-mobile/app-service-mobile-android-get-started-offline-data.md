---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Android)"
description: "Lär dig hur toouse Apptjänst Mobilappar toocache och sync offline data i din Android-App"
documentationcenter: android
author: ggailey777
manager: syntaxc4
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 34508c7394610cf9127e1753637940826b8fd06a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Aktivera offlinesynkronisering av din Android-mobilapp
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen ingår hello offlinesynkronisering funktion i Azure Mobilappar för Android. Offlinesynkronisering kan slutet användare toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas. När hello enheten är online igen, synkroniseras ändringarna med fjärråtkomst hello-serverdelen.

Om det här är din första upplevelse med Azure Mobile Apps du bör först kursen hello [skapa en Android-App]. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello data access paket tooyour tilläggsprojekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].

## <a name="update-hello-app-toosupport-offline-sync"></a>Uppdatera hello app toosupport offlinesynkronisering
Med offlinesynkronisering, du läsa tooand skriva från en *sync tabell* (med hello *IMobileServiceSyncTable* interface), som ingår i en **SQLite** databasen på din enhet.

toopush och pull ändringar mellan hello enheten och Azure Mobile Services som du använder en *synkroniseringskontext* (*MobileServiceClient.SyncContext*), som du initiera med hello lokal databas toostore data lokalt.

1. I `TodoActivity.java`, kommentera hello befintliga definitionen av `mToDoTable` och Avkommentera hello synkroniserade tabell versionen:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. I hello `onCreate` metod, kommentera hello befintliga initieringen av `mToDoTable` och Avkommentera den här definitionen:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. I `refreshItemsFromTable` kommentera hello definition av `results` och Avkommentera den här definitionen:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Kommentera hello definitionen av `refreshItemsFromMobileServiceTable`.
5. Ta bort kommentarerna hello definitionen av `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Ta bort kommentarerna hello definitionen av `sync`:
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-hello-app"></a>Testa hello app
I det här avsnittet testa hello fungerar med WiFi på och stäng sedan av WiFi toocreate ett offline-scenario.

När du lägger till data som de förvaras i hello lokala SQLite store, men inte synkroniserats toohello mobila tjänsten tills du trycker på hello **uppdatera** knappen. Andra appar kan ha olika krav angående när data måste toobe synkroniseras, men för demonstration har den här kursen hello användaren explicit begära den.

När du trycker på knappen startar en ny bakgrundsuppgift. Den första skickar alla ändringar som gjorts toohello lokalt Arkiv med hjälp av synkroniseringskontext hämtar alla ändrade data från Azure toohello lokal tabell.

### <a name="offline-testing"></a>Testa offline
1. Plats hello enhet eller simulator i *Flygplansläge*. Detta skapar ett offline-scenario.
2. Lägga till några *ToDo* funktioner eller markera vissa objekt som slutförd. Avsluta hello enhet eller simulator (eller framtvingar Stäng hello app) och starta om. Kontrollera att dina ändringar har sparats på hello enhet eftersom de lagras i hello lokala SQLite-arkivet.
3. Visa hello innehållet i hello Azure *TodoItem* tabell antingen med ett SQL-verktyg som *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller  *Postman*. Kontrollera att hello nya objekt har *inte* har synkroniserats toohello server
   
       + Node.js-serverdel finns toohello [Azure-portalen](https://portal.azure.com/), och i din Mobilapp backend på **enkelt tabeller** > **TodoItem** tooview hello innehållet i hello `TodoItem` tabell.
       + För en .NET-serverdel visa hello tabell innehållet antingen med ett SQL-verktyg som *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller *Postman*.
4. Aktivera WiFi i hello enhet eller simulatorn. Tryck sedan på hello **uppdatera** knappen.
5. Visa hello TodoItem data i hello Azure-portalen. hello nya och ändrade TodoItems bör nu visas.

## <a name="additional-resources"></a>Ytterligare resurser
* [offlinesynkronisering Data i Azure Mobile Apps]
* [Molnet omfattar: Offlinesynkronisering i Azure Mobile Services] \(Obs: hello video finns i Mobile Services, men offlinesynkronisering fungerar på ett liknande sätt i Azure Mobile Apps\)

<!-- URLs. -->

[offlinesynkronisering Data i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[skapa en Android-App]: app-service-mobile-android-get-started.md

[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

