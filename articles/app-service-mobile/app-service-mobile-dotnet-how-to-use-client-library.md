---
title: "aaaWorking med hello Apptjänst Mobilappar hanteras klientbiblioteket (Windows | Microsoft Docs"
description: "Lär dig hur toouse en .NET-klient för Azure Apptjänst Mobilappar med Windows och Xamarin-appar."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2017
ms.author: glenga
ms.openlocfilehash: b056e606b19406398f5b6faabb0931ad651125e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a>Hur toouse hello hanterade klienten för Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello hanteras klientbiblioteket för Azure App Service mobila appar för Windows och Xamarin-appar. Om du är den nya tooMobile appar, bör du först slutför hello [Azure Mobile Apps quickstart] [ 1] kursen. I den här guiden fokuserar vi på hello klientsidan hanteras SDK. toolearn mer om Hej serversidan SDK: er för Mobile Apps, hello i dokumentationen för hello [SDK för .NET Server] [ 2] eller [SDK för Node.js Server] [ 3].

## <a name="reference-documentation"></a>Referensdokumentation
hello referensdokumentationen för hello klient-SDK finns här: [Azure Mobile Apps .NET klienten referens][4].
Du kan också hitta flera klienten prover i hello [Azure-exempel GitHub-lagringsplatsen][5].

## <a name="supported-platforms"></a>Plattformar som stöds
hello .NET-plattformen stöder hello följande plattformar:

* Xamarin Android-versioner för API-19 till 24 (KitKat via Nougat)
* Xamarin iOS-versioner för iOS version 8.0 och senare
* Universella Windows-plattformen
* Windows Phone 8.1
* Windows Phone 8.0 förutom Silverlight-program

Hej ”server flöde” autentisering använder en webbvy för hello som visas i Användargränssnittet.  Om hello enheten inte är kan toopresent en WebView UI behövs andra metoder för autentisering.  Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.

## <a name="setup"></a>Installationen och förutsättningar
Vi förutsätter att du redan har skapat och publicerat serverdelsprojektet Mobilapp som innehåller minst en tabell.  I hello-koden i det här avsnittet, hello tabellen med namnet `TodoItem` och har hello följande kolumner: `Id`, `Text`, och `Complete`. Den här tabellen är hello samma tabell skapas när du har slutfört den [Azure Mobile Apps quickstart][1].

hello är motsvarande skrivna på klientsidan i C# hello följande klass:

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

Hej [JsonPropertyAttribute] [ 6] är används toodefine hello *PropertyName* mappning mellan hello klienten och fältet hello tabell.

toolearn hur toocreate tabeller i Mobile Apps-serverdel finns hello [SDK för .NET Server avsnittet] [ 7] eller hello [SDK för Node.js Server avsnittet] [ 8] . Om du har skapat din mobilappsserverdel i hello Azure portal med Hej Snabbstart, du kan också använda hello **enkelt tabeller** i hello [Azure-portalen].

### <a name="how-to-install-hello-managed-client-sdk-package"></a>Så här: Installera hello hanterade klient-SDK-paketet
Använd en av följande metoder tooinstall hello hello hanterad klient-SDK-paketet för Mobile Apps från [NuGet][9]:

* **Visual Studio** Högerklicka på ditt projekt, klickar du på **hantera NuGet-paket**, Sök efter hello `Microsoft.Azure.Mobile.Client` paketet och klicka sedan på **installera**.
* **Xamarin Studio** Högerklicka på ditt projekt, klickar du på **Lägg till** > **Lägg till NuGet-paket**, Sök efter hello `Microsoft.Azure.Mobile.Client `paketet och klickar sedan på **Lägg till Paketet**.

Kom ihåg tooadd hello följande i filen huvudaktiviteten **med** instruktionen:

```
using Microsoft.WindowsAzure.MobileServices;
```

### <a name="symbolsource"></a>Så här: arbeta med symboler för felsökning i Visual Studio
hello symboler för hello Microsoft.Azure.Mobile namnområdet är tillgängliga på [SymbolSource][10].  Se toothe [SymbolSource instruktioner] [ 11] toointegrate SymbolSource med Visual Studio.

## <a name="create-client"></a>Skapa hello Mobile Apps-klient
hello följande kod skapar hello [MobileServiceClient] [ 12] objekt som används tooaccess din mobilappsserverdel.

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Ersätt i hello föregående kod, `MOBILE_APP_URL` med hello URL hello mobilappsserverdel, som finns i bladet för din mobilappsserverdel i hello [Azure-portalen]. Hej MobileServiceClient objektet ska vara en singleton.

## <a name="work-with-tables"></a>Arbeta med tabeller
Hej följande avsnittet beskriver hur toosearch och hämta poster och ändra hello data inom hello tabell.  hello följande avsnitt beskrivs:

* [Skapa en tabellreferens](#instantiating)
* [Fråga efter data](#querying)
* [Filtret returnerade data](#filtering)
* [Sortera returnerade data](#sorting)
* [Returnera data på sidor](#paging)
* [Markera specifika kolumner](#selecting)
* [Leta upp en post-ID: t](#lookingup)
* [Hantera ej typbestämd frågor](#untypedqueries)
* [Infoga data](#inserting)
* [Uppdatera data](#updating)
* [Ta bort data](#deleting)
* [Konfliktlösning och Optimistisk samtidighet](#optimisticconcurrency)
* [Bindningen tooa Windows-gränssnittet](#binding)
* [Ändra hello sidstorlek](#pagesize)

### <a name="instantiating"></a>Så här: skapa en tabellreferens
Alla hello-kod som har åtkomst till eller ändrar data i en backend-tabell anropar funktioner på hello `MobileServiceTable` objekt. Skaffa en referenstabell toohello genom att anropa hello [GetTable] metoden på följande sätt:

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

hello returnerade objektet använder hello skrivna serialisering modellen. En modell för ej typbestämd serialisering stöds också. I följande exempel [skapar en tooan ej typbestämd referenstabell]:

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

Du måste ange hello underliggande OData-frågesträng i ej typbestämd frågor.

### <a name="querying"></a>Så här: fråga efter data från din Mobilapp
Det här avsnittet beskrivs hur tooissue frågar toohello mobilappsserverdel, vilket innefattar hello följande funktioner:

* [Filtret returnerade data](#filtering)
* [Sortera returnerade data](#sorting)
* [Returnera data på sidor](#paging)
* [Markera specifika kolumner](#selecting)
* [Leta upp data-ID: t](#lookingup)

> [!NOTE]
> En server-driven storlek är tvingande tooprevent alla rader från att returneras.  Sidindelning håller standard begäranden för stora datamängder från negativt påverka hello-tjänsten.  tooreturn mer än 50 rader använder hello `Skip` och `Take` metod, enligt beskrivningen i [returnerar data på sidor](#paging).

### <a name="filtering"></a>Så här: filtret returnerade data
hello följande kod visar hur toofilter data genom att inkludera en `Where` -satsen i en fråga. Returnerar alla objekt från `todoTable` vars `Complete` -egenskapen har värdet för`false`. Hej [där] funktion gäller en rad filtrering predikat hello frågan mot hello tabell.

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Du kan visa hello hello begäran som skickades toohello backend-URI med meddelandet inspektion programvara, till exempel webbläsare utvecklingsverktyg eller [Fiddler]. Om du tittar på hello URI-begäran Lägg märke till att hello frågesträngen ändras:

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Denna OData-begäran översätts till en SQL-fråga genom hello-serverns SDK:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

Hej funktion som har överförts toohello `Where` metod kan ha ett godtyckligt antal villkor.

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

Det här exemplet kan översättas till en SQL-fråga genom hello-serverns SDK:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Den här frågan kan också delas upp i flera villkor:

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

hello två metoder är likvärdiga och utbytbara.  Hej förstnämnda alternativet&mdash;sammanfoga flera predikat i en fråga&mdash;blir mer kompakt och rekommenderade.

Hej `Where` satsen stöder åtgärder som kan översättas till hello OData delmängd. Åtgärder är:

* Relationsoperatorer (==,! =, <, < =, >, > =),
* Aritmetiska operatorer (+, -, /, *, %),
* Number precision (Math.Floor, Math.Ceiling)
* Strängfunktioner (längd, delsträngen, Ersätt, IndexOf, StartsWith, EndsWith)
* Egenskaper för datum (år, månad, dag, timme, minut, sekund)
* Komma åt egenskaper för ett objekt, och
* Uttryck kombinera någon av dessa åtgärder.

Om utifrån vad hello Server SDK kan användas, kan du hello [OData v3 dokumentationen].

### <a name="sorting"></a>Så här: sortera returnerade data
hello följande kod visar hur toosort data genom att inkludera en [OrderBy] eller [OrderByDescending] i hello fråga. Returnerar objekten från `todoTable` Sortera stigande efter hello `Text` fältet.

```
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <a name="paging"></a>Så här: returnerar data på sidor
Som standard returnerar hello backend endast hello första 50 rader. Du kan öka hello antal returnerade rader genom att anropa hello [ta] metod. Använd `Take` tillsammans med hello [hoppa över] metoden toorequest en viss ”sida” i hello totala dataset returneras av hello frågan. hello returnerar följande fråga när den exekveras, hello de tre översta objekt i hello tabell.

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

hello följande reviderade frågan hoppar över hello först tre resultaten och returnerar hello följande tre resultat. Den här frågan genererar hello andra ”sidan” data, där hello sidstorleken är tre objekt.

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Hej [IncludeTotalCount] metoden begär hello Totalt antal för *alla* hello poster som har returnerats, ignoreras eventuella sidindelning/gräns-sats har angetts:

```
query = query.IncludeTotalCount();
```

Du kan använda frågor liknande toohello föregående exempel med en personsökare kontroll eller jämförbar UI för att navigera mellan sidor i ett verkligt-app.

> [!NOTE]
> toooverride hello 50 rad-gränsen i en mobilappsserverdel, måste du också använda hello [EnableQueryAttribute] toohello offentlig GET-metod och ange hello sidindelning beteende. När tillämpade toohello metoden, hello följande anger max antal returnerade rader too1000:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Så här: Markera specifika kolumner
Du kan ange vilken uppsättning egenskaper tooinclude i hello resultat genom att lägga till en [Välj] satsen tooyour frågan. Till exempel hello följande kod visar hur tooselect ett enda fält och även hur tooselect och formatera flera fält:

```
// Select one field -- just hello Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

Alla hello funktioner beskrivs hittills är additiva, så vi kan hålla länkning dem. Varje länkad anrop påverkar flera hello frågan. Ytterligare ett exempel:

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Så här: Leta upp data-ID: t
Hej [LookupAsync] funktionen kan vara används toolook in objekt från hello-databas med ett visst-ID.

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Så här: köra någon frågor
När du kör en fråga med ett argument tabellobjekt, måste du uttryckligen ange hello OData-frågesträng genom att anropa [ReadAsync], som i följande exempel hello:

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

Åter JSON-värden som du kan använda som en egenskapsuppsättning. Mer information om JToken och Newtonsoft Json.NET finns hello [Json.NET] plats.

### <a name="inserting"></a>Så här: Infoga data i en mobilappsserverdel
Alla klienttyper av måste innehålla en medlem med namnet **Id**, vilket är som standard en sträng. Detta **Id** krävs för att utföra CRUD-åtgärder och offline-synkronisering. hello följande kod visar hur toouse hello [InsertAsync] metoden tooinsert nya rader i en tabell. hello parametern innehåller hello data toobe läggas till som ett .NET-objekt.

```
await todoTable.InsertAsync(todoItem);
```

Om ett unikt värde för anpassad ID inte ingår i hello `todoItem` under en insert-, ett GUID som genereras av hello-server.
Du kan hämta hello genereras Id genom att inspektera hello objekt när hello anropet returnerar.

tooinsert utan angiven data, kan du dra nytta av Json.NET:

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Här är ett exempel som använder en e-postadress som en unik sträng-id:

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Arbeta med ID-värden
Mobile Apps stöder unika anpassade strängvärden för hello tabellen **id** kolumn. Ett strängvärde som gör att program toouse anpassade värden, till exempel e-postadresser eller användarnamn för hello-ID.  Sträng-ID: N ge hello följande fördelar:

* ID: N skapas utan att göra en onödig kommunikation toohello databas.
* Posterna är enklare toomerge från olika tabeller eller databaser.
* ID-värden kan integrera bättre med en programlogik.

När ett strängvärde ID inte har angetts på en infogad post genererar hello mobilappsserverdel ett unikt värde för-ID. Du kan använda hello [Guid.NewGuid] metoden toogenerate egna ID värden på hello-klienten eller i hello backend.

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Så här: ändra data i en mobilappsserverdel
hello följande kod visar hur toouse hello [UpdateAsync] metoden tooupdate en befintlig post med hello med samma ID med ny information. hello parametern innehåller hello data toobe som ett .NET-objekt har uppdaterats.

```
await todoTable.UpdateAsync(todoItem);
```

tooupdate utan angiven data, kan du dra nytta av [Json.NET] på följande sätt:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

En `id` fält måste anges när du gör en uppdatering. hello backend använder hello `id` fältet tooidentify som rad tooupdate. Hej `id` fältet kan hämtas från hello resultatet av hello `InsertAsync` anropa. En `ArgumentException` uppstår om du försöker tooupdate ett objekt utan att ange hello `id` värde.

### <a name="deleting"></a>Så här: ta bort data i en mobilappsserverdel
hello följande kod visar hur toouse hello [DeleteAsync] metoden toodelete en befintlig instans. hello-instans kan identifieras genom hello `id` angivet på hello `todoItem`.

```
await todoTable.DeleteAsync(todoItem);
```

toodelete utan angiven data, kan du dra nytta av Json.NET på följande sätt:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

När du gör en begäran om borttagning måste ett ID anges. Andra egenskaper har inte skickats toohello service eller ignoreras vid hello-tjänsten. Hej resultatet av en `DeleteAsync` anropet är vanligtvis `null`. hello ID toopass i kan hämtas från hello resultatet av hello `InsertAsync` anropa. En `MobileServiceInvalidOperationException` genereras när du försöker toodelete ett objekt utan att ange hello `id` fältet.

### <a name="optimisticconcurrency"></a>Så här: använda Optimistisk samtidighet för konfliktlösning
Två eller flera klienter kan skriva ändringar toohello samma objekt vid hello samma tid. Utan konfliktidentifiering, skulle hello senaste skriva över eventuella tidigare uppdateringar. **Optimistisk samtidighetskontroll** förutsätter att varje transaktion kan genomföra och därför använder inte någon resurslåsning.  Innan du genomför en transaktion verifierar optimistisk samtidighetskontroll att ingen annan transaktion har ändrats hello data. Hello inleder transaktion återställs om hello data har ändrats.

Mobile Apps stöder optimistisk samtidighetskontroll genom att spåra ändringar tooeach objekt som använder hello `version` system egenskapskolumn som har definierats för varje tabell i din mobilappsserverdel. Varje gång en post uppdateras Mobile Apps anger hello `version` egenskapen för att registrera tooa nytt värde. Under varje uppdateringsbegäran hello `version` egenskapen hello postens medföljer hello-begäran är toohello jämfört med samma egenskap för hello-post på hello-servern. Om versionen som har skickats med hello förfrågan matchar inte hello backend sedan hello klientbiblioteket genererar en `MobileServicePreconditionFailedException<T>` undantag. hello-typen som ingår i hello undantag är hello post från hello serverdel som innehåller hello servrar version av hello-post. hello program kan sedan använda denna information toodecide om begäran om uppdatering av tooexecute hello igen med rätt hello `version` värde från hello backend toocommit ändringar.

Definiera en kolumn i hello tabellen klass för hello `version` system egenskapen tooenable Optimistisk samtidighet. Exempel:

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

Program med hjälp av någon tabeller aktivera Optimistisk samtidighet genom att ange hello `Version` flaggan på den `SystemProperties` av hello tabell på följande sätt.

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

I tillägg tooenabling Optimistisk samtidighet, måste du också fånga hello `MobileServicePreconditionFailedException<T>` undantag i koden när du anropar [UpdateAsync].  Lösa hello konflikten genom att använda rätt hello `version` toothe uppdaterat post och anropet [UpdateAsync] med hello löst post. hello följande kod visar hur tooresolve en Skriv-konflikt när upptäcktes:

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at hello remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, hello item has changed since hello last query
        // Resolve hello conflict between hello local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user toochoose hello resoltion between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // tooresolve hello conflict, update hello version of hello item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while hello user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

Mer information finns i hello [Offline datasynkronisering i Azure Mobile Apps] avsnittet.

### <a name="binding"></a>Så här: Bind Mobile Apps data tooa Windows-gränssnittet
Det här avsnittet visar hur toodisplay returnerade dataobjekt med hjälp av UI-element i en Windows-app.  Följande exempelkod Binder toohello källan för hello lista med en fråga om ofullständiga objekt. Den [MobileServiceCollection] skapar en Mobile Apps-medvetna bindning-samling.

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound tooa UI list control
IEnumerable itemsControl  = items;

// Bind this tooa ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Vissa kontroller i hello hanteras runtime stöd för ett gränssnitt kallas [ISupportIncrementalLoading]. Det här gränssnittet gör kontroller toorequest extra data när hello användare rullar. Det finns inbyggt stöd för det här gränssnittet för universella Windows-appar via [MobileServiceIncrementalLoadingCollection], som automatiskt hanterar anrop från hello kontroller. Använd `MobileServiceIncrementalLoadingCollection` i Windows-appar på följande sätt:

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

toouse hello ny samling i Windows Phone 8 och ”Silverlight”, Använd hello `ToCollection` tilläggsmetoder på `IMobileServiceTableQuery<T>` och `IMobileServiceTable<T>`. tooload data, anropa `LoadMoreItemsAsync()`.

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

När du använder hello-samling som skapats genom att anropa `ToCollectionAsync` eller `ToCollection`, du får en samling som kan vara bundna tooUI kontroller.  Den här samlingen är medveten om sidindelning.  Eftersom hello samling läser in data från nätverket, misslyckas inläsning ibland. toohandle dessa fel, åsidosätta hello `OnException` metod på `MobileServiceIncrementalLoadingCollection` toohandle undantag som uppstår till följd av anrop för`LoadMoreItemsAsync`.

Överväg om tabellen innehåller många fält men du bara vill toodisplay några av dem i kontrollen. Du kan använda hello vägledning i föregående avsnitt hello ”[markera specifika kolumner](#selecting)” tooselect specifika kolumner toodisplay i hello Användargränssnittet.

### <a name="pagesize"></a>Ändra hello sidstorlek
Azure Mobile Apps returnerar maximalt 50 objekt per begäran som standard.  Du kan ändra hello växlingsfilens storlek genom att öka hello maximala sidstorleken på både hello klient och server.  tooincrease Hej begärda sidstorlek, ange `PullOptions` när du använder `PullAsync()`:

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Under förutsättning att du har gjort hello `PageSize` lika tooor som är större än 100 inom hello-server, en begäran returnerar upp till 100 poster.

## <a name="#offlinesync"></a>Arbeta med Offline tabeller
Offline tabeller använder en lokal SQLite lagra toostore data för användning offline.  Alla tabellen operations görs mot hello lokala SQLite lagra i stället för hello fjärrservern store.  toocreate en offline-tabell förbereda projektet:

1. Högerklicka på hello lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i hello-lösning.
2. (Valfritt) toosupport Windows-enheter, installera en av följande SQLite runtime paket hello:

   * **Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].
   * **Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].
   * **Uwp** installera [SQLite för hello universell Windows][5].
3. (Valfritt). Windows-enheter klickar du på **referenser** > **Lägg till referens...** , expandera hello **Windows** mappen > **tillägg**, aktivera hello lämpliga **SQLite för Windows** SDK tillsammans med hello  **Visual C++ 2013 Runtime för Windows** SDK.
    Hej varierar SQLite SDK namn något med varje Windows-plattformen.

Innan en tabellreferens kan skapas, måste du förbereda hello lokalt Arkiv:

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Initiering av certifikatarkiv görs normalt omedelbart efter hello klient skapas.  Hej **OfflineDbPath** bör vara ett filnamn som är lämplig för användning på alla plattformar som du stöder.  Om hello sökväg är en fullständigt kvalificerad sökväg (d.v.s. den börjar med ett snedstreck), och sedan används.  Om hello sökväg inte är fullständigt kvalificerad placeras hello filen i en plattformsspecifik plats.

* För iOS och Android-enheter är hello standardsökvägen hello ”personlig”-mappen.
* För Windows-enheter är hello standardsökvägen hello programspecifika ”AppData” mapp.

En tabellreferens kan erhållas med hjälp av hello `GetSyncTable<>` metoden:

```
var table = client.GetSyncTable<TodoItem>();
```

Du behöver inte tooauthenticate toouse en offline-tabell.  Du behöver bara tooauthenticate när du kommunicerar med hello serverdelstjänst.

### <a name="syncoffline"></a>Synkroniserar en Offline-tabell
Offline tabeller är inte synkroniserade med hello backend som standard.  Synkronisering är uppdelat i två delar.  Du kan skicka ändringar separat hämtar nya objekt.  Här är en typisk synkroniseringsmetod:

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //hello first parameter is a query name that is used internally by hello client SDK tooimplement incremental sync.
            //Use a different query name for each unique query in your program
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

    // Simple error/conflict handling. A real application would handle hello various errors like network conditions,
    // server conflicts and others via hello IMobileServiceSyncHandler.
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

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

Om hello första argumentet för`PullAsync` är null, och sedan inte används för inkrementell synkronisering.  Varje synkroniseringsåtgärd hämtar alla poster.

hello SDK utför en implicit `PushAsync()` innan dra poster.

Konflikt hantering sker på en `PullAsync()` metod.  Du kan åtgärda konflikter i hello samma sätt som online-tabeller.  hello konflikt skapas när `PullAsync()` anropas i stället för under hello insert-, update- eller delete. Om flera sådana konflikter uppstår ingår de i en enda MobileServicePushFailedException.  Hantera varje fel separat.

## <a name="#customapi"></a>Arbeta med en anpassad API
En anpassad API kan du toodefine anpassade slutpunkter som exponerar serverfunktioner som inte mappa tooan infoga, uppdatera, ta bort eller Läsåtgärd. Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.

Du kan anropa en anpassad API genom att anropa en hello [InvokeApiAsync] metoder på hello-klienten. Till exempel hello följande kodrad skickar en POST-begäran toohello **completeAll** API på hello serverdel:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Det här formuläret är skrivna metodanrop och kräver att hello **MarkAllResult** returnera typen är definierad. Både skrivna och ej typbestämd metoder stöds.

Hej InvokeApiAsync() metoden läggs/api/toohello API som du vill toocall om hello API som börjar med en '/'.
Exempel:

* `InvokeApiAsync("completeAll",...)`anropar /api/completeAll på hello serverdel
* `InvokeApiAsync("/.auth/me",...)`anropar /.auth/me på hello serverdel

Du kan använda InvokeApiAsync toocall alla WebAPI, inklusive de WebAPIs som inte har definierats med Azure Mobile Apps.  När du använder InvokeApiAsync() skickas hello lämpliga sidhuvud, inklusive autentiseringshuvuden med hello-begäran.

## <a name="authentication"></a>Autentisera användare
Mobile Apps stöder autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory. Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare. Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript. Mer information finns i självstudiekursen hello [Lägg till autentisering tooyour app].

Två autentisering flöden stöds: *klienten hanteras* och *server-hanterade* flöde. Hej server-hanterade flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering. hello klienten hanteras flödet tillåter djupare integrering med specifika funktioner som använder den provider-specifik enhetsspecifika SDK.

> [!NOTE]
> Du bör använda ett klient-hanterade flöde i dina appar i produktion.

tooset konfigurerar autentisering måste du registrera din app med en eller flera identitetsleverantörer.  hello identitetsleverantör genererar ett klient-ID och en klienthemlighet för din app.  Dessa värden anges sedan i din serverdel tooenable autentisering/auktorisering i Azure App Service.  Mer information hello detaljerade instruktioner i självstudiekursen [Lägg till autentisering tooyour app].

hello följande avsnitt beskrivs i det här avsnittet:

* [Klienten hanteras autentisering](#clientflow)
* [Hanterad Server-autentisering](#serverflow)
* [Cachelagring hello autentiseringstoken](#caching)

### <a name="clientflow"></a>Klienten hanteras autentisering
Din app kan kontakta oberoende hello identitetsleverantör och ange sedan hello returnerade token under inloggningen med din serverdel. Det här flödet kan tooprovide en enkel inloggning för användare eller tooretrieve ytterligare användardata från hello identitetsleverantören. Flöde för klientautentisering är prioriterade toousing flödet för en server som hello identitetsleverantör SDK tillhandahåller en mer ursprungligt UX känslan och gör för ytterligare anpassning.

Exempel har angetts för hello efter klient-flöde autentisering mönster:

* [Active Directory Authentication Library](#adal)
* [Facebook eller Google](#client-facebook)
* [Live SDK](#client-livesdk)

#### <a name="adal"></a>Autentisera användare med hello Active Directory Authentication Library
Du kan använda Active Directory Authentication Library (ADAL) hello tooinitiate användarautentisering hello-klient som använder Azure Active Directory-autentisering.

1. Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] kursen. Se till att toocomplete hello valfritt steg för att registrera en native client-program.
2. Öppna projektet i Visual Studio eller Xamarin Studio och lägga till en referens toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-paketet. När du söker kan innehålla förhandsversioner.
3. Lägg till hello följande kod tooyour program, enligt toohello plattform som du använder. I varje, gör du följande ersättningar hello:

   * Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program. Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com. Det här värdet kan kopieras från hello domän-fliken i Azure Active Directory i hello [klassiska Azure-portalen].
   * Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel. Du kan hämta hello klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.
   * Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.
   * Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema. Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.

     hello-kod som behövs för varje plattform som följer:

     **Windows:**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

     **Xamarin.iOS**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync(UIViewController view)
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

     **Xamarin.Android**

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
    }
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ```

#### <a name="client-facebook"></a>Enkel inloggning med hjälp av en token från Facebook eller Google
Du kan använda hello flödet som visas i den här fragment för Facebook eller Google.

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using hello Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // tooMobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

#### <a name="client-livesdk"></a>Enkel inloggning med Account hello Live SDK
tooauthenticate användare, måste du registrera din app på hello Microsoft-konto Developer Center. Konfigurera registreringsinformation på din mobilappsserverdel. toocreate Microsoft kontot har registrerats och ansluta den tooyour mobilappsserverdel måste fullständig hello stegen i [registrera din app toouse för inloggning till en Microsoft-konto]. Om du har både Windows Store och Windows Phone 8/Silverlight-versionerna av din app kan du registrera hello Windows Store-versionen först.

hello följande kod autentiserar med hjälp av Live SDK och hello returnerade token toosign i tooyour mobilappsserverdel.

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get hello URL hello Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create hello authentication client for Windows Store using hello service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create hello authentication client for Windows Phone using hello client ID of hello registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request hello authentication token from hello Live authentication service.
        // hello wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about hello logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use hello Microsoft account auth token toosign in tooApp Service.
            MobileServiceUser loginResult = await App.MobileService
                .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

            // Display a personalized sign-in greeting.
            string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
            var message = string.Format("You are now logged in - {0}", loginResult.UserId);
            var dialog = new MessageDialog(message, title);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
        else
        {
            session = null;
            var dialog = new MessageDialog("You must log in.", "Login Required");
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
}
```

Mer information finns i hello [Windows Live SDK] dokumentation.

### <a name="serverflow"></a>Hanterad Server-autentisering
När du har registrerat din identitetsleverantör anropa hello [LoginAsync] metod på hello [MobileServiceClient] med hello [MobileServiceAuthenticationProvider] värdet för leverantören. Till exempel initierar hello följande kod en server flödet inloggning med Facebook.

```
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

Om du använder en identitetsleverantör än Facebook, ändra hello värdet på [MobileServiceAuthenticationProvider] toohello värde för leverantören.

I ett server-flöde hanterar Azure App Service hello flödet för OAuth-autentisering genom att visa hello-inloggningssida för hello markerade providern.  Hello identitet providern returnerar Azure App Service genererar när en Apptjänst-token för autentisering. Hej [LoginAsync] metoden returnerar en [MobileServiceUser], som innehåller både hello [UserId] av hello autentiserade användare och hello [ MobileServiceAuthenticationToken], som JSON web token (JWT). Den här token kan cachelagras och återanvändas tills den upphör att gälla. Mer information finns i [cachelagring hello autentiseringstoken](#caching).

### <a name="caching"></a>Cachelagring hello autentiseringstoken
I vissa fall undvikas hello anropsmetod toohello inloggningen efter hello första autentiseringen genom att lagra hello autentiseringstoken från hello-providern.  Windows Store och UWP-appar kan använda [PasswordVault] toocache aktuella autentiseringen token efter en lyckad inloggning, enligt följande:

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

hello UserId värdet lagras som hello användarnamn hello autentiseringsuppgifter och hello token är hello lagras som hello lösenord. I efterföljande nystartade företag, kan du kontrollera hello **PasswordVault** för cachelagrade autentiseringsuppgifter. hello följande exempel används cachelagrade autentiseringsuppgifter när de hittas och annars försöker tooauthenticate igen med hello backend:

```
// Try tooretrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create hello current user from hello stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache hello token as shown above.
}
```

När du loggar ut en användare måste du också bort hello lagrade autentiseringsuppgifter på följande sätt:

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin-appar använda hello [Xamarin.Auth] API: er toosecurely store autentiseringsuppgifter i en **konto** objekt. Ett exempel på hur du använder dessa API: er finns i hello [AuthStore.cs] kodfilen i hello [ContosoMoments foto delning exempel](https://github.com/azure-appservice-samples/ContosoMoments).

När du använder klienten hanteras autentisering kan du också cachelagra hello åtkomst-token som erhölls från leverantören som Facebook eller Twitter. Denna token kan vara angivna toorequest en ny autentiseringstoken från hello serverdel på följande sätt:

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Push-meddelanden
hello beskrivs följande avsnitt Push-meddelanden:

* [Registrera dig för Push-meddelanden](#register-for-push)
* [Skaffa en Windows Store paket-SID](#package-sid)
* [Registrera med plattformsoberoende mallar](#register-xplat)

### <a name="register-for-push"></a>Så här: registrera dig för Push-meddelanden
hello Mobile Apps-klienten kan tooregister för push-meddelanden med Azure Notification Hubs. När du registrerat kan du erhålla en referens som hämtas från hello plattformsspecifika Push Notification Service (PNS). Du anger sedan det här värdet tillsammans med alla taggar när du skapar hello registrering. hello registrerar följande kod din Windows-app för push-meddelanden med hello Windows Notification Service (WNS):

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Om du sänder tooWNS, så du måste [hämtar Windows Store-paketet SID](#package-sid).  Mer information om Windows-appar, inklusive hur tooregister för mallen registreringar Se [Lägg till push-meddelanden tooyour app].

Begär taggar från klientens hello stöds inte.  Taggen begäranden släpps tyst från registreringen.
Om du vill tooregister enheten med taggar kan skapa en anpassad API som använder hello Notification Hubs API tooperform hello registrering för din räkning.  [Anropa hello anpassade API](#customapi) i stället för hello `RegisterNativeAsync()` metod.

### <a name="package-sid"></a>Så här: hämta en Windows Store paket-SID
Paket-SID krävs för att aktivera push-meddelanden i Windows Store-appar.  tooreceive paket-SID, registrera programmet med hello Windows Store.

tooobtain det här värdet:

1. I Visual Studio Solution Explorer, högerklicka på hello Windows Store-app-projekt, klickar du på **Store** > **associera appen med hello Store...** .
2. Hello i guiden klickar du på **nästa**, logga in med ditt Microsoft-konto, Skriv ett namn för din app i **reservera ett nytt namn för appen**, klicka på **reservera**.
3. När hello appen registreras har skapats, Välj hello programnamn, klickar du på **nästa**, och klicka sedan på **associera**.
4. Logga in toohello [Windows Dev Center] med ditt Microsoft-Account. Under **Mina appar**, klicka på hello appregistrering som du skapade.
5. Klicka på **apphantering** > **App identitet**, och bläddrar sedan nedåt toofind din **paket-SID**.

Många användningsområden för hello paket-SID behandla det som en URI, i vilket fall du behöver toouse *ms-app: / /* som hello schema. Anteckna hello version av ditt paket-SID som bildas genom att det här värdet som ett prefix.

Xamarin-appar kräver vissa ytterligare kod toobe kan tooregister en app som körs på hello iOS eller Android-plattformar. Mer information finns i avsnittet hello för din plattform:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Så här: registrera push-mallar toosend plattformsoberoende meddelanden
tooregister mallar använder hello `RegisterAsync()` metod med hello mallar, på följande sätt:

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Dina mallar ska vara `JObject` datatyper och kan innehålla flera mallar i hello följande JSON-format:

```
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

Hej metoden **RegisterAsync()** sekundära paneler kan också användas:

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Alla taggar tas bort direkt under registreringen för säkerhet. tooadd taggar tooinstallations eller mallar inom installationer finns i avsnittet [fungerar med serverdelen för hello .NET SDK för Azure Mobile Apps].

toosend meddelanden genom att använda mallarna registrerade finns toohello [Notification Hubs API: er].

## <a name="misc"></a>Diverse avsnitt
### <a name="errors"></a>Så här: hantera fel
När ett fel uppstår i hello backend hello klienten SDK genererar en `MobileServiceInvalidOperationException`.  Följande exempel visar hur toohandle ett undantag som returneras av hello backend:

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into hello database. When hello operation completes
    // and App Service has assigned an Id, hello item is added toohello CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

Ett annat exempel för att behandla felvillkor kan hittas i hello [Mobile Apps filer exempel]. Den [LoggingHandler] exempel ger en delegat loggning hanteraren toolog hello begäranden som gjorts toohello backend.

### <a name="headers"></a>Så här: anpassa begärandehuvuden
toosupport specifik app scenario kan du behöva toocustomize kommunikation med hello mobilappsserverdel. Du kan till exempel vill tooadd en anpassad rubrik tooevery utgående begäran eller även ändra svar statuskoder. Du kan använda en anpassad [DelegatingHandler], som i följande exempel hello:

```
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change hello request-side here based on hello HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do hello request
        var response = await base.SendAsync(request, cancellationToken);

        // Change hello response-side here based on hello HttpResponseMessage

        // Return hello modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Lägg till autentisering tooyour app]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline datasynkronisering i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Lägg till push-meddelanden tooyour app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[registrera din app toouse för inloggning till en Microsoft-konto]: app-service-mobile-how-to-configure-microsoft-authentication.md
[hur tooconfigure App Service för Active Directory-inloggningen]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[ MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[skapar en tooan ej typbestämd referenstabell]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[ta]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Välj]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[hoppa över]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Användar-ID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[där]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portalen]: https://portal.azure.com/
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Dev Center]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Notification Hubs API: er]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobile Apps filer exempel]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentationen]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
