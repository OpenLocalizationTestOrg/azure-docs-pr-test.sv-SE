
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, hello following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
<span data-ttu-id="22005-101">toosave kostnader, du kan pausa och återuppta beräkna resurser på begäran.</span><span class="sxs-lookup"><span data-stu-id="22005-101">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="22005-102">Till exempel om du inte använder hello databasen under hello natten och helger, kan du pausa under dessa tider och återuppta under hello dag.</span><span class="sxs-lookup"><span data-stu-id="22005-102">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="22005-103">Du kommer inte att debiteras för dwu: er medan hello databasen har pausats.</span><span class="sxs-lookup"><span data-stu-id="22005-103">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="22005-104">När du pausar en-databas:</span><span class="sxs-lookup"><span data-stu-id="22005-104">When you pause a database:</span></span>

* <span data-ttu-id="22005-105">Beräknings-och minnesresurser returneras toohello pool med tillgängliga resurser i hello Datacenter</span><span class="sxs-lookup"><span data-stu-id="22005-105">Compute and memory resources are returned toohello pool of available resources in hello data center</span></span>
* <span data-ttu-id="22005-106">DWU är noll för hello varaktighet hello paus.</span><span class="sxs-lookup"><span data-stu-id="22005-106">DWU costs are zero for hello duration of hello pause.</span></span>
* <span data-ttu-id="22005-107">Datalagring påverkas inte och dina data förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="22005-107">Data storage is not affected and your data stays intact.</span></span> 
* <span data-ttu-id="22005-108">SQL Data Warehouse avbryter alla åtgärder som körs eller står i kö.</span><span class="sxs-lookup"><span data-stu-id="22005-108">SQL Data Warehouse cancels all running or queued operations.</span></span>

