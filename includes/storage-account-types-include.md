Det finns två typer av lagringskonton:

### <a name="general-purpose-storage-accounts"></a>Allmänna lagringskonton
Ett allmänt lagringskonto ger du åtkomst till tooAzure Storage-tjänster som tabeller, köer, filer, Blobbar och Azure virtuella diskar under ett enskilt konto. Den här typen av lagringskonto har två prestandanivåer:

* En standard för lagringsprestanda som gör att du toostore tabeller, köer, filer, Blobbar och Azure virtuella diskar.
* Premiumnivån för lagringsprestanda som för närvarande bara stöder virtuella Azure-datordiskar. En detaljerad översikt över Premium-lagring finns i [Premium Storage: högpresterande lagring för virtuella Azure-datorbelastningar](../articles/storage/common/storage-premium-storage.md).

### <a name="blob-storage-accounts"></a>Blob Storage-konton
Ett Blob-lagringskonto är ett specialiserat lagringskonto för lagring av ostrukturerad data som blobbar (objekt) i Azure Storage. BLOB storage-konton är liknande tooyour befintliga allmänna lagringskonton och dela alla hello bra hållbarhet, tillgänglighet, skalbarhet och prestanda funktioner du använder idag, inklusive 100 procent konsekvent API för blockblobar och tilläggsblobar. För program som bara behöver lagring av block- eller tilläggsblobbar, rekommenderar vi att du använder Blob-lagringskonton.

> [!NOTE]
> Blob Storage-konton stöder endast block- och tilläggsblobar, inte sidblobar.
> 
> 

BLOB storage-konton exponerar hello **åtkomstnivå** attribut som kan anges vid kontoskapandet och ändras senare vid behov. Det finns två typer av åtkomstnivåer som kan anges baserat på dina mönster för dataåtkomst:

* En **frekvent** åtkomstnivå som anger att hello objekt i hello storage-konto kommer att användas oftare. Detta ger dig toostore data på en lägre åtkomstkostnad.
* En **kall** åtkomstnivå som anger att hello objekt i hello storage-konto kommer att användas mindre ofta. Detta ger dig toostore data på en lägre kostnad för datalagring.

Om det finns en ändring i hello användningsmönstret för dina data, kan du också växla mellan de olika nivåerna när som helst. Ändra åtkomstnivå för hello kan medföra ytterligare kostnader. Mer information finns i [Priser och fakturering för Blob-lagringskonton](../articles/storage/blobs/storage-blob-storage-tiers.md#pricing-and-billing).

Mer information om Blob-lagringskonton finns i [Azure Blob Storage: frekvent och lågfrekvent nivå](../articles/storage/blobs/storage-blob-storage-tiers.md).

Innan du kan skapa ett lagringskonto måste du ha en Azure-prenumeration som är en plan som ger dig åtkomst till tooa olika Azure-tjänster. Du kan komma igång med Azure med ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/). När du väljer toopurchase ett prenumerationsavtal, kan du välja från en mängd olika [köpalternativ](https://azure.microsoft.com/pricing/purchase-options/). Om du är en [MSDN-prenumerant](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), får du kostnadsfria, månatliga krediter som kan användas med Azure-tjänster, inklusive Azure Storage. Se [Azure Storage-priser ](https://azure.microsoft.com/pricing/details/storage/) för information om volympriser.

hur toocreate ett lagringskonto finns toolearn [skapa ett lagringskonto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) för mer information. Du kan skapa upp too200 unikt namngivna lagringskonton med en enda prenumeration. Se [Skalbarhets- och prestandamål för Azure Storage](../articles/storage/common/storage-scalability-targets.md) för information om begränsningar för lagringskonton.

