<!--author=SharS last changed: 02/22/16-->

### <a name="to-configure-and-register-the-device"></a><span data-ttu-id="eff89-101">Konfigurera och registrera enheten</span><span class="sxs-lookup"><span data-stu-id="eff89-101">To configure and register the device</span></span>
1. <span data-ttu-id="eff89-102">Gå in i Windows PowerShell-gränssnittet på din StorSimple-enhets seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="eff89-102">Access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="eff89-103">Mer instruktioner finns i [Använd PuTTY för att ansluta till enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="eff89-103">See [Use PuTTY to connect to the device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="eff89-104">**Se till att följa proceduren exakt för att du ska få åtkomst till konsolen.**</span><span class="sxs-lookup"><span data-stu-id="eff89-104">**Be sure to follow the procedure exactly or you will not be able to access the console.**</span></span>
2. <span data-ttu-id="eff89-105">I sessionen som öppnas, trycker du på Retur en gång för att få fram en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="eff89-105">In the session that opens up, press Enter one time to get a command prompt.</span></span> 
3. <span data-ttu-id="eff89-106">Du kommer att uppmanas att välja det språk som du vill ställa in för din enhet.</span><span class="sxs-lookup"><span data-stu-id="eff89-106">You will be prompted to choose the language that you would like to set for your device.</span></span> <span data-ttu-id="eff89-107">Ange språk och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="eff89-107">Specify the language, and then press Enter.</span></span> 
   
    ![StorSimple konfigurera och registrera enhet 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)
4. <span data-ttu-id="eff89-109">I menyn för seriekonsolen som visas, väljer du alternativ 1 för att logga in med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="eff89-109">In the serial console menu that is presented, choose option 1 to log on with full access.</span></span> 
   
    ![StorSimple registrera enhet 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
5. <span data-ttu-id="eff89-111">Utför följande steg om du vill konfigurera de minsta nödvändiga nätverksinställningarna för enheten.</span><span class="sxs-lookup"><span data-stu-id="eff89-111">Perform the following steps to configure the minimum required network settings for your device.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="eff89-112">De här konfigurationsstegen behöver genomföras på den aktiva styrenheten för enheten.</span><span class="sxs-lookup"><span data-stu-id="eff89-112">These configuration steps need to be performed on the active controller of the device.</span></span> <span data-ttu-id="eff89-113">Menyn för seriekonsolen indikerar status för styrenheten i banderollmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="eff89-113">The serial console menu indicates the controller state in the banner message.</span></span> <span data-ttu-id="eff89-114">Om du inte är ansluta till den aktiva styrenheten, kopplar du från och ansluter sedan till den aktiva styrenheten.</span><span class="sxs-lookup"><span data-stu-id="eff89-114">If you are not connect to the active controller, disconnect and then connect to the active controller.</span></span>
   > 
   > 
   
   1. <span data-ttu-id="eff89-115">Ange ditt lösenord i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="eff89-115">At the command prompt, type your password.</span></span> <span data-ttu-id="eff89-116">Enheten standardlösenord är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="eff89-116">The default device password is **Password1**.</span></span>
   2. <span data-ttu-id="eff89-117">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eff89-117">Type the following command:</span></span>
      
        `Invoke-HcsSetupWizard`
   3. <span data-ttu-id="eff89-118">En installationsguide kommer visas och hjälpa dig att konfigurera nätverksinställningarna för enheten.</span><span class="sxs-lookup"><span data-stu-id="eff89-118">A setup wizard will appear to help you configure the network settings for the device.</span></span> <span data-ttu-id="eff89-119">Ange följande information:</span><span class="sxs-lookup"><span data-stu-id="eff89-119">Supply the following information:</span></span> 
      
      * <span data-ttu-id="eff89-120">IP-adress för DATA 0-nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="eff89-120">IP address for DATA 0 network interface</span></span>
      * <span data-ttu-id="eff89-121">Nätmask</span><span class="sxs-lookup"><span data-stu-id="eff89-121">Subnet mask</span></span>
      * <span data-ttu-id="eff89-122">Gateway</span><span class="sxs-lookup"><span data-stu-id="eff89-122">Gateway</span></span>
      * <span data-ttu-id="eff89-123">IP-adress för primär DNS-server</span><span class="sxs-lookup"><span data-stu-id="eff89-123">IP address for Primary DNS server</span></span>
      * <span data-ttu-id="eff89-124">IP-adress för primär NTP-server</span><span class="sxs-lookup"><span data-stu-id="eff89-124">IP address for Primary NTP server</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="eff89-125">Du kan behöva vänta några minuter för nätmask och DNS-inställningar som ska användas.</span><span class="sxs-lookup"><span data-stu-id="eff89-125">You may have to wait for a few minutes for the subnet mask and DNS settings to be applied.</span></span> 
      > 
      > 
   4. <span data-ttu-id="eff89-126">Alternativt kan du konfigurera din webbproxyserver.</span><span class="sxs-lookup"><span data-stu-id="eff89-126">Optionally, configure your web proxy server.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="eff89-127">Även om webbproxykonfigurationen är valfri, Tänk på att om du använder en webbproxy, du kan bara konfigurera den här.</span><span class="sxs-lookup"><span data-stu-id="eff89-127">Although web proxy configuration is optional, be aware that if you use a web proxy, you can only configure it here.</span></span> <span data-ttu-id="eff89-128">Mer information finns i [Konfigurera en webbproxy för din enhet](../articles/storsimple/storsimple-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="eff89-128">For more information, go to [Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md).</span></span> 
      > 
      > 
6. <span data-ttu-id="eff89-129">Tryck på Ctrl + C om du vill avsluta installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="eff89-129">Press Ctrl + C to exit the setup wizard.</span></span>
7. <span data-ttu-id="eff89-130">Installera uppdateringar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="eff89-130">Install the updates as follows:</span></span>
   
   1. <span data-ttu-id="eff89-131">Använd följande cmdlet för att ange IP-adresser på båda domänkontrollanterna:</span><span class="sxs-lookup"><span data-stu-id="eff89-131">Use the following cmdlet to set IPs on both the controllers:</span></span>
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. <span data-ttu-id="eff89-132">I Kommandotolken, kör `Get-HcsUpdateAvailability`.</span><span class="sxs-lookup"><span data-stu-id="eff89-132">At the command prompt, run `Get-HcsUpdateAvailability`.</span></span> <span data-ttu-id="eff89-133">Du får ett meddelande att uppdateringar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="eff89-133">You should be notified that updates are available.</span></span>
   3. <span data-ttu-id="eff89-134">Kör `Start-HcsUpdate`.</span><span class="sxs-lookup"><span data-stu-id="eff89-134">Run `Start-HcsUpdate`.</span></span> <span data-ttu-id="eff89-135">Du kan köra det här kommandot på en nod.</span><span class="sxs-lookup"><span data-stu-id="eff89-135">You can run this command on any node.</span></span> <span data-ttu-id="eff89-136">Uppdateringarna tillämpas på den första domänkontrollanten, styrenheten växlar och sedan uppdateringarna tillämpas på en annan domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="eff89-136">Updates will be applied on the first controller, the controller will fail over, and then the updates will be applied on the other controller.</span></span>
      
      <span data-ttu-id="eff89-137">Du kan övervaka förloppet för uppdateringen genom att köra `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="eff89-137">You can monitor the progress of the update by running `Get-HcsUpdateStatus`.</span></span>    
      
      <span data-ttu-id="eff89-138">Följande exempel på utdata visar att uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="eff89-138">The following sample output shows the update in progress.</span></span>
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   : 
      ````
      
      <span data-ttu-id="eff89-139">Följande exempel på utdata visar att uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="eff89-139">The following sample output indicates that the update is finished.</span></span>
      
      ````
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      
      ````
      
      <span data-ttu-id="eff89-140">Det kan ta upp till 11 timmar att tillämpa alla uppdateringar, inklusive Windows-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="eff89-140">It may take up to 11 hours to apply all the updates, including the Windows Updates.</span></span>

8. <span data-ttu-id="eff89-141">När alla uppdateringar har installerats, kör du följande cmdlet för att bekräfta att programuppdateringarna har tillämpats korrekt.</span><span class="sxs-lookup"><span data-stu-id="eff89-141">After all the updates are successfully installed, run the following cmdlet to confirm that the software updates were applied correctly:</span></span>
   
     `Get-HcsSystem`
   
    <span data-ttu-id="eff89-142">Du bör se följande versioner:</span><span class="sxs-lookup"><span data-stu-id="eff89-142">You should see the following versions:</span></span>
   
   * <span data-ttu-id="eff89-143">HcsSoftwareVersion: 6.3.9600.17491</span><span class="sxs-lookup"><span data-stu-id="eff89-143">HcsSoftwareVersion: 6.3.9600.17491</span></span>
   * <span data-ttu-id="eff89-144">CisAgentVersion: 1.0.9037.0</span><span class="sxs-lookup"><span data-stu-id="eff89-144">CisAgentVersion: 1.0.9037.0</span></span>
   * <span data-ttu-id="eff89-145">MdsAgentVersion: 26.0.4696.1433</span><span class="sxs-lookup"><span data-stu-id="eff89-145">MdsAgentVersion: 26.0.4696.1433</span></span>
9. <span data-ttu-id="eff89-146">Kör följande cmdlet för att bekräfta att firmware-uppdatering har tillämpats korrekt:</span><span class="sxs-lookup"><span data-stu-id="eff89-146">Run the following cmdlet to confirm that the firmware update was applied correctly:</span></span>
   
    <span data-ttu-id="eff89-147">`Start-HcsFirmwareCheck`.</span><span class="sxs-lookup"><span data-stu-id="eff89-147">`Start-HcsFirmwareCheck`.</span></span>
   
     <span data-ttu-id="eff89-148">Status för inbyggd programvara ska vara **UpToDate**.</span><span class="sxs-lookup"><span data-stu-id="eff89-148">The firmware status should be **UpToDate**.</span></span>
10. <span data-ttu-id="eff89-149">Kör följande cmdlet för att peka enheten på Microsoft Azure Government-portalen (eftersom den pekar till offentliga Azure klassiska portal som standard).</span><span class="sxs-lookup"><span data-stu-id="eff89-149">Run the following cmdlet to point the device to the Microsoft Azure Government portal (because it points to the public Azure classic portal by default).</span></span> <span data-ttu-id="eff89-150">Både domänkontrollanter kommer att startas om.</span><span class="sxs-lookup"><span data-stu-id="eff89-150">This will restart both controllers.</span></span> <span data-ttu-id="eff89-151">Vi rekommenderar att du använder två PuTTY sessioner samtidigt ansluta till båda domänkontrollanterna så att du kan se när varje styrenhet har startats om.</span><span class="sxs-lookup"><span data-stu-id="eff89-151">We recommend that you use two PuTTY sessions to simultaneously connect to both controllers so that you can see when each controller is restarted.</span></span>
    
     `Set-CloudPlatform -AzureGovt_US`
    
    <span data-ttu-id="eff89-152">Visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="eff89-152">You will see a confirmation message.</span></span> <span data-ttu-id="eff89-153">Acceptera standardvärdet (**Y**).</span><span class="sxs-lookup"><span data-stu-id="eff89-153">Accept the default (**Y**).</span></span>
11. <span data-ttu-id="eff89-154">Kör följande cmdlet för att fortsätta installationsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="eff89-154">Run the following cmdlet to resume setup:</span></span>
    
     `Invoke-HcsSetupWizard`
    
     ![Återuppta installationsguiden](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)
    
    <span data-ttu-id="eff89-156">När du fortsätter installationen kommer guiden att uppdatering 1-version (som motsvarar versionen 17469).</span><span class="sxs-lookup"><span data-stu-id="eff89-156">When you resume setup, the wizard will be the Update 1 version (which corresponds to version 17469).</span></span> 
12. <span data-ttu-id="eff89-157">Acceptera nätverksinställningarna.</span><span class="sxs-lookup"><span data-stu-id="eff89-157">Accept the network settings.</span></span> <span data-ttu-id="eff89-158">Ett verifieringsmeddelande visas när du har accepterat varje inställning.</span><span class="sxs-lookup"><span data-stu-id="eff89-158">You will see a validation message after you accept each setting.</span></span>
13. <span data-ttu-id="eff89-159">Av säkerhetsskäl upphör enhetens administratörslösenord att gälla efter den första sessionen. Du måste ändra lösenordet nu.</span><span class="sxs-lookup"><span data-stu-id="eff89-159">For security reasons, the device administrator password expires after the first session, and you will need to change it now.</span></span> <span data-ttu-id="eff89-160">Ange ett administratörslösenord för enheten när du ombes göra det.</span><span class="sxs-lookup"><span data-stu-id="eff89-160">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="eff89-161">Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="eff89-161">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="eff89-162">Lösenordet måste innehålla tre av följande: gemener, versaler, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="eff89-162">The password must contain three of the following: lowercase, uppercase, numeric, and special characters.</span></span>
    
    <br/>![StorSimple registrera enhet 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)
14. <span data-ttu-id="eff89-164">Det sista steget i installationsguiden registrerar din enhet med StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eff89-164">The final step in the setup wizard registers your device with the StorSimple Manager service.</span></span> <span data-ttu-id="eff89-165">För detta behöver du tjänstens registreringsnyckel som du fick i [steg 2: Hämta nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="eff89-165">For this, you will need the service registration key that you obtained in [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span> <span data-ttu-id="eff89-166">Efter att du angett registreringsnyckeln kan du behöva vänta 2-3 minuter innan enheten är registrerad.</span><span class="sxs-lookup"><span data-stu-id="eff89-166">After you supply the registration key, you may need to wait for 2-3 minutes before the device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="eff89-167">Du kan trycka på Ctrl + C när som helst för att avsluta installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="eff89-167">You can press Ctrl + C at any time to exit the setup wizard.</span></span> <span data-ttu-id="eff89-168">Om du har angett alla nätverksinställningar (IP-adress för Data 0, nätmask och Gateway), kommer informationen att sparas.</span><span class="sxs-lookup"><span data-stu-id="eff89-168">If you have entered all the network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    > 
    > 
    
    ![StorSimple registrering pågår](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)
15. <span data-ttu-id="eff89-170">När enheten är registrerad visas en krypteringsnyckel för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="eff89-170">After the device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="eff89-171">Kopiera den här nyckeln och spara den på säker plats.</span><span class="sxs-lookup"><span data-stu-id="eff89-171">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="eff89-172">**Den här nyckeln kommer att krävas med registreringsnyckeln för tjänsten för att registrera ytterligare enheter med StorSimple Manager-tjänsten.**</span><span class="sxs-lookup"><span data-stu-id="eff89-172">**This key will be required with the service registration key to register additional devices with the StorSimple Manager service.**</span></span> <span data-ttu-id="eff89-173">Referera till [StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) för ytterligare information om den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="eff89-173">Refer to [StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple registrera enhet 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > <span data-ttu-id="eff89-175">Om du vill kopiera text från seriekonsolfönstret, markerar du bara texten.</span><span class="sxs-lookup"><span data-stu-id="eff89-175">To copy the text from the serial console window, simply select the text.</span></span> <span data-ttu-id="eff89-176">Sedan ska du kunna klistra in den i urklipp eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eff89-176">You should then be able to paste it in the clipboard or any text editor.</span></span> 
    > 
    > <span data-ttu-id="eff89-177">ANVÄND INTE Ctrl + C för att kopiera krypteringsnyckeln för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="eff89-177">DO NOT use Ctrl + C to copy the service data encryption key.</span></span> <span data-ttu-id="eff89-178">Ctrl + C avslutar installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="eff89-178">Using Ctrl + C will cause you to exit the setup wizard.</span></span> <span data-ttu-id="eff89-179">Som ett resultat, kommer enhetens administratörslösenord inte att ändras och enheten kommer återgå till standardlösenordet.</span><span class="sxs-lookup"><span data-stu-id="eff89-179">As a result, the device administrator password will not be changed and the device will revert to the default password.</span></span>
    > 
    > 
16. <span data-ttu-id="eff89-180">Avsluta seriekonsolen.</span><span class="sxs-lookup"><span data-stu-id="eff89-180">Exit the serial console.</span></span>
17. <span data-ttu-id="eff89-181">Gå tillbaka till den offentliga Azure-portalen och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="eff89-181">Return to the Azure Government Portal, and complete the following steps:</span></span>
    
    1. <span data-ttu-id="eff89-182">Dubbelklicka på StorSimple Manager-tjänsten för att komma in på **Snabbstart**-sidan.</span><span class="sxs-lookup"><span data-stu-id="eff89-182">Double-click your StorSimple Manager service to access the **Quick Start** page.</span></span>
    2. <span data-ttu-id="eff89-183">Klicka på **Visa anslutna enheter**.</span><span class="sxs-lookup"><span data-stu-id="eff89-183">Click **View connected devices**.</span></span>
    3. <span data-ttu-id="eff89-184">På sidan **Enheter** kontrollerar du att enheten har lyckats ansluta till tjänsten genom att kontrollera dess status.</span><span class="sxs-lookup"><span data-stu-id="eff89-184">On the **Devices** page, verify that the device has successfully connected to the service by looking up the status.</span></span> <span data-ttu-id="eff89-185">Enhetens status ska vara **Online**.</span><span class="sxs-lookup"><span data-stu-id="eff89-185">The device status should be **Online**.</span></span>
       
        ![StorSimple-enhetssidan](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
       
        <span data-ttu-id="eff89-187">Om enhetens status är **Offline**, väntar du några minuter på att enheten ska ansluta.</span><span class="sxs-lookup"><span data-stu-id="eff89-187">If the device status is **Offline**, wait for a couple of minutes for the device to come online.</span></span> 
       
        <span data-ttu-id="eff89-188">Om enheten fortfarande är offline efter några minuter, måste du kontrollera att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="eff89-188">If the device is still offline after a few minutes, then you need to make sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md).</span></span> 
       
        <span data-ttu-id="eff89-189">Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används av service bus för kommunikation av StorSimple Manager-tjänsten till enheten.</span><span class="sxs-lookup"><span data-stu-id="eff89-189">Verify that port 9354 is open for outbound communication as this is used by the service bus for StorSimple Manager service-to-device communication.</span></span>

