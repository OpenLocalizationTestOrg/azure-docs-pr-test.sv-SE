## <a name="create-client"></a>Skapa en klientanslutning
Skapa en klientanslutning genom att skapa ett `WindowsAzure.MobileServiceClient`-objekt.  Ersätt `appUrl` med URL-tooyour Mobilapp.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="table-reference"></a>Arbeta med tabeller
tooaccess eller uppdatera data, skapa en referenstabell toohello backend. Ersätt `tableName` med hello namnet på din tabell

```
var table = client.getTable(tableName);
```

När du har en tabellreferens kan du arbeta mer med din tabell:

* [Fråga en tabell](#querying)
  * [Filtrera data](#table-filter)
  * [Växla genom data](#table-paging)
  * [Sortera data](#sorting-data)
* [Infoga data](#inserting)
* [Ändra data](#modifying)
* [Ta bort data](#deleting)

### <a name="querying"></a>Anvisning: Fråga en tabellreferens
När du har en tabellreferens kan använda du det tooquery för data på hello-server.  Frågor görs på ett "LINQ-liknande" språk.
tooreturn alla data från hello tabell, Använd hello följande kod:

```
/**
 * Process hello results that are received by a call tootable.read()
 *
 * @param {Object} results hello results as a pseudo-array
 * @param {int} results.length hello length of hello results array
 * @param {Object} results[] hello individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - hello properties are hello columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

hello lyckade funktionen anropas med hello resultat.  Använd inte `for (var i in results)` i hello lyckade fungerar som kommer iterera över information som ingår i hello resultat när andra frågan funktioner (som `.includeTotalCount()`) används.

Mer information om hello frågesyntaxen finns hello [fråga objektet dokumentationen].

#### <a name="table-filter"></a>Filtrera data på hello-server
Du kan använda en `where` -sats för hello tabellreferens:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Du kan också använda en funktion som filtrerar hello-objektet.  I det här fallet hello `this` variabeln tilldelas toothe aktuella objekt som ska filtreras.  hello följande kod är funktionellt motsvarande toohello föregående exempel:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="table-paging"></a>Växla genom data
Använda hello `take()` och `skip()` metoder.  Till exempel om du inte vill toosplit hello tabell till rad 100 poster:

```
var totalCount = 0, pages = 0;

// Step 1 - get hello total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Hej `.includeTotalCount()` metoden är används tooadd ett totalCount fältet toohello resultat-objekt.  TotalCount fylls med hello Totalt antal poster som ska returneras om ingen växling används.

Du kan sedan använda hello sidor variabeln och vissa UI knappar tooprovide en sida-lista. Använd `loadPage()` att läsa in hello nya poster för varje sida.  Implementera cachelagring toospeed åtkomst toorecords som redan har lästs in.

#### <a name="sorting-data"></a>Anvisning: Returnera sorterade data
Använd hello `.orderBy()` eller `.orderByDescending()` fråga metoder:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Mer information om hello frågeobjekt finns hello [fråga objektet dokumentationen].

### <a name="inserting"></a>Anvisning: Infoga data
Skapa ett JavaScript-objekt med hello datum och anropet `table.insert()` asynkront:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

På lyckad infogning returneras hello infogat objekt med hello ytterligare fält som krävs för synkroniseringsåtgärder.  Uppdatera ditt eget cacheminne med den här informationen för senare uppdateringar.

hello Azure Mobile Apps Node.js Server SDK stöder dynamisk scheman för utveckling.  Dynamisk schemat kan du tooadd kolumner toohello tabell genom att ange dem i en insert eller update-åtgärd.  Vi rekommenderar att inaktivera dynamisk schemat innan du flyttar dina program tooproduction.

### <a name="modifying"></a>Anvisning: Ändra data
Liknande toohello `.insert()` metod, bör du skapa ett objekt för uppdateringen och sedan anropa `.update()`.  hello uppdatera objekt får innehålla hello-ID för hello poster toobe uppdateras - hello ID erhålls vid läsning av hello post eller när du anropar `.insert()`.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="deleting"></a>Anvisning: Ta bort data
toodelete en post anropet hello `.del()` metod.  Ange en objektreferens hello-ID:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
