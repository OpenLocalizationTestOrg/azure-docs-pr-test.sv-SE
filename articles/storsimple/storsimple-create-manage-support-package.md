---
title: "aaaCreate ett supportpaket för StorSimple | Microsoft Docs"
description: "Lär dig hur toocreate, dekryptera och redigera ett supportpaket för din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 209aeee50e823fd2ca96ababd1d0cf3ea9cdad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="7ad12-103">Skapa och hantera ett stödpaket för StorSimple</span><span class="sxs-lookup"><span data-stu-id="7ad12-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="7ad12-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7ad12-104">Overview</span></span>
<span data-ttu-id="7ad12-105">Ett supportpaket för StorSimple är ett enkelt att använda som samlar in alla relevanta loggar tooassist Microsoft-supporten att felsöka eventuella problem för StorSimple-enhet används.</span><span class="sxs-lookup"><span data-stu-id="7ad12-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="7ad12-106">hello är insamlade loggar krypterade och komprimerade.</span><span class="sxs-lookup"><span data-stu-id="7ad12-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="7ad12-107">Den här självstudiekursen innehåller stegvisa instruktioner toocreate och hantera hello supportpaket.</span><span class="sxs-lookup"><span data-stu-id="7ad12-107">This tutorial includes step-by-step instructions toocreate and manage hello support package.</span></span>

## <a name="create-and-upload-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="7ad12-108">Skapa och ladda upp ett supportpaket i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7ad12-108">Create and upload a support package in hello Azure classic portal</span></span>
<span data-ttu-id="7ad12-109">Du kan skapa och ladda upp en stöd paketet toohello Microsoft Support webbplats via hello **Underhåll** sidan hello tjänsterna i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ad12-109">You can create and upload a support package toohello Microsoft Support site through hello **Maintenance** page of hello service in hello Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7ad12-110">hello överför kräver en nyckel för support.</span><span class="sxs-lookup"><span data-stu-id="7ad12-110">hello upload requires a support passkey.</span></span> <span data-ttu-id="7ad12-111">Supportpersonalen bör ge den här tooyou i ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="7ad12-111">Your support engineer should provide this tooyou in an email.</span></span>
> 
> 

<span data-ttu-id="7ad12-112">Ett paket med stöd för krypterade och komprimerade (CAB-fil) är skapas och överförda toohello supportwebbplats.</span><span class="sxs-lookup"><span data-stu-id="7ad12-112">An encrypted and compressed support package (.cab file) is created and uploaded toohello Support site.</span></span> <span data-ttu-id="7ad12-113">hello supportteknikern kan sedan Hämta paketet från hello supportwebbplats för felsökning av problem med hello.</span><span class="sxs-lookup"><span data-stu-id="7ad12-113">hello support engineer can then retrieve this package from hello Support site for troubleshooting hello issue.</span></span>

<span data-ttu-id="7ad12-114">Utför följande steg i hello klassiska portal toocreate ett supportpaket hello.</span><span class="sxs-lookup"><span data-stu-id="7ad12-114">Perform hello following steps in hello classic portal toocreate a support package.</span></span>

#### <a name="toocreate-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="7ad12-115">toocreate ett supportpaket i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7ad12-115">toocreate a support package in hello Azure classic portal</span></span>
1. <span data-ttu-id="7ad12-116">Välj **enheter** > **Underhåll**.</span><span class="sxs-lookup"><span data-stu-id="7ad12-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="7ad12-117">I hello **stödpaketet** väljer **skapa och ladda upp supportpaket**.</span><span class="sxs-lookup"><span data-stu-id="7ad12-117">In hello **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="7ad12-118">I hello **skapa och ladda upp supportpaket** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="7ad12-118">In hello **Create and upload support package** dialog box, do hello following:</span></span>
   
    ![Skapa för stödpaket](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="7ad12-120">I hello **stöd nyckel** text Ange hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="7ad12-120">In hello **Support Passkey** text box, enter hello passkey.</span></span> <span data-ttu-id="7ad12-121">Supportpersonalen Microsoft bör skicka den här nyckeln tooyou i e-post.</span><span class="sxs-lookup"><span data-stu-id="7ad12-121">Your Microsoft support engineer should send this passkey tooyou in email.</span></span>
   * <span data-ttu-id="7ad12-122">Välj hello kryssrutan tooprovide medgivande tooautomatically överför hello stöd paketet toohello Microsoft-supporten plats.</span><span class="sxs-lookup"><span data-stu-id="7ad12-122">Select hello check box tooprovide consent tooautomatically upload hello support package toohello Microsoft Support site.</span></span>
   * <span data-ttu-id="7ad12-123">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="7ad12-123">Click hello check icon</span></span> ![Kryssikon](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="7ad12-125">.</span><span class="sxs-lookup"><span data-stu-id="7ad12-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="7ad12-126">Skapa ett supportpaket manuellt</span><span class="sxs-lookup"><span data-stu-id="7ad12-126">Manually create a support package</span></span>
<span data-ttu-id="7ad12-127">I vissa fall måste toomanually skapa hello supportpaket via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="7ad12-127">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="7ad12-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ad12-128">For example:</span></span>

* <span data-ttu-id="7ad12-129">Om du behöver tooremove känslig information från loggen filer tidigare toosharing Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="7ad12-129">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="7ad12-130">Om du har problem med överföring hello-paket på grund av problem med tooconnectivity.</span><span class="sxs-lookup"><span data-stu-id="7ad12-130">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="7ad12-131">Du kan dela din manuellt genererad supportpaket Microsoft support via e-post.</span><span class="sxs-lookup"><span data-stu-id="7ad12-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="7ad12-132">Utför följande steg toocreate ett supportpaket i Windows PowerShell för StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="7ad12-132">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="7ad12-133">toocreate ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="7ad12-133">toocreate a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="7ad12-134">toostart en Windows PowerShell-session som administratör på fjärrdatorn hello som har använt tooconnect tooyour StorSimple-enhet, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7ad12-134">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="7ad12-135">Anslut toohello SSAdmin konsolen för din enhet i hello Windows PowerShell-sessionen:</span><span class="sxs-lookup"><span data-stu-id="7ad12-135">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="7ad12-136">Ange Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="7ad12-136">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="7ad12-137">Ange administratörslösenord för enheten hello dialogrutan som öppnas.</span><span class="sxs-lookup"><span data-stu-id="7ad12-137">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="7ad12-138">hello standardlösenordet är:</span><span class="sxs-lookup"><span data-stu-id="7ad12-138">hello default password is:</span></span>
     
      `Password1`
     
      ![Dialogrutan för PowerShell-autentiseringsuppgifter](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="7ad12-140">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ad12-140">Select **OK**.</span></span>
   * <span data-ttu-id="7ad12-141">Ange Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="7ad12-141">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="7ad12-142">Ange hello kommandona i hello-sessionen som öppnas.</span><span class="sxs-lookup"><span data-stu-id="7ad12-142">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="7ad12-143">För nätverksresurser som är lösenordsskyddad, ange:</span><span class="sxs-lookup"><span data-stu-id="7ad12-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="7ad12-144">Du uppmanas för ett lösenord, en sökväg toohello delad nätverksmapp och krypteringslösenfrasen (eftersom hello supportpaket krypteras).</span><span class="sxs-lookup"><span data-stu-id="7ad12-144">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="7ad12-145">Ett supportpaket skapas i hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="7ad12-145">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="7ad12-146">För resurser som inte är lösenordsskyddad, behöver inte hello `-Credential` parameter.</span><span class="sxs-lookup"><span data-stu-id="7ad12-146">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="7ad12-147">Ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="7ad12-147">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="7ad12-148">hello supportpaket skapas för både domänkontrollanter i hello angivna delad nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="7ad12-148">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="7ad12-149">Det är en krypterad, komprimerad fil som kan skickas tooMicrosoft stöd för felsökning.</span><span class="sxs-lookup"><span data-stu-id="7ad12-149">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="7ad12-150">Mer information finns i [kontakta Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="7ad12-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="7ad12-151">hello Export HcsSupportPackage cmdlet-parametrar</span><span class="sxs-lookup"><span data-stu-id="7ad12-151">hello Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="7ad12-152">Du kan använda följande parametrar med hello Export HcsSupportPackage cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="7ad12-152">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="7ad12-153">Parameter</span><span class="sxs-lookup"><span data-stu-id="7ad12-153">Parameter</span></span> | <span data-ttu-id="7ad12-154">Nödvändiga/valfria</span><span class="sxs-lookup"><span data-stu-id="7ad12-154">Required/Optional</span></span> | <span data-ttu-id="7ad12-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7ad12-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="7ad12-156">Krävs</span><span class="sxs-lookup"><span data-stu-id="7ad12-156">Required</span></span> |<span data-ttu-id="7ad12-157">Använd tooprovide hello platsen för hello delad nätverksmapp där hello supportpaket är placerad.</span><span class="sxs-lookup"><span data-stu-id="7ad12-157">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="7ad12-158">Krävs</span><span class="sxs-lookup"><span data-stu-id="7ad12-158">Required</span></span> |<span data-ttu-id="7ad12-159">Använd tooprovide en lösenfras toohelp kryptera hello supportpaket.</span><span class="sxs-lookup"><span data-stu-id="7ad12-159">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="7ad12-160">Valfri</span><span class="sxs-lookup"><span data-stu-id="7ad12-160">Optional</span></span> |<span data-ttu-id="7ad12-161">Använd toosupply autentiseringsuppgifter för hello delad nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="7ad12-161">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="7ad12-162">Valfri</span><span class="sxs-lookup"><span data-stu-id="7ad12-162">Optional</span></span> |<span data-ttu-id="7ad12-163">Använd tooskip hello kryptering lösenfras bekräftelsesteg.</span><span class="sxs-lookup"><span data-stu-id="7ad12-163">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="7ad12-164">Valfri</span><span class="sxs-lookup"><span data-stu-id="7ad12-164">Optional</span></span> |<span data-ttu-id="7ad12-165">Använda toospecify en katalog under *sökväg* vilka hello stöd paketet är placerad.</span><span class="sxs-lookup"><span data-stu-id="7ad12-165">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="7ad12-166">hello standardvärdet är [enhetsnamn]-[aktuellt datum och time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="7ad12-166">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="7ad12-167">Valfri</span><span class="sxs-lookup"><span data-stu-id="7ad12-167">Optional</span></span> |<span data-ttu-id="7ad12-168">Ange som **klustret** (standard) toocreate ett supportpaket för både domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="7ad12-168">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="7ad12-169">Om du vill toocreate ett paket endast för hello aktuella controller anger **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="7ad12-169">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="7ad12-170">Redigera ett supportpaket</span><span class="sxs-lookup"><span data-stu-id="7ad12-170">Edit a support package</span></span>
<span data-ttu-id="7ad12-171">När du har genererat ett supportpaket kan behöva du tooedit hello paketet tooremove känslig information.</span><span class="sxs-lookup"><span data-stu-id="7ad12-171">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="7ad12-172">Detta kan inkludera volymnamn, enhetens IP-adresser och säkerhetskopiering namn från hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="7ad12-172">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad12-173">Du kan bara redigera ett supportpaket som har genererats via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="7ad12-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="7ad12-174">Du kan inte redigera ett paket som skapats i hello klassiska Azure-portalen med StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ad12-174">You can't edit a package created in hello Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="7ad12-175">tooedit ett supportpaket innan du laddar upp den på hello Microsoft Support webbplats först dekryptera hello supportpaket, redigera hello filer och kryptera den igen.</span><span class="sxs-lookup"><span data-stu-id="7ad12-175">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="7ad12-176">Utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="7ad12-176">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="7ad12-177">tooedit ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="7ad12-177">tooedit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="7ad12-178">Generera ett supportpaket som beskrivs ovan, [toocreate ett supportpaket i Windows PowerShell för StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="7ad12-178">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="7ad12-179">[Hämta hello skriptet](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalt på klientdatorerna.</span><span class="sxs-lookup"><span data-stu-id="7ad12-179">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="7ad12-180">Importera hello Windows PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="7ad12-180">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="7ad12-181">Ange hello sökvägen toohello lokal mapp som du hämtade hello skript.</span><span class="sxs-lookup"><span data-stu-id="7ad12-181">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="7ad12-182">tooimport hello modulen, ange:</span><span class="sxs-lookup"><span data-stu-id="7ad12-182">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="7ad12-183">Alla filer som hello *.aes* filer som är komprimerade och krypteras.</span><span class="sxs-lookup"><span data-stu-id="7ad12-183">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="7ad12-184">toodecompress och dekryptera filer, ange:</span><span class="sxs-lookup"><span data-stu-id="7ad12-184">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="7ad12-185">Observera att hello faktiska filnamnstillägg nu visas för alla hello-filer.</span><span class="sxs-lookup"><span data-stu-id="7ad12-185">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="7ad12-187">När du uppmanas att ange hello krypteringslösenfrasen ange hello lösenfras som du använde när hello stödpaketet skapades.</span><span class="sxs-lookup"><span data-stu-id="7ad12-187">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="7ad12-188">Bläddra toohello mapp som innehåller hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="7ad12-188">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="7ad12-189">Eftersom hello loggfiler är nu packa upp och dekrypteras, kommer dessa har ursprungliga filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="7ad12-189">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="7ad12-190">Ändrar dessa filer tooremove kundspecifik information, till exempel volymnamn och enheten IP-adresser, och spara hello-filer.</span><span class="sxs-lookup"><span data-stu-id="7ad12-190">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="7ad12-191">Stäng hello filer toocompress dem med gzip och kryptera dem med AES 256.</span><span class="sxs-lookup"><span data-stu-id="7ad12-191">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="7ad12-192">Detta är hastighet och säkerhet i överföra hello supportpaket över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="7ad12-192">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="7ad12-193">toocompress och kryptera filer, ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="7ad12-193">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="7ad12-195">När du uppmanas ange krypteringslösenfrasen för hello ändrade supportpaket.</span><span class="sxs-lookup"><span data-stu-id="7ad12-195">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="7ad12-196">Skriv ned nya lösenfrasen som hello, så att du kan dela den med Microsoft-supporten när det behövs.</span><span class="sxs-lookup"><span data-stu-id="7ad12-196">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="7ad12-197">Exempel: Redigera filer i ett supportpaket på en lösenordsskyddad resurs</span><span class="sxs-lookup"><span data-stu-id="7ad12-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="7ad12-198">hello som följande exempel visar hur toodecrypt, redigera och kryptera ett supportpaket.</span><span class="sxs-lookup"><span data-stu-id="7ad12-198">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="7ad12-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ad12-199">Next steps</span></span>
* <span data-ttu-id="7ad12-200">Lär dig hur för[Använd paket och enheten loggar tootroubleshoot distributionen enheten](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="7ad12-200">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="7ad12-201">Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7ad12-201">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

