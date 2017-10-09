<!--author=SharS last changed: 02/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="da7dd-101">tooconfigure och registrera hello-enhet</span><span class="sxs-lookup"><span data-stu-id="da7dd-101">tooconfigure and register hello device</span></span>
1. <span data-ttu-id="da7dd-102">Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="da7dd-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="da7dd-103">Se [använda PuTTY tooconnect toohello enhetens seriekonsol](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="da7dd-103">See [Use PuTTY tooconnect toohello device serial console](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="da7dd-104">**Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**</span><span class="sxs-lookup"><span data-stu-id="da7dd-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>
2. <span data-ttu-id="da7dd-105">Tryck på RETUR en gång tooget Kommandotolken i hello-sessionen som öppnas.</span><span class="sxs-lookup"><span data-stu-id="da7dd-105">In hello session that opens up, press Enter one time tooget a command prompt.</span></span>
3. <span data-ttu-id="da7dd-106">Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet.</span><span class="sxs-lookup"><span data-stu-id="da7dd-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="da7dd-107">Ange hello språk och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="da7dd-107">Specify hello language, and then press Enter.</span></span>
   
    ![StorSimple konfigurera och registrera enhet 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. <span data-ttu-id="da7dd-109">Välj alternativ 1 toolog hello seriekonsolen menyn som visas på med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="da7dd-109">In hello serial console menu that is presented, choose option 1 toolog on with full access.</span></span>
   
    ![StorSimple registrera enhet 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. <span data-ttu-id="da7dd-111">Utföra hello följande steg tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten.</span><span class="sxs-lookup"><span data-stu-id="da7dd-111">Perform hello following steps tooconfigure hello minimum required network settings for your device.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="da7dd-112">De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="da7dd-112">These configuration steps need toobe performed on hello active controller of hello device.</span></span> <span data-ttu-id="da7dd-113">hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello.</span><span class="sxs-lookup"><span data-stu-id="da7dd-113">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="da7dd-114">Om du inte ansluta toohello aktiva styrenheten koppla från och ansluta toohello aktiva styrenhet.</span><span class="sxs-lookup"><span data-stu-id="da7dd-114">If you are not connect toohello active controller, disconnect and then connect toohello active controller.</span></span>
   > 
   > 
   
   1. <span data-ttu-id="da7dd-115">Skriv in lösenordet i hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="da7dd-115">At hello command prompt, type your password.</span></span> <span data-ttu-id="da7dd-116">hello enheten standardlösenord är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="da7dd-116">hello default device password is **Password1**.</span></span>
   2. <span data-ttu-id="da7dd-117">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="da7dd-117">Type hello following command:</span></span>
      
        `Invoke-HcsSetupWizard`
   3. <span data-ttu-id="da7dd-118">En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="da7dd-118">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="da7dd-119">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="da7dd-119">Supply hello following information:</span></span>
      
      * <span data-ttu-id="da7dd-120">IP-adress för DATA 0-nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="da7dd-120">IP address for DATA 0 network interface</span></span>
      * <span data-ttu-id="da7dd-121">Nätmask</span><span class="sxs-lookup"><span data-stu-id="da7dd-121">Subnet mask</span></span>
      * <span data-ttu-id="da7dd-122">Gateway</span><span class="sxs-lookup"><span data-stu-id="da7dd-122">Gateway</span></span>
      * <span data-ttu-id="da7dd-123">IP-adress för primär DNS-server</span><span class="sxs-lookup"><span data-stu-id="da7dd-123">IP address for Primary DNS server</span></span>
      * <span data-ttu-id="da7dd-124">IP-adress för primär NTP-server</span><span class="sxs-lookup"><span data-stu-id="da7dd-124">IP address for Primary NTP server</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="da7dd-125">Du kan ha toowait några minuter för hello nätmask och DNS-inställningar toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="da7dd-125">You may have toowait for a few minutes for hello subnet mask and DNS settings toobe applied.</span></span>
      > 
      > 
   4. <span data-ttu-id="da7dd-126">Alternativt kan du konfigurera din webbproxyserver.</span><span class="sxs-lookup"><span data-stu-id="da7dd-126">Optionally, configure your web proxy server.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="da7dd-127">Även om webbproxykonfigurationen är valfri, Tänk på att om du använder en webbproxy, du kan bara konfigurera den här.</span><span class="sxs-lookup"><span data-stu-id="da7dd-127">Although web proxy configuration is optional, be aware that if you use a web proxy, you can only configure it here.</span></span> <span data-ttu-id="da7dd-128">Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="da7dd-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md).</span></span>
      > 
      > 
6. <span data-ttu-id="da7dd-129">Tryck på Ctrl + C tooexit hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="da7dd-129">Press Ctrl + C tooexit hello setup wizard.</span></span>
7. <span data-ttu-id="da7dd-130">Installera hello uppdateringar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="da7dd-130">Install hello updates as follows:</span></span>
   
   1. <span data-ttu-id="da7dd-131">Använd följande cmdlet tooset IP-adresser på båda hello domänkontrollanter hello:</span><span class="sxs-lookup"><span data-stu-id="da7dd-131">Use hello following cmdlet tooset IPs on both hello controllers:</span></span>
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. <span data-ttu-id="da7dd-132">Kommandotolken hello kör `Get-HcsUpdateAvailability`.</span><span class="sxs-lookup"><span data-stu-id="da7dd-132">At hello command prompt, run `Get-HcsUpdateAvailability`.</span></span> <span data-ttu-id="da7dd-133">Du får ett meddelande att uppdateringar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="da7dd-133">You should be notified that updates are available.</span></span>
   3. <span data-ttu-id="da7dd-134">Kör `Start-HcsUpdate`.</span><span class="sxs-lookup"><span data-stu-id="da7dd-134">Run `Start-HcsUpdate`.</span></span> <span data-ttu-id="da7dd-135">Du kan köra det här kommandot på en nod.</span><span class="sxs-lookup"><span data-stu-id="da7dd-135">You can run this command on any node.</span></span> <span data-ttu-id="da7dd-136">Uppdateringarna tillämpas på hello första domänkontrollant, växlar hello domänkontrollant och sedan hello hello uppdateringarna tillämpas på andra domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="da7dd-136">Updates will be applied on hello first controller, hello controller will fail over, and then hello updates will be applied on hello other controller.</span></span>
      
      <span data-ttu-id="da7dd-137">Du kan övervaka hello hello uppdateringen genom att köra `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="da7dd-137">You can monitor hello progress of hello update by running `Get-HcsUpdateStatus`.</span></span>    
      
      <span data-ttu-id="da7dd-138">hello visar följande exempel på utdata hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="da7dd-138">hello following sample output shows hello update in progress.</span></span>
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      <span data-ttu-id="da7dd-139">följande exempel på utdata hello anger hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="da7dd-139">hello following sample output indicates that hello update is finished.</span></span>
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      <span data-ttu-id="da7dd-140">Det kan ta upp too11 timmar tooapply alla hello uppdateringar, inklusive hello Windows-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="da7dd-140">It may take up too11 hours tooapply all hello updates, including hello Windows Updates.</span></span>
8. <span data-ttu-id="da7dd-141">Kör hello följande cmdlet toopoint hello enheten toohello Microsoft Azure Government portalen (eftersom den pekar toohello offentliga klassiska Azure-portalen som standard).</span><span class="sxs-lookup"><span data-stu-id="da7dd-141">Run hello following cmdlet toopoint hello device toohello Microsoft Azure Government portal (because it points toohello public Azure classic portal by default).</span></span> <span data-ttu-id="da7dd-142">Både domänkontrollanter kommer att startas om.</span><span class="sxs-lookup"><span data-stu-id="da7dd-142">This will restart both controllers.</span></span> <span data-ttu-id="da7dd-143">Vi rekommenderar att du använder två PuTTY sessioner toosimultaneously ansluta tooboth domänkontrollanterna så att du kan se när varje styrenhet har startats om.</span><span class="sxs-lookup"><span data-stu-id="da7dd-143">We recommend that you use two PuTTY sessions toosimultaneously connect tooboth controllers so that you can see when each controller is restarted.</span></span>
   
    `Set-CloudPlatform -AzureGovt_US`
   
   <span data-ttu-id="da7dd-144">Visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="da7dd-144">You will see a confirmation message.</span></span> <span data-ttu-id="da7dd-145">Acceptera standardvärdet för hello (**Y**).</span><span class="sxs-lookup"><span data-stu-id="da7dd-145">Accept hello default (**Y**).</span></span>
9. <span data-ttu-id="da7dd-146">Kör följande cmdlet tooresume installationsprogrammet hello:</span><span class="sxs-lookup"><span data-stu-id="da7dd-146">Run hello following cmdlet tooresume setup:</span></span>
   
    `Invoke-HcsSetupWizard`
   
    ![Återuppta installationsguiden](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   <span data-ttu-id="da7dd-148">När du fortsätter installationen är hello guiden hello uppdatering 2-version.</span><span class="sxs-lookup"><span data-stu-id="da7dd-148">When you resume setup, hello wizard will be hello Update 2 version.</span></span>
10. <span data-ttu-id="da7dd-149">Acceptera hello nätverksinställningar.</span><span class="sxs-lookup"><span data-stu-id="da7dd-149">Accept hello network settings.</span></span> <span data-ttu-id="da7dd-150">Ett verifieringsmeddelande visas när du har accepterat varje inställning.</span><span class="sxs-lookup"><span data-stu-id="da7dd-150">You will see a validation message after you accept each setting.</span></span>
11. <span data-ttu-id="da7dd-151">Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange it nu.</span><span class="sxs-lookup"><span data-stu-id="da7dd-151">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="da7dd-152">Ange ett administratörslösenord för enheten när du ombes göra det.</span><span class="sxs-lookup"><span data-stu-id="da7dd-152">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="da7dd-153">Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="da7dd-153">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="da7dd-154">hello lösenordet måste innehålla tre hello följande: gemener, versaler, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="da7dd-154">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>
    
    <br/>![StorSimple registrera enhet 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. <span data-ttu-id="da7dd-156">hello sista steget i hello installationsguiden registrerar din enhet med hello StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="da7dd-156">hello final step in hello setup wizard registers your device with hello StorSimple Manager service.</span></span> <span data-ttu-id="da7dd-157">För detta måste du kommer måste hello tjänstens registreringsnyckel som du fick i [steg 2: hämta hello nyckel för tjänstregistrering](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="da7dd-157">For this, you will need hello service registration key that you obtained in [Step 2: Get hello service registration key](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span></span> <span data-ttu-id="da7dd-158">När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.</span><span class="sxs-lookup"><span data-stu-id="da7dd-158">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="da7dd-159">Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="da7dd-159">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="da7dd-160">Om du har angett alla hello nätverksinställningar (IP-adress för Data 0, nätmask och Gateway) kommer posterna att sparas.</span><span class="sxs-lookup"><span data-stu-id="da7dd-160">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    > 
    > 
    
    ![StorSimple registrering pågår](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. <span data-ttu-id="da7dd-162">När hello enheten är registrerad visas en krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="da7dd-162">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="da7dd-163">Kopiera den här nyckeln och spara den på säker plats.</span><span class="sxs-lookup"><span data-stu-id="da7dd-163">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="da7dd-164">**Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten.**</span><span class="sxs-lookup"><span data-stu-id="da7dd-164">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Manager service.**</span></span> <span data-ttu-id="da7dd-165">Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="da7dd-165">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple registrera enhet 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > <span data-ttu-id="da7dd-167">toocopy hello text från seriekonsolfönstret med hello, bara markera hello text.</span><span class="sxs-lookup"><span data-stu-id="da7dd-167">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="da7dd-168">Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="da7dd-168">You should then be able toopaste it in hello clipboard or any text editor.</span></span>
    > 
    > <span data-ttu-id="da7dd-169">Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="da7dd-169">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="da7dd-170">Med Ctrl + C kommer du tooexit hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="da7dd-170">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="da7dd-171">Därför kommer inte att ändra hello enhetens administratörslösenord och hello enheten kommer återgå toohello standardlösenordet.</span><span class="sxs-lookup"><span data-stu-id="da7dd-171">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    > 
    > 
14. <span data-ttu-id="da7dd-172">Avsluta hello seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="da7dd-172">Exit hello serial console.</span></span>
15. <span data-ttu-id="da7dd-173">Returnera toohello Azure Government-portalen och slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="da7dd-173">Return toohello Azure Government Portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="da7dd-174">Dubbelklicka på din StorSimple Manager service tooaccess hello **Snabbstart** sidan.</span><span class="sxs-lookup"><span data-stu-id="da7dd-174">Double-click your StorSimple Manager service tooaccess hello **Quick Start** page.</span></span>
    2. <span data-ttu-id="da7dd-175">Klicka på **Visa anslutna enheter**.</span><span class="sxs-lookup"><span data-stu-id="da7dd-175">Click **View connected devices**.</span></span>
    3. <span data-ttu-id="da7dd-176">På hello **enheter** kontrollerar hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status.</span><span class="sxs-lookup"><span data-stu-id="da7dd-176">On hello **Devices** page, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="da7dd-177">hello enhetens status ska vara **Online**.</span><span class="sxs-lookup"><span data-stu-id="da7dd-177">hello device status should be **Online**.</span></span>
       
        ![StorSimple-enhetssidan](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        <span data-ttu-id="da7dd-179">Om hello enhetens status är **Offline**, Vänta några minuter för hello enheten toocome online.</span><span class="sxs-lookup"><span data-stu-id="da7dd-179">If hello device status is **Offline**, wait for a couple of minutes for hello device toocome online.</span></span>
       
        <span data-ttu-id="da7dd-180">Om hello enheten fortfarande är offline efter några minuter, måste du till att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i toomake [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="da7dd-180">If hello device is still offline after a few minutes, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md).</span></span>
       
        <span data-ttu-id="da7dd-181">Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används i hello service bus för StorSimple Manager Service-kommunikation till enheten.</span><span class="sxs-lookup"><span data-stu-id="da7dd-181">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Manager Service-to-device communication.</span></span>

