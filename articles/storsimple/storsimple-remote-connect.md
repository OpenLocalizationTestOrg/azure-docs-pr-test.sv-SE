---
title: "aaaConnect via fjärranslutning tooyour StorSimple-enhet | Microsoft Docs"
description: "Förklarar hur tooconfigure enheten för fjärrhantering och hur tooconnect tooWindows PowerShell för StorSimple via HTTP eller HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 55ed8fcdd997901301e0adc164a302216cde0332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a><span data-ttu-id="3ebab-103">Fjärransluta tooyour StorSimple 8000-serieenhet</span><span class="sxs-lookup"><span data-stu-id="3ebab-103">Connect remotely tooyour StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="3ebab-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3ebab-104">Overview</span></span>
<span data-ttu-id="3ebab-105">Du kan använda Windows PowerShell-fjärrkommunikation tooconnect tooyour StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-105">You can use Windows PowerShell remoting tooconnect tooyour StorSimple device.</span></span> <span data-ttu-id="3ebab-106">När du ansluter det här sättet kan se du inte en meny.</span><span class="sxs-lookup"><span data-stu-id="3ebab-106">When you connect this way, you will not see a menu.</span></span> <span data-ttu-id="3ebab-107">(Du se en meny endast om du använder hello seriekonsolen på hello enheten tooconnect.) Med Windows PowerShell-fjärrkommunikation ansluta tooa specifika runspace.</span><span class="sxs-lookup"><span data-stu-id="3ebab-107">(You see a menu only if you use hello serial console on hello device tooconnect.) With Windows PowerShell remoting, you connect tooa specific runspace.</span></span> <span data-ttu-id="3ebab-108">Du kan också ange hello visningsspråket.</span><span class="sxs-lookup"><span data-stu-id="3ebab-108">You can also specify hello display language.</span></span> 

<span data-ttu-id="3ebab-109">Mer information om hur du använder Windows PowerShell-fjärrkommunikation toomanage enheten finns för[använda Windows PowerShell för StorSimple tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3ebab-109">For more information about using Windows PowerShell remoting toomanage your device, go too[Use Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>

<span data-ttu-id="3ebab-110">Den här självstudiekursen beskrivs hur tooconfigure enheten för fjärrhantering och sedan hur tooconnect tooWindows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3ebab-110">This tutorial explains how tooconfigure your device for remote management and then how tooconnect tooWindows PowerShell for StorSimple.</span></span> <span data-ttu-id="3ebab-111">Du kan använda HTTP eller HTTPS tooconnect via Windows PowerShell-fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="3ebab-111">You can use HTTP or HTTPS tooconnect via Windows PowerShell remoting.</span></span> <span data-ttu-id="3ebab-112">Men när du beslutar hur tooconnect tooWindows PowerShell för StorSimple, Tänk hello följande:</span><span class="sxs-lookup"><span data-stu-id="3ebab-112">However, when you are deciding how tooconnect tooWindows PowerShell for StorSimple, consider hello following:</span></span> 

* <span data-ttu-id="3ebab-113">Ansluta direkt toohello enhetens seriekonsol är säkra, men anslutande toohello seriekonsolen över nätverksväxlar är inte.</span><span class="sxs-lookup"><span data-stu-id="3ebab-113">Connecting directly toohello device serial console is secure, but connecting toohello serial console over network switches is not.</span></span> <span data-ttu-id="3ebab-114">Var försiktig av hello säkerhetsrisk när du ansluter toohello enhetens seriekonsol via nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="3ebab-114">Be cautious of hello security risk when connecting toohello device serial console over network switches.</span></span> 
* <span data-ttu-id="3ebab-115">Ansluter via en HTTP-session kan erbjuda mer säkerhet än att ansluta via seriekonsol hello hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="3ebab-115">Connecting through an HTTP session might offer more security than connecting through hello serial console over hello network.</span></span> <span data-ttu-id="3ebab-116">Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="3ebab-116">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span> 
* <span data-ttu-id="3ebab-117">Ansluter via en HTTPS-session med ett självsignerat certifikat är hello säkraste och hello rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="3ebab-117">Connecting through an HTTPS session with a self-signed certificate is hello most secure and hello recommended option.</span></span>

<span data-ttu-id="3ebab-118">Du kan fjärransluta toohello Windows PowerShell-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-118">You can connect remotely toohello Windows PowerShell interface.</span></span> <span data-ttu-id="3ebab-119">Fjärråtkomst tooyour StorSimple-enhet via hello Windows PowerShell-gränssnittet är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="3ebab-119">However, remote access tooyour StorSimple device via hello Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="3ebab-120">Du behöver tooenable fjärrhantering på hello enheten först och sedan på hello-klient som är används tooaccess din enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-120">You need tooenable remote management on hello device first, and then on hello client that is used tooaccess your device.</span></span>

<span data-ttu-id="3ebab-121">hello stegen som beskrivs i den här artikeln utfördes på ett värdsystem som kör Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="3ebab-121">hello steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="3ebab-122">Ansluta via HTTP</span><span class="sxs-lookup"><span data-stu-id="3ebab-122">Connect through HTTP</span></span>
<span data-ttu-id="3ebab-123">Ansluter tooWindows PowerShell för StorSimple via en HTTP-session är säkrare än att ansluta via hello seriekonsol av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-123">Connecting tooWindows PowerShell for StorSimple through an HTTP session offers more security than connecting through hello serial console of your StorSimple device.</span></span> <span data-ttu-id="3ebab-124">Även om detta inte hello säkraste metoden, är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="3ebab-124">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="3ebab-125">Du kan använda hello klassiska Azure-portalen eller hello seriekonsolen tooconfigure för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="3ebab-125">You can use either hello Azure classic portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="3ebab-126">Välj hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="3ebab-126">Select from hello following procedures:</span></span>

* [<span data-ttu-id="3ebab-127">Använda hello Azure klassiska portal tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="3ebab-127">Use hello Azure classic portal tooenable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="3ebab-128">Använda hello seriekonsolen tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="3ebab-128">Use hello serial console tooenable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="3ebab-129">När du aktiverar fjärrhantering, Använd hello följa proceduren tooprepare hello-klient för en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="3ebab-129">After you enable remote management, use hello following procedure tooprepare hello client for a remote connection.</span></span>

* [<span data-ttu-id="3ebab-130">Förbered hello-klient för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="3ebab-130">Prepare hello client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-http"></a><span data-ttu-id="3ebab-131">Använda hello Azure klassiska portal tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="3ebab-131">Use hello Azure classic portal tooenable remote management over HTTP</span></span>
<span data-ttu-id="3ebab-132">Utföra hello följa stegen i hello Azure klassiska portal tooenable fjärrhantering över HTTP.</span><span class="sxs-lookup"><span data-stu-id="3ebab-132">Perform hello following steps in hello Azure classic portal tooenable remote management over HTTP.</span></span>

#### <a name="tooenable-remote-management-through-hello-azure-classic-portal"></a><span data-ttu-id="3ebab-133">tooenable fjärrhantering via hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3ebab-133">tooenable remote management through hello Azure classic portal</span></span>
1. <span data-ttu-id="3ebab-134">Åtkomst **enheter** > **konfigurera** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-134">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="3ebab-135">Bläddra nedåt toohello **fjärrhantering** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3ebab-135">Scroll down toohello **Remote Management** section.</span></span>
3. <span data-ttu-id="3ebab-136">Ange **aktivera fjärrhantering** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-136">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="3ebab-137">Nu kan du välja tooconnect med HTTP.</span><span class="sxs-lookup"><span data-stu-id="3ebab-137">You can now choose tooconnect using HTTP.</span></span> <span data-ttu-id="3ebab-138">(hello är standard tooconnect via HTTPS.) Se till att HTTP har valts.</span><span class="sxs-lookup"><span data-stu-id="3ebab-138">(hello default is tooconnect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3ebab-139">Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="3ebab-139">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   > 
   > 
5. <span data-ttu-id="3ebab-140">Klicka på **spara** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="3ebab-140">Click **Save** at hello bottom of hello page.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a><span data-ttu-id="3ebab-141">Använda hello seriekonsolen tooenable remote management via HTTP</span><span class="sxs-lookup"><span data-stu-id="3ebab-141">Use hello serial console tooenable remote management over HTTP</span></span>
<span data-ttu-id="3ebab-142">Utför följande hello seriekonsolen tooenable remote enhetshantering hello.</span><span class="sxs-lookup"><span data-stu-id="3ebab-142">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="3ebab-143">tooenable fjärrhantering via hello enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="3ebab-143">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="3ebab-144">På menyn för seriekonsolen av hello väljer du alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="3ebab-144">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="3ebab-145">Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3ebab-145">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="3ebab-146">I hello kommandotolk, skriver du:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="3ebab-146">At hello prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="3ebab-147">Du meddelas om hello säkerhetsrisker med att använda HTTP tooconnect toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-147">You will be notified about hello security vulnerabilities of using HTTP tooconnect toohello device.</span></span> <span data-ttu-id="3ebab-148">När du uppmanas bekräfta genom att skriva **Y**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-148">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="3ebab-149">Kontrollera att HTTP har aktiverats genom att skriva:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="3ebab-149">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="3ebab-150">Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsAndHttpEnabled**.hello följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="3ebab-150">Verify that hello **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS och HTTP-aktiverad](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a><span data-ttu-id="3ebab-152">Förbered hello-klient för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="3ebab-152">Prepare hello client for remote connection</span></span>
<span data-ttu-id="3ebab-153">Utför följande steg på klientens hello tooenable fjärrhantering hello.</span><span class="sxs-lookup"><span data-stu-id="3ebab-153">Perform hello following steps on hello client tooenable remote management.</span></span>

#### <a name="tooprepare-hello-client-for-remote-connection"></a><span data-ttu-id="3ebab-154">tooprepare hello klienten för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="3ebab-154">tooprepare hello client for remote connection</span></span>
1. <span data-ttu-id="3ebab-155">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="3ebab-155">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="3ebab-156">Skriv följande kommando tooadd hello IP-adress i listan över betrodda värdar hello StorSimple enheten toohello klientens hello:</span><span class="sxs-lookup"><span data-stu-id="3ebab-156">Type hello following command tooadd hello IP address of hello StorSimple device toohello client’s trusted hosts list:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="3ebab-157">Ersätt <*device_ip*> med hello IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3ebab-157">Replace <*device_ip*> with hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="3ebab-158">Skriv följande kommando toosave hello enheten autentiseringsuppgifter i en variabel hello:</span><span class="sxs-lookup"><span data-stu-id="3ebab-158">Type hello following command toosave hello device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="3ebab-159">Hello i dialogrutan som visas:</span><span class="sxs-lookup"><span data-stu-id="3ebab-159">In hello dialog box that appears:</span></span>
   
   1. <span data-ttu-id="3ebab-160">Skriv hello användarnamn i formatet: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="3ebab-160">Type hello user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="3ebab-161">Ange hello enhetens administratörslösenord som angavs när hello enhet har konfigurerats med hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="3ebab-161">Type hello device administrator password that was set when hello device was configured with hello setup wizard.</span></span> <span data-ttu-id="3ebab-162">hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="3ebab-162">hello default password is *Password1*.</span></span>
5. <span data-ttu-id="3ebab-163">Starta Windows PowerShell-sessionen hello enheten genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ebab-163">Start a Windows PowerShell session on hello device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="3ebab-164">toocreate en Windows PowerShell-session för användning med hello virtuell StorSimple-enhet, Lägg till hello `–Port` parametern och ange hello offentlig port som du konfigurerade i fjärrkommunikation för virtuell StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-164">toocreate a Windows PowerShell session for use with hello StorSimple virtual device, append hello `–Port` parameter and specify hello public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   > 
   > 
   
     <span data-ttu-id="3ebab-165">Du bör nu ha en aktiv Windows PowerShell-session toohello fjärrenheten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-165">At this point, you should have an active remote Windows PowerShell session toohello device.</span></span>
   
    ![PowerShell-fjärrkommunikation med HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="3ebab-167">Ansluta via HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ebab-167">Connect through HTTPS</span></span>
<span data-ttu-id="3ebab-168">Ansluter tooWindows PowerShell för StorSimple via en HTTPS-session är hello säkraste och rekommenderade metoden för enhet som ansluter via fjärranslutning tooyour Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3ebab-168">Connecting tooWindows PowerShell for StorSimple through an HTTPS session is hello most secure and recommended method of remotely connecting tooyour Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="3ebab-169">hello följande procedurer beskrivs hur tooset in hello serial-konsolen och klienten datorer så att du kan använda HTTPS tooconnect tooWindows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3ebab-169">hello following procedures explain how tooset up hello serial console and client computers so that you can use HTTPS tooconnect tooWindows PowerShell for StorSimple.</span></span>

<span data-ttu-id="3ebab-170">Du kan använda hello klassiska Azure-portalen eller hello seriekonsolen tooconfigure för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="3ebab-170">You can use either hello Azure classic portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="3ebab-171">Välj hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="3ebab-171">Select from hello following procedures:</span></span>

* [<span data-ttu-id="3ebab-172">Använda hello Azure klassiska portal tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ebab-172">Use hello Azure classic portal tooenable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="3ebab-173">Använda hello seriekonsolen tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ebab-173">Use hello serial console tooenable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="3ebab-174">När du aktiverar fjärrhantering, Använd följande procedurer tooprepare hello värden för en fjärrhantering hello och ansluter toohello enheten från hello fjärrvärden.</span><span class="sxs-lookup"><span data-stu-id="3ebab-174">After you enable remote management, use hello following procedures tooprepare hello host for a remote management and connect toohello device from hello remote host.</span></span>

* [<span data-ttu-id="3ebab-175">Förbereda hello värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="3ebab-175">Prepare hello host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="3ebab-176">Anslut toohello enhet från hello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="3ebab-176">Connect toohello device from hello remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-https"></a><span data-ttu-id="3ebab-177">Använda hello Azure klassiska portal tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ebab-177">Use hello Azure classic portal tooenable remote management over HTTPS</span></span>
<span data-ttu-id="3ebab-178">Utföra hello följa stegen i hello Azure klassiska portal tooenable fjärrhantering över HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ebab-178">Perform hello following steps in hello Azure classic portal tooenable remote management over HTTPS.</span></span>

#### <a name="tooenable-remote-management-over-https-from-hello-azure-classic-portal"></a><span data-ttu-id="3ebab-179">tooenable remote management via HTTPS från hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3ebab-179">tooenable remote management over HTTPS from hello Azure classic portal</span></span>
1. <span data-ttu-id="3ebab-180">Åtkomst **enheter** > **konfigurera** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-180">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="3ebab-181">Bläddra nedåt toohello **fjärrhantering** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3ebab-181">Scroll down toohello **Remote Management** section.</span></span>
3. <span data-ttu-id="3ebab-182">Ange **aktivera fjärrhantering** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-182">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="3ebab-183">Nu kan du välja tooconnect med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ebab-183">You can now choose tooconnect using HTTPS.</span></span> <span data-ttu-id="3ebab-184">(hello är standard tooconnect via HTTPS.) Kontrollera att HTTPS är markerad.</span><span class="sxs-lookup"><span data-stu-id="3ebab-184">(hello default is tooconnect over HTTPS.) Make sure that HTTPS is selected.</span></span> 
5. <span data-ttu-id="3ebab-185">Klicka på **hämta certifikat för fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-185">Click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="3ebab-186">Ange en plats toosave den här filen.</span><span class="sxs-lookup"><span data-stu-id="3ebab-186">Specify a location toosave this file.</span></span> <span data-ttu-id="3ebab-187">Du behöver tooinstall certifikatet på hello klienten eller värddatorn dator som du ska använda tooconnect toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-187">You will need tooinstall this certificate on hello client or host computer that you will use tooconnect toohello device.</span></span>
6. <span data-ttu-id="3ebab-188">Klicka på **spara** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="3ebab-188">Click **Save** at hello bottom of hello page.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a><span data-ttu-id="3ebab-189">Använda hello seriekonsolen tooenable remote management via HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ebab-189">Use hello serial console tooenable remote management over HTTPS</span></span>
<span data-ttu-id="3ebab-190">Utför följande hello seriekonsolen tooenable remote enhetshantering hello.</span><span class="sxs-lookup"><span data-stu-id="3ebab-190">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="3ebab-191">tooenable fjärrhantering via hello enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="3ebab-191">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="3ebab-192">På menyn för seriekonsolen av hello väljer du alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="3ebab-192">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="3ebab-193">Mer information om hur du använder hello seriekonsolen på hello enhet gå för[ansluta tooWindows PowerShell för StorSimple via enhetens seriekonsol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3ebab-193">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="3ebab-194">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="3ebab-194">At hello prompt, type:</span></span> 
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="3ebab-195">Detta bör aktivera HTTPS på enheten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-195">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="3ebab-196">Kontrollera att HTTPS har aktiverats genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="3ebab-196">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="3ebab-197">Kontrollera att hello **RemoteManagementMode** fältet visar **HttpsEnabled**.hello följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="3ebab-197">Make sure that hello **RemoteManagementMode** field shows **HttpsEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS-aktiverade](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="3ebab-199">Från hello utdata från `Get-HcsSystem`, kopiera hello serienumret för hello enheten och spara den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="3ebab-199">From hello output of `Get-HcsSystem`, copy hello serial number of hello device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3ebab-200">hello serienummer mappar toohello CN-namn i hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="3ebab-200">hello serial number maps toohello CN name in hello certificate.</span></span>
   > 
   > 
5. <span data-ttu-id="3ebab-201">Hämta ett certifikat för fjärrhantering genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="3ebab-201">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="3ebab-202">Ett certifikat liknande toohello följande visas.</span><span class="sxs-lookup"><span data-stu-id="3ebab-202">A certificate similar toohello following will appear.</span></span>
   
    ![Hämta certifikat för fjärrhantering](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="3ebab-204">Kopiera hello i hello certifikat från **---BEGIN CERTIFICATE---** för**---END CERTIFICATE---** i en textredigerare, till exempel Anteckningar och spara den som en .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="3ebab-204">Copy hello information in hello certificate from **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="3ebab-205">(Du kommer att kopiera den här filen tooyour fjärrvärden när du förbereder hello värden.)</span><span class="sxs-lookup"><span data-stu-id="3ebab-205">(You will copy this file tooyour remote host when you prepare hello host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3ebab-206">toogenerate ett nytt certifikat använder hello `Set-HcsRemoteManagementCert` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-206">toogenerate a new certificate, use hello `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   > 
   > 

### <a name="prepare-hello-host-for-remote-management"></a><span data-ttu-id="3ebab-207">Förbereda hello värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="3ebab-207">Prepare hello host for remote management</span></span>
<span data-ttu-id="3ebab-208">tooprepare hello värddatorn för en anslutning som använder en HTTPS-session, utföra hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="3ebab-208">tooprepare hello host computer for a remote connection that uses an HTTPS session, perform hello following procedures:</span></span>

* <span data-ttu-id="3ebab-209">[Importera hello .cer-fil i hello rotarkivet hello klienten eller fjärrvärden](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="3ebab-209">[Import hello .cer file into hello root store of hello client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="3ebab-210">[Lägg till hello enhetens serienummer toohello hosts-filen på din fjärrvärden](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="3ebab-210">[Add hello device serial numbers toohello hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="3ebab-211">Var och en av de här procedurerna beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="3ebab-211">Each of these procedures is described below.</span></span>

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a><span data-ttu-id="3ebab-212">tooimport hello certifikatet på fjärrvärden hello</span><span class="sxs-lookup"><span data-stu-id="3ebab-212">tooimport hello certificate on hello remote host</span></span>
1. <span data-ttu-id="3ebab-213">Högerklicka på hello .cer-fil och välj **installera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-213">Right-click hello .cer file and select **Install certificate**.</span></span> <span data-ttu-id="3ebab-214">Detta startar hello guiden Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="3ebab-214">This will start hello Certificate Import Wizard.</span></span>
   
    ![Guiden Importera 1 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="3ebab-216">För **plats**väljer **lokal dator**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-216">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="3ebab-217">Välj **placera alla certifikat i följande store hello**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-217">Select **Place all certificates in hello following store**, and then click **Browse**.</span></span> <span data-ttu-id="3ebab-218">Navigera toohello rotarkivet på din fjärrvärden och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-218">Navigate toohello root store of your remote host, and then click **Next**.</span></span>
   
    ![Guiden Importera 2 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="3ebab-220">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-220">Click **Finish**.</span></span> <span data-ttu-id="3ebab-221">Visas ett meddelande som talar om att hello importen lyckades.</span><span class="sxs-lookup"><span data-stu-id="3ebab-221">A message that tells you that hello import was successful appears.</span></span>
   
    ![Guiden Importera 3 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a><span data-ttu-id="3ebab-223">tooadd enhetens serienummer toohello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="3ebab-223">tooadd device serial numbers toohello remote host</span></span>
1. <span data-ttu-id="3ebab-224">Starta Anteckningar som administratör och öppna hello värdfilen som finns på \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="3ebab-224">Start Notepad as an administrator, and then open hello hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="3ebab-225">Lägg till följande tre poster tooyour värdfilen hello: **DATA 0 IP-adress**, **fasta IP-adressen för styrenhet 0**, och **domänkontrollant 1 fasta IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="3ebab-225">Add hello following three entries tooyour hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="3ebab-226">Ange hello enhetens serienummer som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3ebab-226">Enter hello device serial number that you saved earlier.</span></span> <span data-ttu-id="3ebab-227">Den här toohello IP-adressen som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="3ebab-227">Map this toohello IP address as shown in hello following image.</span></span> <span data-ttu-id="3ebab-228">Lägg till för styrenhet 0 och 1 **Controller0** och **Controller1** hello slutet av hello serienummer (CN-namn).</span><span class="sxs-lookup"><span data-stu-id="3ebab-228">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at hello end of hello serial number (CN name).</span></span>
   
    ![Lägger till CN-namnet toohosts fil](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="3ebab-230">Spara hello hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="3ebab-230">Save hello hosts file.</span></span>

### <a name="connect-toohello-device-from-hello-remote-host"></a><span data-ttu-id="3ebab-231">Anslut toohello enhet från hello fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="3ebab-231">Connect toohello device from hello remote host</span></span>
<span data-ttu-id="3ebab-232">Använda Windows PowerShell och SSL tooenter en SSAdmin sessionen på din enhet från en fjärrvärd eller klienten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-232">Use Windows PowerShell and SSL tooenter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="3ebab-233">Hej SSAdmin session mappar toooption 1 i hello [seriekonsolen](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) -menyn i enheten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-233">hello SSAdmin session maps toooption 1 in hello [serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="3ebab-234">Utföra hello följa proceduren på hello som du vill toomake hello Windows PowerShell fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="3ebab-234">Perform hello following procedure on hello computer from which you want toomake hello remote Windows PowerShell connection.</span></span>

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="3ebab-235">tooenter en SSAdmin sessionen på hello-enhet med hjälp av Windows PowerShell och SSL</span><span class="sxs-lookup"><span data-stu-id="3ebab-235">tooenter an SSAdmin session on hello device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="3ebab-236">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="3ebab-236">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="3ebab-237">Lägg till hello enhetens IP-adress toohello klientens betrodda värdar genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="3ebab-237">Add hello device IP address toohello client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="3ebab-238">Där <*device_ip*> är hello IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3ebab-238">Where <*device_ip*> is hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="3ebab-239">Skapa en ny autentiseringsuppgift genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="3ebab-239">Create a new credential by typing:</span></span> 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="3ebab-240">Där <*IP målenhet*> är hello IP-adressen för DATA 0 för din enhet, till exempel **10.126.173.90** enligt hello föregående bild av hello hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="3ebab-240">Where <*IP of target device*> is hello IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in hello preceding image of hello hosts file.</span></span> <span data-ttu-id="3ebab-241">Ange dessutom hello administratörslösenordet för enheten.</span><span class="sxs-lookup"><span data-stu-id="3ebab-241">Also, supply hello administrator password for your device.</span></span>
4. <span data-ttu-id="3ebab-242">Skapa en session genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="3ebab-242">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="3ebab-243">Ange hello hello - ComputerName parameter i cmdleten hello <*serienumret för målenhet*>.</span><span class="sxs-lookup"><span data-stu-id="3ebab-243">For hello -ComputerName parameter in hello cmdlet, provide hello <*serial number of target device*>.</span></span> <span data-ttu-id="3ebab-244">Det här serienumret mappades toohello IP-adress för DATA 0 i hello värdfilen på fjärrvärden; till exempel **SHX0991003G44MT** som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="3ebab-244">This serial number was mapped toohello IP address of DATA 0 in hello hosts file on your remote host; for example, **SHX0991003G44MT** as shown in hello following image.</span></span>
5. <span data-ttu-id="3ebab-245">Ange:</span><span class="sxs-lookup"><span data-stu-id="3ebab-245">Type:</span></span> 
   
     `Enter-PSSession $session`
6. <span data-ttu-id="3ebab-246">Du behöver toowait några minuter och du kommer sedan att anslutna tooyour enheten via HTTPS via SSL.</span><span class="sxs-lookup"><span data-stu-id="3ebab-246">You will need toowait a few minutes, and then you will be connected tooyour device via HTTPS over SSL.</span></span> <span data-ttu-id="3ebab-247">Du ser ett meddelande som anger att du är ansluten tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="3ebab-247">You will see a message that indicates you are connected tooyour device.</span></span>
   
    ![PowerShell-fjärrkommunikation med hjälp av HTTPS och SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="3ebab-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ebab-249">Next steps</span></span>
* <span data-ttu-id="3ebab-250">Lär dig mer om [med hjälp av Windows PowerShell tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3ebab-250">Learn more about [using Windows PowerShell tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="3ebab-251">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3ebab-251">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

