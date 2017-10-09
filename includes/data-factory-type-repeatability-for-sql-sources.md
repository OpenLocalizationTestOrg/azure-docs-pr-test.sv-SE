## <a name="repeatability-during-copy"></a><span data-ttu-id="df118-101">Repeterbarhet vid kopiering</span><span class="sxs-lookup"><span data-stu-id="df118-101">Repeatability during Copy</span></span>
<span data-ttu-id="df118-102">När kopieringen data tooAzure SQL/SQL Server från andra data lagras en behov tookeep repeterbarhet i åtanke tooavoid oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="df118-102">When copying data tooAzure SQL/SQL Server from other data stores one needs tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="df118-103">När du kopierar data tooAzure SQL/SQL Server-databas kommer kopieringsaktiviteten av APPEND hello datauppsättning toohello sink Standardtabell som standard.</span><span class="sxs-lookup"><span data-stu-id="df118-103">When copying data tooAzure SQL/SQL Server Database, copy activity will by default APPEND hello data set toohello sink table by default.</span></span> <span data-ttu-id="df118-104">När du kopierar data från en CSV-fil (fil med kommaavgränsade värden) filen datakälla som innehåller två registrerar tooAzure SQL/SQL Server-databas, är det till exempel vilka hello tabellen ser ut som:</span><span class="sxs-lookup"><span data-stu-id="df118-104">For example, when copying data from a CSV (comma separated values data) file source containing two records tooAzure SQL/SQL Server Database, this is what hello table looks like:</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="df118-105">Anta att du fel har påträffats på källfilen och uppdaterade hello mängd ned röret från 2 too4 i hello källfil.</span><span class="sxs-lookup"><span data-stu-id="df118-105">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4 in hello source file.</span></span> <span data-ttu-id="df118-106">Om du kör nytt hello datasektorn för denna period, hittar du två nya poster läggs tooAzure SQL/SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="df118-106">If you re-run hello data slice for that period, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="df118-107">hello nedan förutsätter att ingen av hello kolumner i tabellen hello har hello primärnyckelns begränsning.</span><span class="sxs-lookup"><span data-stu-id="df118-107">hello below assumes none of hello columns in hello table have hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="df118-108">tooavoid detta, behöver du toospecify UPSERT semantik genom att använda en av hello nedan 2 mekanismer som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="df118-108">tooavoid this, you will need toospecify UPSERT semantics by leveraging one of hello below 2 mechanisms stated below.</span></span>

> [!NOTE]
> <span data-ttu-id="df118-109">En sektor går att köra automatiskt i Azure Data Factory enligt hello återförsökspolicyn som anges.</span><span class="sxs-lookup"><span data-stu-id="df118-109">A slice can be re-run automatically in Azure Data Factory as per hello retry policy specified.</span></span>
> 
> 

### <a name="mechanism-1"></a><span data-ttu-id="df118-110">Mekanism 1</span><span class="sxs-lookup"><span data-stu-id="df118-110">Mechanism 1</span></span>
<span data-ttu-id="df118-111">Du kan utnyttja **sqlWriterCleanupScript** egenskapen toofirst utföra rensning av åtgärden när du kör ett segment.</span><span class="sxs-lookup"><span data-stu-id="df118-111">You can leverage **sqlWriterCleanupScript** property toofirst perform cleanup action when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="df118-112">hello Rensa skriptet ska köras första vid kopiering av för ett visst segment som skulle ta bort hello data från hello SQL-tabell motsvarande toothat sektorn.</span><span class="sxs-lookup"><span data-stu-id="df118-112">hello cleanup script would be executed first during copy for a given slice which would delete hello data from hello SQL Table corresponding toothat slice.</span></span> <span data-ttu-id="df118-113">hello aktivitet kommer därefter infoga hello data i hello SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="df118-113">hello activity will subsequently insert hello data into hello SQL Table.</span></span> 

<span data-ttu-id="df118-114">Om hello sektorn är nu kör igen och du hittar hello kvantitet uppdateras som önskade.</span><span class="sxs-lookup"><span data-stu-id="df118-114">If hello slice is now re-run, then you will find hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="df118-115">Anta att hello Flat bricka post tas bort från hello ursprungliga csv.</span><span class="sxs-lookup"><span data-stu-id="df118-115">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="df118-116">Nytt körs hello sektorn skulle sedan skapa hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="df118-116">Then re-running hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
<span data-ttu-id="df118-117">Inget nytt hade toobe klar.</span><span class="sxs-lookup"><span data-stu-id="df118-117">Nothing new had toobe done.</span></span> <span data-ttu-id="df118-118">hello kopieringsaktiviteten kördes hello Rensa skriptet toodelete hello motsvarande data för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="df118-118">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="df118-119">Sedan den läsa hello indata från hello csv (som sedan finns endast 1 post) och infogas i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="df118-119">Then it read hello input from hello csv (which then contained only 1 record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2"></a><span data-ttu-id="df118-120">Metod 2</span><span class="sxs-lookup"><span data-stu-id="df118-120">Mechanism 2</span></span>
> [!IMPORTANT]
> <span data-ttu-id="df118-121">sliceIdentifierColumnName stöds inte för Azure SQL Data Warehouse just nu.</span><span class="sxs-lookup"><span data-stu-id="df118-121">sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse at this time.</span></span> 

<span data-ttu-id="df118-122">En annan mekanism tooachieve repeterbarhet av är att ha en dedikerad kolumn (**sliceIdentifierColumnName**) på hello tabellen som mål.</span><span class="sxs-lookup"><span data-stu-id="df118-122">Another mechanism tooachieve repeatability is by having a dedicated column (**sliceIdentifierColumnName**) in hello target Table.</span></span> <span data-ttu-id="df118-123">Den här kolumnen kan användas av Azure Data Factory tooensure hello käll- och fortsätter att synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="df118-123">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="df118-124">Den här metoden fungerar när det finns flexibilitet för att ändra eller definiera hello mål SQL tabellens schema.</span><span class="sxs-lookup"><span data-stu-id="df118-124">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="df118-125">Den här kolumnen som ska användas av Azure Data Factory för repeterbarhet och pågående hello Azure Data Factory kommer inte att genomföra alla scheman ändras toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="df118-125">This column would be used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory will not make any schema changes toohello Table.</span></span> <span data-ttu-id="df118-126">Sätt toouse den här metoden:</span><span class="sxs-lookup"><span data-stu-id="df118-126">Way toouse this approach:</span></span>

1. <span data-ttu-id="df118-127">Definiera en kolumn av typen binary (32) i hello målet SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="df118-127">Define a column of type binary (32) in hello destination SQL Table.</span></span> <span data-ttu-id="df118-128">Det bör finnas några begränsningar på den här kolumnen.</span><span class="sxs-lookup"><span data-stu-id="df118-128">There should be no constraints on this column.</span></span> <span data-ttu-id="df118-129">Vi namn i den här kolumnen som 'ColumnForADFuseOnly' för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="df118-129">Let's name this column as ‘ColumnForADFuseOnly’ for this example.</span></span>
2. <span data-ttu-id="df118-130">Använd den i hello kopieringsaktiviteten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="df118-130">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

<span data-ttu-id="df118-131">Azure Data Factory kommer att fylla i den här kolumnen enligt dess måste tooensure hello käll- och hålls synkroniserade.</span><span class="sxs-lookup"><span data-stu-id="df118-131">Azure Data Factory will populate this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="df118-132">hello värdena i den här kolumnen får inte användas utanför den här kontexten av hello användare.</span><span class="sxs-lookup"><span data-stu-id="df118-132">hello values of this column should not be used outside of this context by hello user.</span></span> 

<span data-ttu-id="df118-133">Liknande toomechanism 1, Kopieringsaktiviteten kommer automatiskt första Rensa hello data för hello angivna segment från hello målet SQL-tabellen och kör sedan hello kopieringsaktiviteten normalt tooinsert hello data från källan toodestination för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="df118-133">Similar toomechanism 1, Copy Activity will automatically first clean up hello data for hello given slice from hello destination SQL Table and then run hello copy activity normally tooinsert hello data from source toodestination for that slice.</span></span> 

