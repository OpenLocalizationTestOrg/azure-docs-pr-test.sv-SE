
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, hello following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
toosave kostnader, du kan pausa och återuppta beräkna resurser på begäran. Till exempel om du inte använder hello databasen under hello natten och helger, kan du pausa under dessa tider och återuppta under hello dag. Du kommer inte att debiteras för dwu: er medan hello databasen har pausats.

När du pausar en-databas:

* Beräknings-och minnesresurser returneras toohello pool med tillgängliga resurser i hello Datacenter
* DWU är noll för hello varaktighet hello paus.
* Datalagring påverkas inte och dina data förblir intakt. 
* SQL Data Warehouse avbryter alla åtgärder som körs eller står i kö.

