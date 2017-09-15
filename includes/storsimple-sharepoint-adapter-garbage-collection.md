<!--author=SharS last changed: 9/17/15-->

<span data-ttu-id="86bc0-101">I den här proceduren ska du:</span><span class="sxs-lookup"><span data-stu-id="86bc0-101">In this procedure, you will:</span></span>

1. <span data-ttu-id="86bc0-102">[Förbereda för att köra programfilen Underhållaren](#to-prepare-to-run-the-maintainer) .</span><span class="sxs-lookup"><span data-stu-id="86bc0-102">[Prepare to run the Maintainer executable](#to-prepare-to-run-the-maintainer) .</span></span>
2. <span data-ttu-id="86bc0-103">[Förbereda innehållsdatabasen och Papperskorgen för omedelbar borttagning av överblivna Blobbar](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span><span class="sxs-lookup"><span data-stu-id="86bc0-103">[Prepare the content database and Recycle Bin for immediate deletion of orphaned BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span></span>
3. <span data-ttu-id="86bc0-104">[Kör Maintainer.exe](#to-run-the-maintainer).</span><span class="sxs-lookup"><span data-stu-id="86bc0-104">[Run Maintainer.exe](#to-run-the-maintainer).</span></span>
4. <span data-ttu-id="86bc0-105">[Återställa innehållsdatabasen och inställningar för Papperskorgen](#to-revert-the-content-database-and-recycle-bin-settings).</span><span class="sxs-lookup"><span data-stu-id="86bc0-105">[Revert the content database and Recycle Bin settings](#to-revert-the-content-database-and-recycle-bin-settings).</span></span>

#### <a name="to-prepare-to-run-the-maintainer"></a><span data-ttu-id="86bc0-106">Förbereda att köra Underhållaren</span><span class="sxs-lookup"><span data-stu-id="86bc0-106">To prepare to run the Maintainer</span></span>
1. <span data-ttu-id="86bc0-107">Öppna hanteringsgränssnittet för SharePoint 2013 som administratör på frontend webbservern.</span><span class="sxs-lookup"><span data-stu-id="86bc0-107">On the Web front-end server, open the SharePoint 2013 Management Shell as an administrator.</span></span>
2. <span data-ttu-id="86bc0-108">Navigera till mappen *starta enheten*: \Program\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span><span class="sxs-lookup"><span data-stu-id="86bc0-108">Navigate to the folder *boot drive*:\Program Files\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span></span>
3. <span data-ttu-id="86bc0-109">Byt namn på **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** till **web.config**.</span><span class="sxs-lookup"><span data-stu-id="86bc0-109">Rename **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** to **web.config**.</span></span>
4. <span data-ttu-id="86bc0-110">Använd `aspnet_regiis -pdf connectionStrings` att dekryptera filen web.config.</span><span class="sxs-lookup"><span data-stu-id="86bc0-110">Use `aspnet_regiis -pdf connectionStrings` to decrypt the web.config file.</span></span>
5. <span data-ttu-id="86bc0-111">I dekrypterade web.config-filen under den `connectionStrings` nod, Lägg till anslutningssträng för SQL server-instans och innehållsdatabasens namn.</span><span class="sxs-lookup"><span data-stu-id="86bc0-111">In the decrypted web.config file, under the `connectionStrings` node, add the connection string for your SQL server instance and the content database name.</span></span> <span data-ttu-id="86bc0-112">Se följande exempel.</span><span class="sxs-lookup"><span data-stu-id="86bc0-112">See the following example.</span></span>
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. <span data-ttu-id="86bc0-113">Använd `aspnet_regiis –pef connectionStrings` för att kryptera filen web.config.</span><span class="sxs-lookup"><span data-stu-id="86bc0-113">Use `aspnet_regiis –pef connectionStrings` to re-encrypt the web.config file.</span></span> 
7. <span data-ttu-id="86bc0-114">Byt namn på web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span><span class="sxs-lookup"><span data-stu-id="86bc0-114">Rename web.config to Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span></span> 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a><span data-ttu-id="86bc0-115">Om du vill förbereda innehållet frånkopplade databasen och Papperskorgen att direkt ta bort Blobbar</span><span class="sxs-lookup"><span data-stu-id="86bc0-115">To prepare the content database and Recycle Bin to immediately delete orphaned BLOBs</span></span>
1. <span data-ttu-id="86bc0-116">Kör följande uppdateringsfrågor för måldatabasen innehåll i SQL Management Studio på SQL-Server:</span><span class="sxs-lookup"><span data-stu-id="86bc0-116">On the SQL Server, in SQL Management Studio, run the following update queries for the target content database:</span></span> 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. <span data-ttu-id="86bc0-117">På frontend webbserver, under **Central Administration**, redigera den **Web Application allmänna inställningar för** för önskad innehållsdatabas tillfälligt inaktivera Papperskorgen.</span><span class="sxs-lookup"><span data-stu-id="86bc0-117">On the web front-end server, under **Central Administration**, edit the **Web Application General Settings** for the desired content database to temporarily disable the Recycle Bin.</span></span> <span data-ttu-id="86bc0-118">Den här åtgärden kommer även tömma Papperskorgen för alla relaterade webbplatssamlingar.</span><span class="sxs-lookup"><span data-stu-id="86bc0-118">This action will also empty the Recycle Bin for any related site collections.</span></span> <span data-ttu-id="86bc0-119">Gör detta genom att klicka på **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="86bc0-119">To do this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="86bc0-120">Ange den **Status för Papperskorgen** till **OFF**.</span><span class="sxs-lookup"><span data-stu-id="86bc0-120">Set the **Recycle Bin Status** to **OFF**.</span></span>
   
    ![Web Application allmänna inställningar](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a><span data-ttu-id="86bc0-122">Att köra Underhållaren</span><span class="sxs-lookup"><span data-stu-id="86bc0-122">To run the Maintainer</span></span>
* <span data-ttu-id="86bc0-123">På den web front-end-servern, SharePoint 2013 Management Shell, kör av Underhållaren på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="86bc0-123">On the web front-end server, in the SharePoint 2013 Management Shell, run the Maintainer as follows:</span></span>
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > <span data-ttu-id="86bc0-124">Endast den `GarbageCollection` åtgärden stöds för StorSimple just nu.</span><span class="sxs-lookup"><span data-stu-id="86bc0-124">Only the `GarbageCollection` operation is supported for StorSimple at this time.</span></span> <span data-ttu-id="86bc0-125">Observera också att de parametrar som utfärdats för Microsoft.Data.SqlRemoteBlobs.Maintainer.exe är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="86bc0-125">Also note that the parameters issued for Microsoft.Data.SqlRemoteBlobs.Maintainer.exe are case sensitive.</span></span> 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a><span data-ttu-id="86bc0-126">Att återställa databasen och inställningar för Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="86bc0-126">To revert the content database and Recycle Bin settings</span></span>
1. <span data-ttu-id="86bc0-127">Kör följande uppdateringsfrågor för måldatabasen innehåll i SQL Management Studio på SQL-Server:</span><span class="sxs-lookup"><span data-stu-id="86bc0-127">On the SQL Server, in SQL Management Studio, run the following update queries for the target content database:</span></span>
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. <span data-ttu-id="86bc0-128">På frontend webbserver, i **Central Administration**, redigera den **Web Application allmänna inställningar för** för önskad innehållsdatabas återaktivera Papperskorgen.</span><span class="sxs-lookup"><span data-stu-id="86bc0-128">On the web front-end server, in **Central Administration**, edit the **Web Application General Settings** for the desired content database to re-enable the Recycle Bin.</span></span> <span data-ttu-id="86bc0-129">Gör detta genom att klicka på **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="86bc0-129">To do this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="86bc0-130">Ange återvinning Bin Status till **på**.</span><span class="sxs-lookup"><span data-stu-id="86bc0-130">Set the Recycle Bin Status to **ON**.</span></span>

