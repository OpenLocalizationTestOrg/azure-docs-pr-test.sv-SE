## <a name="what-is-blob-storage"></a>Vad är Blob storage?
Azure Blob Storage är en tjänst för att lagra stora mängder ostrukturerad objektdata, exempelvis text eller binär data som kan nås från var som helst i världen via HTTP eller HTTPS. Du kan använda Blob Storage för att exponera data offentligt eller lagra programdata privat.

Vanliga användningsområden för Blob Storage är:

* Leverera bilder eller dokument direkt till en webbläsare.
* Lagra filer för distribuerad åtkomst.
* Direktuppspelning av video och ljud.
* Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering.
* Lagra data för analys av en lokal eller Azure-värdtjänsten.

## <a name="blob-service-concepts"></a>Blob Service-koncept
Blob Service innehåller följande komponenter:

![Diagram över Blob-tjänsten-arkitektur](./media/storage-blob-concepts-include/blob1.png)

* **Storage-konto:** All åtkomst till Azure Storage görs genom ett lagringskonto. Det här lagringskontot kan vara en **Allmänt lagringskonto** eller en **Blob-lagringskonto**, som är specialiserat för lagring av objekt eller BLOB. Mer information finns i [Om Azure Storage-konton](../articles/storage/common/storage-create-storage-account.md).
* **Behållare:** En behållare grupperar en uppsättning blobbar. Alla blobbar måste vara i en behållare. Ett konto kan innehålla ett obegränsat antal behållare. En behållare kan lagra ett obegränsat antal blobbar. Observera att behållarens namn får innehålla endast gemener.
* **Blob:** en fil av valfri typ och storlek. Azure Storage erbjuder tre typer av blobbar: blockblobbar, tilläggsblobbar och sidblobbar.
  
    *Blockblobbar* lämpar sig för lagring av text eller binära filer, exempelvis dokument och mediafiler. En enda blockblobb kan innehålla upp till 50 000 block med upp till 100 MB vardera, vilket ger en total storlek på lite över 4,75 TB (100 MB X 50 000). 

    *Tilläggsblobbar* liknar blockblobbar i det avseendet att de består av block, men de är optimerade för tilläggsåtgärder, så de lämpar sig väl för loggningsscenarier. En enda tilläggsblobb kan innehålla upp till 50 000 block med upp till 4 MB vardera, vilket ger en total storlek på lite över 195 GB (4 MB X 50 000).
  
    *Sidblobbar* kan vara upp till 1 TB stora och är bäst för frekventa läs-/skrivåtgärder. Azure Virtual Machines använder sidblobbar som operativsystem och datadiskar.
  
    Mer information om namngivning av behållare och blobbar finns [namnge och referera till behållare, blobbar och metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

