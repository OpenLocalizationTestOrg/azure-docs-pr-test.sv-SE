<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Gränser för grundläggande elastisk pool

| Poolstorlek (eDTU:er)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Största lagringsutrymme per pool* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Maximalt In-Memory OLTP-lagringsutrymme per pool | Saknas | Saknas | Saknas | Saknas | Saknas | Saknas | Saknas | Saknas |
| Maximalt antal databaser per pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximalt antal samtidiga arbetare (begäranden) per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximalt antal samtidiga inloggningar per pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Minimalt antal eDTU:er per databas | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Maximalt antal eDTU:er per databas | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximalt datalagringsutrymme per databas | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standardgränser för elastisk pool

| Poolstorlek (eDTU:er)  | **50** | **100** | **200**** | **300**** | **400**** | **800****| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Största lagringsutrymme per pool* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maximalt In-Memory OLTP-lagringsutrymme per pool | Saknas | Saknas | Saknas | Saknas | Saknas | Saknas | 
| Maximalt antal databaser per pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximalt antal samtidiga arbetare (begäranden) per pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximalt antal samtidiga inloggningar per pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Minsta antal eDTU:er per databas** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Maximalt antal eDTU:er per databas** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximalt datalagringsutrymme per databas | 50 GB | 100 GB | 200 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Standardgränser för elastisk pool (forts.) 

| Poolstorlek (eDTU:er)  |  **1200**** | **1600**** | **2000**** | **2500**** | **3000**** |
|:---|---:|---:|---:| ---: | ---: |
| Största lagringsutrymme per pool* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maximalt In-Memory OLTP-lagringsutrymme per pool | Saknas | Saknas | Saknas | Saknas | Saknas | 
| Maximalt antal databaser per pool | 500 | 500 | 500 | 500 | 500 | 
| Maximalt antal samtidiga arbetare (begäranden) per pool |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximalt antal samtidiga inloggningar per pool |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minsta antal eDTU:er per databas** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Maximalt antal eDTU:er per databas** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maximalt datalagringsutrymme per databas | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Gränser för Premium elastisk pool

| Poolstorlek (eDTU:er)  | **125** | **250** | **500** | **1000** | **1500*****| 
|:---|---:|---:|---:| ---: | ---: | 
| Största lagringsutrymme per pool* | 250 GB | 500 GB | 750 GB | 1 TB | 1,5 TB | 
| Maximalt In-Memory OLTP-lagringsutrymme per pool | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Maximalt antal databaser per pool | 50 | 100 | 100 | 100 | 100 |  
| Maximalt antal samtidiga arbetare per pool (begäranden) | 200 | 400 | 800 | 1600 |  2400 | 
| Maximalt antal samtidiga inloggningar per pool | 200 | 400 | 800 | 1600 |  2400 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalt antal eDTU:er per databas | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000 | 
| Maximalt antal eDTU:er per databas | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000 |
| Maximalt datalagringsutrymme per databas | 250 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Premiumgränser för elastisk pool (forts.) 

| Poolstorlek (eDTU:er) | **2000***** | **2500***** | **3000***** | **3500***** | **4000*****|
|:---|---:|---:|---:| ---: | ---: | 
| Största lagringsutrymme per pool* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Maximalt In-Memory OLTP-lagringsutrymme per pool | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Maximalt antal databaser per pool | 100 | 100 | 100 | 100 | 100 | 
| Maximalt antal samtidiga arbetare (begäranden) per pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximalt antal samtidiga inloggningar per pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalt antal eDTU:er per databas | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 |  0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximalt antal eDTU:er per databas | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximalt datalagringsutrymme per databas | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Gränser för elastiska Premium RS-pooler

| Poolstorlek (eDTU:er)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Största lagringsutrymme per pool* | 250 GB| 500 GB | 750 GB | 750 GB |
| Maximalt In-Memory OLTP-lagringsutrymme per pool | 1 GB | 2 GB | 4 GB | 10 GB |
| Maximalt antal databaser per pool | 50 | 100 | 100 | 100 |
| Maximalt antal samtidiga arbetare (begäranden) per pool | 200 | 400 | 800 | 1600 |
| Maximalt antal samtidiga inloggningar per pool | 200 | 400 | 800 | 1600 |
| Maximalt antal samtidiga sessioner per pool | 30000 | 30000 | 30000 | 30000 |
| Minimalt antal eDTU:er per databas | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Maximalt antal eDTU:er per databas | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Maximalt datalagringsutrymme per databas | 250 GB | 500 GB | 500 GB | 500 GB | 
||||||||

> [!IMPORTANT]
>\*Grupperade databaser dela poolens lagringsutrymme så att lagring av data i en elastisk pool är begränsad toohello mindre lagringsutrymme för återstående hello-pool eller maximal lagringskapacitet per databas. 
>
>
>\*\* Minsta/högsta antal eDTU:er per databas från 200 eDTU:er och fler finns för närvarande som en förhandsversion.
>
>\*\*\*Hej max data standardlagring per pool för Premium-pooler med 500 edtu: er eller flera är 750 GB. tooobtain Hej högre max data lagringsstorleken per Premium-pool för 1000 eller fler edtu: er, den här storleken måste väljas uttryckligen med hjälp av hello Azure-portalen [PowerShell](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-powershell), hello [Azure CLI](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-azure-cli), eller hello [REST API](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-rest-api). Premium-pooler med mer än 1 TB lagringsutrymme är för närvarande i förhandsversion i hello följande områden: oss East2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, söder Östasien, östra, Östra Australien, Kanada Central och Kanada Öst. Hej max lagringsutrymme per pool för alla andra regioner är för närvarande begränsad too750 GB.
>
