---
title: "Så här används iOS SDK för Azure Mobile Apps"
description: "Så här används iOS SDK för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: 65817208e1b26fb5f9eb56d164f48b44d57dce56
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a><span data-ttu-id="d8ed7-103">Så här används iOS-klientbiblioteket för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="d8ed7-103">How to Use iOS Client Library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="d8ed7-104">Den här guiden lär du dig att utföra vanliga scenarier med senast [Azure Mobile Apps iOS SDK][1].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-104">This guide teaches you to perform common scenarios using the latest [Azure Mobile Apps iOS SDK][1].</span></span> <span data-ttu-id="d8ed7-105">Om du har använt Azure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] för att skapa en serverdel, skapa en tabell och hämta en förskapad iOS Xcode-projekt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend, create a table, and download a pre-built iOS Xcode project.</span></span> <span data-ttu-id="d8ed7-106">I den här guiden fokuserar vi på klientsidan iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-106">In this guide, we focus on the client-side iOS SDK.</span></span> <span data-ttu-id="d8ed7-107">Mer information om SDK för serversidan för backend finns Server SDK HOWTOs.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-107">To learn more about the server-side SDK for the backend, see the Server SDK HOWTOs.</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="d8ed7-108">Referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="d8ed7-108">Reference documentation</span></span>
<span data-ttu-id="d8ed7-109">Referensdokumentationen för klientens iOS SDK finns här: [Azure Mobile Apps iOS klienten referens][2].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-109">The reference documentation for the iOS client SDK is located here: [Azure Mobile Apps iOS Client Reference][2].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d8ed7-110">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="d8ed7-110">Supported Platforms</span></span>
<span data-ttu-id="d8ed7-111">IOS SDK stöder Objective-C-projekt, Swift 2.2 projekt och Swift 2.3 projekt för iOS version 8.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-111">The iOS SDK supports Objective-C projects, Swift 2.2 projects, and Swift 2.3 projects for iOS versions 8.0 or later.</span></span>

<span data-ttu-id="d8ed7-112">”Server flöde” autentisering använder en webbvy för presenterades Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-112">The "server-flow" authentication uses a WebView for the presented UI.</span></span>  <span data-ttu-id="d8ed7-113">Om enheten inte kunna presentera en WebView UI, kan en annan metod för autentisering krävs som inte omfattas av produkten.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-113">If the device is not able to present a WebView UI, then another method of authentication is required that is outside the scope of the product.</span></span>  
<span data-ttu-id="d8ed7-114">Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-114">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="d8ed7-115"><a name="Setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="d8ed7-115"><a name="Setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="d8ed7-116">Den här handboken förutsätts att du har skapat en serverdel med en tabell.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-116">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="d8ed7-117">Den här guiden förutsätter att tabellen har samma schema som tabellerna i dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-117">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span> <span data-ttu-id="d8ed7-118">Den här guiden förutsätter också att i din kod måste du referera till `MicrosoftAzureMobile.framework` och importera `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-118">This guide also assumes that in your code, you reference `MicrosoftAzureMobile.framework` and import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span></span>

## <span data-ttu-id="d8ed7-119"><a name="create-client"></a>Så här: skapa klienten</span><span class="sxs-lookup"><span data-stu-id="d8ed7-119"><a name="create-client"></a>How to: Create Client</span></span>
<span data-ttu-id="d8ed7-120">För att komma åt en Azure Mobile Apps-serverdel i projektet, skapa en `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-120">To access an Azure Mobile Apps backend in your project, create an `MSClient`.</span></span> <span data-ttu-id="d8ed7-121">Ersätt `AppUrl` med app-URL.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-121">Replace `AppUrl` with the app URL.</span></span> <span data-ttu-id="d8ed7-122">Du kan lämna `gatewayURLString` och `applicationKey` tom.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-122">You may leave `gatewayURLString` and `applicationKey` empty.</span></span> <span data-ttu-id="d8ed7-123">Om du ställer in en gateway för autentisering fylla `gatewayURLString` med gateway-URL.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-123">If you set up a gateway for authentication, populate `gatewayURLString` with the gateway URL.</span></span>

<span data-ttu-id="d8ed7-124">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-124">**Objective-C**:</span></span>

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

<span data-ttu-id="d8ed7-125">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-125">**Swift**:</span></span>

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <span data-ttu-id="d8ed7-126"><a name="table-reference"></a>Så här: skapa tabellreferens</span><span class="sxs-lookup"><span data-stu-id="d8ed7-126"><a name="table-reference"></a>How to: Create Table Reference</span></span>
<span data-ttu-id="d8ed7-127">För att få åtkomst till eller uppdatera data skapar du en referens till serverdelstabellen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-127">To access or update data, create a reference to the backend table.</span></span> <span data-ttu-id="d8ed7-128">Ersätt `TodoItem` med namnet på tabellen</span><span class="sxs-lookup"><span data-stu-id="d8ed7-128">Replace `TodoItem` with the name of your table</span></span>

<span data-ttu-id="d8ed7-129">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-129">**Objective-C**:</span></span>

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

<span data-ttu-id="d8ed7-130">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-130">**Swift**:</span></span>

```
let table = client.tableWithName("TodoItem")
```


## <span data-ttu-id="d8ed7-131"><a name="querying"></a>Så här: fråga Data</span><span class="sxs-lookup"><span data-stu-id="d8ed7-131"><a name="querying"></a>How to: Query Data</span></span>
<span data-ttu-id="d8ed7-132">Om du vill skapa en databasfråga fråga den `MSTable` objekt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-132">To create a database query, query the `MSTable` object.</span></span> <span data-ttu-id="d8ed7-133">Följande fråga hämtar alla objekt i `TodoItem` och loggar texten för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-133">The following query gets all the items in `TodoItem` and logs the text of each item.</span></span>

<span data-ttu-id="d8ed7-134">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-134">**Objective-C**:</span></span>

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="d8ed7-135">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-135">**Swift**:</span></span>

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="d8ed7-136"><a name="filtering"></a>Så här: filtret returnerade Data</span><span class="sxs-lookup"><span data-stu-id="d8ed7-136"><a name="filtering"></a>How to: Filter Returned Data</span></span>
<span data-ttu-id="d8ed7-137">Det finns många tillgängliga alternativ för att filtrera resultat.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-137">To filter results, there are many available options.</span></span>

<span data-ttu-id="d8ed7-138">Om du vill filtrera med hjälp av ett predikat, använda en `NSPredicate` och `readWithPredicate`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-138">To filter using a predicate, use an `NSPredicate` and `readWithPredicate`.</span></span> <span data-ttu-id="d8ed7-139">Följande filter returnerade data för att hitta endast ofullständiga Todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-139">The following filters returned data to find only incomplete Todo items.</span></span>

<span data-ttu-id="d8ed7-140">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-140">**Objective-C**:</span></span>

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="d8ed7-141">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-141">**Swift**:</span></span>

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="d8ed7-142"><a name="query-object"></a>Så här: använda MSQuery</span><span class="sxs-lookup"><span data-stu-id="d8ed7-142"><a name="query-object"></a>How to: Use MSQuery</span></span>
<span data-ttu-id="d8ed7-143">Om du vill utföra en komplex fråga (inklusive sortering och sidindelning) skapar du en `MSQuery` objekt, direkt eller genom att använda ett predikat:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-143">To perform a complex query (including sorting and paging), create an `MSQuery` object, directly or by using a predicate:</span></span>

<span data-ttu-id="d8ed7-144">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-144">**Objective-C**:</span></span>

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

<span data-ttu-id="d8ed7-145">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-145">**Swift**:</span></span>

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

<span data-ttu-id="d8ed7-146">`MSQuery`kan du styra flera olika beteenden för frågan.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-146">`MSQuery` lets you control several query behaviors.</span></span>

* <span data-ttu-id="d8ed7-147">Ange ordning av resultat</span><span class="sxs-lookup"><span data-stu-id="d8ed7-147">Specify order of results</span></span>
* <span data-ttu-id="d8ed7-148">Begränsa vilka fält som ska returneras</span><span class="sxs-lookup"><span data-stu-id="d8ed7-148">Limit which fields to return</span></span>
* <span data-ttu-id="d8ed7-149">Begränsa hur många poster för att returnera</span><span class="sxs-lookup"><span data-stu-id="d8ed7-149">Limit how many records to return</span></span>
* <span data-ttu-id="d8ed7-150">Ange totalt antal i svaret</span><span class="sxs-lookup"><span data-stu-id="d8ed7-150">Specify total count in response</span></span>
* <span data-ttu-id="d8ed7-151">Ange anpassad fråga string-parametrar i begäran</span><span class="sxs-lookup"><span data-stu-id="d8ed7-151">Specify custom query string parameters in request</span></span>
* <span data-ttu-id="d8ed7-152">Tillämpa ytterligare funktioner</span><span class="sxs-lookup"><span data-stu-id="d8ed7-152">Apply additional functions</span></span>

<span data-ttu-id="d8ed7-153">Köra en `MSQuery` frågan genom att anropa `readWithCompletion` på objektet.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-153">Execute an `MSQuery` query by calling `readWithCompletion` on the object.</span></span>

## <span data-ttu-id="d8ed7-154"><a name="sorting"></a>Så här: sortera Data med MSQuery</span><span class="sxs-lookup"><span data-stu-id="d8ed7-154"><a name="sorting"></a>How to: Sort Data with MSQuery</span></span>
<span data-ttu-id="d8ed7-155">För att sortera resultaten ska vi titta på ett exempel.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-155">To sort results, let's look at an example.</span></span> <span data-ttu-id="d8ed7-156">Om du vill sortera efter fältet 'text' stigande sedan efter ”klar” fallande anropa `MSQuery` t.ex:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-156">To sort by field 'text' ascending, then by 'complete' descending, invoke `MSQuery` like so:</span></span>

<span data-ttu-id="d8ed7-157">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-157">**Objective-C**:</span></span>

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="d8ed7-158">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-158">**Swift**:</span></span>

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <span data-ttu-id="d8ed7-159"><a name="selecting"></a><a name="parameters"></a>Så här: begränsa fält och expandera frågeparametrar sträng med MSQuery</span><span class="sxs-lookup"><span data-stu-id="d8ed7-159"><a name="selecting"></a><a name="parameters"></a>How to: Limit Fields and Expand Query String Parameters with MSQuery</span></span>
<span data-ttu-id="d8ed7-160">Om du vill begränsa fält som ska returneras i en fråga, ange namnen på fälten i den **selectFields** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-160">To limit fields to be returned in a query, specify the names of the fields in the **selectFields** property.</span></span> <span data-ttu-id="d8ed7-161">Det här exemplet returnerar endast text och slutförda fält:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-161">This example returns only the text and completed fields:</span></span>

<span data-ttu-id="d8ed7-162">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-162">**Objective-C**:</span></span>

```
query.selectFields = @[@"text", @"complete"];
```

<span data-ttu-id="d8ed7-163">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-163">**Swift**:</span></span>

```
query.selectFields = ["text", "complete"]
```

<span data-ttu-id="d8ed7-164">Om du vill inkludera ytterligare fråga string-parametrar i serverbegäran (till exempel eftersom ett anpassat skript för serversidan använder dem) fylla `query.parameters` t.ex:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-164">To include additional query string parameters in the server request (for example, because a custom server-side script uses them), populate `query.parameters` like so:</span></span>

<span data-ttu-id="d8ed7-165">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-165">**Objective-C**:</span></span>

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

<span data-ttu-id="d8ed7-166">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-166">**Swift**:</span></span>

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <span data-ttu-id="d8ed7-167"><a name="paging"></a>Så här: Konfigurera sidstorlek</span><span class="sxs-lookup"><span data-stu-id="d8ed7-167"><a name="paging"></a>How to: Configure Page Size</span></span>
<span data-ttu-id="d8ed7-168">Med Azure Mobile Apps styr sidstorleken antalet poster som hämtas i taget från backend-tabeller.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-168">With Azure Mobile Apps, the page size controls the number of records that are pulled at a time from the backend tables.</span></span> <span data-ttu-id="d8ed7-169">Ett anrop till `pull` data skulle sedan batch in data, baserat på den här sidstorleken tills det inte finns några fler poster att dra.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-169">A call to `pull` data would then batch up data, based on this page size, until there are no more records to pull.</span></span>

<span data-ttu-id="d8ed7-170">Det är möjligt att konfigurera en storlek med **MSPullSettings** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-170">It's possible to configure a page size using **MSPullSettings** as shown below.</span></span> <span data-ttu-id="d8ed7-171">Sidan standardstorleken är 50 och exemplet nedan ändrar till 3.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-171">The default page size is 50, and the example below changes it to 3.</span></span>

<span data-ttu-id="d8ed7-172">Du kan konfigurera en annan sidstorlek av prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-172">You could configure a different page size for performance reasons.</span></span> <span data-ttu-id="d8ed7-173">Om du har ett stort antal poster för små, minskar en hög sidstorlek antalet turer server.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-173">If you have a large number of small data records, a high page size reduces the number of server round-trips.</span></span>

<span data-ttu-id="d8ed7-174">Den här inställningen styr endast sidstorleken på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-174">This setting controls only the page size on the client side.</span></span> <span data-ttu-id="d8ed7-175">Om klienten begär en större storlek än Mobile Apps-serverdel stöder, är sidstorleken begränsat till den maximala serverdelen är konfigurerad för att stödja.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-175">If the client asks for a larger page size than the Mobile Apps backend supports, the page size is capped at the maximum the backend is configured to support.</span></span>

<span data-ttu-id="d8ed7-176">Den här inställningen är också den *nummer* dataposter, inte den *bytestorlek*.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-176">This setting is also the *number* of data records, not the *byte size*.</span></span>

<span data-ttu-id="d8ed7-177">Om du ökar storleken på klienten, bör du också öka sidstorleken på servern.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-177">If you increase the client page size, you should also increase the page size on the server.</span></span> <span data-ttu-id="d8ed7-178">Se [”så här: justera tabellen växlingsfilens storlek”](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) stegen för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-178">See ["How to: Adjust the table paging size"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for the steps to do this.</span></span>

<span data-ttu-id="d8ed7-179">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-179">**Objective-C**:</span></span>

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


<span data-ttu-id="d8ed7-180">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-180">**Swift**:</span></span>

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <span data-ttu-id="d8ed7-181"><a name="inserting"></a>Så här: Infoga Data</span><span class="sxs-lookup"><span data-stu-id="d8ed7-181"><a name="inserting"></a>How to: Insert Data</span></span>
<span data-ttu-id="d8ed7-182">Om du vill infoga en ny tabellrad, skapa en `NSDictionary` och anropa `table insert`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-182">To insert a new table row, create a `NSDictionary` and invoke `table insert`.</span></span> <span data-ttu-id="d8ed7-183">Om [dynamiska schemat] är aktiverad kan Azure App Service mobilserverdel genererar automatiskt nya kolumner som är baserat på den `NSDictionary`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-183">If [Dynamic Schema] is enabled, the Azure App Service mobile backend automatically generates new columns based on the `NSDictionary`.</span></span>

<span data-ttu-id="d8ed7-184">Om `id` har inte angetts serverdelen genererar automatiskt ett nytt unikt ID.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-184">If `id` is not provided, the backend automatically generates a new unique ID.</span></span> <span data-ttu-id="d8ed7-185">Ange en egen `id` att använda e-post-adresser, användarnamn, eller dina egna anpassade värden som ID.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-185">Provide your own `id` to use email addresses, usernames, or your own custom values as ID.</span></span> <span data-ttu-id="d8ed7-186">Tillhandahåller egna ID kan underlätta kopplingar och affärsorienterade databasen logik.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-186">Providing your own ID may ease joins and business-oriented database logic.</span></span>

<span data-ttu-id="d8ed7-187">Den `result` innehåller det nya objekt som har infogats.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-187">The `result` contains the new item that was inserted.</span></span> <span data-ttu-id="d8ed7-188">Det kan ha ytterligare eller ändrade data jämfört med vad som har skickats till servern beroende på din server-logiken.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-188">Depending on your server logic, it may have additional or modified data compared to what was passed to the server.</span></span>

<span data-ttu-id="d8ed7-189">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-189">**Objective-C**:</span></span>

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="d8ed7-190">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-190">**Swift**:</span></span>

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <span data-ttu-id="d8ed7-191"><a name="modifying"></a>Så här: ändra Data</span><span class="sxs-lookup"><span data-stu-id="d8ed7-191"><a name="modifying"></a>How to: Modify Data</span></span>
<span data-ttu-id="d8ed7-192">Om du vill uppdatera en befintlig rad, ändra ett objekt och anropet `update`:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-192">To update an existing row, modify an item and call `update`:</span></span>

<span data-ttu-id="d8ed7-193">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-193">**Objective-C**:</span></span>

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="d8ed7-194">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-194">**Swift**:</span></span>

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

<span data-ttu-id="d8ed7-195">Du kan också ange rad-ID och det uppdaterade fältet:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-195">Alternatively, supply the row ID and the updated field:</span></span>

<span data-ttu-id="d8ed7-196">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-196">**Objective-C**:</span></span>

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="d8ed7-197">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-197">**Swift**:</span></span>

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

<span data-ttu-id="d8ed7-198">Åtminstone den `id` attributet måste anges när du gör uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-198">At minimum, the `id` attribute must be set when making updates.</span></span>

## <span data-ttu-id="d8ed7-199"><a name="deleting"></a>Så här: ta bort Data</span><span class="sxs-lookup"><span data-stu-id="d8ed7-199"><a name="deleting"></a>How to: Delete Data</span></span>
<span data-ttu-id="d8ed7-200">Ta bort ett objekt genom att anropa `delete` med objektet:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-200">To delete an item, invoke `delete` with the item:</span></span>

<span data-ttu-id="d8ed7-201">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-201">**Objective-C**:</span></span>

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="d8ed7-202">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-202">**Swift**:</span></span>

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="d8ed7-203">Du kan också ta bort genom att tillhandahålla en rad-ID:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-203">Alternatively, delete by providing a row ID:</span></span>

<span data-ttu-id="d8ed7-204">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-204">**Objective-C**:</span></span>

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="d8ed7-205">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-205">**Swift**:</span></span>

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="d8ed7-206">Åtminstone den `id` attributet måste anges när att tas bort.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-206">At minimum, the `id` attribute must be set when making deletes.</span></span>

## <span data-ttu-id="d8ed7-207"><a name="customapi"></a>Så här: anropa anpassade API</span><span class="sxs-lookup"><span data-stu-id="d8ed7-207"><a name="customapi"></a>How to: Call Custom API</span></span>
<span data-ttu-id="d8ed7-208">Med en anpassad API kan du exponera backend funktioner.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-208">With a custom API, you can expose any backend functionality.</span></span> <span data-ttu-id="d8ed7-209">Den har inte mappas till en Tabellåtgärd för.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-209">It doesn't have to map to a table operation.</span></span> <span data-ttu-id="d8ed7-210">Inte bara gör du få bättre kontroll över meddelanden, du kan även läsa/set sidhuvuden och ändra svar body-formatet.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-210">Not only do you gain more control over messaging, you can even read/set headers and change the response body format.</span></span> <span data-ttu-id="d8ed7-211">Om du vill veta hur du skapar en anpassad API på serverdelen läsa [anpassade API: er](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span><span class="sxs-lookup"><span data-stu-id="d8ed7-211">To learn how to create a custom API on the backend, read [Custom APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span></span>

<span data-ttu-id="d8ed7-212">För att anropa en anpassad API, anropa `MSClient.invokeAPI`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-212">To call a custom API, call `MSClient.invokeAPI`.</span></span> <span data-ttu-id="d8ed7-213">Förfrågan och svar innehåll behandlas som JSON.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-213">The request and response content are treated as JSON.</span></span> <span data-ttu-id="d8ed7-214">Att använda andra typer av media, [Använd överlagringen för `invokeAPI` ] [ 5].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-214">To use other media types, [use the other overload of `invokeAPI`][5].</span></span>  <span data-ttu-id="d8ed7-215">Att göra en `GET` begäran i stället för en `POST` begära parameter för `HTTPMethod` till `"GET"` och parametern `body` till `nil` (eftersom GET-begäranden saknar meddelandetext.) Om din anpassade API: et stöder andra HTTP-verb, ändra `HTTPMethod` på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-215">To make a `GET` request instead of a `POST` request, set parameter `HTTPMethod` to `"GET"` and parameter `body` to `nil` (since GET requests do not have message bodies.) If your custom API supports other HTTP verbs, change `HTTPMethod` appropriately.</span></span>

<span data-ttu-id="d8ed7-216">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-216">**Objective-C**:</span></span>

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

<span data-ttu-id="d8ed7-217">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-217">**Swift**:</span></span>

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <span data-ttu-id="d8ed7-218"><a name="templates"></a>Så här: registrera push-mallar för att skicka plattformsoberoende meddelanden</span><span class="sxs-lookup"><span data-stu-id="d8ed7-218"><a name="templates"></a>How to: Register push templates to send cross-platform notifications</span></span>
<span data-ttu-id="d8ed7-219">Om du vill registrera mallar skicka mallar med din **client.push registerDeviceToken** metod i klientappen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-219">To register templates, pass templates with your **client.push registerDeviceToken** method in your client app.</span></span>

<span data-ttu-id="d8ed7-220">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-220">**Objective-C**:</span></span>

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

<span data-ttu-id="d8ed7-221">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-221">**Swift**:</span></span>

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

<span data-ttu-id="d8ed7-222">Mallarna är av typen NSDictionary och kan innehålla flera mallar i följande format:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-222">Your templates are of type NSDictionary and can contain multiple templates in the following format:</span></span>

<span data-ttu-id="d8ed7-223">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-223">**Objective-C**:</span></span>

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

<span data-ttu-id="d8ed7-224">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-224">**Swift**:</span></span>

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

<span data-ttu-id="d8ed7-225">Alla taggar tas bort från begäran för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-225">All tags are stripped from the request for security.</span></span>  <span data-ttu-id="d8ed7-226">Om du vill lägga till taggar i installationer eller mallar inom anläggningar finns [arbeta med serverdelen .NET SDK för Azure Mobile Apps][4].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-226">To add tags to installations or templates within installations, see [Work with the .NET backend server SDK for Azure Mobile Apps][4].</span></span>  <span data-ttu-id="d8ed7-227">Om du vill skicka meddelanden med mallarna registrerade arbeta med [Notification Hubs API: er][3].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-227">To send notifications using these registered templates, work with [Notification Hubs APIs][3].</span></span>

## <span data-ttu-id="d8ed7-228"><a name="errors"></a>Så här: hantera fel</span><span class="sxs-lookup"><span data-stu-id="d8ed7-228"><a name="errors"></a>How to: Handle Errors</span></span>
<span data-ttu-id="d8ed7-229">När du anropar en mobila serverdel i Azure App Service, slutförande blocket innehåller en `NSError` parameter.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-229">When you call an Azure App Service mobile backend, the completion block contains an `NSError` parameter.</span></span> <span data-ttu-id="d8ed7-230">När ett fel uppstår i den här parametern är inte är nil.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-230">When an error occurs, this parameter is non-nil.</span></span> <span data-ttu-id="d8ed7-231">I koden, bör du kontrollera den här parametern och hantera fel efter behov, som visas i föregående kodfragment.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-231">In your code, you should check this parameter and handle the error as needed, as demonstrated in the preceding code snippets.</span></span>

<span data-ttu-id="d8ed7-232">Filen [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definierar konstanterna `MSErrorResponseKey`, `MSErrorRequestKey`, och `MSErrorServerItemKey`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-232">The file [`<WindowsAzureMobileServices/MSError.h>`][6] defines the constants `MSErrorResponseKey`, `MSErrorRequestKey`, and `MSErrorServerItemKey`.</span></span> <span data-ttu-id="d8ed7-233">Att få mer information om felet:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-233">To get more data related to the error:</span></span>

<span data-ttu-id="d8ed7-234">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-234">**Objective-C**:</span></span>

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

<span data-ttu-id="d8ed7-235">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-235">**Swift**:</span></span>

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

<span data-ttu-id="d8ed7-236">Dessutom kan definierar filen konstanter för respektive felkod:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-236">In addition, the file defines constants for each error code:</span></span>

<span data-ttu-id="d8ed7-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-237">**Objective-C**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

<span data-ttu-id="d8ed7-238">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-238">**Swift**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

## <span data-ttu-id="d8ed7-239"><a name="adal"></a>Så här: autentiserar användare med Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="d8ed7-239"><a name="adal"></a>How to: Authenticate users with the Active Directory Authentication Library</span></span>
<span data-ttu-id="d8ed7-240">Du kan använda Active Directory Authentication Library (ADAL) för att logga in användare på ditt program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-240">You can use the Active Directory Authentication Library (ADAL) to sign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="d8ed7-241">Flödet för klientautentisering med hjälp av en identitetsleverantör SDK är bättre än att använda den `loginWithProvider:completion:` metoden.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-241">Client flow authentication using an identity provider SDK is preferable to using the `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="d8ed7-242">Flöde för klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-242">Client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="d8ed7-243">Konfigurera mobilappsserverdelen för AAD-inloggning genom att följa den [konfigurera App Service för Active Directory-inloggningen] [ 7] kursen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-243">Configure your mobile app backend for AAD sign-in by following the [How to configure App Service for Active Directory login][7] tutorial.</span></span> <span data-ttu-id="d8ed7-244">Se till att slutföra det valfria steget med att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-244">Make sure to complete the optional step of registering a native client application.</span></span> <span data-ttu-id="d8ed7-245">För iOS, rekommenderar vi att omdirigerings-URI: N har formatet `<app-scheme>://<bundle-id>`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-245">For iOS, we recommend that the redirect URI is of the form `<app-scheme>://<bundle-id>`.</span></span> <span data-ttu-id="d8ed7-246">Mer information finns i [ADAL iOS quickstart][8].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-246">For more information, see the [ADAL iOS quickstart][8].</span></span>
2. <span data-ttu-id="d8ed7-247">Installera ADAL med Cocoapods.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-247">Install ADAL using Cocoapods.</span></span> <span data-ttu-id="d8ed7-248">Redigera din Podfile för att inkludera följande definition ersätter **din PROJECT** med namnet på ditt Xcode-projekt:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-248">Edit your Podfile to include the following definition, replacing **YOUR-PROJECT** with the name of your Xcode project:</span></span>

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   <span data-ttu-id="d8ed7-249">och baljor:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-249">and the Pod:</span></span>

        pod 'ADALiOS'
3. <span data-ttu-id="d8ed7-250">Använda terminalen och kör `pod install` från katalogen som innehåller ditt projekt och sedan öppna den genererade Xcode-arbetsytan (inte projektet).</span><span class="sxs-lookup"><span data-stu-id="d8ed7-250">Using the Terminal, run `pod install` from the directory containing your project, and then open the generated Xcode workspace (not the project).</span></span>
4. <span data-ttu-id="d8ed7-251">Lägg till följande kod i ditt program, beroende på vilket språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-251">Add the following code to your application, according to the language you are using.</span></span> <span data-ttu-id="d8ed7-252">I varje, se dessa ersättningar:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-252">In each, make these replacements:</span></span>

   * <span data-ttu-id="d8ed7-253">Ersätt **INSERT-UTFÄRDARE-här** med namnet på klienten som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-253">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="d8ed7-254">Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-254">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span> <span data-ttu-id="d8ed7-255">Det här värdet kan kopieras från fliken domän i Azure Active Directory på den [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-255">This value can be copied from the Domain tab in your Azure Active Directory in the [Azure classic portal].</span></span>
   * <span data-ttu-id="d8ed7-256">Ersätt **INSERT-resurs-ID-här** med klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-256">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="d8ed7-257">Du kan hämta klient-ID från den **Avancerat** fliken **inställningarna för Azure Active Directory** i portalen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-257">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
   * <span data-ttu-id="d8ed7-258">Ersätt **INSERT-klient-ID-här** med klient-ID som du kopierade från native client-program.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-258">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
   * <span data-ttu-id="d8ed7-259">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-259">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="d8ed7-260">Det här värdet ska vara liknar *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-260">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

<span data-ttu-id="d8ed7-261">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-261">**Objective-C**:</span></span>

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


<span data-ttu-id="d8ed7-262">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-262">**Swift**:</span></span>

    // add the following imports to your bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <span data-ttu-id="d8ed7-263"><a name="facebook-sdk"></a>Så här: autentiserar användare med Facebook-SDK för iOS</span><span class="sxs-lookup"><span data-stu-id="d8ed7-263"><a name="facebook-sdk"></a>How to: Authenticate users with the Facebook SDK for iOS</span></span>
<span data-ttu-id="d8ed7-264">Du kan använda Facebook SDK för iOS för att logga in användare på ditt program med hjälp av Facebook.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-264">You can use the Facebook SDK for iOS to sign users into your application using Facebook.</span></span>  <span data-ttu-id="d8ed7-265">Med hjälp av en flödet för klientautentisering är bättre än att använda den `loginWithProvider:completion:` metoden.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-265">Using a client flow authentication is preferable to using the `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="d8ed7-266">Flödet klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-266">The client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="d8ed7-267">Konfigurera mobilappsserverdelen för Facebook-inloggning genom att följa den [konfigurera App Service för Facebook-inloggningen] [ 9] kursen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-267">Configure your mobile app backend for Facebook sign-in by following the [How to configure App Service for Facebook login][9] tutorial.</span></span>
2. <span data-ttu-id="d8ed7-268">Installera Facebook-SDK för iOS genom att följa den [Facebook SDK för iOS - komma igång] [ 10] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-268">Install the Facebook SDK for iOS by following the [Facebook SDK for iOS - Getting Started][10] documentation.</span></span> <span data-ttu-id="d8ed7-269">Istället för att skapa en app kan du lägga till iOS-plattformen befintliga registreringen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-269">Instead of creating an app, you can add the iOS platform to your existing registration.</span></span>
3. <span data-ttu-id="d8ed7-270">Facebooks dokumentationen innehåller vissa Objective-C-koden i appen delegaten.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-270">Facebook's documentation includes some Objective-C code in the App Delegate.</span></span> <span data-ttu-id="d8ed7-271">Om du använder **Swift**, du kan använda följande översättningar för AppDelegate.swift:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-271">If you are using **Swift**, you can use the following translations for AppDelegate.swift:</span></span>

        // Add the following import to your bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. <span data-ttu-id="d8ed7-272">Förutom att lägga till `FBSDKCoreKit.framework` till ditt projekt du också lägga till en referens till `FBSDKLoginKit.framework` på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-272">In addition to adding `FBSDKCoreKit.framework` to your project, also add a reference to `FBSDKLoginKit.framework` in the same way.</span></span>
5. <span data-ttu-id="d8ed7-273">Lägg till följande kod i ditt program, beroende på vilket språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-273">Add the following code to your application, according to the language you are using.</span></span>

<span data-ttu-id="d8ed7-274">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-274">**Objective-C**:</span></span>

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

<span data-ttu-id="d8ed7-275">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-275">**Swift**:</span></span>

    // Add the following imports to your bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <span data-ttu-id="d8ed7-276"><a name="twitter-fabric"></a>Så här: autentiserar användare med Twitter-infrastrukturen för iOS</span><span class="sxs-lookup"><span data-stu-id="d8ed7-276"><a name="twitter-fabric"></a>How to: Authenticate users with Twitter Fabric for iOS</span></span>
<span data-ttu-id="d8ed7-277">Du kan använda infrastruktur för iOS för att logga in användare på ditt program med Twitter.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-277">You can use Fabric for iOS to sign users into your application using Twitter.</span></span> <span data-ttu-id="d8ed7-278">Flöde för klientautentisering är bättre än att använda den `loginWithProvider:completion:` metoden eftersom den innehåller en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-278">Client Flow authentication is preferable to using the `loginWithProvider:completion:` method, as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="d8ed7-279">Konfigurera mobilappsserverdelen för inloggning Twitter genom att följa den [konfigurera App Service för Twitter inloggningen](app-service-mobile-how-to-configure-twitter-authentication.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-279">Configure your mobile app backend for Twitter sign-in by following the [How to configure App Service for Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="d8ed7-280">Lägg till Fabric i projektet genom att följa den [infrastruktur för iOS - komma igång] dokumentationen och konfigurera TwitterKit.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-280">Add Fabric to your project by following the [Fabric for iOS - Getting Started] documentation and setting up TwitterKit.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d8ed7-281">Som standard skapas Fabric ett Twitter-program.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-281">By default, Fabric creates a Twitter application for you.</span></span> <span data-ttu-id="d8ed7-282">Du kan undvika att skapa ett program genom att registrera den konsumenten och konsumenthemlighet som du skapat tidigare med följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-282">You can avoid creating an application by registering the Consumer Key and Consumer Secret you created earlier using the following code snippets.</span></span>    <span data-ttu-id="d8ed7-283">Alternativt kan du ersätta konsumentnyckel och konsumenthemlighet värdena som du anger till App Service med värden som visas i den [instrumentpanelen].</span><span class="sxs-lookup"><span data-stu-id="d8ed7-283">Alternatively, you can replace the Consumer Key and Consumer Secret values that you provide to App Service with the values you see in the [Fabric Dashboard].</span></span> <span data-ttu-id="d8ed7-284">Om du väljer det här alternativet måste du ange URL: en motringning till ett platshållarvärde som `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-284">If you choose this option, be sure to set the callback URL to a placeholder value, such as `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span></span>
   >
   >

    <span data-ttu-id="d8ed7-285">Lägg till följande kod i ombudet App om du väljer att använda hemligheter som du skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-285">If you choose to use the secrets you created earlier, add the following code to your App Delegate:</span></span>

    <span data-ttu-id="d8ed7-286">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-286">**Objective-C**:</span></span>

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    <span data-ttu-id="d8ed7-287">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-287">**Swift**:</span></span>

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. <span data-ttu-id="d8ed7-288">Lägg till följande kod i ditt program, beroende på vilket språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-288">Add the following code to your application, according to the language you are using.</span></span>

<span data-ttu-id="d8ed7-289">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-289">**Objective-C**:</span></span>

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

<span data-ttu-id="d8ed7-290">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-290">**Swift**:</span></span>

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <span data-ttu-id="d8ed7-291"><a name="google-sdk"></a>Så här: autentiserar användare med Google inloggning SDK för iOS</span><span class="sxs-lookup"><span data-stu-id="d8ed7-291"><a name="google-sdk"></a>How to: Authenticate users with the Google Sign-In SDK for iOS</span></span>
<span data-ttu-id="d8ed7-292">Du kan använda Google inloggning SDK för iOS för att logga in användare på ditt program med hjälp av ett Google-konto.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-292">You can use the Google Sign-In SDK for iOS to sign users into your application using a Google account.</span></span>  <span data-ttu-id="d8ed7-293">Google presenterade nyligen ändringar av deras OAuth säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-293">Google recently announced changes to their OAuth security policies.</span></span>  <span data-ttu-id="d8ed7-294">Dessa principändringar kräver användning av Google-SDK i framtiden.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-294">These policy changes will require the use of the Google SDK in the future.</span></span>

1. <span data-ttu-id="d8ed7-295">Konfigurera mobilappsserverdelen för Google-inloggning genom att följa den [konfigurera App Service för Google inloggningen](app-service-mobile-how-to-configure-google-authentication.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-295">Configure your mobile app backend for Google sign-in by following the [How to configure App Service for Google login](app-service-mobile-how-to-configure-google-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="d8ed7-296">Installera Google-SDK för iOS genom att följa den [starta Google Sign-In för iOS - integreringen](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-296">Install the Google SDK for iOS by following the [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) documentation.</span></span> <span data-ttu-id="d8ed7-297">Du kan hoppa över avsnittet ”autentisera med en Backend-servern”.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-297">You may skip the "Authenticate with a Backend Server" section.</span></span>
3. <span data-ttu-id="d8ed7-298">Lägg till följande till ditt ombud `signIn:didSignInForUser:withError:` metoden enligt det språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-298">Add the following to your delegate's `signIn:didSignInForUser:withError:` method, according to the language you are using.</span></span>

<span data-ttu-id="d8ed7-299">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-299">**Objective-C**:</span></span>

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

<span data-ttu-id="d8ed7-300">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-300">**Swift**:</span></span>

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. <span data-ttu-id="d8ed7-301">Kontrollera att du också lägga till följande `application:didFinishLaunchingWithOptions:` i din app ombud kan ersätta ”SERVER_CLIENT_ID” med samma ID som du använde för att konfigurera App Service i steg 1.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-301">Make sure you also add the following to `application:didFinishLaunchingWithOptions:` in your app delegate, replacing "SERVER_CLIENT_ID" with the same ID that you used to configure App Service in step 1.</span></span>

<span data-ttu-id="d8ed7-302">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-302">**Objective-C**:</span></span>

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 <span data-ttu-id="d8ed7-303">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-303">**Swift**:</span></span>

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. <span data-ttu-id="d8ed7-304">Lägg till följande kod i ditt program i en UIViewController som implementerar det `GIDSignInUIDelegate` protokoll, beroende på vilket språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-304">Add the following code to your application in a UIViewController that implements the `GIDSignInUIDelegate` protocol, according to the language you are using.</span></span>  <span data-ttu-id="d8ed7-305">Du loggas innan som signeras igen och även om du inte behöver ange dina autentiseringsuppgifter igen, visas en dialogruta för medgivande.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-305">You are signed out before being signed in again, and although you don't need to enter your credentials again, you see a consent dialog.</span></span>  <span data-ttu-id="d8ed7-306">Endast anropa denna metod när sessionstoken har gått ut.</span><span class="sxs-lookup"><span data-stu-id="d8ed7-306">Only call this method when the session token has expired.</span></span>

   <span data-ttu-id="d8ed7-307">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-307">**Objective-C**:</span></span>

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   <span data-ttu-id="d8ed7-308">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="d8ed7-308">**Swift**:</span></span>

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
<span data-ttu-id="d8ed7-309">[Azure Mobile Apps Snabbstart]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="d8ed7-309">[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md</span></span>

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
<span data-ttu-id="d8ed7-310">[dynamiska schemat]: http://go.microsoft.com/fwlink/p/?LinkId=296271</span><span class="sxs-lookup"><span data-stu-id="d8ed7-310">[Dynamic Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271</span></span>
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

<span data-ttu-id="d8ed7-311">[instrumentpanelen]: https://www.fabric.io/home</span><span class="sxs-lookup"><span data-stu-id="d8ed7-311">[Fabric Dashboard]: https://www.fabric.io/home</span></span>
<span data-ttu-id="d8ed7-312">[infrastruktur för iOS - komma igång]: https://docs.fabric.io/ios/fabric/getting-started.html</span><span class="sxs-lookup"><span data-stu-id="d8ed7-312">[Fabric for iOS - Getting Started]: https://docs.fabric.io/ios/fabric/getting-started.html</span></span>
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
