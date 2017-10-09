<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> <span data-ttu-id="903e2-101">När du gör ändringar toohello StorSimple nätverkskort för SharePoint RBS konfigurationen måste du vara inloggad med ett konto som tillhör toohello gruppen Domänadministratörer.</span><span class="sxs-lookup"><span data-stu-id="903e2-101">When making changes toohello StorSimple Adapter for SharePoint RBS configuration, you must be logged on with a user account that belongs toohello Domain Admins group.</span></span> <span data-ttu-id="903e2-102">Dessutom måste du komma åt hello konfigurationssidan från en webbläsare som körs på hello samma värden som Central Administration.</span><span class="sxs-lookup"><span data-stu-id="903e2-102">Additionally, you must access hello configuration page from a browser running on hello same host as Central Administration.</span></span>
> 
> 

#### <a name="tooconfigure-rbs"></a><span data-ttu-id="903e2-103">tooconfigure RBS</span><span class="sxs-lookup"><span data-stu-id="903e2-103">tooconfigure RBS</span></span>
1. <span data-ttu-id="903e2-104">Öppna hello Central Administration av SharePoint-sida och Bläddra för**systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="903e2-104">Open hello SharePoint Central Administration page, and browse too**System Settings**.</span></span> 
2. <span data-ttu-id="903e2-105">I hello **Azure StorSimple** klickar du på **konfigurera StorSimple nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="903e2-105">In hello **Azure StorSimple** section, click **Configure StorSimple Adapter**.</span></span>
   
    ![Konfigurera hello StorSimple-kort](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. <span data-ttu-id="903e2-107">På hello **konfigurera StorSimple nätverkskort** sidan:</span><span class="sxs-lookup"><span data-stu-id="903e2-107">On hello **Configure StorSimple Adapter** page:</span></span>
   
   1. <span data-ttu-id="903e2-108">Kontrollera att hello **Aktivera redigering sökväg** är markerad.</span><span class="sxs-lookup"><span data-stu-id="903e2-108">Make sure that hello **Enable editing path** check box is selected.</span></span>
   2. <span data-ttu-id="903e2-109">Hello i textrutan Ange hello Universal Naming Convention (UNC) för hello blobstore.</span><span class="sxs-lookup"><span data-stu-id="903e2-109">In hello text box, type hello Universal Naming Convention (UNC) path of hello BLOB store.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="903e2-110">hello BLOB store volym måste finnas på en iSCSI-volymen som konfigurerats på hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="903e2-110">hello BLOB store volume must be hosted on an iSCSI volume configured on hello StorSimple device.</span></span>

   3. <span data-ttu-id="903e2-111">Klicka på hello **aktivera** knapp under varje hello innehållsdatabaser som du vill tooconfigure för Fjärrlagring.</span><span class="sxs-lookup"><span data-stu-id="903e2-111">Click hello **Enable** button below each of hello content databases that you want tooconfigure for remote storage.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="903e2-112">hello blobstore måste vara delad och kan nås av alla frontend (WFE) webbservrar och hello-användarkonto som har konfigurerats för hello SharePoint-servergrupp måste ha åtkomst toohello resursen.</span><span class="sxs-lookup"><span data-stu-id="903e2-112">hello BLOB store must be shared and reachable by all web front-end (WFE) servers, and hello user account that is configured for hello SharePoint server farm must have access toohello share.</span></span>
      
      ![Aktivera hello RBS provider](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      <span data-ttu-id="903e2-114">När du aktiverar eller inaktiverar RBS dessutom visas hello följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="903e2-114">When you enable or disable RBS, you will also see hello following message.</span></span>
      
      ![Konfigurera StorSimple nätverkskort aktivera inaktivera](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. <span data-ttu-id="903e2-116">Klicka på hello **uppdatering** tooapply hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="903e2-116">Click hello **Update** button tooapply hello configuration.</span></span> <span data-ttu-id="903e2-117">När du klickar på hello **uppdatering** knappen hello RBS Konfigurationsstatus kommer att uppdateras på alla WFE-servrar och hello hela servergruppen kommer att RBS-aktiverade.</span><span class="sxs-lookup"><span data-stu-id="903e2-117">When you click hello **Update** button, hello RBS configuration status will be updated on all WFE servers, and hello entire farm will be RBS-enabled.</span></span> <span data-ttu-id="903e2-118">hello följande meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="903e2-118">hello following message appears.</span></span>
      
      ![Kortet configuration meddelande](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > <span data-ttu-id="903e2-120">Om du konfigurerar RBS för en SharePoint-grupp med ett mycket stort antal databaser (större än 200), kan hello Central Administration av SharePoint-webbsida timeout. Om det inträffar kan du uppdatera hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="903e2-120">If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), hello SharePoint Central Administration web page might time out. If that occurs, refresh hello page.</span></span> <span data-ttu-id="903e2-121">Detta påverkar inte hello konfigurationsprocessen.</span><span class="sxs-lookup"><span data-stu-id="903e2-121">This does not affect hello configuration process.</span></span>

4. <span data-ttu-id="903e2-122">Verifiera hello konfiguration:</span><span class="sxs-lookup"><span data-stu-id="903e2-122">Verify hello configuration:</span></span>
   
   1. <span data-ttu-id="903e2-123">Logga in toohello Central Administration av SharePoint-webbplats och bläddra toohello **konfigurera StorSimple nätverkskort** sidan.</span><span class="sxs-lookup"><span data-stu-id="903e2-123">Log on toohello SharePoint Central Administration website, and browse toohello **Configure StorSimple Adapter** page.</span></span>
   2. <span data-ttu-id="903e2-124">Kontrollera hello configuration information toomake till att de matchar hello-inställningar som du angav.</span><span class="sxs-lookup"><span data-stu-id="903e2-124">Check hello configuration details toomake sure that they match hello settings that you entered.</span></span> 
5. <span data-ttu-id="903e2-125">Kontrollera att RBS fungerar korrekt:</span><span class="sxs-lookup"><span data-stu-id="903e2-125">Verify that RBS works correctly:</span></span>
   
   1. <span data-ttu-id="903e2-126">Ladda upp ett dokument tooSharePoint.</span><span class="sxs-lookup"><span data-stu-id="903e2-126">Upload a document tooSharePoint.</span></span> 
   2. <span data-ttu-id="903e2-127">Bläddra toohello UNC-sökväg som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="903e2-127">Browse toohello UNC path that you configured.</span></span> <span data-ttu-id="903e2-128">Se till att hello RBS katalogstruktur har skapats och att den innehåller hello upp objektet.</span><span class="sxs-lookup"><span data-stu-id="903e2-128">Make sure that hello RBS directory structure was created and that it contains hello uploaded object.</span></span>
6. <span data-ttu-id="903e2-129">(Valfritt) Du kan använda hello Microsoft RBS `Migrate()` PowerShell-cmdlet som ingår i SharePoint toomigrate befintlig BLOB innehåll toohello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="903e2-129">(Optional) You can use hello Microsoft RBS `Migrate()` PowerShell cmdlet included with SharePoint toomigrate existing BLOB content toohello StorSimple device.</span></span> <span data-ttu-id="903e2-130">Mer information finns i [migrerar innehåll till eller från RBS i SharePoint 2013] [ 6] eller [migrerar innehåll till eller från RBS (SharePoint Foundation 2010)] [7].</span><span class="sxs-lookup"><span data-stu-id="903e2-130">For more information, see [Migrate content into or out of RBS in SharePoint 2013][6] or [Migrate content into or out of RBS (SharePoint Foundation 2010)][7].</span></span>
7. <span data-ttu-id="903e2-131">(Valfritt) Test-installationer Kontrollera du att hello-BLOB har flyttats utanför hello innehållsdatabas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="903e2-131">(Optional) On test installations, you can verify that hello BLOBs were moved out of hello content database as follows:</span></span> 
   
   1. <span data-ttu-id="903e2-132">Starta SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="903e2-132">Start SQL Management Studio.</span></span>
   2. <span data-ttu-id="903e2-133">Köra hello ListBlobsInDB_2010.sql eller ListBlobsInDB_2013.sql, enligt följande.</span><span class="sxs-lookup"><span data-stu-id="903e2-133">Run hello ListBlobsInDB_2010.sql or ListBlobsInDB_2013.sql query, as follows.</span></span>
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      <span data-ttu-id="903e2-134">Om RBS är korrekt konfigurerad, visas ett nullvärde i hello SizeOfContentInDB kolumn för alla objekt som har överförts och har externalized med RBS.</span><span class="sxs-lookup"><span data-stu-id="903e2-134">If RBS was configured correctly, a NULL value should appear in hello SizeOfContentInDB column for any object that was uploaded and successfully externalized with RBS.</span></span>
8. <span data-ttu-id="903e2-135">(Valfritt) När du konfigurerar RBS och flytta alla BLOB innehåll toohello StorSimple-enhet kan du flytta hello innehållsdatabasen toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="903e2-135">(Optional) After you configure RBS and move all BLOB content toohello StorSimple device, you can move hello content database toohello device.</span></span> <span data-ttu-id="903e2-136">Om du väljer toomove hello innehållsdatabasen, rekommenderar vi att du konfigurerar hello innehållsdatabasen lagring på hello enhet som en primär volym.</span><span class="sxs-lookup"><span data-stu-id="903e2-136">If you choose toomove hello content database, we recommend that you configure hello content database storage on hello device as a primary volume.</span></span> <span data-ttu-id="903e2-137">Använd upprätta sedan SQL Server best practices toomigrate hello innehållsdatabasen toohello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="903e2-137">Then, use established SQL Server best practices toomigrate hello content database toohello StorSimple device.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="903e2-138">Flytta hello innehållsdatabasen toohello enheten stöds endast för hello StorSimple 8000-serien (den inte stöds för hello 5000 och 7000-serien).</span><span class="sxs-lookup"><span data-stu-id="903e2-138">Moving hello content database toohello device is only supported for hello StorSimple 8000 series (it is not supported for hello 5000 or 7000 series).</span></span>
   
   <span data-ttu-id="903e2-139">Om du sparar Blobbar och hello innehållsdatabasen i separata volymer på hello StorSimple-enhet, rekommenderar vi att du konfigurerar dem i hello samma volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="903e2-139">If you store BLOBs and hello content database in separate volumes on hello StorSimple device, we recommend that you configure them in hello same volume container.</span></span> <span data-ttu-id="903e2-140">Detta säkerställer att de ska säkerhetskopieras tillsammans.</span><span class="sxs-lookup"><span data-stu-id="903e2-140">This ensures that they will be backed up together.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="903e2-141">Om du inte har aktiverat RBS rekommenderar vi inte flytta hello innehållsdatabasen toohello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="903e2-141">If you have not enabled RBS, we do not recommend moving hello content database toohello StorSimple device.</span></span> <span data-ttu-id="903e2-142">Detta är en otestade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="903e2-142">This is an untested configuration.</span></span>
   
9. <span data-ttu-id="903e2-143">Gå toohello nästa steg: [konfigurera skräpinsamling](#configure-garbage-collection).</span><span class="sxs-lookup"><span data-stu-id="903e2-143">Go toohello next step: [Configure garbage collection](#configure-garbage-collection).</span></span>

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
