---
title: "Fjärransluta till din StorSimple-enhet | Microsoft Docs"
description: "Beskriver hur du konfigurerar enheten för fjärrhantering och hur du ansluter till Windows PowerShell för StorSimple via HTTP eller HTTPS."
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
ms.openlocfilehash: ff76884f020a0fb8a1b48bd371c419bd65e85fd3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a><span data-ttu-id="9e9fa-103">Fjärransluta till enheten StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="9e9fa-103">Connect remotely to your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="9e9fa-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9e9fa-104">Overview</span></span>

<span data-ttu-id="9e9fa-105">Du kan fjärransluta till din enhet via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-105">You can remotely connect to your device via Windows PowerShell.</span></span> <span data-ttu-id="9e9fa-106">När du ansluter det här sättet kan ser du inte en meny.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="9e9fa-107">(Du se en meny endast om du använder seriekonsolen på enheten för att ansluta.) Med Windows PowerShell-fjärrkommunikation ansluta till en specifik runspace.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-107">(You see a menu only if you use the serial console on the device to connect.) With Windows PowerShell remoting, you connect to a specific runspace.</span></span> <span data-ttu-id="9e9fa-108">Du kan också ange språk för visning.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-108">You can also specify the display language.</span></span>

<span data-ttu-id="9e9fa-109">Mer information om hur du använder Windows PowerShell-fjärrkommunikation för att hantera enheten, gå till [använda Windows PowerShell för StorSimple att administrera din StorSimple-enhet](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-109">For more information about using Windows PowerShell remoting to manage your device, go to [Use Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="9e9fa-110">Den här självstudiekursen beskrivs hur du konfigurerar enheten för fjärrhantering och hur du ansluter till Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-110">This tutorial explains how to configure your device for remote management and then how to connect to Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="9e9fa-111">Du kan använda HTTP eller HTTPS för att fjärransluta via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-111">You can use HTTP or HTTPS to remotely connect via Windows PowerShell.</span></span> <span data-ttu-id="9e9fa-112">Men när du bestämmer hur du ansluter till Windows PowerShell för StorSimple, Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-112">However, when you are deciding how to connect to Windows PowerShell for StorSimple, consider the following information:</span></span>

* <span data-ttu-id="9e9fa-113">Ansluta direkt till enhetens seriekonsol är säker och ansluter till seriekonsol via nätverksväxlar inte.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-113">Connecting directly to the device serial console is secure, but connecting to the serial console over network switches is not.</span></span> <span data-ttu-id="9e9fa-114">Var försiktig för säkerhetsrisken när du ansluter till enhetens seriekonsol via nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-114">Be cautious of the security risk when connecting to the device serial console over network switches.</span></span>
* <span data-ttu-id="9e9fa-115">Ansluter via en HTTP-session kan erbjuda mer säkerhet än att ansluta via seriekonsolen över nätverket.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-115">Connecting through an HTTP session might offer more security than connecting through the serial console over the network.</span></span> <span data-ttu-id="9e9fa-116">Även om detta inte är den säkraste metoden är är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-116">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="9e9fa-117">Ansluter via en HTTPS-session med ett självsignerat certifikat är den säkraste och det rekommenderade alternativet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-117">Connecting through an HTTPS session with a self-signed certificate is the most secure and the recommended option.</span></span>

<span data-ttu-id="9e9fa-118">Du kan fjärransluta till Windows PowerShell-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-118">You can connect remotely to the Windows PowerShell interface.</span></span> <span data-ttu-id="9e9fa-119">Fjärråtkomst till din StorSimple-enhet via Windows PowerShell-gränssnittet är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-119">However, remote access to your StorSimple device via the Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="9e9fa-120">Du måste först aktivera fjärrhantering på enheten och klicka sedan på klienten som används för åtkomst till din enhet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-120">You must enable remote management on the device first, and then on the client that is used to access your device.</span></span>

<span data-ttu-id="9e9fa-121">Stegen som beskrivs i den här artikeln utfördes på ett värdsystem som kör Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-121">The steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="9e9fa-122">Ansluta via HTTP</span><span class="sxs-lookup"><span data-stu-id="9e9fa-122">Connect through HTTP</span></span>

<span data-ttu-id="9e9fa-123">Ansluter till Windows PowerShell för StorSimple via en HTTP-session är säkrare än att ansluta via seriekonsolen av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-123">Connecting to Windows PowerShell for StorSimple through an HTTP session offers more security than connecting through the serial console of your StorSimple device.</span></span> <span data-ttu-id="9e9fa-124">Även om detta inte är den säkraste metoden är är det acceptabelt på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-124">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="9e9fa-125">Du kan använda Azure-portalen eller seriekonsolen för att konfigurera fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-125">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="9e9fa-126">Välj följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-126">Select from the following procedures:</span></span>

* [<span data-ttu-id="9e9fa-127">Använda Azure portal för att aktivera fjärrhantering över HTTP</span><span class="sxs-lookup"><span data-stu-id="9e9fa-127">Use the Azure portal to enable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="9e9fa-128">Använd seriekonsolen för att aktivera fjärrhantering över HTTP</span><span class="sxs-lookup"><span data-stu-id="9e9fa-128">Use the serial console to enable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="9e9fa-129">När du aktiverar fjärrhantering, Använd följande procedur för att Förbered för en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-129">After you enable remote management, use the following procedure to prepare the client for a remote connection.</span></span>

* [<span data-ttu-id="9e9fa-130">Förbered för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="9e9fa-130">Prepare the client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a><span data-ttu-id="9e9fa-131">Använda Azure portal för att aktivera fjärrhantering över HTTP</span><span class="sxs-lookup"><span data-stu-id="9e9fa-131">Use the Azure portal to enable remote management over HTTP</span></span>

<span data-ttu-id="9e9fa-132">Utför följande steg i Azure-portalen för att aktivera fjärrhantering över HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-132">Perform the following steps in the Azure portal to enable remote management over HTTP.</span></span>

#### <a name="to-enable-remote-management-through-the-azure-portal"></a><span data-ttu-id="9e9fa-133">Så här aktiverar du fjärrhantering via Azure portal</span><span class="sxs-lookup"><span data-stu-id="9e9fa-133">To enable remote management through the Azure portal</span></span>

1. <span data-ttu-id="9e9fa-134">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-134">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="9e9fa-135">Välj **enheter** och markera och klicka på den enhet som du vill konfigurera för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-135">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="9e9fa-136">Gå till **Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-136">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="9e9fa-137">I den **säkerhetsinställningar** bladet, klickar du på **fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-137">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="9e9fa-138">I den **fjärrhantering** bladet ange **aktivera fjärrhantering** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-138">In the **Remote management** blade, set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="9e9fa-139">Nu kan du välja att ansluta med HTTP.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-139">You can now choose to connect using HTTP.</span></span> <span data-ttu-id="9e9fa-140">(Standard är att ansluta via HTTPS.) Se till att HTTP har valts.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-140">(The default is to connect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9e9fa-141">Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="9e9fa-142">Klicka på **spara** och när du uppmanas att bekräfta, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a><span data-ttu-id="9e9fa-143">Använd seriekonsolen för att aktivera fjärrhantering över HTTP</span><span class="sxs-lookup"><span data-stu-id="9e9fa-143">Use the serial console to enable remote management over HTTP</span></span>
<span data-ttu-id="9e9fa-144">Utför följande steg på enhetens seriekonsol för att aktivera fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-144">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="9e9fa-145">Så här aktiverar du fjärrhantering via enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="9e9fa-145">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="9e9fa-146">Välj alternativ 1 på menyn för seriekonsolen.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-146">On the serial console menu, select option 1.</span></span> <span data-ttu-id="9e9fa-147">Mer information om hur du använder seriekonsolen på enheten, gå till [Anslut till Windows PowerShell för StorSimple via enhetens seriekonsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-147">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="9e9fa-148">Skriv följande i Kommandotolken:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="9e9fa-148">At the prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="9e9fa-149">Du meddelas om säkerhetsrisker med att använda HTTP för att ansluta till enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-149">You are notified about the security vulnerabilities of using HTTP to connect to the device.</span></span> <span data-ttu-id="9e9fa-150">När du uppmanas bekräfta genom att skriva **Y**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="9e9fa-151">Kontrollera att HTTP har aktiverats genom att skriva:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="9e9fa-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="9e9fa-152">Kontrollera att den **RemoteManagementMode** fältet visar **HttpsAndHttpEnabled**. Följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-152">Verify that the **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS och HTTP-aktiverad](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a><span data-ttu-id="9e9fa-154">Förbered för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="9e9fa-154">Prepare the client for remote connection</span></span>
<span data-ttu-id="9e9fa-155">Utför följande steg på klienten för att aktivera fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-155">Perform the following steps on the client to enable remote management.</span></span>

#### <a name="to-prepare-the-client-for-remote-connection"></a><span data-ttu-id="9e9fa-156">Så här förbereder du klienten för fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="9e9fa-156">To prepare the client for remote connection</span></span>
1. <span data-ttu-id="9e9fa-157">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="9e9fa-158">Skriv följande kommando för att lägga till IP-adressen för StorSimple-enhet i listan över betrodda värdar klientens:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-158">Type the following command to add the IP address of the StorSimple device to the client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="9e9fa-159">Ersätt <*device_ip*> med IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-159">Replace <*device_ip*> with the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="9e9fa-160">Skriv följande kommando för att spara autentiseringsuppgifter för enheten i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-160">Type the following command to save the device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="9e9fa-161">I dialogrutan som visas:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-161">In the dialog box that appears:</span></span>
   
   1. <span data-ttu-id="9e9fa-162">Ange användarnamnet i formatet: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-162">Type the user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="9e9fa-163">Ange enhetens administratörslösenord som angavs när enheten konfigurerades med installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-163">Type the device administrator password that was set when the device was configured with the setup wizard.</span></span> <span data-ttu-id="9e9fa-164">Standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-164">The default password is *Password1*.</span></span>
5. <span data-ttu-id="9e9fa-165">Starta Windows PowerShell-sessionen på enheten genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-165">Start a Windows PowerShell session on the device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="9e9fa-166">Om du vill skapa en Windows PowerShell-session för användning med den virtuella enheten StorSimple, bifoga den `–Port` parametern och ange den offentliga porten som du konfigurerade i fjärrkommunikation för virtuell StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-166">To create a Windows PowerShell session for use with the StorSimple virtual device, append the `–Port` parameter and specify the public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="9e9fa-167">Du bör nu ha en aktiv Windows PowerShell-fjärrsession till enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-167">At this point, you should have an active remote Windows PowerShell session to the device.</span></span>
   
![PowerShell-fjärrkommunikation med HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="9e9fa-169">Ansluta via HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e9fa-169">Connect through HTTPS</span></span>

<span data-ttu-id="9e9fa-170">Ansluter till Windows PowerShell för StorSimple via en HTTPS-session är den mest säkra och rekommenderade metoden för att fjärransluta till din Microsoft Azure StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-170">Connecting to Windows PowerShell for StorSimple through an HTTPS session is the most secure and recommended method of remotely connecting to your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="9e9fa-171">Följande procedurer beskriver hur du ställer in serial-konsolen och klienten datorerna så att du kan använda HTTPS för att ansluta till Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-171">The following procedures explain how to set up the serial console and client computers so that you can use HTTPS to connect to Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="9e9fa-172">Du kan använda Azure-portalen eller seriekonsolen för att konfigurera fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-172">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="9e9fa-173">Välj följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-173">Select from the following procedures:</span></span>

* [<span data-ttu-id="9e9fa-174">Använda Azure portal för att aktivera fjärrhantering över HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e9fa-174">Use the Azure portal to enable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="9e9fa-175">Använd seriekonsolen för att aktivera fjärrhantering över HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e9fa-175">Use the serial console to enable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="9e9fa-176">När du aktiverar fjärrhantering, Använd följande procedurer för att förbereda värden för en fjärrhantering och ansluter till enheten från fjärrvärden.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-176">After you enable remote management, use the following procedures to prepare the host for a remote management and connect to the device from the remote host.</span></span>

* [<span data-ttu-id="9e9fa-177">Förbereda värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="9e9fa-177">Prepare the host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="9e9fa-178">Ansluta till enheten från fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="9e9fa-178">Connect to the device from the remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a><span data-ttu-id="9e9fa-179">Använda Azure portal för att aktivera fjärrhantering över HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e9fa-179">Use the Azure portal to enable remote management over HTTPS</span></span>

<span data-ttu-id="9e9fa-180">Utför följande steg i Azure-portalen för att aktivera fjärrhantering över HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-180">Perform the following steps in the Azure portal to enable remote management over HTTPS.</span></span>

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a><span data-ttu-id="9e9fa-181">Att aktivera fjärrhantering via HTTPS från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9e9fa-181">To enable remote management over HTTPS from the Azure portal</span></span>

1. <span data-ttu-id="9e9fa-182">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-182">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="9e9fa-183">Välj **enheter** och markera och klicka på den enhet som du vill konfigurera för fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-183">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="9e9fa-184">Gå till **Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-184">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="9e9fa-185">I den **säkerhetsinställningar** bladet, klickar du på **fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-185">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="9e9fa-186">Ställ in **Aktivera fjärrhantering** på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-186">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="9e9fa-187">Du kan nu välja att ansluta med HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-187">You can now choose to connect using HTTPS.</span></span> <span data-ttu-id="9e9fa-188">(Standard är att ansluta via HTTPS.) Kontrollera att HTTPS är markerad.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-188">(The default is to connect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="9e9fa-189">... Och klicka sedan på **hämta certifikat för fjärrhantering**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="9e9fa-190">Ange en plats om du vill spara den här filen.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-190">Specify a location to save this file.</span></span> <span data-ttu-id="9e9fa-191">Du måste installera certifikatet på klienten eller värddatorn datorn som du använder för att ansluta till enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-191">You need to install this certificate on the client or host computer that you will use to connect to the device.</span></span>
6. <span data-ttu-id="9e9fa-192">Klicka på **spara** och klicka sedan på **Ja** när du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a><span data-ttu-id="9e9fa-193">Använd seriekonsolen för att aktivera fjärrhantering över HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e9fa-193">Use the serial console to enable remote management over HTTPS</span></span>

<span data-ttu-id="9e9fa-194">Utför följande steg på enhetens seriekonsol för att aktivera fjärrhantering.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-194">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="9e9fa-195">Så här aktiverar du fjärrhantering via enhetens seriekonsol</span><span class="sxs-lookup"><span data-stu-id="9e9fa-195">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="9e9fa-196">Välj alternativ 1 på menyn för seriekonsolen.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-196">On the serial console menu, select option 1.</span></span> <span data-ttu-id="9e9fa-197">Mer information om hur du använder seriekonsolen på enheten, gå till [Anslut till Windows PowerShell för StorSimple via enhetens seriekonsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-197">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="9e9fa-198">Skriv följande i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-198">At the prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="9e9fa-199">Detta bör aktivera HTTPS på enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="9e9fa-200">Kontrollera att HTTPS har aktiverats genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="9e9fa-201">Se till att den **RemoteManagementMode** fältet visar **HttpsEnabled**. Följande bild visar de här inställningarna i PuTTY.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-201">Make sure that the **RemoteManagementMode** field shows **HttpsEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Seriell HTTPS-aktiverade](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="9e9fa-203">Från utdata från `Get-HcsSystem`, kopiera serienumret på enheten och spara den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-203">From the output of `Get-HcsSystem`, copy the serial number of the device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9e9fa-204">Serienumret mappas till CN-namnet i certifikatet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-204">The serial number maps to the CN name in the certificate.</span></span>
   
5. <span data-ttu-id="9e9fa-205">Hämta ett certifikat för fjärrhantering genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="9e9fa-206">Ett certifikat som liknar följande visas.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-206">A certificate similar to the following will appear.</span></span>
   
    ![Hämta certifikat för fjärrhantering](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="9e9fa-208">Kopiera informationen i certifikatet från **---BEGIN CERTIFICATE---** till **---END CERTIFICATE---** i en textredigerare, till exempel Anteckningar och spara den som en .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-208">Copy the information in the certificate from **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="9e9fa-209">(Du kopierar filen till din fjärrvärden när du förbereder värden.)</span><span class="sxs-lookup"><span data-stu-id="9e9fa-209">(You will copy this file to your remote host when you prepare the host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9e9fa-210">Om du vill skapa ett nytt certifikat, använder den `Set-HcsRemoteManagementCert` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-210">To generate a new certificate, use the `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-the-host-for-remote-management"></a><span data-ttu-id="9e9fa-211">Förbereda värden för fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="9e9fa-211">Prepare the host for remote management</span></span>

<span data-ttu-id="9e9fa-212">Utför följande procedurer för att förbereda värddatorn för en anslutning som använder en HTTPS-session:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-212">To prepare the host computer for a remote connection that uses an HTTPS session, perform the following procedures:</span></span>

* <span data-ttu-id="9e9fa-213">[Importera CER-filen till rotarkivet på klienten eller fjärrvärden](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-213">[Import the .cer file into the root store of the client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="9e9fa-214">[Lägg till enhetens serienummer i hosts-filen på din fjärrvärden](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-214">[Add the device serial numbers to the hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="9e9fa-215">Var och en av de föregående procedurerna beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-215">Each of the preceding procedures, is described below.</span></span>

#### <a name="to-import-the-certificate-on-the-remote-host"></a><span data-ttu-id="9e9fa-216">Importera certifikatet på fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="9e9fa-216">To import the certificate on the remote host</span></span>
1. <span data-ttu-id="9e9fa-217">Högerklicka på .cer-filen och välj **installera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-217">Right-click the .cer file and select **Install certificate**.</span></span> <span data-ttu-id="9e9fa-218">Detta startar guiden Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-218">This starts the Certificate Import Wizard.</span></span>
   
    ![Guiden Importera 1 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="9e9fa-220">För **plats**väljer **lokal dator**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="9e9fa-221">Välj **placera alla certifikat i nedanstående arkiv**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-221">Select **Place all certificates in the following store**, and then click **Browse**.</span></span> <span data-ttu-id="9e9fa-222">Navigera till rotarkivet för din fjärrvärden och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-222">Navigate to the root store of your remote host, and then click **Next**.</span></span>
   
    ![Guiden Importera 2 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="9e9fa-224">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-224">Click **Finish**.</span></span> <span data-ttu-id="9e9fa-225">Visas ett meddelande som talar om att importen lyckades.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-225">A message that tells you that the import was successful appears.</span></span>
   
    ![Guiden Importera 3 certifikat](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a><span data-ttu-id="9e9fa-227">Att lägga till Enhetsserienummer fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="9e9fa-227">To add device serial numbers to the remote host</span></span>
1. <span data-ttu-id="9e9fa-228">Starta Anteckningar som administratör och öppna värdfilen som finns på \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-228">Start Notepad as an administrator, and then open the hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="9e9fa-229">Lägg till följande tre poster i hosts-filen: **DATA 0 IP-adress**, **fasta IP-adressen för styrenhet 0**, och **domänkontrollant 1 fasta IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-229">Add the following three entries to your hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="9e9fa-230">Ange serienumret för enheten som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-230">Enter the device serial number that you saved earlier.</span></span> <span data-ttu-id="9e9fa-231">Mappa till IP-adressen som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-231">Map this to the IP address as shown in the following image.</span></span> <span data-ttu-id="9e9fa-232">Lägg till för styrenhet 0 och 1 **Controller0** och **Controller1** i slutet av serienumret (CN-namn).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at the end of the serial number (CN name).</span></span>
   
    ![Lägger till CN-namn i hosts-filen](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="9e9fa-234">Spara hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-234">Save the hosts file.</span></span>

### <a name="connect-to-the-device-from-the-remote-host"></a><span data-ttu-id="9e9fa-235">Ansluta till enheten från fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="9e9fa-235">Connect to the device from the remote host</span></span>

<span data-ttu-id="9e9fa-236">Använda Windows PowerShell och SSL för att ange en SSAdmin session på din enhet från en fjärrvärd eller klienten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-236">Use Windows PowerShell and SSL to enter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="9e9fa-237">Sessionen SSAdmin mappar till alternativ 1 i den [seriekonsolen](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) -menyn i enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-237">The SSAdmin session maps to option 1 in the [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="9e9fa-238">Utför följande procedur på den dator som du vill göra fjärranslutning i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-238">Perform the following procedure on the computer from which you want to make the remote Windows PowerShell connection.</span></span>

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="9e9fa-239">Ange en SSAdmin session på enheten med hjälp av Windows PowerShell och SSL</span><span class="sxs-lookup"><span data-stu-id="9e9fa-239">To enter an SSAdmin session on the device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="9e9fa-240">Starta en Windows PowerShell-session som administratör.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="9e9fa-241">Lägg till enhetens IP-adress i klientens betrodda värdar genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-241">Add the device IP address to the client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="9e9fa-242">Där <*device_ip*> är IP-adressen för din enhet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-242">Where <*device_ip*> is the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="9e9fa-243">Om du vill skapa en ny autentiseringsuppgift skriver du:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-243">To create a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="9e9fa-244">Där <*IP målenhet*> är IP-adressen för DATA 0 för din enhet, till exempel **10.126.173.90** som visas i föregående bild av hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-244">Where <*IP of target device*> is the IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in the preceding image of the hosts file.</span></span> <span data-ttu-id="9e9fa-245">Dessutom ange administratörslösenordet för enheten.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-245">Also, supply the administrator password for your device.</span></span>
4. <span data-ttu-id="9e9fa-246">Skapa en session genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="9e9fa-247">Parametern - ComputerName i cmdleten, ange den <*serienumret för målenhet*>.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-247">For the -ComputerName parameter in the cmdlet, provide the <*serial number of target device*>.</span></span> <span data-ttu-id="9e9fa-248">Det här serienumret mappades till IP-adressen för DATA 0 i värdfilen på fjärrvärden; till exempel **SHX0991003G44MT** som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-248">This serial number was mapped to the IP address of DATA 0 in the hosts file on your remote host; for example, **SHX0991003G44MT** as shown in the following image.</span></span>
5. <span data-ttu-id="9e9fa-249">Ange:</span><span class="sxs-lookup"><span data-stu-id="9e9fa-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="9e9fa-250">Du kommer att behöva vänta några minuter och sedan ansluts du till din enhet via HTTPS via SSL.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-250">You will need to wait a few minutes, and then you will be connected to your device via HTTPS over SSL.</span></span> <span data-ttu-id="9e9fa-251">Du ser ett meddelande som anger att du är ansluten till din enhet.</span><span class="sxs-lookup"><span data-stu-id="9e9fa-251">You see a message that indicates you are connected to your device.</span></span>
   
    ![PowerShell-fjärrkommunikation med hjälp av HTTPS och SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="9e9fa-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e9fa-253">Next steps</span></span>

* <span data-ttu-id="9e9fa-254">Lär dig mer om [använder Windows PowerShell för att administrera din StorSimple-enhet](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-254">Learn more about [using Windows PowerShell to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="9e9fa-255">Lär dig mer om [använder Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9e9fa-255">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

