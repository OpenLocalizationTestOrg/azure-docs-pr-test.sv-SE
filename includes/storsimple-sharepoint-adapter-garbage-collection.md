<!--author=SharS last changed: 9/17/15-->

<span data-ttu-id="d1ea7-101">I den här proceduren ska du:</span><span class="sxs-lookup"><span data-stu-id="d1ea7-101">In this procedure, you will:</span></span>

1. <span data-ttu-id="d1ea7-102">[Förbereda toorun hello Underhållaren körbara](#to-prepare-to-run-the-maintainer) .</span><span class="sxs-lookup"><span data-stu-id="d1ea7-102">[Prepare toorun hello Maintainer executable](#to-prepare-to-run-the-maintainer) .</span></span>
2. <span data-ttu-id="d1ea7-103">[Förbereda hello innehållsdatabasen och Papperskorgen för omedelbar borttagning av överblivna Blobbar](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span><span class="sxs-lookup"><span data-stu-id="d1ea7-103">[Prepare hello content database and Recycle Bin for immediate deletion of orphaned BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span></span>
3. <span data-ttu-id="d1ea7-104">[Kör Maintainer.exe](#to-run-the-maintainer).</span><span class="sxs-lookup"><span data-stu-id="d1ea7-104">[Run Maintainer.exe](#to-run-the-maintainer).</span></span>
4. <span data-ttu-id="d1ea7-105">[Återställa hello innehållsdatabasen och inställningar för Papperskorgen](#to-revert-the-content-database-and-recycle-bin-settings).</span><span class="sxs-lookup"><span data-stu-id="d1ea7-105">[Revert hello content database and Recycle Bin settings](#to-revert-the-content-database-and-recycle-bin-settings).</span></span>

#### <a name="tooprepare-toorun-hello-maintainer"></a><span data-ttu-id="d1ea7-106">tooprepare toorun hello Underhållaren</span><span class="sxs-lookup"><span data-stu-id="d1ea7-106">tooprepare toorun hello Maintainer</span></span>
1. <span data-ttu-id="d1ea7-107">Öppna hello SharePoint 2013 Management Shell på hello Web front-end-server, som en administratör.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-107">On hello Web front-end server, open hello SharePoint 2013 Management Shell as an administrator.</span></span>
2. <span data-ttu-id="d1ea7-108">Navigera toohello mappen *starta enheten*: \Program\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-108">Navigate toohello folder *boot drive*:\Program Files\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span></span>
3. <span data-ttu-id="d1ea7-109">Byt namn på **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** för**web.config**.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-109">Rename **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** too**web.config**.</span></span>
4. <span data-ttu-id="d1ea7-110">Använd `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-110">Use `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config file.</span></span>
5. <span data-ttu-id="d1ea7-111">I hello dekrypterade web.config-filen under hello `connectionStrings` , lägga till hello anslutningssträngen för SQL server-instans och hello-innehållsdatabasens namn.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-111">In hello decrypted web.config file, under hello `connectionStrings` node, add hello connection string for your SQL server instance and hello content database name.</span></span> <span data-ttu-id="d1ea7-112">Se följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-112">See hello following example.</span></span>
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. <span data-ttu-id="d1ea7-113">Använd `aspnet_regiis –pef connectionStrings` toore-kryptera hello web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-113">Use `aspnet_regiis –pef connectionStrings` toore-encrypt hello web.config file.</span></span> 
7. <span data-ttu-id="d1ea7-114">Byt namn på web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-114">Rename web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span></span> 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a><span data-ttu-id="d1ea7-115">ta bort överblivna Blobbar tooprepare hello innehållsdatabasen och Papperskorgen tooimmediately</span><span class="sxs-lookup"><span data-stu-id="d1ea7-115">tooprepare hello content database and Recycle Bin tooimmediately delete orphaned BLOBs</span></span>
1. <span data-ttu-id="d1ea7-116">Kör hello följande uppdateringsfrågor för hello innehåll måldatabasen på hello SQL Server i SQL Management Studio:</span><span class="sxs-lookup"><span data-stu-id="d1ea7-116">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span> 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. <span data-ttu-id="d1ea7-117">På hello web front-end-server under **Central Administration**, redigera hello **Web Application allmänna inställningar för** för hello önskad innehållsdatabas tootemporarily inaktivera hello Papperskorgen.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-117">On hello web front-end server, under **Central Administration**, edit hello **Web Application General Settings** for hello desired content database tootemporarily disable hello Recycle Bin.</span></span> <span data-ttu-id="d1ea7-118">Den här åtgärden kommer också att tom hello Papperskorgen om eventuella relaterade webbplatssamlingar.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-118">This action will also empty hello Recycle Bin for any related site collections.</span></span> <span data-ttu-id="d1ea7-119">toodo, genom att välja **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-119">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="d1ea7-120">Ange hello **Återanvänd Bin Status** för**OFF**.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-120">Set hello **Recycle Bin Status** too**OFF**.</span></span>
   
    ![Web Application allmänna inställningar](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a><span data-ttu-id="d1ea7-122">toorun hello Underhållaren</span><span class="sxs-lookup"><span data-stu-id="d1ea7-122">toorun hello Maintainer</span></span>
* <span data-ttu-id="d1ea7-123">På hello web front-end-server, hello SharePoint 2013 Management Shell, kör hello Underhållaren på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d1ea7-123">On hello web front-end server, in hello SharePoint 2013 Management Shell, run hello Maintainer as follows:</span></span>
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > <span data-ttu-id="d1ea7-124">Endast hello `GarbageCollection` åtgärden stöds för StorSimple just nu.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-124">Only hello `GarbageCollection` operation is supported for StorSimple at this time.</span></span> <span data-ttu-id="d1ea7-125">Observera också att hello parametrar som utfärdats för Microsoft.Data.SqlRemoteBlobs.Maintainer.exe är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-125">Also note that hello parameters issued for Microsoft.Data.SqlRemoteBlobs.Maintainer.exe are case sensitive.</span></span> 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a><span data-ttu-id="d1ea7-126">toorevert hello innehållsdatabasen och inställningar för Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="d1ea7-126">toorevert hello content database and Recycle Bin settings</span></span>
1. <span data-ttu-id="d1ea7-127">Kör hello följande uppdateringsfrågor för hello innehåll måldatabasen på hello SQL Server i SQL Management Studio:</span><span class="sxs-lookup"><span data-stu-id="d1ea7-127">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span>
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. <span data-ttu-id="d1ea7-128">På hello web front-end-server, i **Central Administration**, redigera hello **Web Application allmänna inställningar för** för hello önskad innehållsdatabas toore aktivera hello Papperskorgen.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-128">On hello web front-end server, in **Central Administration**, edit hello **Web Application General Settings** for hello desired content database toore-enable hello Recycle Bin.</span></span> <span data-ttu-id="d1ea7-129">toodo, genom att välja **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-129">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="d1ea7-130">Ange hello Återanvänd Bin Status för**på**.</span><span class="sxs-lookup"><span data-stu-id="d1ea7-130">Set hello Recycle Bin Status too**ON**.</span></span>

