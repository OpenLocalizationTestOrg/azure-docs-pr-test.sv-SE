---
title: "aaaHow tooUse iOS SDK för Azure Mobile Apps"
description: "Hur tooUse iOS SDK för Azure Mobile Apps"
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
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a><span data-ttu-id="2d4d9-103">Hur tooUse iOS-klientbiblioteket för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="2d4d9-103">How tooUse iOS Client Library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="2d4d9-104">Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [Azure Mobile Apps iOS SDK][1].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-104">This guide teaches you tooperform common scenarios using hello latest [Azure Mobile Apps iOS SDK][1].</span></span> <span data-ttu-id="2d4d9-105">Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel skapa en tabell och hämta en förskapad iOS Xcode-projekt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend, create a table, and download a pre-built iOS Xcode project.</span></span> <span data-ttu-id="2d4d9-106">I den här guiden fokuserar på hello klientsidan iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-106">In this guide, we focus on hello client-side iOS SDK.</span></span> <span data-ttu-id="2d4d9-107">toolearn mer om hello serversidan SDK för hello backend, se hello Server SDK HOWTOs.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-107">toolearn more about hello server-side SDK for hello backend, see hello Server SDK HOWTOs.</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="2d4d9-108">Referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="2d4d9-108">Reference documentation</span></span>
<span data-ttu-id="2d4d9-109">hello referensdokumentationen för klient-hello iOS SDK finns här: [Azure Mobile Apps iOS klienten referens][2].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-109">hello reference documentation for hello iOS client SDK is located here: [Azure Mobile Apps iOS Client Reference][2].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2d4d9-110">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="2d4d9-110">Supported Platforms</span></span>
<span data-ttu-id="2d4d9-111">hello iOS SDK stöder Objective-C-projekt, Swift 2.2 projekt och Swift 2.3 projekt för iOS version 8.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-111">hello iOS SDK supports Objective-C projects, Swift 2.2 projects, and Swift 2.3 projects for iOS versions 8.0 or later.</span></span>

<span data-ttu-id="2d4d9-112">Hej ”server flöde” autentisering använder en webbvy för hello som visas i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-112">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="2d4d9-113">Om hello enheten inte kan toopresent en WebView UI, och sedan en annan metod för autentisering krävs är som utanför hello omfattning hello produkten.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-113">If hello device is not able toopresent a WebView UI, then another method of authentication is required that is outside hello scope of hello product.</span></span>  
<span data-ttu-id="2d4d9-114">Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-114">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="2d4d9-115"><a name="Setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="2d4d9-115"><a name="Setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="2d4d9-116">Den här handboken förutsätts att du har skapat en serverdel med en tabell.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-116">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="2d4d9-117">Den här guiden förutsätter att hello-tabellen har samma schema som hello tabeller i dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-117">This guide assumes that hello table has the same schema as hello tables in those tutorials.</span></span> <span data-ttu-id="2d4d9-118">Den här guiden förutsätter också att i din kod måste du referera till `MicrosoftAzureMobile.framework` och importera `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-118">This guide also assumes that in your code, you reference `MicrosoftAzureMobile.framework` and import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span></span>

## <span data-ttu-id="2d4d9-119"><a name="create-client"></a>Så här: skapa klienten</span><span class="sxs-lookup"><span data-stu-id="2d4d9-119"><a name="create-client"></a>How to: Create Client</span></span>
<span data-ttu-id="2d4d9-120">tooaccess en Azure Mobile Apps-serverdel i projektet, skapa en `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-120">tooaccess an Azure Mobile Apps backend in your project, create an `MSClient`.</span></span> <span data-ttu-id="2d4d9-121">Ersätt `AppUrl` med hello app-URL.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-121">Replace `AppUrl` with hello app URL.</span></span> <span data-ttu-id="2d4d9-122">Du kan lämna `gatewayURLString` och `applicationKey` tom.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-122">You may leave `gatewayURLString` and `applicationKey` empty.</span></span> <span data-ttu-id="2d4d9-123">Om du ställer in en gateway för autentisering fylla `gatewayURLString` med hello gateway-URL.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-123">If you set up a gateway for authentication, populate `gatewayURLString` with hello gateway URL.</span></span>

<span data-ttu-id="2d4d9-124">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-124">**Objective-C**:</span></span>

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

<span data-ttu-id="2d4d9-125">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-125">**Swift**:</span></span>

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <span data-ttu-id="2d4d9-126"><a name="table-reference"></a>Så här: skapa tabellreferens</span><span class="sxs-lookup"><span data-stu-id="2d4d9-126"><a name="table-reference"></a>How to: Create Table Reference</span></span>
<span data-ttu-id="2d4d9-127">tooaccess eller uppdatera data, skapa en referenstabell toohello backend.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-127">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="2d4d9-128">Ersätt `TodoItem` med hello namnet på din tabell</span><span class="sxs-lookup"><span data-stu-id="2d4d9-128">Replace `TodoItem` with hello name of your table</span></span>

<span data-ttu-id="2d4d9-129">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-129">**Objective-C**:</span></span>

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

<span data-ttu-id="2d4d9-130">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-130">**Swift**:</span></span>

```
let table = client.tableWithName("TodoItem")
```


## <span data-ttu-id="2d4d9-131"><a name="querying"></a>Så här: fråga Data</span><span class="sxs-lookup"><span data-stu-id="2d4d9-131"><a name="querying"></a>How to: Query Data</span></span>
<span data-ttu-id="2d4d9-132">toocreate en databasfråga frågan hello `MSTable` objekt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-132">toocreate a database query, query hello `MSTable` object.</span></span> <span data-ttu-id="2d4d9-133">hello följande fråga hämtar alla hello-objekt i `TodoItem` och loggar hello text för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-133">hello following query gets all hello items in `TodoItem` and logs hello text of each item.</span></span>

<span data-ttu-id="2d4d9-134">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-134">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-135">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-135">**Swift**:</span></span>

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

## <span data-ttu-id="2d4d9-136"><a name="filtering"></a>Så här: filtret returnerade Data</span><span class="sxs-lookup"><span data-stu-id="2d4d9-136"><a name="filtering"></a>How to: Filter Returned Data</span></span>
<span data-ttu-id="2d4d9-137">toofilter resultat, det finns många tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-137">toofilter results, there are many available options.</span></span>

<span data-ttu-id="2d4d9-138">toofilter med hjälp av ett predikat, Använd en `NSPredicate` och `readWithPredicate`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-138">toofilter using a predicate, use an `NSPredicate` and `readWithPredicate`.</span></span> <span data-ttu-id="2d4d9-139">hello följande filtrerar returnerade data toofind endast ofullständiga Todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-139">hello following filters returned data toofind only incomplete Todo items.</span></span>

<span data-ttu-id="2d4d9-140">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-140">**Objective-C**:</span></span>

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
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

<span data-ttu-id="2d4d9-141">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-141">**Swift**:</span></span>

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
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

## <span data-ttu-id="2d4d9-142"><a name="query-object"></a>Så här: använda MSQuery</span><span class="sxs-lookup"><span data-stu-id="2d4d9-142"><a name="query-object"></a>How to: Use MSQuery</span></span>
<span data-ttu-id="2d4d9-143">tooperform en komplex fråga, inklusive sortering och sidindelning, skapa ett `MSQuery` objekt, direkt eller genom att använda ett predikat:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-143">tooperform a complex query (including sorting and paging), create an `MSQuery` object, directly or by using a predicate:</span></span>

<span data-ttu-id="2d4d9-144">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-144">**Objective-C**:</span></span>

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

<span data-ttu-id="2d4d9-145">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-145">**Swift**:</span></span>

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

<span data-ttu-id="2d4d9-146">`MSQuery`kan du styra flera olika beteenden för frågan.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-146">`MSQuery` lets you control several query behaviors.</span></span>

* <span data-ttu-id="2d4d9-147">Ange ordning av resultat</span><span class="sxs-lookup"><span data-stu-id="2d4d9-147">Specify order of results</span></span>
* <span data-ttu-id="2d4d9-148">Begränsa vilka fält tooreturn</span><span class="sxs-lookup"><span data-stu-id="2d4d9-148">Limit which fields tooreturn</span></span>
* <span data-ttu-id="2d4d9-149">Begränsa hur många poster tooreturn</span><span class="sxs-lookup"><span data-stu-id="2d4d9-149">Limit how many records tooreturn</span></span>
* <span data-ttu-id="2d4d9-150">Ange totalt antal i svaret</span><span class="sxs-lookup"><span data-stu-id="2d4d9-150">Specify total count in response</span></span>
* <span data-ttu-id="2d4d9-151">Ange anpassad fråga string-parametrar i begäran</span><span class="sxs-lookup"><span data-stu-id="2d4d9-151">Specify custom query string parameters in request</span></span>
* <span data-ttu-id="2d4d9-152">Tillämpa ytterligare funktioner</span><span class="sxs-lookup"><span data-stu-id="2d4d9-152">Apply additional functions</span></span>

<span data-ttu-id="2d4d9-153">Köra en `MSQuery` frågan genom att anropa `readWithCompletion` på hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-153">Execute an `MSQuery` query by calling `readWithCompletion` on hello object.</span></span>

## <span data-ttu-id="2d4d9-154"><a name="sorting"></a>Så här: sortera Data med MSQuery</span><span class="sxs-lookup"><span data-stu-id="2d4d9-154"><a name="sorting"></a>How to: Sort Data with MSQuery</span></span>
<span data-ttu-id="2d4d9-155">toosort resultat ska vi titta på ett exempel.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-155">toosort results, let's look at an example.</span></span> <span data-ttu-id="2d4d9-156">toosort efter fältet 'text' stigande sedan efter ”klar” fallande anropa `MSQuery` t.ex:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-156">toosort by field 'text' ascending, then by 'complete' descending, invoke `MSQuery` like so:</span></span>

<span data-ttu-id="2d4d9-157">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-157">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-158">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-158">**Swift**:</span></span>

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


## <span data-ttu-id="2d4d9-159"><a name="selecting"></a><a name="parameters"></a>Så här: begränsa fält och expandera frågeparametrar sträng med MSQuery</span><span class="sxs-lookup"><span data-stu-id="2d4d9-159"><a name="selecting"></a><a name="parameters"></a>How to: Limit Fields and Expand Query String Parameters with MSQuery</span></span>
<span data-ttu-id="2d4d9-160">toolimit fält toobe returneras i en fråga, ange hello namnen på hello fält i hello **selectFields** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-160">toolimit fields toobe returned in a query, specify hello names of hello fields in hello **selectFields** property.</span></span> <span data-ttu-id="2d4d9-161">Det här exemplet returnerar endast hello text och slutförda fält:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-161">This example returns only hello text and completed fields:</span></span>

<span data-ttu-id="2d4d9-162">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-162">**Objective-C**:</span></span>

```
query.selectFields = @[@"text", @"complete"];
```

<span data-ttu-id="2d4d9-163">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-163">**Swift**:</span></span>

```
query.selectFields = ["text", "complete"]
```

<span data-ttu-id="2d4d9-164">tooinclude ytterligare sträng frågeparametrar Hej server begär (till exempel eftersom ett anpassat skript för serversidan använder dem) kan fylla `query.parameters` t.ex:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-164">tooinclude additional query string parameters in hello server request (for example, because a custom server-side script uses them), populate `query.parameters` like so:</span></span>

<span data-ttu-id="2d4d9-165">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-165">**Objective-C**:</span></span>

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

<span data-ttu-id="2d4d9-166">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-166">**Swift**:</span></span>

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <span data-ttu-id="2d4d9-167"><a name="paging"></a>Så här: Konfigurera sidstorlek</span><span class="sxs-lookup"><span data-stu-id="2d4d9-167"><a name="paging"></a>How to: Configure Page Size</span></span>
<span data-ttu-id="2d4d9-168">Med Azure Mobile Apps hello hello storlek kontroller antalet poster som hämtas i taget från hello backend-tabeller.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-168">With Azure Mobile Apps, hello page size controls hello number of records that are pulled at a time from hello backend tables.</span></span> <span data-ttu-id="2d4d9-169">Ett anrop för`pull` data skulle sedan batch in data, baserat på den här sidstorleken tills det inte finns några fler poster toopull.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-169">A call too`pull` data would then batch up data, based on this page size, until there are no more records toopull.</span></span>

<span data-ttu-id="2d4d9-170">Det är möjligt tooconfigure en storlek med **MSPullSettings** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-170">It's possible tooconfigure a page size using **MSPullSettings** as shown below.</span></span> <span data-ttu-id="2d4d9-171">hello sidan standardstorleken är 50 och hello exemplet nedan ändrar too3.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-171">hello default page size is 50, and hello example below changes it too3.</span></span>

<span data-ttu-id="2d4d9-172">Du kan konfigurera en annan sidstorlek av prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-172">You could configure a different page size for performance reasons.</span></span> <span data-ttu-id="2d4d9-173">Om du har ett stort antal små dataposter minskar en hög sidstorlek hello antalet turer för servern.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-173">If you have a large number of small data records, a high page size reduces hello number of server round-trips.</span></span>

<span data-ttu-id="2d4d9-174">Den här inställningen styr endast hello sidstorleken på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-174">This setting controls only hello page size on hello client side.</span></span> <span data-ttu-id="2d4d9-175">Om hello klienten begär en större storlek än hello Mobile Apps-serverdel stöder, hello storlek är begränsad till hello maximala hello backend är konfigurerade toosupport.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-175">If hello client asks for a larger page size than hello Mobile Apps backend supports, hello page size is capped at hello maximum hello backend is configured toosupport.</span></span>

<span data-ttu-id="2d4d9-176">Den här inställningen är också hello *nummer* dataposter, inte hello *bytestorlek*.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-176">This setting is also hello *number* of data records, not hello *byte size*.</span></span>

<span data-ttu-id="2d4d9-177">Om du ökar hello sidstorleken för klienten, bör du också öka hello sidstorleken på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-177">If you increase hello client page size, you should also increase hello page size on hello server.</span></span> <span data-ttu-id="2d4d9-178">Se [”så här: justera hello tabell växlingsfilens storlek”](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för hello steg toodo.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-178">See ["How to: Adjust hello table paging size"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for hello steps toodo this.</span></span>

<span data-ttu-id="2d4d9-179">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-179">**Objective-C**:</span></span>

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


<span data-ttu-id="2d4d9-180">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-180">**Swift**:</span></span>

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <span data-ttu-id="2d4d9-181"><a name="inserting"></a>Så här: Infoga Data</span><span class="sxs-lookup"><span data-stu-id="2d4d9-181"><a name="inserting"></a>How to: Insert Data</span></span>
<span data-ttu-id="2d4d9-182">tooinsert en ny tabellrad, skapa en `NSDictionary` och anropa `table insert`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-182">tooinsert a new table row, create a `NSDictionary` and invoke `table insert`.</span></span> <span data-ttu-id="2d4d9-183">Om [dynamiska schemat] är aktiverad, hello Azure App Service mobilserverdel genererar automatiskt nya kolumner som är baserat på hello `NSDictionary`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-183">If [Dynamic Schema] is enabled, hello Azure App Service mobile backend automatically generates new columns based on hello `NSDictionary`.</span></span>

<span data-ttu-id="2d4d9-184">Om `id` har inte angetts hello backend genererar automatiskt ett nytt unikt ID.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-184">If `id` is not provided, hello backend automatically generates a new unique ID.</span></span> <span data-ttu-id="2d4d9-185">Ange en egen `id` toouse e-postadresser, användarnamn eller dina egna anpassade värden som ID.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-185">Provide your own `id` toouse email addresses, usernames, or your own custom values as ID.</span></span> <span data-ttu-id="2d4d9-186">Tillhandahåller egna ID kan underlätta kopplingar och affärsorienterade databasen logik.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-186">Providing your own ID may ease joins and business-oriented database logic.</span></span>

<span data-ttu-id="2d4d9-187">Hej `result` innehåller hello nya objekt som har infogats.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-187">hello `result` contains hello new item that was inserted.</span></span> <span data-ttu-id="2d4d9-188">Det kan ha ytterligare beroende på din server-logik eller ändrade data jämfört med toowhat skickades toohello server.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-188">Depending on your server logic, it may have additional or modified data compared toowhat was passed toohello server.</span></span>

<span data-ttu-id="2d4d9-189">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-189">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-190">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-190">**Swift**:</span></span>

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

## <span data-ttu-id="2d4d9-191"><a name="modifying"></a>Så här: ändra Data</span><span class="sxs-lookup"><span data-stu-id="2d4d9-191"><a name="modifying"></a>How to: Modify Data</span></span>
<span data-ttu-id="2d4d9-192">tooupdate en befintlig rad ändra ett objekt och anropet `update`:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-192">tooupdate an existing row, modify an item and call `update`:</span></span>

<span data-ttu-id="2d4d9-193">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-193">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-194">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-194">**Swift**:</span></span>

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

<span data-ttu-id="2d4d9-195">Du kan också ange hello rad-ID och hello uppdateras fältet:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-195">Alternatively, supply hello row ID and hello updated field:</span></span>

<span data-ttu-id="2d4d9-196">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-196">**Objective-C**:</span></span>

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="2d4d9-197">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-197">**Swift**:</span></span>

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

<span data-ttu-id="2d4d9-198">Minimum hello `id` attributet måste anges när du gör uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-198">At minimum, hello `id` attribute must be set when making updates.</span></span>

## <span data-ttu-id="2d4d9-199"><a name="deleting"></a>Så här: ta bort Data</span><span class="sxs-lookup"><span data-stu-id="2d4d9-199"><a name="deleting"></a>How to: Delete Data</span></span>
<span data-ttu-id="2d4d9-200">toodelete ett objekt, anropa `delete` hello objektet:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-200">toodelete an item, invoke `delete` with hello item:</span></span>

<span data-ttu-id="2d4d9-201">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-201">**Objective-C**:</span></span>

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="2d4d9-202">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-202">**Swift**:</span></span>

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="2d4d9-203">Du kan också ta bort genom att tillhandahålla en rad-ID:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-203">Alternatively, delete by providing a row ID:</span></span>

<span data-ttu-id="2d4d9-204">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-204">**Objective-C**:</span></span>

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="2d4d9-205">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-205">**Swift**:</span></span>

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="2d4d9-206">Minimum hello `id` attributet måste anges när att tas bort.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-206">At minimum, hello `id` attribute must be set when making deletes.</span></span>

## <span data-ttu-id="2d4d9-207"><a name="customapi"></a>Så här: anropa anpassade API</span><span class="sxs-lookup"><span data-stu-id="2d4d9-207"><a name="customapi"></a>How to: Call Custom API</span></span>
<span data-ttu-id="2d4d9-208">Med en anpassad API kan du exponera backend funktioner.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-208">With a custom API, you can expose any backend functionality.</span></span> <span data-ttu-id="2d4d9-209">Den har inte toomap tooa tabellen igen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-209">It doesn't have toomap tooa table operation.</span></span> <span data-ttu-id="2d4d9-210">Inte bara gör du få bättre kontroll över meddelanden, du kan även läsa/set sidhuvuden och ändra hello svar body-formatet.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-210">Not only do you gain more control over messaging, you can even read/set headers and change hello response body format.</span></span> <span data-ttu-id="2d4d9-211">hur toocreate en anpassad API på hello serverdel läsa toolearn [anpassade API: er](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span><span class="sxs-lookup"><span data-stu-id="2d4d9-211">toolearn how toocreate a custom API on hello backend, read [Custom APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span></span>

<span data-ttu-id="2d4d9-212">toocall en anpassad API anropa `MSClient.invokeAPI`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-212">toocall a custom API, call `MSClient.invokeAPI`.</span></span> <span data-ttu-id="2d4d9-213">hello förfrågan och svar innehåll behandlas som JSON.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-213">hello request and response content are treated as JSON.</span></span> <span data-ttu-id="2d4d9-214">toouse andra typer av media, [Använd hello andra överlagring för `invokeAPI` ] [ 5].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-214">toouse other media types, [use hello other overload of `invokeAPI`][5].</span></span>  <span data-ttu-id="2d4d9-215">toomake en `GET` begäran i stället för en `POST` begära parameter för `HTTPMethod` för`"GET"` och parametern `body` för`nil` (eftersom GET-begäranden saknar meddelandetext.) Om din anpassade API: et stöder andra HTTP-verb, ändra `HTTPMethod` på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-215">toomake a `GET` request instead of a `POST` request, set parameter `HTTPMethod` too`"GET"` and parameter `body` too`nil` (since GET requests do not have message bodies.) If your custom API supports other HTTP verbs, change `HTTPMethod` appropriately.</span></span>

<span data-ttu-id="2d4d9-216">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-216">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-217">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-217">**Swift**:</span></span>

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

## <span data-ttu-id="2d4d9-218"><a name="templates"></a>Så här: registrera push-mallar toosend plattformsoberoende meddelanden</span><span class="sxs-lookup"><span data-stu-id="2d4d9-218"><a name="templates"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="2d4d9-219">tooregister mallar och skicka mallar med din **client.push registerDeviceToken** metod i klientappen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-219">tooregister templates, pass templates with your **client.push registerDeviceToken** method in your client app.</span></span>

<span data-ttu-id="2d4d9-220">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-220">**Objective-C**:</span></span>

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

<span data-ttu-id="2d4d9-221">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-221">**Swift**:</span></span>

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

<span data-ttu-id="2d4d9-222">Mallarna är av typen NSDictionary och kan innehålla flera mallar i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-222">Your templates are of type NSDictionary and can contain multiple templates in hello following format:</span></span>

<span data-ttu-id="2d4d9-223">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-223">**Objective-C**:</span></span>

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

<span data-ttu-id="2d4d9-224">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-224">**Swift**:</span></span>

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

<span data-ttu-id="2d4d9-225">Alla taggar tas bort från hello begäran för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-225">All tags are stripped from hello request for security.</span></span>  <span data-ttu-id="2d4d9-226">tooadd taggar tooinstallations eller mallar inom installationer, se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][4].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-226">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps][4].</span></span>  <span data-ttu-id="2d4d9-227">toosend meddelanden med mallarna registrerade arbeta med [Notification Hubs API: er][3].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-227">toosend notifications using these registered templates, work with [Notification Hubs APIs][3].</span></span>

## <span data-ttu-id="2d4d9-228"><a name="errors"></a>Så här: hantera fel</span><span class="sxs-lookup"><span data-stu-id="2d4d9-228"><a name="errors"></a>How to: Handle Errors</span></span>
<span data-ttu-id="2d4d9-229">När du anropar en mobila serverdel i Azure App Service hello slutförande block innehåller en `NSError` parameter.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-229">When you call an Azure App Service mobile backend, hello completion block contains an `NSError` parameter.</span></span> <span data-ttu-id="2d4d9-230">När ett fel uppstår i den här parametern är inte är nil.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-230">When an error occurs, this parameter is non-nil.</span></span> <span data-ttu-id="2d4d9-231">I koden, bör du kontrollera den här parametern och hantera hello fel vid behov, som visas i hello föregående kodfragment.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-231">In your code, you should check this parameter and handle hello error as needed, as demonstrated in hello preceding code snippets.</span></span>

<span data-ttu-id="2d4d9-232">hello filen [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definierar hello konstanter `MSErrorResponseKey`, `MSErrorRequestKey`, och `MSErrorServerItemKey`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-232">hello file [`<WindowsAzureMobileServices/MSError.h>`][6] defines hello constants `MSErrorResponseKey`, `MSErrorRequestKey`, and `MSErrorServerItemKey`.</span></span> <span data-ttu-id="2d4d9-233">tooget mer data relaterade toohello fel:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-233">tooget more data related toohello error:</span></span>

<span data-ttu-id="2d4d9-234">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-234">**Objective-C**:</span></span>

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

<span data-ttu-id="2d4d9-235">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-235">**Swift**:</span></span>

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

<span data-ttu-id="2d4d9-236">Dessutom definierar hello filen konstanter för respektive felkod:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-236">In addition, hello file defines constants for each error code:</span></span>

<span data-ttu-id="2d4d9-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-237">**Objective-C**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

<span data-ttu-id="2d4d9-238">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-238">**Swift**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

## <span data-ttu-id="2d4d9-239"><a name="adal"></a>Så här: autentiserar användare med hello Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="2d4d9-239"><a name="adal"></a>How to: Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="2d4d9-240">Du kan använda hello Active Directory Authentication Library (ADAL) toosign användare i ditt program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-240">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="2d4d9-241">Flödet för klientautentisering med hjälp av en identitetsleverantör SDK är bättre toousing hello `loginWithProvider:completion:` metod.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-241">Client flow authentication using an identity provider SDK is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="2d4d9-242">Flöde för klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-242">Client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="2d4d9-243">Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] [ 7] kursen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-243">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][7] tutorial.</span></span> <span data-ttu-id="2d4d9-244">Se till att toocomplete hello valfritt steg för att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-244">Make sure toocomplete hello optional step of registering a native client application.</span></span> <span data-ttu-id="2d4d9-245">För iOS, rekommenderar vi att hello omdirigering URI: N har hello formuläret `<app-scheme>://<bundle-id>`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-245">For iOS, we recommend that hello redirect URI is of hello form `<app-scheme>://<bundle-id>`.</span></span> <span data-ttu-id="2d4d9-246">Mer information finns i hello [ADAL iOS quickstart][8].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-246">For more information, see hello [ADAL iOS quickstart][8].</span></span>
2. <span data-ttu-id="2d4d9-247">Installera ADAL med Cocoapods.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-247">Install ADAL using Cocoapods.</span></span> <span data-ttu-id="2d4d9-248">Redigera din Podfile tooinclude hello definitionen, ersätter **din PROJECT** med hello namnet på ditt Xcode-projekt:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-248">Edit your Podfile tooinclude hello following definition, replacing **YOUR-PROJECT** with hello name of your Xcode project:</span></span>

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   <span data-ttu-id="2d4d9-249">och hello baljor:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-249">and hello Pod:</span></span>

        pod 'ADALiOS'
3. <span data-ttu-id="2d4d9-250">Med hjälp av hello terminalen kör `pod install` från hello katalog som innehåller ditt projekt och öppna sedan hello genereras Xcode-arbetsyta (inte hello-projekt).</span><span class="sxs-lookup"><span data-stu-id="2d4d9-250">Using hello Terminal, run `pod install` from hello directory containing your project, and then open hello generated Xcode workspace (not hello project).</span></span>
4. <span data-ttu-id="2d4d9-251">Lägg till hello följande kod tooyour program, enligt toohello språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-251">Add hello following code tooyour application, according toohello language you are using.</span></span> <span data-ttu-id="2d4d9-252">I varje, se dessa ersättningar:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-252">In each, make these replacements:</span></span>

   * <span data-ttu-id="2d4d9-253">Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-253">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="2d4d9-254">Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com. Det här värdet kan kopieras från hello domän-fliken i Azure Active Directory i hello [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-254">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="2d4d9-255">Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-255">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="2d4d9-256">Du kan hämta klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-256">You can obtain the client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="2d4d9-257">Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-257">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="2d4d9-258">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-258">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="2d4d9-259">Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-259">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

<span data-ttu-id="2d4d9-260">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-260">**Objective-C**:</span></span>

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


<span data-ttu-id="2d4d9-261">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-261">**Swift**:</span></span>

    // add hello following imports tooyour bridging header:
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

## <span data-ttu-id="2d4d9-262"><a name="facebook-sdk"></a>Så här: autentiserar användare med hello Facebook SDK för iOS</span><span class="sxs-lookup"><span data-stu-id="2d4d9-262"><a name="facebook-sdk"></a>How to: Authenticate users with hello Facebook SDK for iOS</span></span>
<span data-ttu-id="2d4d9-263">Du kan använda hello Facebook SDK för iOS toosign användare till programmet med hjälp av Facebook.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-263">You can use hello Facebook SDK for iOS toosign users into your application using Facebook.</span></span>  <span data-ttu-id="2d4d9-264">Med hjälp av en flödet för klientautentisering är bättre toousing hello `loginWithProvider:completion:` metod.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-264">Using a client flow authentication is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="2d4d9-265">hello flödet klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-265">hello client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="2d4d9-266">Konfigurera mobilappsserverdelen för Facebook-inloggning genom att följa den [hur tooconfigure App Service för Facebook-inloggningen] [ 9] kursen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-266">Configure your mobile app backend for Facebook sign-in by following the [How tooconfigure App Service for Facebook login][9] tutorial.</span></span>
2. <span data-ttu-id="2d4d9-267">Installera hello Facebook SDK för iOS med följande hello [Facebook SDK för iOS - komma igång] [ 10] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-267">Install hello Facebook SDK for iOS by following hello [Facebook SDK for iOS - Getting Started][10] documentation.</span></span> <span data-ttu-id="2d4d9-268">Du kan lägga till hello iOS-plattformen tooyour registrering av befintlig istället för att skapa en app.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-268">Instead of creating an app, you can add hello iOS platform tooyour existing registration.</span></span>
3. <span data-ttu-id="2d4d9-269">Facebooks dokumentationen innehåller vissa Objective-C-koden i hello App ombud.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-269">Facebook's documentation includes some Objective-C code in hello App Delegate.</span></span> <span data-ttu-id="2d4d9-270">Om du använder **Swift**, kan du använda följande översättningar för AppDelegate.swift hello:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-270">If you are using **Swift**, you can use hello following translations for AppDelegate.swift:</span></span>

        // Add hello following import tooyour bridging header:
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
4. <span data-ttu-id="2d4d9-271">I tillägg tooadding `FBSDKCoreKit.framework` tooyour projekt, även lägga till en referens för`FBSDKLoginKit.framework` i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-271">In addition tooadding `FBSDKCoreKit.framework` tooyour project, also add a reference too`FBSDKLoginKit.framework` in hello same way.</span></span>
5. <span data-ttu-id="2d4d9-272">Lägg till hello följande kod tooyour program, enligt toohello språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-272">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="2d4d9-273">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-273">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-274">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-274">**Swift**:</span></span>

    // Add hello following imports tooyour bridging header:
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

## <span data-ttu-id="2d4d9-275"><a name="twitter-fabric"></a>Så här: autentiserar användare med Twitter-infrastrukturen för iOS</span><span class="sxs-lookup"><span data-stu-id="2d4d9-275"><a name="twitter-fabric"></a>How to: Authenticate users with Twitter Fabric for iOS</span></span>
<span data-ttu-id="2d4d9-276">Du kan använda infrastrukturen för iOS toosign användare till programmet med Twitter.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-276">You can use Fabric for iOS toosign users into your application using Twitter.</span></span> <span data-ttu-id="2d4d9-277">Flöde för klientautentisering är bättre toousing hello `loginWithProvider:completion:` metoden eftersom den innehåller en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-277">Client Flow authentication is preferable toousing hello `loginWithProvider:completion:` method, as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="2d4d9-278">Konfigurera mobilappsserverdelen för Twitter-inloggning med följande hello [hur tooconfigure App Service för Twitter inloggningen](app-service-mobile-how-to-configure-twitter-authentication.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-278">Configure your mobile app backend for Twitter sign-in by following hello [How tooconfigure App Service for Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="2d4d9-279">Lägg till Fabric tooyour projektet genom följande hello [infrastruktur för iOS - komma igång] dokumentationen och konfigurera TwitterKit.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-279">Add Fabric tooyour project by following hello [Fabric for iOS - Getting Started] documentation and setting up TwitterKit.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2d4d9-280">Som standard skapas Fabric ett Twitter-program.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-280">By default, Fabric creates a Twitter application for you.</span></span> <span data-ttu-id="2d4d9-281">Du kan undvika att skapa ett program genom att registrera hello konsumentnyckel och konsumenthemlighet som du skapat tidigare med hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-281">You can avoid creating an application by registering hello Consumer Key and Consumer Secret you created earlier using hello following code snippets.</span></span>    <span data-ttu-id="2d4d9-282">Alternativt kan du ersätta hello konsumentnyckel och konsumenthemlighet värden du anger tooApp Service med hello värden du finns i hello [instrumentpanelen].</span><span class="sxs-lookup"><span data-stu-id="2d4d9-282">Alternatively, you can replace hello Consumer Key and Consumer Secret values that you provide tooApp Service with hello values you see in hello [Fabric Dashboard].</span></span> <span data-ttu-id="2d4d9-283">Om du väljer det här alternativet kan vara säker på att tooset hello återanrop URL tooa platshållarvärde, som `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-283">If you choose this option, be sure tooset hello callback URL tooa placeholder value, such as `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span></span>
   >
   >

    <span data-ttu-id="2d4d9-284">Om du väljer toouse hello hemligheter som du skapade tidigare, lägger du till hello följande kod tooyour App ombud:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-284">If you choose toouse hello secrets you created earlier, add hello following code tooyour App Delegate:</span></span>

    <span data-ttu-id="2d4d9-285">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-285">**Objective-C**:</span></span>

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

    <span data-ttu-id="2d4d9-286">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-286">**Swift**:</span></span>

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. <span data-ttu-id="2d4d9-287">Lägg till hello följande kod tooyour program, enligt toohello språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-287">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="2d4d9-288">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-288">**Objective-C**:</span></span>

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

<span data-ttu-id="2d4d9-289">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-289">**Swift**:</span></span>

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

## <span data-ttu-id="2d4d9-290"><a name="google-sdk"></a>Så här: autentiserar användare med hello Google inloggning SDK för iOS</span><span class="sxs-lookup"><span data-stu-id="2d4d9-290"><a name="google-sdk"></a>How to: Authenticate users with hello Google Sign-In SDK for iOS</span></span>
<span data-ttu-id="2d4d9-291">Du kan använda hello Google inloggning SDK för iOS toosign användare till programmet använder ett Google-konto.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-291">You can use hello Google Sign-In SDK for iOS toosign users into your application using a Google account.</span></span>  <span data-ttu-id="2d4d9-292">Google presenterade nyligen ändringar tootheir OAuth säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-292">Google recently announced changes tootheir OAuth security policies.</span></span>  <span data-ttu-id="2d4d9-293">Dessa principändringar kräver hello användning av Google SDK i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-293">These policy changes will require hello use of the Google SDK in hello future.</span></span>

1. <span data-ttu-id="2d4d9-294">Konfigurera mobilappsserverdelen för Google-inloggning med följande hello [hur tooconfigure App Service för Google inloggningen](app-service-mobile-how-to-configure-google-authentication.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-294">Configure your mobile app backend for Google sign-in by following hello [How tooconfigure App Service for Google login](app-service-mobile-how-to-configure-google-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="2d4d9-295">Installera hello Google SDK för iOS med följande hello [starta Google Sign-In för iOS - integreringen](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-295">Install hello Google SDK for iOS by following hello [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) documentation.</span></span> <span data-ttu-id="2d4d9-296">Du kan hoppa över hello ”autentisera med en Backend-servern” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-296">You may skip hello "Authenticate with a Backend Server" section.</span></span>
3. <span data-ttu-id="2d4d9-297">Lägg till hello följande tooyour ombud `signIn:didSignInForUser:withError:` metod, enligt toohello språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-297">Add hello following tooyour delegate's `signIn:didSignInForUser:withError:` method, according toohello language you are using.</span></span>

<span data-ttu-id="2d4d9-298">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-298">**Objective-C**:</span></span>

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

<span data-ttu-id="2d4d9-299">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-299">**Swift**:</span></span>

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. <span data-ttu-id="2d4d9-300">Kontrollera att du också lägga till hello följande för`application:didFinishLaunchingWithOptions:` i din app delegera, Ersätt ”SERVER_CLIENT_ID” med hello samma ID som du använde tooconfigure Apptjänst i steg 1.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-300">Make sure you also add hello following too`application:didFinishLaunchingWithOptions:` in your app delegate, replacing "SERVER_CLIENT_ID" with hello same ID that you used tooconfigure App Service in step 1.</span></span>

<span data-ttu-id="2d4d9-301">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-301">**Objective-C**:</span></span>

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 <span data-ttu-id="2d4d9-302">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-302">**Swift**:</span></span>

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. <span data-ttu-id="2d4d9-303">Lägg till följande kod tooyour program i en UIViewController som implementerar hello hello `GIDSignInUIDelegate` protokoll, enligt toohello språk som du använder.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-303">Add hello following code tooyour application in a UIViewController that implements hello `GIDSignInUIDelegate` protocol, according toohello language you are using.</span></span>  <span data-ttu-id="2d4d9-304">Du loggas innan som signeras igen och även om du inte behöver tooenter dina autentiseringsuppgifter igen kan du se en dialogruta för medgivande.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-304">You are signed out before being signed in again, and although you don't need tooenter your credentials again, you see a consent dialog.</span></span>  <span data-ttu-id="2d4d9-305">Endast anropa denna metod när hello sessionstoken har gått ut.</span><span class="sxs-lookup"><span data-stu-id="2d4d9-305">Only call this method when hello session token has expired.</span></span>

   <span data-ttu-id="2d4d9-306">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-306">**Objective-C**:</span></span>

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   <span data-ttu-id="2d4d9-307">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="2d4d9-307">**Swift**:</span></span>

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
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamisk Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Instrumentpanelen]: https://www.fabric.io/home
[Infrastruktur för iOS - komma igång]: https://docs.fabric.io/ios/fabric/getting-started.html
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
