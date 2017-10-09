---
title: "aaaRow säkerhet med Power BI Embedded"
description: "Information om säkerhet på radnivå med Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 384f78826ecc710cf8f101b251ae68b074f3e98b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a><span data-ttu-id="953fa-103">Säkerhet på radnivå med Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="953fa-103">Row level security with Power BI Embedded</span></span>

<span data-ttu-id="953fa-104">Säkerhet på radnivå (RLS) kan vara används toorestrict åtkomst tooparticular användardata inom en rapport eller dataset, vilket gör att för flera olika användare toouse hello samma rapport när alla se olika data.</span><span class="sxs-lookup"><span data-stu-id="953fa-104">Row level security (RLS) can be used toorestrict user access tooparticular data within a report or dataset, allowing for multiple different users toouse hello same report while all seeing different data.</span></span> <span data-ttu-id="953fa-105">Power BI Embedded stöder nu datauppsättningar som konfigurerats med RLS.</span><span class="sxs-lookup"><span data-stu-id="953fa-105">Power BI Embedded now supports datasets configured with RLS.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

<span data-ttu-id="953fa-106">I ordning tootake nytta av RLS är det viktigt att du förstår tre huvudkoncepten; Användare, roller och regler.</span><span class="sxs-lookup"><span data-stu-id="953fa-106">In order tootake advantage of RLS, it’s important you understand three main concepts; Users, Roles, and Rules.</span></span> <span data-ttu-id="953fa-107">Nu ska vi titta närmare på varje:</span><span class="sxs-lookup"><span data-stu-id="953fa-107">Let’s take a closer look at each:</span></span>

<span data-ttu-id="953fa-108">**Användare** – dessa hello faktiska slutanvändare visar rapporter.</span><span class="sxs-lookup"><span data-stu-id="953fa-108">**Users** –  These are hello actual end-users viewing reports.</span></span> <span data-ttu-id="953fa-109">I Power BI Embedded identifieras användare genom hello användarnamn egenskap i en Apptoken.</span><span class="sxs-lookup"><span data-stu-id="953fa-109">In Power BI Embedded, users are identified by hello username property in an App Token.</span></span>

<span data-ttu-id="953fa-110">**Roller** – användarna tillhör tooroles.</span><span class="sxs-lookup"><span data-stu-id="953fa-110">**Roles** – Users belong tooroles.</span></span> <span data-ttu-id="953fa-111">En roll är en behållare för regler och kan vara ett namn som ”försäljningschef” eller ”säljare”.</span><span class="sxs-lookup"><span data-stu-id="953fa-111">A role is a container for rules and can be named something like “Sales Manager” or “Sales Rep”.</span></span> <span data-ttu-id="953fa-112">I Power BI Embedded identifieras användare genom hello roller egenskap i en Apptoken.</span><span class="sxs-lookup"><span data-stu-id="953fa-112">In Power BI Embedded, users are identified by hello roles property in an App Token.</span></span>

<span data-ttu-id="953fa-113">**Regler** – roller har regler och dessa regler är hello faktiska filter som ska tillämpas toobe toohello data.</span><span class="sxs-lookup"><span data-stu-id="953fa-113">**Rules** – Roles have rules, and those rules are hello actual filters that are going toobe applied toohello data.</span></span> <span data-ttu-id="953fa-114">Detta kan vara så enkelt som ”land = USA” eller något mycket mer dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="953fa-114">This could be as simple as “Country = USA” or something much more dynamic.</span></span>

### <a name="example"></a><span data-ttu-id="953fa-115">Exempel</span><span class="sxs-lookup"><span data-stu-id="953fa-115">Example</span></span>

<span data-ttu-id="953fa-116">Hello resten av den här artikeln, kommer vi ger ett exempel på redigering RLS och förbrukar som ett inbäddat program.</span><span class="sxs-lookup"><span data-stu-id="953fa-116">For hello rest of this article, we’ll provide an example of authoring RLS, and then consuming that within an embedded application.</span></span> <span data-ttu-id="953fa-117">Vårt exempel använder hello [Retail analys exempel](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fil.</span><span class="sxs-lookup"><span data-stu-id="953fa-117">Our example uses hello [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX file.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

<span data-ttu-id="953fa-118">Exemplet Retail analys visar försäljning för alla hello butiker i en viss kedja.</span><span class="sxs-lookup"><span data-stu-id="953fa-118">Our Retail Analysis sample shows sales for all hello stores in a particular retail chain.</span></span> <span data-ttu-id="953fa-119">Utan RLS, oavsett vilken distrikt manager loggar in och vyer hello rapporten, ser hello samma data.</span><span class="sxs-lookup"><span data-stu-id="953fa-119">Without RLS, no matter which district manager signs in and views hello report, they’ll see hello same data.</span></span> <span data-ttu-id="953fa-120">Företagsledning har fastställt varje distrikt manager bör endast visas hello sales för hello lagrar de hanterar och toodo kan vi använda RLS.</span><span class="sxs-lookup"><span data-stu-id="953fa-120">Senior management has determined each district manager should only see hello sales for hello stores they manage, and toodo this, we can use RLS.</span></span>

<span data-ttu-id="953fa-121">RLS har skrivits i Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="953fa-121">RLS is authored in Power BI Desktop.</span></span> <span data-ttu-id="953fa-122">När hello datasetet och rapporten öppnas, växel vi toodiagram visa toosee hello schema:</span><span class="sxs-lookup"><span data-stu-id="953fa-122">When hello dataset and report are opened, we can switch toodiagram view toosee hello schema:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

<span data-ttu-id="953fa-123">Här följer några saker toonotice med schemat:</span><span class="sxs-lookup"><span data-stu-id="953fa-123">Here are a few things toonotice with this schema:</span></span>

* <span data-ttu-id="953fa-124">Alla åtgärder som **totalförsäljning**, lagras i hello **försäljning** faktatabell.</span><span class="sxs-lookup"><span data-stu-id="953fa-124">All measures, like **Total Sales**, are stored in hello **Sales** fact table.</span></span>
* <span data-ttu-id="953fa-125">Det finns fyra ytterligare relaterade dimensionstabeller: **objektet**, **tid**, **Store**, och **distrikt**.</span><span class="sxs-lookup"><span data-stu-id="953fa-125">There are four additional related dimension tables: **Item**, **Time**, **Store**, and **District**.</span></span>
* <span data-ttu-id="953fa-126">hello pilar på hello relationen rader visar hur filter kan flöda från en tabell tooanother.</span><span class="sxs-lookup"><span data-stu-id="953fa-126">hello arrows on hello relationship lines indicate which way filters can flow from one table tooanother.</span></span> <span data-ttu-id="953fa-127">Om exempelvis ett filter är placerad **tid [Date]**, i hello aktuella schemat den bara filtrerar ned värdena i hello **försäljning** tabell.</span><span class="sxs-lookup"><span data-stu-id="953fa-127">For example, if a filter is placed on **Time[Date]**, in hello current schema it would only filter down values in hello **Sales** table.</span></span> <span data-ttu-id="953fa-128">Inga andra tabeller kan påverkas av det här filtret eftersom alla hello pilarna på hello relationen rader punkt toohello försäljning tabell och inte direkt.</span><span class="sxs-lookup"><span data-stu-id="953fa-128">No other tables would be affected by this filter since all of hello arrows on hello relationship lines point toohello sales table and not away.</span></span>
* <span data-ttu-id="953fa-129">Hej **distrikt** tabellen anger som hello manager för varje distrikt:</span><span class="sxs-lookup"><span data-stu-id="953fa-129">hello **District** table indicates who hello manager is for each district:</span></span>
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

<span data-ttu-id="953fa-130">Baserat på det här schemat om vi tillämpa ett filter toohello **distrikt Manager** kolumn i hello distrikt tabell och om filtret matchar hello användare som visar hello rapporten, filtret också filtrera ned hello **Store**och **försäljning** tabeller tooonly visa data för det specifika district manager.</span><span class="sxs-lookup"><span data-stu-id="953fa-130">Based on this schema, if we apply a filter toohello **District Manager** column in hello District table, and if that filter matches hello user viewing hello report, that filter will also filter down hello **Store** and **Sales** tables tooonly show data for that particular district manager.</span></span>

<span data-ttu-id="953fa-131">Här är hur:</span><span class="sxs-lookup"><span data-stu-id="953fa-131">Here’s how:</span></span>

1. <span data-ttu-id="953fa-132">Klicka på fliken modellering hello **hantera roller**.</span><span class="sxs-lookup"><span data-stu-id="953fa-132">On hello Modeling tab, click **Manage Roles**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. <span data-ttu-id="953fa-133">Skapa en ny roll som kallas **Manager**.</span><span class="sxs-lookup"><span data-stu-id="953fa-133">Create a new role called **Manager**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. <span data-ttu-id="953fa-134">I hello **distrikt** tabell ange hello följande DAX-uttryck: **[distrikt Manager] = USERNAME()**</span><span class="sxs-lookup"><span data-stu-id="953fa-134">In hello **District** table enter hello following DAX expression: **[District Manager] = USERNAME()**</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. <span data-ttu-id="953fa-135">toomake att hello regler arbetar på hello **Modeling** klickar du på **vyn som roller**, och ange sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="953fa-135">toomake sure hello rules are working, on hello **Modeling** tab, click **View as Roles**, and then enter hello following:</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   <span data-ttu-id="953fa-136">hello rapporter ska nu visa data som om du har loggat in som **Andrew Ma**.</span><span class="sxs-lookup"><span data-stu-id="953fa-136">hello reports will now show data as if you were signed in as **Andrew Ma**.</span></span>

<span data-ttu-id="953fa-137">Använda hello filtret, hello sätt som vi gjorde här, filtrerar ner alla poster i hello **distrikt**, **Store**, och **försäljning** tabeller.</span><span class="sxs-lookup"><span data-stu-id="953fa-137">Applying hello filter, hello way we did here, will filter down all records in hello **District**, **Store**, and **Sales** tables.</span></span> <span data-ttu-id="953fa-138">Emellertid hello filterriktningen på hello relationer mellan **försäljning** och **tid**, **försäljning** och **objektet**, och **Objektet** och **tid** tabeller filtreras inte ned.</span><span class="sxs-lookup"><span data-stu-id="953fa-138">However, because of hello filter direction on hello relationships between **Sales** and **Time**, **Sales** and **Item**, and **Item** and **Time** tables will not be filtered down.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

<span data-ttu-id="953fa-139">Som kan vara ok för det här kravet, men om vi inte vill chefer toosee objekt som de inte har någon försäljning, vi kan aktivera dubbelriktad mellan filtrering för hello relationen och flödet hello säkerhetsfilter i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="953fa-139">That may be ok for this requirement, however, if we don’t want managers toosee items for which they don’t have any sales, we could turn on bidirectional cross-filtering for hello relationship and flow hello security filter in both directions.</span></span> <span data-ttu-id="953fa-140">Detta kan göras genom att redigera hello förhållandet mellan **försäljning** och **objektet**, så här:</span><span class="sxs-lookup"><span data-stu-id="953fa-140">This can be done by editing hello relationship between **Sales** and **Item**, like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

<span data-ttu-id="953fa-141">Filter kan nu också flöda från hello försäljning tabell toohello **objektet** tabell:</span><span class="sxs-lookup"><span data-stu-id="953fa-141">Now, filters can also flow from hello Sales table toohello **Item** table:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> <span data-ttu-id="953fa-142">Om du använder DirectQuery-läge för dina data behöver tooenable dubbelriktad-mellan filtrering genom att välja de här två alternativen:</span><span class="sxs-lookup"><span data-stu-id="953fa-142">If you're using DirectQuery mode for your data, you will need tooenable bidirectional-cross filtering by selecting these two options:</span></span>

1. <span data-ttu-id="953fa-143">**Filen** -> **alternativ och inställningar** -> **förhandsgranskningsfunktioner** -> **aktivera korsfiltrering i båda riktningarna för DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="953fa-143">**File** -> **Options and Settings** -> **Preview Features** -> **Enable cross filtering in both directions for DirectQuery**.</span></span>
2. <span data-ttu-id="953fa-144">**Filen** -> **alternativ och inställningar** -> **DirectQuery** -> **tillåta obegränsad mått i DirectQuery-läge**.</span><span class="sxs-lookup"><span data-stu-id="953fa-144">**File** -> **Options and Settings** -> **DirectQuery** -> **Allow unrestricted measure in DirectQuery mode**.</span></span>

<span data-ttu-id="953fa-145">Mer om dubbelriktad mellan filtrering, hämta hello toolearn [dubbelriktad mellan filtrering i SQL Server Analysis Services 2016 och Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) vitbok.</span><span class="sxs-lookup"><span data-stu-id="953fa-145">toolearn more about bidirectional cross-filtering, download hello [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper.</span></span>

<span data-ttu-id="953fa-146">Detta packar upp alla hello arbete som måste göras i Power BI Desktop toobe, men det finns en mer arbete som behöver toobe klar toomake hello RLS regler vi har definierat arbete i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="953fa-146">This wraps up all hello work that needs toobe done in Power BI Desktop, but there’s one more piece of work that needs toobe done toomake hello RLS rules we defined work in Power BI Embedded.</span></span> <span data-ttu-id="953fa-147">Användare autentiseras och auktoriseras av ditt program och apptoken är används toogrant användaren åtkomst tooa specifika Power BI Embedded rapporten.</span><span class="sxs-lookup"><span data-stu-id="953fa-147">Users are authenticated and authorized by your application and App tokens are used toogrant that user access tooa specific Power BI Embedded report.</span></span> <span data-ttu-id="953fa-148">Power BI Embedded har inte någon särskild information som användaren har.</span><span class="sxs-lookup"><span data-stu-id="953fa-148">Power BI Embedded doesn’t have any specific information on who your user is.</span></span> <span data-ttu-id="953fa-149">För RLS toowork behöver du toopass vissa ytterligare kontext som en del av din apptoken:</span><span class="sxs-lookup"><span data-stu-id="953fa-149">For RLS toowork, you’ll need toopass some additional context as part of your app token:</span></span>

* <span data-ttu-id="953fa-150">**användarnamnet** (valfritt) – används med RLS. Detta är en sträng som du kan använda toohelp identifiera hello användare när du använder RLS regler.</span><span class="sxs-lookup"><span data-stu-id="953fa-150">**username** (optional) – Used with RLS this is a string that can be used toohelp identify hello user when applying RLS rules.</span></span> <span data-ttu-id="953fa-151">Se använder raden säkerhet på radnivå med Powerbi Embedded</span><span class="sxs-lookup"><span data-stu-id="953fa-151">See Using Row Level Security with Power BI Embedded</span></span>
* <span data-ttu-id="953fa-152">**roller** – en sträng som innehåller hello roller tooselect när du använder regler för säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="953fa-152">**roles** – A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="953fa-153">Om skicka fler än en roll ska de skickas som en strängmatris.</span><span class="sxs-lookup"><span data-stu-id="953fa-153">If passing more than one role, they should be passed as a string array.</span></span>

<span data-ttu-id="953fa-154">Du skapar hello token med hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metod.</span><span class="sxs-lookup"><span data-stu-id="953fa-154">You create hello token by using hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) method.</span></span> <span data-ttu-id="953fa-155">Om hello användarnamnsegenskapen finns måste du också ange minst ett värde i roller.</span><span class="sxs-lookup"><span data-stu-id="953fa-155">If hello username property is present, you must also pass at least one value in roles.</span></span>

<span data-ttu-id="953fa-156">Du kan till exempel ändra hello EmbedSample.</span><span class="sxs-lookup"><span data-stu-id="953fa-156">For example, you could change hello EmbedSample.</span></span> <span data-ttu-id="953fa-157">DashboardController rad 55 kunde uppdateras från</span><span class="sxs-lookup"><span data-stu-id="953fa-157">DashboardController line 55 could be updated from</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

<span data-ttu-id="953fa-158">till</span><span class="sxs-lookup"><span data-stu-id="953fa-158">to</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

<span data-ttu-id="953fa-159">hello fullständig apptoken ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="953fa-159">hello full app token will look something like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

<span data-ttu-id="953fa-160">Nu, med alla hello delar tillsammans, när någon loggar in på våra program tooview den här rapporten kan användarna bara ska kan toosee hello data att de tillåts toosee, som definieras av våra säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="953fa-160">Now, with all hello pieces together, when someone logs into our application tooview this report, they’ll only be able toosee hello data that they are allowed toosee, as defined by our row-level security.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a><span data-ttu-id="953fa-161">Se även</span><span class="sxs-lookup"><span data-stu-id="953fa-161">See also</span></span>

[<span data-ttu-id="953fa-162">Säkerhet på radnivå (RLS) med kraften</span><span class="sxs-lookup"><span data-stu-id="953fa-162">Row-level security (RLS) with Power</span></span>](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[<span data-ttu-id="953fa-163">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="953fa-163">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="953fa-164">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="953fa-164">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="953fa-165">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="953fa-165">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="953fa-166">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="953fa-166">More questions?</span></span> [<span data-ttu-id="953fa-167">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="953fa-167">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

