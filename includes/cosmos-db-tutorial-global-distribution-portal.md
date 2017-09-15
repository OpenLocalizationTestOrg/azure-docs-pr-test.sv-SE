
<span data-ttu-id="5efcd-101">Du kan lära dig om Azure Cosmos DB globala distribution i den här Azure fredag video med Scott Hanselman och huvudnamn tekniker Manager Karthik Raman.</span><span class="sxs-lookup"><span data-stu-id="5efcd-101">You can learn about Azure Cosmos DB global distribution in this Azure Friday video with Scott Hanselman and Principal Engineering Manager Karthik Raman.</span></span>

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

<span data-ttu-id="5efcd-102">Mer information om hur globala databasreplikering fungerar i Cosmos-databasen finns [distribuera data globalt med Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="5efcd-102">For more information about how global database replication works in Cosmos DB, see [Distribute data globally with Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span></span>

## <span data-ttu-id="5efcd-103"><a id="addregion"></a>Lägga till den globala databasen områden med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5efcd-103"><a id="addregion"></a>Add global database regions using the Azure Portal</span></span>
<span data-ttu-id="5efcd-104">Azure Cosmos-DB är tillgänglig i alla [Azure-regioner] [ azureregions] över hela världen.</span><span class="sxs-lookup"><span data-stu-id="5efcd-104">Azure Cosmos DB is available in all [Azure regions][azureregions] world-wide.</span></span> <span data-ttu-id="5efcd-105">När du har valt standardnivån för konsekvens för ditt konto, kan du associera en eller flera regioner (beroende på ditt val av standard konsekvenskontroll nivå och global distributionsplatsen behov).</span><span class="sxs-lookup"><span data-stu-id="5efcd-105">After selecting the default consistency level for your database account, you can associate one or more regions (depending on your choice of default consistency level and global distribution needs).</span></span>

1. <span data-ttu-id="5efcd-106">I den [Azure-portalen](https://portal.azure.com/), i fältet till vänster klickar du på **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="5efcd-106">In the [Azure portal](https://portal.azure.com/), in the left bar, click **Azure Cosmos DB**.</span></span>
2. <span data-ttu-id="5efcd-107">I den **Azure Cosmos DB** bladet väljer databasen konto för att ändra.</span><span class="sxs-lookup"><span data-stu-id="5efcd-107">In the **Azure Cosmos DB** blade, select the database account to modify.</span></span>
3. <span data-ttu-id="5efcd-108">I bladet konto klickar du på **replikera data globalt** på menyn.</span><span class="sxs-lookup"><span data-stu-id="5efcd-108">In the account blade, click **Replicate data globally** from the menu.</span></span>
4. <span data-ttu-id="5efcd-109">I den **replikera data globalt** bladet välj regioner att lägga till eller ta bort genom att klicka på regioner på kartan klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5efcd-109">In the **Replicate data globally** blade, select the regions to add or remove by clicking regions in the map, and then click **Save**.</span></span> <span data-ttu-id="5efcd-110">Det finns en kostnad för att lägga till regioner, finns det [sida med priser](https://azure.microsoft.com/pricing/details/documentdb/) eller [distribuera data globalt med DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) artikel för mer information.</span><span class="sxs-lookup"><span data-stu-id="5efcd-110">There is a cost to adding regions, see the [pricing page](https://azure.microsoft.com/pricing/details/documentdb/) or the [Distribute data globally with DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) article for more information.</span></span>
   
    ![Klicka på regioner på kartan för att lägga till eller ta bort dem.][1]
    
<span data-ttu-id="5efcd-112">När du lägger till en andra region i **manuell redundans** är aktiverat på den **replikera data globalt** blad i portalen.</span><span class="sxs-lookup"><span data-stu-id="5efcd-112">Once you add a second region, the **Manual Failover** option is enabled on the **Replicate data globally** blade in the portal.</span></span> <span data-ttu-id="5efcd-113">Du kan använda det här alternativet för att testa redundans processen eller ändra den primära write-regionen.</span><span class="sxs-lookup"><span data-stu-id="5efcd-113">You can use this option to test the failover process or change the primary write region.</span></span> <span data-ttu-id="5efcd-114">När du lägger till en tredje region i **redundans prioriteter** är aktiverat på samma bladet så att du kan ändra ordning för växling vid fel för läsning.</span><span class="sxs-lookup"><span data-stu-id="5efcd-114">Once you add a third region, the **Failover Priorities** option is enabled on the same blade so that you can change the failover order for reads.</span></span>  

### <a name="selecting-global-database-regions"></a><span data-ttu-id="5efcd-115">Markera den globala databasen regioner</span><span class="sxs-lookup"><span data-stu-id="5efcd-115">Selecting global database regions</span></span>
<span data-ttu-id="5efcd-116">Det finns två vanliga scenarier för att konfigurera två eller flera områden:</span><span class="sxs-lookup"><span data-stu-id="5efcd-116">There are two common scenarios for configuring two or more regions:</span></span>

1. <span data-ttu-id="5efcd-117">Leverera låg latens åtkomst till data till slutanvändare oavsett var de befinner sig över hela världen</span><span class="sxs-lookup"><span data-stu-id="5efcd-117">Delivering low-latency access to data to end users no matter where they are located around the globe</span></span>
2. <span data-ttu-id="5efcd-118">Lägga till regionala återhämtning för affärskontinuitet och haveriberedskap (BCDR)</span><span class="sxs-lookup"><span data-stu-id="5efcd-118">Adding regional resiliency for business continuity and disaster recovery (BCDR)</span></span>

<span data-ttu-id="5efcd-119">För att leverera låg latens för slutanvändarna, rekommenderas att distribuera både programmet och lägga till Azure Cosmos-DB i områden som är motsvarar där programmets användare finns.</span><span class="sxs-lookup"><span data-stu-id="5efcd-119">For delivering low-latency to end-users, it is recommended to deploy both the application and add Azure Cosmos DB in the regions thats correspond to where the application's users are located.</span></span>

<span data-ttu-id="5efcd-120">För BCDR, rekommenderas du lägger till regioner baserat på region-par som beskrivs i den [Business affärskontinuitet och haveriberedskap återställning (BCDR): parad Azure-regioner] [ bcdr] artikel.</span><span class="sxs-lookup"><span data-stu-id="5efcd-120">For BCDR, it is recommended to add regions based on the region pairs described in the [Business continuity and disaster recovery (BCDR): Azure Paired Regions][bcdr] article.</span></span>

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
