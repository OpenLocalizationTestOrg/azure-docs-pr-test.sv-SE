## <span data-ttu-id="7b5e9-101"><a name="create-client"></a>Skapa en klientanslutning</span><span class="sxs-lookup"><span data-stu-id="7b5e9-101"><a name="create-client"></a>Create a client connection</span></span>
<span data-ttu-id="7b5e9-102">Skapa en klientanslutning genom att skapa ett `WindowsAzure.MobileServiceClient`-objekt.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-102">Create a client connection by creating a `WindowsAzure.MobileServiceClient` object.</span></span>  <span data-ttu-id="7b5e9-103">Ersätt `appUrl` med URL-tooyour Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-103">Replace `appUrl` with the URL tooyour Mobile App.</span></span>

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <span data-ttu-id="7b5e9-104"><a name="table-reference"></a>Arbeta med tabeller</span><span class="sxs-lookup"><span data-stu-id="7b5e9-104"><a name="table-reference"></a>Work with tables</span></span>
<span data-ttu-id="7b5e9-105">tooaccess eller uppdatera data, skapa en referenstabell toohello backend.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-105">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="7b5e9-106">Ersätt `tableName` med hello namnet på din tabell</span><span class="sxs-lookup"><span data-stu-id="7b5e9-106">Replace `tableName` with hello name of your table</span></span>

```
var table = client.getTable(tableName);
```

<span data-ttu-id="7b5e9-107">När du har en tabellreferens kan du arbeta mer med din tabell:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-107">Once you have a table reference, you can work further with your table:</span></span>

* [<span data-ttu-id="7b5e9-108">Fråga en tabell</span><span class="sxs-lookup"><span data-stu-id="7b5e9-108">Query a Table</span></span>](#querying)
  * [<span data-ttu-id="7b5e9-109">Filtrera data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-109">Filtering Data</span></span>](#table-filter)
  * [<span data-ttu-id="7b5e9-110">Växla genom data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-110">Paging through Data</span></span>](#table-paging)
  * [<span data-ttu-id="7b5e9-111">Sortera data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-111">Sorting Data</span></span>](#sorting-data)
* [<span data-ttu-id="7b5e9-112">Infoga data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-112">Inserting Data</span></span>](#inserting)
* [<span data-ttu-id="7b5e9-113">Ändra data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-113">Modifying Data</span></span>](#modifying)
* [<span data-ttu-id="7b5e9-114">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-114">Deleting Data</span></span>](#deleting)

### <span data-ttu-id="7b5e9-115"><a name="querying"></a>Anvisning: Fråga en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="7b5e9-115"><a name="querying"></a>How to: Query a table reference</span></span>
<span data-ttu-id="7b5e9-116">När du har en tabellreferens kan använda du det tooquery för data på hello-server.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-116">Once you have a table reference, you can use it tooquery for data on hello server.</span></span>  <span data-ttu-id="7b5e9-117">Frågor görs på ett "LINQ-liknande" språk.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-117">Queries are made in a "LINQ-like" language.</span></span>
<span data-ttu-id="7b5e9-118">tooreturn alla data från hello tabell, Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-118">tooreturn all data from hello table, use hello following code:</span></span>

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

<span data-ttu-id="7b5e9-119">hello lyckade funktionen anropas med hello resultat.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-119">hello success function is called with hello results.</span></span>  <span data-ttu-id="7b5e9-120">Använd inte `for (var i in results)` i hello lyckade fungerar som kommer iterera över information som ingår i hello resultat när andra frågan funktioner (som `.includeTotalCount()`) används.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-120">Do not use `for (var i in results)` in hello success function as that will iterate over information that is included in hello results when other query functions (such as `.includeTotalCount()`) are used.</span></span>

<span data-ttu-id="7b5e9-121">Mer information om hello frågesyntaxen finns hello [fråga objektet dokumentationen].</span><span class="sxs-lookup"><span data-stu-id="7b5e9-121">For more information on hello Query syntax, see hello [Query object documentation].</span></span>

#### <span data-ttu-id="7b5e9-122"><a name="table-filter"></a>Filtrera data på hello-server</span><span class="sxs-lookup"><span data-stu-id="7b5e9-122"><a name="table-filter"></a>Filtering data on hello server</span></span>
<span data-ttu-id="7b5e9-123">Du kan använda en `where` -sats för hello tabellreferens:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-123">You can use a `where` clause on hello table reference:</span></span>

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

<span data-ttu-id="7b5e9-124">Du kan också använda en funktion som filtrerar hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-124">You can also use a function that filters hello object.</span></span>  <span data-ttu-id="7b5e9-125">I det här fallet hello `this` variabeln tilldelas toothe aktuella objekt som ska filtreras.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-125">In this case, hello `this` variable is assigned toothe current object being filtered.</span></span>  <span data-ttu-id="7b5e9-126">hello följande kod är funktionellt motsvarande toohello föregående exempel:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-126">hello following code is functionally equivalent toohello prior example:</span></span>

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <span data-ttu-id="7b5e9-127"><a name="table-paging"></a>Växla genom data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-127"><a name="table-paging"></a>Paging through data</span></span>
<span data-ttu-id="7b5e9-128">Använda hello `take()` och `skip()` metoder.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-128">Utilize hello `take()` and `skip()` methods.</span></span>  <span data-ttu-id="7b5e9-129">Till exempel om du inte vill toosplit hello tabell till rad 100 poster:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-129">For example, if you wish toosplit hello table into 100-row records:</span></span>

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

<span data-ttu-id="7b5e9-130">Hej `.includeTotalCount()` metoden är används tooadd ett totalCount fältet toohello resultat-objekt.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-130">hello `.includeTotalCount()` method is used tooadd a totalCount field toohello results object.</span></span>  <span data-ttu-id="7b5e9-131">TotalCount fylls med hello Totalt antal poster som ska returneras om ingen växling används.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-131">The totalCount field is filled with hello total number of records that would be returned if no paging is used.</span></span>

<span data-ttu-id="7b5e9-132">Du kan sedan använda hello sidor variabeln och vissa UI knappar tooprovide en sida-lista. Använd `loadPage()` att läsa in hello nya poster för varje sida.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-132">You can then use hello pages variable and some UI buttons tooprovide a page list; use `loadPage()` to load hello new records for each page.</span></span>  <span data-ttu-id="7b5e9-133">Implementera cachelagring toospeed åtkomst toorecords som redan har lästs in.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-133">Implement caching toospeed access toorecords that have already been loaded.</span></span>

#### <span data-ttu-id="7b5e9-134"><a name="sorting-data"></a>Anvisning: Returnera sorterade data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-134"><a name="sorting-data"></a>How to: Return sorted data</span></span>
<span data-ttu-id="7b5e9-135">Använd hello `.orderBy()` eller `.orderByDescending()` fråga metoder:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-135">Use hello `.orderBy()` or `.orderByDescending()` query methods:</span></span>

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

<span data-ttu-id="7b5e9-136">Mer information om hello frågeobjekt finns hello [fråga objektet dokumentationen].</span><span class="sxs-lookup"><span data-stu-id="7b5e9-136">For more information on hello Query object, see hello [Query object documentation].</span></span>

### <span data-ttu-id="7b5e9-137"><a name="inserting"></a>Anvisning: Infoga data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-137"><a name="inserting"></a>How to: Insert data</span></span>
<span data-ttu-id="7b5e9-138">Skapa ett JavaScript-objekt med hello datum och anropet `table.insert()` asynkront:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-138">Create a JavaScript object with hello appropriate date and call `table.insert()` asynchronously:</span></span>

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

<span data-ttu-id="7b5e9-139">På lyckad infogning returneras hello infogat objekt med hello ytterligare fält som krävs för synkroniseringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-139">On successful insertion, hello inserted item is returned with hello additional fields that are required for sync operations.</span></span>  <span data-ttu-id="7b5e9-140">Uppdatera ditt eget cacheminne med den här informationen för senare uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-140">Update your own cache with this information for later updates.</span></span>

<span data-ttu-id="7b5e9-141">hello Azure Mobile Apps Node.js Server SDK stöder dynamisk scheman för utveckling.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-141">hello Azure Mobile Apps Node.js Server SDK supports dynamic schema for development purposes.</span></span>  <span data-ttu-id="7b5e9-142">Dynamisk schemat kan du tooadd kolumner toohello tabell genom att ange dem i en insert eller update-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-142">Dynamic Schema allows you tooadd columns toohello table by specifying them in an insert or update operation.</span></span>  <span data-ttu-id="7b5e9-143">Vi rekommenderar att inaktivera dynamisk schemat innan du flyttar dina program tooproduction.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-143">We recommend that you turn off dynamic schema before moving your application tooproduction.</span></span>

### <span data-ttu-id="7b5e9-144"><a name="modifying"></a>Anvisning: Ändra data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-144"><a name="modifying"></a>How to: Modify data</span></span>
<span data-ttu-id="7b5e9-145">Liknande toohello `.insert()` metod, bör du skapa ett objekt för uppdateringen och sedan anropa `.update()`.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-145">Similar toohello `.insert()` method, you should create an Update object and then call `.update()`.</span></span>  <span data-ttu-id="7b5e9-146">hello uppdatera objekt får innehålla hello-ID för hello poster toobe uppdateras - hello ID erhålls vid läsning av hello post eller när du anropar `.insert()`.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-146">hello update object must contain hello ID of hello record toobe updated - hello ID is obtained when reading hello record or when calling `.insert()`.</span></span>

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

### <span data-ttu-id="7b5e9-147"><a name="deleting"></a>Anvisning: Ta bort data</span><span class="sxs-lookup"><span data-stu-id="7b5e9-147"><a name="deleting"></a>How to: Delete data</span></span>
<span data-ttu-id="7b5e9-148">toodelete en post anropet hello `.del()` metod.</span><span class="sxs-lookup"><span data-stu-id="7b5e9-148">toodelete a record, call hello `.del()` method.</span></span>  <span data-ttu-id="7b5e9-149">Ange en objektreferens hello-ID:</span><span class="sxs-lookup"><span data-stu-id="7b5e9-149">Pass hello ID in an object reference:</span></span>

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
