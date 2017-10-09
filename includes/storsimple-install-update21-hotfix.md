<!--author=alkohli last changed: 05/19/16-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="d63df-101">toodownload snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="d63df-101">toodownload hotfixes</span></span>
<span data-ttu-id="d63df-102">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="d63df-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="d63df-103">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d63df-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="d63df-104">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="d63df-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="d63df-105">![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="d63df-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="d63df-106">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **3179904**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="d63df-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **3179904**, and then click **Search**.</span></span>
   
    <span data-ttu-id="d63df-107">hello snabbkorrigeringen visas, till exempel **kumulativa programvara paket Update 2.2 för StorSimple 8000-serien**.</span><span class="sxs-lookup"><span data-stu-id="d63df-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 2.2 for StorSimple 8000 Series**.</span></span>
   
    ![Sökkatalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="d63df-109">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d63df-109">Click **Add**.</span></span> <span data-ttu-id="d63df-110">hello uppdateringen läggs toohello korg.</span><span class="sxs-lookup"><span data-stu-id="d63df-110">hello update is added toohello basket.</span></span>
5. <span data-ttu-id="d63df-111">Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**3103616**, **3146621**), och Lägg till varje toohello korg.</span><span class="sxs-lookup"><span data-stu-id="d63df-111">Search for any additional hotfixes listed in hello table above (**3103616**, **3146621**), and add each toohello basket.</span></span>
6. <span data-ttu-id="d63df-112">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="d63df-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="d63df-113">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="d63df-113">Click **Download**.</span></span> <span data-ttu-id="d63df-114">Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear.</span><span class="sxs-lookup"><span data-stu-id="d63df-114">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="d63df-115">hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello.</span><span class="sxs-lookup"><span data-stu-id="d63df-115">hello updates are downloaded toohello specified location and placed in a sub-folder with hello same name as hello update.</span></span> <span data-ttu-id="d63df-116">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="d63df-116">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="d63df-117">hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="d63df-117">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="d63df-118">tooinstall och kontrollera standardläget snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="d63df-118">tooinstall and verify regular mode hotfixes</span></span>
<span data-ttu-id="d63df-119">Utför följande steg tooinstall hello och kontrollera snabbkorrigeringar regular-läge.</span><span class="sxs-lookup"><span data-stu-id="d63df-119">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="d63df-120">Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="d63df-120">If you already installed them using hello Azure Portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="d63df-121">tooinstall hello snabbkorrigeringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="d63df-121">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="d63df-122">Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="d63df-122">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="d63df-123">Hello Kommandotolken, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="d63df-123">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="d63df-124">Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d63df-124">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="d63df-125">Vi rekommenderar att du installerar snabbkorrigeringen hello på hello passiva domänkontrollant först.</span><span class="sxs-lookup"><span data-stu-id="d63df-125">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="d63df-126">snabbkorrigering för tooinstall hello, Kommandotolken hello typ:</span><span class="sxs-lookup"><span data-stu-id="d63df-126">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="d63df-127">Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="d63df-127">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="d63df-128">hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="d63df-128">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="d63df-129">Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser.</span><span class="sxs-lookup"><span data-stu-id="d63df-129">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="d63df-130">Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.</span><span class="sxs-lookup"><span data-stu-id="d63df-130">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="d63df-131">Ange hello lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="d63df-131">Supply hello password when prompted.</span></span>
   
    <span data-ttu-id="d63df-132">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d63df-132">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. <span data-ttu-id="d63df-133">Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.</span><span class="sxs-lookup"><span data-stu-id="d63df-133">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="d63df-134">Om du installerar uppdateringen 2.2 bara installera hello-binärfilen som inleds med ”alla hcsmdssoftwareudpate'.</span><span class="sxs-lookup"><span data-stu-id="d63df-134">If installing Update 2.2, only install hello binary file prefaced with 'all-hcsmdssoftwareudpate'.</span></span> <span data-ttu-id="d63df-135">Installera inte hello Cis och hello MDS agentuppdatering inleds med alla cismdsagentupdatebundle.</span><span class="sxs-lookup"><span data-stu-id="d63df-135">Do not install hello Cis and hello MDS agent update prefaced with all-cismdsagentupdatebundle.</span></span> <span data-ttu-id="d63df-136">Fel toodo så resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="d63df-136">Failure toodo so will result in an error.</span></span> 

5. <span data-ttu-id="d63df-137">Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d63df-137">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="d63df-138">hello-update först slutför på hello passiva domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="d63df-138">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="d63df-139">När hello passiva controller har uppdaterats, finns en växling vid fel och hello uppdatering används sedan på hello andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="d63df-139">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="d63df-140">hello uppdateringen är klar när båda hello domänkontrollanter uppdateras.</span><span class="sxs-lookup"><span data-stu-id="d63df-140">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="d63df-141">hello visar följande exempel på utdata hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="d63df-141">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="d63df-142">Hej `RunInprogress` blir `True` när hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="d63df-142">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 5/5/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="d63df-143">följande exempel på utdata hello anger hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="d63df-143">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="d63df-144">Hej `RunInProgress` blir `False` när hello uppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d63df-144">hello `RunInProgress` will be `False` when hello update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 5/17/2016 9:15:55 AM
    LastUpdateTimestamp : 5/17/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="d63df-145">Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande.</span><span class="sxs-lookup"><span data-stu-id="d63df-145">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="d63df-146">tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="d63df-146">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="d63df-147">Om det är har hello snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="d63df-147">If it is, then hello hotfix has completed.</span></span>

1. <span data-ttu-id="d63df-148">Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d63df-148">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="d63df-149">Ange:</span><span class="sxs-lookup"><span data-stu-id="d63df-149">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="d63df-150">Du bör se hello följande versioner:</span><span class="sxs-lookup"><span data-stu-id="d63df-150">You should see hello following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17708`
   * `CisAgentVersion: 1.0.9299.0`
   * `MdsAgentVersion: 30.0.4698.16` 
     
     <span data-ttu-id="d63df-151">Om hello versionsnummer inte ändras när hello uppdatering, visar hello snabbkorrigeringen har misslyckats tooapply.</span><span class="sxs-lookup"><span data-stu-id="d63df-151">If hello version numbers do not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="d63df-152">Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="d63df-152">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="d63df-153">Du måste starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet innan du tillämpar hello återstående uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d63df-153">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="d63df-154">Upprepa steg 3-5 tooinstall hello återstående snabbkorrigeringar regular-läge.</span><span class="sxs-lookup"><span data-stu-id="d63df-154">Repeat steps 3-5 tooinstall hello remaining regular-mode hotfixes.</span></span>
   
   * <span data-ttu-id="d63df-155">hello iSCSI uppdateringen KB3146621</span><span class="sxs-lookup"><span data-stu-id="d63df-155">hello iSCSI update KB3146621</span></span>
   * <span data-ttu-id="d63df-156">hello WMI uppdateringen KB3103616</span><span class="sxs-lookup"><span data-stu-id="d63df-156">hello WMI update KB3103616</span></span>
3. <span data-ttu-id="d63df-157">Hoppa över det här steget om du uppdaterar från uppdatering 2.</span><span class="sxs-lookup"><span data-stu-id="d63df-157">Skip this step if you are updating from Update 2.</span></span> <span data-ttu-id="d63df-158">Om du uppdaterar från en tidigare version tooUpdate 2, måste du också toodownload:</span><span class="sxs-lookup"><span data-stu-id="d63df-158">If you are updating from a version prior tooUpdate 2, you will also need toodownload:</span></span>

    - <span data-ttu-id="d63df-159">hello LSI drivrutinen KB3121900</span><span class="sxs-lookup"><span data-stu-id="d63df-159">hello LSI driver KB3121900</span></span>

    - <span data-ttu-id="d63df-160">Hej Spaceport uppdatering KB3090322</span><span class="sxs-lookup"><span data-stu-id="d63df-160">hello Spaceport update KB3090322</span></span>

    - <span data-ttu-id="d63df-161">hello Storport uppdateringen KB3080728</span><span class="sxs-lookup"><span data-stu-id="d63df-161">hello Storport update KB3080728</span></span>

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="d63df-162">tooinstall och kontrollera Underhåll läge snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="d63df-162">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="d63df-163">Använda KB3121899 tooinstall disk firmware-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d63df-163">Use KB3121899 tooinstall disk firmware updates.</span></span> <span data-ttu-id="d63df-164">Dessa är störande uppdateringar och ta runt 30 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="d63df-164">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="d63df-165">Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="d63df-165">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="d63df-166">Observera att om den inbyggda programvaran disken är redan uppdaterade, du inte behöver tooinstall uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="d63df-166">Note that if your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="d63df-167">Kör hello `Get-HcsUpdateAvailability` cmdlet från hello enhetens seriekonsol toocheck om uppdateringar är tillgängliga och om hello uppdateringar är störande (underhållsläget) eller utan avbrott (standardläget) uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d63df-167">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="d63df-168">uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.</span><span class="sxs-lookup"><span data-stu-id="d63df-168">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="d63df-169">Placera hello enhet i underhållsläge hello.</span><span class="sxs-lookup"><span data-stu-id="d63df-169">Place hello device in hello Maintenance mode.</span></span> <span data-ttu-id="d63df-170">Observera att du inte bör använda Windows PowerShell-fjärrkommunikation om du ansluter tooa enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="d63df-170">Note that you should not use Windows PowerShell remoting when connecting tooa device in Maintenance mode.</span></span> <span data-ttu-id="d63df-171">I stället köra denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="d63df-171">Instead run this cmdlet on hello device controller when connected through hello device serial console.</span></span> <span data-ttu-id="d63df-172">Ange:</span><span class="sxs-lookup"><span data-stu-id="d63df-172">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="d63df-173">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d63df-173">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="d63df-174">Båda hello domänkontrollanter starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="d63df-174">Both hello controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="d63df-175">tooinstall hello disk firmware-uppdatering, typ:</span><span class="sxs-lookup"><span data-stu-id="d63df-175">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="d63df-176">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d63df-176">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="d63df-177">Övervakaren hello installera förlopp med hjälp av `Get-HcsUpdateStatus` kommando.</span><span class="sxs-lookup"><span data-stu-id="d63df-177">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="d63df-178">hello uppdateringen är klar när hello `RunInProgress` ändras för`False`.</span><span class="sxs-lookup"><span data-stu-id="d63df-178">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="d63df-179">När hello installationen är klar startar hello styrenhet vilket hello Underhåll läge snabbkorrigeringen har installerats om.</span><span class="sxs-lookup"><span data-stu-id="d63df-179">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="d63df-180">Logga in som alternativ 1 med fullständig åtkomst och kontrollera hello disk version på inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="d63df-180">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="d63df-181">Ange:</span><span class="sxs-lookup"><span data-stu-id="d63df-181">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="d63df-182">hello förväntas disk firmware versioner är:</span><span class="sxs-lookup"><span data-stu-id="d63df-182">hello expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="d63df-183">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d63df-183">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    <span data-ttu-id="d63df-184">Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="d63df-184">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="d63df-185">Du kan avsluta hello underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="d63df-185">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="d63df-186">toodo så, Skriv följande kommando för varje enhet controller hello:</span><span class="sxs-lookup"><span data-stu-id="d63df-186">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="d63df-187">hello domänkontrollanter startas om när du avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="d63df-187">hello controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="d63df-188">Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d63df-188">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="d63df-189">Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="d63df-189">Note that hello portal might not show that you installed hello Maintenance mode updates for 24 hours.</span></span>

