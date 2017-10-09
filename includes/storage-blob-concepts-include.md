## <a name="what-is-blob-storage"></a>Vad är Blob Storage?
Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade objektdata, exempelvis text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS. Du kan använda Blob storage tooexpose data offentligt toohello world eller toostore programdata privat.

Vanliga användningsområden för Blob Storage är:

* Betjänar bilder eller dokument direkt tooa webbläsare
* Lagra filer för distribuerad åtkomst
* Direktuppspelning av video och ljud
* Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering
* Lagra data för analys av en tjänst som kan vara lokal eller Azure-baserad

## <a name="blob-service-concepts"></a>Blob Service-koncept
hello Blob-tjänsten innehåller hello följande komponenter:

![Blobb-arkitektur](./media/storage-blob-concepts-include/blob1.png)

* **Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto. Lagringskontot kan vara ett **allmänt lagringskonto** eller ett **Blob Storage-konto** som är specialiserat för lagring av objekt/blobbar. Mer information finns i [Om Azure Storage-konton](../articles/storage/common/storage-create-storage-account.md).
* **Behållare:** En behållare grupperar en uppsättning blobbar. Alla blobbar måste vara i en behållare. Ett konto kan innehålla ett obegränsat antal behållare. En behållare kan lagra ett obegränsat antal blobbar. Observera att hello behållarens namn måste vara gemener.
* **Blob:** en fil av valfri typ och storlek. Azure Storage erbjuder tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar.
  
    *Blockblobbar* lämpar sig för lagring av text eller binära filer, exempelvis dokument och mediafiler. *Tilläggsblobbar* är liknande tooblock blobbar i att de består av block, men de är optimerade för tilläggsåtgärder, så att de är väl för loggningsscenarier. En enda blockblobb kan innehålla upp too50 000 block med upp too100 MB vardera, ger en total storlek på lite över 4,75 TB (100 MB X 50 000). En enda tilläggsblobb kan innehålla upp too50 000 block med upp too4 MB vardera, för en total storlek på lite över 195 GB (4 MB X 50 000).
  
    *Sidblobbar* kan vara upp too1 TB i storlek och är bäst för frekventa läs-/ skrivåtgärder. Azure Virtual Machines använder sig av sidblobbar som operativsystem- och datadiskar.
  
    Mer information om namngivning av behållare och blobbar finns i [Namngivning och referens av behållare, blobbar och metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

