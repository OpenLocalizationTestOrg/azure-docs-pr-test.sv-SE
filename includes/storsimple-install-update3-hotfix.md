<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="a0c3b-101">Ladda ned snabbkorrigerar</span><span class="sxs-lookup"><span data-stu-id="a0c3b-101">To download hotfixes</span></span>
<span data-ttu-id="a0c3b-102">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="a0c3b-103">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a0c3b-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="a0c3b-104">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="a0c3b-105">![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="a0c3b-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="a0c3b-106">I sökrutan i Microsoft Update-katalogen ange Knowledge Base (KB) antalet den snabbkorrigering som du vill hämta, till exempel **3186843**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **3186843**, and then click **Search**.</span></span>
   
    <span data-ttu-id="a0c3b-107">Snabbkorrigeringen listan visas till exempel **kumulativa programvara paket Update 3.0 för StorSimple 8000-serien**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 3.0 for StorSimple 8000 Series**.</span></span>
   
    ![Sökkatalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="a0c3b-109">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-109">Click **Add**.</span></span> <span data-ttu-id="a0c3b-110">Uppdateringen läggs till i korgen.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-110">The update is added to the basket.</span></span>
5. <span data-ttu-id="a0c3b-111">Sök efter eventuella ytterligare snabbkorrigeringar som anges i tabellen ovan (**3186859**), och Lägg till dem till korgen.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-111">Search for any additional hotfixes listed in the table above (**3186859**), and add each to the basket.</span></span>
6. <span data-ttu-id="a0c3b-112">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="a0c3b-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="a0c3b-113">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-113">Click **Download**.</span></span> <span data-ttu-id="a0c3b-114">Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-114">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="a0c3b-115">Uppdateringarna laddas ned till den angivna platsen och placeras i en underordnad mapp med samma namn som uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-115">The updates are downloaded to the specified location and placed in a sub-folder with the same name as the update.</span></span> <span data-ttu-id="a0c3b-116">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-116">The folder can also be copied to a network share that is reachable from the device.</span></span>

> [!NOTE]
> <span data-ttu-id="a0c3b-117">Snabbkorrigeringarna måste vara tillgänglig från båda domänkontrollanter för att identifiera eventuella felmeddelanden från peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-117">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="a0c3b-118">Installera och verifiera snabbkorrigeringar i normalläge</span><span class="sxs-lookup"><span data-stu-id="a0c3b-118">To install and verify regular mode hotfixes</span></span>
<span data-ttu-id="a0c3b-119">Utför följande steg för att installera och verifiera snabbkorrigeringar i normalläge.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-119">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="a0c3b-120">Om du redan har installerat dem med hjälp av Azure Portal, gå vidare till [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="a0c3b-120">If you already installed them using the Azure Portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="a0c3b-121">Gå in i Windows PowerShell-gränssnittet på din StorSimple-enhets seriekonsol för att installera snabbkorrigerarna.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-121">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="a0c3b-122">Följ de detaljerade instruktionerna i [Använd PuTTY för att ansluta till enhetens seriekonsol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="a0c3b-122">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="a0c3b-123">Tryck på **Retur** i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-123">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="a0c3b-124">Välj **Alternativ 1** för att logga in på enheten med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-124">Select **Option 1** to log on to the device with full access.</span></span> <span data-ttu-id="a0c3b-125">Vi rekommenderar att du installerar snabbkorrigeringen på den passiva styrenheten först.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-125">We recommend that you install the hotfix on the passive controller first.</span></span>
3. <span data-ttu-id="a0c3b-126">Ange följande i kommandotolken för att installera snabbkorrigeringen:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-126">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="a0c3b-127">Använd IP i stället för DNS i resurssökvägen i kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-127">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="a0c3b-128">Parametern för autentiseringsuppgifter används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-128">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="a0c3b-129">Vi rekommenderar att du använder parametern för autentiseringsuppgifter för att få åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-129">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="a0c3b-130">Även resurser som är öppna för ”alla” är vanligtvis inte öppna för icke-autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-130">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="a0c3b-131">Ange lösenordet när du uppmanas att göra så.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-131">Supply the password when prompted.</span></span>
   
    <span data-ttu-id="a0c3b-132">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-132">A sample output is shown below.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="a0c3b-133">Skriv **Y** när du uppmanas att bekräfta installationen av snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-133">Type **Y** when prompted to confirm the hotfix installation.</span></span>
5. <span data-ttu-id="a0c3b-134">Övervaka uppdateringen med hjälp av `Get-HcsUpdateStatus`-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-134">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="a0c3b-135">Uppdateringen slutförs först på den passiva styrenheten.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-135">The update will first complete on the passive controller.</span></span> <span data-ttu-id="a0c3b-136">När den passiva styrenheten har uppdaterats sker en redundans och uppdateringen tillämpas sedan på den andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-136">Once the passive controller is updated, there will be a failover and the update will then get applied on the other controller.</span></span> <span data-ttu-id="a0c3b-137">Uppdateringen har slutförts när båda styrenheterna har uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-137">The update is complete when both the controllers are updated.</span></span>
   
    <span data-ttu-id="a0c3b-138">Följande exempel på utdata visar att uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-138">The following sample output shows the update in progress.</span></span> <span data-ttu-id="a0c3b-139">`RunInprogress` kommer att vara `True` när uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-139">The `RunInprogress` will be `True` when the update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="a0c3b-140">Följande exempel på utdata visar att uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-140">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="a0c3b-141">`RunInProgress` kommer att vara `False` när uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-141">The `RunInProgress` will be `False` when the update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > <span data-ttu-id="a0c3b-142">I vissa fall rapporterar cmdlet `False` när uppdateringen fortfarande pågår.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-142">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="a0c3b-143">Om du vill kontrollera att snabbkorrigeringen har slutförts väntar du några minuter och sedan kör du det här kommandot igen och kontrollerar att `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-143">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="a0c3b-144">Om det har det har snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-144">If it is, then the hotfix has completed.</span></span>

1. <span data-ttu-id="a0c3b-145">När programuppdateringen har slutförts kan du kontrollera systemprogramversionerna.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-145">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="a0c3b-146">Ange:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="a0c3b-147">Du bör se följande versioner:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-147">You should see the following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     <span data-ttu-id="a0c3b-148">Om versionsnumren inte ändras efter att uppdateringen, visar att snabbkorrigeringen har kunde inte tillämpas.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-148">If the version numbers do not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="a0c3b-149">Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="a0c3b-150">Du måste starta om den aktiva kontrollenheten via `Restart-HcsController`-cmdlet innan du tillämpar de återstående uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-150">You must restart the active controller via the `Restart-HcsController` cmdlet before applying the remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="a0c3b-151">Upprepa steg 3-5 för att installera snabbkorrigeringen LSI drivrutiner och inbyggd programvara **KB3186859**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-151">Repeat steps 3-5 to install the LSI driver and firmware hotfix **KB3186859**.</span></span> <span data-ttu-id="a0c3b-152">När snabbkorrigeringen har installerats kan du använda den `Get-HcsSystem` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-152">After the hotfix is installed, use the `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="a0c3b-153">LSI-versionen måste vara:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-153">The LSI version should be:</span></span>
   
   * `Lsisas2Version: 2.0.78.00`
3. <span data-ttu-id="a0c3b-154">Upprepa steg 3-5 för att installera uppdateringen Storport och Spaceport **KB3121261**.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-154">Repeat steps 3-5 to install the Storport and Spaceport update **KB3121261**.</span></span>
4. <span data-ttu-id="a0c3b-155">Om du uppdaterar från uppdatering 2 eller en tidigare version, måste du också hämta:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-155">If you are updating from Update 2 or earlier version, you will also need to download:</span></span>
   
   * <span data-ttu-id="a0c3b-156">iSCSI-lösning med hjälp av KB3146621</span><span class="sxs-lookup"><span data-stu-id="a0c3b-156">iSCSI fix using KB3146621</span></span>
   * <span data-ttu-id="a0c3b-157">WMI-lösning med hjälp av KB3103616</span><span class="sxs-lookup"><span data-stu-id="a0c3b-157">WMI fix using KB3103616</span></span>

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="a0c3b-158">Installera och verifiera snabbkorrigeringar i underhållsläge</span><span class="sxs-lookup"><span data-stu-id="a0c3b-158">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="a0c3b-159">Använd KB3121899 att installera uppdateringar av inbyggd disk.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-159">Use KB3121899 to install disk firmware updates.</span></span> <span data-ttu-id="a0c3b-160">Det här är störande uppdateringar och tar cirka 30 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-160">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="a0c3b-161">Du kan välja att installera dem i ett planerat underhållsfönster genom att ansluta till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-161">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="a0c3b-162">Observera att om den inbyggda programvaran för disken redan är uppdaterad så behöver du inte installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-162">Note that if your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="a0c3b-163">Kör `Get-HcsUpdateAvailability`-cmdleten från enhetens seriekonsol för att kontrollera om det finns tillgängliga uppdateringar och om uppdateringarna är störande (underhållsläge) eller avbrottsfria (standardläget).</span><span class="sxs-lookup"><span data-stu-id="a0c3b-163">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="a0c3b-164">Följ anvisningarna nedan om du vill installera uppdateringarna för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-164">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="a0c3b-165">Placera enheten i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-165">Place the device in the Maintenance mode.</span></span> <span data-ttu-id="a0c3b-166">Observera att du inte bör använda Windows PowerShell-fjärrkommunikation när du ansluter till en enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-166">Note that you should not use Windows PowerShell remoting when connecting to a device in Maintenance mode.</span></span> <span data-ttu-id="a0c3b-167">I stället köra denna cmdlet på styrenheten för enheten när du är ansluten till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-167">Instead run this cmdlet on the device controller when connected through the device serial console.</span></span> <span data-ttu-id="a0c3b-168">Ange:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-168">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="a0c3b-169">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-169">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="a0c3b-170">Båda domänkontrollanterna starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-170">Both the controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="a0c3b-171">Ange följande för att installera uppdateringen av inbyggd programvara för disk:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-171">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="a0c3b-172">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-172">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="a0c3b-173">Övervaka installationsförloppet med `Get-HcsUpdateStatus`-kommandot.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-173">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="a0c3b-174">Uppdateringen är slutförd när `RunInProgress` ändras till `False`.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-174">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="a0c3b-175">När installationen är färdig startas styrenheten som snabbkorrigeringen i underhållsläge installerades på om.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-175">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="a0c3b-176">Logga in som alternativ 1 med fullständig åtkomst och verifiera versionen för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-176">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="a0c3b-177">Ange:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-177">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="a0c3b-178">De förväntade versionerna för den inbyggda programvaran för disken är:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-178">The expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="a0c3b-179">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-179">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
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
   
    <span data-ttu-id="a0c3b-180">Kör `Get-HcsFirmwareVersion`-kommandot på den andra styrenheten för att verifiera att programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-180">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="a0c3b-181">Du kan sedan avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-181">You can then exit the maintenance mode.</span></span> <span data-ttu-id="a0c3b-182">För att göra det anger du följande kommando för varje enhetsstyrenhet:</span><span class="sxs-lookup"><span data-stu-id="a0c3b-182">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="a0c3b-183">Domänkontrollanterna starta om när du avslutar underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-183">The controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="a0c3b-184">Efter att uppdateringarna för den inbyggda programvaran för disken har tillämpats och enheten har avslutat underhållsläget kan du gå tillbaka till den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-184">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="a0c3b-185">Observera att portalen inte kan se till att du installerat Underhåll läge uppdateringarna i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="a0c3b-185">Note that the portal might not show that you installed the Maintenance mode updates for 24 hours.</span></span>

