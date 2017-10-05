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
# <a name="row-level-security-with-power-bi-embedded"></a>Säkerhet på radnivå med Power BI Embedded

Säkerhet på radnivå (RLS) kan användas för att begränsa användaråtkomst till vissa uppgifter inom en rapport eller dataset, vilket gör att flera olika användare kan använda samma rapporten när alla se olika data. Power BI Embedded stöder nu datauppsättningar som konfigurerats med RLS.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

För att kunna dra nytta av RLS, är det viktigt att du förstår tre huvudkoncepten; Användare, roller och regler. Nu ska vi titta närmare på varje:

**Användare** – dessa faktiska slutanvändarna visar rapporter. I Power BI Embedded identifieras användare av egenskapen username i en App-Token.

**Roller** – användare som hör till roller. En roll är en behållare för regler och kan vara ett namn som ”försäljningschef” eller ”säljare”. I Power BI Embedded identifieras användare av egenskapen roller i en App-Token.

**Regler** – roller har regler och dessa regler är de faktiska filter som ska tillämpas på data. Detta kan vara så enkelt som ”land = USA” eller något mycket mer dynamiskt.

### <a name="example"></a>Exempel

Vi att ge ett exempel på redigering RLS och förbrukar som ett inbäddat program för resten av den här artikeln. Vårt exempel använder den [Retail analys exempel](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fil.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

Exemplet Retail analys visar försäljning för alla butiker i en viss kedja. Utan RLS, oavsett vilken distrikt manager loggar in och visar rapporten visas de samma data. Företagsledning har fastställt varje distrikt manager bör bara se försäljning för butiker de hanterar och för att göra detta, kan vi använda RLS.

RLS har skrivits i Power BI Desktop. När datasetet och rapporten öppnas, växla vi till diagramvy Se schemat:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Här följer några saker att Observera med schemat:

* Alla åtgärder som **totalförsäljning**, lagras i den **försäljning** faktatabell.
* Det finns fyra ytterligare relaterade dimensionstabeller: **objektet**, **tid**, **Store**, och **distrikt**.
* Pilar på relationen raderna visar hur filter kan flöda från en tabell till en annan. Om exempelvis ett filter är placerad **tid [Date]**, i det aktuella schemat den bara filtrerar ned värdena i den **försäljning** tabell. Inga andra tabeller kan påverkas av det här filtret eftersom alla pilarna på raderna relationen pekar tabellen försäljning och inte direkt.
* Den **distrikt** tabellen anger som hanteraren för varje distrikt:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

Baserat på det här schemat om vi använda ett filter på den **distrikt Manager** kolumnen i tabellen Distrikt och om filtret matchar användare som visar rapporten, filtret också filtrera ned den **Store** och  **Försäljning** tabeller för att endast visa data för det specifika district manager.

Här är hur:

1. På fliken modellering **hantera roller**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Skapa en ny roll som kallas **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. I den **distrikt** tabellen anger du följande DAX-uttryck: **[distrikt Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. Se till att reglerna som arbetar på den **Modeling** klickar du på **vyn som roller**, och ange sedan följande:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   Rapporterna kommer nu att visa data som om du har loggat in som **Andrew Ma**.

Tillämpa filtret, hur vi gjorde här, filtrerar ner alla poster i den **distrikt**, **Store**, och **försäljning** tabeller. Men på grund av filterriktningen på relationerna mellan **försäljning** och **tid**, **försäljning** och **objektet**, och **Objektet** och **tid** tabeller filtreras inte ned.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Som kan vara ok för det här kravet, om vi inte vill chefer att visa objekt som de inte har någon försäljning, vi kan aktivera dubbelriktad cross-filtrering för relationen och flöda säkerhetsfilter i båda riktningarna. Detta kan göras genom att redigera relationen mellan **försäljning** och **objektet**, så här:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Filter kan nu också flöda från tabellen försäljning till den **objektet** tabell:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> Om du använder DirectQuery-läge för dina data, behöver du aktivera dubbelriktad mellan filtrering genom att välja de här två alternativen:

1. **Filen** -> **alternativ och inställningar** -> **förhandsgranskningsfunktioner** -> **aktivera korsfiltrering i båda riktningarna för DirectQuery**.
2. **Filen** -> **alternativ och inställningar** -> **DirectQuery** -> **tillåta obegränsad mått i DirectQuery-läge**.

Om du vill veta mer om dubbelriktad mellan filtrering kan hämta den [dubbelriktad mellan filtrering i SQL Server Analysis Services 2016 och Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) vitbok.

Detta packar upp allt arbete som måste göras i Power BI Desktop, men det finns en mer arbete som måste göras för att göra RLS regler vi har definierat arbete i Power BI Embedded. Användare autentiseras och auktoriseras av ditt program och apptoken som används för att bevilja den användare åtkomsten till en viss Power BI Embedded rapport. Power BI Embedded har inte någon särskild information som användaren har. För RLS ska fungera behöver skicka vissa ytterligare kontext som en del av din apptoken:

* **användarnamnet** (valfritt) – används med RLS. Detta är en sträng som kan användas för att identifiera användaren när du använder RLS regler. Se använder raden säkerhet på radnivå med Powerbi Embedded
* **roller** – en sträng som innehåller rollerna kan välja vid tillämpning av säkerhet på radnivå. Om skicka fler än en roll ska de skickas som en strängmatris.

Du skapar en token med den [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metod. Om egenskapen username finns måste du också ange minst ett värde i roller.

Du kan till exempel ändra EmbedSample. DashboardController rad 55 kunde uppdateras från

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

till

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

Fullständig apptoken ser ut ungefär så här:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Nu, med alla delar tillsammans, när någon loggar in på vår programmet för att visa den här rapporten de kommer bara att kunna se de data som de ska kunna se, som definieras av våra säkerhet på radnivå.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Se även

[Säkerhet på radnivå (RLS) med kraften](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Fler frågor? [Försök med Power BI Community](http://community.powerbi.com/)

