---
title: "aaaCreate en StorSimple 8000-serien stödpaket | Microsoft Docs"
description: "Lär dig hur toocreate, dekryptera och redigera ett supportpaket för enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 857555b6ba31b1527f8f00d19818ebbec6005d0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a><span data-ttu-id="59de8-103">Skapa och hantera ett stödpaket för StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="59de8-103">Create and manage a support package for StorSimple 8000 series</span></span>

## <a name="overview"></a><span data-ttu-id="59de8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="59de8-104">Overview</span></span>

<span data-ttu-id="59de8-105">Ett supportpaket för StorSimple är ett enkelt att använda som samlar in alla relevanta loggar tooassist Microsoft-supporten att felsöka eventuella problem för StorSimple-enhet används.</span><span class="sxs-lookup"><span data-stu-id="59de8-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="59de8-106">hello är insamlade loggar krypterade och komprimerade.</span><span class="sxs-lookup"><span data-stu-id="59de8-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="59de8-107">Den här självstudiekursen innehåller stegvisa instruktioner toocreate och hantera hello stöd paketet för enheten StorSimple 8000-serien.</span><span class="sxs-lookup"><span data-stu-id="59de8-107">This tutorial includes step-by-step instructions toocreate and manage hello support package for your StorSimple 8000 series device.</span></span> <span data-ttu-id="59de8-108">Om du arbetar med en virtuell StorSimple-matris gå för[skapar ett paket i loggen](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="59de8-108">If you are working with a StorSimple Virtual Array, go too[generate a log package](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="create-a-support-package"></a><span data-ttu-id="59de8-109">Skapa ett supportpaket</span><span class="sxs-lookup"><span data-stu-id="59de8-109">Create a support package</span></span>

<span data-ttu-id="59de8-110">I vissa fall måste toomanually skapa hello supportpaket via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="59de8-110">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="59de8-111">Exempel:</span><span class="sxs-lookup"><span data-stu-id="59de8-111">For example:</span></span>

* <span data-ttu-id="59de8-112">Om du behöver tooremove känslig information från loggen filer tidigare toosharing Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="59de8-112">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="59de8-113">Om du har problem med överföring hello-paket på grund av problem med tooconnectivity.</span><span class="sxs-lookup"><span data-stu-id="59de8-113">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="59de8-114">Du kan dela din manuellt genererad supportpaket Microsoft support via e-post.</span><span class="sxs-lookup"><span data-stu-id="59de8-114">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="59de8-115">Utför följande steg toocreate ett supportpaket i Windows PowerShell för StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="59de8-115">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="59de8-116">toocreate ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="59de8-116">toocreate a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="59de8-117">toostart en Windows PowerShell-session som administratör på fjärrdatorn hello som har använt tooconnect tooyour StorSimple-enhet, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="59de8-117">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="59de8-118">Anslut toohello SSAdmin konsolen för din enhet i hello Windows PowerShell-sessionen:</span><span class="sxs-lookup"><span data-stu-id="59de8-118">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   1. <span data-ttu-id="59de8-119">Ange Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="59de8-119">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. <span data-ttu-id="59de8-120">Ange administratörslösenord för enheten hello dialogrutan som öppnas.</span><span class="sxs-lookup"><span data-stu-id="59de8-120">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="59de8-121">hello standardlösenordet är _Password1_.</span><span class="sxs-lookup"><span data-stu-id="59de8-121">hello default password is _Password1_.</span></span>
     
      ![Dialogrutan för PowerShell-autentiseringsuppgifter](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. <span data-ttu-id="59de8-123">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="59de8-123">Select **OK**.</span></span>
   4. <span data-ttu-id="59de8-124">Ange Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="59de8-124">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="59de8-125">Ange hello kommandona i hello-sessionen som öppnas.</span><span class="sxs-lookup"><span data-stu-id="59de8-125">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="59de8-126">För nätverksresurser som är lösenordsskyddad, ange:</span><span class="sxs-lookup"><span data-stu-id="59de8-126">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="59de8-127">Du uppmanas för ett lösenord, en sökväg toohello delad nätverksmapp och krypteringslösenfrasen (eftersom hello supportpaket krypteras).</span><span class="sxs-lookup"><span data-stu-id="59de8-127">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="59de8-128">Ett supportpaket skapas i hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="59de8-128">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="59de8-129">För resurser som inte är lösenordsskyddad, behöver inte hello `-Credential` parameter.</span><span class="sxs-lookup"><span data-stu-id="59de8-129">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="59de8-130">Ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="59de8-130">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="59de8-131">hello supportpaket skapas för både domänkontrollanter i hello angivna delad nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="59de8-131">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="59de8-132">Det är en krypterad, komprimerad fil som kan skickas tooMicrosoft stöd för felsökning.</span><span class="sxs-lookup"><span data-stu-id="59de8-132">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="59de8-133">Mer information finns i [kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="59de8-133">For more information, see [Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="59de8-134">hello Export HcsSupportPackage cmdlet-parametrar</span><span class="sxs-lookup"><span data-stu-id="59de8-134">hello Export-HcsSupportPackage cmdlet parameters</span></span>

<span data-ttu-id="59de8-135">Du kan använda följande parametrar med hello Export HcsSupportPackage cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="59de8-135">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="59de8-136">Parameter</span><span class="sxs-lookup"><span data-stu-id="59de8-136">Parameter</span></span> | <span data-ttu-id="59de8-137">Nödvändiga/valfria</span><span class="sxs-lookup"><span data-stu-id="59de8-137">Required/Optional</span></span> | <span data-ttu-id="59de8-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="59de8-138">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="59de8-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="59de8-139">Required</span></span> |<span data-ttu-id="59de8-140">Använd tooprovide hello platsen för hello delad nätverksmapp där hello supportpaket är placerad.</span><span class="sxs-lookup"><span data-stu-id="59de8-140">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="59de8-141">Krävs</span><span class="sxs-lookup"><span data-stu-id="59de8-141">Required</span></span> |<span data-ttu-id="59de8-142">Använd tooprovide en lösenfras toohelp kryptera hello supportpaket.</span><span class="sxs-lookup"><span data-stu-id="59de8-142">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="59de8-143">Valfri</span><span class="sxs-lookup"><span data-stu-id="59de8-143">Optional</span></span> |<span data-ttu-id="59de8-144">Använd toosupply autentiseringsuppgifter för hello delad nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="59de8-144">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="59de8-145">Valfri</span><span class="sxs-lookup"><span data-stu-id="59de8-145">Optional</span></span> |<span data-ttu-id="59de8-146">Använd tooskip hello kryptering lösenfras bekräftelsesteg.</span><span class="sxs-lookup"><span data-stu-id="59de8-146">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="59de8-147">Valfri</span><span class="sxs-lookup"><span data-stu-id="59de8-147">Optional</span></span> |<span data-ttu-id="59de8-148">Använda toospecify en katalog under *sökväg* vilka hello stöd paketet är placerad.</span><span class="sxs-lookup"><span data-stu-id="59de8-148">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="59de8-149">hello standardvärdet är [enhetsnamn]-[aktuellt datum och time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="59de8-149">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="59de8-150">Valfri</span><span class="sxs-lookup"><span data-stu-id="59de8-150">Optional</span></span> |<span data-ttu-id="59de8-151">Ange som **klustret** (standard) toocreate ett supportpaket för både domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="59de8-151">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="59de8-152">Om du vill toocreate ett paket endast för hello aktuella controller anger **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="59de8-152">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="59de8-153">Redigera ett supportpaket</span><span class="sxs-lookup"><span data-stu-id="59de8-153">Edit a support package</span></span>

<span data-ttu-id="59de8-154">När du har genererat ett supportpaket kan behöva du tooedit hello paketet tooremove känslig information.</span><span class="sxs-lookup"><span data-stu-id="59de8-154">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="59de8-155">Detta kan inkludera volymnamn, enhetens IP-adresser och säkerhetskopiering namn från hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="59de8-155">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59de8-156">Du kan bara redigera ett supportpaket som har genererats via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="59de8-156">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="59de8-157">Du kan inte redigera ett paket som skapats i hello Azure-portalen med Enhetshanteraren för StorSimple-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59de8-157">You can't edit a package created in hello Azure portal with StorSimple Device Manager service.</span></span>

<span data-ttu-id="59de8-158">tooedit ett supportpaket innan du laddar upp den på hello Microsoft Support webbplats först dekryptera hello supportpaket, redigera hello filer och kryptera den igen.</span><span class="sxs-lookup"><span data-stu-id="59de8-158">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="59de8-159">Utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="59de8-159">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="59de8-160">tooedit ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="59de8-160">tooedit a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="59de8-161">Generera ett supportpaket som beskrivs ovan, [toocreate ett supportpaket i Windows PowerShell för StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="59de8-161">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="59de8-162">[Hämta hello skriptet](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalt på klientdatorerna.</span><span class="sxs-lookup"><span data-stu-id="59de8-162">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="59de8-163">Importera hello Windows PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="59de8-163">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="59de8-164">Ange hello sökvägen toohello lokal mapp som du hämtade hello skript.</span><span class="sxs-lookup"><span data-stu-id="59de8-164">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="59de8-165">tooimport hello modulen, ange:</span><span class="sxs-lookup"><span data-stu-id="59de8-165">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="59de8-166">Alla filer som hello *.aes* filer som är komprimerade och krypteras.</span><span class="sxs-lookup"><span data-stu-id="59de8-166">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="59de8-167">toodecompress och dekryptera filer, ange:</span><span class="sxs-lookup"><span data-stu-id="59de8-167">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="59de8-168">Observera att hello faktiska filnamnstillägg nu visas för alla hello-filer.</span><span class="sxs-lookup"><span data-stu-id="59de8-168">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Redigera supportpaket](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="59de8-170">När du uppmanas att ange hello krypteringslösenfrasen ange hello lösenfras som du använde när hello stödpaketet skapades.</span><span class="sxs-lookup"><span data-stu-id="59de8-170">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="59de8-171">Bläddra toohello mapp som innehåller hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="59de8-171">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="59de8-172">Eftersom hello loggfiler är nu packa upp och dekrypteras, kommer dessa har ursprungliga filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="59de8-172">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="59de8-173">Ändrar dessa filer tooremove kundspecifik information, till exempel volymnamn och enheten IP-adresser, och spara hello-filer.</span><span class="sxs-lookup"><span data-stu-id="59de8-173">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="59de8-174">Stäng hello filer toocompress dem med gzip och kryptera dem med AES 256.</span><span class="sxs-lookup"><span data-stu-id="59de8-174">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="59de8-175">Detta är hastighet och säkerhet i överföra hello supportpaket över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="59de8-175">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="59de8-176">toocompress och kryptera filer, ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="59de8-176">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Redigera supportpaket](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="59de8-178">När du uppmanas ange krypteringslösenfrasen för hello ändrade supportpaket.</span><span class="sxs-lookup"><span data-stu-id="59de8-178">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="59de8-179">Skriv ned nya lösenfrasen som hello, så att du kan dela den med Microsoft-supporten när det behövs.</span><span class="sxs-lookup"><span data-stu-id="59de8-179">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="59de8-180">Exempel: Redigera filer i ett supportpaket på en lösenordsskyddad resurs</span><span class="sxs-lookup"><span data-stu-id="59de8-180">Example: Editing files in a support package on a password-protected share</span></span>

<span data-ttu-id="59de8-181">hello som följande exempel visar hur toodecrypt, redigera och kryptera ett supportpaket.</span><span class="sxs-lookup"><span data-stu-id="59de8-181">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="59de8-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59de8-182">Next steps</span></span>

* <span data-ttu-id="59de8-183">Lär dig mer om hello [information som samlas in i hello supportpaket](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span><span class="sxs-lookup"><span data-stu-id="59de8-183">Learn about hello [information collected in hello Support package](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span></span>
* <span data-ttu-id="59de8-184">Lär dig hur för[Använd paket och enheten loggar tootroubleshoot distributionen enheten](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="59de8-184">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="59de8-185">Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="59de8-185">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

