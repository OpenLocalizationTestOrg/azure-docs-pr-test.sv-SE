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
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="a8b3d-103">Hur toouse hello hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="a8b3d-103">How toouse hello managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="a8b3d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a8b3d-104">Overview</span></span>
<span data-ttu-id="a8b3d-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello hanteras klientbiblioteket för Azure App Service mobila appar för Windows och Xamarin-appar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-105">This guide shows you how tooperform common scenarios using hello managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="a8b3d-106">Om du är den nya tooMobile appar, bör du först slutför hello [Azure Mobile Apps quickstart] [ 1] kursen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-106">If you are new tooMobile Apps, you should consider first completing hello [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="a8b3d-107">I den här guiden fokuserar vi på hello klientsidan hanteras SDK.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-107">In this guide, we focus on hello client-side managed SDK.</span></span> <span data-ttu-id="a8b3d-108">toolearn mer om Hej serversidan SDK: er för Mobile Apps, hello i dokumentationen för hello [SDK för .NET Server] [ 2] eller [SDK för Node.js Server] [ 3].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-108">toolearn more about hello server-side SDKs for Mobile Apps, see hello documentation for hello [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="a8b3d-109">Referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="a8b3d-109">Reference documentation</span></span>
<span data-ttu-id="a8b3d-110">hello referensdokumentationen för hello klient-SDK finns här: [Azure Mobile Apps .NET klienten referens][4].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-110">hello reference documentation for hello client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="a8b3d-111">Du kan också hitta flera klienten prover i hello [Azure-exempel GitHub-lagringsplatsen][5].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-111">You can also find several client samples in hello [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a8b3d-112">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="a8b3d-112">Supported Platforms</span></span>
<span data-ttu-id="a8b3d-113">hello .NET-plattformen stöder hello följande plattformar:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-113">hello .NET Platform supports hello following platforms:</span></span>

* <span data-ttu-id="a8b3d-114">Xamarin Android-versioner för API-19 till 24 (KitKat via Nougat)</span><span class="sxs-lookup"><span data-stu-id="a8b3d-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="a8b3d-115">Xamarin iOS-versioner för iOS version 8.0 och senare</span><span class="sxs-lookup"><span data-stu-id="a8b3d-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="a8b3d-116">Universella Windows-plattformen</span><span class="sxs-lookup"><span data-stu-id="a8b3d-116">Universal Windows Platform</span></span>
* <span data-ttu-id="a8b3d-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="a8b3d-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="a8b3d-118">Windows Phone 8.0 förutom Silverlight-program</span><span class="sxs-lookup"><span data-stu-id="a8b3d-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="a8b3d-119">Hej ”server flöde” autentisering använder en webbvy för hello som visas i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-119">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="a8b3d-120">Om hello enheten inte är kan toopresent en WebView UI behövs andra metoder för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-120">If hello device is not able toopresent a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="a8b3d-121">Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="a8b3d-122"><a name="setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="a8b3d-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="a8b3d-123">Vi förutsätter att du redan har skapat och publicerat serverdelsprojektet Mobilapp som innehåller minst en tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="a8b3d-124">I hello-koden i det här avsnittet, hello tabellen med namnet `TodoItem` och har hello följande kolumner: `Id`, `Text`, och `Complete`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-124">In hello code used in this topic, hello table is named `TodoItem` and it has hello following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="a8b3d-125">Den här tabellen är hello samma tabell skapas när du har slutfört den [Azure Mobile Apps quickstart][1].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-125">This table is hello same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="a8b3d-126">hello är motsvarande skrivna på klientsidan i C# hello följande klass:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-126">hello corresponding typed client-side type in C# is hello following class:</span></span>

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

<span data-ttu-id="a8b3d-127">Hej [JsonPropertyAttribute] [ 6] är används toodefine hello *PropertyName* mappning mellan hello klienten och fältet hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-127">hello [JsonPropertyAttribute][6] is used toodefine hello *PropertyName* mapping between hello client field and hello table field.</span></span>

<span data-ttu-id="a8b3d-128">toolearn hur toocreate tabeller i Mobile Apps-serverdel finns hello [SDK för .NET Server avsnittet] [ 7] eller hello [SDK för Node.js Server avsnittet] [ 8] .</span><span class="sxs-lookup"><span data-stu-id="a8b3d-128">toolearn how toocreate tables in your Mobile Apps backend, see hello [.NET Server SDK topic][7] or hello [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="a8b3d-129">Om du har skapat din mobilappsserverdel i hello Azure portal med Hej Snabbstart, du kan också använda hello **enkelt tabeller** i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-129">If you created your Mobile App backend in hello Azure portal using hello QuickStart, you can also use hello **Easy tables** setting in hello [Azure portal].</span></span>

### <a name="how-to-install-hello-managed-client-sdk-package"></a><span data-ttu-id="a8b3d-130">Så här: Installera hello hanterade klient-SDK-paketet</span><span class="sxs-lookup"><span data-stu-id="a8b3d-130">How to: Install hello managed client SDK package</span></span>
<span data-ttu-id="a8b3d-131">Använd en av följande metoder tooinstall hello hello hanterad klient-SDK-paketet för Mobile Apps från [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-131">Use one of hello following methods tooinstall hello managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="a8b3d-132">**Visual Studio** Högerklicka på ditt projekt, klickar du på **hantera NuGet-paket**, Sök efter hello `Microsoft.Azure.Mobile.Client` paketet och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="a8b3d-133">**Xamarin Studio** Högerklicka på ditt projekt, klickar du på **Lägg till** > **Lägg till NuGet-paket**, Sök efter hello `Microsoft.Azure.Mobile.Client `paketet och klickar sedan på **Lägg till Paketet**.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="a8b3d-134">Kom ihåg tooadd hello följande i filen huvudaktiviteten **med** instruktionen:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-134">In your main activity file, remember tooadd hello following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="a8b3d-135"><a name="symbolsource"></a>Så här: arbeta med symboler för felsökning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8b3d-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="a8b3d-136">hello symboler för hello Microsoft.Azure.Mobile namnområdet är tillgängliga på [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-136">hello symbols for hello Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="a8b3d-137">Se toothe [SymbolSource instruktioner] [ 11] toointegrate SymbolSource med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-137">Refer toothe [SymbolSource instructions][11] toointegrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="a8b3d-138"><a name="create-client"></a>Skapa hello Mobile Apps-klient</span><span class="sxs-lookup"><span data-stu-id="a8b3d-138"><a name="create-client"></a>Create hello Mobile Apps client</span></span>
<span data-ttu-id="a8b3d-139">hello följande kod skapar hello [MobileServiceClient] [ 12] objekt som används tooaccess din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-139">hello following code creates hello [MobileServiceClient][12] object that is used tooaccess your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="a8b3d-140">Ersätt i hello föregående kod, `MOBILE_APP_URL` med hello URL hello mobilappsserverdel, som finns i bladet för din mobilappsserverdel i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-140">In hello preceding code, replace `MOBILE_APP_URL` with hello URL of hello Mobile App backend, which is found in the blade for your Mobile App backend in hello [Azure portal].</span></span> <span data-ttu-id="a8b3d-141">Hej MobileServiceClient objektet ska vara en singleton.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-141">hello MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="a8b3d-142">Arbeta med tabeller</span><span class="sxs-lookup"><span data-stu-id="a8b3d-142">Work with Tables</span></span>
<span data-ttu-id="a8b3d-143">Hej följande avsnittet beskriver hur toosearch och hämta poster och ändra hello data inom hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-143">hello following section details how toosearch and retrieve records and modify hello data within hello table.</span></span>  <span data-ttu-id="a8b3d-144">hello följande avsnitt beskrivs:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-144">hello following topics are covered:</span></span>

* [<span data-ttu-id="a8b3d-145">Skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="a8b3d-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="a8b3d-146">Fråga efter data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-146">Query data</span></span>](#querying)
* [<span data-ttu-id="a8b3d-147">Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="a8b3d-148">Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="a8b3d-149">Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="a8b3d-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="a8b3d-150">Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="a8b3d-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="a8b3d-151">Leta upp en post-ID: t</span><span class="sxs-lookup"><span data-stu-id="a8b3d-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="a8b3d-152">Hantera ej typbestämd frågor</span><span class="sxs-lookup"><span data-stu-id="a8b3d-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="a8b3d-153">Infoga data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="a8b3d-154">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="a8b3d-155">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="a8b3d-156">Konfliktlösning och Optimistisk samtidighet</span><span class="sxs-lookup"><span data-stu-id="a8b3d-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="a8b3d-157">Bindningen tooa Windows-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="a8b3d-157">Binding tooa Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="a8b3d-158">Ändra hello sidstorlek</span><span class="sxs-lookup"><span data-stu-id="a8b3d-158">Changing hello Page Size</span></span>](#pagesize)

### <span data-ttu-id="a8b3d-159"><a name="instantiating"></a>Så här: skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="a8b3d-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="a8b3d-160">Alla hello-kod som har åtkomst till eller ändrar data i en backend-tabell anropar funktioner på hello `MobileServiceTable` objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-160">All hello code that accesses or modifies data in a backend table calls functions on hello `MobileServiceTable` object.</span></span> <span data-ttu-id="a8b3d-161">Skaffa en referenstabell toohello genom att anropa hello [GetTable] metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-161">Obtain a reference toohello table by calling hello [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="a8b3d-162">hello returnerade objektet använder hello skrivna serialisering modellen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-162">hello returned object uses hello typed serialization model.</span></span> <span data-ttu-id="a8b3d-163">En modell för ej typbestämd serialisering stöds också.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="a8b3d-164">I följande exempel [skapar en tooan ej typbestämd referenstabell]:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-164">The following example [creates a reference tooan untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="a8b3d-165">Du måste ange hello underliggande OData-frågesträng i ej typbestämd frågor.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-165">In untyped queries, you must specify hello underlying OData query string.</span></span>

### <span data-ttu-id="a8b3d-166"><a name="querying"></a>Så här: fråga efter data från din Mobilapp</span><span class="sxs-lookup"><span data-stu-id="a8b3d-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="a8b3d-167">Det här avsnittet beskrivs hur tooissue frågar toohello mobilappsserverdel, vilket innefattar hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-167">This section describes how tooissue queries toohello Mobile App backend, which includes hello following functionality:</span></span>

* [<span data-ttu-id="a8b3d-168">Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="a8b3d-169">Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="a8b3d-170">Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="a8b3d-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="a8b3d-171">Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="a8b3d-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="a8b3d-172">Leta upp data-ID: t</span><span class="sxs-lookup"><span data-stu-id="a8b3d-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="a8b3d-173">En server-driven storlek är tvingande tooprevent alla rader från att returneras.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-173">A server-driven page size is enforced tooprevent all rows from being returned.</span></span>  <span data-ttu-id="a8b3d-174">Sidindelning håller standard begäranden för stora datamängder från negativt påverka hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-174">Paging keeps default requests for large data sets from negatively impacting hello service.</span></span>  <span data-ttu-id="a8b3d-175">tooreturn mer än 50 rader använder hello `Skip` och `Take` metod, enligt beskrivningen i [returnerar data på sidor](#paging).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-175">tooreturn more than 50 rows, use hello `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="a8b3d-176"><a name="filtering"></a>Så här: filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="a8b3d-177">hello följande kod visar hur toofilter data genom att inkludera en `Where` -satsen i en fråga.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-177">hello following code illustrates how toofilter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="a8b3d-178">Returnerar alla objekt från `todoTable` vars `Complete` -egenskapen har värdet för`false`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-178">It returns all items from `todoTable` whose `Complete` property is equal too`false`.</span></span> <span data-ttu-id="a8b3d-179">Hej [där] funktion gäller en rad filtrering predikat hello frågan mot hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-179">hello [Where] function applies a row filtering predicate to hello query against hello table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="a8b3d-180">Du kan visa hello hello begäran som skickades toohello backend-URI med meddelandet inspektion programvara, till exempel webbläsare utvecklingsverktyg eller [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-180">You can view hello URI of hello request sent toohello backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="a8b3d-181">Om du tittar på hello URI-begäran Lägg märke till att hello frågesträngen ändras:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-181">If you look at hello request URI, notice that hello query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="a8b3d-182">Denna OData-begäran översätts till en SQL-fråga genom hello-serverns SDK:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-182">This OData request is translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="a8b3d-183">Hej funktion som har överförts toohello `Where` metod kan ha ett godtyckligt antal villkor.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-183">hello function that is passed toohello `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="a8b3d-184">Det här exemplet kan översättas till en SQL-fråga genom hello-serverns SDK:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-184">This example would be translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="a8b3d-185">Den här frågan kan också delas upp i flera villkor:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="a8b3d-186">hello två metoder är likvärdiga och utbytbara.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-186">hello two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="a8b3d-187">Hej förstnämnda alternativet&mdash;sammanfoga flera predikat i en fråga&mdash;blir mer kompakt och rekommenderade.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-187">hello former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="a8b3d-188">Hej `Where` satsen stöder åtgärder som kan översättas till hello OData delmängd.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-188">hello `Where` clause supports operations that be translated into hello OData subset.</span></span> <span data-ttu-id="a8b3d-189">Åtgärder är:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-189">Operations include:</span></span>

* <span data-ttu-id="a8b3d-190">Relationsoperatorer (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="a8b3d-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="a8b3d-191">Aritmetiska operatorer (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="a8b3d-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="a8b3d-192">Number precision (Math.Floor, Math.Ceiling)</span><span class="sxs-lookup"><span data-stu-id="a8b3d-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="a8b3d-193">Strängfunktioner (längd, delsträngen, Ersätt, IndexOf, StartsWith, EndsWith)</span><span class="sxs-lookup"><span data-stu-id="a8b3d-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="a8b3d-194">Egenskaper för datum (år, månad, dag, timme, minut, sekund)</span><span class="sxs-lookup"><span data-stu-id="a8b3d-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="a8b3d-195">Komma åt egenskaper för ett objekt, och</span><span class="sxs-lookup"><span data-stu-id="a8b3d-195">Access properties of an object, and</span></span>
* <span data-ttu-id="a8b3d-196">Uttryck kombinera någon av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="a8b3d-197">Om utifrån vad hello Server SDK kan användas, kan du hello [OData v3 dokumentationen].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-197">When considering what hello Server SDK supports, you can consider hello [OData v3 Documentation].</span></span>

### <span data-ttu-id="a8b3d-198"><a name="sorting"></a>Så här: sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="a8b3d-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="a8b3d-199">hello följande kod visar hur toosort data genom att inkludera en [OrderBy] eller [OrderByDescending] i hello fråga.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-199">hello following code illustrates how toosort data by including an [OrderBy] or [OrderByDescending] function in hello query.</span></span> <span data-ttu-id="a8b3d-200">Returnerar objekten från `todoTable` Sortera stigande efter hello `Text` fältet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-200">It returns items from `todoTable` sorted ascending by hello `Text` field.</span></span>

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

### <span data-ttu-id="a8b3d-201"><a name="paging"></a>Så här: returnerar data på sidor</span><span class="sxs-lookup"><span data-stu-id="a8b3d-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="a8b3d-202">Som standard returnerar hello backend endast hello första 50 rader.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-202">By default, hello backend returns only hello first 50 rows.</span></span> <span data-ttu-id="a8b3d-203">Du kan öka hello antal returnerade rader genom att anropa hello [ta] metod.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-203">You can increase hello number of returned rows by calling hello [Take] method.</span></span> <span data-ttu-id="a8b3d-204">Använd `Take` tillsammans med hello [hoppa över] metoden toorequest en viss ”sida” i hello totala dataset returneras av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-204">Use `Take` along with hello [Skip] method toorequest a specific "page" of hello total dataset returned by hello query.</span></span> <span data-ttu-id="a8b3d-205">hello returnerar följande fråga när den exekveras, hello de tre översta objekt i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-205">hello following query, when executed, returns hello top three items in hello table.</span></span>

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="a8b3d-206">hello följande reviderade frågan hoppar över hello först tre resultaten och returnerar hello följande tre resultat.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-206">hello following revised query skips hello first three results and returns hello next three results.</span></span> <span data-ttu-id="a8b3d-207">Den här frågan genererar hello andra ”sidan” data, där hello sidstorleken är tre objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-207">This query produces hello second "page" of data, where hello page size is three items.</span></span>

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="a8b3d-208">Hej [IncludeTotalCount] metoden begär hello Totalt antal för *alla* hello poster som har returnerats, ignoreras eventuella sidindelning/gräns-sats har angetts:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-208">hello [IncludeTotalCount] method requests hello total count for *all* hello records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="a8b3d-209">Du kan använda frågor liknande toohello föregående exempel med en personsökare kontroll eller jämförbar UI för att navigera mellan sidor i ett verkligt-app.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-209">In a real world app, you can use queries similar toohello preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="a8b3d-210">toooverride hello 50 rad-gränsen i en mobilappsserverdel, måste du också använda hello [EnableQueryAttribute] toohello offentlig GET-metod och ange hello sidindelning beteende.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-210">toooverride hello 50-row limit in a Mobile App backend, you must also apply hello [EnableQueryAttribute] toohello public GET method and specify hello paging behavior.</span></span> <span data-ttu-id="a8b3d-211">När tillämpade toohello metoden, hello följande anger max antal returnerade rader too1000:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-211">When applied toohello method, hello following sets the maximum returned rows too1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="a8b3d-212"><a name="selecting"></a>Så här: Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="a8b3d-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="a8b3d-213">Du kan ange vilken uppsättning egenskaper tooinclude i hello resultat genom att lägga till en [Välj] satsen tooyour frågan.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-213">You can specify which set of properties tooinclude in hello results by adding a [Select] clause tooyour query.</span></span> <span data-ttu-id="a8b3d-214">Till exempel hello följande kod visar hur tooselect ett enda fält och även hur tooselect och formatera flera fält:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-214">For example, hello following code shows how tooselect just one field and also how tooselect and format multiple fields:</span></span>

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

<span data-ttu-id="a8b3d-215">Alla hello funktioner beskrivs hittills är additiva, så vi kan hålla länkning dem.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-215">All hello functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="a8b3d-216">Varje länkad anrop påverkar flera hello frågan.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-216">Each chained call affects more of hello query.</span></span> <span data-ttu-id="a8b3d-217">Ytterligare ett exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="a8b3d-218"><a name="lookingup"></a>Så här: Leta upp data-ID: t</span><span class="sxs-lookup"><span data-stu-id="a8b3d-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="a8b3d-219">Hej [LookupAsync] funktionen kan vara används toolook in objekt från hello-databas med ett visst-ID.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-219">hello [LookupAsync] function can be used toolook up objects from hello database with a particular ID.</span></span>

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="a8b3d-220"><a name="untypedqueries"></a>Så här: köra någon frågor</span><span class="sxs-lookup"><span data-stu-id="a8b3d-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="a8b3d-221">När du kör en fråga med ett argument tabellobjekt, måste du uttryckligen ange hello OData-frågesträng genom att anropa [ReadAsync], som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-221">When executing a query using an untyped table object, you must explicitly specify hello OData query string by calling [ReadAsync], as in hello following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="a8b3d-222">Åter JSON-värden som du kan använda som en egenskapsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="a8b3d-223">Mer information om JToken och Newtonsoft Json.NET finns hello [Json.NET] plats.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-223">For more information on JToken and Newtonsoft Json.NET, see hello [Json.NET] site.</span></span>

### <span data-ttu-id="a8b3d-224"><a name="inserting"></a>Så här: Infoga data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="a8b3d-225">Alla klienttyper av måste innehålla en medlem med namnet **Id**, vilket är som standard en sträng.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="a8b3d-226">Detta **Id** krävs för att utföra CRUD-åtgärder och offline-synkronisering. hello följande kod visar hur toouse hello [InsertAsync] metoden tooinsert nya rader i en tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-226">This **Id** is required to perform CRUD operations and for offline sync. hello following code illustrates how toouse hello [InsertAsync] method tooinsert new rows into a table.</span></span> <span data-ttu-id="a8b3d-227">hello parametern innehåller hello data toobe läggas till som ett .NET-objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-227">hello parameter contains hello data toobe inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="a8b3d-228">Om ett unikt värde för anpassad ID inte ingår i hello `todoItem` under en insert-, ett GUID som genereras av hello-server.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-228">If a unique custom ID value is not included in hello `todoItem` during an insert, a GUID is generated by hello server.</span></span>
<span data-ttu-id="a8b3d-229">Du kan hämta hello genereras Id genom att inspektera hello objekt när hello anropet returnerar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-229">You can retrieve hello generated Id by inspecting hello object after hello call returns.</span></span>

<span data-ttu-id="a8b3d-230">tooinsert utan angiven data, kan du dra nytta av Json.NET:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-230">tooinsert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="a8b3d-231">Här är ett exempel som använder en e-postadress som en unik sträng-id:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-231">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="a8b3d-232">Arbeta med ID-värden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-232">Working with ID values</span></span>
<span data-ttu-id="a8b3d-233">Mobile Apps stöder unika anpassade strängvärden för hello tabellen **id** kolumn.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-233">Mobile Apps supports unique custom string values for hello table's **id** column.</span></span> <span data-ttu-id="a8b3d-234">Ett strängvärde som gör att program toouse anpassade värden, till exempel e-postadresser eller användarnamn för hello-ID.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-234">A string value allows applications toouse custom values such as email addresses or user names for hello ID.</span></span>  <span data-ttu-id="a8b3d-235">Sträng-ID: N ge hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-235">String IDs provide you with hello following benefits:</span></span>

* <span data-ttu-id="a8b3d-236">ID: N skapas utan att göra en onödig kommunikation toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-236">IDs are generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="a8b3d-237">Posterna är enklare toomerge från olika tabeller eller databaser.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-237">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="a8b3d-238">ID-värden kan integrera bättre med en programlogik.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-238">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="a8b3d-239">När ett strängvärde ID inte har angetts på en infogad post genererar hello mobilappsserverdel ett unikt värde för-ID.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-239">When a string ID value is not set on an inserted record, hello Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="a8b3d-240">Du kan använda hello [Guid.NewGuid] metoden toogenerate egna ID värden på hello-klienten eller i hello backend.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-240">You can use hello [Guid.NewGuid] method toogenerate your own ID values, either on hello client or in hello backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="a8b3d-241"><a name="modifying"></a>Så här: ändra data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-241"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="a8b3d-242">hello följande kod visar hur toouse hello [UpdateAsync] metoden tooupdate en befintlig post med hello med samma ID med ny information.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-242">hello following code illustrates how toouse hello [UpdateAsync] method tooupdate an existing record with hello same ID with new information.</span></span> <span data-ttu-id="a8b3d-243">hello parametern innehåller hello data toobe som ett .NET-objekt har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-243">hello parameter contains hello data toobe updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="a8b3d-244">tooupdate utan angiven data, kan du dra nytta av [Json.NET] på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-244">tooupdate untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="a8b3d-245">En `id` fält måste anges när du gör en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-245">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="a8b3d-246">hello backend använder hello `id` fältet tooidentify som rad tooupdate.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-246">hello backend uses hello `id` field tooidentify which row tooupdate.</span></span> <span data-ttu-id="a8b3d-247">Hej `id` fältet kan hämtas från hello resultatet av hello `InsertAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-247">hello `id` field can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="a8b3d-248">En `ArgumentException` uppstår om du försöker tooupdate ett objekt utan att ange hello `id` värde.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-248">An `ArgumentException` is raised if you try tooupdate an item without providing hello `id` value.</span></span>

### <span data-ttu-id="a8b3d-249"><a name="deleting"></a>Så här: ta bort data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-249"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="a8b3d-250">hello följande kod visar hur toouse hello [DeleteAsync] metoden toodelete en befintlig instans.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-250">hello following code illustrates how toouse hello [DeleteAsync] method toodelete an existing instance.</span></span> <span data-ttu-id="a8b3d-251">hello-instans kan identifieras genom hello `id` angivet på hello `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-251">hello instance is identified by hello `id` field set on hello `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="a8b3d-252">toodelete utan angiven data, kan du dra nytta av Json.NET på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-252">toodelete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="a8b3d-253">När du gör en begäran om borttagning måste ett ID anges.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-253">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="a8b3d-254">Andra egenskaper har inte skickats toohello service eller ignoreras vid hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-254">Other properties are not passed toohello service or are ignored at hello service.</span></span> <span data-ttu-id="a8b3d-255">Hej resultatet av en `DeleteAsync` anropet är vanligtvis `null`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-255">hello result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="a8b3d-256">hello ID toopass i kan hämtas från hello resultatet av hello `InsertAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-256">hello ID toopass in can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="a8b3d-257">En `MobileServiceInvalidOperationException` genereras när du försöker toodelete ett objekt utan att ange hello `id` fältet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-257">A `MobileServiceInvalidOperationException` is thrown when you try toodelete an item without specifying hello `id` field.</span></span>

### <span data-ttu-id="a8b3d-258"><a name="optimisticconcurrency"></a>Så här: använda Optimistisk samtidighet för konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="a8b3d-258"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="a8b3d-259">Två eller flera klienter kan skriva ändringar toohello samma objekt vid hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-259">Two or more clients may write changes toohello same item at hello same time.</span></span> <span data-ttu-id="a8b3d-260">Utan konfliktidentifiering, skulle hello senaste skriva över eventuella tidigare uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-260">Without conflict detection, hello last write would overwrite any previous updates.</span></span> <span data-ttu-id="a8b3d-261">**Optimistisk samtidighetskontroll** förutsätter att varje transaktion kan genomföra och därför använder inte någon resurslåsning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-261">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="a8b3d-262">Innan du genomför en transaktion verifierar optimistisk samtidighetskontroll att ingen annan transaktion har ändrats hello data.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-262">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified hello data.</span></span> <span data-ttu-id="a8b3d-263">Hello inleder transaktion återställs om hello data har ändrats.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-263">If hello data has been modified, hello committing transaction is rolled back.</span></span>

<span data-ttu-id="a8b3d-264">Mobile Apps stöder optimistisk samtidighetskontroll genom att spåra ändringar tooeach objekt som använder hello `version` system egenskapskolumn som har definierats för varje tabell i din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-264">Mobile Apps supports optimistic concurrency control by tracking changes tooeach item using hello `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="a8b3d-265">Varje gång en post uppdateras Mobile Apps anger hello `version` egenskapen för att registrera tooa nytt värde.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-265">Each time a record is updated, Mobile Apps sets hello `version` property for that record tooa new value.</span></span> <span data-ttu-id="a8b3d-266">Under varje uppdateringsbegäran hello `version` egenskapen hello postens medföljer hello-begäran är toohello jämfört med samma egenskap för hello-post på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-266">During each update request, hello `version` property of hello record included with hello request is compared toohello same property for hello record on hello server.</span></span> <span data-ttu-id="a8b3d-267">Om versionen som har skickats med hello förfrågan matchar inte hello backend sedan hello klientbiblioteket genererar en `MobileServicePreconditionFailedException<T>` undantag.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-267">If the version passed with hello request does not match hello backend, then hello client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="a8b3d-268">hello-typen som ingår i hello undantag är hello post från hello serverdel som innehåller hello servrar version av hello-post.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-268">hello type included with hello exception is hello record from hello backend containing hello servers version of hello record.</span></span> <span data-ttu-id="a8b3d-269">hello program kan sedan använda denna information toodecide om begäran om uppdatering av tooexecute hello igen med rätt hello `version` värde från hello backend toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-269">hello application can then use this information toodecide whether tooexecute hello update request again with hello correct `version` value from hello backend toocommit changes.</span></span>

<span data-ttu-id="a8b3d-270">Definiera en kolumn i hello tabellen klass för hello `version` system egenskapen tooenable Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-270">Define a column on hello table class for hello `version` system property tooenable optimistic concurrency.</span></span> <span data-ttu-id="a8b3d-271">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-271">For example:</span></span>

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

<span data-ttu-id="a8b3d-272">Program med hjälp av någon tabeller aktivera Optimistisk samtidighet genom att ange hello `Version` flaggan på den `SystemProperties` av hello tabell på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-272">Applications using untyped tables enable optimistic concurrency by setting hello `Version` flag on the `SystemProperties` of hello table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="a8b3d-273">I tillägg tooenabling Optimistisk samtidighet, måste du också fånga hello `MobileServicePreconditionFailedException<T>` undantag i koden när du anropar [UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-273">In addition tooenabling optimistic concurrency, you must also catch hello `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="a8b3d-274">Lösa hello konflikten genom att använda rätt hello `version` toothe uppdaterat post och anropet [UpdateAsync] med hello löst post.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-274">Resolve hello conflict by applying hello correct `version` toothe updated record and call [UpdateAsync] with hello resolved record.</span></span> <span data-ttu-id="a8b3d-275">hello följande kod visar hur tooresolve en Skriv-konflikt när upptäcktes:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-275">hello following code shows how tooresolve a write conflict once detected:</span></span>

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

<span data-ttu-id="a8b3d-276">Mer information finns i hello [Offline datasynkronisering i Azure Mobile Apps] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-276">For more information, see hello [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="a8b3d-277"><a name="binding"></a>Så här: Bind Mobile Apps data tooa Windows-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="a8b3d-277"><a name="binding"></a>How to: Bind Mobile Apps data tooa Windows user interface</span></span>
<span data-ttu-id="a8b3d-278">Det här avsnittet visar hur toodisplay returnerade dataobjekt med hjälp av UI-element i en Windows-app.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-278">This section shows how toodisplay returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="a8b3d-279">Följande exempelkod Binder toohello källan för hello lista med en fråga om ofullständiga objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-279">The following example code binds toohello source of hello list with a query for incomplete items.</span></span> <span data-ttu-id="a8b3d-280">Den [MobileServiceCollection] skapar en Mobile Apps-medvetna bindning-samling.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-280">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

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

<span data-ttu-id="a8b3d-281">Vissa kontroller i hello hanteras runtime stöd för ett gränssnitt kallas [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-281">Some controls in hello managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="a8b3d-282">Det här gränssnittet gör kontroller toorequest extra data när hello användare rullar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-282">This interface allows controls toorequest extra data when hello user scrolls.</span></span> <span data-ttu-id="a8b3d-283">Det finns inbyggt stöd för det här gränssnittet för universella Windows-appar via [MobileServiceIncrementalLoadingCollection], som automatiskt hanterar anrop från hello kontroller.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-283">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from hello controls.</span></span> <span data-ttu-id="a8b3d-284">Använd `MobileServiceIncrementalLoadingCollection` i Windows-appar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-284">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="a8b3d-285">toouse hello ny samling i Windows Phone 8 och ”Silverlight”, Använd hello `ToCollection` tilläggsmetoder på `IMobileServiceTableQuery<T>` och `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-285">toouse hello new collection on Windows Phone 8 and "Silverlight" apps, use hello `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="a8b3d-286">tooload data, anropa `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-286">tooload data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="a8b3d-287">När du använder hello-samling som skapats genom att anropa `ToCollectionAsync` eller `ToCollection`, du får en samling som kan vara bundna tooUI kontroller.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-287">When you use hello collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound tooUI controls.</span></span>  <span data-ttu-id="a8b3d-288">Den här samlingen är medveten om sidindelning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-288">This collection is paging-aware.</span></span>  <span data-ttu-id="a8b3d-289">Eftersom hello samling läser in data från nätverket, misslyckas inläsning ibland.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-289">Since hello collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="a8b3d-290">toohandle dessa fel, åsidosätta hello `OnException` metod på `MobileServiceIncrementalLoadingCollection` toohandle undantag som uppstår till följd av anrop för`LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-290">toohandle such failures, override hello `OnException` method on `MobileServiceIncrementalLoadingCollection` toohandle exceptions resulting from calls too`LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="a8b3d-291">Överväg om tabellen innehåller många fält men du bara vill toodisplay några av dem i kontrollen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-291">Consider if your table has many fields but you only want toodisplay some of them in your control.</span></span> <span data-ttu-id="a8b3d-292">Du kan använda hello vägledning i föregående avsnitt hello ”[markera specifika kolumner](#selecting)” tooselect specifika kolumner toodisplay i hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-292">You may use hello guidance in hello preceding section "[Select specific columns](#selecting)" tooselect specific columns toodisplay in hello UI.</span></span>

### <span data-ttu-id="a8b3d-293"><a name="pagesize"></a>Ändra hello sidstorlek</span><span class="sxs-lookup"><span data-stu-id="a8b3d-293"><a name="pagesize"></a>Change hello Page size</span></span>
<span data-ttu-id="a8b3d-294">Azure Mobile Apps returnerar maximalt 50 objekt per begäran som standard.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-294">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="a8b3d-295">Du kan ändra hello växlingsfilens storlek genom att öka hello maximala sidstorleken på både hello klient och server.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-295">You can change hello paging size by increasing hello maximum page size on both hello client and server.</span></span>  <span data-ttu-id="a8b3d-296">tooincrease Hej begärda sidstorlek, ange `PullOptions` när du använder `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-296">tooincrease hello requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="a8b3d-297">Under förutsättning att du har gjort hello `PageSize` lika tooor som är större än 100 inom hello-server, en begäran returnerar upp till 100 poster.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-297">Assuming you have made hello `PageSize` equal tooor greater than 100 within hello server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="a8b3d-298"><a name="#offlinesync"></a>Arbeta med Offline tabeller</span><span class="sxs-lookup"><span data-stu-id="a8b3d-298"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="a8b3d-299">Offline tabeller använder en lokal SQLite lagra toostore data för användning offline.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-299">Offline tables use a local SQLite store toostore data for use when offline.</span></span>  <span data-ttu-id="a8b3d-300">Alla tabellen operations görs mot hello lokala SQLite lagra i stället för hello fjärrservern store.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-300">All table operations are done against hello local SQLite store instead of hello remote server store.</span></span>  <span data-ttu-id="a8b3d-301">toocreate en offline-tabell förbereda projektet:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-301">toocreate an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="a8b3d-302">Högerklicka på hello lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-302">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="a8b3d-303">(Valfritt) toosupport Windows-enheter, installera en av följande SQLite runtime paket hello:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-303">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="a8b3d-304">**Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-304">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="a8b3d-305">**Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-305">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="a8b3d-306">**Uwp** installera [SQLite för hello universell Windows][5].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-306">**Universal Windows Platform** Install [SQLite for hello Universal Windows][5].</span></span>
3. <span data-ttu-id="a8b3d-307">(Valfritt).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-307">(Optional).</span></span> <span data-ttu-id="a8b3d-308">Windows-enheter klickar du på **referenser** > **Lägg till referens...** , expandera hello **Windows** mappen > **tillägg**, aktivera hello lämpliga **SQLite för Windows** SDK tillsammans med hello  **Visual C++ 2013 Runtime för Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-308">For Windows devices, click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**, then enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="a8b3d-309">Hej varierar SQLite SDK namn något med varje Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-309">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="a8b3d-310">Innan en tabellreferens kan skapas, måste du förbereda hello lokalt Arkiv:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-310">Before a table reference can be created, hello local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="a8b3d-311">Initiering av certifikatarkiv görs normalt omedelbart efter hello klient skapas.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-311">Store initialization is normally done immediately after hello client is created.</span></span>  <span data-ttu-id="a8b3d-312">Hej **OfflineDbPath** bör vara ett filnamn som är lämplig för användning på alla plattformar som du stöder.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-312">hello **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="a8b3d-313">Om hello sökväg är en fullständigt kvalificerad sökväg (d.v.s. den börjar med ett snedstreck), och sedan används.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-313">If hello path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="a8b3d-314">Om hello sökväg inte är fullständigt kvalificerad placeras hello filen i en plattformsspecifik plats.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-314">If hello path is not fully qualified, hello file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="a8b3d-315">För iOS och Android-enheter är hello standardsökvägen hello ”personlig”-mappen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-315">For iOS and Android devices, hello default path is hello "Personal Files" folder.</span></span>
* <span data-ttu-id="a8b3d-316">För Windows-enheter är hello standardsökvägen hello programspecifika ”AppData” mapp.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-316">For Windows devices, hello default path is hello application-specific "AppData" folder.</span></span>

<span data-ttu-id="a8b3d-317">En tabellreferens kan erhållas med hjälp av hello `GetSyncTable<>` metoden:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-317">A table reference can be obtained using hello `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="a8b3d-318">Du behöver inte tooauthenticate toouse en offline-tabell.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-318">You do not need tooauthenticate toouse an offline table.</span></span>  <span data-ttu-id="a8b3d-319">Du behöver bara tooauthenticate när du kommunicerar med hello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-319">You only need tooauthenticate when you are communicating with hello backend service.</span></span>

### <span data-ttu-id="a8b3d-320"><a name="syncoffline"></a>Synkroniserar en Offline-tabell</span><span class="sxs-lookup"><span data-stu-id="a8b3d-320"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="a8b3d-321">Offline tabeller är inte synkroniserade med hello backend som standard.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-321">Offline tables are not synchronized with hello backend by default.</span></span>  <span data-ttu-id="a8b3d-322">Synkronisering är uppdelat i två delar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-322">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="a8b3d-323">Du kan skicka ändringar separat hämtar nya objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-323">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="a8b3d-324">Här är en typisk synkroniseringsmetod:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-324">Here is a typical sync method:</span></span>

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

<span data-ttu-id="a8b3d-325">Om hello första argumentet för`PullAsync` är null, och sedan inte används för inkrementell synkronisering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-325">If hello first argument too`PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="a8b3d-326">Varje synkroniseringsåtgärd hämtar alla poster.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-326">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="a8b3d-327">hello SDK utför en implicit `PushAsync()` innan dra poster.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-327">hello SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="a8b3d-328">Konflikt hantering sker på en `PullAsync()` metod.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-328">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="a8b3d-329">Du kan åtgärda konflikter i hello samma sätt som online-tabeller.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-329">You can deal with conflicts in hello same way as online tables.</span></span>  <span data-ttu-id="a8b3d-330">hello konflikt skapas när `PullAsync()` anropas i stället för under hello insert-, update- eller delete.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-330">hello conflict is produced when `PullAsync()` is called instead of during hello insert, update, or delete.</span></span> <span data-ttu-id="a8b3d-331">Om flera sådana konflikter uppstår ingår de i en enda MobileServicePushFailedException.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-331">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="a8b3d-332">Hantera varje fel separat.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-332">Handle each failure separately.</span></span>

## <span data-ttu-id="a8b3d-333"><a name="#customapi"></a>Arbeta med en anpassad API</span><span class="sxs-lookup"><span data-stu-id="a8b3d-333"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="a8b3d-334">En anpassad API kan du toodefine anpassade slutpunkter som exponerar serverfunktioner som inte mappa tooan infoga, uppdatera, ta bort eller Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-334">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="a8b3d-335">Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-335">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="a8b3d-336">Du kan anropa en anpassad API genom att anropa en hello [InvokeApiAsync] metoder på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-336">You call a custom API by calling one of hello [InvokeApiAsync] methods on hello client.</span></span> <span data-ttu-id="a8b3d-337">Till exempel hello följande kodrad skickar en POST-begäran toohello **completeAll** API på hello serverdel:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-337">For example, hello following line of code sends a POST request toohello **completeAll** API on hello backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="a8b3d-338">Det här formuläret är skrivna metodanrop och kräver att hello **MarkAllResult** returnera typen är definierad.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-338">This form is a typed method call and requires that hello **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="a8b3d-339">Både skrivna och ej typbestämd metoder stöds.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-339">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="a8b3d-340">Hej InvokeApiAsync() metoden läggs/api/toohello API som du vill toocall om hello API som börjar med en '/'.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-340">hello InvokeApiAsync() method prepends '/api/' toohello API that you wish toocall unless hello API starts with a '/'.</span></span>
<span data-ttu-id="a8b3d-341">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-341">For example:</span></span>

* <span data-ttu-id="a8b3d-342">`InvokeApiAsync("completeAll",...)`anropar /api/completeAll på hello serverdel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-342">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on hello backend</span></span>
* <span data-ttu-id="a8b3d-343">`InvokeApiAsync("/.auth/me",...)`anropar /.auth/me på hello serverdel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-343">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on hello backend</span></span>

<span data-ttu-id="a8b3d-344">Du kan använda InvokeApiAsync toocall alla WebAPI, inklusive de WebAPIs som inte har definierats med Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-344">You can use InvokeApiAsync toocall any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="a8b3d-345">När du använder InvokeApiAsync() skickas hello lämpliga sidhuvud, inklusive autentiseringshuvuden med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-345">When you use InvokeApiAsync(), hello appropriate headers, including authentication headers, are sent with hello request.</span></span>

## <span data-ttu-id="a8b3d-346"><a name="authentication"></a>Autentisera användare</span><span class="sxs-lookup"><span data-stu-id="a8b3d-346"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="a8b3d-347">Mobile Apps stöder autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-347">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="a8b3d-348">Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-348">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="a8b3d-349">Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-349">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="a8b3d-350">Mer information finns i självstudiekursen hello [Lägg till autentisering tooyour app].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-350">For more information, see hello tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="a8b3d-351">Två autentisering flöden stöds: *klienten hanteras* och *server-hanterade* flöde.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-351">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="a8b3d-352">Hej server-hanterade flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-352">hello server-managed flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="a8b3d-353">hello klienten hanteras flödet tillåter djupare integrering med specifika funktioner som använder den provider-specifik enhetsspecifika SDK.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-353">hello client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="a8b3d-354">Du bör använda ett klient-hanterade flöde i dina appar i produktion.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-354">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="a8b3d-355">tooset konfigurerar autentisering måste du registrera din app med en eller flera identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-355">tooset up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="a8b3d-356">hello identitetsleverantör genererar ett klient-ID och en klienthemlighet för din app.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-356">hello identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="a8b3d-357">Dessa värden anges sedan i din serverdel tooenable autentisering/auktorisering i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-357">These values are then set in your backend tooenable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="a8b3d-358">Mer information hello detaljerade instruktioner i självstudiekursen [Lägg till autentisering tooyour app].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-358">For more information, follow hello detailed instructions in the tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="a8b3d-359">hello följande avsnitt beskrivs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-359">hello following topics are covered in this section:</span></span>

* [<span data-ttu-id="a8b3d-360">Klienten hanteras autentisering</span><span class="sxs-lookup"><span data-stu-id="a8b3d-360">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="a8b3d-361">Hanterad Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="a8b3d-361">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="a8b3d-362">Cachelagring hello autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="a8b3d-362">Caching hello authentication token</span></span>](#caching)

### <span data-ttu-id="a8b3d-363"><a name="clientflow"></a>Klienten hanteras autentisering</span><span class="sxs-lookup"><span data-stu-id="a8b3d-363"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="a8b3d-364">Din app kan kontakta oberoende hello identitetsleverantör och ange sedan hello returnerade token under inloggningen med din serverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-364">Your app can independently contact hello identity provider and then provide hello returned token during login with your backend.</span></span> <span data-ttu-id="a8b3d-365">Det här flödet kan tooprovide en enkel inloggning för användare eller tooretrieve ytterligare användardata från hello identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-365">This client flow enables you tooprovide a single sign-on experience for users or tooretrieve additional user data from hello identity provider.</span></span> <span data-ttu-id="a8b3d-366">Flöde för klientautentisering är prioriterade toousing flödet för en server som hello identitetsleverantör SDK tillhandahåller en mer ursprungligt UX känslan och gör för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-366">Client flow authentication is preferred toousing a server flow as hello identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="a8b3d-367">Exempel har angetts för hello efter klient-flöde autentisering mönster:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-367">Examples are provided for hello following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="a8b3d-368">Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="a8b3d-368">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="a8b3d-369">Facebook eller Google</span><span class="sxs-lookup"><span data-stu-id="a8b3d-369">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="a8b3d-370">Live SDK</span><span class="sxs-lookup"><span data-stu-id="a8b3d-370">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="a8b3d-371"><a name="adal"></a>Autentisera användare med hello Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="a8b3d-371"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="a8b3d-372">Du kan använda Active Directory Authentication Library (ADAL) hello tooinitiate användarautentisering hello-klient som använder Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-372">You can use hello Active Directory Authentication Library (ADAL) tooinitiate user authentication from hello client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="a8b3d-373">Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] kursen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-373">Configure your mobile app backend for AAD sign-on by following hello [How tooconfigure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="a8b3d-374">Se till att toocomplete hello valfritt steg för att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-374">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="a8b3d-375">Öppna projektet i Visual Studio eller Xamarin Studio och lägga till en referens toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-375">In Visual Studio or Xamarin Studio, open your project and add a reference toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="a8b3d-376">När du söker kan innehålla förhandsversioner.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-376">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="a8b3d-377">Lägg till hello följande kod tooyour program, enligt toohello plattform som du använder.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-377">Add hello following code tooyour application, according toohello platform you are using.</span></span> <span data-ttu-id="a8b3d-378">I varje, gör du följande ersättningar hello:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-378">In each, make hello following replacements:</span></span>

   * <span data-ttu-id="a8b3d-379">Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-379">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="a8b3d-380">Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com. Det här värdet kan kopieras från hello domän-fliken i Azure Active Directory i hello [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-380">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="a8b3d-381">Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-381">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="a8b3d-382">Du kan hämta hello klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-382">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="a8b3d-383">Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-383">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="a8b3d-384">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-384">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="a8b3d-385">Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-385">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="a8b3d-386">hello-kod som behövs för varje plattform som följer:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-386">hello code needed for each platform follows:</span></span>

     <span data-ttu-id="a8b3d-387">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="a8b3d-387">**Windows:**</span></span>

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

     <span data-ttu-id="a8b3d-388">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="a8b3d-388">**Xamarin.iOS**</span></span>

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

     <span data-ttu-id="a8b3d-389">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="a8b3d-389">**Xamarin.Android**</span></span>

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

#### <span data-ttu-id="a8b3d-390"><a name="client-facebook"></a>Enkel inloggning med hjälp av en token från Facebook eller Google</span><span class="sxs-lookup"><span data-stu-id="a8b3d-390"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="a8b3d-391">Du kan använda hello flödet som visas i den här fragment för Facebook eller Google.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-391">You can use hello client flow as shown in this snippet for Facebook or Google.</span></span>

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

#### <span data-ttu-id="a8b3d-392"><a name="client-livesdk"></a>Enkel inloggning med Account hello Live SDK</span><span class="sxs-lookup"><span data-stu-id="a8b3d-392"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with hello Live SDK</span></span>
<span data-ttu-id="a8b3d-393">tooauthenticate användare, måste du registrera din app på hello Microsoft-konto Developer Center.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-393">tooauthenticate users, you must register your app at hello Microsoft account Developer Center.</span></span> <span data-ttu-id="a8b3d-394">Konfigurera registreringsinformation på din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-394">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="a8b3d-395">toocreate Microsoft kontot har registrerats och ansluta den tooyour mobilappsserverdel måste fullständig hello stegen i [registrera din app toouse för inloggning till en Microsoft-konto].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-395">toocreate a Microsoft account registration and connect it tooyour Mobile App backend, complete hello steps in [Register your app toouse a Microsoft account login].</span></span> <span data-ttu-id="a8b3d-396">Om du har både Windows Store och Windows Phone 8/Silverlight-versionerna av din app kan du registrera hello Windows Store-versionen först.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-396">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register hello Windows Store version first.</span></span>

<span data-ttu-id="a8b3d-397">hello följande kod autentiserar med hjälp av Live SDK och hello returnerade token toosign i tooyour mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-397">hello following code authenticates using Live SDK and uses hello returned token toosign in tooyour Mobile App backend.</span></span>

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

<span data-ttu-id="a8b3d-398">Mer information finns i hello [Windows Live SDK] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-398">For more information, see hello [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="a8b3d-399"><a name="serverflow"></a>Hanterad Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="a8b3d-399"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="a8b3d-400">När du har registrerat din identitetsleverantör anropa hello [LoginAsync] metod på hello [MobileServiceClient] med hello [MobileServiceAuthenticationProvider] värdet för leverantören.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-400">Once you have registered your identity provider, call hello [LoginAsync] method on hello [MobileServiceClient] with hello [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="a8b3d-401">Till exempel initierar hello följande kod en server flödet inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-401">For example, hello following code initiates a server flow sign-in by using Facebook.</span></span>

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

<span data-ttu-id="a8b3d-402">Om du använder en identitetsleverantör än Facebook, ändra hello värdet på [MobileServiceAuthenticationProvider] toohello värde för leverantören.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-402">If you are using an identity provider other than Facebook, change hello value of [MobileServiceAuthenticationProvider] toohello value for your provider.</span></span>

<span data-ttu-id="a8b3d-403">I ett server-flöde hanterar Azure App Service hello flödet för OAuth-autentisering genom att visa hello-inloggningssida för hello markerade providern.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-403">In a server flow, Azure App Service manages hello OAuth authentication flow by displaying hello sign-in page of hello selected provider.</span></span>  <span data-ttu-id="a8b3d-404">Hello identitet providern returnerar Azure App Service genererar när en Apptjänst-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-404">Once hello identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="a8b3d-405">Hej [LoginAsync] metoden returnerar en [MobileServiceUser], som innehåller både hello [UserId] av hello autentiserade användare och hello [ MobileServiceAuthenticationToken], som JSON web token (JWT).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-405">hello [LoginAsync] method returns a [MobileServiceUser], which provides both hello [UserId] of hello authenticated user and hello [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="a8b3d-406">Den här token kan cachelagras och återanvändas tills den upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-406">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="a8b3d-407">Mer information finns i [cachelagring hello autentiseringstoken](#caching).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-407">For more information, see [Caching hello authentication token](#caching).</span></span>

### <span data-ttu-id="a8b3d-408"><a name="caching"></a>Cachelagring hello autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="a8b3d-408"><a name="caching"></a>Caching hello authentication token</span></span>
<span data-ttu-id="a8b3d-409">I vissa fall undvikas hello anropsmetod toohello inloggningen efter hello första autentiseringen genom att lagra hello autentiseringstoken från hello-providern.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-409">In some cases, hello call toohello login method can be avoided after hello first successful authentication by storing hello authentication token from hello provider.</span></span>  <span data-ttu-id="a8b3d-410">Windows Store och UWP-appar kan använda [PasswordVault] toocache aktuella autentiseringen token efter en lyckad inloggning, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-410">Windows Store and UWP apps can use [PasswordVault] toocache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="a8b3d-411">hello UserId värdet lagras som hello användarnamn hello autentiseringsuppgifter och hello token är hello lagras som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-411">hello UserId value is stored as hello UserName of hello credential and hello token is hello stored as hello Password.</span></span> <span data-ttu-id="a8b3d-412">I efterföljande nystartade företag, kan du kontrollera hello **PasswordVault** för cachelagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-412">On subsequent start-ups, you can check hello **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="a8b3d-413">hello följande exempel används cachelagrade autentiseringsuppgifter när de hittas och annars försöker tooauthenticate igen med hello backend:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-413">hello following example uses cached credentials when they are found, and otherwise attempts tooauthenticate again with hello backend:</span></span>

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

<span data-ttu-id="a8b3d-414">När du loggar ut en användare måste du också bort hello lagrade autentiseringsuppgifter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-414">When you sign out a user, you must also remove hello stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="a8b3d-415">Xamarin-appar använda hello [Xamarin.Auth] API: er toosecurely store autentiseringsuppgifter i en **konto** objekt.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-415">Xamarin    apps use hello [Xamarin.Auth] APIs toosecurely store credentials in an **Account** object.</span></span> <span data-ttu-id="a8b3d-416">Ett exempel på hur du använder dessa API: er finns i hello [AuthStore.cs] kodfilen i hello [ContosoMoments foto delning exempel](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-416">For an example of using these APIs, see hello [AuthStore.cs] code file in hello [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="a8b3d-417">När du använder klienten hanteras autentisering kan du också cachelagra hello åtkomst-token som erhölls från leverantören som Facebook eller Twitter.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-417">When you use client-managed authentication, you can also cache hello access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="a8b3d-418">Denna token kan vara angivna toorequest en ny autentiseringstoken från hello serverdel på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-418">This token can be supplied toorequest a new authentication token from hello backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="a8b3d-419"><a name="pushnotifications"></a>Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-419"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="a8b3d-420">hello beskrivs följande avsnitt Push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-420">hello following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="a8b3d-421">Registrera dig för Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-421">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="a8b3d-422">Skaffa en Windows Store paket-SID</span><span class="sxs-lookup"><span data-stu-id="a8b3d-422">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="a8b3d-423">Registrera med plattformsoberoende mallar</span><span class="sxs-lookup"><span data-stu-id="a8b3d-423">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="a8b3d-424"><a name="register-for-push"></a>Så här: registrera dig för Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-424"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="a8b3d-425">hello Mobile Apps-klienten kan tooregister för push-meddelanden med Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-425">hello Mobile Apps client enables you tooregister for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="a8b3d-426">När du registrerat kan du erhålla en referens som hämtas från hello plattformsspecifika Push Notification Service (PNS).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-426">When registering, you obtain a handle that you obtain from hello platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="a8b3d-427">Du anger sedan det här värdet tillsammans med alla taggar när du skapar hello registrering.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-427">You then provide this value along with any tags when you create hello registration.</span></span> <span data-ttu-id="a8b3d-428">hello registrerar följande kod din Windows-app för push-meddelanden med hello Windows Notification Service (WNS):</span><span class="sxs-lookup"><span data-stu-id="a8b3d-428">hello following code registers your Windows app for push notifications with hello Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="a8b3d-429">Om du sänder tooWNS, så du måste [hämtar Windows Store-paketet SID](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="a8b3d-429">If you are pushing tooWNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="a8b3d-430">Mer information om Windows-appar, inklusive hur tooregister för mallen registreringar Se [Lägg till push-meddelanden tooyour app].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-430">For more information on Windows apps, including how tooregister for template registrations, see [Add push notifications tooyour app].</span></span>

<span data-ttu-id="a8b3d-431">Begär taggar från klientens hello stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-431">Requesting tags from hello client is not supported.</span></span>  <span data-ttu-id="a8b3d-432">Taggen begäranden släpps tyst från registreringen.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-432">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="a8b3d-433">Om du vill tooregister enheten med taggar kan skapa en anpassad API som använder hello Notification Hubs API tooperform hello registrering för din räkning.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-433">If you wish tooregister your device with tags, create a Custom API that uses hello Notification Hubs API tooperform hello registration on your behalf.</span></span>  <span data-ttu-id="a8b3d-434">[Anropa hello anpassade API](#customapi) i stället för hello `RegisterNativeAsync()` metod.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-434">[Call hello Custom API](#customapi) instead of hello `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="a8b3d-435"><a name="package-sid"></a>Så här: hämta en Windows Store paket-SID</span><span class="sxs-lookup"><span data-stu-id="a8b3d-435"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="a8b3d-436">Paket-SID krävs för att aktivera push-meddelanden i Windows Store-appar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-436">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="a8b3d-437">tooreceive paket-SID, registrera programmet med hello Windows Store.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-437">tooreceive a package SID, register your application with hello Windows Store.</span></span>

<span data-ttu-id="a8b3d-438">tooobtain det här värdet:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-438">tooobtain this value:</span></span>

1. <span data-ttu-id="a8b3d-439">I Visual Studio Solution Explorer, högerklicka på hello Windows Store-app-projekt, klickar du på **Store** > **associera appen med hello Store...** .</span><span class="sxs-lookup"><span data-stu-id="a8b3d-439">In Visual Studio Solution Explorer, right-click hello Windows Store app project, click **Store** > **Associate App with hello Store...**.</span></span>
2. <span data-ttu-id="a8b3d-440">Hello i guiden klickar du på **nästa**, logga in med ditt Microsoft-konto, Skriv ett namn för din app i **reservera ett nytt namn för appen**, klicka på **reservera**.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-440">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="a8b3d-441">När hello appen registreras har skapats, Välj hello programnamn, klickar du på **nästa**, och klicka sedan på **associera**.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-441">After hello app registration is successfully created, select hello app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="a8b3d-442">Logga in toohello [Windows Dev Center] med ditt Microsoft-Account.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-442">Log in toohello [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="a8b3d-443">Under **Mina appar**, klicka på hello appregistrering som du skapade.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-443">Under **My apps**, click hello app registration you created.</span></span>
5. <span data-ttu-id="a8b3d-444">Klicka på **apphantering** > **App identitet**, och bläddrar sedan nedåt toofind din **paket-SID**.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-444">Click **App management** > **App identity**, and then scroll down toofind your **Package SID**.</span></span>

<span data-ttu-id="a8b3d-445">Många användningsområden för hello paket-SID behandla det som en URI, i vilket fall du behöver toouse *ms-app: / /* som hello schema.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-445">Many uses of hello package SID treat it as a URI, in which case you need toouse *ms-app://* as hello scheme.</span></span> <span data-ttu-id="a8b3d-446">Anteckna hello version av ditt paket-SID som bildas genom att det här värdet som ett prefix.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-446">Make note of hello version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="a8b3d-447">Xamarin-appar kräver vissa ytterligare kod toobe kan tooregister en app som körs på hello iOS eller Android-plattformar.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-447">Xamarin apps require some additional code toobe able tooregister an app running on hello iOS or Android platforms.</span></span> <span data-ttu-id="a8b3d-448">Mer information finns i avsnittet hello för din plattform:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-448">For more information, see hello topic for your platform:</span></span>

* [<span data-ttu-id="a8b3d-449">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="a8b3d-449">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="a8b3d-450">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="a8b3d-450">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="a8b3d-451"><a name="register-xplat"></a>Så här: registrera push-mallar toosend plattformsoberoende meddelanden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-451"><a name="register-xplat"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="a8b3d-452">tooregister mallar använder hello `RegisterAsync()` metod med hello mallar, på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-452">tooregister templates, use hello `RegisterAsync()` method with hello templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="a8b3d-453">Dina mallar ska vara `JObject` datatyper och kan innehålla flera mallar i hello följande JSON-format:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-453">Your templates should be `JObject` types and can contain multiple templates in hello following JSON format:</span></span>

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

<span data-ttu-id="a8b3d-454">Hej metoden **RegisterAsync()** sekundära paneler kan också användas:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-454">hello method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="a8b3d-455">Alla taggar tas bort direkt under registreringen för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-455">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="a8b3d-456">tooadd taggar tooinstallations eller mallar inom installationer finns i avsnittet [fungerar med serverdelen för hello .NET SDK för Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-456">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="a8b3d-457">toosend meddelanden genom att använda mallarna registrerade finns toohello [Notification Hubs API: er].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-457">toosend notifications utilizing these registered templates, refer toohello [Notification Hubs APIs].</span></span>

## <span data-ttu-id="a8b3d-458"><a name="misc"></a>Diverse avsnitt</span><span class="sxs-lookup"><span data-stu-id="a8b3d-458"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="a8b3d-459"><a name="errors"></a>Så här: hantera fel</span><span class="sxs-lookup"><span data-stu-id="a8b3d-459"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="a8b3d-460">När ett fel uppstår i hello backend hello klienten SDK genererar en `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-460">When an error occurs in hello backend, hello client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="a8b3d-461">Följande exempel visar hur toohandle ett undantag som returneras av hello backend:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-461">The following example shows how toohandle an exception that is returned by hello backend:</span></span>

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

<span data-ttu-id="a8b3d-462">Ett annat exempel för att behandla felvillkor kan hittas i hello [Mobile Apps filer exempel].</span><span class="sxs-lookup"><span data-stu-id="a8b3d-462">Another example of dealing with error conditions can be found in hello [Mobile Apps Files Sample].</span></span> <span data-ttu-id="a8b3d-463">Den [LoggingHandler] exempel ger en delegat loggning hanteraren toolog hello begäranden som gjorts toohello backend.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-463">The [LoggingHandler] example provides a logging delegate handler toolog hello requests being made toohello backend.</span></span>

### <span data-ttu-id="a8b3d-464"><a name="headers"></a>Så här: anpassa begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="a8b3d-464"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="a8b3d-465">toosupport specifik app scenario kan du behöva toocustomize kommunikation med hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-465">toosupport your specific app scenario, you might need toocustomize communication with hello Mobile App backend.</span></span> <span data-ttu-id="a8b3d-466">Du kan till exempel vill tooadd en anpassad rubrik tooevery utgående begäran eller även ändra svar statuskoder.</span><span class="sxs-lookup"><span data-stu-id="a8b3d-466">For example, you may want tooadd a custom header tooevery outgoing request or even change responses status codes.</span></span> <span data-ttu-id="a8b3d-467">Du kan använda en anpassad [DelegatingHandler], som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8b3d-467">You can use a custom [DelegatingHandler], as in hello following example:</span></span>

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
