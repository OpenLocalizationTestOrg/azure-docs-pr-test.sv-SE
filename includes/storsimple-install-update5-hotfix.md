<!--author=alkohli last changed: 08/21/17-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="be700-101">toodownload snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="be700-101">toodownload hotfixes</span></span>

<span data-ttu-id="be700-102">Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.</span><span class="sxs-lookup"><span data-stu-id="be700-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="be700-103">Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="be700-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="be700-104">Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.</span><span class="sxs-lookup"><span data-stu-id="be700-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

    ![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. <span data-ttu-id="be700-106">Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **4037264**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="be700-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **4037264**, and then click **Search**.</span></span>
   
    <span data-ttu-id="be700-107">hello snabbkorrigeringen visas, till exempel **kumulativa programvara paket Update 5.0 för StorSimple 8000-serien**.</span><span class="sxs-lookup"><span data-stu-id="be700-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 5.0 for StorSimple 8000 Series**.</span></span>
   
    ![Sökkatalog](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. <span data-ttu-id="be700-109">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="be700-109">Click **Download**.</span></span> <span data-ttu-id="be700-110">Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear.</span><span class="sxs-lookup"><span data-stu-id="be700-110">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="be700-111">Klicka på hello filer toodownload toohello den angivna platsen och mapp.</span><span class="sxs-lookup"><span data-stu-id="be700-111">Click hello files toodownload toohello specified location and folder.</span></span> <span data-ttu-id="be700-112">hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="be700-112">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>
5. <span data-ttu-id="be700-113">Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**4037266**), och ladda ned hello motsvarande filer toohello specifika mappar som anges i föregående tabell hello.</span><span class="sxs-lookup"><span data-stu-id="be700-113">Search for any additional hotfixes listed in hello table above (**4037266**), and download hello corresponding files toohello specific folders as listed in hello preceding table.</span></span>

> [!NOTE]
> <span data-ttu-id="be700-114">hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="be700-114">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
>
> <span data-ttu-id="be700-115">hello snabbkorrigeringar måste kopieras i 3 separata mappar.</span><span class="sxs-lookup"><span data-stu-id="be700-115">hello hotfixes must be copied in 3 separate folders.</span></span> <span data-ttu-id="be700-116">Till exempel hello enheten MDS-program/TIS agentuppdatering kan kopieras i _FirstOrderUpdate_ mapp, alla hello andra utan avbrott uppdateringar kan kopieras i hello _SecondOrderUpdate_ mapp, och Underhåll läge uppdateringar kopierade i _ThirdOrderUpdate_ mapp.</span><span class="sxs-lookup"><span data-stu-id="be700-116">For example, hello device software/Cis/MDS agent update can be copied in _FirstOrderUpdate_ folder, all hello other non-disruptive updates could be copied in hello _SecondOrderUpdate_ folder, and maintenance mode updates copied in _ThirdOrderUpdate_ folder.</span></span>

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="be700-117">tooinstall och kontrollera standardläget snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="be700-117">tooinstall and verify regular mode hotfixes</span></span>

<span data-ttu-id="be700-118">Utför följande steg tooinstall hello och kontrollera snabbkorrigeringar regular-läge.</span><span class="sxs-lookup"><span data-stu-id="be700-118">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="be700-119">Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="be700-119">If you already installed them using hello Azure portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="be700-120">tooinstall hello snabbkorrigeringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="be700-120">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="be700-121">Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="be700-121">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="be700-122">Hello Kommandotolken, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="be700-122">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="be700-123">Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="be700-123">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="be700-124">Vi rekommenderar att du installerar snabbkorrigeringen hello på hello passiva domänkontrollant först.</span><span class="sxs-lookup"><span data-stu-id="be700-124">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="be700-125">snabbkorrigering för tooinstall hello, Kommandotolken hello typ:</span><span class="sxs-lookup"><span data-stu-id="be700-125">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="be700-126">Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="be700-126">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="be700-127">hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="be700-127">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="be700-128">Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser.</span><span class="sxs-lookup"><span data-stu-id="be700-128">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="be700-129">Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.</span><span class="sxs-lookup"><span data-stu-id="be700-129">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
4. <span data-ttu-id="be700-130">Ange hello lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="be700-130">Supply hello password when prompted.</span></span> <span data-ttu-id="be700-131">Ett exempel på utdata för att installera hello första uppdateringar visas nedan.</span><span class="sxs-lookup"><span data-stu-id="be700-131">A sample output for installing hello first order updates is shown below.</span></span> <span data-ttu-id="be700-132">För hello första uppdatering behöver du toopoint toohello specifik fil.</span><span class="sxs-lookup"><span data-stu-id="be700-132">For hello first order update, you need toopoint toohello specific file.</span></span>

    >[!NOTE] 
    > <span data-ttu-id="be700-133">Du bör installera hello _HcsSoftwareUpdate.exe_ första.</span><span class="sxs-lookup"><span data-stu-id="be700-133">You should install hello _HcsSoftwareUpdate.exe_ first.</span></span> <span data-ttu-id="be700-134">När installationen har slutförts kan du sedan installera _CisMdsAgentUpdate.exe_.</span><span class="sxs-lookup"><span data-stu-id="be700-134">After this install has completed, then install _CisMdsAgentUpdate.exe_.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. <span data-ttu-id="be700-135">Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.</span><span class="sxs-lookup"><span data-stu-id="be700-135">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
6. <span data-ttu-id="be700-136">Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="be700-136">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="be700-137">hello-update först slutför på hello passiva domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="be700-137">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="be700-138">När hello passiva controller har uppdaterats, finns en växling vid fel och hello uppdatering används sedan på hello andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="be700-138">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="be700-139">hello uppdateringen är klar när båda hello domänkontrollanter uppdateras.</span><span class="sxs-lookup"><span data-stu-id="be700-139">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="be700-140">hello visar följande exempel på utdata hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="be700-140">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="be700-141">Hej `RunInprogress` är `True` när hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="be700-141">hello `RunInprogress` is `True` when hello update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="be700-142">följande exempel på utdata hello anger hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="be700-142">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="be700-143">Hej `RunInProgress` är `False` när hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="be700-143">hello `RunInProgress` is `False` when hello update is complete.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="be700-144">Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande.</span><span class="sxs-lookup"><span data-stu-id="be700-144">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="be700-145">tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="be700-145">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="be700-146">Om det är har hello snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="be700-146">If it is, then hello hotfix has completed.</span></span>

7. <span data-ttu-id="be700-147">Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="be700-147">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="be700-148">Ange:</span><span class="sxs-lookup"><span data-stu-id="be700-148">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="be700-149">Du bör se hello följande versioner:</span><span class="sxs-lookup"><span data-stu-id="be700-149">You should see hello following versions:</span></span>
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    <span data-ttu-id="be700-150">Om hello versionsnumret inte ändras efter att de hello uppdateringen anger hello snabbkorrigeringen har misslyckats tooapply.</span><span class="sxs-lookup"><span data-stu-id="be700-150">If hello version number does not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="be700-151">Kontakta [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="be700-151">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) for further assistance.</span></span>
     
    > [!IMPORTANT]
    > <span data-ttu-id="be700-152">Du måste starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet innan du tillämpar hello nästa uppdatering.</span><span class="sxs-lookup"><span data-stu-id="be700-152">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello next update.</span></span>
     
8. <span data-ttu-id="be700-153">Upprepa steg 3-6 tooinstall hello _CisMDSAgentupdate.exe_ agent hämtas tooyour _FirstOrderUpdate_ mapp.</span><span class="sxs-lookup"><span data-stu-id="be700-153">Repeat steps 3-6 tooinstall hello _CisMDSAgentupdate.exe_ agent downloaded tooyour _FirstOrderUpdate_ folder.</span></span>
8. <span data-ttu-id="be700-154">Upprepa steg 3 – 6 tooinstall hello andra uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="be700-154">Repeat steps 3-6 tooinstall hello second order updates.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="be700-155">För andra uppdateringar, flera uppdateringar installeras genom att bara köra hello `Start-HcsHotfix cmdlet` och peka toohello mappen där det finns andra uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="be700-155">For second order updates, multiple updates can be installed by just running hello `Start-HcsHotfix cmdlet` and pointing toohello folder where second order updates are located.</span></span> <span data-ttu-id="be700-156">hello-cmdlet utför alla hello tillgängliga uppdateringar i hello mapp.</span><span class="sxs-lookup"><span data-stu-id="be700-156">hello cmdlet will execute all hello updates available in hello folder.</span></span> <span data-ttu-id="be700-157">Om en uppdatering redan är installerad, identifierar som hello update logik och inte tillämpa uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="be700-157">If an update is already installed, hello update logic will detect that and not apply that update.</span></span>

    <span data-ttu-id="be700-158">När du har installerat alla hello snabbkorrigeringar använder hello `Get-HcsSystem` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="be700-158">After all hello hotfixes are installed, use hello `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="be700-159">hello versioner bör vara:</span><span class="sxs-lookup"><span data-stu-id="be700-159">hello versions should be:</span></span>
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="be700-160">tooinstall och kontrollera Underhåll läge snabbkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="be700-160">tooinstall and verify maintenance mode hotfixes</span></span>

<span data-ttu-id="be700-161">Använda KB4037263 tooinstall disk firmware-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="be700-161">Use KB4037263 tooinstall disk firmware updates.</span></span> <span data-ttu-id="be700-162">Dessa är störande uppdateringar och ta runt 30 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="be700-162">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="be700-163">Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="be700-163">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

> [!NOTE] 
> <span data-ttu-id="be700-164">Om den inbyggda programvaran disken är redan uppdaterade, behöver du inte tooinstall uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="be700-164">If your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="be700-165">Kör hello `Get-HcsUpdateAvailability` cmdlet från hello enhetens seriekonsol toocheck om uppdateringar är tillgängliga och om hello uppdateringar är störande (underhållsläget) eller utan avbrott (standardläget) uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="be700-165">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="be700-166">uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.</span><span class="sxs-lookup"><span data-stu-id="be700-166">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="be700-167">Placera hello enhet i underhållsläge hello.</span><span class="sxs-lookup"><span data-stu-id="be700-167">Place hello device in hello maintenance mode.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="be700-168">Använd inte fjärrkommunikation med Windows PowerShell om du ansluter tooa enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="be700-168">Do not use Windows PowerShell remoting when connecting tooa device in maintenance mode.</span></span> <span data-ttu-id="be700-169">I stället köra denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="be700-169">Instead run this cmdlet on hello device controller when connected through hello device serial console.</span></span>

    <span data-ttu-id="be700-170">tooplace hello domänkontrollant i underhållsläge, skriver du:</span><span class="sxs-lookup"><span data-stu-id="be700-170">tooplace hello controller in maintenance mode, type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="be700-171">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="be700-171">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="be700-172">Båda hello domänkontrollanter starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="be700-172">Both hello controllers then restart into maintenance mode.</span></span>
2. <span data-ttu-id="be700-173">tooinstall hello disk firmware-uppdatering, typ:</span><span class="sxs-lookup"><span data-stu-id="be700-173">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="be700-174">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="be700-174">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="be700-175">Övervakaren hello installera förlopp med hjälp av `Get-HcsUpdateStatus` kommando.</span><span class="sxs-lookup"><span data-stu-id="be700-175">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="be700-176">hello uppdateringen är klar när hello `RunInProgress` ändras för`False`.</span><span class="sxs-lookup"><span data-stu-id="be700-176">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="be700-177">När hello installationen är klar startar hello styrenhet vilket hello Underhåll läge snabbkorrigeringen har installerats om.</span><span class="sxs-lookup"><span data-stu-id="be700-177">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="be700-178">Logga in som alternativ 1 med fullständig åtkomst och kontrollera hello disk version på inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="be700-178">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="be700-179">Ange:</span><span class="sxs-lookup"><span data-stu-id="be700-179">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="be700-180">hello förväntas disk firmware versioner är:</span><span class="sxs-lookup"><span data-stu-id="be700-180">hello expected disk firmware versions are:</span></span>
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   <span data-ttu-id="be700-181">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="be700-181">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    <span data-ttu-id="be700-182">Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="be700-182">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="be700-183">Du kan avsluta hello underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="be700-183">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="be700-184">toodo så, Skriv följande kommando för varje enhet controller hello:</span><span class="sxs-lookup"><span data-stu-id="be700-184">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`

5. <span data-ttu-id="be700-185">hello domänkontrollanter startas om när du avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="be700-185">hello controllers restart when you exit maintenance mode.</span></span> <span data-ttu-id="be700-186">Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be700-186">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure portal.</span></span> <span data-ttu-id="be700-187">Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="be700-187">Note that hello portal might not show that you installed hello maintenance mode updates for 24 hours.</span></span>

