<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> <span data-ttu-id="c7bce-101">När du ändrar StorSimple-kortet för SharePoint RBS konfigurationen måste du vara inloggad med ett konto som tillhör gruppen Domänadministratörer.</span><span class="sxs-lookup"><span data-stu-id="c7bce-101">When making changes to the StorSimple Adapter for SharePoint RBS configuration, you must be logged on with a user account that belongs to the Domain Admins group.</span></span> <span data-ttu-id="c7bce-102">Dessutom måste du komma åt konfigurationssidan från en webbläsare som körs på samma värddator som Central Administration.</span><span class="sxs-lookup"><span data-stu-id="c7bce-102">Additionally, you must access the configuration page from a browser running on the same host as Central Administration.</span></span>
> 
> 

#### <a name="to-configure-rbs"></a><span data-ttu-id="c7bce-103">Så här konfigurerar du RBS</span><span class="sxs-lookup"><span data-stu-id="c7bce-103">To configure RBS</span></span>
1. <span data-ttu-id="c7bce-104">Öppna sidan Central Administration av SharePoint och bläddra till **systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="c7bce-104">Open the SharePoint Central Administration page, and browse to **System Settings**.</span></span> 
2. <span data-ttu-id="c7bce-105">I den **Azure StorSimple** klickar du på **konfigurera StorSimple nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="c7bce-105">In the **Azure StorSimple** section, click **Configure StorSimple Adapter**.</span></span>
   
    ![Konfigurera StorSimple-kort](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. <span data-ttu-id="c7bce-107">På den **konfigurera StorSimple nätverkskort** sidan:</span><span class="sxs-lookup"><span data-stu-id="c7bce-107">On the **Configure StorSimple Adapter** page:</span></span>
   
   1. <span data-ttu-id="c7bce-108">Se till att den **Aktivera redigering sökväg** är markerad.</span><span class="sxs-lookup"><span data-stu-id="c7bce-108">Make sure that the **Enable editing path** check box is selected.</span></span>
   2. <span data-ttu-id="c7bce-109">I rutan skriver du Universal Naming Convention (UNC)-sökvägen till blobstore.</span><span class="sxs-lookup"><span data-stu-id="c7bce-109">In the text box, type the Universal Naming Convention (UNC) path of the BLOB store.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="c7bce-110">BLOB store volym måste finnas på en iSCSI-volymen som konfigurerats på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="c7bce-110">The BLOB store volume must be hosted on an iSCSI volume configured on the StorSimple device.</span></span>

   3. <span data-ttu-id="c7bce-111">Klicka på den **aktivera** knapp under varje innehållsdatabaser som du vill konfigurera för Fjärrlagring.</span><span class="sxs-lookup"><span data-stu-id="c7bce-111">Click the **Enable** button below each of the content databases that you want to configure for remote storage.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="c7bce-112">BLOB-arkivet måste vara delad och kan nås av alla frontend (WFE) webbservrar och det användarkonto som har konfigurerats för SharePoint-servergruppen måste ha åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="c7bce-112">The BLOB store must be shared and reachable by all web front-end (WFE) servers, and the user account that is configured for the SharePoint server farm must have access to the share.</span></span>
      
      ![Aktivera RBS-provider](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      <span data-ttu-id="c7bce-114">När du aktiverar eller inaktiverar RBS dessutom visas följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="c7bce-114">When you enable or disable RBS, you will also see the following message.</span></span>
      
      ![Konfigurera StorSimple nätverkskort aktivera inaktivera](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. <span data-ttu-id="c7bce-116">Klicka på den **uppdatering** för att tillämpa konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c7bce-116">Click the **Update** button to apply the configuration.</span></span> <span data-ttu-id="c7bce-117">När du klickar på den **uppdatering** knappen Konfigurationsstatus RBS kommer att uppdateras på alla WFE-servrar och hela servergruppen kommer att RBS-aktiverade.</span><span class="sxs-lookup"><span data-stu-id="c7bce-117">When you click the **Update** button, the RBS configuration status will be updated on all WFE servers, and the entire farm will be RBS-enabled.</span></span> <span data-ttu-id="c7bce-118">Följande meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="c7bce-118">The following message appears.</span></span>
      
      ![Kortet configuration meddelande](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > <span data-ttu-id="c7bce-120">Om du konfigurerar RBS för en SharePoint-grupp med ett mycket stort antal databaser (större än 200), kan sidan Central Administration av SharePoint timeout.</span><span class="sxs-lookup"><span data-stu-id="c7bce-120">If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), the SharePoint Central Administration web page might time out.</span></span> <span data-ttu-id="c7bce-121">Om det inträffar kan du uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="c7bce-121">If that occurs, refresh the page.</span></span> <span data-ttu-id="c7bce-122">Detta påverkar inte konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c7bce-122">This does not affect the configuration process.</span></span>

4. <span data-ttu-id="c7bce-123">Kontrollera konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="c7bce-123">Verify the configuration:</span></span>
   
   1. <span data-ttu-id="c7bce-124">Logga in på webbplatsen för Central Administration av SharePoint och bläddra till den **konfigurera StorSimple nätverkskort** sidan.</span><span class="sxs-lookup"><span data-stu-id="c7bce-124">Log on to the SharePoint Central Administration website, and browse to the **Configure StorSimple Adapter** page.</span></span>
   2. <span data-ttu-id="c7bce-125">Kontrollera konfigurationsinformation för att se till att de matchar de inställningar som du angett.</span><span class="sxs-lookup"><span data-stu-id="c7bce-125">Check the configuration details to make sure that they match the settings that you entered.</span></span> 
5. <span data-ttu-id="c7bce-126">Kontrollera att RBS fungerar korrekt:</span><span class="sxs-lookup"><span data-stu-id="c7bce-126">Verify that RBS works correctly:</span></span>
   
   1. <span data-ttu-id="c7bce-127">Överför ett dokument till SharePoint.</span><span class="sxs-lookup"><span data-stu-id="c7bce-127">Upload a document to SharePoint.</span></span> 
   2. <span data-ttu-id="c7bce-128">Bläddra till UNC-sökvägen som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c7bce-128">Browse to the UNC path that you configured.</span></span> <span data-ttu-id="c7bce-129">Se till att katalogstrukturen RBS har skapats och att den innehåller överförda objektet.</span><span class="sxs-lookup"><span data-stu-id="c7bce-129">Make sure that the RBS directory structure was created and that it contains the uploaded object.</span></span>
6. <span data-ttu-id="c7bce-130">(Valfritt) Du kan använda Microsoft RBS `Migrate()` PowerShell-cmdlet som ingår i SharePoint för att migrera befintliga BLOB-innehåll till StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="c7bce-130">(Optional) You can use the Microsoft RBS `Migrate()` PowerShell cmdlet included with SharePoint to migrate existing BLOB content to the StorSimple device.</span></span> <span data-ttu-id="c7bce-131">Mer information finns i [migrerar innehåll till eller från RBS i SharePoint 2013] [ 6] eller [migrerar innehåll till eller från RBS (SharePoint Foundation 2010)] [7].</span><span class="sxs-lookup"><span data-stu-id="c7bce-131">For more information, see [Migrate content into or out of RBS in SharePoint 2013][6] or [Migrate content into or out of RBS (SharePoint Foundation 2010)][7].</span></span>
7. <span data-ttu-id="c7bce-132">(Valfritt) På test-installationer kan du kontrollera att Blobbarna har flyttats utanför innehållsdatabasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c7bce-132">(Optional) On test installations, you can verify that the BLOBs were moved out of the content database as follows:</span></span> 
   
   1. <span data-ttu-id="c7bce-133">Starta SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="c7bce-133">Start SQL Management Studio.</span></span>
   2. <span data-ttu-id="c7bce-134">Kör frågan ListBlobsInDB_2010.sql eller ListBlobsInDB_2013.sql på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="c7bce-134">Run the ListBlobsInDB_2010.sql or ListBlobsInDB_2013.sql query, as follows.</span></span>
      
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
      
      <span data-ttu-id="c7bce-135">Om RBS är korrekt konfigurerad, visas ett nullvärde i kolumnen SizeOfContentInDB för alla objekt som har överförts och har externalized med RBS.</span><span class="sxs-lookup"><span data-stu-id="c7bce-135">If RBS was configured correctly, a NULL value should appear in the SizeOfContentInDB column for any object that was uploaded and successfully externalized with RBS.</span></span>
8. <span data-ttu-id="c7bce-136">(Valfritt) När du konfigurerar RBS och flytta alla BLOB-innehåll till StorSimple-enhet kan flytta du innehållsdatabasen till enheten.</span><span class="sxs-lookup"><span data-stu-id="c7bce-136">(Optional) After you configure RBS and move all BLOB content to the StorSimple device, you can move the content database to the device.</span></span> <span data-ttu-id="c7bce-137">Om du väljer att flytta innehållsdatabasen rekommenderar vi att du konfigurerar innehållsdatabasen lagring på enheten som en primär volym.</span><span class="sxs-lookup"><span data-stu-id="c7bce-137">If you choose to move the content database, we recommend that you configure the content database storage on the device as a primary volume.</span></span> <span data-ttu-id="c7bce-138">Använd upprätta sedan SQL Server bästa praxis för att migrera innehållsdatabasen till StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="c7bce-138">Then, use established SQL Server best practices to migrate the content database to the StorSimple device.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="c7bce-139">Flytta innehållsdatabasen till enheten har endast stöd för StorSimple 8000-serien (den inte stöds för 5000 och 7000-serien).</span><span class="sxs-lookup"><span data-stu-id="c7bce-139">Moving the content database to the device is only supported for the StorSimple 8000 series (it is not supported for the 5000 or 7000 series).</span></span>
   
   <span data-ttu-id="c7bce-140">Om du sparar Blobbar och innehållsdatabasen i separata volymer på StorSimple-enheten, rekommenderar vi att du konfigurerar dem i samma volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="c7bce-140">If you store BLOBs and the content database in separate volumes on the StorSimple device, we recommend that you configure them in the same volume container.</span></span> <span data-ttu-id="c7bce-141">Detta säkerställer att de ska säkerhetskopieras tillsammans.</span><span class="sxs-lookup"><span data-stu-id="c7bce-141">This ensures that they will be backed up together.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="c7bce-142">Om du inte har aktiverat RBS rekommenderar vi inte flytta innehållsdatabasen till StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="c7bce-142">If you have not enabled RBS, we do not recommend moving the content database to the StorSimple device.</span></span> <span data-ttu-id="c7bce-143">Detta är en otestade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c7bce-143">This is an untested configuration.</span></span>
   
9. <span data-ttu-id="c7bce-144">Gå till nästa steg: [konfigurera skräpinsamling](#configure-garbage-collection).</span><span class="sxs-lookup"><span data-stu-id="c7bce-144">Go to the next step: [Configure garbage collection](#configure-garbage-collection).</span></span>

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
