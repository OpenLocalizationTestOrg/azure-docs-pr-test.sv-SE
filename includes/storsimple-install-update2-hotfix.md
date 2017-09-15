<!--author=alkohli last changed: 03/17/16-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="bae72-101">Ladda ned snabbkorrigerar</span><span class="sxs-lookup"><span data-stu-id="bae72-101">To download hotfixes</span></span>
<span data-ttu-id="bae72-102">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="bae72-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="bae72-103">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="bae72-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="bae72-104">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="bae72-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="bae72-105">![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="bae72-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="bae72-106">I sökrutan i Microsoft Update-katalogen ange Knowledge Base (KB) antalet den snabbkorrigering som du vill hämta, till exempel **3121901**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="bae72-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **3121901**, and then click **Search**.</span></span>
   
    <span data-ttu-id="bae72-107">Snabbkorrigeringen listan visas till exempel **kumulativa programvara paket Update 2.0 för StorSimple 8000-serien**.</span><span class="sxs-lookup"><span data-stu-id="bae72-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 2.0 for StorSimple 8000 Series**.</span></span>
   
    ![Sökkatalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="bae72-109">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bae72-109">Click **Add**.</span></span> <span data-ttu-id="bae72-110">Uppdateringen läggs till i korgen.</span><span class="sxs-lookup"><span data-stu-id="bae72-110">The update is added to the basket.</span></span>
5. <span data-ttu-id="bae72-111">Sök efter eventuella ytterligare snabbkorrigeringar som anges i tabellen ovan (**3121900**, **3080728**, **3090322**, och **3121899**), och lägga till den korg.</span><span class="sxs-lookup"><span data-stu-id="bae72-111">Search for any additional hotfixes listed in the table above (**3121900**, **3080728**, **3090322**, and **3121899**), and add each the basket.</span></span>
6. <span data-ttu-id="bae72-112">Klicka på **View Basket** (Visa korg).</span><span class="sxs-lookup"><span data-stu-id="bae72-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="bae72-113">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="bae72-113">Click **Download**.</span></span> <span data-ttu-id="bae72-114">Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas.</span><span class="sxs-lookup"><span data-stu-id="bae72-114">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="bae72-115">Uppdateringarna hämtas till den angivna platsen och placeras i en undermapp med samma namn som uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="bae72-115">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="bae72-116">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="bae72-116">The folder can also be copied to a network share that is reachable from the device.</span></span>

> [!NOTE]
> <span data-ttu-id="bae72-117">Snabbkorrigeringarna måste vara tillgänglig från båda domänkontrollanter för att identifiera eventuella felmeddelanden från peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="bae72-117">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="bae72-118">Installera och verifiera snabbkorrigeringar i normalläge</span><span class="sxs-lookup"><span data-stu-id="bae72-118">To install and verify regular mode hotfixes</span></span>
<span data-ttu-id="bae72-119">Utför följande steg för att installera och verifiera snabbkorrigeringar i normalläge.</span><span class="sxs-lookup"><span data-stu-id="bae72-119">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="bae72-120">Om du redan har installerat dem med hjälp av Azure Portal, gå vidare till [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="bae72-120">If you already installed them using the Azure Portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="bae72-121">Gå in i Windows PowerShell-gränssnittet på din StorSimple-enhets seriekonsol för att installera snabbkorrigerarna.</span><span class="sxs-lookup"><span data-stu-id="bae72-121">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="bae72-122">Följ de detaljerade instruktionerna i [Använd PuTTY för att ansluta till enhetens seriekonsol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="bae72-122">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="bae72-123">Tryck på **Retur** i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="bae72-123">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="bae72-124">Välj **Alternativ 1** för att logga in på enheten med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bae72-124">Select **Option 1** to log on to the device with full access.</span></span>
3. <span data-ttu-id="bae72-125">Ange följande i kommandotolken för att installera snabbkorrigeringen:</span><span class="sxs-lookup"><span data-stu-id="bae72-125">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="bae72-126">Använd IP i stället för DNS i resurssökvägen i kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="bae72-126">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="bae72-127">Parametern för autentiseringsuppgifter används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="bae72-127">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="bae72-128">Vi rekommenderar att du använder parametern för autentiseringsuppgifter för att få åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="bae72-128">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="bae72-129">Även resurser som är öppna för ”alla” är vanligtvis inte öppna för icke-autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="bae72-129">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="bae72-130">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bae72-130">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```
4. <span data-ttu-id="bae72-131">Skriv **Y** när du uppmanas att bekräfta installationen av snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="bae72-131">Type **Y** when prompted to confirm the hotfix installation.</span></span>
5. <span data-ttu-id="bae72-132">Övervaka uppdateringen med hjälp av `Get-HcsUpdateStatus`-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bae72-132">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span>
   
    <span data-ttu-id="bae72-133">Följande exempel på utdata visar att uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="bae72-133">The following sample output shows the update in progress.</span></span> <span data-ttu-id="bae72-134">`RunInprogress` kommer att vara `True` när uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="bae72-134">The `RunInprogress` will be `True` when the update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 12/21/2015 10:36:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="bae72-135">Följande exempel på utdata visar att uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="bae72-135">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="bae72-136">`RunInProgress` kommer att vara `False` när uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="bae72-136">The `RunInProgress` will be `False` when the update has completed.</span></span>
   
    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 12/21/2015 10:59:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > <span data-ttu-id="bae72-137">I vissa fall rapporterar cmdlet `False` när uppdateringen fortfarande pågår.</span><span class="sxs-lookup"><span data-stu-id="bae72-137">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="bae72-138">Om du vill kontrollera att snabbkorrigeringen har slutförts väntar du några minuter och sedan kör du det här kommandot igen och kontrollerar att `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="bae72-138">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="bae72-139">Om det har det har snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="bae72-139">If it is, then the hotfix has completed.</span></span>

6. <span data-ttu-id="bae72-140">När programuppdateringen har slutförts, Upprepa steg 3-5 för att installera och övervaka SaaS-agenten och MDS-agenten.</span><span class="sxs-lookup"><span data-stu-id="bae72-140">After the software update is complete, repeat steps 3-5 to install and monitor the SaaS agent and MDS agent .</span></span> <span data-ttu-id="bae72-141">Se till att `all-hcsmdssoftwareupdate_0b438ddf0d5b686aada2378b754fac8c7f2160e9.exe` är installerad före `all-cismdsagentupdatebundle_f98e62f4d56c79e2a6644d027af7a2393a93827a.exe`.</span><span class="sxs-lookup"><span data-stu-id="bae72-141">Ensure that `all-hcsmdssoftwareupdate_0b438ddf0d5b686aada2378b754fac8c7f2160e9.exe` is installed before `all-cismdsagentupdatebundle_f98e62f4d56c79e2a6644d027af7a2393a93827a.exe`.</span></span>
7. <span data-ttu-id="bae72-142">Kontrollera programvaruversioner system.</span><span class="sxs-lookup"><span data-stu-id="bae72-142">Verify the system software versions.</span></span> <span data-ttu-id="bae72-143">Ange:</span><span class="sxs-lookup"><span data-stu-id="bae72-143">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="bae72-144">Du bör se följande versioner:</span><span class="sxs-lookup"><span data-stu-id="bae72-144">You should see the following versions:</span></span>
   
   * <span data-ttu-id="bae72-145">HcsSoftwareVersion: 6.3.9600.17673</span><span class="sxs-lookup"><span data-stu-id="bae72-145">HcsSoftwareVersion: 6.3.9600.17673</span></span>
   * <span data-ttu-id="bae72-146">CisAgentVersion: 1.0.9150.0</span><span class="sxs-lookup"><span data-stu-id="bae72-146">CisAgentVersion: 1.0.9150.0</span></span>
   * <span data-ttu-id="bae72-147">MdsAgentVersion: 30.0.4698.13</span><span class="sxs-lookup"><span data-stu-id="bae72-147">MdsAgentVersion: 30.0.4698.13</span></span>
     
     <span data-ttu-id="bae72-148">Om versionsnumren inte ändras efter att uppdateringen, visar att snabbkorrigeringen har kunde inte tillämpas.</span><span class="sxs-lookup"><span data-stu-id="bae72-148">If the version numbers do not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="bae72-149">Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="bae72-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
8. <span data-ttu-id="bae72-150">Upprepa steg 3-5 om du vill installera snabbkorrigeringar återstående regular-läge.</span><span class="sxs-lookup"><span data-stu-id="bae72-150">Repeat steps 3-5 to install the remaining regular-mode hotfixes.</span></span>
   
   * <span data-ttu-id="bae72-151">Drivrutinens LSI - KB3121900</span><span class="sxs-lookup"><span data-stu-id="bae72-151">The LSI driver - KB3121900</span></span>
   * <span data-ttu-id="bae72-152">Uppdatering för Storport - KB3080728</span><span class="sxs-lookup"><span data-stu-id="bae72-152">The Storport update - KB3080728</span></span>
   * <span data-ttu-id="bae72-153">Uppdateringen Spaceport - KB3090322</span><span class="sxs-lookup"><span data-stu-id="bae72-153">The Spaceport update - KB3090322</span></span>

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="bae72-154">Installera och verifiera snabbkorrigeringar i underhållsläge</span><span class="sxs-lookup"><span data-stu-id="bae72-154">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="bae72-155">Använd KB3121899 att installera uppdateringar av inbyggd disk.</span><span class="sxs-lookup"><span data-stu-id="bae72-155">Use KB3121899 to install disk firmware updates.</span></span> <span data-ttu-id="bae72-156">Det här är störande uppdateringar och tar cirka 30 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="bae72-156">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="bae72-157">Du kan välja att installera dem i ett planerat underhållsfönster genom att ansluta till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="bae72-157">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="bae72-158">Observera att om den inbyggda programvaran för disken redan är uppdaterad så behöver du inte installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="bae72-158">Note that if your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="bae72-159">Kör `Get-HcsUpdateAvailability`-cmdleten från enhetens seriekonsol för att kontrollera om det finns tillgängliga uppdateringar och om uppdateringarna är störande (underhållsläge) eller avbrottsfria (standardläget).</span><span class="sxs-lookup"><span data-stu-id="bae72-159">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="bae72-160">Följ anvisningarna nedan om du vill installera uppdateringarna för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="bae72-160">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="bae72-161">Placera enheten i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="bae72-161">Place the device in the Maintenance mode.</span></span> <span data-ttu-id="bae72-162">Observera att du inte bör använda Windows PowerShell-fjärrkommunikation när du ansluter till en enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="bae72-162">Note that you should not use Windows PowerShell remoting when connecting to a device in Maintenance mode.</span></span> <span data-ttu-id="bae72-163">I stället köra denna cmdlet på styrenheten för enheten när du är ansluten till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="bae72-163">Instead run this cmdlet on the device controller when connected through the device serial console.</span></span> <span data-ttu-id="bae72-164">Ange:</span><span class="sxs-lookup"><span data-stu-id="bae72-164">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="bae72-165">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bae72-165">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17664
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="bae72-166">Båda domänkontrollanterna starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="bae72-166">Both the controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="bae72-167">Ange följande för att installera uppdateringen av inbyggd programvara för disk:</span><span class="sxs-lookup"><span data-stu-id="bae72-167">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="bae72-168">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bae72-168">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="bae72-169">Övervaka installationsförloppet med `Get-HcsUpdateStatus`-kommandot.</span><span class="sxs-lookup"><span data-stu-id="bae72-169">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="bae72-170">Uppdateringen är slutförd när `RunInProgress` ändras till `False`.</span><span class="sxs-lookup"><span data-stu-id="bae72-170">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="bae72-171">När installationen är färdig startas styrenheten som snabbkorrigeringen i underhållsläge installerades på om.</span><span class="sxs-lookup"><span data-stu-id="bae72-171">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="bae72-172">Logga in som alternativ 1 med fullständig åtkomst och verifiera versionen för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="bae72-172">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="bae72-173">Ange:</span><span class="sxs-lookup"><span data-stu-id="bae72-173">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="bae72-174">De förväntade versionerna för den inbyggda programvaran för disken är:</span><span class="sxs-lookup"><span data-stu-id="bae72-174">The expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="bae72-175">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bae72-175">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17664
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
   
    <span data-ttu-id="bae72-176">Kör `Get-HcsFirmwareVersion`-kommandot på den andra styrenheten för att verifiera att programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="bae72-176">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="bae72-177">Du kan sedan avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="bae72-177">You can then exit the maintenance mode.</span></span> <span data-ttu-id="bae72-178">För att göra det anger du följande kommando för varje enhetsstyrenhet:</span><span class="sxs-lookup"><span data-stu-id="bae72-178">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="bae72-179">Domänkontrollanterna starta om när du avslutar underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="bae72-179">The controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="bae72-180">Efter att uppdateringarna för den inbyggda programvaran för disken har tillämpats och enheten har avslutat underhållsläget kan du gå tillbaka till den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bae72-180">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="bae72-181">Observera att portalen inte kan se till att du installerat Underhåll läge uppdateringarna i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="bae72-181">Note that the portal might not show that you installed the Maintenance mode updates for 24 hours.</span></span>

