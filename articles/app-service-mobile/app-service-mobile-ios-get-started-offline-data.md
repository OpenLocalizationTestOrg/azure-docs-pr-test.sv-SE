---
title: aaaEnable offline synkroniserar med iOS-appar | Microsoft Docs
description: "Lär dig hur toouse Azure Apptjänst mobilappar toocache och synkronisera offlinedata i iOS-program."
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 570ea7cf6694ab7317c977331038929b64508ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Aktivera offline synkroniserar med iOS-appar
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Översikt
Den här kursen ingår offline synkroniseras med funktionerna för hello Mobile Apps i Azure App Service för iOS. Med offline synkroniserar slutanvändare kan interagera med en mobil app tooview, lägga till eller ändra data, även om de har någon nätverksanslutning. Ändringarna sparas i en lokal databas. När hello enheten är online igen, har hello ändringarna synkroniserats med hello remote serverdel.

Om detta är din första upplevelse med Mobilappar kan du bör först kursen hello [skapa en iOS-App]. Om du inte använder hello hämtas Snabbkurs serverprojekt måste du lägga till hello dataåtkomst tillägget paket tooyour projekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn mer om hello offlinesynkronisering funktionen finns [offlinesynkronisering Data i Mobile Apps].

## <a name="review-sync"></a>Granska hello klientkod för synkronisering
Hej klientprojektet som du hämtade för hello [skapa en iOS-App] kursen redan innehåller kod som har stöd för offlinesynkronisering med hjälp av en lokal databas för kärnor databaserad. Det här avsnittet sammanfattas ingår redan i självstudiekursen hello-kod. En översikt över funktionen hello finns [offlinesynkronisering Data i Mobile Apps].

Funktionen hello datasynkronisering offline för Mobile Apps kan kan slutanvändarna interagera med en lokal databas även om hello nätverket är inte tillgänglig. toouse funktionerna i appen du initiera hello sync kontexten för `MSClient` och referera till ett lokalt Arkiv. Sedan du refererar till tabellen via hello **MSSyncTable** gränssnitt.

I **QSTodoService.m** (Objective-C) eller **ToDoTableViewController.swift** (Swift), Lägg märke till att hello typ av hello medlem **syncTable** är  **MSSyncTable**. Offlinesynkronisering använder sync tabell gränssnittet i stället för **MSTable**. När en tabell med synkronisering används gå toohello lokalt Arkiv alla åtgärder och synkroniseras med hello remote serverdel med explicit push och pull-åtgärder.

 tooget referens tooa sync tabellen använder hello **syncTableWithName** metod på `MSClient`. tooremove offlinesynkronisering funktioner, Använd **tableWithName** i stället.

Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras. Här är hello relevant kod:

* **Objective-C**. I hello **QSTodoService.init** metod:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **SWIFT**. I hello **ToDoTableViewController.viewDidLoad** metod:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Den här metoden skapar ett lokalt Arkiv med hjälp av hello `MSCoreDataStore` gränssnitt som hello Mobile Apps-SDK tillhandahåller. Du kan också ange ett annat lokalt Arkiv genom att implementera hello `MSSyncContextDataSource` protokoll. Dessutom hello första parametern för **MSSyncContext** är används toospecify en konflikt-hanterare. Eftersom vi har klarat `nil`, vi hämta hello konflikt standardprogram, som inte på någon konflikt.

Nu ska vi utföra hello faktiska synkroniseringsåtgärd och hämta data från hello remote serverdel:

* **Objective-C**. `syncData`först skickar nya ändringar och anropar sedan **pullData** tooget data från hello remote serverdel. I sin tur hello **pullData** metoden hämtar nya data som matchar en fråga:

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in hello sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from hello remote server into hello local table.
       // We're pulling all items and filtering in hello view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets hello caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* **SWIFT**:
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via hello MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep hello server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation tooitem \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting tooserver's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

I hello Objective-C-versionen i `syncData`, vi först anropa **pushWithCompletion** på hello sync-kontext. Den här metoden är medlem i `MSSyncContext` (och inte hello sync själva tabellen) eftersom den skickar ändringarna över alla tabeller. Endast de poster som har ändrats på något sätt lokalt (via CUD operations) skickas toohello server. Hej helper **pullData** anropas, som anropar **MSSyncTable.pullWithQuery** tooretrieve fjärrdata och lagra den på hello lokal databas.

Hello Swift version, eftersom hello utgivarinitierade åtgärden inte var absolut nödvändigt det finns inga anrop för**pushWithCompletion**. Om det finns några väntande ändringar i hello sync kontexten för hello-tabell som gör en push-åtgärd, utfärdar pull alltid en push först. Om du har mer än en synkronisering tabell är den anropet push av bästa tooexplicitly-tooensure som att allt är konsekvent på relaterade tabeller.

I både hello Objective-C och Swift versioner, kan du använda hello **pullWithQuery** metoden toospecify en fråga toofilter hello poster du vill ha tooretrieve. I det här exemplet hello fråga hämtar alla poster i hello remote `TodoItem` tabell.

Hej andra parametern för **pullWithQuery** är en fråge-ID som används för *inkrementell synkronisering*. Inkrementell synkronisering hämtar poster som har ändrats sedan hello senast synkroniserades med hello post `UpdatedAt` tidsstämpeln (kallas `updatedAt` i hello lokal lagring.) hello fråge-ID ska vara en beskrivande sträng som är unik för varje logisk fråga i din app. tooopt utanför inkrementell synkronisering skicka `nil` som hello fråga-ID. Den här metoden kan vara potentiellt ineffektiv eftersom den hämtar alla poster på varje pull-åtgärd.

hello Objective-C-app som ska synkroniseras när du ändrar eller lägger till data, när användaren utför hello uppdatera gest och startas.

hello Swift app synkroniserar när hello användaren utför hello uppdatera gest och startas.

Eftersom hello app synkroniseringar när data har ändrats (Objective-C) eller när hello appen startar (Objective-C och Swift), förutsätter hello app hello användaren är online. I ett senare avsnitt ska du uppdatera hello app så att användare kan redigera även när de är offline.

## <a name="review-core-data"></a>Granska hello Core-datamodell
När du använder hello Core offline datalagret, måste du definiera specifika tabeller och fält i datamodellen. Hej exempelapp innehåller redan en datamodell med hello rätt format. I det här avsnittet går vi igenom dessa tabeller tooshow hur de används.

Öppna **QSDataModel.xcdatamodeld**. Fyra är definierad--tre som används av hello SDK och en som används för att göra hello konfigurationsobjekt själva:
  * MS_TableOperations: Spårar hello objekt behöver toobe synkroniseras med hello-servern.
  * MS_TableOperationErrors: Spårar alla fel som inträffar under offlinesynkronisering.
  * MS_TableConfig: Spårar hello senast uppdaterades för hello senaste synkroniseringsåtgärd för alla pull-åtgärder.
  * TodoItem: Lagrar hello arbetsuppgifter. Hej Systemkolumner **createdAt**, **updatedAt**, och **version** är valfria Systemegenskaper.

> [!NOTE]
> hello Mobile Apps-SDK reserverar kolumnnamn som börjar med ”**``**”. Använd inte det här prefixet med något annat än Systemkolumner. Annars ändras kolumnnamn som när du använder hello remote serverdel.
>
>

När du använder funktionen för hello offlinesynkronisering definiera hello tre systemtabeller och hello datatabell.

### <a name="system-tables"></a>Systemtabeller

**MS_TableOperations**  

![MS_TableOperations attribut][defining-core-data-tableoperations-entity]

| Attribut | Typ |
| --- | --- |
| id | Heltal 64 |
| itemId | Sträng |
| properties | Binära Data |
| Tabell | Sträng |
| tableKind | Heltal 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors attribut][defining-core-data-tableoperationerrors-entity]

| Attribut | Typ |
| --- | --- |
| id |Sträng |
| Åtgärds-ID |Heltal 64 |
| properties |Binära Data |
| tableKind |Heltal 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Attribut | Typ |
| --- | --- |
| id |Sträng |
| key |Sträng |
| KeyType |Heltal 64 |
| Tabell |Sträng |
| värde |Sträng |

### <a name="data-table"></a>Datatabell

**TodoItem**

| Attribut | Typ | Obs! |
| --- | --- | --- |
| id | Strängen som markerats krävs |primärnyckeln i fjärranslutna store |
| Slutför | Booleskt värde | Fältet för att göra-objekt |
| Text |Sträng |Fältet för att göra-objekt |
| CreatedAt | Date | (valfritt) Mappar för**createdAt** Systemegenskapen |
| updatedAt | Date | (valfritt) Mappar för**updatedAt** Systemegenskapen |
| Version | Sträng | (valfritt) Använda toodetect konflikter, maps tooversion |

## <a name="setup-sync"></a>Ändra hello hello appens sync beteende
I det här avsnittet Ändra hello app så att den inte synkroniseras på appen startas eller när du infogar och uppdatera objekt. Det synkroniserar bara när hello uppdateringsknappen gest utförs.

**Objective-C**:

1. I **QSTodoListViewController.m**, ändra hello **viewDidLoad** metoden tooremove hello anrop för`[self refresh]` hello slutet av hello-metoden. Nu hello data inte har synkroniserats med hello server på appen startas. I stället är den synkroniserad med hello innehållet i hello lokalt Arkiv.
2. I **QSTodoService.m**, ändra hello definitionen av `addItem` så att den inte synkronisera efter hello objektet infogas. Ta bort hello `self syncData` blockera och Ersätt den med hello följande:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Ändra hello definitionen av `completeItem` som tidigare nämnts. Ta bort hello-block för `self syncData` och Ersätt den med hello följande:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**SWIFT**:

I `viewDidLoad`i **ToDoTableViewController.swift**, kommentera hello två rader som visas här, toostop synkroniserar appen startas. När hello detta skrivs uppdaterar hello Swift Todo-appen inte hello-tjänsten när någon lägger till eller ett objekt slutförs. Uppdaterar den hello tjänsten endast på appen startas.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>Testa hello app
I det här avsnittet kan du ansluta tooan Ogiltigt URL toosimulate ett offline-scenario. När du lägger till data som de ska lagras i hello lokala Core datalager, men de är inte synkroniserade med hello mobilapp serverdel.

1. Ändra hello mobile app-URL i **QSTodoService.m** tooan Ogiltigt URL och kör hello app igen:

   **Objective-C**. I QSTodoService.m:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **SWIFT**. I ToDoTableViewController.swift:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Lägga till vissa arbetsuppgifter. Avsluta hello simulator (eller framtvingar Stäng hello app) och starta om den. Kontrollera att spara dina ändringar.

3. Visa hello innehållet i hello remote **TodoItem** tabell:
   * En Node.js-serverdel finns toohello [Azure-portalen](https://portal.azure.com/) och på din mobila app serverdel **enkelt tabeller** > **TodoItem**.  
   * Avsluta, använda ett SQL-verktyg, till exempel SQL Server Management Studio eller en REST-klient, till exempel Fiddler eller Postman för en .NET tillbaka.  

4. Kontrollera att hello nya objekt har *inte* har synkroniserats med hello-server.

5. Ändra hello URL: en bakre toohello korrigera i **QSTodoService.m**, och kör hello app.

6. Utföra hello uppdatera gest genom att dra nedåt hello listan med objekt.  
En rotationsruta förloppet visas.

7. Visa hello **TodoItem** data igen. hello nya och ändrade arbetsuppgifter ska nu visas.

## <a name="summary"></a>Sammanfattning
toosupport hello offlinesynkronisering funktionen vi använde hello `MSSyncTable` gränssnitt och initieras `MSClient.syncContext` med ett lokalt Arkiv. I det här fallet var hello lokalt Arkiv en Core databaserad databas.

När du använder ett lokalt Arkiv grundläggande information, måste du definiera flera tabeller med hello [korrigera Systemegenskaper](#review-core-data).

hello normal skapa, läsa, uppdatera och ta bort CRUD-åtgärder för mobilappar fungerar som om hello app fortfarande är ansluten, men alla hello åtgärder mot hello lokalt Arkiv.

När vi synkroniseras hello lokalt Arkiv med hello server använde vi hello **MSSyncTable.pullWithQuery** metod.

## <a name="additional-resources"></a>Ytterligare resurser
* [offlinesynkronisering Data i Mobile Apps]
* [Molnet omfattar: Offlinesynkronisering i Azure Mobile Services] \(hello video handlar om Mobile Services, men Mobile Apps offline synkroniseras fungerar på liknande sätt.\)

<!-- URLs. -->


[skapa en iOS-App]: app-service-mobile-ios-get-started.md
[offlinesynkronisering Data i Mobile Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
