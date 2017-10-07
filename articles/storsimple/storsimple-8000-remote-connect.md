---
title: "aaaConnect via fjärranslutning tooyour StorSimple-enhet | Microsoft Docs"
description: "Förklarar hur tooconfigure enheten för fjärrhantering och hur tooconnect tooWindows PowerShell för StorSimple via HTTP eller HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38b6a6350891b9f6f8fdfc55880b2f47105d947c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a><span data-ttu-id="bb9b1-103">Fjärransluta tooyour StorSimple 8000-serieenhet</span><span class="sxs-lookup"><span data-stu-id="bb9b1-103">Connect remotely tooyour StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="bb9b1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bb9b1-104">Overview</span></span>

<span data-ttu-id="bb9b1-105">Du kan fjärransluta tooyour enheten via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-105">You can remotely connect tooyour device via Windows PowerShell.</span></span> <span data-ttu-id="bb9b1-106">När du ansluter det här sättet kan ser du inte en meny.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="bb9b1-107">(Du se en meny endast om du använder hello seriekonsolen på hello enheten tooconnect.) Med Windows PowerShell-fjärrkommunikation ansluta tooa specifika runspace.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-107">(You see a menu only if you use hello serial console on hello device tooconnect.) With Windows PowerShell remoting, you connect tooa specific runspace.</span></span> <span data-ttu-id="bb9b1-108">Du kan också ange hello visningsspråket.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-108">You can also specify hello display language.</span></span>

<span data-ttu-id="bb9b1-109">Mer information om hur du använder Windows PowerShell-fjärrkommunikation toomanage enheten finns för[använda Windows PowerShell för StorSimple tooadminister StorSimple-enheten](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-109">For more information about using Windows PowerShell remoting toomanage your device, go too[Use Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="bb9b1-110">Den här självstudiekursen beskrivs hur tooconfigure enheten för fjärrhantering och sedan hur tooconnect tooWindows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-110">This tutorial explains how tooconfigure your device for remote management and then how tooconnect tooWindows PowerShell for StorSimple.</span></span> <span data-ttu-id="bb9b1-111">Du kan använda HTTP eller HTTPS tooremotely ansluta via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-111">You can use HTTP or HTTPS tooremotely connect via Windows PowerShell.</span></span> <span data-ttu-id="bb9b1-112">Men när du beslutar hur tooconnect tooWindows PowerShell för StorSimple, Överväg hello följande information:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-112">However, when you are deciding how tooconnect tooWindows PowerShell for StorSimple, consider hello following information:</span></span>

* <span data-ttu-id="bb9b1-113">Ansluta direkt toohello enhetens seriekonsol är säkra, men anslutande toohello seriekonsolen över nätverksväxlar är inte.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-113">Connecting directly toohello device serial console is secure, but connecting toohello serial console over network switches is not.</span></span> <span data-ttu-id="bb9b1-114">Var försiktig av hello säkerhetsrisk när du ansluter toohello enhetens seriekonsol via nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-114">Be cautious of hello security risk when connecting toohello device serial console over network switches.</span></span>
* <span data-ttu-id="bb9b1-115">Ansluter via en HTTP-session kan erbjuda mer säkerhet än att ansluta via seriekonsol hello hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-115">Connecting through an HTTP session might offer more security than connecting through hello serial console over hello network.</span></span> <span data-ttu-id="bb9b1-116">Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-116">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="bb9b1-117">Ansluter via en HTTPS-session med ett självsignerat certifikat är hello säkraste och hello rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-117">Connecting through an HTTPS session with a self-signed certificate is hello most secure and hello recommended option.</span></span>

<span data-ttu-id="bb9b1-118">Du kan fjärransluta toohello Windows PowerShell-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-118">You can connect remotely toohello Windows PowerShell interface.</span></span> <span data-ttu-id="bb9b1-119">Fjärråtkomst tooyour StorSimple-enhet via hello Windows PowerShell-gränssnittet är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-119">However, remote access tooyour StorSimple device via hello Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="bb9b1-120">Du måste aktivera fjärrhantering på hello enheten först och sedan på hello-klient som är används tooaccess din enhet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-120">You must enable remote management on hello device first, and then on hello client that is used tooaccess your device.</span></span>

<span data-ttu-id="bb9b1-121">hello stegen som beskrivs i den här artikeln utfördes på ett värdsystem som kör Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-121">hello steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="bb9b1-122">Ansluta via HTTP</span><span class="sxs-lookup"><span data-stu-id="bb9b1-122">Connect through HTTP</span></span>

<span data-ttu-id="bb9b1-123">Ansluter tooWindows PowerShell för StorSimple via en HTTP-session är säkrare än att ansluta via hello seriekonsol av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-123">Connecting tooWindows PowerShell for StorSimple through an HTTP session offers more security than connecting through hello serial console of your StorSimple device.</span></span> <span data-ttu-id="bb9b1-124">Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-124">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="bb9b1-125">Du kan använda antingen hello Azure-portalen eller hello seriekonsolen tooconfigure fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-125">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="bb9b1-126">Välj hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-126">Select from hello following procedures:</span></span>

* [<span data-ttu-id="bb9b1-127">Använda hello Azure portal tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="bb9b1-127">Use hello Azure portal tooenable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="bb9b1-128">Använda hello seriekonsolen tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="bb9b1-128">Use hello serial console tooenable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="bb9b1-129">När du aktiverar fjärrhantering, Använd hello följa proceduren tooprepare hello-klient för en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-129">After you enable remote management, use hello following procedure tooprepare hello client for a remote connection.</span></span>

* [<span data-ttu-id="bb9b1-130">Förbered hello-klient för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="bb9b1-130">Prepare hello client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-http"></a><span data-ttu-id="bb9b1-131">Använda hello Azure portal tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="bb9b1-131">Use hello Azure portal tooenable remote management over HTTP</span></span>

<span data-ttu-id="bb9b1-132">Utföra hello följa stegen i hello Azure portal tooenable fjärrhantering över HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-132">Perform hello following steps in hello Azure portal tooenable remote management over HTTP.</span></span>

#### <a name="tooenable-remote-management-through-hello-azure-portal"></a><span data-ttu-id="bb9b1-133">tooenable fjärrhantering via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bb9b1-133">tooenable remote management through hello Azure portal</span></span>

1. <span data-ttu-id="bb9b1-134">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-134">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="bb9b1-135">Välj **enheter** och markera och på hello enheten tooconfigure för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-135">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="bb9b1-136">Gå för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-136">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="bb9b1-137">I hello **säkerhetsinställningar** bladet, klickar du på **fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-137">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="bb9b1-138">I hello **fjärrhantering** bladet ange **aktivera fjärrhantering** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-138">In hello **Remote management** blade, set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="bb9b1-139">Nu kan du välja tooconnect med HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-139">You can now choose tooconnect using HTTP.</span></span> <span data-ttu-id="bb9b1-140">(hello är standard tooconnect via HTTPS.) Se till att HTTP har valts.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-140">(hello default is tooconnect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bb9b1-141">Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="bb9b1-142">Klicka på **spara** och när du uppmanas att bekräfta, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a><span data-ttu-id="bb9b1-143">Använda hello seriekonsolen tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="bb9b1-143">Use hello serial console tooenable remote management over HTTP</span></span>
<span data-ttu-id="bb9b1-144">Utför följande hello seriekonsolen tooenable remote enhetshantering hello.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-144">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="bb9b1-145">tooenable fjärrhantering via hello enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="bb9b1-145">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="bb9b1-146">På menyn för seriekonsolen av hello väljer du alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-146">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="bb9b1-147">Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-147">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="bb9b1-148">I hello kommandotolk, skriver du:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="bb9b1-148">At hello prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="bb9b1-149">Du meddelas om hello säkerhetsrisker med att använda HTTP tooconnect toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-149">You are notified about hello security vulnerabilities of using HTTP tooconnect toohello device.</span></span> <span data-ttu-id="bb9b1-150">När du uppmanas bekräfta genom att skriva **Y**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="bb9b1-151">Kontrollera att HTTP har aktiverats genom att skriva:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="bb9b1-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="bb9b1-152">Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsAndHttpEnabled**.hello följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-152">Verify that hello **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS och HTTP-aktiverad](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a><span data-ttu-id="bb9b1-154">Förbered hello-klient för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="bb9b1-154">Prepare hello client for remote connection</span></span>
<span data-ttu-id="bb9b1-155">Utför följande steg på klientens hello tooenable fjärrhantering hello.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-155">Perform hello following steps on hello client tooenable remote management.</span></span>

#### <a name="tooprepare-hello-client-for-remote-connection"></a><span data-ttu-id="bb9b1-156">tooprepare hello klienten för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="bb9b1-156">tooprepare hello client for remote connection</span></span>
1. <span data-ttu-id="bb9b1-157">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="bb9b1-158">Skriv följande kommando tooadd hello IP-adress i listan över betrodda värdar hello StorSimple enheten toohello klientens hello:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-158">Type hello following command tooadd hello IP address of hello StorSimple device toohello client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="bb9b1-159">Ersätt <*device_ip*> med hello IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-159">Replace <*device_ip*> with hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="bb9b1-160">Skriv följande kommando toosave hello enheten autentiseringsuppgifter i en variabel hello:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-160">Type hello following command toosave hello device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="bb9b1-161">Hello i dialogrutan som visas:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-161">In hello dialog box that appears:</span></span>
   
   1. <span data-ttu-id="bb9b1-162">Skriv hello användarnamn i formatet: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-162">Type hello user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="bb9b1-163">Ange hello enhetens administratörslösenord som angavs när hello enhet har konfigurerats med hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-163">Type hello device administrator password that was set when hello device was configured with hello setup wizard.</span></span> <span data-ttu-id="bb9b1-164">hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-164">hello default password is *Password1*.</span></span>
5. <span data-ttu-id="bb9b1-165">Starta Windows PowerShell-sessionen hello enheten genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-165">Start a Windows PowerShell session on hello device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="bb9b1-166">toocreate en Windows PowerShell-session för användning med hello virtuell StorSimple-enhet, Lägg till hello `–Port` parametern och ange hello offentlig port som du konfigurerade i fjärrkommunikation för virtuell StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-166">toocreate a Windows PowerShell session for use with hello StorSimple virtual device, append hello `–Port` parameter and specify hello public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="bb9b1-167">Du bör nu ha en aktiv Windows PowerShell-session toohello fjärrenheten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-167">At this point, you should have an active remote Windows PowerShell session toohello device.</span></span>
   
![PowerShell-fjärrkommunikation med HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="bb9b1-169">Ansluta via HTTPS</span><span class="sxs-lookup"><span data-stu-id="bb9b1-169">Connect through HTTPS</span></span>

<span data-ttu-id="bb9b1-170">Ansluter tooWindows PowerShell för StorSimple via en HTTPS-session är hello säkraste och rekommenderade metoden för enhet som ansluter via fjärranslutning tooyour Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-170">Connecting tooWindows PowerShell for StorSimple through an HTTPS session is hello most secure and recommended method of remotely connecting tooyour Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="bb9b1-171">hello följande procedurer beskrivs hur tooset in hello serial-konsolen och klienten datorer så att du kan använda HTTPS tooconnect tooWindows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-171">hello following procedures explain how tooset up hello serial console and client computers so that you can use HTTPS tooconnect tooWindows PowerShell for StorSimple.</span></span>

<span data-ttu-id="bb9b1-172">Du kan använda antingen hello Azure-portalen eller hello seriekonsolen tooconfigure fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-172">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="bb9b1-173">Välj hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-173">Select from hello following procedures:</span></span>

* [<span data-ttu-id="bb9b1-174">Använda hello Azure portal tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="bb9b1-174">Use hello Azure portal tooenable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="bb9b1-175">Använda hello seriekonsolen tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="bb9b1-175">Use hello serial console tooenable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="bb9b1-176">När du aktiverar fjärrhantering, Använd följande procedurer tooprepare hello värden för en fjärrhantering hello och ansluter toohello enheten från hello fjärrvärden.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-176">After you enable remote management, use hello following procedures tooprepare hello host for a remote management and connect toohello device from hello remote host.</span></span>

* [<span data-ttu-id="bb9b1-177">Förbereda hello värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="bb9b1-177">Prepare hello host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="bb9b1-178">Anslut toohello enhet från hello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="bb9b1-178">Connect toohello device from hello remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-https"></a><span data-ttu-id="bb9b1-179">Använda hello Azure portal tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="bb9b1-179">Use hello Azure portal tooenable remote management over HTTPS</span></span>

<span data-ttu-id="bb9b1-180">Utföra hello följa stegen i hello Azure portal tooenable fjärrhantering över HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-180">Perform hello following steps in hello Azure portal tooenable remote management over HTTPS.</span></span>

#### <a name="tooenable-remote-management-over-https-from-hello-azure-portal"></a><span data-ttu-id="bb9b1-181">tooenable remote management via HTTPS från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bb9b1-181">tooenable remote management over HTTPS from hello Azure portal</span></span>

1. <span data-ttu-id="bb9b1-182">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-182">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="bb9b1-183">Välj **enheter** och markera och på hello enheten tooconfigure för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-183">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="bb9b1-184">Gå för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-184">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="bb9b1-185">I hello **säkerhetsinställningar** bladet, klickar du på **fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-185">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="bb9b1-186">Ange **aktivera fjärrhantering** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-186">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="bb9b1-187">Nu kan du välja tooconnect med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-187">You can now choose tooconnect using HTTPS.</span></span> <span data-ttu-id="bb9b1-188">(hello är standard tooconnect via HTTPS.) Kontrollera att HTTPS är markerad.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-188">(hello default is tooconnect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="bb9b1-189">... Och klicka sedan på **hämta certifikat för fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="bb9b1-190">Ange en plats toosave den här filen.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-190">Specify a location toosave this file.</span></span> <span data-ttu-id="bb9b1-191">Du måste tooinstall certifikatet på hello klienten eller värddatorn dator som du ska använda tooconnect toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-191">You need tooinstall this certificate on hello client or host computer that you will use tooconnect toohello device.</span></span>
6. <span data-ttu-id="bb9b1-192">Klicka på **spara** och klicka sedan på **Ja** när du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a><span data-ttu-id="bb9b1-193">Använda hello seriekonsolen tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="bb9b1-193">Use hello serial console tooenable remote management over HTTPS</span></span>

<span data-ttu-id="bb9b1-194">Utför följande hello seriekonsolen tooenable remote enhetshantering hello.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-194">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="bb9b1-195">tooenable fjärrhantering via hello enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="bb9b1-195">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="bb9b1-196">På menyn för seriekonsolen av hello väljer du alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-196">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="bb9b1-197">Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-197">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="bb9b1-198">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-198">At hello prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="bb9b1-199">Detta bör aktivera HTTPS på enheten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="bb9b1-200">Kontrollera att HTTPS har aktiverats genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="bb9b1-201">Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsEnabled**.hello följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-201">Make sure that hello **RemoteManagementMode** field shows **HttpsEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS-aktiverade](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="bb9b1-203">Från hello utdata från `Get-HcsSystem`, kopiera hello serienumret för hello enheten och spara den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-203">From hello output of `Get-HcsSystem`, copy hello serial number of hello device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bb9b1-204">hello serienummer mappar toohello CN-namn i hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-204">hello serial number maps toohello CN name in hello certificate.</span></span>
   
5. <span data-ttu-id="bb9b1-205">Hämta ett certifikat för fjärrhantering genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="bb9b1-206">Ett certifikat liknande toohello följande visas.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-206">A certificate similar toohello following will appear.</span></span>
   
    ![Hämta certifikat för fjärrhantering](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="bb9b1-208">Kopiera hello i hello certifikat från **---BEGIN CERTIFICATE---** för**---END CERTIFICATE---** i en textredigerare, till exempel Anteckningar och spara den som en .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-208">Copy hello information in hello certificate from **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="bb9b1-209">(Du kommer att kopiera den här filen tooyour fjärrvärden när du förbereder hello värden.)</span><span class="sxs-lookup"><span data-stu-id="bb9b1-209">(You will copy this file tooyour remote host when you prepare hello host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bb9b1-210">toogenerate ett nytt certifikat använder hello `Set-HcsRemoteManagementCert` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-210">toogenerate a new certificate, use hello `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-hello-host-for-remote-management"></a><span data-ttu-id="bb9b1-211">Förbereda hello värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="bb9b1-211">Prepare hello host for remote management</span></span>

<span data-ttu-id="bb9b1-212">tooprepare hello värddatorn för en anslutning som använder en HTTPS-session, utföra hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-212">tooprepare hello host computer for a remote connection that uses an HTTPS session, perform hello following procedures:</span></span>

* <span data-ttu-id="bb9b1-213">[Importera hello .cer-fil i hello rotarkivet hello klienten eller fjärrvärden](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-213">[Import hello .cer file into hello root store of hello client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="bb9b1-214">[Lägg till hello enhetens serienummer toohello hosts-filen på din fjärrvärden](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-214">[Add hello device serial numbers toohello hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="bb9b1-215">Var och en av hello föregående procedurerna beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-215">Each of hello preceding procedures, is described below.</span></span>

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a><span data-ttu-id="bb9b1-216">tooimport hello certifikatet på fjärrvärden hello</span><span class="sxs-lookup"><span data-stu-id="bb9b1-216">tooimport hello certificate on hello remote host</span></span>
1. <span data-ttu-id="bb9b1-217">Högerklicka på hello .cer-fil och välj **installera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-217">Right-click hello .cer file and select **Install certificate**.</span></span> <span data-ttu-id="bb9b1-218">Detta startar hello guiden Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-218">This starts hello Certificate Import Wizard.</span></span>
   
    ![Guiden Importera 1 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="bb9b1-220">För **plats**väljer **lokal dator**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="bb9b1-221">Välj **placera alla certifikat i följande store hello**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-221">Select **Place all certificates in hello following store**, and then click **Browse**.</span></span> <span data-ttu-id="bb9b1-222">Navigera toohello rotarkivet på din fjärrvärden och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-222">Navigate toohello root store of your remote host, and then click **Next**.</span></span>
   
    ![Guiden Importera 2 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="bb9b1-224">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-224">Click **Finish**.</span></span> <span data-ttu-id="bb9b1-225">Visas ett meddelande som talar om att hello importen lyckades.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-225">A message that tells you that hello import was successful appears.</span></span>
   
    ![Guiden Importera 3 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a><span data-ttu-id="bb9b1-227">tooadd enhetens serienummer toohello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="bb9b1-227">tooadd device serial numbers toohello remote host</span></span>
1. <span data-ttu-id="bb9b1-228">Starta Anteckningar som administratör och öppna hello värdfilen som finns på \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-228">Start Notepad as an administrator, and then open hello hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="bb9b1-229">Lägg till följande tre poster tooyour värdfilen hello: **DATA 0 IP-adress**, **fasta IP-adressen för styrenhet 0**, och **domänkontrollant 1 fasta IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-229">Add hello following three entries tooyour hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="bb9b1-230">Ange hello enhetens serienummer som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-230">Enter hello device serial number that you saved earlier.</span></span> <span data-ttu-id="bb9b1-231">Den här toohello IP-adressen som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-231">Map this toohello IP address as shown in hello following image.</span></span> <span data-ttu-id="bb9b1-232">Lägg till för styrenhet 0 och 1 **Controller0** och **Controller1** hello slutet av hello serienummer (CN-namn).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at hello end of hello serial number (CN name).</span></span>
   
    ![Lägger till CN-namnet toohosts fil](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="bb9b1-234">Spara hello hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-234">Save hello hosts file.</span></span>

### <a name="connect-toohello-device-from-hello-remote-host"></a><span data-ttu-id="bb9b1-235">Anslut toohello enhet från hello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="bb9b1-235">Connect toohello device from hello remote host</span></span>

<span data-ttu-id="bb9b1-236">Använda Windows PowerShell och SSL tooenter en SSAdmin sessionen på din enhet från en fjärrvärd eller klienten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-236">Use Windows PowerShell and SSL tooenter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="bb9b1-237">Hej SSAdmin session mappar toooption 1 i hello [seriekonsolen](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) -menyn i enheten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-237">hello SSAdmin session maps toooption 1 in hello [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="bb9b1-238">Utföra hello följa proceduren på hello som du vill toomake hello Windows PowerShell fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-238">Perform hello following procedure on hello computer from which you want toomake hello remote Windows PowerShell connection.</span></span>

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="bb9b1-239">tooenter en SSAdmin sessionen på hello-enhet med hjälp av Windows PowerShell och SSL</span><span class="sxs-lookup"><span data-stu-id="bb9b1-239">tooenter an SSAdmin session on hello device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="bb9b1-240">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="bb9b1-241">Lägg till hello enhetens IP-adress toohello klientens betrodda värdar genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-241">Add hello device IP address toohello client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="bb9b1-242">Där <*device_ip*> är hello IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-242">Where <*device_ip*> is hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="bb9b1-243">toocreate nya autentiseringsuppgifter, skriver du:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-243">toocreate a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="bb9b1-244">Där <*IP målenhet*> är hello IP-adressen för DATA 0 för din enhet, till exempel **10.126.173.90** enligt hello föregående bild av hello hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-244">Where <*IP of target device*> is hello IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in hello preceding image of hello hosts file.</span></span> <span data-ttu-id="bb9b1-245">Ange dessutom hello administratörslösenordet för enheten.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-245">Also, supply hello administrator password for your device.</span></span>
4. <span data-ttu-id="bb9b1-246">Skapa en session genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="bb9b1-247">Ange hello hello - ComputerName parameter i cmdleten hello <*serienumret för målenhet*>.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-247">For hello -ComputerName parameter in hello cmdlet, provide hello <*serial number of target device*>.</span></span> <span data-ttu-id="bb9b1-248">Det här serienumret mappades toohello IP-adress för DATA 0 i hello värdfilen på fjärrvärden; till exempel **SHX0991003G44MT** som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-248">This serial number was mapped toohello IP address of DATA 0 in hello hosts file on your remote host; for example, **SHX0991003G44MT** as shown in hello following image.</span></span>
5. <span data-ttu-id="bb9b1-249">Ange:</span><span class="sxs-lookup"><span data-stu-id="bb9b1-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="bb9b1-250">Du behöver toowait några minuter och du kommer sedan att anslutna tooyour enheten via HTTPS via SSL.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-250">You will need toowait a few minutes, and then you will be connected tooyour device via HTTPS over SSL.</span></span> <span data-ttu-id="bb9b1-251">Du ser ett meddelande som anger att du är ansluten tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="bb9b1-251">You see a message that indicates you are connected tooyour device.</span></span>
   
    ![PowerShell-fjärrkommunikation med hjälp av HTTPS och SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="bb9b1-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb9b1-253">Next steps</span></span>

* <span data-ttu-id="bb9b1-254">Lär dig mer om [med hjälp av Windows PowerShell tooadminister StorSimple-enheten](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-254">Learn more about [using Windows PowerShell tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="bb9b1-255">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bb9b1-255">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

