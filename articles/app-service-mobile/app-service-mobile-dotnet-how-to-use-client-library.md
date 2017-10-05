---
title: "Arbeta med Apptjänst Mobilappar hanterade klientbiblioteket (Windows | Microsoft Docs"
description: "Lär dig hur du använder en .NET-klient för Azure Apptjänst Mobilappar med Windows och Xamarin-appar."
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
ms.openlocfilehash: 5f4cc3e97ba7adde2aaac471951a3130d79910f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="2f64c-103">Så här använder du den hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="2f64c-103">How to use the managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="2f64c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2f64c-104">Overview</span></span>
<span data-ttu-id="2f64c-105">Den här guiden visar hur du utför vanliga scenarier med hjälp av hanterade klientbiblioteket för Azure App Service mobila appar för Windows och Xamarin-appar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-105">This guide shows you how to perform common scenarios using the managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="2f64c-106">Om du är nybörjare på Mobile Apps bör du först slutföra den [Azure Mobile Apps quickstart] [ 1] kursen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-106">If you are new to Mobile Apps, you should consider first completing the [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="2f64c-107">I den här guiden fokuserar på klientsidan hanterade SDK.</span><span class="sxs-lookup"><span data-stu-id="2f64c-107">In this guide, we focus on the client-side managed SDK.</span></span> <span data-ttu-id="2f64c-108">Mer information om SDK: er för serversidan för Mobilappar, finns i dokumentationen för den [SDK för .NET Server] [ 2] eller [SDK för Node.js Server] [ 3].</span><span class="sxs-lookup"><span data-stu-id="2f64c-108">To learn more about the server-side SDKs for Mobile Apps, see the documentation for the [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="2f64c-109">Referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="2f64c-109">Reference documentation</span></span>
<span data-ttu-id="2f64c-110">I referensdokumentationen för klient-SDK finns här: [Azure Mobile Apps .NET klienten referens][4].</span><span class="sxs-lookup"><span data-stu-id="2f64c-110">The reference documentation for the client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="2f64c-111">Du kan också hitta flera klienten exempel i den [Azure-exempel GitHub-lagringsplatsen][5].</span><span class="sxs-lookup"><span data-stu-id="2f64c-111">You can also find several client samples in the [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2f64c-112">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="2f64c-112">Supported Platforms</span></span>
<span data-ttu-id="2f64c-113">.NET-plattformen har stöd för följande plattformar:</span><span class="sxs-lookup"><span data-stu-id="2f64c-113">The .NET Platform supports the following platforms:</span></span>

* <span data-ttu-id="2f64c-114">Xamarin Android-versioner för API-19 till 24 (KitKat via Nougat)</span><span class="sxs-lookup"><span data-stu-id="2f64c-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="2f64c-115">Xamarin iOS-versioner för iOS version 8.0 och senare</span><span class="sxs-lookup"><span data-stu-id="2f64c-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="2f64c-116">Universella Windows-plattformen</span><span class="sxs-lookup"><span data-stu-id="2f64c-116">Universal Windows Platform</span></span>
* <span data-ttu-id="2f64c-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="2f64c-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="2f64c-118">Windows Phone 8.0 förutom Silverlight-program</span><span class="sxs-lookup"><span data-stu-id="2f64c-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="2f64c-119">”Server flöde” autentisering använder en webbvy för presenterades Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-119">The "server-flow" authentication uses a WebView for the presented UI.</span></span>  <span data-ttu-id="2f64c-120">Om enheten inte är kunna presentera en WebView UI behövs andra metoder för autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-120">If the device is not able to present a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="2f64c-121">Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="2f64c-122"><a name="setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="2f64c-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="2f64c-123">Vi förutsätter att du redan har skapat och publicerat serverdelsprojektet Mobilapp som innehåller minst en tabell.</span><span class="sxs-lookup"><span data-stu-id="2f64c-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="2f64c-124">I den kod som används i det här avsnittet tabellen med namnet `TodoItem` och har följande kolumner: `Id`, `Text`, och `Complete`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-124">In the code used in this topic, the table is named `TodoItem` and it has the following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="2f64c-125">Den här tabellen är i samma tabell som skapas när du har slutfört den [Azure Mobile Apps quickstart][1].</span><span class="sxs-lookup"><span data-stu-id="2f64c-125">This table is the same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="2f64c-126">Motsvarande skrivna klientsidan typ i C# är följande:</span><span class="sxs-lookup"><span data-stu-id="2f64c-126">The corresponding typed client-side type in C# is the following class:</span></span>

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

<span data-ttu-id="2f64c-127">Den [JsonPropertyAttribute] [ 6] används för att definiera den *PropertyName* mappning mellan fälten klienten och tabellen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-127">The [JsonPropertyAttribute][6] is used to define the *PropertyName* mapping between the client field and the table field.</span></span>

<span data-ttu-id="2f64c-128">Information om hur du skapar tabeller i Mobile Apps-serverdel finns i [SDK för .NET Server avsnittet] [ 7] eller [SDK för Node.js Server avsnittet][8].</span><span class="sxs-lookup"><span data-stu-id="2f64c-128">To learn how to create tables in your Mobile Apps backend, see the [.NET Server SDK topic][7] or the [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="2f64c-129">Om du har skapat din mobilappsserverdel i Azure portal med Snabbstart, kan du också använda den **enkelt tabeller** i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="2f64c-129">If you created your Mobile App backend in the Azure portal using the QuickStart, you can also use the **Easy tables** setting in the [Azure portal].</span></span>

### <a name="how-to-install-the-managed-client-sdk-package"></a><span data-ttu-id="2f64c-130">Så här: Installera hanterad klient-SDK-paketet</span><span class="sxs-lookup"><span data-stu-id="2f64c-130">How to: Install the managed client SDK package</span></span>
<span data-ttu-id="2f64c-131">Använd någon av följande metoder för att installera hanterad klient-SDK-paketet för Mobile Apps från [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="2f64c-131">Use one of the following methods to install the managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="2f64c-132">**Visual Studio** Högerklicka på ditt projekt, klickar du på **hantera NuGet-paket**, söka efter den `Microsoft.Azure.Mobile.Client` paketet och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="2f64c-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="2f64c-133">**Xamarin Studio** Högerklicka på ditt projekt, klickar du på **Lägg till** > **Lägg till NuGet-paket**, söka efter den `Microsoft.Azure.Mobile.Client `paketet och klickar sedan på **Lägg till Paketet**.</span><span class="sxs-lookup"><span data-stu-id="2f64c-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="2f64c-134">Kom ihåg att lägga till följande i filen huvudaktiviteten **med** instruktionen:</span><span class="sxs-lookup"><span data-stu-id="2f64c-134">In your main activity file, remember to add the following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="2f64c-135"><a name="symbolsource"></a>Så här: arbeta med symboler för felsökning i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f64c-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="2f64c-136">Symboler för Microsoft.Azure.Mobile namnområdet är tillgängliga på [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="2f64c-136">The symbols for the Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="2f64c-137">Referera till den [SymbolSource instruktioner] [ 11] att integrera SymbolSource med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f64c-137">Refer to the [SymbolSource instructions][11] to integrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="2f64c-138"><a name="create-client"></a>Skapa Mobile Apps-klient</span><span class="sxs-lookup"><span data-stu-id="2f64c-138"><a name="create-client"></a>Create the Mobile Apps client</span></span>
<span data-ttu-id="2f64c-139">Följande kod skapar den [MobileServiceClient] [ 12] objekt som används för åtkomst till din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-139">The following code creates the [MobileServiceClient][12] object that is used to access your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="2f64c-140">I föregående kod ersätter `MOBILE_APP_URL` med URL: en för serverdelen för Mobilappen som hittas i bladet för din mobilappsserverdel i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="2f64c-140">In the preceding code, replace `MOBILE_APP_URL` with the URL of the Mobile App backend, which is found in the blade for your Mobile App backend in the [Azure portal].</span></span> <span data-ttu-id="2f64c-141">MobileServiceClient-objektet ska vara en singleton.</span><span class="sxs-lookup"><span data-stu-id="2f64c-141">The MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="2f64c-142">Arbeta med tabeller</span><span class="sxs-lookup"><span data-stu-id="2f64c-142">Work with Tables</span></span>
<span data-ttu-id="2f64c-143">Följande avsnitt beskriver hur du söka efter och hämta poster och ändra data i tabellen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-143">The following section details how to search and retrieve records and modify the data within the table.</span></span>  <span data-ttu-id="2f64c-144">I följande avsnitt beskrivs:</span><span class="sxs-lookup"><span data-stu-id="2f64c-144">The following topics are covered:</span></span>

* [<span data-ttu-id="2f64c-145">Skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="2f64c-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="2f64c-146">Fråga efter data</span><span class="sxs-lookup"><span data-stu-id="2f64c-146">Query data</span></span>](#querying)
* [<span data-ttu-id="2f64c-147">Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="2f64c-148">Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="2f64c-149">Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="2f64c-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="2f64c-150">Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="2f64c-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="2f64c-151">Leta upp en post-ID: t</span><span class="sxs-lookup"><span data-stu-id="2f64c-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="2f64c-152">Hantera ej typbestämd frågor</span><span class="sxs-lookup"><span data-stu-id="2f64c-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="2f64c-153">Infoga data</span><span class="sxs-lookup"><span data-stu-id="2f64c-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="2f64c-154">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="2f64c-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="2f64c-155">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="2f64c-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="2f64c-156">Konfliktlösning och Optimistisk samtidighet</span><span class="sxs-lookup"><span data-stu-id="2f64c-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="2f64c-157">Bindning till en Windows-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="2f64c-157">Binding to a Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="2f64c-158">Ändrar storlek på sidan</span><span class="sxs-lookup"><span data-stu-id="2f64c-158">Changing the Page Size</span></span>](#pagesize)

### <span data-ttu-id="2f64c-159"><a name="instantiating"></a>Så här: skapa en tabellreferens</span><span class="sxs-lookup"><span data-stu-id="2f64c-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="2f64c-160">Den kod som har åtkomst till eller ändrar data i en backend-tabell anropar funktioner på den `MobileServiceTable` objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-160">All the code that accesses or modifies data in a backend table calls functions on the `MobileServiceTable` object.</span></span> <span data-ttu-id="2f64c-161">Hämta en referens till tabellen genom att anropa den [GetTable] metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-161">Obtain a reference to the table by calling the [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="2f64c-162">Det returnerade objektet använder skrivna serialisering modellen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-162">The returned object uses the typed serialization model.</span></span> <span data-ttu-id="2f64c-163">En modell för ej typbestämd serialisering stöds också.</span><span class="sxs-lookup"><span data-stu-id="2f64c-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="2f64c-164">I följande exempel [skapar en referens till en ej typbestämd tabell]:</span><span class="sxs-lookup"><span data-stu-id="2f64c-164">The following example [creates a reference to an untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="2f64c-165">Du måste ange den underliggande OData-frågesträngen i ej typbestämd frågor.</span><span class="sxs-lookup"><span data-stu-id="2f64c-165">In untyped queries, you must specify the underlying OData query string.</span></span>

### <span data-ttu-id="2f64c-166"><a name="querying"></a>Så här: fråga efter data från din Mobilapp</span><span class="sxs-lookup"><span data-stu-id="2f64c-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="2f64c-167">Det här avsnittet beskrivs hur du utfärda frågor till serverdelen för Mobilappen som innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="2f64c-167">This section describes how to issue queries to the Mobile App backend, which includes the following functionality:</span></span>

* [<span data-ttu-id="2f64c-168">Filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="2f64c-169">Sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="2f64c-170">Returnera data på sidor</span><span class="sxs-lookup"><span data-stu-id="2f64c-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="2f64c-171">Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="2f64c-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="2f64c-172">Leta upp data-ID: t</span><span class="sxs-lookup"><span data-stu-id="2f64c-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="2f64c-173">En server-driven sidstorlek tillämpas för att förhindra att alla rader som returneras.</span><span class="sxs-lookup"><span data-stu-id="2f64c-173">A server-driven page size is enforced to prevent all rows from being returned.</span></span>  <span data-ttu-id="2f64c-174">Sidindelning håller standard begäranden för stora datamängder från negativt påverka tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-174">Paging keeps default requests for large data sets from negatively impacting the service.</span></span>  <span data-ttu-id="2f64c-175">Om du vill returnera fler än 50 rader, Använd den `Skip` och `Take` metod, enligt beskrivningen i [returnerar data på sidor](#paging).</span><span class="sxs-lookup"><span data-stu-id="2f64c-175">To return more than 50 rows, use the `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="2f64c-176"><a name="filtering"></a>Så här: filtret returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="2f64c-177">Följande kod visar hur du kan filtrera data genom att inkludera en `Where` -satsen i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2f64c-177">The following code illustrates how to filter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="2f64c-178">Returnerar alla objekt från `todoTable` vars `Complete` -egenskapen är lika med `false`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-178">It returns all items from `todoTable` whose `Complete` property is equal to `false`.</span></span> <span data-ttu-id="2f64c-179">Den [där] funktion gäller en rad filtrering predikat i frågan mot tabellen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-179">The [Where] function applies a row filtering predicate to the query against the table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="2f64c-180">Du kan visa URI för begäran som skickades till serverdelen med hjälp av meddelandet inspektion programvara, till exempel webbläsare utvecklingsverktyg eller [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="2f64c-180">You can view the URI of the request sent to the backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="2f64c-181">Om du tittar på URI-begäran att märka att frågesträngen ändras:</span><span class="sxs-lookup"><span data-stu-id="2f64c-181">If you look at the request URI, notice that the query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="2f64c-182">Denna OData-begäran översätts till en SQL-fråga av Server-SDK:</span><span class="sxs-lookup"><span data-stu-id="2f64c-182">This OData request is translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="2f64c-183">Funktionen som skickas till den `Where` metoden kan ha ett godtyckligt antal villkor.</span><span class="sxs-lookup"><span data-stu-id="2f64c-183">The function that is passed to the `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="2f64c-184">Det här exemplet kan översättas till en SQL-fråga av Server-SDK:</span><span class="sxs-lookup"><span data-stu-id="2f64c-184">This example would be translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="2f64c-185">Den här frågan kan också delas upp i flera villkor:</span><span class="sxs-lookup"><span data-stu-id="2f64c-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="2f64c-186">De två metoderna är likvärdiga och utbytbara.</span><span class="sxs-lookup"><span data-stu-id="2f64c-186">The two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="2f64c-187">Alternativet tidigare&mdash;sammanfoga flera predikat i en fråga&mdash;blir mer kompakt och rekommenderade.</span><span class="sxs-lookup"><span data-stu-id="2f64c-187">The former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="2f64c-188">Den `Where` -satsen stöder åtgärder som kan översättas till OData-delmängden.</span><span class="sxs-lookup"><span data-stu-id="2f64c-188">The `Where` clause supports operations that be translated into the OData subset.</span></span> <span data-ttu-id="2f64c-189">Åtgärder är:</span><span class="sxs-lookup"><span data-stu-id="2f64c-189">Operations include:</span></span>

* <span data-ttu-id="2f64c-190">Relationsoperatorer (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="2f64c-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="2f64c-191">Aritmetiska operatorer (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="2f64c-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="2f64c-192">Number precision (Math.Floor, Math.Ceiling)</span><span class="sxs-lookup"><span data-stu-id="2f64c-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="2f64c-193">Strängfunktioner (längd, delsträngen, Ersätt, IndexOf, StartsWith, EndsWith)</span><span class="sxs-lookup"><span data-stu-id="2f64c-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="2f64c-194">Egenskaper för datum (år, månad, dag, timme, minut, sekund)</span><span class="sxs-lookup"><span data-stu-id="2f64c-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="2f64c-195">Komma åt egenskaper för ett objekt, och</span><span class="sxs-lookup"><span data-stu-id="2f64c-195">Access properties of an object, and</span></span>
* <span data-ttu-id="2f64c-196">Uttryck kombinera någon av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2f64c-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="2f64c-197">När du överväger Server SDK stöder, kan du den [OData v3 dokumentationen].</span><span class="sxs-lookup"><span data-stu-id="2f64c-197">When considering what the Server SDK supports, you can consider the [OData v3 Documentation].</span></span>

### <span data-ttu-id="2f64c-198"><a name="sorting"></a>Så här: sortera returnerade data</span><span class="sxs-lookup"><span data-stu-id="2f64c-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="2f64c-199">Följande kod visar hur du kan sortera data genom att inkludera en [OrderBy] eller [OrderByDescending] funktion i frågan.</span><span class="sxs-lookup"><span data-stu-id="2f64c-199">The following code illustrates how to sort data by including an [OrderBy] or [OrderByDescending] function in the query.</span></span> <span data-ttu-id="2f64c-200">Returnerar objekten från `todoTable` Sortera stigande efter den `Text` fältet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-200">It returns items from `todoTable` sorted ascending by the `Text` field.</span></span>

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

### <span data-ttu-id="2f64c-201"><a name="paging"></a>Så här: returnerar data på sidor</span><span class="sxs-lookup"><span data-stu-id="2f64c-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="2f64c-202">Som standard returnerar endast de första 50 raderna i serverdelen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-202">By default, the backend returns only the first 50 rows.</span></span> <span data-ttu-id="2f64c-203">Du kan öka antalet rader som returneras genom att anropa den [ta] metod.</span><span class="sxs-lookup"><span data-stu-id="2f64c-203">You can increase the number of returned rows by calling the [Take] method.</span></span> <span data-ttu-id="2f64c-204">Använd `Take` tillsammans med den [hoppa över] metod för att begära en specifik ”page” på den totala datamängden som returneras av frågan.</span><span class="sxs-lookup"><span data-stu-id="2f64c-204">Use `Take` along with the [Skip] method to request a specific "page" of the total dataset returned by the query.</span></span> <span data-ttu-id="2f64c-205">Följande fråga när den exekveras, returnerar de översta tre objekten i tabellen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-205">The following query, when executed, returns the top three items in the table.</span></span>

```
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="2f64c-206">Följande reviderade fråga hoppar över de tre första resultaten och returnerar följande tre resultaten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-206">The following revised query skips the first three results and returns the next three results.</span></span> <span data-ttu-id="2f64c-207">Den här frågan ger andra ”sidan” data, där sidstorleken är tre objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-207">This query produces the second "page" of data, where the page size is three items.</span></span>

```
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="2f64c-208">Den [IncludeTotalCount] metoden begär det totala antalet för *alla* poster som har returnerats, ignoreras eventuella sidindelning/gräns-sats har angetts:</span><span class="sxs-lookup"><span data-stu-id="2f64c-208">The [IncludeTotalCount] method requests the total count for *all* the records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="2f64c-209">I verkligheten app, kan du använda frågor liknar föregående exempel med en personsökare kontroll eller jämförbar UI för att navigera mellan sidor.</span><span class="sxs-lookup"><span data-stu-id="2f64c-209">In a real world app, you can use queries similar to the preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="2f64c-210">Om du vill åsidosätta 50-rad-gränsen i en mobilappsserverdel, måste du också använda den [EnableQueryAttribute] för allmänheten GET-metod och ange beteendet sidindelning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-210">To override the 50-row limit in a Mobile App backend, you must also apply the [EnableQueryAttribute] to the public GET method and specify the paging behavior.</span></span> <span data-ttu-id="2f64c-211">När det används metoden, anger följande den maximala returnerade rader till 1000:</span><span class="sxs-lookup"><span data-stu-id="2f64c-211">When applied to the method, the following sets the maximum returned rows to 1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="2f64c-212"><a name="selecting"></a>Så här: Markera specifika kolumner</span><span class="sxs-lookup"><span data-stu-id="2f64c-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="2f64c-213">Du kan ange vilken uppsättning egenskaper som ska inkluderas i resultaten genom att lägga till en [Välj] sats i frågan.</span><span class="sxs-lookup"><span data-stu-id="2f64c-213">You can specify which set of properties to include in the results by adding a [Select] clause to your query.</span></span> <span data-ttu-id="2f64c-214">Följande kod visar till exempel hur du väljer ett enda fält och markera och formatera flera fält:</span><span class="sxs-lookup"><span data-stu-id="2f64c-214">For example, the following code shows how to select just one field and also how to select and format multiple fields:</span></span>

```
// Select one field -- just the Text
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

<span data-ttu-id="2f64c-215">Alla funktioner som beskrivs hittills är tillsatsen, så vi kan hålla länkning dem.</span><span class="sxs-lookup"><span data-stu-id="2f64c-215">All the functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="2f64c-216">Varje länkad anrop påverkar flera av frågan.</span><span class="sxs-lookup"><span data-stu-id="2f64c-216">Each chained call affects more of the query.</span></span> <span data-ttu-id="2f64c-217">Ytterligare ett exempel:</span><span class="sxs-lookup"><span data-stu-id="2f64c-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="2f64c-218"><a name="lookingup"></a>Så här: Leta upp data-ID: t</span><span class="sxs-lookup"><span data-stu-id="2f64c-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="2f64c-219">Den [LookupAsync] funktionen kan användas för att söka efter objekt från databasen med en viss-ID.</span><span class="sxs-lookup"><span data-stu-id="2f64c-219">The [LookupAsync] function can be used to look up objects from the database with a particular ID.</span></span>

```
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="2f64c-220"><a name="untypedqueries"></a>Så här: köra någon frågor</span><span class="sxs-lookup"><span data-stu-id="2f64c-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="2f64c-221">När du kör en fråga med ett argument tabellobjekt, måste du uttryckligen ange frågesträngen OData genom att anropa [ReadAsync], som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2f64c-221">When executing a query using an untyped table object, you must explicitly specify the OData query string by calling [ReadAsync], as in the following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="2f64c-222">Åter JSON-värden som du kan använda som en egenskapsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="2f64c-223">Mer information om JToken och Newtonsoft Json.NET finns i [Json.NET] plats.</span><span class="sxs-lookup"><span data-stu-id="2f64c-223">For more information on JToken and Newtonsoft Json.NET, see the [Json.NET] site.</span></span>

### <span data-ttu-id="2f64c-224"><a name="inserting"></a>Så här: Infoga data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="2f64c-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="2f64c-225">Alla klienttyper av måste innehålla en medlem med namnet **Id**, vilket är som standard en sträng.</span><span class="sxs-lookup"><span data-stu-id="2f64c-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="2f64c-226">Detta **Id** krävs för att utföra CRUD-åtgärder och för offlinesynkronisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-226">This **Id** is required to perform CRUD operations and for offline sync.</span></span> <span data-ttu-id="2f64c-227">Följande kod visar hur du använder den [InsertAsync] metod för att infoga nya rader i en tabell.</span><span class="sxs-lookup"><span data-stu-id="2f64c-227">The following code illustrates how to use the [InsertAsync] method to insert new rows into a table.</span></span> <span data-ttu-id="2f64c-228">Parametern innehåller data som ska läggas till som ett .NET-objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-228">The parameter contains the data to be inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="2f64c-229">Om ett unikt värde för anpassad ID inte ingår i den `todoItem` under en insert-, ett GUID som genereras av servern.</span><span class="sxs-lookup"><span data-stu-id="2f64c-229">If a unique custom ID value is not included in the `todoItem` during an insert, a GUID is generated by the server.</span></span>
<span data-ttu-id="2f64c-230">Du kan hämta den genererade Id genom att inspektera objektet när anropet returnerar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-230">You can retrieve the generated Id by inspecting the object after the call returns.</span></span>

<span data-ttu-id="2f64c-231">Om du vill infoga någon data, kan du dra nytta av Json.NET:</span><span class="sxs-lookup"><span data-stu-id="2f64c-231">To insert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="2f64c-232">Här är ett exempel som använder en e-postadress som en unik sträng-id:</span><span class="sxs-lookup"><span data-stu-id="2f64c-232">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="2f64c-233">Arbeta med ID-värden</span><span class="sxs-lookup"><span data-stu-id="2f64c-233">Working with ID values</span></span>
<span data-ttu-id="2f64c-234">Mobile Apps stöder unika anpassade strängvärden för tabellens **id** kolumn.</span><span class="sxs-lookup"><span data-stu-id="2f64c-234">Mobile Apps supports unique custom string values for the table's **id** column.</span></span> <span data-ttu-id="2f64c-235">Ett strängvärde som gör att program kan använda anpassade värden, till exempel e-postadresser eller användarnamn för-ID.</span><span class="sxs-lookup"><span data-stu-id="2f64c-235">A string value allows applications to use custom values such as email addresses or user names for the ID.</span></span>  <span data-ttu-id="2f64c-236">Sträng-ID: N ge följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2f64c-236">String IDs provide you with the following benefits:</span></span>

* <span data-ttu-id="2f64c-237">ID: N skapas utan att göra onödig kommunikation till databasen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-237">IDs are generated without making a round trip to the database.</span></span>
* <span data-ttu-id="2f64c-238">Poster är enklare att koppla från olika tabeller eller databaser.</span><span class="sxs-lookup"><span data-stu-id="2f64c-238">Records are easier to merge from different tables or databases.</span></span>
* <span data-ttu-id="2f64c-239">ID-värden kan integrera bättre med en programlogik.</span><span class="sxs-lookup"><span data-stu-id="2f64c-239">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="2f64c-240">När ett strängvärde ID inte har angetts på en infogad post genererar serverdelen för Mobilappen ett unikt värde för-ID.</span><span class="sxs-lookup"><span data-stu-id="2f64c-240">When a string ID value is not set on an inserted record, the Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="2f64c-241">Du kan använda den [Guid.NewGuid] metod för att generera en egen ID-värden på klienten eller i serverdelen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-241">You can use the [Guid.NewGuid] method to generate your own ID values, either on the client or in the backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="2f64c-242"><a name="modifying"></a>Så här: ändra data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="2f64c-242"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="2f64c-243">Följande kod visar hur du använder den [UpdateAsync] metod för att uppdatera en befintlig post med samma ID med ny information.</span><span class="sxs-lookup"><span data-stu-id="2f64c-243">The following code illustrates how to use the [UpdateAsync] method to update an existing record with the same ID with new information.</span></span> <span data-ttu-id="2f64c-244">Parametern innehåller data som ska uppdateras som ett .NET-objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-244">The parameter contains the data to be updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="2f64c-245">Om du vill uppdatera ej typbestämd data, kan du dra nytta av [Json.NET] på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-245">To update untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="2f64c-246">En `id` fält måste anges när du gör en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-246">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="2f64c-247">Serverdelen använder den `id` fältet för att identifiera vilken rad att uppdatera.</span><span class="sxs-lookup"><span data-stu-id="2f64c-247">The backend uses the `id` field to identify which row to update.</span></span> <span data-ttu-id="2f64c-248">Den `id` fältet kan hämtas från resultatet av den `InsertAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="2f64c-248">The `id` field can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="2f64c-249">En `ArgumentException` uppstår om du försöker uppdatera ett objekt utan att ange den `id` värde.</span><span class="sxs-lookup"><span data-stu-id="2f64c-249">An `ArgumentException` is raised if you try to update an item without providing the `id` value.</span></span>

### <span data-ttu-id="2f64c-250"><a name="deleting"></a>Så här: ta bort data i en mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="2f64c-250"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="2f64c-251">Följande kod visar hur du använder den [DeleteAsync] metod för att ta bort en befintlig instans.</span><span class="sxs-lookup"><span data-stu-id="2f64c-251">The following code illustrates how to use the [DeleteAsync] method to delete an existing instance.</span></span> <span data-ttu-id="2f64c-252">Instansen identifieras av den `id` fältet set på den `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-252">The instance is identified by the `id` field set on the `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="2f64c-253">Om du vill ta bort ej typbestämd data, kan du dra nytta av Json.NET på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-253">To delete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="2f64c-254">När du gör en begäran om borttagning måste ett ID anges.</span><span class="sxs-lookup"><span data-stu-id="2f64c-254">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="2f64c-255">Andra egenskaper skickas inte till tjänsten eller ignoreras i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-255">Other properties are not passed to the service or are ignored at the service.</span></span> <span data-ttu-id="2f64c-256">Resultatet av en `DeleteAsync` anropet är vanligtvis `null`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-256">The result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="2f64c-257">Det ID: T för att skicka in kan hämtas från resultatet av den `InsertAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="2f64c-257">The ID to pass in can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="2f64c-258">En `MobileServiceInvalidOperationException` när du försöker ta bort ett objekt utan att ange den `id` fältet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-258">A `MobileServiceInvalidOperationException` is thrown when you try to delete an item without specifying the `id` field.</span></span>

### <span data-ttu-id="2f64c-259"><a name="optimisticconcurrency"></a>Så här: använda Optimistisk samtidighet för konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="2f64c-259"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="2f64c-260">Två eller flera klienter kan skriva ändringar till samma objekt samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-260">Two or more clients may write changes to the same item at the same time.</span></span> <span data-ttu-id="2f64c-261">Utan konfliktidentifiering, skulle den senaste skrivningen skriva över eventuella tidigare uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-261">Without conflict detection, the last write would overwrite any previous updates.</span></span> <span data-ttu-id="2f64c-262">**Optimistisk samtidighetskontroll** förutsätter att varje transaktion kan genomföra och därför använder inte någon resurslåsning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-262">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="2f64c-263">Innan du genomför en transaktion verifierar optimistisk samtidighetskontroll att ingen annan transaktion har ändrat data.</span><span class="sxs-lookup"><span data-stu-id="2f64c-263">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified the data.</span></span> <span data-ttu-id="2f64c-264">Om data har ändrats, återställs committing transaktionen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-264">If the data has been modified, the committing transaction is rolled back.</span></span>

<span data-ttu-id="2f64c-265">Mobile Apps stöder optimistisk samtidighetskontroll genom att spåra ändringar av varje objekt med hjälp av den `version` system egenskapskolumn som har definierats för varje tabell i din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-265">Mobile Apps supports optimistic concurrency control by tracking changes to each item using the `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="2f64c-266">Varje gång en post uppdateras Mobile Apps anger den `version` -egenskapen för posten till ett nytt värde.</span><span class="sxs-lookup"><span data-stu-id="2f64c-266">Each time a record is updated, Mobile Apps sets the `version` property for that record to a new value.</span></span> <span data-ttu-id="2f64c-267">Under varje begäran om uppdatering av `version` egenskapen på den post som ingår i begäran jämförs med samma egenskap för posten på servern.</span><span class="sxs-lookup"><span data-stu-id="2f64c-267">During each update request, the `version` property of the record included with the request is compared to the same property for the record on the server.</span></span> <span data-ttu-id="2f64c-268">Om versionen har skickats med förfrågan matchar inte serverdelen och sedan klientbiblioteket genererar en `MobileServicePreconditionFailedException<T>` undantag.</span><span class="sxs-lookup"><span data-stu-id="2f64c-268">If the version passed with the request does not match the backend, then the client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="2f64c-269">Den typ som anges med undantaget är post från serverdel som innehåller servrar version av posten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-269">The type included with the exception is the record from the backend containing the servers version of the record.</span></span> <span data-ttu-id="2f64c-270">Programmet kan sedan använda den här informationen för att bestämma om du vill köra begäran om uppdatering igen med rätt `version` värde från backend att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="2f64c-270">The application can then use this information to decide whether to execute the update request again with the correct `version` value from the backend to commit changes.</span></span>

<span data-ttu-id="2f64c-271">Definiera en kolumn i tabellen-klass för de `version` Systemegenskapen att aktivera Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-271">Define a column on the table class for the `version` system property to enable optimistic concurrency.</span></span> <span data-ttu-id="2f64c-272">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f64c-272">For example:</span></span>

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

<span data-ttu-id="2f64c-273">Program med hjälp av någon tabeller aktivera Optimistisk samtidighet genom att ange den `Version` flaggan på den `SystemProperties` i tabellen på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-273">Applications using untyped tables enable optimistic concurrency by setting the `Version` flag on the `SystemProperties` of the table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="2f64c-274">Utöver att möjliggöra Optimistisk samtidighet, måste du också fånga den `MobileServicePreconditionFailedException<T>` undantag i koden när du anropar [UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="2f64c-274">In addition to enabling optimistic concurrency, you must also catch the `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="2f64c-275">Lös konflikten genom att använda rätt `version` uppdaterade posten och anropet [UpdateAsync] med löst posten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-275">Resolve the conflict by applying the correct `version` to the updated record and call [UpdateAsync] with the resolved record.</span></span> <span data-ttu-id="2f64c-276">Följande kod visar hur du löser en Skriv-konflikt upptäcktes när:</span><span class="sxs-lookup"><span data-stu-id="2f64c-276">The following code shows how to resolve a write conflict once detected:</span></span>

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resoltion between versions
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
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

<span data-ttu-id="2f64c-277">Mer information finns i [Offline datasynkronisering i Azure Mobile Apps] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-277">For more information, see the [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="2f64c-278"><a name="binding"></a>Så här: Bind Mobile Apps data till en Windows-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="2f64c-278"><a name="binding"></a>How to: Bind Mobile Apps data to a Windows user interface</span></span>
<span data-ttu-id="2f64c-279">Det här avsnittet visar hur du visar returnerade dataobjekt med hjälp av UI-element i en Windows-app.</span><span class="sxs-lookup"><span data-stu-id="2f64c-279">This section shows how to display returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="2f64c-280">Följande exempelkod Binder till källan för listan med en fråga om ofullständiga objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-280">The following example code binds to the source of the list with a query for incomplete items.</span></span> <span data-ttu-id="2f64c-281">Den [MobileServiceCollection] skapar en Mobile Apps-medvetna bindning-samling.</span><span class="sxs-lookup"><span data-stu-id="2f64c-281">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="2f64c-282">Vissa kontroller i hanterade körningsmiljön stöd för ett gränssnitt som kallas [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="2f64c-282">Some controls in the managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="2f64c-283">Du kan använda kontroller för att begära extra data när användaren rullar i gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-283">This interface allows controls to request extra data when the user scrolls.</span></span> <span data-ttu-id="2f64c-284">Det finns inbyggt stöd för det här gränssnittet för universella Windows-appar via [MobileServiceIncrementalLoadingCollection], som automatiskt hanterar anrop från kontroller.</span><span class="sxs-lookup"><span data-stu-id="2f64c-284">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from the controls.</span></span> <span data-ttu-id="2f64c-285">Använd `MobileServiceIncrementalLoadingCollection` i Windows-appar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-285">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="2f64c-286">Om du vill använda den nya samlingen på Windows Phone 8 och ”Silverlight” den `ToCollection` tilläggsmetoder på `IMobileServiceTableQuery<T>` och `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-286">To use the new collection on Windows Phone 8 and "Silverlight" apps, use the `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="2f64c-287">Om du vill läsa in data, anropa `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-287">To load data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="2f64c-288">När du använder den samling som skapats genom att anropa `ToCollectionAsync` eller `ToCollection`, du får en samling som kan bindas till UI-kontroller.</span><span class="sxs-lookup"><span data-stu-id="2f64c-288">When you use the collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound to UI controls.</span></span>  <span data-ttu-id="2f64c-289">Den här samlingen är medveten om sidindelning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-289">This collection is paging-aware.</span></span>  <span data-ttu-id="2f64c-290">Eftersom samlingen är läser in data från nätverket, misslyckas inläsning ibland.</span><span class="sxs-lookup"><span data-stu-id="2f64c-290">Since the collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="2f64c-291">Om du vill hantera dessa fel, åsidosätta den `OnException` metod på `MobileServiceIncrementalLoadingCollection` att hantera undantag som uppstår till följd av anrop till `LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-291">To handle such failures, override the `OnException` method on `MobileServiceIncrementalLoadingCollection` to handle exceptions resulting from calls to `LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="2f64c-292">Överväg om tabellen innehåller många fält men du bara vill visa en del av dem i kontrollen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-292">Consider if your table has many fields but you only want to display some of them in your control.</span></span> <span data-ttu-id="2f64c-293">Du kan använda riktlinjerna i föregående avsnitt ”[markera specifika kolumner](#selecting)” att markera specifika kolumner ska visas i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-293">You may use the guidance in the preceding section "[Select specific columns](#selecting)" to select specific columns to display in the UI.</span></span>

### <span data-ttu-id="2f64c-294"><a name="pagesize"></a>Ändra sidstorleken</span><span class="sxs-lookup"><span data-stu-id="2f64c-294"><a name="pagesize"></a>Change the Page size</span></span>
<span data-ttu-id="2f64c-295">Azure Mobile Apps returnerar maximalt 50 objekt per begäran som standard.</span><span class="sxs-lookup"><span data-stu-id="2f64c-295">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="2f64c-296">Du kan ändra växlingsfilens storlek genom att öka den maximala sidstorleken på både klienten och servern.</span><span class="sxs-lookup"><span data-stu-id="2f64c-296">You can change the paging size by increasing the maximum page size on both the client and server.</span></span>  <span data-ttu-id="2f64c-297">För att öka storleken på begärt ange `PullOptions` när du använder `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="2f64c-297">To increase the requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="2f64c-298">Under förutsättning att du har gjort det `PageSize` lika med eller större än 100 inom server, en begäran returnerar upp till 100 poster.</span><span class="sxs-lookup"><span data-stu-id="2f64c-298">Assuming you have made the `PageSize` equal to or greater than 100 within the server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="2f64c-299"><a name="#offlinesync"></a>Arbeta med Offline tabeller</span><span class="sxs-lookup"><span data-stu-id="2f64c-299"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="2f64c-300">Offline tabeller använder en lokal SQLite att lagra data för användning offline.</span><span class="sxs-lookup"><span data-stu-id="2f64c-300">Offline tables use a local SQLite store to store data for use when offline.</span></span>  <span data-ttu-id="2f64c-301">Alla tabellen operations görs mot lokalt SQLite lagra i stället för arkivet för fjärrservern.</span><span class="sxs-lookup"><span data-stu-id="2f64c-301">All table operations are done against the local SQLite store instead of the remote server store.</span></span>  <span data-ttu-id="2f64c-302">Om du vill skapa en tabell som är offline, förbereder du projektet:</span><span class="sxs-lookup"><span data-stu-id="2f64c-302">To create an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="2f64c-303">Högerklicka på lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i lösningen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-303">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="2f64c-304">(Valfritt) Installera en av följande SQLite runtime paket för att stödja Windows-enheter:</span><span class="sxs-lookup"><span data-stu-id="2f64c-304">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="2f64c-305">**Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="2f64c-305">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="2f64c-306">**Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="2f64c-306">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="2f64c-307">**Universella Windows-plattformen** installera [SQLite för universella Windows][5].</span><span class="sxs-lookup"><span data-stu-id="2f64c-307">**Universal Windows Platform** Install [SQLite for the Universal Windows][5].</span></span>
3. <span data-ttu-id="2f64c-308">(Valfritt).</span><span class="sxs-lookup"><span data-stu-id="2f64c-308">(Optional).</span></span> <span data-ttu-id="2f64c-309">Windows-enheter klickar du på **referenser** > **Lägg till referens...** , expandera den **Windows** mappen > **tillägg**, aktivera rätt **SQLite för Windows** SDK tillsammans med **Visual C++ 2013 Runtime för Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="2f64c-309">For Windows devices, click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**, then enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="2f64c-310">SQLite SDK namnen varierar något med varje Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-310">The SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="2f64c-311">Innan en tabellreferens kan skapas, måste du förbereda det lokala arkivet:</span><span class="sxs-lookup"><span data-stu-id="2f64c-311">Before a table reference can be created, the local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="2f64c-312">Initiering av certifikatarkiv görs normalt omedelbart efter att klienten har skapats.</span><span class="sxs-lookup"><span data-stu-id="2f64c-312">Store initialization is normally done immediately after the client is created.</span></span>  <span data-ttu-id="2f64c-313">Den **OfflineDbPath** bör vara ett filnamn som är lämplig för användning på alla plattformar som du stöder.</span><span class="sxs-lookup"><span data-stu-id="2f64c-313">The **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="2f64c-314">Om sökvägen är en fullständigt kvalificerad sökväg (d.v.s. den börjar med ett snedstreck), och sedan används.</span><span class="sxs-lookup"><span data-stu-id="2f64c-314">If the path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="2f64c-315">Om sökvägen inte är fullständigt kvalificerad, placera filen i en plattformsspecifik plats.</span><span class="sxs-lookup"><span data-stu-id="2f64c-315">If the path is not fully qualified, the file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="2f64c-316">Standardsökvägen är mappen ”personliga filer” för iOS och Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="2f64c-316">For iOS and Android devices, the default path is the "Personal Files" folder.</span></span>
* <span data-ttu-id="2f64c-317">Standardsökvägen är mappen programspecifika ”AppData” för Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="2f64c-317">For Windows devices, the default path is the application-specific "AppData" folder.</span></span>

<span data-ttu-id="2f64c-318">En tabellreferens kan erhållas med hjälp av den `GetSyncTable<>` metoden:</span><span class="sxs-lookup"><span data-stu-id="2f64c-318">A table reference can be obtained using the `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="2f64c-319">Du behöver inte autentisera om du vill använda en offline-tabell.</span><span class="sxs-lookup"><span data-stu-id="2f64c-319">You do not need to authenticate to use an offline table.</span></span>  <span data-ttu-id="2f64c-320">Du behöver bara autentisera när du kommunicerar med serverdelstjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-320">You only need to authenticate when you are communicating with the backend service.</span></span>

### <span data-ttu-id="2f64c-321"><a name="syncoffline"></a>Synkroniserar en Offline-tabell</span><span class="sxs-lookup"><span data-stu-id="2f64c-321"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="2f64c-322">Offline tabeller är inte synkroniserade med serverdelen som standard.</span><span class="sxs-lookup"><span data-stu-id="2f64c-322">Offline tables are not synchronized with the backend by default.</span></span>  <span data-ttu-id="2f64c-323">Synkronisering är uppdelat i två delar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-323">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="2f64c-324">Du kan skicka ändringar separat hämtar nya objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-324">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="2f64c-325">Här är en typisk synkroniseringsmetod:</span><span class="sxs-lookup"><span data-stu-id="2f64c-325">Here is a typical sync method:</span></span>

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
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

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
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

<span data-ttu-id="2f64c-326">Om det första argumentet för `PullAsync` är null, och sedan inte används för inkrementell synkronisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-326">If the first argument to `PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="2f64c-327">Varje synkroniseringsåtgärd hämtar alla poster.</span><span class="sxs-lookup"><span data-stu-id="2f64c-327">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="2f64c-328">SDK utför en implicit `PushAsync()` innan dra poster.</span><span class="sxs-lookup"><span data-stu-id="2f64c-328">The SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="2f64c-329">Konflikt hantering sker på en `PullAsync()` metod.</span><span class="sxs-lookup"><span data-stu-id="2f64c-329">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="2f64c-330">Du kan hantera konflikter på samma sätt som online-tabeller.</span><span class="sxs-lookup"><span data-stu-id="2f64c-330">You can deal with conflicts in the same way as online tables.</span></span>  <span data-ttu-id="2f64c-331">Konflikten skapas när `PullAsync()` anropas i stället för under insert-, update- eller delete.</span><span class="sxs-lookup"><span data-stu-id="2f64c-331">The conflict is produced when `PullAsync()` is called instead of during the insert, update, or delete.</span></span> <span data-ttu-id="2f64c-332">Om flera sådana konflikter uppstår ingår de i en enda MobileServicePushFailedException.</span><span class="sxs-lookup"><span data-stu-id="2f64c-332">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="2f64c-333">Hantera varje fel separat.</span><span class="sxs-lookup"><span data-stu-id="2f64c-333">Handle each failure separately.</span></span>

## <span data-ttu-id="2f64c-334"><a name="#customapi"></a>Arbeta med en anpassad API</span><span class="sxs-lookup"><span data-stu-id="2f64c-334"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="2f64c-335">En anpassad API kan du definiera anpassade slutpunkter som exponerar serverfunktioner som inte mappas till en infoga, uppdatera, ta bort eller Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="2f64c-335">A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation.</span></span> <span data-ttu-id="2f64c-336">Genom att använda en anpassad API kan ha du mer kontroll över meddelanden, inklusive läsning och ange HTTP-meddelandehuvuden och definiera ett body-meddelandeformat än JSON.</span><span class="sxs-lookup"><span data-stu-id="2f64c-336">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="2f64c-337">Du kan anropa en anpassad API genom att anropa en av de [InvokeApiAsync] metoder på klienten.</span><span class="sxs-lookup"><span data-stu-id="2f64c-337">You call a custom API by calling one of the [InvokeApiAsync] methods on the client.</span></span> <span data-ttu-id="2f64c-338">Till exempel följande kodrad skickar en POST-begäran till den **completeAll** API på serverdelen:</span><span class="sxs-lookup"><span data-stu-id="2f64c-338">For example, the following line of code sends a POST request to the **completeAll** API on the backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="2f64c-339">Det här formuläret är skrivna metodanrop och kräver att den **MarkAllResult** returnera typen är definierad.</span><span class="sxs-lookup"><span data-stu-id="2f64c-339">This form is a typed method call and requires that the **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="2f64c-340">Både skrivna och ej typbestämd metoder stöds.</span><span class="sxs-lookup"><span data-stu-id="2f64c-340">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="2f64c-341">Metoden InvokeApiAsync() läggs/api /'-API: et som du vill anropa om API: et som börjar med en '/'.</span><span class="sxs-lookup"><span data-stu-id="2f64c-341">The InvokeApiAsync() method prepends '/api/' to the API that you wish to call unless the API starts with a '/'.</span></span>
<span data-ttu-id="2f64c-342">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f64c-342">For example:</span></span>

* <span data-ttu-id="2f64c-343">`InvokeApiAsync("completeAll",...)`anropar /api/completeAll på serverdelen</span><span class="sxs-lookup"><span data-stu-id="2f64c-343">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on the backend</span></span>
* <span data-ttu-id="2f64c-344">`InvokeApiAsync("/.auth/me",...)`anropar /.auth/me på serverdelen</span><span class="sxs-lookup"><span data-stu-id="2f64c-344">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on the backend</span></span>

<span data-ttu-id="2f64c-345">Du kan använda InvokeApiAsync för att anropa alla WebAPI, inklusive de WebAPIs som inte har definierats med Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="2f64c-345">You can use InvokeApiAsync to call any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="2f64c-346">När du använder InvokeApiAsync() skickas lämplig sidhuvud, inklusive autentiseringshuvuden med begäran.</span><span class="sxs-lookup"><span data-stu-id="2f64c-346">When you use InvokeApiAsync(), the appropriate headers, including authentication headers, are sent with the request.</span></span>

## <span data-ttu-id="2f64c-347"><a name="authentication"></a>Autentisera användare</span><span class="sxs-lookup"><span data-stu-id="2f64c-347"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="2f64c-348">Mobile Apps stöder autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account, Twitter och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2f64c-348">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="2f64c-349">Du kan ange behörigheter på tabeller för att begränsa åtkomsten för specifika åtgärder endast autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="2f64c-349">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="2f64c-350">Du kan också använda identiteten för autentiserade användare för att implementera auktoriseringsregler i server-skript.</span><span class="sxs-lookup"><span data-stu-id="2f64c-350">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="2f64c-351">Mer information finns i självstudierna [Lägg till autentisering till din app].</span><span class="sxs-lookup"><span data-stu-id="2f64c-351">For more information, see the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="2f64c-352">Två autentisering flöden stöds: *klienten hanteras* och *server-hanterade* flöde.</span><span class="sxs-lookup"><span data-stu-id="2f64c-352">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="2f64c-353">Server-hanterade flödet ger enklaste autentiseringsupplevelse, eftersom det är beroende av leverantörens Webbgränssnitt för autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-353">The server-managed flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="2f64c-354">Klient-hanterade flödet tillåter djupare integrering med specifika funktioner som använder den provider-specifik enhetsspecifika SDK.</span><span class="sxs-lookup"><span data-stu-id="2f64c-354">The client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="2f64c-355">Du bör använda ett klient-hanterade flöde i dina appar i produktion.</span><span class="sxs-lookup"><span data-stu-id="2f64c-355">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="2f64c-356">Om du vill konfigurera autentisering måste du registrera din app med en eller flera identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="2f64c-356">To set up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="2f64c-357">Identitetsleverantören som genererar ett klient-ID och en klienthemlighet för din app.</span><span class="sxs-lookup"><span data-stu-id="2f64c-357">The identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="2f64c-358">Dessa värden anges sedan i din serverdel för att aktivera autentisering/auktorisering i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2f64c-358">These values are then set in your backend to enable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="2f64c-359">Mer information följer detaljerade instruktioner i självstudiekursen [Lägg till autentisering till din app].</span><span class="sxs-lookup"><span data-stu-id="2f64c-359">For more information, follow the detailed instructions in the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="2f64c-360">I följande avsnitt beskrivs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="2f64c-360">The following topics are covered in this section:</span></span>

* [<span data-ttu-id="2f64c-361">Klienten hanteras autentisering</span><span class="sxs-lookup"><span data-stu-id="2f64c-361">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="2f64c-362">Hanterad Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="2f64c-362">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="2f64c-363">Cachelagring autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="2f64c-363">Caching the authentication token</span></span>](#caching)

### <span data-ttu-id="2f64c-364"><a name="clientflow"></a>Klienten hanteras autentisering</span><span class="sxs-lookup"><span data-stu-id="2f64c-364"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="2f64c-365">Din app kan oberoende kontakta identitetsleverantören och ange sedan det returnerade token under inloggningen med din serverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-365">Your app can independently contact the identity provider and then provide the returned token during login with your backend.</span></span> <span data-ttu-id="2f64c-366">Detta klient-flöde kan du tillhandahålla en enkel inloggning för användare eller hämta ytterligare användardata från identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="2f64c-366">This client flow enables you to provide a single sign-on experience for users or to retrieve additional user data from the identity provider.</span></span> <span data-ttu-id="2f64c-367">Flöde för klientautentisering är att föredra med ett server-flöde som identitetsleverantören SDK tillhandahåller en mer ursprungligt UX känslan och gör för ytterligare anpassning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-367">Client flow authentication is preferred to using a server flow as the identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="2f64c-368">Exempel har angetts för följande-flödet autentisering mönster:</span><span class="sxs-lookup"><span data-stu-id="2f64c-368">Examples are provided for the following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="2f64c-369">Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="2f64c-369">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="2f64c-370">Facebook eller Google</span><span class="sxs-lookup"><span data-stu-id="2f64c-370">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="2f64c-371">Live SDK</span><span class="sxs-lookup"><span data-stu-id="2f64c-371">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="2f64c-372"><a name="adal"></a>Autentisera användarna med Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="2f64c-372"><a name="adal"></a>Authenticate users with the Active Directory Authentication Library</span></span>
<span data-ttu-id="2f64c-373">Du kan använda Active Directory Authentication Library (ADAL) för att initiera användarautentisering från klienten med hjälp av Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-373">You can use the Active Directory Authentication Library (ADAL) to initiate user authentication from the client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="2f64c-374">Konfigurera mobilappsserverdelen för inloggning AAD genom att följa den [konfigurera App Service för Active Directory-inloggningen] kursen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-374">Configure your mobile app backend for AAD sign-on by following the [How to configure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="2f64c-375">Se till att slutföra det valfria steget med att registrera en native client-program.</span><span class="sxs-lookup"><span data-stu-id="2f64c-375">Make sure to complete the optional step of registering a native client application.</span></span>
2. <span data-ttu-id="2f64c-376">Öppna projektet i Visual Studio eller Xamarin Studio och lägga till en referens till den `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-376">In Visual Studio or Xamarin Studio, open your project and add a reference to the `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="2f64c-377">När du söker kan innehålla förhandsversioner.</span><span class="sxs-lookup"><span data-stu-id="2f64c-377">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="2f64c-378">Lägg till följande kod i ditt program, beroende på plattform som du använder.</span><span class="sxs-lookup"><span data-stu-id="2f64c-378">Add the following code to your application, according to the platform you are using.</span></span> <span data-ttu-id="2f64c-379">I varje, gör du följande ersättningar:</span><span class="sxs-lookup"><span data-stu-id="2f64c-379">In each, make the following replacements:</span></span>

   * <span data-ttu-id="2f64c-380">Ersätt **INSERT-UTFÄRDARE-här** med namnet på klienten som du har etablerat ditt program.</span><span class="sxs-lookup"><span data-stu-id="2f64c-380">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="2f64c-381">Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="2f64c-381">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span> <span data-ttu-id="2f64c-382">Det här värdet kan kopieras från fliken domän i Azure Active Directory i den [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="2f64c-382">This value can be copied from the Domain tab in your Azure Active Directory in the [Azure classic portal].</span></span>
   * <span data-ttu-id="2f64c-383">Ersätt **INSERT-resurs-ID-här** med klient-ID för din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-383">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="2f64c-384">Du kan hämta klient-ID från den **Avancerat** fliken **inställningarna för Azure Active Directory** i portalen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-384">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
   * <span data-ttu-id="2f64c-385">Ersätt **INSERT-klient-ID-här** med klient-ID som du kopierade från native client-program.</span><span class="sxs-lookup"><span data-stu-id="2f64c-385">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
   * <span data-ttu-id="2f64c-386">Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="2f64c-386">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="2f64c-387">Det här värdet ska vara liknar *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="2f64c-387">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="2f64c-388">Den kod som behövs för varje plattform som följer:</span><span class="sxs-lookup"><span data-stu-id="2f64c-388">The code needed for each platform follows:</span></span>

     <span data-ttu-id="2f64c-389">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="2f64c-389">**Windows:**</span></span>

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

     <span data-ttu-id="2f64c-390">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="2f64c-390">**Xamarin.iOS**</span></span>

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

     <span data-ttu-id="2f64c-391">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="2f64c-391">**Xamarin.Android**</span></span>

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

#### <span data-ttu-id="2f64c-392"><a name="client-facebook"></a>Enkel inloggning med hjälp av en token från Facebook eller Google</span><span class="sxs-lookup"><span data-stu-id="2f64c-392"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="2f64c-393">Du kan använda flödet som visas i den här fragment för Facebook eller Google.</span><span class="sxs-lookup"><span data-stu-id="2f64c-393">You can use the client flow as shown in this snippet for Facebook or Google.</span></span>

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
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
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
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

#### <span data-ttu-id="2f64c-394"><a name="client-livesdk"></a>Enkel inloggning med Live SDK Account</span><span class="sxs-lookup"><span data-stu-id="2f64c-394"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with the Live SDK</span></span>
<span data-ttu-id="2f64c-395">För att autentisera användare, måste du registrera din app på Microsoft-konto Developer Center.</span><span class="sxs-lookup"><span data-stu-id="2f64c-395">To authenticate users, you must register your app at the Microsoft account Developer Center.</span></span> <span data-ttu-id="2f64c-396">Konfigurera registreringsinformation på din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-396">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="2f64c-397">Skapa ett Microsoft-Kontoregistrering och ansluta till din mobilappsserverdel genom att slutföra stegen i [registrera din app om du vill använda en Microsoft-kontoinloggning].</span><span class="sxs-lookup"><span data-stu-id="2f64c-397">To create a Microsoft account registration and connect it to your Mobile App backend, complete the steps in [Register your app to use a Microsoft account login].</span></span> <span data-ttu-id="2f64c-398">Om du har både Windows Store och Windows Phone 8/Silverlight-versionerna av din app kan du registrera Windows Store-versionen först.</span><span class="sxs-lookup"><span data-stu-id="2f64c-398">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register the Windows Store version first.</span></span>

<span data-ttu-id="2f64c-399">Följande kod autentiserar med hjälp av Live SDK och använder den returnerade token för att logga in på din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="2f64c-399">The following code authenticates using Live SDK and uses the returned token to sign in to your Mobile App backend.</span></span>

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get the URL the Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create the authentication client for Windows Store using the service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create the authentication client for Windows Phone using the client ID of the registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request the authentication token from the Live authentication service.
        // The wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about the logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use the Microsoft account auth token to sign in to App Service.
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

<span data-ttu-id="2f64c-400">Mer information finns i [Windows Live SDK] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2f64c-400">For more information, see the [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="2f64c-401"><a name="serverflow"></a>Hanterad Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="2f64c-401"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="2f64c-402">När du har registrerat din identitetsleverantör anropa den [LoginAsync] metod på [MobileServiceClient] med den [MobileServiceAuthenticationProvider] värdet för leverantören.</span><span class="sxs-lookup"><span data-stu-id="2f64c-402">Once you have registered your identity provider, call the [LoginAsync] method on the [MobileServiceClient] with the [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="2f64c-403">Till exempel initierar följande kod en server flödet inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="2f64c-403">For example, the following code initiates a server flow sign-in by using Facebook.</span></span>

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

<span data-ttu-id="2f64c-404">Om du använder en identitetsleverantör än Facebook, ändrar du värdet för [MobileServiceAuthenticationProvider] med värdet för leverantören.</span><span class="sxs-lookup"><span data-stu-id="2f64c-404">If you are using an identity provider other than Facebook, change the value of [MobileServiceAuthenticationProvider] to the value for your provider.</span></span>

<span data-ttu-id="2f64c-405">I ett server-flöde hanterar Azure App Service flödet för OAuth-autentisering genom att visa inloggningssidan för vald leverantör.</span><span class="sxs-lookup"><span data-stu-id="2f64c-405">In a server flow, Azure App Service manages the OAuth authentication flow by displaying the sign-in page of the selected provider.</span></span>  <span data-ttu-id="2f64c-406">När identitet providern returnerar Azure App Service genererar en Apptjänst-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f64c-406">Once the identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="2f64c-407">Den [LoginAsync] metoden returnerar en [MobileServiceUser], som innehåller både den [UserId] på den autentiserade användaren och [ MobileServiceAuthenticationToken], som JSON web token (JWT).</span><span class="sxs-lookup"><span data-stu-id="2f64c-407">The [LoginAsync] method returns a [MobileServiceUser], which provides both the [UserId] of the authenticated user and the [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="2f64c-408">Den här token kan cachelagras och återanvändas tills den upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="2f64c-408">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="2f64c-409">Mer information finns i [cachelagring autentiseringstoken](#caching).</span><span class="sxs-lookup"><span data-stu-id="2f64c-409">For more information, see [Caching the authentication token](#caching).</span></span>

### <span data-ttu-id="2f64c-410"><a name="caching"></a>Cachelagring autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="2f64c-410"><a name="caching"></a>Caching the authentication token</span></span>
<span data-ttu-id="2f64c-411">I vissa fall undvikas anrop till metoden inloggningen efter den första autentiseringen genom att lagra autentiseringstoken från leverantören.</span><span class="sxs-lookup"><span data-stu-id="2f64c-411">In some cases, the call to the login method can be avoided after the first successful authentication by storing the authentication token from the provider.</span></span>  <span data-ttu-id="2f64c-412">Windows Store och UWP-appar kan använda [PasswordVault] till cachen aktuella autentiseringstoken efter en lyckad inloggning, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2f64c-412">Windows Store and UWP apps can use [PasswordVault] to cache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="2f64c-413">Användar-ID-värdet lagras som användarnamnet för autentiseringsuppgifter och token som är lagrade som lösenord.</span><span class="sxs-lookup"><span data-stu-id="2f64c-413">The UserId value is stored as the UserName of the credential and the token is the stored as the Password.</span></span> <span data-ttu-id="2f64c-414">I efterföljande nystartade företag, kan du kontrollera den **PasswordVault** för cachelagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2f64c-414">On subsequent start-ups, you can check the **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="2f64c-415">I följande exempel används cachelagrade autentiseringsuppgifter när de hittas och annars försöker autentisera igen med serverdelen:</span><span class="sxs-lookup"><span data-stu-id="2f64c-415">The following example uses cached credentials when they are found, and otherwise attempts to authenticate again with the backend:</span></span>

```
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

<span data-ttu-id="2f64c-416">När du loggar ut en användare måste du också bort lagrade autentiseringsuppgifter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-416">When you sign out a user, you must also remove the stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="2f64c-417">Xamarin-appar används i [Xamarin.Auth] -API: er på ett säkert sätt lagra autentiseringsuppgifter i en **konto** objekt.</span><span class="sxs-lookup"><span data-stu-id="2f64c-417">Xamarin    apps use the [Xamarin.Auth] APIs to securely store credentials in an **Account** object.</span></span> <span data-ttu-id="2f64c-418">Ett exempel på hur du använder dessa API: er finns i [AuthStore.cs] kodfilen i den [ContosoMoments foto delning exempel](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="2f64c-418">For an example of using these APIs, see the [AuthStore.cs] code file in the [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="2f64c-419">När du använder klienten hanteras autentisering, kan du Cachelagra den åtkomst-token som hämtades från leverantören, till exempel Facebook och Twitter.</span><span class="sxs-lookup"><span data-stu-id="2f64c-419">When you use client-managed authentication, you can also cache the access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="2f64c-420">Denna token kan anges för att begära en ny autentiseringstoken från serverdelen, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2f64c-420">This token can be supplied to request a new authentication token from the backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="2f64c-421"><a name="pushnotifications"></a>Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="2f64c-421"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="2f64c-422">I följande avsnitt beskrivs Push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="2f64c-422">The following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="2f64c-423">Registrera dig för Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="2f64c-423">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="2f64c-424">Skaffa en Windows Store paket-SID</span><span class="sxs-lookup"><span data-stu-id="2f64c-424">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="2f64c-425">Registrera med plattformsoberoende mallar</span><span class="sxs-lookup"><span data-stu-id="2f64c-425">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="2f64c-426"><a name="register-for-push"></a>Så här: registrera dig för Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="2f64c-426"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="2f64c-427">Mobile Apps-klienten kan du registrera dig för push-meddelanden med Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="2f64c-427">The Mobile Apps client enables you to register for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="2f64c-428">När du registrerar dig, kan du hämta en referens som du har fått från plattformsspecifika Push Notification Service (PNS).</span><span class="sxs-lookup"><span data-stu-id="2f64c-428">When registering, you obtain a handle that you obtain from the platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="2f64c-429">Du kan sedan ange värdet tillsammans med alla taggar när du skapar registreringen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-429">You then provide this value along with any tags when you create the registration.</span></span> <span data-ttu-id="2f64c-430">Följande kod registrerar din Windows-app för push-meddelanden med Windows Notification Service (WNS):</span><span class="sxs-lookup"><span data-stu-id="2f64c-430">The following code registers your Windows app for push notifications with the Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="2f64c-431">Om du sänder till WNS och sedan måste du [hämtar Windows Store-paketet SID](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="2f64c-431">If you are pushing to WNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="2f64c-432">Mer information om Windows-appar, inklusive hur du registrerar dig för mallen registreringar finns [Lägg till push-meddelanden i appen].</span><span class="sxs-lookup"><span data-stu-id="2f64c-432">For more information on Windows apps, including how to register for template registrations, see [Add push notifications to your app].</span></span>

<span data-ttu-id="2f64c-433">Begär taggar från klienten stöds inte.</span><span class="sxs-lookup"><span data-stu-id="2f64c-433">Requesting tags from the client is not supported.</span></span>  <span data-ttu-id="2f64c-434">Taggen begäranden släpps tyst från registreringen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-434">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="2f64c-435">Om du vill registrera din enhet med taggar kan du skapa en anpassad API som använder API: et för Notification Hubs för att utföra registreringen för din räkning.</span><span class="sxs-lookup"><span data-stu-id="2f64c-435">If you wish to register your device with tags, create a Custom API that uses the Notification Hubs API to perform the registration on your behalf.</span></span>  <span data-ttu-id="2f64c-436">[Anropa anpassade API](#customapi) i stället för den `RegisterNativeAsync()` metoden.</span><span class="sxs-lookup"><span data-stu-id="2f64c-436">[Call the Custom API](#customapi) instead of the `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="2f64c-437"><a name="package-sid"></a>Så här: hämta en Windows Store paket-SID</span><span class="sxs-lookup"><span data-stu-id="2f64c-437"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="2f64c-438">Paket-SID krävs för att aktivera push-meddelanden i Windows Store-appar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-438">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="2f64c-439">Registrera ditt program med Windows Store för att ta emot paket-SID.</span><span class="sxs-lookup"><span data-stu-id="2f64c-439">To receive a package SID, register your application with the Windows Store.</span></span>

<span data-ttu-id="2f64c-440">Du kan hämta det här värdet:</span><span class="sxs-lookup"><span data-stu-id="2f64c-440">To obtain this value:</span></span>

1. <span data-ttu-id="2f64c-441">Högerklicka på Windows Store-app-projekt i Visual Studio Solution Explorer, klickar du på **Store** > **associera App med Store...** .</span><span class="sxs-lookup"><span data-stu-id="2f64c-441">In Visual Studio Solution Explorer, right-click the Windows Store app project, click **Store** > **Associate App with the Store...**.</span></span>
2. <span data-ttu-id="2f64c-442">I guiden klickar du på **nästa**, logga in med ditt Microsoft-konto, Skriv ett namn för din app i **reservera ett nytt namn för appen**, klicka på **reservera**.</span><span class="sxs-lookup"><span data-stu-id="2f64c-442">In the wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="2f64c-443">När registreringen app har skapats, Välj namnet på appen, klicka på **nästa**, och klicka sedan på **associera**.</span><span class="sxs-lookup"><span data-stu-id="2f64c-443">After the app registration is successfully created, select the app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="2f64c-444">Logga in på den [Windows Dev Center] med ditt Microsoft-Account.</span><span class="sxs-lookup"><span data-stu-id="2f64c-444">Log in to the [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="2f64c-445">Under **Mina appar**, klicka på appen registreringen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2f64c-445">Under **My apps**, click the app registration you created.</span></span>
5. <span data-ttu-id="2f64c-446">Klicka på **apphantering** > **App identitet**, och rulla sedan hitta din **paket-SID**.</span><span class="sxs-lookup"><span data-stu-id="2f64c-446">Click **App management** > **App identity**, and then scroll down to find your **Package SID**.</span></span>

<span data-ttu-id="2f64c-447">Många användningsområden för paket-SID behandla det som en URI i så fall måste du använda *ms-app: / /* som schema.</span><span class="sxs-lookup"><span data-stu-id="2f64c-447">Many uses of the package SID treat it as a URI, in which case you need to use *ms-app://* as the scheme.</span></span> <span data-ttu-id="2f64c-448">Anteckna versionen av ditt paket-SID som bildas genom att det här värdet som ett prefix.</span><span class="sxs-lookup"><span data-stu-id="2f64c-448">Make note of the version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="2f64c-449">Xamarin-appar kräver ytterligare kod för att kunna registrera en app som körs på iOS eller Android-plattformar.</span><span class="sxs-lookup"><span data-stu-id="2f64c-449">Xamarin apps require some additional code to be able to register an app running on the iOS or Android platforms.</span></span> <span data-ttu-id="2f64c-450">Mer information finns i avsnittet för din plattform:</span><span class="sxs-lookup"><span data-stu-id="2f64c-450">For more information, see the topic for your platform:</span></span>

* [<span data-ttu-id="2f64c-451">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="2f64c-451">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="2f64c-452">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="2f64c-452">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="2f64c-453"><a name="register-xplat"></a>Så här: registrera push-mallar för att skicka plattformsoberoende meddelanden</span><span class="sxs-lookup"><span data-stu-id="2f64c-453"><a name="register-xplat"></a>How to: Register push templates to send cross-platform notifications</span></span>
<span data-ttu-id="2f64c-454">Registrera mallar genom att använda den `RegisterAsync()` metod med mallarna på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2f64c-454">To register templates, use the `RegisterAsync()` method with the templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="2f64c-455">Dina mallar ska vara `JObject` datatyper och kan innehålla flera mallar i följande JSON-format:</span><span class="sxs-lookup"><span data-stu-id="2f64c-455">Your templates should be `JObject` types and can contain multiple templates in the following JSON format:</span></span>

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

<span data-ttu-id="2f64c-456">Metoden **RegisterAsync()** sekundära paneler kan också användas:</span><span class="sxs-lookup"><span data-stu-id="2f64c-456">The method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="2f64c-457">Alla taggar tas bort direkt under registreringen för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="2f64c-457">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="2f64c-458">Om du vill lägga till taggar i installationer eller mallar inom installationer finns i [arbeta med serverdelen .NET SDK för Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="2f64c-458">To add tags to installations or templates within installations, see [Work with the .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="2f64c-459">Om du vill skicka meddelanden genom att använda mallarna registrerade, referera till den [Notification Hubs API: er].</span><span class="sxs-lookup"><span data-stu-id="2f64c-459">To send notifications utilizing these registered templates, refer to the [Notification Hubs APIs].</span></span>

## <span data-ttu-id="2f64c-460"><a name="misc"></a>Diverse avsnitt</span><span class="sxs-lookup"><span data-stu-id="2f64c-460"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="2f64c-461"><a name="errors"></a>Så här: hantera fel</span><span class="sxs-lookup"><span data-stu-id="2f64c-461"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="2f64c-462">När ett fel uppstår i serverdelen kan klienten SDK genererar en `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="2f64c-462">When an error occurs in the backend, the client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="2f64c-463">I följande exempel visas hur du hanterar ett undantag som returneras av serverdelen:</span><span class="sxs-lookup"><span data-stu-id="2f64c-463">The following example shows how to handle an exception that is returned by the backend:</span></span>

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
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

<span data-ttu-id="2f64c-464">Ett annat exempel för att behandla felvillkor kan hittas i den [Mobile Apps filer exempel].</span><span class="sxs-lookup"><span data-stu-id="2f64c-464">Another example of dealing with error conditions can be found in the [Mobile Apps Files Sample].</span></span> <span data-ttu-id="2f64c-465">Den [LoggingHandler] exempel ger en loggning ombud hanterare för att logga begäranden som görs till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-465">The [LoggingHandler] example provides a logging delegate handler to log the requests being made to the backend.</span></span>

### <span data-ttu-id="2f64c-466"><a name="headers"></a>Så här: anpassa begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="2f64c-466"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="2f64c-467">För att stödja specifika app-scenariot, kan du behöva anpassa kommunikationen med serverdelen för Mobilappen.</span><span class="sxs-lookup"><span data-stu-id="2f64c-467">To support your specific app scenario, you might need to customize communication with the Mobile App backend.</span></span> <span data-ttu-id="2f64c-468">Du kanske vill lägga till en anpassad rubrik i varje utgående begäran eller även ändra svar statuskoder.</span><span class="sxs-lookup"><span data-stu-id="2f64c-468">For example, you may want to add a custom header to every outgoing request or even change responses status codes.</span></span> <span data-ttu-id="2f64c-469">Du kan använda en anpassad [DelegatingHandler], som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2f64c-469">You can use a custom [DelegatingHandler], as in the following example:</span></span>

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
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
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

<span data-ttu-id="2f64c-470">[Lägg till autentisering till din app]: app-service-mobile-windows-store-dotnet-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="2f64c-470">[Add authentication to your app]: app-service-mobile-windows-store-dotnet-get-started-users.md</span></span>
<span data-ttu-id="2f64c-471">[Offline datasynkronisering i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="2f64c-471">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="2f64c-472">[Lägg till push-meddelanden i appen]: app-service-mobile-windows-store-dotnet-get-started-push.md</span><span class="sxs-lookup"><span data-stu-id="2f64c-472">[Add push notifications to your app]: app-service-mobile-windows-store-dotnet-get-started-push.md</span></span>
<span data-ttu-id="2f64c-473">[registrera din app om du vill använda en Microsoft-kontoinloggning]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="2f64c-473">[Register your app to use a Microsoft account login]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="2f64c-474">[konfigurera App Service för Active Directory-inloggningen]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="2f64c-474">[How to configure App Service for Active Directory login]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>

<!-- Microsoft URLs. -->
<span data-ttu-id="2f64c-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-479">[ MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-479">[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-480">[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-480">[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-481">[skapar en referens till en ej typbestämd tabell]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-481">[creates a reference to an untyped table]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-491">[ta]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-491">[Take]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-492">[Välj]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-492">[Select]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-493">[hoppa över]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-493">[Skip]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-494">[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-494">[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span></span>
<span data-ttu-id="2f64c-495">[Användar-ID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-495">[UserID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-496">[där]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-496">[Where]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span></span>
<span data-ttu-id="2f64c-497">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="2f64c-497">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="2f64c-498">[klassiska Azure-portalen]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="2f64c-498">[Azure classic portal]: https://manage.windowsazure.com/</span></span>
<span data-ttu-id="2f64c-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span></span>
<span data-ttu-id="2f64c-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span></span>
<span data-ttu-id="2f64c-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span></span>
<span data-ttu-id="2f64c-502">[Windows Dev Center]: https://dev.windows.com/en-us/overview</span><span class="sxs-lookup"><span data-stu-id="2f64c-502">[Windows Dev Center]: https://dev.windows.com/en-us/overview</span></span>
<span data-ttu-id="2f64c-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span></span>
<span data-ttu-id="2f64c-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span></span>
<span data-ttu-id="2f64c-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span></span>
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
<span data-ttu-id="2f64c-506">[Notification Hubs API: er]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span><span class="sxs-lookup"><span data-stu-id="2f64c-506">[Notification Hubs APIs]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span></span>
<span data-ttu-id="2f64c-507">[Mobile Apps filer exempel]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span><span class="sxs-lookup"><span data-stu-id="2f64c-507">[Mobile Apps Files Sample]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span></span>
<span data-ttu-id="2f64c-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span><span class="sxs-lookup"><span data-stu-id="2f64c-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span></span>

<!-- External URLs -->
<span data-ttu-id="2f64c-509">[OData v3 dokumentationen]: http://www.odata.org/documentation/odata-version-3-0/</span><span class="sxs-lookup"><span data-stu-id="2f64c-509">[OData v3 Documentation]: http://www.odata.org/documentation/odata-version-3-0/</span></span>
<span data-ttu-id="2f64c-510">[Fiddler]: http://www.telerik.com/fiddler</span><span class="sxs-lookup"><span data-stu-id="2f64c-510">[Fiddler]: http://www.telerik.com/fiddler</span></span>
<span data-ttu-id="2f64c-511">[Json.NET]: http://www.newtonsoft.com/json</span><span class="sxs-lookup"><span data-stu-id="2f64c-511">[Json.NET]: http://www.newtonsoft.com/json</span></span>
<span data-ttu-id="2f64c-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span><span class="sxs-lookup"><span data-stu-id="2f64c-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span></span>
<span data-ttu-id="2f64c-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span><span class="sxs-lookup"><span data-stu-id="2f64c-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span></span>
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
