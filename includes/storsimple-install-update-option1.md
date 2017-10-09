<!--author=SharS last changed: 03/17/2016-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="80a0f-101">toodownload snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="80a0f-101">toodownload hotfixes</span></span>
<span data-ttu-id="80a0f-102">Utför följande steg toodownload hello programuppdateringen hello.</span><span class="sxs-lookup"><span data-stu-id="80a0f-102">Perform hello following steps toodownload hello software update.</span></span>

1. <span data-ttu-id="80a0f-103">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="80a0f-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="80a0f-104">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="80a0f-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="80a0f-105">![Installera katalog](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="80a0f-105">![Install catalog](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="80a0f-106">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **3063418**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="80a0f-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **3063418**, and then click **Search**.</span></span>
4. <span data-ttu-id="80a0f-107">Du ser hello **StorSimple uppdatering 1.2 enhet uppdatering** paket.</span><span class="sxs-lookup"><span data-stu-id="80a0f-107">You will see hello **StorSimple Update 1.2 Appliance Update** bundle.</span></span> <span data-ttu-id="80a0f-108">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a0f-108">Click **Add**.</span></span> <span data-ttu-id="80a0f-109">hello uppdateringen läggs toohello korg.</span><span class="sxs-lookup"><span data-stu-id="80a0f-109">hello update will be added toohello basket.</span></span>
5. <span data-ttu-id="80a0f-110">Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**3043005** och **3063416**), och Lägg till varje hello korg.</span><span class="sxs-lookup"><span data-stu-id="80a0f-110">Search for any additional hotfixes listed in hello table above (**3043005** and **3063416**), and add each hello basket.</span></span>
6. <span data-ttu-id="80a0f-111">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="80a0f-111">Click **View Basket**.</span></span>
   
    ![Visa korg](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. <span data-ttu-id="80a0f-113">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="80a0f-113">Click **Download**.</span></span> <span data-ttu-id="80a0f-114">Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear.</span><span class="sxs-lookup"><span data-stu-id="80a0f-114">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="80a0f-115">hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello.</span><span class="sxs-lookup"><span data-stu-id="80a0f-115">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="80a0f-116">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="80a0f-116">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="80a0f-117">hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="80a0f-117">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="80a0f-118">tooinstall och kontrollera standardläget snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="80a0f-118">tooinstall and verify regular mode hotfixes</span></span>
<span data-ttu-id="80a0f-119">Utför följande steg tooinstall hello och kontrollera hello regular läge snabbkorrigeringar.</span><span class="sxs-lookup"><span data-stu-id="80a0f-119">Perform hello following steps tooinstall and verify hello regular-mode hotfixes.</span></span> <span data-ttu-id="80a0f-120">Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="80a0f-120">If you already installed them using hello Azure Portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="80a0f-121">tooinstall hello programuppdateringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="80a0f-121">tooinstall hello software update, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="80a0f-122">Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="80a0f-122">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="80a0f-123">Hello Kommandotolken, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="80a0f-123">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="80a0f-124">Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="80a0f-124">Select **Option 1** toolog on toohello device with full access.</span></span>
3. <span data-ttu-id="80a0f-125">tooinstall hello uppdateringspaketet, Kommandotolken hello typ:</span><span class="sxs-lookup"><span data-stu-id="80a0f-125">tooinstall hello update package, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="80a0f-126">Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="80a0f-126">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="80a0f-127">hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="80a0f-127">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="80a0f-128">Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser.</span><span class="sxs-lookup"><span data-stu-id="80a0f-128">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="80a0f-129">Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.</span><span class="sxs-lookup"><span data-stu-id="80a0f-129">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="80a0f-130">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="80a0f-130">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. <span data-ttu-id="80a0f-131">Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.</span><span class="sxs-lookup"><span data-stu-id="80a0f-131">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
5. <span data-ttu-id="80a0f-132">Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="80a0f-132">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span>
   
    <span data-ttu-id="80a0f-133">hello visar följande exempel på utdata hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="80a0f-133">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="80a0f-134">Hej `RunInprogress` blir `True` när hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="80a0f-134">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="80a0f-135">följande exempel på utdata hello anger hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="80a0f-135">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="80a0f-136">Hej `RunInProgress` blir `False` när hello uppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="80a0f-136">hello `RunInProgress` will be `False` when hello update has completed.</span></span>

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > <span data-ttu-id="80a0f-137">Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande.</span><span class="sxs-lookup"><span data-stu-id="80a0f-137">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="80a0f-138">tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="80a0f-138">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="80a0f-139">Om det är har hello snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="80a0f-139">If it is, then hello hotfix has completed.</span></span>
   > 
   > 
6. <span data-ttu-id="80a0f-140">Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="80a0f-140">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="80a0f-141">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="80a0f-141">Type hello following command:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="80a0f-142">Du bör se hello följande versioner:</span><span class="sxs-lookup"><span data-stu-id="80a0f-142">You should see hello following versions:</span></span>
   
   * <span data-ttu-id="80a0f-143">HcsSoftwareVersion: 6.3.9600.17584</span><span class="sxs-lookup"><span data-stu-id="80a0f-143">HcsSoftwareVersion: 6.3.9600.17584</span></span>
   * <span data-ttu-id="80a0f-144">CisAgentVersion: 1.0.9049.0</span><span class="sxs-lookup"><span data-stu-id="80a0f-144">CisAgentVersion: 1.0.9049.0</span></span>
   * <span data-ttu-id="80a0f-145">MdsAgentVersion: 26.0.4696.1433</span><span class="sxs-lookup"><span data-stu-id="80a0f-145">MdsAgentVersion: 26.0.4696.1433</span></span>
     
     <span data-ttu-id="80a0f-146">Om hello versionsnummer inte ändras när hello uppdatering, visar hello snabbkorrigeringen har misslyckats tooapply.</span><span class="sxs-lookup"><span data-stu-id="80a0f-146">If hello version numbers do not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="80a0f-147">Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="80a0f-147">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
7. <span data-ttu-id="80a0f-148">Upprepa steg 3-5 tooinstall hello återstående regular läge snabbkorrigering (KB3043005).</span><span class="sxs-lookup"><span data-stu-id="80a0f-148">Repeat steps 3-5 tooinstall hello remaining regular-mode hotfix (KB3043005).</span></span>

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="80a0f-149">tooinstall och kontrollera Underhåll läge snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="80a0f-149">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="80a0f-150">Använda KB3063416 tooinstall disk firmware-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="80a0f-150">Use KB3063416 tooinstall disk firmware updates.</span></span> <span data-ttu-id="80a0f-151">Dessa är störande uppdateringar och ta runt 30 45 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="80a0f-151">These are disruptive updates and take around 30-45 minutes toocomplete.</span></span> <span data-ttu-id="80a0f-152">Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="80a0f-152">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="80a0f-153">uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.</span><span class="sxs-lookup"><span data-stu-id="80a0f-153">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="80a0f-154">Placera hello enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="80a0f-154">Place hello device in Maintenance mode.</span></span> <span data-ttu-id="80a0f-155">Observera att du inte bör använda Windows PowerShell-fjärrkommunikation om du ansluter tooa enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="80a0f-155">Note that you should not use Windows PowerShell remoting when connecting tooa device in Maintenance mode.</span></span> <span data-ttu-id="80a0f-156">Du behöver toorun denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="80a0f-156">You will need toorun this cmdlet on hello device controller when connected through hello device serial console.</span></span> <span data-ttu-id="80a0f-157">Ange:</span><span class="sxs-lookup"><span data-stu-id="80a0f-157">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="80a0f-158">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="80a0f-158">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="80a0f-159">Båda hello domänkontrollanter starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="80a0f-159">Both hello controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="80a0f-160">tooinstall hello disk firmware-uppdatering, typ:</span><span class="sxs-lookup"><span data-stu-id="80a0f-160">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="80a0f-161">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="80a0f-161">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="80a0f-162">Övervakaren hello installera förlopp med hjälp av `Get-HcsUpdateStatus` kommando.</span><span class="sxs-lookup"><span data-stu-id="80a0f-162">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="80a0f-163">hello uppdateringen är klar när hello `RunInProgress` ändras för`False`.</span><span class="sxs-lookup"><span data-stu-id="80a0f-163">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="80a0f-164">När hello installationen är klar, hello domänkontrollant som hello Underhåll läge snabbkorrigeringen har installerats måste startas om.</span><span class="sxs-lookup"><span data-stu-id="80a0f-164">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed will be rebooted.</span></span> <span data-ttu-id="80a0f-165">Logga in som alternativ 1 med fullständig åtkomst och kontrollera hello disk version på inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="80a0f-165">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="80a0f-166">Ange:</span><span class="sxs-lookup"><span data-stu-id="80a0f-166">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="80a0f-167">hello förväntas disk firmware versioner är:</span><span class="sxs-lookup"><span data-stu-id="80a0f-167">hello expected disk firmware versions are:</span></span>
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   <span data-ttu-id="80a0f-168">Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="80a0f-168">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="80a0f-169">Du kan avsluta hello underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="80a0f-169">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="80a0f-170">Skriv följande kommando för varje enhet controller hello:</span><span class="sxs-lookup"><span data-stu-id="80a0f-170">Type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="80a0f-171">hello domänkontrollanter startas om när du avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="80a0f-171">hello controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="80a0f-172">Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a0f-172">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="80a0f-173">Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="80a0f-173">Note that hello portal might not show that you installed hello Maintenance mode updates for 24 hours.</span></span>

