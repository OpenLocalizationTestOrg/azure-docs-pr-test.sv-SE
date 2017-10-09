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
# <a name="row-level-security-with-power-bi-embedded"></a>Säkerhet på radnivå med Power BI Embedded

Säkerhet på radnivå (RLS) kan vara används toorestrict åtkomst tooparticular användardata inom en rapport eller dataset, vilket gör att för flera olika användare toouse hello samma rapport när alla se olika data. Power BI Embedded stöder nu datauppsättningar som konfigurerats med RLS.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

I ordning tootake nytta av RLS är det viktigt att du förstår tre huvudkoncepten; Användare, roller och regler. Nu ska vi titta närmare på varje:

**Användare** – dessa hello faktiska slutanvändare visar rapporter. I Power BI Embedded identifieras användare genom hello användarnamn egenskap i en Apptoken.

**Roller** – användarna tillhör tooroles. En roll är en behållare för regler och kan vara ett namn som ”försäljningschef” eller ”säljare”. I Power BI Embedded identifieras användare genom hello roller egenskap i en Apptoken.

**Regler** – roller har regler och dessa regler är hello faktiska filter som ska tillämpas toobe toohello data. Detta kan vara så enkelt som ”land = USA” eller något mycket mer dynamiskt.

### <a name="example"></a>Exempel

Hello resten av den här artikeln, kommer vi ger ett exempel på redigering RLS och förbrukar som ett inbäddat program. Vårt exempel använder hello [Retail analys exempel](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fil.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

Exemplet Retail analys visar försäljning för alla hello butiker i en viss kedja. Utan RLS, oavsett vilken distrikt manager loggar in och vyer hello rapporten, ser hello samma data. Företagsledning har fastställt varje distrikt manager bör endast visas hello sales för hello lagrar de hanterar och toodo kan vi använda RLS.

RLS har skrivits i Power BI Desktop. När hello datasetet och rapporten öppnas, växel vi toodiagram visa toosee hello schema:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Här följer några saker toonotice med schemat:

* Alla åtgärder som **totalförsäljning**, lagras i hello **försäljning** faktatabell.
* Det finns fyra ytterligare relaterade dimensionstabeller: **objektet**, **tid**, **Store**, och **distrikt**.
* hello pilar på hello relationen rader visar hur filter kan flöda från en tabell tooanother. Om exempelvis ett filter är placerad **tid [Date]**, i hello aktuella schemat den bara filtrerar ned värdena i hello **försäljning** tabell. Inga andra tabeller kan påverkas av det här filtret eftersom alla hello pilarna på hello relationen rader punkt toohello försäljning tabell och inte direkt.
* Hej **distrikt** tabellen anger som hello manager för varje distrikt:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

Baserat på det här schemat om vi tillämpa ett filter toohello **distrikt Manager** kolumn i hello distrikt tabell och om filtret matchar hello användare som visar hello rapporten, filtret också filtrera ned hello **Store**och **försäljning** tabeller tooonly visa data för det specifika district manager.

Här är hur:

1. Klicka på fliken modellering hello **hantera roller**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Skapa en ny roll som kallas **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. I hello **distrikt** tabell ange hello följande DAX-uttryck: **[distrikt Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. toomake att hello regler arbetar på hello **Modeling** klickar du på **vyn som roller**, och ange sedan hello följande:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   hello rapporter ska nu visa data som om du har loggat in som **Andrew Ma**.

Använda hello filtret, hello sätt som vi gjorde här, filtrerar ner alla poster i hello **distrikt**, **Store**, och **försäljning** tabeller. Emellertid hello filterriktningen på hello relationer mellan **försäljning** och **tid**, **försäljning** och **objektet**, och **Objektet** och **tid** tabeller filtreras inte ned.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Som kan vara ok för det här kravet, men om vi inte vill chefer toosee objekt som de inte har någon försäljning, vi kan aktivera dubbelriktad mellan filtrering för hello relationen och flödet hello säkerhetsfilter i båda riktningarna. Detta kan göras genom att redigera hello förhållandet mellan **försäljning** och **objektet**, så här:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Filter kan nu också flöda från hello försäljning tabell toohello **objektet** tabell:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> Om du använder DirectQuery-läge för dina data behöver tooenable dubbelriktad-mellan filtrering genom att välja de här två alternativen:

1. **Filen** -> **alternativ och inställningar** -> **förhandsgranskningsfunktioner** -> **aktivera korsfiltrering i båda riktningarna för DirectQuery**.
2. **Filen** -> **alternativ och inställningar** -> **DirectQuery** -> **tillåta obegränsad mått i DirectQuery-läge**.

Mer om dubbelriktad mellan filtrering, hämta hello toolearn [dubbelriktad mellan filtrering i SQL Server Analysis Services 2016 och Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) vitbok.

Detta packar upp alla hello arbete som måste göras i Power BI Desktop toobe, men det finns en mer arbete som behöver toobe klar toomake hello RLS regler vi har definierat arbete i Power BI Embedded. Användare autentiseras och auktoriseras av ditt program och apptoken är används toogrant användaren åtkomst tooa specifika Power BI Embedded rapporten. Power BI Embedded har inte någon särskild information som användaren har. För RLS toowork behöver du toopass vissa ytterligare kontext som en del av din apptoken:

* **användarnamnet** (valfritt) – används med RLS. Detta är en sträng som du kan använda toohelp identifiera hello användare när du använder RLS regler. Se använder raden säkerhet på radnivå med Powerbi Embedded
* **roller** – en sträng som innehåller hello roller tooselect när du använder regler för säkerhet på radnivå. Om skicka fler än en roll ska de skickas som en strängmatris.

Du skapar hello token med hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metod. Om hello användarnamnsegenskapen finns måste du också ange minst ett värde i roller.

Du kan till exempel ändra hello EmbedSample. DashboardController rad 55 kunde uppdateras från

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

till

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

hello fullständig apptoken ser ut ungefär så här:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Nu, med alla hello delar tillsammans, när någon loggar in på våra program tooview den här rapporten kan användarna bara ska kan toosee hello data att de tillåts toosee, som definieras av våra säkerhet på radnivå.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Se även

[Säkerhet på radnivå (RLS) med kraften](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)

