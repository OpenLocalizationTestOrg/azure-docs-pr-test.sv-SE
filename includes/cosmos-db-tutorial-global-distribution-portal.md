
Du kan lära dig om Azure Cosmos DB globala distribution i den här Azure fredag video med Scott Hanselman och huvudnamn tekniker Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Mer information om hur globala databasreplikering fungerar i Cosmos-databasen finns [distribuera data globalt med Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).

## <a id="addregion"></a>Lägg till den globala databasen regioner med hjälp av hello Azure-portalen
Azure Cosmos-DB är tillgänglig i alla [Azure-regioner] [ azureregions] över hela världen. När du har valt hello konsekvenskontroll standardnivån för ditt konto, kan du associera en eller flera regioner (beroende på ditt val av standard konsekvenskontroll nivå och global distributionsplatsen behov).

1. I hello [Azure-portalen](https://portal.azure.com/), i hello vänstra fältet, klicka på **Azure Cosmos DB**.
2. I hello **Azure Cosmos DB** bladet, Välj hello databasen konto toomodify.
3. I hello-kontoblad klickar du på **replikera data globalt** hello-menyn.
4. I hello **replikera data globalt** bladet välj hello regioner tooadd eller ta bort genom att klicka på områden i hello mappning och klickar sedan på **spara**. Det finns en kostnad tooadding regioner, se hello [sida med priser](https://azure.microsoft.com/pricing/details/documentdb/) eller hello [distribuera data globalt med DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) artikel för mer information.
   
    ![På hello områden i hello kartan tooadd eller ta bort dem.][1]
    
När du lägger till en andra region hello **manuell redundans** är aktiverat på hello **replikera data globalt** bladet i hello portal. Du kan använda det här alternativet tootest hello failover-processen eller ändra hello primära skrivåtgärder region. När du lägger till en tredje region hello **redundans prioriteter** är aktiverat på hello samma bladet så att du kan ändra hello redundans för läsning.  

### <a name="selecting-global-database-regions"></a>Markera den globala databasen regioner
Det finns två vanliga scenarier för att konfigurera två eller flera områden:

1. Leverera låg latens åtkomst toodata tooend användare oavsett var de befinner sig runt hello världen
2. Lägga till regionala återhämtning för affärskontinuitet och haveriberedskap (BCDR)

För användare-med låg latens tooend bör toodeploy både hello program och lägga till Azure Cosmos DB i hello områden som är motsvarar toowhere hello programmets användare finns.

BCDR, rekommenderas för tooadd regioner baserat på hello region-par som beskrivs i hello [Business affärskontinuitet och haveriberedskap återställning (BCDR): parad Azure-regioner] [ bcdr] artikel.

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
