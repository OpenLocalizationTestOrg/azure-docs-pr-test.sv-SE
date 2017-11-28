---
title: "Säkerhet på radnivå med Power BI Embedded"
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
ms.openlocfilehash: 1cde5b9ee4c716af07d427d4d0eb3f0775d456ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a><span data-ttu-id="80819-103">Säkerhet på radnivå med Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="80819-103">Row level security with Power BI Embedded</span></span>

<span data-ttu-id="80819-104">Säkerhet på radnivå (RLS) kan användas för att begränsa användaråtkomst till vissa uppgifter inom en rapport eller dataset, vilket gör att flera olika användare kan använda samma rapporten när alla se olika data.</span><span class="sxs-lookup"><span data-stu-id="80819-104">Row level security (RLS) can be used to restrict user access to particular data within a report or dataset, allowing for multiple different users to use the same report while all seeing different data.</span></span> <span data-ttu-id="80819-105">Power BI Embedded stöder nu datauppsättningar som konfigurerats med RLS.</span><span class="sxs-lookup"><span data-stu-id="80819-105">Power BI Embedded now supports datasets configured with RLS.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

<span data-ttu-id="80819-106">För att kunna dra nytta av RLS, är det viktigt att du förstår tre huvudkoncepten; Användare, roller och regler.</span><span class="sxs-lookup"><span data-stu-id="80819-106">In order to take advantage of RLS, it’s important you understand three main concepts; Users, Roles, and Rules.</span></span> <span data-ttu-id="80819-107">Nu ska vi titta närmare på varje:</span><span class="sxs-lookup"><span data-stu-id="80819-107">Let’s take a closer look at each:</span></span>

<span data-ttu-id="80819-108">**Användare** – dessa faktiska slutanvändarna visar rapporter.</span><span class="sxs-lookup"><span data-stu-id="80819-108">**Users** –  These are the actual end-users viewing reports.</span></span> <span data-ttu-id="80819-109">I Power BI Embedded identifieras användare av egenskapen username i en App-Token.</span><span class="sxs-lookup"><span data-stu-id="80819-109">In Power BI Embedded, users are identified by the username property in an App Token.</span></span>

<span data-ttu-id="80819-110">**Roller** – användare som hör till roller.</span><span class="sxs-lookup"><span data-stu-id="80819-110">**Roles** – Users belong to roles.</span></span> <span data-ttu-id="80819-111">En roll är en behållare för regler och kan vara ett namn som ”försäljningschef” eller ”säljare”.</span><span class="sxs-lookup"><span data-stu-id="80819-111">A role is a container for rules and can be named something like “Sales Manager” or “Sales Rep”.</span></span> <span data-ttu-id="80819-112">I Power BI Embedded identifieras användare av egenskapen roller i en App-Token.</span><span class="sxs-lookup"><span data-stu-id="80819-112">In Power BI Embedded, users are identified by the roles property in an App Token.</span></span>

<span data-ttu-id="80819-113">**Regler** – roller har regler och dessa regler är de faktiska filter som ska tillämpas på data.</span><span class="sxs-lookup"><span data-stu-id="80819-113">**Rules** – Roles have rules, and those rules are the actual filters that are going to be applied to the data.</span></span> <span data-ttu-id="80819-114">Detta kan vara så enkelt som ”land = USA” eller något mycket mer dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="80819-114">This could be as simple as “Country = USA” or something much more dynamic.</span></span>

### <a name="example"></a><span data-ttu-id="80819-115">Exempel</span><span class="sxs-lookup"><span data-stu-id="80819-115">Example</span></span>

<span data-ttu-id="80819-116">Vi att ge ett exempel på redigering RLS och förbrukar som ett inbäddat program för resten av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="80819-116">For the rest of this article, we’ll provide an example of authoring RLS, and then consuming that within an embedded application.</span></span> <span data-ttu-id="80819-117">Vårt exempel använder den [Retail analys exempel](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fil.</span><span class="sxs-lookup"><span data-stu-id="80819-117">Our example uses the [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX file.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

<span data-ttu-id="80819-118">Exemplet Retail analys visar försäljning för alla butiker i en viss kedja.</span><span class="sxs-lookup"><span data-stu-id="80819-118">Our Retail Analysis sample shows sales for all the stores in a particular retail chain.</span></span> <span data-ttu-id="80819-119">Utan RLS, oavsett vilken distrikt manager loggar in och visar rapporten visas de samma data.</span><span class="sxs-lookup"><span data-stu-id="80819-119">Without RLS, no matter which district manager signs in and views the report, they’ll see the same data.</span></span> <span data-ttu-id="80819-120">Företagsledning har fastställt varje distrikt manager bör bara se försäljning för butiker de hanterar och för att göra detta, kan vi använda RLS.</span><span class="sxs-lookup"><span data-stu-id="80819-120">Senior management has determined each district manager should only see the sales for the stores they manage, and to do this, we can use RLS.</span></span>

<span data-ttu-id="80819-121">RLS har skrivits i Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="80819-121">RLS is authored in Power BI Desktop.</span></span> <span data-ttu-id="80819-122">När datasetet och rapporten öppnas, växla vi till diagramvy Se schemat:</span><span class="sxs-lookup"><span data-stu-id="80819-122">When the dataset and report are opened, we can switch to diagram view to see the schema:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

<span data-ttu-id="80819-123">Här följer några saker att Observera med schemat:</span><span class="sxs-lookup"><span data-stu-id="80819-123">Here are a few things to notice with this schema:</span></span>

* <span data-ttu-id="80819-124">Alla åtgärder som **totalförsäljning**, lagras i den **försäljning** faktatabell.</span><span class="sxs-lookup"><span data-stu-id="80819-124">All measures, like **Total Sales**, are stored in the **Sales** fact table.</span></span>
* <span data-ttu-id="80819-125">Det finns fyra ytterligare relaterade dimensionstabeller: **objektet**, **tid**, **Store**, och **distrikt**.</span><span class="sxs-lookup"><span data-stu-id="80819-125">There are four additional related dimension tables: **Item**, **Time**, **Store**, and **District**.</span></span>
* <span data-ttu-id="80819-126">Pilar på relationen raderna visar hur filter kan flöda från en tabell till en annan.</span><span class="sxs-lookup"><span data-stu-id="80819-126">The arrows on the relationship lines indicate which way filters can flow from one table to another.</span></span> <span data-ttu-id="80819-127">Om exempelvis ett filter är placerad **tid [Date]**, i det aktuella schemat den bara filtrerar ned värdena i den **försäljning** tabell.</span><span class="sxs-lookup"><span data-stu-id="80819-127">For example, if a filter is placed on **Time[Date]**, in the current schema it would only filter down values in the **Sales** table.</span></span> <span data-ttu-id="80819-128">Inga andra tabeller kan påverkas av det här filtret eftersom alla pilarna på raderna relationen pekar tabellen försäljning och inte direkt.</span><span class="sxs-lookup"><span data-stu-id="80819-128">No other tables would be affected by this filter since all of the arrows on the relationship lines point to the sales table and not away.</span></span>
* <span data-ttu-id="80819-129">Den **distrikt** tabellen anger som hanteraren för varje distrikt:</span><span class="sxs-lookup"><span data-stu-id="80819-129">The **District** table indicates who the manager is for each district:</span></span>
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

<span data-ttu-id="80819-130">Baserat på det här schemat om vi använda ett filter på den **distrikt Manager** kolumnen i tabellen Distrikt och om filtret matchar användare som visar rapporten, filtret också filtrera ned den **Store** och  **Försäljning** tabeller för att endast visa data för det specifika district manager.</span><span class="sxs-lookup"><span data-stu-id="80819-130">Based on this schema, if we apply a filter to the **District Manager** column in the District table, and if that filter matches the user viewing the report, that filter will also filter down the **Store** and **Sales** tables to only show data for that particular district manager.</span></span>

<span data-ttu-id="80819-131">Här är hur:</span><span class="sxs-lookup"><span data-stu-id="80819-131">Here’s how:</span></span>

1. <span data-ttu-id="80819-132">På fliken modellering **hantera roller**.</span><span class="sxs-lookup"><span data-stu-id="80819-132">On the Modeling tab, click **Manage Roles**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. <span data-ttu-id="80819-133">Skapa en ny roll som kallas **Manager**.</span><span class="sxs-lookup"><span data-stu-id="80819-133">Create a new role called **Manager**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. <span data-ttu-id="80819-134">I den **distrikt** tabellen anger du följande DAX-uttryck: **[distrikt Manager] = USERNAME()**</span><span class="sxs-lookup"><span data-stu-id="80819-134">In the **District** table enter the following DAX expression: **[District Manager] = USERNAME()**</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. <span data-ttu-id="80819-135">Se till att reglerna som arbetar på den **Modeling** klickar du på **vyn som roller**, och ange sedan följande:</span><span class="sxs-lookup"><span data-stu-id="80819-135">To make sure the rules are working, on the **Modeling** tab, click **View as Roles**, and then enter the following:</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   <span data-ttu-id="80819-136">Rapporterna kommer nu att visa data som om du har loggat in som **Andrew Ma**.</span><span class="sxs-lookup"><span data-stu-id="80819-136">The reports will now show data as if you were signed in as **Andrew Ma**.</span></span>

<span data-ttu-id="80819-137">Tillämpa filtret, hur vi gjorde här, filtrerar ner alla poster i den **distrikt**, **Store**, och **försäljning** tabeller.</span><span class="sxs-lookup"><span data-stu-id="80819-137">Applying the filter, the way we did here, will filter down all records in the **District**, **Store**, and **Sales** tables.</span></span> <span data-ttu-id="80819-138">Men på grund av filterriktningen på relationerna mellan **försäljning** och **tid**, **försäljning** och **objektet**, och **Objektet** och **tid** tabeller filtreras inte ned.</span><span class="sxs-lookup"><span data-stu-id="80819-138">However, because of the filter direction on the relationships between **Sales** and **Time**, **Sales** and **Item**, and **Item** and **Time** tables will not be filtered down.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

<span data-ttu-id="80819-139">Som kan vara ok för det här kravet, om vi inte vill chefer att visa objekt som de inte har någon försäljning, vi kan aktivera dubbelriktad cross-filtrering för relationen och flöda säkerhetsfilter i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="80819-139">That may be ok for this requirement, however, if we don’t want managers to see items for which they don’t have any sales, we could turn on bidirectional cross-filtering for the relationship and flow the security filter in both directions.</span></span> <span data-ttu-id="80819-140">Detta kan göras genom att redigera relationen mellan **försäljning** och **objektet**, så här:</span><span class="sxs-lookup"><span data-stu-id="80819-140">This can be done by editing the relationship between **Sales** and **Item**, like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

<span data-ttu-id="80819-141">Filter kan nu också flöda från tabellen försäljning till den **objektet** tabell:</span><span class="sxs-lookup"><span data-stu-id="80819-141">Now, filters can also flow from the Sales table to the **Item** table:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> <span data-ttu-id="80819-142">Om du använder DirectQuery-läge för dina data, behöver du aktivera dubbelriktad mellan filtrering genom att välja de här två alternativen:</span><span class="sxs-lookup"><span data-stu-id="80819-142">If you're using DirectQuery mode for your data, you will need to enable bidirectional-cross filtering by selecting these two options:</span></span>

1. <span data-ttu-id="80819-143">**Filen** -> **alternativ och inställningar** -> **förhandsgranskningsfunktioner** -> **aktivera korsfiltrering i båda riktningarna för DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="80819-143">**File** -> **Options and Settings** -> **Preview Features** -> **Enable cross filtering in both directions for DirectQuery**.</span></span>
2. <span data-ttu-id="80819-144">**Filen** -> **alternativ och inställningar** -> **DirectQuery** -> **tillåta obegränsad mått i DirectQuery-läge**.</span><span class="sxs-lookup"><span data-stu-id="80819-144">**File** -> **Options and Settings** -> **DirectQuery** -> **Allow unrestricted measure in DirectQuery mode**.</span></span>

<span data-ttu-id="80819-145">Om du vill veta mer om dubbelriktad mellan filtrering kan hämta den [dubbelriktad mellan filtrering i SQL Server Analysis Services 2016 och Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) vitbok.</span><span class="sxs-lookup"><span data-stu-id="80819-145">To learn more about bidirectional cross-filtering, download the [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper.</span></span>

<span data-ttu-id="80819-146">Detta packar upp allt arbete som måste göras i Power BI Desktop, men det finns en mer arbete som måste göras för att göra RLS regler vi har definierat arbete i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="80819-146">This wraps up all the work that needs to be done in Power BI Desktop, but there’s one more piece of work that needs to be done to make the RLS rules we defined work in Power BI Embedded.</span></span> <span data-ttu-id="80819-147">Användare autentiseras och auktoriseras av ditt program och apptoken som används för att bevilja den användare åtkomsten till en viss Power BI Embedded rapport.</span><span class="sxs-lookup"><span data-stu-id="80819-147">Users are authenticated and authorized by your application and App tokens are used to grant that user access to a specific Power BI Embedded report.</span></span> <span data-ttu-id="80819-148">Power BI Embedded har inte någon särskild information som användaren har.</span><span class="sxs-lookup"><span data-stu-id="80819-148">Power BI Embedded doesn’t have any specific information on who your user is.</span></span> <span data-ttu-id="80819-149">För RLS ska fungera behöver skicka vissa ytterligare kontext som en del av din apptoken:</span><span class="sxs-lookup"><span data-stu-id="80819-149">For RLS to work, you’ll need to pass some additional context as part of your app token:</span></span>

* <span data-ttu-id="80819-150">**användarnamnet** (valfritt) – används med RLS. Detta är en sträng som kan användas för att identifiera användaren när du använder RLS regler.</span><span class="sxs-lookup"><span data-stu-id="80819-150">**username** (optional) – Used with RLS this is a string that can be used to help identify the user when applying RLS rules.</span></span> <span data-ttu-id="80819-151">Se använder raden säkerhet på radnivå med Powerbi Embedded</span><span class="sxs-lookup"><span data-stu-id="80819-151">See Using Row Level Security with Power BI Embedded</span></span>
* <span data-ttu-id="80819-152">**roller** – en sträng som innehåller rollerna kan välja vid tillämpning av säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="80819-152">**roles** – A string containing the roles to select when applying Row Level Security rules.</span></span> <span data-ttu-id="80819-153">Om skicka fler än en roll ska de skickas som en strängmatris.</span><span class="sxs-lookup"><span data-stu-id="80819-153">If passing more than one role, they should be passed as a string array.</span></span>

<span data-ttu-id="80819-154">Du skapar en token med den [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metod.</span><span class="sxs-lookup"><span data-stu-id="80819-154">You create the token by using the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) method.</span></span> <span data-ttu-id="80819-155">Om egenskapen username finns måste du också ange minst ett värde i roller.</span><span class="sxs-lookup"><span data-stu-id="80819-155">If the username property is present, you must also pass at least one value in roles.</span></span>

<span data-ttu-id="80819-156">Du kan till exempel ändra EmbedSample.</span><span class="sxs-lookup"><span data-stu-id="80819-156">For example, you could change the EmbedSample.</span></span> <span data-ttu-id="80819-157">DashboardController rad 55 kunde uppdateras från</span><span class="sxs-lookup"><span data-stu-id="80819-157">DashboardController line 55 could be updated from</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

<span data-ttu-id="80819-158">till</span><span class="sxs-lookup"><span data-stu-id="80819-158">to</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

<span data-ttu-id="80819-159">Fullständig apptoken ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="80819-159">The full app token will look something like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

<span data-ttu-id="80819-160">Nu, med alla delar tillsammans, när någon loggar in på vår programmet för att visa den här rapporten de kommer bara att kunna se de data som de ska kunna se, som definieras av våra säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="80819-160">Now, with all the pieces together, when someone logs into our application to view this report, they’ll only be able to see the data that they are allowed to see, as defined by our row-level security.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a><span data-ttu-id="80819-161">Se även</span><span class="sxs-lookup"><span data-stu-id="80819-161">See also</span></span>

[<span data-ttu-id="80819-162">Säkerhet på radnivå (RLS) med kraften</span><span class="sxs-lookup"><span data-stu-id="80819-162">Row-level security (RLS) with Power</span></span>](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[<span data-ttu-id="80819-163">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="80819-163">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="80819-164">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="80819-164">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="80819-165">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="80819-165">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="80819-166">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="80819-166">More questions?</span></span> [<span data-ttu-id="80819-167">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="80819-167">Try the Power BI Community</span></span>](http://community.powerbi.com/)

