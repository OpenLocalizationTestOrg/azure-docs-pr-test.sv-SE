## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Ange struktur definition för rektangulär datauppsättningar
hello structure-avsnittet i hello datauppsättningar JSON är en **valfria** avsnittet för rektangulär tabeller (med rader och kolumner) och innehåller en uppsättning kolumner för hello tabellen. Du använder hello struktur avsnittet för antingen tillhandahåller typinformation för typkonverteringar eller göra kolumnmappningarna. hello följande avsnitt beskrivs de här funktionerna i detalj. 

Varje kolumn innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello-kolumn. |Ja |
| typ |Datatypen för kolumnen hello. Se typen konverteringar avsnittet nedan för mer information om när ska du ange typinformation |Nej |
| Kultur |.NET baserat kultur toobe används när typ har angetts och är .NET typen Datetime eller Datetimeoffset. Standardvärdet är ”en-us”. |Nej |
| Format |Formatera strängen toobe används när typ har angetts och är .NET typen Datetime eller Datetimeoffset. |Nej |

hello visar följande exempel hello struktur avsnittet JSON för en tabell som har tre kolumner användar-ID, namn och lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Använd följande riktlinjer när hello tooinclude ”” strukturinformation och vilka tooinclude i hello **struktur** avsnitt.

* **För strukturerade datakällor** som lagrar data schema och ange information tillsammans med själva (källor som Azure tabell för SQL Server, Oracle, etc.), bör du ange hello ”struktur” avsnittet om du vill göra kolumnmappningen i specifika hello informationen källkolumner toospecific kolumner i mottagare och deras namn är hello inte samma (Mer information finns i kolumnen mappning nedan). 
  
    Som nämnts ovan är hello typinformation valfri i avsnittet ”struktur”. För strukturerade källor typinformation finns redan som en del av datauppsättningsdefinitionen i hello datalagring, så du bör inte innehåller typinformation när du inkluderar hello ”struktur” avsnittet.
* **För schemat för skrivskyddade datakällor (specifikt Azure blob)** du toostore data utan att spara schemat eller typ information med hello data. Du bör ta ”struktur” för dessa typer av datakällor i hello efter 2 fall:
  * Vill du toodo kolumnmappningen.
  * När hello dataset är en datakälla i en Kopieringsaktivitet kan du kan ange av typinformation i ”struktur” och data factory använder denna typinformation för konvertering toonative typer för hello sink. Se [flytta data tooand från Azure Blob](../articles/data-factory/data-factory-azure-blob-connector.md) artikel för mer information.

### <a name="supported-net-based-types"></a>Stöd för. NET-baserade typer
Data factory stöder hello följande CLS-kompatibel .NET baserat värden av typen för att tillhandahålla ange information i ”struktur” för schemat för skrivskyddade datakällor som Azure blob.

* Int16
* Int32 
* Int64
* Enskild
* dubbla
* Decimal
* byte]
* bool
* Sträng 
* GUID
* Datum och tid
* DateTimeOffset
* Tidsintervall 

För Datetime & Datetimeoffset kan du också ange ”kultur” & ”format” sträng toofacilitate tolkning av din egen Datetime-sträng. Se exemplet för typkonvertering nedan.

