---
title: "aaaData katalog begrepp för utvecklare | Microsoft Docs"
description: 'Introduktion toohello nyckelbegrepp i Azure Data Catalog konceptuella modellen hello som exponeras via Catalog REST API: et.'
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: d0b1628ff6c31458cb650efef852244f0c139b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-developer-concepts"></a>Begrepp för utvecklare för Azure Data Catalog
Microsoft **Azure Data Catalog** är en helt hanterad molntjänst som tillhandahåller funktioner för upptäckt av datakälla och gemensamt skapade metadata från datakällan. Utvecklare kan använda hello-tjänsten via dess REST-API: er. Förstå hello begrepp som implementerats i hello-tjänsten är viktigt för utvecklare toosuccessfully integreras med **Azure Data Catalog**.

## <a name="key-concepts"></a>Viktiga begrepp
Hej **Azure Data Catalog** konceptuella modellen baseras på fyra viktiga begrepp: hello **katalog**, **användare**, **tillgångar**, och **Anteckningar**.

![Begrepp][1]

*Bild 1 – Azure Data Catalog förenklad konceptuella modellen*

### <a name="catalog"></a>Katalogen
En **katalog** är hello översta behållaren för alla hello metadata som lagras i en organisation. Det finns en **katalog** tillåts per Azure-konto. Kataloger är bundet tooan Azure-prenumeration, men bara en **katalog** kan skapas för en viss Azure-konto, även om ett konto kan ha flera prenumerationer.

En katalog innehåller **användare** och **tillgångar**.

### <a name="users"></a>Användare
Användare kan säkerhetsobjekt som har behörigheter tooperform åtgärder (söka hello-katalogen, lägga till, redigera eller ta bort objekt o.s.v.) i hello katalog.

Det finns flera olika roller som en användare kan ha. Information om roller finns i avsnittet hello roller och auktorisering.

Enskilda användare och säkerhetsgrupper kan läggas till.

Azure Data Catalog använder Azure Active Directory för identitets- och åtkomsthantering. Varje användare i katalogen måste vara medlem i hello Active Directory för hello-kontot.

### <a name="assets"></a>Tillgångar
En **katalogen** innehåller datatillgångar. **Tillgångar** är hello enheterna för granularitet som hanteras av hello katalog.

hello Granulariteten för en tillgång varierar beroende på datakällan. En tillgång kan vara en tabell eller en vy för SQL Server eller Oracle-databas. En tillgång kan vara ett mått eller en Dimension nyckel prestanda indikator (KPI) för SQL Server Analysis Services. För SQL Server Reporting Services är en tillgång en rapport.

En **tillgången** är hello sak du lägger till eller ta bort från en katalog. Det är resultatet åter från hello enheten **Sök**.

En **tillgången** består från dess namn, plats och typ och anteckningar som beskriver den ytterligare.

### <a name="annotations"></a>Anteckningar
Anteckningar är objekt som representerar metadata om tillgångar.

Exempel på anteckningar är beskrivning, taggar, schemat, dokumentation och så vidare. En fullständig lista över hello resurstyper och anteckningens typer finns i hello tillgången objektet modellen avsnitt.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Gemensamt skapade anteckningar och användarens synvinkel (multipliciteten för åsikt)
En viktig aspekt av Azure Data Catalog är hur den stöder hello gemensamt skapade metadata i hello system. Som skillnad från tooa wiki metoden – där det finns bara ett åsikt och hello senaste skrivaren wins – hello Azure Data Catalog modellen tillåter flera åsikter toolive sida vid sida i hello system.

Den här metoden avspeglar verkliga hälsningsmeddelande av företagsdata där olika användare kan ha olika perspektiv på en given tillgång:

* En databasadministratör kan innehålla information om servicenivåavtal eller hello tillgängliga fönster för ETL massåtgärder
* En data-steward kan ge information om hello business processer toowhich hello tillgången gäller eller hello klassificeringar som hello företag har tillämpat tooit
* En ekonomi analytiker kan innehålla information om hur hello data används vid slutet av perioden rapportuppgifter

Det här exemplet varje användare – hello DBA hello data steward och hello analytiker – kan toosupport lägga till en beskrivning tooa tabell som har registrerats i hello katalog. Alla beskrivningar underhålls i hello system och hello Azure Data Catalog-portalen visas alla beskrivningar.

Det här mönstret är tillämpade toomost hello objekt i hello objektmodellen så objekttyper i hello JSON-nyttolast är ofta matriser för egenskaper där du kan förvänta dig en singleton.

Under hello tillgången är roten till exempel en matris med beskrivning objekt. hello matrisegenskap heter ”beskrivning”. En beskrivning av objektet har en egenskap - Beskrivning. hello mönstret är att varje användare som beskrivning av typer hämtar en beskrivning av objektet som skapats för hello-värdet som angavs av hello användare.

hello UX kan sedan välja hur toodisplay hello kombination. Det finns tre olika mönster för visning.

* hello enklaste mönstret är ”visa alla”. I det här mönstret visas alla hello-objekt i en listvy. hello Azure Data Catalog-portalen UX använder det här mönstret beskrivning.
* En annan mönstret är ”sammanfoga”. I det här mönstret samman alla hello värden från hello olika användare tillsammans med dubblett tas bort. Exempel på detta mönster i hello Azure Data Catalog-portalen UX är hello taggar och experter egenskaper.
* En tredje mönstret är ”sista skrivare vinner”. I det här mönstret visas endast hello senaste värdet i. friendlyName är ett exempel på detta mönster.

## <a name="asset-object-model"></a>Objektmodell för tillgångsinformation
Som introducerades i hello nyckelkoncept avsnittet hello **Azure Data Catalog** objektmodellen innehåller objekt som kan vara tillgångar eller anteckningar. Objekt har egenskaper som kan vara valfri eller nödvändig. Vissa egenskaper gäller tooall objekt. Vissa egenskaper gäller tooall tillgångar. Vissa egenskaper gäller endast toospecific resurstyper.

### <a name="system-properties"></a>Systemegenskaper
<table><tr><td><b>Egenskapsnamn</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr><tr><td>tidsstämpel</td><td>Datum och tid</td><td>hello sista gången hello objektet har ändrats. Det här fältet genereras av hello servern när ett objekt infogas och varje gång ett objekt uppdateras. hello värdet för den här egenskapen ignoreras på indata för åtgärder.</td></tr><tr><td>id</td><td>URI: N</td><td>Absolut url för hello-objekt (skrivskyddad). Det är hello unikt adresserbara URI för hello-objektet.  hello värdet för den här egenskapen ignoreras på indata för åtgärder.</td></tr><tr><td>typ</td><td>Sträng</td><td>hello typ av hello tillgång (skrivskyddad).</td></tr><tr><td>ETag</td><td>Sträng</td><td>En sträng motsvarande toohello version av hello-objekt som kan användas för optimistisk samtidighetskontroll när du utför åtgärder som uppdaterar artiklar i hello katalog. ”*” kan vara används toomatch ett värde.</td></tr></table>

### <a name="common-properties"></a>Gemensamma egenskaper
Dessa egenskaper gäller tooall rot resurstyper och alla anteckningstyper av.

<table>
<tr><td><b>Egenskapsnamn</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr>
<tr><td>fromSourceSystem</td><td>Booleskt värde</td><td>Anger om objektets data är härledd från en källa (till exempel Sql Server-databas, Oracle-databas) eller som användaren.</td></tr>
</table>

### <a name="common-root-properties"></a>Allmänna egenskaper för rot
<p>
Dessa egenskaper gäller tooall rot resurstyper.

<table><tr><td><b>Egenskapsnamn</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr><tr><td>namn</td><td>Sträng</td><td>Ett namn som härleds från hello plats informationen om datakällan</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Unikt beskriver hello datakälla och är en av hello identifierare för hello tillgång. (Se dubbla identity-avsnittet).  hello strukturen för hello dsl varierar efter hello-protokollet och typ av datakälla.</td></tr><tr><td>dataSource</td><td>DataSourceInfo</td><td>Mer information om hello typ av tillgång.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Beskriver hello-användare som nyligen registrerad tillgången.  Innehåller både hello unikt id för hello användare (hello upn) och ett namn (efternamn och förnamn).</td></tr><tr><td>behållar-ID</td><td>Sträng</td><td>ID för hello behållaren tillgång för hello-datakälla. Den här egenskapen stöds inte för hello behållartypen.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Allmänna egenskaper för icke-singleton-kommentar
Dessa egenskaper gäller tooall anteckning för icke-singleton-typer (anteckningar, vilket tillåts toobe flera per tillgång).

<table>
<tr><td><b>Egenskapsnamn</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr>
<tr><td>key</td><td>Sträng</td><td>Ange nyckel som unikt identifierar hello kommentar i hello samlingen för en användare. hello Nyckellängden får inte överstiga 256 tecken.</td></tr>
</table>

### <a name="root-asset-types"></a>Roten resurstyper
Roten resurstyper är de typer som representerar hello olika typer av datatillgångar som kan registreras i hello-katalogen. Det finns en vy som beskriver tillgången och anteckningar som ingår i hello vyn för varje rottyp. Vynamn ska användas i hello motsvarande url-segment i {view_name} när du publicerar en tillgång med REST API.

<table><tr><td><b>Tillgångstypen (namn)</b></td><td><b>Ytterligare egenskaper</b></td><td><b>Datatyp</b></td><td><b>Tillåtna anteckningar</b></td><td><b>Kommentarer</b></td></tr><tr><td>Tabellen (”tabeller”)</td><td></td><td></td><td>Beskrivning<p>friendlyName<p>Tagga<p>Schemat<p>ColumnDescription<p>ColumnTag<p> Expert<p>Förhandsversion<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentation<p></td><td>En tabell representerar inga tabelldata.  Till exempel: SQL-tabell, SQL-vyn, Analysis Services Tabular tabell, Analysis Services flerdimensionella dimensions-, Oracle-tabell, osv.   </td></tr><tr><td>Mått (”mått”)</td><td></td><td></td><td>Beskrivning<p>friendlyName<p>Tagga<p>Expert<p>AccessInstruction<p>Dokumentation<p></td><td>Den här typen representerar ett Analysis Services-mått.</td></tr><tr><td></td><td>Mått</td><td>Kolumn</td><td></td><td>Metadata som beskriver hello mått</td></tr><tr><td></td><td>isCalculated </td><td>Booleskt värde</td><td></td><td>Anger om hello mått beräknas eller inte.</td></tr><tr><td></td><td>MeasureGroup</td><td>Sträng</td><td></td><td>Fysiska behållare för måttet</td></tr><td>KPI (”KPI”)</td><td></td><td></td><td>Beskrivning<p>friendlyName<p>Tagga<p>Expert<p>AccessInstruction<p>Dokumentation</td><td></td></tr><tr><td></td><td>MeasureGroup</td><td>Sträng</td><td></td><td>Fysiska behållare för måttet</td></tr><tr><td></td><td>goalExpression</td><td>Sträng</td><td></td><td>Ett numeriskt MDX-uttryck eller en beräkning som returnerar hello målvärdet för hello KPI.</td></tr><tr><td></td><td>valueExpression</td><td>Sträng</td><td></td><td>Ett MDX-numeriska uttryck som returnerar hello faktiska värdet för hello KPI.</td></tr><tr><td></td><td>statusExpression</td><td>Sträng</td><td></td><td>Ett MDX-uttryck som representerar hello tillståndet för hello KPI vid en angiven tidpunkt.</td></tr><tr><td></td><td>trendExpression</td><td>Sträng</td><td></td><td>Ett MDX-uttryck som utvärderas hello KPI hello värdet över tid. hello trend kan vara något tidsbaserade kriterium som används i en specifik affärskontexten.</td>
<tr><td>Rapporten (”rapporter”)</td><td></td><td></td><td>Beskrivning<p>friendlyName<p>Tagga<p>Expert<p>AccessInstruction<p>Dokumentation<p></td><td>Den här typen representerar en SQL Server Reporting Services-rapport </td></tr><tr><td></td><td>assetCreatedDate</td><td>Sträng</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Sträng</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Sträng</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Sträng</td><td></td><td></td></tr><tr><td>Behållare (”behållare”)</td><td></td><td></td><td>Beskrivning<p>friendlyName<p>Tagga<p>Expert<p>AccessInstruction<p>Dokumentation<p></td><td>Den här typen representerar en behållare för andra tillgångar, till exempel en SQL-databas, en Azure BLOB-behållare eller en Analysis Services-modell.</td></tr></table>

### <a name="annotation-types"></a>Anteckningstyper
Anteckningstyper representerar typer av metadata som kan tilldelas tooother typer hello Catalog.

<table>
<tr><td><b>Anteckningstypen (kapslade vynamn)</b></td><td><b>Ytterligare egenskaper</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr>

<tr><td>Beskrivning (”beskrivningar”)</td><td></td><td></td><td>Den här egenskapen innehåller en beskrivning för en tillgång. Alla användare av hello system kan lägga till egna beskrivning.  Bara den användaren kan redigera hello beskrivning av objektet.  (I administratörer och tillgångsinformation ägare kan det ta bort hello beskrivning objektet men inte redigera den). hello innehåller beskrivningar av användarnas separat.  Därför är en matris med beskrivningar på varje tillgång (ett för varje användare som har bidragit sina kunskaper om hello tillgångar i tillägg toopossibly en som innehåller information som härletts från hello-datakälla).</td></tr>
<tr><td></td><td>description</td><td>Sträng</td><td>En kort beskrivning (2-3 rader) av hello tillgång</td></tr>

<tr><td>Taggen (”-taggar”)</td><td></td><td></td><td>Den här egenskapen definierar en tagg för en tillgång. Alla användare av hello system kan lägga till flera taggar för en tillgång.  Endast hello-användaren som skapade taggen objekt kan redigeras.  (I administratörer och tillgångsinformation ägare kan det ta bort hello taggen objektet men inte redigera den). hello system underhåller användarnas taggar separat.  Därför är en matris med taggen objekt på varje tillgång.</td></tr>
<tr><td></td><td>Taggen</td><td>Sträng</td><td>En tagg som beskriver hello tillgång.</td></tr>

<tr><td>FriendlyName (”friendlyName”)</td><td></td><td></td><td>Den här egenskapen innehåller ett eget namn för en tillgång. FriendlyName är en singleton-anteckning - endast en FriendlyName kan läggas till tooan tillgången.  Endast hello-användaren som skapade FriendlyName objekt kan redigera den. (Administratörer och tillgångsinformation ägare kan ta bort hello FriendlyName objekt, men inte redigera den). hello system underhåller separat användarnas egna namn.</td></tr>
<tr><td></td><td>friendlyName</td><td>Sträng</td><td>Ett eget namn för hello tillgång.</td></tr>

<tr><td>Schemat (”schema”)</td><td></td><td></td><td>hello schemat beskriver hello strukturen i hello data.  Den visar hello attributet (kolumnen, attribut, fält, etc.) namn, typer och andra metadata.  Den här informationen är härledd från hello-datakälla.  Schemat är en singleton-anteckning - bara ett Schema kan läggas till för en tillgång.</td></tr>
<tr><td></td><td>Kolumner</td><td>Kolumnen]</td><td>En matris med kolumnen objekt. De beskriver hello kolumn med information som härrör från hello-datakälla.</td></tr>

<tr><td>ColumnDescription (”columnDescriptions”)</td><td></td><td></td><td>Den här egenskapen innehåller en beskrivning för en kolumn.  Alla användare av hello system kan lägga till egna beskrivningar på flera kolumner (högst en per kolumn). Endast hello-användaren som skapade ColumnDescription objekt kan redigeras.  (Administratörer och tillgångsinformation ägare kan ta bort hello ColumnDescription objekt, men inte redigera den). hello innehåller dessa användare kolumnbeskrivningar separat.  Därför är det en matris med ColumnDescription objekt på varje tillgång (en per kolumn för varje användare som har bidragit sina kunskaper om hello kolumnen dessutom toopossibly en som innehåller information härleds från hello-datakälla).  Hej ColumnDescription är löst bundna toohello schemat så att den kan hämta synkroniserat. Hej ColumnDescription kanske det beskriver en kolumn som inte längre finns i hello schemat.  Nu är det upp toohello writer tookeep beskrivning och schemat som är synkroniserade.  hello-datakällan kan också ha kolumner beskrivningsinformation och de ytterligare ColumnDescription-objekt som skapas när du kör hello-verktyget.</td></tr>
<tr><td></td><td>columnName</td><td>Sträng</td><td>hello namnet på hello kolumn beskrivningen refererar till.</td></tr>
<tr><td></td><td>description</td><td>Sträng</td><td>en kort beskrivning (2-3 rader) för hello kolumnen.</td></tr>

<tr><td>ColumnTag (”columnTags”)</td><td></td><td></td><td>Den här egenskapen innehåller en tagg för en kolumn. Varje användare av hello system kan lägga till flera taggar för en viss kolumn och kan lägga till taggar för flera kolumner. Endast hello-användaren som skapade ColumnTag objekt kan redigeras. (Administratörer och tillgångsinformation ägare kan ta bort hello ColumnTag objekt, men inte redigera den). hello innehåller dessa användare kolumnen taggar separat.  Därför är en matris med ColumnTag objekt på varje tillgång.  Hej ColumnTag är löst bundna toohello schema så att den kan hämta synkroniserat. Hej ColumnTag kanske det beskriver en kolumn som inte längre finns i hello schemat.  Nu är det upp toohello writer tookeep kolumnen taggen och schemat som är synkroniserade.</td></tr>
<tr><td></td><td>columnName</td><td>Sträng</td><td>hello namnet på hello-kolumn som den här taggen refererar till.</td></tr>
<tr><td></td><td>Taggen</td><td>Sträng</td><td>En tagg som beskriver hello kolumn.</td></tr>

<tr><td>Expert (”experter”)</td><td></td><td></td><td>Den här egenskapen innehåller en användare anses vara en expert i hello datamängden. hello experter opinions(descriptions) bubblan toohello överkant hello UX när med beskrivningar. Varje användare kan ange sina egna experter. Bara den användaren kan redigera hello experter objekt. (Administratörer och tillgångsinformation ägare kan ta bort hello Expert objekt, men inte redigera den).</td></tr>
<tr><td></td><td>Expert</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Förhandsgranska (”förhandsgranskningar”)</td><td></td><td></td><td>hello preview innehåller en ögonblicksbild av hello översta 20 rader med data för hello tillgång. Förhandsgranska endast vara meningsfullt för vissa typer av tillgångar (det klokt för tabellen men inte för mått).</td></tr>
<tr><td></td><td>Förhandsgranskning</td><td>objektet]</td><td>Matris med objekt som representerar en kolumn.  Varje objekt har egenskapen mappning tooa kolumnen med ett värde för kolumnen för hello rad.</td></tr>

<tr><td>AccessInstruction (”accessInstructions”)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>Sträng</td><td>hello MIME-typ av hello innehåll.</td></tr>
<tr><td></td><td>Innehåll</td><td>Sträng</td><td>hello instruktioner för hur tooget åt toothis datatillgång. hello innehåll kan vara en URL, en e-postadress eller en uppsättning instruktioner.</td></tr>

<tr><td>TableDataProfile (”tableDataProfiles”)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>hello antalet rader i hello datauppsättning</td></tr>
<tr><td></td><td>Storlek</td><td>lång</td><td>hello storlek i byte av hello datamängd.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Sträng</td><td>hello senaste gången hello schema har ändrats</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Sträng</td><td>hello senaste gången hello datauppsättning har ändrats (data har lagts till, ändra, eller ta bort)</td></tr>

<tr><td>ColumnsDataProfile (”columnsDataProfiles”)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kolumner</td></td><td>ColumnDataProfile]</td><td>En matris med kolumnen data profiler.</td></tr>

<tr><td>ColumnDataClassification (”columnDataClassifications”)</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Sträng</td><td>hello namnet på hello kolumn klassificeringen refererar till.</td></tr>
<tr><td></td><td>Klassificering</td><td>Sträng</td><td>hello klassificering av hello data i den här kolumnen.</td></tr>

<tr><td>Dokumentation (”dokumentation”)</td><td></td><td></td><td>En given tillgång kan ha endast en dokumentation som associeras med den.</td></tr>
<tr><td></td><td>mimeType</td><td>Sträng</td><td>hello MIME-typ av hello innehåll.</td></tr>
<tr><td></td><td>Innehåll</td><td>Sträng</td><td>hello documentation-innehållet.</td></tr>

</table>

### <a name="common-types"></a>Vanliga
Vanliga datatyper som kan användas som hello typer för egenskaper, men är inte objekt.

<table>
<tr><td><b>Gemensam typ.</b></td><td><b>Egenskaper</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>SourceType</td><td>Sträng</td><td>Beskriver hello typ av datakälla.  Till exempel: SQLServer, Oracle-databas, osv.  </td></tr>
<tr><td></td><td>Objekttyp</td><td>Sträng</td><td>Beskriver hello typ av objekt i hello datakällan. Till exempel: tabell, visa för SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokollet</td><td>Sträng</td><td>Krävs. Beskriver en toocommunicate protokoll som används med hello datakällan. Till exempel: ”tds” för SQl-servern ”oracle” för Oracle, osv. Se för[Referensspecifikation - DSL-strukturen för datakälla](data-catalog-dsr.md) hello lista över protokoll som stöds.</td></tr>
<tr><td></td><td>Adress</td><td>Ordlista<string, object></td><td>Krävs. Adressen är en uppsättning data specifika toohello protokoll som används tooidentify hello datakälla refererar till. hello adressdata omfattas tooa viss protokollet, vilket innebär att det är meningslösa utan att känna till hello-protokollet.</td></tr>
<tr><td></td><td>Autentisering</td><td>Sträng</td><td>Valfri. autentiseringsschema för hello används toocommunicate med hello datakällan. Exempel: windows, oauth osv.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Ordlista<string, object></td><td>Valfri. Mer information om hur tooconnect tooa datakälla.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>hello backend utför inte någon validering av angivna egenskaper mot AAD vid publicering.</td></tr>
<tr><td></td><td>UPN</td><td>Sträng</td><td>Användarens unika e-postadress. Måste anges om objekt-ID inte har angetts eller hello gäller ”lastRegisteredBy”-egenskap, annars valfritt.</td></tr>
<tr><td></td><td>objekt-ID</td><td>GUID</td><td>Användaren eller säkerhetsgruppen grupp AAD-identitet. Valfri. Måste anges om UPN-namnet inte anges, annars valfritt.</td></tr>
<tr><td></td><td>Förnamn</td><td>Sträng</td><td>Förnamn för användare (i visningssyfte). Valfri. Endast giltigt i hello kontext för egenskapen ”lastRegisteredBy”. Kan inte anges när säkerhetsobjekt för ”roller”, ”behörigheter” och ”experter”.</td></tr>
<tr><td></td><td>Efternamn</td><td>Sträng</td><td>Efternamn för användaren (för visning). Valfri. Endast giltigt i hello kontext för egenskapen ”lastRegisteredBy”. Kan inte anges när säkerhetsobjekt för ”roller”, ”behörigheter” och ”experter”.</td></tr>

<tr><td>Kolumn</td><td></td><td></td><td></td></tr>
<tr><td></td><td>namn</td><td>Sträng</td><td>Namnet på kolumnen hello eller attribut.</td></tr>
<tr><td></td><td>typ</td><td>Sträng</td><td>datatypen för kolumnen hello eller attribut. hello tillåtna typer beroende data sourceType av hello tillgång.  Endast en delmängd av typer stöds.</td></tr>
<tr><td></td><td>maxLength</td><td>int</td><td>hello tillåten maxlängd för hello kolumnen eller attribut. Härleds från datakällan. Endast tillämpliga toosome typer av datakällor.</td></tr>
<tr><td></td><td>Precision</td><td>Mottagna byte</td><td>hello precisionen för kolumnen hello eller attribut. Härleds från datakällan. Endast tillämpliga toosome typer av datakällor.</td></tr>
<tr><td></td><td>isNullable</td><td>Booleskt värde</td><td>Om hello kolumnen är tillåtet toohave ett null-värde eller inte. Härleds från datakällan. Endast tillämpliga toosome typer av datakällor.</td></tr>
<tr><td></td><td>uttryck</td><td>Sträng</td><td>Om hello-värdet är en beräknad kolumn, innehåller hello-uttryck som representerar hello värde. Härleds från datakällan. Endast tillämpliga toosome typer av datakällor.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>Sträng</td><td>hello namnet på hello kolumn</td></tr>
<tr><td></td><td>typ </td><td>Sträng</td><td>hello typ av hello kolumn</td></tr>
<tr><td></td><td>min. </td><td>Sträng</td><td>hello minimivärdet i hello datamängd</td></tr>
<tr><td></td><td>Max </td><td>Sträng</td><td>hello maxvärdet i hello datauppsättning</td></tr>
<tr><td></td><td>genomsn. </td><td>dubbla</td><td>hello genomsnittligt värde i hello datamängd</td></tr>
<tr><td></td><td>StDev </td><td>dubbla</td><td>hello standardavvikelsen för hello datauppsättning</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>hello antal null-värden i hello datauppsättning</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>hello antalet distinkta värden i hello datamängd</td></tr>


</table>

## <a name="asset-identity"></a>Identitet för tillgångsinformation
Azure Data Catalog använder ”protokoll” och identitetsegenskaper från hello ”adress” egenskapsuppsättning för hello DataSourceLocation ”dsl” egenskapen toogenerate identitet hello tillgången, vilket är används tooaddress hello tillgången i hello katalog.
Hej ”tds”-protokollet har till exempel identitetsegenskaper ”server”, ”databas”, ”schema” och ”objekt”. hello kombinationer av hello-protokollet och hello identitetsegenskaper är används toogenerate hello identitet hello tillgång för SQL Server-tabellen.
Azure Data Catalog innehåller flera inbyggda data käll-protokoll som nämns i [Referensspecifikation - DSL-strukturen för datakälla](data-catalog-dsr.md).
hello uppsättning protokoll som stöds kan utökas genom programmering (se tooData katalog REST API-referensen). Administratörer av hello katalog kan registrera anpassade datakälla protokoll. hello beskrivs följande tabell hello egenskaper som behövs för tooregister anpassade protokollet.

### <a name="custom-data-source-protocol-specification"></a>Anpassade Protokollspecifikationen för datakälla
<table>
<tr><td><b>Typ</b></td><td><b>Egenskaper</b></td><td><b>Datatyp</b></td><td><b>Kommentarer</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>namnområde</td><td>Sträng</td><td>hello namnområde för hello-protokollet. Namespace måste vara mellan 1 too255 tecken, innehåller en eller flera icke-tom delar avgränsade med punkt (.). Varje del måste vara från 1 too255 tecken, börja med en bokstav och endast innehålla bokstäver och siffror.</td></tr>
<tr><td></td><td>namn</td><td>Sträng</td><td>hello namnet på hello-protokollet. Namnet måste vara från 1 too255 tecken, börja med en bokstav och bara innehålla bokstäver, siffror, och hello streck (-).</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty]</td><td>Lista över identitetsegenskaper, måste innehålla minst en, men inga fler än 20 egenskaper. Till exempel: ”server”, ”databas”, ”schema”, ”objektet” är identitetsegenskaper för hello ”tds”-protokollet.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet]</td><td>Lista över identitet anger. Definierar uppsättningar av identitetsegenskaper som representerar giltig tillgången identitet. Måste innehålla minst en, men inga fler än 20 uppsättningar. Exempel: {”server”, ”databas”, ”schema” och ”objekt”} är en identitet för ”tds”-protokollet som definierar identiteten för Sql Server-tabell tillgången.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>namn</td><td>Sträng</td><td>hello namnet på hello-egenskap. Namnet måste bestå av 1 too100 tecken, börja med en bokstav och får bara innehålla bokstäver och siffror.</td></tr>
<tr><td></td><td>typ</td><td>Sträng</td><td>hello typ av hello-egenskap. Värden som stöds: ”bool”, boolean ”,” byte ”,” guid ”,” int ”,” heltal ”,” långa ”,” sträng ”,” url ”</td></tr>
<tr><td></td><td>ignoreCase</td><td>bool</td><td>Anger om fallet ska ignoreras när du använder egenskapens värde. Kan bara anges för egenskaper av typen ”sträng”. Standardvärdet är false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>bool]</td><td>Anger om fallet ska ignoreras för varje del av hello url-sökväg. Kan bara anges för egenskaper av typen ”url”. Standardvärdet är [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>namn</td><td>Sträng</td><td>namnet på hello hello identitet har angetts.</td></tr>
<tr><td></td><td>properties</td><td>String]</td><td>hello lista över identitetsegenskaper inkluderas i den här identiteten anges. Det får inte innehålla dubbletter. Varje egenskap som refererar till identitet uppsättningen måste definieras i hello lista över ”identityProperties” hello-protokollet.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Roller och auktorisering
Microsoft Azure Data Catalog innehåller funktioner för auktorisering för CRUD-åtgärder på tillgångar och anteckningar.

## <a name="key-concepts"></a>Viktiga begrepp
hello Azure Data Catalog använder två mekanismer för godkännande:

* Rollbaserad auktorisering
* Behörighet-baserad auktorisering

### <a name="roles"></a>Roller
Det finns tre roller: **administratör**, **ägare**, och **deltagare**.  Varje roll har sitt omfång och rättigheter som sammanfattas i följande tabell hello.

<table><tr><td><b>Roll</b></td><td><b>Omfång</b></td><td><b>Rättigheter</b></td></tr><tr><td>Administratör</td><td>Katalogen (alla tillgångar/anteckningar i hello katalog)</td><td>Läs Delete ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Ägare</td><td>Varje enskilt objekt (rotobjekt)</td><td>Läs Delete ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Deltagare</td><td>Varje enskild tillgång och anteckningar</td><td>Läsa uppdatera tar du bort ViewRoles Obs: alla rättigheter som hello återkallas om hello läsa i hello-objektet har återkallats från hello deltagare</td></tr></table>

> [!NOTE]
> **Läs**, **uppdatering**, **ta bort**, **ViewRoles** rättigheter är tillämpliga tooany objektet (tillgång eller anteckningen) när **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** är endast tillämplig toohello rot tillgång.
> 
> **Ta bort** rättigheten tooan objekt och subitems eller ett objekt under den. Till exempel tar en tillgång också du bort alla anteckningar för tillgången.
> 
> 

### <a name="permissions"></a>Behörigheter
Behörigheten är lista med poster. Varje åtkomstkontrollpost tilldelar uppsättning rättigheter tooa säkerhetsobjekt. Behörigheter kan bara anges för en tillgång (det vill säga rotobjekt) och Använd toohello tillgång och alla undermappar.

Under hello **Azure Data Catalog** förhandsgranska endast **Läs** höger stöds i hello behörigheter listan tooenable scenariot toorestrict synlighet för en tillgång.

Alla autentiserade användare har som standard **Läs** Högerklicka för alla objekt i katalogen hello om synlighet begränsas toohello uppsättning av säkerhetsobjekt i hello behörigheter.

## <a name="rest-api"></a>REST API
**PLACERA** och **POST** visa objekt begäranden kan använda toocontrol roller och behörigheter: två Systemegenskaper kan anges i tillägget tooitem nyttolast **roller** och  **behörigheter**.

> [!NOTE]
> **behörigheter** endast tillämpliga tooa rotobjekt.
> 
> **Ägare** roll gäller endast tooa rotobjekt.
> 
> Som standard när ett objekt skapas i hello katalogen dess **deltagare** anges toohello för närvarande autentiserad användare. Om objektet ska vara uppdateringsbar av alla, **deltagare** ska anges för&lt;alla&gt; särskilda säkerhetsobjekt i hello **roller** egenskapen när objektet publiceras (för första gången Se följande exempel toohello). **Deltagare** kan inte ändras och hello förblir densamma under livslängden för ett objekt (även **administratör** eller **ägare** saknar hello rätt toochange hello  **Deltagare**). Hej endast värde som stöds för hello explicita inställningen av hello **deltagare** är &lt;alla&gt;: **deltagare** endast kan en användare som skapade ett objekt eller &lt; Alla&gt;.
> 
> 

### <a name="examples"></a>Exempel
**Ange deltagare för&lt;alla&gt; när du publicerar ett objekt.**
Särskilda säkerhetsobjekt &lt;alla&gt; har objectId ”00000000-0000-0000-0000-000000000201”.
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Vissa klientimplementeringar för HTTP kan automatiskt återutfärda begäranden i svaret tooa 302 från hello-servern, men vanligtvis remsans auktorisering huvuden hello-begäran. Eftersom hello Authorization-huvud är obligatoriska toomake begäranden tooAzure Data Catalog, måste du kontrollera hello Authorization-huvud tillhandahålls fortfarande när återutfärda en begäran tooa omdirigerings-plats som anges av Azure Data Catalog. hello visar följande exempelkod den med hjälp av hello .NET i HttpWebRequest-objektet.
> 
> 

**Brödtext**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Tilldela ägare och begränsar synligheten för en befintlig rotobjekt**: **PLACERA** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> I PUT det krävs inte toospecify en artikel nyttolast i hello brödtext: PUT kan bara använda tooupdate-roller och/eller behörigheter.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
