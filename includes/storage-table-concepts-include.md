## <a name="what-is-table-storage"></a>Vad är Table Storage
Azure Table Storage lagrar stora mängder strukturerade data. hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data. Vanliga användningsområden för Table Storage är:

* Lagring av flera TB med strukturerade data som kan serva webbaserade skalningsbara program
* Lagring av datauppsättningar som inte kräver komplexa kopplingar, sekundärnycklar eller lagrade procedurer som kan avnormaliseras för snabb åtkomst
* Ställa snabba datafrågor med hjälp av ett klustrat index
* Åtkomst till data med hjälp av hello OData-protokollet och LINQ-frågor med WCF Data Service .NET-bibliotek

Du kan använda Table storage toostore och fråga stora mängder strukturerad, icke-relationella data och dina tabeller skalar upp efter behov.

## <a name="table-storage-concepts"></a>Koncept för Table Storage
Tabellagring innehåller hello följande komponenter:

![Komponentdiagram för Table Storage][Table1]

* **URL-format:** kod adresserar tabeller i ett konto med det här adressformatet:   
  http://`<storage account>`.table.core.windows.net/`<table>`  
  
  Du kan adressera Azure-tabeller direkt med den här adressen hello OData-protokollet. Mer information finns på [OData.org][OData.org].
* **Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto. Se [Skalbarhets- och prestandamål för Azure Storage](../articles/storage/common/storage-scalability-targets.md) för information om kapacitet för lagringskonton.
* **Tabell**: en tabell är en samling entiteter. Tabeller framtvingar inte något schema på entiteter, vilket innebär att en enda tabell kan innehålla entiteter med olika egenskapsuppsättningar. hello antalet tabeller som ett lagringskonto kan innehålla begränsas bara av hello lagringsgräns konto kapacitet.
* **Entiteten**: en entitet är en uppsättning egenskaper, liknande tooa databasrad. En entitet kan vara upp too1MB i storlek.
* **Egenskaper**: en egenskap är ett namn-värde-par. Varje entitet kan innehålla in too252 egenskaper toostore data. Varje entitet har också tre systemegenskaper som anger en partitionsnyckel, en radnyckel och en tidsstämpel. Enheter med hello samma partitionsnyckel kan frågas snabbare och infogas/uppdateras i atomiska åtgärder. En entitets radnyckel är dess unika identifierare inom en partition.

Mer information om namngivning av tabeller och egenskaper finns [hello förstå tabelltjänst-datamodellen](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
