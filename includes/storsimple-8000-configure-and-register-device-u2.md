<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="047d0-101">tooconfigure och registrera hello-enhet</span><span class="sxs-lookup"><span data-stu-id="047d0-101">tooconfigure and register hello device</span></span>

1. <span data-ttu-id="047d0-102">Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="047d0-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="047d0-103">Se [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="047d0-103">See [Use PuTTY tooconnect toohello device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="047d0-104">**Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**</span><span class="sxs-lookup"><span data-stu-id="047d0-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>

2. <span data-ttu-id="047d0-105">Hello-sessionen som öppnas, trycker du på **RETUR** en gång tooget en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="047d0-105">In hello session that opens up, press **Enter** one time tooget a command prompt.</span></span>

3. <span data-ttu-id="047d0-106">Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet.</span><span class="sxs-lookup"><span data-stu-id="047d0-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="047d0-107">Ange hello språk och tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="047d0-107">Specify hello language, and then press **Enter**.</span></span>

4. <span data-ttu-id="047d0-108">Hej seriekonsolen menyn som visas väljer du alternativ 1 för**logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="047d0-108">In hello serial console menu that is presented, choose option 1 too**log in with full access**.</span></span>
     <span data-ttu-id="047d0-109">Slutför steg 5 – 12 tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten.</span><span class="sxs-lookup"><span data-stu-id="047d0-109">Complete steps 5-12 tooconfigure hello minimum required network settings for your device.</span></span> <span data-ttu-id="047d0-110">**De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet.**</span><span class="sxs-lookup"><span data-stu-id="047d0-110">**These configuration steps need toobe performed on hello active controller of hello device.**</span></span> <span data-ttu-id="047d0-111">hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello.</span><span class="sxs-lookup"><span data-stu-id="047d0-111">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="047d0-112">Om du inte är ansluten toohello aktiva styrenheten, kopplar du från och Anslut toohello aktiva styrenhet.</span><span class="sxs-lookup"><span data-stu-id="047d0-112">If you are not connected toohello active controller, disconnect and then connect toohello active controller.</span></span>

5. <span data-ttu-id="047d0-113">Skriv in lösenordet i hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="047d0-113">At hello command prompt, type your password.</span></span> <span data-ttu-id="047d0-114">hello enheten standardlösenord är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="047d0-114">hello default device password is **Password1**.</span></span>

6. <span data-ttu-id="047d0-115">Typen hello följande kommando: `Invoke-HcsSetupWizard`.</span><span class="sxs-lookup"><span data-stu-id="047d0-115">Type hello following command: `Invoke-HcsSetupWizard`.</span></span>

7. <span data-ttu-id="047d0-116">En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="047d0-116">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="047d0-117">Ange hello hello följande information:</span><span class="sxs-lookup"><span data-stu-id="047d0-117">Supply hello hello following information:</span></span>
   
   * <span data-ttu-id="047d0-118">IP-adress för hello DATA 0-nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="047d0-118">IP address for hello DATA 0 network interface</span></span>
   * <span data-ttu-id="047d0-119">Nätmask</span><span class="sxs-lookup"><span data-stu-id="047d0-119">Subnet mask</span></span>
   * <span data-ttu-id="047d0-120">Gateway</span><span class="sxs-lookup"><span data-stu-id="047d0-120">Gateway</span></span>
   * <span data-ttu-id="047d0-121">IP-adress för primär DNS-server</span><span class="sxs-lookup"><span data-stu-id="047d0-121">IP address for Primary DNS server</span></span>

   <span data-ttu-id="047d0-122">Ett exempel på de utdata som returneras visas nedan.</span><span class="sxs-lookup"><span data-stu-id="047d0-122">A sample output is presented below.</span></span>

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Active
        ---------------------------------------------------------------

        Your device needs toobe registered with hello Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' tooset up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like tooconfigure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    <span data-ttu-id="047d0-123">I föregående exempel på utdata hello, ser du att hello systemet validerar nätverksinställningarna efter varje steg i processen för hello.</span><span class="sxs-lookup"><span data-stu-id="047d0-123">In hello preceding sample output, you can see that hello system is validating network settings after each step in hello process.</span></span>

     > [!NOTE]
     > <span data-ttu-id="047d0-124">Du kan ha toowait några minuter för hello nätmask och hello DNS-inställningar toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="047d0-124">You may have toowait for a few minutes for hello subnet mask and hello DNS settings toobe applied.</span></span> <span data-ttu-id="047d0-125">Om du får ett meddelande om ”Kontrollera hello network connectivity tooData 0” Kontrollera hello fysiska nätverksanslutningen på hello DATA 0-nätverksgränssnittet för din aktiva styrenhet.</span><span class="sxs-lookup"><span data-stu-id="047d0-125">If you get a "Check hello network connectivity tooData 0" error message, check hello physical network connection on hello DATA 0 network interface of your active controller.</span></span>

8. <span data-ttu-id="047d0-126">(Valfritt) konfigurera din webbproxyserver.</span><span class="sxs-lookup"><span data-stu-id="047d0-126">(Optional) configure your web proxy server.</span></span> <span data-ttu-id="047d0-127">Även om webbproxykonfigurationen är valfri, **var medveten om att om du använder en webbproxy så kan du bara konfigurera den här**.</span><span class="sxs-lookup"><span data-stu-id="047d0-127">Although web proxy configuration is optional, **be aware that if you use a web proxy, you can only configure it here**.</span></span> <span data-ttu-id="047d0-128">Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="047d0-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span></span>
9. <span data-ttu-id="047d0-129">Konfigurera en primär NTP-server för din enhet.</span><span class="sxs-lookup"><span data-stu-id="047d0-129">Configure a Primary NTP server for your device.</span></span> <span data-ttu-id="047d0-130">NTP-servrar krävs, eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="047d0-130">NTP servers are required, as your device must synchronize time so that it can authenticate with your cloud service providers.</span></span> <span data-ttu-id="047d0-131">Se till att ditt nätverk tillåter att NTP-trafik toopass från ditt datacenter toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="047d0-131">Ensure that your network allows NTP traffic toopass from your datacenter toohello Internet.</span></span> <span data-ttu-id="047d0-132">Om det inte är möjligt anger du en intern NTP-server.</span><span class="sxs-lookup"><span data-stu-id="047d0-132">If this is not possible, specify an internal NTP server.</span></span>

    <span data-ttu-id="047d0-133">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="047d0-133">A sample output is shown below.</span></span>

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. <span data-ttu-id="047d0-134">Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange it nu.</span><span class="sxs-lookup"><span data-stu-id="047d0-134">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="047d0-135">Ange ett administratörslösenord för enheten när du ombes göra det.</span><span class="sxs-lookup"><span data-stu-id="047d0-135">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="047d0-136">Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="047d0-136">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="047d0-137">hello lösenordet måste innehålla tre hello följande: gemener, versaler, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="047d0-137">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. <span data-ttu-id="047d0-138">hello sista steget i installationsguiden för hello registrerar enheten med hello StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="047d0-138">hello final step in hello setup wizard registers your device with hello StorSimple Device Manager service.</span></span> <span data-ttu-id="047d0-139">För att göra detta måste hello tjänstens registreringsnyckel som du hämtade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="047d0-139">For this, you will need hello service registration key that you obtained in step 2.</span></span> <span data-ttu-id="047d0-140">När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.</span><span class="sxs-lookup"><span data-stu-id="047d0-140">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="047d0-141">Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="047d0-141">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="047d0-142">Om du har angett alla hello nätverksinställningar (IP-adress för Data 0, nätmask och Gateway) kommer posterna att sparas.</span><span class="sxs-lookup"><span data-stu-id="047d0-142">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    
    <span data-ttu-id="047d0-143">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="047d0-143">A sample output is shown below.</span></span>

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. <span data-ttu-id="047d0-144">När hello enheten är registrerad visas en krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="047d0-144">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="047d0-145">Kopiera den här nyckeln och spara den på säker plats.</span><span class="sxs-lookup"><span data-stu-id="047d0-145">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="047d0-146">**Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple enheten Manager-tjänsten.**</span><span class="sxs-lookup"><span data-stu-id="047d0-146">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Device Manager service.**</span></span> <span data-ttu-id="047d0-147">Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="047d0-147">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple registrera enhet 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > <span data-ttu-id="047d0-149">toocopy hello text från seriekonsolfönstret med hello, bara markera hello text.</span><span class="sxs-lookup"><span data-stu-id="047d0-149">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="047d0-150">Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="047d0-150">You should then be able toopaste it in hello clipboard or any text editor.</span></span> <span data-ttu-id="047d0-151">Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="047d0-151">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="047d0-152">Med Ctrl + C kommer du tooexit hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="047d0-152">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="047d0-153">Därför kommer inte att ändra hello enhetens administratörslösenord och hello enheten kommer återgå toohello standardlösenordet.</span><span class="sxs-lookup"><span data-stu-id="047d0-153">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    
13. <span data-ttu-id="047d0-154">Avsluta hello seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="047d0-154">Exit hello serial console.</span></span>
14. <span data-ttu-id="047d0-155">Returnera toohello Azure-portalen och slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="047d0-155">Return toohello Azure portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="047d0-156">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="047d0-156">Go tooyour StorSimple Device Manager service.</span></span>
    2. <span data-ttu-id="047d0-157">Klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="047d0-157">Click **Devices**.</span></span>
    3. <span data-ttu-id="047d0-158">Kontrollera i hello tabular lista över enheter, hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status.</span><span class="sxs-lookup"><span data-stu-id="047d0-158">In hello tabular listing of devices, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="047d0-159">hello enhetens status ska vara **klar tooset in**.</span><span class="sxs-lookup"><span data-stu-id="047d0-159">hello device status should be **Ready tooset up**.</span></span>
       
        ![StorSimple-enhetssidan](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        <span data-ttu-id="047d0-161">Du kan behöva toowait för ett par minuter för hello enhetens status toochange för**klar tooset in**.</span><span class="sxs-lookup"><span data-stu-id="047d0-161">You may need toowait for a couple of minutes for hello device status toochange too**Ready tooset up**.</span></span>
       
        <span data-ttu-id="047d0-162">Om hello enheten inte visas i listan, måste du till att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i toomake [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="047d0-162">If hello device does not show up in this list, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-8000-system-requirements.md).</span></span> <span data-ttu-id="047d0-163">Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används i hello service bus för kommunikation för StorSimple Device Manager-tjänsten till enheten.</span><span class="sxs-lookup"><span data-stu-id="047d0-163">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Device Manager service-to-device communication.</span></span>

