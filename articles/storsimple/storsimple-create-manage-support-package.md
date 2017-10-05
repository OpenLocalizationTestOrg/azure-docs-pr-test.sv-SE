---
title: "Skapa ett paket för stöd av StorSimple | Microsoft Docs"
description: "Lär dig hur du skapar, dekryptera och redigera ett supportpaket för din StorSimple-enhet."
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
ms.openlocfilehash: 32d20e7a8adcfc646c592213fe7395b87a93c985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="a1529-103">Skapa och hantera ett stödpaket för StorSimple</span><span class="sxs-lookup"><span data-stu-id="a1529-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="a1529-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a1529-104">Overview</span></span>
<span data-ttu-id="a1529-105">Ett supportpaket för StorSimple är ett enkelt att använda som samlar in alla relevanta loggar för att hjälpa Microsoft-supporten att felsöka eventuella problem för StorSimple-enhet används.</span><span class="sxs-lookup"><span data-stu-id="a1529-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs to assist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="a1529-106">Insamlade loggarna krypterade och komprimerade.</span><span class="sxs-lookup"><span data-stu-id="a1529-106">The collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="a1529-107">Vägledningen innehåller stegvisa instruktioner för att skapa och hantera support-paketet.</span><span class="sxs-lookup"><span data-stu-id="a1529-107">This tutorial includes step-by-step instructions to create and manage the support package.</span></span>

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="a1529-108">Skapa och ladda upp ett supportpaket i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a1529-108">Create and upload a support package in the Azure classic portal</span></span>
<span data-ttu-id="a1529-109">Du kan skapa och ladda upp ett supportpaket till webbplatsen Microsoft Support via den **Underhåll** sidan tjänsten i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a1529-109">You can create and upload a support package to the Microsoft Support site through the **Maintenance** page of the service in the Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="a1529-110">Överföringen kräver en nyckel för support.</span><span class="sxs-lookup"><span data-stu-id="a1529-110">The upload requires a support passkey.</span></span> <span data-ttu-id="a1529-111">Supportpersonalen bör ger det dig i ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="a1529-111">Your support engineer should provide this to you in an email.</span></span>
> 
> 

<span data-ttu-id="a1529-112">Ett paket med stöd för krypterade och komprimerade (CAB-fil) skapas och överförs till webbplats för Support.</span><span class="sxs-lookup"><span data-stu-id="a1529-112">An encrypted and compressed support package (.cab file) is created and uploaded to the Support site.</span></span> <span data-ttu-id="a1529-113">Supportteknikern kan sedan Hämta paketet från webbplatsen Support för att felsöka problemet.</span><span class="sxs-lookup"><span data-stu-id="a1529-113">The support engineer can then retrieve this package from the Support site for troubleshooting the issue.</span></span>

<span data-ttu-id="a1529-114">Utför följande steg i den klassiska portalen för att skapa ett supportpaket.</span><span class="sxs-lookup"><span data-stu-id="a1529-114">Perform the following steps in the classic portal to create a support package.</span></span>

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="a1529-115">Skapa ett supportpaket i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a1529-115">To create a support package in the Azure classic portal</span></span>
1. <span data-ttu-id="a1529-116">Välj **enheter** > **Underhåll**.</span><span class="sxs-lookup"><span data-stu-id="a1529-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="a1529-117">I den **stödpaketet** väljer **skapa och ladda upp supportpaket**.</span><span class="sxs-lookup"><span data-stu-id="a1529-117">In the **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="a1529-118">I den **skapa och ladda upp supportpaket** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="a1529-118">In the **Create and upload support package** dialog box, do the following:</span></span>
   
    ![Skapa för stödpaket](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="a1529-120">I den **stöd nyckel** text ange nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a1529-120">In the **Support Passkey** text box, enter the passkey.</span></span> <span data-ttu-id="a1529-121">Supportpersonalen Microsoft bör skicka den här nyckeln till dig via e-post.</span><span class="sxs-lookup"><span data-stu-id="a1529-121">Your Microsoft support engineer should send this passkey to you in email.</span></span>
   * <span data-ttu-id="a1529-122">Markera kryssrutan för att ge medgivande till automatiskt överföra support-paket till webbplatsen Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="a1529-122">Select the check box to provide consent to automatically upload the support package to the Microsoft Support site.</span></span>
   * <span data-ttu-id="a1529-123">Klicka på kryssikonen</span><span class="sxs-lookup"><span data-stu-id="a1529-123">Click the check icon</span></span> ![Kryssikon](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="a1529-125">.</span><span class="sxs-lookup"><span data-stu-id="a1529-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="a1529-126">Skapa ett supportpaket manuellt</span><span class="sxs-lookup"><span data-stu-id="a1529-126">Manually create a support package</span></span>
<span data-ttu-id="a1529-127">I vissa fall måste du manuellt skapa stöd paketet via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="a1529-127">In some cases, you'll need to manually create the support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="a1529-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a1529-128">For example:</span></span>

* <span data-ttu-id="a1529-129">Om du behöver ta bort känslig information från loggfilerna innan du delar med Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="a1529-129">If you need to remove sensitive information from your log files prior to sharing with Microsoft Support.</span></span>
* <span data-ttu-id="a1529-130">Om du har problem med överföring av paket på grund av problem med nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="a1529-130">If you are having difficulty uploading the package due to connectivity issues.</span></span>

<span data-ttu-id="a1529-131">Du kan dela din manuellt genererad supportpaket Microsoft support via e-post.</span><span class="sxs-lookup"><span data-stu-id="a1529-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="a1529-132">Utför följande steg för att skapa ett supportpaket i Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="a1529-132">Perform the following steps to create a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="a1529-133">Skapa ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="a1529-133">To create a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="a1529-134">Ange följande kommando för att starta en Windows PowerShell-session som administratör på den fjärrdator som används för att ansluta till din StorSimple-enhet:</span><span class="sxs-lookup"><span data-stu-id="a1529-134">To start a Windows PowerShell session as an administrator on the remote computer that's used to connect to your StorSimple device, enter the following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="a1529-135">Anslut till konsolen SSAdmin av enheten i Windows PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="a1529-135">In the Windows PowerShell session, connect to the SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="a1529-136">Ange följande i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="a1529-136">At the command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="a1529-137">I dialogrutan som öppnas anger du ditt administratörslösenord för enheten.</span><span class="sxs-lookup"><span data-stu-id="a1529-137">In the dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="a1529-138">Standardlösenordet är:</span><span class="sxs-lookup"><span data-stu-id="a1529-138">The default password is:</span></span>
     
      `Password1`
     
      ![Dialogrutan för PowerShell-autentiseringsuppgifter](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="a1529-140">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1529-140">Select **OK**.</span></span>
   * <span data-ttu-id="a1529-141">Ange följande i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="a1529-141">At the command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="a1529-142">Ange ett kommando i sessionen som öppnas.</span><span class="sxs-lookup"><span data-stu-id="a1529-142">In the session that opens, enter the appropriate command.</span></span>
   
   * <span data-ttu-id="a1529-143">För nätverksresurser som är lösenordsskyddad, ange:</span><span class="sxs-lookup"><span data-stu-id="a1529-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="a1529-144">Du uppmanas för ett lösenord, en sökväg till den delade nätverksmappen och krypteringslösenfrasen (eftersom paketet stöd krypteras).</span><span class="sxs-lookup"><span data-stu-id="a1529-144">You'll be prompted for a password, a path to the network shared folder, and an encryption passphrase (because the support package is encrypted).</span></span> <span data-ttu-id="a1529-145">Ett supportpaket skapas i den angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="a1529-145">A support package is then created in the specified folder.</span></span>
   * <span data-ttu-id="a1529-146">För resurser som inte är lösenordsskyddad, behöver du inte den `-Credential` parameter.</span><span class="sxs-lookup"><span data-stu-id="a1529-146">For shares that are not password protected, you do not need the `-Credential` parameter.</span></span> <span data-ttu-id="a1529-147">Ange följande:</span><span class="sxs-lookup"><span data-stu-id="a1529-147">Enter the following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="a1529-148">Stöd för paketet skapas för både domänkontrollanter i den angivna delade mappen.</span><span class="sxs-lookup"><span data-stu-id="a1529-148">The support package is created for both controllers in the specified network shared folder.</span></span> <span data-ttu-id="a1529-149">Det är en krypterad, komprimerad fil som kan skickas till Microsoft Support för felsökning.</span><span class="sxs-lookup"><span data-stu-id="a1529-149">It's an encrypted, compressed file that can be sent to Microsoft Support for troubleshooting.</span></span> <span data-ttu-id="a1529-150">Mer information finns i [kontakta Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="a1529-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="a1529-151">Export-HcsSupportPackage cmdlet-parametrarna</span><span class="sxs-lookup"><span data-stu-id="a1529-151">The Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="a1529-152">Du kan använda följande parametrar med cmdleten Export-HcsSupportPackage.</span><span class="sxs-lookup"><span data-stu-id="a1529-152">You can use the following parameters with the Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="a1529-153">Parameter</span><span class="sxs-lookup"><span data-stu-id="a1529-153">Parameter</span></span> | <span data-ttu-id="a1529-154">Nödvändiga/valfria</span><span class="sxs-lookup"><span data-stu-id="a1529-154">Required/Optional</span></span> | <span data-ttu-id="a1529-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a1529-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="a1529-156">Krävs</span><span class="sxs-lookup"><span data-stu-id="a1529-156">Required</span></span> |<span data-ttu-id="a1529-157">Använd för att ange platsen för den delade nätverksmappen där support-paketet är placerad.</span><span class="sxs-lookup"><span data-stu-id="a1529-157">Use to provide the location of the network shared folder in which the support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="a1529-158">Krävs</span><span class="sxs-lookup"><span data-stu-id="a1529-158">Required</span></span> |<span data-ttu-id="a1529-159">Använd för att ange en lösenfras för att kryptera support-paketet.</span><span class="sxs-lookup"><span data-stu-id="a1529-159">Use to provide a passphrase to help encrypt the support package.</span></span> |
| `-Credential` |<span data-ttu-id="a1529-160">Valfri</span><span class="sxs-lookup"><span data-stu-id="a1529-160">Optional</span></span> |<span data-ttu-id="a1529-161">Använd för att ange autentiseringsuppgifter för den delade nätverksmappen.</span><span class="sxs-lookup"><span data-stu-id="a1529-161">Use to supply access credentials for the network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="a1529-162">Valfri</span><span class="sxs-lookup"><span data-stu-id="a1529-162">Optional</span></span> |<span data-ttu-id="a1529-163">Använd för att hoppa över steg kryptering lösenfras bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="a1529-163">Use to skip the encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="a1529-164">Valfri</span><span class="sxs-lookup"><span data-stu-id="a1529-164">Optional</span></span> |<span data-ttu-id="a1529-165">Använd för att ange en katalog under *sökvägen* i paketets stöd är placerad.</span><span class="sxs-lookup"><span data-stu-id="a1529-165">Use to specify a directory under *Path* in which the support package is placed.</span></span> <span data-ttu-id="a1529-166">Standardvärdet är [enhetsnamn]-[aktuellt datum och time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="a1529-166">The default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="a1529-167">Valfri</span><span class="sxs-lookup"><span data-stu-id="a1529-167">Optional</span></span> |<span data-ttu-id="a1529-168">Ange som **klustret** (standard) för att skapa ett supportpaket för både domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="a1529-168">Specify as **Cluster** (default) to create a support package for both controllers.</span></span> <span data-ttu-id="a1529-169">Om du vill skapa ett paket endast för den aktuella domänkontrollanten, ange **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="a1529-169">If you want to create a package only for the current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="a1529-170">Redigera ett supportpaket</span><span class="sxs-lookup"><span data-stu-id="a1529-170">Edit a support package</span></span>
<span data-ttu-id="a1529-171">När du har genererat ett supportpaket kan behöva du redigera paket för att ta bort känslig information.</span><span class="sxs-lookup"><span data-stu-id="a1529-171">After you have generated a support package, you might need to edit the package to remove sensitive information.</span></span> <span data-ttu-id="a1529-172">Detta kan inkludera volymnamn, enhetens IP-adresser och säkerhetskopiering namn från loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="a1529-172">This can include volume names, device IP addresses, and backup names from the log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1529-173">Du kan bara redigera ett supportpaket som har genererats via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="a1529-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="a1529-174">Du kan inte redigera ett paket som skapats i den klassiska Azure-portalen med StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a1529-174">You can't edit a package created in the Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="a1529-175">Om du vill redigera ett supportpaket innan den skickas på webbplatsen Microsoft Support först dekryptera stöd för paketet, redigera filerna och kryptera den igen.</span><span class="sxs-lookup"><span data-stu-id="a1529-175">To edit a support package before uploading it on the Microsoft Support site, first decrypt the support package, edit the files, and then re-encrypt it.</span></span> <span data-ttu-id="a1529-176">Utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="a1529-176">Perform the following steps.</span></span>

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="a1529-177">Så här redigerar du ett supportpaket i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="a1529-177">To edit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="a1529-178">Generera ett supportpaket som beskrivs ovan, [att skapa ett supportpaket i Windows PowerShell för StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="a1529-178">Generate a support package as described earlier, in [To create a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="a1529-179">[Hämta skriptet](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalt på klientdatorerna.</span><span class="sxs-lookup"><span data-stu-id="a1529-179">[Download the script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="a1529-180">Importera Windows PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a1529-180">Import the Windows PowerShell module.</span></span> <span data-ttu-id="a1529-181">Ange sökvägen till den lokala mappen där du har hämtat skriptet.</span><span class="sxs-lookup"><span data-stu-id="a1529-181">Specify the path to the local folder in which you downloaded the script.</span></span> <span data-ttu-id="a1529-182">Ange om du vill importera modulen:</span><span class="sxs-lookup"><span data-stu-id="a1529-182">To import the module, enter:</span></span>
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. <span data-ttu-id="a1529-183">Alla filer är *.aes* filer som är komprimerade och krypteras.</span><span class="sxs-lookup"><span data-stu-id="a1529-183">All the files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="a1529-184">För att expandera och dekryptera filer, ange:</span><span class="sxs-lookup"><span data-stu-id="a1529-184">To decompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    <span data-ttu-id="a1529-185">Observera att de faktiska filnamnstillägg som visas för alla filer.</span><span class="sxs-lookup"><span data-stu-id="a1529-185">Note that the actual file extensions are now displayed for all the files.</span></span>
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="a1529-187">Ange lösenordet som du använde när support paketet skapades när du uppmanas att ange krypteringslösenfrasen.</span><span class="sxs-lookup"><span data-stu-id="a1529-187">When you're prompted for the encryption passphrase, enter the passphrase that you used when the support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="a1529-188">Bläddra till den mapp som innehåller loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="a1529-188">Browse to the folder that contains the log files.</span></span> <span data-ttu-id="a1529-189">Eftersom filerna är nu packa upp och dekrypteras, kommer dessa har ursprungliga filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="a1529-189">Because the log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="a1529-190">Ändra de här filerna för att ta bort alla kundspecifik information, till exempel volymnamn och enheten IP-adresser och spara filerna.</span><span class="sxs-lookup"><span data-stu-id="a1529-190">Modify these files to remove any customer-specific information, such as volume names and device IP addresses, and save the files.</span></span>
7. <span data-ttu-id="a1529-191">Stäng filer om du vill komprimera dem med gzip och kryptera dem med AES 256.</span><span class="sxs-lookup"><span data-stu-id="a1529-191">Close the files to compress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="a1529-192">Detta är hastighet och säkerhet i överföra paketets stöd över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="a1529-192">This is for speed and security in transferring the support package over a network.</span></span> <span data-ttu-id="a1529-193">Om du vill komprimera och kryptera filer, anger du följande:</span><span class="sxs-lookup"><span data-stu-id="a1529-193">To compress and encrypt files, enter the following:</span></span>
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Redigera supportpaket](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="a1529-195">När du uppmanas ange krypteringslösenfrasen för det ändrade stöd paketet.</span><span class="sxs-lookup"><span data-stu-id="a1529-195">When prompted, provide an encryption passphrase for the modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="a1529-196">Skriv ned den nya lösenfrasen så att du kan dela den med Microsoft-supporten när det behövs.</span><span class="sxs-lookup"><span data-stu-id="a1529-196">Write down the new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="a1529-197">Exempel: Redigera filer i ett supportpaket på en lösenordsskyddad resurs</span><span class="sxs-lookup"><span data-stu-id="a1529-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="a1529-198">I följande exempel visar hur du dekryptera, redigera och kryptera ett supportpaket.</span><span class="sxs-lookup"><span data-stu-id="a1529-198">The following example shows how to decrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="a1529-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1529-199">Next steps</span></span>
* <span data-ttu-id="a1529-200">Lär dig hur du [använder paket och enhetsloggar för att felsöka enheten distributionen](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="a1529-200">Learn how to [use support packages and device logs to troubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="a1529-201">Lär dig hur du [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="a1529-201">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

