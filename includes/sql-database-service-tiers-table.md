<!--
Used in:
sql-database-performance-guidance.md  
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->

### <a name="basic-service-tier"></a>Grundläggande tjänstenivå
| **Prestandanivå** | **Basic** |
| --- | :---: |
| Maximala DTU:er | 5 |
| Maximal databasstorlek* |2 GB|
| Maximal InMemory-OLTP-lagring |Saknas |
| Maximalt antal samtidiga arbetare (antal begäranden) |30 |
| Maximalt antal samtidiga inloggningar |30 |
| Maximalt antal samtidiga sessioner |300 |
|||

### <a name="standard-service-tier"></a>Standardtjänstenivå
| **Prestandanivå** | **S0** | **S1** | **S2** | **S3** |
| --- |---:| ---:|---:|---:|---:|
| Maximala DTU:er | 10 | 20 | 50 | 100 |
| Maximal databasstorlek* | 250 GB| 250 GB | 250 GB | 250 GB |
| Maximal InMemory-OLTP-lagring | Saknas | Saknas | Saknas | Saknas |
| Maximalt antal samtidiga arbetare (antal begäranden)| 60 | 90 | 120 | 200 |
| Maximalt antal samtidiga inloggningar | 60 | 90 | 120 | 200 |
| Maximalt antal samtidiga sessioner |600 | 900 | 1200 | 2400 |
||||||

### <a name="premium-service-tier"></a>Premium tjänstenivån 
| **Prestandanivå** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| --- |---:|---:|---:|---:|---:|---:|
| Maximala DTU:er | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Maximal databasstorlek* | 500 GB | 500 GB | 500 GB | 500 GB | 4 TB | 4 TB |
| Maximal InMemory-OLTP-lagring | 1 GB | 2 GB | 4 GB | 8 GB | 14 GB | 32 GB |
| Maximalt antal samtidiga arbetare (antal begäranden)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximalt antal samtidiga inloggningar | 200 | 400| 800| 1600| 2400| 6400 |
| Maximalt antal samtidiga sessioner | 30000| 30000| 30000| 30000| 30000| 30000 |
|||||||

### <a name="premium-rs-service-tier"></a>Premiumnivån för RS 
| **Prestandanivå** | **PRS1** | **PRS2** | **PRS4** | **PRS6** |
| --- |---:|---:|---:|---:|---:|---:|
| Maximala DTU:er | 125 | 250 | 500 | 1000 |
| Maximal databasstorlek* | 500 GB | 500 GB | 500 GB | 500 GB |
| Maximal InMemory-OLTP-lagring | 1 GB | 2 GB | 4 GB | 8 GB |
| Maximalt antal samtidiga arbetare (antal begäranden)| 200 | 400 | 800 | 1600 |
| Maximalt antal samtidiga inloggningar | 200 | 400| 800| 1600|
| Maximalt antal samtidiga sessioner | 30000| 30000| 30000| 30000|
|||||||

\*Maxstorlek för databasen refererar toohello maxstorleken för hello data i hello-databas. 
