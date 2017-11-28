<!--author=alkohli last changed: 01/18/2017-->


#### <a name="to-configure-and-register-the-device"></a><span data-ttu-id="21d65-101">Konfigurera och registrera enheten</span><span class="sxs-lookup"><span data-stu-id="21d65-101">To configure and register the device</span></span>

1. <span data-ttu-id="21d65-102">Gå in i Windows PowerShell-gränssnittet på din StorSimple-enhets seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="21d65-102">Access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="21d65-103">Mer instruktioner finns i [Använd PuTTY för att ansluta till enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="21d65-103">See [Use PuTTY to connect to the device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="21d65-104">**Se till att följa proceduren exakt för att du ska få åtkomst till konsolen.**</span><span class="sxs-lookup"><span data-stu-id="21d65-104">**Be sure to follow the procedure exactly or you will not be able to access the console.**</span></span>

2. <span data-ttu-id="21d65-105">Öppna en kommandotolk genom att trycka på **Retur** i sessionen som öppnas.</span><span class="sxs-lookup"><span data-stu-id="21d65-105">In the session that opens up, press **Enter** one time to get a command prompt.</span></span>

3. <span data-ttu-id="21d65-106">Du kommer att uppmanas att välja det språk som du vill ställa in för din enhet.</span><span class="sxs-lookup"><span data-stu-id="21d65-106">You will be prompted to choose the language that you would like to set for your device.</span></span> <span data-ttu-id="21d65-107">Ange språket och tryck på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="21d65-107">Specify the language, and then press **Enter**.</span></span>

4. <span data-ttu-id="21d65-108">I seriemenyn för konsolen som visas väljer du alternativ 1 för att **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="21d65-108">In the serial console menu that is presented, choose option 1 to **log in with full access**.</span></span>
     <span data-ttu-id="21d65-109">Slutför steg 5–12 för att konfigurera de minsta nödvändiga nätverksinställningarna för din enhet.</span><span class="sxs-lookup"><span data-stu-id="21d65-109">Complete steps 5-12 to configure the minimum required network settings for your device.</span></span> <span data-ttu-id="21d65-110">**De här konfigurationsstegen behöver genomföras på den aktiva styrenheten för enheten.**</span><span class="sxs-lookup"><span data-stu-id="21d65-110">**These configuration steps need to be performed on the active controller of the device.**</span></span> <span data-ttu-id="21d65-111">Menyn för seriekonsolen indikerar status för styrenheten i banderollmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="21d65-111">The serial console menu indicates the controller state in the banner message.</span></span> <span data-ttu-id="21d65-112">Om du inte är ansluten till den aktiva styrenheten, kopplar du från och ansluter till den aktiva styrenheten.</span><span class="sxs-lookup"><span data-stu-id="21d65-112">If you are not connected to the active controller, disconnect and then connect to the active controller.</span></span>

5. <span data-ttu-id="21d65-113">Ange ditt lösenord i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="21d65-113">At the command prompt, type your password.</span></span> <span data-ttu-id="21d65-114">Enheten standardlösenord är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="21d65-114">The default device password is **Password1**.</span></span>

6. <span data-ttu-id="21d65-115">Ange följande kommando: `Invoke-HcsSetupWizard`.</span><span class="sxs-lookup"><span data-stu-id="21d65-115">Type the following command: `Invoke-HcsSetupWizard`.</span></span>

7. <span data-ttu-id="21d65-116">En installationsguide kommer visas och hjälpa dig att konfigurera nätverksinställningarna för enheten.</span><span class="sxs-lookup"><span data-stu-id="21d65-116">A setup wizard will appear to help you configure the network settings for the device.</span></span> <span data-ttu-id="21d65-117">Ange följande information:</span><span class="sxs-lookup"><span data-stu-id="21d65-117">Supply the the following information:</span></span>
   
   * <span data-ttu-id="21d65-118">IP-adress för DATA 0-nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="21d65-118">IP address for the DATA 0 network interface</span></span>
   * <span data-ttu-id="21d65-119">Nätmask</span><span class="sxs-lookup"><span data-stu-id="21d65-119">Subnet mask</span></span>
   * <span data-ttu-id="21d65-120">Gateway</span><span class="sxs-lookup"><span data-stu-id="21d65-120">Gateway</span></span>
   * <span data-ttu-id="21d65-121">IP-adress för primär DNS-server</span><span class="sxs-lookup"><span data-stu-id="21d65-121">IP address for Primary DNS server</span></span>

   <span data-ttu-id="21d65-122">Ett exempel på de utdata som returneras visas nedan.</span><span class="sxs-lookup"><span data-stu-id="21d65-122">A sample output is presented below.</span></span>

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Active
        ---------------------------------------------------------------

        Your device needs to be registered with the Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' to set up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like to configure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    <span data-ttu-id="21d65-123">I föregående exempelutdata ser du att nätverksinställningarna verifieras efter varje steg i processen.</span><span class="sxs-lookup"><span data-stu-id="21d65-123">In the preceding sample output, you can see that the system is validating network settings after each step in the process.</span></span>

     > [!NOTE]
     > <span data-ttu-id="21d65-124">Du kan behöva vänta några minuter för att nätmask- och DNS-inställningarna ska appliceras.</span><span class="sxs-lookup"><span data-stu-id="21d65-124">You may have to wait for a few minutes for the subnet mask and the DNS settings to be applied.</span></span> <span data-ttu-id="21d65-125">Om du får ett felmeddelande om att "kontrollera nätverksanslutningen till Data 0", kontrollera den fysiska nätverksanslutningen på DATA 0-nätverksgränssnittet för din aktiva styrenhet.</span><span class="sxs-lookup"><span data-stu-id="21d65-125">If you get a "Check the network connectivity to Data 0" error message, check the physical network connection on the DATA 0 network interface of your active controller.</span></span>

8. <span data-ttu-id="21d65-126">(Valfritt) konfigurera din webbproxyserver.</span><span class="sxs-lookup"><span data-stu-id="21d65-126">(Optional) configure your web proxy server.</span></span> <span data-ttu-id="21d65-127">Även om webbproxykonfigurationen är valfri, **var medveten om att om du använder en webbproxy så kan du bara konfigurera den här**.</span><span class="sxs-lookup"><span data-stu-id="21d65-127">Although web proxy configuration is optional, **be aware that if you use a web proxy, you can only configure it here**.</span></span> <span data-ttu-id="21d65-128">Mer information finns i [Konfigurera en webbproxy för din enhet](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="21d65-128">For more information, go to [Configure web proxy for your device](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span></span>
9. <span data-ttu-id="21d65-129">Konfigurera en primär NTP-server för din enhet.</span><span class="sxs-lookup"><span data-stu-id="21d65-129">Configure a Primary NTP server for your device.</span></span> <span data-ttu-id="21d65-130">NTP-servrar krävs, eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="21d65-130">NTP servers are required, as your device must synchronize time so that it can authenticate with your cloud service providers.</span></span> <span data-ttu-id="21d65-131">Kontrollera att ditt nätverk tillåter att NTP-trafik skickas från ditt datacenter till Internet.</span><span class="sxs-lookup"><span data-stu-id="21d65-131">Ensure that your network allows NTP traffic to pass from your datacenter to the Internet.</span></span> <span data-ttu-id="21d65-132">Om det inte är möjligt anger du en intern NTP-server.</span><span class="sxs-lookup"><span data-stu-id="21d65-132">If this is not possible, specify an internal NTP server.</span></span>

    <span data-ttu-id="21d65-133">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="21d65-133">A sample output is shown below.</span></span>

    ```
        Would you like to configure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. <span data-ttu-id="21d65-134">Av säkerhetsskäl upphör enhetens administratörslösenord att gälla efter den första sessionen. Du måste ändra lösenordet nu.</span><span class="sxs-lookup"><span data-stu-id="21d65-134">For security reasons, the device administrator password expires after the first session, and you will need to change it now.</span></span> <span data-ttu-id="21d65-135">Ange ett administratörslösenord för enheten när du ombes göra det.</span><span class="sxs-lookup"><span data-stu-id="21d65-135">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="21d65-136">Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="21d65-136">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="21d65-137">Lösenordet måste innehålla tre av följande: gemener, versaler, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="21d65-137">The password must contain three of the following: lowercase, uppercase, numeric, and special characters.</span></span>

    ```
        The device administrator password must be between 8 and 15 characters. The password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. <span data-ttu-id="21d65-138">I det sista steget i installationsguiden registreras din enhet med StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="21d65-138">The final step in the setup wizard registers your device with the StorSimple Device Manager service.</span></span> <span data-ttu-id="21d65-139">För det här behöver du tjänstens registreringsnyckel som du fick i steg 2.</span><span class="sxs-lookup"><span data-stu-id="21d65-139">For this, you will need the service registration key that you obtained in step 2.</span></span> <span data-ttu-id="21d65-140">Efter att du angett registreringsnyckeln kan du behöva vänta 2-3 minuter innan enheten är registrerad.</span><span class="sxs-lookup"><span data-stu-id="21d65-140">After you supply the registration key, you may need to wait for 2-3 minutes before the device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="21d65-141">Du kan trycka på Ctrl + C när som helst för att avsluta installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="21d65-141">You can press Ctrl + C at any time to exit the setup wizard.</span></span> <span data-ttu-id="21d65-142">Om du har angett alla nätverksinställningar (IP-adress för Data 0, nätmask och Gateway), kommer informationen att sparas.</span><span class="sxs-lookup"><span data-stu-id="21d65-142">If you have entered all the network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    
    <span data-ttu-id="21d65-143">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="21d65-143">A sample output is shown below.</span></span>

    ```
        The service registration key is available in the StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. <span data-ttu-id="21d65-144">När enheten är registrerad visas en krypteringsnyckel för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="21d65-144">After the device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="21d65-145">Kopiera den här nyckeln och spara den på säker plats.</span><span class="sxs-lookup"><span data-stu-id="21d65-145">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="21d65-146">**Den här nyckeln krävs tillsammans med tjänstregistreringsnyckeln för att registrera ytterligare enheter med StorSimple Device Manager-tjänsten.**</span><span class="sxs-lookup"><span data-stu-id="21d65-146">**This key will be required with the service registration key to register additional devices with the StorSimple Device Manager service.**</span></span> <span data-ttu-id="21d65-147">Referera till [StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) för ytterligare information om den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="21d65-147">Refer to [StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple registrera enhet 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > <span data-ttu-id="21d65-149">Om du vill kopiera text från seriekonsolfönstret, markerar du bara texten.</span><span class="sxs-lookup"><span data-stu-id="21d65-149">To copy the text from the serial console window, simply select the text.</span></span> <span data-ttu-id="21d65-150">Sedan ska du kunna klistra in den i urklipp eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="21d65-150">You should then be able to paste it in the clipboard or any text editor.</span></span> <span data-ttu-id="21d65-151">ANVÄND INTE Ctrl + C för att kopiera krypteringsnyckeln för tjänstdata.</span><span class="sxs-lookup"><span data-stu-id="21d65-151">DO NOT use Ctrl + C to copy the service data encryption key.</span></span> <span data-ttu-id="21d65-152">Ctrl + C avslutar installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="21d65-152">Using Ctrl + C will cause you to exit the setup wizard.</span></span> <span data-ttu-id="21d65-153">Som ett resultat, kommer enhetens administratörslösenord inte att ändras och enheten kommer återgå till standardlösenordet.</span><span class="sxs-lookup"><span data-stu-id="21d65-153">As a result, the device administrator password will not be changed and the device will revert to the default password.</span></span>
    
13. <span data-ttu-id="21d65-154">Avsluta seriekonsolen.</span><span class="sxs-lookup"><span data-stu-id="21d65-154">Exit the serial console.</span></span>
14. <span data-ttu-id="21d65-155">Gå tillbaka till Azure Portal och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="21d65-155">Return to the Azure portal, and complete the following steps:</span></span>
    
    1. <span data-ttu-id="21d65-156">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="21d65-156">Go to your StorSimple Device Manager service.</span></span>
    2. <span data-ttu-id="21d65-157">Klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="21d65-157">Click **Devices**.</span></span>
    3. <span data-ttu-id="21d65-158">I listan med enheter kontrollerar du att enheten har anslutit till tjänsten genom att leta upp dess status.</span><span class="sxs-lookup"><span data-stu-id="21d65-158">In the tabular listing of devices, verify that the device has successfully connected to the service by looking up the status.</span></span> <span data-ttu-id="21d65-159">Enhetens status bör vara **Redo för installation**.</span><span class="sxs-lookup"><span data-stu-id="21d65-159">The device status should be **Ready to set up**.</span></span>
       
        ![StorSimple-enhetssidan](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        <span data-ttu-id="21d65-161">Du kan behöva vänta några minuter innan enhetens status ändras till **Redo för installation**.</span><span class="sxs-lookup"><span data-stu-id="21d65-161">You may need to wait for a couple of minutes for the device status to change to **Ready to set up**.</span></span>
       
        <span data-ttu-id="21d65-162">Om enheten inte visas i listan kontrollerar du att ditt brandväggsnätverk är konfigurerat enligt [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="21d65-162">If the device does not show up in this list, then you need to make sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-8000-system-requirements.md).</span></span> <span data-ttu-id="21d65-163">Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används av Service Bus för kommunikation mellan StorSimple Device Manager-tjänsten och enheten.</span><span class="sxs-lookup"><span data-stu-id="21d65-163">Verify that port 9354 is open for outbound communication as this is used by the service bus for StorSimple Device Manager service-to-device communication.</span></span>

