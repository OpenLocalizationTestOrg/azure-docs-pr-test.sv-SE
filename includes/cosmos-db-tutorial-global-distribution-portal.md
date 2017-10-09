
<span data-ttu-id="be662-101">Du kan lära dig om Azure Cosmos DB globala distribution i den här Azure fredag video med Scott Hanselman och huvudnamn tekniker Manager Karthik Raman.</span><span class="sxs-lookup"><span data-stu-id="be662-101">You can learn about Azure Cosmos DB global distribution in this Azure Friday video with Scott Hanselman and Principal Engineering Manager Karthik Raman.</span></span>

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

<span data-ttu-id="be662-102">Mer information om hur globala databasreplikering fungerar i Cosmos-databasen finns [distribuera data globalt med Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="be662-102">For more information about how global database replication works in Cosmos DB, see [Distribute data globally with Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span></span>

## <span data-ttu-id="be662-103"><a id="addregion"></a>Lägg till den globala databasen regioner med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be662-103"><a id="addregion"></a>Add global database regions using hello Azure Portal</span></span>
<span data-ttu-id="be662-104">Azure Cosmos-DB är tillgänglig i alla [Azure-regioner] [ azureregions] över hela världen.</span><span class="sxs-lookup"><span data-stu-id="be662-104">Azure Cosmos DB is available in all [Azure regions][azureregions] world-wide.</span></span> <span data-ttu-id="be662-105">När du har valt hello konsekvenskontroll standardnivån för ditt konto, kan du associera en eller flera regioner (beroende på ditt val av standard konsekvenskontroll nivå och global distributionsplatsen behov).</span><span class="sxs-lookup"><span data-stu-id="be662-105">After selecting hello default consistency level for your database account, you can associate one or more regions (depending on your choice of default consistency level and global distribution needs).</span></span>

1. <span data-ttu-id="be662-106">I hello [Azure-portalen](https://portal.azure.com/), i hello vänstra fältet, klicka på **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="be662-106">In hello [Azure portal](https://portal.azure.com/), in hello left bar, click **Azure Cosmos DB**.</span></span>
2. <span data-ttu-id="be662-107">I hello **Azure Cosmos DB** bladet, Välj hello databasen konto toomodify.</span><span class="sxs-lookup"><span data-stu-id="be662-107">In hello **Azure Cosmos DB** blade, select hello database account toomodify.</span></span>
3. <span data-ttu-id="be662-108">I hello-kontoblad klickar du på **replikera data globalt** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="be662-108">In hello account blade, click **Replicate data globally** from hello menu.</span></span>
4. <span data-ttu-id="be662-109">I hello **replikera data globalt** bladet välj hello regioner tooadd eller ta bort genom att klicka på områden i hello mappning och klickar sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="be662-109">In hello **Replicate data globally** blade, select hello regions tooadd or remove by clicking regions in hello map, and then click **Save**.</span></span> <span data-ttu-id="be662-110">Det finns en kostnad tooadding regioner, se hello [sida med priser](https://azure.microsoft.com/pricing/details/documentdb/) eller hello [distribuera data globalt med DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) artikel för mer information.</span><span class="sxs-lookup"><span data-stu-id="be662-110">There is a cost tooadding regions, see hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/) or hello [Distribute data globally with DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) article for more information.</span></span>
   
    ![På hello områden i hello kartan tooadd eller ta bort dem.][1]
    
<span data-ttu-id="be662-112">När du lägger till en andra region hello **manuell redundans** är aktiverat på hello **replikera data globalt** bladet i hello portal.</span><span class="sxs-lookup"><span data-stu-id="be662-112">Once you add a second region, hello **Manual Failover** option is enabled on hello **Replicate data globally** blade in hello portal.</span></span> <span data-ttu-id="be662-113">Du kan använda det här alternativet tootest hello failover-processen eller ändra hello primära skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="be662-113">You can use this option tootest hello failover process or change hello primary write region.</span></span> <span data-ttu-id="be662-114">När du lägger till en tredje region hello **redundans prioriteter** är aktiverat på hello samma bladet så att du kan ändra hello redundans för läsning.</span><span class="sxs-lookup"><span data-stu-id="be662-114">Once you add a third region, hello **Failover Priorities** option is enabled on hello same blade so that you can change hello failover order for reads.</span></span>  

### <a name="selecting-global-database-regions"></a><span data-ttu-id="be662-115">Markera den globala databasen regioner</span><span class="sxs-lookup"><span data-stu-id="be662-115">Selecting global database regions</span></span>
<span data-ttu-id="be662-116">Det finns två vanliga scenarier för att konfigurera två eller flera områden:</span><span class="sxs-lookup"><span data-stu-id="be662-116">There are two common scenarios for configuring two or more regions:</span></span>

1. <span data-ttu-id="be662-117">Leverera låg latens åtkomst toodata tooend användare oavsett var de befinner sig runt hello världen</span><span class="sxs-lookup"><span data-stu-id="be662-117">Delivering low-latency access toodata tooend users no matter where they are located around hello globe</span></span>
2. <span data-ttu-id="be662-118">Lägga till regionala återhämtning för affärskontinuitet och haveriberedskap (BCDR)</span><span class="sxs-lookup"><span data-stu-id="be662-118">Adding regional resiliency for business continuity and disaster recovery (BCDR)</span></span>

<span data-ttu-id="be662-119">För användare-med låg latens tooend bör toodeploy både hello program och lägga till Azure Cosmos DB i hello områden som är motsvarar toowhere hello programmets användare finns.</span><span class="sxs-lookup"><span data-stu-id="be662-119">For delivering low-latency tooend-users, it is recommended toodeploy both hello application and add Azure Cosmos DB in hello regions thats correspond toowhere hello application's users are located.</span></span>

<span data-ttu-id="be662-120">BCDR, rekommenderas för tooadd regioner baserat på hello region-par som beskrivs i hello [Business affärskontinuitet och haveriberedskap återställning (BCDR): parad Azure-regioner] [ bcdr] artikel.</span><span class="sxs-lookup"><span data-stu-id="be662-120">For BCDR, it is recommended tooadd regions based on hello region pairs described in hello [Business continuity and disaster recovery (BCDR): Azure Paired Regions][bcdr] article.</span></span>

<!--

## <a id="selectwriteregion"></a>Select hello write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive hello write (insert, upsert, replace, delete) requests. tooset hello active write region, do hello following  


1. In hello **Azure Cosmos DB** blade, select hello database account toomodify.
2. In hello account blade, click **Replicate data globally** from hello menu.
3. In hello **Replicate data globally** blade, click **Manual Failover** from hello top bar.
    ![Change hello write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region toobecome hello new write region, click hello checkbox tooconfirm triggering a failover, and click OK
    ![Change hello write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
